---
title: 'Öğretici: Apache Kafka Üretici ve Tüketici API’lerini kullanma - Azure HDInsight | Microsoft Docs'
description: HDInsight’ta Apache Kafka Üretici ve Tüketici API’lerini kullanmayı öğrenin. Bu öğreticide, bu API’leri HDInsight üzerinde Kafka ile kullanmayı öğreneceksiniz.
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
ms.date: 04/16/2018
ms.author: larryfr
ms.openlocfilehash: b602f8bfe316e9c11dbff18273f37c99407c3da6
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-use-the-apache-kafka-producer-and-consumer-apis"></a>Öğretici: Apache Kafka Üretici ve Tüketici API’lerini kullanma

HDInsight’ta Kafka Üretici ve Tüketici API’lerini kullanmayı öğrenin.

Kafka Üretici API’si, uygulamaların Kafka kümesine veri akışları göndermesine olanak tanır. Kafka Tüketici API’si, uygulamaların kümeden veri akışları okumasına olanak tanır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Geliştirme ortamınızı kurma
> * Dağıtım ortamınızı ayarlama
> * Kodu anlama
> * Uygulama derleme ve dağıtma
> * Uygulamayı küme üzerinde çalıştırma

API’ler hakkında daha fazla bilgi için [Üretici API’si](https://kafka.apache.org/documentation/#producerapi) ve [Tüketici API’si](https://kafka.apache.org/documentation/#consumerapi) hakkında Apache belgelerine bakın.

## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma

Aşağıdaki bileşenlerin geliştirme ortamınızda yüklü olması gerekir:

* [Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) veya OpenJDK gibi eşdeğeri.

* [Apache Maven](http://maven.apache.org/)

* Bir SSH istemcisi ve `scp` komutu. Daha fazla bilgi için [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md) belgesine bakın.

* Bir metin düzenleyicisi veya Java IDE.

Dağıtım iş istasyonunuza Java ve JDK yüklerken aşağıdaki ortam değişkenleri ayarlanabilir. Ancak, bunların mevcut olup olmadığını ve sisteminiz için doğru değerleri içerip içermediğini denetlemeniz gerekir.

* `JAVA_HOME` - JDK’nın yüklendiği dizine işaret etmelidir.
* `PATH` - aşağıdaki yolları içermelidir:
  
    * `JAVA_HOME` (veya eşdeğer yol).
    * `JAVA_HOME\bin` (veya eşdeğer yol).
    * Maven'ın yüklendiği dizin.

## <a name="set-up-your-deployment-environment"></a>Dağıtım ortamınızı ayarlama

Bu öğreticide HDInsight 3.6 üzerinde Kafka kullanılması gerekir. HDInsight kümesi üzerinde Kafka oluşturma hakkında bilgi edinmek için [HDInsight üzerinde Kafka kullanmaya başlama](apache-kafka-get-started.md) belgesine bakın.

## <a name="understand-the-code"></a>Kodu anlama

Örnek uygulama, [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) konumunda `Producer-Consumer` alt dizininde bulunur. Uygulama öncelikli olarak dört dosyadan oluşur:

* `pom.xml`: Bu dosya, proje bağımlılıklarını, Java sürümünü ve paketleme yöntemlerini tanımlar.
* `Producer.java`: Bu dosya, üretici API’sini kullanarak Kafka’ya 1 milyon (1.000.000) rastgele cümle gönderir.
* `Consumer.java`: Bu dosya, tüketici API’sini kullanarak Kafka’dan verileri okur ve STDOUT’a yayar.
* `Run.java`: Üretici ve tüketici kodunu çalıştırmak için kullanılan komut satırı arabirimi.

### <a name="pomxml"></a>Pom.xml

`pom.xml` dosyasında aşağıdaki önemli şeyler anlaşılır:

* Bağımlılıklar: Bu proje, `kafka-clients` paketi tarafından sağlanan Kafka üretici ve tüketici API’lerini kullanır. Aşağıdaki XML kodu, bu bağımlılığı tanımlar:

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

    * `maven-compiler-plugin`: Proje tarafından kullanılan Java sürümünü 8 olarak ayarlamak için kullanılır. HDInsight 3.6 tarafından kullanılan Java sürümüdür.
    * `maven-shade-plugin`: Bu uygulamayı ve tüm bağımlılıkları içeren bir uber jar oluşturmak için kullanılır. Ana sınıfı belirtmek zorunda olmadan doğrudan Jar dosyasını çalıştırabilmeniz için uygulamanın giriş noktasını ayarlamak için de kullanılır.

### <a name="producerjava"></a>Producer.java

Üretici, verileri bir Kafka konusunda depolamak için Kafka aracı konakları (çalışan düğümleri) ile iletişim kurar. Aşağıdaki kod parçacığı `Producer.java` dosyasında bulunur:

```java
package com.microsoft.example;

import org.apache.kafka.clients.producer.KafkaProducer;
import org.apache.kafka.clients.producer.ProducerRecord;
import java.util.Properties;
import java.util.Random;
import java.io.IOException;

public class Producer
{
    public static void produce(String brokers) throws IOException
    {

        // Set properties used to configure the producer
        Properties properties = new Properties();
        // Set the brokers (bootstrap servers)
        properties.setProperty("bootstrap.servers", brokers);
        // Set how to serialize key/value pairs
        properties.setProperty("key.serializer","org.apache.kafka.common.serialization.StringSerializer");
        properties.setProperty("value.serializer","org.apache.kafka.common.serialization.StringSerializer");
        KafkaProducer<String, String> producer = new KafkaProducer<>(properties);

        // So we can generate random sentences
        Random random = new Random();
        String[] sentences = new String[] {
                "the cow jumped over the moon",
                "an apple a day keeps the doctor away",
                "four score and seven years ago",
                "snow white and the seven dwarfs",
                "i am at two with nature"
        };

        String progressAnimation = "|/-\\";
        // Produce a bunch of records
        for(int i = 0; i < 1000000; i++) {
            // Pick a sentence at random
            String sentence = sentences[random.nextInt(sentences.length)];
            // Send the sentence to the test topic
            producer.send(new ProducerRecord<String, String>("test", sentence));
            String progressBar = "\r" + progressAnimation.charAt(i % progressAnimation.length()) + " " + i;
            System.out.write(progressBar.getBytes());
        }
    }
}
```

Bu kod, Kafka aracı konaklarına (çalışan düğümleri) bağlanır ve sonra Üretici API’sini kullanarak Kafka’ya 1.000.000 cümle gönderir.

### <a name="consumerjava"></a>Consumer.java

Tüketici, Kafka aracı konakları (çalışan düğümleri) ile iletişim kurar ve bir döngüdeki kayıtları okur. Aşağıdaki kod parçacığı `Consumer.java` dosyasında bulunur:

```java
package com.microsoft.example;

import org.apache.kafka.clients.consumer.KafkaConsumer;
import org.apache.kafka.clients.consumer.ConsumerRecords;
import org.apache.kafka.clients.consumer.ConsumerRecord;
import java.util.Properties;
import java.util.Arrays;

public class Consumer {
    public static void consume(String brokers, String groupId) {
        // Create a consumer
        KafkaConsumer<String, String> consumer;
        // Configure the consumer
        Properties properties = new Properties();
        // Point it to the brokers
        properties.setProperty("bootstrap.servers", brokers);
        // Set the consumer group (all consumers must belong to a group).
        properties.setProperty("group.id", groupId);
        // Set how to serialize key/value pairs
        properties.setProperty("key.deserializer","org.apache.kafka.common.serialization.StringDeserializer");
        properties.setProperty("value.deserializer","org.apache.kafka.common.serialization.StringDeserializer");
        // When a group is first created, it has no offset stored to start reading from. This tells it to start
        // with the earliest record in the stream.
        properties.setProperty("auto.offset.reset","earliest");
        consumer = new KafkaConsumer<>(properties);

        // Subscribe to the 'test' topic
        consumer.subscribe(Arrays.asList("test"));

        // Loop until ctrl + c
        int count = 0;
        while(true) {
            // Poll for records
            ConsumerRecords<String, String> records = consumer.poll(200);
            // Did we get any?
            if (records.count() == 0) {
                // timeout/nothing to read
            } else {
                // Yes, loop over records
                for(ConsumerRecord<String, String> record: records) {
                    // Display record and count
                    count += 1;
                    System.out.println( count + ": " + record.value());
                }
            }
        }
    }
}
```

Bu kodda tüketici, konu başlangıcından okumak üzere yapılandırılmıştır (`auto.offset.reset` değeri `earliest` olarak ayarlanır.)

### <a name="runjava"></a>Run.java

`Run.java` dosyası, üretici veya tüketici kodunu çalıştıran bir komut satırı arabirimi sağlar. Kafka aracı konağı bilgilerini bir parametre olarak sağlamanız gerekir. İsteğe bağlı olarak, tüketici işlemi tarafından kullanılan bir grup kimliği değeri ekleyebilirsiniz. Aynı grup kimliğini kullanarak birden fazla tüketici örneği oluşturursanız, bu örnekler konudan okuma yük dengelemesi yapar.

```java
package com.microsoft.example;

import java.io.IOException;
import java.util.UUID;

// Handle starting producer or consumer
public class Run {
    public static void main(String[] args) throws IOException {
        if(args.length < 2) {
            usage();
        }

        // Get the brokers
        String brokers = args[1];
        switch(args[0].toLowerCase()) {
            case "producer":
                Producer.produce(brokers);
                break;
            case "consumer":
                // Either a groupId was passed in, or we need a random one
                String groupId;
                if(args.length == 3) {
                    groupId = args[2];
                } else {
                    groupId = UUID.randomUUID().toString();
                }
                Consumer.consume(brokers, groupId);
                break;
            default:
                usage();
        }
        System.exit(0);
    }
    // Display usage
    public static void usage() {
        System.out.println("Usage:");
        System.out.println("kafka-example.jar <producer|consumer> brokerhosts [groupid]");
        System.exit(1);
    }
}
```

## <a name="build-and-deploy-the-example"></a>Örnek derleme ve dağıtma

1. [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) adresinden örnekleri indirin.

2. Dizinleri `Producer-Consumer` dizininin konumuna geçirin ve aşağıdaki komutu kullanın:

    ```
    mvn clean package
    ```

    Bu komut, `kafka-producer-consumer-1.0-SNAPSHOT.jar` adlı dosyayı içeren `target` adlı bir dizin oluşturur.

3. `kafka-producer-consumer-1.0-SNAPSHOT.jar` dosyasını HDInsight kümenize kopyalamak için aşağıdaki komutları kullanın:
   
    ```bash
    scp ./target/kafka-producer-consumer-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-producer-consumer.jar
    ```
   
    **SSHUSER** değerini kümenizin SSH kullanıcısı ile, **CLUSTERNAME** değerini kümenizin adıyla değiştirin. İstendiğinde, SSH kullanıcısının parolasını girin.

## <a id="run"></a> Örneği çalıştırma

1. Kümeye SSH bağlantısı açmak için aşağıdaki komutu kullanın:

    ```bash
    ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    **SSHUSER** değerini kümenizin SSH kullanıcısı ile, **CLUSTERNAME** değerini kümenizin adıyla değiştirin. İstendiğinde, SSH kullanıcı hesabının parolasını girin. HDInsight ile `scp` kullanma hakkında daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Bu örnek tarafından kullanılan Kafka konularını oluşturmak için aşağıdaki adımları kullanın:

    1. Küme adını bir değişkene kaydedip JSON ayrıştırma yardımcı programını (`jq`) yüklemek için aşağıdaki komutları kullanın. İstendiğinde, Kafka kümesi adını girin:
    
        ```bash
        sudo apt -y install jq
        read -p 'Enter your Kafka cluster name:' CLUSTERNAME
        ```
    
    2. Kafka aracısı ana bilgisayarlarını ve Zookeeper ana bilgisayarlarını almak için aşağıdaki komutları kullanın. İstendiğinde, küme oturum açma (yönetici) hesabı için parolayı girin.
    
        ```bash
        export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`; \
        export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`; \
        ```

    3. `test` konusunu oluşturmak için aşağıdaki komutu kullanın:

        ```bash
        /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
        ```

3. Üreticiyi çalıştırmak ve konuya veri yazmak için aşağıdaki komutu kullanın:

    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS
    ```

4. Üretici tamamlandıktan sonra, konu başlığından okumak için aşağıdaki komutu kullanın:
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS
    ```
   
    Okunan kayıtlar, kayıt sayısıyla birlikte gösterilir.

5. Tüketiciden çıkış yapmak için __Ctrl + C__ tuşlarını kullanın.

### <a name="multiple-consumers"></a>Birden çok tüketici

Kafka tüketicileri kayıtları okurken bir tüketici grubu kullanır. Birden çok tüketiciyle aynı grubun kullanılması, konu başlığından yük dengeli okuma yapılmasına neden olur. Gruptaki her bir tüketici, kayıtların bir kısmını alır.

Tüketici uygulaması, grup kimliği olarak kullanılan bir parametre kabul eder Örneğin, aşağıdaki komut bir `mygroup` grup kimliği kullanarak tüketiciyi başlatır:
   
```bash
java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS mygroup
```

Bu işlemi uygulamada görmek için aşağıdaki komutu kullanın:

```bash
tmux new-session 'java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS mygroup' \; split-w
indow -h 'java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS mygroup' \; attach
```

Bu komut, terminali iki sütuna bölmek için `tmux` kullanır. Her sütunda aynı grup kimliği değerine sahip bir tüketici başlatılır. Tüketici okumayı tamamladıktan sonra her birinin yalnızca kayıtların bir bölümünü okuduğuna dikkat edin. `tmux` komutundan çıkmak için __Ctrl + C __ tuş birleşimini iki kez kullanın.

Aynı gruptaki istemcilerin tüketimi, konu başlığının bölümleri aracılığıyla işlenir. Daha önce oluşturulan `test` konu başlığında sekiz bölüm vardır. Sekiz tüketici başlatırsanız, her tüketici konunun tek bir bölümünden kayıtları okur.

> [!IMPORTANT]
> Bir tüketici grubunda bölümden daha fazla tüketici örneği olamaz. Bu örnekte, konu başlığındaki bölüm sayısı sekiz olduğu için bir tüketici grubu en fazla bu sayıda tüketici içerebilir. Ya da her biri en fazla sekiz tüketici içeren birden fazla tüketici grubunuz olabilir.

Kafka’ya depolanan kayıtlar bir bölümde alındıkları sırayla depolanır. *Bir bölüm* içindeki kayıtlar için sıralı teslim sağlamak üzere, tüketici örneklerinin bölüm sayısıyla eşleştiği bir tüketici grubu oluşturun. *Konu başlığı içindeki* kayıtların sıralı teslim edilmesini sağlayabilmek için, yalnızca bir tüketici örneği içeren bir tüketici grubu oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, HDInsight üzerinde Kafka ile Kafka Üretici ve Tüketici API’sini nasıl kullanacağınızı öğrendiniz. Kafka ile çalışma hakkında daha fazla bilgi için aşağıdakileri kullanın:

* [Kafka günlüklerini çözümleme](apache-kafka-log-analytics-operations-management.md)
* [Kafka kümeleri arasında verileri çoğaltma](apache-kafka-mirroring.md)
* [HDInsight ile Kafka Akışı API’si](apache-kafka-streams-api.md)