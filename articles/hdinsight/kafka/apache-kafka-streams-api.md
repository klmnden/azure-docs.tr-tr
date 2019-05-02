---
title: "Öğretici: Apache Kafka akışlar API'si - Azure HDInsight kullanma "
description: HDInsight’ta Apache Kafka Akışları API’sini kullanmayı öğrenin. Bu API, Kafka’daki konular arasında akış işleme gerçekleştirmenize olanak sağlar.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 04/02/2019
ms.openlocfilehash: 9425af0f39d14287b49fe06a81172281feb24e83
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64715961"
---
# <a name="tutorial-apache-kafka-streams-api"></a>Öğretici: Apache Kafka akışlar API'si

Apache Kafka akışlar API'si kullanan bir uygulama oluşturun ve HDInsight üzerinde Kafka ile çalıştırma hakkında bilgi edinin. 

Bu öğreticide kullanılan uygulama, akışa alma sözcük sayısıdır. Bir Kafka konusundan metin verilerini okur, tek tek sözcükleri ayıklar ve sonra sözcüğü ve sayıyı başka bir Kafka konusunda depolar.

> [!NOTE]  
> Kafka akış işleme yapılan genellikle Apache Spark veya Apache Storm kullanma. Kafka (HDInsight 3.5 ve 3.6), sürüm 1.1.0 Kafka akış API'sini kullanıma sunmuştur. Bu API, girdi ve çıktı konuları arasında veri akışlarını dönüştürmenize olanak sağlar. Bazı durumlarda bu, bir Spark veya Storm akış çözümü oluşturulmasına alternatif olabilir. 
>
> Kafka Akışları hakkında daha fazla bilgi için, Apache.org adresindeki [Akışların Tanıtımı](https://kafka.apache.org/10/documentation/streams/) belgelerine bakın.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Kodu anlama
> * Uygulama derleme ve dağıtma
> * Kafka konuları yapılandırma
> * Kodu çalıştırma

## <a name="prerequisites"></a>Önkoşullar

* HDInsight 3.6 kümesi üzerinde bir Kafka. HDInsight kümesinde Kafka oluşturmak nasıl öğrenmek için bkz. [HDInsight üzerinde Apache Kafka kullanmaya başlama](apache-kafka-get-started.md) belge.

* Bölümündeki adımları tamamlamanız [Apache Kafka tüketicisi ve Producer API](apache-kafka-producer-consumer-api.md) belge. Bu belgede yer alan adımlarda, bu öğreticide oluşturulan örnek uygulama ve konular kullanılmaktadır.

* [Java Developer Kit (JDK) 8 sürümünü](https://aka.ms/azure-jdks) veya OpenJDK gibi eşdeğeri.

* [Apache Maven](https://maven.apache.org/download.cgi) düzgün [yüklü](https://maven.apache.org/install.html) Apache göre.  Maven derleme sistemi Java projeleri için proje olur.

* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="understand-the-code"></a>Kodu anlama

Örnek uygulama, [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) konumunda `Streaming` alt dizininde bulunur. Uygulama iki dosyadan oluşur:

* `pom.xml`: Bu dosya, proje bağımlılıkları, Java sürümü ve paketleme yöntemleri tanımlar.
* `Stream.java`: Bu dosya, akış mantığını uygular.

### <a name="pomxml"></a>Pom.xml

`pom.xml` dosyasında aşağıdaki önemli şeyler anlaşılır:

* Bağımlılıklar: Bu proje tarafından sağlanan Kafka akışlar API'si kullanır `kafka-clients` paket. Aşağıdaki XML kodu, bu bağımlılığı tanımlar:

    ```xml
    <!-- Kafka client for producer/consumer operations -->
    <dependency>
      <groupId>org.apache.kafka</groupId>
      <artifactId>kafka-clients</artifactId>
      <version>${kafka.version}</version>
    </dependency>
    ```

    > [!NOTE]  
    > `${kafka.version}` girişi, `pom.xml` dosyasının `<properties>..</properties>` bölümünde bildirilir ve HDInsight kümesinin Kafka sürümüne yapılandırılır.

* Eklentiler: Maven eklentileri çeşitli özellikler sunar. Bu projede aşağıdaki eklentiler kullanılır:

    * `maven-compiler-plugin`: 8 proje tarafından kullanılan Java sürümünü ayarlamak için kullanılır. HDInsight 3.6 için Java 8 gerekir.
    * `maven-shade-plugin`: Herhangi bir bağımlılığın yanı sıra bu uygulamayı içeren bir uber jar oluşturmak için kullanılır. Ana sınıfı belirtmek zorunda olmadan doğrudan Jar dosyasını çalıştırabilmeniz için uygulamanın giriş noktasını ayarlamak için de kullanılır.

### <a name="streamjava"></a>Stream.java

[Stream.java](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/blob/master/Streaming/src/main/java/com/microsoft/example/Stream.java) dosyası, sözcük sayısı uygulamasını çalıştırmak için Akışlar API’sini kullanır. `test` adlı bir Kafka konusundan verileri okur ve `wordcounts` adlı bir konuya sözcük sayılarını yazar.

Aşağıdaki kod, sözcük sayısı uygulamasını tanımlar:

```java
package com.microsoft.example;

import org.apache.kafka.common.serialization.Serde;
import org.apache.kafka.common.serialization.Serdes;
import org.apache.kafka.streams.KafkaStreams;
import org.apache.kafka.streams.KeyValue;
import org.apache.kafka.streams.StreamsConfig;
import org.apache.kafka.streams.kstream.KStream;
import org.apache.kafka.streams.kstream.KStreamBuilder;

import java.util.Arrays;
import java.util.Properties;

public class Stream
{
    public static void main( String[] args ) {
        Properties streamsConfig = new Properties();
        // The name must be unique on the Kafka cluster
        streamsConfig.put(StreamsConfig.APPLICATION_ID_CONFIG, "wordcount-example");
        // Brokers
        streamsConfig.put(StreamsConfig.BOOTSTRAP_SERVERS_CONFIG, args[0]);
        // SerDes for key and values
        streamsConfig.put(StreamsConfig.KEY_SERDE_CLASS_CONFIG, Serdes.String().getClass().getName());
        streamsConfig.put(StreamsConfig.VALUE_SERDE_CLASS_CONFIG, Serdes.String().getClass().getName());

        // Serdes for the word and count
        Serde<String> stringSerde = Serdes.String();
        Serde<Long> longSerde = Serdes.Long();

        KStreamBuilder builder = new KStreamBuilder();
        KStream<String, String> sentences = builder.stream(stringSerde, stringSerde, "test");
        KStream<String, Long> wordCounts = sentences
                .flatMapValues(value -> Arrays.asList(value.toLowerCase().split("\\W+")))
                .map((key, word) -> new KeyValue<>(word, word))
                .countByKey("Counts")
                .toStream();
        wordCounts.to(stringSerde, longSerde, "wordcounts");

        KafkaStreams streams = new KafkaStreams(builder, streamsConfig);
        streams.start();

        Runtime.getRuntime().addShutdownHook(new Thread(streams::close));
    }
}
```

## <a name="build-and-deploy-the-example"></a>Örnek derleme ve dağıtma

Projeyi derlemek ve HDInsight kümesi üzerinde Kafka’nıza dağıtmak için aşağıdaki adımları kullanın:

1. Geçerli dizin konumuna ayarlayın `hdinsight-kafka-java-get-started-master\Streaming` dizin geçirin ve ardından aşağıdaki komutu kullanarak bir jar paketi oluşturun:

    ```cmd
    mvn clean package
    ```

    Bu komut, `target/kafka-streaming-1.0-SNAPSHOT.jar` konumunda paketi oluşturur.

2. `sshuser` değerini, kümenizin SSH kullanıcısı ile, `clustername` değerini kümenizin adıyla değiştirin. Kopyalamak için aşağıdaki komutu kullanın `kafka-streaming-1.0-SNAPSHOT.jar` HDInsight kümenize dosya. İstendiğinde, SSH kullanıcı hesabının parolasını girin.

    ```cmd
    scp ./target/kafka-streaming-1.0-SNAPSHOT.jar sshuser@clustername-ssh.azurehdinsight.net:kafka-streaming.jar
    ```

## <a name="create-apache-kafka-topics"></a>Apache Kafka konularını oluşturma

1. `sshuser` değerini, kümenizin SSH kullanıcısı ile, `CLUSTERNAME` değerini kümenizin adıyla değiştirin. Aşağıdaki komutu girerek küme için bir SSH bağlantısı açın. İstendiğinde, SSH kullanıcı hesabının parolasını girin.

    ```bash
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. Yükleme [jq](https://stedolan.github.io/jq/), bir komut satırı JSON işlemcisine giden. Açık SSH bağlantısından yüklemek için komutu girin `jq`:

    ```bash
    sudo apt -y install jq
    ```

3. Ortam değişkenlerini ayarlamak. Değiştirin `PASSWORD` ve `CLUSTERNAME` ile küme oturum açma parolasını ve küme adı sırasıyla, sonra komutu girin:

    ```bash
    export password='PASSWORD'
    export clusterNameA='CLUSTERNAME'
    ```

4. Büyük küçük harfleri doğru extract küme adı. Küme adı gerçek büyük küçük harfleri, küme nasıl oluşturulduğuna bağlı olarak, beklenenden farklı olabilir. Bu komut gerçek büyük/küçük harf almak, bir değişkende depolayın ve sonra görünen doğru cased adı ve daha önce belirtilen adı. Aşağıdaki komutu girin:

    ```bash
    export clusterName=$(curl -u admin:$password -sS -G "https://$clusterNameA.azurehdinsight.net/api/v1/clusters" \
  	| jq -r '.items[].Clusters.cluster_name')
    echo $clusterName, $clusterNameA
    ```

5. Kafka aracısı ve Apache Zookeeper konakları almak için aşağıdaki komutları kullanın. İstendiğinde, küme oturum açma (yönetici) hesabı için parolayı girin. İki defa parolanız istenir.

    ```bash
    export KAFKAZKHOSTS=`curl -sS -u admin:$password -G \
    https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER \
  	| jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`;
    export KAFKABROKERS=`curl -sS -u admin:$password -G \
    https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER \
  	| jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`;
    ```

6. Akış işlemi tarafından kullanılan konular oluşturmak için aşağıdaki komutları kullanın:

    > [!NOTE]  
    > `test` konusunun zaten mevcut olduğuna dair bir hata alabilirsiniz. Üretici ve Tüketici API’si öğreticisinde oluşturulmuş olabileceğinden bu sorun değildir.

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcounts --zookeeper $KAFKAZKHOSTS
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic RekeyedIntermediateTopic --zookeeper $KAFKAZKHOSTS
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcount-example-Counts-changelog --zookeeper $KAFKAZKHOSTS
    ```

    Konular aşağıdaki amaçlar için kullanılır:

   * `test`: Bu konu, burada kayıtları alınan yöneliktir. Akış uygulaması buradan okur.
   * `wordcounts`: Bu konu, akış uygulaması çıktısını depoladığı yöneliktir.
   * `RekeyedIntermediateTopic`: Bu konu sayısı tarafından güncelleştirildiğinden verileri bölümlemek için kullanılan `countByKey` işleci.
   * `wordcount-example-Counts-changelog`: Bu konu tarafından kullanılan bir durumu deposudur `countByKey` işlemi

     > [!IMPORTANT]  
     > HDInsight üzerinde Kafka otomatik olarak konular oluşturmak için de yapılandırılabilir. Daha fazla bilgi için [Otomatik konu oluşturmayı yapılandırma](apache-kafka-auto-create-topics.md) belgesine bakın.

## <a name="run-the-code"></a>Kodu çalıştırma

1. Arka plan işlemi olarak akış uygulamasını başlatmak için aşağıdaki komutu kullanın:

    ```bash
    java -jar kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS &
    ```

    > [!NOTE]  
    > Apache log4j hakkında uyarı alabilirsiniz. Bunu yoksayabilirsiniz.

2. `test` konusuna kayıtlar göndermek için aşağıdaki komutu kullanarak üretici uygulamasını başlatın:

    ```bash
    java -jar kafka-producer-consumer.jar producer test $KAFKABROKERS
    ```

3. Üretici tamamlandıktan sonra `wordcounts` konusunda depolanan bilgileri görüntülemek için aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic wordcounts --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer --from-beginning
    ```

    > [!NOTE]  
    > `--property` parametreleri, konsol tüketicisine sayı (değer) ile birlikte anahtarı (sözcük) yazdırmasını bildirir. Bu parametre, Kafka’dan bu değerler okunurken kullanılacak seri kaldırıcıyı da yapılandırır.

    Çıktı aşağıdaki metne benzer:
   
        dwarfs  13635
        ago     13664
        snow    13636
        dwarfs  13636
        ago     13665
        a       13803
        ago     13666
        a       13804
        ago     13667
        ago     13668
        jumped  13640
        jumped  13641
   
    > [!NOTE]  
    > `--from-beginning` parametresi, tüketiciyi konuda depolanan kayıtların başında başlayacak şekilde yapılandırır. Her sözcükle karşılaşıldığında sayı artar, bu nedenle konu, artan sayıyla birlikte her sözcük için birden fazla giriş içerir.

4. Üreticiden çıkış yapmak için __Ctrl + C__ tuşlarını kullanın. Uygulamadan ve tüketiciden çıkış yapmak için __Ctrl + C__ tuşlarını kullanmaya devam edin.

5. Akış işlemi tarafından kullanılan konuları silmek için aşağıdaki komutları kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --delete --topic test --zookeeper $KAFKAZKHOSTS
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --delete --topic wordcounts --zookeeper $KAFKAZKHOSTS
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --delete --topic RekeyedIntermediateTopic --zookeeper $KAFKAZKHOSTS
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --delete --topic wordcount-example-Counts-changelog --zookeeper $KAFKAZKHOSTS
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, HDInsight üzerinde Kafka ile Apache Kafka akışlar API'si kullanmayı öğrendiniz. Kafka ile çalışma hakkında daha fazla bilgi için aşağıdakileri kullanın:

* [Apache Kafka günlüklerini çözümleme](apache-kafka-log-analytics-operations-management.md)
* [Apache Kafka kümeleri arasında verileri çoğaltma](apache-kafka-mirroring.md)
