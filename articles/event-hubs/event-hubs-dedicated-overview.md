---
title: Ayrılmış event hubs - Azure Event Hubs'a genel bakış | Microsoft Docs
description: Bu makalede, event hubs'ın tek kiracılı dağıtımları sunan adanmış Azure Event Hubs hakkında genel bir bakış sağlar.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 52e092e6e48f004656860cb5d078e780039584ab
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66730233"
---
# <a name="overview-of-event-hubs-dedicated"></a>Ayrılmış Event Hubs'a genel bakış

*Event Hubs kümeleri* En zorlu akış ihtiyaçları olan müşteriler için tek kiracılı dağıtımları sunar. Bu tek kiracılı teklifi garantili % 99,99 SLA'sı vardır ve fiyatlandırma katmanı yalnızca bizim adanmış üzerinde kullanılabilir. Bir olay hub'ları kümesi garantili kapasitesi ve saniyenin gecikme süresi ile saniyede milyonlarca giriş olabilir. Ayrılmış bir küme içinde oluşturulan ad alanları ve olay hub'ları ve daha fazlasını standart teklifi, ancak herhangi bir giriş sınır tüm özellikleri içerir. Ayrıca popüler içerir [Event Hubs yakalama](event-hubs-capture-overview.md) otomatik olarak batch ve günlük veri akışları için Azure depolama veya Azure Data Lake izin vererek, hiçbir ek ücret ödemeden özelliği. 

Kümeleri sağlanan ve tarafından faturalandırılır **kapasite birimleri (cu)** , CPU ve bellek kaynakları önceden ayrılmış bir miktarı. Her küme için 1, 2, 4, 8, 12, 16 veya 20 cu satın alabilirsiniz. Alma ve akış CU ne kadar çeşitli etkenlere bağlıdır üreticileri ve tüketicileri yükü şekli çıkış sayısı gibi oranı (daha fazla ayrıntı için aşağıda kıyaslama sonuçları bakın). 

> [!NOTE]
> Tüm Event Hubs kümeleri Kafka-varsayılan olarak etkindir ve kullanılabilir Kafka uç noktaları destekleyen mevcut tarafından Kafka tabanlı uygulamalar. Kafka etkin olması, küme, Kafka olmayan kullanım örnekleri; etkilemez seçeneği veya bir kümede Kafka devre dışı bırakmak için gereksinim yoktur.

## <a name="why-dedicated"></a>Ayrılmış neden?

Event Hubs adanmış, kurumsal düzeyde kapasiteye ihtiyaç duyan müşteriler için üç ilgi çekici avantajları sağlar:

#### <a name="single-tenancy-guarantees-capacity-for-better-performance"></a>Tek kiracılı daha iyi performans için kapasiteyi garanti eder.

Ayrılmış bir küme tam ölçekli kapasite garantisi verir ve akış verileri ile herhangi bir uyum sağlamak için tam olarak dayanıklı depolama yanı sıra bir saniyenin gecikme gigabayt en çok trafiği veri bloğu. 

#### <a name="inclusive-and-exclusive-access-to-features"></a>Dahil ve hariç özelliklere erişim 
Adanmış teklif hiçbir ek maliyet yanı sıra özel erişim BYOK gibi yaklaşan özelliklerden en yakalama gibi özellikler içerir. Böylece altyapı Bakımı ve istemci tarafı özellikleri oluşturmaya daha fazla zaman üzerinde daha az zaman harcayın hizmet aynı zamanda Yük Dengeleme, işletim sistemi güncelleştirmeleri, güvenlik yamaları ve müşteri için bölümleme yönetir.  

#### <a name="cost-savings"></a>Maliyet tasarrufu
En yüksek giriş birimler (> 100 işleme birimi), küme maliyetleri önemli ölçüde değerinden saat başına aktarım hızı birimlerini standart teklifi karşılaştırılabilir miktarı satın.


## <a name="event-hubs-dedicated-quotas-and-limits"></a>Event Hubs kotaları ve sınırlamaları ayrılmış

Event Hubs Dedicated teklifi, bir aylık fiyatla, en az 4 saatlik kullanım ücreti alınır. Adanmış katmanı, zorlu iş yüklerine sahip müşteriler için tüm özellikleri standart plan, ancak Kurumsal ölçek kapasitesi ve sınırları ile sunar. 

| Özellik | Standart | Adanmış |
| --- |:---:|:---:|
| Bant genişliği | 20 işleme birimi (en fazla 40 işleme birimi) | 20 cu |
| Ad Alanları |  1 | 50 CU başına |
| Event Hubs |  ad alanı başına 10 | ad alanı başına 1000 |
| Giriş olayları | Milyon olay başına ödeme yapın | Dahil |
| İleti Boyutu | 1 milyon bayt | 1 milyon bayt |
| Bölümler | ad alanı başına 40 | CU başına 2000 |
| Tüketici grupları | Olay hub'ı başına 20 | Sınır, CU başına 1000 olay hub'ı başına |
| Aracılı bağlantılar | en fazla 1.000 dahil 5.000 | 100 bin adet dahil ve en fazla |
| İleti Saklama | 7 gün, dahil edilen işleme birimi 84 GB | 90 gün, CU dahil edilen 10 TB |
| Capture | Saat başına ödeme yapın | Dahil |

## <a name="how-to-onboard"></a>Nasıl eklemek için

İçin Self Servis deneyimi [bir olay hub'ları kümesi oluşturma](event-hubs-dedicated-cluster-create-portal.md) aracılığıyla [Azure portalı](https://aka.ms/eventhubsclusterquickstart) önizlemeye sunuldu. Herhangi bir sorunuz veya ekleme, Event Hubs Dedicated için yardıma ihtiyacı varsa lütfen şuraya başvurun [Event Hubs ekibine](mailto:askeventhubs@microsoft.com).

## <a name="faqs"></a>SSS

#### <a name="what-can-i-achieve-with-a-cluster"></a>Hangi ben bir kümeyle elde edebilirsiniz?

Bir olay hub'ları kümesi için ne kadar içe alma akış ve, üreticiler, Tüketiciler, hangi, başlayan kümeniz işleme ve oranı ve çok daha fazlası gibi çeşitli faktörlere bağlıdır. 

Aşağıdaki tabloda, bizim test sırasında alanımız Kıyaslama sonuçlar gösterilir:

| Yükü şekli | Alıcılar | Giriş bant genişliği| Giriş iletileri | Çıkış bant genişliği | Çıkış iletileri | Toplam işleme birimi | CU başına işleme birimi |
| ------------- | --------- | ---------------- | ------------------ | ----------------- | ------------------- | --------- | ---------- |
| 100x1KB toplu | 2 | 400 MB/sn | 400 bin iletileri/sn | 800 MB/sn | 800 k ileti sayısı/sn | 400 işleme birimi | 100 işleme birimi | 
| 10x10KB toplu | 2 | 666 MB/sn | 66.6 k ileti sayısı/sn | 1.33 GB/sn | 133 k ileti sayısı/sn | 666 işleme birimi | 166 işleme birimi |
| 6x32KB toplu | 1 | 1,05 GB/sn | 34 k iletileri / sn | 1,05 GB/sn | 34 k ileti sayısı/sn | 1000 işleme birimi | 250 işleme birimi |

Testinizde aşağıdaki ölçütleri izin kullanıldı:

- Adanmış katmanı Event Hubs kümesi dört kapasite birimleri (cu) ile kullanıldı. 
- Alma işlemi için kullanılan olay hub'ı 200 bölümler vardı. 
- Tüm bölümleri alma iki alıcı uygulamalar tarafından alınan veri alındı.

#### <a name="can-i-scale-updown-my-cluster"></a>Ben yukarı/aşağı kümem ölçeklendirebilirsiniz?

Oluşturulduktan sonra küme en az 4 saatlik kullanım için faturalandırılırsınız. Self Servis deneyimi Önizleme sürümünde, gönderdiğiniz bir [destek isteği](https://ms.portal.azure.com/#create/Microsoft.Support) altında Event Hubs ekibine *teknik > kotasına > Ölçek yukarı veya aşağı ayrılmış küme ölçeklendirme isteği* ölçek kümenizi artırın veya azaltın. Bu, kümenizi ölçeklendirme isteği tamamlamak için en fazla 7 gün sürebilir. 

#### <a name="how-will-geo-dr-work-with-my-cluster"></a>Coğrafi-DR, kümem ile nasıl çalışır?

Coğrafi bir ad alanı bir adanmış katmanı kümesi altında bir adanmış katmanı kümesi altında başka bir ad alanı ile çift kullanabilirsiniz. Bir adanmış katmanı ad aktarım hızı sınırı hatalara neden olabilecek uyumsuz olacağından, bizim standart, sunan bir ad alanıyla eşleştirildikten öneririz değil. 

#### <a name="can-i-migrate-my-standard-namespaces-to-belong-to-a-dedicated-tier-cluster"></a>Adanmış katmanı kümeye ait standart my ad alanı geçişini sağlayabilir miyim?
Şu anda bir geçiş işlemi, olay hub'ları verilerinizi standart bir ad alanındaki bir adanmış bir geçiş için desteklemiyoruz. 

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs Dedicated hakkında ek ayrıntıları almak için Microsoft satış temsilcinize veya Microsoft Support başvurun. Bir küme oluşturun veya olay hub'ları aşağıdaki bağlantıları ziyaret ederek fiyatlandırma katmanları hakkında daha fazla bilgi:

- [Azure portalı ile bir olay hub'ları kümesi oluşturma](https://aka.ms/eventhubsclusterquickstart) 
- [Event Hubs Dedicated fiyatlandırması](https://azure.microsoft.com/pricing/details/event-hubs/). Event Hubs adanmış kapasite hakkında ek ayrıntıları almak için Microsoft satış temsilcinize veya Microsoft Support de başvurabilirsiniz.
- [Event Hubs SSS Sayfasındaki](event-hubs-faq.md) fiyatlandırma bilgileri içeriyor ve Event Hubs hakkında sık sorulan bazı sorular yanıtlanmaktadır.
