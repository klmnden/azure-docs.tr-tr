---
title: 'Öğretici: Apache Kafka Akışları API’sini kullanma - Azure HDInsight | Microsoft Docs'
description: HDInsight’ta Apache Kafka Akışları API’sini kullanmayı öğrenin. Bu API, Kafka’daki konular arasında akış işleme gerçekleştirmenize olanak sağlar.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: tutorial
ms.date: 04/17/2018
ms.author: larryfr
ms.openlocfilehash: 8aff28079a0aaa7c02d8a187cb379ecdbedcd854
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-apache-kafka-streams-api"></a>Öğretici: Apache Kafka akışları API’si

Kafka Akışları API’sini kullanan bir uygulama oluşturmayı ve bunu HDInsight üzerinde Kafka ile çalıştırmayı öğrenin. 

Bu öğreticide kullanılan uygulama, akışa alma sözcük sayısıdır. Bir Kafka konusundan metin verilerini okur, tek tek sözcükleri ayıklar ve sonra sözcüğü ve sayıyı başka bir Kafka konusunda depolar.

> [!NOTE]
> Kafka akışı işleme genellikle Apache Spark veya Storm kullanılarak gerçekleştirilir. Kafka 0.10.0 sürümünde (HDInsight 3.5 ve 3.6), Kafka Akışları API’si sunulmuştur. Bu API, girdi ve çıktı konuları arasında veri akışlarını dönüştürmenize olanak sağlar. Bazı durumlarda bu, bir Spark veya Storm akış çözümü oluşturulmasına alternatif olabilir. 
>
> Kafka Akışları hakkında daha fazla bilgi için, Apache.org adresindeki [Akışların Tanıtımı](https://kafka.apache.org/10/documentation/streams/) belgelerine bakın.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Geliştirme ortamınızı kurma
> * Kodu anlama
> * Uygulama derleme ve dağıtma
> * Kafka konuları yapılandırma
> * Kodu çalıştırma

## <a name="prerequisites"></a>Ön koşullar

* HDInsight 3.6 kümesi üzerinde bir Kafka. HDInsight kümesi üzerinde Kafka oluşturma hakkında bilgi edinmek için [HDInsight üzerinde Kafka kullanmaya başlama](apache-kafka-get-started.md) belgesine bakın.

* [Kafka Tüketici ve Üretici API’si](apache-kafka-producer-consumer-api.md) belgesindeki adımları tamamlayın. Bu belgede yer alan adımlarda, bu öğreticide oluşturulan örnek uygulama ve konular kullanılmaktadır.

## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma

Aşağıdaki bileşenlerin geliştirme ortamınızda yüklü olması gerekir:

* [Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) veya OpenJDK gibi eşdeğeri.

* [Apache Maven](http://maven.apache.org/)

* Bir SSH istemcisi ve `scp` komutu. Daha fazla bilgi için [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md) belgesine bakın.

## <a name="understand-the-code"></a>Kodu anlama

Örnek uygulama, [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) konumunda `Streaming` alt dizininde bulunur. Uygulama iki dosyadan oluşur:

* `pom.xml`: Bu dosya, proje bağımlılıklarını, Java sürümünü ve paketleme yöntemlerini tanımlar.
* `Stream.java`: Bu dosya, akış mantığını uygular.

### <a name="pomxml"></a>Pom.xml

`pom.xml` dosyasında aşağıdaki önemli şeyler anlaşılır:

* Bağımlılıklar: Bu proje, `kafka-clients` paketi tarafından sağlanan Kafka Akışları API’sini kullanır. Aşağıdaki XML kodu, bu bağımlılığı tanımlar:

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

* Eklentiler: Maven eklentileri çeşitli özellikler sağlar. Bu projede aşağıdaki eklentiler kullanılır:

    * `maven-compiler-plugin`: Proje tarafından kullanılan Java sürümünü 8 olarak ayarlamak için kullanılır. HDInsight 3.6 için Java 8 gerekir.
    * `maven-shade-plugin`: Bu uygulamayı ve tüm bağımlılıkları içeren bir uber jar oluşturmak için kullanılır. Ana sınıfı belirtmek zorunda olmadan doğrudan Jar dosyasını çalıştırabilmeniz için uygulamanın giriş noktasını ayarlamak için de kullanılır.

### <a name="streamjava"></a>Stream.java

`Stream.java` dosyası, sözcük sayısı uygulamasını uygulamak için Akışlar API’sini kullanır. `test` adlı bir Kafka konusundan verileri okur ve `wordcounts` adlı bir konuya sözcük sayılarını yazar.

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

1. [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) adresinden örnekleri indirin.

2. Dizinleri `Streaming` dizinine değiştirin ve sonra aşağıdaki komutu kullanarak bir jar paketi oluşturun:

    ```bash
    mvn clean package
    ```

    Bu komut, `target/kafka-streaming-1.0-SNAPSHOT.jar` konumunda paketi oluşturur.

3. `kafka-streaming-1.0-SNAPSHOT.jar` dosyasını HDInsight kümenize kopyalamak için aşağıdaki komutu kullanın:
   
    ```bash
    scp ./target/kafka-streaming-1.0-SNAPSHOT.jar sshuser@clustername-ssh.azurehdinsight.net:kafka-streaming.jar
    ```
   
    `sshuser` değerini, kümenizin SSH kullanıcısı ile, `clustername` değerini kümenizin adıyla değiştirin. İstendiğinde, SSH kullanıcı hesabının parolasını girin. HDInsight ile `scp` kullanma hakkında daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="create-kafka-topics"></a>Kafka konuları oluşturma

1. Kümeye SSH bağlantısı açmak için aşağıdaki komutu kullanın:

    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    `sshuser` değerini, kümenizin SSH kullanıcısı ile, `clustername` değerini kümenizin adıyla değiştirin. İstendiğinde, SSH kullanıcı hesabının parolasını girin. HDInsight ile `scp` kullanma hakkında daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Küme adını bir değişkene kaydedip JSON ayrıştırma yardımcı programını (`jq`) yüklemek için aşağıdaki komutları kullanın. İstendiğinde, Kafka kümesi adını girin:

    ```bash
    sudo apt -y install jq
    read -p 'Enter your Kafka cluster name:' CLUSTERNAME
    ```

3. Kafka aracısı ana bilgisayarlarını ve Zookeeper ana bilgisayarlarını almak için aşağıdaki komutları kullanın. İstendiğinde, küme oturum açma (yönetici) hesabı için parolayı girin. İki defa parolanız istenir.

    ```bash
    export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`; \
    export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`; \
    ```

4. Akış işlemi tarafından kullanılan konular oluşturmak için aşağıdaki komutları kullanın:

    > [!NOTE]
    > `test` konusunun zaten mevcut olduğuna dair bir hata alabilirsiniz. Üretici ve Tüketici API’si öğreticisinde oluşturulmuş olabileceğinden bu sorun değildir.

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcounts --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic RekeyedIntermediateTopic --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcount-example-Counts-changelog --zookeeper $KAFKAZKHOSTS
    ```

    Konular aşağıdaki amaçlar için kullanılır:

    * `test`: Bu konu, kayıtların alındığı konudur. Akış uygulaması buradan okur.
    * `wordcounts`: Bu konu, akış uygulamasının çıktısını depoladığı konudur.
    * `RekeyedIntermediateTopic`: Bu konu, `countByKey` işleci tarafından sayı güncelleştirilirken verileri yeniden bölümlemek için kullanılır.
    * `wordcount-example-Counts-changelog`: Bu konu, `countByKey` işlemi tarafından kullanılan durum deposudur

    > [!IMPORTANT]
    > HDInsight üzerinde Kafka otomatik olarak konular oluşturmak için de yapılandırılabilir. Daha fazla bilgi için [Otomatik konu oluşturmayı yapılandırma](apache-kafka-auto-create-topics.md) belgesine bakın.

## <a name="run-the-code"></a>Kodu çalıştırma

1. Arka plan işlemi olarak akış uygulamasını başlatmak için aşağıdaki komutu kullanın:

    ```bash
    java -jar kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS &
    ```

    > [!NOTE]
    > log4j ile ilgili bir uyarı alabilirsiniz. Bunu yoksayabilirsiniz.

2. `test` konusuna kayıtlar göndermek için aşağıdaki komutu kullanarak üretici uygulamasını başlatın:

    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS
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
    > `--from-beggining` parametresi, tüketiciyi konuda depolanan kayıtların başında başlayacak şekilde yapılandırır. Her sözcükle karşılaşıldığında sayı artar, bu nedenle konu, artan sayıyla birlikte her sözcük için birden fazla giriş içerir.

7. Üreticiden çıkış yapmak için __Ctrl + C__ tuşlarını kullanın. Uygulamadan ve tüketiciden çıkış yapmak için __Ctrl + C__ tuşlarını kullanmaya devam edin.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, HDInsight üzerinde Kafka ile Kafka Akışları API’sini nasıl kullanacağınızı öğrendiniz. Kafka ile çalışma hakkında daha fazla bilgi için aşağıdakileri kullanın:

* [Kafka günlüklerini çözümleme](apache-kafka-log-analytics-operations-management.md)
* [Kafka kümeleri arasında verileri çoğaltma](apache-kafka-mirroring.md)
