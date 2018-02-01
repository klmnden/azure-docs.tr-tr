---
title: "Apache Kafka'yı Kullanmaya Başlama - Azure HDInsight | Microsoft Docs"
description: "Azure HDInsight üzerinde Apache Kafka kümesi oluşturmayı öğrenin. Konu başlığı, abonelik ve tüketici oluşturmayı öğrenin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 43585abf-bec1-4322-adde-6db21de98d7f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/18/2018
ms.author: larryfr
ms.openlocfilehash: 6191d81d6b55f5ffe943f800be542d7ea4614eaf
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2018
---
# <a name="start-with-apache-kafka-on-hdinsight"></a>HDInsight üzerinde Apache Kafka'yı kullanmaya başlama

Azure HDInsight üzerinde [Apache Kafka](https://kafka.apache.org) kümesi oluşturmayı ve kullanmayı öğrenin. Kafka, HDInsight ile birlikte kullanılabilen, açık kaynaklı bir dağıtılmış akış platformudur. Yayımla-abone ol ileti kuyruğuna benzer işlevler sağladığı için genellikle ileti aracısı olarak kullanılır. Kafka genellikle Apache Spark ve Apache Storm ile birlikte kullanılır.

> [!NOTE]
> Kafka’nın şu anda HDInsight ile kullanılabilen iki sürümü vardır: 0.9.0 (HDInsight 3.4) ve 0.10.0 (HDInsight 3.5 ve 3.6). Bu belgedeki adımlarda HDInsight 3.6 üzerinde Kafka kullandığınız kabul edilmiştir.

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="create-a-kafka-cluster"></a>Kafka kümesi oluşturma

HDInsight kümesinde Kafka oluşturmak için aşağıdaki adımları kullanın:

1. [Azure portalında](https://portal.azure.com) **+Kaynak oluştur**, **Veri ve Analiz**, **HDInsight** adımlarını izleyin.
   
    ![HDInsight kümesi oluşturma](./media/apache-kafka-get-started/create-hdinsight.png)

2. **Temel Bilgiler** bölümünden aşağıdaki bilgileri girin:

    * **Küme Adı**: HDInsight kümesinin adı.
    * **Abonelik**: Kullanılacak abonelik.
    * **Küme oturumu kullanıcı adı** ve **Küme oturumu parolası**: HTTPS üzerinden kümeye erişirken kullanılan oturum açma bilgileri. Ambari Web kullanıcı arabirimi veya REST API gibi hizmetlere erişmek için bu kimlik bilgilerini kullanın.
    * **Güvenli Kabuk (SSH) kullanıcı adı**: SSH üzerinden kümeye erişirken kullanılan oturum açma bilgileri. Varsayılan olarak parola, küme oturum açma parolası ile aynıdır.
    * **Kaynak Grubu**: Kümenin oluşturulduğu kaynak grubu.
    * **Konum**: Kümenin oluşturulacağı Azure bölgesi.

        > [!IMPORTANT]
        > Verilerin yüksek kullanılabilirliği için, __üç hata etki alanı__ içeren bir konum (bölge) seçmenizi öneririz. Daha fazla bilgi için [Verilerin yüksek kullanılabilirliği](#data-high-availability) bölümüne bakın.
   
 ![Abonelik seçme](./media/apache-kafka-get-started/hdinsight-basic-configuration.png)

3. **Küme türü**’nü seçin, sonra **Küme yapılandırması**’ndan aşağıdaki değerleri ayarlayın:
   
    * **Küme Türü**: Kafka
    * **Sürüm**: Kafka 0.10.0 (HDI 3.6)

    Son olarak, **Seç** düğmesini kullanarak ayarları kaydedin.
     
 ![Küme türü seçme](./media/apache-kafka-get-started/set-hdinsight-cluster-type.png)

4. Küme türünü seçtikten sonra __Seç__ düğmesini kullanarak küme türünü ayarlayın. Ardından, __İleri__ düğmesini kullanarak temel yapılandırmayı tamamlayın.

5. **Depolama**’dan bir depolama hesabı seçin veya oluşturun. Bu belgedeki adımlar için diğer alanları varsayılan değerlerinde bırakın. __İleri__ düğmesini kullanarak depolama yapılandırmasını kaydedin.

    ![HDInsight depolama hesabı ayarlarını belirleme](./media/apache-kafka-get-started/set-hdinsight-storage-account.png)

6. Devam etmek için __Uygulamalar (isteğe bağlı)__ bölümünden __İleri__'yi seçin. Bu örnek için uygulama gerekmez.

7. Devam etmek için __Küme boyutu__’ndan __İleri__'yi seçin.

    > [!WARNING]
    > HDInsight üzerinde Kafka'yı kullanabilmeniz için kümenizin en az üç çalışan düğümü içermesi gerekir. Daha fazla bilgi için [Verilerin yüksek kullanılabilirliği](#data-high-availability) bölümüne bakın.

    ![Kafka kümesi boyutunu ayarlama](./media/apache-kafka-get-started/kafka-cluster-size.png)

    > [!IMPORTANT]
    > **Çalışan düğümü başına disk sayısı** girdisi, HDInsight üzerinde Kafka'nın ölçeklenebilirliğini denetler. HDInsight üzerinde Kafka, kümedeki sanal makinelerin yerel diskini kullanır. Kafka G/Ç açısından yoğun olduğundan, yüksek aktarım hızı ve düğüm başına daha fazla depolama alanı sağlamak için [Azure Yönetilen Diskler](../../virtual-machines/windows/managed-disks-overview.md) kullanılır. Yönetilen diskin türü __Standart__ (HDD) veya __Premium__ (SSD) olabilir. Premium diskler, DS ve GS serisi VM'lerle kullanılır. Diğer tüm VM türleri standart disk kullanır.

8. Devam etmek için __Gelişmiş ayarlar__’dan __İleri__'yi seçin.

9. **Özet**’ten kümenin yapılandırmasını gözden geçirin. Yanlış olan ayarları değiştirmek için __Düzenle__ bağlantılarını kullanın. Son olarak, __Oluştur__ düğmesini kullanarak kümeyi oluşturun.
   
    ![Küme yapılandırma özeti](./media/apache-kafka-get-started/hdinsight-configuration-summary.png)
   
    > [!NOTE]
    > Kümenin oluşturulması 20 dakika sürebilir.

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

> [!IMPORTANT]
> Aşağıdaki adımları gerçekleştirirken bir SSH istemcisi kullanmanız gerekir. Daha fazla bilgi için [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md) belgesine bakın.

Kümeye bağlanmak için istemcinizde SSH kullanın:

```ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net```

**SSHUSER** ifadesini küme oluşturma sırasında sağladığınız SSH kullanıcı adıyla değiştirin. **CLUSTERNAME** değerini kümenin adıyla değiştirin.

İstendiğinde, SSH hesabı için kullanılan parolayı girin.

Bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="getkafkainfo"></a>Zookeeper ve Aracı konak bilgilerini alma

Kafka ile çalışırken konak değerlerini bilmeniz gerekir; *Zookeeper* konakları ve *Aracı* konakları. Bu konaklar Kafka API’si ve Kafka ile gönderilen yardımcı programların birçoğu ile birlikte kullanılır.

Konak bilgilerini içeren ortam değişkenlerini oluşturmak için aşağıdaki adımları kullanın. Bu ortam değişkenleri bu belgedeki adımlarda kullanılır.

1. Küme ile SSH bağlantısından aşağıdaki komutu kullanarak `jq` yardımcı programını yükleyin. Bu yardımcı program JSON belgelerini ayrıştırmak için kullanılır ve aracı konak bilgilerini almak için yararlıdır:
   
    ```bash
    sudo apt -y install jq
    ```

2. Ambari’den alınan bilgilerle ortam değişkenlerini ayarlamak için aşağıdaki komutları kullanın:

    ```bash
    CLUSTERNAME='your cluster name'
    export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`

    export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`

    echo '$KAFKAZKHOSTS='$KAFKAZKHOSTS
    echo '$KAFKABROKERS='$KAFKABROKERS
    ```

    > [!IMPORTANT]
    > `CLUSTERNAME=` değerini Kafka kümesinin adı olarak ayarlayın. İstendiğinde, küme oturum açma (yönetici) hesabı için parolayı girin.

    Aşağıdaki metin `$KAFKAZKHOSTS` içeriğinin bir örneğidir:
   
    `zk0-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181,zk2-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181`
   
    Aşağıdaki metin `$KAFKABROKERS` içeriğinin bir örneğidir:
   
    `wn1-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092,wn0-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092`

    > [!NOTE]
    > `cut` komutu, konak listesini iki konak girdisine düşürmek için kullanılır. Kafka tüketicisi veya üreticisi oluştururken konakların tam listesini sağlamanız gerekmez.
   
    > [!WARNING]
    > Bu oturumdan döndürülen bilgileri her zaman doğru olarak kabul etmeyin. Kümeyi ölçeklendirirseniz yeni aracılar eklenir veya kaldırılır. Bir hata oluşur ve bir düğüm değiştirilirse, düğümün konak adı değişebilir.
    >
    > Geçerli bilgilere sahip olduğunuzdan emin olmak için, kullanmadan hemen önce Zookeeper ve aracı konakların bilgilerini almanız gerekir.

## <a name="create-a-topic"></a>Konu başlığı oluşturma

Kafka, veri akışlarını *topics* (konu başlıkları) adlı kategorilerde depolar. Kafka ile birlikte verilen betiği kullanarak, bir küme baş düğümüne yönelik SSH bağlantısından konu başlığı oluşturun:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
```

Bu komut, `$KAFKAZKHOSTS` içinde depolanan konak bilgilerini kullanarak Zookeeper’a bağlanır ve ardından **test** adlı Kafka konu başlığını oluşturur. Konu başlıklarını listelemek üzere aşağıdaki betiği kullanarak, konu başlığının oluşturulduğunu doğrulayabilirsiniz:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
```

Bu komut, **test** konu başlıklarını içeren Kafka konu başlıklarını listeler.

## <a name="produce-and-consume-records"></a>Kayıt oluşturma ve kullanma

Kafka, konu başlıklarında *records* (kayıtlar) depolar. Kayıtlar, *Üreticiler* tarafından oluşturulur ve *tüketiciler* tarafından kullanılır. Üreticiler, Kafka *aracılarına* kayıtlar üretir. HDInsight kümenizdeki her çalışan düğümü bir Kafka aracısıdır.

Daha önce oluşturduğunuz test konu başlığında kayıt depolamak ve ardından bir tüketici kullanarak bunları okumak için aşağıdaki adımları kullanın:

1. SSH oturumundan, Kafka ile birlikte sağlanan bir betik kullanarak kayıtları konu başlığına yazın:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test
    ```
   
    Bu komuttan sonra isteme geri döndürülmezsiniz. Bunun yerine, birkaç metin iletisi yazın ve ardından **Ctrl + C** tuşlarını kullanarak, konu başlığına göndermeyi durdurun. Her satır ayrı bir kayıt olarak gönderilir.

2. Konu başlığından kayıt okumak için, Kafka ile birlikte sağlanan bir betik kullanın:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic test --from-beginning
    ```
   
    Bu komutla, kayıtlar konu başlığından alınır ve görüntülenir. `--from-beginning` kullanılması, tüketiciye akışın başından başlamasını söyler, böylece tüm kayıtlar alınır.

    > [!NOTE]
    > Kafka’nın eski bir sürümünü kullanıyorsanız `--bootstrap-server $KAFKABROKERS` komutunu `--zookeeper $KAFKAZKHOSTS` ile değiştirmeniz gerekebilir.

3. Tüketiciyi durdurmak için __Ctrl + C__ tuşlarını kullanın.

Ayrıca programlı olarak üretici ve tüketici de oluşturabilirsiniz. Bu API’yi kullanmayla ilgili bir örnek için [HDInsight ile Kafka Üretici ve Tüketici API’si](apache-kafka-producer-consumer-api.md) belgesine göz atın.

## <a name="data-high-availability"></a>Verilerin yüksek kullanılabilirliği

Her Azure bölgesi (konum) _hata etki alanları_ sağlar. Hata etki alanı, bir Azure veri merkezinde temel donanımlardan oluşan mantıksal bir gruplandırmadır. Her hata etki alanı ortak bir güç kaynağı ve ağ anahtarına sahiptir. Bir HDInsight kümesi içindeki düğümleri uygulayan sanal makineler ve yönetilen diskler, bu hata etki alanlarına dağıtılır. Bu mimari, fiziksel donanım hatalarının olası etkisini sınırlar.

Bir bölgedeki hata etki alanlarının sayısı hakkında bilgi almak için [Linux sanal makinelerinin kullanılabilirliği](../../virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) belgesine bakın.

> [!IMPORTANT]
> Üç hata etki alanı içeren ve çoğaltma faktörü 3 olan bir Azure bölgesi kullanmanız önerilir.

Yalnızca iki hata etki alanı içeren bir bölge kullanmanız gerekiyorsa, çoğaltmaları iki hata etki alanına eşit oranda yaymak için çoğaltma faktörü olarak 4 kullanın.

### <a name="kafka-and-fault-domains"></a>Kafka ve hata etki alanları

Kafka, hata etki alanları ile uyumlu değildir. Konular için bölüm çoğaltmaları oluşturulurken, çoğaltmalar yüksek kullanılabilirlik için düzgün şekilde dağıtılmayabilir. Yüksek kullanılabilirlik sağlamak için [Kafka bölüm yeniden dengeleme aracını](https://github.com/hdinsight/hdinsight-kafka-tools) kullanın. Bu araç bir SSH oturumundan Kafka kümenizin baş düğümüne doğru çalıştırılmalıdır.

Kafka verilerinizin en yüksek kullanılabilirliğe sahip olmasını istiyorsanız, konu başlığınız için bölüm çoğaltmalarını aşağıdaki durumlarda yeniden dengelemeniz gerekir:

* Yeni bir konu veya bölüm oluşturulduğunda

* Bir kümenin ölçeğini artırdığınızda

## <a name="delete-the-cluster"></a>Küme silme

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](../hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, HDInsight üzerinde Apache Kafka ile çalışmanın temel bilgilerini öğrendiniz. Kafka ile çalışma hakkında daha fazla bilgi için aşağıdakileri kullanın:

* [Kafka günlüklerini çözümleme](apache-kafka-log-analytics-operations-management.md)
* [Kafka kümeleri arasında verileri çoğaltma](apache-kafka-mirroring.md)
* [HDInsight ile Kafka Üretici ve Tüketici API’si](apache-kafka-producer-consumer-api.md)
* [HDInsight ile Kafka Akışı API’si](apache-kafka-streams-api.md)
* [Apache Spark akışını (DStream) HDInsight üzerinde Kafka ile kullanma](../hdinsight-apache-spark-with-kafka.md)
* [Apache Spark Yapılandırılmış Akışını HDInsight üzerinde Kafka ile kullanma](../hdinsight-apache-kafka-spark-structured-streaming.md)
* [HDInsight üzerinde verileri Kafka’dan Cosmos DB’ye taşımak için Apache Spark Yapılandırılmış Akış’ı kullanma](../apache-kafka-spark-structured-streaming-cosmosdb.md)
* [Apache Storm’u HDInsight üzerinde Kafka ile kullanma](../hdinsight-apache-storm-with-kafka.md)
* [Azure Sanal Ağ üzerinden Kafka’ya bağlanma](apache-kafka-connect-vpn-gateway.md)
