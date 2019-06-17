---
title: Azure Event Grid coğrafi olağanüstü durum kurtarma | Microsoft Docs
description: Nasıl Azure Event Grid coğrafi olağanüstü durum kurtarma (GeoDR) otomatik olarak desteklediği açıklanmaktadır.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/24/2019
ms.author: spelluru
ms.openlocfilehash: 5b5c973a8daa8776efb0909092c569ea46902265
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66307325"
---
# <a name="server-side-geo-disaster-recovery-in-azure-event-grid"></a>Azure Event Grid, sunucu tarafı coğrafi olağanüstü durum kurtarma
Event Grid, artık meta verileri yalnızca için yeni, ancak tüm mevcut etki alanlarını, konuları ve olay abonelikleri otomatik coğrafi olağanüstü durum kurtarma (GeoDR) sahiptir. Azure bölgesinin tamamını kalırsa, Event Grid zaten eşleştirilmiş bir bölge için eşitlenen olay güvenlikle ilişkili altyapıyı meta verilerinizi sahip olur. Yeni olaylarınızı müdahale sizin tarafınızdan yeniden flow'u başlar. 

Olağanüstü durum kurtarma, iki ölçüm ile ölçülür.

- [Kurtarma noktası hedefi (RPO)](https://en.wikipedia.org/wiki/Disaster_recovery#Recovery_Point_Objective): dakika veya saat kaybolabilir veri.
- [Kurtarma süresi hedefi (RTO)](https://en.wikipedia.org/wiki/Disaster_recovery#Recovery_time_objective): aşağı dakika saatlik hizmeti olabilir.

Event Grid'ın otomatik yük devretme farklı RPO ve RTO için meta verilerinizi (olay abonelikleri vb.) ve verilerini (olaylar) ' var. Aşağıdaki sorguyu farklı belirtiminden gerekirse yine de kendi uygulayabileceğiniz [istemci-tarafı başarısız konuyu kullanarak üzerinden sistem durumu API'leri](custom-disaster-recovery.md).

## <a name="recovery-point-objective-rpo"></a>Kurtarma noktası hedefi (RPO)
- **Meta veri RPO**: sıfır dakika. Event grid'de bir kaynak oluşturulur zaman anında bölgeler arasında çoğaltılır. Bir yük devretme işlemi gerçekleştiğinde, meta veri kaybolur.
- **Data RPO**: Sistem iyi durumdaysa ve mevcut trafikten geri düşmediyse etkinlikler için RPO yaklaşık 5 dakikadır.

## <a name="recovery-time-objective-rto"></a>Kurtarma süresi hedefi (RTO)
- **Meta veri RTO**: Genellikle 60 dakika içinde çok daha hızlı bir şekilde gerçekleşir ancak Event Grid konuları ve abonelikleri için oluşturma/güncelleştirme/silme çağrılarını kabul başlar.
- **Veri RTO**: Meta veri gibi genellikle çok daha hızlı bir şekilde bölgesel yük devretme sonrasında yeni trafiği kabul 60 dakika içinde Event Grid ancak başlayacak ortaya çıkar.

> [!NOTE]
> Meta verileri Event Grid hakkında GeoDR maliyeti: 0 ABD Doları.


## <a name="next-steps"></a>Sonraki adımlar
Kendi istemci tarafı yük devretme mantığı kullanmak istiyorsanız bkz [# kendi olağanüstü durum kurtarma için Event Grid özel konulardaki oluşturun](custom-disaster-recovery.md)
