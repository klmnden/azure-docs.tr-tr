---
title: Kafka MirrorMaker Kafka ekosistemi için Azure Event Hubs ile kullanma | Microsoft Docs
description: Olay hub'ları Kafka kümede yansıtmak üzere Kafka MirrorMaker kullanın.
services: event-hubs
documentationcenter: .net
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.topic: mirror-maker
ms.custom: mvc
ms.date: 05/07/2018
ms.author: bahariri
ms.openlocfilehash: 0693fc2fff5735fb2b3c0a9b8f1d3d256746f40d
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35298330"
---
# <a name="using-kafka-mirrormaker-with-event-hubs-for-kafka-ecosystems"></a>Kafka MirrorMaker Kafka ekosistemlerini için Event Hubs ile kullanma

> [!NOTE]
> Bu örnek [GitHub](https://github.com/Azure/azure-event-hubs)'da sağlanır

Bir ana için modern bulut ölçeği uygulamaları güncelleştirmek için geliştirmek ve hizmet kesintiye uğratmadan Altyapı değiştirme olanağı konusudur. Bu öğretici bir Kafka etkinleştirilmiş olay hub'ı nasıl gösterir ve Kafka MirrorMaker mevcut bir Kafka ardışık düzeni Azure'da "Kafka Giriş akışı Event Hubs hizmetinde yansıtma tarafından" tümleştirebilirsiniz. 

Bir Azure olay hub'ları Kafka uç nokta Kafka protokolünü (yani Kafka istemciler) kullanarak Azure Event Hubs için bağlanmanıza olanak sağlar. Kafka uygulamaya küçük değişiklikler yaparak Azure Event Hubs'a bağlanın ve Azure ekosistemi avantajlarından. Kafka etkin olay hub'ları şu anda 1.0 ve sonraki Kafka sürümlerini destekler.

Bu örnek, Kafka Aracısı Kafka MirrorMaker kullanarak Kafka etkin event hub'ındaki yansıtma gösterilmektedir.

   ![Event Hubs ile Kafka MirrorMaker](./media/event-hubs-kafka-mirror-maker-tutorial/evnent-hubs-mirror-maker1.png)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için olduğundan emin olun:

* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
    * Ubuntu’da JDK’yi yüklemek için `apt-get install default-jdk` komutunu çalıştırın.
    * JAVA_HOME ortam değişkenini JDK’nin yüklü olduğu klasöre işaret edecek şekilde ayarladığınızdan emin olun.
* [Karşıdan](http://maven.apache.org/download.cgi) ve [yükleme](http://maven.apache.org/install.html) Maven ikili arşiv
    * Ubuntu’da Maven’i yüklemek için `apt-get install maven` komutunu çalıştırabilirsiniz.
* [Git](https://www.git-scm.com/downloads)
    * Ubuntu’da Git’i yüklemek için `sudo apt-get install git` komutunu çalıştırabilirsiniz.

## <a name="create-an-event-hubs-namespace"></a>Bir olay hub'ları ad alanı oluşturma

Bir olay hub'ları ad alanı, tüm Event Hubs hizmetinden alıp göndermek için gereklidir. Bkz: [bir Kafka oluşturma etkin olay hub'ı](event-hubs-create.md) bir olay hub'ları Kafka uç nokta alma hakkında yönergeler için. Daha sonra kullanmak için olay hub'ları bağlantı dizesini kopyaladığınızdan emin olun.

## <a name="clone-the-example-project"></a>Örnek Proje kopyalama

Bir Kafka, sahip olduğunuza göre olay hub'ları bağlantı dizesi etkin, Azure Event Hubs depoyu kopyalayın ve gidin `mirror-maker` alt:

```shell
git clone https://github.com/Azure/azure-event-hubs.git
cd azure-event-hubs/samples/kafka/mirror-maker
```

## <a name="set-up-a-kafka-cluster"></a>Kafka küme ayarlama

Kullanmak [Kafka Hızlı Başlangıç Kılavuzu](https://kafka.apache.org/quickstart) istenen ayarları ile bir kümede ayarlayın (veya var olan bir Kafka kümesi kullanmak için).

## <a name="kafka-mirrormaker"></a>Kafka MirrorMaker

Kafka MirrorMaker "yansıtma" akışının sağlar. Verilen kaynak ve hedef Kafka kümeleri, kaynak kümeye gönderilen iletiler kaynak ve hedef kümeler tarafından alınan MirrorMaker sağlar. Bu örnek, bir kaynak bir hedef Kafka etkinleştirilmiş olay hub'ı Kafka kümeyle yansıtma gösterilmektedir. Bu senaryo, olay hub'ları için var olan bir Kafka ardışık düzen tarafından veri akışını kesintiye uğratmadan veri göndermek için kullanılabilir. 

Kafka MirrorMaker hakkında daha ayrıntılı bilgi için bkz: [Kafka yansıtma/MirrorMaker Kılavuzu](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330).

### <a name="configuration"></a>Yapılandırma

Kafka MirrorMaker yapılandırmak için bu Kafka küme tüketici/kaynağı olarak ve bir Kafka etkinleştirilmiş olay hub'ı üretici/hedefine olarak verin.

#### <a name="consumer-configuration"></a>Tüketici yapılandırma

Tüketici yapılandırma dosyasını güncelleştirme `source-kafka.config`, Kafka küme kaynağının özelliklerini MirrorMaker söyler.

##### <a name="source-kafkaconfig"></a>Kaynak kafka.config

```xml
bootstrap.servers={SOURCE.KAFKA.IP.ADDRESS1}:{SOURCE.KAFKA.PORT1},{SOURCE.KAFKA.IP.ADDRESS2}:{SOURCE.KAFKA.PORT2},etc
group.id=example-mirrormaker-group
exclude.internal.topics=true
client.id=mirror_maker_consumer
```

#### <a name="producer-configuration"></a>Üretici yapılandırma

Şimdi üretici yapılandırma dosyasını güncelleştirme `mirror-eventhub.config`, yinelenen (veya "yansıtılmış") verileri Event Hubs hizmetine göndermek için MirrorMaker söyler. Özellikle, değiştirme `bootstrap.servers` ve `sasl.jaas.config` olay hub'ları Kafka uç noktaya işaret edecek şekilde. Event Hubs hizmeti aşağıdaki yapılandırmada son üç özelliklerini ayarlayarak elde güvenli (SASL) iletişim gerektirir: 

##### <a name="mirror-eventhubconfig"></a>Yansıtma eventhub.config

```xml
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
client.id=mirror_maker_producer

#Required for Event Hubs
sasl.mechanism=PLAIN
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

### <a name="run-mirrormaker"></a>MirrorMaker Çalıştır

Kök yeni güncelleştirilmiş yapılandırma dosyalarını kullanarak Kafka dizin Kafka MirrorMaker komut dosyasını çalıştırın. Yapılandırma dosyaları Kafka dizin kök dizinine kopyalayın veya yollarına aşağıdaki komutta güncelleştirme emin olun.

```shell
bin/kafka-mirror-maker.sh --consumer.config source-kafka.config --num.streams 1 --producer.config mirror-eventhub.config --whitelist=".*"
```

Giriş İstatistikleri olayları Kafka etkin olay hub'ı ulaşıyor doğrulamak için bkz [Azure portal](https://azure.microsoft.com/features/azure-portal/), veya bir tüketici karşı olay hub'ı çalıştırın.

Çalışan MirrorMaker ile Kafka küme kaynağına gönderilen olaylar hem Kafka küme tarafından alınır ve yansıtılmış Kafka olay hub'ı hizmeti etkin. MirrorMaker ve olay hub'ları Kafka uç kullanarak, mevcut bir Kafka ardışık düzeni yönetilen Azure Event Hubs hizmetine var olan küme değiştirme veya devam eden veri akışı engellemeden geçirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Event Hubs hakkında bilgi edinin](event-hubs-what-is-event-hubs.md)
* [Kafka ekosistemi için olay hub'ları hakkında bilgi edinin](event-hubs-for-kafka-ecosystem-overview.md)
* Daha fazla bilgi edinmek [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) Kafka akış olayları için şirket içi Kafka olay hub'ları bulut üzerinde etkin.
* Kafka akış öğrenin etkin olay hub'ları kullanarak [yerel Kafka uygulamaları](event-hubs-quickstart-kafka-enabled-event-hubs.md), [Apache Flink](event-hubs-kafka-flink-tutorial.md), veya [Akka akışları](event-hubs-kafka-akka-streams-tutorial.md).
