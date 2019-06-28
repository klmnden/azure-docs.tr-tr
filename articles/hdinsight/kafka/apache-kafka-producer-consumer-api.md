---
title: "Öğretici: Apache Kafka üretici ve tüketici API'lerini - Azure HDInsight'ı kullanın "
description: HDInsight’ta Apache Kafka Üretici ve Tüketici API’lerini kullanmayı öğrenin. Bu öğreticide, bu API’leri HDInsight üzerinde Kafka ile kullanmayı öğreneceksiniz.
author: dhgoelmsft
ms.author: dhgoel
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 06/24/2019
ms.openlocfilehash: 7a23d30e940417a6191cf14ad5d60159bd11c3da
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446410"
---
# <a name="tutorial-use-the-apache-kafka-producer-and-consumer-apis"></a>Öğretici: Apache Kafka üretici ve tüketici API'lerini kullanma

HDInsight’ta Apache Kafka Üretici ve Tüketici API’lerini kullanmayı öğrenin.

Kafka Üretici API’si, uygulamaların Kafka kümesine veri akışları göndermesine olanak tanır. Kafka Tüketici API’si, uygulamaların kümeden veri akışları okumasına olanak tanır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Önkoşullar
> * Kodu anlama
> * Uygulama derleme ve dağıtma
> * Uygulamayı küme üzerinde çalıştırma

API’ler hakkında daha fazla bilgi için [Üretici API’si](https://kafka.apache.org/documentation/#producerapi) ve [Tüketici API’si](https://kafka.apache.org/documentation/#consumerapi) hakkında Apache belgelerine bakın.

## <a name="prerequisites"></a>Önkoşullar

* HDInsight 3.6 üzerinde Apache Kafka. HDInsight kümesinde Kafka oluşturmak nasıl öğrenmek için bkz. [HDInsight üzerinde Apache Kafka kullanmaya başlama](apache-kafka-get-started.md).

* [Java Developer Kit (JDK) 8 sürümünü](https://aka.ms/azure-jdks) veya OpenJDK gibi eşdeğeri.

* [Apache Maven](https://maven.apache.org/download.cgi) düzgün [yüklü](https://maven.apache.org/install.html) Apache göre.  Maven derleme sistemi Java projeleri için proje olur.

* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="understand-the-code"></a>Kodu anlama

Örnek uygulama, [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) konumunda `Producer-Consumer` alt dizininde bulunur. Uygulama öncelikli olarak dört dosyadan oluşur:

* `pom.xml`: Bu dosya, proje bağımlılıkları, Java sürümü ve paketleme yöntemleri tanımlar.
* `Producer.java`: Bu dosya, rastgele tümceler producer API kullanarak Kafka'ya gönderir.
* `Consumer.java`: Bu dosya, Kafka'dan veri okumak ve STDOUT yaymak için tüketici API kullanır.
* `Run.java`: Üretici ve tüketici kodu çalıştırmak için kullanılan komut satırı arabirimi.

### <a name="pomxml"></a>Pom.xml

`pom.xml` dosyasında aşağıdaki önemli şeyler anlaşılır:

* Bağımlılıkları: Kafka üretici ve tüketici API'lerini tarafından sağlanan bu proje dayanan `kafka-clients` paket. Aşağıdaki XML kodu, bu bağımlılığı tanımlar:

    ```xml
    <!-- Kafka client for producer/consumer operations -->
    <dependency>
      <groupId>org.apache.kafka</groupId>
      <artifactId>kafka-clients</artifactId>
      <version>${kafka.version}</version>
    </dependency>
    ```

    `${kafka.version}` girişi, `pom.xml` dosyasının `<properties>..</properties>` bölümünde bildirilir ve HDInsight kümesinin Kafka sürümüne yapılandırılır.

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

[Run.java](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/blob/master/Producer-Consumer/src/main/java/com/microsoft/example/Run.java) dosyası üretici veya tüketici kod çalışan bir komut satırı arabirimi sağlar. Kafka aracı konağı bilgilerini bir parametre olarak sağlamanız gerekir. İsteğe bağlı olarak tüketici işlemi tarafından kullanılan bir grup kimliği değerini içerebilir. Aynı grup kimliği kullanan birden fazla tüketici örnekleri oluşturursanız, bunlar Yük Dengelemesi konu başlığından okumak.

## <a name="build-and-deploy-the-example"></a>Örnek derleme ve dağıtma

1. İndirin ve ayıklayın örneklerden [ https://github.com/Azure-Samples/hdinsight-kafka-java-get-started ](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started).

2. Geçerli dizin konumuna ayarlayın `hdinsight-kafka-java-get-started\Producer-Consumer` dizin ve aşağıdaki komutu kullanın:

    ```cmd
    mvn clean package
    ```

    Bu komut, `kafka-producer-consumer-1.0-SNAPSHOT.jar` adlı dosyayı içeren `target` adlı bir dizin oluşturur.

3. `sshuser` değerini, kümenizin SSH kullanıcısı ile, `CLUSTERNAME` değerini kümenizin adıyla değiştirin. Kopyalamak için aşağıdaki komutu girin `kafka-producer-consumer-1.0-SNAPSHOT.jar` HDInsight kümenize dosya. İstendiğinde, SSH kullanıcısının parolasını girin.

    ```cmd
    scp ./target/kafka-producer-consumer-1.0-SNAPSHOT.jar sshuser@CLUSTERNAME-ssh.azurehdinsight.net:kafka-producer-consumer.jar
    ```

## <a id="run"></a> Örneği çalıştırma

1. `sshuser` değerini, kümenizin SSH kullanıcısı ile, `CLUSTERNAME` değerini kümenizin adıyla değiştirin. Aşağıdaki komutu girerek küme için bir SSH bağlantısı açın. İstendiğinde, SSH kullanıcı hesabının parolasını girin.

    ```cmd
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

5. Kafka aracısı ve Apache Zookeeper konakları almak için aşağıdaki komutu kullanın:

    ```bash
    export KAFKABROKERS=`curl -sS -u admin:$password -G https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER \
  	| jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    ```

6. Kafka konu oluşturma `myTest`, aşağıdaki komutu girerek:

    ```bash
    java -jar kafka-producer-consumer.jar create myTest $KAFKABROKERS
    ```

7. Üreticiyi çalıştırmak ve konuya veri yazmak için aşağıdaki komutu kullanın:

    ```bash
    java -jar kafka-producer-consumer.jar producer myTest $KAFKABROKERS
    ```

8. Üretici tamamlandıktan sonra, konu başlığından okumak için aşağıdaki komutu kullanın:

    ```bash
    java -jar kafka-producer-consumer.jar consumer myTest $KAFKABROKERS
    ```

    Okunan kayıtlar, kayıt sayısıyla birlikte gösterilir.

9. Tüketiciden çıkış yapmak için __Ctrl + C__ tuşlarını kullanın.

### <a name="multiple-consumers"></a>Birden çok tüketici

Kafka tüketicileri kayıtları okurken bir tüketici grubu kullanır. Birden çok tüketiciyle aynı grubun kullanılması, konu başlığından yük dengeli okuma yapılmasına neden olur. Gruptaki her bir tüketici, kayıtların bir kısmını alır.

Tüketici uygulaması, grup kimliği olarak kullanılan bir parametre kabul eder Örneğin, aşağıdaki komut bir `myGroup` grup kimliği kullanarak tüketiciyi başlatır:

```bash
java -jar kafka-producer-consumer.jar consumer myTest $KAFKABROKERS myGroup
```

Tüketiciden çıkış yapmak için __Ctrl + C__ tuşlarını kullanın.

Bu işlemi uygulamada görmek için aşağıdaki komutu kullanın:

```bash
tmux new-session 'java -jar kafka-producer-consumer.jar consumer myTest $KAFKABROKERS myGroup' \
\; split-window -h 'java -jar kafka-producer-consumer.jar consumer myTest $KAFKABROKERS myGroup' \
\; attach
```

Bu komut, terminali iki sütuna bölmek için `tmux` kullanır. Her sütunda aynı grup kimliği değerine sahip bir tüketici başlatılır. Tüketici okumayı tamamladıktan sonra her birinin yalnızca kayıtların bir bölümünü okuduğuna dikkat edin. Kullanım __Ctrl + C__ iki kez çıkmak için `tmux`.

Aynı gruptaki istemcilerin tüketimi, konu başlığının bölümleri aracılığıyla işlenir. Bu kod örneğinde, daha önce oluşturulan `test` konusunda sekiz bölüm vardır. Sekiz tüketici başlatırsanız, her tüketici konunun tek bir bölümünden kayıtları okur.

> [!IMPORTANT]  
> Bir tüketici grubunda bölümden daha fazla tüketici örneği olamaz. Bu örnekte, konu başlığındaki bölüm sayısı sekiz olduğu için bir tüketici grubu en fazla bu sayıda tüketici içerebilir. Ya da her biri en fazla sekiz tüketici içeren birden fazla tüketici grubunuz olabilir.

Kafka’ya depolanan kayıtlar bir bölümde alındıkları sırayla depolanır. *Bir bölüm* içindeki kayıtlar için sıralı teslim sağlamak üzere, tüketici örneklerinin bölüm sayısıyla eşleştiği bir tüketici grubu oluşturun. *Konu başlığı içindeki* kayıtların sıralı teslim edilmesini sağlayabilmek için, yalnızca bir tüketici örneği içeren bir tüketici grubu oluşturun.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğretici ile oluşturulan kaynakları temizlemek için kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, ilişkili HDInsight kümesini ve kaynak grubuyla ilişkili diğer tüm kaynakları da siler.

Azure portalını kullanarak kaynak grubunu kaldırmak için:

1. Azure portalında sol taraftaki menüyü genişleterek hizmet menüsünü açın ve sonra __Kaynak Grupları__'nı seçerek kaynak gruplarınızın listesini görüntüleyin.
2. Silinecek kaynak grubunu bulun ve sonra listenin sağ tarafındaki __Daha fazla__ düğmesine (...) sağ tıklayın.
3. __Kaynak grubunu sil__'i seçip onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, HDInsight üzerinde Kafka ile Apache Kafka üretici ve tüketici API'sini kullanma hakkında bilgi edindiniz. Kafka ile çalışma hakkında daha fazla bilgi için aşağıdakileri kullanın:

> [!div class="nextstepaction"]
> [Apache Kafka günlüklerini çözümleme](apache-kafka-log-analytics-operations-management.md)