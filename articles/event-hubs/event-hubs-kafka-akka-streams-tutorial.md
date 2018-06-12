---
title: Akka akışlar için Kafka ekosistemi Azure Event Hubs ile kullanma | Microsoft Docs
description: Olay hub'ı etkin bir Kafka Akka akışları bağlanma
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
ms.date: 06/06/2018
ms.author: bahariri
ms.openlocfilehash: 9db27340a2210ea0be0564b15241952477e592ba
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35304136"
---
# <a name="using-akka-streams-with-event-hubs-for-kafka-ecosystem"></a>Akka akışları Kafka ekosistemi için Event Hubs ile kullanma

Apache Kafka kullanmanın avantajları için bağlanabilir çerçeveleri ekosistemi biridir. Olay hub'ı Kafka etkin Kafka esnekliğini ölçeklenebilirlik, tutarlılık ve Azure ekosistemi desteği ile birleştirir.

Bu öğretici Akka akışları Kafka etkin event hubs'a Protokolü istemcileriniz değiştirme veya kendi kümeleri çalıştıran bağlanma gösterir. Azure Event Hubs ekosistemi destekleyen Kafka için [Apache Kafka sürüm 1.0.](https://kafka.apache.org/10/documentation.html)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşullara sahip olduğunuzdan emin olun:

* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
* [Java Geliştirme Seti (JDK) 1.8 +](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
    * Ubuntu’da JDK’yi yüklemek için `apt-get install default-jdk` komutunu çalıştırın.
    * JAVA_HOME ortam değişkenini JDK’nin yüklü olduğu klasöre işaret edecek şekilde ayarladığınızdan emin olun.
* [Karşıdan](http://maven.apache.org/download.cgi) ve [yükleme](http://maven.apache.org/install.html) Maven ikili arşiv
    * Ubuntu’da Maven’i yüklemek için `apt-get install maven` komutunu çalıştırabilirsiniz.
* [Git](https://www.git-scm.com/downloads)
    * Ubuntu’da Git’i yüklemek için `sudo apt-get install git` komutunu çalıştırabilirsiniz.

## <a name="create-an-event-hubs-namespace"></a>Bir olay hub'ları ad alanı oluşturma

Bir olay hub'ları ad alanı, tüm Event Hubs hizmetinden gönderip için gereklidir. Bkz: [Kafka etkin Event Hubs oluşturmak](event-hubs-create-kafka-enabled.md) bir olay hub'ları Kafka uç nokta alma hakkında bilgi için. Daha sonra kullanmak için olay hub'ları bağlantı dizesini kopyaladığınızdan emin olun.

## <a name="clone-the-example-project"></a>Örnek Proje kopyalama

Bir olay hub'ı Kafka etkin bağlantı dizesine sahip olduğunuza göre Azure Event Hubs depoyu kopyalayın ve gidin `akka` alt:

```shell
git clone https://github.com/Azure/azure-event-hubs.git
cd azure-event-hubs/samples/kafka/akka
```

## <a name="akka-streams-producer"></a>Akka akışları üretici

Sağlanan Akka akışları üretici örneği kullanarak, iletileri Event Hubs hizmetine gönderir.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Bir olay hub'ları Kafka uç nokta sağlar

#### <a name="producer-applicationconf"></a>Üretici application.conf

Güncelleştirme `bootstrap.servers` ve `sasl.jaas.config` değerler `producer/src/main/resources/application.conf` doğru kimlik doğrulaması ile olay hub'ları Kafka uç üretici yönlendirmek için.

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

Üretici komut satırından çalıştırmak için JAR oluşturmak ve ardından Maven içinde çalıştırın (veya sonra Java için sınıf Kafka gerekli JAR(s) ekleyerek çalıştırın Maven kullanarak JAR oluşturmak):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="AkkaTestProducer"
```

Konu, Kafka etkinleştirilmiş olay hub'ına olayların gönderilmesi üretici başlar `test`ve stdout olaylara yazdırır.

## <a name="akka-streams-consumer"></a>Akka akışları tüketici

Sağlanan tüketici örneği kullanarak, Kafka etkin olay hub'larından iletileri alırsınız.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Bir olay hub'ları Kafka uç nokta sağlar

#### <a name="consumer-applicationconf"></a>Tüketici application.conf

Güncelleştirme `bootstrap.servers` ve `sasl.jaas.config` değerler `consumer/src/main/resources/application.conf` doğru kimlik doğrulaması olay hub'ları Kafka noktayla tüketiciye yönlendirmek için.

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

Tüketici komut satırından çalıştırmak için JAR oluşturmak ve ardından Maven içinde çalıştırın (veya sonra Java için sınıf Kafka gerekli JAR(s) ekleyerek çalıştırın Maven kullanarak JAR oluşturmak):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="AkkaTestConsumer"
```

Kafka etkin olay hub'ı olayları varsa (örneğin, üretici de çalıştırıyorsa), olaylar konusundan almaya tüketici başlamadan sonra `test`. 

Kullanıma [Akka akışları Kafka Kılavuzu](https://doc.akka.io/docs/akka-stream-kafka/current/home.html) Akka akışları hakkında daha ayrıntılı bilgi için.

## <a name="next-steps"></a>Sonraki adımlar

* [Event Hubs hakkında bilgi edinin](event-hubs-what-is-event-hubs.md)
* [Kafka Ekosistemi için Event Hubs hakkında bilgi edinin](event-hubs-for-kafka-ecosystem-overview.md)
* Kullanım [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) için [Kafka şirket içi akışı olayları Kafka için Event Hubs bulut üzerinde etkin.](event-hubs-kafka-mirror-maker-tutorial.md)
* Kafka akış öğrenin etkin olay hub'ları kullanarak [yerel Kafka uygulamaları](event-hubs-quickstart-kafka-enabled-event-hubs.md) veya [Apache Flink](event-hubs-kafka-flink-tutorial.md)
