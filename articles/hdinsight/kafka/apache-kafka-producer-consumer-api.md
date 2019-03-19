---
title: "Öğretici: Apache Kafka üretici ve tüketici API'lerini - Azure HDInsight'ı kullanın "
description: HDInsight’ta Apache Kafka Üretici ve Tüketici API’lerini kullanmayı öğrenin. Bu öğreticide, bu API’leri HDInsight üzerinde Kafka ile kullanmayı öğreneceksiniz.
services: hdinsight
author: dhgoelmsft
ms.author: dhgoel
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 11/06/2018
ms.openlocfilehash: 556fc1f04cc6a1d1b594bdd3787ed43d30f607c6
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58091180"
---
# <a name="tutorial-use-the-apache-kafka-producer-and-consumer-apis"></a>Öğretici: Apache Kafka üretici ve tüketici API'lerini kullanma

HDInsight’ta Apache Kafka Üretici ve Tüketici API’lerini kullanmayı öğrenin.

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

* [Java JDK 8](https://aka.ms/azure-jdks) veya OpenJDK gibi eşdeğeri.

* [Apache Maven](https://maven.apache.org/)

* Bir SSH istemcisi ve `scp` komutu. Daha fazla bilgi için [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md) belgesine bakın.

* Bir metin düzenleyicisi veya Java IDE.

Dağıtım iş istasyonunuza Java ve JDK yüklerken aşağıdaki ortam değişkenleri ayarlanabilir. Ancak, bunların mevcut olup olmadığını ve sisteminiz için doğru değerleri içerip içermediğini denetlemeniz gerekir.

* `JAVA_HOME` - JDK’nın yüklendiği dizine işaret etmelidir.
* `PATH` - aşağıdaki yolları içermelidir:

    * `JAVA_HOME` (veya eşdeğer yol).
    * `JAVA_HOME\bin` (veya eşdeğer yol).
    * Maven'ın yüklendiği dizin.

## <a name="set-up-your-deployment-environment"></a>Dağıtım ortamınızı ayarlama

Bu öğreticide HDInsight 3.6 üzerinde Apache Kafka kullanılması gerekir. HDInsight kümesinde Kafka oluşturmak nasıl öğrenmek için bkz. [HDInsight üzerinde Apache Kafka kullanmaya başlama](apache-kafka-get-started.md) belge.

## <a name="understand-the-code"></a>Kodu anlama

Örnek uygulama, [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) konumunda `Producer-Consumer` alt dizininde bulunur. Uygulama öncelikli olarak dört dosyadan oluşur:

* `pom.xml`: Bu dosya, proje bağımlılıkları, Java sürümü ve paketleme yöntemleri tanımlar.
* `Producer.java`: Bu dosya, rastgele tümceler producer API kullanarak Kafka'ya gönderir.
* `Consumer.java`: Bu dosya, Kafka'dan veri okumak ve STDOUT yaymak için tüketici API kullanır.
* `Run.java`: Üretici ve tüketici kodu çalıştırmak için kullanılan komut satırı arabirimi.

### <a name="pomxml"></a>Pom.xml

`pom.xml` dosyasında aşağıdaki önemli şeyler anlaşılır:

* Bağımlılıklar: Kafka üretici ve tüketici API'lerini tarafından sağlanan bu proje dayanan `kafka-clients` paket. Aşağıdaki XML kodu, bu bağımlılığı tanımlar:

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

    * `maven-compiler-plugin`: 8 proje tarafından kullanılan Java sürümünü ayarlamak için kullanılır. HDInsight 3.6 tarafından kullanılan Java sürümüdür.
    * `maven-shade-plugin`: Herhangi bir bağımlılığın yanı sıra bu uygulamayı içeren bir uber jar oluşturmak için kullanılır. Ana sınıfı belirtmek zorunda olmadan doğrudan Jar dosyasını çalıştırabilmeniz için uygulamanın giriş noktasını ayarlamak için de kullanılır.

### <a name="producerjava"></a>Producer.java

Üretici, Kafka aracı konakları (çalışan düğümleri) ile iletişim kurar ve verileri bir Kafka konusuna gönderir. Aşağıdaki kod parçacığı dandır [Producer.java](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/blob/master/Producer-Consumer/src/main/java/com/microsoft/example/Producer.java) dosya [GitHub deposu](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) ve üretici özelliklerini ayarlama işlemi gösterilmektedir:

```java
Properties properties = new Properties();
// Set the brokers (bootstrap servers)
properties.setProperty("bootstrap.servers", brokers);
// Set how to serialize key/value pairs
properties.setProperty("key.serializer","org.apache.kafka.common.serialization.StringSerializer");
properties.setProperty("value.serializer","org.apache.kafka.common.serialization.StringSerializer");
KafkaProducer<String, String> producer = new KafkaProducer<>(properties);
```

### <a name="consumerjava"></a>Consumer.java

Tüketici, Kafka aracı konakları (çalışan düğümleri) ile iletişim kurar ve bir döngüdeki kayıtları okur. [Consumer.java](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/blob/master/Producer-Consumer/src/main/java/com/microsoft/example/Consumer.java) dosyasından alınan aşağıdaki kod parçacığı tüketici özelliklerini ayarlar:

```java
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
```

Bu kodda tüketici, konu başlangıcından okumak üzere yapılandırılmıştır (`auto.offset.reset` değeri `earliest` olarak ayarlanır.)

### <a name="runjava"></a>Run.java

[Run.java](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/blob/master/Producer-Consumer/src/main/java/com/microsoft/example/Run.java) dosyası, üretici veya tüketici kodunu çalıştıran bir komut satırı arabirimi sağlar. Kafka aracı konağı bilgilerini bir parametre olarak sağlamanız gerekir. İsteğe bağlı olarak, tüketici işlemi tarafından kullanılan bir grup kimliği değeri ekleyebilirsiniz. Aynı grup kimliğini kullanarak birden fazla tüketici örneği oluşturursanız, bu örnekler konudan okuma yük dengelemesi yapar.

## <a name="build-and-deploy-the-example"></a>Örnek derleme ve dağıtma

1. ve 2 için derleme adımları atlayın ve önceden oluşturulmuş jars(kafka-producer-consumer.jar) dan indirin [ https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/tree/master/Prebuilt-Jars ](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/tree/master/Prebuilt-Jars). Ardından, bu jar dosyası HDInsight kümenize kopyalayabilirsiniz.

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
    
    2. Kafka aracısı ve Apache Zookeeper konakları almak için aşağıdaki komutları kullanın. İstendiğinde, küme oturum açma (yönetici) hesabı için parolayı girin.
    
        ```bash
        export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`; \
        ```

    3. `test` konusunu oluşturmak için aşağıdaki komutu kullanın:

        ```bash
        java -jar kafka-producer-consumer.jar create test $KAFKABROKERS
        ```

3. Üreticiyi çalıştırmak ve konuya veri yazmak için aşağıdaki komutu kullanın:

    ```bash
    java -jar kafka-producer-consumer.jar producer test $KAFKABROKERS
    ```

4. Üretici tamamlandıktan sonra, konu başlığından okumak için aşağıdaki komutu kullanın:

    ```bash
    java -jar kafka-producer-consumer.jar consumer test $KAFKABROKERS
    ```

    Okunan kayıtlar, kayıt sayısıyla birlikte gösterilir.

5. Tüketiciden çıkış yapmak için __Ctrl + C__ tuşlarını kullanın.

### <a name="multiple-consumers"></a>Birden çok tüketici

Kafka tüketicileri kayıtları okurken bir tüketici grubu kullanır. Birden çok tüketiciyle aynı grubun kullanılması, konu başlığından yük dengeli okuma yapılmasına neden olur. Gruptaki her bir tüketici, kayıtların bir kısmını alır.

Tüketici uygulaması, grup kimliği olarak kullanılan bir parametre kabul eder Örneğin, aşağıdaki komut bir `mygroup` grup kimliği kullanarak tüketiciyi başlatır:

```bash
java -jar kafka-producer-consumer.jar consumer test $KAFKABROKERS mygroup
```

Bu işlemi uygulamada görmek için aşağıdaki komutu kullanın:

```bash
tmux new-session 'java -jar kafka-producer-consumer.jar consumer test $KAFKABROKERS mygroup' \; split-w
indow -h 'java -jar kafka-producer-consumer.jar consumer test $KAFKABROKERS mygroup' \; attach
```

Bu komut, terminali iki sütuna bölmek için `tmux` kullanır. Her sütunda aynı grup kimliği değerine sahip bir tüketici başlatılır. Tüketici okumayı tamamladıktan sonra her birinin yalnızca kayıtların bir bölümünü okuduğuna dikkat edin. Kullanım __Ctrl + C__ iki kez çıkmak için `tmux`.

Aynı gruptaki istemcilerin tüketimi, konu başlığının bölümleri aracılığıyla işlenir. Bu kod örneğinde, daha önce oluşturulan `test` konusunda sekiz bölüm vardır. Sekiz tüketici başlatırsanız, her tüketici konunun tek bir bölümünden kayıtları okur.

> [!IMPORTANT]  
> Bir tüketici grubunda bölümden daha fazla tüketici örneği olamaz. Bu örnekte, konu başlığındaki bölüm sayısı sekiz olduğu için bir tüketici grubu en fazla bu sayıda tüketici içerebilir. Ya da her biri en fazla sekiz tüketici içeren birden fazla tüketici grubunuz olabilir.

Kafka’ya depolanan kayıtlar bir bölümde alındıkları sırayla depolanır. *Bir bölüm* içindeki kayıtlar için sıralı teslim sağlamak üzere, tüketici örneklerinin bölüm sayısıyla eşleştiği bir tüketici grubu oluşturun. *Konu başlığı içindeki* kayıtların sıralı teslim edilmesini sağlayabilmek için, yalnızca bir tüketici örneği içeren bir tüketici grubu oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, HDInsight üzerinde Kafka ile Apache Kafka üretici ve tüketici API'sini kullanma hakkında bilgi edindiniz. Kafka ile çalışma hakkında daha fazla bilgi için aşağıdakileri kullanın:

* [Apache Kafka günlüklerini çözümleme](apache-kafka-log-analytics-operations-management.md)
* [Apache Kafka kümeleri arasında verileri çoğaltma](apache-kafka-mirroring.md)
* [Apache Kafka akışlar API'si ile HDInsight](apache-kafka-streams-api.md)
