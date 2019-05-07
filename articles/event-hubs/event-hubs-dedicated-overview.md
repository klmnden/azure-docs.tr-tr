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
ms.openlocfilehash: e8a2d8321a42e8b3d090c1ce1fdb3fd9a7ee3714
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65138655"
---
# <a name="overview-of-event-hubs-dedicated"></a>Ayrılmış Event Hubs'a genel bakış

Azure Event Hubs kümeleri, En zorlu akış ihtiyaçları olan müşteriler için tek kiracılı dağıtımları sunar. Bu tek kiracılı teklifi garantili % 99,99 SLA'sı vardır. Fiyatlandırma katmanı adanmış üzerinde kullanılabilir.

Bir olay hub'ları kümesi garantili kapasitesi ve subsecond gecikme süresi ile saniyede milyonlarca giriş olabilir. Ad alanları ve event hubs adanmış küme içinde oluşturulan tüm özellikleri ve daha fazlasını standart teklifi, ancak herhangi bir giriş sınır içerir. Ayrıca [Event Hubs yakalama](event-hubs-capture-overview.md) hiçbir ek ücret ödemeden özelliği. Azure depolama veya Azure Data Lake için otomatik olarak batch ve günlük veri akışları için kullanabilirsiniz.

Adanmış kümeleri sağlanabilir ve kapasite birimleri (cu) tarafından faturalandırılır. Cu ön tahsis miktarda CPU ve bellek kaynak var. 1, 2, 4, 8, 12, 16 veya her küme için 20 cu satın alabilirsiniz. Ne kadar içe alabilir ve akış CU başına üreticileri ve tüketicileri, yükü şekli ve çıkış oranı sayısı gibi etkenlere bağlıdır.

Daha fazla bilgi için kıyaslama sonuçları tabloya bakın.

## <a name="why-use-event-hubs-dedicated"></a>Event Hubs Dedicated neden kullanmalısınız?

Event Hubs adanmış, kurumsal düzeyde kapasiteye ihtiyaç duyan müşteriler için üç avantaj sunar.

#### <a name="single-tenancy-guarantees-capacity-for-better-performance"></a>Tek kiracılı daha iyi performans için kapasiteyi garanti eder.

Ayrılmış bir küme tam ölçekli kapasite garanti eder. Giriş akış verileri ile trafik artışlarını karşılamak için tam olarak dayanıklı depolama ve subsecond gecikme gigabayt kadar alabilir.

#### <a name="inclusive-and-exclusive-access-to-features"></a>Dahil ve hariç özelliklere erişim 
Adanmış teklif hiçbir ek ücret ödemeden yakalama gibi özellikler içerir. Ayrıca, özel erişim gibi BYOK yakında çıkacak özellikler sunar. Hizmet ayrıca Yük Dengeleme, işletim sistemi güncelleştirmeleri, güvenlik yamaları ve bölümleme yönetir. Bu özellikler sayesinde daha az zaman, altyapı bakımına ve istemci tarafı özellikleri oluşturmaya daha fazla zaman ayırabilirsiniz.

#### <a name="cost-savings"></a>Maliyet tasarrufları
Yüksek giriş hacimlerinde bir küme maliyetleri önemli ölçüde daha az ise değerinden saat başına aktarım hızı birimlerini (işleme birimi) benzer bir miktar standart teklifinde satın. Yüksek bir birimdir > 100 işleme birimi.


## <a name="event-hubs-standard-vs-dedicated"></a>Event Hubs standart vs. Adanmış

Aşağıdaki tablo, Event Hubs kullanılabilir hizmet katmanlarını karşılaştırır. Event Hubs adanmış teklif, standart özelliklerinin çoğu için kullanım fiyatlarıyla karşılaştırıldığında aylık sabit fiyata göre faturalandırılır. Adanmış katmanı, tüm özellikleri standart planı ancak zorlu iş yüklerine sahip müşteriler için Kurumsal ölçekte kapasitesi sunar.

| Özellik | Standart | Adanmış |
| --- |:---:|:---:|
| Giriş olayları | Milyon olay başına ödeme yapın | Dahil |
| Üretilen iş birimi (1 MB/sn giriş, 2 MB/sn çıkış) | Saat başına ödeme yapın | Dahil |
| İleti boyutu | 1 MB | 1 MB |
| Bölümler | ad alanı başına 40 | CU başına 2.000 |
| Tüketici grupları | Olay hub'ı başına 20 | Olay hub'ı başına 1000 |
| En yüksek bant genişliği | 20 işleme birimi (en fazla 40 işleme birimi) | 20 cu |
| Aracılı bağlantılar | 1.000 dahil | 100.000 dahil |
| İleti saklama | Bir gün dahil | En fazla yedi gün dahil |
| Capture | Saat başına ödeme yapın | Dahil |

## <a name="what-can-i-achieve-with-a-cluster"></a>Hangi ben bir kümeyle elde edebilirsiniz?

Bir olay hub'ları kümesi için ne kadar içe alma akış ve, üreticiler, tüketicileriniz, hızı, alma ve işleme ve daha fazlasını bağlıdır.

Aşağıdaki tabloda, test sırasında gerçekleştirilen kıyaslama sonuçları gösterilmektedir.

| Yükü şekli | Alıcılar | Giriş bant genişliği| Giriş iletileri | Çıkış bant genişliği | Çıkış iletileri | Toplam işleme birimi | CU başına işleme birimi |
| ------------- | --------- | ---------------- | ------------------ | ----------------- | ------------------- | --------- | ---------- |
| 100x1KB toplu | 2 | 400 MB/sn | 400.000 ileti sayısı/sn | 800 MB/sn | 800.000 ileti sayısı/sn | 400 işleme birimi | 100 işleme birimi | 
| 10x10KB toplu | 2 | 666 MB/sn | 66,600 ileti sayısı/sn | 1.33 GB/sn | 133,000 ileti sayısı/sn | 666 işleme birimi | 166 işleme birimi |
| 6x32KB toplu | 1 | 1,05 GB/sn | 34.000 ileti sayısı/sn | 1,05 GB/sn | 34.000 ileti sayısı/sn | 1000 işleme birimi | 250 işleme birimi |

Testinizde aşağıdaki ölçütler kullanıldı:

- Adanmış katmanı Event Hubs küme ile dört kapasite birimleri kullanıldı.
- Alma işlemi için kullanılan olay hub'ı 200 bölümler vardı.
- İki alıcı uygulama tarafından alınan veri alındı. Bunlar, tüm bölümleri veri alındı.

## <a name="use-event-hubs-dedicated"></a>Ayrılmış Event Hubs'ı kullanın

Event Hubs adanmış, kullanılacak [fatura desteği ile iletişime](https://ms.portal.azure.com/#create/Microsoft.Support) ya da Microsoft temsilcinizle. Ekleyerek veya kaldırarak cu gereksinimlerinizi karşılayacak şekilde ay boyunca yukarı veya aşağı kapasitenizi ölçeklendirebilirsiniz. Event Hubs ürün ekibi, size en uygun esnek dağıtım almak için yardımcı olur.

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs adanmış kapasite hakkında ek bilgi almak için Microsoft satış temsilcinize veya Microsoft Support başvurun. Event Hubs fiyatlandırma katmanları hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın:

- [Event Hubs Dedicated fiyatlandırması](https://azure.microsoft.com/pricing/details/event-hubs/). Event Hubs adanmış kapasite hakkında ek bilgi almak için Microsoft satış temsilcinize veya Microsoft Support de başvurabilirsiniz.
- [Event Hubs SSS Sayfasındaki](event-hubs-faq.md) fiyatlandırma bilgileri içeriyor ve Event Hubs hakkında sık sorulan bazı sorular yanıtlanmaktadır.
