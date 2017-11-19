---
title: "Azure Hizmetleri karşılaştırma Mesajlaşma"
description: "Karşılaştırır Azure olay kılavuz, olay hub'ları ve hizmet veri yolu. Farklı senaryolar için kullanmak için hangi hizmet önerir."
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 11/15/2017
ms.author: tomfitz
ms.openlocfilehash: 94771578d94b5b9bc23451049a78506e80c87d26
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="choose-between-azure-services-that-deliver-messages"></a>İletileri teslim Azure hizmetleri arasında seçim

Azure bir çözüm boyunca olay iletileri teslim etmeye yardımcı üç hizmetleri sunar. Bu hizmetler şunlardır:

* [Olay kılavuz](/azure/event-grid/)
* [Event Hubs](/azure/event-hubs/)
* [Service Bus](/azure/service-bus-messaging/)

Bazı benzerlikler sahip olsa da her hizmetin belirli senaryoları için tasarlanmıştır. Bu makalede, bu hizmetleri ve uygulamanız için seçmesi anlamanıza yardımcı arasındaki farklar açıklanmaktadır. Çoğu durumda, ileti Hizmetleri tamamlayıcı ve birlikte kullanılabilir.

## <a name="event-vs-message-services"></a>Olay iletisi Hizmetleri karşılaştırması

Olay teslim hizmetleri ve bir ileti teslim hizmetleri arasında dikkat edilecek önemli bir fark yoktur.

### <a name="event"></a>Olay

Bir olay, bir eylem veya bir durum değişikliği basit bir bildirimidir. Olay verileri ne hakkında bilgi içerir ancak olay tetiklenen veri yok. Örneğin, bir olay, bir dosyanın oluşturulduğu aboneleri bildirir. Dosya hakkında genel bilgiler içerebilir, ancak dosya içermiyor. Genellikle, olayları olay işleyicileri gerçek zamanlı olarak davranacak şekilde tetikler.

### <a name="message"></a>İleti

Bir ileti tüketilen veya başka bir yerde depolanan bir hizmet tarafından üretilen işlenmemiş verilerdir. İleti, ileti işlem hattını tetiklenen verileri içerir. Bu ileti bir e-ticaret sipariş kullanıcı telemetri için herhangi bir şey olabilir. Bir olay bildirimi bir ileti yayımcısı yanıt bekleyebilir. Örneğin, bir ileti ham verileri içeren ancak sistem bu verilerden bir dosya oluşturmak için sonraki bölümü bekliyor.

## <a name="comparison-of-services"></a>Hizmetleri karşılaştırması

| Hizmet | Amaç | Tür | Kullanılması gereken durumlar |
| ------- | ------- | ---- | ----------- |
| Event Grid | Geriye dönük programlama | Olay dağıtım | Durum değişiklikleri tepki |
| Event Hubs | Büyük veri ardışık düzen | Olay akışı | Telemetri ve Dağıtılmış veri akışı |
| Service Bus | Yüksek değerli Kurumsal Mesajlaşma | İleti | Sipariş işleme ve finansal işlemler |

### <a name="event-grid"></a>Event Grid

Olay kılavuz olay denetimli, geriye dönük programlama sağlayan bir olay devre kartı ' dir. Bir yayımlama kullanır-model abone olun. Yayımcıları olayları yayma, ancak ilgili olayları işlenir hiçbir Beklenti sahip. Aboneler işlemek için istedikleri hangi olayların karar verir.

Olay kılavuz Azure Hizmetleri ile son derece tümleşiktir ve üçüncü taraf hizmetleri ile tümleştirilebilir. Olay tüketimi basitleştirir ve sabit yoklama gereksinimini ortadan kaldırarak maliyetlerini düşürür. Olay kılavuz, Azure ve Azure olmayan kaynakları olaylarından verimli ve güvenilir bir şekilde yönlendirir. Kayıtlı abone uç noktaları olaylara dağıtır. Olay iletisi hizmetler ve uygulamalar değişikliklere tepki vermek için gereken bilgileri içerir. Olay kılavuz veri ardışık değil ve güncelleştirildi gerçek nesne dağıtmaz.

Aşağıdaki özelliklere sahiptir:

* dinamik olarak ölçeklenebilir
* Düşük maliyet
* Sunucusuz

### <a name="event-hubs"></a>Event Hubs

Azure Event Hubs büyük veri işlem hattı ' dir. Yakalama, bekletme ve yeniden yürütme telemetri ve olay akışı veri kolaylaştırır. Veriler çok sayıda eş zamanlı kaynaklardan gelebilir. Olay hub'ları akış işleme altyapılar ve Analiz Hizmetleri, çeşitli için kullanılabilir duruma getirilmek üzere telemetri ve olay verilerini sağlar. Veri akışları veya ile birlikte gelen olay toplu olarak kullanılabilir. Bu hizmet yinelenen yeniden yürütme saklı ham verilerin yanı sıra, gerçek zamanlı işleme için hızlı veri alma sağlayan tek bir çözüm sağlar. Veri akışı, bir dosyaya işleme ve analizi için yakalayabilirsiniz.

Aşağıdaki özelliklere sahiptir:

* Düşük gecikme süresi
* Alma ve saniye başına milyonlarca olayı işleme yeteneği

### <a name="service-bus"></a>Service Bus

Hizmet veri yolu geleneksel Kurumsal uygulamalara yöneliktir. Bu kurumsal uygulamalar işlemleri, sıralama, yinelenen algılama ve anlık tutarlılık gerektirir. Hizmet veri yolu bulut yerel uygulamaları iş süreçlerini güvenilir durumu geçişi Yönetimi sağlamak için etkinleştirir. Kayıp veya yinelenen yüksek değerli iletilerini işlerken Azure hizmet veri yolu kullanın. Hizmet veri yolu ayrıca karma bulut çözümleri arasında yüksek güvenlikli iletişimi kolaylaştırır ve mevcut şirket içi sistemlerde bulut çözümleri bağlanabilir.

Service Bus aracılı Mesajlaşma system ' dir. Kullanıcı taraf iletileri almaya hazır olana kadar iletileri bir "aracıda" (örneğin, bir kuyruk) depolar.

Aşağıdaki özelliklere sahiptir:

* yoklama gerektirir güvenilir zaman uyumsuz ileti teslimi (bir hizmet olarak Mesajlaşma Kurumsal)
* FIFO, toplu işleme oturumları, işlemleri, ulaşmayan posta, zamana bağlı denetim, Yönlendirme ve filtreleme ve yinelenen saptama gibi gelişmiş Mesajlaşma Özellikler

## <a name="use-the-services-together"></a>Hizmetleri birlikte kullanma

Bazı durumlarda, farklı rolleri yerine getirmek için yan yana Hizmetleri'ni kullanın. Örneğin, bir e-ticaret sitesi, site telemetri yakalamak için olay hub'ları ve olayı öğeyi sevk edilen gibi olaylara yanıt vermek kılavuz siparişi işlemek için hizmet veri yolu kullanabilirsiniz.

Diğer durumlarda, birlikte bir olay ve veri işlem hattı oluşturmak için bunları bağlarsınız. Diğer hizmetlerin olaylara yanıt için olay kılavuzunu kullanın. Bir veri ambarına veri geçirmek için Event Hubs ile olay kılavuzu kullanarak, bir örnek için bkz: [veri ambarına büyük veri akışı](event-grid-event-hubs-integration.md). Aşağıdaki resimde veri akışı için iş akışı gösterilmektedir.

![Akış verilerine genel bakış](./media/compare-messaging-services/overview.png)

## <a name="next-steps"></a>Sonraki adımlar

* Azure Mesajlaşma hizmetleri hakkında daha fazla bilgi için blog gönderisine bakın [olayları, veri noktaları ve verileriniz için sağ Azure Mesajlaşma hizmeti seçme iletileri -](https://azure.microsoft.com/blog/events-data-points-and-messages-choosing-the-right-azure-messaging-service-for-your-data/).
* Olay kılavuz giriş için bkz: [hakkında olay kılavuz](overview.md).
* Olay Kılavuzu ile çalışmaya başlamak için bkz: [Azure olay kılavuz oluşturma ve rota özel olaylarla](custom-event-quickstart.md).
* Event Hubs ile çalışmaya başlamak için bkz: [bir olay hub'ları ad alanı oluşturup Azure portalını kullanarak bir event hub](../event-hubs/event-hubs-create.md).
* Service Bus ile çalışmaya başlamak için bkz: [Azure portalını kullanarak bir hizmet veri yolu ad alanı oluşturma](../service-bus-messaging/service-bus-create-namespace-portal.md).