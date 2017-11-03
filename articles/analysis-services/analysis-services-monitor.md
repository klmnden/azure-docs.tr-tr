---
title: "Azure Analysis Services sunucusu ölçümlerini izlemek | Microsoft Docs"
description: "Analysis Services sunucusu ölçümlerini içinde bir Azure portalına izlemek öğrenin."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 09/14/2017
ms.author: owend
ms.openlocfilehash: f9b32029f0a7065fff73ddb6417fc5c1c7e658a5
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="monitor-server-metrics"></a>İzleyici Sunucusu ölçümleri

Analysis Services performans ve sunucularınızı durumunu izlemenize yardımcı olması için ölçümleri sağlar. Örneğin, bellek ve CPU kullanımı, istemci bağlantıları ve sorgu kaynak tüketimini sayısı izleyin. Analysis Services aynı izleme framework en diğer Azure hizmetleriyle kullanır. Daha fazla bilgi için bkz: [Microsoft Azure ölçümlerini](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

Daha fazla ayrıntılı tanılama işlemleri gerçekleştirmek için performans, izleme ve kaynak grubu veya abonelik birden çok hizmet kaynakları arasında eğilimlerini, kullanma [Azure İzleyici](https://azure.microsoft.com/services/monitor/). Azure İzleyicisi'ni (hizmeti) bir ek hizmet ücretlerin alınmasına neden olabilir.


## <a name="to-monitor-metrics-for-an-analysis-services-server"></a>Bir Analysis Services sunucusuna ölçümlerini izlemek için

1. Azure portalında seçin **ölçümleri**.

    ![Azure portalında izleme](./media/analysis-services-monitor/aas-monitor-portal.png)

2. İçinde **kullanılabilir ölçümler**, grafiğinizde içerecek şekilde ölçümleri seçin. 

    ![İzleyici grafik](./media/analysis-services-monitor/aas-monitor-chart.png)

## <a name="server-metrics"></a>Sunucu ölçümleri
Hangi ölçümleri izleme senaryonuz için en iyi olduğunu belirlemek için bu tabloyu kullanın. Yalnızca aynı birimi ölçümlerini aynı grafikte gösterilebilir.

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|
|---|---|---|---|---|
|CommandPoolJobQueueLength|Komut havuzu iş sırası uzunluğu|Sayı|Ortalama|Komut iş parçacığı havuzunun sıraya işlerin sayısı.|
|CurrentConnections|Bağlantı: Geçerli bağlantılar|Sayı|Ortalama|İstemci bağlantı kuran geçerli sayısı.|
|CurrentUserSessions|Geçerli kullanıcı oturumları|Sayı|Ortalama|Oluşturulan kullanıcı oturumları geçerli sayısı.|
|mashup_engine_memory_metric|M altyapısı belleği|Bayt|Ortalama|Mashup altyapısı işlemler tarafından bellek kullanımı|
|mashup_engine_qpu_metric|M altyapısı QPU|Sayı|Ortalama|Mashup altyapısı işlemler tarafından QPU kullanımı|
|memory_metric|Bellek|Bayt|Ortalama|Bellek. 0-25 GB S1, S2 için 0-50 GB ve 0-100 aralığında S4 için GB|
|memory_thrashing_metric|Bellek yoğun disk belleği|Yüzde|Ortalama|Ortalama bellek yoğun disk belleği'ü tıklatın.|
|CleanerCurrentPrice|Bellek: Temizleyici geçerli fiyatı|Sayı|Ortalama|Geçerli fiyat bellek, $/ bayt/saat normalleştirilmiş 1000.|
|CleanerMemoryNonshrinkable|Bellek: Temizleyici bellek nonshrinkable|Bayt|Ortalama|Arka plan tarafından temizleyici temizleme değil tabi bayt cinsinden bellek miktarı.|
|CleanerMemoryShrinkable|: Temizleyici bellek daraltılabilir|Bayt|Ortalama|Temizleyici tarafından arka plan temizleme tabi bayt cinsinden bellek miktarı.|
|MemoryLimitHard|Bellek: Sabit bellek sınırı|Bayt|Ortalama|Yapılandırma dosyasından sabit bellek sınırı.|
|MemoryLimitHigh|Bellek: Bellek sınırı yüksek|Bayt|Ortalama|Yapılandırma dosyasından yüksek bellek sınırı.|
|MemoryLimitLow|Bellek: Bellek sınırı düşük|Bayt|Ortalama|Yapılandırma dosyasından düşük bellek sınırı.|
|MemoryLimitVertiPaq|Bellek: Bellek sınırı VertiPaq|Bayt|Ortalama|Yapılandırma dosyasından bellek içi sınırı.|
|MemoryUsage|Bellek: Bellek kullanımı|Bayt|Ortalama|Temizleyici bellek fiyatı hesaplamak için kullanılan gibi sunucu işleminin bellek kullanımı. Process\PrivateBytes ek bellek eşlemeli veri olan eşlenen veya altyapısı bellek sınırı aşan bellek içi analytics altyapısı (VertiPaq) tarafından ayrılan tüm bellek yoksayılıyor boyutu sayaç için eşittir.|
|Kota|Bellek: Kota|Bayt|Ortalama|Bayt cinsinden geçerli bellek kotasını. Bellek kotasını bellek grant veya bellek ayırma de denir.|
|QuotaBlocked|Bellek: Engellenen kota|Sayı|Ortalama|Geçerli diğer bellek kotalarını serbest kadar engellenen kota istek sayısı.|
|VertiPaqNonpaged|Bellek: VertiPaq belleği olmayan havuz|Bayt|Ortalama|Bellek içi altyapısı tarafından kullanım için belirlenen çalışma belleğin bayt kilitli.|
|VertiPaqPaged|Bellek: Disk belleği VertiPaq|Bayt|Ortalama|Bellek içi veriler için kullanılan disk belleği bayt.|
|ProcessingPoolJobQueueLength|İşlem havuzu iş sırası uzunluğu|Sayı|Ortalama|İşleme iş parçacığı havuzunun sıraya olmayan-g/Ç işlerin sayısı.|
|RowsConvertedPerSec|İşlem: Satır sayısı / sn dönüştürülür.|CountPerSecond|Ortalama|İşleme sırasında dönüştürülmüş satır oranı.|
|RowsReadPerSec|İşlem: Satır sayısı / sn okuyun.|CountPerSecond|Ortalama|Satır oranını tüm ilişkisel veritabanlarından okuyun.|
|RowsWrittenPerSec|İşlem: saniye yazılan satırları|CountPerSecond|Ortalama|İşleme sırasında yazılmış satırları oranı.|
|qpu_metric|QPU|Sayı|Ortalama|QPU. S1, S2 için 0-200 ve 0-400 S4 için için 0-100 aralığında|
|QueryPoolBusyThreads|Sorgu havuzu meşgul iş parçacıkları|Sayı|Ortalama|Sorgu iş parçacığı havuzu meşgul iş parçacığı sayısı.|
|SuccessfullConnectionsPerSec|Başarılı bağlantıları sayısı / sn|CountPerSecond|Ortalama|Başarılı bağlantının tamamlamalar oranı.|
|CommandPoolBusyThreads|İş parçacıkları: Komut havuzu meşgul iş parçacıkları|Sayı|Ortalama|Meşgul komutu iş parçacığı havuzu iş parçacıkları sayısı.|
|CommandPoolIdleThreads|İş parçacıkları: boş iş parçacığı havuzu komutu|Sayı|Ortalama|Komut iş parçacığı havuzundaki boş iş parçacığı sayısı.|
|LongParsingBusyThreads|İş parçacıkları: meşgul iş parçacığı ayrıştırma uzun|Sayı|Ortalama|Meşgul uzun ayrıştırma iş parçacığı havuzu iş parçacıkları sayısı.|
|LongParsingIdleThreads|İş parçacıkları: boş iş parçacığı ayrıştırma uzun|Sayı|Ortalama|Uzun ayrıştırma iş parçacığı havuzundaki boş iş parçacığı sayısı.|
|LongParsingJobQueueLength|İş parçacıkları: Uzun iş kuyruğu uzunluğu ayrıştırma|Sayı|Ortalama|Uzun ayrıştırma iş parçacığı havuzunun sıraya işlerin sayısı.|
|ProcessingPoolIOJobQueueLength|İş parçacıkları: işleme havuzu g/ç iş sırası uzunluğu|Sayı|Ortalama|İşleme iş parçacığı havuzunun sıraya g/ç işlerin sayısı.|
|ProcessingPoolBusyIOJobThreads|İş parçacıkları: havuz meşgul g/ç iş iş parçacığı işleme|Sayı|Ortalama|Çalışan g/ç iş işleme iş parçacığı havuzundaki iş parçacığı sayısı.|
|ProcessingPoolBusyNonIOThreads|İş parçacıkları: yoğun olmayan-g/Ç iş parçacığı havuzu işleme|Sayı|Ortalama|Çalışan olmayan-g/Ç iş işleme iş parçacığı havuzundaki iş parçacığı sayısı.|
|ProcessingPoolIdleIOJobThreads|İş parçacıkları: havuz boşta g/ç iş iş parçacığı işleme|Sayı|Ortalama|G/ç işleri işleme iş parçacığı havuzundaki boş iş parçacığı sayısı.|
|ProcessingPoolIdleNonIOThreads|İş parçacıkları: boş olmayan-g/Ç iş parçacığı havuzu işleme|Sayı|Ortalama|Olmayan-g/Ç işleri için ayrılmış işleme iş parçacığı havuzundaki boş iş parçacığı sayısı.|
|QueryPoolIdleThreads|İş parçacıkları: Sorgu havuzu boşta iş parçacıkları|Sayı|Ortalama|G/ç işleri işleme iş parçacığı havuzundaki boş iş parçacığı sayısı.|
|QueryPoolJobQueueLength|İş parçacıkları: Sorgu havuzu iş sırası uzunluğu|Sayı|Ortalama|Sorgu iş parçacığı havuzunun sıraya işlerin sayısı.|
|ShortParsingBusyThreads|İş parçacıkları: meşgul iş parçacığı ayrıştırma kısa|Sayı|Ortalama|Meşgul kısa ayrıştırma iş parçacığı havuzu iş parçacıkları sayısı.|
|ShortParsingIdleThreads|İş parçacıkları: boş iş parçacığı ayrıştırma kısa|Sayı|Ortalama|Kısa ayrıştırma iş parçacığı havuzundaki boş iş parçacığı sayısı.|
|ShortParsingJobQueueLength|İş parçacıkları: İş kuyruğu uzunluğu ayrıştırma kısa|Sayı|Ortalama|Kısa ayrıştırma iş parçacığı havuzunun sıraya işlerin sayısı.|
|TotalConnectionFailures|Toplam bağlantı hataları|Sayı|Ortalama|Toplam bağlantı denemesi başarısız oldu.|
|TotalConnectionRequests|Toplam bağlantı istekleri|Sayı|Ortalama|Toplam bağlantı istekleri. |

## <a name="next-steps"></a>Sonraki adımlar
[Microsoft Azure'da izleme](../monitoring-and-diagnostics/monitoring-overview.md)   
[Microsoft azure'da ölçümleri](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md)   
[REST API Azure izleyicisinde ölçümleri](https://msdn.microsoft.com/library/azure/dn931930.aspx)