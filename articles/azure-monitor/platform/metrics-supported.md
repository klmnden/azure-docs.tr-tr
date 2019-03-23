---
title: Azure İzleyici desteklenen ölçümler kaynak türüne göre
description: Azure İzleyici ile her bir kaynak türü için mevcut olan ölçümler listesi.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 09/14/2018
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: e9b562cb04bb8916245d9df7b9b6d526bd443a24
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58352145"
---
# <a name="supported-metrics-with-azure-monitor"></a>Azure İzleyici ile desteklenen ölçümler

Azure İzleyici, bunları portalda grafik, REST API aracılığıyla erişmesini veya bunları sorgulama gibi ölçümleri ile etkileşim kurmak için çeşitli yollar sağlar PowerShell veya CLI kullanarak. Aşağıda tüm ölçümler tam listesi ile Azure İzleyicisi'nin ölçüm ardışık düzen şu anda kullanılabilir. Diğer ölçümleri portalı veya eski API'leri kullanarak mevcut olabilir. Bu listede yalnızca birleştirilmiş Azure İzleyici ölçüm ardışık düzeni'ni kullanarak mevcut olan ölçümler içerir. Sorgulamak ve erişmek için bu ölçümleri lütfen [2018-01-01 API sürümü](https://docs.microsoft.com/rest/api/monitor/metricdefinitions)

> [!NOTE]
> Çok boyutlu ölçümlerin tanılama ayarları aracılığıyla gönderilmesi şu anda desteklenmemektedir. Boyutlu ölçümler, boyut değerlerinin toplamı alınarak düzleştirilmiş tek yönlü ölçümler olarak dışarı aktarılır.
>
> *Örneğin*: Bir olay Hub'ındaki 'Gelen iletiler' ölçümü temelinde araştırılıp bir kuyruk düzeyi. Ancak, tanılama ayarları aracılığıyla dışarı aktarılan ölçüm, Olay Hub’ındaki tüm kuyruklarda tüm gelen iletiler halinde ifade edilir.
>
>

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|qpu_metric|QPU|Sayı|Ortalama|QPU. S1, S2 için 0-200 ve S4 için 0-400 için 0-100 aralığında|ServerResourceType|
|memory_metric|Bellek|Bayt|Ortalama|Bellek. 0-25 GB S1, S2 için 0-50 GB ve 0-100 aralığında S4 için GB|ServerResourceType|
|TotalConnectionRequests|Toplam bağlantı istekleri|Sayı|Ortalama|Toplam bağlantı istekleri. Bunlar, gelen bağlantılardır.|ServerResourceType|
|SuccessfullConnectionsPerSec|Saniye başına başarılı bağlantılar|CountPerSecond|Ortalama|Başarıyla tamamlanan bağlantı oranı.|ServerResourceType|
|TotalConnectionFailures|Toplam bağlantı hataları|Sayı|Ortalama|Toplam bağlantı denemesi başarısız oldu.|ServerResourceType|
|CurrentUserSessions|Geçerli kullanıcı oturumları|Sayı|Ortalama|Geçerli kullanıcı oturumlarının sayısı.|ServerResourceType|
|QueryPoolBusyThreads|Sorgu havuzu meşgul iş parçacıkları|Sayı|Ortalama|Sorgu iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|ServerResourceType|
|CommandPoolJobQueueLength|Komut havuzu iş kuyruğu uzunluğu|Sayı|Ortalama|Komut iş parçacığı havuzu kuyruğundaki iş sayısı.|ServerResourceType|
|ProcessingPoolJobQueueLength|İşleme havuzu iş kuyruğu uzunluğu|Sayı|Ortalama|İşleme iş parçacığı havuzu kuyruğundaki/ç olmayan iş sayısı.|ServerResourceType|
|CurrentConnections|Bağlantı: Geçerli bağlantılar|Sayı|Ortalama|Kurulan istemci bağlantılarının geçerli sayısı.|ServerResourceType|
|CleanerCurrentPrice|Bellek: Temizleyici geçerli fiyatı|Sayı|Ortalama|Geçerli fiyat, bellek $/ bayt/zaman normalleştirilmiş 1000.|ServerResourceType|
|CleanerMemoryShrinkable|Bellek: Temizleyici belleği daraltılabilir|Bayt|Ortalama|Tarafından arka plan Temizleyicisi temizleme bayt cinsinden bellek miktarı.|ServerResourceType|
|CleanerMemoryNonshrinkable|Bellek: Temizleyici belleği daraltılamaz|Bayt|Ortalama|Tarafından arka plan Temizleyicisi kapsamında olmayan bayt cinsinden bellek miktarı.|ServerResourceType|
|MemoryUsage|Bellek: Bellek Kullanımı|Bayt|Ortalama|Temizleyici bellek fiyatı hesaplanırken kullanılan sunucu işlemi bellek kullanımı. Process\PrivateBytes artı eşlenmiş veya ve xVelocity altyapısı bellek sınırını xVelocity bellek içi analiz altyapısı (VertiPaq) tarafından ayrılan herhangi bir bellek yok sayılıyor, bellekle eşlenen verilerin boyutunu sayaç eşittir.|ServerResourceType|
|MemoryLimitHard|Bellek: Bellek sınırı sabit|Bayt|Ortalama|Yapılandırma dosyasından sabit bellek sınırı.|ServerResourceType|
|MemoryLimitHigh|Bellek: Bellek sınırı yüksek|Bayt|Ortalama|Yapılandırma dosyasından yüksek bellek sınırı.|ServerResourceType|
|MemoryLimitLow|Bellek: Bellek sınırı düşük|Bayt|Ortalama|Yapılandırma dosyasından düşük bellek sınırı.|ServerResourceType|
|MemoryLimitVertiPaq|Bellek: Bellek sınırı VertiPaq|Bayt|Ortalama|Yapılandırma dosyasından bellek içi sınır.|ServerResourceType|
|Kota|Bellek: Kota|Bayt|Ortalama|Bayt cinsinden geçerli bellek kotası. Bellek kotası bellek ataması veya bellek ayırma de denir.|ServerResourceType|
|QuotaBlocked|Bellek: Kota engellendi|Sayı|Ortalama|Diğer bellek kotaları serbest bırakılana kadar engellenen kota isteklerinin geçerli sayısı.|ServerResourceType|
|VertiPaqNonpaged|Bellek: Disk belleği olmayan VertiPaq|Bayt|Ortalama|Bayt bellek kullanımı için bellek içi altyapısı tarafından belirlenen çalışma kilitli.|ServerResourceType|
|VertiPaqPaged|Bellek: Disk belleği olan VertiPaq|Bayt|Ortalama|Bellek içi veriler için disk belleğine alınan bellek bayt sayısı.|ServerResourceType|
|RowsReadPerSec|İşleniyor: Satır sayısı / sn okuyun|CountPerSecond|Ortalama|Satır oranı tüm ilişkisel veritabanlarından okunan.|ServerResourceType|
|RowsConvertedPerSec|İşleniyor: Satır sayısı / sn dönüştürüldü|CountPerSecond|Ortalama|İşleme sırasında dönüştürülen bir satır oranı.|ServerResourceType|
|RowsWrittenPerSec|İşleniyor: Saniye yazılan satır|CountPerSecond|Ortalama|İşleme sırasında yazılan satır oranı.|ServerResourceType|
|CommandPoolBusyThreads|İş parçacıkları: Komut havuzu meşgul iş parçacıkları|Sayı|Ortalama|Komut iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|ServerResourceType|
|CommandPoolIdleThreads|İş parçacıkları: Komut havuzu boşta iş parçacıkları|Sayı|Ortalama|Komut iş parçacığı havuzundaki boşta iş parçacığı sayısı.|ServerResourceType|
|LongParsingBusyThreads|İş parçacıkları: Uzun ayrıştırma meşgul iş parçacıkları|Sayı|Ortalama|Uzun iş parçacığı İnceleme havuzundaki meşgul iş parçacığı sayısı.|ServerResourceType|
|LongParsingIdleThreads|İş parçacıkları: Uzun ayrıştırma boşta iş parçacıkları|Sayı|Ortalama|Uzun iş parçacığı İnceleme havuzundaki boşta iş parçacığı sayısı.|ServerResourceType|
|LongParsingJobQueueLength|İş parçacıkları: Uzun ayrıştırma iş kuyruğu uzunluğu|Sayı|Ortalama|Uzun ayrıştırma iş parçacığı havuzu kuyruğundaki iş sayısı.|ServerResourceType|
|ProcessingPoolBusyIOJobThreads|İş parçacıkları: İşleme havuzu meşgul g/ç işi iş parçacıkları|Sayı|Ortalama|İşleme iş parçacığı havuzunda g/ç işleri çalıştıran iş parçacığı sayısı.|ServerResourceType|
|ProcessingPoolBusyNonIOThreads|İş parçacıkları: İşleme havuzu meşgul/ç olmayan iş parçacıkları|Sayı|Ortalama|İşleme iş parçacığı havuzunda olmayan g/Ç işleri çalıştıran iş parçacığı sayısı.|ServerResourceType|
|ProcessingPoolIOJobQueueLength|İş parçacıkları: İşleme havuzu g/ç iş kuyruğu uzunluğu|Sayı|Ortalama|İşleme iş parçacığı havuzu kuyruğundaki g/ç iş sayısı.|ServerResourceType|
|ProcessingPoolIdleIOJobThreads|İş parçacıkları: İşleme havuzu boşta g/ç işi iş parçacıkları|Sayı|Ortalama|İşleme iş parçacığı havuzundaki g/ç işleri için boşta iş parçacığı sayısı.|ServerResourceType|
|ProcessingPoolIdleNonIOThreads|İş parçacıkları: İşleme havuzu boşta/ç olmayan iş parçacıkları|Sayı|Ortalama|/ Ç olmayan işler için ayrılan işleme iş parçacığı havuzundaki boşta iş parçacığı sayısı.|ServerResourceType|
|QueryPoolIdleThreads|İş parçacıkları: Sorgu havuzu boşta iş parçacıkları|Sayı|Ortalama|İşleme iş parçacığı havuzundaki g/ç işleri için boşta iş parçacığı sayısı.|ServerResourceType|
|QueryPoolJobQueueLength|İş parçacıkları: Sorgu havuzu iş kuyruğu uzunluğu|Sayı|Ortalama|Sorgu iş parçacığı havuzu kuyruğundaki iş sayısı.|ServerResourceType|
|ShortParsingBusyThreads|İş parçacıkları: Kısa ayrıştırma meşgul iş parçacıkları|Sayı|Ortalama|Kısa ayrıştırma iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|ServerResourceType|
|ShortParsingIdleThreads|İş parçacıkları: Kısa ayrıştırma boşta iş parçacıkları|Sayı|Ortalama|Kısa ayrıştırma iş parçacığı havuzundaki boşta iş parçacığı sayısı.|ServerResourceType|
|ShortParsingJobQueueLength|İş parçacıkları: Kısa ayrıştırma iş kuyruğu uzunluğu|Sayı|Ortalama|Kısa ayrıştırma iş parçacığı havuzu kuyruğundaki iş sayısı.|ServerResourceType|
|memory_thrashing_metric|Bellek çok yavaş|Yüzde|Ortalama|Ortalama bellek temizleme.|ServerResourceType|
|mashup_engine_qpu_metric|M altyapı QPU'su|Sayı|Ortalama|Karma altyapısı işlemleri tarafından kullanılan qpu|ServerResourceType|
|mashup_engine_memory_metric|M altyapı belleği|Bayt|Ortalama|Karma altyapısı işlemleri tarafından bellek kullanımı|ServerResourceType|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TotalRequests|Toplam ağ geçidi istekleri|Sayı|Toplam|Ağ geçidi istekleri sayısı|Konum, ana bilgisayar adı|
|SuccessfulRequests|Başarılı ağ geçidi istekleri|Sayı|Toplam|Başarılı ağ geçidi istek sayısı|Konum, ana bilgisayar adı|
|UnauthorizedRequests|Yetkisiz ağ geçidi istekleri|Sayı|Toplam|Yetkisiz ağ geçidi istekleri sayısı|Konum, ana bilgisayar adı|
|FailedRequests|Başarısız ağ geçidi istekleri|Sayı|Toplam|Ağ geçidi istek sayısı|Konum, ana bilgisayar adı|
|OtherRequests|Diğer ağ geçidi istekleri|Sayı|Toplam|Diğer ağ geçidi istekleri sayısı|Konum, ana bilgisayar adı|
|Süre|Ağ geçidi isteklerinin toplam süre|Milisaniye|Ortalama|Toplam süre, ağ geçidi istekleri milisaniye|Konum, ana bilgisayar adı|
|Kapasite|Kapasite|Yüzde|Ortalama|ApiManagement hizmeti için kullanım ölçümü|Konum|

## <a name="microsoftautomationautomationaccounts"></a>Microsoft.Automation/automationAccounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TotalJob|Toplam iş sayısı|Sayı|Toplam|İşlerin toplam sayısı|Runbook durumu|

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CoreCount|Adanmış çekirdek sayısı|Sayı|Toplam|Adanmış çekirdekler batch hesabındaki toplam sayısı|Boyut yok|
|TotalNodeCount|Adanmış düğüm sayısı|Sayı|Toplam|Adanmış düğümler batch hesabındaki toplam sayısı|Boyut yok|
|LowPriorityCoreCount|LowPriority çekirdek sayısı|Sayı|Toplam|Düşük öncelikli çekirdekler batch hesabındaki toplam sayısı|Boyut yok|
|TotalLowPriorityNodeCount|Düşük öncelikli düğüm sayısı|Sayı|Toplam|Düşük öncelikli düğümler batch hesabındaki toplam sayısı|Boyut yok|
|CreatingNodeCount|Düğüm sayısı oluşturma|Sayı|Toplam|Oluşturulan düğüm sayısı|Boyut yok|
|StartingNodeCount|Başlangıç düğüm sayısı|Sayı|Toplam|Başlangıç düğüm sayısı|Boyut yok|
|WaitingForStartTaskNodeCount|Başlangıç görevi düğüm sayısı için bekleniyor|Sayı|Toplam|Başlangıç görevinin tamamlanmasını için bekleyen düğüm sayısı|Boyut yok|
|StartTaskFailedNodeCount|Düğüm sayısı başlangıç görevi başarısız oldu|Sayı|Toplam|Başlangıç görevi başarısız olduğu düğüm sayısı|Boyut yok|
|IdleNodeCount|Boş düğüm sayısı|Sayı|Toplam|Boş düğüm sayısı|Boyut yok|
|OfflineNodeCount|Çevrimdışı düğüm sayısı|Sayı|Toplam|Çevrimdışı düğüm sayısı|Boyut yok|
|RebootingNodeCount|Düğüm sayısı yeniden başlatılıyor|Sayı|Toplam|Düğüm yeniden başlatma sayısı|Boyut yok|
|ReimagingNodeCount|Düğüm sayısı görüntüsü yeniden oluşturuluyor|Sayı|Toplam|Yeniden görüntü düğüm sayısı|Boyut yok|
|RunningNodeCount|Çalışan düğüm sayısı|Sayı|Toplam|Çalışan düğüm sayısı|Boyut yok|
|LeavingPoolNodeCount|Bırakma havuzu düğüm sayısı|Sayı|Toplam|Havuzdan çıkılıyor düğüm sayısı|Boyut yok|
|UnusableNodeCount|Kullanılamaz durumda düğüm sayısı|Sayı|Toplam|Kullanılamaz durumda düğüm sayısı|Boyut yok|
|PreemptedNodeCount|Etkisiz düğüm sayısı|Sayı|Toplam|Etkisiz düğüm sayısı|Boyut yok|
|TaskStartEvent|Görev Başlangıç olayları|Sayı|Toplam|Başlatıldığından görev toplam sayısı|Boyut yok|
|TaskCompleteEvent|Görev Tamamlandı olayları|Sayı|Toplam|Tamamlanan görevler toplam sayısı|Boyut yok|
|TaskFailEvent|Görev başarısız olayları|Sayı|Toplam|Toplam başarısız durumda tamamlanan görev sayısı|Boyut yok|
|PoolCreateEvent|Havuz oluşturma olayları|Sayı|Toplam|Oluşturulmuş havuzu toplam sayısı|Boyut yok|
|PoolResizeStartEvent|Havuz yeniden boyutlandırma başlangıç olayları|Sayı|Toplam|Başlatmış havuzu yeniden boyutlandırır toplam sayısı|Boyut yok|
|PoolResizeCompleteEvent|Havuz yeniden boyutlandırma tam olayları|Sayı|Toplam|Tamamlanan havuzu yeniden boyutlandırır toplam sayısı|Boyut yok|
|PoolDeleteStartEvent|Havuz silme başlangıç olayları|Sayı|Toplam|Başlatmış havuzu siler toplam sayısı|Boyut yok|
|PoolDeleteCompleteEvent|Havuzu tüm olayları Sil|Sayı|Toplam|Tamamlanan havuzu siler toplam sayısı|Boyut yok|
|JobDeleteCompleteEvent|İşin tüm olayları Sil|Sayı|Toplam|Başarıyla silinen işlerin toplam sayısı.|Boyut yok|
|JobDeleteStartEvent|Proje başlangıç olayları Sil|Sayı|Toplam|Silinecek istenen işlerin toplam sayısı.|Boyut yok|
|JobDisableCompleteEvent|İşi devre dışı bırakma tamamlandı olayları|Sayı|Toplam|Başarıyla devre dışı bırakılmış işlerin toplam sayısı.|Boyut yok|
|JobDisableStartEvent|İşi devre dışı başlangıç olayları|Sayı|Toplam|Devre dışı bırakılması istenen işlerin toplam sayısı.|Boyut yok|
|JobStartEvent|Proje başlangıç olayları|Sayı|Toplam|Başarıyla başlatıldı işlerin toplam sayısı.|Boyut yok|
|JobTerminateCompleteEvent|İşi sonlandırmak tam olayları|Sayı|Toplam|Başarıyla sonlandırıldı işlerin toplam sayısı.|Boyut yok|
|JobTerminateStartEvent|İşi sonlandırmak başlangıç olayları|Sayı|Toplam|Sonlandırılacak istenen işlerin toplam sayısı.|Boyut yok|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|connectedclients|Bağlı İstemciler|Sayı|Maksimum||ShardId|
|totalcommandsprocessed|İşlemler Toplamı|Sayı|Toplam||ShardId|
|cachehits|İsabetli Önbellek Okuma Sayısı|Sayı|Toplam||ShardId|
|cachemisses|İsabetsiz Önbellek Okuma Sayısı|Sayı|Toplam||ShardId|
|getcommands|Alınanlar|Sayı|Toplam||ShardId|
|setcommands|Kümeler|Sayı|Toplam||ShardId|
|operationsPerSecond|Saniye başına işlem|Sayı|Maksimum||ShardId|
|evictedkeys|Çıkarılan Anahtarlar|Sayı|Toplam||ShardId|
|totalkeys|Toplam Anahtar Sayısı|Sayı|Maksimum||ShardId|
|expiredkeys|Süresi Dolan Anahtarlar|Sayı|Toplam||ShardId|
|usedmemory|Kullanılan Bellek|Bayt|Maksimum||ShardId|
|usedmemorypercentage|Kullanılan bellek yüzdesi|Yüzde|Maksimum||ShardId|
|usedmemoryRss|Kullanılan bellek RSS|Bayt|Maksimum||ShardId|
|serverLoad|Sunucu Yükü|Yüzde|Maksimum||ShardId|
|cacheWrite|Önbellek Yazması|BytesPerSecond|Maksimum||ShardId|
|cacheRead|Önbellek Okuması|BytesPerSecond|Maksimum||ShardId|
|percentProcessorTime|CPU|Yüzde|Maksimum||ShardId|
|cacheLatency|Önbellek gecikme mikrosaniye (Önizleme)|Sayı|Ortalama||ShardId, SampleType|
|hatalar|Hatalar|Sayı|Maksimum||ShardId, ErrorType|
|connectedclients0|Bağlı istemciler (parça 0)|Sayı|Maksimum||Boyut yok|
|totalcommandsprocessed0|Toplam işlemler (parça 0)|Sayı|Toplam||Boyut yok|
|cachehits0|İsabetli Önbellek Okuma (parça 0)|Sayı|Toplam||Boyut yok|
|cachemisses0|İsabetsiz önbellek okuma sayısı (parça 0)|Sayı|Toplam||Boyut yok|
|getcommands0|(Parça 0) alır|Sayı|Toplam||Boyut yok|
|setcommands0|Ayarlar (parça 0)|Sayı|Toplam||Boyut yok|
|operationsPerSecond0|(Parça 0) saniye başına işlem|Sayı|Maksimum||Boyut yok|
|evictedkeys0|Çıkarılan anahtarlar (parça 0)|Sayı|Toplam||Boyut yok|
|totalkeys0|Toplam anahtarları (parça 0)|Sayı|Maksimum||Boyut yok|
|expiredkeys0|Süresi dolan anahtarlar (parça 0)|Sayı|Toplam||Boyut yok|
|usedmemory0|Kullanılan bellek (parça 0)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss0|Kullanılan bellek RSS (parça 0)|Bayt|Maksimum||Boyut yok|
|serverLoad0|Sunucu iş yükü (parça 0)|Yüzde|Maksimum||Boyut yok|
|cacheWrite0|Önbellek yazma (parça 0)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead0|Önbellek Okuma (parça 0)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime0|CPU (parça 0)|Yüzde|Maksimum||Boyut yok|
|connectedclients1|Bağlı istemciler (parça 1)|Sayı|Maksimum||Boyut yok|
|totalcommandsprocessed1|Toplam işlemler (parça 1)|Sayı|Toplam||Boyut yok|
|cachehits1|(Parça 1) İsabetli Önbellek okuma sayısı|Sayı|Toplam||Boyut yok|
|cachemisses1|(Parça 1) İsabetsiz Önbellek okuma sayısı|Sayı|Toplam||Boyut yok|
|getcommands1|(Parça 1) alır|Sayı|Toplam||Boyut yok|
|setcommands1|Ayarlar (parça 1)|Sayı|Toplam||Boyut yok|
|operationsPerSecond1|(Parça 1) saniye başına işlem|Sayı|Maksimum||Boyut yok|
|evictedkeys1|Çıkarılan anahtarlar (parça 1)|Sayı|Toplam||Boyut yok|
|totalkeys1|Toplam anahtarları (parça 1)|Sayı|Maksimum||Boyut yok|
|expiredkeys1|Süresi dolan anahtarlar (parça 1)|Sayı|Toplam||Boyut yok|
|usedmemory1|Kullanılan bellek (parça 1)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss1|Kullanılan bellek RSS (parça 1)|Bayt|Maksimum||Boyut yok|
|serverLoad1|Sunucu iş yükü (parça 1)|Yüzde|Maksimum||Boyut yok|
|cacheWrite1|Önbellek yazma (parça 1)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead1|Önbellek Okuma (parça 1)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime1|CPU (parça 1)|Yüzde|Maksimum||Boyut yok|
|connectedclients2|Bağlı istemciler (parça 2)|Sayı|Maksimum||Boyut yok|
|totalcommandsprocessed2|Toplam işlemler (parça 2)|Sayı|Toplam||Boyut yok|
|cachehits2|(Parça 2) İsabetli Önbellek okuma sayısı|Sayı|Toplam||Boyut yok|
|cachemisses2|(Parça 2) İsabetsiz Önbellek okuma sayısı|Sayı|Toplam||Boyut yok|
|getcommands2|(Parça 2) alır|Sayı|Toplam||Boyut yok|
|setcommands2|Ayarlar (parça 2)|Sayı|Toplam||Boyut yok|
|operationsPerSecond2|(Parça 2) saniye başına işlem|Sayı|Maksimum||Boyut yok|
|evictedkeys2|Çıkarılan anahtarlar (parça 2)|Sayı|Toplam||Boyut yok|
|totalkeys2|Toplam anahtarları (parça 2)|Sayı|Maksimum||Boyut yok|
|expiredkeys2|Süresi dolan anahtarlar (parça 2)|Sayı|Toplam||Boyut yok|
|usedmemory2|Kullanılan bellek (parça 2)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss2|Kullanılan bellek RSS (parça 2)|Bayt|Maksimum||Boyut yok|
|serverLoad2|Sunucu iş yükü (parça 2)|Yüzde|Maksimum||Boyut yok|
|cacheWrite2|Önbellek yazma (parça 2)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead2|Önbellek Okuma (parça 2)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime2|CPU (parça 2)|Yüzde|Maksimum||Boyut yok|
|connectedclients3|Bağlı istemciler (parça 3)|Sayı|Maksimum||Boyut yok|
|totalcommandsprocessed3|Toplam işlemler (parça 3)|Sayı|Toplam||Boyut yok|
|cachehits3|(Parça 3) İsabetli Önbellek okuma sayısı|Sayı|Toplam||Boyut yok|
|cachemisses3|İsabetsiz önbellek okuma sayısı (parça 3)|Sayı|Toplam||Boyut yok|
|getcommands3|(Parça 3) alır|Sayı|Toplam||Boyut yok|
|setcommands3|Ayarlar (parça 3)|Sayı|Toplam||Boyut yok|
|operationsPerSecond3|(Parça 3) saniye başına işlem|Sayı|Maksimum||Boyut yok|
|evictedkeys3|Çıkarılan anahtarlar (parça 3)|Sayı|Toplam||Boyut yok|
|totalkeys3|Toplam anahtarları (parça 3)|Sayı|Maksimum||Boyut yok|
|expiredkeys3|Süresi dolan anahtarlar (parça 3)|Sayı|Toplam||Boyut yok|
|usedmemory3|Kullanılan bellek (parça 3)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss3|Kullanılan bellek RSS (parça 3)|Bayt|Maksimum||Boyut yok|
|serverLoad3|Sunucu iş yükü (parça 3)|Yüzde|Maksimum||Boyut yok|
|cacheWrite3|Önbellek yazma (parça 3)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead3|Önbellek Okuma (parça 3)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime3|CPU (parça 3)|Yüzde|Maksimum||Boyut yok|
|connectedclients4|Bağlı istemciler (parça 4)|Sayı|Maksimum||Boyut yok|
|totalcommandsprocessed4|Toplam işlemler (parça 4)|Sayı|Toplam||Boyut yok|
|cachehits4|İsabetli Önbellek Okuma (parça 4)|Sayı|Toplam||Boyut yok|
|cachemisses4|İsabetsiz önbellek okuma sayısı (parça 4)|Sayı|Toplam||Boyut yok|
|getcommands4|(Parça 4) alır|Sayı|Toplam||Boyut yok|
|setcommands4|Ayarlar (parça 4)|Sayı|Toplam||Boyut yok|
|operationsPerSecond4|(Parça 4) saniye başına işlem|Sayı|Maksimum||Boyut yok|
|evictedkeys4|Çıkarılan anahtarlar (parça 4)|Sayı|Toplam||Boyut yok|
|totalkeys4|Toplam anahtarları (parça 4)|Sayı|Maksimum||Boyut yok|
|expiredkeys4|Süresi dolan anahtarlar (parça 4)|Sayı|Toplam||Boyut yok|
|usedmemory4|Kullanılan bellek (parça 4)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss4|Kullanılan bellek RSS (parça 4)|Bayt|Maksimum||Boyut yok|
|serverLoad4|Sunucu iş yükü (parça 4)|Yüzde|Maksimum||Boyut yok|
|cacheWrite4|Önbellek yazma (parça 4)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead4|Önbellek Okuma (parça 4)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime4|CPU (parça 4)|Yüzde|Maksimum||Boyut yok|
|connectedclients5|Bağlı istemciler (parça 5)|Sayı|Maksimum||Boyut yok|
|totalcommandsprocessed5|Toplam işlemler (parça 5)|Sayı|Toplam||Boyut yok|
|cachehits5|(Parça 5) İsabetli Önbellek okuma sayısı|Sayı|Toplam||Boyut yok|
|cachemisses5|İsabetsiz önbellek okuma sayısı (parça 5)|Sayı|Toplam||Boyut yok|
|getcommands5|(Parça 5) alır|Sayı|Toplam||Boyut yok|
|setcommands5|Ayarlar (parça 5)|Sayı|Toplam||Boyut yok|
|operationsPerSecond5|(Parça 5) saniye başına işlem|Sayı|Maksimum||Boyut yok|
|evictedkeys5|Çıkarılan anahtarlar (parça 5)|Sayı|Toplam||Boyut yok|
|totalkeys5|Toplam anahtarları (parça 5)|Sayı|Maksimum||Boyut yok|
|expiredkeys5|Süresi dolan anahtarlar (parça 5)|Sayı|Toplam||Boyut yok|
|usedmemory5|Kullanılan bellek (parça 5)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss5|Kullanılan bellek RSS (parça 5)|Bayt|Maksimum||Boyut yok|
|serverLoad5|Sunucu iş yükü (parça 5)|Yüzde|Maksimum||Boyut yok|
|cacheWrite5|Önbellek yazma (parça 5)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead5|Önbellek Okuma (parça 5)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime5|CPU (parça 5)|Yüzde|Maksimum||Boyut yok|
|connectedclients6|Bağlı istemciler (parça 6)|Sayı|Maksimum||Boyut yok|
|totalcommandsprocessed6|Toplam işlemler (parça 6)|Sayı|Toplam||Boyut yok|
|cachehits6|(Parça 6) İsabetli Önbellek okuma sayısı|Sayı|Toplam||Boyut yok|
|cachemisses6|İsabetsiz önbellek okuma sayısı (parça 6)|Sayı|Toplam||Boyut yok|
|getcommands6|(Parça 6) alır|Sayı|Toplam||Boyut yok|
|setcommands6|Ayarlar (parça 6)|Sayı|Toplam||Boyut yok|
|operationsPerSecond6|(Parça 6) saniye başına işlem|Sayı|Maksimum||Boyut yok|
|evictedkeys6|Çıkarılan anahtarlar (parça 6)|Sayı|Toplam||Boyut yok|
|totalkeys6|Toplam anahtarları (parça 6)|Sayı|Maksimum||Boyut yok|
|expiredkeys6|Süresi dolan anahtarlar (parça 6)|Sayı|Toplam||Boyut yok|
|usedmemory6|Kullanılan bellek (parça 6)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss6|Kullanılan bellek RSS (parça 6)|Bayt|Maksimum||Boyut yok|
|serverLoad6|Sunucu iş yükü (parça 6)|Yüzde|Maksimum||Boyut yok|
|cacheWrite6|Önbellek yazma (parça 6)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead6|Önbellek Okuma (parça 6)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime6|CPU (parça 6)|Yüzde|Maksimum||Boyut yok|
|connectedclients7|Bağlı istemciler (parça 7)|Sayı|Maksimum||Boyut yok|
|totalcommandsprocessed7|Toplam işlemler (parça 7)|Sayı|Toplam||Boyut yok|
|cachehits7|İsabetli Önbellek Okuma (parça 7)|Sayı|Toplam||Boyut yok|
|cachemisses7|İsabetsiz önbellek okuma sayısı (parça 7)|Sayı|Toplam||Boyut yok|
|getcommands7|(Parça 7) alır|Sayı|Toplam||Boyut yok|
|setcommands7|Ayarlar (parça 7)|Sayı|Toplam||Boyut yok|
|operationsPerSecond7|(Parça 7) saniye başına işlem|Sayı|Maksimum||Boyut yok|
|evictedkeys7|Çıkarılan anahtarlar (parça 7)|Sayı|Toplam||Boyut yok|
|totalkeys7|Toplam anahtarları (parça 7)|Sayı|Maksimum||Boyut yok|
|expiredkeys7|Süresi dolan anahtarlar (parça 7)|Sayı|Toplam||Boyut yok|
|usedmemory7|Kullanılan bellek (parça 7)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss7|Kullanılan bellek RSS (parça 7)|Bayt|Maksimum||Boyut yok|
|serverLoad7|Sunucu iş yükü (parça 7)|Yüzde|Maksimum||Boyut yok|
|cacheWrite7|Önbellek yazma (parça 7)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead7|Önbellek Okuma (parça 7)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime7|CPU (parça 7)|Yüzde|Maksimum||Boyut yok|
|connectedclients8|Bağlı istemciler (parça 8)|Sayı|Maksimum||Boyut yok|
|totalcommandsprocessed8|Toplam işlemler (parça 8)|Sayı|Toplam||Boyut yok|
|cachehits8|(Parça 8) İsabetli Önbellek okuma sayısı|Sayı|Toplam||Boyut yok|
|cachemisses8|(Parça 8) İsabetsiz Önbellek okuma sayısı|Sayı|Toplam||Boyut yok|
|getcommands8|(Parça 8) alır|Sayı|Toplam||Boyut yok|
|setcommands8|Ayarlar (parça 8)|Sayı|Toplam||Boyut yok|
|operationsPerSecond8|(Parça 8) saniye başına işlem|Sayı|Maksimum||Boyut yok|
|evictedkeys8|Çıkarılan anahtarlar (parça 8)|Sayı|Toplam||Boyut yok|
|totalkeys8|Toplam anahtarları (parça 8)|Sayı|Maksimum||Boyut yok|
|expiredkeys8|Süresi dolan anahtarlar (parça 8)|Sayı|Toplam||Boyut yok|
|usedmemory8|Kullanılan bellek (parça 8)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss8|Kullanılan bellek RSS (parça 8)|Bayt|Maksimum||Boyut yok|
|serverLoad8|Sunucu iş yükü (parça 8)|Yüzde|Maksimum||Boyut yok|
|cacheWrite8|Önbellek yazma (parça 8)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead8|Önbellek Okuma (parça 8)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime8|CPU (parça 8)|Yüzde|Maksimum||Boyut yok|
|connectedclients9|Bağlı istemciler (parça 9)|Sayı|Maksimum||Boyut yok|
|totalcommandsprocessed9|Toplam işlemler (parça 9)|Sayı|Toplam||Boyut yok|
|cachehits9|İsabetli Önbellek Okuma (parça 9)|Sayı|Toplam||Boyut yok|
|cachemisses9|İsabetsiz önbellek okuma sayısı (parça 9)|Sayı|Toplam||Boyut yok|
|getcommands9|(Parça 9) alır|Sayı|Toplam||Boyut yok|
|setcommands9|Ayarlar (parça 9)|Sayı|Toplam||Boyut yok|
|operationsPerSecond9|İşlem / saniye (parça 9)|Sayı|Maksimum||Boyut yok|
|evictedkeys9|Çıkarılan anahtarlar (parça 9)|Sayı|Toplam||Boyut yok|
|totalkeys9|Toplam anahtarları (parça 9)|Sayı|Maksimum||Boyut yok|
|expiredkeys9|Süresi dolan anahtarlar (parça 9)|Sayı|Toplam||Boyut yok|
|usedmemory9|Kullanılan bellek (parça 9)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss9|Kullanılan bellek RSS (parça 9)|Bayt|Maksimum||Boyut yok|
|serverLoad9|Sunucu iş yükü (parça 9)|Yüzde|Maksimum||Boyut yok|
|cacheWrite9|Önbellek yazma (parça 9)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead9|Önbellek Okuma (parça 9)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime9|CPU (parça 9)|Yüzde|Maksimum||Boyut yok|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.ClassicCompute/virtualMachines

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|Sanal Makineler tarafından şu anda kullanılan ayrılmış işlem birimlerinin yüzdesi.|Boyut yok|
|Ağ Girişi|Ağ Girişi|Bayt|Toplam|Sanal Makineler tarafından tüm ağ arabirimleri üzerinde alınan bayt sayısı (Gelen Trafik).|Boyut yok|
|Ağ Çıkışı|Ağ Çıkışı|Bayt|Toplam|Sanal Makineler tarafından tüm ağ arabirimleri üzerinde gönderilen bayt sayısı (Giden Trafik).|Boyut yok|
|Disk Okuma Bayt/sn|Disk Okuma|BytesPerSecond|Ortalama|İzleme dönemi boyunca diskten okunan ortalama bayt.|Boyut yok|
|Disk Yazma Bayt/Sn|Disk Yazma|BytesPerSecond|Ortalama|İzleme dönemi boyunca diske yazılan ortalama bayt.|Boyut yok|
|Disk Okuma İşlemi/Sn|Disk Okuma İşlemi/Sn|CountPerSecond|Ortalama|Disk Okuma IOPS.|Boyut yok|
|Disk Yazma İşlemi/Sn|Disk Yazma İşlemi/Sn|CountPerSecond|Ortalama|Disk Yazma IOPS.|Boyut yok|

## <a name="microsoftclassiccomputedomainnamesslotsroles"></a>Microsoft.ClassicCompute/domainNames/slots/roles

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|Sanal Makineler tarafından şu anda kullanılan ayrılmış işlem birimlerinin yüzdesi.|RoleInstanceId|
|Ağ Girişi|Ağ Girişi|Bayt|Toplam|Sanal Makineler tarafından tüm ağ arabirimleri üzerinde alınan bayt sayısı (Gelen Trafik).|RoleInstanceId|
|Ağ Çıkışı|Ağ Çıkışı|Bayt|Toplam|Sanal Makineler tarafından tüm ağ arabirimleri üzerinde gönderilen bayt sayısı (Giden Trafik).|RoleInstanceId|
|Disk Okuma Bayt/sn|Disk Okuma|BytesPerSecond|Ortalama|İzleme dönemi boyunca diskten okunan ortalama bayt.|RoleInstanceId|
|Disk Yazma Bayt/Sn|Disk Yazma|BytesPerSecond|Ortalama|İzleme dönemi boyunca diske yazılan ortalama bayt.|RoleInstanceId|
|Disk Okuma İşlemi/Sn|Disk Okuma İşlemi/Sn|CountPerSecond|Ortalama|Disk Okuma IOPS.|RoleInstanceId|
|Disk Yazma İşlemi/Sn|Disk Yazma İşlemi/Sn|CountPerSecond|Ortalama|Disk Yazma IOPS.|RoleInstanceId|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TotalCalls|Toplam Çağrı Sayısı|Sayı|Toplam|Toplam çağrı sayısı.|Boyut yok|
|SuccessfulCalls|Başarılı Çağrılar|Sayı|Toplam|Başarılı olan çağrıların sayısı.|Boyut yok|
|TotalErrors|Hatalar Toplamı|Sayı|Toplam|Hata yanıtıyla sonuçlanan toplam çağrı sayısı (HTTP yanıt kodu 4xx veya 5xx).|Boyut yok|
|BlockedCalls|Engellenen Çağrılar|Sayı|Toplam|Oran veya kota sınırını aşan çağrı sayısı.|Boyut yok|
|ServerErrors|Sunucu Hataları|Sayı|Toplam|Hizmet iç hatasıyla sonuçlanan çağrı sayısı (HTTP yanıt kodu 5xx).|Boyut yok|
|ClientErrors|İstemci Hataları|Sayı|Toplam|İstemci tarafında hatayla sonuçlanan çağrı sayısı (HTTP yanıt kodu 4xx).|Boyut yok|
|Elinden|Data Girişi|Bayt|Toplam|Gelen verilerin bayt cinsinden boyutu.|Boyut yok|
|DataOut|Giden Veriler|Bayt|Toplam|Giden verilerin bayt cinsinden boyutu.|Boyut yok|
|Gecikme süresi|Gecikme süresi|Milisaniye|Ortalama|Milisaniye cinsinden gecikme süresi.|Boyut yok|
|CharactersTranslated|Çevrilen Karakterler|Sayı|Toplam|Gelen metin isteğindeki toplam karakter sayısı.|Boyut yok|
|SpeechSessionDuration|Konuşma Oturumu Süresi|Saniye|Toplam|Konuşma oturumunun saniye cinsinden toplam süresi.|Boyut yok|
|TotalTransactions|Toplam İşlemler|Sayı|Toplam|Toplam işlem sayısı.|Boyut yok|
|TotalTokenCalls|Toplam Belirteç Çağrısı|Sayı|Toplam|Toplam belirteç çağrısı sayısı.|Boyut yok|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|Sanal Makineler tarafından kullanılan ayrılmış işlem birimlerinin geçerli yüzdesi|Boyut yok|
|Ağ Girişi|Ağ Girişi|Bayt|Toplam|Sanal Makineler tarafından tüm ağ arabirimleri üzerinde alınan bayt sayısı (Gelen Trafik)|Boyut yok|
|Ağ Çıkışı|Ağ Çıkışı|Bayt|Toplam|Sanal Makineler tarafından tüm ağ arabirimleri üzerinde gönderilen bayt sayısı (Giden Trafik)|Boyut yok|
|Diskten Okunan Bayt|Diskten Okunan Bayt|Bayt|Toplam|İzleme dönemi boyunca diskten okunan toplam bayt|Boyut yok|
|Diske Yazılan Bayt|Diske Yazılan Bayt|Bayt|Toplam|İzleme dönemi boyunca diske yazılan toplam bayt|Boyut yok|
|Disk Okuma İşlemi/Sn|Disk Okuma İşlemi/Sn|CountPerSecond|Ortalama|Disk Okuma IOPS|Boyut yok|
|Disk Yazma İşlemi/Sn|Disk Yazma İşlemi/Sn|CountPerSecond|Ortalama|Disk Yazma IOPS|Boyut yok|
|Kalan CPU Kredisi|Kalan CPU Kredisi|Sayı|Ortalama|Veri bloğunun kullanabileceği toplam kredi sayısı|Boyut yok|
|Tüketilen CPU Kredisi|Tüketilen CPU Kredisi|Sayı|Ortalama|Sanal Makine tarafından tüketilen toplam kredi sayısı|Boyut yok|
|Disk Başına Okuma Bayt/sn|Veri Disk Okuma Bayt/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme döneminde tek bir diskten okunan toplam bayt/sn|SlotId|
|Disk Başına Yazma Bayt/sn|Veri Diski Yazma Bayt/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme döneminde tek bir diske yazılan toplam bayt/sn|SlotId|
|Disk Başına Okuma İşlemi/Sn|Veri Diski Okuma İşlemi/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme döneminde tek bir diskten okuma sırasında yapılan toplam IOPS|SlotId|
|Disk Başına Yazma İşlemi/Sn|Veri Diski Yazma İşlemi/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme döneminde tek bir diske yazma sırasında yapılan toplam IOPS|SlotId|
|Disk Başına QD|Veri diski QD (Önizleme)|Sayı|Ortalama|Veri Diski Sıra Derinliği (veya Sıra Uzunluğu)|SlotId|
|İşletim Sistemi Diski Başına Okuma Bayt/sn|İşletim Sistemi Diski Okuma Bayt/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme döneminde işletim sistemi diski için tek bir diskten okunan toplam bayt/sn|Boyut yok|
|İşletim Sistemi Diski Başına Yazma Bayt/sn|İşletim Sistemi Diski Yazma Bayt/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme döneminde işletim sistemi diski için tek bir diske yazılan toplam bayt/sn|Boyut yok|
|İşletim Sistemi Diski Başına Okuma İşlemi/Sn|İşletim Sistemi Diski Okuma İşlemi/Sn (Önizleme)|CountPerSecond|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten okuma sırasında yapılan toplam IOPS|Boyut yok|
|İşletim Sistemi Diski Başına Yazma İşlemi/Sn|İşletim Sistemi Diski Yazma İşlemi/Sn (Önizleme)|CountPerSecond|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diske yazma sırasında yapılan toplam IOPS|Boyut yok|
|İşletim Sistemi Disk Başına QD|İşletim sistemi diski QD (Önizleme)|Sayı|Ortalama|İşletim Sistemi Diski Sıra Derinliği (veya Sıra Uzunluğu)|Boyut yok|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|Sanal Makineler tarafından kullanılan ayrılmış işlem birimlerinin geçerli yüzdesi|Boyut yok|
|Ağ Girişi|Ağ Girişi|Bayt|Toplam|Sanal Makineler tarafından tüm ağ arabirimleri üzerinde alınan bayt sayısı (Gelen Trafik)|Boyut yok|
|Ağ Çıkışı|Ağ Çıkışı|Bayt|Toplam|Sanal Makineler tarafından tüm ağ arabirimleri üzerinde gönderilen bayt sayısı (Giden Trafik)|Boyut yok|
|Diskten Okunan Bayt|Diskten Okunan Bayt|Bayt|Toplam|İzleme dönemi boyunca diskten okunan toplam bayt|Boyut yok|
|Diske Yazılan Bayt|Diske Yazılan Bayt|Bayt|Toplam|İzleme dönemi boyunca diske yazılan toplam bayt|Boyut yok|
|Disk Okuma İşlemi/Sn|Disk Okuma İşlemi/Sn|CountPerSecond|Ortalama|Disk Okuma IOPS|Boyut yok|
|Disk Yazma İşlemi/Sn|Disk Yazma İşlemi/Sn|CountPerSecond|Ortalama|Disk Yazma IOPS|Boyut yok|
|Kalan CPU Kredisi|Kalan CPU Kredisi|Sayı|Ortalama|Veri bloğunun kullanabileceği toplam kredi sayısı|Boyut yok|
|Tüketilen CPU Kredisi|Tüketilen CPU Kredisi|Sayı|Ortalama|Sanal Makine tarafından tüketilen toplam kredi sayısı|Boyut yok|
|Disk Başına Okuma Bayt/sn|Veri Disk Okuma Bayt/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme döneminde tek bir diskten okunan toplam bayt/sn|SlotId|
|Disk Başına Yazma Bayt/sn|Veri Diski Yazma Bayt/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme döneminde tek bir diske yazılan toplam bayt/sn|SlotId|
|Disk Başına Okuma İşlemi/Sn|Veri Diski Okuma İşlemi/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme döneminde tek bir diskten okuma sırasında yapılan toplam IOPS|SlotId|
|Disk Başına Yazma İşlemi/Sn|Veri Diski Yazma İşlemi/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme döneminde tek bir diske yazma sırasında yapılan toplam IOPS|SlotId|
|Disk Başına QD|Veri diski QD (Önizleme)|Sayı|Ortalama|Veri Diski Sıra Derinliği (veya Sıra Uzunluğu)|SlotId|
|İşletim Sistemi Diski Başına Okuma Bayt/sn|İşletim sistemi diski Okuma Bayt/sn|CountPerSecond|Ortalama|İzleme döneminde işletim sistemi diski için tek bir diskten okunan toplam bayt/sn|Boyut yok|
|İşletim Sistemi Diski Başına Yazma Bayt/sn|İşletim Sistemi Diski Yazma Bayt/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme döneminde işletim sistemi diski için tek bir diske yazılan toplam bayt/sn|Boyut yok|
|İşletim Sistemi Diski Başına Okuma İşlemi/Sn|İşletim Sistemi Diski Okuma İşlemi/Sn (Önizleme)|CountPerSecond|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten okuma sırasında yapılan toplam IOPS|Boyut yok|
|İşletim Sistemi Diski Başına Yazma İşlemi/Sn|İşletim Sistemi Diski Yazma İşlemi/Sn (Önizleme)|CountPerSecond|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diske yazma sırasında yapılan toplam IOPS|Boyut yok|
|İşletim Sistemi Disk Başına QD|İşletim sistemi diski QD (Önizleme)|Sayı|Ortalama|İşletim Sistemi Diski Sıra Derinliği (veya Sıra Uzunluğu)|Boyut yok|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|Sanal Makineler tarafından kullanılan ayrılmış işlem birimlerinin geçerli yüzdesi|Boyut yok|
|Ağ Girişi|Ağ Girişi|Bayt|Toplam|Sanal Makineler tarafından tüm ağ arabirimleri üzerinde alınan bayt sayısı (Gelen Trafik)|Boyut yok|
|Ağ Çıkışı|Ağ Çıkışı|Bayt|Toplam|Sanal Makineler tarafından tüm ağ arabirimleri üzerinde gönderilen bayt sayısı (Giden Trafik)|Boyut yok|
|Diskten Okunan Bayt|Diskten Okunan Bayt|Bayt|Toplam|İzleme dönemi boyunca diskten okunan toplam bayt|Boyut yok|
|Diske Yazılan Bayt|Diske Yazılan Bayt|Bayt|Toplam|İzleme dönemi boyunca diske yazılan toplam bayt|Boyut yok|
|Disk Okuma İşlemi/Sn|Disk Okuma İşlemi/Sn|CountPerSecond|Ortalama|Disk Okuma IOPS|Boyut yok|
|Disk Yazma İşlemi/Sn|Disk Yazma İşlemi/Sn|CountPerSecond|Ortalama|Disk Yazma IOPS|Boyut yok|
|Kalan CPU Kredisi|Kalan CPU Kredisi|Sayı|Ortalama|Veri bloğunun kullanabileceği toplam kredi sayısı|Boyut yok|
|Tüketilen CPU Kredisi|Tüketilen CPU Kredisi|Sayı|Ortalama|Sanal Makine tarafından tüketilen toplam kredi sayısı|Boyut yok|
|Disk Başına Okuma Bayt/sn|Veri Disk Okuma Bayt/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme döneminde tek bir diskten okunan toplam bayt/sn|SlotId|
|Disk Başına Yazma Bayt/sn|Veri Diski Yazma Bayt/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme döneminde tek bir diske yazılan toplam bayt/sn|SlotId|
|Disk Başına Okuma İşlemi/Sn|Veri Diski Okuma İşlemi/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme döneminde tek bir diskten okuma sırasında yapılan toplam IOPS|SlotId|
|Disk Başına Yazma İşlemi/Sn|Veri Diski Yazma İşlemi/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme döneminde tek bir diske yazma sırasında yapılan toplam IOPS|SlotId|
|Disk Başına QD|Veri diski QD (Önizleme)|Sayı|Ortalama|Veri Diski Sıra Derinliği (veya Sıra Uzunluğu)|SlotId|
|İşletim Sistemi Diski Başına Okuma Bayt/sn|İşletim Sistemi Diski Okuma Bayt/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme döneminde işletim sistemi diski için tek bir diskten okunan toplam bayt/sn|Boyut yok|
|İşletim Sistemi Diski Başına Yazma Bayt/sn|İşletim Sistemi Diski Yazma Bayt/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme döneminde işletim sistemi diski için tek bir diske yazılan toplam bayt/sn|Boyut yok|
|İşletim Sistemi Diski Başına Okuma İşlemi/Sn|İşletim Sistemi Diski Okuma İşlemi/Sn (Önizleme)|CountPerSecond|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten okuma sırasında yapılan toplam IOPS|Boyut yok|
|İşletim Sistemi Diski Başına Yazma İşlemi/Sn|İşletim Sistemi Diski Yazma İşlemi/Sn (Önizleme)|CountPerSecond|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diske yazma sırasında yapılan toplam IOPS|Boyut yok|
|İşletim Sistemi Disk Başına QD|İşletim sistemi diski QD (Önizleme)|Sayı|Ortalama|İşletim Sistemi Diski Sıra Derinliği (veya Sıra Uzunluğu)|Boyut yok|

## <a name="microsoftcontainerinstancecontainergroups"></a>Microsoft.ContainerInstance/containerGroups

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CpuUsage|CPU Kullanımı|Sayı|Ortalama|Milicore'daki tüm çekirdeklerde CPU kullanımı.|kapsayıcı adı|
|MemoryUsage|Bellek Kullanımı|Bayt|Ortalama|Bayt olarak toplam bellek kullanımı.|kapsayıcı adı|
|NetworkBytesReceivedPerSecond|Saniyede Alınan Ağ Bant Sayısı|Bayt|Ortalama|Saniyede alınan ağ bayt sayısı.|Boyut yok|
|NetworkBytesTransmittedPerSecond|Saniyede Aktarılan Ağ Bant Sayısı|Bayt|Ortalama|Saniyede aktarılan ağ bant sayısı.|Boyut yok|

## <a name="microsoftcontainerservicemanagedclusters"></a>Microsoft.ContainerService/managedClusters

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|kube_node_status_allocatable_cpu_cores|Yönetilen bir kümede kullanılabilir cpu çekirdek sayısı|Sayı|Toplam|Yönetilen bir kümede kullanılabilir cpu çekirdek sayısı|Boyut yok|
|kube_node_status_allocatable_memory_bytes|Yönetilen bir kümede kullanılabilir belleğin toplam tutar|Bayt|Toplam|Yönetilen bir kümede kullanılabilir belleğin toplam tutar|Boyut yok|
|kube_pod_status_ready|Hazır durumda pod'ların sayısını|Sayı|Toplam|Hazır durumda pod'ların sayısını|ad alanı, pod|
|kube_node_status_condition|Çeşitli düğümü koşulları için durumları|Sayı|Toplam|Çeşitli düğümü koşulları için durumları|Koşul, durum, düğüm|
|kube_pod_status_phase|Aşama tarafından pod'ların sayısını|Sayı|Toplam|Aşama tarafından pod'ların sayısını|Aşama, ad alanı, pod|

## <a name="microsoftcustomerinsightshubs"></a>Microsoft.CustomerInsights/hubs

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|DCIApiCalls|Müşteri öngörüleri API çağrıları|Sayı|Toplam||Boyut yok|
|DCIMappingImportOperationSuccessfulLines|Eşleme içeri aktarma işlemi başarılı satırları|Sayı|Toplam||Boyut yok|
|DCIMappingImportOperationFailedLines|Satırları eşleme içeri aktarma işlemi başarısız oldu|Sayı|Toplam||Boyut yok|
|DCIMappingImportOperationTotalLines|Eşleme içeri aktarma işlemi toplam satırlar|Sayı|Toplam||Boyut yok|
|DCIMappingImportOperationRuntimeInSeconds|Eşleme alma işlemi çalışma zamanı saniye|Saniye|Toplam||Boyut yok|
|DCIOutboundProfileExportSucceeded|Giden profili dışarı aktarma başarılı oldu|Sayı|Toplam||Boyut yok|
|DCIOutboundProfileExportFailed|Giden profili dışarı aktarılamadı|Sayı|Toplam||Boyut yok|
|DCIOutboundProfileExportDuration|Giden profili dışarı aktarma süresi|Saniye|Toplam||Boyut yok|
|DCIOutboundKpiExportSucceeded|Giden KPI dışarı aktarma başarılı oldu|Sayı|Toplam||Boyut yok|
|DCIOutboundKpiExportFailed|Giden KPI dışarı aktarma başarısız oldu|Sayı|Toplam||Boyut yok|
|DCIOutboundKpiExportDuration|Giden KPI verme süresi|Saniye|Toplam||Boyut yok|
|DCIOutboundKpiExportStarted|Giden KPI dışarı aktarma başlatıldı|Saniye|Toplam||Boyut yok|
|DCIOutboundKpiRecordCount|Giden KPI kayıt sayısı|Saniye|Toplam||Boyut yok|
|DCIOutboundProfileExportCount|Giden profili dışarı aktarma sayısı|Saniye|Toplam||Boyut yok|
|DCIOutboundInitialProfileExportFailed|Giden ilk profili dışarı aktarılamadı|Saniye|Toplam||Boyut yok|
|DCIOutboundInitialProfileExportSucceeded|Giden ilk profili dışarı aktarma başarılı oldu|Saniye|Toplam||Boyut yok|
|DCIOutboundInitialKpiExportFailed|Giden ilk KPI dışarı aktarma başarısız oldu|Saniye|Toplam||Boyut yok|
|DCIOutboundInitialKpiExportSucceeded|Giden ilk KPI dışarı aktarma başarılı oldu|Saniye|Toplam||Boyut yok|
|DCIOutboundInitialProfileExportDurationInSeconds|Saniye cinsinden giden ilk profili dışarı aktarma süresi|Saniye|Toplam||Boyut yok|
|AdlaJobForStandardKpiFailed|Adla işiniz için standart KPI saniyeler içinde başarısız oldu|Saniye|Toplam||Boyut yok|
|AdlaJobForStandardKpiTimeOut|Adla işiniz için standart KPI zaman aşımı saniye|Saniye|Toplam||Boyut yok|
|AdlaJobForStandardKpiCompleted|Adla işiniz için standart KPI saniyeler içinde tamamlandı|Saniye|Toplam||Boyut yok|
|ImportASAValuesFailed|İçeri aktarma ASA değerleri başarısız sayısı|Sayı|Toplam||Boyut yok|
|ImportASAValuesSucceeded|İçeri aktarma ASA değerleri sayısı başarılı oldu|Sayı|Toplam||Boyut yok|
|DCIProfilesCount|Profili örnek sayısı|Sayı|Son||Boyut yok|
|DCIInteractionsPerMonthCount|Etkileşimler her ay sayısı|Sayı|Son||Boyut yok|
|DCIKpisCount|KPI sayısı|Sayı|Son||Boyut yok|
|DCISegmentsCount|Segment Sayısı|Sayı|Son||Boyut yok|
|DCIPredictiveMatchPoliciesCount|Tahmine dayalı eşleşme sayısı|Sayı|Son||Boyut yok|
|DCIPredictionsCount|Tahmin sayısı|Sayı|Son||Boyut yok|

## <a name="microsoftdatafactorydatafactories"></a>Microsoft.DataFactory/datafactories

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|FailedRuns|Başarısız Çalışma Sayısı|Sayı|Toplam||pipelineName, activityName, windowEnd, windowStart|
|SuccessfulRuns|Başarılı Çalışma Sayısı|Sayı|Toplam||pipelineName, activityName, windowEnd, windowStart|

## <a name="microsoftdatafactoryfactories"></a>Microsoft.DataFactory/factories

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|PipelineFailedRuns|İşlem hattı çalıştırmaları ölçümleri başarısız oldu|Sayı|Toplam||FailureType, adı|
|PipelineSucceededRuns|İşlem hattı çalıştırmaları ölçümleri başarılı oldu|Sayı|Toplam||FailureType, adı|
|ActivityFailedRuns|Etkinlik çalıştırmaları ölçümlerinin başarısız oldu|Sayı|Toplam||ActivityType, PipelineName, FailureType, adı|
|ActivitySucceededRuns|Etkinlik çalıştırmaları ölçümlerinin başarılı oldu|Sayı|Toplam||ActivityType, PipelineName, FailureType, adı|
|TriggerFailedRuns|Tetikleyici çalıştırmaları ölçümleri başarısız oldu|Sayı|Toplam||Ad, FailureType|
|TriggerSucceededRuns|Tetikleyici çalıştırmaları ölçümleri başarılı oldu|Sayı|Toplam||Ad, FailureType|
|IntegrationRuntimeCpuPercentage|Tümleştirme çalışma zamanı CPU kullanımı|Yüzde|Ortalama||IntegrationRuntimeName, NodeName|
|IntegrationRuntimeAvailableMemory|Tümleştirme çalışma zamanı kullanılabilir bellek|Bayt|Ortalama||IntegrationRuntimeName, NodeName|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|JobEndedSuccess|Başarılı işler|Sayı|Toplam|Başarılı İşler sayısı.|Boyut yok|
|JobEndedFailure|Başarısız olan işler|Sayı|Toplam|Başarısız işleri sayısı.|Boyut yok|
|JobEndedCancelled|İptal edilen işler|Sayı|Toplam|İptal edilen işler sayısı.|Boyut yok|
|JobAUEndedSuccess|Başarılı AU saati|Saniye|Toplam|Toplam AU saati için başarılı işler.|Boyut yok|
|JobAUEndedFailure|Başarısız AU saati|Saniye|Toplam|Toplam AU saati için başarısız olan işler.|Boyut yok|
|JobAUEndedCancelled|İptal edilen AU saati|Saniye|Toplam|İptal edilen işler için toplam AU saati.|Boyut yok|

## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TotalStorage|Toplam Depolama Alanı|Bayt|Maksimum|Hesapta depolanan verilerin toplam miktarı.|Boyut yok|
|DataWritten|Yazılan Veriler|Bayt|Toplam|Toplam hesabına yazılan veri miktarı.|Boyut yok|
|DataRead|Okunan Veriler|Bayt|Toplam|Toplam veri miktarı hesaptan okuyun.|Boyut yok|
|WriteRequests|Yazma istekleri|Sayı|Toplam|Hesabı için yazma isteği veri sayısı.|Boyut yok|
|ReadRequests|Okuma istekleri|Sayı|Toplam|Okuma istekleri hesabına veri sayısı.|Boyut yok|

## <a name="microsoftdbformariadbservers"></a>Microsoft.DBforMariaDB/servers

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|Boyut yok|
|memory_percent|Bellek yüzdesi|Yüzde|Ortalama|Bellek yüzdesi|Boyut yok|
|io_consumption_percent|G/ç yüzdesi|Yüzde|Ortalama|G/ç yüzdesi|Boyut yok|
|storage_percent|Depolama yüzdesi|Yüzde|Ortalama|Depolama yüzdesi|Boyut yok|
|storage_used|Kullanılan depolama|Bayt|Ortalama|Kullanılan depolama|Boyut yok|
|storage_limit|Depolama sınırı|Bayt|Ortalama|Depolama sınırı|Boyut yok|
|serverlog_storage_percent|Sunucu günlüğü depolama yüzdesi|Yüzde|Ortalama|Sunucu günlüğü depolama yüzdesi|Boyut yok|
|serverlog_storage_usage|Kullanılan sunucu günlük depolama alanı|Bayt|Ortalama|Kullanılan sunucu günlük depolama alanı|Boyut yok|
|serverlog_storage_limit|Sunucu günlük depolama sınırı|Bayt|Ortalama|Sunucu günlük depolama sınırı|Boyut yok|
|active_connections|Etkin bağlantılar|Sayı|Ortalama|Etkin bağlantılar|Boyut yok|
|connections_failed|Başarısız Bağlantılar|Sayı|Toplam|Başarısız Bağlantılar|Boyut yok|
|network_bytes_egress|Ağ Çıkışı|Bayt|Toplam|Etkin bağlantılar arasında ağ çıkışı|Boyut yok|
|network_bytes_ingress|Ağ Girişi|Bayt|Toplam|Etkin bağlantılar arasında ağ içinde|Boyut yok|

## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|Boyut yok|
|memory_percent|Bellek yüzdesi|Yüzde|Ortalama|Bellek yüzdesi|Boyut yok|
|io_consumption_percent|G/ç yüzdesi|Yüzde|Ortalama|G/ç yüzdesi|Boyut yok|
|storage_percent|Depolama yüzdesi|Yüzde|Ortalama|Depolama yüzdesi|Boyut yok|
|storage_used|Kullanılan depolama|Bayt|Ortalama|Kullanılan depolama|Boyut yok|
|storage_limit|Depolama sınırı|Bayt|Ortalama|Depolama sınırı|Boyut yok|
|serverlog_storage_percent|Sunucu günlüğü depolama yüzdesi|Yüzde|Ortalama|Sunucu günlüğü depolama yüzdesi|Boyut yok|
|serverlog_storage_usage|Kullanılan sunucu günlük depolama alanı|Bayt|Ortalama|Kullanılan sunucu günlük depolama alanı|Boyut yok|
|serverlog_storage_limit|Sunucu günlük depolama sınırı|Bayt|Ortalama|Sunucu günlük depolama sınırı|Boyut yok|
|active_connections|Etkin bağlantılar|Sayı|Ortalama|Etkin bağlantılar|Boyut yok|
|connections_failed|Başarısız Bağlantılar|Sayı|Toplam|Başarısız Bağlantılar|Boyut yok|
|seconds_behind_master|Saniyeler içinde çoğaltma gecikmesi|Sayı|Ortalama|Saniyeler içinde çoğaltma gecikmesi|Boyut yok|
|network_bytes_egress|Ağ Çıkışı|Bayt|Toplam|Etkin bağlantılar arasında ağ çıkışı|Boyut yok|
|network_bytes_ingress|Ağ Girişi|Bayt|Toplam|Etkin bağlantılar arasında ağ içinde|Boyut yok|

## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|Boyut yok|
|memory_percent|Bellek yüzdesi|Yüzde|Ortalama|Bellek yüzdesi|Boyut yok|
|io_consumption_percent|G/ç yüzdesi|Yüzde|Ortalama|G/ç yüzdesi|Boyut yok|
|storage_percent|Depolama yüzdesi|Yüzde|Ortalama|Depolama yüzdesi|Boyut yok|
|storage_used|Kullanılan depolama|Bayt|Ortalama|Kullanılan depolama|Boyut yok|
|storage_limit|Depolama sınırı|Bayt|Ortalama|Depolama sınırı|Boyut yok|
|serverlog_storage_percent|Sunucu günlüğü depolama yüzdesi|Yüzde|Ortalama|Sunucu günlüğü depolama yüzdesi|Boyut yok|
|serverlog_storage_usage|Kullanılan sunucu günlük depolama alanı|Bayt|Ortalama|Kullanılan sunucu günlük depolama alanı|Boyut yok|
|serverlog_storage_limit|Sunucu günlük depolama sınırı|Bayt|Ortalama|Sunucu günlük depolama sınırı|Boyut yok|
|active_connections|Etkin bağlantılar|Sayı|Ortalama|Etkin bağlantılar|Boyut yok|
|connections_failed|Başarısız Bağlantılar|Sayı|Toplam|Başarısız Bağlantılar|Boyut yok|
|network_bytes_egress|Ağ Çıkışı|Bayt|Toplam|Etkin bağlantılar arasında ağ çıkışı|Boyut yok|
|network_bytes_ingress|Ağ Girişi|Bayt|Toplam|Etkin bağlantılar arasında ağ içinde|Boyut yok|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/ıothubs

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Telemetri ileti gönderme denemeleri|Sayı|Toplam|IOT hub'ınıza gönderilecek CİHAZDAN buluta telemetri iletilerini sayısı çalıştı|Boyut yok|
|d2c.telemetry.ingress.Success|Gönderilen telemetri iletilerini|Sayı|Toplam|Başarılı bir şekilde IOT hub'ınıza CİHAZDAN buluta telemetri ileti sayısı|Boyut yok|
|c2d.commands.egress.complete.success|Komut tamamlandı|Sayı|Toplam|Cihaz tarafından başarıyla tamamlandı bulut-cihaz komutlarının sayısı|Boyut yok|
|c2d.commands.egress.abandon.success|Terk komutları|Sayı|Toplam|Cihaz tarafından terk bulut-cihaz komutlarının sayısı|Boyut yok|
|c2d.commands.egress.reject.success|Reddedilen komutları|Sayı|Toplam|Cihaz tarafından reddedilen bulut-cihaz komutlarının sayısı|Boyut yok|
|devices.totalDevices|(Kullanım dışı) toplam cihaz sayısı|Sayı|Toplam|IOT hub'ınıza kayıtlı cihaz sayısı|Boyut yok|
|devices.connectedDevices.allProtocol|Bağlı cihazlar (kullanım dışı) |Sayı|Toplam|IOT hub'ınıza bağlı cihazların sayısı|Boyut yok|
|d2c.telemetry.egress.success|Yönlendirme: teslim telemetri iletilerini|Sayı|Toplam|İletileri IOT Hub'ın yönlendirme kullanarak tüm uç noktalara başarıyla teslim sayısı. Bir ileti birden çok Uç noktalara yönlendirilir, bu değer her başarılı bir teslimat için bir tane artırır. Bir ileti birden çok kez aynı uç noktasına teslim ise bu değer her başarılı bir teslimat için bir tane artırır.|Boyut yok|
|d2c.telemetry.egress.dropped|Yönlendirme: bırakılan telemetri iletilerini |Sayı|Toplam|İletileri IOT Hub'ın nedeniyle ölü uç noktalarına yönlendirme tarafından bırakılan sayısı. Bu değer, geri dönüş yol bırakılan iletiler var. iletilmemiş olarak sunulan iletiler sayılmaz.|Boyut yok|
|d2c.telemetry.egress.orphaned|Yönlendirme: yalnız bırakılmış telemetri iletilerini |Sayı|Toplam|(Temel kural dahil) tüm yönlendirme kuralları ile eşleşmedi çünkü iletileri IOT Hub'ı yönlendirerek yalnız bırakılmış sayısı. |Boyut yok|
|d2c.telemetry.egress.invalid|Yönlendirme: uyumsuz telemetri iletilerini|Sayı|Toplam|IOT Hub'ın yönlendirme uç nokta ile uyumsuzluk nedeniyle ileti teslim edilemedi sayısı. Bu değer, yeniden denemeler içermez.|Boyut yok|
|d2c.telemetry.egress.fallback|Yönlendirme: ileti geri dönüş için teslim|Sayı|Toplam|Geri dönüş rota ile ilişkili uç noktasına iletileri IOT Hub'ın yönlendirme teslim sayısı.|Boyut yok|
|d2c.endpoints.egress.eventHubs|Yönlendirme: iletilerin olay Hub'ına teslim|Sayı|Toplam|IOT hub'ı başarıyla yönlendirme, olay hub'ı uç noktalar için ileti teslim sayısı.|Boyut yok|
|d2c.endpoints.latency.eventHubs|Yönlendirme: gecikme süresi için Event Hub ileti|Milisaniye|Ortalama|Ortalama gecikme süresi (milisaniye) IOT hub'ına giriş iletisi ve ileti giriş arasında bir olay hub'ı uç nokta olarak.|Boyut yok|
|d2c.endpoints.egress.serviceBusQueues|Yönlendirme: Service Bus kuyruğuna iletiler teslim|Sayı|Toplam|IOT hub'ı başarıyla yönlendirme iletileri Service Bus kuyruğu Uç noktalara teslim sayısı.|Boyut yok|
|d2c.endpoints.latency.serviceBusQueues|Yönlendirme: gecikme süresi için Service Bus kuyruğuna ileti|Milisaniye|Ortalama|Ortalama gecikme süresi (milisaniye) ileti giriş IOT hub'ına telemetri iletisi giriş arasındaki içine bir Service Bus kuyruk uç noktası.|Boyut yok|
|d2c.endpoints.egress.serviceBusTopics|Yönlendirme: Service Bus konusuna iletiler teslim|Sayı|Toplam|IOT hub'ı başarıyla yönlendirme iletileri Service Bus konu Uç noktalara teslim sayısı.|Boyut yok|
|d2c.endpoints.latency.serviceBusTopics|Yönlendirme: gecikme süresi için Service Bus konusuna ileti|Milisaniye|Ortalama|Ortalama gecikme süresi (milisaniye) ileti giriş IOT hub'ına telemetri iletisi giriş arasındaki içine bir Service Bus konu başlığı uç noktası.|Boyut yok|
|d2c.endpoints.egress.builtIn.events|Yönlendirme: iletiler iletiler/olaylar için teslim|Sayı|Toplam|IOT hub'ı başarıyla Yönlendirme iletilerini yerleşik uç noktası (iletiler/olaylar) teslim sayısı.|Boyut yok|
|d2c.endpoints.latency.builtIn.events|Yönlendirme: gecikme süresi, iletiler/olaylar ileti|Milisaniye|Ortalama|Ortalama gecikme süresi (milisaniye) ileti giriş IOT hub'ına telemetri iletisi giriş arasındaki içine yerleşik uç noktası (iletiler/olaylar).|Boyut yok|
|d2c.endpoints.egress.storage|Yönlendirme: depolama için ileti teslim|Sayı|Toplam|IOT hub'ı başarıyla yönlendirme depolama uç noktaları için ileti teslim sayısı.|Boyut yok|
|d2c.endpoints.latency.storage|Yönlendirme: depolama gecikmesi ileti|Milisaniye|Ortalama|Ortalama gecikme süresi (milisaniye) ileti giriş IOT hub'ına telemetri iletisi giriş arasındaki içine bir depolama uç noktası.|Boyut yok|
|d2c.endpoints.egress.Storage.bytes|Yönlendirme: veri depolama için teslim|Bayt|Toplam|Veri (bayt), IOT Hub'ın yönlendirme depolama uç noktaları için teslim.|Boyut yok|
|d2c.endpoints.egress.Storage.BLOBS|Yönlendirme: BLOB depolama alanına teslim|Sayı|Toplam|IOT Hub'ın yönlendirme BLOB Depolama uç noktaları için teslim sayısı.|Boyut yok|
|d2c.twin.read.success|Cihazlardan başarılı ikizi okumaları|Sayı|Toplam|Tüm başarılı cihaz tarafından başlatılan çiftlerde okuma sayısı.|Boyut yok|
|d2c.twin.read.failure|Cihazlardan çiftlerde okuma başarısız oldu|Sayı|Toplam|Tüm sayısı, cihaz tarafından başlatılan çiftlerde okuma başarısız oldu.|Boyut yok|
|d2c.Twin.Read.size|Çiftlerde okuma cihazlardan yanıt boyutu|Bayt|Ortalama|Ortalama, en düşük ve en fazla başarılı olan tüm cihaz tarafından başlatılan ikizi okur.|Boyut yok|
|d2c.twin.update.success|Cihazlardan başarılı ikizi güncelleştirmeleri|Sayı|Toplam|Tüm başarılı tarafından başlatılan cihaz ikizi güncelleştirmeleri sayısı.|Boyut yok|
|d2c.twin.update.failure|Cihaz ikizi güncelleştirmeleri başarısız oldu|Sayı|Toplam|Tüm sayısı tarafından başlatılan cihaz ikizi güncelleştirmeleri başarısız oldu.|Boyut yok|
|d2c.twin.update.size|Cihaz ikizi güncelleştirmeleri boyutu|Bayt|Ortalama|Cihaz tarafından başlatılan ortalama, en düşük ve en büyük boyutu başarılı olan tüm güncelleştirmeleri çifti.|Boyut yok|
|c2d.methods.success|Başarılı bir doğrudan yöntem çağrıları|Sayı|Toplam|Tüm başarılı bir doğrudan yöntem çağrılarının sayısı.|Boyut yok|
|c2d.methods.failure|Doğrudan yöntem çağrıları başarısız oldu|Sayı|Toplam|Tüm sayısı doğrudan yöntem çağrısı başarısız oldu.|Boyut yok|
|c2d.methods.requestSize|Doğrudan yöntem çağrılarını isteği boyutu|Bayt|Ortalama|Ortalama, minimum ve maksimum başarılı olan tüm yöntemi istekleri doğrudan.|Boyut yok|
|c2d.methods.responseSize|Doğrudan yöntem çağrılarını yanıt boyutu|Bayt|Ortalama|Ortalama, en az ve en fazla doğrudan yöntem yanıtların tümü başarılı.|Boyut yok|
|c2d.twin.read.success|Arka uçtan başarılı ikizi okumaları|Sayı|Toplam|Tüm başarılı arka uç başlatılan çiftlerde okuma sayısı.|Boyut yok|
|c2d.Twin.Read.failure|Arka uçtan başarısız ikizi okumaları|Sayı|Toplam|Tüm sayısı, arka uç başlatılan çiftlerde okuma başarısız oldu.|Boyut yok|
|c2d.Twin.Read.size|Yanıt boyutu ikizinin arka ucundan okur|Bayt|Ortalama|Ortalama, en düşük ve en fazla başarılı olan tüm arka uç başlatılan ikizi okur.|Boyut yok|
|c2d.twin.update.success|Arka uç başarılı ikizi güncelleştirmeleri|Sayı|Toplam|Tüm başarılı arka uç başlatılan ikizi güncelleştirmeleri sayısı.|Boyut yok|
|c2d.twin.update.failure|Arka uç başarısız ikizi güncelleştirmeleri|Sayı|Toplam|Tüm sayısı, arka uç başlatılan ikizi güncelleştirmeleri başarısız oldu.|Boyut yok|
|c2d.twin.update.size|Arka uç ikizi güncelleştirmeleri boyutu|Bayt|Ortalama|Arka uç başlatılan ortalama, en düşük ve en büyük boyutu başarılı olan tüm güncelleştirmeleri çifti.|Boyut yok|
|twinQueries.success|Başarılı çifti sorguları|Sayı|Toplam|Tüm başarılı ikizi sorgularının sayısı.|Boyut yok|
|twinQueries.failure|Başarısız çifti sorguları|Sayı|Toplam|Tüm başarısız ikizi sorgularının sayısı.|Boyut yok|
|twinQueries.resultSize|İkiz sorgu sonucu boyutu|Bayt|Ortalama|Ortalama, en düşük ve en fazla sonuç boyutunun tüm başarılı çifti sorguları.|Boyut yok|
|jobs.createTwinUpdateJob.success|İkiz güncelleştirmesi işlerinin başarılı oluşturma|Sayı|Toplam|Tüm oluşturma başarılı ikiz güncelleştirmesi işlerin sayısı.|Boyut yok|
|jobs.createTwinUpdateJob.failure|İkiz güncelleştirmesi işlerinin başarısız oluşturma|Sayı|Toplam|İkiz güncelleştirmesi işlerinin başarısız olan tüm oluşturma sayısı.|Boyut yok|
|jobs.createDirectMethodJob.success|Yöntem çağırma işlerinin başarılı oluşturma|Sayı|Toplam|Tüm oluşturma başarılı doğrudan yöntem çağırma işlerin sayısı.|Boyut yok|
|jobs.createDirectMethodJob.failure|Yöntem çağırma işlerinin başarısız oluşturma|Sayı|Toplam|Tüm başarısız oluşturma doğrudan yöntem çağırma işlerin sayısı.|Boyut yok|
|jobs.listJobs.success|Başarılı çağrılar listeleyemeyeceksiniz|Sayı|Toplam|Tüm başarılı çağrı listeleyemeyeceksiniz sayısı.|Boyut yok|
|jobs.listJobs.failure|Başarısız çağrıları listeleyemeyeceksiniz|Sayı|Toplam|Tüm başarısız çağrılar listeleyemeyeceksiniz sayısı.|Boyut yok|
|jobs.cancelJob.success|Başarılı iş iptalleri|Sayı|Toplam|Bir işi iptal etmek için tüm başarılı çağrı sayısı.|Boyut yok|
|jobs.cancelJob.failure|Başarısız iş iptalleri|Sayı|Toplam|Bir işi iptal etmek için tüm başarısız çağrıların sayısı.|Boyut yok|
|jobs.queryJobs.success|İş başarılı sorguları|Sayı|Toplam|Sorgu işleri yapılan tüm başarılı çağrı sayısı.|Boyut yok|
|jobs.queryJobs.failure|İş sorgularının başarısız oldu|Sayı|Toplam|Tüm başarısız çağrılar sorgu iş sayısı.|Boyut yok|
|Jobs.Completed|Tamamlanan işler|Sayı|Toplam|Tüm tamamlanmış işlerin sayısı.|Boyut yok|
|Jobs.Failed|Başarısız olan işler|Sayı|Toplam|Başarısız olan tüm işleri sayısı.|Boyut yok|
|d2c.telemetry.ingress.sendThrottle|Azaltma hatalarının sayısındaki|Sayı|Toplam|Cihaz işleme nedeniyle azaltma hatalarının sayısındaki kısıtlar|Boyut yok|
|dailyMessageQuotaUsed|Kullanılan iletilerinin toplam sayısı|Sayı|Ortalama|Günümüzde kullanılan toplam ileti sayısı. Bu, sıfırlanır, toplu bir değerdir, her gün 00:00 UTC.|Boyut yok|
|deviceDataUsage|Toplam devicedata kullanım (kullanım dışı)|Bayt|Toplam|Iothub için bağlı tüm cihazlara giden ve gelen aktarılan bayt|Boyut yok|
|deviceDataUsageV2|Toplam cihaz veri kullanımı (Önizleme)|Bayt|Toplam|Iothub için bağlı tüm cihazlara giden ve gelen aktarılan bayt|Boyut yok|
|totalDeviceCount|Toplam cihaz sayısı (Önizleme)|Sayı|Ortalama|IOT hub'ınıza kayıtlı cihaz sayısı|Boyut yok|
|connectedDeviceCount|Bağlı cihazlar (Önizleme)|Sayı|Ortalama|IOT hub'ınıza bağlı cihazların sayısı|Boyut yok|
|Yapılandırmaları|Ölçümleri yapılandırma|Sayı|Toplam|Yapılandırma işlemleri için ölçümleri|Boyut yok|

## <a name="microsoftdevicesprovisioningservices"></a>Microsoft.Devices/provisioningServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|RegistrationAttempts|Kayıt denemesi|Sayı|Toplam|Cihaz kayıtları denemesi sayısı|ProvisioningServiceName, IotHubName, Status|
|DeviceAssignments|Atanan cihazlar|Sayı|Toplam|Bir IOT hub'ına atanan cihaz sayısı|ProvisioningServiceName, IotHubName|
|AttestationAttempts|Kanıtlama denemeleri|Sayı|Toplam|Cihaz karşıladığımızı denemesi sayısı|ProvisioningServiceName, durumu, Protokolü|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.DocumentDB/databaseAccounts

### <a name="request-metrics"></a>İstek ölçümleri

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar| Zaman ayrıntı düzeyi| Eski ölçüm eşleme | Kullanım |
|---|---|---|---|---|---| ---| ---| ---|
| TotalRequests |   Toplam İstek Sayısı| Sayı   | Sayı | Yapılan istek sayısı|  DatabaseName, CollectionName, bölge, StatusCode|   Tümü |   Hizmet kullanılamıyor, istekler daraltıldı, saniye başına ortalama istek Http 2xx, 3xx, Http 400, Http 401 Http TotalRequests, iç sunucu hatası |    Durum kodu, bir dakikalık ayrıntı düzeyi, koleksiyon başına istekleri izlemek için kullanılır. Saniye başına ortalama istek almak için dakika sayısı toplama kullanın ve 60 bölün. |
| MetadataRequests |    Meta veri isteği   |Sayı| Sayı   | Meta veri isteği sayısı. Azure Cosmos DB koleksiyonları, veritabanları vb. listeleme olanak tanır, her hesap için sistem meta veri koleksiyonu tutar ve bunların yapılandırmalarının ücretsiz.    | DatabaseName, CollectionName, bölge, StatusCode| Tümü|  |Meta veri isteği nedeniyle kısıtlamalar izlemek için kullanılır.|
| MongoRequests |   Mongo istekleri| Sayı | Sayı|  Mongo istekleri sayısı   | DatabaseName CollectionName, bölgesi CommandName, hata kodu| Tümü |Mongo sorgusu istek hızı, Mongo güncelleştirme isteği oranı oranı, Mongo silmek, Mongo istek hızı Ekle istek Mongo sayısı istek oranı|   Mongo istek hataları izlemek için kullanılan komut başına kullanımları yazın. |

### <a name="request-unit-metrics"></a>İstek birimi ölçümleri

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar| Zaman ayrıntı düzeyi| Eski ölçüm eşleme | Kullanım |
|---|---|---|---|---|---| ---| ---| ---|
| MongoRequestCharge|   Mongo istek yükü |  Sayı   |Toplam  |Tüketilen mongo istek birimleri|  DatabaseName CollectionName, bölgesi CommandName, hata kodu|   Tümü |Mongo sorgusu istek yükü, Mongo güncelleştirme ücreti, Mongo istek yükü silin, istek yükü, Mongo Ekle Mongo sayısı isteği ücret iste| Bir dakika içinde Mongo kaynak RU'ları izlemek için kullanılır.|
| TotalRequestUnits |Toplam istek birimleri|   Sayı|  Toplam|  Tüketilen birimler iste| DatabaseName, CollectionName, bölge, StatusCode    |Tümü|   TotalRequestUnits|  Dakikalık bir ayrıntı düzeyi toplam RU kullanımını izlemek için kullanılır. Tüketilen ortalama RU almak için toplam toplama dakikada kullanın ve 60 bölün.|
| ProvisionedThroughput |Sağlanan Aktarım Hızı|    Sayı|  Maksimum |Koleksiyon ayrıntı düzeyinde sağlanan aktarım hızı|  DatabaseName, CollectionName|   5 DK| |   Koleksiyon başına sağlanan aktarım hızı izlemek için kullanılır.|

### <a name="storage-metrics"></a>Depolama ölçümleri

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar| Zaman ayrıntı düzeyi| Eski ölçüm eşleme | Kullanım |
|---|---|---|---|---|---| ---| ---| ---|
| AvailableStorage| Kullanılabilir depolama alanı   |Bayt| Toplam|  Bölge başına 5 dakikalık ayrıntı düzeyi, raporlanan toplam kullanılabilir depolama alanı|   DatabaseName, CollectionName, bölge|   5 DK| Kullanılabilir depolama alanı|   Kullanılabilir depolama alanı izlemek için kullanılan kapasite (yalnızca sabit depolama koleksiyonlar için geçerlidir) en az ayrıntı düzeyi 5 dakika olmalıdır.|
| DataUsage |Veri Kullanımı |Bayt| Toplam   |Bölge başına 5 dakikalık ayrıntı düzeyi, rapor edilen toplam veri kullanımı|    DatabaseName, CollectionName, bölge|   5 DK  |Veri boyutu  | En düşük ayrıntı düzeyi, koleksiyon ve bölge toplam veri kullanımı izlemek için kullanılan, 5 dakika olmalıdır.|
| IndexUsage|   Dizin kullanımı|    Bayt|  Toplam   |Bölge başına 5 dakikalık ayrıntı düzeyi, toplam dizin kullanım raporlama|    DatabaseName, CollectionName, bölge|   5 DK| Dizin Boyutu| En düşük ayrıntı düzeyi, koleksiyon ve bölge toplam veri kullanımı izlemek için kullanılan, 5 dakika olmalıdır. |
| DocumentQuota|    Belge kota| Bayt|  Toplam|  Bölge başına 5 dakikalık ayrıntı düzeyi, toplam depolama kotası bildirdi. Sabit depolama koleksiyonlar için geçerlidir| DatabaseName, CollectionName, bölge|   5 DK  |Depolama Kapasitesi|  En düşük ayrıntı düzeyi, koleksiyon ve bölge toplam kota izlemek için kullanılan, 5 dakika olmalıdır.|
| DocumentCount|    Belge Sayısı| Sayı   |Toplam  |Bölge başına 5 dakikalık ayrıntı düzeyi, bildirilen Totaldocument sayısı|  DatabaseName, CollectionName, bölge|   5 DK  |Belge Sayısı|En düşük ayrıntı düzeyi, koleksiyon ve bölge belge sayısı izlemek için kullanılan, 5 dakika olmalıdır.|

### <a name="latency-metrics"></a>Gecikme süresi ölçülerini

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar| Zaman ayrıntı düzeyi| Kullanım |
|---|---|---|---|---|---| ---| ---|
| ReplicationLatency    | Çoğaltma gecikmesi|  Milisaniye|   Minimum, maksimum, ortalama | Kaynak ve hedef bölgede coğrafi özellikli hesabının P99 çoğaltma gecikmesi| SourceRegion, TargetRegion| Tümü | Coğrafi olarak çoğaltılmış bir hesap için herhangi iki bölgeleri arasında P99 çoğaltma gecikmesi izlemek için kullanılır. |

### <a name="availability-metrics"></a>Kullanılabilirlik ölçümlerini

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar| Zaman ayrıntı düzeyi| Eski ölçüm eşleme | Kullanım |
|---|---|---|---|---|---| ---| ---| ---|
| ServiceAvailability   | Hizmet kullanılabilirliği| Yüzde |Minimum, maksimum|   Hesap istekleri kullanılabilirliğine bir saat ayrıntı düzeyi|  |   1H  | Hizmet kullanılabilirliği  | Toplam başarılı isteklerin yüzdesi budur. Bir isteği 410, durum kodu ise, sistem hatası nedeniyle başarısız olarak kabul edilir 500 veya 503 saat ayrıntı düzeyi hesabında kullanılabilirliğini izlemek için kullanılır. |

### <a name="cassandra-api-metrics"></a>Cassandra API ölçümleri

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar| Zaman ayrıntı düzeyi| Kullanım |
|---|---|---|---|---|---| ---| ---|
| CassandraRequests | Cassandra istekleri |  Sayı|  Sayı|  Yapılan Cassandra API isteklerinin sayısı|  DatabaseName, CollectionName, hata kodu, bölge, OperationType, kaynak türü|   Tümü| Dakikalık bir ayrıntı düzeyi Cassandra istekleri izlemek için kullanılır. Saniye başına ortalama istek almak için dakika sayısı toplama kullanın ve 60 bölün.|
| CassandraRequestCharges|  Cassandra isteği ücretleri| Sayı|   Topla, Min, Max, Avg| İstek birimleri Cassandra API istekleri tarafından kullanılan|   DatabaseName, CollectionName, bölge, OperationType, kaynak türü|  Tümü| Dakika başına Cassandra API hesabı tarafından kullanılan RU'ları izlemek için kullanılır.|
| CassandraConnectionClosures   | Cassandra bağlantı kapanışlar |Sayı| Sayı   |Cassandra kapatılan bağlantılar sayısı|    ClosureReason, bölge|  Tümü | İstemcileri ve Azure Cosmos DB Cassandra API'SİNİN arasındaki bağlantıyı izlemek için kullanılır.|

## <a name="microsofteventgridtopics"></a>Microsoft.EventGrid/topics

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|PublishSuccessCount|Yayımlanan olaylar|Sayı|Toplam|Bu konu başlığında yayımlanan toplam olay sayısı|Boyut yok|
|PublishFailCount|Başarısız olaylar|Sayı|Toplam|Bu konuya yayımlanamadı toplam olay sayısı|ErrorType, hata|
|UnmatchedEventCount|Eşleşmeyen olay|Sayı|Toplam|Bu konu için olay aboneliklerden herhangi birine eşleşmeyen toplam olay sayısı|Boyut yok|

## <a name="microsofteventgrideventsubscriptions"></a>Microsoft.EventGrid/eventSubscriptions

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|MatchedEventCount|Eşleşen Olaylar|Sayı|Toplam|Bu olay aboneliğine eşleşen toplam olay sayısı|Boyut yok|
|DeliveryAttemptFailCount|Olayları teslim başarısız|Sayı|Toplam|Toplam olay sayısı olay aboneliği için teslim edilemedi|Hata, ErrorType|
|DeliverySuccessCount|Teslim edilen olaylar|Sayı|Toplam|Bu olay aboneliğine teslim toplam olay sayısı|Boyut yok|
|DestinationProcessingDurationInMs|Hedef işleme süresi|Milisaniye|Ortalama|Hedef işleme süresinin milisaniye cinsinden|Boyut yok|
|DroppedEventCount|Bırakılan olayları|Sayı|Toplam|Bu olay aboneliğine eşleşen toplam bırakılan olayları|Boyut yok|
|DeadLetteredCount|Geçerliliğini yitirmiş olayları|Sayı|Toplam|Bu olay aboneliğine eşleşen toplam geçerliliğini yitirmiş olayları|DeadLetterReason|

## <a name="microsofteventgridextensiontopics"></a>Microsoft.EventGrid/extensionTopics

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|PublishSuccessCount|Yayımlanan olaylar|Sayı|Toplam|Bu konu başlığında yayımlanan toplam olay sayısı|Boyut yok|
|PublishFailCount|Başarısız olaylar|Sayı|Toplam|Bu konuya yayımlanamadı toplam olay sayısı|ErrorType, hata|
|UnmatchedEventCount|Eşleşmeyen olay|Sayı|Toplam|Bu konu için olay aboneliklerden herhangi birine eşleşmeyen toplam olay sayısı|Boyut yok|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SuccessfulRequests|Başarılı istekler (Önizleme)|Sayı|Toplam|Microsoft.EventHub için Başarılı İstekler. (Önizleme)|EntityName|
|ServerErrors|Sunucu Hataları. (Önizleme)|Sayı|Toplam|Microsoft.EventHub için Sunucu Hataları. (Önizleme)|EntityName|
|UserErrors|Kullanıcı Hataları. (Önizleme)|Sayı|Toplam|Microsoft.EventHub için Kullanıcı Hataları. (Önizleme)|EntityName|
|QuotaExceededErrors|Kota Aşım Hataları. (Önizleme)|Sayı|Toplam|Microsoft.EventHub için Kota Aşım Hataları. (Önizleme)|EntityName|
|ThrottledRequests|Daraltılmış İstekler. (Önizleme)|Sayı|Toplam|Microsoft.EventHub için Daraltılmış İstekler. (Önizleme)|EntityName|
|IncomingRequests|Gelen istekler (Önizleme)|Sayı|Toplam|Microsoft.EventHub Gelen İstekler. (Önizleme)|EntityName|
|IncomingMessages|Gelen iletiler (Önizleme)|Sayı|Toplam|Microsoft.EventHub için Gelen iletiler. (Önizleme)|EntityName|
|OutgoingMessages|Giden iletiler (Önizleme)|Sayı|Toplam|Microsoft.EventHub Giden İletiler. (Önizleme)|EntityName|
|IncomingBytes|Gelen Baytlar. (Önizleme)|Bayt|Toplam|Microsoft.EventHub için Gelen Baytlar. (Önizleme)|EntityName|
|OutgoingBytes|Giden Baytlar. (Önizleme)|Bayt|Toplam|Microsoft.EventHub için Giden Baytlar. (Önizleme)|EntityName|
|ActiveConnections|ActiveConnections (Önizleme)|Sayı|Ortalama|Microsoft.EventHub için Toplam Etkin Bağlantı. (Önizleme)|Boyut yok|
|ConnectionsOpened|Açılan Bağlantılar. (Önizleme)|Sayı|Ortalama|Microsoft.EventHub için Açılan Bağlantılar. (Önizleme)|EntityName|
|ConnectionsClosed|Kapatılan Bağlantılar. (Önizleme)|Sayı|Ortalama|Microsoft.EventHub için Kapatılan Bağlantılar. (Önizleme)|EntityName|
|CaptureBacklog|Kapsam yakalayın. (Önizleme)|Sayı|Toplam|Microsoft.EventHub için kapsam yakalayın. (Önizleme)|EntityName|
|CapturedMessages|Yakalanan İletiler. (Önizleme)|Sayı|Toplam|Microsoft.EventHub için Yakalanan İletiler. (Önizleme)|EntityName|
|CapturedBytes|Yakalanan Baytlar. (Önizleme)|Bayt|Toplam|Microsoft.EventHub için Yakalanan Baytlar. (Önizleme)|EntityName|
|Boyut|Boyut (Önizleme)|Bayt|Ortalama|EventHub'ın Bayt cinsinden boyutu. (Önizleme)|EntityName|
|INREQS|Gelen İstekler|Sayı|Toplam|Bir ad alanı için gelen toplam gönderme isteği|Boyut yok|
|SUCCREQ|Başarılı İstekler|Sayı|Toplam|Bir ad alanı için toplam başarılı istek sayısı|Boyut yok|
|FAILREQ|Başarısız İstekler|Sayı|Toplam|Bir ad alanı için toplam başarısız istek sayısı|Boyut yok|
|SVRBSY|Sunucu Meşgul Hataları|Sayı|Toplam|Bir ad alanı için sunucu meşgul hatalarının toplam sayısı|Boyut yok|
|INTERR|İç Sunucu Hataları|Sayı|Toplam|Bir ad alanı için toplam iç sunucu hatası sayısı|Boyut yok|
|MISCERR|Diğer Hatalar|Sayı|Toplam|Bir ad alanı için toplam başarısız istek sayısı|Boyut yok|
|INMSGS|Gelen İletiler (Kullanım dışı)|Sayı|Toplam|Bir ad alanı için toplam gelen iletiler. Bu ölçüm kullanımdan kaldırılmıştır. Lütfen bunun yerine gelen iletiler ölçümünü kullanın|Boyut yok|
|EHINMSGS|Gelen İletiler|Sayı|Toplam|Bir ad alanı için toplam gelen ileti sayısı|Boyut yok|
|OUTMSGS|Giden İletiler (Kullanım dışı)|Sayı|Toplam|Toplam giden iletileri için bir ad alanı. Bu ölçüm kullanımdan kaldırılmıştır. Lütfen bunun yerine giden iletiler ölçümünü kullanın|Boyut yok|
|EHOUTMSGS|Giden İletiler|Sayı|Toplam|Bir ad alanı için toplam giden ileti sayısı|Boyut yok|
|EHINMBS|Gelen bayt (Kullanım dışı)|Bayt|Toplam|Olay hub'ı gelen ileti aktarım hızı için bir ad alanı. Bu ölçüm kullanımdan kaldırılmıştır. Lütfen bunun yerine gelen bayt ölçümünü kullanın|Boyut yok|
|EHINBYTES|Gelen bayt|Bayt|Toplam|Bir ad alanı için Olay Hub'ı gelen ileti işleme hızı|Boyut yok|
|EHOUTMBS|Giden bayt (Kullanım dışı)|Bayt|Toplam|Olay hub'ı giden ileti aktarım hızı için bir ad alanı. Bu ölçüm kullanımdan kaldırılmıştır. Lütfen bunun yerine Giden bayt ölçümünü kullanın|Boyut yok|
|EHOUTBYTES|Giden bayt|Bayt|Toplam|Bir ad alanı için Olay Hub'ı giden ileti işleme hızı|Boyut yok|
|EHABL|Kapsam mesajlarını arşivle|Sayı|Toplam|Bir ad alanı için kapsamda arşivlenen Olay Hub'ı iletileri|Boyut yok|
|EHAMSGS|Mesajları arşivleme|Sayı|Toplam|Bir ad alanında Olay Hub'ı tarafından arşivlenen iletiler|Boyut yok|
|EHAMBS|Mesaj işlemelerini arşivle|Bayt|Toplam|Bir ad alanında Olay Hub'ı tarafından arşivlenen ileti işleme hızı|Boyut yok|

## <a name="microsofteventhubclusters"></a>Microsoft.EventHub/clusters

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SuccessfulRequests|Başarılı istekler (Önizleme)|Sayı|Toplam|Microsoft.EventHub için Başarılı İstekler. (Önizleme)|Boyut yok|
|ServerErrors|Sunucu Hataları. (Önizleme)|Sayı|Toplam|Microsoft.EventHub için Sunucu Hataları. (Önizleme)|Boyut yok|
|UserErrors|Kullanıcı Hataları. (Önizleme)|Sayı|Toplam|Microsoft.EventHub için Kullanıcı Hataları. (Önizleme)|Boyut yok|
|QuotaExceededErrors|Kota Aşım Hataları. (Önizleme)|Sayı|Toplam|Microsoft.EventHub için Kota Aşım Hataları. (Önizleme)|Boyut yok|
|ThrottledRequests|Daraltılmış İstekler. (Önizleme)|Sayı|Toplam|Microsoft.EventHub için Daraltılmış İstekler. (Önizleme)|Boyut yok|
|IncomingRequests|Gelen istekler (Önizleme)|Sayı|Toplam|Microsoft.EventHub Gelen İstekler. (Önizleme)|Boyut yok|
|IncomingMessages|Gelen iletiler (Önizleme)|Sayı|Toplam|Microsoft.EventHub için Gelen iletiler. (Önizleme)|Boyut yok|
|OutgoingMessages|Giden iletiler (Önizleme)|Sayı|Toplam|Microsoft.EventHub Giden İletiler. (Önizleme)|Boyut yok|
|IncomingBytes|Gelen Baytlar. (Önizleme)|Bayt|Toplam|Microsoft.EventHub için Gelen Baytlar. (Önizleme)|Boyut yok|
|OutgoingBytes|Giden Baytlar. (Önizleme)|Bayt|Toplam|Microsoft.EventHub için Giden Baytlar. (Önizleme)|Boyut yok|
|ActiveConnections|ActiveConnections (Önizleme)|Sayı|Ortalama|Microsoft.EventHub için Toplam Etkin Bağlantı. (Önizleme)|Boyut yok|
|ConnectionsOpened|Açılan Bağlantılar. (Önizleme)|Sayı|Ortalama|Microsoft.EventHub için Açılan Bağlantılar. (Önizleme)|Boyut yok|
|ConnectionsClosed|Kapatılan Bağlantılar. (Önizleme)|Sayı|Ortalama|Microsoft.EventHub için Kapatılan Bağlantılar. (Önizleme)|Boyut yok|
|CaptureBacklog|Kapsam yakalayın. (Önizleme)|Sayı|Toplam|Microsoft.EventHub için kapsam yakalayın. (Önizleme)|Boyut yok|
|CapturedMessages|Yakalanan İletiler. (Önizleme)|Sayı|Toplam|Microsoft.EventHub için Yakalanan İletiler. (Önizleme)|Boyut yok|
|CapturedBytes|Yakalanan Baytlar. (Önizleme)|Bayt|Toplam|Microsoft.EventHub için Yakalanan Baytlar. (Önizleme)|Boyut yok|
|CPU|CPU (Önizleme)|Yüzde|Maksimum|Yüzde olarak Olay Hub'ı Kümesinin CPU kullanımı|Rol|
|AvailableMemory|Kullanılabilir bellek (Önizleme)|Sayı|Maksimum|Bayt cinsinden Olay Hub'ı Kümesi için kullanılabilir bellek|Rol|

## <a name="microsofthdinsightclusters"></a>Microsoft.HDInsight/clusters

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|GatewayRequests|Ağ geçidi istekleri|Sayı|Toplam|Ağ geçidi istekleri sayısı|ClusterDnsName, HttpStatus|
|CategorizedGatewayRequests|Kategorilere ayrılmış bir ağ geçidi istekleri|Sayı|Toplam|Ağ geçidi istekleri (1xx/2xx/3xx/4xx/5xx) kategorilere göre sayısı|ClusterDnsName, HttpStatus|

## <a name="microsoftinsightsautoscalesettings"></a>Microsoft.Insights/AutoscaleSettings

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ObservedMetricValue|Gözlenen Ölçüm Değeri|Sayı|Ortalama|Otomatik ölçeklendirme yürütüldüğünde hesaplanan değer|MetricTriggerSource|
|MetricThreshold|Ölçüm Eşiği|Sayı|Ortalama|Otomatik ölçeklendirme çalıştırıldığında yapılandırılan otomatik ölçek eşiği.|MetricTriggerRule|
|ObservedCapacity|Gözlenen Kapasite|Sayı|Ortalama|Otomatik ölçeklendirme yürütüldüğünde bildirilen kapasite.|Boyut yok|
|ScaleActionsInitiated|Başlatılan Ölçeklendirme Eylemleri|Sayı|Toplam|Ölçeklendirme işleminin yönü.|ScaleDirection|

## <a name="microsoftinsightscomponents"></a>Microsoft.Insights/Components

(Genel Önizleme)

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|availabilityResults/süresi|Test süresi|Milisaniye|Ortalama|Test süresi|availabilityResult/ad, availabilityResult/konum, availabilityResult/başarılı|
|billingMeters/telemetryCount|Veri noktası sayısı|Sayı|Toplam|Bu Application Insights kaynağına gönderilen veri noktası sayısı. Bu ölçüm, en fazla iki saat gecikmeyle işlenir.|Faturalama/telemetryItemType, faturalandırma/telemetryItemSource|
|billingMeters/telemetrySize|Veri noktası hacmi|Bayt|Toplam|Bu Application Insights kaynağına gönderilen veri hacmi. Bu ölçüm, en fazla iki saat gecikmeyle işlenir.|Faturalama/telemetryItemType, faturalandırma/telemetryItemSource|
|browserTimings/networkDuration|Sayfa yükleme ağ bağlantı süresi|Milisaniye|Ortalama|Kullanıcı isteği ve ağ bağlantı arasındaki süre. DNS aramasını ve aktarım bağlantısını içerir.|Boyut yok|
|browserTimings/processingDuration|İstemci işlem süresi|Milisaniye|Ortalama|DOM'un yüklenmesi arasında bir belgenin son bayt alma süresi. Zaman uyumsuz istekler hala işleniyor.|Boyut yok|
|browserTimings/receiveDuration|Yanıt süresi alınıyor|Milisaniye|Ortalama|İlk ve son bayt veya bağlantının kesilmesi arasındaki süre.|Boyut yok|
|browserTimings/sendDuration|İstek gönderme süresi|Milisaniye|Ortalama|Ağ bağlantısı ve ilk baytın alınması arasında geçen süre.|Boyut yok|
|browserTimings/totalDuration|Tarayıcı sayfa yükleme süresi|Milisaniye|Ortalama|Kullanıcı isteğinden DOM, stil sayfaları, betikler ve resimler yüklenene kadar geçen süre.|Boyut yok|
|bağımlılıkları/sayısı|Bağımlılık çağrıları|Sayı|Sayı|Uygulama tarafından dış kaynaklara yapılan çağrıların sayısı.|Bağımlılık/türü, bağımlılık/performanceBucket, bağımlılık/başarı, işlemi/Sentetik, bulut/Roleınstance, bulut/roleName|
|bağımlılıkları/süresi|Bağımlılık süresi|Milisaniye|Ortalama|Uygulama tarafından dış kaynaklara yapılan çağrıların süresi.|Bağımlılık/türü, bağımlılık/performanceBucket, bağımlılık/başarı, işlemi/Sentetik, bulut/Roleınstance, bulut/roleName|
|bağımlılıkları/başarısız oldu|Bağımlılık hataları|Sayı|Sayı|Uygulama tarafından dış kaynaklara yapılan başarısız bağımlılık çağrılarının sayısı.|Bağımlılık/türü, bağımlılık/performanceBucket, işlemi/Sentetik, bulut/Roleınstance, bulut/roleName|
|Sayfa görüntülemesi/sayısı|Sayfa görünümleri|Sayı|Sayı|Sayfa görüntüleme sayısı.|işlem/Sentetik|
|Sayfa görüntülemesi/süresi|Sayfa görüntüleme yükleme süresi|Milisaniye|Ortalama|Sayfa görüntüleme yükleme süresi|işlem/Sentetik|
|performanceCounters/requestExecutionTime|HTTP isteği yürütme süresi|Milisaniye|Ortalama|En son isteğin yürütülme süresi.|Bulut/Roleınstance|
|performanceCounters/requestsInQueue|Uygulama kuyruğundaki HTTP istekleri|Sayı|Ortalama|Uygulama istek kuyruğunun uzunluğu.|Bulut/Roleınstance|
|performanceCounters/requestsPerSecond|HTTP isteği oranı|CountPerSecond|Ortalama|Saniyede ASP.NET'ten uygulamaya yapılan tüm isteklerin oranı.|Bulut/Roleınstance|
|performanceCounters/exceptionsPerSecond|Özel durum oranı|CountPerSecond|Ortalama|Windows'a raporlanan, işlenen ve yakalanamayan özel durumların (.NET özel durumları ve .NET özel durumlarına dönüştürülmüş yönetilmeyen özel durumlar gibi) sayısı.|Bulut/Roleınstance|
|performanceCounters/processIOBytesPerSecond|İşlem GÇ hızı|BytesPerSecond|Ortalama|Dosyalar, ağ ve cihazlar üzerinde saniyede okunan ve yazılan toplam bayt.|Bulut/Roleınstance|
|performanceCounters/processCpuPercentage|İşlem CPU'su|Yüzde|Ortalama|Tüm işlem iş parçacıklarının sürenin yüzdesini işlemci yönergeleri yürütmek için kullanılır. Bu, 0 ile 100 arasında değişebilir. Bu ölçüm yalnızca w3wp işleminin performansını gösterir.|Bulut/Roleınstance|
|performanceCounters/processorCpuPercentage|İşlemci zamanı|Yüzde|Ortalama|İşlemcinin boş olmayan iş parçacıklarında harcadığı sürenin yüzdesi.|Bulut/Roleınstance|
|performanceCounters/memoryAvailableBytes|Uygun bellek|Bayt|Ortalama|Bir işleme veya sistem kullanımına ayırmak için hemen kullanılabilir fiziksel bellek.|Bulut/Roleınstance|
|performanceCounters/processPrivateBytes|İşleme özel bayt sayısı|Bayt|Ortalama|İzlenen uygulama işlemleri için özel olarak atanan bellek.|Bulut/Roleınstance|
|istekleri/süresi|Sunucu yanıt süresi|Milisaniye|Ortalama|Bir HTTP isteğinin alınmasıyla yanıtın gönderilmesi arasında geçen süre.|İstek/performanceBucket, istek/resultCode, işlemi/Sentetik, bulut/Roleınstance, istek/başarı, bulut/roleName|
|isteği/sayısı|Sunucu istekleri|Sayı|Sayı|Tamamlanan HTTP isteği sayısı.|İstek/performanceBucket, istek/resultCode, işlemi/Sentetik, bulut/Roleınstance, istek/başarı, bulut/roleName|
|/ başarısız istekleri|Başarısız istekler|Sayı|Sayı|HTTP isteği sayısı, başarısız olarak işaretlemiş. Çoğu durumda bunlar isteği bir yanıt koduna sahip > 400 veya 401 eşit değildir.|İstek/performanceBucket, istek/resultCode, işlemi/Sentetik, bulut/Roleınstance, bulut/roleName|
|özel durumlar/sayısı|Özel durumlar|Sayı|Toplam|Tüm yakalanmayan özel durumların birleştirilmiş sayısı.|Bulut/roleName, bulut/Roleınstance, istemci/türü|
|özel durumlar/tarayıcı|Tarayıcı özel durumları|Sayı|Toplam|Tarayıcıda oluşan yakalanmayan özel durum sayısı.|Boyut yok|
|özel durumlar/sunucu|Sunucu özel durumları|Sayı|Toplam|Sunucu uygulamasında oluşan yakalanmayan özel durum sayısı.|Bulut/roleName, bulut/Roleınstance|
|izlemeleri/sayısı|İzleme sayısı|Sayı|Toplam|İzleme belgesi sayısı|İzleme/Err, işlemi/Sentetik, bulut/roleName, bulut/Roleınstance|

## <a name="microsoftkeyvaultvaults"></a>Microsoft.KeyVault/vaults

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ServiceApiHit|Hizmet API'si Toplam İsabet|Sayı|Sayı|Hizmet API'si isabetlerinin toplam sayısı|ActivityType, ActivityName|
|ServiceApiLatency|Hizmet API'si Toplam Gecikme|Milisaniye|Ortalama|Hizmet API'si isteklerinin toplam gecikme süresi|ActivityType, ActivityName, StatusCode|
|ServiceApiResult|Hizmet API'si Toplam Sonuç|Sayı|Sayı|Hizmet API'si sonuçlarının toplam sayısı|ActivityType, ActivityName, StatusCode|

## <a name="microsoftkustoclusters"></a>Microsoft.Kusto/Clusters

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ClusterDataCapacityFactor|Önbellek kullanımı|Yüzde|Ortalama|Küme kapsamında kullanım düzeyi|Boyut yok|
|QueryDuration|Sorgu süresi|Milisaniye|Ortalama|Saniye cinsinden sorgu süresi|QueryStatus|
|IngestionsLoadFactor|Alımı kullanımı|Yüzde|Ortalama|Kümede kullanılan alma yuvası oranı|Boyut yok|
|IsEngineAnsweringQuery|Canlı|Sayı|Ortalama|Küme sorgulara yanıt sağlamlık denetimi gösterir.|Boyut yok|
|IngestCommandOriginalSizeInMb|Alma birim (MB cinsinden)|Sayı|Toplam|Genel (MB cinsinden) kümeye içe alınan veri hacmi|Boyut yok|
|EventAgeSeconds|Alma gecikmesi (saniye cinsinden)|Saniye|Ortalama|Alma süresi (örneğin ileti içinde EventHub) kaynak kümeye saniye|Boyut yok|
|EventReceivedFromEventHub|(Event Hubs için) işlenen olaylar|Sayı|Toplam|Olay Hub'ından başlayan kümeniz, küme tarafından işlenen olay sayısı|Boyut yok|
|IngestionResult|Alma sonucu|Sayı|Sayı|Alma işlemi sayısı|IngestionResultDetails|
|EngineCPU|CPU|Yüzde|Ortalama|CPU kullanımı düzeyi|Boyut yok|

## <a name="microsoftlocationbasedservicesaccounts"></a>Microsoft.LocationBasedServices/accounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Kullanım|Kullanım|Sayı|Sayı|Sayısı, API çağrıları|ApiCategory, ApiName|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|RunsStarted|Başlatılan Çalıştırmalar|Sayı|Toplam|Başlatılan iş akışı çalıştırma sayısı.|Boyut yok|
|RunsCompleted|Tamamlanan Çalıştırmalar|Sayı|Toplam|Tamamlanan iş akışı çalıştırma sayısı.|Boyut yok|
|RunsSucceeded|Başarılı Çalıştırmalar|Sayı|Toplam|Başarılı iş akışı çalıştırma sayısı.|Boyut yok|
|RunsFailed|Başarısız Çalıştırmalar|Sayı|Toplam|Başarısız iş akışı çalıştırma sayısı.|Boyut yok|
|RunsCancelled|İptal Edilen Çalıştırmalar|Sayı|Toplam|İptal edilen iş akışı çalıştırma sayısı.|Boyut yok|
|RunLatency|Çalıştırma Gecikmesi|Saniye|Ortalama|Tamamlanan iş akışı çalıştırmalarının gecikme süresi.|Boyut yok|
|RunSuccessLatency|Başarılı Çalıştırma Gecikmesi|Saniye|Ortalama|Başarılı iş akışı çalıştırmalarının gecikme süresi.|Boyut yok|
|RunThrottledEvents|Çalıştırılması Kısıtlanan Olaylar|Sayı|Toplam|İş akışı eylemi veya tetikleyiciyle kısıtlanan olay sayısı.|Boyut yok|
|RunFailurePercentage|Çalıştırma Hatası Yüzdesi|Yüzde|Toplam|Başarısız olan iş akışı çalıştırmalarının yüzdesi.|Boyut yok|
|ActionsStarted|Başlatılan Eylemler |Sayı|Toplam|Başlatılan iş akışı eylemi sayısı.|Boyut yok|
|ActionsCompleted|Tamamlanan Eylemler |Sayı|Toplam|Tamamlanan iş akışı eylemi sayısı.|Boyut yok|
|ActionsSucceeded|Başarılı Eylemler |Sayı|Toplam|Başarılı iş akışı eylemi sayısı.|Boyut yok|
|ActionsFailed|Başarısız Eylemler|Sayı|Toplam|Başarısız iş akışı eylemi sayısı.|Boyut yok|
|ActionsSkipped|Atlanan Eylemler |Sayı|Toplam|Atlanan iş akışı eylemi sayısı.|Boyut yok|
|ActionLatency|Eylem Gecikmesi |Saniye|Ortalama|Tamamlanan iş akışı eylemlerinin gecikme süresi.|Boyut yok|
|ActionSuccessLatency|Başarılı Eylem Gecikmesi |Saniye|Ortalama|Başarılı iş akışı eylemlerinin gecikme süresi.|Boyut yok|
|ActionThrottledEvents|Eylemi Kısıtlanan Olaylar|Sayı|Toplam|İş akışı eylemi kısıtlanan olay sayısı.|Boyut yok|
|TriggersStarted|Başlatılan Tetikleyiciler |Sayı|Toplam|Başlatılan iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersCompleted|Tamamlanan Tetikleyiciler |Sayı|Toplam|Tamamlanan iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersSucceeded|Başarılı Olan Tetikleyiciler |Sayı|Toplam|Başarılı iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersFailed|Başarısız Olan Tetikleyiciler |Sayı|Toplam|Başarısız iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersSkipped|Atlanan Tetikleyiciler|Sayı|Toplam|Atlanan iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersFired|Tetiklenen Tetikleyiciler |Sayı|Toplam|Harekete geçirilen iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggerLatency|Tetikleyici Gecikme Süresi |Saniye|Ortalama|Tamamlanan iş akışı tetikleyicilerinin gecikmesi.|Boyut yok|
|TriggerFireLatency|Tetikleyici Tetikleme Gecikme Süresi |Saniye|Ortalama|Başlatılan iş akışı tetikleyicilerinin gecikmesi.|Boyut yok|
|TriggerSuccessLatency|Tetikleyici Başarı Gecikme Süresi |Saniye|Ortalama|Başarılı iş akışı tetikleyicilerinin gecikmesi.|Boyut yok|
|TriggerThrottledEvents|Tetikleyici Kısıtlanan Olaylar|Sayı|Toplam|İş akışı tetikleyicisiyle kısıtlanan olay sayısı.|Boyut yok|
|BillableActionExecutions|Faturalanabilir Eylem Yürütmeleri|Sayı|Toplam|Faturalandırılan iş akışı eylemi yürütmelerinin sayısı.|Boyut yok|
|BillableTriggerExecutions|Faturalanabilir Tetikleyici Yürütmeleri|Sayı|Toplam|Faturalandırılan iş akışı tetikleyicisi yürütmelerinin sayısı.|Boyut yok|
|TotalBillableExecutions|Toplam Faturalanabilir Yürütme Sayısı|Sayı|Toplam|Faturalandırılan iş akışı yürütmelerinin sayısı.|Boyut yok|
|BillingUsageNativeOperation|Yerel İşlem Yürütmelerinin Kullanımının Faturalanması|Sayı|Toplam|Faturalandırılan yerel işlem yürütmelerinin sayısı.|Boyut yok|
|BillingUsageStandardConnector|Standart Bağlayıcı Yürütmelerinin Kullanımının Faturalandırılması|Sayı|Toplam|Faturalandırılan standart bağlayıcı yürütmelerinin sayısı.|Boyut yok|
|BillingUsageStorageConsumption|Depolama Tüketimi Yürütmelerinin Kullanımının Faturalandırılması|Sayı|Toplam|Faturalandırılan depolama tüketimi yürütmelerinin sayısı.|Boyut yok|
|BillingUsageNativeOperation|Yerel İşlem Yürütmelerinin Kullanımının Faturalanması|Sayı|Toplam|Faturalandırılan yerel işlem yürütmelerinin sayısı.|Boyut yok|
|BillingUsageStandardConnector|Standart Bağlayıcı Yürütmelerinin Kullanımının Faturalandırılması|Sayı|Toplam|Faturalandırılan standart bağlayıcı yürütmelerinin sayısı.|Boyut yok|
|BillingUsageStorageConsumption|Depolama Tüketimi Yürütmelerinin Kullanımının Faturalandırılması|Sayı|Toplam|Faturalandırılan depolama tüketimi yürütmelerinin sayısı.|Boyut yok|

## <a name="microsoftnetappnetappaccountscapacitypoolsvolumes"></a>Microsoft.NetApp/netAppAccounts/capacityPools/Volumes

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|AverageOtherLatency|Ortalama diğer gecikme süresi|MS/işlem|Ortalama|(Yani, okuma veya yazma değil) diğer milisaniye cinsinden gecikme süresi işlemi başına ortalama|Boyut yok|
|AverageReadLatency|Ortalama okuma gecikme süresi|MS/işlem|Ortalama|Ortalama okuma işlemi başına milisaniye cinsinden gecikme süresi|Boyut yok|
|AverageTotalLatency|Ortalama toplam gecikme süresi|MS/işlem|Ortalama|İşlem başına milisaniye cinsinden ortalama toplam gecikme süresi|Boyut yok|
|AverageWriteLatency|Ortalama yazma gecikme süresi|MS/işlem|Ortalama|İşlem başına milisaniye cinsinden ortalama yazma gecikme süresi|Boyut yok|
|FilesystemOtherOps|Dosya sistemi diğer işlemler|OPS|Ortalama|Dosya sistemi sayısı (yani, okuma veya yazma değil) diğer işlemler|Boyut yok|
|FilesystemReadOps|Dosya sistemi işlemleri oku|OPS|Ortalama|Dosya sistemi sayısı okuma işlemleri|Boyut yok|
|FilesystemTotalOps|Toplam dosya sistemi işlemleri|OPS|Ortalama|Tüm dosya sistemi işlemleri toplamı|Boyut yok|
|FilesystemWriteOps|Dosya sistemi yazma işlemleri|OPS|Ortalama|Dosya sistemi yazma işlemlerinin sayısı|Boyut yok|
|IoBytesPerOtherOps|Diğer işlemler başına g/ç bayt|bayt/işlem|Ortalama|(Okuma veya yazma değil) numarasını In/out diğer işlem başına bayt|Boyut yok|
|IoBytesPerReadOps|GÇ bayt başına okuma işlemleri|bayt/işlem|Ortalama|İçeri/dışarı okuma işlemi başına bayt sayısı|Boyut yok|
|IoBytesPerTotalOps|Bütün işlemler op başına g/ç bayt|bayt/işlem|Ortalama|Tüm/giden bayt işlemi toplamı|Boyut yok|
|IoBytesPerWriteOps|Yazma işlemi başına g/ç bayt|bayt/işlem|Ortalama|Giriş/Çıkış yazma işlemi başına bayt sayısı|Boyut yok|
|OtherIops|Diğer IOPS|işlem/saniye|Ortalama|Diğer daraltma veya genişletme işlemi / saniye|Boyut yok|
|OtherThroughput|Diğer aktarım hızı|MB/sn|Ortalama|(Yani, okuma veya yazma değil) diğer aktarım hızını saniye başına megabayt cinsinden|Boyut yok|
|ReadIops|Okuma IOPS|işlem/saniye|Ortalama|Daraltma/genişletme işlemleri saniye başına okuma|Boyut yok|
|ReadThroughput|Okuma aktarım hızı|MB/sn|Ortalama|Saniye başına megabayt cinsinden okuma aktarım hızı|Boyut yok|
|TotalIops|Toplam IOPS|işlem/saniye|Ortalama|Tüm giriş/çıkış işlemleri saniye başına toplam|Boyut yok|
|TotalThroughput|Toplam işleme|MB/sn|Ortalama|Tüm aktarım hızını saniye başına megabayt cinsinden toplam|Boyut yok|
|VolumeAllocatedSize|Boyutu ayrılmış birim|bayt|Ortalama|Ayrılmış (değil gerçek bayt kullanılır) birimin boyutu|Boyut yok|
|VolumeLogicalSize|Mantıksal birim boyutu|bayt|Ortalama|Mantıksal birimin boyutunu (kullanılan bayt)|Boyut yok|
|VolumeSnapshotSize|Birim anlık görüntü boyutu|bayt|Ortalama|Birimdeki tüm anlık görüntülerin boyutu|Boyut yok|
|WriteIops|Yazma IOPS|işlem/saniye|Ortalama|Yazma/saniye başına işlem|Boyut yok|
|WriteThroughput|Aktarım hızı yazma|MB/sn|Ortalama|Aktarım hızı, saniye başına megabayt cinsinden yazma|Boyut yok|

## <a name="microsoftnetappnetappaccountscapacitypools"></a>Microsoft.NetApp/netAppAccounts/capacityPools

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|VolumePoolAllocatedSize|Birim ayrılmamış havuz boyutu|bayt|Ortalama|Ayrılmış (değil gerçek bayt kullanılır) havuz boyutu|Boyut yok|
|VolumePoolAllocatedUsed|Ayrılmış birim havuzu kullanılan|bayt|Ortalama|Havuzun ayrılan kullanılan boyut|Boyut yok|
|VolumePoolTotalLogicalSize|Birim havuzu toplam mantıksal boyutu|bayt|Ortalama|Mantıksal boyutu havuza ait birimlerin toplamı|Boyut yok|
|VolumePoolTotalSnapshotSize|Birim havuzu toplam anlık görüntü boyutu|bayt|Ortalama|Havuzdaki tüm anlık görüntü Topla|Boyut yok|

## <a name="microsoftnetworknetworkinterfaces"></a>Microsoft.Network/networkInterfaces

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BytesSentRate|Gönderilen bayt sayısı|Sayı|Toplam|Ağ arabirimi gönderilen bayt sayısı|Boyut yok|
|BytesReceivedRate|Alınan bayt sayısı|Sayı|Toplam|Ağ arabirimi alınan bayt sayısı|Boyut yok|
|PacketsSentRate|Gönderilen paketleri|Sayı|Toplam|Ağ arabirimi gönderilen paket sayısı|Boyut yok|
|PacketsReceivedRate|Alınan paket sayısı|Sayı|Toplam|Ağ arabirimi alınan paket sayısı|Boyut yok|

## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|VipAvailability|Veri yolu kullanılabilirliği|Sayı|Ortalama|Yük Dengeleyici veri yolu kullanılabilirlik zaman süresi başına ortalama|FrontendIPAddress, FrontendPort|
|DipAvailability|Araştırma durumu|Sayı|Ortalama|Zaman süresi başına ortalama yük dengeleyici sistem durumu araştırma durumu|ProtocolType Backendport'a, FrontendIPAddress FrontendPort, BackendIPAddress|
|ByteCount|Bayt sayısı|Sayı|Toplam|Toplam süre içinde aktarılan bayt sayısı|FrontendIPAddress, FrontendPort, yönü|
|PacketCount|Paket sayısı|Sayı|Toplam|Zaman aralığı içinde gönderilen paketlerin toplam sayısı|FrontendIPAddress, FrontendPort, yönü|
|SYNCount|SYN sayısı|Sayı|Toplam|SYN zaman aralığı içinde gönderilen paketlerin toplam sayısı|FrontendIPAddress, FrontendPort, yönü|
|SnatConnectionCount|SNAT bağlantısı sayısı|Sayı|Toplam|Toplam süre içinde oluşturulan yeni SNAT bağlantılarının sayısıdır|FrontendIPAddress, BackendIPAddress, ConnectionState|
|AllocatedSnatPorts|Ayrılmış SNAT bağlantı noktaları (Önizleme)|Sayı|Toplam|Toplam ayrılan süre içinde SNAT bağlantı noktalarının sayısı|FrontendIPAddress, BackendIPAddress, ProtocolType|
|UsedSnatPorts|Kullanılan SNAT bağlantı noktaları (Önizleme)|Sayı|Toplam|Toplam süre içinde kullanılan SNAT bağlantı noktalarının sayısı|FrontendIPAddress, BackendIPAddress, ProtocolType|

## <a name="microsoftnetworkdnszones"></a>Microsoft.Network/dnszones

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|QueryVolume|Sorgu birim|Sayı|Toplam|Bir DNS bölgesi için sunulan sorgu sayısı|Boyut yok|
|RecordSetCount|Kayıt kümesi sayısı|Sayı|Maksimum|Bir DNS bölgesinde kayıt kümesi sayısı|Boyut yok|
|RecordSetCapacityUtilization|Kayıt kümesi kapasite kullanımı|Yüzde|Maksimum|Bir DNS bölgesi tarafından kullanılan kayıt kümesi kapasite yüzdesi|Boyut yok|

## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAddresses

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|PacketsInDDoS|Gelen paket DDoS|CountPerSecond|Maksimum|Gelen paket DDoS|Boyut yok|
|PacketsDroppedDDoS|DDoS bırakılan gelen paketleri|CountPerSecond|Maksimum|DDoS bırakılan gelen paketleri|Boyut yok|
|PacketsForwardedDDoS|DDoS iletilen gelen paketler|CountPerSecond|Maksimum|DDoS iletilen gelen paketler|Boyut yok|
|TCPPacketsInDDoS|Gelen TCP paketleri DDoS|CountPerSecond|Maksimum|Gelen TCP paketleri DDoS|Boyut yok|
|TCPPacketsDroppedDDoS|DDoS bırakılan gelen TCP paketleri|CountPerSecond|Maksimum|DDoS bırakılan gelen TCP paketleri|Boyut yok|
|TCPPacketsForwardedDDoS|DDoS iletilen gelen TCP paket|CountPerSecond|Maksimum|DDoS iletilen gelen TCP paket|Boyut yok|
|UDPPacketsInDDoS|UDP paketlerini DDoS gelen|CountPerSecond|Maksimum|UDP paketlerini DDoS gelen|Boyut yok|
|UDPPacketsDroppedDDoS|DDoS bırakılan gelen UDP paketleri|CountPerSecond|Maksimum|DDoS bırakılan gelen UDP paketleri|Boyut yok|
|UDPPacketsForwardedDDoS|DDoS iletilen gelen UDP paketlerini|CountPerSecond|Maksimum|DDoS iletilen gelen UDP paketlerini|Boyut yok|
|BytesInDDoS|Gelen bayt DDoS|BytesPerSecond|Maksimum|Gelen bayt DDoS|Boyut yok|
|BytesDroppedDDoS|Gelen bayt DDoS bırakıldı|BytesPerSecond|Maksimum|Gelen bayt DDoS bırakıldı|Boyut yok|
|BytesForwardedDDoS|DDoS iletilen gelen bayt|BytesPerSecond|Maksimum|DDoS iletilen gelen bayt|Boyut yok|
|TCPBytesInDDoS|Gelen TCP bayt DDoS|BytesPerSecond|Maksimum|Gelen TCP bayt DDoS|Boyut yok|
|TCPBytesDroppedDDoS|DDoS gelen TCP bayt bırakıldı|BytesPerSecond|Maksimum|DDoS gelen TCP bayt bırakıldı|Boyut yok|
|TCPBytesForwardedDDoS|DDoS iletilen gelen TCP bayt|BytesPerSecond|Maksimum|DDoS iletilen gelen TCP bayt|Boyut yok|
|UDPBytesInDDoS|UDP bayt DDoS gelen|BytesPerSecond|Maksimum|UDP bayt DDoS gelen|Boyut yok|
|UDPBytesDroppedDDoS|DDoS gelen UDP bayt bırakıldı|BytesPerSecond|Maksimum|DDoS gelen UDP bayt bırakıldı|Boyut yok|
|UDPBytesForwardedDDoS|DDoS iletilen gelen UDP bayt|BytesPerSecond|Maksimum|DDoS iletilen gelen UDP bayt|Boyut yok|
|IfUnderDDoSAttack|Altında DDoS saldırı veya değil|Sayı|Maksimum|Altında DDoS saldırı veya değil|Boyut yok|
|DDoSTriggerTCPPackets|DDoS saldırılarının tetiklemek için gelen TCP paket|CountPerSecond|Maksimum|DDoS saldırılarının tetiklemek için gelen TCP paket|Boyut yok|
|DDoSTriggerUDPPackets|DDoS saldırılarının tetiklemek için gelen UDP paketlerini|CountPerSecond|Maksimum|DDoS saldırılarının tetiklemek için gelen UDP paketlerini|Boyut yok|
|DDoSTriggerSYNPackets|DDoS saldırılarının tetiklemek için gelen SYN paketleri|CountPerSecond|Maksimum|DDoS saldırılarının tetiklemek için gelen SYN paketleri|Boyut yok|
|VipAvailability|Veri yolu kullanılabilirliği|Sayı|Ortalama|IP adresi kullanılabilirlik zaman süresi başına ortalama|Bağlantı noktası|
|ByteCount|Bayt sayısı|Sayı|Toplam|Toplam süre içinde aktarılan bayt sayısı|Bağlantı noktası, yönü|
|PacketCount|Paket sayısı|Sayı|Toplam|Zaman aralığı içinde gönderilen paketlerin toplam sayısı|Bağlantı noktası, yönü|
|SynCount|SYN sayısı|Sayı|Toplam|SYN zaman aralığı içinde gönderilen paketlerin toplam sayısı|Bağlantı noktası, yönü|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Aktarım hızı|Aktarım hızı|BytesPerSecond|Toplam|Uygulama ağ geçidi hizmet saniye başına bayt sayısı|Boyut yok|
|UnhealthyHostCount|İyi durumda olmayan konak sayısı|Sayı|Ortalama|İyi durumda olmayan arka uç ana bilgisayar sayısı|BackendSettingsPool|
|HealthyHostCount|Konak sayısı|Sayı|Ortalama|Sağlıklı bir arka uç ana bilgisayar sayısı|BackendSettingsPool|
|TotalRequests|Toplam İstek Sayısı|Sayı|Toplam|Uygulama ağ geçidi hizmet verdi başarılı istek sayısı|BackendSettingsPool|
|FailedRequests|Başarısız İstekler|Sayı|Toplam|Uygulama ağ geçidi hizmet verdi başarısız istek sayısı|BackendSettingsPool|
|ResponseStatus|Yanıt durumu|Sayı|Toplam|Uygulama ağ geçidi tarafından döndürülen http yanıtı durum|HttpStatusGroup|
|CurrentConnections|Geçerli bağlantılar|Sayı|Toplam|Application Gateway ile kurulan geçerli bağlantı sayısı|Boyut yok|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.Network/virtualNetworkGateways

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|AverageBandwidth|Ağ geçidi S2S bant genişliği|BytesPerSecond|Ortalama|Bir ağ geçidinin saniye başına bayt cinsinden ortalama siteden siteye bant genişliği|Boyut yok|
|P2SBandwidth|Ağ geçidi P2S bant genişliği|BytesPerSecond|Ortalama|Bir ağ geçidinin saniye başına bayt cinsinden ortalama noktadan siteye bant genişliği|Boyut yok|
|P2SConnectionCount|P2S bağlantısı sayısı|Sayı|Maksimum|Noktadan siteye bağlantı ağ geçidi sayısı|Protokol|
|TunnelAverageBandwidth|Tünel bant genişliği|BytesPerSecond|Ortalama|Bir tünel saniye başına bayt cinsinden ortalama bant genişliği|ConnectionName, RemoteIP|
|TunnelEgressBytes|Tünel çıkış bayt|Bayt|Toplam|Bir tünel Giden bayt|ConnectionName, RemoteIP|
|TunnelIngressBytes|Tünel giriş bayt|Bayt|Toplam|Bir tünel gelen bayt sayısı|ConnectionName, RemoteIP|
|TunnelEgressPackets|Tünel çıkış paketlerinin|Sayı|Toplam|Tünel giden paket sayısı|ConnectionName, RemoteIP|
|TunnelIngressPackets|Tünel giriş paketlerinin|Sayı|Toplam|Tünel gelen paket sayısı|ConnectionName, RemoteIP|
|TunnelEgressPacketDropTSMismatch|Tünel çıkış TS uyuşmazlığı paket bırakma|Sayı|Toplam|Giden paket bırakma sanal makine sayısının dışında bir tünel trafik seçicisini uyuşmazlığı|ConnectionName, RemoteIP|
|TunnelIngressPacketDropTSMismatch|Tünel giriş TS uyuşmazlığı paket bırakma|Sayı|Toplam|Gelen paket bırakma sanal makine sayısının dışında bir tünel trafik seçicisini uyuşmazlığı|ConnectionName, RemoteIP|

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Ortalama|Saniye başına bit ingressing Azure|Boyut yok|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Ortalama|Saniye başına Azure egressing BITS|Boyut yok|

## <a name="microsoftnetworkexpressroutecircuitspeerings"></a>Microsoft.Network/expressRouteCircuits/peerings

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Ortalama|Saniye başına bit ingressing Azure|Boyut yok|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Ortalama|Saniye başına Azure egressing BITS|Boyut yok|

## <a name="microsoftnetworkconnections"></a>Microsoft.Network/connections

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Ortalama|Saniye başına bit ingressing Azure|Boyut yok|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Ortalama|Saniye başına Azure egressing BITS|Boyut yok|

## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|QpsByEndpoint|Sorgu tarafından döndürülen uç noktası|Sayı|Toplam|Belirli bir zaman dilimi içinde döndürülen bir Traffic Manager uç nokta sayısı|EndpointName|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Uç nokta olarak uç nokta durumu|Sayı|Maksimum|uç noktanın araştırma durumu "etkinse," 1, aksi durumda 0.|EndpointName|

## <a name="microsoftnetworknetworkwatchersconnectionmonitors"></a>Microsoft.Network/networkWatchers/connectionMonitors

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ProbesFailedPercent|% Araştırmaları başarısız oldu|Yüzde|Ortalama|araştırmalar izleme bağlantı başarısız oldu|Boyut yok|
|AverageRoundtripMs|Ort. Gidiş dönüş süresi (ms)|Milisaniye|Ortalama|Kaynak ve hedef arasında gönderilen yoklamalar izleme bağlantı için ortalama ağ gidiş dönüş süresi (ms)|Boyut yok|

## <a name="microsoftnetworkfrontdoors"></a>Microsoft.Network/frontdoors

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|RequestCount|İstek Sayısı|Sayı|Toplam|HTTP/S proxy tarafından sunulan istemci isteklerinin sayısı|Httpstatus'a, HttpStatusGroup, ClientRegion, ClientCountry|
|RequestSize|İstek boyutu|Bayt|Toplam|İstekleri olarak istemcilerden HTTP/S Ara sunucuya gönderilen bayt sayısı|Httpstatus'a, HttpStatusGroup, ClientRegion, ClientCountry|
|ResponseSize|Yanıt boyutu|Bayt|Toplam|Yanıt HTTP/S proxy sunucudan istemcilere gönderilen bayt sayısı|Httpstatus'a, HttpStatusGroup, ClientRegion, ClientCountry|
|BackendRequestCount|Arka uç istek sayısı|Sayı|Toplam|İçin arka uçlar HTTP/S Ara sunucuya gönderilen istek sayısı|Httpstatus'a, HttpStatusGroup, arka uç|
|BackendRequestLatency|Arka uç istek gecikme süresi|Milisaniye|Ortalama|Ara sunucu HTTP/S son yanıt bayt arka uçtan alınan kadar zaman İstek HTTP/S proxy tarafından arka ucuna gönderilen hesaplandığında|Arka uç|
|TotalLatency|Toplam gecikme süresi|Milisaniye|Ortalama|İstemci, HTTP/S Proxy'den gelen son yanıt bayt onaylanır kadar ne zaman istemci isteği HTTP/S proxy tarafından alındı hesaplandığında|Httpstatus'a, HttpStatusGroup, ClientRegion, ClientCountry|
|BackendHealthPercentage|Arka uç sistem durumu yüzdesi|Yüzde|Ortalama|Başarılı sistem durumu yüzdesi için arka uçlar HTTP/S Ara sunucuya araştırmaları|Arka uç, BackendPool|
|WebApplicationFirewallRequestCount|Web uygulaması güvenlik duvarı istek sayısı|Sayı|Toplam|Web uygulaması güvenlik duvarı tarafından işlenen istemci isteklerinin sayısı|PolicyName, RuleName, eylem|

## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|registration.all|Kayıt İşlemleri|Sayı|Toplam|Tüm başarılı kayıt işlemlerinin sayısı (oluşturma, güncelleştirme, sorgu ve silme işlemleri). |Boyut yok|
|registration.Create|Kayıt Oluşturma İşlemleri|Sayı|Toplam|Tüm başarılı kayıt oluşturma işlemlerinin sayısı.|Boyut yok|
|registration.Update|Kayıt Güncelleme İşlemleri|Sayı|Toplam|Tüm başarılı kayıt güncelleştirmelerinin sayısı.|Boyut yok|
|registration.get|Kayıt Okuma İşlemleri|Sayı|Toplam|Tüm başarılı kayıt sorgularının sayısı.|Boyut yok|
|registration.Delete|Kayıt Silme İşlemleri|Sayı|Toplam|Tüm başarılı kayıt silme işlemlerinin sayısı.|Boyut yok|
|gelen|Gelen İletiler|Sayı|Toplam|Başarılı olan tüm gönderme API'si çağrılarının sayısı. |Boyut yok|
|incoming.Scheduled|Zamanlanan Anında İletme Bildirimleri Gönderildi|Sayı|Toplam|Zamanlanan Anında İletme Bildirimleri İptal Edildi|Boyut yok|
|incoming.Scheduled.Cancel|Zamanlanan Anında İletme Bildirimleri İptal Edildi|Sayı|Toplam|Zamanlanan Anında İletme Bildirimleri İptal Edildi|Boyut yok|
|scheduled.pending|Bekleyen Zamanlanmış Bildirimler|Sayı|Toplam|Bekleyen Zamanlanmış Bildirimler|Boyut yok|
|installation.all|Yükleme Yönetimi İşlemleri|Sayı|Toplam|Yükleme Yönetimi İşlemleri|Boyut yok|
|installation.get|Yükleme İşlemlerini Al|Sayı|Toplam|Yükleme İşlemlerini Al|Boyut yok|
|installation.upsert|Yükleme İşlemleri Oluştur veya Güncelleştir|Sayı|Toplam|Yükleme İşlemleri Oluştur veya Güncelleştir|Boyut yok|
|installation.Patch|Yükleme İşlemlerine Düzeltme Eki Uygula|Sayı|Toplam|Yükleme İşlemlerine Düzeltme Eki Uygula|Boyut yok|
|installation.Delete|Yükleme İşlemlerini Sil|Sayı|Toplam|Yükleme İşlemlerini Sil|Boyut yok|
|outgoing.allpns.Success|Başarılı bildirimler|Sayı|Toplam|Başarılı olan tüm bildirimlerin sayısı.|Boyut yok|
|outgoing.allpns.invalidpayload|Yük Hataları|Sayı|Toplam|PNS kötü yük hatası döndürdüğü için başarısız olan gönderim sayısı.|Boyut yok|
|outgoing.allpns.pnserror|Harici Bildirim Sistemi Hataları|Sayı|Toplam|PNS ile iletişim kurmada sorun yaşandığı için başarısız olan gönderim sayısı (kimlik doğrulaması sorunları hariç).|Boyut yok|
|outgoing.allpns.channelerror|Kanal Hataları|Sayı|Toplam|Kanal geçersiz olduğu, doğru uygulamayla ilişkili olmadığı, kısıtlandığı veya kanalın süresi dolduğu için başarısız olan gönderim sayısı.|Boyut yok|
|outgoing.allpns.badorexpiredchannel|Hatalı veya Süresi Dolmuş Kanal Hataları|Sayı|Toplam|Kayıttaki kanal/belirteç/registrationId geçersiz olduğundan veya süresi dolduğundan başarısız olan gönderim sayısı.|Boyut yok|
|outgoing.WNS.Success|WNS Başarılı Bildirimler|Sayı|Toplam|Başarılı olan tüm bildirimlerin sayısı.|Boyut yok|
|outgoing.WNS.invalidcredentials|WNS Yetkilendirme Hataları (Geçersiz Kimlik Bilgileri)|Sayı|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediği veya kimlik bilgileri engellendiği için başarısız olan gönderim sayısı. (Windows Live kimlik bilgilerini tanımıyor).|Boyut yok|
|outgoing.wns.badchannel|WNS Geçersiz Kanal Hatası|Sayı|Toplam|Kayıttaki Channelurı tanınmadığı için başarısız olan gönderim sayısı (WNS durumu: (404 bulunamadı).|Boyut yok|
|outgoing.wns.expiredchannel|WNS Süresi Dolan Kanal Hatası|Sayı|Toplam|Channelurı'nin süresi dolduğu için başarısız olan gönderim sayısı (WNS durumu: 410 Gone).|Boyut yok|
|outgoing.WNS.throttled|WNS Kısıtlanan Bildirimler|Sayı|Toplam|WNS bu uygulamayı kısıtladığı için başarısız olan gönderim sayısı (WNS durumu: 406 Kabul edilemez).|Boyut yok|
|outgoing.WNS.tokenproviderunreachable|WNS Yetkilendirme Hataları (Ulaşılamıyor)|Sayı|Toplam|Windows Live hizmetine erişilemiyor.|Boyut yok|
|outgoing.WNS.invalidtoken|WNS Yetkilendirme Hataları (Geçersiz Belirteç)|Sayı|Toplam|WNS'ye sağlanan belirteç geçerli değil (WNS durumu: 401 Yetkisiz).|Boyut yok|
|outgoing.WNS.wrongtoken|WNS Yetkilendirme Hataları (Yanlış Belirteç)|Sayı|Toplam|WNS'ye sağlanan belirteç geçerli, ancak başka bir uygulama için (WNS durumu: 403 Yasak). Kayıttaki Channelurı başka bir uygulama ile ilişkili ise bu durum oluşabilir. İstemci uygulaması bildirim hub'ında kimlik bilgileri bulunan uygulamayla ilişkili olup olmadığını denetleyin.|Boyut yok|
|outgoing.WNS.invalidnotificationformat|WNS Geçersiz Bildirim Biçimi|Sayı|Toplam|Bildirimin biçimi geçersiz (WNS durumu: 400). WNS tüm geçersiz reddetmediğini olduğunu unutmayın.|Boyut yok|
|outgoing.WNS.invalidnotificationsize|WNS Geçersiz Bildirim Boyutu Hatası|Sayı|Toplam|Bildirim yükü çok büyük (WNS durumu: 413).|Boyut yok|
|outgoing.WNS.channelthrottled|WNS Kanal Kısıtlandı|Sayı|Toplam|Bildirim, kayıttaki Channelurı kısıtlandığı için bırakıldı (WNS yanıt üst bilgisi: X-WNS-NotificationStatus:channelThrottled).|Boyut yok|
|outgoing.wns.channeldisconnected|WNS Kanal Bağlantısı Kesildi|Sayı|Toplam|Bildirim, kayıttaki Channelurı kısıtlandığı için bırakıldı (WNS yanıt üst bilgisi: X-WNS-DeviceConnectionStatus: bağlantısı kesildi).|Boyut yok|
|outgoing.WNS.dropped|WNS Bırakılan Bildirimler|Sayı|Toplam|Bildirim, kayıttaki ChannelURI kısıtlandığı için bırakıldı (X-WNS-NotificationStatus: bırakıldı, ancak X-WNS-DeviceConnectionStatus: bağlantısı kesildi durumda değil).|Boyut yok|
|outgoing.wns.pnserror|WNS Hataları|Sayı|Toplam|Bildirim, WNS ile iletişim kurulurken oluşan hatalardan dolayı teslim edilemedi.|Boyut yok|
|outgoing.wns.authenticationerror|WNS Kimlik Doğrulaması Hataları|Sayı|Toplam|Bildirim, Windows Live ile iletişim kurulurken oluşan hatalardan, geçersiz kimlik bilgilerinden veya yanlış belirteçten dolayı teslim edilemedi.|Boyut yok|
|outgoing.apns.Success|APNS Başarılı Bildirimler|Sayı|Toplam|Başarılı olan tüm bildirimlerin sayısı.|Boyut yok|
|outgoing.apns.invalidcredentials|APNS Yetkilendirme Hataları|Sayı|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediği veya kimlik bilgileri engellendiği için başarısız olan gönderim sayısı.|Boyut yok|
|outgoing.apns.badchannel|APNS Geçersiz Kanal Hatası|Sayı|Toplam|Belirteç geçersiz olduğu için başarısız olan gönderim sayısı (APNS durum kodu: 8).|Boyut yok|
|outgoing.apns.expiredchannel|APNS Süresi Dolan Kanal Hatası|Sayı|Toplam|APNS geri bildirim kanalı tarafından geçersiz kılınan belirteç sayısı.|Boyut yok|
|outgoing.apns.invalidnotificationsize|APNS Geçersiz Bildirim Boyutu Hatası|Sayı|Toplam|Yük fazla büyük olduğu için başarısız olan gönderim sayısı (APNS durum kodu: 7).|Boyut yok|
|outgoing.apns.pnserror|APNS Hataları|Sayı|Toplam|APNS ile iletişim kurulurken oluşan hatalardan dolayı başarısız olan gönderim sayısı.|Boyut yok|
|outgoing.GCM.Success|GCM Başarılı Bildirimler|Sayı|Toplam|Başarılı olan tüm bildirimlerin sayısı.|Boyut yok|
|outgoing.GCM.invalidcredentials|GCM Yetkilendirme Hataları (Geçersiz Kimlik Bilgileri)|Sayı|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediği veya kimlik bilgileri engellendiği için başarısız olan gönderim sayısı.|Boyut yok|
|outgoing.gcm.badchannel|GCM Geçersiz Kanal Hatası|Sayı|Toplam|Kayıttaki RegistrationId tanınmadığı için başarısız olan gönderim sayısı (GCM sonucu: Geçersiz kayıt).|Boyut yok|
|outgoing.GCM.expiredchannel|GCM Süresi Dolan Kanal Hatası|Sayı|Toplam|Kayıttaki RegistrationId süresi dolduğu için başarısız olan gönderim sayısı (GCM sonucu: NotRegistered).|Boyut yok|
|outgoing.GCM.throttled|GCM Kısıtlanan Bildirimler|Sayı|Toplam|GCM bu uygulamayı kısıtladığı için başarısız olan gönderim sayısı (GCM durum kodu: 501-599 veya sonucu: kullanılamaz).|Boyut yok|
|outgoing.GCM.invalidnotificationformat|GCM Geçersiz Bildirim Biçimi|Sayı|Toplam|Yük doğru bir şekilde biçimlendirilmediği için başarısız olan gönderim sayısı (GCM sonucu: Invaliddatakey veya Invalidttl).|Boyut yok|
|outgoing.GCM.invalidnotificationsize|GCM Geçersiz Bildirim Boyutu Hatası|Sayı|Toplam|Yük fazla büyük olduğu için başarısız olan gönderim sayısı (GCM sonucu: MessageTooBig).|Boyut yok|
|outgoing.gcm.wrongchannel|GCM Yanlış Kanal Hatası|Sayı|Toplam|Kayıttaki RegistrationId geçerli uygulamayla ilişkili olmadığı için başarısız olan gönderim sayısı (GCM sonucu: Invalidpackagename).|Boyut yok|
|outgoing.gcm.pnserror|GCM Hataları|Sayı|Toplam|GCM ile iletişim kurulurken oluşan hatalardan dolayı başarısız olan gönderim sayısı.|Boyut yok|
|outgoing.gcm.authenticationerror|GCM Kimlik Doğrulaması Hataları|Sayı|Toplam|PNS belirtilen kimlik kimlik bilgilerini kabul etmediği için başarısız olan gönderim sayısı engellendiği veya Senderıd uygulamada doğru yapılandırılmamış (GCM sonucu: MismatchedSenderId).|Boyut yok|
|outgoing.mpns.Success|MPNS Başarılı Bildirimler|Sayı|Toplam|Başarılı olan tüm bildirimlerin sayısı.|Boyut yok|
|outgoing.mpns.invalidcredentials|MPNS Geçersiz Kimlik Bilgileri|Sayı|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediği veya kimlik bilgileri engellendiği için başarısız olan gönderim sayısı.|Boyut yok|
|outgoing.mpns.badchannel|MPNS Geçersiz Kanal Hatası|Sayı|Toplam|Kayıttaki Channelurı tanınmadığı için başarısız olan gönderim sayısı (MPNS durumu: (404 bulunamadı).|Boyut yok|
|outgoing.mpns.throttled|MPNS Kısıtlanan Bildirimler|Sayı|Toplam|MPNS bu uygulamayı kısıtladığı için başarısız olan gönderim sayısı (WNS MPNS: 406 Kabul edilemez).|Boyut yok|
|outgoing.mpns.invalidnotificationformat|MPNS Geçersiz Bildirim Biçimi|Sayı|Toplam|Bildirimin yükü fazla büyük olduğu için başarısız olan gönderim sayısı.|Boyut yok|
|outgoing.mpns.channeldisconnected|MPNS Kanalın Bağlantısı Kesildi|Sayı|Toplam|Kayıttaki Channelurı'nin bağlantısı kesildiği için başarısız olan gönderim sayısı (MPNS durumu: 412) bulunamadı.|Boyut yok|
|outgoing.mpns.dropped|MPNS Bırakılan Bildirimler|Sayı|Toplam|MPNS tarafından bırakılan gönderim sayısı (MPNS yanıt üst bilgisi: X-NotificationStatus: QueueFull veya gizlenen).|Boyut yok|
|outgoing.mpns.pnserror|MPNS Hataları|Sayı|Toplam|MPNS ile iletişim kurulurken oluşan hatalardan dolayı başarısız olan gönderim sayısı.|Boyut yok|
|outgoing.mpns.authenticationerror|MPNS Kimlik Doğrulaması Hataları|Sayı|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediği veya kimlik bilgileri engellendiği için başarısız olan gönderim sayısı.|Boyut yok|
|notificationhub.pushes|Tüm Giden Bildirimler|Sayı|Toplam|Bildirim hub'ındaki tüm giden bildirimler|Boyut yok|
|incoming.all.Requests|Tüm Gelen İstekler|Sayı|Toplam|Bildirim hub'ı için toplam gelen istek sayısı|Boyut yok|
|incoming.all.failedrequests|Tüm Gelen Başarısız İstekler|Sayı|Toplam|Bildirim hub'ı için toplam gelen başarısız istek sayısı|Boyut yok|

## <a name="microsoftoperationalinsightsworkspaces"></a>Microsoft.OperationalInsights/workspaces

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Average_ % boş Inode'ları|% Boş Inode'ları|Sayı|Ortalama|Average_ % boş Inode'ları|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ boş alan yüzdesi|% Boş alan|Sayı|Ortalama|Average_ boş alan yüzdesi|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % kullanılan Inode|% Kullanılan Inode|Sayı|Ortalama|Average_ % kullanılan Inode|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % kullanılan alan|% Kullanılan alan|Sayı|Ortalama|Average_ % kullanılan alan|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Disk Okuma Bayt/sn|Disk Okuma Bayt/sn|Sayı|Ortalama|Average_Disk Okuma Bayt/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Disk Okuma/sn|Disk Okuma/sn|Sayı|Ortalama|Average_Disk Okuma/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Disk aktarımı/sn|Disk aktarımı/sn|Sayı|Ortalama|Average_Disk aktarımı/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Disk Yazma Bayt/sn|Disk Yazma Bayt/sn|Sayı|Ortalama|Average_Disk Yazma Bayt/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Disk Yazma/sn|Disk Yazma/sn|Sayı|Ortalama|Average_Disk Yazma/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Free megabayt|Boş megabayt|Sayı|Ortalama|Average_Free megabayt|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Logical Disk Bayt/sn|Mantıksal Disk Bayt/sn|Sayı|Ortalama|Average_Logical Disk Bayt/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % kullanılabilir bellek|% Kullanılabilir bellek|Sayı|Ortalama|Average_ % kullanılabilir bellek|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % kullanılabilir takas alanı|% Kullanılabilir takas alanı|Sayı|Ortalama|Average_ % kullanılabilir takas alanı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % kullanılan bellek|% Kullanılan bellek|Sayı|Ortalama|Average_ % kullanılan bellek|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % kullanılan takas alanı|% Kullanılan takas alanı|Sayı|Ortalama|Average_ % kullanılan takas alanı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Available MBayt bellek|Kullanılabilir MBayt belleği|Sayı|Ortalama|Average_Available MBayt bellek|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Available MBayt değiştirme|Kullanılabilir MBayt değiştirme|Sayı|Ortalama|Average_Available MBayt değiştirme|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Page Okuma/sn|Sayfa Okuma/sn|Sayı|Ortalama|Average_Page Okuma/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Page Yazma/sn|Sayfa Yazma/sn|Sayı|Ortalama|Average_Page Yazma/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Pages/sn|Sayfa/sn|Sayı|Ortalama|Average_Pages/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Used MBayt takas alanı|Kullanılan MBayt takas alanı|Sayı|Ortalama|Average_Used MBayt takas alanı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Used bellek MBayt'ı|Kullanılan bellek MBayt|Sayı|Ortalama|Average_Used bellek MBayt'ı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|İletilen Average_Total bayt|Aktarılan toplam bayt|Sayı|Ortalama|İletilen Average_Total bayt|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Alınan Average_Total bayt sayısı|Alınan toplam bayt sayısı|Sayı|Ortalama|Alınan Average_Total bayt sayısı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Total bayt|Toplam Bayt|Sayı|Ortalama|Average_Total bayt|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|İletilen Average_Total paket sayısı|Aktarılan toplam paket sayısı|Sayı|Ortalama|İletilen Average_Total paket sayısı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Alınan Average_Total paketleri|Alınan toplam paket sayısı|Sayı|Ortalama|Alınan Average_Total paketleri|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Total Rx hataları|Toplam Rx hataları|Sayı|Ortalama|Average_Total Rx hataları|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Total Tx hataları|Toplam Tx hataları|Sayı|Ortalama|Average_Total Tx hataları|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Total çakışmaları|Toplam çakışmaları|Sayı|Ortalama|Average_Total çakışmaları|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Avg. Disk sn/Okuma|Ort. Disk sn/Okuma|Sayı|Ortalama|Average_Avg. Disk sn/Okuma|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Avg. Disk sn/Aktarım|Ort. Disk sn/Aktarım|Sayı|Ortalama|Average_Avg. Disk sn/Aktarım|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Avg. Disk sn/yazma|Ort. Disk sn/yazma|Sayı|Ortalama|Average_Avg. Disk sn/yazma|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Physical Disk Bayt/sn|Fiziksel Disk Bayt/sn|Sayı|Ortalama|Average_Physical Disk Bayt/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Pct ayrıcalıklı zaman|PCT ayrıcalıklı zaman|Sayı|Ortalama|Average_Pct ayrıcalıklı zaman|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Pct kullanıcı zamanı|PCT kullanıcı zamanı|Sayı|Ortalama|Average_Pct kullanıcı zamanı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Used bellek KB|Kullanılan bellek KB|Sayı|Ortalama|Average_Used bellek KB|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Virtual paylaşılan bellek|Paylaşılan sanal bellek|Sayı|Ortalama|Average_Virtual paylaşılan bellek|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % DPC Zamanı|% DPC Zamanı|Sayı|Ortalama|Average_ % DPC Zamanı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % boş zaman|% Boş zaman|Sayı|Ortalama|Average_ % boş zaman|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % kesme zamanı|% Kesme Zamanı|Sayı|Ortalama|Average_ % kesme zamanı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % GÇ bekleme zamanı|% GÇ bekleme zamanı|Sayı|Ortalama|Average_ % GÇ bekleme zamanı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % iyi olma zamanı|% İyi olma zamanı|Sayı|Ortalama|Average_ % iyi olma zamanı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % ayrıcalıklı zaman|% Ayrıcalıklı Zaman|Sayı|Ortalama|Average_ % ayrıcalıklı zaman|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % işlemci zamanı|% İşlemci zamanı|Sayı|Ortalama|Average_ % işlemci zamanı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % kullanıcı zamanı|% Kullanıcı Zamanı|Sayı|Ortalama|Average_ % kullanıcı zamanı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Free fiziksel bellek|Boş fiziksel bellek|Sayı|Ortalama|Average_Free fiziksel bellek|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Disk belleği dosyalarında Average_Free alanı|Disk belleği dosyasındaki boş alan|Sayı|Ortalama|Disk belleği dosyalarında Average_Free alanı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Free sanal bellek|Boş sanal bellek|Sayı|Ortalama|Average_Free sanal bellek|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Processes|İşlemler|Sayı|Ortalama|Average_Processes|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Disk belleği dosyalarında depolanan Average_Size|Disk belleği dosyalarında depolanan boyut|Sayı|Ortalama|Disk belleği dosyalarında depolanan Average_Size|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Uptime|Çalışma Süresi|Sayı|Ortalama|Average_Uptime|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Users|Kullanıcılar|Sayı|Ortalama|Average_Users|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Avg. Disk sn/Okuma|Ort. Disk sn/Okuma|Sayı|Ortalama|Average_Avg. Disk sn/Okuma|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Avg. Disk sn/yazma|Ort. Disk sn/yazma|Sayı|Ortalama|Average_Avg. Disk sn/yazma|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Current Disk kuyruğu uzunluğu|Geçerli Disk Sırası Uzunluğu|Sayı|Ortalama|Average_Current Disk kuyruğu uzunluğu|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Disk Okuma/sn|Disk Okuma/sn|Sayı|Ortalama|Average_Disk Okuma/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Disk aktarımı/sn|Disk aktarımı/sn|Sayı|Ortalama|Average_Disk aktarımı/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Disk Yazma/sn|Disk Yazma/sn|Sayı|Ortalama|Average_Disk Yazma/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Free megabayt|Boş megabayt|Sayı|Ortalama|Average_Free megabayt|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ boş alan yüzdesi|% Boş alan|Sayı|Ortalama|Average_ boş alan yüzdesi|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Available MBayt|Kullanılabilir MBayt|Sayı|Ortalama|Average_Available MBayt|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % kullanılan Kaydedilmiş Bayt yüzdesi|% Kullanılan kaydedilmiş bayt|Sayı|Ortalama|Average_ % kullanılan Kaydedilmiş Bayt yüzdesi|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Bytes alınan/sn|Alınan Bayt/sn|Sayı|Ortalama|Average_Bytes alınan/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Bytes gönderilen/sn|Gönderilen bayt/sn|Sayı|Ortalama|Average_Bytes gönderilen/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Bytes toplam/sn|Toplam Bayt/sn|Sayı|Ortalama|Average_Bytes toplam/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % işlemci zamanı|% İşlemci zamanı|Sayı|Ortalama|Average_ % işlemci zamanı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Processor kuyruğu uzunluğu|İşlemci kuyruğu uzunluğu|Sayı|Ortalama|Average_Processor kuyruğu uzunluğu|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Sinyal|Sinyal|Sayı|Toplam|Sinyal|Computer, OSType, Version, SourceComputerId|
|Güncelleştirme|Güncelleştirme|Sayı|Ortalama|Güncelleştirme|Bilgisayar, ürün, Sınıflandırma, isteğe bağlı, onaylanan UpdateState|
|Olay|Olay|Sayı|Ortalama|Olay|Kaynak, EventLog, bilgisayar, EventCategory, EventLevel, EventLevelName, EventID|

## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft.PowerBIDedicated/capacities

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|QueryDuration|Sorgu süresi|Milisaniye|Ortalama|Son aralığı DAX sorgusunun süresi|Boyut yok|
|QueryPoolJobQueueLength|İş parçacıkları: Sorgu havuzu iş kuyruğu uzunluğu|Sayı|Ortalama|Sorgu iş parçacığı havuzu kuyruğundaki iş sayısı.|Boyut yok|
|qpu_high_utilization_metric|QPU yüksek kullanımı|Sayı|Toplam|Son dakika, 1 yüksek QPU kullanımı, aksi durumda 0 QPU yüksek kullanımı|Boyut yok|
|memory_metric|Bellek|Bayt|Ortalama|Bellek. A1, 0-5 Aralık 0-3 GB için A2 GB, 0-10 GB için A3, A4 için 0-25 GB, A5 için 0-50 GB ve 0-100 GB için A6|Boyut yok|
|memory_thrashing_metric|Bellek çok yavaş|Yüzde|Ortalama|Ortalama bellek temizleme.|Boyut yok|

## <a name="microsoftrelaynamespaces"></a>Microsoft.Relay/namespaces

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
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

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SearchLatency|Arama gecikme süresi|Saniye|Ortalama|Arama hizmeti için ortalama bir arama gecikme süresi|Boyut yok|
|SearchQueriesPerSecond|Saniye başına arama sorguları|CountPerSecond|Ortalama|Arama hizmeti için saniye başına arama sorguları|Boyut yok|
|ThrottledSearchQueriesPercentage|Kısıtlanmış arama sorguları yüzdesi|Yüzde|Ortalama|Arama hizmeti için kısıtlanmış arama sorguları yüzdesi|Boyut yok|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SuccessfulRequests|Başarılı istekler (Önizleme)|Sayı|Toplam|Ad alanı (Önizleme) için toplam başarılı istek|EntityName|
|ServerErrors|Sunucu Hataları. (Önizleme)|Sayı|Toplam|Microsoft.ServiceBus için Sunucu Hataları. (Önizleme)|EntityName|
|UserErrors|Kullanıcı Hataları. (Önizleme)|Sayı|Toplam|Microsoft.ServiceBus için Kullanıcı Hataları. (Önizleme)|EntityName|
|ThrottledRequests|Daraltılmış İstekler. (Önizleme)|Sayı|Toplam|Microsoft.ServiceBus için Daraltılmış İstekler. (Önizleme)|EntityName|
|IncomingRequests|Gelen istekler (Önizleme)|Sayı|Toplam|Microsoft.ServiceBus için Gelen İstekler. (Önizleme)|EntityName|
|IncomingMessages|Gelen iletiler (Önizleme)|Sayı|Toplam|Microsoft.ServiceBus için Gelen İletiler. (Önizleme)|EntityName|
|OutgoingMessages|Giden iletiler (Önizleme)|Sayı|Toplam|Microsoft.ServiceBus için Giden İletiler. (Önizleme)|EntityName|
|ActiveConnections|ActiveConnections (Önizleme)|Sayı|Toplam|Microsoft.ServiceBus için Toplam Etkin Bağlantılar. (Önizleme)|Boyut yok|
|Boyut|Boyut (Önizleme)|Bayt|Ortalama|Sıranın/Konunun Bayt cinsinden boyutu. (Önizleme)|EntityName|
|İletiler|Bir Sıradaki/Konudaki iletilerin sayısı. (Önizleme)|Sayı|Ortalama|Bir Sıradaki/Konudaki iletilerin sayısı. (Önizleme)|EntityName|
|ActiveMessages|Bir Sıradaki/Konudaki etkin iletilerin sayısı. (Önizleme)|Sayı|Ortalama|Bir Sıradaki/Konudaki etkin iletilerin sayısı. (Önizleme)|EntityName|
|CPUXNS|Ad alanı başına CPU kullanımı|Yüzde|Maksimum|Service Bus premium ad alanı CPU kullanım ölçümü|Boyut yok|
|WSXNS|Ad alanı başına bellek boyutu kullanımı|Yüzde|Maksimum|Service Bus premium ad alanı bellek kullanım ölçümü|Boyut yok|

## <a name="microsoftsignalrservicesignalr"></a>Microsoft.SignalRService/SignalR

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ConnectionCount|Bağlantı sayısı|Sayı|Maksimum|Kullanıcı bağlantı miktarı.|Uç Nokta|
|MessageCount|İleti Sayısı|Sayı|Toplam|İletilerin toplam tutar.|Boyut yok|
|InboundTraffic|Gelen trafik|Bayt|Toplam|Gelen trafik hizmeti|Boyut yok|
|OutboundTraffic|Giden trafik|Bayt|Toplam|Giden trafik hizmeti|Boyut yok|
|UserErrors|Kullanıcı hataları|Yüzde|Maksimum|Kullanıcı hatalarının yüzdesi|Boyut yok|
|SystemErrors|Sistem hataları|Yüzde|Maksimum|Sistem hatalarının yüzdesi|Boyut yok|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|Boyut yok|
|physical_data_read_percent|Veri G/Ç yüzdesi|Yüzde|Ortalama|Veri G/Ç yüzdesi|Boyut yok|
|log_write_percent|Günlük G/Ç yüzdesi|Yüzde|Ortalama|Günlük G/Ç yüzdesi|Boyut yok|
|dtu_consumption_percent|DTU yüzdesi|Yüzde|Ortalama|DTU yüzdesi|Boyut yok|
|depolama|Toplam veritabanı boyutu|Bayt|Maksimum|Toplam veritabanı boyutu|Boyut yok|
|connection_successful|Başarılı bağlantılar|Sayı|Toplam|Başarılı bağlantılar|Boyut yok|
|connection_failed|Başarısız Bağlantılar|Sayı|Toplam|Başarısız Bağlantılar|Boyut yok|
|blocked_by_firewall|Güvenlik Duvarı tarafından engellendi|Sayı|Toplam|Güvenlik Duvarı tarafından engellendi|Boyut yok|
|Kilitlenme|Kilitlenmeler|Sayı|Toplam|Kilitlenmeler|Boyut yok|
|storage_percent|Veri boyutu yüzdesi|Yüzde|Maksimum|Veri boyutu yüzdesi|Boyut yok|
|xtp_storage_percent|Bellek içi OLTP depolama yüzdesi|Yüzde|Ortalama|Bellek içi OLTP depolama yüzdesi|Boyut yok|
|workers_percent|Çalışan yüzdesi|Yüzde|Ortalama|Çalışan yüzdesi|Boyut yok|
|sessions_percent|Oturumları yüzdesi|Yüzde|Ortalama|Oturumları yüzdesi|Boyut yok|
|dtu_limit|DTU Limit|Sayı|Ortalama|DTU Limit|Boyut yok|
|dtu_used|Kullanılan DTU|Sayı|Ortalama|Kullanılan DTU|Boyut yok|
|dwu_limit|DWU limiti|Sayı|Maksimum|DWU limiti|Boyut yok|
|dwu_consumption_percent|DWU yüzdesi|Yüzde|Maksimum|DWU yüzdesi|Boyut yok|
|dwu_used|Kullanılan DWU|Sayı|Maksimum|Kullanılan DWU|Boyut yok|
|dw_cpu_percent|DW nodu düzeyinde CPU yüzdesi|Yüzde|Ortalama|DW nodu düzeyinde CPU yüzdesi|DwLogicalNodeId|
|dw_physical_data_read_percent|DW düğüm düzeyi veri g/ç yüzdesi|Yüzde|Ortalama|DW düğüm düzeyi veri g/ç yüzdesi|DwLogicalNodeId|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|Boyut yok|
|physical_data_read_percent|Veri G/Ç yüzdesi|Yüzde|Ortalama|Veri G/Ç yüzdesi|Boyut yok|
|log_write_percent|Günlük G/Ç yüzdesi|Yüzde|Ortalama|Günlük G/Ç yüzdesi|Boyut yok|
|dtu_consumption_percent|DTU yüzdesi|Yüzde|Ortalama|DTU yüzdesi|Boyut yok|
|storage_percent|Depolama yüzdesi|Yüzde|Ortalama|Depolama yüzdesi|Boyut yok|
|workers_percent|Çalışan yüzdesi|Yüzde|Ortalama|Çalışan yüzdesi|Boyut yok|
|sessions_percent|Oturumları yüzdesi|Yüzde|Ortalama|Oturumları yüzdesi|Boyut yok|
|eDTU_limit|eDTU sınırı|Sayı|Ortalama|eDTU sınırı|Boyut yok|
|storage_limit|Depolama sınırı|Bayt|Ortalama|Depolama sınırı|Boyut yok|
|eDTU_used|kullanılan eDTU|Sayı|Ortalama|kullanılan eDTU|Boyut yok|
|storage_used|Kullanılan depolama|Bayt|Ortalama|Kullanılan depolama|Boyut yok|
|xtp_storage_percent|Bellek içi OLTP depolama yüzdesi|Yüzde|Ortalama|Bellek içi OLTP depolama yüzdesi|Boyut yok|

## <a name="microsoftsqlmanagedinstances"></a>Microsoft.Sql/managedInstances

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|virtual_core_count|Sanal çekirdek sayısı|Sayı|Ortalama|Sanal çekirdek sayısı|Boyut yok|
|avg_cpu_percent|Ortalama CPU yüzdesi|Yüzde|Ortalama|Ortalama CPU yüzdesi|Boyut yok|
|reserved_storage_mb|Ayrılan depolama alanı|Sayı|Ortalama|Ayrılan depolama alanı|Boyut yok|
|storage_space_used_mb|Kullanılan depolama alanı|Sayı|Ortalama|Kullanılan depolama alanı|Boyut yok|
|io_requests|G/ç isteği sayısı|Sayı|Ortalama|G/ç isteği sayısı|Boyut yok|
|io_bytes_read|GÇ Okunan Bayt|Bayt|Ortalama|GÇ Okunan Bayt|Boyut yok|
|io_bytes_written|GÇ yazılan bayt|Bayt|Ortalama|GÇ yazılan bayt|Boyut yok|

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|UsedCapacity|Kullanılan kapasite|Bayt|Toplam|Hesabın kullanılan kapasitesi|Boyut yok|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı başarılı ve başarısız isteklerin yanı sıra hataları üretilen istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın.|ResponseType, GeoType, ApiName, kimlik doğrulaması|
|Giriş|Giriş|Bayt|Toplam|Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|GeoType, ApiName, kimlik doğrulaması|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|GeoType, ApiName, kimlik doğrulaması|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer, AverageE2ELatency öğesinde belirtilen ağ gecikme süresini içermez.|GeoType, ApiName, kimlik doğrulaması|
|Başarı E2e|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|GeoType, ApiName, kimlik doğrulaması|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik TotalBillableRequests değeri ve beklenmeyen hata üreten dahil ilgili istek sayısına göre bölme göre hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|GeoType, ApiName, kimlik doğrulaması|

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BlobCapacity|Blob Kapasitesi|Bayt|Toplam|Bayt olarak depolama hesabının Blob hizmeti tarafından kullanılan depolama miktarı.|BlobType|
|BLOB sayısı|Blob Sayısı|Sayı|Toplam|Depolama hesabının Blob hizmetindeki Blob sayısı.|BlobType|
|ContainerCount|Blob Kapsayıcı Sayısı|Sayı|Ortalama|Depolama hesabının Blob hizmetindeki kapsayıcı sayısı.|Boyut yok|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı başarılı ve başarısız isteklerin yanı sıra hataları üretilen istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın.|ResponseType, GeoType, ApiName, kimlik doğrulaması|
|Giriş|Giriş|Bayt|Toplam|Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|GeoType, ApiName, kimlik doğrulaması|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|GeoType, ApiName, kimlik doğrulaması|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer, AverageE2ELatency öğesinde belirtilen ağ gecikme süresini içermez.|GeoType, ApiName, kimlik doğrulaması|
|Başarı E2e|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|GeoType, ApiName, kimlik doğrulaması|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik TotalBillableRequests değeri ve beklenmeyen hata üreten dahil ilgili istek sayısına göre bölme göre hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|GeoType, ApiName, kimlik doğrulaması|

## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft.Storage/storageAccounts/fileServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|FileCapacity|Dosya Kapasitesi|Bayt|Ortalama|Bayt olarak, depolama hesabının Dosya hizmetinin kullandığı depolama miktarı.|Boyut yok|
|FileCount|Dosya Sayısı|Sayı|Ortalama|Depolama hesabının Dosya hizmetindeki dosya sayısı.|Boyut yok|
|FileShareCount|Dosya Paylaşım Sayısı|Sayı|Ortalama|Depolama hesabının Dosya hizmetindeki dosya paylaşımlarının sayısı.|Boyut yok|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı başarılı ve başarısız isteklerin yanı sıra hataları üretilen istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın.|ResponseType, GeoType, ApiName, kimlik doğrulaması|
|Giriş|Giriş|Bayt|Toplam|Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|GeoType, ApiName, kimlik doğrulaması|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|GeoType, ApiName, kimlik doğrulaması|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer, AverageE2ELatency öğesinde belirtilen ağ gecikme süresini içermez.|GeoType, ApiName, kimlik doğrulaması|
|Başarı E2e|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|GeoType, ApiName, kimlik doğrulaması|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik TotalBillableRequests değeri ve beklenmeyen hata üreten dahil ilgili istek sayısına göre bölme göre hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|GeoType, ApiName, kimlik doğrulaması|

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|QueueCapacity|Kuyruk Kapasitesi|Bayt|Ortalama|Bayt olarak depolama hesabının Kuyruk hizmeti tarafından kullanılan depolama miktarı.|Boyut yok|
|QueueCount|Kuyruk Sayısı|Sayı|Ortalama|Depolama hesabının Kuyruk hizmetindeki sıra sayısı.|Boyut yok|
|QueueMessageCount|Kuyruk İleti Sayısı|Sayı|Ortalama|Depolama hesabının Kuyruk hizmetindeki sıra iletilerinin yaklaşık sayısı.|Boyut yok|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı başarılı ve başarısız isteklerin yanı sıra hataları üretilen istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın.|ResponseType, GeoType, ApiName, kimlik doğrulaması|
|Giriş|Giriş|Bayt|Toplam|Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|GeoType, ApiName, kimlik doğrulaması|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|GeoType, ApiName, kimlik doğrulaması|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer, AverageE2ELatency öğesinde belirtilen ağ gecikme süresini içermez.|GeoType, ApiName, kimlik doğrulaması|
|Başarı E2e|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|GeoType, ApiName, kimlik doğrulaması|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik TotalBillableRequests değeri ve beklenmeyen hata üreten dahil ilgili istek sayısına göre bölme göre hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|GeoType, ApiName, kimlik doğrulaması|

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TableCapacity|Tablo Kapasitesi|Bayt|Ortalama|Bayt olarak depolama hesabının Tablo hizmeti tarafından kullanılan depolama miktarı.|Boyut yok|
|TableCount|Tablo Sayısı|Sayı|Ortalama|Depolama hesabının Tablo hizmetindeki tablo sayısı.|Boyut yok|
|TableEntityCount|Tablo Varlık Sayısı|Sayı|Ortalama|Depolama hesabının Tablo hizmetindeki tablo varlıklarının sayısı.|Boyut yok|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı başarılı ve başarısız isteklerin yanı sıra hataları üretilen istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın.|ResponseType, GeoType, ApiName, kimlik doğrulaması|
|Giriş|Giriş|Bayt|Toplam|Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|GeoType, ApiName, kimlik doğrulaması|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|GeoType, ApiName, kimlik doğrulaması|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer, AverageE2ELatency öğesinde belirtilen ağ gecikme süresini içermez.|GeoType, ApiName, kimlik doğrulaması|
|Başarı E2e|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|GeoType, ApiName, kimlik doğrulaması|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik TotalBillableRequests değeri ve beklenmeyen hata üreten dahil ilgili istek sayısına göre bölme göre hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|GeoType, ApiName, kimlik doğrulaması|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ResourceUtilization|SU Kullanım Yüzdesi|Yüzde|Maksimum|SU Kullanım Yüzdesi|Boyut yok|
|Inputevents|Giriş Olayları|Sayı|Toplam|Giriş Olayları|Boyut yok|
|InputEventBytes|Giriş Olayı Bayt Sayısı|Bayt|Toplam|Giriş Olayı Bayt Sayısı|Boyut yok|
|LateInputEvents|Geç Giriş Olayları|Sayı|Toplam|Geç Giriş Olayları|Boyut yok|
|OutputEvents|Çıkış Olayları|Sayı|Toplam|Çıkış Olayları|Boyut yok|
|ConversionErrors|Veri Dönüştürme Hataları|Sayı|Toplam|Veri Dönüştürme Hataları|Boyut yok|
|Hatalar|Çalışma Zamanı Hataları|Sayı|Toplam|Çalışma Zamanı Hataları|Boyut yok|
|DroppedOrAdjustedEvents|Sıralama dışındaki Olaylar|Sayı|Toplam|Sıralama dışındaki Olaylar|Boyut yok|
|AMLCalloutRequests|İşlev İstekleri|Sayı|Toplam|İşlev İstekleri|Boyut yok|
|AMLCalloutFailedRequests|Başarısız İşlev İstekleri|Sayı|Toplam|Başarısız İşlev İstekleri|Boyut yok|
|AMLCalloutInputEvents|İşlev Olayları|Sayı|Toplam|İşlev Olayları|Boyut yok|
|DeserializationError|Giriş Serileştirme Kaldırma Hataları|Sayı|Toplam|Giriş Serileştirme Kaldırma Hataları|Boyut yok|
|EarlyInputEvents|Erken Giriş Olayları|Sayı|Toplam|Erken Giriş Olayları|Boyut yok|
|OutputWatermarkDelaySeconds|Eşik Gecikmesi|Saniye|Maksimum|Eşik Gecikmesi|Boyut yok|

## <a name="microsofttimeseriesinsightsenvironments"></a>Microsoft.TimeSeriesInsights/environments

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|IngressReceivedMessages|Giriş iletileri alındı|Sayı|Toplam|Tüm olay hub'ı veya IOT hub'ı olay kaynakları okuma ileti sayısı|Boyut yok|
|IngressReceivedInvalidMessages|Giriş geçersiz ileti alındı.|Sayı|Toplam|Tüm olay hub'ı veya IOT hub'ı olay kaynakları okuma geçersiz ileti sayısı|Boyut yok|
|IngressReceivedBytes|Giriş alınan bayt|Bayt|Toplam|Tüm olay kaynaklarından okunan bayt sayısı|Boyut yok|
|IngressStoredBytes|Giriş bayt depolanan|Bayt|Toplam|Olayların başarıyla işlendi ve sorgu için kullanılabilir toplam boyutu|Boyut yok|
|IngressStoredEvents|Giriş olayları depolanan|Sayı|Toplam|Düzleştirilmiş olayları başarıyla işlendi ve sorgu sayısı|Boyut yok|
|IngressReceivedMessagesTimeLag|Alınan iletilerin zaman gecikmesini giriş|Saniye|Maksimum|İleti sıraya alınan olay kaynağındaki zamanı giriş işlendiği saati arasındaki farkı|Boyut yok|
|IngressReceivedMessagesCountLag|Giriş alınan iletilerin sayısını gecikme|Sayı|Ortalama|Son sıradaki ileti sıra numarası arasındaki fark, giriş işlenmekte olan ileti bölüm ve sıra sayısı olay kaynağı|Boyut yok|

## <a name="microsofttimeseriesinsightsenvironmentseventsources"></a>Microsoft.TimeSeriesInsights/environments/eventsources

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|IngressReceivedMessages|Giriş iletileri alındı|Sayı|Toplam|İletilerin olay kaynağından okuma sayısı|Boyut yok|
|IngressReceivedInvalidMessages|Giriş geçersiz ileti alındı.|Sayı|Toplam|Olay kaynağından okuma geçersiz ileti sayısı|Boyut yok|
|IngressReceivedBytes|Giriş alınan bayt|Bayt|Toplam|Olay kaynağından okunan bayt sayısı|Boyut yok|
|IngressStoredBytes|Giriş bayt depolanan|Bayt|Toplam|Olayların başarıyla işlendi ve sorgu için kullanılabilir toplam boyutu|Boyut yok|
|IngressStoredEvents|Giriş olayları depolanan|Sayı|Toplam|Düzleştirilmiş olayları başarıyla işlendi ve sorgu sayısı|Boyut yok|
|IngressReceivedMessagesTimeLag|Alınan iletilerin zaman gecikmesini giriş|Saniye|Maksimum|İleti sıraya alınan olay kaynağındaki zamanı giriş işlendiği saati arasındaki farkı|Boyut yok|
|IngressReceivedMessagesCountLag|Giriş alınan iletilerin sayısını gecikme|Sayı|Ortalama|Son sıradaki ileti sıra numarası arasındaki fark, giriş işlenmekte olan ileti bölüm ve sıra sayısı olay kaynağı|Boyut yok|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CpuPercentage|CPU Yüzdesi|Yüzde|Ortalama|CPU Yüzdesi|Örnek|
|MemoryPercentage|Bellek Yüzdesi|Yüzde|Ortalama|Bellek Yüzdesi|Örnek|
|DiskQueueLength|Disk Kuyruğu Uzunluğu|Sayı|Ortalama|Disk Kuyruğu Uzunluğu|Örnek|
|HttpQueueLength|Http Kuyruk Uzunluğu|Sayı|Ortalama|Http Kuyruk Uzunluğu|Örnek|
|BytesReceived|Data Girişi|Bayt|Toplam|Data Girişi|Örnek|
|BytesSent|Giden Veriler|Bayt|Toplam|Giden Veriler|Örnek|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft.Web/sites (işlevler hariç)

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
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
|AppConnections|Bağlantılar|Sayı|Ortalama|Bağlantılar|Örnek|
|Tanıtıcılar|Tanıtıcı Sayısı|Sayı|Ortalama|Tanıtıcı Sayısı|Örnek|
|İş Parçacıkları|İş Parçacığı Sayısı|Sayı|Ortalama|İş Parçacığı Sayısı|Örnek|
|PrivateBytes|Özel Baytlar|Bayt|Ortalama|Özel Baytlar|Örnek|
|IoReadBytesPerSecond|GÇ Okunan Bayt / Saniye|BytesPerSecond|Toplam|GÇ Okunan Bayt / Saniye|Örnek|
|IoWriteBytesPerSecond|GÇ Yazılan Bayt / Saniye|BytesPerSecond|Toplam|GÇ Yazılan Bayt / Saniye|Örnek|
|IoOtherBytesPerSecond|GÇ Diğer Bayt / Saniye|BytesPerSecond|Toplam|GÇ Diğer Bayt / Saniye|Örnek|
|IoReadOperationsPerSecond|GÇ Okuma İşlemi / Saniye|BytesPerSecond|Toplam|GÇ Okuma İşlemi / Saniye|Örnek|
|IoWriteOperationsPerSecond|GÇ Yazma İşlemi / Saniye|BytesPerSecond|Toplam|GÇ Yazma İşlemi / Saniye|Örnek|
|IoOtherOperationsPerSecond|GÇ Diğer İşlemler / Saniye|BytesPerSecond|Toplam|GÇ Diğer İşlemler / Saniye|Örnek|
|RequestsInApplicationQueue|Uygulama Kuyruğundaki İstekler|Sayı|Ortalama|Uygulama Kuyruğundaki İstekler|Örnek|
|CurrentAssemblies|Geçerli Derlemeler|Sayı|Ortalama|Geçerli Derlemeler|Örnek|
|TotalAppDomains|Toplam Uygulama Etki Alanları|Sayı|Ortalama|Toplam Uygulama Etki Alanları|Örnek|
|TotalAppDomainsUnloaded|Yüklenmemiş Toplam Uygulama Etki Alanları|Sayı|Ortalama|Yüklenmemiş Toplam Uygulama Etki Alanları|Örnek|
|Gen0Collections|Gen 0 Atık Toplamaları|Sayı|Toplam|Gen 0 Atık Toplamaları|Örnek|
|Gen1Collections|Gen 1 Atık Toplamaları|Sayı|Toplam|Gen 1 Atık Toplamaları|Örnek|
|Gen2Collections|Gen 2 Atık Toplamaları|Sayı|Toplam|Gen 2 Atık Toplamaları|Örnek|

## <a name="microsoftwebsites-functions"></a>Microsoft.Web/sites (işlevler)

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BytesReceived|Data Girişi|Bayt|Toplam|Data Girişi|Örnek|
|BytesSent|Giden Veriler|Bayt|Toplam|Giden Veriler|Örnek|
|Http5xx|Http Sunucu Hataları|Sayı|Toplam|Http Sunucu Hataları|Örnek|
|MemoryWorkingSet|Bellek çalışma kümesi|Bayt|Ortalama|Bellek çalışma kümesi|Örnek|
|AverageMemoryWorkingSet|Ortalama bellek çalışma kümesi|Bayt|Ortalama|Ortalama bellek çalışma kümesi|Örnek|
|FunctionExecutionUnits|İşlev Yürütme Birimleri|Sayı|Toplam|İşlev Yürütme Birimleri|Örnek|
|İşlev yürütme sayısı|İşlev Yürütme Sayısı|Sayı|Toplam|İşlev Yürütme Sayısı|Örnek|
|PrivateBytes|Özel Baytlar|Bayt|Ortalama|Özel Baytlar|Örnek|
|IoReadBytesPerSecond|GÇ Okunan Bayt / Saniye|BytesPerSecond|Toplam|GÇ Okunan Bayt / Saniye|Örnek|
|IoWriteBytesPerSecond|GÇ Yazılan Bayt / Saniye|BytesPerSecond|Toplam|GÇ Yazılan Bayt / Saniye|Örnek|
|IoOtherBytesPerSecond|GÇ Diğer Bayt / Saniye|BytesPerSecond|Toplam|GÇ Diğer Bayt / Saniye|Örnek|
|IoReadOperationsPerSecond|GÇ Okuma İşlemi / Saniye|BytesPerSecond|Toplam|GÇ Okuma İşlemi / Saniye|Örnek|
|IoWriteOperationsPerSecond|GÇ Yazma İşlemi / Saniye|BytesPerSecond|Toplam|GÇ Yazma İşlemi / Saniye|Örnek|
|IoOtherOperationsPerSecond|GÇ Diğer İşlemler / Saniye|BytesPerSecond|Toplam|GÇ Diğer İşlemler / Saniye|Örnek|
|RequestsInApplicationQueue|Uygulama Kuyruğundaki İstekler|Sayı|Ortalama|Uygulama Kuyruğundaki İstekler|Örnek|
|CurrentAssemblies|Geçerli Derlemeler|Sayı|Ortalama|Geçerli Derlemeler|Örnek|
|TotalAppDomains|Toplam Uygulama Etki Alanları|Sayı|Ortalama|Toplam Uygulama Etki Alanları|Örnek|
|TotalAppDomainsUnloaded|Yüklenmemiş Toplam Uygulama Etki Alanları|Sayı|Ortalama|Yüklenmemiş Toplam Uygulama Etki Alanları|Örnek|
|Gen0Collections|Gen 0 Atık Toplamaları|Sayı|Toplam|Gen 0 Atık Toplamaları|Örnek|
|Gen1Collections|Gen 1 Atık Toplamaları|Sayı|Toplam|Gen 1 Atık Toplamaları|Örnek|
|Gen2Collections|Gen 2 Atık Toplamaları|Sayı|Toplam|Gen 2 Atık Toplamaları|Örnek|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
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
|FunctionExecutionUnits|İşlev Yürütme Birimleri|Sayı|Toplam|İşlev Yürütme Birimleri|Örnek|
|İşlev yürütme sayısı|İşlev Yürütme Sayısı|Sayı|Toplam|İşlev Yürütme Sayısı|Örnek|
|AppConnections|Bağlantılar|Sayı|Ortalama|Bağlantılar|Örnek|
|Tanıtıcılar|Tanıtıcı Sayısı|Sayı|Ortalama|Tanıtıcı Sayısı|Örnek|
|İş Parçacıkları|İş Parçacığı Sayısı|Sayı|Ortalama|İş Parçacığı Sayısı|Örnek|
|PrivateBytes|Özel Baytlar|Bayt|Ortalama|Özel Baytlar|Örnek|
|IoReadBytesPerSecond|GÇ Okunan Bayt / Saniye|BytesPerSecond|Toplam|GÇ Okunan Bayt / Saniye|Örnek|
|IoWriteBytesPerSecond|GÇ Yazılan Bayt / Saniye|BytesPerSecond|Toplam|GÇ Yazılan Bayt / Saniye|Örnek|
|IoOtherBytesPerSecond|GÇ Diğer Bayt / Saniye|BytesPerSecond|Toplam|GÇ Diğer Bayt / Saniye|Örnek|
|IoReadOperationsPerSecond|GÇ Okuma İşlemi / Saniye|BytesPerSecond|Toplam|GÇ Okuma İşlemi / Saniye|Örnek|
|IoWriteOperationsPerSecond|GÇ Yazma İşlemi / Saniye|BytesPerSecond|Toplam|GÇ Yazma İşlemi / Saniye|Örnek|
|IoOtherOperationsPerSecond|GÇ Diğer İşlemler / Saniye|BytesPerSecond|Toplam|GÇ Diğer İşlemler / Saniye|Örnek|
|RequestsInApplicationQueue|Uygulama Kuyruğundaki İstekler|Sayı|Ortalama|Uygulama Kuyruğundaki İstekler|Örnek|
|CurrentAssemblies|Geçerli Derlemeler|Sayı|Ortalama|Geçerli Derlemeler|Örnek|
|TotalAppDomains|Toplam Uygulama Etki Alanları|Sayı|Ortalama|Toplam Uygulama Etki Alanları|Örnek|
|TotalAppDomainsUnloaded|Yüklenmemiş Toplam Uygulama Etki Alanları|Sayı|Ortalama|Yüklenmemiş Toplam Uygulama Etki Alanları|Örnek|
|Gen0Collections|Gen 0 Atık Toplamaları|Sayı|Toplam|Gen 0 Atık Toplamaları|Örnek|
|Gen1Collections|Gen 1 Atık Toplamaları|Sayı|Toplam|Gen 1 Atık Toplamaları|Örnek|
|Gen2Collections|Gen 2 Atık Toplamaları|Sayı|Toplam|Gen 2 Atık Toplamaları|Örnek|

## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft.Web/hostingEnvironments/multiRolePools

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
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
|TotalFrontEnds|Toplam Ön Uç Sayısı|Sayı|Ortalama|Toplam Ön Uç Sayısı|Boyut yok|
|SmallAppServicePlanInstances|Kısa App Service Planı Çalışanları|Sayı|Ortalama|Kısa App Service Planı Çalışanları|Boyut yok|
|MediumAppServicePlanInstances|Orta App Service Planı Çalışanları|Sayı|Ortalama|Orta App Service Planı Çalışanları|Boyut yok|
|LargeAppServicePlanInstances|Uzun App Service Planı Çalışanları|Sayı|Ortalama|Uzun App Service Planı Çalışanları|Boyut yok|

## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft.Web/hostingEnvironments/workerPools

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Bugün için WorkersTotal|Toplam Çalışanlar|Sayı|Ortalama|Toplam Çalışanlar|Boyut yok|
|WorkersAvailable|Kullanılabilir Çalışanlar|Sayı|Ortalama|Kullanılabilir Çalışanlar|Boyut yok|
|WorkersUsed|Kullanılan Çalışanlar|Sayı|Ortalama|Kullanılan Çalışanlar|Boyut yok|
|CpuPercentage|CPU Yüzdesi|Yüzde|Ortalama|CPU Yüzdesi|Örnek|
|MemoryPercentage|Bellek Yüzdesi|Yüzde|Ortalama|Bellek Yüzdesi|Örnek|

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici'de ölçümleri hakkında bilgi edinin](../../azure-monitor/platform/data-collection.md)
* [Ölçümler üzerinde uyarı oluşturma](../../azure-monitor/platform/alerts-overview.md)
* [Depolama, olay hub'ı veya Log Analytics ölçümleri dışarı aktarma](../../azure-monitor/platform/diagnostic-logs-overview.md)
