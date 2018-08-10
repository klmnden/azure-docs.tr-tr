---
title: Azure Event Hubs SSS | Microsoft Docs
description: Azure Event Hubs sık sorulan sorular (SSS)
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.date: 06/07/2018
ms.author: shvija
ms.openlocfilehash: 3e98d27c297e223231e0990adc51f8765678b884
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "40006830"
---
# <a name="event-hubs-frequently-asked-questions"></a>Olay hub'ları hakkında sık sorulan sorular

## <a name="general"></a>Genel

### <a name="what-is-the-difference-between-event-hubs-basic-and-standard-tiers"></a>Event Hubs temel ve standart katmanları arasındaki fark nedir?

Azure Event Hubs standart katmanı temel katmana sistemlerdekinden özellikleri sağlar. Standart aşağıdaki özellikler eklenmiştir:

* Olay saklama uzun
* Dahil edilen sayıdan daha fazla bilgi için bir fazla kullanım ücreti olan ek aracılı bağlantılar
* Birden fazla [tüketici grubu](event-hubs-features.md#consumer-groups)
* [Yakalama](event-hubs-capture-overview.md)

Fiyatlandırma katmanları, Event Hubs adanmış, dahil olmak üzere hakkında daha fazla bilgi için bkz. [olay hub'ları fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="what-are-event-hubs-throughput-units"></a>Event Hubs işleme birimleri nedir?

Event Hubs işleme birimleri, Azure portalını ya da olay hub'ları Resource Manager şablonları aracılığıyla açıkça seçtiğiniz. İşleme birimleri bir Event Hubs ad alanındaki tüm event hubs için geçerlidir ve her bir üretilen iş birimi ad alanına aşağıdaki özellikleri sağlar:

* Yukarı giriş olayı (bir olay hub'ına gönderilen olaylar), ancak hiçbir fazla 1000 giriş olayı, yönetim işlemleri veya denetim saniyede 1 MB'a kadar saniye başına API çağrısı.
* En fazla saniye başına 2 MB çıkış olayı (bir olay hub'ınden tüketilen olaylar), ancak en fazla 4096 çıkış olayı.
* 84 GB'a kadar olay depolama (varsayılan 24 saatlik saklama süresi için yeterlidir).

Event Hubs işleme birimleri, belirli bir saatte seçilen birimleri, maksimum sayısına göre faturalandırılır. Otomatik olarak [sayı üretilen iş birimleri otomatik şişme kullanarak artırmak](event-hubs-auto-inflate.md) kullanım arttıkça.

### <a name="how-are-event-hubs-throughput-unit-limits-enforced"></a>Event Hubs işleme birimi limitlerini nasıl zorlanır?

Toplam giriş üretilen işi veya bir ad alanındaki tüm event hubs'da toplam giriş olayı oranı toplam üretilen iş birimi kullanım sınırlarını aşıyorsa, Gönderenler azaltılır ve giriş kotasının aşıldığını belirten hatalar alır.

Toplam çıkış üretilen işi veya toplam çıkış olayı oranı, bir ad alanındaki tüm event hubs'da toplam üretilen iş birimi kullanım sınırlarını aşıyorsa, alıcılar azaltılır ve çıkış kotasının aşıldığını belirten hatalar alır. Böylece hiçbir gönderen olay tüketiminin yavaşlamasına neden olmaz ya da bir alıcı bir olay hub'ına gönderilen olaylar engellemez, giriş ve çıkış kotaları ayrı ayrı uygulanır.

### <a name="is-there-a-limit-on-the-number-of-throughput-units-that-can-be-selected"></a>Seçilebilecek üretilen iş birimi sayısı sınırlı mıdır?

Varsayılan ad alanı başına 20 üretilen iş birimi kotası vardır. Bir destek bileti doldurarak daha büyük bir aktarım hızı birimi kotası isteyebilir. 20 üretilen iş birimi sınırı aşan paketleri 20 ve 100 işleme birimleri kullanılabilir. 20'den fazla işleme birimi kullanılarak üretilen iş birimlerinin sayısı bir destek bileti olmadan değiştirme olanağı kaldırdığını unutmayın.

Kullanarak [otomatik şişme](event-hubs-auto-inflate.md) özelliği, kullanım arttıkça sayı üretilen iş birimleri otomatik olarak artırabilir.

### <a name="can-i-use-a-single-amqp-connection-to-send-and-receive-from-multiple-event-hubs"></a>Birden fazla event hubs'dan alıp göndermek için tek bir AMQP bağlantısı kullanabilir miyim?

Tüm event hubs aynı ad alanında olduğu sürece Evet.

### <a name="what-is-the-maximum-retention-period-for-events"></a>Olaylar için maksimum bekletme süresi nedir?

Event Hubs standart katmanı şu anda bir maksimum Bekletme dönemi 7 gün destekler. Olay hub’larının kalıcı veri depoları olarak kullanılmak üzere tasarlanmadığını unutmayın. 24 saatten fazla uzun saklama süreleri, olay akışının aynı sistemlerde yeniden Yürüt kullanışlı olduğu senaryolar yöneliktir; Örneğin, denenmesi veya doğrulanması var olan veriler üzerinde yeni bir makine öğrenme modeli. 7 gün sonra saklama ileti varsa, etkinleştirme [Event Hubs yakalama](event-hubs-capture-overview.md) depolama hesabı ya da Azure Data Lake hizmeti hesabı seçtiğiniz hub, olayı, olay hub'ından veri çeker. Etkinleştirme yakalama, satın alınan işleme birimlerine göre bir ücret doğurur.

### <a name="where-is-azure-event-hubs-available"></a>Azure Event Hubs nerede kullanılabilir?

Azure Event hubs'ı desteklenen tüm Azure bölgelerinde kullanılabilir. Bir liste için ziyaret [Azure bölgeleri](https://azure.microsoft.com/regions/) sayfası.  

## <a name="best-practices"></a>En iyi uygulamalar

### <a name="how-many-partitions-do-i-need"></a>Kaç tane bölümler ihtiyacım var?

Kurulum sonrasında bir olay hub'ındaki bölüm sayısı değiştirilemez unutmayın. Aklınızda başlamadan önce kaç bölümleriyle ilgili ihtiyaçları önemlidir. 

Event Hubs, bir tüketici grubu başına tek bölüm okuyucusu izin vermek için tasarlanmıştır. Çoğu durumda, varsayılan ayarı olan dört bölüm yeterli kullanın. Olay işleme ölçeğini arıyorsanız, ek bölümler eklemeyi düşünün isteyebilirsiniz. Belirli bir aktarım hızı sınırı yoktur bir bölüme, ancak ad alanınızdaki toplam üretilen iş tarafından üretilen iş birimlerinin sayısı sınırlıdır. Ad alanınız içinde üretilen iş birimlerinin sayısı arttıkça, ek bölümler kendi en fazla aktarım hızı elde etmek eşzamanlı okuyucu izin vermek isteyebilirsiniz.

Uygulamanızı benzeşim belirli bir bölüme sahip bir model varsa, ancak bölüm sayısının artırılması, hiçbir Avantajı'ndan olmayabilir. Bu konu hakkında daha fazla bilgi için bkz. [kullanılabilirlik ve tutarlılık](event-hubs-availability-and-consistency.md).

## <a name="pricing"></a>Fiyatlandırma

### <a name="where-can-i-find-more-pricing-information"></a>Diğer fiyatlandırma bilgileri nerede bulabilirim?

Event Hubs fiyatlandırması hakkında tam bilgi için bkz. [olay hub'ları fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>Event Hubs olaylarını 24 saatten uzun süre tutma ücreti var mıdır?

Event Hubs standart katman için en fazla 7 gün 24 saatten uzun süreler ileti bekletme izin vermiyor. Boyutu depolanan olaylarının toplam sayısı, seçili üretilen iş birimi (aktarım hızı birimi başına 84 GB) için depolama sınırını aşarsa, izin verilen kullanımı aşan boyut yayımlanan Azure Blob Depolama fiyatı üzerinden ücretlendirilir. Her bir üretilen iş birimindeki depolama alanı kullanım sınırı, 24 saatlik saklama süreleri için tüm depolama maliyetlerini kapsar (varsayılan) aktarım hızı birimi en büyük giriş kullanım sınırında kullanıldığında bile.

### <a name="how-is-the-event-hubs-storage-size-calculated-and-charged"></a>Nasıl Event hubs'ı depolama boyutu hesaplanır ücretlendirilir ve nedir?

Tüm event hubs dahili ek yükler için olay üst bilgileri veya disk depolama yapıları dahil olmak üzere, depolanmış tüm olayların toplam boyutu gün boyunca ölçülür. Günün sonunda en büyük depolama boyutu hesaplanır. Günlük depolama alanı kullanım sınırı, gün boyunca seçilen en az aktarım hızı birimi sayısına göre hesaplanır (her bir aktarım hızı birimi 84 GB'lık kullanım sınırı sağlar). Toplam boyut hesaplanan günlük depolama sınırını aşarsa, fazla depolama Azure Blob Depolama fiyatları kullanılarak faturalandırılır (adresindeki **yerel olarak yedekli depolama** oranı).

### <a name="how-are-event-hubs-ingress-events-calculated"></a>Event hubs'ı giriş olayları nasıl hesaplanır?

Olay hub'ına gönderilen her olay Faturalanabilir mesaj olarak sayılır. Bir *giriş olayı* 64 KB veya daha küçük veri birimi olarak tanımlanır. Boyutu 64 KB eşit veya daha az olan herhangi bir olay, Faturalanabilir bir olay olarak kabul edilir. Olay 64 KB'den büyükse, Faturalanabilir olayların sayısı olay boyutu 64 KB'ın katlarına göre hesaplanır. Örneğin, olay hub'ına gönderilen 8 KB'lik bir olay bir olay olarak faturalandırılır ancak olay hub'ına gönderilen 96 KB'lık mesaj iki olay faturalandırılır.

Yönetim işlemleri ve denetim çağrıları kontrol noktaları gibi düzgün Faturalandırılabilir giriş olayları olarak kabul edilmez, ancak en fazla aktarım hızı birimi kullanım sınırından düşülür gibi bir olay hub'ından tüketilen olayların.

### <a name="do-brokered-connection-charges-apply-to-event-hubs"></a>Aracılı bağlantı ücretleri Event Hubs için geçerli midir?

Bağlantı ücretleri, yalnızca AMQP protokolünü kullanıldığında uygulanır. Gönderen sistem veya cihazların sayısı ne olursa olsun, HTTP kullanarak olay göndermeye ilişkin herhangi bir bağlantı ücreti yoktur. Gibi amaçlarla AMQP'yi (örneğin, daha verimli olay akışı elde etmek veya IOT komut çift yönlü iletişim sağlamak ve denetim senaryoları) kullanmayı planlıyorsanız bkz [Event Hubs fiyatlandırma bilgileri](https://azure.microsoft.com/pricing/details/event-hubs/) kaç hakkında daha fazla ayrıntı için bağlantılar, her hizmet katmanında dahil edilir.

### <a name="how-is-event-hubs-capture-billed"></a>Event Hubs Yakalama nasıl faturalandırılır?

Ad alanındaki tüm olay hub'ı yakalama seçeneği etkin olduğunda yakalama özelliği etkinleştirilir. Event Hubs yakalama, satın alınan işleme birimi başına saatlik faturalandırılır. Aktarım hızı birimi sayısı arttıkça veya azaldıkça olarak Event Hubs yakalama faturalandırması bu değişiklikleri tam saatlik adımlar olarak yansıtır. Event Hubs yakalama faturalandırması hakkında daha fazla bilgi için bkz: [Event Hubs fiyatlandırma bilgileri](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="will-i-be-billed-for-the-storage-account-i-select-for-event-hubs-capture"></a>Event Hubs yakalama için seçtiğim depolama hesabına için faturalandırılır mıyım?

Yakalama, sağladığınız bir olay hub'ında etkinleştirildiğinde bir depolama hesabını kullanır. Bu, depolama hesabı olduğundan, bu yapılandırma için herhangi bir değişiklik Azure aboneliğinize faturalandırılır.

## <a name="quotas"></a>Kotalar

### <a name="are-there-any-quotas-associated-with-event-hubs"></a>Event Hubs ile ilişkili herhangi bir kota var mı?

Tüm Event Hubs kotaları listesi için bkz. [kotalar](event-hubs-quotas.md).

## <a name="troubleshooting"></a>Sorun giderme

### <a name="what-are-some-of-the-exceptions-generated-by-event-hubs-and-their-suggested-actions"></a>Event Hubs ve önerilen eylemlerinin tarafından oluşturulan özel durumları bazıları nelerdir?

Olası Event Hubs özel durumları listesi için bkz. [özel durumlar genel bakış](event-hubs-messaging-exceptions.md).

### <a name="diagnostic-logs"></a>Tanılama günlükleri

Event hubs'ı destekleyen iki tür [tanılama günlükleri](event-hubs-diagnostic-logs.md) -yakalama Hata günlüklerini ve işlem günlükleri - ikisi için de json'da temsil edilir ve Azure portalı üzerinden etkinleştirilebilir.

### <a name="support-and-sla"></a>Destek ve SLA

Event Hubs için teknik destek, aracılığıyla kullanılabilir [topluluk forumları](https://social.msdn.microsoft.com/forums/azure/home?forum=servbus). Faturalandırma ve abonelik yönetim desteği ücretsiz olarak sunulmaktadır.

Sunduğumuz SLA hakkında daha fazla bilgi için bkz: [hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/) sayfası.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)
* [Olay Hub’ı oluşturma](event-hubs-create.md)
* [Olay hub'ları otomatik şişme](event-hubs-auto-inflate.md)
