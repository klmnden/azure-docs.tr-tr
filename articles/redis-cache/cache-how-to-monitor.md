---
title: "Azure Redis önbelleği izleme | Microsoft Docs"
description: "Nasıl sistem durumunu ve performansını Azure Redis önbelleği örneklerinizi izleyeceğinizi öğrenin"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 7e70b153-9c87-4290-85af-2228f31df118
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: sdanie
ms.openlocfilehash: 8996f5ce03e39557d9cc9c3de1ec214f5cd664b4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-monitor-azure-redis-cache"></a>Azure Redis Önbelleğini izleme
Azure Redis önbelleği kullanan [Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) önbellek örneklerinizi izleme için çeşitli seçenekler sağlamak için. Ölçümleri görüntülemek, ölçümler grafiklerde panosuna Sabitle PIN, izleme grafikleri, tarih ve saat aralığı özelleştirme, eklemek ve ölçümleri grafikten kaldırabileceğiniz ve belirli koşullar karşılandığında Uyarıları ayarlayın. Bu araçlar, Azure Redis önbelleği örnekleri durumunu izlemenize ve önbelleğe alma uygulamaları yönetmenize yardımcı olmak etkinleştirin.

Azure Redis önbelleği örnekleri için ölçümleri, Redis kullanılarak toplanır [bilgisi](http://redis.io/commands/info) 30 gün boyunca dakika ve otomatik olarak depolanan başına yaklaşık iki kez komutu (bkz [verme önbellek ölçümleri](#export-cache-metrics) farklı bir bekletme ilkesi yapılandırmak için) bunlar kullanılabilir ölçümler grafiklerde görüntülenir ve uyarı kuralları tarafından değerlendirilen şekilde. Her önbellek ölçümü için kullanılan farklı bilgi değerler hakkında daha fazla bilgi için bkz: [kullanılabilir Ölçümler ve aralıklarını raporlama](#available-metrics-and-reporting-intervals).

<a name="view-cache-metrics"></a>

Önbellek ölçümleri görüntülemek üzere [Gözat](cache-configure.md#configure-redis-cache-settings) önbelleği Örneğiniz için [Azure portal](https://portal.azure.com).  Azure Redis önbelleği hakkında bazı yerleşik grafiklerde sağlar **genel bakış** dikey ve **Redis ölçümleri** dikey. Her bir grafik ekleme veya kaldırma ölçümleri ve raporlama aralığı değiştirme özelleştirilebilir.

![Ölçümleri redis](./media/cache-how-to-monitor/redis-cache-redis-metrics-blade.png)

## <a name="view-pre-configured-metrics-charts"></a>Önceden yapılandırılmış ölçümleri grafikleri görüntüleme

**Genel bakış** dikey olan aşağıdaki izleme grafikleri önceden yapılandırılmış.

* [İzleme grafikleri](#monitoring-charts)
* [Kullanım grafikleri](#usage-charts)

### <a name="monitoring-charts"></a>İzleme grafikleri
**İzleme** bölümüne **genel bakış** dikey sahip **İsabetli ve isabetsiz okuma**, **alır ve ayarlar**, **bağlantıları**, ve **toplam komutları** grafik.

![İzleme grafikleri](./media/cache-how-to-monitor/redis-cache-monitoring-part.png)

### <a name="usage-charts"></a>Kullanım grafikleri
**Kullanım** bölümüne **genel bakış** dikey sahip **Redis sunucu iş yükü**, **bellek kullanımı**, **ağ bant genişliği**, ve **CPU kullanımı** grafikleri ve ayrıca görüntüler **fiyatlandırma katmanı** önbellek örneği için.

![Kullanım grafikleri](./media/cache-how-to-monitor/redis-cache-usage-part.png)

**Fiyatlandırma katmanı** önbellek fiyatlandırma katmanı ve için kullanılabilir görüntüler [ölçek](cache-how-to-scale.md) farklı bir fiyatlandırma katmanı önbelleğine.

## <a name="view-metrics-with-azure-monitor"></a>Azure İzleyicisi ile metrikleri görüntüleyin
Azure İzleyicisi'ni kullanarak özel grafikler oluşturma ve Redis ölçümleri görüntülemek için tıklatın **ölçümleri** gelen **kaynak menü**ve aralığı, grafik türü ve daha fazlasını raporlama istenen ölçümleri kullanarak grafiğinizi özelleştirmek.

![Ölçümleri redis](./media/cache-how-to-monitor/redis-cache-monitor.png)

Azure İzleyicisi'ni kullanarak ölçümleri ile çalışma hakkında daha fazla bilgi için bkz: [Microsoft Azure ölçümlerini genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

<a name="how-to-view-metrics-and-customize-chart"></a>
<a name="enable-cache-diagnostics"></a>
## <a name="export-cache-metrics"></a>Önbellek ölçümleri dışarı aktarma
Önbellek ölçümleridir Azure İzleyicisi'nde varsayılan olarak, [30 gün boyunca depolanan](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#store-and-archive) ve ardından silinir. 30 günden uzun, önbellek ölçümlerinizi kalıcı hale getirmek için şunları yapabilirsiniz [bir depolama hesabı atamak](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md) ve belirtin bir **bekletme (gün)** önbellek ölçümlerinizi için ilke. 

Önbellek ölçümlerinizi için bir depolama hesabı yapılandırmak için:

1. Tıklatın **tanılama** gelen **kaynak menü** içinde **Redis önbelleği** dikey.
2. Tıklatın **üzerinde**.
3. Denetleme **bir depolama hesabı arşive**.
4. Önbellek ölçümleri depolanacağı depolama hesabı seçin.
5. Denetleme **1 dakika** onay kutusunu ve belirtin bir **bekletme (gün)** ilkesi. Herhangi bir bekletme ilkesi uygulamak ve verileri sonsuza kadar tutmak, ayarlayın, istemiyorsanız, **bekletme (gün)** için **0**.
6. **Kaydet** düğmesine tıklayın.

![Tanılama redis](./media/cache-how-to-monitor/redis-cache-diagnostics.png)

>[!NOTE]
>Önbellek ölçümlerinizi depolama arşivleme ek olarak, şunları da yapabilirsiniz [olay hub'ına akış veya günlük analizi için Gönder](../monitoring-and-diagnostics/monitoring-overview-metrics.md#export-metrics).
>
>

Ölçümlerinizi erişmek için daha önce bu makalesinde açıklandığı gibi Azure Portalı'nda görüntüleyebilirsiniz ve ayrıca erişmesine kullanarak [Azure İzleyici ölçümleri REST API](../monitoring-and-diagnostics/monitoring-overview-metrics.md#access-metrics-via-the-rest-api).

> [!NOTE]
> Depolama hesaplarını değiştirirseniz, önceden yapılandırılmış depolama hesabındaki veri indirme için kullanılabilir kalır, ancak Azure portalında görüntülenmez.  
> 
> 

## <a name="available-metrics-and-reporting-intervals"></a>Kullanılabilir Ölçümler ve aralıklarını raporlama
Önbellek ölçümleri de dahil olmak üzere çeşitli raporlama aralıkları kullanarak raporlanır **saati aşan**, **Bugün**, **geçen hafta**, ve **özel**. **Ölçüm** her ölçümleri grafiği için dikey grafikte her ölçümü için ortalama, minimum ve maksimum değerleri görüntüler ve raporlama aralığı için bir toplama bazı ölçümleri görüntüler. 

Her ölçüm iki sürümlerini içerir. Bir ölçüm ölçer kullanmak önbellekleri ve tüm önbelleği için performans [Kümeleme](cache-how-to-premium-clustering.md), içeren ölçüm ikinci sürümü `(Shard 0-9)` bir önbellekte tek parça adı ölçümleri performans. Örneğin bir önbellek 4 parça varsa `Cache Hits` tüm önbelleği isabetli okuma sayısının toplam tutar ve `Cache Hits (Shard 3)` İsabeti yalnızca o parça önbelleği için.

> [!NOTE]
> Önbellek hiçbir bağlı etkin istemci uygulamalarıyla boş olsa bile, bağlanan istemciler, bellek kullanımı ve gerçekleştirilen işlemler gibi bazı önbellek etkinliği görebilirsiniz. Bu etkinlik bir Azure Redis önbelleği örneği işlem sırasında normaldir.
> 
> 

| Ölçüm | Açıklama |
| --- | --- |
| İsabetli Önbellek okuma sayısı |Belirtilen raporlama aralığı sırasında anahtar başarılı arama sayısı. Bu eşlendiği `keyspace_hits` Redis gelen [bilgisi](http://redis.io/commands/info) komutu. |
| İsabetsiz önbellek okuma sayısı |Belirtilen raporlama aralığı sırasında anahtar başarısız arama sayısı. Bu eşlendiği `keyspace_misses` Redis bilgisi komutu. Önbellek isabetsizliği önbellek ile ilgili bir sorun var. mutlaka anlamına gelmez. Örneğin, edilgen önbellek programlama düzeni kullanırken, bir uygulama bir öğe için önbelleğe ilk arar. Öğe (önbellek isabetsizliği) yoksa, öğe veritabanından alınan ve bir sonraki seferde önbelleğe eklenir. Önbellek isabetsizliği programlama edilgen önbellek düzeni için normal davranışın ' dir. Önbellek İsabetsizlik Sayısı beklenenden daha yüksek ise ve önbellekten okur doldurur uygulama mantığı inceleyin. Öğeleri çıkarılacak bellek baskısı nedeniyle önbelleğinden sonra bazı önbellek isabetsizlik olabilir, ancak bellek baskısı izlemek için daha iyi bir ölçüm olacaktır `Used Memory` veya `Evicted Keys`. |
| Bağlanan istemciler |Belirtilen raporlama aralığı sırasında önbelleğe istemci bağlantılarının sayısı. Bu eşlendiği `connected_clients` Redis bilgisi komutu. Bir kez [bağlantı sınırı](cache-configure.md#default-redis-server-configuration) önbellek ulaştı sonraki bağlantı girişimleri başarısız olduğundan. Hiçbir etkin istemci uygulaması olsa bile hala olabilir bağlı istemciler dahili işlemleri ve bağlantılar nedeniyle birkaç örneklerini unutmayın. |
| Çıkarılan anahtarları |Belirtilen raporlama aralığı nedeniyle sırasında önbellekten çıkarılmasına öğe sayısı `maxmemory` sınırı. Bu eşlendiği `evicted_keys` Redis bilgisi komutu. |
| Süresi dolan anahtarları |Öğe sayısı belirtilen zaman aralığı boyunca önbellekten süresi. Bu değer eşlendiği `expired_keys` Redis bilgisi komutu. |
| Toplam anahtarları  | Önbellek son raporlama süre boyunca anahtarlarında maksimum sayısı. Bu eşlendiği `keyspace` Redis bilgisi komutu. Etkin, kümeleme ile önbellekler için temel ölçümleri sisteminin bir sınırlama nedeniyle toplam anahtarlarını raporlama aralığı sayısı anahtarları vardı parça anahtarları en fazla sayısını döndürür.  |
| Alır |Belirtilen raporlama aralığı sırasında önbellekten alma işlemlerinin sayısı. Aşağıdaki toplamı değerleri Redis bilgisi tüm komutu bu değerdir: `cmdstat_get`, `cmdstat_hget`, `cmdstat_hgetall`, `cmdstat_hmget`, `cmdstat_mget`, `cmdstat_getbit`, ve `cmdstat_getrange`ve Raporlama zaman aralığı boyunca Önbelleği İsabetli ve isabetsiz toplamına eşittir. |
| Redis sunucu iş yükü |Redis sunucu işleme ve iletiler için boşta beklenmiyor meşgul olduğundan döngüleri yüzdesi. Bu sayaç Redis sunucu performans tavan erişti ve CPU işleyemiyor gelir 100 ulaşırsa herhangi daha hızlı çalışır. Ardından yüksek Redis sunucusu yükü görüyorsanız istemci zaman aşımı özel görürsünüz. Bu durumda ölçeklendirmeyi veya birden çok önbelleklerine verilerinizi bölümleme düşünmelisiniz. |
| Ayarlar |Belirtilen raporlama aralığı sırasında önbelleğe ayarlama işlemleri sayısı. Aşağıdaki toplamı değerleri Redis bilgisi tüm komutu bu değerdir: `cmdstat_set`, `cmdstat_hset`, `cmdstat_hmset`, `cmdstat_hsetnx`, `cmdstat_lset`, `cmdstat_mset`, `cmdstat_msetnx`, `cmdstat_setbit`, `cmdstat_setex`, `cmdstat_setrange`, ve `cmdstat_setnx`. |
| Toplam işlemler |Komutları belirtilen zaman aralığı boyunca önbellek sunucusu tarafından işlenen toplam sayısı. Bu değer eşlendiği `total_commands_processed` Redis bilgisi komutu. Azure Redis önbelleği tamamen pub/alt için kullanıldığında olacağı için hiçbir ölçümleri Not `Cache Hits`, `Cache Misses`, `Gets`, veya `Sets`, olacaktır, ancak `Total Operations` pub/alt işlemleri için önbellek kullanımı yansıtacak ölçümleri. |
| Kullanılan bellek |Önbellek anahtar/değer çiftlerini MB önbellek için belirtilen zaman aralığı boyunca kullanılan bellek miktarıdır. Bu değer eşlendiği `used_memory` Redis bilgisi komutu. Bu, meta veriler veya parçalanma içermez. |
| Kullanılan bellek RSS |Önbellek parçalanma ve meta verileri de dahil olmak üzere belirtilen Raporlama zaman aralığı boyunca, MB olarak kullanılan bellek miktarıdır. Bu değer eşlendiği `used_memory_rss` Redis bilgisi komutu. |
| CPU |CPU kullanımı Azure Redis önbelleği sunucusunun bir yüzdesi olarak belirtilen raporlama aralığı içinde. Bu değer işletim sistemine eşlemeleri `\Processor(_Total)\% Processor Time` performans sayacı. |
| Önbellek Okuma |Belirtilen raporlama aralığı sırasında (MB/sn) saniye başına megabayt önbellekten veri miktarını okuyun. Bu değer, önbellek barındıran ve Redis belirli değil sanal makineyi destekleyen ağ arabirim kartları türetilir. **Bu değer bu önbelleği tarafından kullanılan ağ bant genişliği karşılık gelir. Sunucu tarafı ağ bant genişliği sınırları için uyarıları ayarlama istiyorsanız, bunu kullanarak oluşturmanız `Cache Read` sayacı. Bkz: [Bu tablo](cache-faq.md#cache-performance) çeşitli önbellek fiyatlandırma katmanlarına ve boyutları gözlemlenen bant genişliği sınırları.** |
| Önbellek yazma |Megabayt cinsinden önbellek (MB/sn) belirtilen saniye başına yazılan veri miktarını aralığı raporlama. Bu değer, önbellek barındıran ve Redis belirli değil sanal makineyi destekleyen ağ arabirim kartları türetilir. Bu değer, ağ bant genişliği, önbelleği istemci tarafından gönderilen verilerin karşılık gelir. |

<a name="operations-and-alerts"></a>
## <a name="alerts"></a>Uyarılar
Ölçümleri ve etkinlik açtığında göre uyarıları almak üzere yapılandırabilirsiniz. Azure İzleyici tetikler, aşağıdakileri yapmak için bir uyarı yapılandırmanıza olanak sağlar:

* Bir e-posta bildirimi gönder
* bir Web kancası çağırın
* Bir Azure mantıksal uygulamayı çağırmak

Önbelleğinizi uyarı kurallarını yapılandırmak için tıklatın **uyarı kuralları** gelen **kaynak menü**.

![İzleme](./media/cache-how-to-monitor/redis-cache-monitoring.png)

Yapılandırma ve Uyarıları kullanma hakkında daha fazla bilgi için bkz: [genel bakış, uyarıları](../monitoring-and-diagnostics/insights-alerts-portal.md).

## <a name="activity-logs"></a>Etkinlik Günlükleri
Etkinlik günlükleri, Azure Redis önbelleği örnekleri üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Daha önce "denetim günlüklerini" veya "işletimsel logs" olarak bilinirdi. Etkinlik Günlükleri'ni kullanarak, belirleyebilirsiniz "ne, kimin, ne zaman ve" herhangi bir Azure Redis önbelleği örnekleri üzerinde gerçekleştirilen işlemleri (PUT, POST, silme) yazmak için. 

> [!NOTE]
> Etkinlik günlükleri okuma (GET) işlemleri dahil etmeyin.
>
>

Önbelleğinizi etkinlik günlükleri görüntülemek için **etkinlik günlükleri** gelen **kaynak menü**.

Etkinlik günlükleri hakkında daha fazla bilgi için bkz: [Azure etkinlik günlüğü'ne genel bakış](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).











