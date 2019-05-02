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
ms.openlocfilehash: 52d46009464c7417d5b525154fdac09c030aabe7
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64708019"
---
# <a name="overview-of-event-hubs-dedicated"></a>Ayrılmış Event Hubs'a genel bakış

*Event Hubs kümeleri* En zorlu akış ihtiyaçları olan müşteriler için tek kiracılı dağıtımları sunar. Bu tek kiracılı teklifi garantili % 99,99 SLA'sı vardır ve fiyatlandırma katmanı yalnızca bizim adanmış üzerinde kullanılabilir. Bir olay hub'ları kümesi garantili kapasitesi ve saniyenin gecikme süresi ile saniyede milyonlarca giriş olabilir. Ayrılmış bir küme içinde oluşturulan ad alanları ve olay hub'ları ve daha fazlasını standart teklifi, ancak herhangi bir giriş sınır tüm özellikleri içerir. Ayrıca popüler içerir [Event Hubs yakalama](event-hubs-capture-overview.md) otomatik olarak batch ve günlük veri akışları için Azure depolama veya Azure Data Lake izin vererek, hiçbir ek ücret ödemeden özelliği. 

Adanmış kümeleri sağlanan ve tarafından faturalandırılır **kapasite birimleri (cu)**, CPU ve bellek kaynakları önceden ayrılmış bir miktarı. Her küme için 1, 2, 4, 8, 12, 16 veya 20 cu satın alabilirsiniz. Alma ve akış CU ne kadar çeşitli etkenlere bağlıdır üreticileri ve tüketicileri yükü şekli çıkış sayısı gibi oranı (daha fazla ayrıntı için aşağıda kıyaslama sonuçları bakın). 

## <a name="why-dedicated"></a>Ayrılmış neden?

Event Hubs adanmış, kurumsal düzeyde kapasiteye ihtiyaç duyan müşteriler için üç ilgi çekici avantajları sağlar:

#### <a name="single-tenancy-guarantees-capacity-for-better-performance"></a>Tek kiracılı daha iyi performans için kapasiteyi garanti eder.

Ayrılmış bir küme tam ölçekli kapasite garantisi verir ve akış verileri ile herhangi bir uyum sağlamak için tam olarak dayanıklı depolama yanı sıra bir saniyenin gecikme gigabayt en çok trafiği veri bloğu. 

#### <a name="inclusive-and-exclusive-access-to-features"></a>Dahil ve hariç özelliklere erişim 
Adanmış teklif hiçbir ek maliyet yanı sıra özel erişim BYOK gibi yaklaşan özelliklerden en yakalama gibi özellikler içerir. Böylece altyapı Bakımı ve istemci tarafı özellikleri oluşturmaya daha fazla zaman üzerinde daha az zaman harcayın hizmet aynı zamanda Yük Dengeleme, işletim sistemi güncelleştirmeleri, güvenlik yamaları ve müşteri için bölümleme yönetir.  

#### <a name="cost-savings"></a>Maliyet tasarrufu
En yüksek giriş birimler (> 100 işleme birimi), küme maliyetleri önemli ölçüde değerinden saat başına aktarım hızı birimlerini standart teklifi karşılaştırılabilir miktarı satın.


## <a name="event-hubs-standard-vs-dedicated"></a>Event Hubs standart vs. Adanmış

Aşağıdaki tablo, Event Hubs kullanılabilir hizmet katmanlarını karşılaştırır. Event Hubs adanmış teklif standart özelliklerinin çoğu için kullanım fiyatlarıyla karşılaştırıldığında sabit bir aylık fiyat ile faturalandırılır. Adanmış katmanı, tüm özellikleri standart plan, ancak zorlu iş yüklerine sahip müşteriler için Kurumsal ölçekte kapasiteyle sunar. 

| Özellik | Standart | Adanmış |
| --- |:---:|:---:|
| Giriş olayları | Milyon olay başına ödeme yapın | Dahil |
| Üretilen iş birimi (1 MB/sn giriş, 2 MB/sn çıkış) | Saat başına ödeme yapın | Dahil |
| İleti Boyutu | 1 MB | 1 MB |
| Bölümler | ad alanı başına 40 | CU başına 2000 |
| Tüketici grupları | Olay hub'ı başına 20 | Olay hub'ı başına 1000 |
| En çok, Bant Genişliği | 20 işleme birimi (en fazla 40 işleme birimi)    | 20 cu |
| Aracılı bağlantılar | 1.000 dahil | 100 bin adet dahil |
| İleti Saklama | 1 gün dahil | 7 gün dahil |
| Capture | Saat başına ödeme yapın | Dahil |

## <a name="what-can-i-achieve-with-a-cluster"></a>Hangi ben bir kümeyle elde edebilirsiniz?

Bir olay hub'ları kümesi için ne kadar içe alma akış ve, üreticiler, Tüketiciler, hangi, başlayan kümeniz işleme ve oranı ve çok daha fazlası gibi çeşitli faktörlere bağlıdır. 

Aşağıdaki tabloda, bizim test sırasında alanımız Kıyaslama sonuçlar gösterilir:

| Yükü şekli | Alıcılar | Giriş bant genişliği| Giriş iletileri | Çıkış bant genişliği | Çıkış iletileri | Toplam işleme birimi | CU başına işleme birimi |
| ------------- | --------- | ---------------- | ------------------ | ----------------- | ------------------- | --------- | ---------- |
| 100x1KB toplu | 2 | 400 MB/sn | 400 bin iletileri/sn | 800 MB/sn | 800 k iletileri/sn | 400 işleme birimi | 100 işleme birimi | 
| 10x10KB toplu | 2 | 666 MB/sn | 66.6 k iletileri/sn | 1.33 GB/sn | 133 k iletileri/sn | 666 işleme birimi | 166 işleme birimi |
| 6x32KB toplu | 1 | 1,05 GB/sn | 34 k iletileri / sn | 1,05 GB/sn | 34 k iletileri/sn | 1000 işleme birimi | 250 işleme birimi |

Testinizde aşağıdaki ölçütleri izin kullanıldı:

- Adanmış katmanı Event Hubs kümesi dört kapasite birimleri (cu) ile kullanıldı. 
- Alma işlemi için kullanılan olay hub'ı 200 bölümler vardı. 
- Tüm bölümleri alma iki alıcı uygulamalar tarafından alınan veri alındı.

## <a name="how-to-onboard"></a>Nasıl eklemek için

Bu SKU ekleneceği [fatura desteği ile iletişime](https://ms.portal.azure.com/#create/Microsoft.Support) ya da Microsoft temsilcinizle. Ekleyerek veya kaldırarak cu gereksinimlerinizi karşılayacak şekilde ay boyunca yukarı veya aşağı kapasitenizi ölçeklendirebilirsiniz. Özel planı, sizin için uygun olan esnek dağıtım almak için Event Hubs ürün ekibinden daha uygulamalı bir ekleme deneyimi açısından benzersizdir. 

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs ayrılmış kapasitesi hakkında ek ayrıntıları almak için Microsoft satış temsilcinize veya Microsoft Support başvurun. Ayrıca daha fazla fiyatlandırma katmanları aşağıdaki bağlantıları inceleyerek Event Hubs hakkında bilgi edinebilirsiniz:

- [Event Hubs Dedicated fiyatlandırması](https://azure.microsoft.com/pricing/details/event-hubs/). Event Hubs adanmış kapasite hakkında ek ayrıntıları almak için Microsoft satış temsilcinize veya Microsoft Support de başvurabilirsiniz.
- [Event Hubs SSS Sayfasındaki](event-hubs-faq.md) fiyatlandırma bilgileri içeriyor ve Event Hubs hakkında sık sorulan bazı sorular yanıtlanmaktadır.
