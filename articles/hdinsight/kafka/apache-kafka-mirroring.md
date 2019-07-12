---
title: Apache Kafka konularını - Azure HDInsight yansıtma
description: İkincil bir kümeye konuları yansıtarak bir HDInsight kümesinde Kafka kopyasını korumak için Apache Kafka'nın yansıtma özelliğini kullanmayı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/24/2019
ms.openlocfilehash: bdc393d041bd40fd27493ccc8f3c4f39adfa35b2
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657113"
---
# <a name="use-mirrormaker-to-replicate-apache-kafka-topics-with-kafka-on-hdinsight"></a>HDInsight üzerinde Kafka ile Apache Kafka konularını çoğaltma MirrorMaker kullanın

İkincil bir kümeye konuları çoğaltmak için Apache Kafka'nın yansıtma özelliğini kullanmayı öğrenin. Yansıtma sürekli bir işlem olarak çalışan veya aralıklı olarak geçirme yöntemi olarak kullanılan bir kümeden veri.

Bu örnekte, yansıtma konular iki HDInsight kümeleri arasında çoğaltmak için kullanılır. Farklı veri merkezlerinde farklı sanal ağlardaki iki kümeleridir.

> [!WARNING]  
> Yansıtma, hataya dayanıklılık elde etmek için bir yol değerlendirilmemelidir. Uzaklık içindeki bir konuya öğeler için birincil ve ikincil küme arasında farklı nedenle istemciler birbirinin yerine iki kullanamazsınız.
>
> Hataya dayanıklılık hakkında endişeleriniz varsa, çoğaltma konular için küme içinde ayarlamanız gerekir. Daha fazla bilgi için [HDInsight üzerinde Apache Kafka ile çalışmaya başlama](apache-kafka-get-started.md).

## <a name="how-apache-kafka-mirroring-works"></a>Apache Kafka ile yansıtma nasıl çalışır

Kullanarak çalışır yansıtma [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) konuları birincil kümesindeki kayıtları kullanma ve ardından ikincil küme üzerinde yerel bir kopyasını oluşturmak için aracı (Apache Kafka parçası). MirrorMaker kullanan bir (veya daha fazla) *tüketiciler* birincil kümeden okuyan ve *üretici* yerel (ikincil) kümesine yazar.

Kafka kümeleri farklı Azure bölgelerindeki en kullanışlı yansıtma olağanüstü durum kurtarma Kurulumu kullanır. Bunu başarmak için kümeler bulunduğu sanal ağ birbirine eşlenir.

Aşağıdaki diyagramda yansıtma işlemi ve nasıl kümeleri iletişimin akış gösterilmektedir:

![Yansıtma işlem diyagramı](./media/apache-kafka-mirroring/kafka-mirroring-vnets2.png)

Birincil ve ikincil küme düğümlerine ve bölümlerine sayısında farklı olabilir ve uzaklık içinde konuları da farklıdır. Kayıt siparişi anahtar başına temelinde korunacak şekilde yansıtma bölümleme için kullanılan anahtar değerini korur.

### <a name="mirroring-across-network-boundaries"></a>Ağ sınırları ötesinde yansıtma

Farklı ağlarda Kafka kümeleri arasında yansıtmak gerekiyorsa, aşağıdaki ek hususlar vardır:

* **Ağ geçitleri**: Ağları TCP/IP'yi düzeyinde iletişim kurabilmesi gerekir.

* **Sunucu adresi**: Kendi IP adresleri veya tam etki alanı adlarını kullanarak Küme düğümlerinizi adres seçebilirsiniz.

    * **IP adresleri**: IP adresi reklam kullanmak için Kafka kümeleri yapılandırırsanız, IP adresleri aracı düğümleri ve zookeeper düğümleri kullanarak yansıtma kuruluma devam edebilirsiniz.
    
    * **Etki alanı adları**: Kafka kümeleriniz için IP adresi reklam yapılandırmazsanız, kümelerin tam olarak nitelikli etki alanı adlarını (FQDN) kullanarak birbirine mümkün olması gerekir. Bu, diğer ağlara istekleri iletmek üzere yapılandırıldığı her ağındaki bir etki alanı adı sistemi (DNS) sunucusu gerektirir. Bir Azure sanal ağ ile sağlanan otomatik DNS kullanmak yerine ağ oluştururken, özel bir DNS sunucusu ve sunucunun IP adresini belirtmeniz gerekir. Sanal ağ oluşturulduktan sonra ardından bu IP adresini kullanan bir Azure sanal makine oluşturma sonra yükleyin ve DNS yazılım üzerinde yapılandırmanız gerekir.

    > [!WARNING]  
    > Oluşturun ve HDInsight sanal ağa yüklemeden önce özel bir DNS sunucusu yapılandırın. Sanal ağ için yapılandırılan DNS sunucusunun kullanılacağını HDInsight için gereken ek yapılandırma yoktur.

İki Azure sanal ağları bağlama hakkında daha fazla bilgi için bkz. [bir VNet-VNet bağlantısını yapılandırma](../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).

## <a name="mirroring-architecture"></a>Yansıtma mimarisi

Bu mimari iki farklı kaynak grupları ve sanal ağları kümelerinde özellikleri: bir **birincil** ve **ikincil**.

### <a name="creation-steps"></a>Oluşturma adımları

1. İki yeni kaynak grubu oluşturun:

    |Kaynak Grubu | Location |
    |---|---|
    | kafka-primary-rg | Orta ABD |
    | kafka ikincil rg | Orta Kuzey ABD |

1. Yeni bir sanal ağ oluşturma **kafka birincil vnet** içinde **kafka-primary-rg**. Varsayılan ayarları bırakın.
1. Yeni bir sanal ağ oluşturma **kafka ikincil vnet** içinde **kafka ikincil rg**, varsayılan ayarlarla de.

1. İki yeni Kafka kümeleri oluşturun:

    | Küme adı | Kaynak Grubu | Sanal Ağ | Depolama Hesabı |
    |---|---|---|---|
    | kafka birincil küme | kafka-primary-rg | kafka birincil vnet | kafkaprimarystorage |
    | kafka ikincil küme | kafka ikincil rg | kafka ikincil vnet | kafkasecondarystorage |

1. Sanal Ağ eşlemesi oluşturun. Bu adımda, iki eşlemeler oluşturulmaktadır: birinden **kafka birincil vnet** için **kafka ikincil vnet** ve bir yedekleme **kafka ikincil vnet** için  **kafka birincil vnet**.
    1. Seçin **kafka birincil vnet** sanal ağ.
    1. Tıklayın **eşlemeler** altında **ayarları**.
    1.           **Ekle**'yi tıklatın.
    1. Üzerinde **ekleme eşlemesi** ekranında, aşağıdaki ekran görüntüsünde gösterildiği gibi ayrıntılarını girin.

        ![vnet Eşlemesi Ekle](./media/apache-kafka-mirroring/add-vnet-peering.png)

1. IP reklam yapılandırın:
    1. Birincil küme için Ambari panoya gidin: `https://PRIMARYCLUSTERNAME.azurehdinsight.net`.
    1. Tıklayın **Hizmetleri** > **Kafka**. Tıklayın **yapılandırmaları** sekmesi.
    1. Aşağıdaki yapılandırma satırları altına eklemek **kafka env şablon** bölümü. **Kaydet**’e tıklayın.
    
        ```
        # Configure Kafka to advertise IP addresses instead of FQDN
        IP_ADDRESS=$(hostname -i)
        echo advertised.listeners=$IP_ADDRESS
        sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
        echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
        ```

    1. Bir not girin **yapılandırmayı Kaydet** tıklayın ve ekranı **Kaydet**.
    1. Yapılandırma uyarısı ile istenirse tıklayın **yine de devam**.
    1. Tıklayın **Tamam** üzerinde **yapılandırma değişikliklerini kaydetmek**.
    1. Tıklayın **yeniden** > **yeniden tüm etkilenen** içinde **yeniden başlatma gerekli** bildirim. Tıklayın **yeniden başlatmayı Onayla tüm**.

        ![kafka düğümleri yeniden başlatın](./media/apache-kafka-mirroring/ambari-restart-notification.png)

1. Kafka, tüm ağ arabirimleri üzerinde dinleyecek şekilde yapılandırın.
    1. Kalın **yapılandırmaları** sekmesinde altında **Hizmetleri** > **Kafka**. İçinde **Kafka Aracısı** bölümünde kümesi **dinleyicileri** özelliğini `PLAINTEXT://0.0.0.0:9092`.
    1. **Kaydet**’e tıklayın.
    1. Tıklayın **yeniden**, ve **yeniden başlatmayı Onayla tüm**.

1. Kayıt Aracısı IP adresleri ve Zookeeper için birincil küme yöneliktir.
    1. Tıklayın **konakları** Ambari Panoda.
    1. Aracıları ve Zookeepers için IP adreslerini not edin. Aracı düğüme sahip **aşağı** ana bilgisayar adını ve zookeeper ilk iki harf düğümünüz **zk** ana bilgisayar adının ilk iki harf olarak.

        ![IP adreslerini görüntüle](./media/apache-kafka-mirroring/view-node-ip-addresses2.png)

1. İkinci küme için önceki üç adımı yineleyin **kafka ikincil küme**: IP reklam yapılandırma ve ayarlama dinleyicileri aracısı ve Zookeeper IP adreslerini not edin.

## <a name="create-topics"></a>Konu oluşturma

1. Bağlanma **birincil** SSH kullanarak kümeye:

    ```bash
    ssh sshuser@PRIMARYCLUSTER-ssh.azurehdinsight.net
    ```

    Değiştirin **sshuser** ile kümeyi oluştururken kullanılan SSH kullanıcı adı. Değiştirin **BASENAME** kümeyi oluştururken kullanılan temel adına sahip.

    Bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Apache Zookeeper ana birincil küme için bir değişken oluşturmak için aşağıdaki komutu kullanın. Dizeleri ister `ZOOKEEPER_IP_ADDRESS1` daha önce kaydedilen gerçek IP adreslerini ile değiştirilmelidir gibi `10.23.0.11` ve `10.23.0.7`. FQDN çözümleme ile özel bir DNS sunucusu kullanıyorsanız, izleyin [adımları](apache-kafka-get-started.md#getkafkainfo) aracısı ve zookeeper adları alınamadı.:

    ```bash
    # get the zookeeper hosts for the primary cluster
    export PRIMARY_ZKHOSTS='ZOOKEEPER_IP_ADDRESS1:2181, ZOOKEEPER_IP_ADDRESS2:2181, ZOOKEEPER_IP_ADDRESS3:2181'
    ```

3. Adlı bir konu oluşturmak için `testtopic`, aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic testtopic --zookeeper $PRIMARY_ZKHOSTS
    ```

3. Konu oluşturulduğunu doğrulamak için aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $PRIMARY_ZKHOSTS
    ```

    Yanıtı içeren `testtopic`.

4. Bu Zookeeper konak bilgilerini görüntülemek için aşağıdakileri kullanın ( **birincil**) küme:

    ```bash
    echo $PRIMARY_ZKHOSTS
    ```

    Bu bilgiler aşağıdaki metne benzer döndürür:

    `10.23.0.11:2181,10.23.0.7:2181,10.23.0.9:2181`

    Bu bilgileri kaydedin. Sonraki bölümde kullanılır.

## <a name="configure-mirroring"></a>Yansıtmasını yapılandırma

1. Bağlanma **ikincil** farklı bir SSH oturumundan küme:

    ```bash
    ssh sshuser@SECONDARYCLUSTER-ssh.azurehdinsight.net
    ```

    Değiştirin **sshuser** ile kümeyi oluştururken kullanılan SSH kullanıcı adı. Değiştirin **SECONDARYCLUSTER** kümeyi oluştururken kullandığınız ada sahip.

    Bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. A `consumer.properties` ile iletişimi yapılandırmak için kullanılan dosya **birincil** kümesi. Dosyayı oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    nano consumer.properties
    ```

    Aşağıdaki metni içeriğini kullanın `consumer.properties` dosyası:

    ```yaml
    zookeeper.connect=PRIMARY_ZKHOSTS
    group.id=mirrorgroup
    ```

    Değiştirin **PRIMARY_ZKHOSTS** Zookeeper IP adreslerinden ile **birincil** kümesi.

    Bu dosya, birincil Kafka kümeden okunurken kullanılacak tüketici bilgileri açıklar. Daha fazla bilgi tüketici yapılandırma için bkz: [tüketici yapılandırmaları](https://kafka.apache.org/documentation#consumerconfigs) kafka.apache.org adresindeki.

    Dosyayı kaydetmek için kullanın **Ctrl + X**, **Y**, ardından **Enter**.

3. İkincil küme ile iletişim kuran üretici yapılandırmadan önce kurulum Aracısı IP adresleri için bir değişken **ikincil** kümesi. Bu değişken oluşturmak için aşağıdaki komutları kullanın:

    ```bash
    export SECONDARY_BROKERHOSTS='BROKER_IP_ADDRESS1:9092,BROKER_IP_ADDRESS2:9092,BROKER_IP_ADDRESS2:9092'
    ```

    Komut `echo $SECONDARY_BROKERHOSTS` bilgileri aşağıdaki metne benzer döndürmesi gerekir:

    `10.23.0.14:9092,10.23.0.4:9092,10.23.0.12:9092`

4. A `producer.properties` iletişim kurmak için kullanılan dosya **ikincil** kümesi. Dosyayı oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    nano producer.properties
    ```

    Aşağıdaki metni içeriğini kullanın `producer.properties` dosyası:

    ```yaml
    bootstrap.servers=SECONDARY_BROKERHOSTS
    compression.type=none
    ```

    Değiştirin **SECONDARY_BROKERHOSTS** önceki adımda kullanılan Aracısı IP adreslerine sahip.

    Daha fazla bilgi üretici yapılandırma için bkz: [üretici yapılandırmaları](https://kafka.apache.org/documentation#producerconfigs) kafka.apache.org adresindeki.

5. Bir ortam değişkenini Zookeeper konak kümesinin ikincil IP adresleriyle oluşturmak için aşağıdaki komutları kullanın:

    ```bash
    # get the zookeeper hosts for the secondary cluster
    export SECONDARY_ZKHOSTS='ZOOKEEPER_IP_ADDRESS1:2181,ZOOKEEPER_IP_ADDRESS2:2181,ZOOKEEPER_IP_ADDRESS3:2181'
    ```

7. HDInsight üzerinde Kafka için varsayılan yapılandırma konuları otomatik olarak oluşturulmasını izin vermez. Yansıtma işlemini başlatmadan önce aşağıdaki seçeneklerden birini kullanmanız gerekir:

    * **Konular üzerinde ikincil küme oluşturma**: Bu seçenek, bölümler ve çoğaltma faktörü sayısını ayarlamanızı sağlar.

        Aşağıdaki komutu kullanarak önceden konuları oluşturabilirsiniz:

        ```bash
        /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic testtopic --zookeeper $SECONDARY_ZKHOSTS
        ```

        Değiştirin `testtopic` oluşturmak için konu adına sahip.

    * **Küme için otomatik konu oluşturmayı yapılandırma**: Bunları farklı sayıda bölüm ve çoğaltma faktörü birincil konuyu daha oluşturun ancak bu seçenek MirrorMaker konuları, otomatik olarak oluşturmasını sağlar.

        Konular otomatik olarak oluşturmak için ikincil küme yapılandırmak için aşağıdaki adımları gerçekleştirin:

        1. İkincil küme için Ambari panoya gidin: `https://SECONDARYCLUSTERNAME.azurehdinsight.net`.
        1. Tıklayın **Hizmetleri** > **Kafka**. Tıklayın **yapılandırmaları** sekmesi.
        5. İçinde __filtre__ değeri alanına, `auto.create`. Bu özellikler ve görüntüler listesini filtreler `auto.create.topics.enable` ayarı.
        6. Değiştirin `auto.create.topics.enable` true ve ardından __Kaydet__. Not ekleyin ve ardından __Kaydet__ yeniden.
        7. Seçin __Kafka__ hizmetini seçin __yeniden__ve ardından __etkilenen tüm yeniden başlatma__. Sorulduğunda, __Onayla yeniden tüm__.

        ![konu otomatik oluşturma yapılandırma](./media/apache-kafka-mirroring/kafka-enable-auto-create-topics.png)

## <a name="start-mirrormaker"></a>MirrorMaker'ı Başlat

1. İçin SSH bağlantısından **ikincil** küme, MirrorMaker işlemini başlatmak için aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-run-class.sh kafka.tools.MirrorMaker --consumer.config consumer.properties --producer.config producer.properties --whitelist testtopic --num.streams 4
    ```

    Bu örnekte kullanılan parametreler şunlardır:

    * **--consumer.config**: Tüketici özellikleri içeren dosyayı belirtir. Bu özellikler okur bir tüketici oluşturmak için kullanılan *birincil* Kafka kümesi.

    * **--producer.config**: Üretici özellikleri içeren dosyayı belirtir. Bu özellikler yazan bir üretici oluşturmak için kullanılan *ikincil* Kafka kümesi.

    * **--beyaz liste**: MirrorMaker birincil kümeden ikincil çoğaltır konuların listesi.

    * **--num.streams**: Oluşturmak için tüketici iş parçacığı sayısı.

    Tüketici ikincil düğümünde artık iletileri almak için bekliyor.

2. İçin SSH bağlantısından **birincil** küme, bir üretici başlatmak ve konuya ileti göndermek için aşağıdaki komutu kullanın:

    ```bash
    export PRIMARY_BROKERHOSTS=BROKER_IP_ADDRESS1:9092,BROKER_IP_ADDRESS2:9092,BROKER_IP_ADDRESS2:9092
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $SOURCE_BROKERHOSTS --topic testtopic
    ```

     Boş bir satır ile imleç ulaşırsınız zaman içinde birkaç metin iletisi yazın. İletiler konu başlığına gönderilen **birincil** kümesi. İşiniz bittiğinde, kullanın **Ctrl + C** üretici işlemi sonlandırmak için.

3. İçin SSH bağlantısından **ikincil** küme, kullanın **Ctrl + C** MirrorMaker işlemi sonlandırmak için. Bu işlemi sonlandırmak için birkaç saniye sürebilir. İletileri ikincil çoğaltıldığını doğrulamak için aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $SECONDARY_ZKHOSTS --topic testtopic --from-beginning
    ```

    Artık konuların listesini içeren `testtopic`, ikincil birincil kümeden konuya MirrorMaster yansıtan zaman oluşturuldu. Konu başlığından alınan iletileri birincil kümede girdiğiniz olanlarla aynıdır.

## <a name="delete-the-cluster"></a>Küme silme

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

Bu belgedeki adımlarda kümeleri farklı bir Azure kaynak grubunda oluşturuldu. Oluşturulan tüm kaynakları silmek için oluşturulan iki kaynak gruplarını silebilirsiniz: **kafka-primary-rg** ve **kafka secondary_rg**. Kaynak grupları silme kümeleri, sanal ağlar ve depolama hesapları dahil olmak üzere bu belgede, aşağıdaki tarafından oluşturulan tüm kaynakları kaldırır.

## <a name="next-steps"></a>Sonraki Adımlar

Bu belgede, nasıl kullanacağınızı öğrendiniz [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) bir kopyasını oluşturmak için bir [Apache Kafka](https://kafka.apache.org/) kümesi. Kafka ile çalışmak için diğer yollarını bulmak için aşağıdaki bağlantıları kullanın:

* [Apache Kafka MirrorMaker belgeleri](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) cwiki.apache.org konumunda.
* [HDInsight üzerinde Apache Kafka ile çalışmaya başlama](apache-kafka-get-started.md)
* [HDInsight üzerinde Apache Kafka ile Apache Spark kullanma](../hdinsight-apache-spark-with-kafka.md)
* [Apache Storm'u HDInsight üzerinde Apache Kafka ile kullanma](../hdinsight-apache-storm-with-kafka.md)
* [Apache Kafka ile bir Azure sanal ağına bağlanma](apache-kafka-connect-vpn-gateway.md)
