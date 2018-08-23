---
title: Apache Kafka MirrorMaker Azure Event Hubs ile Kafka ekosistemi için kullanın. | Microsoft Docs
description: Event Hubs, Kafka kümesi yansıtmak üzere Kafka MirrorMaker'ı kullanın.
services: event-hubs
documentationcenter: .net
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.topic: mirror-maker
ms.custom: mvc
ms.date: 05/07/2018
ms.author: bahariri
ms.openlocfilehash: d9ac8137e1e86edcdfe824ae29c1a8d46126900c
ms.sourcegitcommit: 974c478174f14f8e4361a1af6656e9362a30f515
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "42059765"
---
# <a name="use-kafka-mirrormaker-with-event-hubs-for-apache-kafka"></a>Kafka MirrorMaker Event Hubs ile Apache Kafka için kullanın.

Bu öğreticide, bir Kafka Aracısı Kafka MirrorMaker kullanarak bir Kafka etkin olay hub'ındaki yansıtmak üzere gösterilmektedir.

   ![Event Hubs ile Kafka MirrorMaker](./media/event-hubs-kafka-mirror-maker-tutorial/evnent-hubs-mirror-maker1.png)

> [!NOTE]
> Bu örnek [GitHub](https://github.com/Azure/azure-event-hubs)'da sağlanır


Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Event Hubs ad alanı oluşturma
> * Örnek projesini kopyalama
> * Bir Kafka kümesi ayarlama
> * Kafka MirrorMaker'ı yapılandırma
> * Kafka MirrorMaker çalıştırın

## <a name="introduction"></a>Giriş
Bir ana modern bulut ölçeğinde uygulamalar için güncelleştirme, geliştirmek ve hizmeti kesintiye uğratmadan Altyapı değiştirme olanağı noktadır. Bu öğreticide, bir Kafka özellikli bir olay hub'ı nasıl gösterilir ve Kafka MirrorMaker mevcut bir Kafka işlem hattını Azure'a "Event Hubs hizmeti Kafka giriş akışında yansıtarak" tümleştirilebilir. 

Bir Azure olay hub'ları Kafka uç nokta Kafka protokolünü (diğer bir deyişle, Kafka istemciler) kullanarak Azure Event Hubs için bağlamanıza olanak sağlar. Kafka uygulamaya küçük değişiklikler yaparak, Azure Event Hubs'a bağlanma ve Azure ekosistemi avantajlarından yararlanın. Kafka etkin Event Hubs şu anda 1.0 ve üzeri Kafka sürümleri destekler.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için şunlar sahip olduğunuzdan emin olun:

* Okumak [Apache Kafka için Event Hubs](event-hubs-for-kafka-ecosystem-overview.md) makalesi. 
* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
    * Ubuntu’da JDK’yi yüklemek için `apt-get install default-jdk` komutunu çalıştırın.
    * JAVA_HOME ortam değişkenini JDK’nin yüklü olduğu klasöre işaret edecek şekilde ayarladığınızdan emin olun.
* [İndirme](http://maven.apache.org/download.cgi) ve [yükleme](http://maven.apache.org/install.html) Maven ikili Arşivi
    * Ubuntu’da Maven’i yüklemek için `apt-get install maven` komutunu çalıştırabilirsiniz.
* [Git](https://www.git-scm.com/downloads)
    * Ubuntu’da Git’i yüklemek için `sudo apt-get install git` komutunu çalıştırabilirsiniz.

## <a name="create-an-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma

Bir Event Hubs ad alanı, tüm Event Hubs hizmetinden gönderip için gereklidir. Bkz: [olay hub'ı etkin bir Kafka oluşturma](event-hubs-create.md) bir olay hub'ları Kafka uç nokta alma hakkında yönergeler için. Daha sonra kullanmak için Event Hubs bağlantı dizesi kopyaladığınızdan emin olun.

## <a name="clone-the-example-project"></a>Örnek projesini kopyalama

Artık sahip olduğunuza göre Event Hubs bağlantı dizesi bir Kafka etkin, Azure Event Hubs depoyu kopyalamak ve gidin `mirror-maker` alt klasörü:

```shell
git clone https://github.com/Azure/azure-event-hubs.git
cd azure-event-hubs/samples/kafka/mirror-maker
```

## <a name="set-up-a-kafka-cluster"></a>Bir Kafka kümesi ayarlama

Kullanma [Kafka Hızlı Başlangıç Kılavuzu](https://kafka.apache.org/quickstart) istediğiniz ayarlarla kümesi ayarlama (veya var olan bir Kafka kümesi kullanmak için).

## <a name="configure-kafka-mirrormaker"></a>Kafka MirrorMaker'ı yapılandırma

Kafka MirrorMaker "yansıtma" akışının sağlar. Kaynak ve hedef verilen Kafka kümeleri, MirrorMaker kaynak kümeye gönderilen tüm iletileri kaynak ve hedef kümeler tarafından alınan sağlar. Bu örnek, bir kaynak bir hedef Kafka özellikli bir olay hub'ı ile Kafka kümesi yansıtmak gösterilmektedir. Bu senaryo, olay hub'ları için mevcut bir Kafka hattından veri akışını kesintiye uğratmadan veri göndermek için kullanılabilir. 

Kafka MirrorMaker hakkında daha ayrıntılı bilgi için bkz: [Kafka yansıtma/MirrorMaker Kılavuzu](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330).

Kafka MirrorMaker yapılandırmak için bu tüketici/kaynağı olarak bir Kafka kümesi ve bir Kafka özellikli olay hub'ı üretici/hedefine olarak verin.

#### <a name="consumer-configuration"></a>Tüketici yapılandırma

Tüketici yapılandırma dosyasını güncelleştirmeniz `source-kafka.config`, Kafka kümesi kaynağının özelliklerini MirrorMaker söyler.

##### <a name="source-kafkaconfig"></a>Kaynak kafka.config

```xml
bootstrap.servers={SOURCE.KAFKA.IP.ADDRESS1}:{SOURCE.KAFKA.PORT1},{SOURCE.KAFKA.IP.ADDRESS2}:{SOURCE.KAFKA.PORT2},etc
group.id=example-mirrormaker-group
exclude.internal.topics=true
client.id=mirror_maker_consumer
```

#### <a name="producer-configuration"></a>Üretici yapılandırma

Şimdi üretici yapılandırma dosyasını Güncelleştir `mirror-eventhub.config`, Event Hubs hizmeti için yinelenen (veya "yansıtılmış") veri göndermek için MirrorMaker söyler. Özellikle, değişiklik `bootstrap.servers` ve `sasl.jaas.config` olay hub'ları Kafka uç noktanıza işaret edecek şekilde. Event Hubs hizmeti aşağıdaki yapılandırmada son üç özelliklerini ayarlayarak elde güvenli (SASL) iletişim gerektirir: 

##### <a name="mirror-eventhubconfig"></a>Yansıtma eventhub.config

```xml
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
client.id=mirror_maker_producer

#Required for Event Hubs
sasl.mechanism=PLAIN
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

## <a name="run-kafka-mirrormaker"></a>Kafka MirrorMaker çalıştırın

Yeni güncelleştirilmiş yapılandırma dosyalarını kullanarak Kafka dizin kökünden Kafka MirrorMaker betiği çalıştırın. Yapılandırma dosyaları Kafka dizin kök dizinine kopyalayın veya aşağıdaki komutta kendi yollarınızı güncelleştirmeniz emin olun.

```shell
bin/kafka-mirror-maker.sh --consumer.config source-kafka.config --num.streams 1 --producer.config mirror-eventhub.config --whitelist=".*"
```

Giriş İstatistikleri olayları Kafka özellikli bir olay hub'ı ulaşıyor doğrulamak için bkz [Azure portalında](https://azure.microsoft.com/features/azure-portal/), veya bir tüketici karşı olay hub'ı çalıştırın.

MirrorMaker, çalışan ile Kafka kümesi kaynağına gönderilen tüm olayları hem Kafka kümesi alınır ve olay hub'ı hizmet yansıtılmış Kafka etkin. MirrorMaker ve olay hub'ları Kafka uç nokta kullanarak, mevcut bir Kafka işlem hattını yönetilen Azure Event Hubs hizmeti için mevcut kümeyi değiştirme veya tüm devam eden veri akışı engellemeden geçirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Event Hubs ad alanı oluşturma
> * Örnek projesini kopyalama
> * Bir Kafka kümesi ayarlama
> * Kafka MirrorMaker'ı yapılandırma
> * Kafka MirrorMaker çalıştırın

Apache Kafka için Event Hubs hakkında daha fazla bilgi edinmek için sonraki makaleye geçin:

> [!div class="nextstepaction"]
> [Apache Flink Azure Event Hubs ile Kafka için kullanın.](event-hubs-kafka-flink-tutorial.md)