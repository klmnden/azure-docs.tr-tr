---
title: Azure IOT Hub ile hdınsight'ta Apache Kafka kullanın | Microsoft Docs
description: Azure IOT Hub ile hdınsight'ta Apache Kafka kullanmayı öğrenin. Kafka bağlanmak Azure IOT Hub proje Kafka için bir kaynak ve havuz bağlayıcı sağlar. Kaynak bağlayıcısını IOT Hub'ından veri okuyabilir ve IOT Hub'ına havuz bağlayıcı yazar.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/15/2018
ms.author: larryfr
ms.openlocfilehash: 33fdb5b099efc40fec94a860b21cda75ced44fe9
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34267590"
---
# <a name="use-apache-kafka-on-hdinsight-with-azure-iot-hub"></a>Azure IOT Hub ile hdınsight'ta Apache Kafka kullanın

Nasıl kullanacağınızı öğrenin [Kafka bağlanmak Azure IOT Hub](https://github.com/Azure/toketi-kafka-connect-iothub) Hdınsight üzerinde Apache Kafka ve Azure IOT hub'ı arasında veri taşımak için bağlayıcı. Bu belgede, kümedeki bir kenar düğümüne IOT hub'ı Bağlayıcısı'nı çalıştırma öğrenin.

Bağlanmak Kafka API, sürekli olarak Kafka veri çekme veya veri Kafka başka bir sisteme gönderme bağlayıcıları uygulamanızı sağlar. [Kafka bağlanmak Azure IOT Hub](https://github.com/Azure/toketi-kafka-connect-iothub) Kafka Azure IOT Hub'ından veri çeker Bağlayıcıdır. Bu ayrıca veri Kafka IOT Hub'ına gönderebilir. 

IOT Hub'ından çekme, kullandığınız bir __kaynak__ bağlayıcı. IOT Hub'ına Ftp'den, kullandığınız bir __havuz__ bağlayıcı. IOT hub'ı Bağlayıcısı, hem kaynak hem de havuz bağlayıcılar sağlar.

Aşağıdaki diyagramda Bağlayıcısı'nı kullanırken, Hdınsight'ta Azure IOT Hub ve Kafka arasındaki veri akışını gösterir.

![IOT Hub'ından Kafka için bağlayıcı üzerinden akan veri gösteren görüntü](./media/apache-kafka-connector-iot-hub/iot-hub-kafka-connector-hdinsight.png)

Bağlanma API'si hakkında daha fazla bilgi için bkz: [ https://kafka.apache.org/documentation/#connect ](https://kafka.apache.org/documentation/#connect).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

* Hdınsight kümesi üzerinde Kafka. Daha fazla bilgi için [HDInsight üzerinde Kafka hızlı başlangıcı](apache-kafka-get-started.md) belgesine bakın.

* Kenar düğümüne Kafka kümedeki. Daha fazla bilgi için bkz: [Hdınsight ile kenar düğümünü kullanmak](../hdinsight-apps-use-edge-node.md) belge.

* Azure IOT hub'ı. Bu öğretici için ı öneririz [Azure IOT Hub'ına bağlanmak Raspberry Pi'yi çevrimiçi simulator](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started) belge.

* Bir SSH istemcisi. Bu belgede yer alan adımlarda, kümeye bağlanmak için SSH kullanılır. Daha fazla bilgi için [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md) belgesine bakın.

## <a name="install-the-connector"></a>Bağlayıcısı'nı yüklemek

1. En son sürümü Kafka bağlanmak için Azure IOT Hub'ından karşıdan [ https://github.com/Azure/toketi-kafka-connect-iothub/releases/ ](https://github.com/Azure/toketi-kafka-connect-iothub/releases).

2. Hdınsight kümesinde, Kafka kenar düğümüne .jar dosyasını yüklemek için aşağıdaki komutu kullanın:

    > [!NOTE]
    > Değiştir `sshuser` Hdınsight kümeniz için SSH kullanıcı hesabıyla. Değiştir `new-edgenode` kenar düğümü ada sahip. Değiştir `clustername` küme adı ile. SSH uç kenar düğümüne için daha fazla bilgi için bkz: [kenar düğümleri Hdınsight ile kullanılan](../hdinsight-apps-use-edge-node.md#access-an-edge-node) belge.

    ```bash
    scp kafka-connect-iothub-assembly*.jar sshuser@new-edgenode.clustername-ssh.azurehdinsight.net:
    ```

3. Dosya kopyalama işlemi tamamlandıktan sonra SSH kullanarak kenar düğümüne bağlanma:

    ```bash
    ssh sshuser@new-edgenode.clustername-ssh.azurehdinsight.net
    ```

    Daha fazla bilgi için [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md) belgesine bakın.

4. Kafka Bağlayıcısı'nı yüklemek için `libs` dizin, aşağıdaki komutu kullanın:

    ```bash
    sudo mv kafka-connect-iothub-assembly*.jar /usr/hdp/current/kafka-broker/libs/
    ```

> [!TIP]
> Bu belgede adımların kalanını sorunları halinde önceden derlenmiş .jar dosyasını kullanırken çalıştırırsanız, paket kaynağından oluşturma deneyin.
>
> Bağlayıcıyı oluşturmak için bir Scala geliştirme ortamı sahip [Scala yapı aracı](http://www.scala-sbt.org/).
>
> 1. Bir bağlayıcı için bir kaynak karşıdan [ https://github.com/Azure/toketi-kafka-connect-iothub/ ](https://github.com/Azure/toketi-kafka-connect-iothub/) geliştirme ortamınız için.
>
> 2. Bir komut isteminde proje dizininde, yapı ve projeyi paket için aşağıdaki komutu kullanın:
>
>    ```bash
>    sbt assembly
>    ```
>
>    Bu komut adlı bir dosya oluşturur `kafka-connect-iothub-assembly_2.11-0.6.jar` içinde `target/scala-2.11` projesi için dizin.

## <a name="configure-kafka"></a>Kafka yapılandırın

Bir SSH bağlantısı kenar düğümüne, Kafka bağlayıcı tek başına modunda çalışacak şekilde yapılandırmak için aşağıdaki adımları kullanın:

1. Küme adı bir değişkene kaydedin. Bir değişkeni kullanarak, bu bölümdeki diğer komutlar kopyala/yapıştır kolaylaştırır:

    ```bash
    read -p "Enter the Kafka on HDInsight cluster name: " CLUSTERNAME
    ```

2. Yükleme `jq` yardımcı programı. Bu yardımcı programı, Ambari sorgularından döndürülen JSON belgeleri işlemek üzere kolaylaştırır:

    ```bash
    sudo apt -y install jq
    ```

3. Aracıların Kafka adresini alın. Kümedeki birçok aracıların olabilir, ancak bir veya iki başvurmak yeterlidir. İki Aracısı ana adresini almak için aşağıdaki komutu kullanın:

    ```bash
    export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    echo $KAFKABROKERS
    ```

    İstendiğinde, küme oturum açma (yönetici) hesabı için parolayı girin. Döndürülen değer aşağıdaki metne benzer:

    `wn0-kafka.w5ijyohcxt5uvdhhuaz5ra4u5f.ex.internal.cloudapp.net:9092,wn1-kafka.w5ijyohcxt5uvdhhuaz5ra4u5f.ex.internal.cloudapp.net:9092`

4. Zookeeper düğümleri adresini alın. Kümede birçok Zookeeper düğümleri vardır, ancak bir veya iki başvurmak yeterlidir. İki Zookeeper düğümleri adresini almak için aşağıdaki komutu kullanın:

    ```bash
    export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    ```

5. Bağlayıcı tek başına modunda çalışırken `/usr/hdp/current/kafka-broker/config/connect-standalone.properties` dosya Kafka aracıları ile iletişim kurmak için kullanılır. Düzenlenecek `connect-standalone.properties` dosya, aşağıdaki komutu kullanın:

    ```bash
    sudo nano /usr/hdp/current/kafka-broker/config/connect-standalone.properties
    ```

    * Aracıların Kafka bulmak kenar düğümünü tek başına yapılandırma ile ile başlayan satırı bulun `bootstrap.servers=`. Değiştir `localhost:9092` önceki adımdan Aracısı konaklarla değeri.

    * Değişiklik `key.converter=` ve `value.converter=` aşağıdaki değerleri satırlarına:

        ```text
        key.converter=org.apache.kafka.connect.storage.StringConverter
        value.converter=org.apache.kafka.connect.storage.StringConverter
        ```

        > [!IMPORTANT]
        > Bu değişiklik ile Kafka dahil konsol üretici kullanarak test etmenizi sağlar. Diğer üreticileri ve tüketicileri için farklı dönüştürücüler gerekebilir. Diğer dönüştürücü değerleri kullanma hakkında daha fazla bilgi için bkz: [ https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md ](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md).

    * Bir satır aşağıdaki metni içeren dosyanın sonuna ekleyin:

        ```text
        consumer.max.poll.records=10
        ```

        > [!TIP]
        > Bu değişiklik, bir kerede 10 kayıt sınırlayarak havuz bağlayıcı zaman aşımlarına engellemektir. Daha fazla bilgi için bkz. [https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md).

6. Dosyayı kaydetmek için kullanın __Ctrl + X__, __Y__ve ardından __Enter__.

7. Bağlayıcı tarafından kullanılan Konular oluşturmak için aşağıdaki komutları kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic iotin --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic iotout --zookeeper $KAFKAZKHOSTS
    ```

    Doğrulamak için `iotin` ve `iotout` konuları var, aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
    ```

    `iotin` Konu IOT Hub'ından iletileri almak için kullanılır. `iotout` Konu IOT Hub'ına iletileri göndermek için kullanılır.

## <a name="get-iot-hub-connection-information"></a>IOT Hub bağlantı bilgilerini alma

Bağlayıcı tarafından kullanılan IOT hub bilgilerini almak için aşağıdaki adımları kullanın:

1. IOT hub'ınız için Event Hub ile uyumlu uç nokta adı ve Event Hub ile uyumlu uç noktasını alın. Bu bilgileri almak için aşağıdaki yöntemlerden birini kullanın:

    * __Gelen [Azure portal](https://portal.azure.com/)__, aşağıdaki adımları kullanın:

        1. IOT Hub'ına gidin ve seçin __uç noktaları__.
        2. Gelen __yerleşik uç noktaları__seçin __olayları__.
        3. Gelen __özellikleri__, aşağıdaki alanlar değerini kopyalayın:

            * __Olay Hub ile uyumlu adı__
            * __Olay Hub ile uyumlu uç noktası__
            * __Bölümler__

        > [!IMPORTANT]
        > Portal uç nokta değerinden Bu örnekte gerekmeyen ek metinler içerebilir. Bu desenle eşleşen metni ayıklayın `sb://<randomnamespace>.servicebus.windows.net/`.

    * __Gelen [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)__, aşağıdaki komutu kullanın:

        ```azure-cli
        az iot hub show --name myhubname --query "{EventHubCompatibleName:properties.eventHubEndpoints.events.path,EventHubCompatibleEndpoint:properties.eventHubEndpoints.events.endpoint,Partitions:properties.eventHubEndpoints.events.partitionCount}"
        ```

        Değiştir `myhubname` IOT hub'ınızın adıyla. Yanıt aşağıdakine benzer:

        ```text
        "EventHubCompatibleEndpoint": "sb://ihsuprodbnres006dednamespace.servicebus.windows.net/",
        "EventHubCompatibleName": "iothub-ehub-myhub08-207673-d44b2a856e",
        "Partitions": 2
        ```

2. Alma __paylaşılan erişim ilkesi__ ve __anahtar__. Bu örneğin __hizmet__ anahtarı. Bu bilgileri almak için aşağıdaki yöntemlerden birini kullanın:

    * __Gelen [Azure portal](https://portal.azure.com/)__, aşağıdaki adımları kullanın:

        1. Seçin __paylaşılan erişim ilkeleri__ve ardından __hizmet__.
        2. Kopya __birincil anahtar__ değeri.
        3. Kopya __bağlantı dizesi--birincil anahtar__ değeri.

    * __Gelen [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)__, aşağıdaki komutu kullanın:

        1. Birincil anahtar değeri almak için aşağıdaki komutu kullanın:

            ```azure-cli
            az iot hub policy show --hub-name myhubname --name service --query "primaryKey"
            ```

            Değiştir `myhubname` IOT hub'ınızın adıyla. Yanıt için birincil anahtardır `service` bu hub için ilke.

        2. Bağlantı dizesini almak için `service` İlkesi, aşağıdaki komutu kullanın:

            ```azure-cli
            az iot hub show-connection-string --name myhubname --policy-name service --query "connectionString"
            ```

            Değiştir `myhubname` IOT hub'ınızın adıyla. Bağlantı dizesidir yanıt `service` ilkesi.

## <a name="configure-the-source-connection"></a>Kaynak bağlantısı yapılandırma

IOT Hub ile çalışmaya kaynağını yapılandırmak için kenar düğümüne bir SSH bağlantısı aşağıdaki eylemleri gerçekleştirin:

1. Bir kopyasını oluşturmak `connect-iot-source.properties` dosyasını `/usr/hdp/current/kafka-broker/config/` dizin. Toketi-kafka-connect-iothub projeden dosyasını karşıdan yüklemek için aşağıdaki komutu kullanın:

    ```bash
    sudo wget -P /usr/hdp/current/kafka-broker/config/ https://raw.githubusercontent.com/Azure/toketi-kafka-connect-iothub/master/connect-iothub-source.properties
    ```

2. Düzenlenecek `connect-iot-source.properties` dosya ve IOT hub bilgi eklemek için aşağıdaki komutu kullanın:

    ```bash
    sudo nano /usr/hdp/current/kafka-broker/config/connect-iothub-source.properties
    ```

    Düzenleyicisi'ni bulun ve aşağıdaki girdileri değiştirin:

    * `Kafka.Topic=PLACEHOLDER`: Değiştirin `PLACEHOLDER` ile `iotin`. IOT hub'ından alınan iletilerin yerleştirilir `iotin` konu.
    * `IotHub.EventHubCompatibleName=PLACEHOLDER`: Değiştirin `PLACEHOLDER` Event Hub ile uyumlu ada sahip.
    * `IotHub.EventHubCompatibleEndpoint=PLACEHOLDER`: Değiştirin `PLACEHOLDER` Event Hub ile uyumlu uç noktası ile.
    * `IotHub.Partitions=PLACEHOLDER`: Değiştirin `PLACEHOLDER` önceki adımlardan bölüm sayısı ile.
    * `IotHub.AccessKeyName=PLACEHOLDER`: Değiştirin `PLACEHOLDER` ile `service`.
    * `IotHub.AccessKeyValue=PLACEHOLDER`: Değiştirin `PLACEHOLDER` birincil anahtarı ile `service` ilkesi.
    * `IotHub.StartType=PLACEHOLDER`: Değiştirin `PLACEHOLDER` UTC tarihi. Bağlayıcı iletileri denetleme başlatıldığında bu tarihidir. Tarih biçimi `yyyy-mm-ddThh:mm:ssZ`.
    * `BatchSize=100`: Değiştirin `100` ile `5`. Bu değişiklik, IOT hub'ında beş yeni iletiler sonra Kafka iletileri okumak bağlayıcı neden olur.

    Bir örnek yapılandırma için bkz: [ https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Source.md ](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Source.md).

3. Değişiklikleri kaydetmek için kullanın __Ctrl + X__, __Y__ve ardından __Enter__.

Bağlayıcı kaynak yapılandırma hakkında daha fazla bilgi için bkz: [ https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Source.md ](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Source.md).

## <a name="configure-the-sink-connection"></a>Havuz bağlantı yapılandırma

IOT Hub ile çalışmaya havuz bağlantı yapılandırmak için kenar düğümüne bir SSH bağlantısı aşağıdaki eylemleri gerçekleştirin:

1. Bir kopyasını oluşturmak `connect-iothub-sink.properties` dosyasını `/usr/hdp/current/kafka-broker/config/` dizin. Toketi-kafka-connect-iothub projeden dosyasını karşıdan yüklemek için aşağıdaki komutu kullanın:

    ```bash
    sudo wget -P /usr/hdp/current/kafka-broker/config/ https://raw.githubusercontent.com/Azure/toketi-kafka-connect-iothub/master/connect-iothub-sink.properties
    ```

2. Düzenlenecek `connect-iothub-sink.properties` dosya ve IOT hub bilgi eklemek için aşağıdaki komutu kullanın:

    ```bash
    sudo nano /usr/hdp/current/kafka-broker/config/connect-iothub-sink.properties
    ```

    Düzenleyicisi'ni bulun ve aşağıdaki girdileri değiştirin:

    * `topics=PLACEHOLDER`: Değiştirin `PLACEHOLDER` ile `iotout`. Yazılan iletilerin `iotout` konu, IOT hub'ına iletilir.
    * `IotHub.ConnectionString=PLACEHOLDER`: Değiştirin `PLACEHOLDER` için bağlantı dizesiyle `service` ilkesi.

    Bir örnek yapılandırma için bkz: [ https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md ](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md).

3. Değişiklikleri kaydetmek için kullanın __Ctrl + X__, __Y__ve ardından __Enter__.

Bağlayıcı havuz yapılandırma hakkında daha fazla bilgi için bkz: [ https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md ](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md).

## <a name="start-the-source-connector"></a>Kaynak Bağlayıcısı'nı Başlat

Kaynak Bağlayıcısı'nı başlatmak için bir SSH bağlantısı kenar düğümüne aşağıdaki komutu kullanın:

```bash
/usr/hdp/current/kafka-broker/bin/connect-standalone.sh /usr/hdp/current/kafka-broker/config/connect-standalone.properties /usr/hdp/current/kafka-broker/config/connect-iothub-source.properties
```

Bağlayıcı başladıktan sonra iletileri, aygıttan IOT hub'ına gönderir. Bağlayıcı IOT hub'ından iletileri okur ve onları Kafka konusunda depolar gibi konsola bilgileri günlüğe kaydeder:

```text
[2017-08-29 20:15:46,112] INFO Polling for data - Obtained 5 SourceRecords from IotHub (com.microsoft.azure.iot.kafka.co
nnect.IotHubSourceTask:39)
[2017-08-29 20:15:54,106] INFO Finished WorkerSourceTask{id=AzureIotHubConnector-0} commitOffsets successfully in 4 ms (
org.apache.kafka.connect.runtime.WorkerSourceTask:356)
```

> [!NOTE]
> Bağlayıcısı başlatılırken birkaç uyarıları görebilirsiniz. Bu uyarılar IOT hub'ından iletileri alma ile ilgili sorunlara neden olmaz.

Bağlayıcı durdurmak için kullanma __Ctrl + C__.

## <a name="start-the-sink-connector"></a>Havuz Bağlayıcısı'nı Başlat

Bir SSH bağlantısı kenar düğümüne, havuz Bağlayıcısı'nı tek başına modunda başlatmak için aşağıdaki komutu kullanın:

```bash
/usr/hdp/current/kafka-broker/bin/connect-standalone.sh /usr/hdp/current/kafka-broker/config/connect-standalone.properties /usr/hdp/current/kafka-broker/config/connect-iothub-sink.properties
```

Bağlayıcı çalışırken, aşağıdakine benzer bilgiler görüntülenir:

```text
[2017-08-30 17:49:16,150] INFO Started tasks to send 1 messages to devices. (com.microsoft.azure.iot.kafka.connect.sink.
IotHubSinkTask:47)
[2017-08-30 17:49:16,150] INFO WorkerSinkTask{id=AzureIotHubSinkConnector-0} Committing offsets (org.apache.kafka.connec
t.runtime.WorkerSinkTask:262)
```

> [!NOTE]
> Bağlayıcısı başlatılırken birkaç uyarıları fark edebilirsiniz. Bunları güvenle yok sayabilirsiniz.

Bağlayıcı aracılığıyla iletileri göndermek için aşağıdaki adımları kullanın:

1. Kafka küme başka bir SSH oturumu açın:

    ```bash
    ssh sshuser@new-edgenode.clustername-ssh.azurehdinsight.net
    ```
2. İleti göndermek için `iotout` konu, aşağıdaki komutu kullanın:

    > [!WARNING]
    > Bu yeni bir SSH bağlantı olduğundan `$KAFKABROKERS` değişkeni herhangi bir bilgi içermiyor. Ayarlamak için aşağıdaki yöntemlerden birini kullanın:
    >
    > * İlk üç adımlarda [yapılandırma Kafka](#configure-kafka) bölümü.
    > * Kullanım `echo $KAFKABROKERS` değerleri alın ve sonra değiştirmek için önceki SSH bağlantı `$KAFKABROKERS` gerçek değerlerle aşağıdaki komutta.

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic iotout
    ```

    Bu komut normal Bash komutuna döndürmez. Bunun yerine, klavye girdisini gönderir `iotout` konu.

3. Cihazınız için bir ileti göndermek için bir JSON belgesi SSH oturumu için yapıştırın `kafka-console-producer`.

    > [!IMPORTANT]
    > Değerini ayarlamalısınız `"deviceId"` girişi Cihazınızı kimliği. Aşağıdaki örnekte, cihazın adlı `fakepi`:

    ```text
    {"messageId":"msg1","message":"Turn On","deviceId":"fakepi"}
    ```

    Bu JSON belgesi için şemayı daha ayrıntılı anlatılan [ https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md ](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md).

    Sanal Raspberry Pi'yi cihaz kullanıyorsanız ve çalışıyorsa, şu iletiyi aygıt tarafından kaydedilir:

    ```text
    Receive message: Turn On
    ```

    JSON belgesini yeniden gönderme, ancak değerini değiştirme `"message"` girişi. Yeni değer aygıt tarafından kaydedilir.

Havuz Bağlayıcısı'nı kullanma hakkında daha fazla bilgi için bkz: [ https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md ](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Kafka bağlanmak API Hdınsight'ta IOT Kafka Bağlayıcısı'nı başlatmak için nasıl kullanılacağı hakkında bilgi edindiniz. Kafka ile çalışmak için diğer yollarını keşfetmek için aşağıdaki bağlantıları kullanın:

* [Apache Spark’ı HDInsight üzerinde Kafka ile kullanma](../hdinsight-apache-spark-with-kafka.md)
* [Apache Storm’u HDInsight üzerinde Kafka ile kullanma](../hdinsight-apache-storm-with-kafka.md)
