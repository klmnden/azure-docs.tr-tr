---
title: "Apache Kafka konuları - Azure Hdınsight yansıtma | Microsoft Docs"
description: "İkincil bir küme konulara yansıtma tarafından Kafka Hdınsight kümesinde bir kopyasını korumak için Apache Kafka'nın yansıtma özelliğini kullanmayı öğrenin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 015d276e-f678-4f2b-9572-75553c56625b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/07/2017
ms.author: larryfr
ms.openlocfilehash: 6a197dc7abbb70ad632cab25291504628bc8e32c
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="use-mirrormaker-to-replicate-apache-kafka-topics-with-kafka-on-hdinsight-preview"></a>Kafka (Önizleme) hdınsight'ta Apache Kafka konuları çoğaltmak için MirrorMaker kullanın

İkincil bir kümeye konuları çoğaltmak için Apache Kafka'nın yansıtma özelliğini kullanmayı öğrenin. Yansıtma bulunabilir sürekli bir işlem olarak çalışan, veya zaman zaman geçiş işleminin bir yöntem olarak kullanılan bir kümeden veri.

Bu örnekte, yansıtma konular iki Hdınsight kümeleri arasında çoğaltmak için kullanılır. Her iki kümeleri aynı bölgede bir Azure sanal ağındaki ' dir.

> [!WARNING]
> Yansıtma hataya dayanıklılık sağlamak için bir yöntem düşünülmemelidir. Bir konu içindeki öğeleri uzaklık kaynak ve hedef kümeler arasında farklı istemcileri iki birbirinin yerine kullanamazlar.
>
> Hataya dayanıklılık hakkında endişeleriniz varsa, küme içinde çoğaltma konular için ayarlamanız gerekir. Daha fazla bilgi için bkz: [Kafka hdınsight kullanmaya başlama](apache-kafka-get-started.md).

## <a name="how-kafka-mirroring-works"></a>Yansıtma Kafka nasıl çalışır?

Yansıtma için Works MirrorMaker aracını (Apache Kafka parçası) kullanarak kayıtları kaynak kümede konulardan kullanmak ve sonra hedef kümede yerel bir kopyasını oluşturun. MirrorMaker kullanan bir (veya daha fazla) *tüketicileri* kaynak kümeden okuyan ve *üretici* yerel (hedef) kümeye yazar.

Aşağıdaki diyagram yansıtma işlemini gösterir:

![Yansıtma işlemi diyagramı](./media/apache-kafka-mirroring/kafka-mirroring.png)

Hdınsight üzerinde Apache Kafka, ortak internet üzerinden Kafka hizmetine erişim sağlamaz. Kafka üreticileri veya tüketicileri Kafka kümedeki düğümlerin aynı Azure sanal ağ içinde olmalıdır. Bu örnekte, Kafka kaynak ve hedef kümeler Azure sanal ağında yer alır. Aşağıdaki diyagramda, iletişim kümeleri arasında nasıl aktığını gösterir:

![Kaynak ve hedef Kafka diyagramı bir Azure sanal ağında kümeleri](./media/apache-kafka-mirroring/spark-kafka-vnet.png)

Kaynak ve hedef küme düğümlerini ve bölümleri sayısında farklı olabilir ve uzaklıkları konuları içinde de farklıdır. Kayıt siparişi bir anahtar başına temelinde korunacak şekilde yansıtma bölümleme için kullanılan anahtar değerini korur.

### <a name="mirroring-across-network-boundaries"></a>Ağ sınırları boyunca yansıtma

Farklı ağlarda Kafka kümeler arasında yansıtma gerekiyorsa, aşağıdaki dikkat edilecek diğer noktalar vardır:

* **Ağ geçitleri**: ağları TCPIP düzeyinde iletişim kurabilmesi gerekir.

* **Ad çözümlemesi**: her ağ Kafka kümelerde sağlayabilmelidir ana bilgisayar adları kullanarak birbirine bağlamak. Bu diğer ağlara istekleri iletmek için yapılandırılan her ağındaki bir etki alanı adı sistemi (DNS) sunucusu gerektirebilir.

    Bir Azure sanal ağ ile sağlanan otomatik DNS kullanmak yerine ağ oluşturulurken özel bir DNS sunucusu ve sunucunun IP adresini belirtmeniz gerekir. Sanal ağ oluşturulduktan sonra ardından bu IP adresini kullanan bir Azure sanal makine oluşturmak daha sonra yüklemek ve DNS yazılım üzerinde yapılandırmanız gerekir.

    > [!WARNING]
    > Oluşturun ve Hdınsight sanal ağınıza yüklemeden önce özel DNS sunucusunu yapılandırın. Sanal ağ için yapılandırılan DNS sunucusunun kullanılacağını Hdınsight için gereken ek yapılandırma yoktur.

İki Azure sanal ağları bağlama ile ilgili daha fazla bilgi için bkz: [VNet-VNet bağlantı yapılandırma](../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).

## <a name="create-kafka-clusters"></a>Kafka kümeleri oluşturma

Bir Azure sanal ağı oluşturmak ve Kafka el ile kümeleri olsa da, bir Azure Resource Manager şablonunu kullanmak daha kolaydır. Bir Azure sanal ağı ve iki Kafka kümeleri Azure aboneliğinize dağıtmak için aşağıdaki adımları kullanın.

1. Azure'da oturum açın ve Azure portalında şablon açmak için aşağıdaki düğmesini kullanın.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json" target="_blank"><img src="./media/apache-kafka-mirroring/deploy-to-azure.png" alt="Deploy to Azure"></a>
   
    Azure Resource Manager şablonu bulunur **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json**.

    > [!WARNING]
    > HDInsight üzerinde Kafka'yı kullanabilmeniz için kümenizin en az üç çalışan düğümü içermesi gerekir. Bu şablon, üç alt düğümleri içeren bir Kafka küme oluşturur.

2. Üzerinde girişleri doldurmak için aşağıdaki bilgileri kullanın **özel dağıtım** dikey penceresinde:
    
    ![Hdınsight özel dağıtım](./media/apache-kafka-mirroring/parameters.png)
    
    * **Kaynak grubu**: bir grup oluşturun veya varolan bir tanesini seçin. Bu grup, Hdınsight kümesi içerir.

    * **Konum**: coğrafi olarak yakın bir konum seçin.
     
    * **Temel küme adı**: Kafka kümeleri için bu değer taban adı olarak kullanılır. Örneğin, **hdı** adlı kümeleri oluşturur **kaynak hdı** ve **taşınmaya hdı**.

    * **Oturum açma kullanıcı adı küme**: kaynak ve hedef için yönetici kullanıcı adı Kafka kümeleri.

    * **Oturum açma parolası küme**: kaynak ve hedef için yönetici kullanıcı parolası Kafka kümeleri.

    * **SSH kullanıcı adı**: kaynak ve hedef Kafka için kümeleri oluşturmak için SSH kullanıcı.

    * **SSH parolası**: kaynak ve hedef SSH kullanıcı için parola Kafka kümeleri.

3. Okuma **hüküm ve koşullar**ve ardından **hüküm ve koşulları yukarıda belirtildiği ediyorum**.

4. Son olarak, denetleme **panoya Sabitle** ve ardından **satın alma**. Kümeleri oluşturmak için yaklaşık 20 dakika sürer.

> [!IMPORTANT]
> Hdınsight kümeleri adını **kaynak BASENAME** ve **taşınmaya BASENAME**, BASENAME şablona verdiğiniz adı olduğu. Kümeye bağlanırken bu adları daha sonraki adımlarda kullanın.

## <a name="create-topics"></a>Konuları oluşturun

1. Bağlanmak **kaynak** SSH kullanarak küme:

    ```bash
    ssh sshuser@source-BASENAME-ssh.azurehdinsight.net
    ```

    Değiştir **sshuser** küme oluşturulurken kullanılan SSH kullanıcı adı. Değiştir **BASENAME** küme oluşturulurken kullanılan taban adına sahip.

    Bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Kaynak kümenin Zookeeper ana bilgisayarları bulmak için aşağıdaki komutları kullanın:

    ```bash
    # Install jq if it is not installed
    sudo apt -y install jq
    # get the zookeeper hosts for the source cluster
    export SOURCE_ZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    ```

    Değiştir `$CLUSTERNAME` kaynak kümesi adı. İstendiğinde, küme oturum açma (Yönetici) hesabı için parolayı girin.

3. Adlı bir konu başlığı oluşturmak için `testtopic`, aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic testtopic --zookeeper $SOURCE_ZKHOSTS
    ```

3. Konu oluşturulduğunu doğrulamak için aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $SOURCE_ZKHOSTS
    ```

    Yanıtı içeren `testtopic`.

4. Bu Zookeeper ana bilgisayar bilgilerini görüntülemek için aşağıdakini kullanın ( **kaynak**) küme:

    ```bash
    echo $SOURCE_ZKHOSTS
    ```

    Bu bilgiler aşağıdaki metni benzer döndürür:

    `zk0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181,zk1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181`

    Bu bilgileri kaydedin. Sonraki bölümde kullanılır.

## <a name="configure-mirroring"></a>Yansıtmasını yapılandırma

1. Bağlanmak **hedef** küme farklı bir SSH oturumu kullanarak:

    ```bash
    ssh sshuser@dest-BASENAME-ssh.azurehdinsight.net
    ```

    Değiştir **sshuser** küme oluşturulurken kullanılan SSH kullanıcı adı. Değiştir **BASENAME** küme oluşturulurken kullanılan taban adına sahip.

    Bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. A `consumer.properties` ile iletişim yapılandırmak için kullanılan dosya **kaynak** küme. Dosyası oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    nano consumer.properties
    ```

    Aşağıdaki metni içeriğini kullanmak `consumer.properties` dosyası:

    ```yaml
    zookeeper.connect=SOURCE_ZKHOSTS
    group.id=mirrorgroup
    ```

    Değiştir **SOURCE_ZKHOSTS** Zookeeper konakları bilgilerle **kaynak** küme.

    Bu dosya kaynak Kafka küme okurken kullanılacak tüketici bilgiler açıklanmaktadır. Daha fazla bilgi tüketici yapılandırma için bkz: [tüketici yapılandırmalar](https://kafka.apache.org/documentation#consumerconfigs) kafka.apache.org adresindeki.

    Dosyayı kaydetmek için kullanın **Ctrl + X**, **Y**ve ardından **Enter**.

3. Hedef küme ile iletişim kurar üretici yapılandırmadan önce ana bilgisayarlar için aracı bulmalıdır **hedef** küme. Bu bilgileri almak için aşağıdaki komutları kullanın:

    ```bash
    sudo apt -y install jq
    DEST_BROKERHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    echo $DEST_BROKERHOSTS
    ```

    Değiştir `$CLUSTERNAME` hedef küme adı. İstendiğinde, küme oturum açma (Yönetici) hesabı için parolayı girin.

    `echo` Komut bilgilerini aşağıdaki metni benzer döndürür:

        wn0-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn1-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092

4. A `producer.properties` iletişim kurmak için kullanılan dosya __hedef__ küme. Dosyası oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    nano producer.properties
    ```

    Aşağıdaki metni içeriğini kullanmak `producer.properties` dosyası:

    ```yaml
    bootstrap.servers=DEST_BROKERS
    compression.type=none
    ```

    Değiştir **DEST_BROKERS** önceki adımdan Aracısı bilgilerle.

    Daha fazla bilgi üretici yapılandırma için bkz: [üretici yapılandırmalar](https://kafka.apache.org/documentation#producerconfigs) kafka.apache.org adresindeki.

## <a name="start-mirrormaker"></a>MirrorMaker Başlat

1. SSH bağlantı **hedef** küme, MirrorMaker işlemini başlatmak için aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-run-class.sh kafka.tools.MirrorMaker --consumer.config consumer.properties --producer.config producer.properties --whitelist testtopic --num.streams 4
    ```

    Bu örnekte kullanılan parametreler şunlardır:

    * **--consumer.config**: tüketici özellikler içeren dosyayı belirtir. Bu özellikleri okur bir tüketici oluşturmak için kullanılan *kaynak* Kafka küme.

    * **--producer.config**: üretici özellikler içeren dosyayı belirtir. Bu özellikler Yazar bir üretici oluşturmak için kullanılan *hedef* Kafka küme.

    * **--beyaz liste**: MirrorMaker hedef kaynak kümeden çoğaltır konuların listesi.

    * **--num.streams**: oluşturmak için tüketici iş parçacığı sayısı.

 Başlangıç sırasında aşağıdaki metni benzer bilgi MirrorMaker döndürür:

    ```json
    {metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-3, security.protocol=PLAINTEXT}{metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-0, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-kafka.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-2, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-1, security.protocol=PLAINTEXT}
    ```

2. SSH bağlantı **kaynak** küme, bir üretici başlatmak ve konu başlığına ileti göndermek için aşağıdaki komutu kullanın:

    ```bash
    SOURCE_BROKERHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $SOURCE_BROKERHOSTS --topic testtopic
    ```

    Değiştir `$CLUSTERNAME` kaynak kümesi adı. İstendiğinde, küme oturum açma (Yönetici) hesabı için parolayı girin.

     Boş bir satırı bir imleç ile geldiğinde, birkaç metin iletileri yazın. İletiler konu başlığına gönderilen **kaynak** küme. İşiniz bittiğinde, kullanın **Ctrl + C** üretici işlemi sonlandırmak için.

3. SSH bağlantı **hedef** küme, kullanın **Ctrl + C** MirrorMaker işlemi sonlandırmak için. Konu ve iletileri hedefe çoğaltıldığını doğrulamak için aşağıdaki komutları kullanın:

    ```bash
    DEST_ZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $DEST_ZKHOSTS
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $DEST_ZKHOSTS --topic testtopic --from-beginning
    ```

    Değiştir `$CLUSTERNAME` hedef küme adı. İstendiğinde, küme oturum açma (Yönetici) hesabı için parolayı girin.

    Konu listesi şimdi içerir `testtopic`, hedef kaynak kümesinden konuya MirrorMaster yansıtan zaman oluşturuldu. Konusundan alınan iletileri kaynak kümede girilen adıyla aynıdır.

## <a name="delete-the-cluster"></a>Küme silme

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

Bu belgede yer alan adımlar her iki kümeleri aynı Azure kaynak grubu oluşturmak için Azure portalında kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi bu belge, Azure sanal ağ ve depolama hesabı kümeleri tarafından kullanılan uygulayarak oluşturduğunuz tüm kaynakları kaldırır.

## <a name="next-steps"></a>Sonraki Adımlar

Bu belgede, MirrorMaker Kafka küme bir kopyasını oluşturmak için nasıl kullanılacağı hakkında bilgi edindiniz. Kafka ile çalışmak için diğer yollarını keşfetmek için aşağıdaki bağlantıları kullanın:

* [Apache Kafka MirrorMaker belgelerine](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) cwiki.apache.org adresindeki.
* [Hdınsight üzerinde Apache Kafka kullanmaya başlama](apache-kafka-get-started.md)
* [Apache Spark’ı HDInsight üzerinde Kafka ile kullanma](../hdinsight-apache-spark-with-kafka.md)
* [Apache Storm’u HDInsight üzerinde Kafka ile kullanma](../hdinsight-apache-storm-with-kafka.md)
* [Azure Sanal Ağ üzerinden Kafka’ya bağlanma](apache-kafka-connect-vpn-gateway.md)
