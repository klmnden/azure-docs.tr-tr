---
title: Apache Kafka için Azure Event Hubs’a akış oluşturma | Microsoft Docs
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
ms.date: 08/01/2018
ms.author: bahariri
ms.openlocfilehash: 2fbca7179d8bc64a92f79f6eaced8d4f1666c530
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49069225"
---
# <a name="stream-into-event-hubs-for-the-apache-kafka"></a>Apache Kafka için Event Hubs’da akış yapma
Bu hızlı başlangıçta, protokol istemcilerinizi değiştirmenize veya kendi kümelerinizi çalıştırmanıza gerek kalmadan Kafka etkin Event Hubs’a nasıl akış oluşturulacağı gösterilir. Yalnızca uygulamalarınızdaki bir yapılandırma değişikliğiyle Kafka etkin Event Hubs ile konuşmak için üreticilerinizi ve tüketicilerinizi nasıl kullanacağınızı öğrenirsiniz. Azure Event Hubs [Apache Kafka sürüm 1.0](https://kafka.apache.org/10/documentation.html)’ı destekler.

> [!NOTE]
> Bu örnek [GitHub](https://github.com/Azure/azure-event-hubs)'da sağlanır

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için aşağıdaki önkoşulların karşılandığından emin olun:

* [Apache Kafka için Event Hubs](event-hubs-for-kafka-ecosystem-overview.md) makalesini okuyun.
* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
* Bir Maven ikili arşivini [indirin](http://maven.apache.org/download.cgi) ve [yükleyin](http://maven.apache.org/install.html).
* [Git](https://www.git-scm.com/)
* [Kafka etkin Event Hubs ad alanı](event-hubs-create.md)

## <a name="create-a-kafka-enabled-event-hubs-namespace"></a>Kafka etkin Event Hubs ad alanı oluşturma

1. [Azure portalda][Azure portalda] oturum açın ve ekranın sol üst köşesindeki **Kaynak oluştur**'a tıklayın.

2. Event Hubs araması yapın ve burada gösterilen seçenekleri belirleyin:
    
    ![Portalda Event Hubs arama](./media/event-hubs-create-kafka-enabled/event-hubs-create-event-hubs.png)
 
3. Benzersiz bir ad belirtin ve ad alanında Kafka'yı etkinleştirin. **Oluştur**’a tıklayın.
    
    ![Ad alanı oluşturma](./media/event-hubs-create-kafka-enabled/create-kafka-namespace.png)
 
4. Ad alanı oluşturulduktan sonra **Ayarlar** sekmesinde **Paylaşılan erişim ilkeleri**'ne tıklayarak bağlantı dizesini alın.

    ![Paylaşılan erişim ilkeleri’ne tıklayın.](./media/event-hubs-create/create-event-hub7.png)

5. Varsayılan **RootManageSharedAccessKey** ilkesini seçebilir veya yeni bir ilke ekleyebilirsiniz. İlke adına tıklayın ve bağlantı dizesini kopyalayın. 
    
    ![İlke seçme](./media/event-hubs-create/create-event-hub8.png)
 
6. Bu bağlantı dizesini Kafka uygulaması yapılandırmanıza ekleyin.

Artık Kafka protokolünü kullanan uygulamalarınızdaki olayların akışını Event Hubs'a yapabilirsiniz.

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
Bu makalede protokol istemcilerinizi değiştirmenize veya kendi kümelerinizi çalıştırmanıza gerek kalmadan Kafka etkin Event Hubs’a nasıl akış oluşturacağınızı öğrendiniz. Daha fazla bilgi edinmek için aşağıdaki öğreticiyle devam edin:

> [!div class="nextstepaction"]
> [Event Hubs ile Kafka MirrorMaker'ı kullanma](event-hubs-kafka-mirror-maker-tutorial.md)
