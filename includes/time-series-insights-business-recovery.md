---
ms.topic: include
ms.service: time-series-insights
author: kingdomofends
ms.author: adgera
ms.date: 04/29/2019
ms.openlocfilehash: 8a3c630b54ff95a9b1200e2421c787a514a0aa52
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66431055"
---
## <a name="business-disaster-recovery"></a>İş olağanüstü durum kurtarma

Bu bölümde bir olağanüstü durum oluştuğunda dahi, uygulamaları ve Hizmetleri çalışır durumda tutmak Azure Time Series Insights özelliklerini açıklar (olarak bilinen *iş olağanüstü durum kurtarma*).

### <a name="high-availability"></a>Yüksek kullanılabilirlik

Time Series Insights belirli bir Azure hizmet olarak sağlar *yüksek kullanılabilirlik* açarken Azure bölgesi düzeyinde kullanarak özellikleri. Örneğin, Azure, Azure'nın aracılığıyla olağanüstü durum kurtarma olanaklarını destekler *bölgeler arası kullanılabilirlik* özelliği.

Azure'ı (ve herhangi bir zaman serisi görüşleri örneğine de kullanılabilir) aracılığıyla sağlanan ek yüksek kullanılabilirlik özellikleri şunlardır:

- **Yük devretme**: Azure sağlar [coğrafi çoğaltma ve Yük Dengeleme](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region).
- **Veri geri yüklemeleri** ve **depolama kurtarma**: Azure sağlar [korumak ve verileri kurtarmak için çeşitli seçenekler](https://docs.microsoft.com/azure/architecture/resiliency/recovery-data-corruption).
- **Site recovery**: Azure site kurtarma özellikleri aracılığıyla sağlar [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/).

Cihazlar ve kullanıcılar için küresel, bölgeler arası yüksek kullanılabilirlik sağlamak ilgili Azure özellikleri etkinleştirdiğinizden emin olun.

> [!NOTE]
> Azure, bölgeler arası kullanılabilirlik etkinleştirmek için yapılandırılmışsa, hiçbir ek bölgeler arası kullanılabilirlik yapılandırması Azure zaman serisi Öngörülerinde gereklidir.

### <a name="iot-and-event-hubs"></a>IOT ve olay hub'ları

Bazı Azure IOT hizmetleri de yerleşik iş olağanüstü durum kurtarma özellikleri içerir:

- [IOT hub'ı yüksek kullanılabilirlik, olağanüstü durum kurtarma](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr), bölge içi yedeklilik dahil olmak üzere
- [Olay hub'ı ilkeleri](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr)
- [Azure depolama yedekliliği](https://docs.microsoft.com/azure/storage/common/storage-redundancy)

Time Series Insights diğer hizmetlerle tümleştirme ek olağanüstü durum kurtarma fırsat sağlar. Örneğin, olay hub'ına gönderilen telemetri Azure Blob Depolama veritabanı yedek kalıcı.

### <a name="time-series-insights"></a>Time Series Insights

Bunlar kesintiye olsa bile, zaman serisi öngörüleri verilerini, uygulamaları ve hizmetleri çalıştırmak, tutmak için birkaç yolu vardır. 

Ancak, ortamınıza Azure Time Series tam bir yedekleme kopyasını ayrıca aşağıdaki amaçlar için gerekli olduğunu belirlemek:

- Olarak bir *yük devretme örneği* Time Series Insights'ı, verileri yeniden yönlendirme ve trafik için özel olarak
- Veri ve denetim bilgilerini korumak için

Genel olarak, zaman serisi görüşleri ortamına çoğaltmak için en iyi yolu ikinci zaman serisi görüşleri ortamına bir yedekten Azure bölgesi oluşturmaktır. Olayları, ayrıca ikincil bu ortama birincil olay kaynağınızdan gönderilir. İkinci ve adanmış bir tüketici grubu kullandığınızdan emin olun. Bu kaynağın iş olağanüstü durum kurtarma Kılavuzu, daha önce açıklandığı gibi izleyin.

Yinelenen bir ortam oluşturmak için:

1. İkinci bir bölgede bir ortam oluşturun. Daha fazla bilgi için [Azure portalında yeni bir zaman serisi görüşleri ortamı oluşturma](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started).
1. Olay kaynağınız için ikinci bir adanmış bir tüketici grubu oluşturun.
1. Bu olay kaynak yeni ortama bağlanın. İkinci ve adanmış bir tüketici grubu belirlediğiniz emin olun.
1. Time Series Insights gözden [IOT hub'ı](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub) ve [Event Hubs](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-data-access) belgeleri.

Bir olay oluşursa:

1. Bir olağanüstü durum olayı sırasında birincil bölge etkileniyorsa, yedekleme zaman serisi görüşleri ortamına işlemleri yeniden yönlendir.
1. Yedekleme ve telemetri ve sorgu zaman serisi öngörüleri verilerini kurtarmak için ikinci bir bölge kullanın.

> [!IMPORTANT]
> Bir yük devretme gerçekleşirse:
> 
> * Bir gecikme de oluşabilir.
> * İleti işleme içeriklerinin anlık bir ani değişiklik gerçekleşebilir, bu gibi işlemleri yönlendirilir.
> 
> Daha fazla bilgi için [zaman serisi görüşleri'nde gecikme süresini azaltmak](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-environment-mitigate-latency).

