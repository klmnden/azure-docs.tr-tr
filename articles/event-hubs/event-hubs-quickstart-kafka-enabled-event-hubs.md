---
title: Kafka Ekosistemi için Azure Event Hubs’a akış oluşturma | Microsoft Docs
description: Kafka protokolü ve API'leri kullanılarak Event Hubs’a akış oluşturun.
services: event-hubs
documentationcenter: ''
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/08/2018
ms.author: bahariri
ms.openlocfilehash: 8ef6240d19ce1ac1b891c95ce525a8bd211a2900
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35297232"
---
# <a name="stream-into-event-hubs-for-the-kafka-ecosystem"></a>Kafka Ekosistemi için Event Hubs’a akış oluşturma

> [!NOTE]
> Bu örnek [GitHub](https://github.com/Azure/azure-event-hubs)'da sağlanır

Bu hızlı başlangıçta, protokol istemcilerinizi değiştirmenize veya kendi kümelerinizi çalıştırmanıza gerek kalmadan Kafka etkin Event Hubs’a nasıl akış oluşturulacağı gösterilir. Yalnızca uygulamalarınızdaki bir yapılandırma değişikliğiyle Kafka etkin Event Hubs ile konuşmak için üreticilerinizi ve tüketicilerinizi nasıl kullanacağınızı öğrenirsiniz. Kafka ekosistemi için Azure Event Hubs [Apache Kafka sürüm 1.0](https://kafka.apache.org/10/documentation.html)’ı destekler

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için aşağıdaki önkoşulların karşılandığından emin olun:

* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
* Bir Maven ikili arşivini [indirin](http://maven.apache.org/download.cgi) ve [yükleyin](http://maven.apache.org/install.html).
* [Git](https://www.git-scm.com/)
* [Kafka etkin Event Hubs ad alanı](event-hubs-create.md)

## <a name="send-and-receive-messages-with-kafka-in-event-hubs"></a>Event Hubs’da Kafka ile ileti gönderme ve alma

1. [Azure Event Hubs deposunu](https://github.com/Azure/azure-event-hubs) kopyalayın.

2. `azure-event-hubs/samples/kafka/quickstart/producer` sayfasına gidin.

3. `src/main/resources/producer.config` konumundaki üreticinin yapılandırma ayrıntılarını aşağıdaki şekilde güncelleştirin:

    ```xml
    bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
    ```
4. Üretici kodunu çalıştırın ve Kafka etkin Event Hubs’a akış oluşturun:
   
    ```shell
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```
5. `azure-event-hubs/samples/kafka/quickstart/consumer` sayfasına gidin.

6. `src/main/resources/consumer.config` konumundaki tüketicinin yapılandırma ayrıntılarını aşağıdaki şekilde güncelleştirin:
   
    ```xml
    bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
    ```

7. Tüketici kodunu çalıştırın ve Kafka istemcilerinizi kullanarak Kafka etkin Event Hubs’tan işleyin:

    ```java
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestConsumer"                                    
    ```

Event Hubs Kafka kümenizin olayları varsa, bu olayları artık tüketiciden almaya başlarsınız.

## <a name="next-steps"></a>Sonraki adımlar

* [Event Hubs hakkında bilgi edinin](event-hubs-what-is-event-hubs.md)
* [Kafka Ekosistemi için Event Hubs hakkında bilgi edinin](event-hubs-for-kafka-ecosystem-overview.md)
* [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) kullanarak [olayları Kafka şirket içinden bulutta Kafka etkin Event Hubs’a akışla aktarın.](event-hubs-kafka-mirror-maker-tutorial.md)
* [Apache Flink](event-hubs-kafka-flink-tutorial.md) ya da [Akka Streams](event-hubs-kafka-akka-streams-tutorial.md) kullanarak Kafka etkin Event Hubs’a nasıl akış oluşturacağınızı öğrenin.
