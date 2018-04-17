---
title: Apache Kafka üretici ve tüketici API'leri - Azure Hdınsight kullanın | Microsoft Docs
description: Hdınsight üzerinde Kafka ile Apache Kafka üretici ve tüketici API'ları kullanmayı öğrenin. Bu API'leri, yazma ve Apache Kafka okuma uygulamalar geliştirmenize olanak sağlar.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: larryfr
ms.openlocfilehash: 01592401c4c88adeed49b11df4e7963e27b1bcee
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="apache-kafka-producer-and-consumer-apis"></a>Apache Kafka üretici ve tüketici API'leri

Hdınsight üzerinde Kafka ile Kafka üretici ve tüketici API'lerini kullanan bir uygulama oluşturmayı öğrenin.

API'lerde belgeler için bkz: [üretici API](https://kafka.apache.org/documentation/#producerapi) ve [tüketici API](https://kafka.apache.org/documentation/#consumerapi).

## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma

Aşağıdaki bileşenlerin geliştirme ortamınızda yüklü olması gerekir:

* [Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) veya OpenJDK gibi eşdeğeri.

* [Apache Maven](http://maven.apache.org/)

* Bir SSH istemcisi ve `scp` komutu. Daha fazla bilgi için [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md) belgesine bakın.

## <a name="set-up-your-deployment-environment"></a>Dağıtım ortamınızı ayarlama

Bu örnek, Hdınsight 3.6 üzerinde Kafka gerektirir. Hdınsight kümesinde bir Kafka oluşturmayı öğrenmek için bkz: [hdınsight'ta Kafka başlayarak](apache-kafka-get-started.md) belge.

## <a name="build-and-deploy-the-example"></a>Derleme ve örnek dağıtma

1. Örneklerden karşıdan [ https://github.com/Azure-Samples/hdinsight-kafka-java-get-started ](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started).

2. Dizin konumuna değiştirmek `Producer-Consumer` dizin ve aşağıdaki komutu kullanın:

    ```
    mvn clean package
    ```

    Bu komut, `kafka-producer-consumer-1.0-SNAPSHOT.jar` adlı dosyayı içeren `target` adlı bir dizin oluşturur.

3. `kafka-producer-consumer-1.0-SNAPSHOT.jar` dosyasını HDInsight kümenize kopyalamak için aşağıdaki komutları kullanın:
   
    ```bash
    scp ./target/kafka-producer-consumer-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-producer-consumer.jar
    ```
   
    **SSHUSER** değerini kümenizin SSH kullanıcısı ile, **CLUSTERNAME** değerini kümenizin adıyla değiştirin. İstendiğinde, SSH kullanıcısının parolasını girin.

## <a id="run"></a> Örneği çalıştırın

1. Küme için bir SSH bağlantısı açmak için aşağıdaki komutu kullanın:

    ```bash
    ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    **SSHUSER** değerini kümenizin SSH kullanıcısı ile, **CLUSTERNAME** değerini kümenizin adıyla değiştirin. İstenirse, SSH kullanıcı hesabı için parolayı girin. Kullanma hakkında daha fazla bilgi için `scp` Hdınsight ile bkz [Hdınsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Bu örnek tarafından kullanılan konular Kafka oluşturmak için aşağıdaki komutları kullanın:

    ```bash
    sudo apt -y install jq

    CLUSTERNAME='your cluster name'

    export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`

    export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
    ```

    Değiştir __küme adınızı__ Hdınsight kümenizin adıyla. İstendiğinde, Hdınsight küme oturum açma hesabı için parolayı girin.

    > [!NOTE]
    > Küme oturum açma, varsayılan değerinden farklı olup olmadığını `admin`, yerine `admin` , küme oturum açma adınızı önceki komutlarıyla değeri.

3. Üretici çalıştırın ve konuya veri yazmak için aşağıdaki komutu kullanın:

    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS
    ```

4. Üretici tamamladığında, konuyu okumak için aşağıdaki komutu kullanın:
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS
    ```
   
    Okunan kayıtlar, kayıt sayısıyla birlikte gösterilir.

5. Tüketiciden çıkış yapmak için __Ctrl + C__ tuşlarını kullanın.

### <a name="multiple-consumers"></a>Birden çok tüketici

Kafka tüketicileri kayıtları okurken bir tüketici grubu kullanır. Birden çok tüketiciyle aynı grubun kullanılması, konu başlığından yük dengeli okuma yapılmasına neden olur. Gruptaki her bir tüketici, kayıtların bir kısmını alır.

Tüketici uygulama grubunun kimliği olarak kullanılan bir parametre kabul eder Örneğin, aşağıdaki komut, bir grup kimliği kullanan bir tüketici başlatır `mygroup`:
   
```bash
java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS mygroup
```

Bu eylem işlemde görmek için aşağıdaki adımları kullanın.

1. 1 ve 2. adımları yineleyin [örneği çalıştırmak](#run) bölümüne yeni bir SSH oturumu açın.

2. Her iki SSH oturumları aşağıdaki komutu girin:

    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS mygroup
    ```
    
    > [!IMPORTANT]
    > Her iki komut aynı anda, böylece her ikisi de aynı anda çalıştırdıkları girin.

    İleti sayısı burada yalnızca tek bir tüketici b önceki bölümde aynı değil okuma dikkat edin. Bunun yerine, iletileri okuma örnekleri arasında bölünür.

Aynı gruptaki istemcilerin tüketimi, konu başlığının bölümleri aracılığıyla işlenir. Daha önce oluşturulan `test` konu başlığında sekiz bölüm vardır. Sekiz SSH oturumu açıp tüm oturumlarda bir tüketici başlatırsanız her tüketici, konu başlığının tek bir bölümündeki kayıtları okur.

> [!IMPORTANT]
> Bir tüketici grubunda bölümden daha fazla tüketici örneği olamaz. Bu örnekte, konu başlığındaki bölüm sayısı sekiz olduğu için bir tüketici grubu en fazla bu sayıda tüketici içerebilir. Ya da her biri en fazla sekiz tüketici içeren birden fazla tüketici grubunuz olabilir.

Kafka’ya depolanan kayıtlar bir bölümde alındıkları sırayla depolanır. *Bir bölüm* içindeki kayıtlar için sıralı teslim sağlamak üzere, tüketici örneklerinin bölüm sayısıyla eşleştiği bir tüketici grubu oluşturun. *Konu başlığı içindeki* kayıtların sıralı teslim edilmesini sağlayabilmek için, yalnızca bir tüketici örneği içeren bir tüketici grubu oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Kafka üretici ve tüketici API hdınsight'ta Kafka ile nasıl kullanılacağı hakkında bilgi edindiniz. Kafka ile çalışma hakkında daha fazla bilgi için aşağıdakileri kullanın:

* [Kafka günlüklerini çözümleme](apache-kafka-log-analytics-operations-management.md)
* [Kafka kümeleri arasında verileri çoğaltma](apache-kafka-mirroring.md)
* [HDInsight ile Kafka Akışı API’si](apache-kafka-streams-api.md)
* [Apache Spark akışını (DStream) HDInsight üzerinde Kafka ile kullanma](../hdinsight-apache-spark-with-kafka.md)
* [Apache Spark Yapılandırılmış Akışını HDInsight üzerinde Kafka ile kullanma](../hdinsight-apache-kafka-spark-structured-streaming.md)
* [HDInsight üzerinde verileri Kafka’dan Cosmos DB’ye taşımak için Apache Spark Yapılandırılmış Akış’ı kullanma](../apache-kafka-spark-structured-streaming-cosmosdb.md)
* [Apache Storm’u HDInsight üzerinde Kafka ile kullanma](../hdinsight-apache-storm-with-kafka.md)
* [Azure Sanal Ağ üzerinden Kafka’ya bağlanma](apache-kafka-connect-vpn-gateway.md)