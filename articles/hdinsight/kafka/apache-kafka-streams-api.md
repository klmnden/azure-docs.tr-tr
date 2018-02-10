---
title: "Azure Hdınsight Apache Kafka akışları API - kullanma | Microsoft Docs"
description: "İle Kafka Hdınsight üzerinde Apache Kafka akışları API kullanmayı öğrenin. Bu API, akış Kafka konularında arasında işleme gerçekleştirmenizi sağlar."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: larryfr
ms.openlocfilehash: be6ed6d4c0c3a5fa55166b84b128881d434c4ab2
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="apache-kafka-streams-api"></a>Apache Kafka streams API

Kafka akışları API'sini kullanan bir uygulama oluşturmak ve Hdınsight'ta Kafka ile çalıştırma hakkında bilgi edinin.

Apache Kafka ile çalışırken, akış işleme genellikle yapılır Apache Spark veya Storm kullanarak. Kafka sürümünde 0.10.0 (Hdınsight 3.5 ve 3.6) Kafka akışları API sunmuştur. Bu API, veri akışlarını Kafka üzerinde çalışan bir Web uygulaması kullanarak, giriş ve çıkış konuları arasında dönüştürme olanak sağlar. Bazı durumlarda, bu bir Spark ya da çözüm akış Storm oluşturmak için bir alternatif olabilir. Kafka akışları hakkında daha fazla bilgi için bkz: [giriş akışlara](https://kafka.apache.org/10/documentation/streams/) Apache.org belgeler.

## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma

Aşağıdaki bileşenlerin geliştirme ortamınızda yüklü olması gerekir:

* [Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) veya OpenJDK gibi eşdeğeri.

* [Apache Maven](http://maven.apache.org/)

* Bir SSH istemcisi ve `scp` komutu. Daha fazla bilgi için [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md) belgesine bakın.

## <a name="set-up-your-deployment-environment"></a>Dağıtım ortamınızı ayarlama

Bu örnek, Hdınsight 3.6 üzerinde Kafka gerektirir. Hdınsight kümesinde bir Kafka oluşturmayı öğrenmek için bkz: [hdınsight'ta Kafka başlayarak](apache-kafka-get-started.md) belge.

## <a name="build-and-deploy-the-example"></a>Derleme ve örnek dağıtma

Derleme ve Hdınsight kümesinde, Kafka projeyi dağıtmak için aşağıdaki adımları kullanın.

1. Örnekleri [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) sayfasından indirebilirsiniz.

2. Değiştirme dizinleri `Streaming` dizin ve ardından şu komutu jar paketi oluşturmak için:

    ```bash
    mvn clean package
    ```

    Bu komut, `kafka-streaming-1.0-SNAPSHOT.jar` adlı dosyayı içeren `target` adlı bir dizin oluşturur.

3. Kopyalamak için aşağıdaki komutu kullanın `kafka-streaming-1.0-SNAPSHOT.jar` Hdınsight kümenize dosyası:
   
    ```bash
    scp ./target/kafka-streaming-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-streaming.jar
    ```
   
    **SSHUSER** değerini kümenizin SSH kullanıcısı ile, **CLUSTERNAME** değerini kümenizin adıyla değiştirin. İstenirse, SSH kullanıcı hesabı için parolayı girin. Kullanma hakkında daha fazla bilgi için `scp` Hdınsight ile bkz [Hdınsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="run-the-example"></a>Örneği çalıştırın

1. Küme için bir SSH bağlantısı açmak için aşağıdaki komutu kullanın:

    ```bash
    ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    **SSHUSER** değerini kümenizin SSH kullanıcısı ile, **CLUSTERNAME** değerini kümenizin adıyla değiştirin. İstenirse, SSH kullanıcı hesabı için parolayı girin. Kullanma hakkında daha fazla bilgi için `scp` Hdınsight ile bkz [Hdınsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

4. Bu örnek tarafından kullanılan konular Kafka oluşturmak için aşağıdaki komutları kullanın:

    ```bash
    sudo apt -y install jq

    CLUSTERNAME='your cluster name'

    export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`

    export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcounts --zookeeper $KAFKAZKHOSTS
    ```

    Değiştir __küme adınızı__ Hdınsight kümenizin adıyla. İstendiğinde, Hdınsight küme oturum açma hesabı için parolayı girin.

    > [!NOTE]
    > Küme oturum açma, varsayılan değerinden farklı olup olmadığını `admin`, yerine `admin` , küme oturum açma adınızı önceki komutlarıyla değeri.

5. Bu örneği çalıştırmak için üç şey yapmanız gerekir:

    * İçinde yer alan akışları çözüm Başlat `kafka-streaming.jar`.
    * Yazar bir üretici Başlat `test` konu.
    * Yazılan çıkış görmenize olanak tanıyan bir tüketici Başlat `wordcounts` konu

    > [!NOTE]
    > Doğrulamak gereken `auto.create.topics.enable` özelliği ayarlanmış `true` Kafka Aracısı Yapılandırma dosyasında. Bu özellik görüntülenebilir ve Gelişmiş Kafka Aracısı Yapılandırma dosyasında Ambari Web kullanıcı Arabirimi kullanılarak değiştirilebilir. Aksi takdirde, Ara konu oluşturmak için gereken `RekeyedIntermediateTopic` aşağıdaki komutu kullanarak bu örneği çalıştırmadan önce el ile:
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic RekeyedIntermediateTopic  --zookeeper $KAFKAZKHOSTS
    ```
    
    Bu işlemleri gerçekleştirmek için üç SSH oturum açarak. Ancak, ardından ayarlamanız gerekir `$KAFKABROKERS` ve `$KAFKAZKHOSTS` her çalıştırarak bu bölümünden her SSH oturumunda adım 4. Daha kolay bir çözüm kullanmaktır `tmux` birden çok bölümlere geçerli SSH görüntü bölebilirsiniz yardımcı programı. Akış, üretici ve tüketici kullanarak başlatmak için `tmux`, aşağıdaki komutu kullanın:

    ```bash
    tmux new-session '/usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic wordcounts --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer' \; split-window -h 'java -jar kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS' \; split-window -v '/usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test' \; attach
    ```

    Bu komut, SSH görüntü üç bölümlere ayırır:

    * Hangi okuma iletileri bir konsol tüketici sol bölümüne çalıştıran `wordcounts` konu:`/usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic wordcounts --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer`

        > [!NOTE]
        > `--property` Parametre sayısı (değer) ile birlikte (word) anahtarı yazdırmak için konsol tüketici söyleyin. Bu parametre, ayrıca bu değerleri Kafka okurken kullanılacak seri durumdan çıkarıcının yapılandırır.

    * Sağ üst kısmında akışları API çözüm çalıştırır:`java -jar kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS`

    * Alt sağ bölümündeki konsol üretici çalıştırır ve iletileri göndermek için girmenizi üzerinde bekler `test` konu:`/usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test`
 
6. Sonra `tmux` komutu böler görüntüleme, imlecinizi sağ alt bölümünde. Cümleleri girerek başlayın. Her tümcenin sonra sol bölmede benzersiz sözcük sayısını göstermek üzere güncelleştirilir. Çıktı aşağıdaki metne benzer:
   
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
    > Bir sözcükle karşılaşılan her durumda sayı artar.

7. Kullanım __Ctrl + C__ üretici çıkmak için. Kullanmaya devam __Ctrl + C__ uygulama ve tüketici çıkmak için.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, hdınsight'ta Kafka ile Kafka akışları API kullanma hakkında bilgi edindiniz. Kafka ile çalışma hakkında daha fazla bilgi için aşağıdakileri kullanın:

* [Kafka günlüklerini çözümleme](apache-kafka-log-analytics-operations-management.md)
* [Kafka kümeleri arasında verileri çoğaltma](apache-kafka-mirroring.md)
* [HDInsight ile Kafka Üretici ve Tüketici API’si](apache-kafka-producer-consumer-api.md)
* [Apache Spark akışını (DStream) HDInsight üzerinde Kafka ile kullanma](../hdinsight-apache-spark-with-kafka.md)
* [Apache Spark Yapılandırılmış Akışını HDInsight üzerinde Kafka ile kullanma](../hdinsight-apache-kafka-spark-structured-streaming.md)
* [HDInsight üzerinde verileri Kafka’dan Cosmos DB’ye taşımak için Apache Spark Yapılandırılmış Akış’ı kullanma](../apache-kafka-spark-structured-streaming-cosmosdb.md)
* [Apache Storm’u HDInsight üzerinde Kafka ile kullanma](../hdinsight-apache-storm-with-kafka.md)
* [Azure Sanal Ağ üzerinden Kafka’ya bağlanma](apache-kafka-connect-vpn-gateway.md)
