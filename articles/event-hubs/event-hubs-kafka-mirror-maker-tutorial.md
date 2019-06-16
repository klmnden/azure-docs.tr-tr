---
title: Apache Kafka MirrorMaker - Azure Event hubs'ı kullanma | Microsoft Docs
description: Bu makalede, bir Kafka kümesi AzureEvent hub'larındaki yansıtmak üzere Kafka MirrorMaker'ı kullanma hakkında bilgi sağlar.
services: event-hubs
documentationcenter: .net
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.topic: conceptual
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: bahariri
ms.openlocfilehash: a7271eb6b8cbc8a117b5a8e75edfe02985ec3452
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60821522"
---
# <a name="use-kafka-mirrormaker-with-event-hubs-for-apache-kafka"></a>Kafka MirrorMaker Event Hubs ile Apache Kafka için kullanın.

Bu öğreticide, bir Kafka Aracısı Kafka MirrorMaker kullanarak bir Kafka etkin olay hub'ındaki yansıtmak üzere gösterilmektedir.

   ![Event Hubs ile Kafka MirrorMaker](./media/event-hubs-kafka-mirror-maker-tutorial/evnent-hubs-mirror-maker1.png)

> [!NOTE]
> Bu örnek [GitHub](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/mirror-maker)'da sağlanır


Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Event Hubs ad alanı oluşturma
> * Örnek projeyi kopyalama
> * Bir Kafka kümesi ayarlama
> * Kafka MirrorMaker'ı yapılandırma
> * Kafka MirrorMaker çalıştırın

## <a name="introduction"></a>Giriş
Bir ana modern bulut ölçeğinde uygulamalar için güncelleştirme, geliştirmek ve hizmeti kesintiye uğratmadan Altyapı değiştirme olanağı noktadır. Bu öğreticide, bir Kafka özellikli bir olay hub'ı nasıl gösterilir ve Kafka MirrorMaker mevcut bir Kafka işlem hattını Azure'a "Event Hubs hizmeti Kafka giriş akışında yansıtarak" tümleştirilebilir. 

Bir Azure olay hub'ları Kafka uç nokta Kafka protokolünü (diğer bir deyişle, Kafka istemciler) kullanarak Azure Event Hubs için bağlamanıza olanak sağlar. Kafka uygulamaya küçük değişiklikler yaparak, Azure Event Hubs'a bağlanma ve Azure ekosistemi avantajlarından yararlanın. Kafka etkin Event Hubs şu anda 1.0 ve üzeri Kafka sürümleri destekler.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için şunlar sahip olduğunuzdan emin olun:

* [Apache Kafka için Event Hubs](event-hubs-for-kafka-ecosystem-overview.md) makalesini okuyun. 
* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
* [Java Development Kit (JDK) 1.7+](https://aka.ms/azure-jdks)
    * Ubuntu’da JDK’yi yüklemek için `apt-get install default-jdk` komutunu çalıştırın.
    * JAVA_HOME ortam değişkenini JDK’nin yüklü olduğu klasöre işaret edecek şekilde ayarladığınızdan emin olun.
* [İndirme](https://maven.apache.org/download.cgi) ve [yükleme](https://maven.apache.org/install.html) Maven ikili Arşivi
    * Ubuntu’da Maven’i yüklemek için `apt-get install maven` komutunu çalıştırabilirsiniz.
* [Git](https://www.git-scm.com/downloads)
    * Ubuntu’da Git’i yüklemek için `sudo apt-get install git` komutunu çalıştırabilirsiniz.

## <a name="create-an-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma

Herhangi bir Event Hubs hizmetinden göndermek ve almak için Event Hubs ad alanı gereklidir. Event Hubs Kafka uç noktası alma yönergeleri için bkz. [Kafka özellikli Event Hubs oluşturma](event-hubs-create.md). Daha sonra kullanmak için Event Hubs bağlantı dizesi kopyaladığınızdan emin olun.

## <a name="clone-the-example-project"></a>Örnek projeyi kopyalama

Artık sahip olduğunuza göre Event Hubs bağlantı dizesi bir Kafka etkin, Azure Event Hubs Kafka deposu için kopyalama ve gidin `mirror-maker` alt klasörü:

```shell
git clone https://github.com/Azure/azure-event-hubs-for-kafka.git
cd azure-event-hubs-for-kafka/tutorials/mirror-maker
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

## <a name="samples"></a>Örnekler
GitHub üzerindeki aşağıdaki örneklere bakın:

- [Github'da Bu öğretici için örnek kod](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/mirror-maker)
- [Azure Container Instance üzerinde çalışan Azure olay hub'ları Kafka MirrorMaker](https://github.com/djrosanova/EventHubsMirrorMaker)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Event Hubs ad alanı oluşturma
> * Örnek projeyi kopyalama
> * Bir Kafka kümesi ayarlama
> * Kafka MirrorMaker'ı yapılandırma
> * Kafka MirrorMaker çalıştırın

Event Hubs ve Kafka için Event Hubs hakkında daha fazla bilgi edinmek için aşağıdaki konuya bakın:  

- [Event Hubs hakkında bilgi edinin](event-hubs-what-is-event-hubs.md)
- [Apache Kafka için Event Hubs](event-hubs-for-kafka-ecosystem-overview.md)
- [Kafka özellikli Event Hubs oluşturma](event-hubs-create-kafka-enabled.md)
- [Kafka uygulamalarınızdan Event Hubs'a akış yapma](event-hubs-quickstart-kafka-enabled-event-hubs.md)
- [Apache Spark'ı Kafka özellikli bir olay hub'ına bağlama](event-hubs-kafka-spark-tutorial.md)
- [Apache Flink'i Kafka özellikli bir olay hub'ına bağlama](event-hubs-kafka-flink-tutorial.md)
- [Kafka Connect'i Kafka özellikli olay hub'ıyla tümleştirme](event-hubs-kafka-connect-tutorial.md)
- [Akka Streams’i Kafka özellikli olay hub'ına bağlama](event-hubs-kafka-akka-streams-tutorial.md)
- [GitHub'ımızdaki örnekleri inceleme](https://github.com/Azure/azure-event-hubs-for-kafka)
