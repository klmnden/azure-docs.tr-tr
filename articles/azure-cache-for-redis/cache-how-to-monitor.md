---
title: Azure önbelleği için Redis izleme | Microsoft Docs
description: Azure önbelleği için Redis örneği durumunu ve performansını izleme hakkında bilgi edinin
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: 7e70b153-9c87-4290-85af-2228f31df118
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: yegu
ms.openlocfilehash: 32d0fb2ba17d322c0a273ebaf0a21d2b3ca0668f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60830696"
---
# <a name="how-to-monitor-azure-cache-for-redis"></a>Azure önbelleği için Redis izleme
Azure önbelleği için Redis kullandığı [Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) önbelleği örneklerinizin izlemek için çeşitli seçenekler sağlayacak. Ölçümleri görüntüleyin, ölçüm grafikleri başlangıç panosuna sabitlemek, tarih ve saat aralığı grafikleri izleme özelleştirme, ekleyin ve ölçümleri grafikten kaldırabileceğiniz ve belirli koşullar karşılandığında uyarılar ayarlayın. Bu araçlar, Azure önbelleği için Redis örneği ve önbelleğe alma uygulamalarınızı yönetmenize yardımcı durumunu izlemenize olanak tanır.

Azure önbelleği için Redis örneği için toplanan ölçümler Redis kullanarak [bilgisi](https://redis.io/commands/info) 30 gün boyunca dakika ve otomatik olarak depolanan başına yaklaşık iki kez komut (bkz [dışarı önbellek ölçümleri](#export-cache-metrics) yapılandırmak için bir farklı bekletme ilkesi) için ölçümler grafiklerde görüntülenen edilebilmeleri ve uyarı kuralları tarafından değerlendirilen. Her önbellek ölçümü için kullanılan farklı bilgisi değerleri hakkında daha fazla bilgi için bkz: [kullanılabilir Ölçümler ve raporlama aralıkları](#available-metrics-and-reporting-intervals).

<a name="view-cache-metrics"></a>

Önbellek ölçümleri görüntülemek için [Gözat](cache-configure.md#configure-azure-cache-for-redis-settings) cache Örneğinize [Azure portalında](https://portal.azure.com).  Azure önbelleği için Redis üzerinde bazı yerleşik grafikler sağlar **genel bakış** dikey penceresinde ve **Redis ölçümleri** dikey penceresi. Her grafiğin ekleme veya kaldırma Ölçümler ve raporlama aralığını değiştirmeyi özelleştirilebilir.

![Redis ölçümleri](./media/cache-how-to-monitor/redis-cache-redis-metrics-blade.png)

## <a name="view-pre-configured-metrics-charts"></a>Önceden yapılandırılmış ölçüm grafikleri görüntüleme

**Genel bakış** dikey penceresinde bulunur aşağıdaki önceden yapılandırılmış izleme grafikleri.

* [İzleme grafiklerini](#monitoring-charts)
* [Kullanım grafikleri](#usage-charts)

### <a name="monitoring-charts"></a>İzleme grafiklerini
**İzleme** konusundaki **genel bakış** dikey penceresinde bulunur **denk gelir ve isabetsiz okumaları**, **alır ve ayarlar**, **bağlantıları**, ve **toplam komutları** grafikler.

![İzleme grafiklerini](./media/cache-how-to-monitor/redis-cache-monitoring-part.png)

### <a name="usage-charts"></a>Kullanım grafikleri
**Kullanım** konusundaki **genel bakış** dikey penceresinde bulunur **Redis sunucu yükü**, **bellek kullanımı**, **ağ bant genişliği**, ve **CPU kullanımı** grafikleri ve ayrıca görüntüler **fiyatlandırma katmanı** önbellek örneği için.

![Kullanım grafikleri](./media/cache-how-to-monitor/redis-cache-usage-part.png)

**Fiyatlandırma katmanı** önbellek fiyatlandırma katmanı ve yapmak için kullanılabilir görüntüler [ölçek](cache-how-to-scale.md) önbelleğe farklı bir fiyatlandırma katmanı.

## <a name="view-metrics-with-azure-monitor"></a>Azure İzleyici ile ölçümlerini görüntüleme
Azure İzleyicisi'ni kullanarak özel grafikler oluşturma ve Redis ölçümleri görüntülemek için tıklatın **ölçümleri** gelen **kaynak menüsünde**ve aralığı, grafik türü ve diğer raporlama istediğiniz ölçümleri kullanarak grafiğinizi özelleştirin.

![Redis ölçümleri](./media/cache-how-to-monitor/redis-cache-monitor.png)

Azure İzleyicisi'ni kullanarak ölçümleri ile çalışma hakkında daha fazla bilgi için bkz. [Microsoft azure'da ölçümlere genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

<a name="how-to-view-metrics-and-customize-chart"></a>
<a name="enable-cache-diagnostics"></a>
## <a name="export-cache-metrics"></a>Önbellek ölçümleri dışarı aktarma
Varsayılan olarak, Azure İzleyici'de önbellek ölçümleridir [30 gün saklanan](../azure-monitor/platform/data-platform-metrics.md) ve ardından silinir. Kalıcı önbellek ölçümlerinizi 30 günden daha uzun bir süre için [bir depolama hesabı atamak](../azure-monitor/platform/archive-diagnostic-logs.md) belirtin bir **bekletme (gün)** önbellek ölçümlerinizi ilkesi. 

Önbellek ölçümlerinizi için bir depolama hesabı yapılandırmak için:

1. Tıklayın **tanılama** gelen **kaynak menüsünde** içinde **Azure önbelleği için Redis** dikey penceresi.
2. Tıklayın **üzerinde**.
3. Denetleme **bir depolama hesabında arşivle**.
4. Önbellek ölçümlerini depolanacağı depolama hesabı seçin.
5. Denetleme **1 dakika** onay kutusunu belirtin bir **bekletme (gün)** ilkesi. İstemiyorsanız, herhangi bir bekletme ilkesi uygulamak ve verileri süresiz olarak bekletmek ayarlayın **bekletme (gün)** için **0**.
6. **Kaydet**’e tıklayın.

![Tanılama redis](./media/cache-how-to-monitor/redis-cache-diagnostics.png)

>[!NOTE]
>Depolama için önbellek ölçümlerinizi arşivleme yanı sıra, ayrıca [bunları bir olay hub'ına akış veya Azure İzleyici günlüklerine şirketlerde](../azure-monitor/platform/rest-api-walkthrough.md#retrieve-metric-values).
>
>

Ölçümlerinizin erişmek için daha önce bu makalede açıklanan Azure portalında görüntüleyebilir ve bunları da erişebilirsiniz kullanarak [Azure İzleyici ölçümleri REST API](../azure-monitor/platform/stream-monitoring-data-event-hubs.md).

> [!NOTE]
> Depolama hesaplarını değiştirirseniz, önceden yapılandırılmış depolama hesabındaki verileri indirme için kullanılabilir kalır, ancak Azure portalında görüntülenmez.  
> 
> 

## <a name="available-metrics-and-reporting-intervals"></a>Kullanılabilir Ölçümler ve raporlama aralıkları
Önbellek ölçümlerini dahil olmak üzere çeşitli raporlama aralıkları kullanarak rapor **saati aşan**, **Bugün**, **geçen hafta**, ve **özel**. **Ölçüm** her bir ölçüm grafiği dikey grafikteki her ölçümün ortalama, minimum ve maksimum değerleri görüntüler ve bazı ölçümler için raporlama aralığı toplam görüntüleme. 

Her ölçüm iki sürümünü içerir. Bir ölçüm ölçer performans kullanan önbellekler yanı sıra, tüm önbelleği [Kümeleme](cache-how-to-premium-clustering.md), ikinci bir sürümü içeren ölçüm, `(Shard 0-9)` tek parça bir önbellekte adı ölçüler performans. Örneğin, bir önbellek 4 parçaya sahipse `Cache Hits` tüm önbelleği isabetli okuma sayısının toplam tutar ve `Cache Hits (Shard 3)` yalnızca İsabeti için bu önbelleğin parça.

> [!NOTE]
> Önbellek ile hiçbir bağlı etkin istemci uygulamaları boşta olduğunda bile, bağlı istemciler, bellek kullanımı ve gerçekleştirilen işlemleri gibi bazı önbellek etkinliği görebilirsiniz. Bu etkinlik, bir Azure Cache, Redis örneği için işlemi sırasında normaldir.
> 
> 

| Ölçüm | Açıklama |
| --- | --- |
| İsabetli Önbellek Okuma Sayısı |Belirtilen raporlama aralığı sırasında anahtar başarılı arama sayısı. Bu eşlendiği `keyspace_hits` redis [bilgisi](https://redis.io/commands/info) komutu. |
| Önbellek gecikme süresi (Önizleme) | Gecikme süresi önbellek, önbellek düğümler arası gecikme süresini kapalı göre hesaplanır. Bu ölçüm, mikro saniye cinsinden ölçülür ve üç boyutu vardır: "Ortalama", "Min" ve "ortalama, minimum ve maksimum gecikme süresini önbelleği belirtilen zaman aralığı boyunca sırasıyla temsil eden Max". |
| İsabetsiz Önbellek Okuma Sayısı |Belirtilen raporlama aralığı sırasında başarısız anahtar arama sayısı. Bu eşlendiği `keyspace_misses` Redis bilgisi komutu. İsabetsiz önbellek okuma sayısı, önbellek ile ilgili bir sorun var. olmak zorunda gelmez. Örneğin, programlama edilgen önbellek düzeni kullanılırken, bir uygulama önbellekteki bir öğenin ilk arar. Öğesi (önbellek isabetsizliği) yoksa, öğe veritabanından alınan ve bir sonraki seferde önbelleğe eklenir. İsabetsiz önbellek okuma sayısı programlama edilgen önbellek düzeni için normal davranış ' dir. Önbellek kaçaklarının sayısı beklenenden yüksekse uygulama mantığı doldurur ve önbellekten okur inceleyin. Öğeleri çıkarıldığına bellek baskısı nedeniyle önbellekten sonra bazı önbellek isabetsizliği olabilir, ancak bellek Basıncı izlemek için daha iyi bir ölçüm olacaktır `Used Memory` veya `Evicted Keys`. |
| Önbellek Okuması |Önbelleğin megabayt (MB/sn) belirtilen raporlama aralığı sırasında saniye başına veri miktarını okuyabilir. Bu değer, önbellek barındıran ve Redis belirli değil sanal makineyi destekleyen ağ arabirim kartlarından elde edilir. **Bu değer bu önbelleği tarafından kullanılan ağ bant genişliği karşılık gelir. Uyarılar için sunucu tarafı ağ bant genişliği sınırlarını ayarlamak istiyorsanız, bunu kullanarak oluşturma `Cache Read` sayacı. Bkz: [bu tabloda](cache-faq.md#cache-performance) gözlemlenen bant genişliği sınırlarını çeşitli önbellek fiyatlandırma katmanları ve boyutları için.** |
| Önbellek Yazması |Önbelleğin megabayt (MB/sn) belirtilen sırasında saniye başına yazılan veri miktarı aralığı raporlama. Bu değer, önbellek barındıran ve Redis belirli değil sanal makineyi destekleyen ağ arabirim kartlarından elde edilir. Bu değer, ağ bant genişliğini önbelleğe istemciden gönderilen veri karşılık gelir. |
| Bağlı İstemciler |Belirtilen raporlama aralığı boyunca önbelleğe istemci bağlantılarının sayısı. Bu eşlendiği `connected_clients` Redis bilgisi komutu. Bir kez [bağlantı sınırı](cache-configure.md#default-redis-server-configuration) ulaşıldığında sonraki bağlantı denemelerinde önbellek için başarısız olan. Hiçbir etkin istemci uygulaması olsa bile, yine de olabilir bağlı istemciler dahili süreçleri ve bağlantıları nedeniyle birkaç örneklerini unutmayın. |
| CPU |CPU kullanım yüzdesini Azure önbelleği için Redis sunucusu bir yüzdesi olarak belirtilen raporlama aralığı sırasında. Bu değer işletim sistemine eşler `\Processor(_Total)\% Processor Time` performans sayacı. |
| Hatalar | Belirli hataları ve performans sorunları, önbelleğin bir belirtilen zaman aralığı boyunca yaşıyor olabilir. Bu ölçüm, sekiz boyutları farklı hata türleri temsil eden sahiptir, ancak daha sonra eklediğiniz. Şu anda temsil hata türleri aşağıdaki gibidir: <br/><ul><li>**Yük devretme** – bir önbellek yük devrettiğinde (ikincil asıl teşvik eder)</li><li>**Kilitlenme** – önbellek beklenmedik bir şekilde düğümlerden birini kilitlendiğinde</li><li>**Dataloss** – önbellekte dataloss olduğunda</li><li>**UnresponsiveClients** – istemcilerin veri sunucudan yeterince hızlı değil okurken</li><li>**AOF** – AOF Kalıcılık için ilgili bir sorun oluştuğunda</li><li>**RDB** – RDB Kalıcılık için ilgili bir sorun oluştuğunda</li><li>**İçeri aktarma** – içeri aktarma RDB için ilgili bir sorun oluştuğunda</li><li>**Dışarı aktarma** – RDB dışarı aktarma için ilgili bir sorun oluştuğunda</li></ul> |
| Çıkarılan Anahtarlar |Öğe için belirtilen raporlama aralığı sırasında önbellekten çıkarılmasına `maxmemory` sınırı. Bu eşlendiği `evicted_keys` Redis bilgisi komutu. |
| Süresi Dolan Anahtarlar |Öğe sayısı, belirtilen zaman aralığı boyunca önbellekten süresi doldu. Bu değer eşlendiği `expired_keys` Redis bilgisi komutu.|
| Alınanlar |Belirtilen raporlama aralığı sırasında önbellekten get işlemleri sayısı. Toplam aşağıdaki tüm komut Redis bilgisi değerleri değerdir: `cmdstat_get`, `cmdstat_hget`, `cmdstat_hgetall`, `cmdstat_hmget`, `cmdstat_mget`, `cmdstat_getbit`, ve `cmdstat_getrange`ve Önbelleği İsabetli ve isabetsiz toplamına eşittir Raporlama zaman aralığı boyunca. |
| Saniye başına işlem | Komutlar belirtilen zaman aralığı boyunca önbellek sunucu tarafından saniyede işlenen toplam sayısı.  Bu değer, Redis bilgisi komuttan "instantaneous_ops_per_sec için" eşler. |
| Redis Sunucu Yükü |Döngüleri Redis sunucu işleme ve iletileri için boşta beklenmiyor meşgul olduğu yüzdesi. Bu sayaç, Redis sunucusunun performans üst sınıra ulaşıp ulaşmadığını ve CPU işleme koyamıyoruz geldiğini 100 ulaşırsa herhangi daha hızlı çalışır. Yüksek Redis sunucu yükü görüyorsanız, istemci zaman aşımı özel durumları görürsünüz. Bu durumda artırma veya birden çok önbelleklerine verilerinizi bölümlemeyi göz önünde bulundurmalısınız. |
| Kümeler |Belirtilen raporlama aralığı boyunca önbelleğe ayarlama işlemleri sayısı. Toplam aşağıdaki tüm komut Redis bilgisi değerleri değerdir: `cmdstat_set`, `cmdstat_hset`, `cmdstat_hmset`, `cmdstat_hsetnx`, `cmdstat_lset`, `cmdstat_mset`, `cmdstat_msetnx`, `cmdstat_setbit`, `cmdstat_setex`, `cmdstat_setrange` , ve `cmdstat_setnx`. |
| Toplam Anahtar Sayısı  | Son raporlama süre boyunca önbellekteki anahtarlar maksimum sayısı. Bu eşlendiği `keyspace` Redis bilgisi komutu. Etkin, kümeleme ile önbellekler için temel alınan ölçümleri sistemi ile ilgili bir sınırlama nedeniyle Raporlama zaman aralığı boyunca maksimum anahtar sayısı olan parça anahtarlarının sayısı toplam anahtarlarını döndürür.  |
| İşlemler Toplamı |Komutlar belirtilen zaman aralığı boyunca önbellek sunucu tarafından işlenen toplam sayısı. Bu değer eşlendiği `total_commands_processed` Redis bilgisi komutu. Azure önbelleği için Redis tamamen pub/sub için kullanıldığında olacağını ölçüm için Not `Cache Hits`, `Cache Misses`, `Gets`, veya `Sets`, olacaktır ancak `Total Operations` yansıtan pub/sub işlemleri için önbellek kullanım ölçümleri. |
| Kullanılan Bellek |Önbellek anahtar/değer çiftleri MB önbelleğinde belirtilen zaman aralığı boyunca kullanılan bellek miktarı. Bu değer eşlendiği `used_memory` Redis bilgisi komutu. Bu meta veriler ya da parçalanma içermez. |
| Kullanılan bellek yüzdesi | Belirtilen zaman aralığı boyunca kullanılan toplam bellek %.  Bu bilgileri Redis komutu yüzdesini hesaplamak için "used_memory" değeri başvuruyor. |
| Kullanılan bellek RSS |Önbellek parçalanma ve meta verileri de dahil olmak üzere belirtilen Raporlama zaman aralığı boyunca, MB olarak kullanılan bellek miktarı. Bu değer eşlendiği `used_memory_rss` Redis bilgisi komutu. |

<a name="operations-and-alerts"></a>
## <a name="alerts"></a>Uyarılar
Ölçümlere ve etkinlik günlüklerine göre uyarı alacak şekilde yapılandırmanızı ayarlayabilirsiniz. Azure İzleyici, tetiklendiğinde aşağıdaki işlemleri yapmak üzere bir uyarı yapılandırmanıza olanak tanır:

* E-posta bildirimi gönderme
* Web kancası çağırma
* Bir Azure Mantıksal Uygulamasını çağırma

Önbelleğinizi uyarı kurallarını yapılandırmak için tıklayın **uyarı kuralları** gelen **kaynak menüsünde**.

![İzleme](./media/cache-how-to-monitor/redis-cache-monitoring.png)

Yapılandırma ve uyarılar kullanma hakkında daha fazla bilgi için bkz. [genel bakış, uyarı](../monitoring-and-diagnostics/insights-alerts-portal.md).

## <a name="activity-logs"></a>Etkinlik Günlükleri
Etkinlik günlükleri, Azure önbelleği için Redis örneği üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Daha önce "denetim günlüklerini" veya "işlem günlüklerinde" olarak biliniyordu. Etkinlik günlüklerini kullanarak, belirleyebilirsiniz "ne, kim ve ne zaman" işlemlerini (PUT, POST, DELETE), Azure önbelleği için Redis örneği üzerinde gerçekleştirilen herhangi bir yazma için. 

> [!NOTE]
> Etkinlik günlükleri, okuma (GET) işlemlerini içermez.
>
>

Önbellek hesabınız için etkinlik günlüklerini görüntülemek için tıklayın **etkinlik günlüklerini** gelen **kaynak menüsünde**.

Etkinlik günlükleri hakkında daha fazla bilgi için bkz. [Azure etkinlik günlüğü'ne genel bakış](../azure-monitor/platform/activity-logs-overview.md).











