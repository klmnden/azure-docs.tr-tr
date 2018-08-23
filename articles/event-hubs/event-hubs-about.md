---
title: Azure Event Hubs nedir? | Microsoft Docs
description: Saniyede milyonlarca olayın eklenmesini sağlayan bir Büyük Veri akış hizmeti olan Azure Event Hubs hakkında daha fazla bilgi edinin.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: overview
ms.custom: mvc
ms.date: 08/01/2018
ms.author: shvija
ms.openlocfilehash: 8437b1c10facc28c5fd71b70dd7acf01b7d39e8e
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "42023851"
---
# <a name="what-is-azure-event-hubs"></a>Azure Event Hubs nedir?

Azure Event Hubs saniyede milyonlarca olay alıp işleme kapasitesine sahip olan bir Büyük Veri akış platformu ve olay alma hizmetidir. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. 

Event Hubs, aşağıdaki yaygın senaryoların bazılarında kullanılır:

- Anomali algılama (sahte/aykırı değer)
- Uygulama günlüğüne kaydetme
- Tıklama dizileri gibi analiz işlem hatları
- Canlı pano oluşturma
- Veri arşivleme
- İşlem gerçekleştirme
- Kullanıcı telemetrisi işleme
- Cihaz telemetrisi akışı 

## <a name="why-use-event-hubs"></a>Event Hubs’ı neden kullanmalıyım?

Veriler ancak kolay bir şekilde işlenebildiğinde ve veri kaynaklarından tam zamanında içgörüler alındığında değerlidir. Event Hubs; düşük gecikme süresine sahip dağıtılmış bir akış işleme platformu ile tam kapsamlı bir Büyük Veri işlem hattı oluşturmak için Azure içindeki ve dışındaki veriler ve analiz hizmetleriyle sorunsuz bir tümleştirme sunar.

Event Hubs bir olay işlem hattı için "ön kapı" görevi yapar ve çözüm mimarilerinde genelde *olay alıcı* olarak adlandırılır. Olay yutucu, bir olay akışının üretimini ilgili olayların kullanılmasına ayıran, olay yayımcıları ile olay tüketicileri arasında duran bir bileşen veya hizmettir. Event Hubs tutma süresi arabelleğine sahip birleştirilmiş akış platformu sunarak olay oluşturucuları olay tüketicilerinden ayırır. 

Aşağıdaki bölümlerde Azure Event Hubs hizmetinin temel özellikleri anlatılmaktadır: 

## <a name="fully-managed-paas"></a>Tam olarak yönetilen PaaS 

Event Hubs, çok az yapılandırmaya veya yönetim yüküne sahip olan yönetilen bir hizmet olduğundan işletme çözümlerinize odaklanmanızı sağlar. [Apache Kafka ekosistemleri için Event Hubs](event-hubs-for-kafka-ecosystem-overview.md), kümelerinizi yönetmeye, yapılandırmaya veya çalıştırmaya gerek kalmadan PaaS Kafka deneyimi sunar.

## <a name="support-for-real-time-and-batch-processing"></a>Gerçek zamanlı ve toplu işlem desteği

Eyleme dönüştürülebilir içgörüler elde etmek için akışınızı gerçek zamanlı olarak ekleyin, arabelleğe alın, depolayın ve işleyin. Event Hubs, [bölümlenmiş tüketici modeli](event-hubs-features.md#partitions) kullanarak birden fazla uygulamanın akışı aynı anda işlemesini ve bu sayede işleme hızını denetlemenizi sağlar.

Verilerinizi neredeyse gerçek zamanlı olarak [yakalayıp](event-hubs-capture-overview.md) uzun süreli saklama veya mikro toplu işlem için bir [Azure Blob depolama alanında](https://azure.microsoft.com/services/storage/blobs/) veya [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) içinde depolayın. Bunu gerçek zamanlı analiz elde etmek için kullandığınız akış üzerinde gerçekleştirebilirsiniz. Capture özelliği hızlıdır, çalıştırmak için yönetim maliyeti yoktur ve Event Hubs  [aktarım hızı birimleriyle](event-hubs-features.md#throughput-units) otomatik olarak ölçeklendirilir. Event Hubs Capture, verileri yakalamak yerine işlemeye odaklanmanızı sağlar.

Azure Event Hubs, sunucusuz mimari için [Azure İşlevleri](/azure/azure-functions/) ile de tümleştirilebilir.

## <a name="scalable"></a>Ölçeklenebilir 

Event Hubs ile megabayt boyutunda veri akışlarıyla başlayıp gigabayt veya terabayt boyutlarına ulaşabilirsiniz. [Otomatik şişme](event-hubs-auto-inflate.md) özelliği, aktarım hızı birimlerini kullanım ihtiyaçlarınıza göre ölçeklendirmek için kullanabileceğiniz birçok özellikten bir tanesidir. 

## <a name="rich-ecosystem"></a>Zengin ekosistem

[Apache Kafka ekosistemleri için Event Hubs](event-hubs-for-kafka-ecosystem-overview.md), [Apache Kafka (1.0 ve üzeri)](https://kafka.apache.org/) istemcilerinin ve uygulamalarının küme yönetmeye gerek kalmadan Event Hubs ile iletişim kurmasını sağlar.
 
Çeşitli [dillerde (.NET, Java, Python, Go, Node.js)](https://github.com/Azure/azure-event-hubs) kullanılabilen geniş ekosistem sayesinde akışlarınızı Event Hubs'dan işlemeye kolayca başlayabilirsiniz. Desteklenen tüm istemci dilleri, düşük düzeyde tümleştirme sağlar.

## <a name="key-architecture-components"></a>Temel mimari bileşenler

Event Hubs, ileti akışı işleme olanağı sağlar ancak geleneksel kurumsal mesajlaşmadan farklı özelliklere sahiptir. Event Hubs özellikleri, yüksek işleme ve olay işleme senaryoları üzerine inşa edilmiştir. Event Hubs şu [temel bileşenleri](event-hubs-features.md) içerir:

- **Olay oluşturucuları**: Olay hub'ına veri gönderen tüm varlıklardır. Olay yayımcıları HTTPS, AMQP 1.0 veya Apache Kafka (1.0 ve üzeri) kullanarak olayları yayımlayabilir
- **Bölümler**: Her tüketici ileti akışının yalnızca belirli bir alt kümesini veya bölümünü okur.
- **Tüketici grupları**: Tüm olay hub'ının bir görünümüdür (durum, konum veya uzaklık). Tüketici grupları birden çok tüketen uygulamayı her biri olay akışının ayrı bir görünümüne sahip olacak ve akışı kendi hızlarında ve kendi sapmalarıyla bağımsız bir şekilde okuyacak şekilde etkinleştirir.
- **Aktarım hızı birimleri**:Event Hubs'ın aktarım hızı kapasitesini denetleyen önceden satın alınan kapasite birimleridir.
- **Olay alıcıları**: Bir olay hub'ından olay verilerini okuyan tüm varlıklardır. Tüm Event Hubs tüketicileri AMQP 1.0 oturumu üzerinden bağlanır ve olaylar, kullanılabilir olduğu anda oturum üzerinden iletilir.

Aşağıdaki şekilde Event Hubs akış işleme mimarisi gösterilmektedir:

![Event Hubs](./media/event-hubs-about/event_hubs_architecture.png)


## <a name="next-steps"></a>Sonraki adımlar

Event Hubs'ı kullanmaya başlamak için aşağıdaki makalelere bakın:

* [Event Hubs'a alma](event-hubs-quickstart-portal.md)
* [Event Hubs özelliklerine genel bakış](event-hubs-features.md)
* [Sık sorulan sorular](event-hubs-faq.md)


