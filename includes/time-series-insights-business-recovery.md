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
ms.openlocfilehash: e3e7044148e262e6977ae3be96f7cb61218114a3
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66388733"
---
## <a name="business-disaster-recovery"></a>İş olağanüstü durum kurtarma

Bu bölümde, uygulama ve bir olağanüstü durum oluştuğunda dahi çalışan hizmetleri tutan Azure Time Series Insights özelliklerini açıklar. Bu kavram, iş olağanüstü durum kurtarma bilinir.

### <a name="high-availability"></a>Yüksek kullanılabilirlik

Bir Azure hizmeti olduğundan, zaman serisi görüşleri Azure bölgesi düzeyinde fazlalıkları kullanarak bazı yüksek kullanılabilirlik özellikleri sağlar. Örneğin, Microsoft Azure, Azure'nın bölgeler arası kullanılabilirlik özelliği aracılığıyla olağanüstü durum kurtarma olanaklarını destekler.

Azure ve, sağlanan ek yüksek kullanılabilirlik özellikleri, herhangi bir zaman serisi görüşleri örneğine de mevcuttur, içerir:

* **Yük devretme**: Azure sağlar [coğrafi çoğaltma ve Yük Dengeleme](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region).
* **Veri geri yüklemeleri** ve **depolama kurtarma**: Azure sağlar [korumak ve verileri kurtarmak için çeşitli seçenekler](https://docs.microsoft.com/azure/architecture/resiliency/recovery-data-corruption).
* **Site recovery**: Aracılığıyla kullanılabilir olan özelliklerin [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/).

Cihazlar ve kullanıcılar için küresel, bölgeler arası yüksek kullanılabilirlik sağlamak aşağıdaki Azure özelliklerini etkinleştirmek emin olun.

> [!NOTE]
> Azure, bölgeler arası kullanılabilirlik etkinleştirmek için yapılandırılmışsa, Azure Time Series Insights içinde ek bölgeler arası kullanılabilirlik yapılandırma gerekmiyor.

### <a name="iot-and-event-hubs"></a>IOT ve olay hub'ları

Bazı Azure IOT hizmetleri de yerleşik iş olağanüstü durum kurtarma özellikleri içerir:

* [Azure IOT hub'ı yüksek kullanılabilirlik, olağanüstü durum kurtarma](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr), bölge içi yedeklilik içerir.
* [Azure Event Hubs ilkeleri](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr).
* [Azure depolama yedekliliği](https://docs.microsoft.com/azure/storage/common/storage-redundancy).

Time Series Insights diğer hizmetlerle tümleştirme ek olağanüstü durum kurtarma fırsat sağlar. Örneğin, olay hub'ına gönderilen telemetri Azure Blob Depolama veritabanı yedek kalıcı.

### <a name="time-series-insights"></a>Time Series Insights

Time Series Insights verileriniz, uygulamalarınız ve bunlar kesintiye bile çalışan hizmetleri korumak için birkaç yolu vardır. Azure Time Series ortamınızı tam bir yedekleme kopyasını gerekli olduğunu belirlemek de:

* Verileri yeniden yönlendirme ve trafik için bir zaman serisi öngörüleri özgü yük devretme örneği.
* Denetim ve veri koruma amacıyla.

Genel olarak, zaman serisi görüşleri ortamına çoğaltmak için en iyi yolu ikinci zaman serisi görüşleri ortamına bir yedekten Azure bölgesi oluşturmaktır. Olayları, ayrıca ikincil bu ortama birincil olay kaynağınızdan gönderilir. İkinci ayrılmış bir tüketici grubu kullanın ve daha önce sağlanan bu kaynağın iş olağanüstü durum kurtarma, faydalandığından emin olun.

Özellikle, yinelenen bir ortam oluşturmak için:

1. İkinci bir bölgede bir ortam oluşturun. Yönergeler için [Azure portalında yeni bir zaman serisi görüşleri ortamı oluşturma](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started).
1. Olay kaynağınız için ikinci bir adanmış bir tüketici grubu oluşturun.
1. Bu olay kaynak yeni ortama bağlanın. İkinci adanmış bir tüketici grubu tanımlamak emin olun.
1. Time Series Insights gözden [IOT hub'ı](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub) ve [olay hub'ı](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-data-access) belgeleri.

Son olarak:

1. Bir olağanüstü durum olayı sırasında birincil bölge etkileniyorsa, yedekleme zaman serisi görüşleri ortamına işlemleri yeniden yönlendir.
1. Yedekleme ve telemetri ve sorgu zaman serisi öngörüleri verilerini kurtarmak için ikinci bir bölge kullanın.

> [!IMPORTANT]
> * Not gecikme yük devretmesi durumunda karşılaşmış.
> * Yük devretme işlemleri yönlendirilir gibi iletisi işlenirken içeriklerinin anlık bir depo da neden olabilir.
> * Daha fazla bilgi için [zaman serisi görüşleri'nde gecikme süresini azaltmak](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-environment-mitigate-latency).
