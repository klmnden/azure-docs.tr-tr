---
title: Kullanım olay hub'ından Apache Kafka uygulama - Azure Event Hubs | Microsoft Docs
description: Bu makalede, Azure Event Hubs ile Apache Kafka desteği hakkında bilgi sağlar.
services: event-hubs
documentationcenter: .net
author: shvija
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: bahariri
ms.openlocfilehash: 8bf381e7c66e06bbaa140ed865f0f7c9b4f001af
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60821708"
---
# <a name="use-azure-event-hubs-from-apache-kafka-applications"></a>Apache Kafka uygulamalarından Azure Event Hubs'ı kullanın
Event Hubs, kullanılabilir bir Kafka uç noktası sağlar, varolan Kafka kendi Kafka kümesi çalıştıran alternatif olarak tabanlı uygulamalar. Event hubs'ı destekleyen [Apache Kafka protokolü 1.0 ve üzeri](https://kafka.apache.org/documentation/)ve MirrorMaker dahil olmak üzere, mevcut Kafka uygulamaları ile çalışır.  

## <a name="what-does-event-hubs-for-kafka-provide"></a>Event Hubs, Kafka için neler sağlar?

Olay hub'ları Kafka özelliği için bir protokol head ile Kafka sürümleri 1.0 ve daha sonra hem okuma hem de Kafka konularını yazmak için ikili uyumlu olan Azure Event Hubs üzerinde sağlar. Kafka uç noktasından uygulamalarınızı kod değişikliği ancak en az bir yapılandırma değişikliği kullanarak başlayabilir. Kafka kümenize işaret eden yerine olay hub'ınız tarafından kullanıma sunulan Kafka uç noktasını işaret edecek şekilde yapılandırmaları bağlantı dizesinde güncelle Ardından, Event Hubs'a Kafka protokolünü kullanan uygulamalarınızı olaylardan akış başlatabilirsiniz. Bu tümleştirme gibi çerçeveleri de destekler. [Kafka bağlanma](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/connect), hangi şu anda Önizleme aşamasındadır. 

Kafka ile Event Hubs kavramsal olarak neredeyse aynı: her iki bölümlenmiş günlük verilerinin akışı için yerleşik oldukları. Aşağıdaki tablo, Event Hubs ile Kafka arasındaki kavramları eşler.

### <a name="kafka-and-event-hub-conceptual-mapping"></a>Kafka ve olay hub'ı kavramsal eşleme

| Kafka kavramı | Olay hub'ları kavramını|
| --- | --- |
| Küme | Ad Alanı |
| Konu | Olay Hub'ı |
| Bölüm | Bölüm|
| Tüketici Grubu | Tüketici Grubu |
| Uzaklık | Uzaklık|

### <a name="key-differences-between-kafka-and-event-hubs"></a>Event Hubs ile Kafka arasındaki temel farklılıklar

Sırada [Apache Kafka](https://kafka.apache.org/) seçtiğiniz her yerde, çalıştırabileceğiniz, yazılım, Event Hubs, Azure Blob depolama alanına benzer bir bulut hizmetidir. Sunucu yok veya yönetmek için ağ ve hiçbir aracıları yapılandırmak için vardır. İçinde konuları canlı bir FQDN olduğundan, bir ad alanı oluşturur ve ardından olay hub'ları veya bu ad alanı içindeki konuları oluşturun. Event Hubs ve ad alanları hakkında daha fazla bilgi için bkz: [Event Hubs özellikleri](event-hubs-features.md#namespace). İstemciler, aracıları veya kümesindeki makineleri bilmeniz gerekmez. Bu nedenle bir bulut hizmeti olarak tek bir kararlı sanal IP adresi Event Hubs uç noktası olarak kullanır. 

Event Hubs ölçek tarafından kaç tane işleme birimi, saniyede 1 MB ile entitling her bir üretilen iş birimi veya giriş saniyede 1000 olay ile satın aldığınız denetlenir. Sınırlarınızı ile ulaştığında varsayılan olarak, Event Hubs işleme birimleri ölçeklendirir. [otomatik Şişme](event-hubs-auto-inflate.md) özelliği; bu da Event Hubs ile works Kafka özellik için özellik. 

### <a name="security-and-authentication"></a>Güvenlik ve kimlik doğrulaması

Azure Event Hubs, tüm iletişimler için SSL veya TLS gerektirir ve kimlik doğrulaması için paylaşılan erişim imzaları (SAS) kullanır. Bu gereksinim, Event Hubs içinde bir Kafka uç nokta için de geçerlidir. Kafka ile uyumluluk için Event Hubs SASL DÜZ kimlik doğrulaması ve SASL SSL Aktarım güvenliği için kullanır. Event Hubs'nda güvenlik hakkında daha fazla bilgi için bkz. [Event Hubs kimlik doğrulama ve güvenlik](event-hubs-authentication-and-security-model-overview.md).

## <a name="other-event-hubs-features-available-for-kafka"></a>Kafka için diğer Event Hubs özellikleri

Kafka özelliği için Event Hubs ile bir protokol okunup yazılacağını başka ile geçerli Kafka üreticiler, Kafka aracılığıyla yayımlama devam edebilirsiniz ve Event Hubs, Azure Stream Analytics veya Azure işlevleri gibi okuyucularıyla ekleyebilirsiniz. böylece sağlar. Ayrıca, Event Hubs özellikleri gibi [yakalama](event-hubs-capture-overview.md) ve [coğrafi olağanüstü durum kurtarma](event-hubs-geo-dr.md) Kafka özelliği için Event Hubs ile de çalışır.

## <a name="features-that-are-not-yet-supported"></a>Henüz desteklenmeyen özellikler 

Henüz desteklenmeyen Kafka özelliklerin listesi aşağıda verilmiştir:

*   Idempotent üretici
*   İşlem
*   Sıkıştırma
*   Boyutu bağlı bekletme
*   Günlük sıkıştırma
*   Varolan bir konuya bölümleri ekleme
*   HTTP Kafka API'si desteği
*   Kafka akışlar

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Kafka için Event hubs'a giriş sağlandı. Daha fazla bilgi için aşağıdaki bağlantılara bakın:

- [Kafka özellikli Event Hubs oluşturma](event-hubs-create-kafka-enabled.md)
- [Kafka uygulamalarınızdan Event Hubs'a akış yapma](event-hubs-quickstart-kafka-enabled-event-hubs.md)
- [Kafka özellikli bir olay hub'ında Kafka aracısını yansıtma](event-hubs-kafka-mirror-maker-tutorial.md)
- [Apache Spark'ı Kafka özellikli bir olay hub'ına bağlama](event-hubs-kafka-spark-tutorial.md)
- [Apache Flink'i Kafka özellikli bir olay hub'ına bağlama](event-hubs-kafka-flink-tutorial.md)
- [Kafka Connect'i Kafka özellikli olay hub'ıyla tümleştirme](event-hubs-kafka-connect-tutorial.md)
- [Akka Streams’i Kafka özellikli olay hub'ına bağlama](event-hubs-kafka-akka-streams-tutorial.md)
- [GitHub'ımızdaki örnekleri inceleme](https://github.com/Azure/azure-event-hubs-for-kafka)


