---
title: Akka akışları Azure Event Hubs ile için Apache Kafka kullanma | Microsoft Docs
description: Olay hub'ı etkin Akka akışları bir Kafka'ya bağlanma
services: event-hubs
documentationcenter: ''
author: basilhariri
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: mvc
ms.date: 08/06/2018
ms.author: bahariri
ms.openlocfilehash: 9cfaec69d3c9cea7f6f3860e8f07df6dc1638a9a
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50416078"
---
# <a name="using-akka-streams-with-event-hubs-for-apache-kafka"></a>Akka akışları için Apache Kafka ile Event hubs'ı kullanma
Bu öğreticide Protokolü istemcilerinize değiştirme veya kendi kümeleri çalıştıran Akka akışları Kafka özellikli event hubs'a bağlanma gösterilmektedir. Kafka için Azure Event Hubs'ı destekleyen [Apache Kafka sürüm 1.0.](https://kafka.apache.org/10/documentation.html)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Event Hubs ad alanı oluşturma
> * Örnek projesini kopyalama
> * Üretici Akka akışları çalıştırma 
> * Tüketici Akka akışları çalıştırma

> [!NOTE]
> Bu örnek [GitHub](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/akka/java)'da sağlanır

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulların karşılandığından emin olun:

* [Apache Kafka için Event Hubs](event-hubs-for-kafka-ecosystem-overview.md) makalesini okuyun. 
* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
* [Java Geliştirme Seti (JDK) 1.8 +](https://aka.ms/azure-jdks)
    * Ubuntu’da JDK’yi yüklemek için `apt-get install default-jdk` komutunu çalıştırın.
    * JAVA_HOME ortam değişkenini JDK’nin yüklü olduğu klasöre işaret edecek şekilde ayarladığınızdan emin olun.
* [İndirme](http://maven.apache.org/download.cgi) ve [yükleme](http://maven.apache.org/install.html) Maven ikili Arşivi
    * Ubuntu’da Maven’i yüklemek için `apt-get install maven` komutunu çalıştırabilirsiniz.
* [Git](https://www.git-scm.com/downloads)
    * Ubuntu’da Git’i yüklemek için `sudo apt-get install git` komutunu çalıştırabilirsiniz.

## <a name="create-an-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma

Tüm Event Hubs hizmetinden gönderileceği ve alınacağı bir Event Hubs ad alanı gereklidir. Bkz: [Event Hubs etkin Kafka oluşturma](event-hubs-create-kafka-enabled.md) bir olay hub'ları Kafka uç nokta alma hakkında bilgi için. Daha sonra kullanmak için Event Hubs bağlantı dizesi kopyaladığınızdan emin olun.

## <a name="clone-the-example-project"></a>Örnek projesini kopyalama

Event Hubs, Kafka özellikli bir bağlantı dizesi olduğuna göre Azure Event Hubs Kafka deposu için kopyalama ve gidin `akka` alt klasörü:

```shell
git clone https://github.com/Azure/azure-event-hubs-for-kafka.git
cd azure-event-hubs-for-kafka/tutorials/akka/java
```

## <a name="run-akka-streams-producer"></a>Üretici Akka akışları çalıştırma

Sağlanan Akka akışları üretici örneği kullanarak Event Hubs hizmeti için iletiler gönderin.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Bir olay hub'ları Kafka uç noktası sağlayın

#### <a name="producer-applicationconf"></a>Üretici application.conf

Güncelleştirme `bootstrap.servers` ve `sasl.jaas.config` değerler `producer/src/main/resources/application.conf` üretici doğru kimlik doğrulaması ile Event Hubs Kafka uç noktasına yönlendirmek için.

```xml
akka.kafka.producer {
    #Akka Kafka producer properties can be defined here


    # Properties defined by org.apache.kafka.clients.producer.ProducerConfig
    # can be defined in this configuration section.
    kafka-clients {
        bootstrap.servers="{YOUR.EVENTHUBS.FQDN}:9093"
        sasl.mechanism=PLAIN
        security.protocol=SASL_SSL
        sasl.jaas.config="org.apache.kafka.common.security.plain.PlainLoginModule required username=\"$ConnectionString\" password=\"{YOUR.EVENTHUBS.CONNECTION.STRING}\";"
    }
}
```

### <a name="run-producer-from-the-command-line"></a>Üretici komut satırından çalıştırma

Üretici komut satırından çalıştırmak için JAR oluşturmak ve ardından Maven içinde çalıştırın (veya daha sonra sınıf için gerekli Kafka JAR(s) ekleyerek Java'da çalıştırma Maven kullanarak bir JAR oluşturmak):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="AkkaTestProducer"
```

Üretici konu, Kafka etkin olay hub'ına olay göndermeye başlar `test`ve stdout olaylara yazdırır.

## <a name="run-akka-streams-consumer"></a>Tüketici Akka akışları çalıştırma

Sağlanan tüketici örneği kullanarak iletiler Kafka özellikli event hubs'dan alır.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Bir olay hub'ları Kafka uç noktası sağlayın

#### <a name="consumer-applicationconf"></a>Tüketici application.conf

Güncelleştirme `bootstrap.servers` ve `sasl.jaas.config` değerler `consumer/src/main/resources/application.conf` tüketici doğru kimlik doğrulaması ile Event Hubs Kafka uç noktasına yönlendirmek için.

```xml
akka.kafka.consumer {
    #Akka Kafka consumer properties defined here
    wakeup-timeout=60s

    # Properties defined by org.apache.kafka.clients.consumer.ConsumerConfig
    # defined in this configuration section.
    kafka-clients {
       request.timeout.ms=60000
       group.id=akka-example-consumer

       bootstrap.servers="{YOUR.EVENTHUBS.FQDN}:9093"
       sasl.mechanism=PLAIN
       security.protocol=SASL_SSL
       sasl.jaas.config="org.apache.kafka.common.security.plain.PlainLoginModule required username=\"$ConnectionString\" password=\"{YOUR.EVENTHUBS.CONNECTION.STRING}\";"
    }
}
```

### <a name="run-consumer-from-the-command-line"></a>Tüketici komut satırından çalıştırma

Tüketici komut satırından çalıştırmak için JAR oluşturmak ve ardından Maven içinde çalıştırın (veya daha sonra sınıf için gerekli Kafka JAR(s) ekleyerek Java'da çalıştırma Maven kullanarak bir JAR oluşturmak):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="AkkaTestConsumer"
```

Kafka özellikli bir olay hub'ı olayları varsa (örneğin, üretici da çalışıyorsa), sonra da tüketici, konu başlığından olayları almaya başlar `test`. 

Kullanıma [Akka akışları Kafka Kılavuzu](https://doc.akka.io/docs/akka-stream-kafka/current/home.html) Akka akışları hakkında daha ayrıntılı bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Protokolü istemcilerinize değiştirme veya kendi kümeleri çalıştıran Akka akışları Kafka özellikli event hubs'a bağlanma öğrendiniz. Kafka için Azure Event Hubs'ı destekleyen [Apache Kafka sürüm 1.0.](https://kafka.apache.org/10/documentation.html). Bu öğreticinin bir parçası olarak aşağıdaki eylemleri yapar: 

> [!div class="checklist"]
> * Event Hubs ad alanı oluşturma
> * Örnek projesini kopyalama
> * Üretici Akka akışları çalıştırma 
> * Tüketici Akka akışları çalıştırma

Kafka için Event Hubs ile Event Hubs hakkında daha fazla bilgi edinmek için şu konuya bakın:  

* [Event Hubs hakkında bilgi edinin](event-hubs-what-is-event-hubs.md)
* [Kafka için Event Hubs hakkında bilgi edinin](event-hubs-for-kafka-ecosystem-overview.md)
* [Kafka için Event Hubs GitHub'ındaki diğer örnekleri keşfedin](https://github.com/Azure/azure-event-hubs-for-kafka)
* [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) kullanarak [olayları Kafka şirket içinden bulutta Kafka etkin Event Hubs’a akışla aktarın.](event-hubs-kafka-mirror-maker-tutorial.md)
* Kafka akışı yapmayı öğrenin etkin Event Hubs kullanarak [yerel Kafka uygulamalar](event-hubs-quickstart-kafka-enabled-event-hubs.md) veya [Apache Flink](event-hubs-kafka-flink-tutorial.md)
