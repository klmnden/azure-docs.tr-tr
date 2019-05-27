---
title: include dosyası
description: include dosyası
ms.topic: include
ms.custom: include file
services: time-series-insights
ms.service: time-series-insights
author: kingdomofends
ms.author: adgera
ms.date: 04/29/2019
ms.openlocfilehash: e87a82e985ed1d1794f9da00546f167ef01e1779
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66147446"
---
## <a name="business-disaster-recovery"></a>İş olağanüstü durum kurtarma

Bu bölümde, uygulama ve bir olağanüstü durum oluştuğunda dahi çalışan hizmetleri tutan Azure Time Series Insights özelliklerini açıklar (**iş olağanüstü durum kurtarma**).

### <a name="high-availability"></a>Yüksek oranda kullanılabilirlik

Time Series Insights belirli bir Azure hizmet olarak sağlar **yüksek kullanılabilirlik** açarken Azure bölgesi düzeyinde kullanarak özellikleri. Örneğin, Microsoft Azure aracılığıyla Azure'nın olağanüstü durum kurtarma olanaklarını destekler **bölgeler arası kullanılabilirlik** özelliği.

Azure'ı (ve herhangi bir zaman serisi görüşleri örneğine de kullanılabilir) aracılığıyla sağlanan ek yüksek kullanılabilirlik özellikleri şunlardır:

1. **Yük devretme**: Azure sağlar [coğrafi çoğaltma ve Dengeleme yüklenirken](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region).
1. **Veri geri yüklemeleri** ve **depolama kurtarma**: Azure sağlar [korumak ve verileri kurtarmak için çeşitli seçenekler](https://docs.microsoft.com/azure/architecture/resiliency/recovery-data-corruption).
1. **Site Recovery** aracılığıyla özellikleri [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/).

Kullanıcılar ve cihazlar için genel, bölgeler arası, yüksek kullanılabilirlik sağlamak aşağıdaki Azure özelliklerini etkinleştirmek emin olun.

> [!NOTE]
> Azure etkinleştirmek için yapılandırılmışsa **bölgeler arası kullanılabilirlik**, Azure Time Series Insights içinde ek bölgeler arası kullanılabilirlik yapılandırma gerekmiyor.

### <a name="iot-and-event-hubs"></a>IOT ve olay hub'ları

Bazı Azure IOT hizmetleri de yerleşik iş olağanüstü durum kurtarma özellikleri içerir:

1. [IOT hub'ı yüksek kullanılabilirlik, olağanüstü durum kurtarma](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr) bölge içi yedeklilik dahil olmak üzere.
1. [Olay hub'ı İlkesi](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr).
1. [Azure depolama yedekliliği](https://docs.microsoft.com/azure/storage/common/storage-redundancy).

Time Series Insights diğer hizmetlerle tümleştirme ek olağanüstü durum kurtarma fırsat sağlar. Örneğin, olay Hub'ına gönderilen telemetri Azure Blob Depolama veritabanı yedek kalıcı.

### <a name="time-series-insights"></a>Time Series Insights

Time Series Insights verileriniz, uygulamalarınız ve bunlar kesintiye bile çalışan hizmetleri korumak için birkaç yolu vardır. Azure Time Series ortamınızı tam, yinelenen, yedek bir kopyasını gerekli olduğunu belirlemek de:

1. Bir Time Series Insights özel olarak **yük devretme örneği** verileri yeniden yönlendirme ve trafik için.
1. Denetim ve veri koruma amacıyla.

Genel olarak, zaman serisi görüşleri ortamına çoğaltmak için en iyi yolu ikinci zaman serisi görüşleri ortamına bir yedekten Azure bölgesi oluşturmaktır. Olayları, ayrıca ikincil bu ortama birincil olay kaynağınızdan gönderilir. İkinci, adanmış, tüketici grubu kullanın ve bu kaynağın iş olağanüstü durum kurtarma Kılavuzu (yukarıda sağlanan) izlemek için emin olun.

Özellikle, yinelenen bir ortam oluşturmak için:

1. İkinci bir bölgede bir ortam oluşturun ([Azure portalında yeni bir zaman serisi görüşleri ortamı oluşturma](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started)).
1. Olay kaynağınız için ikinci bir adanmış bir tüketici grubu oluşturun.
1. Bu olay kaynağı, ikinci ve adanmış bir tüketici grubu tanımlamak sağlamaktan yeni ortama bağlanın.
1. Time Series Insights gözden [IOT hub'ı](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub) ve [olay hub'ı](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-data-access) belgeleri.

Son olarak:

* Bir olağanüstü durum olayı sırasında birincil bölge etkileniyorsa, yedekleme zaman serisi görüşleri ortamına işlemleri yeniden yönlendir.
* Yedekleme ve telemetri ve sorgu zaman serisi öngörüleri verilerini kurtarmak için ikinci bir bölge kullanın.

> [!IMPORTANT]
> * Not gecikme yük devretmesi durumunda karşılaşmış.
> * Yük devretme işlemleri yönlendirilir gibi iletisi işlenirken içeriklerinin anlık bir depo da neden olabilir.
> * Daha fazla bilgi için [zaman serisi görüşleri'nde gecikme süresini azaltmak](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-environment-mitigate-latency).