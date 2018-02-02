---
title: Azure Event Hubs ile ilgili SSS | Microsoft Docs
description: "Azure Event Hubs sık sorulan sorular (SSS)"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: bfa10984-eb22-4671-861a-f377a90d9372
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2018
ms.author: sethm
ms.openlocfilehash: 6bdcbbe37613d5384017409f3be2772085e276ae
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="event-hubs-frequently-asked-questions"></a>Olay hub'ları sık sorulan sorular

## <a name="general"></a>Genel

### <a name="what-is-the-difference-between-event-hubs-basic-and-standard-tiers"></a>Olay hub'ları temel ve standart katmanları arasındaki fark nedir?

Azure Event hubs standart katmanı temel katmanında nelerin kullanılabildiğini ötesinde özellikler sunar. Aşağıdaki özellikleri, standart eklenmiştir:
* Uzun olay saklama
* Aşma bir ücret dahil sayıdan daha fazla bilgi için ile ek aracılı bağlantılar
* Birden çok tek bir tüketici grubu
* [Yakalama](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview)

Fiyatlandırma katmanlarına, olay hub'ları ayrılmış, dahil olmak üzere hakkında daha fazla bilgi için bkz: [olay hub'ın fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="what-are-event-hubs-throughput-units"></a>Event Hubs işleme birimleri nelerdir?

Event Hubs işleme birimleri, Azure portal veya olay hub'ları Resource Manager şablonları ile açıkça seçersiniz. Olay hub'ları ad alanındaki tüm event hubs işleme birimleri uygulamak ve her işleme birimi ad alanı için aşağıdaki özellikleri hakkı verir:

* Yedeklemek için giriş olayları (olayları bir event hub'ına gönderilen), ancak hiçbir 1000'den fazla giriş olayları, yönetim işlemlerini veya denetim saniyede 1 MB API saniyede çağırır.
* 2 MB saniye başına çıkış olayları (olayları bir event hub'ından tüketilen).
* 84 GB'a kadar olay depolama (varsayılan 24 saatlik saklama süresi için yeterlidir).

Olay hub'ları üretilen iş birimleri saatlik, belirtilen saatte seçilen birim sayısının göre faturalandırılır. Otomatik olarak [numara üretilen iş birimleri artırmak](event-hubs-auto-inflate.md) kullanımınızı arttıkça.

### <a name="how-are-event-hubs-throughput-unit-limits-enforced"></a>Event Hubs işleme birimi sınırları nasıl uygulanır?
Toplam giriş işleme veya bir ad alanındaki tüm event hubs arasında toplam giriş olay hızı toplam verimlilik birim kesintileri aşarsa Gönderenler kısıtlanan ve giriş kotası aşıldı gösteren hatalar alabilirsiniz.

Toplam çıkış işleme veya bir ad alanındaki tüm event hubs arasında toplam olay çıkış hızı toplam verimlilik birim kesintileri aşarsa, alıcılar kısıtlanan ve çıkış kotası aşıldı gösteren hatalar alabilirsiniz. Giriş ve çıkış kotaları ayrı olarak, böylece gönderen olay tüketimi yavaşlamaya neden olabilir ya da bir alıcı olayları bir event hub'ına gönderilen engelleyebilirsiniz uygulanır.

### <a name="is-there-a-limit-on-the-number-of-throughput-units-that-can-be-selected"></a>Seçilebilecek üretilen iş birimi sayısı sınırlı mıdır?
Varsayılan kota ad alanı başına 20 işleme birimidir yoktur. Bir destek bileti dosyalama tarafından üretilen iş birimleri büyük kotası isteyebilir. 20 işleme birimi sınırı aşan paketleri 20 ve 100 üretilen iş birimleri içinde kullanılabilir. 20'den fazla üretilen iş birimleri kullanılarak bir destek bileti dosyalama olmadan üretilen iş birimleri sayısını değiştirme olanağı kaldırdığını unutmayın.

Kullanarak [otomatik Şişir](event-hubs-auto-inflate.md) özelliği, kullanımınızı arttıkça sayı üretilen iş birimleri otomatik olarak artırabilir.

### <a name="can-i-use-a-single-amqp-connection-to-send-and-receive-from-multiple-event-hubs"></a>Birden çok olay hub'larından alıp göndermek için tek bir AMQP bağlantı kullanabilir miyim?
Evet, aynı adlı ad alanındaki tüm event hubs olduğu sürece.

### <a name="what-is-the-maximum-retention-period-for-events"></a>Olaylar için maksimum bekletme süresi nedir?
Olay hub'ları standart katmanı, şu anda maksimum Bekletme dönemi 7 gün destekler. Olay hub’larının kalıcı veri depoları olarak kullanılmak üzere tasarlanmadığını unutmayın. Bekletme süreleri 24 saatten fazla olay akışının aynı sistemlere yeniden yürütme için uygun olduğu senaryoları için tasarlanmıştır; Örneğin, eğitmek veya yeni bir makine öğrenimi modeline var olan verileri doğrulayın. 7 gün dışında tutma iletisi varsa, etkinleştirme [olay hub'ları yakalama](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview) üzerinde olay hub'ı olay hub'ınızı verileri depolama veya seçtiğiniz Azure Data Lake hizmeti hesabına çeker. Yakalama etkinleştirme, satın alınan işleme birimine dayalı bir ücret doğurur.

### <a name="where-is-azure-event-hubs-available"></a>Burada Azure Event Hubs var mı?
Azure Event Hubs tüm desteklenen Azure bölgelerde kullanılabilir. Bir liste için ziyaret [Azure bölgeleri](https://azure.microsoft.com/regions/) sayfası.  

## <a name="best-practices"></a>En iyi uygulamalar

### <a name="how-many-partitions-do-i-need"></a>Kaç tane bölümleri ihtiyacım var mı?

Kurulumdan sonra bir olay hub'ındaki bölüm sayısı değiştirilemez unutmayın. Aklınızda başlamadan önce gereken kaç bölümleri hakkında düşünmek önemlidir. 

Olay hub'ları tüketici grubu başına tek bir bölüm okuyucusu izin verecek şekilde tasarlanmıştır. Çoğu durumda, varsayılan ayarı olan dört bölüm yeterli kullanın. Olay işleme ölçeğini arıyorsanız, ek bölümler eklemeyi düşünün isteyebilirsiniz. Belirli üretilen iş sınırı yoktur bir bölüme ancak, ad alanınız içinde toplam verimlilik üretilen iş birimleri sayısı ile sınırlıdır. Ad alanınız içinde işleme birimlerinin sayısı arttıkça, ek bölümler kendi en yüksek verimlilik elde etmek eşzamanlı okuyucunun bağlanmasına izin vermek isteyebilirsiniz.

Uygulamanız bir benzeşim belirli bir bölüme sahip bir model varsa, ancak bölüm sayısını artırmayı size herhangi bir avantajı, olmayabilir. Bu konu hakkında daha fazla bilgi için bkz: [kullanılabilirlik ve tutarlılık](event-hubs-availability-and-consistency.md).

## <a name="pricing"></a>Fiyatlandırma

### <a name="where-can-i-find-more-pricing-information"></a>Daha fazla fiyatlandırma bilgileri nereden bulabilirim?
Event Hubs fiyatlandırması hakkında tam bilgi için bkz: [olay hub'ın fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>Olay hub'ları olayları 24 saatten fazla koruma için bir ücret var mı?
Olay hub'ları standart katmanı ileti bekletme dönemleri en fazla 7 gün 24 saatten uzun izin vermez. Seçili üretilen iş birimleri (işleme birimi başına 84 GB) sayısı depolama indirimi saklı olaylarının toplam sayısı boyutunu aşarsa, indirimi aşıyor boyutu yayımlanan Azure Blob Depolama hızında ücretlendirilir. Her işleme birimi depolama indirimi 24 saatlik bekletme dönemleri tüm depolama maliyetlerini kapsayan (varsayılan) bile işleme birimi izin verilen en fazla giriş kullanım için kullanılır.

### <a name="how-is-the-event-hubs-storage-size-calculated-and-charged"></a>Nasıl olay hub'ları depolama boyutu hesaplanan ücret ve nedir?
Tüm event hubs olay üstbilgileri veya disk depolama yapılarına iç herhangi ek yük dahil olmak üzere tüm saklı olayların toplam boyutu gün boyunca ölçülür. Günün sonunda en büyük depolama boyutu hesaplanır. Günlük depolama alanı kullanım sınırı, gün boyunca seçilen en az aktarım hızı birimi sayısına göre hesaplanır (her bir aktarım hızı birimi 84 GB'lık kullanım sınırı sağlar). Hesaplanan günlük depolama indirimi toplam boyutu aşarsa, aşırı depolama Azure Blob Depolama fiyatlarına kullanarak faturalandırılır (adresindeki **yerel olarak yedekli depolama** hızı).

### <a name="how-are-event-hubs-ingress-events-calculated"></a>Olay hub'ları giriş olayları nasıl hesaplanır?
Olay hub'ına gönderilen her olayın Faturalanabilir ileti olarak sayılır. Bir *giriş olay* 64 KB veya daha küçük veri birimi olarak tanımlanır. Küçük veya eşittir 64 KB boyutunda herhangi bir olayın Faturalanabilir bir olay olarak kabul edilir. Olay 64 KB'den büyükse Faturalanabilir olay sayısı olay boyutu 64 KB'ün katları göre hesaplanır. Örneğin, olay hub'ına gönderilen bir 8 KB olay bir olay olarak faturalandırılır ancak olay hub'ına gönderilen bir 96 KB ileti iki olayları olarak faturalandırılır.

Yönetim işlemlerini ve denetim çağrıları kontrol noktaları gibi iyi Faturalanabilir giriş olayları sayılmaz, ancak en fazla işleme birimi indirimi tahakkuk gibi bir olay hub'dan tüketilen olaylar.

### <a name="do-brokered-connection-charges-apply-to-event-hubs"></a>Aracılı bağlantı ücretler Event Hubs'a geçerli?
Bağlantı ücretleri yalnızca AMQP Protokolü kullanıldığında geçerlidir. Gönderen sistem veya cihazların sayısı ne olursa olsun, HTTP kullanarak olay göndermeye ilişkin herhangi bir bağlantı ücreti yoktur. AMQP (örneğin, olay daha verimli akış elde etmek veya IOT Komuttaki çift yönlü iletişimi etkinleştirmek ve senaryoları denetlemek için) kullanmak planlıyorsanız bkz [olay hub'ın fiyatlandırma bilgileri](https://azure.microsoft.com/pricing/details/event-hubs/) kaç bağlantıları her hizmet katmanında dahil edilen hakkında ayrıntılar için sayfa.

### <a name="how-is-event-hubs-capture-billed"></a>Event Hubs Yakalama nasıl faturalandırılır?
Tüm olay hub'ad alanında yakalama seçeneği etkin olduğunda yakalama etkinleştirilir. Olay hub'ları yakalama satın alınan işleme birimi saatlik faturalandırılır. Üretilen iş birimi sayısı artırılabilir veya azaltılabilir gibi olay hub'ları yakalama faturalama tüm saat halinde bu değişiklikleri yansıtır. Olay hub'ları yakalama faturalama hakkında daha fazla bilgi için bkz: [olay hub'ın fiyatlandırma bilgileri](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="will-i-be-billed-for-the-storage-account-i-select-for-event-hubs-capture"></a>Olay hub'ları yakalama için seçtiğim depolama hesabı için faturalandırılmaya?
Yakalama bir olay hub'ına etkinleştirildiğinde sağlayan bir depolama hesabı kullanır. Depolama hesabınız olarak, bu yapılandırma için değişiklikleri Azure aboneliğinize faturalandırılır.

## <a name="quotas"></a>Kotalar

### <a name="are-there-any-quotas-associated-with-event-hubs"></a>Event Hubs ile ilişkili olan kotaları bulunur?
Tüm Event Hubs kotaları listesi için bkz: [kotaları](event-hubs-quotas.md).

## <a name="troubleshooting"></a>Sorun giderme

### <a name="what-are-some-of-the-exceptions-generated-by-event-hubs-and-their-suggested-actions"></a>Olay hub'ları ve bunların önerilen eylemleri tarafından oluşturulan özel durumları bazıları nelerdir?
Olası olay hub'ları özel durumlar listesi için bkz: [özel durumlar genel bakış](event-hubs-messaging-exceptions.md).

### <a name="diagnostic-logs"></a>Tanılama günlükleri
İki tür olay hub'ları destekler [tanılama günlükleri](event-hubs-diagnostic-logs.md) -yakalama Hata günlüklerini ve işlem günlüklerini - her ikisi de json'da temsil edilir ve Azure Portalı aracılığıyla açılabilir.

### <a name="support-and-sla"></a>Destek ve SLA
Olay hub'ları için teknik destek aracılığıyla kullanılabilir [topluluk forumları](https://social.msdn.microsoft.com/forums/azure/home). Faturalandırma ve abonelik yönetim desteği ücretsiz olarak sunulmaktadır.

Bizim SLA hakkında daha fazla bilgi için bkz: [hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/) sayfası.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)
* [Olay Hub’ı oluşturma](event-hubs-create.md)
* [Olay hub'ları otomatik-Şişir](event-hubs-auto-inflate.md)
