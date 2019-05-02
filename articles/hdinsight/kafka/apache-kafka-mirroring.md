---
title: Apache Kafka konularını - Azure HDInsight yansıtma
description: İkincil bir kümeye konuları yansıtarak bir HDInsight kümesinde Kafka kopyasını korumak için Apache Kafka'nın yansıtma özelliğini kullanmayı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/01/2018
ms.openlocfilehash: ba04ed7c95cbf00d5996ef237d3ac65053da0662
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64727381"
---
# <a name="use-mirrormaker-to-replicate-apache-kafka-topics-with-kafka-on-hdinsight"></a>HDInsight üzerinde Kafka ile Apache Kafka konularını çoğaltma MirrorMaker kullanın

İkincil bir kümeye konuları çoğaltmak için Apache Kafka'nın yansıtma özelliğini kullanmayı öğrenin. Yansıtma sürekli bir işlem olarak çalışan veya aralıklı olarak geçirme yöntemi olarak kullanılan bir kümeden veri.

Bu örnekte, yansıtma konular iki HDInsight kümeleri arasında çoğaltmak için kullanılır. Bir Azure sanal ağ aynı bölgedeki iki kümeleridir.

> [!WARNING]  
> Yansıtma, hataya dayanıklılık elde etmek için bir yol değerlendirilmemelidir. Uzaklık içindeki bir konuya öğelerine kaynak ve hedef kümeler arasında farklı nedenle istemciler birbirinin yerine iki kullanamazsınız.
>
> Hataya dayanıklılık hakkında endişeleriniz varsa, çoğaltma konular için küme içinde ayarlamanız gerekir. Daha fazla bilgi için [HDInsight üzerinde Apache Kafka ile çalışmaya başlama](apache-kafka-get-started.md).

## <a name="how-apache-kafka-mirroring-works"></a>Apache Kafka ile yansıtma nasıl çalışır

Kullanarak çalışır yansıtma [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) Aracı (Apache Kafka parçası) kaynak kümede konulardan, kayıtları ve ardından hedef kümede yerel bir kopyasını oluşturun. MirrorMaker kullanan bir (veya daha fazla) *tüketiciler* kaynak kümesinden okuyan ve *üretici* (hedef) yerel kümeye yazar.

Aşağıdaki diyagramda, yansıtma işlemini gösterir:

![Yansıtma işlem diyagramı](./media/apache-kafka-mirroring/kafka-mirroring.png)

HDInsight üzerinde Apache Kafka, genel internet üzerinden Kafka hizmetine erişim sağlamaz. Kafka üreticileri veya tüketiciler Kafka kümesindeki düğümler aynı Azure sanal ağ içinde olmalıdır. Bu örnekte, Kafka kaynak ve hedef kümeler, bir Azure sanal ağında yer alır. Aşağıdaki diyagramda, nasıl kümeleri iletişimin akış gösterilmektedir:

![Bir Azure sanal ağında diyagramı kaynak ve hedef Kafka kümeleri](./media/apache-kafka-mirroring/spark-kafka-vnet.png)

Kaynak ve hedef küme düğümlerine ve bölümlerine sayısında farklı olabilir ve uzaklık içinde konuları da farklıdır. Kayıt siparişi anahtar başına temelinde korunacak şekilde yansıtma bölümleme için kullanılan anahtar değerini korur.

### <a name="mirroring-across-network-boundaries"></a>Ağ sınırları ötesinde yansıtma

Farklı ağlarda Kafka kümeleri arasında yansıtmak gerekiyorsa, aşağıdaki ek hususlar vardır:

* **Ağ geçitleri**: Ağları TCPIP düzeyinde iletişim kurabilmesi gerekir.

* **Ad çözümlemesi**: Her ağ içinde Kafka kümeleri ana bilgisayar adları kullanarak birbirine mümkün olması gerekir. Bu, diğer ağlara istekleri iletmek üzere yapılandırıldığı her ağındaki bir etki alanı adı sistemi (DNS) sunucusu gerektirebilir.

    Bir Azure sanal ağ ile sağlanan otomatik DNS kullanmak yerine ağ oluştururken, özel bir DNS sunucusu ve sunucunun IP adresini belirtmeniz gerekir. Sanal ağ oluşturulduktan sonra ardından bu IP adresini kullanan bir Azure sanal makine oluşturma sonra yükleyin ve DNS yazılım üzerinde yapılandırmanız gerekir.

    > [!WARNING]  
    > Oluşturun ve HDInsight sanal ağa yüklemeden önce özel bir DNS sunucusu yapılandırın. Sanal ağ için yapılandırılan DNS sunucusunun kullanılacağını HDInsight için gereken ek yapılandırma yoktur.

İki Azure sanal ağları bağlama hakkında daha fazla bilgi için bkz. [bir VNet-VNet bağlantısını yapılandırma](../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).

## <a name="create-apache-kafka-clusters"></a>Apache Kafka kümeleri oluşturma

Bir Azure sanal ağı oluşturabilirsiniz ve el ile Kafka kümeleri olsa da bir Azure Resource Manager şablonu kullanmak daha kolaydır. Bir Azure sanal ağı ve iki Kafka kümeleri, Azure aboneliğinize dağıtmak için aşağıdaki adımları kullanın.

1. Aşağıdaki düğmeyi kullanarak Azure'da oturum açın ve şablonu Azure portalında açın.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json" target="_blank"><img src="./media/apache-kafka-mirroring/deploy-to-azure.png" alt="Deploy to Azure"></a>
   
    Azure Resource Manager şablonu **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json** sayfasında bulunur.

    > [!WARNING]  
    > HDInsight üzerinde Kafka'yı kullanabilmeniz için kümenizin en az üç çalışan düğümü içermesi gerekir. Bu şablon, üç çalışan düğümü içeren bir Kafka kümesi oluşturur.

2. Girişleri doldurmak için aşağıdaki bilgileri kullanın **özel dağıtım** dikey penceresinde:
    
    ![HDInsight özel dağıtım](./media/apache-kafka-mirroring/parameters.png)
    
    * **Kaynak grubu**: Bir grup oluşturun veya var olanı seçin. Bu grup, HDInsight kümesi içerir.

    * **Konum**: Coğrafi olarak yakın bir konum seçin.
     
    * **Temel küme adı**: Bu değer, Kafka kümeleri için temel adı olarak kullanılır. Örneğin, girme **hdı** adlandırılmış kümeler oluşturur **kaynak hdı** ve **dest hdı**.

    * **Küme oturum açma kullanıcı adı**: Kaynak ve hedef için yönetici kullanıcı adı, Kafka kümeleri.

    * **Küme oturum açma parolası**: Kaynak ve hedef için yönetici kullanıcı parolası, Kafka kümeleri.

    * **SSH kullanıcı adı**: İçin kaynak ve hedef Kafka kümeleri oluşturmak için SSH kullanıcısı.

    * **SSH parolası**: Kaynak ve hedef SSH kullanıcısının parolasını Kafka kümeleri.

3. **Hüküm ve Koşullar**’ı okuyun ve ardından **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum**’u seçin.

4. Son olarak, **Panoya sabitle**’yi işaretleyin ve **Satın Al**’ı seçin. Kümelerin oluşturulması yaklaşık 20 dakika sürer.

> [!IMPORTANT]  
> HDInsight kümeleri adı **kaynak BASENAME** ve **dest BASENAME**, BASENAME şablona verdiğiniz adı olduğu yer. Kümeye bağlanırken bu adlar daha sonraki adımlarda kullanın.

## <a name="create-topics"></a>Konu oluşturma

1. Bağlanma **kaynak** SSH kullanarak kümeye:

    ```bash
    ssh sshuser@source-BASENAME-ssh.azurehdinsight.net
    ```

    Değiştirin **sshuser** ile kümeyi oluştururken kullanılan SSH kullanıcı adı. Değiştirin **BASENAME** kümeyi oluştururken kullanılan temel adına sahip.

    Bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Kaynak kümesi için Apache Zookeeper konakları bulmak için aşağıdaki komutları kullanın:

    ```bash
    # Install jq if it is not installed
    sudo apt -y install jq
    # get the zookeeper hosts for the source cluster
    export SOURCE_ZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    ```

    Değiştirin `$CLUSTERNAME` ile kaynak kümesinin adı. İstendiğinde, küme oturum açma (yönetici) hesabı için parolayı girin.

3. Adlı bir konu oluşturmak için `testtopic`, aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic testtopic --zookeeper $SOURCE_ZKHOSTS
    ```

3. Konu oluşturulduğunu doğrulamak için aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $SOURCE_ZKHOSTS
    ```

    Yanıtı içeren `testtopic`.

4. Bu Zookeeper konak bilgilerini görüntülemek için aşağıdakileri kullanın ( **kaynak**) küme:

    ```bash
    echo $SOURCE_ZKHOSTS
    ```

    Bu bilgiler aşağıdaki metne benzer döndürür:

    `zk0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181,zk1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181`

    Bu bilgileri kaydedin. Sonraki bölümde kullanılır.

## <a name="configure-mirroring"></a>Yansıtmasını yapılandırma

1. Bağlanma **hedef** farklı bir SSH oturumundan küme:

    ```bash
    ssh sshuser@dest-BASENAME-ssh.azurehdinsight.net
    ```

    Değiştirin **sshuser** ile kümeyi oluştururken kullanılan SSH kullanıcı adı. Değiştirin **BASENAME** kümeyi oluştururken kullanılan temel adına sahip.

    Bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. A `consumer.properties` ile iletişimi yapılandırmak için kullanılan dosya **kaynak** kümesi. Dosyayı oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    nano consumer.properties
    ```

    Aşağıdaki metni içeriğini kullanın `consumer.properties` dosyası:

    ```yaml
    zookeeper.connect=SOURCE_ZKHOSTS
    group.id=mirrorgroup
    ```

    Değiştirin **SOURCE_ZKHOSTS** Zookeeper konak bilgilerle **kaynak** kümesi.

    Bu dosya, Kafka kümesi kaynaktan okunurken kullanılacak tüketici bilgileri açıklar. Daha fazla bilgi tüketici yapılandırma için bkz: [tüketici yapılandırmaları](https://kafka.apache.org/documentation#consumerconfigs) kafka.apache.org adresindeki.

    Dosyayı kaydetmek için kullanın **Ctrl + X**, **Y**, ardından **Enter**.

3. Hedef küme iletişim kuran üretici yapılandırmadan önce ana bilgisayar için aracı bulmalıdır **hedef** kümesi. Bu bilgileri almak için aşağıdaki komutları kullanın:

    ```bash
    sudo apt -y install jq
    DEST_BROKERHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    echo $DEST_BROKERHOSTS
    ```

    Değiştirin `$CLUSTERNAME` hedef küme adı. İstendiğinde, küme oturum açma (yönetici) hesabı için parolayı girin.

    `echo` Komut bilgileri aşağıdaki metne benzer döndürür:

        wn0-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn1-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092

4. A `producer.properties` iletişim kurmak için kullanılan dosya __hedef__ kümesi. Dosyayı oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    nano producer.properties
    ```

    Aşağıdaki metni içeriğini kullanın `producer.properties` dosyası:

    ```yaml
    bootstrap.servers=DEST_BROKERS
    compression.type=none
    ```

    Değiştirin **DEST_BROKERS** önceki adımdan aracısı bilgileri.

    Daha fazla bilgi üretici yapılandırma için bkz: [üretici yapılandırmaları](https://kafka.apache.org/documentation#producerconfigs) kafka.apache.org adresindeki.

5. Zookeeper konakları için hedef küme bulmak için aşağıdaki komutları kullanın:

    ```bash
    # Install jq if it is not installed
    sudo apt -y install jq
    # get the zookeeper hosts for the source cluster
    export DEST_ZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    ```

    Değiştirin `$CLUSTERNAME` hedef küme adı. İstendiğinde, küme oturum açma (yönetici) hesabı için parolayı girin.

7. HDInsight üzerinde Kafka için varsayılan yapılandırma konuları otomatik olarak oluşturulmasını izin vermez. Yansıtma işlemini başlatmadan önce aşağıdaki seçeneklerden birini kullanmanız gerekir:

    * **Konular hedef kümede oluşturma**: Bu seçenek, bölümler ve çoğaltma faktörü sayısını ayarlamanızı sağlar.

        Aşağıdaki komutu kullanarak önceden konuları oluşturabilirsiniz:

        ```bash
        /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic testtopic --zookeeper $DEST_ZKHOSTS
        ```

        Değiştirin `testtopic` oluşturmak için konu adına sahip.

    * **Küme için otomatik konu oluşturmayı yapılandırma**: Bunları farklı sayıda bölüm ve çoğaltma faktörü kaynak konuyu daha oluşturun ancak bu seçenek MirrorMaker konuları, otomatik olarak oluşturmasını sağlar.

        Konular otomatik olarak oluşturmak için hedef küme yapılandırmak için aşağıdaki adımları gerçekleştirin:

        1. Gelen [Azure portalında](https://portal.azure.com), Kafka kümesi hedef seçin.
        2. Küme genel bakış'tan seçin __küme Panosu__. Ardından __HDInsight küme Panosu__. İstendiğinde, küme için oturum açma (Yönetici) kimlik bilgilerini kullanarak kimlik doğrulaması.
        3. Seçin __Kafka__ sayfasının sol taraftaki listeden hizmet.
        4. Seçin __yapılandırmaları__ sayfanın ortasındaki.
        5. İçinde __filtre__ değeri alanına, `auto.create`. Bu özellikler ve görüntüler listesini filtreler `auto.create.topics.enable` ayarı.
        6. Değiştirin `auto.create.topics.enable` true ve ardından __Kaydet__. Not ekleyin ve ardından __Kaydet__ yeniden.
        7. Seçin __Kafka__ hizmetini seçin __yeniden__ve ardından __etkilenen tüm yeniden başlatma__. Sorulduğunda, __Onayla yeniden tüm__.

## <a name="start-mirrormaker"></a>MirrorMaker'ı Başlat

1. İçin SSH bağlantısından **hedef** küme, MirrorMaker işlemini başlatmak için aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-run-class.sh kafka.tools.MirrorMaker --consumer.config consumer.properties --producer.config producer.properties --whitelist testtopic --num.streams 4
    ```

    Bu örnekte kullanılan parametreler şunlardır:

    * **--consumer.config**: Tüketici özellikleri içeren dosyayı belirtir. Bu özellikler okur bir tüketici oluşturmak için kullanılan *kaynak* Kafka kümesi.

    * **--producer.config**: Üretici özellikleri içeren dosyayı belirtir. Bu özellikler yazan bir üretici oluşturmak için kullanılan *hedef* Kafka kümesi.

    * **--beyaz liste**: MirrorMaker hedef kaynak kümeden çoğaltır konuların listesi.

    * **--num.streams**: Oluşturmak için tüketici iş parçacığı sayısı.

   Başlangıç sırasında aşağıdaki metne benzer bilgiler MirrorMaker döndürür:

    ```json
    {metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-3, security.protocol=PLAINTEXT}{metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-0, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-kafka.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-2, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-1, security.protocol=PLAINTEXT}
    ```

2. İçin SSH bağlantısından **kaynak** küme, bir üretici başlatmak ve konuya ileti göndermek için aşağıdaki komutu kullanın:

    ```bash
    SOURCE_BROKERHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $SOURCE_BROKERHOSTS --topic testtopic
    ```

    Değiştirin `$CLUSTERNAME` ile kaynak kümesinin adı. İstendiğinde, küme oturum açma (yönetici) hesabı için parolayı girin.

     Boş bir satır ile imleç ulaşırsınız zaman içinde birkaç metin iletisi yazın. İletiler konu başlığına gönderilen **kaynak** kümesi. İşiniz bittiğinde, kullanın **Ctrl + C** üretici işlemi sonlandırmak için.

3. İçin SSH bağlantısından **hedef** küme, kullanın **Ctrl + C** MirrorMaker işlemi sonlandırmak için. Bu işlemi sonlandırmak için birkaç saniye sürebilir. İletileri hedefe çoğaltıldığını doğrulamak için aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $DEST_ZKHOSTS --topic testtopic --from-beginning
    ```

    Değiştirin `$CLUSTERNAME` hedef küme adı. İstendiğinde, küme oturum açma (yönetici) hesabı için parolayı girin.

    Artık konuların listesini içeren `testtopic`, hedef kaynak kümesinden konuya MirrorMaster yansıtan zaman oluşturuldu. Konu başlığından alınan iletileri kaynak kümede girilen ile aynıdır.

## <a name="delete-the-cluster"></a>Küme silme

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

Bu belgedeki adımlarda, aynı Azure kaynak grubu içinde her iki küme oluşturma olduğundan, Azure portalında kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, bu belge, Azure sanal ağ ve küme tarafından kullanılan depolama hesabı'nı izleyerek oluşturulan tüm kaynakları kaldırır.

## <a name="next-steps"></a>Sonraki Adımlar

Bu belgede, nasıl kullanacağınızı öğrendiniz [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) bir kopyasını oluşturmak için bir [Apache Kafka](https://kafka.apache.org/) kümesi. Kafka ile çalışmak için diğer yollarını bulmak için aşağıdaki bağlantıları kullanın:

* [Apache Kafka MirrorMaker belgeleri](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) cwiki.apache.org konumunda.
* [HDInsight üzerinde Apache Kafka ile çalışmaya başlama](apache-kafka-get-started.md)
* [HDInsight üzerinde Apache Kafka ile Apache Spark kullanma](../hdinsight-apache-spark-with-kafka.md)
* [Apache Storm'u HDInsight üzerinde Apache Kafka ile kullanma](../hdinsight-apache-storm-with-kafka.md)
* [Apache Kafka ile bir Azure sanal ağına bağlanma](apache-kafka-connect-vpn-gateway.md)
