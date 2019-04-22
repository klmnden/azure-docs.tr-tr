---
title: Azure Analysis Services sunucu ölçümlerini izleme | Microsoft Docs
description: Analysis Services sunucu ölçümlerini da Azure portal'ı izleme hakkında bilgi edinin.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: cdffa8e138062a91bd1876ac6e44728c47d9cdd7
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58893189"
---
# <a name="monitor-server-metrics"></a>Sunucu ölçümlerini izleme

Analysis Services, performansı ve durumu sunucularınızın izlemenize yardımcı olması için ölçümler sağlar. Örneğin, bellek ve CPU kullanımı, istemci bağlantıları ve sorgu kaynak tüketimi sayısını izleyin. Analysis Services, çoğu diğer Azure hizmetlerinde aynı izleme framework kullanır. Daha fazla bilgi için bkz. [Microsoft azure'da ölçümleri](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

Daha ayrıntılı tanılama işlemleri gerçekleştirmek için izleme, performans ve birden çok hizmet kaynaklarını bir kaynak grubu veya abonelik arasında eğilimleri belirlemek, kullanın [Azure İzleyici](https://azure.microsoft.com/services/monitor/). Azure İzleyici (hizmet) hizmet ücretlerinin alınmasına neden.


## <a name="to-monitor-metrics-for-an-analysis-services-server"></a>Bir Analysis Services sunucu ölçümlerini izleme

1. Azure portalında **ölçümleri**.

    ![Azure portalında izleme](./media/analysis-services-monitor/aas-monitor-portal.png)

2. İçinde **kullanılabilir ölçümler**, grafiğinizde içerecek şekilde ölçümleri seçin. 

    ![İzleyici grafiği](./media/analysis-services-monitor/aas-monitor-chart.png)

<a id="#server-metrics"></a>

## <a name="server-metrics"></a>Sunucu ölçümleri

Hangi ölçümleri izleme senaryonuz için en iyi olduğunu belirlemek için bu tabloyu kullanın. Aynı grafiğe yalnızca aynı birimden ölçümler gösterilebilir.

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|
|---|---|---|---|---|
|CommandPoolJobQueueLength|Komut havuzu iş kuyruğu uzunluğu|Sayı|Ortalama|Komut iş parçacığı havuzu kuyruğundaki iş sayısı.|
|CurrentConnections|Bağlantı: Geçerli bağlantılar|Sayı|Ortalama|Kurulan istemci bağlantılarının geçerli sayısı.|
|CurrentUserSessions|Geçerli kullanıcı oturumları|Sayı|Ortalama|Geçerli kullanıcı oturumlarının sayısı.|
|mashup_engine_memory_metric|M altyapı belleği|Bayt|Ortalama|Karma altyapısı işlemleri tarafından bellek kullanımı|
|mashup_engine_qpu_metric|M altyapı QPU'su|Sayı|Ortalama|Karma altyapısı işlemleri tarafından kullanılan qpu|
|memory_metric|Bellek|Bayt|Ortalama|Bellek. 0-25 GB S1, S2 için 0-50 GB ve 0-100 aralığında S4 için GB|
|memory_thrashing_metric|Bellek çok yavaş|Yüzde|Ortalama|Ortalama bellek temizleme.|
|CleanerCurrentPrice|Bellek: Temizleyici geçerli fiyatı|Sayı|Ortalama|Geçerli fiyat, bellek $/ bayt/zaman normalleştirilmiş 1000.|
|CleanerMemoryNonshrinkable|Bellek: Temizleyici belleği daraltılamaz|Bayt|Ortalama|Tarafından arka plan Temizleyicisi kapsamında olmayan bayt cinsinden bellek miktarı.|
|CleanerMemoryShrinkable|Bellek: Temizleyici belleği daraltılabilir|Bayt|Ortalama|Tarafından arka plan Temizleyicisi temizleme bayt cinsinden bellek miktarı.|
|MemoryLimitHard|Bellek: Bellek sınırı sabit|Bayt|Ortalama|Yapılandırma dosyasından sabit bellek sınırı.|
|MemoryLimitHigh|Bellek: Bellek sınırı yüksek|Bayt|Ortalama|Yapılandırma dosyasından yüksek bellek sınırı.|
|MemoryLimitLow|Bellek: Bellek sınırı düşük|Bayt|Ortalama|Yapılandırma dosyasından düşük bellek sınırı.|
|MemoryLimitVertiPaq|Bellek: Bellek sınırı VertiPaq|Bayt|Ortalama|Yapılandırma dosyasından bellek içi sınır.|
|MemoryUsage|Bellek: Bellek Kullanımı|Bayt|Ortalama|Temizleyici bellek fiyatı hesaplanırken kullanılan sunucu işlemi bellek kullanımı. Process\PrivateBytes artı eşlenmiş veya altyapısı bellek sınırını aşan bellek içi analiz altyapısı (VertiPaq) tarafından ayrılan bir bellek yok sayılıyor, bellekle eşlenen verilerin boyutunu sayaç eşittir.|
|Kota|Bellek: Kota|Bayt|Ortalama|Bayt cinsinden geçerli bellek kotası. Bellek kotası bellek ataması veya bellek ayırma de denir.|
|QuotaBlocked|Bellek: Kota engellendi|Sayı|Ortalama|Diğer bellek kotaları serbest bırakılana kadar engellenen kota isteklerinin geçerli sayısı.|
|VertiPaqNonpaged|Bellek: Disk belleği olmayan VertiPaq|Bayt|Ortalama|Bayt bellek kullanımı için bellek içi altyapısı tarafından belirlenen çalışma kilitli.|
|VertiPaqPaged|Bellek: Disk belleği olan VertiPaq|Bayt|Ortalama|Bellek içi veriler için disk belleğine alınan bellek bayt sayısı.|
|ProcessingPoolJobQueueLength|İşleme havuzu iş kuyruğu uzunluğu|Sayı|Ortalama|İşleme iş parçacığı havuzu kuyruğundaki/ç olmayan iş sayısı.|
|RowsConvertedPerSec|İşleniyor: Satır sayısı / sn dönüştürüldü|CountPerSecond|Ortalama|İşleme sırasında dönüştürülen bir satır oranı.|
|RowsReadPerSec|İşleniyor: Satır sayısı / sn okuyun|CountPerSecond|Ortalama|Satır oranı tüm ilişkisel veritabanlarından okunan.|
|RowsWrittenPerSec|İşleniyor: Saniye yazılan satır|CountPerSecond|Ortalama|İşleme sırasında yazılan satır oranı.|
|qpu_metric|QPU|Sayı|Ortalama|QPU. S1, S2 için 0-200 ve S4 için 0-400 için 0-100 aralığında|
|QueryPoolBusyThreads|Sorgu havuzu meşgul iş parçacıkları|Sayı|Ortalama|Sorgu iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|
|SuccessfullConnectionsPerSec|Saniye başına başarılı bağlantılar|CountPerSecond|Ortalama|Başarıyla tamamlanan bağlantı oranı.|
|CommandPoolBusyThreads|İş parçacıkları: Komut havuzu meşgul iş parçacıkları|Sayı|Ortalama|Komut iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|
|CommandPoolIdleThreads|İş parçacıkları: Komut havuzu boşta iş parçacıkları|Sayı|Ortalama|Komut iş parçacığı havuzundaki boşta iş parçacığı sayısı.|
|LongParsingBusyThreads|İş parçacıkları: Uzun ayrıştırma meşgul iş parçacıkları|Sayı|Ortalama|Uzun iş parçacığı İnceleme havuzundaki meşgul iş parçacığı sayısı.|
|LongParsingIdleThreads|İş parçacıkları: Uzun ayrıştırma boşta iş parçacıkları|Sayı|Ortalama|Uzun iş parçacığı İnceleme havuzundaki boşta iş parçacığı sayısı.|
|LongParsingJobQueueLength|İş parçacıkları: Uzun ayrıştırma iş kuyruğu uzunluğu|Sayı|Ortalama|Uzun ayrıştırma iş parçacığı havuzu kuyruğundaki iş sayısı.|
|ProcessingPoolIOJobQueueLength|İş parçacıkları: İşleme havuzu g/ç iş kuyruğu uzunluğu|Sayı|Ortalama|İşleme iş parçacığı havuzu kuyruğundaki g/ç iş sayısı.|
|ProcessingPoolBusyIOJobThreads|İş parçacıkları: İşleme havuzu meşgul g/ç işi iş parçacıkları|Sayı|Ortalama|İşleme iş parçacığı havuzunda g/ç işleri çalıştıran iş parçacığı sayısı.|
|ProcessingPoolBusyNonIOThreads|İş parçacıkları: İşleme havuzu meşgul/ç olmayan iş parçacıkları|Sayı|Ortalama|İşleme iş parçacığı havuzunda olmayan g/Ç işleri çalıştıran iş parçacığı sayısı.|
|ProcessingPoolIdleIOJobThreads|İş parçacıkları: İşleme havuzu boşta g/ç işi iş parçacıkları|Sayı|Ortalama|İşleme iş parçacığı havuzundaki g/ç işleri için boşta iş parçacığı sayısı.|
|ProcessingPoolIdleNonIOThreads|İş parçacıkları: İşleme havuzu boşta/ç olmayan iş parçacıkları|Sayı|Ortalama|/ Ç olmayan işler için ayrılan işleme iş parçacığı havuzundaki boşta iş parçacığı sayısı.|
|QueryPoolIdleThreads|İş parçacıkları: Sorgu havuzu boşta iş parçacıkları|Sayı|Ortalama|İşleme iş parçacığı havuzundaki g/ç işleri için boşta iş parçacığı sayısı.|
|QueryPoolJobQueueLength|İş parçacıkları: Sorgu havuzu iş kuyruğu uzunluğu|Sayı|Ortalama|Sorgu iş parçacığı havuzu kuyruğundaki iş sayısı.|
|ShortParsingBusyThreads|İş parçacıkları: Kısa ayrıştırma meşgul iş parçacıkları|Sayı|Ortalama|Kısa ayrıştırma iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|
|ShortParsingIdleThreads|İş parçacıkları: Kısa ayrıştırma boşta iş parçacıkları|Sayı|Ortalama|Kısa ayrıştırma iş parçacığı havuzundaki boşta iş parçacığı sayısı.|
|ShortParsingJobQueueLength|İş parçacıkları: Kısa ayrıştırma iş kuyruğu uzunluğu|Sayı|Ortalama|Kısa ayrıştırma iş parçacığı havuzu kuyruğundaki iş sayısı.|
|TotalConnectionFailures|Toplam bağlantı hataları|Sayı|Ortalama|Toplam bağlantı denemesi başarısız oldu.|
|TotalConnectionRequests|Toplam bağlantı istekleri|Sayı|Ortalama|Toplam bağlantı istekleri. |

## <a name="next-steps"></a>Sonraki adımlar
[Microsoft Azure'da izleme](../monitoring-and-diagnostics/monitoring-overview.md)   
[Microsoft azure'da ölçümleri](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md)   
[Azure İzleyici REST API de ölçümleri](/rest/api/monitor/metrics)
