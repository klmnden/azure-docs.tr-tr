---
title: 'Öğretici: Apache Storm, Apache Kafka - Azure HDInsight ile veri okuma ve yazma için kullanın'
description: HDInsight üzerinde Apache Storm ve Apache Kafka kullanarak akış işlem hattı oluşturmayı öğrenin. Bu öğreticide, Kafka'dan veri akışı yapmak için KafkaBolt ve KafkaSpout bileşenlerini kullanırsınız.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 06/13/2019
ms.openlocfilehash: fba9159fc4752a701c891fbe92a2e7e8023f0a54
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165942"
---
# <a name="tutorial-use-apache-storm-with-apache-kafka-on-hdinsight"></a>Öğretici: Apache Storm'u HDInsight üzerinde Apache Kafka ile kullanma

Bu öğreticide nasıl kullanılacağını gösterir. bir [Apache Storm](https://storm.apache.org/) ile veri okuma ve yazma için topoloji [Apache Kafka](https://kafka.apache.org/) HDInsight üzerinde. Bu öğretici Ayrıca verileri kalıcı hale getirmek nasıl gösterir [Apache Hadoop HDFS](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html) Storm kümesinde uyumlu depolama.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Storm ve Kafka
> * Kodu anlama
> * Kafka ve Storm kümeleri oluşturma
> * Topoloji oluşturma
> * Topolojiyi yapılandırma
> * Kafka konusu oluşturma
> * Topolojileri başlatma
> * Topolojileri durdurma
> * Kaynakları temizleme

## <a name="prerequisites"></a>Önkoşullar

* Kafka konuları oluşturmayı bilme. Daha fazla bilgi için [HDInsight üzerinde Kafka hızlı başlangıcı](./kafka/apache-kafka-get-started.md) belgesine bakın.

* Storm çözümleri (topolojileri) oluşturmayı ve dağıtmayı bilme. Özellikle, kullandığınız topolojileri [Apache Storm Flux](https://storm.apache.org/releases/current/flux.html) framework. Daha fazla bilgi için [Java'da bir Apache Storm topolojisi oluşturma](./storm/apache-storm-develop-java-topology.md) belge.

* [Java JDK 1.8](https://www.oracle.com/technetwork/pt/java/javase/downloads/jdk8-downloads-2133151.html) veya üstü. HDInsight 3.5 veya üstü için Java 8 gerekir.

* [Maven 3.x](https://maven.apache.org/download.cgi)

* SSH istemcisi (`ssh` ve `scp` komutları gerekir) - Bilgi için bkz. [HDInsight ile SSH'yi kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

Dağıtım iş istasyonunuza Java ve JDK yüklerken aşağıdaki ortam değişkenleri ayarlanabilir. Ancak, bunların mevcut olup olmadığını ve sisteminiz için doğru değerleri içerip içermediğini denetlemeniz gerekir.

* `JAVA_HOME` - JDK’nın yüklendiği dizine işaret etmelidir.
* `PATH` - aşağıdaki yolları içermelidir:
  
    * `JAVA_HOME` (veya eşdeğer yol).
    * `JAVA_HOME\bin` (veya eşdeğer yol).
    * Maven'ın yüklendiği dizin.

> [!IMPORTANT]  
> Bu belgede yer alan adımlar hem HDInsight üzerinde Storm hem de HDInsight kümesi üzerinde Kafka içeren bir Azure kaynak grubu gerektirir. Bu kümelerin her ikisi de Storm kümesinin Kafka kümesiyle doğrudan iletişim kurmasına olanak tanıyan bir Azure Sanal Ağı içinde bulunur.
> 
> Size kolaylık sağlamak için bu belgede, tüm gerekli Azure kaynaklarını oluşturabilecek bir şablonun bağlantıları sağlanır. 
>
> Sanal ağ üzerinde HDInsight kullanma hakkında daha fazla bilgi için [Sanal ağ kullanarak HDInsight’ı genişletme](hdinsight-extend-hadoop-virtual-network.md) belgesine bakın.

## <a name="storm-and-kafka"></a>Storm ve Kafka

Apache Storm, Apache Kafka ile çalışmak için çeşitli bileşenleri sağlar. Bu öğreticide aşağıdaki bileşenler kullanılır:

* `org.apache.storm.kafka.KafkaSpout`: Bu bileşen, Kafka'dan veri okur. Bu bileşen, aşağıdaki bileşenlere dayanır:

    * `org.apache.storm.kafka.SpoutConfig`: Spout bileşeni için yapılandırma sağlar.

    * `org.apache.storm.spout.SchemeAsMultiScheme` ve `org.apache.storm.kafka.StringScheme`: Nasıl kafka'dan veri Storm tanımlama grubu dönüştürülür.

* `org.apache.storm.kafka.bolt.KafkaBolt`: Bu bileşen, Kafka için verileri yazar. Bu bileşen, aşağıdaki bileşenlere dayanır:

    * `org.apache.storm.kafka.bolt.selector.DefaultTopicSelector`: Yazılan konu açıklar.

    * `org.apache.kafka.common.serialization.StringSerializer`: Bolt verileri bir dize değeri olarak seri hale getirme için yapılandırır.

    * `org.apache.storm.kafka.bolt.mapper.FieldNameBasedTupleToKafkaMapper`: Storm topolojisini Kafka'da depolanmış alanlara içinde kullanılan tanımlama grubu veri yapısı haritaları.

Bu bileşenler `org.apache.storm : storm-kafka` paketinde sağlanır. Storm sürümüyle eşleşen paket sürümünü kullanın. HDInsight 3.6 için, Storm sürümü 1.1.0'dır.
Ayrıca, ek Kafka bileşenlerini içeren `org.apache.kafka : kafka_2.10` paketi de gereklidir. Kafka sürümüyle eşleşen paket sürümünü kullanın. HDInsight 3.6 için 1.1.1 Kafka sürümüdür.

Aşağıdaki XML bağımlılık bildirimidir `pom.xml` için bir [Apache Maven](https://maven.apache.org/) proje:

```xml
<!-- Storm components for talking to Kafka -->
<dependency>
    <groupId>org.apache.storm</groupId>
    <artifactId>storm-kafka</artifactId>
    <version>1.1.0</version>
</dependency>
<!-- needs to be the same Kafka version as used on your cluster -->
<dependency>
    <groupId>org.apache.kafka</groupId>
    <artifactId>kafka_2.10</artifactId>
    <version>1.1.1</version>
    <!-- Exclude components that are loaded from the Storm cluster at runtime -->
    <exclusions>
        <exclusion>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
        </exclusion>
        <exclusion>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
        </exclusion>
        <exclusion>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

## <a name="understanding-the-code"></a>Kodu anlama

Bu belgede kullanılan kod [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka) adresinde sağlanır.

Bu öğreticide iki topoloji sağlanmaktadır:

* Kafka-yazıcı: Rastgele cümleler oluşturur ve bunları Kafka'ya depolar.

* Kafka okuyucusu: Kafka'dan verileri okur ve Storm kümesi için HDFS uyumlu bir dosya deposunda depolanır.

    > [!WARNING]  
    > Storm'un HDInsight tarafından kullanılan HDFS uyumlu depolamada çalışmasını sağlamak için, bir betik eylemi gerekir. Betik, Storm için çeşitli jar dosyalarını `extlib` yoluna yükler. Bu öğreticideki şablon, küme oluşturma sırasında betiği otomatik olarak kullanır.
    >
    > Storm kümesini oluşturmak için bu belgedeki şablonu kullanmazsanız, betik eylemini kümenize el ile uygulamanız gerekir.
    >
    > Betik eylemi şu konumdadır [ https://hdiconfigactions.blob.core.windows.net/linuxstormextlibv01/stormextlib.sh ](https://hdiconfigactions.blob.core.windows.net/linuxstormextlibv01/stormextlib.sh) ve Storm kümesinin gözetmen ve nimbus düğümleri uygulanır. Betik eylemlerini kullanma hakkında daha fazla bilgi için, [Betik eylemlerini kullanarak HDInsight'ı özelleştirme](hdinsight-hadoop-customize-cluster-linux.md) belgesine bakın.

Topolojiler [Flux](https://storm.apache.org/releases/1.1.2/flux.html) kullanılarak tanımlanır. Flux Storm 0.10.x sürümünde kullanıma sunulmuştur ve topoloji yapılandırmasını koddan ayırmanıza olanak tanır. Flux çerçevesini kullanan Topolojiler için, topoloji YAML dosyasında tanımlanır. YAML dosyası topolojinin bir parçası olarak eklenebilir. Ayrıca, topolojiyi gönderirken kullandığınız tek başına bir dosya da olabilir. Flux, bu örnekte kullanılan çalışma zamanında değişken değiştirme özelliğini de destekler.

Aşağıdaki parametreler, bu topolojiler için çalışma zamanında ayarlanır:

* `${kafka.topic}`: Topolojileri okuma/yazma için Kafka konu adı.

* `${kafka.broker.hosts}`: Kafka aracıları konaklar çalıştırın. Aracı bilgisi, KafkaBolt tarafından Kafka'ya yazarken kullanılır.

* `${kafka.zookeeper.hosts}`: Zookeeper Kafka kümesinin üzerinde çalıştığı konakların.

* `${hdfs.url}`: Dosya sistemi HDFSBolt bileşeni için URL. Verileri bir Azure depolama hesabına veya Azure Data Lake Storage yazılacağını gösterir.

* `${hdfs.write.dir}`: Veri yazılan dizin.

Flux topolojileriyle ilgili daha fazla bilgi için bkz. [https://storm.apache.org/releases/1.1.2/flux.html](https://storm.apache.org/releases/1.1.2/flux.html).

### <a name="kafka-writer"></a>Kafka-yazıcı

Kafka-yazıcı topolojisinde, Kafka bolt bileşeni iki dize değerini parametre olarak alır. Bu parametreler bolt'un __anahtar__ ve __ileti__ değerleri olarak Kafka'ya hangi tanımlama grubu alanlarını gönderdiğini gösterir. Anahtar, Kafka'da verileri bölümlemek için kullanılır. İleti, depolanmakta olan verilerdir.

Bu örnekte, `com.microsoft.example.SentenceSpout` bileşeni iki alan (`key` ve `message`) içeren bir tanımlama grubu gösterir. Kafka bolt bu alanları ayıklar ve içlerindeki verileri Kafka'ya gönderir.

Alanların `key` ve `message`adlarını kullanması zorunlu değildir. Bu projede bu adların kullanılmasının nedeni eşlemenin daha kolay anlaşılmasını sağlamaktır.

Aşağıdaki YAML, Kafka-yazıcı bileşeninin tanımıdır:

```yaml
# kafka-writer
---

# topology definition
# name to be used when submitting
name: "kafka-writer"

# Components - constructors, property setters, and builder arguments.
# Currently, components must be declared in the order they are referenced
components:
  # Topic selector for KafkaBolt
  - id: "topicSelector"
    className: "org.apache.storm.kafka.bolt.selector.DefaultTopicSelector"
    constructorArgs:
      - "${kafka.topic}"

  # Mapper for KafkaBolt
  - id: "kafkaMapper"
    className: "org.apache.storm.kafka.bolt.mapper.FieldNameBasedTupleToKafkaMapper"
    constructorArgs:
      - "key"
      - "message"

  # Producer properties for KafkaBolt
  - id: "producerProperties"
    className: "java.util.Properties"
    configMethods:
      - name: "put"
        args:
          - "bootstrap.servers"
          - "${kafka.broker.hosts}"
      - name: "put"
        args:
          - "acks"
          - "1"
      - name: "put"
        args:
          - "key.serializer"
          - "org.apache.kafka.common.serialization.StringSerializer"
      - name: "put"
        args:
          - "value.serializer"
          - "org.apache.kafka.common.serialization.StringSerializer"
 

# Topology configuration
config:
  topology.workers: 2

# Spout definitions
spouts:
  - id: "sentence-spout"
    className: "com.microsoft.example.SentenceSpout"
    parallelism: 8

# Bolt definitions
bolts:
  - id: "kafka-bolt"
    className: "org.apache.storm.kafka.bolt.KafkaBolt"
    parallelism: 8
    configMethods:
    - name: "withProducerProperties"
      args: [ref: "producerProperties"]
    - name: "withTopicSelector"
      args: [ref: "topicSelector"]
    - name: "withTupleToKafkaMapper"
      args: [ref: "kafkaMapper"]

# Stream definitions

streams:
  - name: "spout --> kafka" # Streams data from the sentence spout to the Kafka bolt
    from: "sentence-spout"
    to: "kafka-bolt"
    grouping:
      type: SHUFFLE
```

### <a name="kafka-reader"></a>Kafka-okuyucu

Kafka-okuyucu topolojisinde, spout bileşeni Kafka'dan verileri dize değerleri olarak okur. Ardından veriler günlük bileşeni tarafından Storm günlüğüne ve HDFS bolt bileşeni tarafından Storm kümesi için HDFS uyumlu dosya sistemine yazılır.

```yaml
# kafka-reader
---

# topology definition
# name to be used when submitting
name: "kafka-reader"

# Components - constructors, property setters, and builder arguments.
# Currently, components must be declared in the order they are referenced
components:
  # Convert data from Kafka into string tuples in storm
  - id: "stringScheme"
    className: "org.apache.storm.kafka.StringScheme"
  - id: "stringMultiScheme"
    className: "org.apache.storm.spout.SchemeAsMultiScheme"
    constructorArgs:
      - ref: "stringScheme"

  - id: "zkHosts"
    className: "org.apache.storm.kafka.ZkHosts"
    constructorArgs:
      - "${kafka.zookeeper.hosts}"

  # Spout configuration
  - id: "spoutConfig"
    className: "org.apache.storm.kafka.SpoutConfig"
    constructorArgs:
      # brokerHosts
      - ref: "zkHosts"
      # topic
      - "${kafka.topic}"
      # zkRoot
      - ""
      # id
      - "readerid"
    properties:
      - name: "scheme"
        ref: "stringMultiScheme"

    # How often to sync files to HDFS; every 1000 tuples.
  - id: "syncPolicy"
    className: "org.apache.storm.hdfs.bolt.sync.CountSyncPolicy"
    constructorArgs:
      - 1

  # Rotate files when they hit 5 MB
  - id: "rotationPolicy"
    className: "org.apache.storm.hdfs.bolt.rotation.FileSizeRotationPolicy"
    constructorArgs:
      - 5
      - "KB"

  # File format; read the directory from filters at run time, and use a .txt extension when writing.
  - id: "fileNameFormat"
    className: "org.apache.storm.hdfs.bolt.format.DefaultFileNameFormat"
    configMethods:
      - name: "withPath"
        args: ["${hdfs.write.dir}"]
      - name: "withExtension"
        args: [".txt"]

  # Internal file format; fields delimited by `|`.
  - id: "recordFormat"
    className: "org.apache.storm.hdfs.bolt.format.DelimitedRecordFormat"
    configMethods:
      - name: "withFieldDelimiter"
        args: ["|"]

# Topology configuration
config:
  topology.workers: 2

# Spout definitions
spouts:
  - id: "kafka-spout"
    className: "org.apache.storm.kafka.KafkaSpout"
    constructorArgs:
      - ref: "spoutConfig"
    # Set to the number of partitions for the topic
    parallelism: 8

# Bolt definitions
bolts:
  - id: "logger-bolt"
    className: "com.microsoft.example.LoggerBolt"
    parallelism: 1
  
  - id: "hdfs-bolt"
    className: "org.apache.storm.hdfs.bolt.HdfsBolt"
    configMethods:
      - name: "withConfigKey"
        args: ["hdfs.config"]
      - name: "withFsUrl"
        args: ["${hdfs.url}"]
      - name: "withFileNameFormat"
        args: [ref: "fileNameFormat"]
      - name: "withRecordFormat"
        args: [ref: "recordFormat"]
      - name: "withRotationPolicy"
        args: [ref: "rotationPolicy"]
      - name: "withSyncPolicy"
        args: [ref: "syncPolicy"]
    parallelism: 1

# Stream definitions

streams:
  # Stream data to log
  - name: "kafka --> log" # name isn't used (placeholder for logging, UI, etc.)
    from: "kafka-spout"
    to: "logger-bolt"
    grouping:
      type: SHUFFLE
  
  # stream data to file
  - name: "kafka --> hdfs"
    from: "kafka-spout"
    to: "hdfs-bolt"
    grouping:
      type: SHUFFLE
```

### <a name="property-substitutions"></a>Özellik değişimleri

Proje, topolojilerin kullandığı parametreleri geçirmek için kullanılan `dev.properties` adlı bir dosya içerir. Bu dosya şu özellikleri tanımlar:

| dev.properties dosyası | Açıklama |
| --- | --- |
| `kafka.zookeeper.hosts` | [Apache ZooKeeper](https://zookeeper.apache.org/) konaklar Kafka kümesi için. |
| `kafka.broker.hosts` | Kafka aracısı konakları (çalışan düğümleri). |
| `kafka.topic` | Topolojileri kullanan Kafka konusu. |
| `hdfs.write.dir` | Kafka-okuyucu topolojisinin yazdığı dizin. |
| `hdfs.url` | Storm kümesi tarafından kullanılan dosya sistemi. Azure Depolama hesapları için `wasb:///` değerini kullanın. Azure Data Lake depolama Gen2 için değerini kullanın `abfs:///`. Azure Data Lake depolama Gen1 için değerini kullanın `adl:///`. |

## <a name="create-the-clusters"></a>Kümeleri oluşturma

HDInsight üzerinde Apache Kafka, genel internet üzerinden Kafka aracılarına erişim sağlamaz. Kafka kullanan her özellik aynı Azure sanal ağı içinde olmalıdır. Bu öğreticide hem Kafka hem de Storm kümeleri aynı Azure sanal ağı içinde yer alır. 

Aşağıdaki diyagramda Storm ile Kafka arasındaki iletişimin nasıl aktığı gösterilmektedir:

![Bir Azure sanal ağında Storm ve Kafka kümeleri diyagramı](./media/hdinsight-apache-storm-with-kafka/storm-kafka-vnet.png)

> [!NOTE]  
> Kümeye SSH gibi diğer hizmetlere ve [Apache Ambari](https://ambari.apache.org/) internet üzerinden erişilebilir. HDInsight üzerinde kullanılabilir olan genel bağlantı noktaları hakkında daha fazla bilgi için bkz. [HDInsight Tarafından Kullanılan Bağlantı Noktaları ve URI’ler](hdinsight-hadoop-port-settings-for-services.md).

Bir Azure Sanal Ağı oluşturmak ve sonra bunun içinde Kafka ve Storm kümeleri oluşturmak için aşağıdaki adımları kullanın:

1. Aşağıdaki düğmeyi kullanarak Azure'da oturum açın ve şablonu Azure portalında açın.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fhdinsight-storm-java-kafka%2Fmaster%2Fcreate-kafka-storm-clusters-in-vnet.json" target="_blank"><img src="./media/hdinsight-apache-storm-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
   
    Azure Resource Manager şablonu **https://github.com/Azure-Samples/hdinsight-storm-java-kafka/blob/master/create-kafka-storm-clusters-in-vnet.json** sayfasında bulunur. Aşağıdaki kaynakları oluşturur:
    
    * Azure kaynak grubu
    * Azure Sanal Ağ
    * Azure Storage hesabı
    * HDInsight sürüm 3.6 üzerinde Kafka (üç çalışan düğümü)
    * HDInsight sürüm 3.6 üzerinde Storm (üç çalışan düğümü)

   > [!WARNING]  
   > HDInsight üzerinde Kafka'yı kullanabilmeniz için kümenizin en az üç çalışan düğümü içermesi gerekir. Bu şablon, üç çalışan düğümü içeren bir Kafka kümesi oluşturur.

2. **Özel dağıtım** bölümündeki girdileri doldurmak için aşağıdaki yönergeleri kullanın:

   1. **Özelleştirilmiş şablon** bölümündeki girişleri doldurmak için aşağıdaki bilgileri kullanın:

      | Ayar | Değer |
      | --- | --- |
      | Abonelik | Azure aboneliğiniz |
      | Kaynak grubu | Kaynakları içeren kaynak grubu. |
      | Location | İçinde kaynakların oluşturulduğu Azure bölgesi. |
      | Kafka Kümesi Adı | Kafka kümesinin adı. |
      | Storm Kümesi Adı | Storm kümesinin adı. |
      | Küme Oturum Açma Kullanıcı Adı | Kümeler için yönetici kullanıcı adı. |
      | Küme Oturum Açma Parolası | Kümeler için yönetici kullanıcı parolası. |
      | SSH Kullanıcı Adı | Kümeler için oluşturulacak SSH kullanıcısı. |
      | SSH Parolası | SSH kullanıcısı için parola. |
   
      ![Şablon parametrelerinin resmi](./media/hdinsight-apache-storm-with-kafka/storm-kafka-template.png)

3. **Hüküm ve Koşullar**’ı okuyun ve ardından **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum**’u seçin.

4. Son olarak, **Panoya sabitle**’yi işaretleyin ve **Satın Al**’ı seçin.

> [!NOTE]  
> Kümelerin oluşturulması 20 dakikaya kadar sürebilir.

## <a name="build-the-topology"></a>Topoloji oluşturma

1. Geliştirme ortamında [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka) adresinden projeyi indirin, komut satırı açın ve dizinleri projeyi indirdiğiniz konumla değiştirin.

2. **hdinsight-storm-java-kafka** dizininde, aşağıdaki komutu kullanarak projeyi derleyin ve dağıtım için paket oluşturun:

   ```bash
   mvn clean package
   ```

    Paket işlemi, `target` dizininde `KafkaTopology-1.0-SNAPSHOT.jar` adlı bir dosya oluşturur.

3. Paketi HDInsight kümesindeki Storm'a kopyalamak için aşağıdaki komutları kullanın. `sshuser` değerini kümenin SSH kullanıcı adıyla değiştirin. `stormclustername` değerini __Storm__ kümesinin adıyla değiştirin.

   ```bash
   scp ./target/KafkaTopology-1.0-SNAPSHOT.jar sshuser@stormclustername-ssh.azurehdinsight.net:KafkaTopology-1.0-SNAPSHOT.jar
   ```

    İstendiğinde, kümeleri oluştururken kullandığınız parolayı girin.

## <a name="configure-the-topology"></a>Topolojiyi yapılandırma

1. Aşağıdaki yöntemlerden birini kullanarak HDInsight kümesinde **Kafka** için Kafka aracı konaklarını bulun:

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
    $clusterName = Read-Host -Prompt "Enter the Kafka cluster name"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER" `
        -Credential $creds `
        -UseBasicParsing
    $respObj = ConvertFrom-Json $resp.Content
    $brokerHosts = $respObj.host_components.HostRoles.host_name[0..1]
    ($brokerHosts -join ":9092,") + ":9092"
    ```

    > [!IMPORTANT]  
    > Aşağıdaki Bash örneğinde `$CLUSTERNAME` öğesinin __Kafka__ küme adını içerdiği varsayılır. Ayrıca, [jq](https://stedolan.github.io/jq/) sürüm 1.5 veya üstünün yüklü olduğu da varsayılır. İstendiğinde, küme oturum açma hesabı için parolayı girin.

    ```bash
    curl -su admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER" | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2
    ```

    Döndürülen değer aşağıdaki metne benzer:

        wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092

    > [!IMPORTANT]  
    > Kümeniz için ikiden fazla aracı konağı olabilir, ama istemcilere tüm konakların listesini sağlamanız gerekmez. Bir veya iki tanesi yeterlidir.

2. Aşağıdaki yöntemlerden birini kullanarak HDInsight kümesinde __Kafka__ için Zookeeper konaklarını bulun:

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
    $clusterName = Read-Host -Prompt "Enter the Kafka cluster name"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" `
        -Credential $creds `
        -UseBasicParsing
    $respObj = ConvertFrom-Json $resp.Content
    $zookeeperHosts = $respObj.host_components.HostRoles.host_name[0..1]
    ($zookeeperHosts -join ":2181,") + ":2181"
    ```

    > [!IMPORTANT]  
    > Aşağıdaki Bash örneğinde `$CLUSTERNAME` öğesinin __Kafka__ kümesinin adını içerdiği varsayılır. Ayrıca, [jq](https://stedolan.github.io/jq/)'nun yüklü olduğu da varsayılır. İstendiğinde, küme oturum açma hesabı için parolayı girin.

    ```bash
    curl -su admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2
    ```

    Döndürülen değer aşağıdaki metne benzer:

        zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181

    > [!IMPORTANT]  
    > İkiden fazla Zookeeper düğümü olsa da, istemcilere tüm konakların listesini sağlamanız gerekmez. Bir veya iki tanesi yeterlidir.

    Bu değeri kaydedin çünkü daha sonra kullanılacaktır.

3. Proje kökündeki `dev.properties` dosyasını düzenleyin. Bu dosyadaki ilgili satırlara __Kafka__ kümesi için Aracı ve Zookeeper konaklarının bilgilerini ekleyin. Aşağıdaki örnek, önceki adımlardan alınan örnek değerler kullanılarak yapılandırılır:

        kafka.zookeeper.hosts: zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181
        kafka.broker.hosts: wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092
        kafka.topic: stormtopic

    > [!IMPORTANT]  
    > `hdfs.url` girdisi, Azure Depolama hesabı kullanan bir küme için yapılandırılır. Bu topoloji, Data Lake depolama kullanan bir Storm kümesi ile kullanmak için bu değeri değiştirmek `wasb` için `adl`.

4. `dev.properties` dosyasını kaydedin ve ardından aşağıdaki komutu kullanarak bu dosyayı **Storm** kümesine yükleyin:

     ```bash
    scp dev.properties USERNAME@BASENAME-ssh.azurehdinsight.net:dev.properties
    ```

    **USERNAME** değerini kümenin SSH kullanıcı adıyla değiştirin. **BASENAME** değerini kümeyi oluştururken kullandığınız temel adıyla değiştirin.

## <a name="create-the-kafka-topic"></a>Kafka konusu oluşturma

Kafka, verileri bir _konu_ içinde depolar. Storm topolojilerini başlatmadan önce konuyu oluşturmanız gerekir. Topolojiyi oluşturmak için aşağıdaki adımları kullanın:

1. Aşağıdaki komutu kullanarak SSH üzerinden __Kafka__ kümesine bağlanın. `sshuser` değerini kümeyi oluştururken kullanılan SSH kullanıcı adıyla değiştirin. `kafkaclustername` değerini, Kafka kümenizin adıyla değiştirin:

    ```bash
    ssh sshuser@kafkaclustername-ssh.azurehdinsight.net
    ```

    İstendiğinde, kümeleri oluştururken kullandığınız parolayı girin.
   
    Bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Kafka konusunu oluşturmak için aşağıdaki komutu kullanın. `$KAFKAZKHOSTS` değerini topolojiyi yapılandırırken kullandığınız Zookeeper konak bilgisiyle değiştirin:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic stormtopic --zookeeper $KAFKAZKHOSTS
    ```

    Bu komut Kafka kümesi için Zookeeper'a bağlanır ve `stormtopic` adlı yeni bir konu oluşturur. Bu konu Storm topolojileri tarafından kullanılır.

## <a name="start-the-writer"></a>Yazıcıyı başlatma

1. SSH kullanarak **Storm** kümesine bağlanmak için aşağıdakini kullanın. `sshuser` değerini kümeyi oluştururken kullanılan SSH kullanıcı adıyla değiştirin. `stormclustername` değerini Storm kümesinin adıyla değiştirin:

    ```bash
    ssh sshuser@stormclustername-ssh.azurehdinsight.net
    ```

    İstendiğinde, kümeleri oluştururken kullandığınız parolayı girin.
   
    Bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

2. SSH bağlantısından Storm kümesine, yazıcı topolojisini başlatmak için aşağıdaki komutu kullanın:

    ```bash
    storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writer.yaml --filter dev.properties
    ```

    Bu komutla kullanılan parametreler şunlardır:

    * `org.apache.storm.flux.Flux`: Flux yapılandırın ve bu topoloji çalıştırmak için kullanın.

    * `--remote`: Nimbus topolojiye gönderin. Topoloji, kümedeki çalışan düğümlerine dağıtılır.

    * `-R /writer.yaml`: Kullanım `writer.yaml` topolojisini yapılandırmak için dosya. `-R`, bu kaynağın jar dosyası içinde yer aldığını gösterir. Bu, jar dosyasının kökünde yer aldığından yolu `/writer.yaml` şeklindedir.

    * `--filter`: Girdileri doldurmak `writer.yaml` değerleri kullanarak topoloji `dev.properties` dosya. Örneğin, dosyadaki `kafka.topic` girdisinin değeri topoloji tanımındaki `${kafka.topic}` girdisi yerine kullanılır.

## <a name="start-the-reader"></a>Okuyucuyu başlatma

1. SSH oturumundan Storm kümesine, okuyucu topolojisini başlatmak için aşağıdaki komutu kullanın:

   ```bash
   storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /reader.yaml --filter dev.properties
   ```

2. Bir dakika bekleyin ve ardından okuyucu topolojisi tarafından oluşturulan dosyaları görüntülemek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -ls /stormdata
    ```

    Çıktı aşağıdaki metne benzer:

        Found 173 items
        -rw-r--r--   1 storm supergroup       5137 2018-04-09 19:00 /stormdata/hdfs-bolt-4-0-1523300453088.txt
        -rw-r--r--   1 storm supergroup       5128 2018-04-09 19:00 /stormdata/hdfs-bolt-4-1-1523300453624.txt
        -rw-r--r--   1 storm supergroup       5131 2018-04-09 19:00 /stormdata/hdfs-bolt-4-10-1523300455170.txt
        ...

3. Dosyanın içeriğini görüntülemek için aşağıdaki komutu kullanın. `filename.txt` değerini dosyanın adıyla değiştirin:

    ```bash
    hdfs dfs -cat /stormdata/filename.txt
    ```

    Aşağıdaki metin, örnek bir dosya içeriğidir:

        four score and seven years ago
        snow white and the seven dwarfs
        i am at two with nature
        snow white and the seven dwarfs
        i am at two with nature
        four score and seven years ago
        an apple a day keeps the doctor away

## <a name="stop-the-topologies"></a>Topolojileri durdurma

SSH oturumundan Storm kümesine, Storm topolojilerini durdurmak için aşağıdaki komutları kullanın:

  ```bash
  storm kill kafka-writer
  storm kill kafka-reader
  ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğretici ile oluşturulan kaynakları temizlemek için kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, ilişkili HDInsight kümesini ve kaynak grubuyla ilişkili diğer tüm kaynakları da siler.

Azure portalını kullanarak kaynak grubunu kaldırmak için:

1. Azure portalında sol taraftaki menüyü genişleterek hizmet menüsünü açın ve sonra __Kaynak Grupları__'nı seçerek kaynak gruplarınızın listesini görüntüleyin.
2. Silinecek kaynak grubunu bulun ve sonra listenin sağ tarafındaki __Daha fazla__ düğmesine (...) sağ tıklayın.
3. __Kaynak grubunu sil__'i seçip onaylayın.

> [!WARNING]  
> HDInsight kümesi faturalandırması küme oluşturulduğunda başlar ve küme silindiğinde sona erer. Fatura dakikalara eşit olarak dağıtıldığından, kullanılmayan kümelerinizi mutlaka silmelisiniz.
> 
> HDInsight üzerinde Kafka kümesinin silinmesi Kafka’da depolanmış tüm verileri siler.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici sayesinde nasıl kullanacağınızı öğrendiniz bir [Apache Storm](https://storm.apache.org/) yazma ve okuma için topoloji [Apache Kafka](https://kafka.apache.org/) HDInsight üzerinde. Ayrıca verileri depolamak nasıl öğrendiniz [Apache Hadoop HDFS](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html) HDInsight tarafından kullanılan uyumlu depolama.

HDInsight üzerinde Kafka kullanma hakkında daha fazla bilgi edinmek için [Apache Kafka üretici ve tüketici API'si](kafka/apache-kafka-producer-consumer-api.md) belge.

Linux tabanlı HDInsight'ta topolojileri dağıtma ve izleme hakkında bilgi için bkz. [Linux tabanlı HDInsight'ta Apache Storm topolojilerini dağıtma ve yönetme](storm/apache-storm-deploy-monitor-topology-linux.md)
