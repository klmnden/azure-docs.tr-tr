---
title: Gönüllü bir geçiş aracı Azure İzleyici uyarılar için nasıl çalıştığını anlamak
description: Uyarılar Geçiş Aracı nasıl çalıştığını anlamak ve sorunlarını giderebilirsiniz.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: 015000388c5629dbd8ed8833931a809ebd738bd6
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67295520"
---
# <a name="understand-how-the-migration-tool-works"></a>Geçiş Aracı nasıl çalıştığını anlamak

Olarak [daha önce duyurulduğu gibi](monitoring-classic-retirement.md), Azure İzleyici'de klasik uyarılar, 31 Ağustos 2019 tarafından kullanımdan (başlangıçta 30 Haziran 2019 oluştu). Geçiş Aracı, Azure portalında Klasik uyarı kuralları kullanan ve kendilerini geçişini isteyen müşteriler için kullanılabilir.

Bu makalede, gönüllü bir geçiş aracının nasıl çalıştığı açıklanmaktadır. Ayrıca, çözümler için bazı yaygın sorunlar açıklanmaktadır.

> [!NOTE]
> Sunum geçiş aracının gecikme nedeniyle, o tarihten Klasik uyarılar geçiş yapıldı [31 Ağustos 2019'için Genişletilmiş](https://azure.microsoft.com/updates/azure-monitor-classic-alerts-retirement-date-extended-to-august-31st-2019/) ilk duyurulan tarihinden 30 Haziran 2019.

## <a name="classic-alert-rules-that-will-not-be-migrated"></a>Geçirilmez Klasik uyarı kuralları

> [!IMPORTANT]
> Etkinlik günlüğü uyarıları (dahil olmak üzere hizmet sistem durumu uyarılarını) ve günlük uyarıları geçişten etkilenmez. Geçiş yalnızca açıklanan Klasik uyarı kuralları geçerlidir [burada](monitoring-classic-retirement.md#retirement-of-classic-monitoring-and-alerting-platform).

Aracı neredeyse tüm geçirmeniz mümkün olmakla birlikte [Klasik uyarı kuralları](monitoring-classic-retirement.md#retirement-of-classic-monitoring-and-alerting-platform), bazı özel durumlar vardır. Aşağıdaki uyarı kuralları aracını kullanarak (veya Eylül 2019 başlangıç otomatik geçiş sırasında) geçirilmeyecektir:

- Sanal makine Konuk ölçümleri (hem Windows hem de Linux) üzerinde Klasik uyarı kuralları. Bkz: [yeni ölçüm uyarıları bu uyarı kuralları yeniden oluşturmak için yönergeler](#guest-metrics-on-virtual-machines) bu makalenin ilerleyen bölümlerinde.
- Klasik depolama ölçümleri Klasik uyarı kuralları. Bkz: [izleme Klasik depolama hesaplarınızı rehberlik](https://azure.microsoft.com/blog/modernize-alerting-using-arm-storage-accounts/).
- Bazı depolama hesabı ölçümleri Klasik uyarı kuralları. Bkz: [ayrıntıları](#storage-account-metrics) bu makalenin ilerleyen bölümlerinde.
- Bazı Cosmos DB ölçümler üzerinde Klasik uyarı kuralları. Ayrıntılar gelecek güncelleştirmelerden birinde eklenecektir.

Aboneliğinizi Klasik bu tür bir kurallar varsa, bunları el ile taşımanız gerekir. Otomatik geçişini sunamıyoruz çünkü bu türdeki tüm var olan ve klasik ölçüm uyarıları Haziran 2020'ye kadar çalışmaya devam eder. Bu uzantı, yeni uyarılar için kaydırmak için zaman verir. Bununla birlikte, Ağustos 2019 sonra yeni Klasik uyarı oluşturulabilir.

> [!NOTE]
> Uyarı kurallarınızı Klasik geçersiz olması durumunda yukarıda listelenen özel durumların yanı sıra, yani üzerinde olduğu [kullanım ölçümleri](#classic-alert-rules-on-deprecated-metrics) ya da silinmiş, kaynakları gönüllü bir geçiş sırasında geçirilmez. Bu tür geçersiz Klasik uyarı kuralları, otomatik geçiş gerçekleştiğinde silinir.

### <a name="guest-metrics-on-virtual-machines"></a>Sanal makinelerdeki Konuk ölçümleri

Konuk ölçümleri Konuk ölçümleri yeni ölçüm uyarıları oluşturmadan önce Azure İzleyici özel ölçümler depoya gönderilmesi gerekir. Tanılama Ayarları'nda Azure Monitor havuzu etkinleştirmek için aşağıdaki yönergeleri izleyin:

- [Windows sanal makineler için konuk ölçümlerini etkinleştirme](collect-custom-metrics-guestos-resource-manager-vm.md)
- [Linux Vm'leri için konuk ölçümlerini etkinleştirme](collect-custom-metrics-linux-telegraf.md)

Bu adımları tamamladıktan sonra konuk ölçümlerine yeni ölçüm uyarıları oluşturabilirsiniz. Ve yeni ölçüm uyarıları oluşturduktan sonra Klasik uyarıları silebilirsiniz.

### <a name="storage-account-metrics"></a>Depolama hesabı ölçümleri

Bu ölçümler ile ilgili uyarılar dışında tüm Klasik depolama hesapları uyarılarda geçirilebilir:

- PercentAuthorizationError
- PercentClientOtherError
- Percentnetworkerror'da
- PercentServerOtherError
- PercentSuccess
- Percentthrottlingerror'da
- Percenttimeouterror'da
- AnonymousThrottlingError
- SASThrottlingError
- ThrottlingError

Klasik uyarı yüzde ölçümlere ilişkin kuralları geçirilmelidir temel alarak [eski ve yeni depolama ölçümleri arasındaki eşlemeyi](https://docs.microsoft.com/azure/storage/common/storage-metrics-migration#metrics-mapping-between-old-metrics-and-new-metrics). Eşikleri mutlak bir yeni ölçüm kullanılabilir olduğu için uygun şekilde değiştirilmesi gerekir.

Aynı işlevselliği sağlayan birleştirilmiş ölçüm olduğundan AnonymousThrottlingError, SASThrottlingError ve ThrottlingError Klasik uyarı kuralları iki yeni uyarılarına genişletecektir bölmeniz gerekir. Eşiklerini uygun şekilde uyarlanabilen gerekecektir.

### <a name="classic-alert-rules-on-deprecated-metrics"></a>Kullanım dışı bırakılmış ölçümler üzerinde Klasik uyarı kuralları

Bunlar Klasik uyarı, daha önce desteklenen, ancak sonunda kullanım dışı bırakılan ölçümlere ilişkin kurallardır. Küçük bir müşteri yüzdesi geçersiz Klasik uyarı kuralları gibi ölçümlere sahip olabilir. Bu uyarı kuralları geçersiz olduğundan, bunlar geçirilmeyecektir.

| Kaynak türü| Kullanım dışı metric(s) |
|-------------|----------------- |
| Microsoft.DBforMySQL/servers | compute_consumption_percent, compute_limit |
| Microsoft.DBforPostgreSQL/servers | compute_consumption_percent, compute_limit |
| Microsoft.Network/publicIPAddresses | defaultddostriggerrate |
| Microsoft.SQL/servers/databases | service_level_objective storage_limit, storage_used, azaltma, dtu_consumption_percent, storage_used |
| Microsoft.Web/hostingEnvironments/multirolepools | averagememoryworkingset |
| Microsoft.Web/hostingEnvironments/workerpools | bytesreceived, httpqueuelength |

## <a name="how-equivalent-new-alert-rules-and-action-groups-are-created"></a>Nasıl oluşturulduğunu eşdeğer yeni uyarı kuralları ve Eylem grupları

Geçiş Aracı, eşdeğer yeni uyarı kuralları ve Eylem grupları uyarı kurallarınızı Klasik dönüştürür. Çoğu Klasik uyarı kuralları için eşdeğer yeni uyarı kuralları aynı özelliklere sahip aynı Ölçüm gibi bulunan `windowSize` ve `aggregationType`. Ancak, bazı Klasik uyarı yeni sistemde farklı, eşdeğer bir ölçüm olan ölçümlere ilişkin kurallardır vardır. Aşağıdaki ilkeleri, aşağıdaki bölümde belirtilmediği sürece Klasik uyarılar geçişi için geçerlidir:

- **Sıklık**: Klasik veya yeni bir uyarı kuralı için koşul ne sıklıkla denetleyeceğini tanımlar. `frequency` Klasik uyarı kurallarında kullanıcı tarafından yapılandırılabilir değildir ve her zaman 1 dakika bıraktığınız Application Insights bileşenlerini hariç tüm kaynak türleri için 5 dakika şeklindeydi. Bu nedenle eşdeğer kuralları sıklığını da 5 dakika ile 1 dakika ayarlanır.
- **Toplama türü**: Ölçüm ilgi pencere üzerinde nasıl toplanır tanımlar. `aggregationType` De klasik uyarılar ve yeni uyarılar için çoğu ölçümün arasında aynıdır. Ölçüm Klasik uyarılar ve yeni uyarıları, eşdeğer arasında farklı olduğundan bazı durumlarda `aggregationType` veya `primary Aggregation Type` ölçü kullanılır için tanımlanmış.
- **Birimleri**: Uyarının oluşturulduğu ölçüm özelliği. Bazı eşdeğer ölçümlerin farklı birimleri vardır. Eşik gerektiği gibi uygun şekilde ayarlanır. Örneğin, özgün ölçü birimlerini saniye ancak milisaniye birimini eşdeğer yeni ölçüm içeriyor, özgün eşiği aynı davranışı sağlamak açısından, 1000 ile çarpılır.
- **Pencere boyutu**: Ölçüm karşı eşiği Karşılaştırılacak veriler toplanır penceresini tanımlar. Standart `windowSize` 5mins, 15mins, 30mins, değerler 1 saat, 3 saat, 6 saat, 12 saat, 1 gün için eşdeğer yeni uyarı kuralı yapılan bir değişiklik yoktur. Diğer değerleri, en yakın `windowSize` kullanılmak üzere seçilir. Çoğu müşteri için bu değişiklik ile hiçbir etkisi yoktur. Müşterilerin küçük bir yüzdesi eşiği tam aynı davranışı sağlamak için ince ayar yapmanız olabilir.

Aşağıdaki bölümlerde, biz farklı, eşdeğer bir ölçüm yeni sistemde de sahip ölçümleri ayrıntılı olarak açıklanmaktadır. Klasik ve yeni uyarı kuralları için aynı kalan herhangi bir ölçümü listelenmez. Yeni sistemde de desteklenen ölçümlerin bir listesini bulabilirsiniz [burada](metrics-supported.md).

### <a name="microsoftstorageaccountsservices"></a>Microsoft.StorageAccounts/services

Blob, tablo, dosya ve kuyruk gibi depolama hesabı Hizmetleri için aşağıda gösterildiği gibi aşağıdaki ölçümleri için eşdeğer ölçümleri eşlenir:

| Klasik uyarılar ölçüm | Yeni uyarılar eşdeğer ölçümü | Açıklamalar|
|--------------------------|---------------------------------|---------|
| AnonymousAuthorizationError| İşlem ölçüm boyutları "ResponseType" ile "AuthorizationError" ve "Kimlik doğrulaması" = "Anonim" =| |
| AnonymousClientOtherError | İşlem ölçüm boyutları "ResponseType" ile "ClientOtherError" ve "Kimlik doğrulaması" = "Anonim" = | |
| AnonymousClientTimeOutError| İşlem ölçüm boyutları "ResponseType" ile "ClientTimeOutError" ve "Kimlik doğrulaması" = "Anonim" = | |
| AnonymousNetworkError | İşlem ölçüm boyutları "ResponseType" ile "NetworkError" ve "Kimlik doğrulaması" = "Anonim" = | |
| AnonymousServerOtherError | İşlem ölçüm boyutları "ResponseType" ile "ServerOtherError" ve "Kimlik doğrulaması" = "Anonim" = | |
| AnonymousServerTimeOutError | İşlem ölçüm boyutları "ResponseType" ile "ServerTimeOutError" ve "Kimlik doğrulaması" = "Anonim" = | |
| AnonymousSuccess | İşlem ölçüm boyutları "ResponseType" ile "Başarılı" ve "Kimlik doğrulaması" = "Anonim" = | |
| AuthorizationError | İşlem ölçüm boyutları "ResponseType" ile "AuthorizationError" = | |
| AverageE2ELatency | Başarı E2e | |
| AverageServerLatency | SuccessServerLatency | |
| Kapasite | BlobCapacity | Kullanım `aggregationType` 'Son' yerine ' ortalama'. Ölçüm yalnızca Blob Hizmetleri için geçerlidir |
| ClientOtherError | İşlem ölçüm boyutları "ResponseType" ile "ClientOtherError" =  | |
| ClientTimeoutError | İşlem ölçüm boyutları "ResponseType" ile "ClientTimeOutError" = | |
| ContainerCount | ContainerCount | Kullanım `aggregationType` 'Son' yerine ' ortalama'. Ölçüm yalnızca Blob Hizmetleri için geçerlidir |
| NetworkError | İşlem ölçüm boyutları "ResponseType" ile "NetworkError" = | |
| ObjectCount | BLOB sayısı| Kullanım `aggregationType` 'Son' yerine ' ortalama'. Ölçüm yalnızca Blob Hizmetleri için geçerlidir |
| SASAuthorizationError | İşlem ölçüm boyutları "ResponseType" ile "AuthorizationError" ve "Kimlik doğrulaması" = "SAS" = | |
| SASClientOtherError | İşlem ölçüm boyutları "ResponseType" ile "ClientOtherError" ve "Kimlik doğrulaması" = "SAS" = | |
| SASClientTimeOutError | İşlem ölçüm boyutları "ResponseType" ile "ClientTimeOutError" ve "Kimlik doğrulaması" = "SAS" = | |
| SASNetworkError | İşlem ölçüm boyutları "ResponseType" ile "NetworkError" ve "Kimlik doğrulaması" = "SAS" = | |
| SASServerOtherError | İşlem ölçüm boyutları "ResponseType" ile "ServerOtherError" ve "Kimlik doğrulaması" = "SAS" = | |
| SASServerTimeOutError | İşlem ölçüm boyutları "ResponseType" ile "ServerTimeOutError" ve "Kimlik doğrulaması" = "SAS" = | |
| SASSuccess | İşlem ölçüm boyutları "ResponseType" ile "Başarılı" ve "Kimlik doğrulaması" = "SAS" = | |
| ServerOtherError | İşlem ölçüm boyutları "ResponseType" ile "ServerOtherError" = | |
| ServerTimeOutError | İşlem ölçüm boyutları "ResponseType" ile "ServerTimeOutError" =  | |
| Başarılı | İşlem ölçüm boyutları "ResponseType" = "Başarılı" | |
| TotalBillableRequests| İşlemler | |
| TotalEgress | Çıkış | |
| Totalıngress | Giriş | |
| TotalRequests | İşlemler | |

### <a name="microsoftinsightscomponents"></a>Microsoft.insights/Components

Application Insights için aşağıda gösterildiği gibi eşdeğer ölçümler şunlardır:

| Klasik uyarılar ölçüm | Yeni uyarılar eşdeğer ölçümü | Açıklamalar|
|--------------------------|---------------------------------|---------|
| availability.availabilityMetric.value | availabilityResults/availabilityPercentage|   |
| availability.durationMetric.value | availabilityResults/süresi| Özgün eşiği Klasik ölçüm birimi saniyeler içinde olan ve yeni bir milisaniye olarak 1000 ile çarpın.  |
| basicExceptionBrowser.count | özel durumlar/tarayıcı|  Kullanım `aggregationType` 'toplam' yerine ' count'. |
| basicExceptionServer.count | özel durumlar/sunucu| Kullanım `aggregationType` 'toplam' yerine ' count'.  |
| clientPerformance.clientProcess.value | browserTimings/processingDuration| Özgün eşiği Klasik ölçüm birimi saniyeler içinde olan ve yeni bir milisaniye olarak 1000 ile çarpın.  |
| clientPerformance.networkConnection.value | browserTimings/networkDuration|  Özgün eşiği Klasik ölçüm birimi saniyeler içinde olan ve yeni bir milisaniye olarak 1000 ile çarpın. |
| clientPerformance.receiveRequest.value | browserTimings/receiveDuration| Özgün eşiği Klasik ölçüm birimi saniyeler içinde olan ve yeni bir milisaniye olarak 1000 ile çarpın.  |
| clientPerformance.sendRequest.value | browserTimings/sendDuration| Özgün eşiği Klasik ölçüm birimi saniyeler içinde olan ve yeni bir milisaniye olarak 1000 ile çarpın.  |
| clientPerformance.total.value | browserTimings/totalDuration| Özgün eşiği Klasik ölçüm birimi saniyeler içinde olan ve yeni bir milisaniye olarak 1000 ile çarpın.  |
| performanceCounter.available_bytes.value | performanceCounters/memoryAvailableBytes|   |
| performanceCounter.io_data_bytes_per_sec.value | performanceCounters/processIOBytesPerSecond|   |
| performanceCounter.number_of_exceps_thrown_per_sec.value | performanceCounters/exceptionsPerSecond|   |
| performanceCounter.percentage_processor_time_normalized.value | performanceCounters/processCpuPercentage|   |
| performanceCounter.percentage_processor_time.value | performanceCounters/processCpuPercentage| Eşik, tüm çekirdek arasında özgün ölçüm olan ve yeni ölçüm için bir çekirdek normalleştirilmiştir uygun şekilde değiştirilmesi gerekir. Geçiş Aracı eşikleri değiştirmez.  |
| performanceCounter.percentage_processor_total.value | performanceCounters/processorCpuPercentage|   |
| performanceCounter.process_private_bytes.value | performanceCounters/processPrivateBytes|   |
| performanceCounter.request_execution_time.value | performanceCounters/requestExecutionTime|   |
| performanceCounter.requests_in_application_queue.value | performanceCounters/requestsInQueue|   |
| performanceCounter.requests_per_sec.value | performanceCounters/requestsPerSecond|   |
| Request.Duration | istekleri/süresi| Özgün eşiği Klasik ölçüm birimi saniyeler içinde olan ve yeni bir milisaniye olarak 1000 ile çarpın.  |
| Request.rate | İstek/oranı|   |
| requestFailed.count | / başarısız istekleri| Kullanım `aggregationType` 'toplam' yerine ' count'.   |
| View.Count | Sayfa görüntülemesi/sayısı| Kullanım `aggregationType` 'toplam' yerine ' count'.   |

### <a name="how-equivalent-action-groups-are-created"></a>Eşdeğer Eylem grupları oluşturulur nasıl

Klasik uyarı kurallarınız e-posta, Web kancası, mantıksal uygulama ve runbook eylemleri uyarıya bağlı kendi kural. Yeni uyarı kuralları arasında birden çok uyarı kuralları yeniden kullanılabilir eylem gruplarını kullanın. Geçiş Aracı eylemi kaç uyarı kuralları kullanan bağımsız olarak aynı eylemleri için tek bir eylem grubu oluşturur. Geçiş Aracı tarafından oluşturulan eylem grupları adlandırma biçimi 'Migrated_AG *' kullanın.

## <a name="rollout-phases"></a>Dağıtım aşamaları

Geçiş Aracı aşamada Klasik uyarı kuralları kullanan müşterilere sunuluyor. Abonelik sahipleri, abonelik aracını kullanarak geçirilmesi hazır olduğunda bir e-posta alırsınız.

> [!NOTE]
> Aracı aşamalı olarak sunulacaktır nedeniyle bazı Abonelikleriniz henüz erken aşamalarında geçirilmesi hazır olmadığını görebilirsiniz.

Abonelikleri çoğu şu anda geçiş için hazır olarak işaretlenir. Yalnızca aşağıdaki kaynak türlerinde Klasik uyarılar sahip olan abonelikler geçiş için hala hazır değil.

- Microsoft.classicCompute/domainNames/slots/roles
- Microsoft.documentDB/databases
- Microsoft.insights/Components

## <a name="who-can-trigger-the-migration"></a>Kimin geçiş tetiklenebilir mi?

İzleme katkıda bulunan yerleşik rolünün abonelik düzeyinde olan herhangi bir kullanıcı geçiş tetikleyebilirsiniz. Özel bir rol aşağıdaki izinlere sahip olan kullanıcılar, geçiş de tetikleyebilirsiniz:

- \* / Okuma
- Microsoft.Insights/actiongroups/*
- Microsoft.Insights/AlertRules/*
- Microsoft.Insights/metricAlerts/*

> [!NOTE]
> İzinlere sahip ek olarak, aboneliğiniz ayrıca Microsoft.AlertsManagement kaynak sağlayıcısı ile kaydedilmelidir. Bu, Application Insights ile ilgili hata Anomali Uyarılar başarıyla geçirmek için gereklidir. 

## <a name="common-problems-and-remedies"></a>Yaygın sorunlar ve çözümler

Çalıştırdıktan sonra [geçişini](alerts-using-migration-tool.md), geçişin tamamlandığını veya herhangi bir işlem yapmanız gerekip gerekmediğini bildirmek için sağlanan adreslerde e-posta alırsınız. Bu bölümde, bazı yaygın sorunlar ve bunlarla nasıl açıklanmaktadır.

### <a name="validation-failed"></a>Doğrulama başarısız oldu

Aboneliğinizdeki Klasik uyarı kuralları için son değişiklikler nedeniyle bazı abonelik geçirilemez. Bu sorun geçicidir. Geçiş durumu geri taşındıktan sonra geçişi yeniden başlatabilirsiniz **geçiş için hazır** birkaç gün içinde.

### <a name="policy-or-scope-lock-preventing-us-from-migrating-your-rules"></a>Bize kurallarınızı geçiş dan engelleyen ilke veya kapsam Kilitle

Geçişin bir parçası olarak yeni ölçüm uyarıları ve yeni eylem grubu oluşturulur ve klasik uyarı kuralları ardından silinir. Ancak, bize kaynakları oluşturmasını engelleyen bir ilke veya kapsam kilit yok. İlke veya kapsam kilit bağlı olarak bazı veya tüm kuralları taşınamıyor. Kapsam kilit ya da ilke geçici olarak kaldırılması ve geçişi yeniden harekete bu sorunu çözebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Geçiş aracını kullanma](alerts-using-migration-tool.md)
- [Geçiş için hazırlama](alerts-prepare-migration.md)
