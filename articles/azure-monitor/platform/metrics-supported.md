---
title: Azure İzleyici desteklenen ölçümler kaynak türüne göre
description: Azure İzleyici ile her bir kaynak türü için mevcut olan ölçümler listesi.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 05/20/2019
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: 70f6e26d423781ba53865304a3fe8440fb120a7a
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705164"
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
|qpu_metric|QPU|Count|Average|QPU. S1, S2 için 0-200 ve S4 için 0-400 için 0-100 aralığında|ServerResourceType|
|memory_metric|Bellek|Bayt|Average|Bellek. 0-25 GB S1, S2 için 0-50 GB ve 0-100 aralığında S4 için GB|ServerResourceType|
|TotalConnectionRequests|Toplam bağlantı istekleri|Count|Average|Toplam bağlantı istekleri. Bunlar, gelen bağlantılardır.|ServerResourceType|
|SuccessfullConnectionsPerSec|Saniye başına başarılı bağlantılar|CountPerSecond|Average|Başarıyla tamamlanan bağlantı oranı.|ServerResourceType|
|TotalConnectionFailures|Toplam bağlantı hataları|Count|Average|Toplam bağlantı denemesi başarısız oldu.|ServerResourceType|
|CurrentUserSessions|Geçerli kullanıcı oturumları|Count|Average|Geçerli kullanıcı oturumlarının sayısı.|ServerResourceType|
|QueryPoolBusyThreads|Sorgu havuzu meşgul iş parçacıkları|Count|Average|Sorgu iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|ServerResourceType|
|CommandPoolJobQueueLength|Komut havuzu iş kuyruğu uzunluğu|Count|Average|Komut iş parçacığı havuzu kuyruğundaki iş sayısı.|ServerResourceType|
|ProcessingPoolJobQueueLength|İşleme havuzu iş kuyruğu uzunluğu|Count|Average|İşleme iş parçacığı havuzu kuyruğundaki/ç olmayan iş sayısı.|ServerResourceType|
|CurrentConnections|Bağlantı: Geçerli bağlantılar|Count|Average|Kurulan istemci bağlantılarının geçerli sayısı.|ServerResourceType|
|CleanerCurrentPrice|Bellek: Temizleyici geçerli fiyatı|Count|Average|Geçerli fiyat, bellek $/ bayt/zaman normalleştirilmiş 1000.|ServerResourceType|
|CleanerMemoryShrinkable|Bellek: Temizleyici belleği daraltılabilir|Bayt|Average|Tarafından arka plan Temizleyicisi temizleme bayt cinsinden bellek miktarı.|ServerResourceType|
|CleanerMemoryNonshrinkable|Bellek: Temizleyici belleği daraltılamaz|Bayt|Average|Tarafından arka plan Temizleyicisi kapsamında olmayan bayt cinsinden bellek miktarı.|ServerResourceType|
|MemoryUsage|Bellek: Bellek kullanımı|Bayt|Average|Temizleyici bellek fiyatı hesaplanırken kullanılan sunucu işlemi bellek kullanımı. Process\PrivateBytes artı eşlenmiş veya ve xVelocity altyapısı bellek sınırını xVelocity bellek içi analiz altyapısı (VertiPaq) tarafından ayrılan herhangi bir bellek yok sayılıyor, bellekle eşlenen verilerin boyutunu sayaç eşittir.|ServerResourceType|
|MemoryLimitHard|Bellek: Bellek sınırı sabit|Bayt|Average|Yapılandırma dosyasından sabit bellek sınırı.|ServerResourceType|
|MemoryLimitHigh|Bellek: Bellek sınırı yüksek|Bayt|Average|Yapılandırma dosyasından yüksek bellek sınırı.|ServerResourceType|
|MemoryLimitLow|Bellek: Bellek sınırı düşük|Bayt|Average|Yapılandırma dosyasından düşük bellek sınırı.|ServerResourceType|
|MemoryLimitVertiPaq|Bellek: Bellek sınırı VertiPaq|Bayt|Average|Yapılandırma dosyasından bellek içi sınır.|ServerResourceType|
|Kota|Bellek: Kota|Bayt|Average|Bayt cinsinden geçerli bellek kotası. Bellek kotası bellek ataması veya bellek ayırma de denir.|ServerResourceType|
|QuotaBlocked|Bellek: Kota engellendi|Count|Average|Diğer bellek kotaları serbest bırakılana kadar engellenen kota isteklerinin geçerli sayısı.|ServerResourceType|
|VertiPaqNonpaged|Bellek: Disk belleği olmayan VertiPaq|Bayt|Average|Bayt bellek kullanımı için bellek içi altyapısı tarafından belirlenen çalışma kilitli.|ServerResourceType|
|VertiPaqPaged|Bellek: Disk belleği olan VertiPaq|Bayt|Average|Bellek içi veriler için disk belleğine alınan bellek bayt sayısı.|ServerResourceType|
|RowsReadPerSec|İşleme: Satır sayısı / sn okuyun|CountPerSecond|Average|Satır oranı tüm ilişkisel veritabanlarından okunan.|ServerResourceType|
|RowsConvertedPerSec|İşleme: Satır sayısı / sn dönüştürüldü|CountPerSecond|Average|İşleme sırasında dönüştürülen bir satır oranı.|ServerResourceType|
|RowsWrittenPerSec|İşleme: Saniye yazılan satır|CountPerSecond|Average|İşleme sırasında yazılan satır oranı.|ServerResourceType|
|CommandPoolBusyThreads|İş parçacıkları: Komut havuzu meşgul iş parçacıkları|Count|Average|Komut iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|ServerResourceType|
|CommandPoolIdleThreads|İş parçacıkları: Komut havuzu boşta iş parçacıkları|Count|Average|Komut iş parçacığı havuzundaki boşta iş parçacığı sayısı.|ServerResourceType|
|LongParsingBusyThreads|İş parçacıkları: Uzun ayrıştırma meşgul iş parçacıkları|Count|Average|Uzun iş parçacığı İnceleme havuzundaki meşgul iş parçacığı sayısı.|ServerResourceType|
|LongParsingIdleThreads|İş parçacıkları: Uzun ayrıştırma boşta iş parçacıkları|Count|Average|Uzun iş parçacığı İnceleme havuzundaki boşta iş parçacığı sayısı.|ServerResourceType|
|LongParsingJobQueueLength|İş parçacıkları: Uzun ayrıştırma iş kuyruğu uzunluğu|Count|Average|Uzun ayrıştırma iş parçacığı havuzu kuyruğundaki iş sayısı.|ServerResourceType|
|ProcessingPoolBusyIOJobThreads|İş parçacıkları: İşleme havuzu meşgul g/ç işi iş parçacıkları|Count|Average|İşleme iş parçacığı havuzunda g/ç işleri çalıştıran iş parçacığı sayısı.|ServerResourceType|
|ProcessingPoolBusyNonIOThreads|İş parçacıkları: İşleme havuzu meşgul/ç olmayan iş parçacıkları|Count|Average|İşleme iş parçacığı havuzunda olmayan g/Ç işleri çalıştıran iş parçacığı sayısı.|ServerResourceType|
|ProcessingPoolIOJobQueueLength|İş parçacıkları: İşleme havuzu g/ç iş kuyruğu uzunluğu|Count|Average|İşleme iş parçacığı havuzu kuyruğundaki g/ç iş sayısı.|ServerResourceType|
|ProcessingPoolIdleIOJobThreads|İş parçacıkları: İşleme havuzu boşta g/ç işi iş parçacıkları|Count|Average|İşleme iş parçacığı havuzundaki g/ç işleri için boşta iş parçacığı sayısı.|ServerResourceType|
|ProcessingPoolIdleNonIOThreads|İş parçacıkları: İşleme havuzu boşta/ç olmayan iş parçacıkları|Count|Average|/ Ç olmayan işler için ayrılan işleme iş parçacığı havuzundaki boşta iş parçacığı sayısı.|ServerResourceType|
|QueryPoolIdleThreads|İş parçacıkları: Sorgu havuzu boşta iş parçacıkları|Count|Average|İşleme iş parçacığı havuzundaki g/ç işleri için boşta iş parçacığı sayısı.|ServerResourceType|
|QueryPoolJobQueueLength|İş parçacıkları: Sorgu havuzu iş kuyruğu uzunluğu|Count|Average|Sorgu iş parçacığı havuzu kuyruğundaki iş sayısı.|ServerResourceType|
|ShortParsingBusyThreads|İş parçacıkları: Kısa ayrıştırma meşgul iş parçacıkları|Count|Average|Kısa ayrıştırma iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|ServerResourceType|
|ShortParsingIdleThreads|İş parçacıkları: Kısa ayrıştırma boşta iş parçacıkları|Count|Average|Kısa ayrıştırma iş parçacığı havuzundaki boşta iş parçacığı sayısı.|ServerResourceType|
|ShortParsingJobQueueLength|İş parçacıkları: Kısa ayrıştırma iş kuyruğu uzunluğu|Count|Average|Kısa ayrıştırma iş parçacığı havuzu kuyruğundaki iş sayısı.|ServerResourceType|
|memory_thrashing_metric|Bellek çok yavaş|Percent|Average|Ortalama bellek temizleme.|ServerResourceType|
|mashup_engine_qpu_metric|M altyapı QPU'su|Count|Average|Karma altyapısı işlemleri tarafından kullanılan qpu|ServerResourceType|
|mashup_engine_memory_metric|M altyapı belleği|Bayt|Average|Karma altyapısı işlemleri tarafından bellek kullanımı|ServerResourceType|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TotalRequests|Toplam ağ geçidi istekleri|Count|Toplam|Ağ geçidi istekleri sayısı|Konum, ana bilgisayar adı|
|SuccessfulRequests|Başarılı ağ geçidi istekleri|Count|Toplam|Başarılı ağ geçidi istek sayısı|Konum, ana bilgisayar adı|
|UnauthorizedRequests|Yetkisiz ağ geçidi istekleri|Count|Toplam|Yetkisiz ağ geçidi istekleri sayısı|Konum, ana bilgisayar adı|
|FailedRequests|Başarısız ağ geçidi istekleri|Count|Toplam|Ağ geçidi istek sayısı|Konum, ana bilgisayar adı|
|OtherRequests|Diğer ağ geçidi istekleri|Count|Toplam|Diğer ağ geçidi istekleri sayısı|Konum, ana bilgisayar adı|
|Duration|Ağ geçidi isteklerinin toplam süre|Milisaniye|Average|Toplam süre, ağ geçidi istekleri milisaniye|Konum, ana bilgisayar adı|
|Kapasite|Kapasite|Percent|Average|ApiManagement hizmeti için kullanım ölçümü|Location|
|EventHubTotalEvents|Toplam EventHub olayları|Count|Toplam|Event Hubs'a gönderilen olay sayısı|Location|
|EventHubSuccessfulEvents|Başarılı EventHub olayları|Count|Toplam|Başarılı EventHub olay sayısı|Location|
|EventHubTotalFailedEvents|Başarısız EventHub olayları|Count|Toplam|Başarısız EventHub olay sayısı|Location|
|EventHubRejectedEvents|Reddedilen EventHub olayları|Count|Toplam|Reddedilen EventHub olay sayısı (yanlış yapılandırma veya yetkisiz)|Location|
|EventHubThrottledEvents|EventHub kısıtlanan olaylar|Count|Toplam|EventHub kısıtlanan olay sayısı|Location|
|EventHubTimedoutEvents|EventHub olaylarını zaman aşımına uğradı|Count|Toplam|Zaman aşımına EventHub olay sayısı|Location|
|EventHubDroppedEvents|Bırakılan EventHub olayları|Count|Toplam|Olay sayısı kuyruk boyutu sınırına ulaşıldı nedeniyle atlandı|Location|
|EventHubTotalBytesSent|EventHub olayların boyutu|Bayt|Toplam|Bayt cinsinden EventHub olayların toplam boyutu|Location|
|İstekler|İstekler|Count|Toplam|Ağ geçidi istekleri|Konum, BackendResponseCode, LastErrorReason, GatewayResponseCode|

## <a name="microsoftautomationautomationaccounts"></a>Microsoft.Automation/automationAccounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TotalJob|Toplam iş sayısı|Count|Toplam|İşlerin toplam sayısı|Runbook durumu|
|TotalUpdateDeploymentRuns|Toplam güncelleştirme dağıtım çalışmaları|Count|Toplam|Toplam yazılım güncelleştirme dağıtımı çalıştırır|SoftwareUpdateConfigurationName, durumu|
|TotalUpdateDeploymentMachineRuns|Toplam güncelleştirme dağıtım makine çalıştırması yok|Count|Toplam|Toplam yazılım güncelleştirme dağıtım makinesini çalıştırmak bir yazılım güncelleştirme dağıtımı içinde çalıştırır|SoftwareUpdateConfigurationName, Status, TargetComputer, SoftwareUpdateConfigurationRunId|

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CoreCount|Adanmış çekirdek sayısı|Count|Toplam|Adanmış çekirdekler batch hesabındaki toplam sayısı|Boyut yok|
|TotalNodeCount|Adanmış düğüm sayısı|Count|Toplam|Adanmış düğümler batch hesabındaki toplam sayısı|Boyut yok|
|LowPriorityCoreCount|LowPriority çekirdek sayısı|Count|Toplam|Düşük öncelikli çekirdekler batch hesabındaki toplam sayısı|Boyut yok|
|TotalLowPriorityNodeCount|Düşük öncelikli düğüm sayısı|Count|Toplam|Düşük öncelikli düğümler batch hesabındaki toplam sayısı|Boyut yok|
|CreatingNodeCount|Düğüm sayısı oluşturma|Count|Toplam|Oluşturulan düğüm sayısı|Boyut yok|
|StartingNodeCount|Başlangıç düğüm sayısı|Count|Toplam|Başlangıç düğüm sayısı|Boyut yok|
|WaitingForStartTaskNodeCount|Başlangıç görevi düğüm sayısı için bekleniyor|Count|Toplam|Başlangıç görevinin tamamlanmasını için bekleyen düğüm sayısı|Boyut yok|
|StartTaskFailedNodeCount|Düğüm sayısı başlangıç görevi başarısız oldu|Count|Toplam|Başlangıç görevi başarısız olduğu düğüm sayısı|Boyut yok|
|IdleNodeCount|Boş düğüm sayısı|Count|Toplam|Boş düğüm sayısı|Boyut yok|
|OfflineNodeCount|Çevrimdışı düğüm sayısı|Count|Toplam|Çevrimdışı düğüm sayısı|Boyut yok|
|RebootingNodeCount|Düğüm sayısı yeniden başlatılıyor|Count|Toplam|Düğüm yeniden başlatma sayısı|Boyut yok|
|ReimagingNodeCount|Düğüm sayısı görüntüsü yeniden oluşturuluyor|Count|Toplam|Yeniden görüntü düğüm sayısı|Boyut yok|
|RunningNodeCount|Çalışan düğüm sayısı|Count|Toplam|Çalışan düğüm sayısı|Boyut yok|
|LeavingPoolNodeCount|Bırakma havuzu düğüm sayısı|Count|Toplam|Havuzdan çıkılıyor düğüm sayısı|Boyut yok|
|UnusableNodeCount|Kullanılamaz durumda düğüm sayısı|Count|Toplam|Kullanılamaz durumda düğüm sayısı|Boyut yok|
|PreemptedNodeCount|Etkisiz düğüm sayısı|Count|Toplam|Etkisiz düğüm sayısı|Boyut yok|
|TaskStartEvent|Görev Başlangıç olayları|Count|Toplam|Başlatıldığından görev toplam sayısı|Boyut yok|
|TaskCompleteEvent|Görev Tamamlandı olayları|Count|Toplam|Tamamlanan görevler toplam sayısı|Boyut yok|
|TaskFailEvent|Görev başarısız olayları|Count|Toplam|Toplam başarısız durumda tamamlanan görev sayısı|Boyut yok|
|PoolCreateEvent|Havuz oluşturma olayları|Count|Toplam|Oluşturulmuş havuzu toplam sayısı|Boyut yok|
|PoolResizeStartEvent|Havuz yeniden boyutlandırma başlangıç olayları|Count|Toplam|Başlatmış havuzu yeniden boyutlandırır toplam sayısı|Boyut yok|
|PoolResizeCompleteEvent|Havuz yeniden boyutlandırma tam olayları|Count|Toplam|Tamamlanan havuzu yeniden boyutlandırır toplam sayısı|Boyut yok|
|PoolDeleteStartEvent|Havuz silme başlangıç olayları|Count|Toplam|Başlatmış havuzu siler toplam sayısı|Boyut yok|
|PoolDeleteCompleteEvent|Havuzu tüm olayları Sil|Count|Toplam|Tamamlanan havuzu siler toplam sayısı|Boyut yok|
|JobDeleteCompleteEvent|İşin tüm olayları Sil|Count|Toplam|Başarıyla silinen işlerin toplam sayısı.|Boyut yok|
|JobDeleteStartEvent|Proje başlangıç olayları Sil|Count|Toplam|Silinecek istenen işlerin toplam sayısı.|Boyut yok|
|JobDisableCompleteEvent|İşi devre dışı bırakma tamamlandı olayları|Count|Toplam|Başarıyla devre dışı bırakılmış işlerin toplam sayısı.|Boyut yok|
|JobDisableStartEvent|İşi devre dışı başlangıç olayları|Count|Toplam|Devre dışı bırakılması istenen işlerin toplam sayısı.|Boyut yok|
|JobStartEvent|Proje başlangıç olayları|Count|Toplam|Başarıyla başlatıldı işlerin toplam sayısı.|Boyut yok|
|JobTerminateCompleteEvent|İşi sonlandırmak tam olayları|Count|Toplam|Başarıyla sonlandırıldı işlerin toplam sayısı.|Boyut yok|
|JobTerminateStartEvent|İşi sonlandırmak başlangıç olayları|Count|Toplam|Sonlandırılacak istenen işlerin toplam sayısı.|Boyut yok|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|connectedclients|Bağlı istemciler|Count|Maksimum||ShardId|
|totalcommandsprocessed|İşlemler toplamı|Count|Toplam||ShardId|
|cachehits|İsabetli Önbellek okuma sayısı|Count|Toplam||ShardId|
|cachemisses|İsabetsiz önbellek okuma sayısı|Count|Toplam||ShardId|
|getcommands|Alır|Count|Toplam||ShardId|
|setcommands|Ayarlar|Count|Toplam||ShardId|
|operationsPerSecond|Saniye başına işlem|Count|Maksimum||ShardId|
|evictedkeys|Çıkarılan anahtarlar|Count|Toplam||ShardId|
|totalkeys|Toplam anahtar sayısı|Count|Maksimum||ShardId|
|expiredkeys|Süresi dolan anahtarlar|Count|Toplam||ShardId|
|usedmemory|Kullanılan bellek|Bayt|Maksimum||ShardId|
|usedmemorypercentage|Kullanılan bellek yüzdesi|Percent|Maksimum||ShardId|
|usedmemoryRss|Kullanılan bellek RSS|Bayt|Maksimum||ShardId|
|serverLoad|Sunucu yükü|Percent|Maksimum||ShardId|
|cacheWrite|Önbellek yazması|BytesPerSecond|Maksimum||ShardId|
|cacheRead|Önbellek okuması|BytesPerSecond|Maksimum||ShardId|
|percentProcessorTime|CPU|Percent|Maksimum||ShardId|
|cacheLatency|Önbellek gecikme mikrosaniye (Önizleme)|Count|Average||ShardId, SampleType|
|hatalar|Hatalar|Count|Maksimum||ShardId, ErrorType|
|connectedclients0|Bağlı istemciler (parça 0)|Count|Maksimum||Boyut yok|
|totalcommandsprocessed0|Toplam işlemler (parça 0)|Count|Toplam||Boyut yok|
|cachehits0|İsabetli Önbellek Okuma (parça 0)|Count|Toplam||Boyut yok|
|cachemisses0|İsabetsiz önbellek okuma sayısı (parça 0)|Count|Toplam||Boyut yok|
|getcommands0|(Parça 0) alır|Count|Toplam||Boyut yok|
|setcommands0|Ayarlar (parça 0)|Count|Toplam||Boyut yok|
|operationsPerSecond0|(Parça 0) saniye başına işlem|Count|Maksimum||Boyut yok|
|evictedkeys0|Çıkarılan anahtarlar (parça 0)|Count|Toplam||Boyut yok|
|totalkeys0|Toplam anahtarları (parça 0)|Count|Maksimum||Boyut yok|
|expiredkeys0|Süresi dolan anahtarlar (parça 0)|Count|Toplam||Boyut yok|
|usedmemory0|Kullanılan bellek (parça 0)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss0|Kullanılan bellek RSS (parça 0)|Bayt|Maksimum||Boyut yok|
|serverLoad0|Sunucu iş yükü (parça 0)|Percent|Maksimum||Boyut yok|
|cacheWrite0|Önbellek yazma (parça 0)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead0|Önbellek Okuma (parça 0)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime0|CPU (parça 0)|Percent|Maksimum||Boyut yok|
|connectedclients1|Bağlı istemciler (parça 1)|Count|Maksimum||Boyut yok|
|totalcommandsprocessed1|Toplam işlemler (parça 1)|Count|Toplam||Boyut yok|
|cachehits1|(Parça 1) İsabetli Önbellek okuma sayısı|Count|Toplam||Boyut yok|
|cachemisses1|(Parça 1) İsabetsiz Önbellek okuma sayısı|Count|Toplam||Boyut yok|
|getcommands1|(Parça 1) alır|Count|Toplam||Boyut yok|
|setcommands1|Ayarlar (parça 1)|Count|Toplam||Boyut yok|
|operationsPerSecond1|(Parça 1) saniye başına işlem|Count|Maksimum||Boyut yok|
|evictedkeys1|Çıkarılan anahtarlar (parça 1)|Count|Toplam||Boyut yok|
|totalkeys1|Toplam anahtarları (parça 1)|Count|Maksimum||Boyut yok|
|expiredkeys1|Süresi dolan anahtarlar (parça 1)|Count|Toplam||Boyut yok|
|usedmemory1|Kullanılan bellek (parça 1)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss1|Kullanılan bellek RSS (parça 1)|Bayt|Maksimum||Boyut yok|
|serverLoad1|Sunucu iş yükü (parça 1)|Percent|Maksimum||Boyut yok|
|cacheWrite1|Önbellek yazma (parça 1)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead1|Önbellek Okuma (parça 1)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime1|CPU (parça 1)|Percent|Maksimum||Boyut yok|
|connectedclients2|Bağlı istemciler (parça 2)|Count|Maksimum||Boyut yok|
|totalcommandsprocessed2|Toplam işlemler (parça 2)|Count|Toplam||Boyut yok|
|cachehits2|(Parça 2) İsabetli Önbellek okuma sayısı|Count|Toplam||Boyut yok|
|cachemisses2|(Parça 2) İsabetsiz Önbellek okuma sayısı|Count|Toplam||Boyut yok|
|getcommands2|(Parça 2) alır|Count|Toplam||Boyut yok|
|setcommands2|Ayarlar (parça 2)|Count|Toplam||Boyut yok|
|operationsPerSecond2|(Parça 2) saniye başına işlem|Count|Maksimum||Boyut yok|
|evictedkeys2|Çıkarılan anahtarlar (parça 2)|Count|Toplam||Boyut yok|
|totalkeys2|Toplam anahtarları (parça 2)|Count|Maksimum||Boyut yok|
|expiredkeys2|Süresi dolan anahtarlar (parça 2)|Count|Toplam||Boyut yok|
|usedmemory2|Kullanılan bellek (parça 2)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss2|Kullanılan bellek RSS (parça 2)|Bayt|Maksimum||Boyut yok|
|serverLoad2|Sunucu iş yükü (parça 2)|Percent|Maksimum||Boyut yok|
|cacheWrite2|Önbellek yazma (parça 2)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead2|Önbellek Okuma (parça 2)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime2|CPU (parça 2)|Percent|Maksimum||Boyut yok|
|connectedclients3|Bağlı istemciler (parça 3)|Count|Maksimum||Boyut yok|
|totalcommandsprocessed3|Toplam işlemler (parça 3)|Count|Toplam||Boyut yok|
|cachehits3|(Parça 3) İsabetli Önbellek okuma sayısı|Count|Toplam||Boyut yok|
|cachemisses3|İsabetsiz önbellek okuma sayısı (parça 3)|Count|Toplam||Boyut yok|
|getcommands3|(Parça 3) alır|Count|Toplam||Boyut yok|
|setcommands3|Ayarlar (parça 3)|Count|Toplam||Boyut yok|
|operationsPerSecond3|(Parça 3) saniye başına işlem|Count|Maksimum||Boyut yok|
|evictedkeys3|Çıkarılan anahtarlar (parça 3)|Count|Toplam||Boyut yok|
|totalkeys3|Toplam anahtarları (parça 3)|Count|Maksimum||Boyut yok|
|expiredkeys3|Süresi dolan anahtarlar (parça 3)|Count|Toplam||Boyut yok|
|usedmemory3|Kullanılan bellek (parça 3)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss3|Kullanılan bellek RSS (parça 3)|Bayt|Maksimum||Boyut yok|
|serverLoad3|Sunucu iş yükü (parça 3)|Percent|Maksimum||Boyut yok|
|cacheWrite3|Önbellek yazma (parça 3)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead3|Önbellek Okuma (parça 3)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime3|CPU (parça 3)|Percent|Maksimum||Boyut yok|
|connectedclients4|Bağlı istemciler (parça 4)|Count|Maksimum||Boyut yok|
|totalcommandsprocessed4|Toplam işlemler (parça 4)|Count|Toplam||Boyut yok|
|cachehits4|İsabetli Önbellek Okuma (parça 4)|Count|Toplam||Boyut yok|
|cachemisses4|İsabetsiz önbellek okuma sayısı (parça 4)|Count|Toplam||Boyut yok|
|getcommands4|(Parça 4) alır|Count|Toplam||Boyut yok|
|setcommands4|Ayarlar (parça 4)|Count|Toplam||Boyut yok|
|operationsPerSecond4|(Parça 4) saniye başına işlem|Count|Maksimum||Boyut yok|
|evictedkeys4|Çıkarılan anahtarlar (parça 4)|Count|Toplam||Boyut yok|
|totalkeys4|Toplam anahtarları (parça 4)|Count|Maksimum||Boyut yok|
|expiredkeys4|Süresi dolan anahtarlar (parça 4)|Count|Toplam||Boyut yok|
|usedmemory4|Kullanılan bellek (parça 4)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss4|Kullanılan bellek RSS (parça 4)|Bayt|Maksimum||Boyut yok|
|serverLoad4|Sunucu iş yükü (parça 4)|Percent|Maksimum||Boyut yok|
|cacheWrite4|Önbellek yazma (parça 4)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead4|Önbellek Okuma (parça 4)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime4|CPU (parça 4)|Percent|Maksimum||Boyut yok|
|connectedclients5|Bağlı istemciler (parça 5)|Count|Maksimum||Boyut yok|
|totalcommandsprocessed5|Toplam işlemler (parça 5)|Count|Toplam||Boyut yok|
|cachehits5|(Parça 5) İsabetli Önbellek okuma sayısı|Count|Toplam||Boyut yok|
|cachemisses5|İsabetsiz önbellek okuma sayısı (parça 5)|Count|Toplam||Boyut yok|
|getcommands5|(Parça 5) alır|Count|Toplam||Boyut yok|
|setcommands5|Ayarlar (parça 5)|Count|Toplam||Boyut yok|
|operationsPerSecond5|(Parça 5) saniye başına işlem|Count|Maksimum||Boyut yok|
|evictedkeys5|Çıkarılan anahtarlar (parça 5)|Count|Toplam||Boyut yok|
|totalkeys5|Toplam anahtarları (parça 5)|Count|Maksimum||Boyut yok|
|expiredkeys5|Süresi dolan anahtarlar (parça 5)|Count|Toplam||Boyut yok|
|usedmemory5|Kullanılan bellek (parça 5)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss5|Kullanılan bellek RSS (parça 5)|Bayt|Maksimum||Boyut yok|
|serverLoad5|Sunucu iş yükü (parça 5)|Percent|Maksimum||Boyut yok|
|cacheWrite5|Önbellek yazma (parça 5)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead5|Önbellek Okuma (parça 5)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime5|CPU (parça 5)|Percent|Maksimum||Boyut yok|
|connectedclients6|Bağlı istemciler (parça 6)|Count|Maksimum||Boyut yok|
|totalcommandsprocessed6|Toplam işlemler (parça 6)|Count|Toplam||Boyut yok|
|cachehits6|(Parça 6) İsabetli Önbellek okuma sayısı|Count|Toplam||Boyut yok|
|cachemisses6|İsabetsiz önbellek okuma sayısı (parça 6)|Count|Toplam||Boyut yok|
|getcommands6|(Parça 6) alır|Count|Toplam||Boyut yok|
|setcommands6|Ayarlar (parça 6)|Count|Toplam||Boyut yok|
|operationsPerSecond6|(Parça 6) saniye başına işlem|Count|Maksimum||Boyut yok|
|evictedkeys6|Çıkarılan anahtarlar (parça 6)|Count|Toplam||Boyut yok|
|totalkeys6|Toplam anahtarları (parça 6)|Count|Maksimum||Boyut yok|
|expiredkeys6|Süresi dolan anahtarlar (parça 6)|Count|Toplam||Boyut yok|
|usedmemory6|Kullanılan bellek (parça 6)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss6|Kullanılan bellek RSS (parça 6)|Bayt|Maksimum||Boyut yok|
|serverLoad6|Sunucu iş yükü (parça 6)|Percent|Maksimum||Boyut yok|
|cacheWrite6|Önbellek yazma (parça 6)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead6|Önbellek Okuma (parça 6)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime6|CPU (parça 6)|Percent|Maksimum||Boyut yok|
|connectedclients7|Bağlı istemciler (parça 7)|Count|Maksimum||Boyut yok|
|totalcommandsprocessed7|Toplam işlemler (parça 7)|Count|Toplam||Boyut yok|
|cachehits7|İsabetli Önbellek Okuma (parça 7)|Count|Toplam||Boyut yok|
|cachemisses7|İsabetsiz önbellek okuma sayısı (parça 7)|Count|Toplam||Boyut yok|
|getcommands7|(Parça 7) alır|Count|Toplam||Boyut yok|
|setcommands7|Ayarlar (parça 7)|Count|Toplam||Boyut yok|
|operationsPerSecond7|(Parça 7) saniye başına işlem|Count|Maksimum||Boyut yok|
|evictedkeys7|Çıkarılan anahtarlar (parça 7)|Count|Toplam||Boyut yok|
|totalkeys7|Toplam anahtarları (parça 7)|Count|Maksimum||Boyut yok|
|expiredkeys7|Süresi dolan anahtarlar (parça 7)|Count|Toplam||Boyut yok|
|usedmemory7|Kullanılan bellek (parça 7)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss7|Kullanılan bellek RSS (parça 7)|Bayt|Maksimum||Boyut yok|
|serverLoad7|Sunucu iş yükü (parça 7)|Percent|Maksimum||Boyut yok|
|cacheWrite7|Önbellek yazma (parça 7)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead7|Önbellek Okuma (parça 7)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime7|CPU (parça 7)|Percent|Maksimum||Boyut yok|
|connectedclients8|Bağlı istemciler (parça 8)|Count|Maksimum||Boyut yok|
|totalcommandsprocessed8|Toplam işlemler (parça 8)|Count|Toplam||Boyut yok|
|cachehits8|(Parça 8) İsabetli Önbellek okuma sayısı|Count|Toplam||Boyut yok|
|cachemisses8|(Parça 8) İsabetsiz Önbellek okuma sayısı|Count|Toplam||Boyut yok|
|getcommands8|(Parça 8) alır|Count|Toplam||Boyut yok|
|setcommands8|Ayarlar (parça 8)|Count|Toplam||Boyut yok|
|operationsPerSecond8|(Parça 8) saniye başına işlem|Count|Maksimum||Boyut yok|
|evictedkeys8|Çıkarılan anahtarlar (parça 8)|Count|Toplam||Boyut yok|
|totalkeys8|Toplam anahtarları (parça 8)|Count|Maksimum||Boyut yok|
|expiredkeys8|Süresi dolan anahtarlar (parça 8)|Count|Toplam||Boyut yok|
|usedmemory8|Kullanılan bellek (parça 8)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss8|Kullanılan bellek RSS (parça 8)|Bayt|Maksimum||Boyut yok|
|serverLoad8|Sunucu iş yükü (parça 8)|Percent|Maksimum||Boyut yok|
|cacheWrite8|Önbellek yazma (parça 8)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead8|Önbellek Okuma (parça 8)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime8|CPU (parça 8)|Percent|Maksimum||Boyut yok|
|connectedclients9|Bağlı istemciler (parça 9)|Count|Maksimum||Boyut yok|
|totalcommandsprocessed9|Toplam işlemler (parça 9)|Count|Toplam||Boyut yok|
|cachehits9|İsabetli Önbellek Okuma (parça 9)|Count|Toplam||Boyut yok|
|cachemisses9|İsabetsiz önbellek okuma sayısı (parça 9)|Count|Toplam||Boyut yok|
|getcommands9|(Parça 9) alır|Count|Toplam||Boyut yok|
|setcommands9|Ayarlar (parça 9)|Count|Toplam||Boyut yok|
|operationsPerSecond9|İşlem / saniye (parça 9)|Count|Maksimum||Boyut yok|
|evictedkeys9|Çıkarılan anahtarlar (parça 9)|Count|Toplam||Boyut yok|
|totalkeys9|Toplam anahtarları (parça 9)|Count|Maksimum||Boyut yok|
|expiredkeys9|Süresi dolan anahtarlar (parça 9)|Count|Toplam||Boyut yok|
|usedmemory9|Kullanılan bellek (parça 9)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss9|Kullanılan bellek RSS (parça 9)|Bayt|Maksimum||Boyut yok|
|serverLoad9|Sunucu iş yükü (parça 9)|Percent|Maksimum||Boyut yok|
|cacheWrite9|Önbellek yazma (parça 9)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead9|Önbellek Okuma (parça 9)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime9|CPU (parça 9)|Percent|Maksimum||Boyut yok|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.ClassicCompute/virtualMachines

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Average|Sanal makineler tarafından şu anda kullanılan ayrılmış işlem birimlerinin yüzdesi.|Boyut yok|
|Ağ Girişi|Ağ Girişi|Bayt|Toplam|(Gelen trafik) sanal makineler tarafından tüm ağ arabirimleri üzerinde alınan bayt sayısı.|Boyut yok|
|Ağ Çıkışı|Ağ Çıkışı|Bayt|Toplam|(Giden trafik) sanal makineler tarafından tüm ağ arabirimleri üzerinde gönderilen bayt sayısı.|Boyut yok|
|Disk okuma bayt/sn|Disk okuma|BytesPerSecond|Average|İzleme dönemi boyunca diskten okunan ortalama bayt.|Boyut yok|
|Disk Yazma Bayt/sn|Disk yazma|BytesPerSecond|Average|İzleme dönemi boyunca diske yazılan ortalama bayt.|Boyut yok|
|Disk okuma işlemi/sn|Disk okuma işlemi/sn|CountPerSecond|Average|Disk okuma IOPS.|Boyut yok|
|Disk yazma işlemi/sn|Disk yazma işlemi/sn|CountPerSecond|Average|Disk yazma IOPS değeri.|Boyut yok|

## <a name="microsoftclassiccomputedomainnamesslotsroles"></a>Microsoft.ClassicCompute/domainNames/slots/roles

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Average|Sanal makineler tarafından şu anda kullanılan ayrılmış işlem birimlerinin yüzdesi.|RoleInstanceId|
|Ağ Girişi|Ağ Girişi|Bayt|Toplam|(Gelen trafik) sanal makineler tarafından tüm ağ arabirimleri üzerinde alınan bayt sayısı.|RoleInstanceId|
|Ağ Çıkışı|Ağ Çıkışı|Bayt|Toplam|(Giden trafik) sanal makineler tarafından tüm ağ arabirimleri üzerinde gönderilen bayt sayısı.|RoleInstanceId|
|Disk okuma bayt/sn|Disk okuma|BytesPerSecond|Average|İzleme dönemi boyunca diskten okunan ortalama bayt.|RoleInstanceId|
|Disk Yazma Bayt/sn|Disk yazma|BytesPerSecond|Average|İzleme dönemi boyunca diske yazılan ortalama bayt.|RoleInstanceId|
|Disk okuma işlemi/sn|Disk okuma işlemi/sn|CountPerSecond|Average|Disk okuma IOPS.|RoleInstanceId|
|Disk yazma işlemi/sn|Disk yazma işlemi/sn|CountPerSecond|Average|Disk yazma IOPS değeri.|RoleInstanceId|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TotalCalls|Toplam çağrı sayısı|Count|Toplam|Toplam çağrı sayısı.|ApiName, OperationName, bölge|
|SuccessfulCalls|Başarılı çağrılar|Count|Toplam|Başarılı çağrı sayısı.|ApiName, OperationName, bölge|
|TotalErrors|Toplam hata|Count|Toplam|Toplam hata yanıtı (HTTP yanıt kodu 4xx veya 5xx) ile çağrı sayısı.|ApiName, OperationName, bölge|
|BlockedCalls|Engellenen çağrılar|Count|Toplam|Bu aşıldı oran veya kota sınırını çağrı sayısı.|ApiName, OperationName, bölge|
|ServerErrors|Sunucu hataları|Count|Toplam|Hizmet iç hatası (HTTP yanıt kodu 5xx) ile çağrı sayısı.|ApiName, OperationName, bölge|
|ClientErrors|İstemci hataları|Count|Toplam|İstemci tarafı hatası (HTTP yanıt kodu 4xx) ile çağrı sayısı.|ApiName, OperationName, bölge|
|Elinden|Verileri|Bayt|Toplam|Gelen verilerin bayt cinsinden boyutu.|ApiName, OperationName, bölge|
|DataOut|Veri çıkışı|Bayt|Toplam|Giden verilerin bayt cinsinden boyutu.|ApiName, OperationName, bölge|
|Gecikme süresi|Gecikme süresi|Milisaniye|Average|Milisaniye cinsinden gecikme süresi.|ApiName, OperationName, bölge|
|CharactersTranslated|Çevrilen karakterler|Count|Toplam|Karakter gelen metin isteğindeki toplam sayısı.|ApiName, OperationName, bölge|
|CharactersTrained|Eğitim karakter|Count|Toplam|Toplam karakter sayısı eğitim.|ApiName, OperationName, bölge|
|SpeechSessionDuration|Konuşma oturumu süresi|Saniye|Toplam|Konuşma oturumunun saniye cinsinden toplam süresi.|ApiName, OperationName, bölge|
|TotalTransactions|Toplam işlem|Count|Toplam|Toplam işlem sayısı.|Boyut yok|
|TotalTokenCalls|Belirteç toplam çağrı sayısı|Count|Toplam|Belirteç çağrıları toplam sayısı.|ApiName, OperationName, bölge|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|Sanal Makineler tarafından kullanılan ayrılmış işlem birimlerinin geçerli yüzdesi|Boyut yok|
|Ağ Girişi|Faturalanabilir ağ|Bayt|Toplam|(Gelen trafik) sanal makineler tarafından tüm ağ arabirimleri üzerinde alınan Faturalanabilir bayt sayısı|Boyut yok|
|Ağ Çıkışı|Faturalanabilir giden ağ|Bayt|Toplam|(Giden trafik) sanal makineler tarafından tüm ağ arabirimleri üzerinde Faturalanabilir bayt sayısı|Boyut yok|
|Disk okuma bayt sayısı|Disk okuma bayt sayısı|Bayt|Toplam|İzleme dönemi boyunca diskten okunan bayt|Boyut yok|
|Disk yazma bayt sayısı|Disk yazma bayt sayısı|Bayt|Toplam|İzleme dönemi boyunca diske yazılan bayt|Boyut yok|
|Disk okuma işlemi/sn|Disk okuma işlemi/sn|CountPerSecond|Average|Disk okuma IOPS|Boyut yok|
|Disk yazma işlemi/sn|Disk yazma işlemi/sn|CountPerSecond|Average|Disk yazma IOPS|Boyut yok|
|Kalan CPU kredisi|Kalan CPU kredisi|Count|Average|Kredi veri bloğunun kullanabileceği toplam sayısı|Boyut yok|
|Tüketilen CPU kredisi|Tüketilen CPU kredisi|Count|Average|Toplam sanal makine tarafından tüketilen kredi sayısı|Boyut yok|
|Disk Okuma Bayt/sn|Veri Disk Okuma Bayt/sn (kullanım dışı)|CountPerSecond|Average|İzleme döneminde tek bir diskten okuma bayt/sn|SlotId|
|Disk Yazma Bayt/sn|Veri diski yazma bayt / (kullanım dışı) sn|CountPerSecond|Average|İzleme döneminde tek bir diske yazılan bayt/sn|SlotId|
|Disk başına okuma işlemi/sn|Veri diski okuma işlemi/sn (kullanım dışı)|CountPerSecond|Average|İzleme döneminde tek bir diskten okuma IOPS|SlotId|
|Disk yazma işlemi/sn|Veri diski yazma işlemi / (kullanım dışı) sn|CountPerSecond|Average|İzleme döneminde tek bir diskten IOPS yazma|SlotId|
|Disk başına QD|Veri diski QD (kullanım dışı)|Count|Average|Veri diski sıra derinliği (veya sıra uzunluğu)|SlotId|
|İşletim sistemi diski başına Okuma Bayt/sn|İşletim sistemi diski Okuma Bayt/sn (kullanım dışı)|CountPerSecond|Average|İzleme döneminde işletim sistemi diski için tek bir diskten okuma bayt/sn|Boyut yok|
|İşletim sistemi diski başına Yazma Bayt/sn|İşletim sistemi diski yazma bayt/sn (kullanım dışı)|CountPerSecond|Average|İzleme döneminde işletim sistemi diski için tek bir diske yazılan bayt/sn|Boyut yok|
|İşletim sistemi diski okuma işlemi/sn başına|İşletim sistemi diski okuma işlemi/sn (kullanım dışı)|CountPerSecond|Average|İşletim sistemi diski için izleme döneminde tek bir diskten okuma IOPS|Boyut yok|
|İşletim sistemi diski başına yazma işlemi/sn|İşletim sistemi diski yazma işlemi/sn (kullanım dışı)|CountPerSecond|Average|İzleme döneminde işletim sistemi diski için tek bir diskten IOPS yazma|Boyut yok|
|İşletim sistemi Disk başına QD|İşletim sistemi diski QD (kullanım dışı)|Count|Average|İşletim sistemi diski sıra derinliği (veya sıra uzunluğu)|Boyut yok|
|Veri Disk Okuma Bayt/sn|Veri Disk Okuma Bayt/sn (Önizleme)|CountPerSecond|Average|İzleme döneminde tek bir diskten okuma bayt/sn|LUN|
|Veri diski yazma bayt/sn|Veri diski yazma bayt/sn (Önizleme)|CountPerSecond|Average|İzleme döneminde tek bir diske yazılan bayt/sn|LUN|
|Veri diski okuma işlemi/sn|Veri diski okuma işlemi/sn (Önizleme)|CountPerSecond|Average|İzleme döneminde tek bir diskten okuma IOPS|LUN|
|Veri diski yazma işlemi/sn|Veri diski yazma işlemi/sn (Önizleme)|CountPerSecond|Average|İzleme döneminde tek bir diskten IOPS yazma|LUN|
|Veri diski sıra derinliği|Veri diski sıra derinliği (Önizleme)|Count|Average|Veri diski sıra derinliği (veya sıra uzunluğu)|LUN|
|İşletim sistemi diski Okuma Bayt/sn|İşletim sistemi diski Okuma Bayt/sn (Önizleme)|CountPerSecond|Average|İzleme döneminde işletim sistemi diski için tek bir diskten okuma bayt/sn|Boyut yok|
|İşletim sistemi diski yazma bayt/sn|İşletim sistemi diski yazma bayt/sn (Önizleme)|CountPerSecond|Average|İzleme döneminde işletim sistemi diski için tek bir diske yazılan bayt/sn|Boyut yok|
|İşletim sistemi diski okuma işlemi/sn|İşletim sistemi diski okuma işlemi/sn (Önizleme)|CountPerSecond|Average|İşletim sistemi diski için izleme döneminde tek bir diskten okuma IOPS|Boyut yok|
|İşletim sistemi diski yazma işlemi/sn|İşletim sistemi diski yazma işlemi/sn (Önizleme)|CountPerSecond|Average|İzleme döneminde işletim sistemi diski için tek bir diskten IOPS yazma|Boyut yok|
|İşletim sistemi diski sıra derinliği|İşletim sistemi diski sıra derinliği (Önizleme)|Count|Average|İşletim sistemi diski sıra derinliği (veya sıra uzunluğu)|Boyut yok|
|Gelen akışlar|Gelen akışlar (Önizleme)|Count|Average|Gelen akışlar numara geçerli akış gelen yönde (VM'ye giden trafik)|Boyut yok|
|Giden akışlar|Giden akışlar (Önizleme)|Count|Average|Giden akışlar numara geçerli akış giden yönde (VM dışında giden trafik)|Boyut yok|
|Gelen akışlar maksimum oluşturma oranı|Gelen akışlar maksimum oluşturma oranı (Önizleme)|CountPerSecond|Average|En fazla oluşturma hızı gelen akışlar (VM'ye giden trafik)|Boyut yok|
|Giden akışlar maksimum oluşturma oranı|Giden akışlar maksimum oluşturma oranı (Önizleme)|CountPerSecond|Average|En fazla oluşturma hızı giden akışlar (VM dışında giden trafik)|Boyut yok|
|Premium veri Disk önbellek okuma isabet|Premium veri Disk önbellek okuma (Önizleme) basın|Percent|Average|Premium veri Disk önbellek okuma isabet|LUN|
|Premium veri Disk önbellek okuma isabetsiz|Premium veri Disk önbellek okuma isabetsiz (Önizleme)|Percent|Average|Premium veri Disk önbellek okuma isabetsiz|LUN|
|Premium işletim sistemi Disk önbellek okuma isabet|Premium işletim sistemi Disk önbellek okuma (Önizleme) basın|Percent|Average|Premium işletim sistemi Disk önbellek okuma isabet|Boyut yok|
|Premium işletim sistemi Disk önbellek okuma isabetsiz|Premium işletim sistemi Disk önbellek okuma isabetsiz (Önizleme)|Percent|Average|Premium işletim sistemi Disk önbellek okuma isabetsiz|Boyut yok|
|Toplam ağ|Toplam ağ|Bayt|Toplam|(Gelen trafik) sanal makineler tarafından tüm ağ arabirimleri üzerinde alınan bayt sayısı|Boyut yok|
|Toplam dış ağ|Toplam dış ağ|Bayt|Toplam|(Giden trafik) sanal makineler tarafından tüm ağ arabirimleri üzerinde gönderilen bayt sayısı|Boyut yok|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|Sanal Makineler tarafından kullanılan ayrılmış işlem birimlerinin geçerli yüzdesi|VMName|
|Ağ Girişi|Faturalanabilir ağ|Bayt|Toplam|(Gelen trafik) sanal makineler tarafından tüm ağ arabirimleri üzerinde alınan Faturalanabilir bayt sayısı|VMName|
|Ağ Çıkışı|Faturalanabilir giden ağ|Bayt|Toplam|(Giden trafik) sanal makineler tarafından tüm ağ arabirimleri üzerinde Faturalanabilir bayt sayısı|VMName|
|Disk okuma bayt sayısı|Disk okuma bayt sayısı|Bayt|Toplam|İzleme dönemi boyunca diskten okunan bayt|VMName|
|Disk yazma bayt sayısı|Disk yazma bayt sayısı|Bayt|Toplam|İzleme dönemi boyunca diske yazılan bayt|VMName|
|Disk okuma işlemi/sn|Disk okuma işlemi/sn|CountPerSecond|Average|Disk okuma IOPS|VMName|
|Disk yazma işlemi/sn|Disk yazma işlemi/sn|CountPerSecond|Average|Disk yazma IOPS|VMName|
|Kalan CPU kredisi|Kalan CPU kredisi|Count|Average|Kredi veri bloğunun kullanabileceği toplam sayısı|Boyut yok|
|Tüketilen CPU kredisi|Tüketilen CPU kredisi|Count|Average|Toplam sanal makine tarafından tüketilen kredi sayısı|Boyut yok|
|Disk Okuma Bayt/sn|Veri Disk Okuma Bayt/sn (kullanım dışı)|CountPerSecond|Average|İzleme döneminde tek bir diskten okuma bayt/sn|SlotId|
|Disk Yazma Bayt/sn|Veri diski yazma bayt / (kullanım dışı) sn|CountPerSecond|Average|İzleme döneminde tek bir diske yazılan bayt/sn|SlotId|
|Disk başına okuma işlemi/sn|Veri diski okuma işlemi/sn (kullanım dışı)|CountPerSecond|Average|İzleme döneminde tek bir diskten okuma IOPS|SlotId|
|Disk yazma işlemi/sn|Veri diski yazma işlemi / (kullanım dışı) sn|CountPerSecond|Average|İzleme döneminde tek bir diskten IOPS yazma|SlotId|
|Disk başına QD|Veri diski QD (kullanım dışı)|Count|Average|Veri diski sıra derinliği (veya sıra uzunluğu)|SlotId|
|İşletim sistemi diski başına Okuma Bayt/sn|İşletim sistemi diski Okuma Bayt/sn (kullanım dışı)|CountPerSecond|Average|İzleme döneminde işletim sistemi diski için tek bir diskten okuma bayt/sn|Boyut yok|
|İşletim sistemi diski başına Yazma Bayt/sn|İşletim sistemi diski yazma bayt/sn (kullanım dışı)|CountPerSecond|Average|İzleme döneminde işletim sistemi diski için tek bir diske yazılan bayt/sn|Boyut yok|
|İşletim sistemi diski okuma işlemi/sn başına|İşletim sistemi diski okuma işlemi/sn (kullanım dışı)|CountPerSecond|Average|İşletim sistemi diski için izleme döneminde tek bir diskten okuma IOPS|Boyut yok|
|İşletim sistemi diski başına yazma işlemi/sn|İşletim sistemi diski yazma işlemi/sn (kullanım dışı)|CountPerSecond|Average|İzleme döneminde işletim sistemi diski için tek bir diskten IOPS yazma|Boyut yok|
|İşletim sistemi Disk başına QD|İşletim sistemi diski QD (kullanım dışı)|Count|Average|İşletim sistemi diski sıra derinliği (veya sıra uzunluğu)|Boyut yok|
|Veri Disk Okuma Bayt/sn|Veri Disk Okuma Bayt/sn (Önizleme)|CountPerSecond|Average|İzleme döneminde tek bir diskten okuma bayt/sn|LUN, VMName|
|Veri diski yazma bayt/sn|Veri diski yazma bayt/sn (Önizleme)|CountPerSecond|Average|İzleme döneminde tek bir diske yazılan bayt/sn|LUN, VMName|
|Veri diski okuma işlemi/sn|Veri diski okuma işlemi/sn (Önizleme)|CountPerSecond|Average|İzleme döneminde tek bir diskten okuma IOPS|LUN, VMName|
|Veri diski yazma işlemi/sn|Veri diski yazma işlemi/sn (Önizleme)|CountPerSecond|Average|İzleme döneminde tek bir diskten IOPS yazma|LUN, VMName|
|Veri diski sıra derinliği|Veri diski sıra derinliği (Önizleme)|Count|Average|Veri diski sıra derinliği (veya sıra uzunluğu)|LUN, VMName|
|İşletim sistemi diski Okuma Bayt/sn|İşletim sistemi diski Okuma Bayt/sn (Önizleme)|CountPerSecond|Average|İzleme döneminde işletim sistemi diski için tek bir diskten okuma bayt/sn|VMName|
|İşletim sistemi diski yazma bayt/sn|İşletim sistemi diski yazma bayt/sn (Önizleme)|CountPerSecond|Average|İzleme döneminde işletim sistemi diski için tek bir diske yazılan bayt/sn|VMName|
|İşletim sistemi diski okuma işlemi/sn|İşletim sistemi diski okuma işlemi/sn (Önizleme)|CountPerSecond|Average|İşletim sistemi diski için izleme döneminde tek bir diskten okuma IOPS|VMName|
|İşletim sistemi diski yazma işlemi/sn|İşletim sistemi diski yazma işlemi/sn (Önizleme)|CountPerSecond|Average|İzleme döneminde işletim sistemi diski için tek bir diskten IOPS yazma|VMName|
|İşletim sistemi diski sıra derinliği|İşletim sistemi diski sıra derinliği (Önizleme)|Count|Average|İşletim sistemi diski sıra derinliği (veya sıra uzunluğu)|VMName|
|Gelen akışlar|Gelen akışlar (Önizleme)|Count|Average|Gelen akışlar numara geçerli akış gelen yönde (VM'ye giden trafik)|VMName|
|Giden akışlar|Giden akışlar (Önizleme)|Count|Average|Giden akışlar numara geçerli akış giden yönde (VM dışında giden trafik)|VMName|
|Gelen akışlar maksimum oluşturma oranı|Gelen akışlar maksimum oluşturma oranı (Önizleme)|CountPerSecond|Average|En fazla oluşturma hızı gelen akışlar (VM'ye giden trafik)|VMName|
|Giden akışlar maksimum oluşturma oranı|Giden akışlar maksimum oluşturma oranı (Önizleme)|CountPerSecond|Average|En fazla oluşturma hızı giden akışlar (VM dışında giden trafik)|VMName|
|Premium veri Disk önbellek okuma isabet|Premium veri Disk önbellek okuma (Önizleme) basın|Percent|Average|Premium veri Disk önbellek okuma isabet|LUN, VMName|
|Premium veri Disk önbellek okuma isabetsiz|Premium veri Disk önbellek okuma isabetsiz (Önizleme)|Percent|Average|Premium veri Disk önbellek okuma isabetsiz|LUN, VMName|
|Premium işletim sistemi Disk önbellek okuma isabet|Premium işletim sistemi Disk önbellek okuma (Önizleme) basın|Percent|Average|Premium işletim sistemi Disk önbellek okuma isabet|VMName|
|Premium işletim sistemi Disk önbellek okuma isabetsiz|Premium işletim sistemi Disk önbellek okuma isabetsiz (Önizleme)|Percent|Average|Premium işletim sistemi Disk önbellek okuma isabetsiz|VMName|
|Toplam ağ|Toplam ağ|Bayt|Toplam|(Gelen trafik) sanal makineler tarafından tüm ağ arabirimleri üzerinde alınan bayt sayısı|VMName|
|Toplam dış ağ|Toplam dış ağ|Bayt|Toplam|(Giden trafik) sanal makineler tarafından tüm ağ arabirimleri üzerinde gönderilen bayt sayısı|VMName|

## <a name="microsoftcontainerinstancecontainergroups"></a>Microsoft.ContainerInstance/containerGroups

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CpuUsage|CPU kullanımı|Count|Average|Milicore'daki tüm çekirdeklerde CPU kullanımı.|kapsayıcı adı|
|MemoryUsage|Bellek kullanımı|Bayt|Average|Bayt olarak toplam bellek kullanımı.|kapsayıcı adı|
|NetworkBytesReceivedPerSecond|Saniye başına alınan ağ bayt sayısı|Bayt|Average|Saniye başına alınan ağ bayt sayısı.|Boyut yok|
|NetworkBytesTransmittedPerSecond|Saniye başına gönderilen ağ bayt sayısı|Bayt|Average|Saniye başına gönderilen ağ bayt sayısı.|Boyut yok|

## <a name="microsoftcontainerregistryregistries"></a>Microsoft.ContainerRegistry/registries

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TotalPullCount|Toplam istek sayısı|Count|Average|Toplam görüntü sayısı çeker|Boyut yok|
|SuccessfulPullCount|Başarılı istek sayısı|Count|Average|Başarılı görüntü çeken sayısı|Boyut yok|
|TotalPushCount|Toplam alım sayısı|Count|Average|Toplam görüntü sayısı gönderim|Boyut yok|
|SuccessfulPushCount|Başarılı bir itme sayısı|Count|Average|Başarılı görüntü gönderim sayısı|Boyut yok|
|RunDuration|Çalıştırma süresi|Milisaniye|Toplam|Milisaniye cinsinden çalıştırma süresi|Boyut yok|

## <a name="microsoftcontainerservicemanagedclusters"></a>Microsoft.ContainerService/managedClusters

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|kube_node_status_allocatable_cpu_cores|Yönetilen bir kümede kullanılabilir cpu çekirdek sayısı|Count|Toplam|Yönetilen bir kümede kullanılabilir cpu çekirdek sayısı|Boyut yok|
|kube_node_status_allocatable_memory_bytes|Yönetilen bir kümede kullanılabilir belleğin toplam tutar|Bayt|Toplam|Yönetilen bir kümede kullanılabilir belleğin toplam tutar|Boyut yok|
|kube_pod_status_ready|Hazır durumda pod'ların sayısını|Count|Toplam|Hazır durumda pod'ların sayısını|ad alanı, pod|
|kube_node_status_condition|Çeşitli düğümü koşulları için durumları|Count|Toplam|Çeşitli düğümü koşulları için durumları|Koşul, durum, Durumu2, düğüm|
|kube_pod_status_phase|Aşama tarafından pod'ların sayısını|Count|Toplam|Aşama tarafından pod'ların sayısını|Aşama, ad alanı, pod|

## <a name="microsoftcustomerinsightshubs"></a>Microsoft.CustomerInsights/hubs

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|DCIApiCalls|Müşteri öngörüleri API çağrıları|Count|Toplam||Boyut yok|
|DCIMappingImportOperationSuccessfulLines|Eşleme içeri aktarma işlemi başarılı satırları|Count|Toplam||Boyut yok|
|DCIMappingImportOperationFailedLines|Satırları eşleme içeri aktarma işlemi başarısız oldu|Count|Toplam||Boyut yok|
|DCIMappingImportOperationTotalLines|Eşleme içeri aktarma işlemi toplam satırlar|Count|Toplam||Boyut yok|
|DCIMappingImportOperationRuntimeInSeconds|Eşleme alma işlemi çalışma zamanı saniye|Saniye|Toplam||Boyut yok|
|DCIOutboundProfileExportSucceeded|Giden profili dışarı aktarma başarılı oldu|Count|Toplam||Boyut yok|
|DCIOutboundProfileExportFailed|Giden profili dışarı aktarılamadı|Count|Toplam||Boyut yok|
|DCIOutboundProfileExportDuration|Giden profili dışarı aktarma süresi|Saniye|Toplam||Boyut yok|
|DCIOutboundKpiExportSucceeded|Giden KPI dışarı aktarma başarılı oldu|Count|Toplam||Boyut yok|
|DCIOutboundKpiExportFailed|Giden KPI dışarı aktarma başarısız oldu|Count|Toplam||Boyut yok|
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
|ImportASAValuesFailed|İçeri aktarma ASA değerleri başarısız sayısı|Count|Toplam||Boyut yok|
|ImportASAValuesSucceeded|İçeri aktarma ASA değerleri sayısı başarılı oldu|Count|Toplam||Boyut yok|
|DCIProfilesCount|Profili örnek sayısı|Count|Son||Boyut yok|
|DCIInteractionsPerMonthCount|Etkileşimler her ay sayısı|Count|Son||Boyut yok|
|DCIKpisCount|KPI sayısı|Count|Son||Boyut yok|
|DCISegmentsCount|Segment sayısı|Count|Son||Boyut yok|
|DCIPredictiveMatchPoliciesCount|Tahmine dayalı eşleşme sayısı|Count|Son||Boyut yok|
|DCIPredictionsCount|Tahmin sayısı|Count|Son||Boyut yok|

## <a name="microsoftdataboxedgedataboxedgedevices"></a>Microsoft.DataBoxEdge/dataBoxEdgeDevices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|NICReadThroughput|Okuma aktarım hızı (ağ)|BytesPerSecond|Average|Ağ geçidi tüm birimleri raporlama döneminde cihazdaki ağ arabiriminin okuma aktarım hızı.|InstanceName|
|NICWriteThroughput|Aktarım hızı (ağ) yazma|BytesPerSecond|Average|Ağ arabirimi ağ geçidi tüm birimleri raporlama döneminde cihazdaki yazma aktarım hızı.|InstanceName|
|CloudReadThroughputPerShare|Bulut indirme aktarım hızı (paylaşım)|BytesPerSecond|Average|İndirme azure'a aktarım hızını raporlama dönemi sırasında paylaşımından.|Paylaş|
|CloudUploadThroughputPerShare|Bulut karşıya yükleme verimi (paylaşım)|BytesPerSecond|Average|Karşıya yükleme azure'a aktarım hızını raporlama dönemi sırasında paylaşımından.|Paylaş|
|BytesUploadedToCloudPerShare|Bulut karşıya yüklenen bayt sayısı (paylaşım)|Bayt|Average|Azure'a raporlama döneminde bir paylaşımdan karşıya yüklenen toplam bayt sayısı.|Paylaş|
|TotalCapacity|Toplam Kapasite|Bayt|Average|Toplam Kapasite|Boyut yok|
|Requiredcapacity|Kullanılabilir kapasite|Bayt|Average|Bayt raporlama dönemi sırasında kullanılabilir kapasiteden.|Boyut yok|
|CloudUploadThroughput|Bulut karşıya yükleme verimi|BytesPerSecond|Average|Bulut karşıya yükleme azure'a aktarım hızını raporlama süresi boyunca.|Boyut yok|
|CloudReadThroughput|Bulut indirme aktarım hızı|BytesPerSecond|Average|Bulut indirme azure'a aktarım hızını raporlama süresi boyunca.|Boyut yok|
|BytesUploadedToCloud|Bulut karşıya yüklenen bayt sayısı (cihaz)|Bayt|Average|Azure'a raporlama döneminde bir CİHAZDAN karşıya yüklenen toplam bayt sayısı.|Boyut yok|
|HyperVVirtualProcessorUtilization|Edge işlem - CPU yüzdesi|Percent|Average|CPU kullanım yüzdesi|InstanceName|
|HyperVMemoryUtilization|İşlem - bellek kullanımı kenar|Percent|Average|Kullanımdaki RAM miktarı|InstanceName|

## <a name="microsoftdatafactorydatafactories"></a>Microsoft.DataFactory/datafactories

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|FailedRuns|Başarısız çalıştırmalar|Count|Toplam||pipelineName, activityName|
|SuccessfulRuns|Başarılı çalıştırmalar|Count|Toplam||pipelineName, activityName|

## <a name="microsoftdatafactoryfactories"></a>Microsoft.DataFactory/factories

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|PipelineFailedRuns|İşlem hattı çalıştırmaları ölçümleri başarısız oldu|Count|Toplam||FailureType, adı|
|PipelineSucceededRuns|İşlem hattı çalıştırmaları ölçümleri başarılı oldu|Count|Toplam||FailureType, adı|
|ActivityFailedRuns|Etkinlik çalıştırmaları ölçümlerinin başarısız oldu|Count|Toplam||ActivityType, PipelineName, FailureType, adı|
|ActivitySucceededRuns|Etkinlik çalıştırmaları ölçümlerinin başarılı oldu|Count|Toplam||ActivityType, PipelineName, FailureType, adı|
|TriggerFailedRuns|Tetikleyici çalıştırmaları ölçümleri başarısız oldu|Count|Toplam||Ad, FailureType|
|TriggerSucceededRuns|Tetikleyici çalıştırmaları ölçümleri başarılı oldu|Count|Toplam||Ad, FailureType|
|IntegrationRuntimeCpuPercentage|Tümleştirme çalışma zamanı CPU kullanımı|Percent|Average||IntegrationRuntimeName, NodeName|
|IntegrationRuntimeAvailableMemory|Tümleştirme çalışma zamanı kullanılabilir bellek|Bayt|Average||IntegrationRuntimeName, NodeName|
|MaxAllowedResourceCount|İzin verilen en fazla varlık sayısı|Count|Maksimum||Boyut yok|
|MaxAllowedFactorySizeInGbUnits|İzin verilen en yüksek Fabrika boyutu (GB birim)|Count|Maksimum||Boyut yok|
|ResourceCount|Toplam varlık sayısı|Count|Maksimum||Boyut yok|
|FactorySizeInGbUnits|Toplam Fabrika boyutu (GB birim)|Count|Maksimum||Boyut yok|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|JobEndedSuccess|Başarılı işler|Count|Toplam|Başarılı İşler sayısı.|Boyut yok|
|JobEndedFailure|Başarısız olan işler|Count|Toplam|Başarısız işleri sayısı.|Boyut yok|
|JobEndedCancelled|İptal edilen işler|Count|Toplam|İptal edilen işler sayısı.|Boyut yok|
|JobAUEndedSuccess|Başarılı AU saati|Saniye|Toplam|Toplam AU saati için başarılı işler.|Boyut yok|
|JobAUEndedFailure|Başarısız AU saati|Saniye|Toplam|Toplam AU saati için başarısız olan işler.|Boyut yok|
|JobAUEndedCancelled|İptal edilen AU saati|Saniye|Toplam|İptal edilen işler için toplam AU saati.|Boyut yok|

## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TotalStorage|Toplam Depolama Alanı|Bayt|Maksimum|Hesapta depolanan verilerin toplam miktarı.|Boyut yok|
|DataWritten|Yazılan veriler|Bayt|Toplam|Toplam hesabına yazılan veri miktarı.|Boyut yok|
|DataRead|Veri okuma|Bayt|Toplam|Toplam veri miktarı hesaptan okuyun.|Boyut yok|
|WriteRequests|Yazma istekleri|Count|Toplam|Hesabı için yazma isteği veri sayısı.|Boyut yok|
|ReadRequests|Okuma istekleri|Count|Toplam|Okuma istekleri hesabına veri sayısı.|Boyut yok|

## <a name="microsoftdbformariadbservers"></a>Microsoft.DBforMariaDB/servers

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Percent|Average|CPU yüzdesi|Boyut yok|
|memory_percent|Bellek yüzdesi|Percent|Average|Bellek yüzdesi|Boyut yok|
|io_consumption_percent|G/ç yüzdesi|Percent|Average|G/ç yüzdesi|Boyut yok|
|storage_percent|Depolama yüzdesi|Percent|Average|Depolama yüzdesi|Boyut yok|
|storage_used|Kullanılan depolama|Bayt|Average|Kullanılan depolama|Boyut yok|
|storage_limit|Depolama sınırı|Bayt|Average|Depolama sınırı|Boyut yok|
|serverlog_storage_percent|Sunucu günlüğü depolama yüzdesi|Percent|Average|Sunucu günlüğü depolama yüzdesi|Boyut yok|
|serverlog_storage_usage|Kullanılan sunucu günlük depolama alanı|Bayt|Average|Kullanılan sunucu günlük depolama alanı|Boyut yok|
|serverlog_storage_limit|Sunucu günlük depolama sınırı|Bayt|Average|Sunucu günlük depolama sınırı|Boyut yok|
|active_connections|Etkin bağlantılar|Count|Average|Etkin bağlantılar|Boyut yok|
|connections_failed|Başarısız Bağlantılar|Count|Toplam|Başarısız Bağlantılar|Boyut yok|
|seconds_behind_master|Saniyeler içinde çoğaltma gecikmesi|Count|Average|Saniyeler içinde çoğaltma gecikmesi|Boyut yok|
|backup_storage_used|Kullanılan yedekleme depolama alanı|Bayt|Average|Kullanılan yedekleme depolama alanı|Boyut yok|
|network_bytes_egress|Ağ Çıkışı|Bayt|Toplam|Etkin bağlantılar arasında ağ çıkışı|Boyut yok|
|network_bytes_ingress|Ağ Girişi|Bayt|Toplam|Etkin bağlantılar arasında ağ içinde|Boyut yok|

## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Percent|Average|CPU yüzdesi|Boyut yok|
|memory_percent|Bellek yüzdesi|Percent|Average|Bellek yüzdesi|Boyut yok|
|io_consumption_percent|G/ç yüzdesi|Percent|Average|G/ç yüzdesi|Boyut yok|
|storage_percent|Depolama yüzdesi|Percent|Average|Depolama yüzdesi|Boyut yok|
|storage_used|Kullanılan depolama|Bayt|Average|Kullanılan depolama|Boyut yok|
|storage_limit|Depolama sınırı|Bayt|Average|Depolama sınırı|Boyut yok|
|serverlog_storage_percent|Sunucu günlüğü depolama yüzdesi|Percent|Average|Sunucu günlüğü depolama yüzdesi|Boyut yok|
|serverlog_storage_usage|Kullanılan sunucu günlük depolama alanı|Bayt|Average|Kullanılan sunucu günlük depolama alanı|Boyut yok|
|serverlog_storage_limit|Sunucu günlük depolama sınırı|Bayt|Average|Sunucu günlük depolama sınırı|Boyut yok|
|active_connections|Etkin bağlantılar|Count|Average|Etkin bağlantılar|Boyut yok|
|connections_failed|Başarısız Bağlantılar|Count|Toplam|Başarısız Bağlantılar|Boyut yok|
|seconds_behind_master|Saniyeler içinde çoğaltma gecikmesi|Count|Average|Saniyeler içinde çoğaltma gecikmesi|Boyut yok|
|backup_storage_used|Kullanılan yedekleme depolama alanı|Bayt|Average|Kullanılan yedekleme depolama alanı|Boyut yok|
|network_bytes_egress|Ağ Çıkışı|Bayt|Toplam|Etkin bağlantılar arasında ağ çıkışı|Boyut yok|
|network_bytes_ingress|Ağ Girişi|Bayt|Toplam|Etkin bağlantılar arasında ağ içinde|Boyut yok|

## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Percent|Average|CPU yüzdesi|Boyut yok|
|memory_percent|Bellek yüzdesi|Percent|Average|Bellek yüzdesi|Boyut yok|
|io_consumption_percent|G/ç yüzdesi|Percent|Average|G/ç yüzdesi|Boyut yok|
|storage_percent|Depolama yüzdesi|Percent|Average|Depolama yüzdesi|Boyut yok|
|storage_used|Kullanılan depolama|Bayt|Average|Kullanılan depolama|Boyut yok|
|storage_limit|Depolama sınırı|Bayt|Average|Depolama sınırı|Boyut yok|
|serverlog_storage_percent|Sunucu günlüğü depolama yüzdesi|Percent|Average|Sunucu günlüğü depolama yüzdesi|Boyut yok|
|serverlog_storage_usage|Kullanılan sunucu günlük depolama alanı|Bayt|Average|Kullanılan sunucu günlük depolama alanı|Boyut yok|
|serverlog_storage_limit|Sunucu günlük depolama sınırı|Bayt|Average|Sunucu günlük depolama sınırı|Boyut yok|
|active_connections|Etkin bağlantılar|Count|Average|Etkin bağlantılar|Boyut yok|
|connections_failed|Başarısız Bağlantılar|Count|Toplam|Başarısız Bağlantılar|Boyut yok|
|backup_storage_used|Kullanılan yedekleme depolama alanı|Bayt|Average|Kullanılan yedekleme depolama alanı|Boyut yok|
|network_bytes_egress|Ağ Çıkışı|Bayt|Toplam|Etkin bağlantılar arasında ağ çıkışı|Boyut yok|
|network_bytes_ingress|Ağ Girişi|Bayt|Toplam|Etkin bağlantılar arasında ağ içinde|Boyut yok|
|pg_replica_log_delay_in_seconds|Çoğaltma gecikmesi|Saniye|Maksimum|Saniyeler içinde çoğaltma gecikmesi|Boyut yok|
|pg_replica_log_delay_in_bytes|Yinelemeler boyunca en fazla gecikme|Bayt|Maksimum|Bayt cinsinden en fazla istek çoğaltma lag|Boyut yok|

## <a name="microsoftdbforpostgresqlserversv2"></a>Microsoft.DBforPostgreSQL/serversv2

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Percent|Average|CPU yüzdesi|Boyut yok|
|memory_percent|Bellek yüzdesi|Percent|Average|Bellek yüzdesi|Boyut yok|
|IOPS|IOPS|Count|Average|Saniye başına g/ç işlemleri|Boyut yok|
|storage_percent|Depolama yüzdesi|Percent|Average|Depolama yüzdesi|Boyut yok|
|storage_used|Kullanılan depolama|Bayt|Average|Kullanılan depolama|Boyut yok|
|active_connections|Etkin bağlantılar|Count|Average|Etkin bağlantılar|Boyut yok|
|network_bytes_egress|Ağ Çıkışı|Bayt|Toplam|Etkin bağlantılar arasında ağ çıkışı|Boyut yok|
|network_bytes_ingress|Ağ Girişi|Bayt|Toplam|Etkin bağlantılar arasında ağ içinde|Boyut yok|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/ıothubs

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Telemetri ileti gönderme denemeleri|Count|Toplam|IOT hub'ınıza gönderilecek CİHAZDAN buluta telemetri iletilerini sayısı çalıştı|Boyut yok|
|d2c.telemetry.ingress.Success|Gönderilen telemetri iletilerini|Count|Toplam|Başarılı bir şekilde IOT hub'ınıza CİHAZDAN buluta telemetri ileti sayısı|Boyut yok|
|c2d.commands.egress.complete.success|Komut tamamlandı|Count|Toplam|Cihaz tarafından başarıyla tamamlandı bulut-cihaz komutlarının sayısı|Boyut yok|
|c2d.commands.egress.abandon.success|Terk komutları|Count|Toplam|Cihaz tarafından terk bulut-cihaz komutlarının sayısı|Boyut yok|
|c2d.commands.egress.reject.success|Reddedilen komutları|Count|Toplam|Cihaz tarafından reddedilen bulut-cihaz komutlarının sayısı|Boyut yok|
|devices.totalDevices|(Kullanım dışı) toplam cihaz sayısı|Count|Toplam|IOT hub'ınıza kayıtlı cihaz sayısı|Boyut yok|
|devices.connectedDevices.allProtocol|Bağlı cihazlar (kullanım dışı) |Count|Toplam|IOT hub'ınıza bağlı cihazların sayısı|Boyut yok|
|d2c.telemetry.egress.success|Yönlendirme: teslim telemetri iletilerini|Count|Toplam|İletileri IOT Hub'ın yönlendirme kullanarak tüm uç noktalara başarıyla teslim sayısı. Bir ileti birden çok Uç noktalara yönlendirilir, bu değer her başarılı bir teslimat için bir tane artırır. Bir ileti birden çok kez aynı uç noktasına teslim ise bu değer her başarılı bir teslimat için bir tane artırır.|Boyut yok|
|d2c.telemetry.egress.dropped|Yönlendirme: bırakılan telemetri iletilerini |Count|Toplam|İletileri IOT Hub'ın nedeniyle ölü uç noktalarına yönlendirme tarafından bırakılan sayısı. Bu değer, geri dönüş yol bırakılan iletiler var. iletilmemiş olarak sunulan iletiler sayılmaz.|Boyut yok|
|d2c.telemetry.egress.orphaned|Yönlendirme: yalnız bırakılmış telemetri iletilerini |Count|Toplam|(Temel kural dahil) tüm yönlendirme kuralları ile eşleşmedi çünkü iletileri IOT Hub'ı yönlendirerek yalnız bırakılmış sayısı. |Boyut yok|
|d2c.telemetry.egress.invalid|Yönlendirme: uyumsuz telemetri iletilerini|Count|Toplam|IOT Hub'ın yönlendirme uç nokta ile uyumsuzluk nedeniyle ileti teslim edilemedi sayısı. Bu değer, yeniden denemeler içermez.|Boyut yok|
|d2c.telemetry.egress.fallback|Yönlendirme: ileti geri dönüş için teslim|Count|Toplam|Geri dönüş rota ile ilişkili uç noktasına iletileri IOT Hub'ın yönlendirme teslim sayısı.|Boyut yok|
|d2c.endpoints.egress.eventHubs|Yönlendirme: iletilerin olay Hub'ına teslim|Count|Toplam|IOT hub'ı başarıyla yönlendirme, olay hub'ı uç noktalar için ileti teslim sayısı.|Boyut yok|
|d2c.endpoints.latency.eventHubs|Yönlendirme: gecikme süresi için Event Hub ileti|Milisaniye|Average|Ortalama gecikme süresi (milisaniye) IOT hub'ına giriş iletisi ve ileti giriş arasında bir olay hub'ı uç nokta olarak.|Boyut yok|
|d2c.endpoints.egress.serviceBusQueues|Yönlendirme: Service Bus kuyruğuna iletiler teslim|Count|Toplam|IOT hub'ı başarıyla yönlendirme iletileri Service Bus kuyruğu Uç noktalara teslim sayısı.|Boyut yok|
|d2c.endpoints.latency.serviceBusQueues|Yönlendirme: gecikme süresi için Service Bus kuyruğuna ileti|Milisaniye|Average|Ortalama gecikme süresi (milisaniye) ileti giriş IOT hub'ına telemetri iletisi giriş arasındaki içine bir Service Bus kuyruk uç noktası.|Boyut yok|
|d2c.endpoints.egress.serviceBusTopics|Yönlendirme: Service Bus konusuna iletiler teslim|Count|Toplam|IOT hub'ı başarıyla yönlendirme iletileri Service Bus konu Uç noktalara teslim sayısı.|Boyut yok|
|d2c.endpoints.latency.serviceBusTopics|Yönlendirme: gecikme süresi için Service Bus konusuna ileti|Milisaniye|Average|Ortalama gecikme süresi (milisaniye) ileti giriş IOT hub'ına telemetri iletisi giriş arasındaki içine bir Service Bus konu başlığı uç noktası.|Boyut yok|
|d2c.endpoints.egress.builtIn.events|Yönlendirme: iletiler iletiler/olaylar için teslim|Count|Toplam|IOT hub'ı başarıyla Yönlendirme iletilerini yerleşik uç noktası (iletiler/olaylar) teslim sayısı. Bu ölçüm, yalnızca Yönlendirme etkinleştirildiğinde çalışmaya başlar (https://aka.ms/iotrouting) IOT hub'ının.|Boyut yok|
|d2c.endpoints.latency.builtIn.events|Yönlendirme: gecikme süresi, iletiler/olaylar ileti|Milisaniye|Average|Ortalama gecikme süresi (milisaniye) ileti giriş IOT hub'ına telemetri iletisi giriş arasındaki içine yerleşik uç noktası (iletiler/olaylar). Bu ölçüm, yalnızca Yönlendirme etkinleştirildiğinde çalışmaya başlar (https://aka.ms/iotrouting) IOT hub'ının.|Boyut yok|
|d2c.endpoints.egress.storage|Yönlendirme: depolama için ileti teslim|Count|Toplam|IOT hub'ı başarıyla yönlendirme depolama uç noktaları için ileti teslim sayısı.|Boyut yok|
|d2c.endpoints.latency.storage|Yönlendirme: depolama gecikmesi ileti|Milisaniye|Average|Ortalama gecikme süresi (milisaniye) ileti giriş IOT hub'ına telemetri iletisi giriş arasındaki içine bir depolama uç noktası.|Boyut yok|
|d2c.endpoints.egress.Storage.bytes|Yönlendirme: veri depolama için teslim|Bayt|Toplam|Veri (bayt), IOT Hub'ın yönlendirme depolama uç noktaları için teslim.|Boyut yok|
|d2c.endpoints.egress.Storage.BLOBS|Yönlendirme: BLOB depolama alanına teslim|Count|Toplam|IOT Hub'ın yönlendirme BLOB Depolama uç noktaları için teslim sayısı.|Boyut yok|
|EventGridDeliveries|Event Grid teslim (Önizleme)|Count|Toplam|IOT hub'ı olayların sayısı, Event Grid yayımladı. Sonucu boyutu başarılı ve başarısız istek sayısı için kullanın. EventType boyut olay türünü gösterir (https://aka.ms/ioteventgrid).|Sonuç, olay türü|
|EventGridLatency|Event Grid olay yayımlandığında IOT hub'ı olayın için ne zaman oluşturulduğu gelen ortalama gecikme süresi (milisaniye). Bu sayı, tüm olay türleri arasındaki bir ortalamadır. Belirli bir olay türü gecikme görmek için EventType boyut kullanın.|olay türü|
|d2c.twin.read.success|Cihazlardan başarılı ikizi okumaları|Count|Toplam|Tüm başarılı cihaz tarafından başlatılan çiftlerde okuma sayısı.|Boyut yok|
|d2c.twin.read.failure|Cihazlardan çiftlerde okuma başarısız oldu|Count|Toplam|Tüm sayısı, cihaz tarafından başlatılan çiftlerde okuma başarısız oldu.|Boyut yok|
|d2c.Twin.Read.size|Çiftlerde okuma cihazlardan yanıt boyutu|Bayt|Average|Ortalama, en düşük ve en fazla başarılı olan tüm cihaz tarafından başlatılan ikizi okur.|Boyut yok|
|d2c.twin.update.success|Cihazlardan başarılı ikizi güncelleştirmeleri|Count|Toplam|Tüm başarılı tarafından başlatılan cihaz ikizi güncelleştirmeleri sayısı.|Boyut yok|
|d2c.twin.update.failure|Cihaz ikizi güncelleştirmeleri başarısız oldu|Count|Toplam|Tüm sayısı tarafından başlatılan cihaz ikizi güncelleştirmeleri başarısız oldu.|Boyut yok|
|d2c.twin.update.size|Cihaz ikizi güncelleştirmeleri boyutu|Bayt|Average|Cihaz tarafından başlatılan ortalama, en düşük ve en büyük boyutu başarılı olan tüm güncelleştirmeleri çifti.|Boyut yok|
|c2d.methods.success|Başarılı bir doğrudan yöntem çağrıları|Count|Toplam|Tüm başarılı bir doğrudan yöntem çağrılarının sayısı.|Boyut yok|
|c2d.methods.failure|Doğrudan yöntem çağrıları başarısız oldu|Count|Toplam|Tüm sayısı doğrudan yöntem çağrısı başarısız oldu.|Boyut yok|
|c2d.methods.requestSize|Doğrudan yöntem çağrılarını isteği boyutu|Bayt|Average|Ortalama, minimum ve maksimum başarılı olan tüm yöntemi istekleri doğrudan.|Boyut yok|
|c2d.methods.responseSize|Doğrudan yöntem çağrılarını yanıt boyutu|Bayt|Average|Ortalama, en az ve en fazla doğrudan yöntem yanıtların tümü başarılı.|Boyut yok|
|c2d.twin.read.success|Arka uçtan başarılı ikizi okumaları|Count|Toplam|Tüm başarılı arka uç başlatılan çiftlerde okuma sayısı.|Boyut yok|
|c2d.Twin.Read.failure|Arka uçtan başarısız ikizi okumaları|Count|Toplam|Tüm sayısı, arka uç başlatılan çiftlerde okuma başarısız oldu.|Boyut yok|
|c2d.Twin.Read.size|Yanıt boyutu ikizinin arka ucundan okur|Bayt|Average|Ortalama, en düşük ve en fazla başarılı olan tüm arka uç başlatılan ikizi okur.|Boyut yok|
|c2d.twin.update.success|Arka uç başarılı ikizi güncelleştirmeleri|Count|Toplam|Tüm başarılı arka uç başlatılan ikizi güncelleştirmeleri sayısı.|Boyut yok|
|c2d.twin.update.failure|Arka uç başarısız ikizi güncelleştirmeleri|Count|Toplam|Tüm sayısı, arka uç başlatılan ikizi güncelleştirmeleri başarısız oldu.|Boyut yok|
|c2d.twin.update.size|Arka uç ikizi güncelleştirmeleri boyutu|Bayt|Average|Arka uç başlatılan ortalama, en düşük ve en büyük boyutu başarılı olan tüm güncelleştirmeleri çifti.|Boyut yok|
|twinQueries.success|Başarılı çifti sorguları|Count|Toplam|Tüm başarılı ikizi sorgularının sayısı.|Boyut yok|
|twinQueries.failure|Başarısız çifti sorguları|Count|Toplam|Tüm başarısız ikizi sorgularının sayısı.|Boyut yok|
|twinQueries.resultSize|İkiz sorgu sonucu boyutu|Bayt|Average|Ortalama, en düşük ve en fazla sonuç boyutunun tüm başarılı çifti sorguları.|Boyut yok|
|jobs.createTwinUpdateJob.success|İkiz güncelleştirmesi işlerinin başarılı oluşturma|Count|Toplam|Tüm oluşturma başarılı ikiz güncelleştirmesi işlerin sayısı.|Boyut yok|
|jobs.createTwinUpdateJob.failure|İkiz güncelleştirmesi işlerinin başarısız oluşturma|Count|Toplam|İkiz güncelleştirmesi işlerinin başarısız olan tüm oluşturma sayısı.|Boyut yok|
|jobs.createDirectMethodJob.success|Yöntem çağırma işlerinin başarılı oluşturma|Count|Toplam|Tüm oluşturma başarılı doğrudan yöntem çağırma işlerin sayısı.|Boyut yok|
|jobs.createDirectMethodJob.failure|Yöntem çağırma işlerinin başarısız oluşturma|Count|Toplam|Tüm başarısız oluşturma doğrudan yöntem çağırma işlerin sayısı.|Boyut yok|
|jobs.listJobs.success|Başarılı çağrılar listeleyemeyeceksiniz|Count|Toplam|Tüm başarılı çağrı listeleyemeyeceksiniz sayısı.|Boyut yok|
|jobs.listJobs.failure|Başarısız çağrıları listeleyemeyeceksiniz|Count|Toplam|Tüm başarısız çağrılar listeleyemeyeceksiniz sayısı.|Boyut yok|
|jobs.cancelJob.success|Başarılı iş iptalleri|Count|Toplam|Bir işi iptal etmek için tüm başarılı çağrı sayısı.|Boyut yok|
|jobs.cancelJob.failure|Başarısız iş iptalleri|Count|Toplam|Bir işi iptal etmek için tüm başarısız çağrıların sayısı.|Boyut yok|
|jobs.queryJobs.success|İş başarılı sorguları|Count|Toplam|Sorgu işleri yapılan tüm başarılı çağrı sayısı.|Boyut yok|
|jobs.queryJobs.failure|İş sorgularının başarısız oldu|Count|Toplam|Tüm başarısız çağrılar sorgu iş sayısı.|Boyut yok|
|Jobs.Completed|Tamamlanan İşler|Count|Toplam|Tüm tamamlanmış işlerin sayısı.|Boyut yok|
|Jobs.Failed|Başarısız olan işler|Count|Toplam|Başarısız olan tüm işleri sayısı.|Boyut yok|
|d2c.telemetry.ingress.sendThrottle|Azaltma hatalarının sayısındaki|Count|Toplam|Cihaz işleme nedeniyle azaltma hatalarının sayısındaki kısıtlar|Boyut yok|
|dailyMessageQuotaUsed|Kullanılan iletilerinin toplam sayısı|Count|Average|Günümüzde kullanılan toplam ileti sayısı. Bu, sıfırlanır, toplu bir değerdir, her gün 00:00 UTC.|Boyut yok|
|deviceDataUsage|Toplam cihaz veri kullanımı|Bayt|Toplam|Iothub için bağlı tüm cihazlara giden ve gelen aktarılan bayt|Boyut yok|
|totalDeviceCount|Toplam cihaz sayısı (Önizleme)|Count|Average|IOT hub'ınıza kayıtlı cihaz sayısı|Boyut yok|
|connectedDeviceCount|Bağlı cihazlar (Önizleme)|Count|Average|IOT hub'ınıza bağlı cihazların sayısı|Boyut yok|
|Yapılandırmaları|Ölçümleri yapılandırma|Count|Toplam|Yapılandırma işlemleri için ölçümleri|Boyut yok|

## <a name="microsoftdevicesprovisioningservices"></a>Microsoft.Devices/provisioningServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|RegistrationAttempts|Kayıt denemesi|Count|Toplam|Cihaz kayıtları denemesi sayısı|ProvisioningServiceName, IotHubName, Status|
|DeviceAssignments|Atanan cihazlar|Count|Toplam|Bir IOT hub'ına atanan cihaz sayısı|ProvisioningServiceName, IotHubName|
|AttestationAttempts|Kanıtlama denemeleri|Count|Toplam|Cihaz karşıladığımızı denemesi sayısı|ProvisioningServiceName, durumu, Protokolü|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.DocumentDB/databaseAccounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|AvailableStorage|Kullanılabilir depolama alanı|Bayt|Toplam|5 dakikada bir ayrıntı düzeyinde bildirilen toplam kullanılabilir depolama alanı|CollectionName, DatabaseName, bölge|
|CassandraConnectionClosures|Cassandra bağlantı kapanışlar|Count|Toplam|1 dakika ayrıntı düzeyinde bildirilen kapatılan Cassandra bağlantı sayısı|Bölge, ClosureReason|
|CassandraRequestCharges|Cassandra isteği ücretleri|Count|Toplam|Cassandra isteklerini için tüketilen ru|DatabaseName, CollectionName, bölge, OperationType, kaynak türü|
|CassandraRequests|Cassandra istekleri|Count|Count|Cassandra istekleri sayısı|DatabaseName, CollectionName, bölge, OperationType, kaynak türü, hata kodu|
|DataUsage|Veri Kullanımı|Bayt|Toplam|5 dakikada bir ayrıntı düzeyinde bildirilen toplam veri kullanımı|CollectionName, DatabaseName, bölge|
|DocumentCount|Belge sayısı|Count|Toplam|5 dakikada bir ayrıntı düzeyinde bildirilen toplam belge sayısı|CollectionName, DatabaseName, bölge|
|DocumentQuota|Belge kota|Bayt|Toplam|Toplam depolama kotasını 5 dakikada bir ayrıntı düzeyinde bildirilen|CollectionName, DatabaseName, bölge|
|IndexUsage|Dizin kullanımı|Bayt|Toplam|5 dakikada bir ayrıntı düzeyinde bildirilen toplam dizin kullanımı|CollectionName, DatabaseName, bölge|
|MetadataRequests|Meta veri isteği|Count|Count|Meta veri isteği sayısı. Cosmos DB koleksiyonları, veritabanları vb. listeleme olanak tanır, her hesap için sistem meta veri koleksiyonu tutar ve bunların yapılandırmalarının ücretsiz.|DatabaseName, CollectionName, bölge, StatusCode, |
|MongoRequestCharge|Mongo istek yükü|Count|Toplam|Tüketilen mongo istek birimleri|DatabaseName CollectionName, bölgesi CommandName, hata kodu|
|MongoRequests|Mongo istekleri|Count|Count|Mongo istekleri sayısı|DatabaseName CollectionName, bölgesi CommandName, hata kodu|
|ProvisionedThroughput|Sağlanan Aktarım Hızı|Count|Maksimum|Sağlanan Aktarım Hızı|DatabaseName, CollectionName|
|ReplicationLatency|P99 Çoğaltma gecikmesi|Milisaniye|Average|Kaynak ve hedef bölgede coğrafi özellikli hesabının P99 çoğaltma gecikmesi|SourceRegion, TargetRegion|
|ServiceAvailability|Hizmet kullanılabilirliği|Percent|Average|Hesap istekleri kullanılabilirliğine bir saat, gün veya ay ayrıntı düzeyi|Boyut yok|
|TotalRequestUnits|Toplam istek birimleri|Count|Toplam|Tüketilen birimler iste|DatabaseName, CollectionName, bölge, StatusCode, OperationType|
|TotalRequests|Toplam İstek Sayısı|Count|Count|Yapılan istek sayısı|DatabaseName, CollectionName, bölge, StatusCode, OperationType|

## <a name="microsofteventgridtopics"></a>Microsoft.EventGrid/topics

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|PublishSuccessCount|Yayımlanan olaylar|Count|Toplam|Bu konu başlığında yayımlanan toplam olay sayısı|Boyut yok|
|PublishFailCount|Başarısız olayları yayımlama|Count|Toplam|Bu konuya yayımlanamadı toplam olay sayısı|ErrorType, hata|
|UnmatchedEventCount|Eşleşmeyen olay|Count|Toplam|Bu konu için olay aboneliklerden herhangi birine eşleşmeyen toplam olay sayısı|Boyut yok|
|PublishSuccessLatencyInMs|Başarı gecikme süresi yayımlama|Count|Toplam|Başarı gecikme süresi, milisaniye cinsinden yayımlama|Boyut yok|

## <a name="microsofteventgrideventsubscriptions"></a>Microsoft.EventGrid/eventSubscriptions

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|MatchedEventCount|Eşleşen olaylar|Count|Toplam|Bu olay aboneliğine eşleşen toplam olay sayısı|Boyut yok|
|DeliveryAttemptFailCount|Olayları teslim başarısız|Count|Toplam|Toplam olay sayısı olay aboneliği için teslim edilemedi|Hata, ErrorType|
|DeliverySuccessCount|Teslim edilen olaylar|Count|Toplam|Bu olay aboneliğine teslim toplam olay sayısı|Boyut yok|
|DestinationProcessingDurationInMs|Hedef işleme süresi|Milisaniye|Average|Hedef işleme süresinin milisaniye cinsinden|Boyut yok|
|DroppedEventCount|Bırakılan olayları|Count|Toplam|Bu olay aboneliğine eşleşen toplam bırakılan olayları|DropReason|
|DeadLetteredCount|Geçerliliğini yitirmiş olayları|Count|Toplam|Bu olay aboneliğine eşleşen toplam geçerliliğini yitirmiş olayları|DeadLetterReason|

## <a name="microsofteventgridextensiontopics"></a>Microsoft.EventGrid/extensionTopics

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|PublishSuccessCount|Yayımlanan olaylar|Count|Toplam|Bu konu başlığında yayımlanan toplam olay sayısı|Boyut yok|
|PublishFailCount|Başarısız olaylar|Count|Toplam|Bu konuya yayımlanamadı toplam olay sayısı|ErrorType, hata|
|UnmatchedEventCount|Eşleşmeyen olay|Count|Toplam|Bu konu için olay aboneliklerden herhangi birine eşleşmeyen toplam olay sayısı|Boyut yok|
|PublishSuccessLatencyInMs|Başarı gecikme süresi yayımlama|Count|Toplam|Başarı gecikme süresi, milisaniye cinsinden yayımlama|Boyut yok|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SuccessfulRequests|Başarılı istekler|Count|Toplam|Microsoft.EventHub için başarılı istekler.|EntityName, |
|ServerErrors|Sunucu hataları.|Count|Toplam|Microsoft.EventHub için sunucu hataları.|EntityName, |
|UserErrors|Kullanıcı hataları.|Count|Toplam|Microsoft.EventHub için kullanıcı hataları.|EntityName, |
|QuotaExceededErrors|Kota aşım hataları.|Count|Toplam|Microsoft.EventHub için kota aşım hataları.|EntityName, |
|ThrottledRequests|Daraltılmış istekler.|Count|Toplam|Microsoft.EventHub için daraltılmış istekler.|EntityName, |
|IncomingRequests|Gelen istekler|Count|Toplam|Microsoft.EventHub için gelen istekler.|EntityName|
|IncomingMessages|Gelen iletiler|Count|Toplam|Microsoft.EventHub için gelen iletiler.|EntityName|
|OutgoingMessages|Giden iletiler|Count|Toplam|Microsoft.eventhub giden iletiler.|EntityName|
|IncomingBytes|Gelen bayt sayısı.|Bayt|Toplam|Microsoft.EventHub için gelen baytlar.|EntityName|
|OutgoingBytes|Giden bayt sayısı.|Bayt|Toplam|Microsoft.EventHub için giden baytlar.|EntityName|
|ActiveConnections|ActiveConnections|Count|Average|Microsoft.EventHub için toplam etkin bağlantılar.|Boyut yok|
|ConnectionsOpened|Açılan bağlantılar.|Count|Average|Microsoft.EventHub için açılan bağlantılar.|EntityName|
|ConnectionsClosed|Kapatılan bağlantılar.|Count|Average|Microsoft.EventHub için kapatılan bağlantılar.|EntityName|
|CaptureBacklog|Kapsam yakalayın.|Count|Toplam|Microsoft.EventHub için kapsam yakalayın.|EntityName|
|CapturedMessages|Yakalanan iletiler.|Count|Toplam|Microsoft.EventHub için yakalanan iletiler.|EntityName|
|CapturedBytes|Yakalanan baytlar.|Bayt|Toplam|Microsoft.EventHub için yakalanan baytlar.|EntityName|
|Size|Size|Bayt|Average|EventHub bayt cinsinden boyutu.|EntityName|
|INREQS|(Kullanım dışı) gelen istekleri|Count|Toplam|(Kullanım dışı) bir ad alanı için toplam gelen gönderme isteği|Boyut yok|
|SUCCREQ|Başarılı istekler (kullanım dışı)|Count|Toplam|(Kullanım dışı) bir ad alanı için toplam başarılı istek|Boyut yok|
|FAILREQ|Başarısız istekler (kullanım dışı)|Count|Toplam|(Kullanım dışı) bir ad alanı için toplam başarısız istek|Boyut yok|
|SVRBSY|Sunucu meşgul hataları (kullanım dışı)|Count|Toplam|(Kullanım dışı) bir ad alanı için toplam sunucu meşgul hataları|Boyut yok|
|INTERR|İç sunucu hataları (kullanım dışı)|Count|Toplam|(Kullanım dışı) bir ad alanı için toplam iç sunucu hataları|Boyut yok|
|MISCERR|(Kullanım dışı) diğer hatalar|Count|Toplam|(Kullanım dışı) bir ad alanı için toplam başarısız istek|Boyut yok|
|INMSGS|(Kullanım dışı) gelen iletiler (kullanım dışı)|Count|Toplam|Bir ad alanı için toplam gelen iletiler. Bu ölçüm kullanımdan kaldırılmıştır. Lütfen bunun yerine gelen iletiler ölçümünü kullanın (kullanım dışı)|Boyut yok|
|EHINMSGS|Gelen iletiler (kullanım dışı)|Count|Toplam|(Kullanım dışı) bir ad alanı için toplam gelen ileti sayısı|Boyut yok|
|OUTMSGS|(Kullanım dışı) giden iletiler (kullanım dışı)|Count|Toplam|Toplam giden iletileri için bir ad alanı. Bu ölçüm kullanımdan kaldırılmıştır. Lütfen bunun yerine giden iletiler ölçümünü kullanın (kullanım dışı)|Boyut yok|
|EHOUTMSGS|Giden iletiler (kullanım dışı)|Count|Toplam|Toplam Giden iletiler için bir ad alanı (kullanım dışı)|Boyut yok|
|EHINMBS|Gelen bayt (kullanım dışı) (kullanım dışı)|Bayt|Toplam|Olay hub'ı gelen ileti aktarım hızı için bir ad alanı. Bu ölçüm kullanımdan kaldırılmıştır. Lütfen bunun yerine gelen bayt ölçümünü kullanın (kullanım dışı)|Boyut yok|
|EHINBYTES|Gelen bayt (kullanım dışı)|Bayt|Toplam|Olay hub'ı gelen ileti aktarım hızı için bir ad alanı (kullanım dışı)|Boyut yok|
|EHOUTMBS|Giden bayt (kullanım dışı) (kullanım dışı)|Bayt|Toplam|Olay hub'ı giden ileti aktarım hızı için bir ad alanı. Bu ölçüm kullanımdan kaldırılmıştır. Lütfen bunun yerine Giden bayt ölçümünü kullanın (kullanım dışı)|Boyut yok|
|EHOUTBYTES|Giden bayt (kullanım dışı)|Bayt|Toplam|Olay hub'ı giden ileti işleme için bir ad alanı (kullanım dışı)|Boyut yok|
|EHABL|Kapsam mesajlarını arşivle (kullanım dışı)|Count|Toplam|Olay hub'ı arşiv iletileri biriktirme listesi için bir ad alanı (kullanım dışı)|Boyut yok|
|EHAMSGS|Arşiv iletiler (kullanım dışı)|Count|Toplam|İletiler (kullanım dışı) ad alanında olay hub'ı arşivlenmiş|Boyut yok|
|EHAMBS|Mesaj işlemelerini arşivle (kullanım dışı)|Bayt|Toplam|(Kullanım dışı) ad alanında olay hub'ı arşivlenen ileti işleme hızı|Boyut yok|

## <a name="microsofteventhubclusters"></a>Microsoft.EventHub/clusters

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SuccessfulRequests|Başarılı istekler (Önizleme)|Count|Toplam|Microsoft.EventHub için başarılı istekler. (Önizleme)|Boyut yok|
|ServerErrors|Sunucu hataları. (Önizleme)|Count|Toplam|Microsoft.EventHub için sunucu hataları. (Önizleme)|Boyut yok|
|UserErrors|Kullanıcı hataları. (Önizleme)|Count|Toplam|Microsoft.EventHub için kullanıcı hataları. (Önizleme)|Boyut yok|
|QuotaExceededErrors|Kota aşım hataları. (Önizleme)|Count|Toplam|Microsoft.EventHub için kota aşım hataları. (Önizleme)|Boyut yok|
|ThrottledRequests|Daraltılmış istekler. (Önizleme)|Count|Toplam|Microsoft.EventHub için daraltılmış istekler. (Önizleme)|Boyut yok|
|IncomingRequests|Gelen istekler (Önizleme)|Count|Toplam|Microsoft.EventHub için gelen istekler. (Önizleme)|Boyut yok|
|IncomingMessages|Gelen iletiler (Önizleme)|Count|Toplam|Microsoft.EventHub için gelen iletiler. (Önizleme)|Boyut yok|
|OutgoingMessages|Giden iletiler (Önizleme)|Count|Toplam|Microsoft.eventhub giden iletiler. (Önizleme)|Boyut yok|
|IncomingBytes|Gelen bayt sayısı. (Önizleme)|Bayt|Toplam|Microsoft.EventHub için gelen baytlar. (Önizleme)|Boyut yok|
|OutgoingBytes|Giden bayt sayısı. (Önizleme)|Bayt|Toplam|Microsoft.EventHub için giden baytlar. (Önizleme)|Boyut yok|
|ActiveConnections|ActiveConnections (Önizleme)|Count|Average|Microsoft.EventHub için toplam etkin bağlantılar. (Önizleme)|Boyut yok|
|ConnectionsOpened|Açılan bağlantılar. (Önizleme)|Count|Average|Microsoft.EventHub için açılan bağlantılar. (Önizleme)|Boyut yok|
|ConnectionsClosed|Kapatılan bağlantılar. (Önizleme)|Count|Average|Microsoft.EventHub için kapatılan bağlantılar. (Önizleme)|Boyut yok|
|CaptureBacklog|Kapsam yakalayın. (Önizleme)|Count|Toplam|Microsoft.EventHub için kapsam yakalayın. (Önizleme)|Boyut yok|
|CapturedMessages|Yakalanan iletiler. (Önizleme)|Count|Toplam|Microsoft.EventHub için yakalanan iletiler. (Önizleme)|Boyut yok|
|CapturedBytes|Yakalanan baytlar. (Önizleme)|Bayt|Toplam|Microsoft.EventHub için yakalanan baytlar. (Önizleme)|Boyut yok|
|CPU|CPU (Önizleme)|Percent|Maksimum|Yüzde olarak olay hub'ı kümesinin CPU kullanımı|Role|
|AvailableMemory|Kullanılabilir bellek (Önizleme)|Count|Maksimum|Bayt cinsinden olay hub'ı küme için kullanılabilir bellek|Role|

## <a name="microsofthdinsightclusters"></a>Microsoft.HDInsight/clusters

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|GatewayRequests|Ağ geçidi istekleri|Count|Toplam|Ağ geçidi istekleri sayısı|ClusterDnsName, HttpStatus|
|CategorizedGatewayRequests|Kategorilere ayrılmış bir ağ geçidi istekleri|Count|Toplam|Ağ geçidi istekleri (1xx/2xx/3xx/4xx/5xx) kategorilere göre sayısı|ClusterDnsName, HttpStatus|
|NumActiveWorkers|Etkin çalışan sayısı|Count|Maksimum|Etkin çalışan sayısı|ClusterDnsName, MetricName|

## <a name="microsoftinsightsautoscalesettings"></a>Microsoft.Insights/AutoscaleSettings

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ObservedMetricValue|Gözlenen ölçüm değeri|Count|Average|Otomatik ölçeklendirme yürütüldüğünde hesaplanan değer|MetricTriggerSource|
|MetricThreshold|Ölçüm eşiği|Count|Average|Otomatik ölçeklendirme çalıştırıldığında yapılandırılan otomatik ölçek eşiği.|MetricTriggerRule|
|ObservedCapacity|Gözlenen kapasite|Count|Average|Yürütüldüğünde kapasite için otomatik ölçeklendirme bildirdi.|Boyut yok|
|ScaleActionsInitiated|Başlatılan ölçeklendirme eylemleri|Count|Toplam|Ölçeklendirme işleminin yönü.|ScaleDirection|

## <a name="microsoftinsightscomponents"></a>Microsoft.Insights/Components

(Genel Önizleme)

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|availabilityResults/availabilityPercentage|Kullanılabilirlik|Yüzde|Average|Başarıyla tamamlanan kullanılabilirlik testleri yüzdesi|availabilityResult/adı availabilityResult/konum|
|availabilityResults/sayısı|Kullanılabilirlik testleri|Count|Count|Kullanılabilirlik testi sayısı|availabilityResult/ad, availabilityResult/konum, availabilityResult/başarılı|
|availabilityResults/süresi|Kullanılabilirlik testinin süresi|Milisaniye|Average|Kullanılabilirlik testinin süresi|availabilityResult/ad, availabilityResult/konum, availabilityResult/başarılı|
|browserTimings/networkDuration|Sayfa yükleme ağ bağlantı süresi|Milisaniye|Average|Kullanıcı isteği ve ağ bağlantı arasındaki süre. DNS aramasını ve aktarım bağlantısını içerir.|Boyut yok|
|browserTimings/processingDuration|İstemci işlem süresi|Milisaniye|Average|DOM'un yüklenmesi arasında bir belgenin son bayt alma süresi. Zaman uyumsuz istekler hala işleniyor.|Boyut yok|
|browserTimings/receiveDuration|Yanıt süresi alınıyor|Milisaniye|Average|Saat arasında ilk ve son bayt veya bağlantının kesilmesi.|Boyut yok|
|browserTimings/sendDuration|İstek gönderme süresi|Milisaniye|Average|Ağ bağlantısı ve ilk baytı alma arasındaki süre.|Boyut yok|
|browserTimings/totalDuration|Tarayıcı sayfa yükleme süresi|Milisaniye|Average|Kullanıcı isteğinden DOM, stil sayfaları, betikler ve resimler yüklenene kadar süre.|Boyut yok|
|bağımlılıkları/sayısı|Bağımlılık çağrıları|Count|Count|Uygulama tarafından dış kaynaklara yapılan çağrıların sayısı.|Bağımlılık/türü, bağımlılık/performanceBucket, bağımlılık/başarı, işlemi/Sentetik, bulut/Roleınstance, bulut/roleName|
|bağımlılıkları/süresi|Bağımlılık süresi|Milisaniye|Average|Uygulama tarafından dış kaynaklara yapılan çağrıların süresi.|Bağımlılık/türü, bağımlılık/performanceBucket, bağımlılık/başarı, işlemi/Sentetik, bulut/Roleınstance, bulut/roleName|
|bağımlılıkları/başarısız oldu|Bağımlılık çağrısı hataları|Count|Count|Uygulama tarafından dış kaynaklara yapılan başarısız bağımlılık çağrılarının sayısı.|Bağımlılık/türü, bağımlılık/performanceBucket, işlemi/Sentetik, bulut/Roleınstance, bulut/roleName|
|Sayfa görüntülemesi/sayısı|Sayfa görüntülemeleri|Count|Count|Sayfa görüntüleme sayısı.|işlem/Sentetik|
|Sayfa görüntülemesi/süresi|Sayfa görünümü yükleme süresi|Milisaniye|Average|Sayfa görünümü yükleme süresi|işlem/Sentetik|
|performanceCounters/requestExecutionTime|HTTP isteği yürütme süresi|Milisaniye|Average|En son isteği yürütme süresi.|Bulut/Roleınstance|
|performanceCounters/requestsInQueue|Uygulama kuyruğundaki HTTP istekleri|Count|Average|Uygulama istek kuyruğunun uzunluğu.|Bulut/Roleınstance|
|performanceCounters/requestsPerSecond|HTTP isteği oranı|CountPerSecond|Average|ASP.net'ten saniyede uygulamaya yapılan tüm isteklerin oranı.|Bulut/Roleınstance|
|performanceCounters/exceptionsPerSecond|özel durum oranı|CountPerSecond|Average|Windows .NET özel durumları ve .NET özel durumlarına dönüştürülmüş yönetilmeyen özel durumlar dahil olmak üzere, raporlanan, işlenen ve yakalanamayan özel durum sayısı.|Bulut/Roleınstance|
|performanceCounters/processIOBytesPerSecond|İşlem GÇ hızı|BytesPerSecond|Average|Saniyede okunan ve dosyaları, ağ ve cihazlar için yazılan toplam bayt sayısı.|Bulut/Roleınstance|
|performanceCounters/processCpuPercentage|İşlem CPU'su|Percent|Average|Tüm işlem iş parçacıklarının sürenin yüzdesini işlemci yönergeleri yürütmek için kullanılır. Bu, 0 ile 100 arasında değişebilir. Bu ölçüm yalnızca w3wp işleminin performansını gösterir.|Bulut/Roleınstance|
|performanceCounters/processorCpuPercentage|İşlemci zamanı|Percent|Average|İşlemcinin boşta olmayan iş parçacıklarında geçirdiği sürenin yüzdesi.|Bulut/Roleınstance|
|performanceCounters/memoryAvailableBytes|Kullanılabilir bellek|Bayt|Average|Bir işlem veya sistem kullanımı için hemen kullanılabilir fiziksel bellek.|Bulut/Roleınstance|
|performanceCounters/processPrivateBytes|İşleme özel bayt sayısı|Bayt|Average|İzlenen uygulama işlemleri için özel olarak atanan bellek.|Bulut/Roleınstance|
|istekleri/süresi|Sunucu yanıt süresi|Milisaniye|Average|Bir HTTP isteğinin alınmasıyla yanıtın gönderilmesi tamamlama arasındaki süre.|İstek/performanceBucket, istek/resultCode, işlemi/Sentetik, bulut/Roleınstance, istek/başarı, bulut/roleName|
|isteği/sayısı|Sunucu istekleri|Count|Count|Tamamlanan HTTP isteği sayısı.|İstek/performanceBucket, istek/resultCode, işlemi/Sentetik, bulut/Roleınstance, istek/başarı, bulut/roleName|
|/ başarısız istekleri|Başarısız istekler|Count|Count|HTTP isteği sayısı, başarısız olarak işaretlemiş. Çoğu durumda bunlar isteği bir yanıt koduna sahip > 400 veya 401 eşit değildir.|İstek/performanceBucket, istek/resultCode, işlemi/Sentetik, bulut/Roleınstance, bulut/roleName|
|İstek/oranı|Sunucu isteği oranı|CountPerSecond|Average|Sunucu isteklerinin Saniyedeki oranı|İstek/performanceBucket, istek/resultCode, işlemi/Sentetik, bulut/Roleınstance, istek/başarı, bulut/roleName|
|özel durumlar/sayısı|Özel durumlar|Count|Count|Tüm Yakalanmayan Özel durumların birleştirilmiş sayısı.|Bulut/roleName, bulut/Roleınstance, istemci/türü|
|özel durumlar/tarayıcı|Tarayıcı özel durumları|Count|Count|Tarayıcıda oluşan Yakalanmayan Özel durum sayısı.|Boyut yok|
|özel durumlar/sunucu|Sunucu özel durumları|Count|Count|Sunucu uygulamasında oluşan Yakalanmayan Özel durum sayısı.|Bulut/roleName, bulut/Roleınstance|
|izlemeleri/sayısı|izlemeleri|Count|Count|İzleme belgesi sayısı|İzleme/Err, işlemi/Sentetik, bulut/roleName, bulut/Roleınstance|

## <a name="microsoftkeyvaultvaults"></a>Microsoft.KeyVault/vaults

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ServiceApiHit|Toplam hizmet API'si isabetlerinin|Count|Count|Toplam sayısı hizmet API'si isabetlerinin|ActivityType, ActivityName|
|ServiceApiLatency|Hizmet API'si toplam gecikme|Milisaniye|Average|Toplam gecikme süresi hizmet API'si isteklerinin|ActivityType, ActivityName, StatusCode|
|ServiceApiResult|Hizmet API'si toplam sonuç|Count|Count|Toplam sayısı hizmet API'si sonuçlarının|ActivityType, ActivityName, StatusCode|

## <a name="microsoftkustoclusters"></a>Microsoft.Kusto/Clusters

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CacheUtilization|Önbellek kullanımı|Percent|Average|Küme kapsamında kullanım düzeyi|Yok.|
|QueryDuration|Sorgu süresi|Milisaniye|Average|Saniye cinsinden sorgu süresi|Sorgu durumu|
|IngestionUtilization|Alımı kullanımı|Percent|Average|Kümede kullanılan alma yuvası oranı|Yok.|
|KeepAlive|Canlı|Count|Average|Küme sorgulara yanıt sağlamlık denetimi gösterir.|Yok.|
|IngestionVolumeInMB|Alma birim (MB cinsinden)|Count|Toplam|Genel (MB cinsinden) kümeye içe alınan veri hacmi|Database|
|IngestionLatencyInSeconds|Alma gecikmesi (saniye cinsinden)|Saniye|Average|Alma süresi (örneğin ileti içinde EventHub) kaynak kümeye saniye|Yok.|
|EventProcessedForEventHubs|(Event Hubs için) işlenen olaylar|Count|Toplam|Olay Hub'ından başlayan kümeniz, küme tarafından işlenen olay sayısı|Yok.|
|IngestionResult|Alma sonucu|Count|Count|Alma işlemi sayısı|Durum|
|CPU|CPU|Percent|Average|CPU kullanımı düzeyi|Yok.|
| ContinuousExportNumOfRecordsExported | Sürekli dışarı aktarma dışarı aktarılan kayıt sayısı | Count | Toplam | Dışa aktarma işlemi sırasında yazılan her depolama yapıt için verilen kayıt sayısı  | Yok. |
| ExportUtilization | Dışarı aktarma kullanımı | Percent | Maksimum | Dışarı aktarma kullanımı | Yok. |
| ContinuousExportPendingCount | Sürekli dışarı aktarma bekleyen sayısı | Count | Maksimum | İş yürütme için hazır bekleyen sürekli sayısı dışarı aktarma | Yok. |
| ContinuousExportMaxLatenessMinutes | Sürekli dışarı aktarma en fazla Lateness dakika | Count | Maksimum | Bekleyen ve yürütme için hazır olan en fazla süreyi dakika cinsinden, tüm sürekli dışarı aktarma | Yok. |

## <a name="microsoftlocationbasedservicesaccounts"></a>Microsoft.LocationBasedServices/accounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Kullanım|Kullanım|Count|Count|Sayısı, API çağrıları|ApiCategory, ApiName|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|RunsStarted|Başlatılan çalıştırmalar|Count|Toplam|Başlatılan iş akışı sayısı çalıştırır.|Boyut yok|
|RunsCompleted|Tamamlanan çalıştırmalar|Count|Toplam|Tamamlanan iş akışı sayısı çalıştırır.|Boyut yok|
|RunsSucceeded|Başarılı çalıştırmalar|Count|Toplam|İş akışı başarılı çalıştırma sayısı.|Boyut yok|
|RunsFailed|Başarısız çalıştırmalar|Count|Toplam|İş akışı başarısız çalıştırma sayısı.|Boyut yok|
|RunsCancelled|İptal edilen çalıştırmalar|Count|Toplam|İş akışı iptal edilmiş çalıştırma sayısı.|Boyut yok|
|RunLatency|Çalıştırma gecikmesi|Saniye|Average|Gecikme süresi tamamlanan iş akışı çalıştırır.|Boyut yok|
|RunSuccessLatency|Başarılı çalıştırma gecikmesi|Saniye|Average|Gecikme süresi başarılı iş akışı çalıştırır.|Boyut yok|
|RunThrottledEvents|Çalıştırılması kısıtlanan olaylar|Count|Toplam|Sayısı, iş akışı eylemi veya tetikleyiciyle kısıtlanan olay.|Boyut yok|
|RunFailurePercentage|Çalıştırma hatası yüzdesi|Percent|Toplam|Başarısız iş akışı yüzdesini çalıştırır.|Boyut yok|
|ActionsStarted|Başlatılan Eylemler |Count|Toplam|Başlatılan iş akışı eylemi sayısı.|Boyut yok|
|ActionsCompleted|Tamamlanan Eylemler |Count|Toplam|Tamamlanan iş akışı eylemi sayısı.|Boyut yok|
|ActionsSucceeded|Başarılı Eylemler |Count|Toplam|İş akışı eylemi sayısı başarılı oldu.|Boyut yok|
|ActionsFailed|Başarısız Eylemler|Count|Toplam|Başarısız iş akışı eylemi sayısı.|Boyut yok|
|ActionsSkipped|Atlanan Eylemler |Count|Toplam|Atlanan iş akışı eylemi sayısı.|Boyut yok|
|ActionLatency|Eylem gecikmesi |Saniye|Average|Tamamlanan iş akışı eylemlerinin gecikme süresi.|Boyut yok|
|ActionSuccessLatency|Başarılı eylem gecikmesi |Saniye|Average|Başarılı iş akışı eylemlerinin gecikme süresi.|Boyut yok|
|ActionThrottledEvents|Eylemi kısıtlanan olaylar|Count|Toplam|Kısıtlanan olay iş akışı eylemi sayısı...|Boyut yok|
|TriggersStarted|Başlatılan Tetikleyiciler |Count|Toplam|Başlatılan iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersCompleted|Tamamlanan Tetikleyiciler |Count|Toplam|Tamamlanan iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersSucceeded|Başarılı olan Tetikleyiciler |Count|Toplam|İş akışı tetikleyicisi sayısı başarılı oldu.|Boyut yok|
|TriggersFailed|Başarısız olan Tetikleyiciler |Count|Toplam|Başarısız iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersSkipped|Atlanan Tetikleyiciler|Count|Toplam|Atlanan iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersFired|Tetiklenen Tetikleyiciler |Count|Toplam|Harekete geçirilen iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggerLatency|Tetikleyici gecikme süresi |Saniye|Average|Tamamlanan iş akışı tetikleyicilerinin gecikmesi.|Boyut yok|
|TriggerFireLatency|Tetikleyici tetikleme gecikme süresi |Saniye|Average|Başlatılan iş akışı tetikleyicilerinin gecikmesi.|Boyut yok|
|TriggerSuccessLatency|Tetikleyici başarı gecikme süresi |Saniye|Average|Başarılı iş akışı tetikleyicilerinin gecikmesi.|Boyut yok|
|TriggerThrottledEvents|Tetikleyici kısıtlanan olaylar|Count|Toplam|Kısıtlanan olay iş akışı tetikleyicisi sayısı.|Boyut yok|
|BillableActionExecutions|Faturalanabilir eylem yürütmeleri|Count|Toplam|Faturalandırılan iş akışı eylemi yürütmelerinin sayısı.|Boyut yok|
|BillableTriggerExecutions|Faturalanabilir tetikleyici yürütmeleri|Count|Toplam|Faturalandırılan iş akışı tetikleyicisi yürütmelerinin sayısı.|Boyut yok|
|TotalBillableExecutions|Toplam Faturalanabilir yürütme sayısı|Count|Toplam|Faturalandırılan iş akışı yürütmelerinin sayısı.|Boyut yok|
|BillingUsageNativeOperation|Yerel işlem yürütmeler için fatura kullanım|Count|Toplam|Faturalandırılan yerel işlem yürütmelerinin sayısı.|Boyut yok|
|BillingUsageStandardConnector|Standart bağlayıcı yürütmeleri için fatura kullanım|Count|Toplam|Faturalandırılan standart bağlayıcı yürütmelerinin sayısı.|Boyut yok|
|BillingUsageStorageConsumption|Depolama tüketim yürütmeler için fatura kullanım|Count|Toplam|Faturalandırılan depolama tüketimi yürütmelerinin sayısı.|Boyut yok|
|BillingUsageNativeOperation|Yerel işlem yürütmeler için fatura kullanım|Count|Toplam|Faturalandırılan yerel işlem yürütmelerinin sayısı.|Boyut yok|
|BillingUsageStandardConnector|Standart bağlayıcı yürütmeleri için fatura kullanım|Count|Toplam|Faturalandırılan standart bağlayıcı yürütmelerinin sayısı.|Boyut yok|
|BillingUsageStorageConsumption|Depolama tüketim yürütmeler için fatura kullanım|Count|Toplam|Faturalandırılan depolama tüketimi yürütmelerinin sayısı.|Boyut yok|

## <a name="microsoftlogicintegrationserviceenvironments"></a>Microsoft.Logic/integrationServiceEnvironments

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|RunsStarted|Başlatılan çalıştırmalar|Count|Toplam|Başlatılan iş akışı sayısı çalıştırır.|Boyut yok|
|RunsCompleted|Tamamlanan çalıştırmalar|Count|Toplam|Tamamlanan iş akışı sayısı çalıştırır.|Boyut yok|
|RunsSucceeded|Başarılı çalıştırmalar|Count|Toplam|İş akışı başarılı çalıştırma sayısı.|Boyut yok|
|RunsFailed|Başarısız çalıştırmalar|Count|Toplam|İş akışı başarısız çalıştırma sayısı.|Boyut yok|
|RunsCancelled|İptal edilen çalıştırmalar|Count|Toplam|İş akışı iptal edilmiş çalıştırma sayısı.|Boyut yok|
|RunLatency|Çalıştırma gecikmesi|Saniye|Average|Gecikme süresi tamamlanan iş akışı çalıştırır.|Boyut yok|
|RunSuccessLatency|Başarılı çalıştırma gecikmesi|Saniye|Average|Gecikme süresi başarılı iş akışı çalıştırır.|Boyut yok|
|RunThrottledEvents|Çalıştırılması kısıtlanan olaylar|Count|Toplam|Sayısı, iş akışı eylemi veya tetikleyiciyle kısıtlanan olay.|Boyut yok|
|RunStartThrottledEvents|Çalıştırma başlangıç kısıtlanan olaylar|Count|Toplam|Kısıtlanan olay iş akışı başlatma çalıştırma sayısı.|Boyut yok|
|RunFailurePercentage|Çalıştırma hatası yüzdesi|Percent|Toplam|Başarısız iş akışı yüzdesini çalıştırır.|Boyut yok|
|ActionsStarted|Başlatılan Eylemler |Count|Toplam|Başlatılan iş akışı eylemi sayısı.|Boyut yok|
|ActionsCompleted|Tamamlanan Eylemler |Count|Toplam|Tamamlanan iş akışı eylemi sayısı.|Boyut yok|
|ActionsSucceeded|Başarılı Eylemler |Count|Toplam|İş akışı eylemi sayısı başarılı oldu.|Boyut yok|
|ActionsFailed|Başarısız Eylemler |Count|Toplam|Başarısız iş akışı eylemi sayısı.|Boyut yok|
|ActionsSkipped|Atlanan Eylemler |Count|Toplam|Atlanan iş akışı eylemi sayısı.|Boyut yok|
|ActionLatency|Eylem gecikmesi |Saniye|Average|Tamamlanan iş akışı eylemlerinin gecikme süresi.|Boyut yok|
|ActionSuccessLatency|Başarılı eylem gecikmesi |Saniye|Average|Başarılı iş akışı eylemlerinin gecikme süresi.|Boyut yok|
|ActionThrottledEvents|Eylemi kısıtlanan olaylar|Count|Toplam|Kısıtlanan olay iş akışı eylemi sayısı...|Boyut yok|
|TriggersStarted|Başlatılan Tetikleyiciler |Count|Toplam|Başlatılan iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersCompleted|Tamamlanan Tetikleyiciler |Count|Toplam|Tamamlanan iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersSucceeded|Başarılı olan Tetikleyiciler |Count|Toplam|İş akışı tetikleyicisi sayısı başarılı oldu.|Boyut yok|
|TriggersFailed|Başarısız olan Tetikleyiciler |Count|Toplam|Başarısız iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersSkipped|Atlanan Tetikleyiciler|Count|Toplam|Atlanan iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersFired|Tetiklenen Tetikleyiciler |Count|Toplam|Harekete geçirilen iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggerLatency|Tetikleyici gecikme süresi |Saniye|Average|Tamamlanan iş akışı tetikleyicilerinin gecikmesi.|Boyut yok|
|TriggerFireLatency|Tetikleyici tetikleme gecikme süresi |Saniye|Average|Başlatılan iş akışı tetikleyicilerinin gecikmesi.|Boyut yok|
|TriggerSuccessLatency|Tetikleyici başarı gecikme süresi |Saniye|Average|Başarılı iş akışı tetikleyicilerinin gecikmesi.|Boyut yok|
|TriggerThrottledEvents|Tetikleyici kısıtlanan olaylar|Count|Toplam|Kısıtlanan olay iş akışı tetikleyicisi sayısı.|Boyut yok|
|IntegrationServiceEnvironmentWorkflowProcessorUsage|İş akışı tümleştirmesi hizmeti ortamı için işlemci kullanımı|Percent|Average|Tümleştirme hizmeti ortamı için iş akışı işlemci kullanımı.|Boyut yok|
|IntegrationServiceEnvironmentWorkflowMemoryUsage|İş akışı tümleştirmesi hizmeti ortamı için bellek kullanımı|Percent|Average|Tümleştirme hizmeti ortamı için iş akışı bellek kullanımı.|Boyut yok|
|IntegrationServiceEnvironmentConnectorProcessorUsage|Tümleştirme hizmeti ortamı için bağlayıcı işlemci kullanımı|Percent|Average|Tümleştirme hizmeti ortamı için bağlayıcı işlemci kullanımı.|Boyut yok|
|IntegrationServiceEnvironmentConnectorMemoryUsage|Tümleştirme hizmeti ortamı için bağlayıcı bellek kullanımı|Percent|Average|Tümleştirme hizmeti ortamı için bağlayıcı bellek kullanımı.|Boyut yok|

## <a name="microsoftmachinelearningservicesworkspaces"></a>Microsoft.MachineLearningServices/workspaces

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Tamamlanan çalıştırmalar|Tamamlanan çalıştırmalar|Count|Toplam|Bu çalışma alanı başarıyla için tamamlanan çalıştırma sayısı|Senaryo|
|Başlatılan çalıştırmalar|Başlatılan çalıştırmalar|Count|Toplam|Bu çalışma alanı için başlatılan çalıştırma sayısı|Senaryo|
|Başarısız çalıştırmalar|Başarısız çalıştırmalar|Count|Toplam|Bu çalışma alanı için başarısız çalıştırma sayısı|Senaryo|

## <a name="microsoftmapsaccounts"></a>Microsoft.Maps/accounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Kullanım|Kullanım|Count|Count|Sayısı, API çağrıları|ApiCategory, ApiName, ResultType, ResponseCode|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Average|Kullanılabilirlik API'leri|ApiCategory, ApiName|

## <a name="microsoftnetappnetappaccountscapacitypoolsvolumes"></a>Microsoft.NetApp/netAppAccounts/capacityPools/Volumes

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|AverageOtherLatency|Ortalama diğer gecikme süresi|MS/işlem|Average|(Yani, okuma veya yazma değil) diğer milisaniye cinsinden gecikme süresi işlemi başına ortalama|Boyut yok|
|AverageReadLatency|Ortalama okuma gecikme süresi|MS/işlem|Average|Ortalama okuma işlemi başına milisaniye cinsinden gecikme süresi|Boyut yok|
|AverageTotalLatency|Ortalama toplam gecikme süresi|MS/işlem|Average|İşlem başına milisaniye cinsinden ortalama toplam gecikme süresi|Boyut yok|
|AverageWriteLatency|Ortalama yazma gecikme süresi|MS/işlem|Average|İşlem başına milisaniye cinsinden ortalama yazma gecikme süresi|Boyut yok|
|FilesystemOtherOps|Dosya sistemi diğer işlemler|OPS|Average|Dosya sistemi sayısı (yani, okuma veya yazma değil) diğer işlemler|Boyut yok|
|FilesystemReadOps|Dosya sistemi işlemleri oku|OPS|Average|Dosya sistemi sayısı okuma işlemleri|Boyut yok|
|FilesystemTotalOps|Toplam dosya sistemi işlemleri|OPS|Average|Tüm dosya sistemi işlemleri toplamı|Boyut yok|
|FilesystemWriteOps|Dosya sistemi yazma işlemleri|OPS|Average|Dosya sistemi yazma işlemlerinin sayısı|Boyut yok|
|IoBytesPerOtherOps|Diğer işlemler başına g/ç bayt|bayt/işlem|Average|(Okuma veya yazma değil) numarasını In/out diğer işlem başına bayt|Boyut yok|
|IoBytesPerReadOps|GÇ bayt başına okuma işlemleri|bayt/işlem|Average|İçeri/dışarı okuma işlemi başına bayt sayısı|Boyut yok|
|IoBytesPerTotalOps|Bütün işlemler op başına g/ç bayt|bayt/işlem|Average|Tüm/giden bayt işlemi toplamı|Boyut yok|
|IoBytesPerWriteOps|Yazma işlemi başına g/ç bayt|bayt/işlem|Average|Giriş/Çıkış yazma işlemi başına bayt sayısı|Boyut yok|
|OtherIops|Diğer IOPS|işlem/saniye|Average|Diğer daraltma veya genişletme işlemi / saniye|Boyut yok|
|OtherThroughput|Diğer aktarım hızı|MB/sn|Average|(Yani, okuma veya yazma değil) diğer aktarım hızını saniye başına megabayt cinsinden|Boyut yok|
|ReadIops|Okuma IOPS|işlem/saniye|Average|Daraltma/genişletme işlemleri saniye başına okuma|Boyut yok|
|ReadThroughput|Okuma aktarım hızı|MB/sn|Average|Saniye başına megabayt cinsinden okuma aktarım hızı|Boyut yok|
|TotalIops|Toplam IOPS|işlem/saniye|Average|Tüm giriş/çıkış işlemleri saniye başına toplam|Boyut yok|
|TotalThroughput|Toplam aktarım hızı|MB/sn|Average|Tüm aktarım hızını saniye başına megabayt cinsinden toplam|Boyut yok|
|VolumeAllocatedSize|Boyutu ayrılmış birim|bayt|Average|Ayrılmış (değil gerçek bayt kullanılır) birimin boyutu|Boyut yok|
|VolumeLogicalSize|Mantıksal birim boyutu|bayt|Average|Mantıksal birimin boyutunu (kullanılan bayt)|Boyut yok|
|VolumeSnapshotSize|Birim anlık görüntü boyutu|bayt|Average|Birimdeki tüm anlık görüntülerin boyutu|Boyut yok|
|WriteIops|Yazma IOPS|işlem/saniye|Average|Yazma/saniye başına işlem|Boyut yok|
|WriteThroughput|Aktarım hızı yazma|MB/sn|Average|Aktarım hızı, saniye başına megabayt cinsinden yazma|Boyut yok|

## <a name="microsoftnetappnetappaccountscapacitypools"></a>Microsoft.NetApp/netAppAccounts/capacityPools

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|VolumePoolAllocatedSize|Birim ayrılmamış havuz boyutu|bayt|Average|Ayrılmış (değil gerçek bayt kullanılır) havuz boyutu|Boyut yok|
|VolumePoolAllocatedUsed|Ayrılmış birim havuzu kullanılan|bayt|Average|Havuzun ayrılan kullanılan boyut|Boyut yok|
|VolumePoolTotalLogicalSize|Birim havuzu toplam mantıksal boyutu|bayt|Average|Mantıksal boyutu havuza ait birimlerin toplamı|Boyut yok|
|VolumePoolTotalSnapshotSize|Birim havuzu toplam anlık görüntü boyutu|bayt|Average|Havuzdaki tüm anlık görüntü Topla|Boyut yok|

## <a name="microsoftnetworknetworkinterfaces"></a>Microsoft.Network/networkInterfaces

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BytesSentRate|Gönderilen bayt sayısı|Count|Toplam|Ağ arabirimi gönderilen bayt sayısı|Boyut yok|
|BytesReceivedRate|Alınan bayt sayısı|Count|Toplam|Ağ arabirimi alınan bayt sayısı|Boyut yok|
|PacketsSentRate|Gönderilen paketleri|Count|Toplam|Ağ arabirimi gönderilen paket sayısı|Boyut yok|
|PacketsReceivedRate|Alınan paket sayısı|Count|Toplam|Ağ arabirimi alınan paket sayısı|Boyut yok|

## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|VipAvailability|Veri yolu kullanılabilirliği|Count|Average|Yük Dengeleyici veri yolu kullanılabilirlik zaman süresi başına ortalama|FrontendIPAddress, FrontendPort|
|DipAvailability|Araştırma durumu|Count|Average|Zaman süresi başına ortalama yük dengeleyici sistem durumu araştırma durumu|ProtocolType Backendport'a, FrontendIPAddress FrontendPort, BackendIPAddress|
|ByteCount|Bayt sayısı|Count|Toplam|Toplam süre içinde aktarılan bayt sayısı|FrontendIPAddress, FrontendPort, yönü|
|PacketCount|Paket sayısı|Count|Toplam|Zaman aralığı içinde gönderilen paketlerin toplam sayısı|FrontendIPAddress, FrontendPort, yönü|
|SYNCount|SYN sayısı|Count|Toplam|SYN zaman aralığı içinde gönderilen paketlerin toplam sayısı|FrontendIPAddress, FrontendPort, yönü|
|SnatConnectionCount|SNAT bağlantısı sayısı|Count|Toplam|Toplam süre içinde oluşturulan yeni SNAT bağlantılarının sayısıdır|FrontendIPAddress, BackendIPAddress, ConnectionState|
|AllocatedSnatPorts|Ayrılmış SNAT bağlantı noktaları (Önizleme)|Count|Toplam|Toplam ayrılan süre içinde SNAT bağlantı noktalarının sayısı|FrontendIPAddress, BackendIPAddress, ProtocolType|
|UsedSnatPorts|Kullanılan SNAT bağlantı noktaları (Önizleme)|Count|Toplam|Toplam süre içinde kullanılan SNAT bağlantı noktalarının sayısı|FrontendIPAddress, BackendIPAddress, ProtocolType|

## <a name="microsoftnetworkdnszones"></a>Microsoft.Network/dnszones

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|QueryVolume|Sorgu birim|Count|Toplam|Bir DNS bölgesi için sunulan sorgu sayısı|Boyut yok|
|RecordSetCount|Kayıt kümesi sayısı|Count|Maksimum|Bir DNS bölgesinde kayıt kümesi sayısı|Boyut yok|
|RecordSetCapacityUtilization|Kayıt kümesi kapasite kullanımı|Percent|Maksimum|Bir DNS bölgesi tarafından kullanılan kayıt kümesi kapasite yüzdesi|Boyut yok|

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
|IfUnderDDoSAttack|Altında DDoS saldırı veya değil|Count|Maksimum|Altında DDoS saldırı veya değil|Boyut yok|
|DDoSTriggerTCPPackets|DDoS saldırılarının tetiklemek için gelen TCP paket|CountPerSecond|Maksimum|DDoS saldırılarının tetiklemek için gelen TCP paket|Boyut yok|
|DDoSTriggerUDPPackets|DDoS saldırılarının tetiklemek için gelen UDP paketlerini|CountPerSecond|Maksimum|DDoS saldırılarının tetiklemek için gelen UDP paketlerini|Boyut yok|
|DDoSTriggerSYNPackets|DDoS saldırılarının tetiklemek için gelen SYN paketleri|CountPerSecond|Maksimum|DDoS saldırılarının tetiklemek için gelen SYN paketleri|Boyut yok|
|VipAvailability|Veri yolu kullanılabilirliği|Count|Average|IP adresi kullanılabilirlik zaman süresi başına ortalama|Port|
|ByteCount|Bayt sayısı|Count|Toplam|Toplam süre içinde aktarılan bayt sayısı|Bağlantı noktası, yönü|
|PacketCount|Paket sayısı|Count|Toplam|Zaman aralığı içinde gönderilen paketlerin toplam sayısı|Bağlantı noktası, yönü|
|SynCount|SYN sayısı|Count|Toplam|SYN zaman aralığı içinde gönderilen paketlerin toplam sayısı|Bağlantı noktası, yönü|

## <a name="microsoftnetworkazurefirewalls"></a>Microsoft.Network/azurefirewalls

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ApplicationRuleHit|Uygulama kuralları isabet sayısı|Count|Toplam|Uygulama kuralları isabet ürün sayısı|Durum, neden, Protokolü|
|NetworkRuleHit|Ağ kuralları isabet sayısı|Count|Toplam|Ağ kuralları isabet ürün sayısı|Durum, neden, Protokolü|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Aktarım hızı|Aktarım hızı|BytesPerSecond|Toplam|Uygulama ağ geçidi hizmet saniye başına bayt sayısı|Boyut yok|
|UnhealthyHostCount|İyi durumda olmayan konak sayısı|Count|Average|İyi durumda olmayan arka uç ana bilgisayar sayısı|BackendSettingsPool|
|HealthyHostCount|Konak sayısı|Count|Average|Sağlıklı bir arka uç ana bilgisayar sayısı|BackendSettingsPool|
|TotalRequests|Toplam İstek Sayısı|Count|Toplam|Uygulama ağ geçidi hizmet verdi başarılı istek sayısı|BackendSettingsPool|
|FailedRequests|Başarısız İstekler|Count|Toplam|Uygulama ağ geçidi hizmet verdi başarısız istek sayısı|BackendSettingsPool|
|ResponseStatus|Yanıt durumu|Count|Toplam|Uygulama ağ geçidi tarafından döndürülen http yanıtı durum|HttpStatusGroup|
|CurrentConnections|Geçerli bağlantılar|Count|Toplam|Application Gateway ile kurulan geçerli bağlantı sayısı|Boyut yok|
|CapacityUnits|Geçerli kapasite birimleri|Count|Average|Kullanılan kapasite birimleri|Boyut yok|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.Network/virtualNetworkGateways

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|AverageBandwidth|Ağ geçidi S2S bant genişliği|BytesPerSecond|Average|Bir ağ geçidinin saniye başına bayt cinsinden ortalama siteden siteye bant genişliği|Boyut yok|
|P2SBandwidth|Ağ geçidi P2S bant genişliği|BytesPerSecond|Average|Bir ağ geçidinin saniye başına bayt cinsinden ortalama noktadan siteye bant genişliği|Boyut yok|
|P2SConnectionCount|P2S bağlantısı sayısı|Count|Maksimum|Noktadan siteye bağlantı ağ geçidi sayısı|Protocol|
|TunnelAverageBandwidth|Tünel bant genişliği|BytesPerSecond|Average|Bir tünel saniye başına bayt cinsinden ortalama bant genişliği|ConnectionName, RemoteIP|
|TunnelEgressBytes|Tünel çıkış bayt|Bayt|Toplam|Bir tünel Giden bayt|ConnectionName, RemoteIP|
|TunnelIngressBytes|Tünel giriş bayt|Bayt|Toplam|Bir tünel gelen bayt sayısı|ConnectionName, RemoteIP|
|TunnelEgressPackets|Tünel çıkış paketlerinin|Count|Toplam|Tünel giden paket sayısı|ConnectionName, RemoteIP|
|TunnelIngressPackets|Tünel giriş paketlerinin|Count|Toplam|Tünel gelen paket sayısı|ConnectionName, RemoteIP|
|TunnelEgressPacketDropTSMismatch|Tünel çıkış TS uyuşmazlığı paket bırakma|Count|Toplam|Giden paket bırakma sanal makine sayısının dışında bir tünel trafik seçicisini uyuşmazlığı|ConnectionName, RemoteIP|
|TunnelIngressPacketDropTSMismatch|Tünel giriş TS uyuşmazlığı paket bırakma|Count|Toplam|Gelen paket bırakma sanal makine sayısının dışında bir tünel trafik seçicisini uyuşmazlığı|ConnectionName, RemoteIP|

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Average|Saniye başına bit ingressing Azure|Peeringtype öğesi|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Average|Saniye başına Azure egressing BITS|Peeringtype öğesi|

## <a name="microsoftnetworkexpressroutecircuitspeerings"></a>Microsoft.Network/expressRouteCircuits/peerings

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Average|Saniye başına bit ingressing Azure|Boyut yok|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Average|Saniye başına Azure egressing BITS|Boyut yok|

## <a name="microsoftnetworkconnections"></a>Microsoft.Network/connections

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Average|Saniye başına bit ingressing Azure|Boyut yok|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Average|Saniye başına Azure egressing BITS|Boyut yok|

## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|QpsByEndpoint|Sorgu tarafından döndürülen uç noktası|Count|Toplam|Belirli bir zaman dilimi içinde döndürülen bir Traffic Manager uç nokta sayısı|EndpointName|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Uç nokta olarak uç nokta durumu|Count|Maksimum|uç noktanın araştırma durumu "etkinse," 1, aksi durumda 0.|EndpointName|

## <a name="microsoftnetworknetworkwatchersconnectionmonitors"></a>Microsoft.Network/networkWatchers/connectionMonitors

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ProbesFailedPercent|% Araştırmaları başarısız oldu|Percent|Average|araştırmalar izleme bağlantı başarısız oldu|Boyut yok|
|AverageRoundtripMs|Ort. Gidiş dönüş süresi (ms)|Milisaniye|Average|Kaynak ve hedef arasında gönderilen yoklamalar izleme bağlantı için ortalama ağ gidiş dönüş süresi (ms)|Boyut yok|

## <a name="microsoftnetworkfrontdoors"></a>Microsoft.Network/frontdoors

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|RequestCount|İstek Sayısı|Count|Toplam|HTTP/S proxy tarafından sunulan istemci isteklerinin sayısı|Httpstatus'a, HttpStatusGroup, ClientRegion, ClientCountry|
|RequestSize|İstek boyutu|Bayt|Toplam|İstekleri olarak istemcilerden HTTP/S Ara sunucuya gönderilen bayt sayısı|Httpstatus'a, HttpStatusGroup, ClientRegion, ClientCountry|
|ResponseSize|Yanıt boyutu|Bayt|Toplam|Yanıt HTTP/S proxy sunucudan istemcilere gönderilen bayt sayısı|Httpstatus'a, HttpStatusGroup, ClientRegion, ClientCountry|
|BackendRequestCount|Arka uç istek sayısı|Count|Toplam|İçin arka uçlar HTTP/S Ara sunucuya gönderilen istek sayısı|Httpstatus'a, HttpStatusGroup, arka uç|
|BackendRequestLatency|Arka uç istek gecikme süresi|Milisaniye|Average|Ara sunucu HTTP/S son yanıt bayt arka uçtan alınan kadar zaman İstek HTTP/S proxy tarafından arka ucuna gönderilen hesaplandığında|Arka uç|
|TotalLatency|Toplam gecikme süresi|Milisaniye|Average|İstemci, HTTP/S Proxy'den gelen son yanıt bayt onaylanır kadar ne zaman istemci isteği HTTP/S proxy tarafından alındı hesaplandığında|Httpstatus'a, HttpStatusGroup, ClientRegion, ClientCountry|
|BackendHealthPercentage|Arka uç sistem durumu yüzdesi|Percent|Average|Başarılı sistem durumu yüzdesi için arka uçlar HTTP/S Ara sunucuya araştırmaları|Arka uç, BackendPool|
|WebApplicationFirewallRequestCount|Web uygulaması güvenlik duvarı istek sayısı|Count|Toplam|Web uygulaması güvenlik duvarı tarafından işlenen istemci isteklerinin sayısı|PolicyName, RuleName, eylem|

## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|registration.all|Kayıt işlemleri|Count|Toplam|Tüm başarılı kayıt işlemlerinin (oluşturma, güncelleştirme, sorgu ve silme işlemleri) sayısı. |Boyut yok|
|registration.Create|Kayıt oluşturma işlemleri|Count|Toplam|Tüm başarılı kayıt oluşturma işlemlerinin sayısı.|Boyut yok|
|registration.Update|Kayıt Güncelleme işlemleri|Count|Toplam|Tüm başarılı kayıt güncelleştirmelerinin sayısı.|Boyut yok|
|registration.get|Kayıt okuma işlemleri|Count|Toplam|Tüm başarılı kayıt sorgularının sayısı.|Boyut yok|
|registration.Delete|Kayıt silme işlemleri|Count|Toplam|Tüm başarılı kayıt silme işlemlerinin sayısı.|Boyut yok|
|gelen|Gelen iletiler|Count|Toplam|Sayı başarılı olan tüm gönderme API'si çağrılarının. |Boyut yok|
|incoming.Scheduled|Zamanlanan anında iletme bildirimleri gönderildi|Count|Toplam|Zamanlanan anında iletme bildirimleri iptal edildi|Boyut yok|
|incoming.Scheduled.Cancel|Zamanlanan anında iletme bildirimleri iptal edildi|Count|Toplam|Zamanlanan anında iletme bildirimleri iptal edildi|Boyut yok|
|scheduled.pending|Bekleyen zamanlanmış bildirimler|Count|Toplam|Bekleyen zamanlanmış bildirimler|Boyut yok|
|installation.all|Yükleme yönetimi işlemleri|Count|Toplam|Yükleme yönetimi işlemleri|Boyut yok|
|installation.get|Yükleme işlemlerini Al|Count|Toplam|Yükleme işlemlerini Al|Boyut yok|
|installation.upsert|Yükleme operasyonları oluşturun veya güncelleştirin|Count|Toplam|Yükleme operasyonları oluşturun veya güncelleştirin|Boyut yok|
|installation.Patch|Düzeltme eki yükleme işlemleri|Count|Toplam|Düzeltme eki yükleme işlemleri|Boyut yok|
|installation.Delete|Yükleme işlemlerini sil|Count|Toplam|Yükleme işlemlerini sil|Boyut yok|
|outgoing.allpns.Success|Başarılı bildirimler|Count|Toplam|Başarılı olan tüm bildirimlerin sayısı.|Boyut yok|
|outgoing.allpns.invalidpayload|Yük hataları|Count|Toplam|PNS kötü yük hatası döndürdüğü için başarısız olan gönderim sayısı.|Boyut yok|
|outgoing.allpns.pnserror|Harici bildirim sistemi hataları|Count|Toplam|(Kimlik doğrulaması sorunları hariç) PNS ile iletişim kurulurken bir sorun nedeniyle başarısız olan gönderim sayısı.|Boyut yok|
|outgoing.allpns.channelerror|Kanal hataları|Count|Toplam|Kanal kısıtlanan veya süresi dolmuş doğru uygulama ile ilişkili olmayan geçersiz olduğu için başarısız olan gönderim sayısı.|Boyut yok|
|outgoing.allpns.badorexpiredchannel|Hatalı veya süresi dolmuş kanal hataları|Count|Toplam|Kanal/belirteç/RegistrationId kayıttaki süresi dolmuş veya geçersiz olduğu için başarısız olan gönderim sayısı.|Boyut yok|
|outgoing.WNS.Success|WNS başarılı bildirimler|Count|Toplam|Başarılı olan tüm bildirimlerin sayısı.|Boyut yok|
|outgoing.WNS.invalidcredentials|WNS yetkilendirme hataları (geçersiz kimlik bilgileri)|Count|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediği veya kimlik bilgileri engellendiği için başarısız olan gönderim sayısı. (Windows Live kimlik bilgilerini tanımıyor).|Boyut yok|
|outgoing.wns.badchannel|WNS geçersiz kanal hatası|Count|Toplam|Kayıttaki Channelurı tanınmadığı için başarısız olan gönderim sayısı (WNS durumu: (404 bulunamadı).|Boyut yok|
|outgoing.wns.expiredchannel|WNS süresi dolan kanal hatası|Count|Toplam|Channelurı'nin süresi dolduğu için başarısız olan gönderim sayısı (WNS durumu: 410 Gone).|Boyut yok|
|outgoing.WNS.throttled|WNS kısıtlanan bildirimler|Count|Toplam|WNS bu uygulamayı kısıtladığı için başarısız olan gönderim sayısı (WNS durumu: 406 Kabul edilemez).|Boyut yok|
|outgoing.WNS.tokenproviderunreachable|WNS yetkilendirme hataları (ulaşılamıyor)|Count|Toplam|Windows Live erişilebilir değil.|Boyut yok|
|outgoing.WNS.invalidtoken|WNS yetkilendirme hataları (geçersiz belirteç)|Count|Toplam|WNS'ye sağlanan belirteç geçerli değil (WNS durumu: 401 Yetkisiz).|Boyut yok|
|outgoing.WNS.wrongtoken|WNS yetkilendirme hataları (yanlış belirteç)|Count|Toplam|WNS'ye sağlanan belirteç geçerli, ancak başka bir uygulama için (WNS durumu: 403 Yasak). Kayıttaki Channelurı başka bir uygulama ile ilişkili ise bu durum oluşabilir. İstemci uygulaması bildirim hub'ında kimlik bilgileri bulunan uygulamayla ilişkili olup olmadığını denetleyin.|Boyut yok|
|outgoing.WNS.invalidnotificationformat|WNS geçersiz bildirim biçimi|Count|Toplam|Bildirimin biçimi geçersiz (WNS durumu: 400). WNS tüm geçersiz reddetmediğini olduğunu unutmayın.|Boyut yok|
|outgoing.WNS.invalidnotificationsize|WNS geçersiz bildirim boyutu hatası|Count|Toplam|Bildirim yükü çok büyük (WNS durumu: 413).|Boyut yok|
|outgoing.WNS.channelthrottled|WNS kanal kısıtlandı|Count|Toplam|Bildirim, kayıttaki Channelurı kısıtlandığı için bırakıldı (WNS yanıt üst bilgisi: X-WNS-NotificationStatus:channelThrottled).|Boyut yok|
|outgoing.wns.channeldisconnected|WNS kanal bağlantısı kesildi|Count|Toplam|Bildirim, kayıttaki Channelurı kısıtlandığı için bırakıldı (WNS yanıt üst bilgisi: X-WNS-DeviceConnectionStatus: bağlantısı kesildi).|Boyut yok|
|outgoing.WNS.dropped|WNS bırakılan bildirimler|Count|Toplam|Bildirim, kayıttaki Channelurı kısıtlandığı için bırakıldı (X-WNS-NotificationStatus: bırakıldı, ancak değil X-WNS-DeviceConnectionStatus: bağlantısı kesildi).|Boyut yok|
|outgoing.wns.pnserror|WNS hataları|Count|Toplam|Bildirim, WNS ile iletişim kurulurken oluşan hatalardan dolayı teslim edilemedi.|Boyut yok|
|outgoing.wns.authenticationerror|WNS kimlik doğrulaması hataları|Count|Toplam|Bildirim, Windows Live geçersiz kimlik bilgileri veya yanlış belirteçten ile iletişim kurulurken oluşan hatalardan dolayı teslim edilemedi.|Boyut yok|
|outgoing.apns.Success|APNS başarılı bildirimler|Count|Toplam|Başarılı olan tüm bildirimlerin sayısı.|Boyut yok|
|outgoing.apns.invalidcredentials|APNS yetkilendirme hataları|Count|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediği veya kimlik bilgileri engellendiği için başarısız olan gönderim sayısı.|Boyut yok|
|outgoing.apns.badchannel|APNS geçersiz kanal hatası|Count|Toplam|Belirteç geçersiz olduğu için başarısız olan gönderim sayısı (APNS durum kodu: 8).|Boyut yok|
|outgoing.apns.expiredchannel|APNS süresi dolan kanal hatası|Count|Toplam|APNS geri bildirim kanalı tarafından geçersiz kılınan belirteç sayısı.|Boyut yok|
|outgoing.apns.invalidnotificationsize|APNS geçersiz bildirim boyutu hatası|Count|Toplam|Yük fazla büyük olduğu için başarısız olan gönderim sayısı (APNS durum kodu: 7).|Boyut yok|
|outgoing.apns.pnserror|APNS hataları|Count|Toplam|APNS ile iletişim kurulurken oluşan hatalardan dolayı başarısız olan gönderim sayısı.|Boyut yok|
|outgoing.GCM.Success|GCM başarılı bildirimler|Count|Toplam|Başarılı olan tüm bildirimlerin sayısı.|Boyut yok|
|outgoing.GCM.invalidcredentials|GCM yetkilendirme hataları (geçersiz kimlik bilgileri)|Count|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediği veya kimlik bilgileri engellendiği için başarısız olan gönderim sayısı.|Boyut yok|
|outgoing.gcm.badchannel|GCM geçersiz kanal hatası|Count|Toplam|Kayıttaki RegistrationId tanınmadığı için başarısız olan gönderim sayısı (GCM sonucu: Geçersiz kayıt).|Boyut yok|
|outgoing.GCM.expiredchannel|GCM süresi dolan kanal hatası|Count|Toplam|Kayıttaki RegistrationId süresi dolduğu için başarısız olan gönderim sayısı (GCM sonucu: NotRegistered).|Boyut yok|
|outgoing.GCM.throttled|GCM kısıtlanan bildirimler|Count|Toplam|GCM bu uygulamayı kısıtladığı için başarısız olan gönderim sayısı (GCM durum kodu: 501-599 veya sonucu: kullanılamaz).|Boyut yok|
|outgoing.GCM.invalidnotificationformat|GCM geçersiz bildirim biçimi|Count|Toplam|Yük doğru bir şekilde biçimlendirilmediği için başarısız olan gönderim sayısı (GCM sonucu: Invaliddatakey veya Invalidttl).|Boyut yok|
|outgoing.GCM.invalidnotificationsize|GCM geçersiz bildirim boyutu hatası|Count|Toplam|Yük fazla büyük olduğu için başarısız olan gönderim sayısı (GCM sonucu: MessageTooBig).|Boyut yok|
|outgoing.gcm.wrongchannel|GCM yanlış kanal hatası|Count|Toplam|Kayıttaki RegistrationId geçerli uygulamayla ilişkili olmadığı için başarısız olan gönderim sayısı (GCM sonucu: Invalidpackagename).|Boyut yok|
|outgoing.gcm.pnserror|GCM hataları|Count|Toplam|GCM ile iletişim kurulurken oluşan hatalardan dolayı başarısız olan gönderim sayısı.|Boyut yok|
|outgoing.gcm.authenticationerror|GCM kimlik doğrulaması hataları|Count|Toplam|PNS belirtilen kimlik kimlik bilgilerini kabul etmediği için başarısız olan gönderim sayısı engellendiği veya Senderıd uygulamada doğru yapılandırılmamış (GCM sonucu: MismatchedSenderId).|Boyut yok|
|outgoing.mpns.Success|MPNS başarılı bildirimler|Count|Toplam|Başarılı olan tüm bildirimlerin sayısı.|Boyut yok|
|outgoing.mpns.invalidcredentials|MPNS geçersiz kimlik bilgileri|Count|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediği veya kimlik bilgileri engellendiği için başarısız olan gönderim sayısı.|Boyut yok|
|outgoing.mpns.badchannel|MPNS geçersiz kanal hatası|Count|Toplam|Kayıttaki Channelurı tanınmadığı için başarısız olan gönderim sayısı (MPNS durumu: (404 bulunamadı).|Boyut yok|
|outgoing.mpns.throttled|MPNS kısıtlanan bildirimler|Count|Toplam|MPNS bu uygulamayı kısıtladığı için başarısız olan gönderim sayısı (WNS MPNS: 406 Kabul edilemez).|Boyut yok|
|outgoing.mpns.invalidnotificationformat|MPNS geçersiz bildirim biçimi|Count|Toplam|Bildirimin yükü fazla büyük olduğu için başarısız olan gönderim sayısı.|Boyut yok|
|outgoing.mpns.channeldisconnected|MPNS kanalın bağlantısı kesildi|Count|Toplam|Kayıttaki Channelurı'nin bağlantısı kesildiği için başarısız olan gönderim sayısı (MPNS durumu: 412) bulunamadı.|Boyut yok|
|outgoing.mpns.dropped|MPNS bırakılan bildirimler|Count|Toplam|MPNS tarafından bırakılan gönderim sayısı (MPNS yanıt üst bilgisi: X-NotificationStatus: QueueFull veya gizlenen).|Boyut yok|
|outgoing.mpns.pnserror|MPNS hataları|Count|Toplam|MPNS ile iletişim kurulurken oluşan hatalardan dolayı başarısız olan gönderim sayısı.|Boyut yok|
|outgoing.mpns.authenticationerror|MPNS kimlik doğrulaması hataları|Count|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediği veya kimlik bilgileri engellendiği için başarısız olan gönderim sayısı.|Boyut yok|
|notificationhub.pushes|Tüm giden bildirimler|Count|Toplam|Bildirim hub'ı tüm giden bildirimler|Boyut yok|
|incoming.all.Requests|Tüm gelen istekler|Count|Toplam|Bildirim hub'ı için toplam gelen istek sayısı|Boyut yok|
|incoming.all.failedrequests|Tüm gelen başarısız istekler|Count|Toplam|Bildirim hub'ı için toplam gelen başarısız istek|Boyut yok|

## <a name="microsoftoperationalinsightsworkspaces"></a>Microsoft.OperationalInsights/workspaces

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Average_ % boş Inode'ları|% Boş Inode'ları|Count|Average|Average_ % boş Inode'ları|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ boş alan yüzdesi|% Boş alan|Count|Average|Average_ boş alan yüzdesi|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % kullanılan Inode|% Kullanılan Inode|Count|Average|Average_ % kullanılan Inode|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % kullanılan alan|% Kullanılan alan|Count|Average|Average_ % kullanılan alan|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Disk Okuma Bayt/sn|Disk Okuma Bayt/sn|Count|Average|Average_Disk Okuma Bayt/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Disk Okuma/sn|Disk Okuma/sn|Count|Average|Average_Disk Okuma/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Disk aktarımı/sn|Disk aktarımı/sn|Count|Average|Average_Disk aktarımı/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Disk Yazma Bayt/sn|Disk Yazma Bayt/sn|Count|Average|Average_Disk Yazma Bayt/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Disk Yazma/sn|Disk Yazma/sn|Count|Average|Average_Disk Yazma/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Free megabayt|Boş megabayt|Count|Average|Average_Free megabayt|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Logical Disk Bayt/sn|Mantıksal Disk Bayt/sn|Count|Average|Average_Logical Disk Bayt/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % kullanılabilir bellek|% Kullanılabilir bellek|Count|Average|Average_ % kullanılabilir bellek|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % kullanılabilir takas alanı|% Kullanılabilir takas alanı|Count|Average|Average_ % kullanılabilir takas alanı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % kullanılan bellek|% Kullanılan bellek|Count|Average|Average_ % kullanılan bellek|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % kullanılan takas alanı|% Kullanılan takas alanı|Count|Average|Average_ % kullanılan takas alanı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Available MBayt bellek|Kullanılabilir MBayt belleği|Count|Average|Average_Available MBayt bellek|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Available MBayt değiştirme|Kullanılabilir MBayt değiştirme|Count|Average|Average_Available MBayt değiştirme|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Page Okuma/sn|Sayfa Okuma/sn|Count|Average|Average_Page Okuma/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Page Yazma/sn|Sayfa Yazma/sn|Count|Average|Average_Page Yazma/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Pages/sn|Sayfa/sn|Count|Average|Average_Pages/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Used MBayt takas alanı|Kullanılan MBayt takas alanı|Count|Average|Average_Used MBayt takas alanı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Used bellek MBayt'ı|Kullanılan bellek MBayt|Count|Average|Average_Used bellek MBayt'ı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|İletilen Average_Total bayt|Aktarılan toplam bayt|Count|Average|İletilen Average_Total bayt|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Alınan Average_Total bayt sayısı|Alınan toplam bayt sayısı|Count|Average|Alınan Average_Total bayt sayısı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Total bayt|Toplam bayt sayısı|Count|Average|Average_Total bayt|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|İletilen Average_Total paket sayısı|Aktarılan toplam paket sayısı|Count|Average|İletilen Average_Total paket sayısı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Alınan Average_Total paketleri|Alınan toplam paket sayısı|Count|Average|Alınan Average_Total paketleri|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Total Rx hataları|Toplam Rx hataları|Count|Average|Average_Total Rx hataları|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Total Tx hataları|Toplam Tx hataları|Count|Average|Average_Total Tx hataları|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Total çakışmaları|Toplam çakışmaları|Count|Average|Average_Total çakışmaları|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Avg. Disk sn/Okuma|Ort. Disk sn/Okuma|Count|Average|Average_Avg. Disk sn/Okuma|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Avg. Disk sn/Aktarım|Ort. Disk sn/Aktarım|Count|Average|Average_Avg. Disk sn/Aktarım|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Avg. Disk sn/yazma|Ort. Disk sn/yazma|Count|Average|Average_Avg. Disk sn/yazma|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Physical Disk Bayt/sn|Fiziksel Disk Bayt/sn|Count|Average|Average_Physical Disk Bayt/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Pct ayrıcalıklı zaman|PCT ayrıcalıklı zaman|Count|Average|Average_Pct ayrıcalıklı zaman|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Pct kullanıcı zamanı|PCT kullanıcı zamanı|Count|Average|Average_Pct kullanıcı zamanı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Used bellek KB|Kullanılan bellek KB|Count|Average|Average_Used bellek KB|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Virtual paylaşılan bellek|Paylaşılan sanal bellek|Count|Average|Average_Virtual paylaşılan bellek|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % DPC Zamanı|% DPC Zamanı|Count|Average|Average_ % DPC Zamanı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % boş zaman|% Boş zaman|Count|Average|Average_ % boş zaman|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % kesme zamanı|% Kesme Zamanı|Count|Average|Average_ % kesme zamanı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % GÇ bekleme zamanı|% GÇ bekleme zamanı|Count|Average|Average_ % GÇ bekleme zamanı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % iyi olma zamanı|% İyi olma zamanı|Count|Average|Average_ % iyi olma zamanı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % ayrıcalıklı zaman|% Ayrıcalıklı Zaman|Count|Average|Average_ % ayrıcalıklı zaman|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % işlemci zamanı|% İşlemci zamanı|Count|Average|Average_ % işlemci zamanı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % kullanıcı zamanı|% Kullanıcı Zamanı|Count|Average|Average_ % kullanıcı zamanı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Free fiziksel bellek|Boş fiziksel bellek|Count|Average|Average_Free fiziksel bellek|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Disk belleği dosyalarında Average_Free alanı|Disk belleği dosyasındaki boş alan|Count|Average|Disk belleği dosyalarında Average_Free alanı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Free sanal bellek|Boş sanal bellek|Count|Average|Average_Free sanal bellek|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Processes|İşlemler|Count|Average|Average_Processes|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Disk belleği dosyalarında depolanan Average_Size|Disk belleği dosyalarında depolanan boyut|Count|Average|Disk belleği dosyalarında depolanan Average_Size|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Uptime|Çalışma Süresi|Count|Average|Average_Uptime|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Users|Kullanıcılar|Count|Average|Average_Users|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Avg. Disk sn/Okuma|Ort. Disk sn/Okuma|Count|Average|Average_Avg. Disk sn/Okuma|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Avg. Disk sn/yazma|Ort. Disk sn/yazma|Count|Average|Average_Avg. Disk sn/yazma|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Current Disk kuyruğu uzunluğu|Geçerli Disk Sırası Uzunluğu|Count|Average|Average_Current Disk kuyruğu uzunluğu|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Disk Okuma/sn|Disk Okuma/sn|Count|Average|Average_Disk Okuma/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Disk aktarımı/sn|Disk aktarımı/sn|Count|Average|Average_Disk aktarımı/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Disk Yazma/sn|Disk Yazma/sn|Count|Average|Average_Disk Yazma/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Free megabayt|Boş megabayt|Count|Average|Average_Free megabayt|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ boş alan yüzdesi|% Boş alan|Count|Average|Average_ boş alan yüzdesi|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Available MBayt|Kullanılabilir MBayt|Count|Average|Average_Available MBayt|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % kullanılan Kaydedilmiş Bayt yüzdesi|% Kullanılan kaydedilmiş bayt|Count|Average|Average_ % kullanılan Kaydedilmiş Bayt yüzdesi|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Bytes alınan/sn|Alınan Bayt/sn|Count|Average|Average_Bytes alınan/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Bytes gönderilen/sn|Gönderilen bayt/sn|Count|Average|Average_Bytes gönderilen/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Bytes toplam/sn|Toplam Bayt/sn|Count|Average|Average_Bytes toplam/sn|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_ % işlemci zamanı|% İşlemci zamanı|Count|Average|Average_ % işlemci zamanı|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Average_Processor kuyruğu uzunluğu|İşlemci kuyruğu uzunluğu|Count|Average|Average_Processor kuyruğu uzunluğu|Bilgisayar, ObjectName, InstanceName, sayaç yolu, Analytics'teki|
|Sinyal|Sinyal|Count|Toplam|Sinyal|Computer, OSType, Version, SourceComputerId|
|Güncelleştirme|Güncelleştirme|Count|Average|Güncelleştirme|Bilgisayar, ürün, Sınıflandırma, isteğe bağlı, onaylanan UpdateState|
|Olay|Olay|Count|Average|Olay|Kaynak, EventLog, bilgisayar, EventCategory, EventLevel, EventLevelName, EventID|

## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft.PowerBIDedicated/capacities

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|QueryDuration|Sorgu süresi|Milisaniye|Average|Son aralığı DAX sorgusunun süresi|Boyut yok|
|QueryPoolJobQueueLength|İş parçacıkları: Sorgu havuzu iş kuyruğu uzunluğu|Count|Average|Sorgu iş parçacığı havuzu kuyruğundaki iş sayısı.|Boyut yok|
|qpu_high_utilization_metric|QPU yüksek kullanımı|Count|Toplam|Son dakika, 1 yüksek QPU kullanımı, aksi durumda 0 QPU yüksek kullanımı|Boyut yok|
|memory_metric|Bellek|Bayt|Average|Bellek. A1, 0-5 Aralık 0-3 GB için A2 GB, 0-10 GB için A3, A4 için 0-25 GB, A5 için 0-50 GB ve 0-100 GB için A6|Boyut yok|
|memory_thrashing_metric|Bellek çok yavaş|Percent|Average|Ortalama bellek temizleme.|Boyut yok|

## <a name="microsoftrelaynamespaces"></a>Microsoft.Relay/namespaces

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Microsoft.Relay için başarılı|Microsoft.Relay için başarılı|Count|Toplam|Microsoft.Relay için başarılı ListenerConnections clienterror.|EntityName|
|Microsoft.Relay için Listenerconnections'da|Microsoft.Relay için Listenerconnections'da|Count|Toplam|Microsoft.Relay için listenerconnections'da Senderconnections'da.|EntityName|
|Microsoft.Relay ServerError|Microsoft.Relay ServerError|Count|Toplam|Microsoft.Relay için listenerconnections'da ServerError.|EntityName|
|Microsoft.Relay SenderConnections-başarılı|Microsoft.Relay SenderConnections-başarılı|Count|Toplam|Başarılı Microsoft.Relay için SenderConnections.|EntityName|
|Microsoft.Relay SenderConnections Senderconnections'da|Microsoft.Relay SenderConnections Senderconnections'da|Count|Toplam|Microsoft.Relay için SenderConnections Senderconnections'da.|EntityName|
|SenderConnections-ServerError|SenderConnections-ServerError|Count|Toplam|Microsoft.Relay için SenderConnections ServerError.|EntityName|
|Microsoft.Relay TotalRequests|Microsoft.Relay TotalRequests|Count|Toplam|Microsoft.Relay için toplam Listenerconnection sayısı.|EntityName|
|Microsoft.Relay SenderConnections TotalRequests|Microsoft.Relay SenderConnections TotalRequests|Count|Toplam|SenderConnections istekleri sayısı için toplam sayısı.|EntityName|
|ActiveConnections|ActiveConnections|Count|Toplam|Microsoft.Relay için toplam ActiveConnection sayısı.|EntityName|
|ActiveListeners|ActiveListeners|Count|Toplam|Microsoft.Relay için toplam Activelistener sayısı.|EntityName|
|BytesTransferred|BytesTransferred|Count|Toplam|Microsoft.Relay için toplam BytesTransferred sayısı.|EntityName|
|ListenerDisconnects|ListenerDisconnects|Count|Toplam|İçin Listenerdisconnect sayısı.|EntityName|
|SenderDisconnects|SenderDisconnects|Count|Toplam|Microsoft.Relay için toplam Senderdisconnect sayısı.|EntityName|

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SearchLatency|Arama gecikme süresi|Saniye|Average|Arama hizmeti için ortalama bir arama gecikme süresi|Boyut yok|
|SearchQueriesPerSecond|Saniye başına arama sorguları|CountPerSecond|Average|Arama hizmeti için saniye başına arama sorguları|Boyut yok|
|ThrottledSearchQueriesPercentage|Kısıtlanmış arama sorguları yüzdesi|Percent|Average|Arama hizmeti için kısıtlanmış arama sorguları yüzdesi|Boyut yok|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SuccessfulRequests|Başarılı istekler (Önizleme)|Count|Toplam|Ad alanı (Önizleme) için toplam başarılı istek|EntityName|
|ServerErrors|Sunucu hataları. (Önizleme)|Count|Toplam|Microsoft.ServiceBus için sunucu hataları. (Önizleme)|EntityName|
|UserErrors|Kullanıcı hataları. (Önizleme)|Count|Toplam|Microsoft.ServiceBus için kullanıcı hataları. (Önizleme)|EntityName|
|ThrottledRequests|Daraltılmış istekler. (Önizleme)|Count|Toplam|Microsoft.ServiceBus için daraltılmış istekler. (Önizleme)|EntityName|
|IncomingRequests|Gelen istekler (Önizleme)|Count|Toplam|Microsoft.ServiceBus için gelen istekler. (Önizleme)|EntityName|
|IncomingMessages|Gelen iletiler (Önizleme)|Count|Toplam|Microsoft.ServiceBus için gelen iletiler. (Önizleme)|EntityName|
|OutgoingMessages|Giden iletiler (Önizleme)|Count|Toplam|Microsoft.ServiceBus için giden iletiler. (Önizleme)|EntityName|
|ActiveConnections|ActiveConnections (Önizleme)|Count|Toplam|Microsoft.ServiceBus için toplam etkin bağlantılar. (Önizleme)|Boyut yok|
|Size|Boyut (Önizleme)|Bayt|Average|Sıranın/konunun bayt cinsinden boyutu. (Önizleme)|EntityName|
|İletiler|Bir sıradaki/konudaki iletilerin sayısı. (Önizleme)|Count|Average|Bir sıradaki/konudaki iletilerin sayısı. (Önizleme)|EntityName|
|ActiveMessages|Bir sıradaki/konudaki etkin iletilerin sayısı. (Önizleme)|Count|Average|Bir sıradaki/konudaki etkin iletilerin sayısı. (Önizleme)|EntityName|
|DeadletteredMessages|Bir sıradaki/konudaki iletilerin eski lettered sayısı. (Önizleme)|Count|Average|Bir sıradaki/konudaki iletilerin eski lettered sayısı. (Önizleme)|EntityName|
|ScheduledMessages|Bir sıradaki/konudaki zamanlanmış iletilerin sayısı. (Önizleme)|Count|Average|Bir sıradaki/konudaki zamanlanmış iletilerin sayısı. (Önizleme)|EntityName|
|CPUXNS|Ad alanı başına CPU kullanımı|Percent|Maksimum|Service bus premium ad alanı CPU kullanım ölçümü|Boyut yok|
|WSXNS|Ad alanı başına bellek boyutu kullanımı|Percent|Maksimum|Service bus premium ad alanı bellek kullanım ölçümü|Boyut yok|

## <a name="microsoftservicefabricmeshapplications"></a>Microsoft.ServiceFabricMesh/applications

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|AllocatedCpu|AllocatedCpu|Count|Average|Milicore'daki bu kapsayıcıya ayrılan Cpu|ApplicationName, HizmetAdı, CodePackageName, ServiceReplicaName|
|AllocatedMemory|AllocatedMemory|Bayt|Average|Bu kapsayıcı MB cinsinden ayrılan bellek|ApplicationName, HizmetAdı, CodePackageName, ServiceReplicaName|
|ActualCpu|ActualCpu|Count|Average|Milicore'daki gerçek CPU kullanımı|ApplicationName, HizmetAdı, CodePackageName, ServiceReplicaName|
|ActualMemory|ActualMemory|Bayt|Average|MB cinsinden gerçek bellek kullanımı|ApplicationName, HizmetAdı, CodePackageName, ServiceReplicaName|
|CpuUtilization|CpuUtilization|Percent|Average|CPU yüzdesi cinsinden AllocatedCpu kullanımı bu kapsayıcı için|ApplicationName, HizmetAdı, CodePackageName, ServiceReplicaName|
|MemoryUtilization|MemoryUtilization|Percent|Average|CPU yüzdesi cinsinden AllocatedCpu kullanımı bu kapsayıcı için|ApplicationName, HizmetAdı, CodePackageName, ServiceReplicaName|
|ApplicationStatus|ApplicationStatus|Count|Average|Service Fabric Mesh uygulamasının durumu|ApplicationName, durumu|
|ServiceStatus|ServiceStatus|Count|Average|Bir Service Fabric Mesh uygulama hizmetinin sistem durumu|ApplicationName, durumu, ServiceName|
|ServiceReplicaStatus|ServiceReplicaStatus|Count|Average|Service Fabric Mesh uygulaması'nda hizmet çoğaltma sistem durumu|ApplicationName, durumu, HizmetAdı, ServiceReplicaName|
|ContainerStatus|ContainerStatus|Count|Average|Service Fabric Mesh uygulama kapsayıcısında durumu|ApplicationName ServiceName, CodePackageName ServiceReplicaName, durumu|
|RestartCount|RestartCount|Count|Average|Service Fabric Mesh uygulama bir kapsayıcı sayısı yeniden başlatın|ApplicationName durum, ServiceName ServiceReplicaName, CodePackageName|

## <a name="microsoftsignalrservicesignalr"></a>Microsoft.SignalRService/SignalR

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ConnectionCount|Bağlantı sayısı|Count|Maksimum|Kullanıcı bağlantı miktarı.|Uç Nokta|
|MessageCount|İleti sayısı|Count|Toplam|İletilerin toplam tutar.|Boyut yok|
|InboundTraffic|Gelen Trafik|Bayt|Toplam|Gelen trafik hizmeti|Boyut yok|
|OutboundTraffic|Giden Trafik|Bayt|Toplam|Giden trafik hizmeti|Boyut yok|
|UserErrors|Kullanıcı hataları|Percent|Maksimum|Kullanıcı hatalarının yüzdesi|Boyut yok|
|SystemErrors|Sistem hataları|Percent|Maksimum|Sistem hatalarının yüzdesi|Boyut yok|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Percent|Average|CPU yüzdesi|Boyut yok|
|physical_data_read_percent|Veri G/Ç yüzdesi|Percent|Average|Veri G/Ç yüzdesi|Boyut yok|
|log_write_percent|Günlük g/ç yüzdesi|Percent|Average|Günlük g/ç yüzdesi|Boyut yok|
|dtu_consumption_percent|DTU yüzdesi|Percent|Average|DTU yüzdesi|Boyut yok|
|depolama|Kullanılan veri alanı|Bayt|Maksimum|Toplam veritabanı boyutu|Boyut yok|
|connection_successful|Başarılı bağlantılar|Count|Toplam|Başarılı bağlantılar|Boyut yok|
|connection_failed|Başarısız Bağlantılar|Count|Toplam|Başarısız Bağlantılar|Boyut yok|
|blocked_by_firewall|Güvenlik Duvarı tarafından engellendi|Count|Toplam|Güvenlik Duvarı tarafından engellendi|Boyut yok|
|Kilitlenme|Kilitlenmeler|Count|Toplam|Kilitlenmeler|Boyut yok|
|storage_percent|Kullanılan yüzde veri alanı|Percent|Maksimum|Veri boyutu yüzdesi|Boyut yok|
|xtp_storage_percent|Bellek içi OLTP depolama yüzdesi|Percent|Average|Bellek içi OLTP depolama yüzdesi|Boyut yok|
|workers_percent|Çalışan yüzde|Percent|Average|Çalışan yüzde|Boyut yok|
|sessions_percent|Oturumları yüzdesi|Percent|Average|Oturumları yüzdesi|Boyut yok|
|dtu_limit|DTU Limit|Count|Average|DTU Limit|Boyut yok|
|dtu_used|Kullanılan DTU|Count|Average|Kullanılan DTU|Boyut yok|
|cpu_limit|CPU sınırı|Count|Average|CPU sınırı|Boyut yok|
|cpu_used|Kullanılan CPU|Count|Average|Kullanılan CPU|Boyut yok|
|dwu_limit|DWU limiti|Count|Maksimum|DWU limiti|Boyut yok|
|dwu_consumption_percent|DWU yüzdesi|Percent|Maksimum|DWU yüzdesi|Boyut yok|
|dwu_used|Kullanılan DWU|Count|Maksimum|Kullanılan DWU|Boyut yok|
|dw_cpu_percent|DW nodu düzeyinde CPU yüzdesi|Percent|Average|DW nodu düzeyinde CPU yüzdesi|DwLogicalNodeId|
|dw_physical_data_read_percent|DW düğüm düzeyi veri g/ç yüzdesi|Percent|Average|DW düğüm düzeyi veri g/ç yüzdesi|DwLogicalNodeId|
    |cache_hit_percent|Önbellek isabet yüzdesi|Percent|Maksimum|Önbellek isabet yüzdesi|Boyut yok|
|cache_used_percent|Kullanılan önbellek yüzdesi|Percent|Maksimum|Kullanılan önbellek yüzdesi|Boyut yok|
|local_tempdb_usage_percent|Yerel tempdb yüzdesi|Percent|Average|Yerel tempdb yüzdesi|Boyut yok|
|app_cpu_billed|Uygulama CPU faturalandırılır|Count|Toplam|Uygulama CPU faturalandırılır|Boyut yok|
|app_cpu_percent|Uygulama CPU yüzdesi|Percent|Average|Uygulama CPU yüzdesi|Boyut yok|
|app_memory_percent|Uygulama kullanılan bellek yüzdesi|Percent|Average|Uygulama kullanılan bellek yüzdesi|Boyut yok|
|allocated_data_storage|Ayrılmış veri alanı|Bayt|Average|Ayrılmış veri alanı|Boyut yok|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Percent|Average|CPU yüzdesi|Boyut yok|
|physical_data_read_percent|Veri G/Ç yüzdesi|Percent|Average|Veri G/Ç yüzdesi|Boyut yok|
|log_write_percent|Günlük g/ç yüzdesi|Percent|Average|Günlük g/ç yüzdesi|Boyut yok|
|dtu_consumption_percent|DTU yüzdesi|Percent|Average|DTU yüzdesi|Boyut yok|
|storage_percent|Kullanılan yüzde veri alanı||Percent|Average|Depolama yüzdesi|Boyut yok|
|workers_percent|Çalışan yüzde|Percent|Average|Çalışan yüzde|Boyut yok|
|sessions_percent|Oturumları yüzdesi|Percent|Average|Oturumları yüzdesi|Boyut yok|
|eDTU_limit|eDTU sınırı|Count|Average|eDTU sınırı|Boyut yok|
|storage_limit|Veri en büyük boyutu|Bayt|Average|Depolama sınırı|Boyut yok|
|eDTU_used|kullanılan eDTU|Count|Average|kullanılan eDTU|Boyut yok|
|storage_used|Kullanılan veri alanı|Bayt|Average|Kullanılan depolama|Boyut yok|
|xtp_storage_percent|Bellek içi OLTP depolama yüzdesi|Percent|Average|Bellek içi OLTP depolama yüzdesi|Boyut yok|
|cpu_limit|CPU sınırı|Count|Average|CPU sınırı|Boyut yok|
|cpu_used|Kullanılan CPU|Count|Average|Kullanılan CPU|Boyut yok|
|allocated_data_storage|Ayrılmış veri alanı|Bayt|Average|Ayrılmış veri alanı|Boyut yok|
|allocated_data_storage_percent|Veriler ayrılan alanı yüzde|Percent|Maksimum|Veriler ayrılan alanı yüzde|Boyut yok|

## <a name="microsoftsqlmanagedinstances"></a>Microsoft.Sql/managedInstances

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|virtual_core_count|Sanal çekirdek sayısı|Count|Average|Sanal çekirdek sayısı|Boyut yok|
|avg_cpu_percent|Ortalama CPU yüzdesi|Percent|Average|Ortalama CPU yüzdesi|Boyut yok|
|reserved_storage_mb|Ayrılan depolama alanı|Count|Average|Ayrılan depolama alanı|Boyut yok|
|storage_space_used_mb|Kullanılan depolama alanı|Count|Average|Kullanılan depolama alanı|Boyut yok|
|io_requests|G/ç isteği sayısı|Count|Average|G/ç isteği sayısı|Boyut yok|
|io_bytes_read|GÇ Okunan Bayt|Bayt|Average|GÇ Okunan Bayt|Boyut yok|
|io_bytes_written|GÇ yazılan bayt|Bayt|Average|GÇ yazılan bayt|Boyut yok|

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|UsedCapacity|Kullanılan kapasite|Bayt|Ortalama|Hesabın kullanılan kapasitesi|Boyut yok|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı başarılı ve başarısız isteklerin yanı sıra hataları üretilen istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın.|ResponseType, GeoType, ApiName, kimlik doğrulaması|
|Giriş|Giriş|Bayt|Toplam|Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|GeoType, ApiName, kimlik doğrulaması|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|GeoType, ApiName, kimlik doğrulaması|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer AverageE2ELatency'de belirtilen ağ gecikmesini içermez.|GeoType, ApiName, kimlik doğrulaması|
|Başarı E2e|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|GeoType, ApiName, kimlik doğrulaması|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik TotalBillableRequests değeri ve beklenmeyen hata üreten dahil ilgili istek sayısına göre bölme göre hesaplanır. Azaltılmış kullanılabilirlik ve depolama hizmetine veya belirtilen API işlemi için beklenmeyen tüm hatalar sonuçlanır.|GeoType, ApiName, kimlik doğrulaması|

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BlobCapacity|Blob Kapasitesi|Bayt|Average|Bayt olarak depolama hesabının Blob hizmeti tarafından kullanılan depolama miktarı.|BlobType, katman|
|BLOB sayısı|Blob Sayısı|Sayı|Toplam|Depolama hesabının Blob hizmetindeki Blob sayısı.|BlobType|       |BLOB sayısı|Blob Sayısı|Count|Average|Depolama hesabının Blob hizmetindeki Blob sayısı.|BlobType, katman|
|ContainerCount|Blob Kapsayıcı Sayısı|Sayı|Ortalama|Depolama hesabının Blob hizmetindeki kapsayıcı sayısı.|Boyut yok|
|IndexCapacity|Dizin kapasite|Bayt|Average|Bayt cinsinden ADLS Gen2'ye (hiyerarşik) dizin tarafından kullanılan depolama miktarı.|Boyut yok|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı başarılı ve başarısız isteklerin yanı sıra hataları üretilen istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın.|ResponseType, GeoType, ApiName, kimlik doğrulaması|
|Giriş|Giriş|Bayt|Toplam|Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|GeoType, ApiName, kimlik doğrulaması|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|GeoType, ApiName, kimlik doğrulaması|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer AverageE2ELatency'de belirtilen ağ gecikmesini içermez.|GeoType, ApiName, kimlik doğrulaması|
|Başarı E2e|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|GeoType, ApiName, kimlik doğrulaması|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik TotalBillableRequests değeri ve beklenmeyen hata üreten dahil ilgili istek sayısına göre bölme göre hesaplanır. Azaltılmış kullanılabilirlik ve depolama hizmetine veya belirtilen API işlemi için beklenmeyen tüm hatalar sonuçlanır.|GeoType, ApiName, kimlik doğrulaması|

## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft.Storage/storageAccounts/fileServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|FileCapacity|Dosya kapasitesi|Bayt|Average|Bayt olarak depolama hesabının dosya hizmeti tarafından kullanılan depolama miktarı.|Boyut yok|
|FileCount|Dosya sayısı|Count|Average|Depolama hesabının dosya hizmetindeki dosya sayısı.|Boyut yok|
|FileShareCount|Dosya paylaşım sayısı|Count|Average|Depolama hesabının dosya hizmetindeki dosya sayısı paylaşır.|Boyut yok|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı başarılı ve başarısız isteklerin yanı sıra hataları üretilen istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın.|ResponseType, GeoType, ApiName, kimlik doğrulaması|
|Giriş|Giriş|Bayt|Toplam|Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|GeoType, ApiName, kimlik doğrulaması|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|GeoType, ApiName, kimlik doğrulaması|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer AverageE2ELatency'de belirtilen ağ gecikmesini içermez.|GeoType, ApiName, kimlik doğrulaması|
|Başarı E2e|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|GeoType, ApiName, kimlik doğrulaması|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik TotalBillableRequests değeri ve beklenmeyen hata üreten dahil ilgili istek sayısına göre bölme göre hesaplanır. Azaltılmış kullanılabilirlik ve depolama hizmetine veya belirtilen API işlemi için beklenmeyen tüm hatalar sonuçlanır.|GeoType, ApiName, kimlik doğrulaması|

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|QueueCapacity|Kuyruk Kapasitesi|Bayt|Ortalama|Bayt olarak depolama hesabının Kuyruk hizmeti tarafından kullanılan depolama miktarı.|Boyut yok|
|QueueCount|Kuyruk Sayısı|Sayı|Ortalama|Depolama hesabının Kuyruk hizmetindeki sıra sayısı.|Boyut yok|
|QueueMessageCount|Kuyruk İleti Sayısı|Sayı|Ortalama|Depolama hesabının Kuyruk hizmetindeki sıra iletilerinin yaklaşık sayısı.|Boyut yok|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı başarılı ve başarısız isteklerin yanı sıra hataları üretilen istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın.|ResponseType, GeoType, ApiName, kimlik doğrulaması|
|Giriş|Giriş|Bayt|Toplam|Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|GeoType, ApiName, kimlik doğrulaması|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|GeoType, ApiName, kimlik doğrulaması|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer AverageE2ELatency'de belirtilen ağ gecikmesini içermez.|GeoType, ApiName, kimlik doğrulaması|
|Başarı E2e|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|GeoType, ApiName, kimlik doğrulaması|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik TotalBillableRequests değeri ve beklenmeyen hata üreten dahil ilgili istek sayısına göre bölme göre hesaplanır. Azaltılmış kullanılabilirlik ve depolama hizmetine veya belirtilen API işlemi için beklenmeyen tüm hatalar sonuçlanır.|GeoType, ApiName, kimlik doğrulaması|

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TableCapacity|Tablo Kapasitesi|Bayt|Ortalama|Bayt olarak depolama hesabının Tablo hizmeti tarafından kullanılan depolama miktarı.|Boyut yok|
|TableCount|Tablo Sayısı|Sayı|Ortalama|Depolama hesabının Tablo hizmetindeki tablo sayısı.|Boyut yok|
|TableEntityCount|Tablo Varlık Sayısı|Sayı|Ortalama|Depolama hesabının Tablo hizmetindeki tablo varlıklarının sayısı.|Boyut yok|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı başarılı ve başarısız isteklerin yanı sıra hataları üretilen istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın.|ResponseType, GeoType, ApiName, kimlik doğrulaması|
|Giriş|Giriş|Bayt|Toplam|Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|GeoType, ApiName, kimlik doğrulaması|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|GeoType, ApiName, kimlik doğrulaması|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer AverageE2ELatency'de belirtilen ağ gecikmesini içermez.|GeoType, ApiName, kimlik doğrulaması|
|Başarı E2e|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|GeoType, ApiName, kimlik doğrulaması|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik TotalBillableRequests değeri ve beklenmeyen hata üreten dahil ilgili istek sayısına göre bölme göre hesaplanır. Azaltılmış kullanılabilirlik ve depolama hizmetine veya belirtilen API işlemi için beklenmeyen tüm hatalar sonuçlanır.|GeoType, ApiName, kimlik doğrulaması|

## <a name="microsoftstoragesyncstoragesyncservices"></a>microsoft.storagesync/storageSyncServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ServerSyncSessionResult|Eşitleme oturumu sonucu|Count|Average|Bulut uç noktası ile bir eşitleme oturumu günlükleri değeri 1 her bir sunucu uç noktası başarıyla zaman ölçümü tamamlar|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncSyncSessionAppliedFilesCount|Eşitlenmiş dosyaları|Count|Toplam|Eşitlenmiş dosyaları sayısı|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncSyncSessionPerItemErrorsCount|Dosyalar eşitleniyor değil|Count|Toplam|Eşitlenemeyen dosya sayısı|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncBatchTransferredFileBytes|Eşitlenen bayt|Bayt|Toplam|Eşitleme oturumlarına aktarılan toplam dosya boyutu|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncServerHeartbeat|Sunucu çevrimiçi durumu|Count|Maksimum|Bulut uç noktası ile bir sinyal günlükleri her 1 değerini kayıtlı sunucu başarıyla zaman ölçümü kaydeder|ServerName|
|StorageSyncRecallIOTotalSizeBytes|Bulut katmanlaması geri çağırma|Bayt|Toplam|Sunucu tarafından geri verilerin toplam boyutu|ServerName|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ResourceUtilization|SU Kullanım Yüzdesi|Percent|Maksimum|SU Kullanım Yüzdesi|LogicalName, bölüm kimliği|
|Inputevents|Giriş Olayları|Count|Toplam|Giriş Olayları|LogicalName, bölüm kimliği|
|InputEventBytes|Giriş Olayı Bayt Sayısı|Bayt|Toplam|Giriş Olayı Bayt Sayısı|LogicalName, bölüm kimliği|
|LateInputEvents|Geç Giriş Olayları|Count|Toplam|Geç Giriş Olayları|LogicalName, bölüm kimliği|
|OutputEvents|Çıkış Olayları|Count|Toplam|Çıkış Olayları|LogicalName, bölüm kimliği|
|ConversionErrors|Veri Dönüştürme Hataları|Count|Toplam|Veri Dönüştürme Hataları|LogicalName, bölüm kimliği|
|Hatalar|Çalışma Zamanı Hataları|Count|Toplam|Çalışma Zamanı Hataları|LogicalName, bölüm kimliği|
|DroppedOrAdjustedEvents|Sıralama dışı olaylar|Count|Toplam|Sıralama dışı olaylar|LogicalName, bölüm kimliği|
|AMLCalloutRequests|İşlev İstekleri|Count|Toplam|İşlev İstekleri|LogicalName, bölüm kimliği|
|AMLCalloutFailedRequests|Başarısız İşlev İstekleri|Count|Toplam|Başarısız İşlev İstekleri|LogicalName, bölüm kimliği|
|AMLCalloutInputEvents|İşlev Olayları|Count|Toplam|İşlev Olayları|LogicalName, bölüm kimliği|
|DeserializationError|Giriş Serileştirme Kaldırma Hataları|Count|Toplam|Giriş Serileştirme Kaldırma Hataları|LogicalName, bölüm kimliği|
|EarlyInputEvents|Erken Giriş Olayları|Count|Toplam|Erken Giriş Olayları|LogicalName, bölüm kimliği|
|OutputWatermarkDelaySeconds|Eşik Gecikmesi|Saniye|Maksimum|Eşik Gecikmesi|LogicalName, bölüm kimliği|
|InputEventsSourcesBacklogged|Biriktirme Listesindeki Giriş Olayları|Count|Maksimum|Biriktirme Listesindeki Giriş Olayları|LogicalName, bölüm kimliği|
|InputEventsSourcesPerSecond|Alınan Giriş Kaynakları|Count|Toplam|Alınan Giriş Kaynakları|LogicalName, bölüm kimliği|

## <a name="microsofttimeseriesinsightsenvironments"></a>Microsoft.TimeSeriesInsights/environments

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|IngressReceivedMessages|Giriş iletileri alındı|Count|Toplam|Tüm olay hub'ı veya IOT hub'ı olay kaynakları okuma ileti sayısı|Boyut yok|
|IngressReceivedInvalidMessages|Giriş geçersiz ileti alındı.|Count|Toplam|Tüm olay hub'ı veya IOT hub'ı olay kaynakları okuma geçersiz ileti sayısı|Boyut yok|
|IngressReceivedBytes|Giriş alınan bayt|Bayt|Toplam|Tüm olay kaynaklarından okunan bayt sayısı|Boyut yok|
|IngressStoredBytes|Giriş bayt depolanan|Bayt|Toplam|Olayların başarıyla işlendi ve sorgu için kullanılabilir toplam boyutu|Boyut yok|
|IngressStoredEvents|Giriş olayları depolanan|Count|Toplam|Düzleştirilmiş olayları başarıyla işlendi ve sorgu sayısı|Boyut yok|
|IngressReceivedMessagesTimeLag|Alınan iletilerin zaman gecikmesini giriş|Saniye|Maksimum|İleti sıraya alınan olay kaynağındaki zamanı giriş işlendiği saati arasındaki farkı|Boyut yok|
|IngressReceivedMessagesCountLag|Giriş alınan iletilerin sayısını gecikme|Count|Average|Son sıradaki ileti sıra numarası arasındaki fark, giriş işlenmekte olan ileti bölüm ve sıra sayısı olay kaynağı|Boyut yok|
|WarmStorageMaxProperties|Orta Gecikmeli depolama Max özellikleri|Count|Maksimum|S1/S2 SKU ve özellikleri tarafından sıcak Store PAYG SKU'su için izin verilen en fazla sayısını ortam tarafından izin verilen sayısı kullanılan özellikleri|Boyut yok|
|WarmStorageUsedProperties|Orta Gecikmeli depolama kullanılan özellikleri |Count|Maksimum|S1/S2 SKU ve PAYG SKU için yarı etkin Store tarafından kullanılan özellikleri sayısı için ortamı tarafından kullanılan özellik sayısı|Boyut yok|

## <a name="microsofttimeseriesinsightsenvironmentseventsources"></a>Microsoft.TimeSeriesInsights/environments/eventsources

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|IngressReceivedMessages|Giriş iletileri alındı|Count|Toplam|İletilerin olay kaynağından okuma sayısı|Boyut yok|
|IngressReceivedInvalidMessages|Giriş geçersiz ileti alındı.|Count|Toplam|Olay kaynağından okuma geçersiz ileti sayısı|Boyut yok|
|IngressReceivedBytes|Giriş alınan bayt|Bayt|Toplam|Olay kaynağından okunan bayt sayısı|Boyut yok|
|IngressStoredBytes|Giriş bayt depolanan|Bayt|Toplam|Olayların başarıyla işlendi ve sorgu için kullanılabilir toplam boyutu|Boyut yok|
|IngressStoredEvents|Giriş olayları depolanan|Count|Toplam|Düzleştirilmiş olayları başarıyla işlendi ve sorgu sayısı|Boyut yok|
|IngressReceivedMessagesTimeLag|Alınan iletilerin zaman gecikmesini giriş|Saniye|Maksimum|İleti sıraya alınan olay kaynağındaki zamanı giriş işlendiği saati arasındaki farkı|Boyut yok|
|IngressReceivedMessagesCountLag|Giriş alınan iletilerin sayısını gecikme|Count|Average|Son sıradaki ileti sıra numarası arasındaki fark, giriş işlenmekte olan ileti bölüm ve sıra sayısı olay kaynağı|Boyut yok|
|WarmStorageMaxProperties|Orta Gecikmeli depolama Max özellikleri|Count|Maksimum|S1/S2 SKU ve özellikleri tarafından sıcak Store PAYG SKU'su için izin verilen en fazla sayısını ortam tarafından izin verilen sayısı kullanılan özellikleri|Boyut yok|
|WarmStorageUsedProperties|Orta Gecikmeli depolama kullanılan özellikleri |Count|Maksimum|S1/S2 SKU ve PAYG SKU için yarı etkin Store tarafından kullanılan özellikleri sayısı için ortamı tarafından kullanılan özellik sayısı|Boyut yok|

## <a name="microsoftvmwarecloudsimplevirtualmachines"></a>Microsoft.VMwareCloudSimple/virtualMachines

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|DiskReadBytesPerSecond|Disk okuma bayt/sn|BytesPerSecond|Average|Örnek süre okuma işlemleri nedeniyle ortalama disk aktarım hızı.|Boyut yok|
|DiskWriteBytesPerSecond|Disk Yazma Bayt/sn|BytesPerSecond|Average|Örnek süre yazma işlemleri nedeniyle ortalama disk aktarım hızı.|Boyut yok|
|Disk okuma bayt sayısı|Disk okuma bayt sayısı|Bayt|Toplam|Örnek süre okuma işlemleri nedeniyle toplam disk aktarım hızı.|Boyut yok|
|Disk yazma bayt sayısı|Disk yazma bayt sayısı|Bayt|Toplam|Örnek süre yazma işlemleri nedeniyle toplam disk aktarım hızı.|Boyut yok|
|DiskReadOperations|Disk okuma işlemleri|Count|Toplam|Önceki örnek döneme okuma işlemlerinin g/ç sayısı. Bu işlemler, değişken boyutlu olabileceğine dikkat edin.|Boyut yok|
|DiskWriteOperations|Disk yazma işlemleri|Count|Toplam|Önceki örnek döneme yazma işlemlerini GÇ sayısı. Bu işlemler, değişken boyutlu olabileceğine dikkat edin.|Boyut yok|
|Disk okuma işlemi/sn|Disk okuma işlemi/sn|CountPerSecond|Average|Okuma işlemleri önceki örnek dönemdeki ortalama g/ç sayısı. Bu işlemler, değişken boyutlu olabileceğine dikkat edin.|Boyut yok|
|Disk yazma işlemi/sn|Disk yazma işlemi/sn|CountPerSecond|Average|Önceki örnek dönemdeki ortalama g/ç sayısı yazma işlemlerini. Bu işlemler, değişken boyutlu olabileceğine dikkat edin.|Boyut yok|
|DiskReadLatency|Disk okuma gecikmesi|Milisaniye|Average|Toplam Okuma gecikme süresi. Cihaz ve çekirdek gecikmeleri okuyun.|Boyut yok|
|DiskWriteLatency|Disk yazma gecikmesi|Milisaniye|Average|Toplam yazma gecikme süresi. Cihaz ve çekirdek yazma gecikme süresi.|Boyut yok|
|NetworkInBytesPerSecond|Bayt/sn ağ|BytesPerSecond|Average|Alınan trafik için ortalama ağ aktarım hızı.|Boyut yok|
|NetworkOutBytesPerSecond|Çıkış bayt/sn ağ|BytesPerSecond|Average|Ortalama ağ aktarım hızı için iletilen trafiği.|Boyut yok|
|Ağ Girişi|Ağ Girişi|Bayt|Toplam|Alınan trafik için toplam ağ aktarım hızı.|Boyut yok|
|Ağ Çıkışı|Ağ Çıkışı|Bayt|Toplam|Toplam ağ aktarım hızı için iletilen trafiği.|Boyut yok|
|MemoryUsed|Kullanılan bellek|Bayt|Average|VM tarafından kullanılmakta olan makine bellek miktarı.|Boyut yok|
|MemoryGranted|Verilen bellek|Bayt|Average|VM konak tarafından verilen bellek miktarı. Bellek, konağı bir kez dokunulan ve verilen bellek takas veya olabilir VMkernel bellek gerekiyorsa hemen ballooned kadar atanmaz.|Boyut yok|
|MemoryActive|Bellek etkin|Bayt|Average|Süresi geçen küçük penceresinde VM tarafından kullanılan bellek miktarı. "True" sayısı ne kadar bellek sanal makine şu anda gereksinimi varsa budur. Ek, kullanılmayan bellek takas veya herhangi bir etkisi ile konuğun performans ballooned olabilir.|Boyut yok|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Average|CPU kullanımı. Bu değer, %100 temsil eden sistem üzerindeki tüm işlemci çekirdeği ile bildirilir. Örneğin, dört çekirdek sistem yüzdesi 50 kullanarak 2 yönlü VM tamamen iki çekirdek kullanıyor.|Boyut yok|
|PercentageCpuReady|CPU yüzdesi için hazır|Milisaniye|Toplam|Hazır olma saatinin vakit CPU(s) son güncelleştirme aralığı kullanılabilir hale gelmesi beklenirken ' dir.|Boyut yok|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CpuPercentage|CPU Yüzdesi|Percent|Average|CPU Yüzdesi|Örnek|
|MemoryPercentage|Bellek yüzdesi|Percent|Average|Bellek yüzdesi|Örnek|
|DiskQueueLength|Disk kuyruğu uzunluğu|Count|Average|Disk kuyruğu uzunluğu|Örnek|
|HttpQueueLength|HTTP kuyruk uzunluğu|Count|Average|HTTP kuyruk uzunluğu|Örnek|
|BytesReceived|Verileri|Bayt|Toplam|Verileri|Örnek|
|BytesSent|Veri çıkışı|Bayt|Toplam|Veri çıkışı|Örnek|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft.Web/sites (işlevler hariç)

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU zamanı|CPU süresi|Saniye|Toplam|CPU süresi|Örnek|
|İstekler|İstekler|Count|Toplam|İstekler|Örnek|
|BytesReceived|Verileri|Bayt|Toplam|Verileri|Örnek|
|BytesSent|Veri çıkışı|Bayt|Toplam|Veri çıkışı|Örnek|
|Http101|HTTP 101|Count|Toplam|HTTP 101|Örnek|
|Http2xx|HTTP 2xx|Count|Toplam|HTTP 2xx|Örnek|
|Http3xx|HTTP 3xx|Count|Toplam|HTTP 3xx|Örnek|
|Http401|HTTP 401|Count|Toplam|HTTP 401|Örnek|
|Http403|HTTP 403|Count|Toplam|HTTP 403|Örnek|
|Http404|HTTP 404|Count|Toplam|HTTP 404|Örnek|
|Http406|HTTP 406|Count|Toplam|HTTP 406|Örnek|
|Http4xx|HTTP 4xx|Count|Toplam|HTTP 4xx|Örnek|
|Http5xx|HTTP sunucu hataları|Count|Toplam|HTTP sunucu hataları|Örnek|
|MemoryWorkingSet|Bellek çalışma kümesi|Bayt|Average|Bellek çalışma kümesi|Örnek|
|AverageMemoryWorkingSet|Ortalama bellek çalışma kümesi|Bayt|Average|Ortalama bellek çalışma kümesi|Örnek|
|AverageResponseTime|Ortalama yanıt süresi|Saniye|Average|Ortalama yanıt süresi|Örnek|
|AppConnections|Bağlantılar|Count|Average|Bağlantılar|Örnek|
|İşleme|Tanıtıcı sayısı|Count|Average|Tanıtıcı sayısı|Örnek|
|İş Parçacıkları|İş parçacığı sayısı|Count|Average|İş parçacığı sayısı|Örnek|
|PrivateBytes|Özel bayt sayısı|Bayt|Average|Özel bayt sayısı|Örnek|
|IoReadBytesPerSecond|GÇ Okunan Bayt / saniye|BytesPerSecond|Toplam|GÇ Okunan Bayt / saniye|Örnek|
|IoWriteBytesPerSecond|GÇ yazılan bayt / saniye|BytesPerSecond|Toplam|GÇ yazılan bayt / saniye|Örnek|
|IoOtherBytesPerSecond|GÇ diğer bayt / saniye|BytesPerSecond|Toplam|GÇ diğer bayt / saniye|Örnek|
|IoReadOperationsPerSecond|GÇ Okuma işlemi / saniye|BytesPerSecond|Toplam|GÇ Okuma işlemi / saniye|Örnek|
|IoWriteOperationsPerSecond|GÇ Yazma işlemi / saniye|BytesPerSecond|Toplam|GÇ Yazma işlemi / saniye|Örnek|
|IoOtherOperationsPerSecond|GÇ diğer işlemler / saniye|BytesPerSecond|Toplam|GÇ diğer işlemler / saniye|Örnek|
|RequestsInApplicationQueue|Uygulama kuyruğundaki istekler|Count|Average|Uygulama kuyruğundaki istekler|Örnek|
|CurrentAssemblies|Geçerli derlemeler|Count|Average|Geçerli derlemeler|Örnek|
|TotalAppDomains|Toplam uygulama etki alanları|Count|Average|Toplam uygulama etki alanları|Örnek|
|TotalAppDomainsUnloaded|Yüklenmemiş toplam uygulama etki alanları|Count|Average|Yüklenmemiş toplam uygulama etki alanları|Örnek|
|Gen0Collections|Gen 0 atık toplamaları|Count|Toplam|Gen 0 atık toplamaları|Örnek|
|Gen1Collections|Gen 1 çöp koleksiyonları|Count|Toplam|Gen 1 çöp koleksiyonları|Örnek|
|Gen2Collections|Gen 2 Atık toplamaları|Count|Toplam|Gen 2 Atık toplamaları|Örnek|

## <a name="microsoftwebsites-functions"></a>Microsoft.Web/sites (işlevler)

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BytesReceived|Verileri|Bayt|Toplam|Verileri|Örnek|
|BytesSent|Veri çıkışı|Bayt|Toplam|Veri çıkışı|Örnek|
|Http5xx|HTTP sunucu hataları|Count|Toplam|HTTP sunucu hataları|Örnek|
|MemoryWorkingSet|Bellek çalışma kümesi|Bayt|Average|Bellek çalışma kümesi|Örnek|
|AverageMemoryWorkingSet|Ortalama bellek çalışma kümesi|Bayt|Average|Ortalama bellek çalışma kümesi|Örnek|
|FunctionExecutionUnits|İşlev yürütme birimleri|MB / milisaniye|Toplam|[İşlev yürütme birimleri](https://github.com/Azure/Azure-Functions/wiki/Consumption-Plan-Cost-Billing-FAQ#how-can-i-view-graphs-of-execution-count-and-gb-seconds)|Örnek|
|İşlev yürütme sayısı|İşlev yürütme sayısı|Count|Toplam|İşlev yürütme sayısı|Örnek|
|PrivateBytes|Özel bayt sayısı|Bayt|Average|Özel bayt sayısı|Örnek|
|IoReadBytesPerSecond|GÇ Okunan Bayt / saniye|BytesPerSecond|Toplam|GÇ Okunan Bayt / saniye|Örnek|
|IoWriteBytesPerSecond|GÇ yazılan bayt / saniye|BytesPerSecond|Toplam|GÇ yazılan bayt / saniye|Örnek|
|IoOtherBytesPerSecond|GÇ diğer bayt / saniye|BytesPerSecond|Toplam|GÇ diğer bayt / saniye|Örnek|
|IoReadOperationsPerSecond|GÇ Okuma işlemi / saniye|BytesPerSecond|Toplam|GÇ Okuma işlemi / saniye|Örnek|
|IoWriteOperationsPerSecond|GÇ Yazma işlemi / saniye|BytesPerSecond|Toplam|GÇ Yazma işlemi / saniye|Örnek|
|IoOtherOperationsPerSecond|GÇ diğer işlemler / saniye|BytesPerSecond|Toplam|GÇ diğer işlemler / saniye|Örnek|
|RequestsInApplicationQueue|Uygulama kuyruğundaki istekler|Count|Average|Uygulama kuyruğundaki istekler|Örnek|
|CurrentAssemblies|Geçerli derlemeler|Count|Average|Geçerli derlemeler|Örnek|
|TotalAppDomains|Toplam uygulama etki alanları|Count|Average|Toplam uygulama etki alanları|Örnek|
|TotalAppDomainsUnloaded|Yüklenmemiş toplam uygulama etki alanları|Count|Average|Yüklenmemiş toplam uygulama etki alanları|Örnek|
|Gen0Collections|Gen 0 atık toplamaları|Count|Toplam|Gen 0 atık toplamaları|Örnek|
|Gen1Collections|Gen 1 çöp koleksiyonları|Count|Toplam|Gen 1 çöp koleksiyonları|Örnek|
|Gen2Collections|Gen 2 Atık toplamaları|Count|Toplam|Gen 2 Atık toplamaları|Örnek|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU zamanı|CPU süresi|Saniye|Toplam|CPU süresi|Örnek|
|İstekler|İstekler|Count|Toplam|İstekler|Örnek|
|BytesReceived|Verileri|Bayt|Toplam|Verileri|Örnek|
|BytesSent|Veri çıkışı|Bayt|Toplam|Veri çıkışı|Örnek|
|Http101|HTTP 101|Count|Toplam|HTTP 101|Örnek|
|Http2xx|HTTP 2xx|Count|Toplam|HTTP 2xx|Örnek|
|Http3xx|HTTP 3xx|Count|Toplam|HTTP 3xx|Örnek|
|Http401|HTTP 401|Count|Toplam|HTTP 401|Örnek|
|Http403|HTTP 403|Count|Toplam|HTTP 403|Örnek|
|Http404|HTTP 404|Count|Toplam|HTTP 404|Örnek|
|Http406|HTTP 406|Count|Toplam|HTTP 406|Örnek|
|Http4xx|HTTP 4xx|Count|Toplam|HTTP 4xx|Örnek|
|Http5xx|HTTP sunucu hataları|Count|Toplam|HTTP sunucu hataları|Örnek|
|MemoryWorkingSet|Bellek çalışma kümesi|Bayt|Average|Bellek çalışma kümesi|Örnek|
|AverageMemoryWorkingSet|Ortalama bellek çalışma kümesi|Bayt|Average|Ortalama bellek çalışma kümesi|Örnek|
|AverageResponseTime|Ortalama yanıt süresi|Saniye|Average|Ortalama yanıt süresi|Örnek|
|FunctionExecutionUnits|İşlev yürütme birimleri|Count|Toplam|İşlev yürütme birimleri|Örnek|
|İşlev yürütme sayısı|İşlev yürütme sayısı|Count|Toplam|İşlev yürütme sayısı|Örnek|
|AppConnections|Bağlantılar|Count|Average|Bağlantılar|Örnek|
|İşleme|Tanıtıcı sayısı|Count|Average|Tanıtıcı sayısı|Örnek|
|İş Parçacıkları|İş parçacığı sayısı|Count|Average|İş parçacığı sayısı|Örnek|
|PrivateBytes|Özel bayt sayısı|Bayt|Average|Özel bayt sayısı|Örnek|
|IoReadBytesPerSecond|GÇ Okunan Bayt / saniye|BytesPerSecond|Toplam|GÇ Okunan Bayt / saniye|Örnek|
|IoWriteBytesPerSecond|GÇ yazılan bayt / saniye|BytesPerSecond|Toplam|GÇ yazılan bayt / saniye|Örnek|
|IoOtherBytesPerSecond|GÇ diğer bayt / saniye|BytesPerSecond|Toplam|GÇ diğer bayt / saniye|Örnek|
|IoReadOperationsPerSecond|GÇ Okuma işlemi / saniye|BytesPerSecond|Toplam|GÇ Okuma işlemi / saniye|Örnek|
|IoWriteOperationsPerSecond|GÇ Yazma işlemi / saniye|BytesPerSecond|Toplam|GÇ Yazma işlemi / saniye|Örnek|
|IoOtherOperationsPerSecond|GÇ diğer işlemler / saniye|BytesPerSecond|Toplam|GÇ diğer işlemler / saniye|Örnek|
|RequestsInApplicationQueue|Uygulama kuyruğundaki istekler|Count|Average|Uygulama kuyruğundaki istekler|Örnek|
|CurrentAssemblies|Geçerli derlemeler|Count|Average|Geçerli derlemeler|Örnek|
|TotalAppDomains|Toplam uygulama etki alanları|Count|Average|Toplam uygulama etki alanları|Örnek|
|TotalAppDomainsUnloaded|Yüklenmemiş toplam uygulama etki alanları|Count|Average|Yüklenmemiş toplam uygulama etki alanları|Örnek|
|Gen0Collections|Gen 0 atık toplamaları|Count|Toplam|Gen 0 atık toplamaları|Örnek|
|Gen1Collections|Gen 1 çöp koleksiyonları|Count|Toplam|Gen 1 çöp koleksiyonları|Örnek|
|Gen2Collections|Gen 2 Atık toplamaları|Count|Toplam|Gen 2 Atık toplamaları|Örnek|

## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft.Web/hostingEnvironments/multiRolePools

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|İstekler|İstekler|Count|Toplam|İstekler|Örnek|
|BytesReceived|Verileri|Bayt|Toplam|Verileri|Örnek|
|BytesSent|Veri çıkışı|Bayt|Toplam|Veri çıkışı|Örnek|
|Http101|HTTP 101|Count|Toplam|HTTP 101|Örnek|
|Http2xx|HTTP 2xx|Count|Toplam|HTTP 2xx|Örnek|
|Http3xx|HTTP 3xx|Count|Toplam|HTTP 3xx|Örnek|
|Http401|HTTP 401|Count|Toplam|HTTP 401|Örnek|
|Http403|HTTP 403|Count|Toplam|HTTP 403|Örnek|
|Http404|HTTP 404|Count|Toplam|HTTP 404|Örnek|
|Http406|HTTP 406|Count|Toplam|HTTP 406|Örnek|
|Http4xx|HTTP 4xx|Count|Toplam|HTTP 4xx|Örnek|
|Http5xx|HTTP sunucu hataları|Count|Toplam|HTTP sunucu hataları|Örnek|
|AverageResponseTime|Ortalama yanıt süresi|Saniye|Average|Ortalama yanıt süresi|Örnek|
|CpuPercentage|CPU Yüzdesi|Percent|Average|CPU Yüzdesi|Örnek|
|MemoryPercentage|Bellek yüzdesi|Percent|Average|Bellek yüzdesi|Örnek|
|DiskQueueLength|Disk kuyruğu uzunluğu|Count|Average|Disk kuyruğu uzunluğu|Örnek|
|HttpQueueLength|HTTP kuyruk uzunluğu|Count|Average|HTTP kuyruk uzunluğu|Örnek|
|ActiveRequests|Etkin istekler|Count|Toplam|Etkin istekler|Örnek|
|TotalFrontEnds|Toplam ön Uçlar|Count|Average|Toplam ön Uçlar|Boyut yok|
|SmallAppServicePlanInstances|Kısa App Service planı çalışanları|Count|Average|Kısa App Service planı çalışanları|Boyut yok|
|MediumAppServicePlanInstances|Orta App Service planı çalışanları|Count|Average|Orta App Service planı çalışanları|Boyut yok|
|LargeAppServicePlanInstances|Uzun App Service planı çalışanları|Count|Average|Uzun App Service planı çalışanları|Boyut yok|

## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft.Web/hostingEnvironments/workerPools

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Bugün için WorkersTotal|Toplam çalışanlar|Count|Average|Toplam çalışanlar|Boyut yok|
|WorkersAvailable|Kullanılabilir çalışanlar|Count|Average|Kullanılabilir çalışanlar|Boyut yok|
|WorkersUsed|Kullanılan çalışanlar|Count|Average|Kullanılan çalışanlar|Boyut yok|
|CpuPercentage|CPU Yüzdesi|Percent|Average|CPU Yüzdesi|Örnek|
|MemoryPercentage|Bellek yüzdesi|Percent|Average|Bellek yüzdesi|Örnek|

## <a name="next-steps"></a>Sonraki adımlar
* [Azure İzleyici'de ölçümleri hakkında bilgi edinin](data-platform.md)
* [Ölçümler üzerinde uyarı oluşturma](alerts-overview.md)
* [Depolama, olay hub'ı veya Log Analytics ölçümleri dışarı aktarma](diagnostic-logs-overview.md)
