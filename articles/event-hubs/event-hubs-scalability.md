---
title: Ölçeklenebilirlik - Azure Event Hubs'a | Microsoft Docs
description: Azure Event Hubs'ı ölçeklendirme hakkında bilgi sağlar.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 06/18/2019
ms.author: shvija
ms.openlocfilehash: 3eb20013a6b3afaddce10f2e4652add0edf22a9a
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67276788"
---
# <a name="scaling-with-event-hubs"></a>Event Hubs ile ölçeklendirme

Event Hubs ile ölçeklendirme etkileyen iki faktörleri vardır.
*   İşleme birimleri
*   Bölümler

## <a name="throughput-units"></a>İşleme birimleri

Event Hubs işleme kapasitesi, *işleme birimleri* tarafından denetlenir. İşleme birimleri önceden satın alınan kapasite birimleridir. Tek bir aktarım hızı sağlar:

* Giriş: İkinci veya 1000 olaya (hangisi önce gerçekleşirse) saniye başına başına 1 MB'a kadar.
* Çıkış: İkinci veya 4096 olay / saniye başına 2 MB'a kadar.

Satın alınan işleme birimlerinin kapasitesi aşıldığında giriş azaltılır ve [ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) döndürülür. Çıkış, azaltma özel durumları oluşturmaz, ancak yine de satın alınan işleme birimlerinin kapasitesiyle sınırlıdır. Yayımlama hızı özel durumları alırsanız veya daha yüksek çıkış görmeyi bekliyorsanız ad alanı için kaç tane işleme birimi satın aldığınızı denetlediğinizden emin olun. Üretilen iş birimleri yönetebileceğiniz **ölçek** alanlarının dikey [Azure portalında](https://portal.azure.com). Üretilen iş birimleri program aracılığıyla kullanarak da yönetebilirsiniz [olay hub'ları API](event-hubs-api-overview.md).

İşleme birimleri önceden satın alınır ve saat başına faturalandırılır. Satın alındıktan sonra işleme birimleri en az bir saat için faturalandırılır. En fazla 20 işleme birimi bir Event Hubs ad alanı için satın alınabilir ve bu ad alanındaki tüm event hubs arasında paylaşılır.

**Otomatik şişme** özellik Event Hubs'ın otomatik olarak ölçeklendirilebilir üretilen iş birimi sayısını artırarak, kullanım karşılaması gerekir. Üretilen iş birimleri artırma engeller azaltma senaryoları, burada:

- Veri giriş hızlarını kümesi işleme birimleri en fazla.
- Veri çıkışı istek hızları kümesi işleme birimleri en fazla.

Tüm istekleri ServerBusy hatalarla başarısız olmadan en düşük eşikten yüksek yük artırdığı durumlarda Event Hubs hizmeti verimliliğini artırır. 

Hakkında daha fazla bilgi için otomatik şişme özelliği için bkz: [işleme birimlerini otomatik ölçeklendirme](event-hubs-auto-inflate.md).

## <a name="partitions"></a>Bölümler

Bölümler izin verin, Ölçek, aşağı akış işleme için. Event Hubs bölümlerle sunan bölümlenmiş tüketici modelinin nedeniyle, olaylarınızı aynı anda işlenirken ölçeklendirme. Bir olay hub'ı 32 adede kadar bölümlere sahip olabilir.

En iyi ölçeği elde etmek için 1:1 işleme birimleri ve bölümlerini dengelemeniz önerilir. Garantili bir giriş ve çıkış en fazla bir işleme biriminden oluşan tek bir bölüm vardır. Bir bölüme daha yüksek performans sağlamak olabilir, ancak performans garanti edilmez. Bir olay hub'ındaki bölüm sayısı en az üretilen iş birimlerinin sayısı için önerilir nedeni budur.

Toplam aktarım hızı gerektiren üzerinde planlama göz önünde bulundurulduğunda, ihtiyaç duyduğunuz üretilen iş birimlerinin sayısı ve en düşük bölüm sayısı, ancak kaç bölümler gerekir biliyor musunuz? Gelecekteki bir üretilen iş hacmi gereksinimlerinizi yanı sıra ulaşmak istediğiniz aşağı akış paralelliğiyle üzerinde göre bölüm seçin. Sahip olduğunuz bir olay hub'ı bölüm sayısı için ücret alınmaz.

Event Hubs ayrıntılı fiyatlandırma bilgileri için bkz. [Event Hubs fiyatlandırması](https://azure.microsoft.com/pricing/details/event-hubs/).

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

- [İşleme birimlerini otomatik ölçeklendirme](event-hubs-auto-inflate.md)
- [Event Hubs hizmetine genel bakış](event-hubs-what-is-event-hubs.md)
