---
title: Azure Event Hubs Kafka ekosistemlerini için | Microsoft Docs
description: Azure Event Hubs'a genel bakış ve Kafka giriş etkin
services: event-hubs
documentationcenter: .net
author: djrosanova
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.date: 05/07/2018
ms.author: darosa
ms.openlocfilehash: 51e32737be4eaf3c61cdbe50b82a05c205800ac4
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="event-hubs-for-kafka-ecosystems"></a>Event Hubs Kafka ekosistemlerini için

Event Hubs Kafka ekosistemlerini için kullanılabilecek bir Kafka uç nokta sağlar, varolan Kafka kendi Kafka küme çalışan alternatif olarak tabanlı uygulamaları. Olay hub'ları Kafka ekosistemi desteklediği için [Apache Kafka 1.0](https://kafka.apache.org/10/documentation.html) ve daha yeni istemci sürümleri ve mevcut Kafka uygulamalarınızı çalışır MirrorMaker dahil olmak üzere. Bağlantı dizesini değiştirin ve akış Event Hubs'a Kafka protokolünü kullanan, uygulamalardan olaylarını başlatın.

## <a name="what-does-event-hubs-for-kafka-ecosystems-provide"></a>Event Hubs Kafka ekosistemlerini için neler sağlar?

Event Hubs Kafka ekosistemi için ikili 1.0 Kafka sürümleriyle ve daha sonra hem okuma hem de Kafka konuları yazmak için uyumlu Azure Event Hubs üstünde Protokolü head sağlar. Kavramsal olarak Kafka ve Event Hubs neredeyse aynıdır: veri akışı için oluşturulmuş iki bölümlenmiş günlükleri oldukları. Aşağıdaki tabloda Kafka ve Event Hubs arasında kavramları eşler.

### <a name="kafka-and-event-hub-conceptual-mapping"></a>Kafka ve olay hub'ı kavramsal eşleme

| Kafka kavramı | Olay hub'ları kavramını|
| --- | --- |
| Küme | Ad Alanı |
| Konu | Event Hubs |
| Bölüm | Bölüm|
| Tüketici Grubu | Tüketici Grubu |
| Uzaklık | Uzaklık|

### <a name="key-differences-between-kafka-and-event-hubs"></a>Kafka ve olay hub'ları arasındaki farklar

Sırada [Apache Kafka](https://kafka.apache.org/) seçtiğiniz her yerde hangi çalıştırabilirsiniz, yazılım, olay hub'ları, Azure Blob depolama alanına benzer bir bulut hizmetidir. Sunucusu yok veya yönetmek için ağ ve yapılandırmak için hiçbir aracıların vardır. İçinde konularınızın canlı bir FQDN olduğundan, bir ad alanı oluşturun ve sonra olay hub'ları veya bu ad alanı içindeki konuları oluşturun. Olay hub'ları ve ad alanları hakkında daha fazla bilgi için bkz: [Event Hubs nedir](event-hubs-what-is-event-hubs.md). İstemcileri aracıların ya da makineleri bir küme içinde hakkında bilmeniz gerek yoktur bir bulut hizmeti olarak Event Hubs tek bir tutarlı sanal IP adresi uç noktası olarak kullanır. 

Olay hub'ları ölçek kaç tane işleme birimlerine göre saniye başına 1 MB ile entitling her işleme birimi veya giriş saniye başına 1000 olay ile satın aldığınız denetlenir. İle sınırına ulaştığında varsayılan olarak, Event Hubs işleme birimleri ölçeklendirir. [otomatik Şişir](event-hubs-auto-inflate.md) özellik; bu da özellik Kafka ekosistemlerini için Event Hubs ile çalışır. 

### <a name="security-and-authentication"></a>Güvenlik ve kimlik doğrulaması

Azure Event Hubs tüm iletişim için SSL veya TLS gerektirir ve kimlik doğrulaması için paylaşılan erişim imzaları (SAS) kullanır. Bu gereksinim, olay hub'ları içinde bir Kafka uç noktası için de geçerlidir. Kafka ile uyumluluk için olay hub'ları SASL DÜZ kimlik doğrulama ve SASL SSL için taşıma güvenliği için kullanır. Olay hub'ları da güvenlik hakkında daha fazla bilgi için bkz: [Event Hubs kimlik doğrulaması ve güvenlik](event-hubs-authentication-and-security-model-overview.md).

## <a name="other-event-hubs-features-available-for-kafka"></a>Kafka için kullanılabilen diğer Event Hubs özellikleri

Olay hub'ları Kafka ekosistemi için bir protokolle yazmak ve başka bir işlemle, böylece geçerli Kafka üreticileri Kafka aracılığıyla yayımlama devam edebilirsiniz ve Azure Stream Analytics veya Azure işlevleri gibi Event Hubs ile okuyucular ekleyebilirsiniz okumak etkinleştirir. Ayrıca, Event Hubs özellikleri gibi [yakalama](event-hubs-capture-overview.md) ve [coğrafi olağanüstü durum kurtarma](event-hubs-geo-dr.md) Kafka ekosistemlerini için Event Hubs ile de çalışır.

## <a name="features-that-are-not-supported-in-the-preview"></a>Önizlemede desteklenmeyen özellikler

Olay hub'ları Kafka ekosistemlerini tümleştirmesi için genel önizlemesi için aşağıdaki Kafka özellikler desteklenmez:

*   Idempotent üretici
*   İşlem
*   Sıkıştırma
*   Boyutu tabanlı bekletme
*   Günlük sıkıştırma
*   Varolan bir konuyu bölümleri ekleme
*   HTTP Kafka API desteği
*   Kafka Bağlan
*   Kafka akışlar

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede ekosistemlerini Kafka için Event Hubs bir giriş sağlanır. Daha fazla bilgi için aşağıdaki bağlantılara bakın:

* [Olay hub'ları Kafka oluşturma etkin](event-hubs-create-kafka-enabled.md)
* [Olay hub'ları bir akışa, Kafka uygulamalardan](event-hubs-quickstart-kafka-enabled-event-hubs.md)
* [Event Hubs öğreticisi](event-hubs-dotnet-standard-getstarted-send.md) ile çalışmaya başlayın
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)

 
 

