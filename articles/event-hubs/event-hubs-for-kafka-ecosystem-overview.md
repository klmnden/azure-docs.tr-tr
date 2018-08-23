---
title: Azure Event Hubs için Apache Kafka | Microsoft Docs
description: Azure Event Hubs'a genel bakış ve Kafka'ya giriş etkin
services: event-hubs
documentationcenter: .net
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.date: 08/16/2018
ms.author: bahariri
ms.openlocfilehash: 16c101068be48ba1435ef230b29c679fcef17d08
ms.sourcegitcommit: 974c478174f14f8e4361a1af6656e9362a30f515
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "42057154"
---
# <a name="azure-event-hubs-for-apache-kafka-preview"></a>Azure Event Hubs için Apache Kafka (Önizleme)

Event Hubs, kullanılabilir bir Kafka uç noktası sağlar, varolan Kafka kendi Kafka kümesi çalıştıran alternatif olarak tabanlı uygulamalar. Event hubs'ı destekleyen [Apache Kafka 1.0](https://kafka.apache.org/10/documentation.html) ve daha yeni istemci sürümleri ve mevcut Kafka uygulamalarınızı çalışır MirrorMaker dahil olmak üzere. 

## <a name="what-does-event-hubs-for-kafka-provide"></a>Event Hubs, Kafka için neler sağlar?

Olay hub'ları Kafka özelliği için bir protokol head ile Kafka sürümleri 1.0 ve daha sonra hem okuma hem de Kafka konularını yazmak için ikili uyumlu olan Azure Event Hubs üzerinde sağlar. Kafka uç noktasından uygulamalarınızı kod değişikliği ancak en az bir yapılandırma değişikliği kullanarak başlayabilir. Kafka kümenize işaret eden yerine olay hub'ınız tarafından kullanıma sunulan Kafka uç noktasını işaret edecek şekilde yapılandırmaları bağlantı dizesinde güncelle Ardından, Event Hubs'a Kafka protokolünü kullanan uygulamalarınızı olaylardan akış başlatabilirsiniz. 

Kafka ile Event Hubs kavramsal olarak neredeyse aynı: her iki bölümlenmiş günlük verilerinin akışı için yerleşik oldukları. Aşağıdaki tablo, Event Hubs ile Kafka arasındaki kavramları eşler.

### <a name="kafka-and-event-hub-conceptual-mapping"></a>Kafka ve olay hub'ı kavramsal eşleme

| Kafka kavramı | Olay hub'ları kavramını|
| --- | --- |
| Küme | Ad Alanı |
| Konu | Event Hubs |
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

## <a name="features-that-are-not-supported-in-the-preview"></a>Önizleme sürümünde desteklenmeyen özellikler

Kafka tümleştirme için Event Hubs genel Önizleme için Kafka aşağıdaki özellikleri desteklenmez:

*   Idempotent üretici
*   İşlem
*   Sıkıştırma
*   Boyutu bağlı bekletme
*   Günlük sıkıştırma
*   Varolan bir konuya bölümleri ekleme
*   HTTP Kafka API'si desteği
*   Kafka bağlanma
*   Kafka akışlar

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Kafka için Event hubs'a giriş sağlandı. Daha fazla bilgi için aşağıdaki bağlantılara bakın:

* [Event Hubs Kafka oluşturma etkin](event-hubs-create-kafka-enabled.md)
* [Kafka uygulamalarınızdan Event hubs'ta Stream](event-hubs-quickstart-kafka-enabled-event-hubs.md)
* [Event Hubs öğreticisi](event-hubs-dotnet-standard-getstarted-send.md) ile çalışmaya başlayın
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)

 
 

