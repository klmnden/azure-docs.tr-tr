---
title: "Azure İzleyici ölçümleri - kaynak türü başına desteklenen ölçümleri | Microsoft Docs"
description: "Azure İzleyicisi ile her bir kaynak türü için kullanılabilir ölçümleri listesi."
author: anirudhcavale
manager: ashwink
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 63d4ac65-1688-40d1-85c8-7cd408285b0f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: ancav
ms.openlocfilehash: 673f5a5cd6832adb031ef72ce25f8a1622717cfd
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2017
---
# <a name="supported-metrics-with-azure-monitor"></a>Azure İzleyicisi ile desteklenen ölçümleri
Azure İzleyicisi, portalda grafik, REST API aracılığıyla erişme veya bunları sorgulama gibi ölçümleri ile etkileşim kurmak için çeşitli yollar sağlar PowerShell veya CLI kullanarak. Aşağıda tüm ölçümleri tam bir listesi ile Azure monitörün ölçüm ardışık düzen şu anda kullanılabilir.

> [!NOTE]
> Portalı veya eski API'lerini kullanarak diğer ölçümleri bulunabilir. Bu liste, yalnızca ölçümleri birleştirilmiş Azure ölçüm işlem hattını izleme ile kullanılabilen içerir. Sorgulamak ve erişmek için kullanım ölçümleri boyutlarla lütfen [2017-05-01-Önizleme api sürümü](https://docs.microsoft.com/rest/api/monitor/metricdefinitions)
>
>

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|qpu_metric|QPU|Sayı|Ortalama|QPU. S1, S2 için 0-200 ve 0-400 S4 için için 0-100 aralığında|ServerResourceType|
|memory_metric|Bellek|Bayt|Ortalama|Bellek. 0-25 GB S1, S2 için 0-50 GB ve 0-100 aralığında S4 için GB|ServerResourceType|
|TotalConnectionRequests|Toplam bağlantı istekleri|Sayı|Ortalama|Toplam bağlantı istekleri. Varış bunlar.|ServerResourceType|
|SuccessfullConnectionsPerSec|Başarılı bağlantıları sayısı / sn|CountPerSecond|Ortalama|Başarılı bağlantının tamamlamalar oranı.|ServerResourceType|
|TotalConnectionFailures|Toplam bağlantı hataları|Sayı|Ortalama|Toplam bağlantı denemesi başarısız oldu.|ServerResourceType|
|CurrentUserSessions|Geçerli kullanıcı oturumları|Sayı|Ortalama|Oluşturulan kullanıcı oturumları geçerli sayısı.|ServerResourceType|
|QueryPoolBusyThreads|Sorgu havuzu meşgul iş parçacıkları|Sayı|Ortalama|Sorgu iş parçacığı havuzu meşgul iş parçacığı sayısı.|ServerResourceType|
|CommandPoolJobQueueLength|Komut havuzu iş sırası uzunluğu|Sayı|Ortalama|Komut iş parçacığı havuzunun sıraya işlerin sayısı.|ServerResourceType|
|ProcessingPoolJobQueueLength|İşlem havuzu iş sırası uzunluğu|Sayı|Ortalama|İşleme iş parçacığı havuzunun sıraya olmayan-g/Ç işlerin sayısı.|ServerResourceType|
|CurrentConnections|Bağlantı: Geçerli bağlantılar|Sayı|Ortalama|İstemci bağlantı kuran geçerli sayısı.|ServerResourceType|
|CleanerCurrentPrice|Bellek: Temizleyici geçerli fiyatı|Sayı|Ortalama|Geçerli fiyat bellek, $/ bayt/saat normalleştirilmiş 1000.|ServerResourceType|
|CleanerMemoryShrinkable|: Temizleyici bellek daraltılabilir|Bayt|Ortalama|Temizleyici tarafından arka plan temizleme tabi bayt cinsinden bellek miktarı.|ServerResourceType|
|CleanerMemoryNonshrinkable|Bellek: Temizleyici bellek nonshrinkable|Bayt|Ortalama|Arka plan tarafından temizleyici temizleme değil tabi bayt cinsinden bellek miktarı.|ServerResourceType|
|MemoryUsage|Bellek: Bellek kullanımı|Bayt|Ortalama|Temizleyici bellek fiyatı hesaplamak için kullanılan gibi sunucu işleminin bellek kullanımı. Process\PrivateBytes ek bellek eşlemeli veri olan eşlenen veya xVelocity altyapısı bellek sınırı aşan xVelocity bellek içi analytics altyapısı (VertiPaq) tarafından ayrılan bellek yoksayılıyor boyutu sayaç için eşittir.|ServerResourceType|
|MemoryLimitHard|Bellek: Sabit bellek sınırı|Bayt|Ortalama|Yapılandırma dosyasından sabit bellek sınırı.|ServerResourceType|
|MemoryLimitHigh|Bellek: Bellek sınırı yüksek|Bayt|Ortalama|Yapılandırma dosyasından yüksek bellek sınırı.|ServerResourceType|
|MemoryLimitLow|Bellek: Bellek sınırı düşük|Bayt|Ortalama|Yapılandırma dosyasından düşük bellek sınırı.|ServerResourceType|
|MemoryLimitVertiPaq|Bellek: Bellek sınırı VertiPaq|Bayt|Ortalama|Yapılandırma dosyasından bellek içi sınırı.|ServerResourceType|
|Kota|Bellek: Kota|Bayt|Ortalama|Bayt cinsinden geçerli bellek kotasını. Bellek kotasını bellek grant veya bellek ayırma de denir.|ServerResourceType|
|QuotaBlocked|Bellek: Engellenen kota|Sayı|Ortalama|Geçerli diğer bellek kotalarını serbest kadar engellenen kota istek sayısı.|ServerResourceType|
|VertiPaqNonpaged|Bellek: VertiPaq belleği olmayan havuz|Bayt|Ortalama|Bellek içi altyapısı tarafından kullanım için belirlenen çalışma belleğin bayt kilitli.|ServerResourceType|
|VertiPaqPaged|Bellek: Disk belleği VertiPaq|Bayt|Ortalama|Bellek içi veriler için kullanılan disk belleği bayt.|ServerResourceType|
|RowsReadPerSec|İşlem: Satır sayısı / sn okuyun.|CountPerSecond|Ortalama|Satır oranını tüm ilişkisel veritabanlarından okuyun.|ServerResourceType|
|RowsConvertedPerSec|İşlem: Satır sayısı / sn dönüştürülür.|CountPerSecond|Ortalama|İşleme sırasında dönüştürülmüş satır oranı.|ServerResourceType|
|RowsWrittenPerSec|İşlem: saniye yazılan satırları|CountPerSecond|Ortalama|İşleme sırasında yazılmış satırları oranı.|ServerResourceType|
|CommandPoolBusyThreads|İş parçacıkları: Komut havuzu meşgul iş parçacıkları|Sayı|Ortalama|Meşgul komutu iş parçacığı havuzu iş parçacıkları sayısı.|ServerResourceType|
|CommandPoolIdleThreads|İş parçacıkları: boş iş parçacığı havuzu komutu|Sayı|Ortalama|Komut iş parçacığı havuzundaki boş iş parçacığı sayısı.|ServerResourceType|
|LongParsingBusyThreads|İş parçacıkları: meşgul iş parçacığı ayrıştırma uzun|Sayı|Ortalama|Meşgul uzun ayrıştırma iş parçacığı havuzu iş parçacıkları sayısı.|ServerResourceType|
|LongParsingIdleThreads|İş parçacıkları: boş iş parçacığı ayrıştırma uzun|Sayı|Ortalama|Uzun ayrıştırma iş parçacığı havuzundaki boş iş parçacığı sayısı.|ServerResourceType|
|LongParsingJobQueueLength|İş parçacıkları: Uzun iş kuyruğu uzunluğu ayrıştırma|Sayı|Ortalama|Uzun ayrıştırma iş parçacığı havuzunun sıraya işlerin sayısı.|ServerResourceType|
|ProcessingPoolBusyIOJobThreads|İş parçacıkları: havuz meşgul g/ç iş iş parçacığı işleme|Sayı|Ortalama|Çalışan g/ç iş işleme iş parçacığı havuzundaki iş parçacığı sayısı.|ServerResourceType|
|ProcessingPoolBusyNonIOThreads|İş parçacıkları: yoğun olmayan-g/Ç iş parçacığı havuzu işleme|Sayı|Ortalama|Çalışan olmayan-g/Ç iş işleme iş parçacığı havuzundaki iş parçacığı sayısı.|ServerResourceType|
|ProcessingPoolIOJobQueueLength|İş parçacıkları: işleme havuzu g/ç iş sırası uzunluğu|Sayı|Ortalama|İşleme iş parçacığı havuzunun sıraya g/ç işlerin sayısı.|ServerResourceType|
|ProcessingPoolIdleIOJobThreads|İş parçacıkları: havuz boşta g/ç iş iş parçacığı işleme|Sayı|Ortalama|G/ç işleri işleme iş parçacığı havuzundaki boş iş parçacığı sayısı.|ServerResourceType|
|ProcessingPoolIdleNonIOThreads|İş parçacıkları: boş olmayan-g/Ç iş parçacığı havuzu işleme|Sayı|Ortalama|Olmayan-g/Ç işleri için ayrılmış işleme iş parçacığı havuzundaki boş iş parçacığı sayısı.|ServerResourceType|
|QueryPoolIdleThreads|İş parçacıkları: Sorgu havuzu boşta iş parçacıkları|Sayı|Ortalama|G/ç işleri işleme iş parçacığı havuzundaki boş iş parçacığı sayısı.|ServerResourceType|
|QueryPoolJobQueueLength|İş parçacıkları: Sorgu havuzu iş kuyruğu lengt|Sayı|Ortalama|Sorgu iş parçacığı havuzunun sıraya işlerin sayısı.|ServerResourceType|
|ShortParsingBusyThreads|İş parçacıkları: meşgul iş parçacığı ayrıştırma kısa|Sayı|Ortalama|Meşgul kısa ayrıştırma iş parçacığı havuzu iş parçacıkları sayısı.|ServerResourceType|
|ShortParsingIdleThreads|İş parçacıkları: boş iş parçacığı ayrıştırma kısa|Sayı|Ortalama|Kısa ayrıştırma iş parçacığı havuzundaki boş iş parçacığı sayısı.|ServerResourceType|
|ShortParsingJobQueueLength|İş parçacıkları: İş kuyruğu uzunluğu ayrıştırma kısa|Sayı|Ortalama|Kısa ayrıştırma iş parçacığı havuzunun sıraya işlerin sayısı.|ServerResourceType|
|memory_thrashing_metric|Bellek yoğun disk belleği|Yüzde|Ortalama|Ortalama bellek yoğun disk belleği'ü tıklatın.|ServerResourceType|
|mashup_engine_qpu_metric|M altyapısı QPU|Sayı|Ortalama|Mashup altyapısı işlemler tarafından QPU kullanımı|ServerResourceType|
|mashup_engine_memory_metric|M altyapısı belleği|Bayt|Ortalama|Mashup altyapısı işlemler tarafından bellek kullanımı|ServerResourceType|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TotalRequests|Toplam ağ geçidi istekleri|Sayı|Toplam|Ağ geçidi istek sayısı|Konum, ana bilgisayar adı|
|SuccessfulRequests|Başarılı ağ geçidi istekleri|Sayı|Toplam|Başarılı ağ geçidi istek sayısı|Konum, ana bilgisayar adı|
|UnauthorizedRequests|Yetkisiz ağ geçidi istekleri|Sayı|Toplam|Yetkisiz ağ geçidi istek sayısı|Konum, ana bilgisayar adı|
|FailedRequests|Başarısız ağ geçidi istekleri|Sayı|Toplam|Ağ geçidi istek sayısı|Konum, ana bilgisayar adı|
|OtherRequests|Diğer ağ geçidi istekleri|Sayı|Toplam|Diğer ağ geçidi istek sayısı|Konum, ana bilgisayar adı|
|Süre|Ağ geçidi isteklerin toplam süre|Milisaniye|Ortalama|Toplam süre, ağ geçidi istekleri milisaniye|Konum, ana bilgisayar adı|
|Kapasite|Kapasite (Önizleme)|Yüzde|Maksimum|ApiManagement hizmet için kullanım ölçümü|Konum|

## <a name="microsoftautomationautomationaccounts"></a>Microsoft.Automation/automationAccounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TotalJob|Toplam iş sayısı|Sayı|Toplam|İşlerin toplam sayısı|RunbookName, durumu|

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CoreCount|Ayrılmış çekirdek sayısı|Sayı|Toplam|Batch hesabında ayrılmış çekirdek toplam sayısı|Hiç boyut|
|TotalNodeCount|Ayrılmış düğüm sayısı|Sayı|Toplam|Batch hesabında ayrılmış düğüm toplam sayısı|Hiç boyut|
|LowPriorityCoreCount|LowPriority çekirdek sayısı|Sayı|Toplam|Düşük öncelikli çekirdek batch hesabında toplam sayısı|Hiç boyut|
|TotalLowPriorityNodeCount|Düşük öncelikli düğüm sayısı|Sayı|Toplam|Düşük öncelikli düğüm batch hesabında toplam sayısı|Hiç boyut|
|CreatingNodeCount|Düğüm sayısı oluşturma|Sayı|Toplam|Oluşturulan düğüm sayısı|Hiç boyut|
|StartingNodeCount|Başlangıç düğüm sayısı|Sayı|Toplam|Başlangıç düğüm sayısı|Hiç boyut|
|WaitingForStartTaskNodeCount|Başlangıç görevi düğüm sayısı için bekleniyor|Sayı|Toplam|Başlangıç görevinin tamamlanmasını için bekleyen düğüm sayısı|Hiç boyut|
|StartTaskFailedNodeCount|Düğüm sayısı başlangıç görevi başarısız oldu|Sayı|Toplam|Başlangıç görevi başarısız olduğu düğüm sayısı|Hiç boyut|
|IdleNodeCount|Boşta düğüm sayısı|Sayı|Toplam|Boşta düğüm sayısı|Hiç boyut|
|OfflineNodeCount|Çevrimdışı düğüm sayısı|Sayı|Toplam|Çevrimdışı düğüm sayısı|Hiç boyut|
|RebootingNodeCount|Düğüm sayısı yeniden başlatılıyor|Sayı|Toplam|Düğüm yeniden başlatma sayısı|Hiç boyut|
|ReimagingNodeCount|Düğüm sayısı yeniden görüntüleme|Sayı|Toplam|Yeniden görüntüsünü oluşturuyor düğüm sayısı|Hiç boyut|
|RunningNodeCount|Çalışan düğüm sayısı|Sayı|Toplam|Düğümler çalışan sayısı|Hiç boyut|
|LeavingPoolNodeCount|Bırakma havuzu düğüm sayısı|Sayı|Toplam|Havuz bırakarak düğüm sayısı|Hiç boyut|
|UnusableNodeCount|Kullanılamaz düğüm sayısı|Sayı|Toplam|Kullanılamaz düğüm sayısı|Hiç boyut|
|PreemptedNodeCount|Etkisiz düğüm sayısı|Sayı|Toplam|Etkisiz düğüm sayısı|Hiç boyut|
|TaskStartEvent|Görev Başlangıç olayları|Sayı|Toplam|Başlattığınız görevlerin toplam sayısı|Hiç boyut|
|TaskCompleteEvent|Görev Tamamlandı olayları|Sayı|Toplam|Tamamlanan görevler toplam sayısı|Hiç boyut|
|TaskFailEvent|Görev başarısız olayları|Sayı|Toplam|Başarısız bir durumda tamamlanan görevler toplam sayısı|Hiç boyut|
|PoolCreateEvent|Havuz oluşturma olayları|Sayı|Toplam|Oluşturulmuş havuzu toplam sayısı|Hiç boyut|
|PoolResizeStartEvent|Havuzu yeniden boyutlandırma başlangıç olayları|Sayı|Toplam|Başlattığınız havuzu yeniden boyutlandırır toplam sayısı|Hiç boyut|
|PoolResizeCompleteEvent|Havuzu yeniden boyutlandırma tam olayları|Sayı|Toplam|Tamamlanan havuzu yeniden boyutlandırır toplam sayısı|Hiç boyut|
|PoolDeleteStartEvent|Havuz başlangıç olayları Sil|Sayı|Toplam|Başlattığınız havuzu siler toplam sayısı|Hiç boyut|
|PoolDeleteCompleteEvent|Havuz tüm olayları Sil|Sayı|Toplam|Tamamlanan havuzu siler toplam sayısı|Hiç boyut|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|connectedclients|Bağlı İstemciler|Sayı|Maksimum||Hiç boyut|
|totalcommandsprocessed|İşlemler Toplamı|Sayı|Toplam||Hiç boyut|
|cachehits|İsabetli Önbellek Okuma Sayısı|Sayı|Toplam||Hiç boyut|
|cachemisses|İsabetsiz Önbellek Okuma Sayısı|Sayı|Toplam||Hiç boyut|
|getcommands|Alınanlar|Sayı|Toplam||Hiç boyut|
|setcommands|Kümeler|Sayı|Toplam||Hiç boyut|
|evictedkeys|Çıkarılan Anahtarlar|Sayı|Toplam||Hiç boyut|
|totalkeys|Toplam Anahtar Sayısı|Sayı|Maksimum||Hiç boyut|
|expiredkeys|Süresi Dolan Anahtarlar|Sayı|Toplam||Hiç boyut|
|usedmemory|Kullanılan Bellek|Bayt|Maksimum||Hiç boyut|
|usedmemoryRss|Kullanılan bellek RSS|Bayt|Maksimum||Hiç boyut|
|serverLoad|Sunucu Yükü|Yüzde|Maksimum||Hiç boyut|
|cacheWrite|Önbellek Yazması|BytesPerSecond|Maksimum||Hiç boyut|
|cacheRead|Önbellek Okuması|BytesPerSecond|Maksimum||Hiç boyut|
|percentProcessorTime|CPU|Yüzde|Maksimum||Hiç boyut|
|connectedclients0|Bağlanan istemciler (parça 0)|Sayı|Maksimum||Hiç boyut|
|totalcommandsprocessed0|Toplam işlemler (parça 0)|Sayı|Toplam||Hiç boyut|
|cachehits0|İsabetli Önbellek Okuma (parça 0)|Sayı|Toplam||Hiç boyut|
|cachemisses0|Önbellek isabetsizliği (parça 0)|Sayı|Toplam||Hiç boyut|
|getcommands0|(Parça 0) alır|Sayı|Toplam||Hiç boyut|
|setcommands0|Ayarlar (parça 0)|Sayı|Toplam||Hiç boyut|
|evictedkeys0|Çıkarılan anahtarları (parça 0)|Sayı|Toplam||Hiç boyut|
|totalkeys0|Toplam anahtarları (parça 0)|Sayı|Maksimum||Hiç boyut|
|expiredkeys0|Süresi dolan anahtarları (parça 0)|Sayı|Toplam||Hiç boyut|
|usedmemory0|Kullanılan bellek (parça 0)|Bayt|Maksimum||Hiç boyut|
|usedmemoryRss0|Kullanılan bellek RSS (parça 0)|Bayt|Maksimum||Hiç boyut|
|serverLoad0|Sunucu iş yükü (parça 0)|Yüzde|Maksimum||Hiç boyut|
|cacheWrite0|Önbellek yazma (parça 0)|BytesPerSecond|Maksimum||Hiç boyut|
|cacheRead0|Önbellek Okuma (parça 0)|BytesPerSecond|Maksimum||Hiç boyut|
|percentProcessorTime0|CPU (parça 0)|Yüzde|Maksimum||Hiç boyut|
|connectedclients1|Bağlanan istemciler (parça 1)|Sayı|Maksimum||Hiç boyut|
|totalcommandsprocessed1|Toplam işlemler (parça 1)|Sayı|Toplam||Hiç boyut|
|cachehits1|İsabetli Önbellek Okuma (parça 1)|Sayı|Toplam||Hiç boyut|
|cachemisses1|Önbellek isabetsizliği (parça 1)|Sayı|Toplam||Hiç boyut|
|getcommands1|(Parça 1) alır|Sayı|Toplam||Hiç boyut|
|setcommands1|Ayarlar (parça 1)|Sayı|Toplam||Hiç boyut|
|evictedkeys1|Çıkarılan anahtarları (parça 1)|Sayı|Toplam||Hiç boyut|
|totalkeys1|Toplam anahtarları (parça 1)|Sayı|Maksimum||Hiç boyut|
|expiredkeys1|Süresi dolan anahtarları (parça 1)|Sayı|Toplam||Hiç boyut|
|usedmemory1|Kullanılan bellek (parça 1)|Bayt|Maksimum||Hiç boyut|
|usedmemoryRss1|Kullanılan bellek RSS (parça 1)|Bayt|Maksimum||Hiç boyut|
|serverLoad1|Sunucu iş yükü (parça 1)|Yüzde|Maksimum||Hiç boyut|
|cacheWrite1|Önbellek yazma (parça 1)|BytesPerSecond|Maksimum||Hiç boyut|
|cacheRead1|Önbellek Okuma (parça 1)|BytesPerSecond|Maksimum||Hiç boyut|
|percentProcessorTime1|CPU (parça 1)|Yüzde|Maksimum||Hiç boyut|
|connectedclients2|Bağlanan istemciler (parça 2)|Sayı|Maksimum||Hiç boyut|
|totalcommandsprocessed2|Toplam işlemler (parça 2)|Sayı|Toplam||Hiç boyut|
|cachehits2|İsabetli Önbellek Okuma (parça 2)|Sayı|Toplam||Hiç boyut|
|cachemisses2|Önbellek isabetsizliği (parça 2)|Sayı|Toplam||Hiç boyut|
|getcommands2|(Parça 2) alır|Sayı|Toplam||Hiç boyut|
|setcommands2|Ayarlar (parça 2)|Sayı|Toplam||Hiç boyut|
|evictedkeys2|Çıkarılan anahtarları (parça 2)|Sayı|Toplam||Hiç boyut|
|totalkeys2|Toplam anahtarları (parça 2)|Sayı|Maksimum||Hiç boyut|
|expiredkeys2|Süresi dolan anahtarları (parça 2)|Sayı|Toplam||Hiç boyut|
|usedmemory2|Kullanılan bellek (parça 2)|Bayt|Maksimum||Hiç boyut|
|usedmemoryRss2|Kullanılan bellek RSS (parça 2)|Bayt|Maksimum||Hiç boyut|
|serverLoad2|Sunucu iş yükü (parça 2)|Yüzde|Maksimum||Hiç boyut|
|cacheWrite2|Önbellek yazma (parça 2)|BytesPerSecond|Maksimum||Hiç boyut|
|cacheRead2|Önbellek Okuma (parça 2)|BytesPerSecond|Maksimum||Hiç boyut|
|percentProcessorTime2|CPU (parça 2)|Yüzde|Maksimum||Hiç boyut|
|connectedclients3|Bağlanan istemciler (parça 3)|Sayı|Maksimum||Hiç boyut|
|totalcommandsprocessed3|Toplam işlemler (parça 3)|Sayı|Toplam||Hiç boyut|
|cachehits3|İsabetli Önbellek Okuma (parça 3)|Sayı|Toplam||Hiç boyut|
|cachemisses3|Önbellek isabetsizliği (parça 3)|Sayı|Toplam||Hiç boyut|
|getcommands3|(3 parça) alır|Sayı|Toplam||Hiç boyut|
|setcommands3|Ayarlar (parça 3)|Sayı|Toplam||Hiç boyut|
|evictedkeys3|Çıkarılan anahtarları (parça 3)|Sayı|Toplam||Hiç boyut|
|totalkeys3|Toplam anahtarları (parça 3)|Sayı|Maksimum||Hiç boyut|
|expiredkeys3|Süresi dolan anahtarları (parça 3)|Sayı|Toplam||Hiç boyut|
|usedmemory3|Kullanılan bellek (parça 3)|Bayt|Maksimum||Hiç boyut|
|usedmemoryRss3|Kullanılan bellek RSS (parça 3)|Bayt|Maksimum||Hiç boyut|
|serverLoad3|Sunucu iş yükü (parça 3)|Yüzde|Maksimum||Hiç boyut|
|cacheWrite3|Önbellek yazma (parça 3)|BytesPerSecond|Maksimum||Hiç boyut|
|cacheRead3|Önbellek Okuma (parça 3)|BytesPerSecond|Maksimum||Hiç boyut|
|percentProcessorTime3|CPU (parça 3)|Yüzde|Maksimum||Hiç boyut|
|connectedclients4|Bağlanan istemciler (parça 4)|Sayı|Maksimum||Hiç boyut|
|totalcommandsprocessed4|Toplam işlemler (parça 4)|Sayı|Toplam||Hiç boyut|
|cachehits4|İsabetli Önbellek Okuma (parça 4)|Sayı|Toplam||Hiç boyut|
|cachemisses4|Önbellek isabetsizliği (parça 4)|Sayı|Toplam||Hiç boyut|
|getcommands4|(4 parça) alır|Sayı|Toplam||Hiç boyut|
|setcommands4|Ayarlar (parça 4)|Sayı|Toplam||Hiç boyut|
|evictedkeys4|Çıkarılan anahtarları (parça 4)|Sayı|Toplam||Hiç boyut|
|totalkeys4|Toplam anahtarları (parça 4)|Sayı|Maksimum||Hiç boyut|
|expiredkeys4|Süresi dolan anahtarları (parça 4)|Sayı|Toplam||Hiç boyut|
|usedmemory4|Kullanılan bellek (parça 4)|Bayt|Maksimum||Hiç boyut|
|usedmemoryRss4|Kullanılan bellek RSS (parça 4)|Bayt|Maksimum||Hiç boyut|
|serverLoad4|Sunucu iş yükü (parça 4)|Yüzde|Maksimum||Hiç boyut|
|cacheWrite4|Önbellek yazma (parça 4)|BytesPerSecond|Maksimum||Hiç boyut|
|cacheRead4|Önbellek Okuma (parça 4)|BytesPerSecond|Maksimum||Hiç boyut|
|percentProcessorTime4|CPU (parça 4)|Yüzde|Maksimum||Hiç boyut|
|connectedclients5|Bağlanan istemciler (parça 5)|Sayı|Maksimum||Hiç boyut|
|totalcommandsprocessed5|Toplam işlemler (parça 5)|Sayı|Toplam||Hiç boyut|
|cachehits5|İsabetli Önbellek Okuma (parça 5)|Sayı|Toplam||Hiç boyut|
|cachemisses5|Önbellek isabetsizliği (parça 5)|Sayı|Toplam||Hiç boyut|
|getcommands5|(5 parça) alır|Sayı|Toplam||Hiç boyut|
|setcommands5|Ayarlar (parça 5)|Sayı|Toplam||Hiç boyut|
|evictedkeys5|Çıkarılan anahtarları (parça 5)|Sayı|Toplam||Hiç boyut|
|totalkeys5|Toplam anahtarları (parça 5)|Sayı|Maksimum||Hiç boyut|
|expiredkeys5|Süresi dolan anahtarları (parça 5)|Sayı|Toplam||Hiç boyut|
|usedmemory5|Kullanılan bellek (parça 5)|Bayt|Maksimum||Hiç boyut|
|usedmemoryRss5|Kullanılan bellek RSS (parça 5)|Bayt|Maksimum||Hiç boyut|
|serverLoad5|Sunucu iş yükü (parça 5)|Yüzde|Maksimum||Hiç boyut|
|cacheWrite5|Önbellek yazma (parça 5)|BytesPerSecond|Maksimum||Hiç boyut|
|cacheRead5|Önbellek Okuma (parça 5)|BytesPerSecond|Maksimum||Hiç boyut|
|percentProcessorTime5|CPU (parça 5)|Yüzde|Maksimum||Hiç boyut|
|connectedclients6|Bağlanan istemciler (parça 6)|Sayı|Maksimum||Hiç boyut|
|totalcommandsprocessed6|Toplam işlemler (parça 6)|Sayı|Toplam||Hiç boyut|
|cachehits6|İsabetli Önbellek Okuma (parça 6)|Sayı|Toplam||Hiç boyut|
|cachemisses6|Önbellek isabetsizliği (parça 6)|Sayı|Toplam||Hiç boyut|
|getcommands6|(Parça 6) alır|Sayı|Toplam||Hiç boyut|
|setcommands6|Ayarlar (parça 6)|Sayı|Toplam||Hiç boyut|
|evictedkeys6|Çıkarılan anahtarları (parça 6)|Sayı|Toplam||Hiç boyut|
|totalkeys6|Toplam anahtarları (parça 6)|Sayı|Maksimum||Hiç boyut|
|expiredkeys6|Süresi dolan anahtarları (parça 6)|Sayı|Toplam||Hiç boyut|
|usedmemory6|Kullanılan bellek (parça 6)|Bayt|Maksimum||Hiç boyut|
|usedmemoryRss6|Kullanılan bellek RSS (parça 6)|Bayt|Maksimum||Hiç boyut|
|serverLoad6|Sunucu iş yükü (parça 6)|Yüzde|Maksimum||Hiç boyut|
|cacheWrite6|Önbellek yazma (parça 6)|BytesPerSecond|Maksimum||Hiç boyut|
|cacheRead6|Önbellek Okuma (parça 6)|BytesPerSecond|Maksimum||Hiç boyut|
|percentProcessorTime6|CPU (parça 6)|Yüzde|Maksimum||Hiç boyut|
|connectedclients7|Bağlanan istemciler (parça 7)|Sayı|Maksimum||Hiç boyut|
|totalcommandsprocessed7|Toplam işlemler (parça 7)|Sayı|Toplam||Hiç boyut|
|cachehits7|İsabetli Önbellek Okuma (parça 7)|Sayı|Toplam||Hiç boyut|
|cachemisses7|Önbellek isabetsizliği (parça 7)|Sayı|Toplam||Hiç boyut|
|getcommands7|(Parça 7) alır|Sayı|Toplam||Hiç boyut|
|setcommands7|Ayarlar (parça 7)|Sayı|Toplam||Hiç boyut|
|evictedkeys7|Çıkarılan anahtarları (parça 7)|Sayı|Toplam||Hiç boyut|
|totalkeys7|Toplam anahtarları (parça 7)|Sayı|Maksimum||Hiç boyut|
|expiredkeys7|Süresi dolan anahtarları (parça 7)|Sayı|Toplam||Hiç boyut|
|usedmemory7|Kullanılan bellek (parça 7)|Bayt|Maksimum||Hiç boyut|
|usedmemoryRss7|Kullanılan bellek RSS (parça 7)|Bayt|Maksimum||Hiç boyut|
|serverLoad7|Sunucu iş yükü (parça 7)|Yüzde|Maksimum||Hiç boyut|
|cacheWrite7|Önbellek yazma (parça 7)|BytesPerSecond|Maksimum||Hiç boyut|
|cacheRead7|Önbellek Okuma (parça 7)|BytesPerSecond|Maksimum||Hiç boyut|
|percentProcessorTime7|CPU (parça 7)|Yüzde|Maksimum||Hiç boyut|
|connectedclients8|Bağlanan istemciler (parça 8)|Sayı|Maksimum||Hiç boyut|
|totalcommandsprocessed8|Toplam işlemler (parça 8)|Sayı|Toplam||Hiç boyut|
|cachehits8|İsabetli Önbellek Okuma (parça 8)|Sayı|Toplam||Hiç boyut|
|cachemisses8|Önbellek isabetsizliği (parça 8)|Sayı|Toplam||Hiç boyut|
|getcommands8|(Parça 8) alır|Sayı|Toplam||Hiç boyut|
|setcommands8|Ayarlar (parça 8)|Sayı|Toplam||Hiç boyut|
|evictedkeys8|Çıkarılan anahtarları (parça 8)|Sayı|Toplam||Hiç boyut|
|totalkeys8|Toplam anahtarları (parça 8)|Sayı|Maksimum||Hiç boyut|
|expiredkeys8|Süresi dolan anahtarları (parça 8)|Sayı|Toplam||Hiç boyut|
|usedmemory8|Kullanılan bellek (parça 8)|Bayt|Maksimum||Hiç boyut|
|usedmemoryRss8|Kullanılan bellek RSS (parça 8)|Bayt|Maksimum||Hiç boyut|
|serverLoad8|Sunucu iş yükü (parça 8)|Yüzde|Maksimum||Hiç boyut|
|cacheWrite8|Önbellek yazma (parça 8)|BytesPerSecond|Maksimum||Hiç boyut|
|cacheRead8|Önbellek Okuma (parça 8)|BytesPerSecond|Maksimum||Hiç boyut|
|percentProcessorTime8|CPU (parça 8)|Yüzde|Maksimum||Hiç boyut|
|connectedclients9|Bağlanan istemciler (parça 9)|Sayı|Maksimum||Hiç boyut|
|totalcommandsprocessed9|Toplam işlemler (parça 9)|Sayı|Toplam||Hiç boyut|
|cachehits9|İsabetli Önbellek Okuma (parça 9)|Sayı|Toplam||Hiç boyut|
|cachemisses9|Önbellek isabetsizliği (parça 9)|Sayı|Toplam||Hiç boyut|
|getcommands9|Alır (parça 9)|Sayı|Toplam||Hiç boyut|
|setcommands9|Ayarlar (parça 9)|Sayı|Toplam||Hiç boyut|
|evictedkeys9|Çıkarılan anahtarları (parça 9)|Sayı|Toplam||Hiç boyut|
|totalkeys9|Toplam anahtarları (parça 9)|Sayı|Maksimum||Hiç boyut|
|expiredkeys9|Süresi dolan anahtarları (parça 9)|Sayı|Toplam||Hiç boyut|
|usedmemory9|Kullanılan bellek (parça 9)|Bayt|Maksimum||Hiç boyut|
|usedmemoryRss9|Kullanılan bellek RSS (parça 9)|Bayt|Maksimum||Hiç boyut|
|serverLoad9|Sunucu iş yükü (parça 9)|Yüzde|Maksimum||Hiç boyut|
|cacheWrite9|Önbellek yazma (parça 9)|BytesPerSecond|Maksimum||Hiç boyut|
|cacheRead9|Önbellek Okuma (parça 9)|BytesPerSecond|Maksimum||Hiç boyut|
|percentProcessorTime9|CPU (parça 9)|Yüzde|Maksimum||Hiç boyut|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.ClassicCompute/virtualMachines

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU Yüzdesi|CPU Yüzdesi|Yüzde|Ortalama|Sanal Makineler tarafından şu anda kullanılan ayrılmış işlem birimlerinin yüzdesi.|Hiç boyut|
|Ağ Girişi|Ağ Girişi|Bayt|Toplam|Sanal Makineler tarafından tüm ağ arabirimleri üzerinde alınan bayt sayısı (Gelen Trafik).|Hiç boyut|
|Ağ Çıkışı|Ağ Çıkışı|Bayt|Toplam|Sanal Makineler tarafından tüm ağ arabirimleri üzerinde gönderilen bayt sayısı (Giden Trafik).|Hiç boyut|
|Disk Okuma Bayt/sn|Disk Okuma|BytesPerSecond|Ortalama|İzleme dönemi boyunca diskten okunan ortalama bayt.|Hiç boyut|
|Disk Yazma Bayt/Sn|Disk Yazma|BytesPerSecond|Ortalama|İzleme dönemi boyunca diske yazılan ortalama bayt.|Hiç boyut|
|Disk Okuma İşlemi/Sn|Disk Okuma İşlemi/Sn|CountPerSecond|Ortalama|Disk Okuma IOPS.|Hiç boyut|
|Disk Yazma İşlemi/Sn|Disk Yazma İşlemi/Sn|CountPerSecond|Ortalama|Disk Yazma IOPS.|Hiç boyut|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TotalCalls|Toplam Çağrı Sayısı|Sayı|Toplam|Toplam çağrı sayısı.|Hiç boyut|
|SuccessfulCalls|Başarılı Çağrılar|Sayı|Toplam|Başarılı olan çağrıların sayısı.|Hiç boyut|
|TotalErrors|Hatalar Toplamı|Sayı|Toplam|Hata yanıtıyla sonuçlanan toplam çağrı sayısı (HTTP yanıt kodu 4xx veya 5xx).|Hiç boyut|
|BlockedCalls|Engellenen Çağrılar|Sayı|Toplam|Oran veya kota sınırını aşan çağrı sayısı.|Hiç boyut|
|ServerErrors|Sunucu Hataları|Sayı|Toplam|Hizmet iç hatasıyla sonuçlanan çağrı sayısı (HTTP yanıt kodu 5xx).|Hiç boyut|
|ClientErrors|İstemci Hataları|Sayı|Toplam|İstemci tarafında hatayla sonuçlanan çağrı sayısı (HTTP yanıt kodu 4xx).|Hiç boyut|
|Elinden|Data Girişi|Bayt|Toplam|Gelen verilerin bayt cinsinden boyutu.|Hiç boyut|
|DataOut|Giden Veriler|Bayt|Toplam|Giden verilerin bayt cinsinden boyutu.|Hiç boyut|
|Gecikme süresi|Gecikme süresi|Milisaniye|Ortalama|Milisaniye cinsinden gecikme süresi.|Hiç boyut|
|CharactersTranslated|Çevrilen Karakterler|Sayı|Toplam|Gelen metin isteğindeki toplam karakter sayısı.|Hiç boyut|
|SpeechSessionDuration|Konuşma Oturumu Süresi|Saniye|Toplam|Konuşma oturumunun saniye cinsinden toplam süresi.|Hiç boyut|
|TotalTransactions|Toplam işlemler|Sayı|Toplam|Toplam işlem sayısı|Hiç boyut|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU Yüzdesi|CPU Yüzdesi|Yüzde|Ortalama|Sanal Makineler tarafından kullanılan ayrılmış işlem birimlerinin geçerli yüzdesi|Hiç boyut|
|Ağ Girişi|Ağ Girişi|Bayt|Toplam|Sanal Makineler tarafından tüm ağ arabirimleri üzerinde alınan bayt sayısı (Gelen Trafik)|Hiç boyut|
|Ağ Çıkışı|Ağ Çıkışı|Bayt|Toplam|Sanal Makineler tarafından tüm ağ arabirimleri üzerinde gönderilen bayt sayısı (Giden Trafik)|Hiç boyut|
|Diskten Okunan Bayt|Diskten Okunan Bayt|Bayt|Toplam|İzleme dönemi boyunca diskten okunan toplam bayt|Hiç boyut|
|Diske Yazılan Bayt|Diske Yazılan Bayt|Bayt|Toplam|İzleme dönemi boyunca diske yazılan toplam bayt|Hiç boyut|
|Disk Okuma İşlemi/Sn|Disk Okuma İşlemi/Sn|CountPerSecond|Ortalama|Disk Okuma IOPS|Hiç boyut|
|Disk Yazma İşlemi/Sn|Disk Yazma İşlemi/Sn|CountPerSecond|Ortalama|Disk Yazma IOPS|Hiç boyut|
|Kalan CPU Kredisi|Kalan CPU Kredisi|Sayı|Ortalama|Veri bloğunun kullanabileceği toplam kredi sayısı|Hiç boyut|
|Tüketilen CPU Kredisi|Tüketilen CPU Kredisi|Sayı|Ortalama|Sanal Makine tarafından tüketilen toplam kredi sayısı|Hiç boyut|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU Yüzdesi|CPU Yüzdesi|Yüzde|Ortalama|Sanal Makineler tarafından kullanılan ayrılmış işlem birimlerinin geçerli yüzdesi|Hiç boyut|
|Ağ Girişi|Ağ Girişi|Bayt|Toplam|Sanal Makineler tarafından tüm ağ arabirimleri üzerinde alınan bayt sayısı (Gelen Trafik)|Hiç boyut|
|Ağ Çıkışı|Ağ Çıkışı|Bayt|Toplam|Sanal Makineler tarafından tüm ağ arabirimleri üzerinde gönderilen bayt sayısı (Giden Trafik)|Hiç boyut|
|Diskten Okunan Bayt|Diskten Okunan Bayt|Bayt|Toplam|İzleme dönemi boyunca diskten okunan toplam bayt|Hiç boyut|
|Diske Yazılan Bayt|Diske Yazılan Bayt|Bayt|Toplam|İzleme dönemi boyunca diske yazılan toplam bayt|Hiç boyut|
|Disk Okuma İşlemi/Sn|Disk Okuma İşlemi/Sn|CountPerSecond|Ortalama|Disk Okuma IOPS|Hiç boyut|
|Disk Yazma İşlemi/Sn|Disk Yazma İşlemi/Sn|CountPerSecond|Ortalama|Disk Yazma IOPS|Hiç boyut|
|Kalan CPU Kredisi|Kalan CPU Kredisi|Sayı|Ortalama|Veri bloğunun kullanabileceği toplam kredi sayısı|Hiç boyut|
|Tüketilen CPU Kredisi|Tüketilen CPU Kredisi|Sayı|Ortalama|Sanal Makine tarafından tüketilen toplam kredi sayısı|Hiç boyut|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU Yüzdesi|CPU Yüzdesi|Yüzde|Ortalama|Sanal Makineler tarafından kullanılan ayrılmış işlem birimlerinin geçerli yüzdesi|Hiç boyut|
|Ağ Girişi|Ağ Girişi|Bayt|Toplam|Sanal Makineler tarafından tüm ağ arabirimleri üzerinde alınan bayt sayısı (Gelen Trafik)|Hiç boyut|
|Ağ Çıkışı|Ağ Çıkışı|Bayt|Toplam|Sanal Makineler tarafından tüm ağ arabirimleri üzerinde gönderilen bayt sayısı (Giden Trafik)|Hiç boyut|
|Diskten Okunan Bayt|Diskten Okunan Bayt|Bayt|Toplam|İzleme dönemi boyunca diskten okunan toplam bayt|Hiç boyut|
|Diske Yazılan Bayt|Diske Yazılan Bayt|Bayt|Toplam|İzleme dönemi boyunca diske yazılan toplam bayt|Hiç boyut|
|Disk Okuma İşlemi/Sn|Disk Okuma İşlemi/Sn|CountPerSecond|Ortalama|Disk Okuma IOPS|Hiç boyut|
|Disk Yazma İşlemi/Sn|Disk Yazma İşlemi/Sn|CountPerSecond|Ortalama|Disk Yazma IOPS|Hiç boyut|
|Kalan CPU Kredisi|Kalan CPU Kredisi|Sayı|Ortalama|Veri bloğunun kullanabileceği toplam kredi sayısı|Hiç boyut|
|Tüketilen CPU Kredisi|Tüketilen CPU Kredisi|Sayı|Ortalama|Sanal Makine tarafından tüketilen toplam kredi sayısı|Hiç boyut|

## <a name="microsoftcustomerinsightshubs"></a>Microsoft.CustomerInsights/hubs

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|DCIApiCalls|Müşteri Öngörüler API çağrıları|Sayı|Toplam||Hiç boyut|
|DCIMappingImportOperationSuccessfulLines|Eşleme içeri aktarma işlemi başarılı satırları|Sayı|Toplam||Hiç boyut|
|DCIMappingImportOperationFailedLines|Satırları alma işlemi eşleme işlemi başarısız oldu|Sayı|Toplam||Hiç boyut|
|DCIMappingImportOperationTotalLines|Eşleme içeri aktarma işlemi toplam satırları|Sayı|Toplam||Hiç boyut|
|DCIMappingImportOperationRuntimeInSeconds|Eşleme alma işlemi çalışma zamanı saniye cinsinden|Saniye|Toplam||Hiç boyut|
|DCIOutboundProfileExportSucceeded|Giden profili dışarı aktarma başarılı oldu|Sayı|Toplam||Hiç boyut|
|DCIOutboundProfileExportFailed|Giden profili dışarı aktarma başarısız oldu|Sayı|Toplam||Hiç boyut|
|DCIOutboundProfileExportDuration|Giden profili verme süresi|Saniye|Toplam||Hiç boyut|
|DCIOutboundKpiExportSucceeded|Giden KPI dışarı aktarma başarılı oldu|Sayı|Toplam||Hiç boyut|
|DCIOutboundKpiExportFailed|Giden KPI verme başarısız oldu|Sayı|Toplam||Hiç boyut|
|DCIOutboundKpiExportDuration|Giden KPI verme süresi|Saniye|Toplam||Hiç boyut|
|DCIOutboundKpiExportStarted|Giden KPI verme başlatıldı|Saniye|Toplam||Hiç boyut|
|DCIOutboundKpiRecordCount|Giden KPI kayıt sayısı|Saniye|Toplam||Hiç boyut|
|DCIOutboundProfileExportCount|Giden profil verme sayısı|Saniye|Toplam||Hiç boyut|
|DCIOutboundInitialProfileExportFailed|Giden ilk profili dışarı aktarma başarısız oldu|Saniye|Toplam||Hiç boyut|
|DCIOutboundInitialProfileExportSucceeded|Giden ilk profili dışarı aktarma başarılı oldu|Saniye|Toplam||Hiç boyut|
|DCIOutboundInitialKpiExportFailed|Giden ilk KPI verme başarısız oldu|Saniye|Toplam||Hiç boyut|
|DCIOutboundInitialKpiExportSucceeded|Giden ilk KPI dışarı aktarma başarılı oldu|Saniye|Toplam||Hiç boyut|
|DCIOutboundInitialProfileExportDurationInSeconds|Giden ilk profil verme süresini saniye cinsinden|Saniye|Toplam||Hiç boyut|
|AdlaJobForStandardKpiFailed|Saniye cinsinden başarısız standart KPI için adla işi|Saniye|Toplam||Hiç boyut|
|AdlaJobForStandardKpiTimeOut|Standart KPI saniye olarak zaman aşımı için adla işi|Saniye|Toplam||Hiç boyut|
|AdlaJobForStandardKpiCompleted|Saniye cinsinden tamamlandı standart KPI için adla işi|Saniye|Toplam||Hiç boyut|
|ImportASAValuesFailed|İçeri aktarma ASA değerleri sayımı başarısız oldu|Sayı|Toplam||Hiç boyut|
|ImportASAValuesSucceeded|İçeri aktarma ASA değerleri Count başarılı oldu|Sayı|Toplam||Hiç boyut|
|DCIProfilesCount|Profil örnek sayısı|Sayı|Son||Hiç boyut|
|DCIInteractionsPerMonthCount|Etkileşimleri başına ay sayısı|Sayı|Son||Hiç boyut|
|DCIKpisCount|KPI sayısı|Sayı|Son||Hiç boyut|
|DCISegmentsCount|Segment Sayısı|Sayı|Son||Hiç boyut|
|DCIPredictiveMatchPoliciesCount|Tahmine dayalı eşleşme sayısı|Sayı|Son||Hiç boyut|
|DCIPredictionsCount|Tahmin sayısı|Sayı|Son||Hiç boyut|

## <a name="microsoftdatafactorydatafactories"></a>Microsoft.DataFactory/datafactories

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SuccessfulRuns|Başarılı Çalışma Sayısı|Sayı|Toplam|Başarılı çalıştırır sayısı.|Hiç boyut|
|FailedRuns|Başarısız Çalışma Sayısı|Sayı|Toplam|Başarısız çalıştırır sayısı.|Hiç boyut|

## <a name="microsoftdatafactoryfactories"></a>Microsoft.DataFactory/factories

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|PipelineFailedRuns|Ardışık Düzen çalıştırır ölçümleri başarısız oldu|Sayı|Toplam||Hiç boyut|
|PipelineSucceededRuns|Ardışık Düzen çalıştırır ölçümleri başarılı oldu|Sayı|Toplam||Hiç boyut|
|ActivityFailedRuns|Etkinlik çalıştığında ölçümleri başarısız oldu|Sayı|Toplam||Hiç boyut|
|ActivitySucceededRuns|Etkinlik çalıştığında ölçümleri başarılı oldu|Sayı|Toplam||Hiç boyut|
|TriggerFailedRuns|Tetikleyici çalıştırır ölçümleri başarısız oldu|Sayı|Toplam||Hiç boyut|
|TriggerSucceededRuns|Tetikleyici çalıştırır ölçümleri başarılı oldu|Sayı|Toplam||Hiç boyut|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|JobEndedSuccess|Başarılı işler|Sayı|Toplam|Başarılı işlerin sayısı.|Hiç boyut|
|JobEndedFailure|Başarısız olan işler|Sayı|Toplam|Başarısız olan işler sayısı.|Hiç boyut|
|JobEndedCancelled|İptal edilen işler|Sayı|Toplam|İptal edilen işlerin sayısı.|Hiç boyut|
|JobAUEndedSuccess|Başarılı AU zamanı|Saniye|Toplam|Toplam AU süredir başarılı işler.|Hiç boyut|
|JobAUEndedFailure|Başarısız AU zamanı|Saniye|Toplam|Başarısız olan işler toplam AU süresi.|Hiç boyut|
|JobAUEndedCancelled|İptal edilen AU zamanı|Saniye|Toplam|İptal edilen işler için toplam AU süre.|Hiç boyut|

## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TotalStorage|Toplam depolama alanı|Bayt|Maksimum|Hesapta depolanan verilere toplam miktarı.|Hiç boyut|
|DataWritten|Yazılan veriler|Bayt|Toplam|Toplam hesabına yazılan veri miktarı.|Hiç boyut|
|DataRead|Okunan Veriler|Bayt|Toplam|Toplam veri miktarını hesabından okuyun.|Hiç boyut|
|WriteRequests|İstekleri Yazın|Sayı|Toplam|Hesaba veri sayısı yazma isteği.|Hiç boyut|
|ReadRequests|İstekleri Okuyun|Sayı|Toplam|Veri sayısı hesabına istekleri okuyun.|Hiç boyut|

## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|Hiç boyut|
|compute_limit|Birim sınırı işlem|Sayı|Ortalama|Birim sınırı işlem|Hiç boyut|
|compute_consumption_percent|Birim yüzdesi işlem|Yüzde|Ortalama|Birim yüzdesi işlem|Hiç boyut|
|memory_percent|Bellek yüzdesi|Yüzde|Ortalama|Bellek yüzdesi|Hiç boyut|
|io_consumption_percent|G/ç yüzdesi|Yüzde|Ortalama|G/ç yüzdesi|Hiç boyut|
|storage_percent|Depolama yüzdesi|Yüzde|Ortalama|Depolama yüzdesi|Hiç boyut|
|storage_used|Kullanılan depolama|Bayt|Ortalama|Kullanılan depolama|Hiç boyut|
|storage_limit|Depolama sınırı|Bayt|Ortalama|Depolama sınırı|Hiç boyut|
|active_connections|Toplam etkin bağlantılar|Sayı|Ortalama|Toplam etkin bağlantılar|Hiç boyut|
|connections_failed|Toplam başarısız bağlantıları|Sayı|Ortalama|Toplam başarısız bağlantıları|Hiç boyut|

## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|Hiç boyut|
|compute_limit|Birim sınırı işlem|Sayı|Ortalama|Birim sınırı işlem|Hiç boyut|
|compute_consumption_percent|Birim yüzdesi işlem|Yüzde|Ortalama|Birim yüzdesi işlem|Hiç boyut|
|memory_percent|Bellek yüzdesi|Yüzde|Ortalama|Bellek yüzdesi|Hiç boyut|
|io_consumption_percent|G/ç yüzdesi|Yüzde|Ortalama|G/ç yüzdesi|Hiç boyut|
|storage_percent|Depolama yüzdesi|Yüzde|Ortalama|Depolama yüzdesi|Hiç boyut|
|storage_used|Kullanılan depolama|Bayt|Ortalama|Kullanılan depolama|Hiç boyut|
|storage_limit|Depolama sınırı|Bayt|Ortalama|Depolama sınırı|Hiç boyut|
|active_connections|Toplam etkin bağlantılar|Sayı|Ortalama|Toplam etkin bağlantılar|Hiç boyut|
|connections_failed|Toplam başarısız bağlantıları|Sayı|Ortalama|Toplam başarısız bağlantıları|Hiç boyut|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Telemetri ileti gönderme denemeleri|Sayı|Toplam|IOT hub'ınıza gönderilecek cihaz bulut telemetri iletilerini sayısı çalıştı|Hiç boyut|
|d2c.telemetry.ingress.Success|Gönderilen telemetri iletilerini|Sayı|Toplam|Başarıyla IOT hub'ına gönderilen cihaz bulut telemetri iletisi sayısı|Hiç boyut|
|c2d.Commands.egress.Complete.Success|Tamamlanan komutları|Sayı|Toplam|Aygıt tarafından başarıyla tamamlandı bulut cihaz komutlarının sayısı|Hiç boyut|
|c2d.Commands.egress.Abandon.Success|Terk komutları|Sayı|Toplam|Aygıt tarafından terk bulut cihaz komutlarının sayısı|Hiç boyut|
|c2d.Commands.egress.Reject.Success|Reddedilen komutları|Sayı|Toplam|Aygıt tarafından reddedilen bulut cihaz komutlarının sayısı|Hiç boyut|
|devices.totalDevices|Toplam aygıt|Sayı|Toplam|IOT hub'ınıza kayıtlı cihaz sayısı|Hiç boyut|
|devices.connectedDevices.allProtocol|Bağlı aygıtlar|Sayı|Toplam|IOT hub'ına bağlı aygıt sayısı|Hiç boyut|
|d2c.telemetry.egress.Success|Telemetri iletilerini teslim|Sayı|Toplam|İletiler (toplam) uç noktalara başarıyla yazılmış sayısı|Hiç boyut|
|d2c.telemetry.egress.dropped|Bırakılan iletiler|Sayı|Toplam|Teslim endpoint ölü olduğundan bırakılan ileti sayısı|Hiç boyut|
|d2c.telemetry.egress.orphaned|Yalnız bırakılmış ileti|Sayı|Toplam|Geri dönüş yolu da dahil olmak üzere tüm yollar eşleşmeyen ileti sayısı|Hiç boyut|
|d2c.telemetry.egress.invalid|Geçersiz iletileri|Sayı|Toplam|Uç noktası ile uyumsuzluğu nedeniyle teslim edilmedi ileti sayısı|Hiç boyut|
|d2c.telemetry.egress.fallback|Geri dönüş koşulla eşleşen iletileri|Sayı|Toplam|Geri dönüş uç noktasına yazılan ileti sayısı|Hiç boyut|
|d2c.endpoints.egress.eventHubs|Olay hub'ı uç noktaları için teslim edilen ileti|Sayı|Toplam|İletiler için olay hub'ı uç noktaları başarıyla yazılmış sayısı|Hiç boyut|
|d2c.endpoints.latency.eventHubs|Olay hub'ı uç noktaları için ileti gecikme süresi|Milisaniye|Ortalama|Milisaniye cinsinden bir olay hub'ı uç içine ileti giriş IOT hub'ına ve ileti giriş arasındaki ortalama gecikme süresi|Hiç boyut|
|d2c.endpoints.egress.serviceBusQueues|Hizmet veri yolu kuyruğu uç noktaları için teslim edilen ileti|Sayı|Toplam|İletileri başarıyla hizmet veri yolu kuyruğu Uç noktalara yazılmış sayısı|Hiç boyut|
|d2c.endpoints.latency.serviceBusQueues|Hizmet veri yolu kuyruğu uç noktalar için ileti gecikme süresi|Milisaniye|Ortalama|Milisaniye cinsinden bir hizmet veri yolu kuyruğu uç noktası içine ileti giriş IOT hub'ına ve ileti giriş arasındaki ortalama gecikme süresi|Hiç boyut|
|d2c.endpoints.egress.serviceBusTopics|Hizmet veri yolu konusu uç noktaları için teslim edilen ileti|Sayı|Toplam|İletileri başarıyla hizmet veri yolu konusu Uç noktalara yazılmış sayısı|Hiç boyut|
|d2c.endpoints.latency.serviceBusTopics|Hizmet veri yolu konusu uç noktalar için ileti gecikme süresi|Milisaniye|Ortalama|Milisaniye cinsinden bir hizmet veri yolu konusu uç noktası içine ileti giriş IOT hub'ına ve ileti giriş arasındaki ortalama gecikme süresi|Hiç boyut|
|d2c.endpoints.egress.builtIn.events|Yerleşik uç noktası (iletileri/olayları) teslim edilen ileti|Sayı|Toplam|İletileri (iletileri/olayları) yerleşik uç noktası başarıyla yazılmış sayısı|Hiç boyut|
|d2c.endpoints.latency.builtIn.events|Yerleşik uç noktası (iletileri/olayları) için ileti gecikme süresi|Milisaniye|Ortalama|Milisaniye cinsinden yerleşik uç noktası (iletileri/olayları) içine ileti giriş IOT hub'ına ve ileti giriş arasındaki ortalama gecikme süresi |Hiç boyut|
|d2c.endpoints.egress.Storage|Depolama uç noktaları için teslim edilen ileti|Sayı|Toplam|İletileri depolama Uç noktalara başarıyla yazılmış sayısı|Hiç boyut|
|d2c.endpoints.latency.Storage|Depolama uç noktaları için ileti gecikme süresi|Milisaniye|Ortalama|Milisaniye cinsinden bir depolama uç noktası içine ileti giriş IOT hub'ına ve ileti giriş arasındaki ortalama gecikme süresi|Hiç boyut|
|d2c.endpoints.egress.Storage.bytes|Depolama alanına yazılan veriler|Bayt|Toplam|Veri depolama uç noktaları için yazılan bayt miktarı|Hiç boyut|
|d2c.endpoints.egress.Storage.BLOBS|BLOB Depolama birimine yazıldı|Sayı|Toplam|BLOB storage uç noktaları için yazılmış sayısı|Hiç boyut|
|d2c.Twin.Read.Success|Başarılı twin aygıtlardan okur|Sayı|Toplam|Tüm başarılı aygıt tarafından başlatılan twin sayısını okur.|Hiç boyut|
|d2c.Twin.Read.failure|Aygıtlardan Twin okuma başarısız oldu|Sayı|Toplam|Tüm sayısı aygıt tarafından başlatılan twin okuma başarısız oldu.|Hiç boyut|
|d2c.Twin.Read.size|Aygıtlardan twin yanıt boyutu okur|Bayt|Ortalama|Cihaz tarafından başlatılan ortalama, min ve max tüm başarılı değeri okuma çifti.|Hiç boyut|
|d2c.Twin.Update.Success|Aygıtlardan başarılı twin güncelleştirmeleri|Sayı|Toplam|Tüm başarılı twin aygıt tarafından başlatılan güncelleştirme sayısı.|Hiç boyut|
|d2c.Twin.Update.failure|Aygıtlardan Twin güncelleştirmeler başarısız oldu|Sayı|Toplam|Tüm sayısı aygıt tarafından başlatılan twin güncelleştirmeler başarısız oldu.|Hiç boyut|
|d2c.Twin.Update.size|Aygıtlardan twin güncelleştirmeleri boyutu|Bayt|Ortalama|Cihaz tarafından başlatılan ortalama, min ve max boyutu tüm başarılı güncelleştirmeleri çifti.|Hiç boyut|
|c2d.methods.Success|Başarılı doğrudan yöntem çağrıları|Sayı|Toplam|Tüm başarılı doğrudan yöntemi çağrı sayısı.|Hiç boyut|
|c2d.methods.failure|Doğrudan yöntem çağrılarını başarısız oldu|Sayı|Toplam|Tüm sayısı doğrudan yöntem çağrıları başarısız oldu.|Hiç boyut|
|c2d.methods.requestSize|Doğrudan yöntem çağrılarını isteği boyutu|Bayt|Ortalama|Ortalama, min ve max tüm başarılı doğrudan yöntemi istekleri.|Hiç boyut|
|c2d.methods.responseSize|Doğrudan yöntem çağrılarını yanıt boyutu|Bayt|Ortalama|Ortalama, min ve max tüm başarılı doğrudan yöntemi yanıtların.|Hiç boyut|
|c2d.Twin.Read.Success|Arka ucunuzdan başarılı twin okur|Sayı|Toplam|Tüm başarılı arka uç başlatılan twin sayısını okur.|Hiç boyut|
|c2d.Twin.Read.failure|Arka ucunuzdan başarısız twin okur|Sayı|Toplam|Tüm sayısı arka uç başlatılan twin okuma başarısız oldu.|Hiç boyut|
|c2d.Twin.Read.size|Arka uç twin okuma yanıt boyutu|Bayt|Ortalama|Arka uç başlatılan ortalama, min ve max tüm başarılı değeri okuma çifti.|Hiç boyut|
|c2d.Twin.Update.Success|Arka uç başarılı twin güncelleştirmeleri|Sayı|Toplam|Tüm başarılı twin arka uç başlatılan güncelleştirme sayısı.|Hiç boyut|
|c2d.Twin.Update.failure|Arka uç başarısız twin güncelleştirmeleri|Sayı|Toplam|Tüm sayısı arka uç başlatılan twin güncelleştirmeler başarısız oldu.|Hiç boyut|
|c2d.Twin.Update.size|Arka uç twin güncelleştirmelerini boyutu|Bayt|Ortalama|Arka uç başlatılan ortalama, min ve max boyutu tüm başarılı güncelleştirmeleri çifti.|Hiç boyut|
|twinQueries.success|Başarılı twin sorguları|Sayı|Toplam|Tüm başarılı twin sorguları sayısı.|Hiç boyut|
|twinQueries.failure|Başarısız twin sorguları|Sayı|Toplam|Tüm başarısız twin sorguları sayısı.|Hiç boyut|
|twinQueries.resultSize|Twin sorguları sonuç boyutu|Bayt|Ortalama|Ortalama, min ve max tüm başarılı twin sorgular sonuç boyutunun.|Hiç boyut|
|jobs.createTwinUpdateJob.success|Twin güncelleştirme işlerinin başarılı oluşturma|Sayı|Toplam|Tüm başarılı twin güncelleştirme işlerinin oluşturulması sayısı.|Hiç boyut|
|jobs.createTwinUpdateJob.failure|Twin güncelleştirme işlerinin başarısız oluşturma|Sayı|Toplam|Tüm oluşturma işlemi başarısız twin güncelleştirme işlerinin sayısı.|Hiç boyut|
|jobs.createDirectMethodJob.success|Yöntem çağırma işlerinin başarılı oluşturma|Sayı|Toplam|Tüm başarılı bir şekilde oluşturulduktan doğrudan yöntemi çağırma işlerin sayısı.|Hiç boyut|
|jobs.createDirectMethodJob.failure|Yöntem çağırma işlerinin başarısız oluşturma|Sayı|Toplam|Tüm oluşturma işlemi başarısız doğrudan yöntemi çağırma işlerin sayısı.|Hiç boyut|
|jobs.listJobs.success|Liste işleri başarılı çağrıları|Sayı|Toplam|Liste işleri için tüm başarılı çağrı sayısı.|Hiç boyut|
|jobs.listJobs.failure|Liste işleri çağrı başarısız oldu|Sayı|Toplam|Tüm başarısız çağrılar listesi işlerinin sayısı.|Hiç boyut|
|jobs.cancelJob.success|Başarılı iş iptalleri|Sayı|Toplam|Bir işi iptal etmek için tüm başarılı çağrı sayısı.|Hiç boyut|
|jobs.cancelJob.failure|Başarısız iş iptalleri|Sayı|Toplam|Bir işi iptal etmek için tüm başarısız çağrı sayısı.|Hiç boyut|
|jobs.queryJobs.success|İş başarılı sorguları|Sayı|Toplam|Sorgu işlerinin tüm başarılı çağrı sayısı.|Hiç boyut|
|jobs.queryJobs.failure|Başarısız işi sorgular|Sayı|Toplam|Tüm başarısız çağrılar sorgu işlerinin sayısı.|Hiç boyut|
|Jobs.Completed|Tamamlanan işler|Sayı|Toplam|Tüm tamamlanmış işlerin sayısı.|Hiç boyut|
|Jobs.Failed|Başarısız olan işler|Sayı|Toplam|Tüm başarısız işler sayısı.|Hiç boyut|
|d2c.telemetry.ingress.sendThrottle|Azaltma hatalarının sayısı|Sayı|Toplam|Kısıtlama hataları aygıt işleme nedeniyle sayısını kısıtlar|Hiç boyut|
|dailyMessageQuotaUsed|Kullanılan iletilerin toplam sayısı|Sayı|Ortalama|Günümüzde kullanılan toplam ileti sayısı. Bu, sıfırlanır, toplu bir değerdir her gün 00:00 UTC adresindeki.|Hiç boyut|

## <a name="microsoftdevicesprovisioningservices"></a>Microsoft.Devices/provisioningServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|RegistrationAttempts|Kayıt denemeleri|Sayı|Toplam|Çalıştığınız cihaz kayıt sayısı|ProvisioningServiceName, IotHubName, durumu|
|DeviceAssignments|Atanan aygıtlar|Sayı|Toplam|Bir IOT hub'ına atanmış cihazların sayısı|ProvisioningServiceName, IotHubName|
|AttestationAttempts|Kanıtlama denemeleri|Sayı|Toplam|Aygıt attestations denemesi sayısı|ProvisioningServiceName, durumu, Protokolü|

## <a name="microsoftdeviceselasticpools"></a>Microsoft.Devices/ElasticPools

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|elasticPool.requestedUsageRate|İstenen kullanım oranı|Yüzde|Ortalama|İstenen kullanım oranı|Hiç boyut|

## <a name="microsoftdeviceselasticpoolsiothubtenants"></a>Microsoft.Devices/ElasticPools/IotHubTenants

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|tenantHub.requestedUsageRate|İstenen kullanım oranı|Yüzde|Ortalama|İstenen kullanım oranı|Hiç boyut|
|deviceDataUsage|Toplam devicedata kullanımı|Sayı|Toplam|Iothub'a bağlı cihazlara giden ve gelen bayt|Hiç boyut|
|d2c.telemetry.ingress.allProtocol|Telemetri ileti gönderme denemeleri|Sayı|Toplam|IOT hub'ınıza gönderilecek cihaz bulut telemetri iletilerini sayısı çalıştı|Hiç boyut|
|d2c.telemetry.ingress.Success|Gönderilen telemetri iletilerini|Sayı|Toplam|Başarıyla IOT hub'ına gönderilen cihaz bulut telemetri iletisi sayısı|Hiç boyut|
|c2d.Commands.egress.Complete.Success|Tamamlanan komutları|Sayı|Toplam|Aygıt tarafından başarıyla tamamlandı bulut cihaz komutlarının sayısı|Hiç boyut|
|c2d.Commands.egress.Abandon.Success|Terk komutları|Sayı|Toplam|Aygıt tarafından terk bulut cihaz komutlarının sayısı|Hiç boyut|
|c2d.Commands.egress.Reject.Success|Reddedilen komutları|Sayı|Toplam|Aygıt tarafından reddedilen bulut cihaz komutlarının sayısı|Hiç boyut|
|devices.totalDevices|Toplam aygıt|Sayı|Toplam|IOT hub'ınıza kayıtlı cihaz sayısı|Hiç boyut|
|devices.connectedDevices.allProtocol|Bağlı aygıtlar|Sayı|Toplam|IOT hub'ına bağlı aygıt sayısı|Hiç boyut|
|d2c.telemetry.egress.Success|Telemetri iletilerini teslim|Sayı|Toplam|İletiler (toplam) uç noktalara başarıyla yazılmış sayısı|Hiç boyut|
|d2c.telemetry.egress.dropped|Bırakılan iletiler|Sayı|Toplam|Teslim endpoint ölü olduğundan bırakılan ileti sayısı|Hiç boyut|
|d2c.telemetry.egress.orphaned|Yalnız bırakılmış ileti|Sayı|Toplam|Geri dönüş yolu da dahil olmak üzere tüm yollar eşleşmeyen ileti sayısı|Hiç boyut|
|d2c.telemetry.egress.invalid|Geçersiz iletileri|Sayı|Toplam|Uç noktası ile uyumsuzluğu nedeniyle teslim edilmedi ileti sayısı|Hiç boyut|
|d2c.telemetry.egress.fallback|Geri dönüş koşulla eşleşen iletileri|Sayı|Toplam|Geri dönüş uç noktasına yazılan ileti sayısı|Hiç boyut|
|d2c.endpoints.egress.eventHubs|Olay hub'ı uç noktaları için teslim edilen ileti|Sayı|Toplam|İletiler için olay hub'ı uç noktaları başarıyla yazılmış sayısı|Hiç boyut|
|d2c.endpoints.latency.eventHubs|Olay hub'ı uç noktaları için ileti gecikme süresi|Milisaniye|Ortalama|Milisaniye cinsinden bir olay hub'ı uç içine ileti giriş IOT hub'ına ve ileti giriş arasındaki ortalama gecikme süresi|Hiç boyut|
|d2c.endpoints.egress.serviceBusQueues|Hizmet veri yolu kuyruğu uç noktaları için teslim edilen ileti|Sayı|Toplam|İletileri başarıyla hizmet veri yolu kuyruğu Uç noktalara yazılmış sayısı|Hiç boyut|
|d2c.endpoints.latency.serviceBusQueues|Hizmet veri yolu kuyruğu uç noktalar için ileti gecikme süresi|Milisaniye|Ortalama|Milisaniye cinsinden bir hizmet veri yolu kuyruğu uç noktası içine ileti giriş IOT hub'ına ve ileti giriş arasındaki ortalama gecikme süresi|Hiç boyut|
|d2c.endpoints.egress.serviceBusTopics|Hizmet veri yolu konusu uç noktaları için teslim edilen ileti|Sayı|Toplam|İletileri başarıyla hizmet veri yolu konusu Uç noktalara yazılmış sayısı|Hiç boyut|
|d2c.endpoints.latency.serviceBusTopics|Hizmet veri yolu konusu uç noktalar için ileti gecikme süresi|Milisaniye|Ortalama|Milisaniye cinsinden bir hizmet veri yolu konusu uç noktası içine ileti giriş IOT hub'ına ve ileti giriş arasındaki ortalama gecikme süresi|Hiç boyut|
|d2c.endpoints.egress.builtIn.events|Yerleşik uç noktası (iletileri/olayları) teslim edilen ileti|Sayı|Toplam|İletileri (iletileri/olayları) yerleşik uç noktası başarıyla yazılmış sayısı|Hiç boyut|
|d2c.endpoints.latency.builtIn.events|Yerleşik uç noktası (iletileri/olayları) için ileti gecikme süresi|Milisaniye|Ortalama|Milisaniye cinsinden yerleşik uç noktası (iletileri/olayları) içine ileti giriş IOT hub'ına ve ileti giriş arasındaki ortalama gecikme süresi |Hiç boyut|
|d2c.endpoints.egress.Storage|Depolama uç noktaları için teslim edilen ileti|Sayı|Toplam|İletileri depolama Uç noktalara başarıyla yazılmış sayısı|Hiç boyut|
|d2c.endpoints.latency.Storage|Depolama uç noktaları için ileti gecikme süresi|Milisaniye|Ortalama|Milisaniye cinsinden bir depolama uç noktası içine ileti giriş IOT hub'ına ve ileti giriş arasındaki ortalama gecikme süresi|Hiç boyut|
|d2c.endpoints.egress.Storage.bytes|Depolama alanına yazılan veriler|Bayt|Toplam|Veri depolama uç noktaları için yazılan bayt miktarı|Hiç boyut|
|d2c.endpoints.egress.Storage.BLOBS|BLOB Depolama birimine yazıldı|Sayı|Toplam|BLOB storage uç noktaları için yazılmış sayısı|Hiç boyut|
|d2c.Twin.Read.Success|Başarılı twin aygıtlardan okur|Sayı|Toplam|Tüm başarılı aygıt tarafından başlatılan twin sayısını okur.|Hiç boyut|
|d2c.Twin.Read.failure|Aygıtlardan Twin okuma başarısız oldu|Sayı|Toplam|Tüm sayısı aygıt tarafından başlatılan twin okuma başarısız oldu.|Hiç boyut|
|d2c.Twin.Read.size|Aygıtlardan twin yanıt boyutu okur|Bayt|Ortalama|Cihaz tarafından başlatılan ortalama, min ve max tüm başarılı değeri okuma çifti.|Hiç boyut|
|d2c.Twin.Update.Success|Aygıtlardan başarılı twin güncelleştirmeleri|Sayı|Toplam|Tüm başarılı twin aygıt tarafından başlatılan güncelleştirme sayısı.|Hiç boyut|
|d2c.Twin.Update.failure|Aygıtlardan Twin güncelleştirmeler başarısız oldu|Sayı|Toplam|Tüm sayısı aygıt tarafından başlatılan twin güncelleştirmeler başarısız oldu.|Hiç boyut|
|d2c.Twin.Update.size|Aygıtlardan twin güncelleştirmeleri boyutu|Bayt|Ortalama|Cihaz tarafından başlatılan ortalama, min ve max boyutu tüm başarılı güncelleştirmeleri çifti.|Hiç boyut|
|c2d.methods.Success|Başarılı doğrudan yöntem çağrıları|Sayı|Toplam|Tüm başarılı doğrudan yöntemi çağrı sayısı.|Hiç boyut|
|c2d.methods.failure|Doğrudan yöntem çağrılarını başarısız oldu|Sayı|Toplam|Tüm sayısı doğrudan yöntem çağrıları başarısız oldu.|Hiç boyut|
|c2d.methods.requestSize|Doğrudan yöntem çağrılarını isteği boyutu|Bayt|Ortalama|Ortalama, min ve max tüm başarılı doğrudan yöntemi istekleri.|Hiç boyut|
|c2d.methods.responseSize|Doğrudan yöntem çağrılarını yanıt boyutu|Bayt|Ortalama|Ortalama, min ve max tüm başarılı doğrudan yöntemi yanıtların.|Hiç boyut|
|c2d.Twin.Read.Success|Arka ucunuzdan başarılı twin okur|Sayı|Toplam|Tüm başarılı arka uç başlatılan twin sayısını okur.|Hiç boyut|
|c2d.Twin.Read.failure|Arka ucunuzdan başarısız twin okur|Sayı|Toplam|Tüm sayısı arka uç başlatılan twin okuma başarısız oldu.|Hiç boyut|
|c2d.Twin.Read.size|Arka uç twin okuma yanıt boyutu|Bayt|Ortalama|Arka uç başlatılan ortalama, min ve max tüm başarılı değeri okuma çifti.|Hiç boyut|
|c2d.Twin.Update.Success|Arka uç başarılı twin güncelleştirmeleri|Sayı|Toplam|Tüm başarılı twin arka uç başlatılan güncelleştirme sayısı.|Hiç boyut|
|c2d.Twin.Update.failure|Arka uç başarısız twin güncelleştirmeleri|Sayı|Toplam|Tüm sayısı arka uç başlatılan twin güncelleştirmeler başarısız oldu.|Hiç boyut|
|c2d.Twin.Update.size|Arka uç twin güncelleştirmelerini boyutu|Bayt|Ortalama|Arka uç başlatılan ortalama, min ve max boyutu tüm başarılı güncelleştirmeleri çifti.|Hiç boyut|
|twinQueries.success|Başarılı twin sorguları|Sayı|Toplam|Tüm başarılı twin sorguları sayısı.|Hiç boyut|
|twinQueries.failure|Başarısız twin sorguları|Sayı|Toplam|Tüm başarısız twin sorguları sayısı.|Hiç boyut|
|twinQueries.resultSize|Twin sorguları sonuç boyutu|Bayt|Ortalama|Ortalama, min ve max tüm başarılı twin sorgular sonuç boyutunun.|Hiç boyut|
|jobs.createTwinUpdateJob.success|Twin güncelleştirme işlerinin başarılı oluşturma|Sayı|Toplam|Tüm başarılı twin güncelleştirme işlerinin oluşturulması sayısı.|Hiç boyut|
|jobs.createTwinUpdateJob.failure|Twin güncelleştirme işlerinin başarısız oluşturma|Sayı|Toplam|Tüm oluşturma işlemi başarısız twin güncelleştirme işlerinin sayısı.|Hiç boyut|
|jobs.createDirectMethodJob.success|Yöntem çağırma işlerinin başarılı oluşturma|Sayı|Toplam|Tüm başarılı bir şekilde oluşturulduktan doğrudan yöntemi çağırma işlerin sayısı.|Hiç boyut|
|jobs.createDirectMethodJob.failure|Yöntem çağırma işlerinin başarısız oluşturma|Sayı|Toplam|Tüm oluşturma işlemi başarısız doğrudan yöntemi çağırma işlerin sayısı.|Hiç boyut|
|jobs.listJobs.success|Liste işleri başarılı çağrıları|Sayı|Toplam|Liste işleri için tüm başarılı çağrı sayısı.|Hiç boyut|
|jobs.listJobs.failure|Liste işleri çağrı başarısız oldu|Sayı|Toplam|Tüm başarısız çağrılar listesi işlerinin sayısı.|Hiç boyut|
|jobs.cancelJob.success|Başarılı iş iptalleri|Sayı|Toplam|Bir işi iptal etmek için tüm başarılı çağrı sayısı.|Hiç boyut|
|jobs.cancelJob.failure|Başarısız iş iptalleri|Sayı|Toplam|Bir işi iptal etmek için tüm başarısız çağrı sayısı.|Hiç boyut|
|jobs.queryJobs.success|İş başarılı sorguları|Sayı|Toplam|Sorgu işlerinin tüm başarılı çağrı sayısı.|Hiç boyut|
|jobs.queryJobs.failure|Başarısız işi sorgular|Sayı|Toplam|Tüm başarısız çağrılar sorgu işlerinin sayısı.|Hiç boyut|
|Jobs.Completed|Tamamlanan işler|Sayı|Toplam|Tüm tamamlanmış işlerin sayısı.|Hiç boyut|
|Jobs.Failed|Başarısız olan işler|Sayı|Toplam|Tüm başarısız işler sayısı.|Hiç boyut|
|d2c.telemetry.ingress.sendThrottle|Azaltma hatalarının sayısı|Sayı|Toplam|Kısıtlama hataları aygıt işleme nedeniyle sayısını kısıtlar|Hiç boyut|
|dailyMessageQuotaUsed|Kullanılan iletilerin toplam sayısı|Sayı|Ortalama|Günümüzde kullanılan toplam ileti sayısı. Bu, sıfırlanır, toplu bir değerdir her gün 00:00 UTC adresindeki.|Hiç boyut|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.DocumentDB/databaseAccounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|MetadataRequests|Meta veri istekleri|Sayı|Sayı|Meta veri isteği sayısı. Cosmos DB tutar koleksiyonları, veritabanları, vb. listeleme olanak sağlayan, her hesap için sistem meta veri toplama ve yapılandırmalarına, ücretsiz olarak.|GlobalDatabaseAccountName, DatabaseName, CollectionName, bölge, StatusCode|
|MongoRequestCharge|Mongo isteği ücret|Sayı|Toplam|Tüketilen mongo istek birimleri|GlobalDatabaseAccountName, DatabaseName, CollectionName, bölge, CommandName, hata kodu|
|MongoRequests|Mongo istekleri|Sayı|Sayı|Yapılan Mongo isteklerinin sayısı|GlobalDatabaseAccountName, DatabaseName, CollectionName, bölge, CommandName, hata kodu|
|TotalRequestUnits|Toplam istek birimleri|Sayı|Toplam|Birimleri tüketilen isteği|GlobalDatabaseAccountName, DatabaseName, CollectionName, bölge, StatusCode|
|TotalRequests|Toplam İstek Sayısı|Sayı|Sayı|Yapılan isteklerin sayısı|GlobalDatabaseAccountName, DatabaseName, CollectionName, bölge, StatusCode|


## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SuccessfulRequests|Başarılı istekler (Önizleme)|Sayı|Toplam|Microsoft.EventHub için Başarılı İstekler. (Önizleme)|EntityName|
|ServerErrors|Sunucu Hataları. (Önizleme)|Sayı|Toplam|Microsoft.EventHub için Sunucu Hataları. (Önizleme)|EntityName|
|UserErrors|Kullanıcı Hataları. (Önizleme)|Sayı|Toplam|Microsoft.EventHub için Kullanıcı Hataları. (Önizleme)|EntityName|
|QuotaExceededErrors|Kota Aşım Hataları. (Önizleme)|Sayı|Toplam|Microsoft.EventHub için Kota Aşım Hataları. (Önizleme)|EntityName|
|ThrottledRequests|Daraltılmış İstekler. (Önizleme)|Sayı|Toplam|Microsoft.EventHub için Daraltılmış İstekler. (Önizleme)|EntityName|
|IncomingRequests|Gelen istekleri (Önizleme)|Sayı|Toplam|Microsoft.EventHub Gelen İstekler. (Önizleme)|EntityName|
|IncomingMessages|Gelen iletileri (Önizleme)|Sayı|Toplam|Microsoft.EventHub için Gelen iletiler. (Önizleme)|EntityName|
|OutgoingMessages|Giden iletiler (Önizleme)|Sayı|Toplam|Microsoft.EventHub Giden İletiler. (Önizleme)|EntityName|
|IncomingBytes|Gelen Baytlar. (Önizleme)|Bayt|Toplam|Microsoft.EventHub için Gelen Baytlar. (Önizleme)|EntityName|
|OutgoingBytes|Giden Baytlar. (Önizleme)|Bayt|Toplam|Microsoft.EventHub için Giden Baytlar. (Önizleme)|EntityName|
|ActiveConnections|ActiveConnections (Önizleme)|Sayı|Toplam|Microsoft.EventHub için Toplam Etkin Bağlantı. (Önizleme)|EntityName|
|ConnectionsOpened|Açılan Bağlantılar. (Önizleme)|Sayı|Toplam|Microsoft.EventHub için Açılan Bağlantılar. (Önizleme)|EntityName|
|ConnectionsClosed|Kapatılan Bağlantılar. (Önizleme)|Sayı|Toplam|Microsoft.EventHub için Kapatılan Bağlantılar. (Önizleme)|EntityName|
|CaptureBacklog|Kapsam yakalayın. (Önizleme)|Sayı|Toplam|Microsoft.EventHub için kapsam yakalayın. (Önizleme)|EntityName|
|CapturedMessages|Yakalanan İletiler. (Önizleme)|Sayı|Toplam|Microsoft.EventHub için Yakalanan İletiler. (Önizleme)|EntityName|
|CapturedBytes|Yakalanan Baytlar. (Önizleme)|Bayt|Toplam|Microsoft.EventHub için Yakalanan Baytlar. (Önizleme)|EntityName|
|INREQS|Gelen İstekler|Sayı|Toplam|Bir ad alanı için gelen toplam gönderme isteği|Hiç boyut|
|SUCCREQ|Başarılı İstekler|Sayı|Toplam|Bir ad alanı için toplam başarılı istek sayısı|Hiç boyut|
|FAILREQ|Başarısız Olan İstekler|Sayı|Toplam|Bir ad alanı için toplam başarısız istek sayısı|Hiç boyut|
|SVRBSY|Sunucu Meşgul Hataları|Sayı|Toplam|Bir ad alanı için sunucu meşgul hatalarının toplam sayısı|Hiç boyut|
|INTERR|İç Sunucu Hataları|Sayı|Toplam|Bir ad alanı için toplam iç sunucu hatası sayısı|Hiç boyut|
|MISCERR|Diğer Hatalar|Sayı|Toplam|Bir ad alanı için toplam başarısız istek sayısı|Hiç boyut|
|INMSGS|Gelen İletiler (Kullanım dışı)|Sayı|Toplam|Bir ad alanı için toplam gelen iletileri. Bu ölçüm kullanım dışıdır. Lütfen gelen iletileri ölçüm kullanın|Hiç boyut|
|EHINMSGS|Gelen İletiler|Sayı|Toplam|Bir ad alanı için toplam gelen ileti sayısı|Hiç boyut|
|OUTMSGS|Giden İletiler (Kullanım dışı)|Sayı|Toplam|Toplam giden bir ad alanı için iletileri. Bu ölçüm kullanım dışıdır. Lütfen giden iletiler ölçüm kullanın|Hiç boyut|
|EHOUTMSGS|Giden İletiler|Sayı|Toplam|Bir ad alanı için toplam giden ileti sayısı|Hiç boyut|
|EHINMBS|Gelen bayt (Kullanım dışı)|Bayt|Toplam|Olay hub'ı gelen ileti üretilen iş için bir ad alanı. Bu ölçüm kullanım dışıdır. Lütfen gelen bayt ölçüm kullanın|Hiç boyut|
|EHINBYTES|Gelen bayt|Bayt|Toplam|Bir ad alanı için Olay Hub'ı gelen ileti işleme hızı|Hiç boyut|
|EHOUTMBS|Giden bayt (Kullanım dışı)|Bayt|Toplam|Olay hub'ı giden ileti üretilen iş için bir ad alanı. Bu ölçüm kullanım dışıdır. Lütfen Giden bayt ölçüm kullanın|Hiç boyut|
|EHOUTBYTES|Giden bayt|Bayt|Toplam|Bir ad alanı için Olay Hub'ı giden ileti işleme hızı|Hiç boyut|
|EHABL|Kapsam mesajlarını arşivle|Sayı|Toplam|Bir ad alanı için kapsamda arşivlenen Olay Hub'ı iletileri|Hiç boyut|
|EHAMSGS|Mesajları arşivleme|Sayı|Toplam|Bir ad alanında Olay Hub'ı tarafından arşivlenen iletiler|Hiç boyut|
|EHAMBS|Mesaj işlemelerini arşivle|Bayt|Toplam|Bir ad alanında Olay Hub'ı tarafından arşivlenen ileti işleme hızı|Hiç boyut|

## <a name="microsoftinsightsautoscalesettings"></a>Microsoft.Insights/AutoscaleSettings

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ObservedMetricValue|Gözlenen Ölçüm Değeri|Sayı|Ortalama|Otomatik ölçeklendirme yürütüldüğünde hesaplanan değer|MetricTriggerSource|
|MetricThreshold|Ölçüm Eşiği|Sayı|Ortalama|Otomatik ölçeklendirme çalıştırıldığında yapılandırılan otomatik ölçek eşiği.|MetricTriggerRule|
|ObservedCapacity|Gözlenen Kapasite|Sayı|Ortalama|Otomatik ölçeklendirme yürütüldüğünde bildirilen kapasite.|Hiç boyut|
|ScaleActionsInitiated|Başlatılan Ölçeklendirme Eylemleri|Sayı|Toplam|Ölçeklendirme işleminin yönü.|ScaleDirection|

## <a name="microsoftlocationbasedservicesaccounts"></a>Microsoft.LocationBasedServices/accounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Gecikme süresi|Gecikme süresi|Milisaniye|Ortalama|API çağrılarının|OperationName, OperationResult|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|RunsStarted|Başlatılan Çalıştırmalar|Sayı|Toplam|Başlatılan iş akışı çalıştırma sayısı.|Hiç boyut|
|RunsCompleted|Tamamlanan Çalıştırmalar|Sayı|Toplam|Tamamlanan iş akışı çalıştırma sayısı.|Hiç boyut|
|RunsSucceeded|Başarılı Çalıştırmalar|Sayı|Toplam|Başarılı iş akışı çalıştırma sayısı.|Hiç boyut|
|RunsFailed|Başarısız Çalıştırmalar|Sayı|Toplam|Başarısız iş akışı çalıştırma sayısı.|Hiç boyut|
|RunsCancelled|İptal Edilen Çalıştırmalar|Sayı|Toplam|İptal edilen iş akışı çalıştırma sayısı.|Hiç boyut|
|RunLatency|Çalıştırma Gecikmesi|Saniye|Ortalama|Tamamlanan iş akışı çalıştırmalarının gecikme süresi.|Hiç boyut|
|RunSuccessLatency|Başarılı Çalıştırma Gecikmesi|Saniye|Ortalama|Başarılı iş akışı çalıştırmalarının gecikme süresi.|Hiç boyut|
|RunThrottledEvents|Çalıştırılması Kısıtlanan Olaylar|Sayı|Toplam|İş akışı eylemi veya tetikleyiciyle kısıtlanan olay sayısı.|Hiç boyut|
|RunFailurePercentage|Çalıştırma Hatası Yüzdesi|Yüzde|Toplam|Başarısız olan iş akışı çalıştırmalarının yüzdesi.|Hiç boyut|
|ActionsStarted|Başlatılan Eylemler |Sayı|Toplam|Başlatılan iş akışı eylemi sayısı.|Hiç boyut|
|ActionsCompleted|Tamamlanan Eylemler |Sayı|Toplam|Tamamlanan iş akışı eylemi sayısı.|Hiç boyut|
|ActionsSucceeded|Başarılı Eylemler |Sayı|Toplam|Başarılı iş akışı eylemi sayısı.|Hiç boyut|
|ActionsFailed|Başarısız Eylemler|Sayı|Toplam|Başarısız iş akışı eylemi sayısı.|Hiç boyut|
|ActionsSkipped|Atlanan Eylemler |Sayı|Toplam|Atlanan iş akışı eylemi sayısı.|Hiç boyut|
|ActionLatency|Eylem Gecikmesi |Saniye|Ortalama|Tamamlanan iş akışı eylemlerinin gecikme süresi.|Hiç boyut|
|ActionSuccessLatency|Başarılı Eylem Gecikmesi |Saniye|Ortalama|Başarılı iş akışı eylemlerinin gecikme süresi.|Hiç boyut|
|ActionThrottledEvents|Eylemi Kısıtlanan Olaylar|Sayı|Toplam|İş akışı eylemi kısıtlanan olay sayısı.|Hiç boyut|
|TriggersStarted|Başlatılan Tetikleyiciler |Sayı|Toplam|Başlatılan iş akışı tetikleyicisi sayısı.|Hiç boyut|
|TriggersCompleted|Tamamlanan Tetikleyiciler |Sayı|Toplam|Tamamlanan iş akışı tetikleyicisi sayısı.|Hiç boyut|
|TriggersSucceeded|Başarılı Olan Tetikleyiciler |Sayı|Toplam|Başarılı iş akışı tetikleyicisi sayısı.|Hiç boyut|
|TriggersFailed|Başarısız Olan Tetikleyiciler |Sayı|Toplam|Başarısız iş akışı tetikleyicisi sayısı.|Hiç boyut|
|TriggersSkipped|Atlanan Tetikleyiciler|Sayı|Toplam|Atlanan iş akışı tetikleyicisi sayısı.|Hiç boyut|
|TriggersFired|Tetiklenen Tetikleyiciler |Sayı|Toplam|Harekete geçirilen iş akışı tetikleyicisi sayısı.|Hiç boyut|
|TriggerLatency|Tetikleyici Gecikme Süresi |Saniye|Ortalama|Tamamlanan iş akışı tetikleyicilerinin gecikmesi.|Hiç boyut|
|TriggerFireLatency|Tetikleyici Tetikleme Gecikme Süresi |Saniye|Ortalama|Başlatılan iş akışı tetikleyicilerinin gecikmesi.|Hiç boyut|
|TriggerSuccessLatency|Tetikleyici Başarı Gecikme Süresi |Saniye|Ortalama|Başarılı iş akışı tetikleyicilerinin gecikmesi.|Hiç boyut|
|TriggerThrottledEvents|Tetikleyici Kısıtlanan Olaylar|Sayı|Toplam|İş akışı tetikleyicisiyle kısıtlanan olay sayısı.|Hiç boyut|
|BillableActionExecutions|Faturalanabilir Eylem Yürütmeleri|Sayı|Toplam|Faturalandırılan iş akışı eylemi yürütmelerinin sayısı.|Hiç boyut|
|BillableTriggerExecutions|Faturalanabilir Tetikleyici Yürütmeleri|Sayı|Toplam|Faturalandırılan iş akışı tetikleyicisi yürütmelerinin sayısı.|Hiç boyut|
|TotalBillableExecutions|Toplam Faturalanabilir Yürütme Sayısı|Sayı|Toplam|Faturalandırılan iş akışı yürütmelerinin sayısı.|Hiç boyut|

## <a name="microsoftnetworknetworkinterfaces"></a>Microsoft.Network/networkInterfaces

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BytesSentRate|Gönderilen bayt|Sayı|Toplam|Ağ arabirimi gönderilen bayt sayısı|Hiç boyut|
|BytesReceivedRate|Alınan bayt|Sayı|Toplam|Bayt alınan ağ arabirimi|Hiç boyut|
|PacketsSentRate|Gönderilen paketleri|Sayı|Toplam|Ağ arabirimi gönderilen paket sayısı|Hiç boyut|
|PacketsReceivedRate|Alınan paket sayısı|Sayı|Toplam|Ağ arabirimi alınan paket sayısı|Hiç boyut|

## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|VipAvailability|VIP kullanılabilirliği|Sayı|Ortalama|VIP uç noktaları, araştırma sonuçlarına dayalı kullanılabilirliği|Vipaddress'sinden, VipPort|
|DipAvailability|DIP kullanılabilirliği|Sayı|Ortalama|Araştırma sonuçlarına dayalı DIP uç kullanılabilirliği|ProtocolType, DipPort, Vipaddress'sinden, VipPort, DipAddress|
|ByteCount|Bayt sayısı|Sayı|Toplam|Toplam süre içinde aktarılan bayt sayısı|Vipaddress'sinden, VipPort, yönü|
|PacketCount|Paket sayısı|Sayı|Toplam|Toplam süre içinde gönderilen paket sayısı|Vipaddress'sinden, VipPort, yönü|
|SYNCount|Eşitlemeye sayısı|Sayı|Toplam|Eşitlemeye süre içinde aktarılan paketlerin toplam sayısı|Vipaddress'sinden, VipPort, yönü|
|SnatConnectionCount|SNAT bağlantısı sayısı|Sayı|Toplam|Toplam süre içinde oluşturulan yeni SNAT bağlantı sayısı|Vipaddress'sinden, DipAddress, ConnectionState|

## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAddresses

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|PacketsInDDoS|Gelen paket DDoS|CountPerSecond|Maksimum|Gelen paket DDoS|Hiç boyut|
|PacketsDroppedDDoS|Gelen paket DDoS bırakıldı|CountPerSecond|Maksimum|Gelen paket DDoS bırakıldı|Hiç boyut|
|PacketsForwardedDDoS|DDoS iletilen gelen paketler|CountPerSecond|Maksimum|DDoS iletilen gelen paketler|Hiç boyut|
|TCPPacketsInDDoS|Gelen TCP paketleri DDoS|CountPerSecond|Maksimum|Gelen TCP paketleri DDoS|Hiç boyut|
|TCPPacketsDroppedDDoS|DDoS bırakılan gelen TCP paketleri|CountPerSecond|Maksimum|DDoS bırakılan gelen TCP paketleri|Hiç boyut|
|TCPPacketsForwardedDDoS|DDoS iletilen gelen TCP paketleri|CountPerSecond|Maksimum|DDoS iletilen gelen TCP paketleri|Hiç boyut|
|UDPPacketsInDDoS|UDP paketlerini DDoS gelen|CountPerSecond|Maksimum|UDP paketlerini DDoS gelen|Hiç boyut|
|UDPPacketsDroppedDDoS|DDoS bırakılan gelen UDP paketleri|CountPerSecond|Maksimum|DDoS bırakılan gelen UDP paketleri|Hiç boyut|
|UDPPacketsForwardedDDoS|DDoS iletilen gelen UDP paketleri|CountPerSecond|Maksimum|DDoS iletilen gelen UDP paketleri|Hiç boyut|
|BytesInDDoS|Gelen bayt DDoS|BytesPerSecond|Maksimum|Gelen bayt DDoS|Hiç boyut|
|BytesDroppedDDoS|Gelen bayt DDoS bırakıldı|BytesPerSecond|Maksimum|Gelen bayt DDoS bırakıldı|Hiç boyut|
|BytesForwardedDDoS|DDoS iletilen gelen bayt|BytesPerSecond|Maksimum|DDoS iletilen gelen bayt|Hiç boyut|
|TCPBytesInDDoS|Gelen TCP bayt DDoS|BytesPerSecond|Maksimum|Gelen TCP bayt DDoS|Hiç boyut|
|TCPBytesDroppedDDoS|Gelen TCP bayt DDoS bırakıldı|BytesPerSecond|Maksimum|Gelen TCP bayt DDoS bırakıldı|Hiç boyut|
|TCPBytesForwardedDDoS|DDoS iletilen gelen TCP bayt|BytesPerSecond|Maksimum|DDoS iletilen gelen TCP bayt|Hiç boyut|
|UDPBytesInDDoS|UDP bayt DDoS gelen|BytesPerSecond|Maksimum|UDP bayt DDoS gelen|Hiç boyut|
|UDPBytesDroppedDDoS|Gelen UDP bayt DDoS bırakıldı|BytesPerSecond|Maksimum|Gelen UDP bayt DDoS bırakıldı|Hiç boyut|
|UDPBytesForwardedDDoS|DDoS iletilen gelen UDP bayt|BytesPerSecond|Maksimum|DDoS iletilen gelen UDP bayt|Hiç boyut|
|IfUnderDDoSAttack|DDoS altında saldırı veya değil|Sayı|Maksimum|DDoS altında saldırı veya değil|Hiç boyut|
|DDoSTriggerTCPPackets|DDoS azaltma tetiklemek için gelen TCP paketleri|CountPerSecond|Maksimum|DDoS azaltma tetiklemek için gelen TCP paketleri|Hiç boyut|
|DDoSTriggerUDPPackets|UDP paketlerini DDoS azaltma tetiklemek için gelen|CountPerSecond|Maksimum|UDP paketlerini DDoS azaltma tetiklemek için gelen|Hiç boyut|
|DDoSTriggerSYNPackets|Gelen DDoS azaltma tetiklemek için Eşitlemeye paketleri|CountPerSecond|Maksimum|Gelen DDoS azaltma tetiklemek için Eşitlemeye paketleri|Hiç boyut|
|VipAvailability|Kullanılabilirlik|Sayı|Ortalama|Ortalama IPADDRESS kullanılabilirlik süre içinde|Bağlantı noktası|
|ByteCount|Bayt sayısı|Sayı|Toplam|Toplam süre içinde aktarılan bayt sayısı|Bağlantı noktası, yönü|
|PacketCount|Paket sayısı|Sayı|Toplam|Toplam süre içinde gönderilen paket sayısı|Bağlantı noktası, yönü|
|SynCount|Eşitlemeye sayısı|Sayı|Toplam|Eşitlemeye süre içinde aktarılan paketlerin toplam sayısı|Bağlantı noktası, yönü|

## <a name="microsoftnetworkvirtualnetworks"></a>Microsoft.Network/virtualNetworks

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|PacketsInDroppedVMProtection|Bırakılan gelen paketleri için VM koruma|CountPerSecond|Ortalama|Bırakılan gelen paketleri için VM koruma|Hiç boyut|
|PacketsOutDroppedVMProtection|Giden paketler için VM koruma bırakıldı|CountPerSecond|Ortalama|Giden paketler için VM koruma bırakıldı|Hiç boyut|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Aktarım hızı|Aktarım hızı|BytesPerSecond|Toplam|Uygulama ağ geçidi hizmet verdi Saniyedeki bayt sayısı|Hiç boyut|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.Network/virtualNetworkGateways

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TunnelAverageBandwidth|Tünel bant genişliği|BytesPerSecond|Ortalama|Bir tünel Saniyedeki bayt cinsinden ortalama bant genişliği|ConnectionName, RemoteIP|
|TunnelEgressBytes|Tünel çıkış bayt|Bayt|Toplam|Bir tünel Giden bayt sayısı|ConnectionName, RemoteIP|
|TunnelIngressBytes|Tünel giriş bayt|Bayt|Toplam|Bir tünel gelen bayt sayısı|ConnectionName, RemoteIP|
|TunnelEgressPackets|Tünel çıkış paketlerinin|Sayı|Toplam|Bir tünel giden paket sayısı|ConnectionName, RemoteIP|
|TunnelIngressPackets|Tünel giriş paketlerinin|Sayı|Toplam|Bir tünel gelen paket sayısı|ConnectionName, RemoteIP|
|TunnelEgressPacketDropTSMismatch|Tünel çıkış TS uyuşmazlığı paket bırakma|Sayı|Toplam|Gelen trafik Seçici uyuşmazlığı bir tünel giden paket bırakma sayısı|ConnectionName, RemoteIP|
|TunnelIngressPacketDropTSMismatch|Tünel giriş TS uyuşmazlığı paket bırakma|Sayı|Toplam|Trafik Seçici uyuşmazlığı bir tünel öğesinden gelen paket bırakma sayısı|ConnectionName, RemoteIP|

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Ortalama|Saniye başına BITS ingressing Azure|Hiç boyut|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Ortalama|Saniye başına Azure egressing BITS|Hiç boyut|

## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|QpsByEndpoint|Sorgular tarafından döndürülen uç noktası|Sayı|Toplam|Trafik Yöneticisi uç noktası belirtilen zaman çerçevesinde döndürülen sayısı|endpointName|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Bitiş noktası tarafından uç nokta durumu|Sayı|Maksimum|uç noktanın araştırma durumu "etkinse," 1, 0 Aksi takdirde.|endpointName|

## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|registration.all|Kayıt İşlemleri|Sayı|Toplam|Tüm başarılı kayıt işlemlerinin sayısı (oluşturma, güncelleştirme, sorgu ve silme işlemleri). |Hiç boyut|
|registration.Create|Kayıt Oluşturma İşlemleri|Sayı|Toplam|Tüm başarılı kayıt oluşturma işlemlerinin sayısı.|Hiç boyut|
|registration.Update|Kayıt Güncelleme İşlemleri|Sayı|Toplam|Tüm başarılı kayıt güncelleştirmelerinin sayısı.|Hiç boyut|
|registration.get|Kayıt Okuma İşlemleri|Sayı|Toplam|Tüm başarılı kayıt sorgularının sayısı.|Hiç boyut|
|registration.Delete|Kayıt Silme İşlemleri|Sayı|Toplam|Tüm başarılı kayıt silme işlemlerinin sayısı.|Hiç boyut|
|gelen|Gelen İletiler|Sayı|Toplam|Başarılı olan tüm gönderme API'si çağrılarının sayısı. |Hiç boyut|
|incoming.Scheduled|Zamanlanan Anında İletme Bildirimleri Gönderildi|Sayı|Toplam|Zamanlanan Anında İletme Bildirimleri İptal Edildi|Hiç boyut|
|incoming.Scheduled.Cancel|Zamanlanan Anında İletme Bildirimleri İptal Edildi|Sayı|Toplam|Zamanlanan Anında İletme Bildirimleri İptal Edildi|Hiç boyut|
|Scheduled.Pending|Bekleyen Zamanlanmış Bildirimler|Sayı|Toplam|Bekleyen Zamanlanmış Bildirimler|Hiç boyut|
|installation.all|Yükleme Yönetimi İşlemleri|Sayı|Toplam|Yükleme Yönetimi İşlemleri|Hiç boyut|
|installation.get|Yükleme İşlemlerini Al|Sayı|Toplam|Yükleme İşlemlerini Al|Hiç boyut|
|installation.upsert|Yükleme İşlemleri Oluştur veya Güncelleştir|Sayı|Toplam|Yükleme İşlemleri Oluştur veya Güncelleştir|Hiç boyut|
|installation.Patch|Yükleme İşlemlerine Düzeltme Eki Uygula|Sayı|Toplam|Yükleme İşlemlerine Düzeltme Eki Uygula|Hiç boyut|
|installation.Delete|Yükleme İşlemlerini Sil|Sayı|Toplam|Yükleme İşlemlerini Sil|Hiç boyut|
|outgoing.allpns.Success|Başarılı bildirimler|Sayı|Toplam|Başarılı olan tüm bildirimlerin sayısı.|Hiç boyut|
|outgoing.allpns.invalidpayload|Yük Hataları|Sayı|Toplam|PNS kötü yük hatası döndürdüğü için başarısız olan gönderim sayısı.|Hiç boyut|
|outgoing.allpns.pnserror|Harici Bildirim Sistemi Hataları|Sayı|Toplam|PNS ile iletişim kurmada sorun yaşandığı için başarısız olan gönderim sayısı (kimlik doğrulaması sorunları hariç).|Hiç boyut|
|outgoing.allpns.channelerror|Kanal Hataları|Sayı|Toplam|Kanal geçersiz olduğu, doğru uygulamayla ilişkili olmadığı, kısıtlandığı veya kanalın süresi dolduğu için başarısız olan gönderim sayısı.|Hiç boyut|
|outgoing.allpns.badorexpiredchannel|Hatalı veya Süresi Dolmuş Kanal Hataları|Sayı|Toplam|Kayıttaki kanal/belirteç/registrationId geçersiz olduğundan veya süresi dolduğundan başarısız olan gönderim sayısı.|Hiç boyut|
|outgoing.WNS.Success|WNS Başarılı Bildirimler|Sayı|Toplam|Başarılı olan tüm bildirimlerin sayısı.|Hiç boyut|
|outgoing.WNS.invalidcredentials|WNS Yetkilendirme Hataları (Geçersiz Kimlik Bilgileri)|Sayı|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediği veya kimlik bilgileri engellendiği için başarısız olan gönderim sayısı. (Windows Live kimlik bilgilerini tanımıyor).|Hiç boyut|
|outgoing.WNS.badchannel|WNS Geçersiz Kanal Hatası|Sayı|Toplam|Kayıttaki ChannelURI tanınmadığı için başarısız olan gönderim sayısı (WNS durumu: 404 bulunamadı).|Hiç boyut|
|outgoing.WNS.expiredchannel|WNS Süresi Dolan Kanal Hatası|Sayı|Toplam|ChannelURI'nin süresi dolduğu için başarısız olan gönderim sayısı (WNS durumu: 410 Kaybedildi).|Hiç boyut|
|outgoing.WNS.throttled|WNS Kısıtlanan Bildirimler|Sayı|Toplam|WNS bu uygulamayı kısıtladığı için başarısız olan gönderim sayısı (WNS durumu: 406 Kabul Edilemez).|Hiç boyut|
|outgoing.WNS.tokenproviderunreachable|WNS Yetkilendirme Hataları (Ulaşılamıyor)|Sayı|Toplam|Windows Live hizmetine erişilemiyor.|Hiç boyut|
|outgoing.WNS.invalidtoken|WNS Yetkilendirme Hataları (Geçersiz Belirteç)|Sayı|Toplam|WNS'ye sağlanan belirteç geçerli değil (WNS durumu: 401 Yetkisiz).|Hiç boyut|
|outgoing.WNS.wrongtoken|WNS Yetkilendirme Hataları (Yanlış Belirteç)|Sayı|Toplam|WNS için sağlanan belirteç geçerli ancak başka bir uygulama için (WNS durumu: 403 Yasak). Kayıtta ChannelURI başka bir uygulama ile ilişkili ise bu durum oluşabilir. İstemci uygulaması aynı uygulama kimlik bilgilerini bildirim hub'ı ile ilişkili olup olmadığını denetleyin.|Hiç boyut|
|outgoing.WNS.invalidnotificationformat|WNS Geçersiz Bildirim Biçimi|Sayı|Toplam|Bildirim biçimi geçersiz (WNS durumu: 400). WNS tüm geçersiz yüklerini Reddet değil unutmayın.|Hiç boyut|
|outgoing.WNS.invalidnotificationsize|WNS Geçersiz Bildirim Boyutu Hatası|Sayı|Toplam|Bildirim yükü fazla büyük (WNS durumu: 413).|Hiç boyut|
|outgoing.WNS.channelthrottled|WNS Kanal Kısıtlandı|Sayı|Toplam|Bildirim, kayıttaki ChannelURI kısıtlandığı için bırakıldı (WNS yanıt üst bilgisi: X-WNS-NotificationStatus:channelThrottled).|Hiç boyut|
|outgoing.WNS.channeldisconnected|WNS Kanal Bağlantısı Kesildi|Sayı|Toplam|Bildirim, kayıttaki ChannelURI kısıtlandığı için bırakıldı (WNS yanıt üst bilgisi: X-WNS-DeviceConnectionStatus: bağlantısı kesildi).|Hiç boyut|
|outgoing.WNS.dropped|WNS Bırakılan Bildirimler|Sayı|Toplam|Bildirim, kayıttaki ChannelURI kısıtlandığı için bırakıldı (X-WNS-NotificationStatus: bırakıldı, ancak X-WNS-DeviceConnectionStatus: bağlantısı kesildi durumda değil).|Hiç boyut|
|outgoing.WNS.pnserror|WNS Hataları|Sayı|Toplam|Bildirim, WNS ile iletişim kurulurken oluşan hatalardan dolayı teslim edilemedi.|Hiç boyut|
|outgoing.WNS.authenticationerror|WNS Kimlik Doğrulaması Hataları|Sayı|Toplam|Bildirim, Windows Live ile iletişim kurulurken oluşan hatalardan, geçersiz kimlik bilgilerinden veya yanlış belirteçten dolayı teslim edilemedi.|Hiç boyut|
|outgoing.apns.Success|APNS Başarılı Bildirimler|Sayı|Toplam|Başarılı olan tüm bildirimlerin sayısı.|Hiç boyut|
|outgoing.apns.invalidcredentials|APNS Yetkilendirme Hataları|Sayı|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediği veya kimlik bilgileri engellendiği için başarısız olan gönderim sayısı.|Hiç boyut|
|outgoing.apns.badchannel|APNS Geçersiz Kanal Hatası|Sayı|Toplam|Belirteç geçersiz olduğu için başarısız olan gönderim sayısı (APNS durum kodu: 8).|Hiç boyut|
|outgoing.apns.expiredchannel|APNS Süresi Dolan Kanal Hatası|Sayı|Toplam|APNS geri bildirim kanalı tarafından geçersiz kılınan belirteç sayısı.|Hiç boyut|
|outgoing.apns.invalidnotificationsize|APNS Geçersiz Bildirim Boyutu Hatası|Sayı|Toplam|Yük fazla büyük olduğu için başarısız olan gönderim sayısı (APNS durum kodu: 7).|Hiç boyut|
|outgoing.apns.pnserror|APNS Hataları|Sayı|Toplam|APNS ile iletişim kurulurken oluşan hatalardan dolayı başarısız olan gönderim sayısı.|Hiç boyut|
|outgoing.GCM.Success|GCM Başarılı Bildirimler|Sayı|Toplam|Başarılı olan tüm bildirimlerin sayısı.|Hiç boyut|
|outgoing.GCM.invalidcredentials|GCM Yetkilendirme Hataları (Geçersiz Kimlik Bilgileri)|Sayı|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediği veya kimlik bilgileri engellendiği için başarısız olan gönderim sayısı.|Hiç boyut|
|outgoing.GCM.badchannel|GCM Geçersiz Kanal Hatası|Sayı|Toplam|Kayıttaki registrationId tanınmadığı için başarısız olan gönderim sayısı (GCM sonucu: Invalid Registration).|Hiç boyut|
|outgoing.GCM.expiredchannel|GCM Süresi Dolan Kanal Hatası|Sayı|Toplam|Kayıttaki registrationId geçersiz olduğu veya süresi dolduğu için başarısız olan gönderim sayısı (GCM sonucu: NotRegistered).|Hiç boyut|
|outgoing.GCM.throttled|GCM Kısıtlanan Bildirimler|Sayı|Toplam|GCM bu uygulamayı kısıtladığı için başarısız olan gönderim sayısı (GCM durum kodu: 501-599 veya sonucu: Unavailable).|Hiç boyut|
|outgoing.GCM.invalidnotificationformat|GCM Geçersiz Bildirim Biçimi|Sayı|Toplam|Yük doğru bir şekilde biçimlendirilmediği için başarısız olan gönderim sayısı (GCM sonucu: InvalidDataKey veya InvalidTtl).|Hiç boyut|
|outgoing.GCM.invalidnotificationsize|GCM Geçersiz Bildirim Boyutu Hatası|Sayı|Toplam|Yük fazla büyük olduğu için başarısız olan gönderim sayısı (GCM sonucu: MessageTooBig).|Hiç boyut|
|outgoing.GCM.wrongchannel|GCM Yanlış Kanal Hatası|Sayı|Toplam|Kayıttaki registrationId geçerli uygulamayla ilişkili olmadığı için başarısız olan gönderim sayısı (GCM sonucu: InvalidPackageName).|Hiç boyut|
|outgoing.GCM.pnserror|GCM Hataları|Sayı|Toplam|GCM ile iletişim kurulurken oluşan hatalardan dolayı başarısız olan gönderim sayısı.|Hiç boyut|
|outgoing.GCM.authenticationerror|GCM Kimlik Doğrulaması Hataları|Sayı|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediği, kimlik bilgileri engellendiği veya SenderId uygulamada doğru bir şekilde yapılandırılmadığı için başarısız olan gönderim sayısı (GCM sonucu: MismatchedSenderId).|Hiç boyut|
|outgoing.mpns.Success|MPNS Başarılı Bildirimler|Sayı|Toplam|Başarılı olan tüm bildirimlerin sayısı.|Hiç boyut|
|outgoing.mpns.invalidcredentials|MPNS Geçersiz Kimlik Bilgileri|Sayı|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediği veya kimlik bilgileri engellendiği için başarısız olan gönderim sayısı.|Hiç boyut|
|outgoing.mpns.badchannel|MPNS Geçersiz Kanal Hatası|Sayı|Toplam|Kayıttaki ChannelURI tanınmadığı için başarısız olan gönderim sayısı (MPNS durumu: 404 bulunamadı).|Hiç boyut|
|outgoing.mpns.throttled|MPNS Kısıtlanan Bildirimler|Sayı|Toplam|MPNS bu uygulamayı kısıtladığı için başarısız olan gönderim sayısı (WNS MPNS: 406 Kabul Edilemez).|Hiç boyut|
|outgoing.mpns.invalidnotificationformat|MPNS Geçersiz Bildirim Biçimi|Sayı|Toplam|Bildirimin yükü fazla büyük olduğu için başarısız olan gönderim sayısı.|Hiç boyut|
|outgoing.mpns.channeldisconnected|MPNS Kanalın Bağlantısı Kesildi|Sayı|Toplam|Kayıttaki ChannelURI'nin bağlantısı kesildiği için başarısız olan gönderim sayısı (MPNS durumu: 412 bulunamadı).|Hiç boyut|
|outgoing.mpns.dropped|MPNS Bırakılan Bildirimler|Sayı|Toplam|MPNS tarafından bırakılan gönderim sayısı (MPNS yanıt üst bilgisi: X-NotificationStatus: QueueFull veya Suppressed).|Hiç boyut|
|outgoing.mpns.pnserror|MPNS Hataları|Sayı|Toplam|MPNS ile iletişim kurulurken oluşan hatalardan dolayı başarısız olan gönderim sayısı.|Hiç boyut|
|outgoing.mpns.authenticationerror|MPNS Kimlik Doğrulaması Hataları|Sayı|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediği veya kimlik bilgileri engellendiği için başarısız olan gönderim sayısı.|Hiç boyut|
|notificationhub.pushes|Tüm Giden Bildirimler|Sayı|Toplam|Bildirim hub'ındaki tüm giden bildirimler|Hiç boyut|
|incoming.all.Requests|Tüm Gelen İstekler|Sayı|Toplam|Bildirim hub'ı için toplam gelen istek sayısı|Hiç boyut|
|incoming.all.failedrequests|Tüm Gelen Başarısız İstekler|Sayı|Toplam|Bildirim hub'ı için toplam gelen başarısız istek sayısı|Hiç boyut|

## <a name="microsoftrelaynamespaces"></a>Microsoft.Relay/namespaces

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ListenerConnections-Success|ListenerConnections-Success|Sayı|Toplam|Microsoft.Relay için başarılı ListenerConnections.|EntityName|
|ListenerConnections-ClientError|ListenerConnections-ClientError|Sayı|Toplam|Microsoft.Relay için ListenerConnections'da ClientError.|EntityName|
|ListenerConnections-ServerError|ListenerConnections-ServerError|Sayı|Toplam|Microsoft.Relay için ListenerConnections'da ServerError.|EntityName|
|SenderConnections-Success|SenderConnections-Success|Sayı|Toplam|Microsoft.Relay için başarılı SenderConnections.|EntityName|
|SenderConnections-ClientError|SenderConnections-ClientError|Sayı|Toplam|Microsoft.Relay için SenderConnections'da ClientError.|EntityName|
|SenderConnections-ServerError|SenderConnections-ServerError|Sayı|Toplam|Microsoft.Relay için SenderConnections ServerError.|EntityName|
|ListenerConnections-TotalRequests|ListenerConnections-TotalRequests|Sayı|Toplam|Microsoft.Relay için toplam ListenerConnection sayısı.|EntityName|
|SenderConnections-TotalRequests|SenderConnections-TotalRequests|Sayı|Toplam|Microsoft.Relay için toplam SenderConnections istekleri sayısı.|EntityName|
|ActiveConnections|ActiveConnections|Sayı|Toplam|Microsoft.Relay için toplam ActiveConnection sayısı.|EntityName|
|ActiveListeners|ActiveListeners|Sayı|Toplam|Microsoft.Relay için toplam ActiveListener sayısı.|EntityName|
|BytesTransferred|BytesTransferred|Sayı|Toplam|Microsoft.Relay için toplam BytesTransferred sayısı.|EntityName|
|ListenerDisconnects|ListenerDisconnects|Sayı|Toplam|Microsoft.Relay için ListenerDisconnect sayısı.|EntityName|
|SenderDisconnects|SenderDisconnects|Sayı|Toplam|Microsoft.Relay için toplam SenderDisconnect sayısı.|EntityName|

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SearchLatency|Arama gecikme süresi|Saniye|Ortalama|Arama hizmeti için ortalama arama gecikme süresi|Hiç boyut|
|SearchQueriesPerSecond|Saniye başına arama sorguları|CountPerSecond|Ortalama|Arama hizmeti için saniye başına arama sorguları|Hiç boyut|
|ThrottledSearchQueriesPercentage|Kısıtlanmış arama sorguları yüzdesi|Yüzde|Ortalama|Arama hizmeti için kısıtlanan arama sorguları yüzdesi|Hiç boyut|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SuccessfulRequests|Başarılı istekler (Önizleme)|Sayı|Toplam|Ad alanı (Önizleme) toplam başarılı istekler|EntityName|
|ServerErrors|Sunucu Hataları. (Önizleme)|Sayı|Toplam|Microsoft.ServiceBus için Sunucu Hataları. (Önizleme)|EntityName|
|UserErrors|Kullanıcı Hataları. (Önizleme)|Sayı|Toplam|Microsoft.ServiceBus için Kullanıcı Hataları. (Önizleme)|EntityName|
|ThrottledRequests|Daraltılmış İstekler. (Önizleme)|Sayı|Toplam|Microsoft.ServiceBus için Daraltılmış İstekler. (Önizleme)|EntityName|
|IncomingRequests|Gelen istekleri (Önizleme)|Sayı|Toplam|Microsoft.ServiceBus için Gelen İstekler. (Önizleme)|EntityName|
|IncomingMessages|Gelen iletileri (Önizleme)|Sayı|Toplam|Microsoft.ServiceBus için Gelen İletiler. (Önizleme)|EntityName|
|OutgoingMessages|Giden iletiler (Önizleme)|Sayı|Toplam|Microsoft.ServiceBus için Giden İletiler. (Önizleme)|EntityName|
|ActiveConnections|ActiveConnections (Önizleme)|Sayı|Toplam|Microsoft.ServiceBus için Toplam Etkin Bağlantılar. (Önizleme)|EntityName|
|ConnectionsOpened|Açılan Bağlantılar. (Önizleme)|Sayı|Toplam|Microsoft.ServiceBus için Açılan Bağlantılar. (Önizleme)|EntityName|
|ConnectionsClosed|Kapatılan Bağlantılar. (Önizleme)|Sayı|Toplam|Microsoft.ServiceBus için Kapatılan Bağlantılar. (Önizleme)|EntityName|
|CPUXNS|Ad alanı başına CPU kullanımı|Yüzde|Maksimum|Service Bus premium ad alanı CPU kullanım ölçümü|Hiç boyut|
|WSXNS|Ad alanı başına bellek boyutu kullanımı|Yüzde|Maksimum|Service Bus premium ad alanı bellek kullanım ölçümü|Hiç boyut|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|Hiç boyut|
|physical_data_read_percent|Veri G/Ç yüzdesi|Yüzde|Ortalama|Veri G/Ç yüzdesi|Hiç boyut|
|log_write_percent|Günlük g/ç yüzdesi|Yüzde|Ortalama|Günlük g/ç yüzdesi|Hiç boyut|
|dtu_consumption_percent|DTU yüzdesi|Yüzde|Ortalama|DTU yüzdesi|Hiç boyut|
|depolama|Toplam veritabanı boyutu|Bayt|Maksimum|Toplam veritabanı boyutu|Hiç boyut|
|connection_successful|Başarılı bağlantıları|Sayı|Toplam|Başarılı bağlantıları|Hiç boyut|
|connection_failed|Başarısız bağlantı sayısı|Sayı|Toplam|Başarısız bağlantı sayısı|Hiç boyut|
|blocked_by_firewall|Güvenlik Duvarı tarafından engellendi|Sayı|Toplam|Güvenlik Duvarı tarafından engellendi|Hiç boyut|
|Kilitlenme|Kilitlenmeleri|Sayı|Toplam|Kilitlenmeleri|Hiç boyut|
|storage_percent|Veri boyutu yüzdesi|Yüzde|Maksimum|Veri boyutu yüzdesi|Hiç boyut|
|xtp_storage_percent|Bellek içi OLTP depolama yüzdesi|Yüzde|Ortalama|Bellek içi OLTP depolama yüzdesi|Hiç boyut|
|workers_percent|Çalışanlar yüzdesi|Yüzde|Ortalama|Çalışanlar yüzdesi|Hiç boyut|
|sessions_percent|Oturumları yüzdesi|Yüzde|Ortalama|Oturumları yüzdesi|Hiç boyut|
|dtu_limit|DTU sınırı|Sayı|Ortalama|DTU sınırı|Hiç boyut|
|dtu_used|Kullanılan DTU|Sayı|Ortalama|Kullanılan DTU|Hiç boyut|
|dwu_limit|DWU sınırı|Sayı|Maksimum|DWU sınırı|Hiç boyut|
|dwu_consumption_percent|DWU yüzdesi|Yüzde|Maksimum|DWU yüzdesi|Hiç boyut|
|dwu_used|Kullanılan DWU|Sayı|Maksimum|Kullanılan DWU|Hiç boyut|
|dw_cpu_percent|DW düğüm düzeyi CPU yüzdesi|Yüzde|Ortalama|DW düğüm düzeyi CPU yüzdesi|dw_logical_node_id|
|dw_physical_data_read_percent|DW düğüm düzeyi veri g/ç yüzdesi|Yüzde|Ortalama|DW düğüm düzeyi veri g/ç yüzdesi|dw_logical_node_id|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|Hiç boyut|
|database_cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|DatabaseResourceId|
|physical_data_read_percent|Veri G/Ç yüzdesi|Yüzde|Ortalama|Veri G/Ç yüzdesi|Hiç boyut|
|database_physical_data_read_percent|Veri G/Ç yüzdesi|Yüzde|Ortalama|Veri G/Ç yüzdesi|DatabaseResourceId|
|log_write_percent|Günlük g/ç yüzdesi|Yüzde|Ortalama|Günlük g/ç yüzdesi|Hiç boyut|
|database_log_write_percent|Günlük g/ç yüzdesi|Yüzde|Ortalama|Günlük g/ç yüzdesi|DatabaseResourceId|
|dtu_consumption_percent|DTU yüzdesi|Yüzde|Ortalama|DTU yüzdesi|Hiç boyut|
|database_dtu_consumption_percent|DTU yüzdesi|Yüzde|Ortalama|DTU yüzdesi|DatabaseResourceId|
|storage_percent|Depolama yüzdesi|Yüzde|Ortalama|Depolama yüzdesi|Hiç boyut|
|workers_percent|Çalışanlar yüzdesi|Yüzde|Ortalama|Çalışanlar yüzdesi|Hiç boyut|
|database_workers_percent|Çalışanlar yüzdesi|Yüzde|Ortalama|Çalışanlar yüzdesi|DatabaseResourceId|
|sessions_percent|Oturumları yüzdesi|Yüzde|Ortalama|Oturumları yüzdesi|Hiç boyut|
|database_sessions_percent|Oturumları yüzdesi|Yüzde|Ortalama|Oturumları yüzdesi|DatabaseResourceId|
|eDTU_limit|eDTU sınırı|Sayı|Ortalama|eDTU sınırı|Hiç boyut|
|storage_limit|Depolama sınırı|Bayt|Ortalama|Depolama sınırı|Hiç boyut|
|eDTU_used|kullanılan eDTU|Sayı|Ortalama|kullanılan eDTU|Hiç boyut|
|storage_used|Kullanılan depolama|Bayt|Ortalama|Kullanılan depolama|Hiç boyut|
|database_storage_used|Kullanılan depolama|Bayt|Ortalama|Kullanılan depolama|DatabaseResourceId|
|xtp_storage_percent|Bellek içi OLTP depolama yüzdesi|Yüzde|Ortalama|Bellek içi OLTP depolama yüzdesi|Hiç boyut|

## <a name="microsoftsqlservers"></a>Microsoft.Sql/servers

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|dtu_consumption_percent|DTU yüzdesi|Yüzde|Ortalama|DTU yüzdesi|ElasticPoolResourceId|
|database_dtu_consumption_percent|DTU yüzdesi|Yüzde|Ortalama|DTU yüzdesi|DatabaseResourceId, ElasticPoolResourceId|
|storage_used|Kullanılan depolama|Bayt|Ortalama|Kullanılan depolama|ElasticPoolResourceId|
|database_storage_used|Kullanılan depolama|Bayt|Ortalama|Kullanılan depolama|DatabaseResourceId, ElasticPoolResourceId|

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|UsedCapacity|Kullanılan kapasite|Bayt|Ortalama|Hesabın kullanılan kapasitesi|Hiç boyut|
|İşlemler|İşlemler|Sayı|Toplam|Depolama hizmet ya da belirtilen API işlemi için yapılan isteklerin sayısı. Bu sayı, hataları üretilen isteklerinin yanı sıra başarılı ve başarısız istekleri içerir. ResponseType boyut yanıt farklı türde sayısı için kullanın.|ResponseType, GeoType, ApiName|
|Giriş|Giriş|Bayt|Toplam|Bayt cinsinden giriş veri miktarı. Bu sayı, Azure içinde giriş yanı sıra Azure Storage içine bir dış istemcinin giriş içerir.|GeoType, ApiName|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış veri miktarı. Bu sayı, çıkış Azure içinde yanı sıra Azure Storage içine dış bir istemciden çıkış içerir. Sonuç olarak, bu sayı Faturalanabilir çıkış yansıtmaz.|GeoType, ApiName|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Milisaniye cinsinden başarılı bir isteği işlemek için Azure Storage tarafından kullanılan ortalama gecikme süresi. Bu değer AverageE2ELatency içinde belirtilen ağ gecikme süresi dahil değildir.|GeoType, ApiName|
|SuccessE2ELatency|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Depolama hizmetine veya milisaniye cinsinden belirtilen API işlem iletilen başarılı istek ortalama uçtan uca gecikme. Bu değer, istek okuma, bir yanıt gönderir ve yanıtın bildirim almak için Azure Storage içinde gerekli işleme süresini içerir.|GeoType, ApiName|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama Birimi hizmeti veya belirtilen API işlemi için yüzdesi. Kullanılabilirlik TotalBillableRequests değerini almak ve da beklenmeyen hatalar üretilen dahil olmak üzere, geçerli isteklerin sayısına bölünerek hesaplanır. Depolama hizmet veya belirtilen API işlemi için daha az kullanılabilirlik tüm beklenmeyen hatalar sonucu.|GeoType, ApiName|

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BlobCapacity|Blob Kapasitesi|Bayt|Ortalama|Bayt olarak depolama hesabının Blob hizmeti tarafından kullanılan depolama miktarı.|BlobType|
|BLOB sayısı|Blob Sayısı|Sayı|Ortalama|Depolama hesabının Blob hizmetindeki Blob sayısı.|BlobType|
|ContainerCount|Blob Kapsayıcı Sayısı|Sayı|Ortalama|Depolama hesabının Blob hizmetindeki kapsayıcı sayısı.|Hiç boyut|
|İşlemler|İşlemler|Sayı|Toplam|Depolama hizmet ya da belirtilen API işlemi için yapılan isteklerin sayısı. Bu sayı, hataları üretilen isteklerinin yanı sıra başarılı ve başarısız istekleri içerir. ResponseType boyut yanıt farklı türde sayısı için kullanın.|ResponseType, GeoType, ApiName|
|Giriş|Giriş|Bayt|Toplam|Bayt cinsinden giriş veri miktarı. Bu sayı, Azure içinde giriş yanı sıra Azure Storage içine bir dış istemcinin giriş içerir.|GeoType, ApiName|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış veri miktarı. Bu sayı, çıkış Azure içinde yanı sıra Azure Storage içine dış bir istemciden çıkış içerir. Sonuç olarak, bu sayı Faturalanabilir çıkış yansıtmaz.|GeoType, ApiName|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Milisaniye cinsinden başarılı bir isteği işlemek için Azure Storage tarafından kullanılan ortalama gecikme süresi. Bu değer AverageE2ELatency içinde belirtilen ağ gecikme süresi dahil değildir.|GeoType, ApiName|
|SuccessE2ELatency|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Depolama hizmetine veya milisaniye cinsinden belirtilen API işlem iletilen başarılı istek ortalama uçtan uca gecikme. Bu değer, istek okuma, bir yanıt gönderir ve yanıtın bildirim almak için Azure Storage içinde gerekli işleme süresini içerir.|GeoType, ApiName|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama Birimi hizmeti veya belirtilen API işlemi için yüzdesi. Kullanılabilirlik TotalBillableRequests değerini almak ve da beklenmeyen hatalar üretilen dahil olmak üzere, geçerli isteklerin sayısına bölünerek hesaplanır. Depolama hizmet veya belirtilen API işlemi için daha az kullanılabilirlik tüm beklenmeyen hatalar sonucu.|GeoType, ApiName|

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TableCapacity|Tablo Kapasitesi|Bayt|Ortalama|Bayt olarak depolama hesabının Tablo hizmeti tarafından kullanılan depolama miktarı.|Hiç boyut|
|TableCount|Tablo Sayısı|Sayı|Ortalama|Depolama hesabının Tablo hizmetindeki tablo sayısı.|Hiç boyut|
|TableEntityCount|Tablo Varlık Sayısı|Sayı|Ortalama|Depolama hesabının Tablo hizmetindeki tablo varlıklarının sayısı.|Hiç boyut|
|İşlemler|İşlemler|Sayı|Toplam|Depolama hizmet ya da belirtilen API işlemi için yapılan isteklerin sayısı. Bu sayı, hataları üretilen isteklerinin yanı sıra başarılı ve başarısız istekleri içerir. ResponseType boyut yanıt farklı türde sayısı için kullanın.|ResponseType, GeoType, ApiName|
|Giriş|Giriş|Bayt|Toplam|Bayt cinsinden giriş veri miktarı. Bu sayı, Azure içinde giriş yanı sıra Azure Storage içine bir dış istemcinin giriş içerir.|GeoType, ApiName|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış veri miktarı. Bu sayı, çıkış Azure içinde yanı sıra Azure Storage içine dış bir istemciden çıkış içerir. Sonuç olarak, bu sayı Faturalanabilir çıkış yansıtmaz.|GeoType, ApiName|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Milisaniye cinsinden başarılı bir isteği işlemek için Azure Storage tarafından kullanılan ortalama gecikme süresi. Bu değer AverageE2ELatency içinde belirtilen ağ gecikme süresi dahil değildir.|GeoType, ApiName|
|SuccessE2ELatency|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Depolama hizmetine veya milisaniye cinsinden belirtilen API işlem iletilen başarılı istek ortalama uçtan uca gecikme. Bu değer, istek okuma, bir yanıt gönderir ve yanıtın bildirim almak için Azure Storage içinde gerekli işleme süresini içerir.|GeoType, ApiName|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama Birimi hizmeti veya belirtilen API işlemi için yüzdesi. Kullanılabilirlik TotalBillableRequests değerini almak ve da beklenmeyen hatalar üretilen dahil olmak üzere, geçerli isteklerin sayısına bölünerek hesaplanır. Depolama hizmet veya belirtilen API işlemi için daha az kullanılabilirlik tüm beklenmeyen hatalar sonucu.|GeoType, ApiName|

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|QueueCapacity|Kuyruk Kapasitesi|Bayt|Ortalama|Bayt olarak depolama hesabının Kuyruk hizmeti tarafından kullanılan depolama miktarı.|Hiç boyut|
|QueueCount|Kuyruk Sayısı|Sayı|Ortalama|Depolama hesabının Kuyruk hizmetindeki sıra sayısı.|Hiç boyut|
|QueueMessageCount|Kuyruk İleti Sayısı|Sayı|Ortalama|Depolama hesabının Kuyruk hizmetindeki sıra iletilerinin yaklaşık sayısı.|Hiç boyut|
|İşlemler|İşlemler|Sayı|Toplam|Depolama hizmet ya da belirtilen API işlemi için yapılan isteklerin sayısı. Bu sayı, hataları üretilen isteklerinin yanı sıra başarılı ve başarısız istekleri içerir. ResponseType boyut yanıt farklı türde sayısı için kullanın.|ResponseType, GeoType, ApiName|
|Giriş|Giriş|Bayt|Toplam|Bayt cinsinden giriş veri miktarı. Bu sayı, Azure içinde giriş yanı sıra Azure Storage içine bir dış istemcinin giriş içerir.|GeoType, ApiName|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış veri miktarı. Bu sayı, çıkış Azure içinde yanı sıra Azure Storage içine dış bir istemciden çıkış içerir. Sonuç olarak, bu sayı Faturalanabilir çıkış yansıtmaz.|GeoType, ApiName|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Milisaniye cinsinden başarılı bir isteği işlemek için Azure Storage tarafından kullanılan ortalama gecikme süresi. Bu değer AverageE2ELatency içinde belirtilen ağ gecikme süresi dahil değildir.|GeoType, ApiName|
|SuccessE2ELatency|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Depolama hizmetine veya milisaniye cinsinden belirtilen API işlem iletilen başarılı istek ortalama uçtan uca gecikme. Bu değer, istek okuma, bir yanıt gönderir ve yanıtın bildirim almak için Azure Storage içinde gerekli işleme süresini içerir.|GeoType, ApiName|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama Birimi hizmeti veya belirtilen API işlemi için yüzdesi. Kullanılabilirlik TotalBillableRequests değerini almak ve da beklenmeyen hatalar üretilen dahil olmak üzere, geçerli isteklerin sayısına bölünerek hesaplanır. Depolama hizmet veya belirtilen API işlemi için daha az kullanılabilirlik tüm beklenmeyen hatalar sonucu.|GeoType, ApiName|

## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft.Storage/storageAccounts/fileServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|FileCapacity|Dosya Kapasitesi|Bayt|Ortalama|Bayt olarak, depolama hesabının Dosya hizmetinin kullandığı depolama miktarı.|Hiç boyut|
|FileCount|Dosya Sayısı|Sayı|Ortalama|Depolama hesabının Dosya hizmetindeki dosya sayısı.|Hiç boyut|
|FileShareCount|Dosya Paylaşım Sayısı|Sayı|Ortalama|Depolama hesabının Dosya hizmetindeki dosya paylaşımlarının sayısı.|Hiç boyut|
|İşlemler|İşlemler|Sayı|Toplam|Depolama hizmet ya da belirtilen API işlemi için yapılan isteklerin sayısı. Bu sayı, hataları üretilen isteklerinin yanı sıra başarılı ve başarısız istekleri içerir. ResponseType boyut yanıt farklı türde sayısı için kullanın.|ResponseType, GeoType, ApiName|
|Giriş|Giriş|Bayt|Toplam|Bayt cinsinden giriş veri miktarı. Bu sayı, Azure içinde giriş yanı sıra Azure Storage içine bir dış istemcinin giriş içerir.|GeoType, ApiName|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış veri miktarı. Bu sayı, çıkış Azure içinde yanı sıra Azure Storage içine dış bir istemciden çıkış içerir. Sonuç olarak, bu sayı Faturalanabilir çıkış yansıtmaz.|GeoType, ApiName|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Milisaniye cinsinden başarılı bir isteği işlemek için Azure Storage tarafından kullanılan ortalama gecikme süresi. Bu değer AverageE2ELatency içinde belirtilen ağ gecikme süresi dahil değildir.|GeoType, ApiName|
|SuccessE2ELatency|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Depolama hizmetine veya milisaniye cinsinden belirtilen API işlem iletilen başarılı istek ortalama uçtan uca gecikme. Bu değer, istek okuma, bir yanıt gönderir ve yanıtın bildirim almak için Azure Storage içinde gerekli işleme süresini içerir.|GeoType, ApiName|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama Birimi hizmeti veya belirtilen API işlemi için yüzdesi. Kullanılabilirlik TotalBillableRequests değerini almak ve da beklenmeyen hatalar üretilen dahil olmak üzere, geçerli isteklerin sayısına bölünerek hesaplanır. Depolama hizmet veya belirtilen API işlemi için daha az kullanılabilirlik tüm beklenmeyen hatalar sonucu.|GeoType, ApiName|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ResourceUtilization|SU Kullanım Yüzdesi|Yüzde|Maksimum|SU Kullanım Yüzdesi|Hiç boyut|
|InputEvents|Giriş Olayları|Sayı|Toplam|Giriş Olayları|Hiç boyut|
|InputEventBytes|Giriş Olayı Bayt Sayısı|Bayt|Toplam|Giriş Olayı Bayt Sayısı|Hiç boyut|
|LateInputEvents|Geç Giriş Olayları|Sayı|Toplam|Geç Giriş Olayları|Hiç boyut|
|OutputEvents|Çıkış Olayları|Sayı|Toplam|Çıkış Olayları|Hiç boyut|
|ConversionErrors|Veri Dönüştürme Hataları|Sayı|Toplam|Veri Dönüştürme Hataları|Hiç boyut|
|Hatalar|Çalışma Zamanı Hataları|Sayı|Toplam|Çalışma Zamanı Hataları|Hiç boyut|
|DroppedOrAdjustedEvents|Sıralama dışındaki Olaylar|Sayı|Toplam|Sıralama dışındaki Olaylar|Hiç boyut|
|AMLCalloutRequests|İşlev İstekleri|Sayı|Toplam|İşlev İstekleri|Hiç boyut|
|AMLCalloutFailedRequests|Başarısız İşlev İstekleri|Sayı|Toplam|Başarısız İşlev İstekleri|Hiç boyut|
|AMLCalloutInputEvents|İşlev Olayları|Sayı|Toplam|İşlev Olayları|Hiç boyut|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CpuPercentage|CPU Yüzdesi|Yüzde|Ortalama|CPU Yüzdesi|Örnek|
|MemoryPercentage|Bellek Yüzdesi|Yüzde|Ortalama|Bellek Yüzdesi|Örnek|
|DiskQueueLength|Disk Kuyruğu Uzunluğu|Sayı|Ortalama|Disk Kuyruğu Uzunluğu|Örnek|
|HttpQueueLength|Http Kuyruk Uzunluğu|Sayı|Ortalama|Http Kuyruk Uzunluğu|Örnek|
|BytesReceived|Data Girişi|Bayt|Toplam|Data Girişi|Örnek|
|BytesSent|Giden Veriler|Bayt|Toplam|Giden Veriler|Örnek|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft.Web/sites (işlevler hariç)

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU zamanı|CPU Süresi|Saniye|Toplam|CPU Süresi|Örnek|
|İstekler|İstekler|Sayı|Toplam|İstekler|Örnek|
|BytesReceived|Data Girişi|Bayt|Toplam|Data Girişi|Örnek|
|BytesSent|Giden Veriler|Bayt|Toplam|Giden Veriler|Örnek|
|Http101|Http 101|Sayı|Toplam|Http 101|Örnek|
|Http2xx|Http 2xx|Sayı|Toplam|Http 2xx|Örnek|
|Http3xx|Http 3xx|Sayı|Toplam|Http 3xx|Örnek|
|Http401|Http 401|Sayı|Toplam|Http 401|Örnek|
|Http403|Http 403|Sayı|Toplam|Http 403|Örnek|
|Http404|Http 404|Sayı|Toplam|Http 404|Örnek|
|Http406|Http 406|Sayı|Toplam|Http 406|Örnek|
|Http4xx|Http 4xx|Sayı|Toplam|Http 4xx|Örnek|
|Http5xx|Http Sunucu Hataları|Sayı|Toplam|Http Sunucu Hataları|Örnek|
|MemoryWorkingSet|Bellek çalışma kümesi|Bayt|Ortalama|Bellek çalışma kümesi|Örnek|
|AverageMemoryWorkingSet|Ortalama bellek çalışma kümesi|Bayt|Ortalama|Ortalama bellek çalışma kümesi|Örnek|
|AverageResponseTime|Ortalama Yanıt Süresi|Saniye|Ortalama|Ortalama Yanıt Süresi|Örnek|

## <a name="microsoftwebsites-functions"></a>Microsoft.Web/sites (işlev)

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BytesReceived|Data Girişi|Bayt|Toplam|Data Girişi|Örnek|
|BytesSent|Giden Veriler|Bayt|Toplam|Giden Veriler|Örnek|
|Http5xx|Http Sunucu Hataları|Sayı|Toplam|Http Sunucu Hataları|Örnek|
|MemoryWorkingSet|Bellek çalışma kümesi|Bayt|Ortalama|Bellek çalışma kümesi|Örnek|
|AverageMemoryWorkingSet|Ortalama bellek çalışma kümesi|Bayt|Ortalama|Ortalama bellek çalışma kümesi|Örnek|
|FunctionExecutionUnits|İşlev Yürütme Birimleri|Sayı|Toplam|İşlev Yürütme Birimleri|Örnek|
|İşlev yürütme sayısı|İşlev Yürütme Sayısı|Sayı|Toplam|İşlev Yürütme Sayısı|Örnek|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU zamanı|CPU Süresi|Saniye|Toplam|CPU Süresi|Örnek|
|İstekler|İstekler|Sayı|Toplam|İstekler|Örnek|
|BytesReceived|Data Girişi|Bayt|Toplam|Data Girişi|Örnek|
|BytesSent|Giden Veriler|Bayt|Toplam|Giden Veriler|Örnek|
|Http101|Http 101|Sayı|Toplam|Http 101|Örnek|
|Http2xx|Http 2xx|Sayı|Toplam|Http 2xx|Örnek|
|Http3xx|Http 3xx|Sayı|Toplam|Http 3xx|Örnek|
|Http401|Http 401|Sayı|Toplam|Http 401|Örnek|
|Http403|Http 403|Sayı|Toplam|Http 403|Örnek|
|Http404|Http 404|Sayı|Toplam|Http 404|Örnek|
|Http406|Http 406|Sayı|Toplam|Http 406|Örnek|
|Http4xx|Http 4xx|Sayı|Toplam|Http 4xx|Örnek|
|Http5xx|Http Sunucu Hataları|Sayı|Toplam|Http Sunucu Hataları|Örnek|
|MemoryWorkingSet|Bellek çalışma kümesi|Bayt|Ortalama|Bellek çalışma kümesi|Örnek|
|AverageMemoryWorkingSet|Ortalama bellek çalışma kümesi|Bayt|Ortalama|Ortalama bellek çalışma kümesi|Örnek|
|AverageResponseTime|Ortalama Yanıt Süresi|Saniye|Ortalama|Ortalama Yanıt Süresi|Örnek|
|FunctionExecutionUnits|İşlev Yürütme Birimleri|Sayı|Ortalama|İşlev Yürütme Birimleri|Örnek|
|İşlev yürütme sayısı|İşlev Yürütme Sayısı|Sayı|Ortalama|İşlev Yürütme Sayısı|Örnek|

## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft.Web/hostingEnvironments/multiRolePools

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|İstekler|İstekler|Sayı|Toplam|İstekler|Örnek|
|BytesReceived|Data Girişi|Bayt|Toplam|Data Girişi|Örnek|
|BytesSent|Giden Veriler|Bayt|Toplam|Giden Veriler|Örnek|
|Http101|Http 101|Sayı|Toplam|Http 101|Örnek|
|Http2xx|Http 2xx|Sayı|Toplam|Http 2xx|Örnek|
|Http3xx|Http 3xx|Sayı|Toplam|Http 3xx|Örnek|
|Http401|Http 401|Sayı|Toplam|Http 401|Örnek|
|Http403|Http 403|Sayı|Toplam|Http 403|Örnek|
|Http404|Http 404|Sayı|Toplam|Http 404|Örnek|
|Http406|Http 406|Sayı|Toplam|Http 406|Örnek|
|Http4xx|Http 4xx|Sayı|Toplam|Http 4xx|Örnek|
|Http5xx|Http Sunucu Hataları|Sayı|Toplam|Http Sunucu Hataları|Örnek|
|AverageResponseTime|Ortalama Yanıt Süresi|Saniye|Ortalama|Ortalama Yanıt Süresi|Örnek|
|CpuPercentage|CPU Yüzdesi|Yüzde|Ortalama|CPU Yüzdesi|Örnek|
|MemoryPercentage|Bellek Yüzdesi|Yüzde|Ortalama|Bellek Yüzdesi|Örnek|
|DiskQueueLength|Disk Kuyruğu Uzunluğu|Sayı|Ortalama|Disk Kuyruğu Uzunluğu|Örnek|
|HttpQueueLength|Http Kuyruk Uzunluğu|Sayı|Ortalama|Http Kuyruk Uzunluğu|Örnek|
|ActiveRequests|Etkin İstekler|Sayı|Toplam|Etkin İstekler|Örnek|
|TotalFrontEnds|Toplam Ön Uç Sayısı|Sayı|Ortalama|Toplam Ön Uç Sayısı|Örnek|
|SmallAppServicePlanInstances|Kısa App Service Planı Çalışanları|Sayı|Ortalama|Kısa App Service Planı Çalışanları|Örnek|
|MediumAppServicePlanInstances|Orta App Service Planı Çalışanları|Sayı|Ortalama|Orta App Service Planı Çalışanları|Örnek|
|LargeAppServicePlanInstances|Uzun App Service Planı Çalışanları|Sayı|Ortalama|Uzun App Service Planı Çalışanları|Örnek|

## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft.Web/hostingEnvironments/workerPools

|Ölçüm|Ölçüm görünen adı|Birim|Toplama türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Bugün için WorkersTotal|Toplam Çalışanlar|Sayı|Ortalama|Toplam Çalışanlar|Örnek|
|WorkersAvailable|Kullanılabilir Çalışanlar|Sayı|Ortalama|Kullanılabilir Çalışanlar|Örnek|
|WorkersUsed|Kullanılan Çalışanlar|Sayı|Ortalama|Kullanılan Çalışanlar|Örnek|
|CpuPercentage|CPU Yüzdesi|Yüzde|Ortalama|CPU Yüzdesi|Örnek|
|MemoryPercentage|Bellek Yüzdesi|Yüzde|Ortalama|Bellek Yüzdesi|Örnek|

## <a name="next-steps"></a>Sonraki adımlar
* [Azure İzleyicisi'nde ölçümleri hakkında bilgi edinin](monitoring-overview-metrics.md)
* [Ölçümleri uyarı oluşturma](insights-receive-alert-notifications.md)
* [Depolama, olay hub'ı veya günlük analizi ölçümleri dışarı aktarma](monitoring-overview-of-diagnostic-logs.md)
