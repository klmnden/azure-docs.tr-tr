---
title: HDInsight, Kurumsal güvenlik paketi - Azure ile Apache Kafka ilkeleri yapılandırma
description: Kurumsal Güvenlik Paketi ile Azure HDInsight içinde Kafka için Apache Ranger ilkelerini yapılandırmayı öğrenin.
services: hdinsight
ms.service: hdinsight
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.topic: tutorial
ms.date: 09/24/2018
ms.openlocfilehash: aa6702ccf00faa3d63d5458cfbd77ac15fbfbeaa
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51633057"
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

**sales_user** ve **marketing_user** etki alanı kullanıcılarını nasıl oluşturacağınızı öğrenmek için [Kurumsal Güvenlik Paketi ile HDInsight kümesi oluşturma](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds#create-a-domain-joined-hdinsight-cluster) bölümünü ziyaret edin. Bir üretim senaryosunda, etki alanı kullanıcıları Active Directory kiracınızdan gelir.

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

**salesevents** ve **marketingspend** adlı iki konu oluşturmak için:

1. Kümeye SSH bağlantısı açmak için aşağıdaki komutu kullanın:

   ```bash
   ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net
   ```

   `SSHUSER` değerini, kümenizin SSH kullanıcısı ile, `CLUSTERNAME` değerini kümenizin adıyla değiştirin. İstendiğinde, SSH kullanıcı hesabının parolasını girin. HDInsight ile `scp` kullanma hakkında daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix).

   Bir üretim senaryosunda, küme oluşturma sırasında oluşturulan kullanıcılar kümede SSH kullanabilir.

2. Küme adını bir değişkene kaydedip JSON ayrıştırma yardımcı programını (`jq`) yüklemek için aşağıdaki komutları kullanın. İstendiğinde, Kafka kümesi adını girin.

   ```bash
   sudo apt -y install jq
   read -p 'Enter your Kafka cluster name:' CLUSTERNAME
   ```

3. Kafka aracısı ana bilgisayarlarını ve Zookeeper ana bilgisayarlarını almak için aşağıdaki komutları kullanın. İstendiğinde, küme yöneticisi hesabı için parolayı girin.

   ```bash
   export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`; \
   export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`; \
   ```
> [!Note]
> Devam etmeden önce henüz ayarlamadıysanız geliştirme ortamınızı ayarlamanız gerekebilir. Java JDK, Apache Maven ve scp desteğine sahip bir SSH istemcisine ihtiyacınız olacaktır. Ayrıntılı bilgi için bu [kurulum yönergelerini](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/tree/master/DomainJoined-Producer-Consumer) inceleyin.
1. [Apache Kafka etki alanına katılmış üretici tüketici örneklerini](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/tree/master/DomainJoined-Producer-Consumer) indirin.

1. [Öğretici: Apache Kafka Üretici ve Tüketici API’lerini kullanma](https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-producer-consumer-api#build-and-deploy-the-example) sayfasının **Örneği derleme ve dağıtma** bölümündeki 2 ve 3 numaralı adımları izleyin.

1. Aşağıdaki komutları çalıştırın:

   ```bash
   java -jar -Djava.security.auth.login.config=/usr/hdp/current/kafka-broker/config/kafka_client_jaas.conf kafka-producer-consumer.jar create salesevents $KAFKABROKERS
   java -jar -Djava.security.auth.login.config=/usr/hdp/current/kafka-broker/config/kafka_client_jaas.conf kafka-producer-consumer.jar create marketingspend $KAFKABROKERS
   ```

   >[!NOTE] 
   >Yalnızca kök gibi Kafka hizmetinin süreç sahibi Zookeeper znodes `/config/topics` öğesine yazabilir. Ayrıcalıklı olmayan bir kullanıcı bir konu oluşturduğunda Ranger ilkeleri zorlanmaz. Bunun nedeni, `kafka-topics.sh` betiğinin konuyu oluşturmak için Zookeeper ile doğrudan iletişim kurmasıdır. Girdiler Zookeeper düğümlerine eklenir ve aracı tarafındaki izleyiciler konuları buna göre izler ve oluşturur. Yetkilendirme Ranger eklentisi aracılığıyla yapılamaz ve yukarıdaki komut Kafka aracısı üzerinden `sudo` kullanılarak yürütülür.


## <a name="test-the-ranger-policies"></a>Ranger ilkelerini test etme

Yapılandırılan Ranger ilkelerine bağlı olarak, **sales_user** **salesevents** konusunu üretebilir/kullanabilir ancak **marketingspend** konusunu üretemez/kullanamaz. Buna karşılık, **marketing_user** **marketingspend** konusunu üretebilir/kullanabilir ancak **salesevents** konusunu üretemez/kullanamaz.

1. Kümeye yeni bir SSH bağlantısı açın. **sales_user1** olarak oturum açmak için aşağıdaki komutu kullanın:

   ```bash
   ssh sales_user1@CLUSTERNAME-ssh.azurehdinsight.net
   ```

2. Aşağıdaki komutu yürütün:

   ```bash
   export KAFKA_OPTS="-Djava.security.auth.login.config=/usr/hdp/current/kafka-broker/config/kafka_client_jaas.conf"
   ```

3. Aşağıdaki ortam değişkenlerini ayarlamak için önceki adımdan aracı ve Zookeeper adlarını kullanın:

   ```bash
   export KAFKABROKERS=<brokerlist>:9092 
   ```

   Örnek: `export KAFKABROKERS=wn0-khdicl.contoso.com:9092,wn1-khdicl.contoso.com:9092`

   ```bash
   export KAFKAZKHOSTS=<zklist>:2181
   ```

   Örnek: `export KAFKAZKHOSTS=zk1-khdicl.contoso.com:2181,zk2-khdicl.contoso.com:2181`

4. **sales_user1** kullanıcısının **salesevents** konusuna üretebildiğini doğrulayın.
   
   **salesevents** konusu için konsol üreticisini başlatmak için aşağıdaki komutu yürütün:

   ```bash
   /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic salesevents --security-protocol SASL_PLAINTEXT
   ```

   Daha sonra, konsolda birkaç ileti girin. Konsol üreticisinden çıkmak için **Ctrl + C** tuşlarına basın.

5. **salesevents** konusundan öğeleri kullanmak için aşağıdaki komutu yürütün:

   ```bash
   /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --topic salesevents --security-protocol PLAINTEXTSASL --from-beginning
   ```
 
6. Önceki adımda girdiğiniz iletilerin göründüğünü ve **sales_user1** kullanıcısının **marketingspend** konusuna üretim yapamadığını doğrulayın.

   Yukarıdakiyle aynı ssh penceresinden, **marketingspend** konusuna üretim yapmak için aşağıdaki komutu çalıştırın:

   ```bash
   /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic marketingspend --security-protocol SASL_PLAINTEXT
   ```

   Bir yetkilendirme hatası oluşur ve bu yok sayılabilir. 

7. **marketing_user1** kullanıcısından **salesevents** konusundan öğeleri kullanamayacağına dikkat edin.

   Yukarıdaki 1-3. adımları bu kez **marketing_user1** olarak yineleyin.

   **salesevents** konusundan öğeleri kullanmak için aşağıdaki komutu yürütün:

   ```bash
   /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --topic marketingspend --security-protocol PLAINTEXTSASL --from-beginning
   ```

   Önceki iletiler görülemez.

8. Ranger kullanıcı arabiriminden denetim erişimi olaylarını görüntüleyin.

   ![Ranger Kullanıcı Arabirimi Denetim İlkesi](./media/apache-domain-joined-run-kafka/apache-ranger-admin-audit.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Kafka’ya kendi anahtarınızı getirin](https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-byok)
* [Kurumsal Güvenlik Paketi ile Hadoop güvenliğine giriş](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-introduction)
