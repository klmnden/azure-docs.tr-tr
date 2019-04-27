---
title: HDInsight, Kurumsal güvenlik paketi - Azure ile Apache Kafka ilkeleri yapılandırma
description: Kurumsal Güvenlik Paketi ile Azure HDInsight içinde Kafka için Apache Ranger ilkelerini yapılandırmayı öğrenin.
ms.service: hdinsight
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.topic: tutorial
ms.date: 01/14/2019
ms.openlocfilehash: 381958b462b668447a6548029a1be616df7a3a4b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60536432"
---
# <a name="tutorial-configure-apache-kafka-policies-in-hdinsight-with-enterprise-security-package-preview"></a>Öğretici: HDInsight, Kurumsal güvenlik paketi (Önizleme) ile Apache Kafka ilkeleri yapılandırma

Kurumsal güvenlik paketi (ESP) Apache Kafka kümeleri için Apache Ranger ilkelerini yapılandırmayı öğrenin. ESP kümeleri bir etki alanına bağlıdır ve kullanıcıların etki alanı kimlik bilgileriyle kimlik doğrulaması yapmasına olanak sağlar. Bu öğreticide, `sales*` ve `marketingspend` konularına erişimi kısıtlamak için iki Ranger ilkesi oluşturacaksınız.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Etki alanı kullanıcılarını oluşturma
> * Ranger ilkelerini oluşturma
> * Kafka kümesinde konu oluşturma
> * Ranger ilkelerini test etme

## <a name="before-you-begin"></a>Başlamadan önce

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

* [Azure Portal](https://portal.azure.com/) oturum açın.

* [Kurumsal Güvenlik Paketi ile HDInsight Kafka kümesi](apache-domain-joined-configure-using-azure-adds.md) oluşturun.

## <a name="connect-to-apache-ranger-admin-ui"></a>Apache Ranger Yönetici Arabirimine bağlanma

1. Bir tarayıcıdan, `https://<ClusterName>.azurehdinsight.net/Ranger/` URL’sini kullanarak Ranger Yönetici kullanıcı arabirimine bağlanın. `<ClusterName>` öğesini Kafka kümenizin adıyla değiştirmeyi unutmayın.

    > [!NOTE]  
    > Ranger kimlik bilgileri Hadoop kümesi kimlik bilgileriyle aynı değildir. Tarayıcıların ön belleğe alınmış Hadoop kimlik bilgilerini kullanmasını önlemek için Ranger Yönetici Arabirimine yeni bir InPrivate tarayıcı penceresinden bağlanın.

2. Azure Active Directory (AD) yönetici kimlik bilgilerinizi kullanarak oturum açın. Azure AD yönetici kimlik bilgileri HDInsight küme kimlik bilgileri veya Linux HDInsight düğümü SSH kimlik bilgileriyle aynı değildir.

   ![Apache Ranger Yönetici Arabirimi](./media/apache-domain-joined-run-kafka/apache-ranger-admin-login.png)

## <a name="create-domain-users"></a>Etki alanı kullanıcılarını oluşturma

**sales_user** ve **marketing_user** etki alanı kullanıcılarını nasıl oluşturacağınızı öğrenmek için [Kurumsal Güvenlik Paketi ile HDInsight kümesi oluşturma](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds) bölümünü ziyaret edin. Bir üretim senaryosunda, etki alanı kullanıcıları Active Directory kiracınızdan gelir.

## <a name="create-ranger-policy"></a>Ranger ilkesini oluşturma

**sales_user** ve **marketing_user** için Ranger ilkesi oluşturun.

1. **Ranger Yönetici Arabirimini** açın.

2. **Kafka** altında **\<ClusterName>_kafka** seçeneğine tıklayın. Bir önceden yapılandırılmış ilke listelenebilir.

3. **Yeni İlke Ekle**’ye tıklayıp aşağıdaki değerleri girin:

   |**Ayar**  |**Önerilen değer**  |
   |---------|---------|
   |İlke Adı  |  hdi satış* ilkesi   |
   |Konu   |  satış* |
   |Kullanıcı Seçin  |  sales_user1 |
   |İzinler  | yayımlama, kullanma, oluşturma |

   Konu adında şu joker karakterler bulunabilir:

   * ’*’ karakterlerin sıfır veya daha fazla kez geçtiğini gösterir.
   * ’?‘ tek karakteri gösterir.

   ![Apache Ranger Yönetici Arabirimi Oluşturma İlkesi](./media/apache-domain-joined-run-kafka/apache-ranger-admin-create-policy.png)   

   >[!NOTE]
   >**Select User** için bir etki alanı kullanıcısı otomatik olarak doldurulmazsa, Ranger’ın Azure AD ile eşitlenmesi için birkaç dakika bekleyin.

4. **Add**’e tıklayarak ilkeyi kaydedin.

5. **Add New Policy**’ye tıklayıp aşağıdaki değerleri girin:

   |**Ayar**  |**Önerilen değer**  |
   |---------|---------|
   |İlke Adı  |  hdi pazarlama ilkesi   |
   |Konu   |  marketingspend |
   |Kullanıcı Seçin  |  marketing_user1 |
   |İzinler  | yayımlama, kullanma, oluşturma |

   ![Apache Ranger Yönetici Arabirimi Oluşturma İlkesi](./media/apache-domain-joined-run-kafka/apache-ranger-admin-create-policy-2.png)  

6. **Add**’e tıklayarak ilkeyi kaydedin.

## <a name="create-topics-in-a-kafka-cluster-with-esp"></a>ESP ile Kafka kümesinde konu oluşturma

İki Konular oluşturmak için `salesevents` ve `marketingspend`:

1. Kümeye SSH bağlantısı açmak için aşağıdaki komutu kullanın:

   ```bash
   ssh DOMAINADMIN@CLUSTERNAME-ssh.azurehdinsight.net
   ```

   Değiştirin `DOMAINADMIN` sırasında yapılandırılan kümeniz için yönetici kullanıcı ile [küme oluşturma](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds#create-a-hdinsight-cluster-with-esp)ve yerine `CLUSTERNAME` değerini kümenizin adıyla. İstenirse, yönetici kullanıcı hesabı için parolayı girin. HDInsight ile `SSH` kullanma hakkında daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix).

2. Küme adını bir değişkene kaydedip JSON ayrıştırma yardımcı programını (`jq`) yüklemek için aşağıdaki komutları kullanın. İstendiğinde, Kafka kümesi adını girin.

   ```bash
   sudo apt -y install jq
   read -p 'Enter your Kafka cluster name:' CLUSTERNAME
   ```

3. Konaklar Kafka Aracısı almak için aşağıdaki komutları kullanın. İstendiğinde, küme yöneticisi hesabı için parolayı girin.

   ```bash
   export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`; \
   ```

   > [!Note]  
   > Devam etmeden önce henüz ayarlamadıysanız geliştirme ortamınızı ayarlamanız gerekebilir. Java JDK, Apache Maven ve scp desteğine sahip bir SSH istemcisine ihtiyacınız olacaktır. Daha fazla ayrıntı için [kurulum yönergeleri](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/tree/master/DomainJoined-Producer-Consumer).
   
1. [Apache Kafka etki alanına katılmış üretici tüketici örneklerini](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/tree/master/DomainJoined-Producer-Consumer) indirin.

1. Adım 2 ve 3 altında izleyin **oluşturun ve örnek dağıtın** içinde [Öğreticisi: Apache Kafka üretici ve tüketici API'lerini kullanma](https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-producer-consumer-api#build-and-deploy-the-example)

1. Aşağıdaki komutları çalıştırın:

   ```bash
   java -jar -Djava.security.auth.login.config=/usr/hdp/current/kafka-broker/config/kafka_client_jaas.conf kafka-producer-consumer.jar create salesevents $KAFKABROKERS
   java -jar -Djava.security.auth.login.config=/usr/hdp/current/kafka-broker/config/kafka_client_jaas.conf kafka-producer-consumer.jar create marketingspend $KAFKABROKERS
   ```

## <a name="test-the-ranger-policies"></a>Ranger ilkelerini test etme

Yapılandırılmış, Ranger ilkelerine bağlı **sales_user** üretim tüketebileceği/konu `salesevents` ancak değil konu `marketingspend`. Buna karşılık, **marketing_user** üretim tüketebileceği/konu `marketingspend` ancak değil konu `salesevents`.

1. Kümeye yeni bir SSH bağlantısı açın. **sales_user1** olarak oturum açmak için aşağıdaki komutu kullanın:

   ```bash
   ssh sales_user1@CLUSTERNAME-ssh.azurehdinsight.net
   ```

2. Aşağıdaki komutu yürütün:

   ```bash
   export KAFKA_OPTS="-Djava.security.auth.login.config=/usr/hdp/current/kafka-broker/config/kafka_client_jaas.conf"
   ```

3. Aracısı adları, aşağıdaki ortam değişkenlerini ayarlamak için önceki bölümden kullanın:

   ```bash
   export KAFKABROKERS=<brokerlist>:9092
   ```

   Örnek: `export KAFKABROKERS=wn0-khdicl.contoso.com:9092,wn1-khdicl.contoso.com:9092`

4. Adım 3'altında izleyin **oluşturun ve örnek dağıtın** içinde [Öğreticisi: Apache Kafka üretici ve tüketici API'lerini kullanma](https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-producer-consumer-api#build-and-deploy-the-example) emin olmak için `kafka-producer-consumer.jar` da kullanılabilir **sales_user**.

5. Doğrulayın **sales_user1** konuya üretebilir `salesevents` aşağıdaki komutu yürüterek:

   ```bash
   java -jar kafka-producer-consumer.jar producer salesevents $KAFKABROKERS
   ```

6. Konu başlığından kullanmak için aşağıdaki komutu yürütün `salesevents`:

   ```bash
   java -jar kafka-producer-consumer.jar consumer salesevents $KAFKABROKERS
   ```

   İletileri okuyabilir doğrulayın.

7. Doğrulayın **sales_user1** konuya üretemiyor `marketingspend` yürüterek aşağıdaki aynı ssh penceresi:

   ```bash
   java -jar kafka-producer-consumer.jar producer marketingspend $KAFKABROKERS
   ```

   Bir yetkilendirme hatası oluşur ve bu yok sayılabilir.

8. Dikkat **marketing_user1** konu başlığından kullanamıyor `salesevents`.

   Yukarıdaki ancak bu kez olarak 1-4 adımları yineleyin **marketing_user1**.

   Konu başlığından kullanmak için aşağıdaki komutu yürütün `salesevents`:

   ```bash
   java -jar kafka-producer-consumer.jar consumer salesevents $KAFKABROKERS
   ```

   Önceki iletiler görülemez.

9. Ranger kullanıcı arabiriminden denetim erişimi olaylarını görüntüleyin.

   ![Ranger Kullanıcı Arabirimi Denetim İlkesi](./media/apache-domain-joined-run-kafka/apache-ranger-admin-audit.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Kafka için kendi anahtarını Getir](https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-byok)
* [Kurumsal güvenlik paketi ile Apache Hadoop güvenliğine giriş](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-introduction)
