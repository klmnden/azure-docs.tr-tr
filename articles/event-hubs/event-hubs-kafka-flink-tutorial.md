---
title: Apache Flink Azure Event Hubs ile Kafka ekosistemi için kullanın. | Microsoft Docs
description: Olay hub'ı bir Kafka için bağlantı Apache Flink etkin
services: event-hubs
documentationcenter: ''
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: mvc
ms.date: 06/06/2018
ms.author: bahariri
ms.openlocfilehash: cb7ef0e9b6a612e3f4116cb626903770e4035368
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35304137"
---
# <a name="apache-flink-with-event-hubs-for-the-kafka-ecosystem"></a>Apache Flink Kafka ekosistemi için Event Hubs ile

Apache Kafka kullanmanın avantajları için bağlanabilir çerçeveleri ekosistemi biridir. Kafka olay hub'ları birleştiren Kafka esnekliğini ölçeklenebilirlik, tutarlılık ve Azure ekosistemi desteği ile etkinleştirilir.

Bu öğretici, Apache Flink Kafka etkin event hubs'a Protokolü istemcileriniz değiştirme veya kendi kümeleri çalıştıran bağlanma gösterir. Azure Event Hubs Kafka ekosistemi desteklediği için [Apache Kafka sürüm 1.0.](https://kafka.apache.org/10/documentation.html)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşullara sahip olduğunuzdan emin olun:

* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
    * Ubuntu’da JDK’yi yüklemek için `apt-get install default-jdk` komutunu çalıştırın.
    * JAVA_HOME ortam değişkenini JDK’nin yüklü olduğu klasöre işaret edecek şekilde ayarladığınızdan emin olun.
* [Karşıdan](http://maven.apache.org/download.cgi) ve [yükleme](http://maven.apache.org/install.html) Maven ikili arşiv
    * Ubuntu’da Maven’i yüklemek için `apt-get install maven` komutunu çalıştırabilirsiniz.
* [Git](https://www.git-scm.com/downloads)
    * Ubuntu’da Git’i yüklemek için `sudo apt-get install git` komutunu çalıştırabilirsiniz.

## <a name="create-an-event-hubs-namespace"></a>Bir olay hub'ları ad alanı oluşturma

Bir olay hub'ları ad alanı, tüm Event Hubs hizmetinden gönderip için gereklidir. Bkz: [Kafka etkin Event Hubs oluşturmak](event-hubs-create-kafka-enabled.md) bir olay hub'ları Kafka uç nokta alma hakkında bilgi için. Daha sonra kullanmak için olay hub'ları bağlantı dizesini kopyaladığınızdan emin olun.

## <a name="clone-the-example-project"></a>Örnek Proje kopyalama

Bir olay hub'ı Kafka etkin bağlantı dizesine sahip olduğunuza göre Azure Event Hubs depoyu kopyalayın ve gidin `flink` alt:

```shell
git clone https://github.com/Azure/azure-event-hubs.git
cd azure-event-hubs/samples/kafka/flink
```

## <a name="flink-producer"></a>Flink üretici

Sağlanan Flink üretici örneği kullanarak, iletileri Event Hubs hizmetine gönderir.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Bir olay hub'ları Kafka uç nokta sağlar

#### <a name="producerconfig"></a>Producer.config

Güncelleştirme `bootstrap.servers` ve `sasl.jaas.config` değerler `producer/src/main/resources/producer.config` doğru kimlik doğrulaması ile olay hub'ları Kafka uç üretici yönlendirmek için.

```xml
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
client.id=FlinkExampleProducer
sasl.mechanism=PLAIN
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
   username="$ConnectionString" \
   password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

### <a name="run-producer-from-the-command-line"></a>Üretici komut satırından çalıştırma

Üretici komut satırından çalıştırmak için JAR oluşturmak ve ardından Maven içinde çalıştırın (veya sonra Java için sınıf Kafka gerekli JAR(s) ekleyerek çalıştırın Maven kullanarak JAR oluşturmak):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="FlinkTestProducer"
```

Üretici şimdi başlayacak konusuna etkin olay hub'ı Kafka olayların gönderilmesi `test` ve stdout olaylara yazdırma.

## <a name="flink-consumer"></a>Flink tüketici

Gelen iletileri almak sağlanan tüketici örnek kullanarak Event Hubs Kafka etkin.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Bir olay hub'ları Kafka uç nokta sağlar

#### <a name="consumerconfig"></a>Consumer.config

Güncelleştirme `bootstrap.servers` ve `sasl.jaas.config` değerler `consumer/src/main/resources/consumer.config` doğru kimlik doğrulaması olay hub'ları Kafka noktayla tüketiciye yönlendirmek için.

```xml
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
group.id=FlinkExampleConsumer
sasl.mechanism=PLAIN
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
   username="$ConnectionString" \
   password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

### <a name="run-consumer-from-the-command-line"></a>Tüketici komut satırından çalıştırma

Tüketici komut satırından çalıştırmak için JAR oluşturmak ve ardından Maven içinde çalıştırın (veya sonra Java için sınıf Kafka gerekli JAR(s) ekleyerek çalıştırın Maven kullanarak JAR oluşturmak):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="FlinkTestConsumer"
```

Kafka etkin olay hub'ı olaylar (örneğin, üretici da çalıştırılıyorsa) varsa, ardından tüketici şimdi olayları konusundan almaya başlamadan `test`.

Kullanıma [Flink'ın Kafka bağlayıcı Kılavuzu](https://ci.apache.org/projects/flink/flink-docs-stable/dev/connectors/kafka.html) Flink Kafka için bağlanma hakkında daha ayrıntılı bilgi için.

## <a name="next-steps"></a>Sonraki adımlar

* [Event Hubs hakkında bilgi edinin](event-hubs-what-is-event-hubs.md)
* [Kafka Ekosistemi için Event Hubs hakkında bilgi edinin](event-hubs-for-kafka-ecosystem-overview.md)
* Kullanım [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) için [Kafka şirket içi akışı olayları Kafka için Event Hubs bulut üzerinde etkin.](event-hubs-kafka-mirror-maker-tutorial.md)
* Kafka akış öğrenin etkin olay hub'ları kullanarak [yerel Kafka uygulamaları](event-hubs-quickstart-kafka-enabled-event-hubs.md) veya [Akka akışları](event-hubs-kafka-akka-streams-tutorial.md).
