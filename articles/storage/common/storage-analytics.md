---
title: "Günlükleri ve ölçüm verilerini toplamak için Azure Storage Analytics kullanın | Microsoft Docs"
description: "Storage Analytics Ölçüm verilerini tüm depolama hizmetlerini izlemenizi ve Blob, kuyruk ve tablo depolama için günlükleri toplamak için etkinleştirir."
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 7894993b-ca42-4125-8f17-8f6dfe3dca76
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/03/2017
ms.author: tamram
ms.openlocfilehash: 9ae9dd0b078911a695d441cd3891be720dc204ac
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="storage-analytics"></a>Depolama Analizi

Azure Depolama Analizi günlük kaydı yapar ve depolama hesabı için ölçüm verileri sağlar. Bu verileri kullanarak istekleri izleyebilir, kullanım eğilimlerini çözümleyebilir ve depolama hesabınızdaki sorunları tanılayabilirsiniz.

Storage Analytics kullanmak için onu ayrı ayrı izlemek istediğiniz her hizmet için etkinleştirmelisiniz. Şuradan etkinleştirebilirsiniz [Azure Portal](https://portal.azure.com). Ayrıntılar için bkz [Azure Portal'da depolama hesabı izleme](storage-monitor-storage-account.md). Storage Analytics REST API veya istemci kitaplığı yoluyla programlı olarak etkinleştirebilirsiniz. Kullanım [Blob hizmeti özellik alma](https://msdn.microsoft.com/library/hh452239.aspx), [sıra hizmeti özelliklerini alma](https://msdn.microsoft.com/library/hh452243.aspx), [tablo hizmeti özellik alma](https://msdn.microsoft.com/library/hh452238.aspx), ve [dosya hizmeti özelliklerini alma](https://msdn.microsoft.com/library/mt427369.aspx) her hizmet için depolama çözümlemeleri etkinleştirme işlemleri.

Toplanan veriler (günlük için) iyi bilinen bir blob ve Blob hizmeti ve tablo hizmeti API'leri kullanılarak erişilebilecek (için ölçümleri), iyi bilinen tablolara depolanır.

Storage Analytics bir 20 TB depolama hesabınız için toplam sınırı bağımsızdır depolanan veri miktarına sahiptir. Faturalama ve veri bekletme ilkeleri hakkında daha fazla bilgi için bkz: [depolama çözümlemeleri ve faturalama](https://msdn.microsoft.com/library/hh360997.aspx). Depolama hesabı sınırları hakkında daha fazla bilgi için bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](storage-scalability-targets.md).

Ayrıntılı bir kılavuz tanımlamak, tanılama ve Azure Storage ile ilgili sorunları gidermek için depolama çözümlemeleri ve diğer araçları kullanma hakkında bilgi için bkz: [izleme, tanılama ve Microsoft Azure Storage sorun giderme](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="about-storage-analytics-logging"></a>Storage Analytics oturum açma hakkında
Storage Analytics bir depolama hizmetine başarılı ve başarısız istekler hakkında ayrıntılı bilgi günlüğe kaydeder. Bu bilgiler, istekleri ayrı ayrı izlemek ve depolama hizmeti ile ilgili sorunları tanılamak için kullanılabilir. İstekleri en iyi çaba ilkesine göre günlüğe kaydedilir.

Yalnızca depolama hizmeti etkinliği ise günlük girişleri oluşturulur. Örneğin, bir depolama hesabı Blob hizmeti ancak tablo veya kuyruğu hizmetlerinin etkinlik varsa, yalnızca Blob hizmetiyle ilişkili günlükleri oluşturulur.

Storage Analytics günlüğü Azure dosyaları için kullanılamıyor.

### <a name="logging-authenticated-requests"></a>Kimlik doğrulaması günlüğe kaydetme istekleri
Kimliği doğrulanmış istekler aşağıdaki türlerini kaydedilir:

* Başarılı istek sayısı.
* İstek zaman aşımı, azaltma, ağ, yetkilendirme ve başka hatalar da dahil olmak üzere, başarısız oldu.
* Başarılı ve başarısız istekleri dahil olmak üzere paylaşılan erişim imzası (SAS), kullanarak istek sayısı.
* Analiz verileri için istek sayısı.

Storage Analytics kendisini günlük oluşturma veya silme gibi tarafından yapılan istekleri günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam bir liste belgelenen [depolama Analytics günlüğe yazılan işlemler ve durum iletileri](https://msdn.microsoft.com/library/hh343260.aspx) ve [depolama Analytics günlük biçimi](https://msdn.microsoft.com/library/hh343259.aspx) Konular.

### <a name="logging-anonymous-requests"></a>Anonim istekler günlüğe kaydetme
Anonim istekler aşağıdaki türlerini kaydedilir:

* Başarılı istek sayısı.
* Sunucu hataları.
* İstemci ve sunucu zaman aşımı hataları.
* Başarısız GET istekleri 304 hata koduyla (değişiklik).

Diğer tüm başarısız anonim istekler günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam bir liste belgelenen [depolama Analytics günlüğe yazılan işlemler ve durum iletileri](https://msdn.microsoft.com/library/hh343260.aspx) ve [depolama Analytics günlük biçimi](https://msdn.microsoft.com/library/hh343259.aspx) Konular.

### <a name="how-logs-are-stored"></a>Günlükleri nasıl depolanır
Tüm günlükler blok blobları Storage Analytics bir depolama hesabı için etkinleştirildiğinde, otomatik olarak oluşturulan $logs adlı bir kapsayıcıda depolanır. $Logs kapsayıcı depolama hesabı blob ad alanında örneğin bulunur: `http://<accountname>.blob.core.windows.net/$logs`. Storage Analytics etkinleştirildikten sonra bu kapsayıcı içeriğini silinebilir silinemiyor.

> [!NOTE]
> İşlem listeleme bir kapsayıcı, gibi gerçekleştirildiğinde $logs kapsayıcı görüntülenmeyen [ListContainers](https://msdn.microsoft.com/library/azure/dd179352.aspx) yöntemi. Doğrudan erişim gerekir. Örneğin, kullanabileceğiniz [ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx) BLOB'ları erişmek için yöntemi `$logs` kapsayıcı.
> İstekleri günlüğe gibi depolama çözümlemeleri Ara sonuçların taşı olarak yükleyin. Düzenli aralıklarla, Storage Analytics bu blokları tamamlama ve bir blob olarak kullanılmasını.
> 
> 

Yinelenen kayıtları aynı saat içinde oluşturulan günlükleri için mevcut olabilir. Bir kayıt yinelenen olup olmadığını kontrol ederek belirleyebilirsiniz **RequestId** ve **işlemi** numarası.

### <a name="log-naming-conventions"></a>Günlük adlandırma kuralları
Her günlük şu biçimde yazılır.

    <service-name>/YYYY/MM/DD/hhmm/<counter>.log

Aşağıdaki tabloda günlük ad içindeki her bir öznitelik açıklanmaktadır.

| Öznitelik | Açıklama |
| --- | --- |
| < hizmet-adı > |Depolama hizmet adı. Örneğin: blob, tablo veya kuyruğu. |
| YYYY |Günlük için dört rakamlı yıl. Örneğin: 2011. |
| MM |Günlük için iki basamaklı ayı. Örneğin: 07. |
| DD |Günlük için iki basamaklı ayı. Örneğin: 07. |
| ss |İki basamaklı saat, 24 saatlik UTC biçiminde günlükleri için başlangıç saati gösterir. Örneğin: 18. |
| mm |Günlükleri için başlangıç dakika gösteren iki basamaklı bir sayı. Bu değer Storage Analytics geçerli sürümde desteklenmiyor ve değeri her zaman 00 olacaktır. |
| <counter> |Altı basamaklı bir saat süre depolama hizmeti için oluşturulan günlük BLOB'ları sayısını gösteren sıfır tabanlı bir sayacı. Bu sayaç 000000 başlatır. Örneğin: 000001. |

Önceki örneklerde birleştiren bir tam örnek günlük adını verilmiştir.

    blob/2011/07/31/1800/000001.log

Önceki günlük erişmek için kullanılan URI bir örnek verilmiştir.

    https://<accountname>.blob.core.windows.net/$logs/blob/2011/07/31/1800/000001.log

Bir depolama istek günlüğe kaydedildiğinde, sonuçta elde edilen günlük adı tamamlanmış olduğunda istenen işlem saati hatalarla ilintilidir. Örneğin, bir GetBlob istek 6: 30'da tamamlandı 7/31/2011 ' günlük şu önek ile yazılmış:`blob/2011/07/31/1800/`

### <a name="log-metadata"></a>Log meta verileri
Tüm günlük BLOB'lar blob içerdiği günlük verileri tanımlamak için kullanılan meta verileriyle depolanır. Aşağıdaki tabloda, her meta veri özniteliğinin açıklanmaktadır.

| Öznitelik | Açıklama |
| --- | --- |
| LogType |Günlük Okuma, yazma veya silme işlemleri için ilgili bilgiler içeren olup olmadığını açıklar. Bu değer, bir tür veya üçünü virgülle ayrılmış bir birleşimini içerebilir. Örnek 1: yazma; Örnek 2: Okuma, yazma; Örnek 3: Okuma, yazma, silme. |
| StartTime |YYYY biçiminde günlüğündeki bir girişi en erken zaman-aa-: ssZ. Örneğin: 2011-07-31T18:21:46Z. |
| EndTime |YYYY biçiminde günlüğündeki bir girişi son süresini-aa-: ssZ. Örneğin: 2011-07-31T18:22:09Z. |
| LogVersion |Günlük biçimi sürümü. Şu anda desteklenen tek değer 1.0 ' dir. |

Aşağıdaki listede önceki örneklerde kullanarak tam örnek meta verilerini görüntüler.

* LogType yazma =
* StartTime 2011 =-07-31T18:21:46Z
* EndTime 2011 =-07-31T18:22:09Z
* LogVersion = 1.0

### <a name="accessing-logging-data"></a>Günlük verilerine erişme
Tüm verileri `$logs` kapsayıcı Blob hizmeti API'leri kullanılarak erişilebilir, Azure tarafından sağlanan .NET API'lerini de dahil olmak üzere kitaplığı yönetilen. Depolama hesabının yöneticisine okuyun ve günlükleri, silme, ancak oluşturabilir veya bunları güncelleştirin. Günlüğün meta veri ve günlük adı için bir günlük sorgulanırken kullanılabilir. Verilen saatlik günlükleri bozuk görünmesi mümkündür, ancak meta verileri, günlük girişlerini timespan her zaman bir oturum açma belirtir. Bu nedenle, belirli bir günlük için arama yaparken günlük adları ve meta verileri bir bileşimini kullanabilirsiniz.

## <a name="about-storage-analytics-metrics"></a>Storage Analytics ölçümleri hakkında
Storage Analytics bir depolama birimi hizmeti istekleriyle ilgili toplu işlem istatistiklerini ve kapasite verilerini dahil ölçümleri depolayabilirsiniz. İşlem düzeyinde hem de API işlemi ve aynı zamanda depolama hizmeti düzeyinde bildirilir ve kapasite depolama hizmet düzeyinde bildirilir. Ölçüm verilerini depolama hizmeti kullanım çözümlemek, depolama birimi hizmeti karşı yapılan istekleri ile sorunlarını tanılamak ve bir hizmeti kullanan uygulamaların performansını artırmak için kullanılabilir.

Storage Analytics kullanmak için onu ayrı ayrı izlemek istediğiniz her hizmet için etkinleştirmelisiniz. Şuradan etkinleştirebilirsiniz [Azure Portal](https://portal.azure.com). Ayrıntılar için bkz [Azure Portal'da depolama hesabı izleme](storage-monitor-storage-account.md). Storage Analytics REST API veya istemci kitaplığı yoluyla programlı olarak etkinleştirebilirsiniz. Kullanım **hizmet özellikleri Al** her hizmet için depolama çözümlemeleri etkinleştirme işlemleri.

### <a name="transaction-metrics"></a>İşlem ölçümlerini
Güçlü bir veri kümesi istenen API işlemi, giriş/çıkış, kullanılabilirlik, hatalar dahil olmak üzere her depolama hizmeti için saat veya dakika aralıklarla kaydedilir ve istek yüzdelerini kategorilere. İşlem ayrıntıları tam bir listesini görebilir [Storage Analytics Ölçüm tablosu şeması](https://msdn.microsoft.com/library/hh343264.aspx) konu.

İşlem verileri iki düzeyde – hizmet düzeyi ve API işlem düzeyinde kaydedilir. Hizmet düzeyinde hiçbir istek hizmetinde yapılan olsa bile API işlemleri saatte bir tablo varlığına yazılan tüm özetlemeye istatistikleri istedi. İşlemi, bir saat içinde istediyseniz API işlem düzeyinde istatistikleri yalnızca bir varlığa yazılır.

Örneğin, gerçekleştirdiğiniz bir **GetBlob** Blob Storage Analytics ölçümleri hizmetiniz işlemi isteği oturum ve Blob hizmeti için toplanan veriler dahil yanı sıra **GetBlob** işlemi. Ancak, yoksa **GetBlob** saatte istenen işlem, bir varlık için yazılmayacak `$MetricsTransactionsBlob` bu işlem için.

İşlem ölçümlerini, kullanıcı isteklerini ve depolama analizi tarafından kendisini yapılan istekleri için kaydedilir. Örneğin, günlükleri ve tablo varlıkları yazma isteklerini Storage Analytics tarafından kaydedilir. Bu istekler nasıl faturalandırılır hakkında daha fazla bilgi için bkz: [depolama çözümlemeleri ve faturalama](https://msdn.microsoft.com/library/hh360997.aspx).

### <a name="capacity-metrics"></a>Kapasite ölçümleri
> [!NOTE]
> Şu anda, kapasite ölçümlerini yalnızca Blob hizmeti için kullanılabilir. Sıra hizmeti ve tablo hizmeti için kapasite ölçümlerini Storage Analytics gelecekteki sürümlerinde kullanılabilir.
> 
> 

Kapasite verileri günlük bir depolama hesabının Blob hizmeti için kaydedilir ve iki tablo varlıkları yazılır. Bir varlık için kullanıcı verilerini istatistikleri sağlar ve diğer ilgili istatistikler sağlar `$logs` depolama analizi tarafından kullanılan blob kapsayıcısı. `$MetricsCapacityBlob` Tablosu aşağıdaki İstatistikler içerir:

* **Kapasite**: bayt cinsinden depolama hesabının Blob hizmeti tarafından kullanılan depolama alanı miktarı.
* **ContainerCount**: depolama hesabının Blob hizmeti blob kapsayıcı sayısı.
* **ObjectCount**: kaydedilmiş ve kaydedilmemiş blok veya sayfa BLOB Depolama hesabının Blob hizmetinde sayısı.

Kapasite ölçümleri hakkında daha fazla bilgi için bkz: [Storage Analytics Ölçüm tablosu şeması](https://msdn.microsoft.com/library/hh343264.aspx).

### <a name="how-metrics-are-stored"></a>Ölçümleri nasıl depolanır
Tüm ölçüm verilerini depolama hizmetlerinin her biri için bu hizmet için ayrılmış üç tablolarında depolanır: işlem bilgileri için bir tablo, dakikalık işlem bilgileri için bir tablo ve kapasite bilgileri için başka bir tablo. İstek ve yanıt verilerini işlem ve dakika işlem bilgilerini oluşur ve depolama alanı kullanım verilerini kapasite bilgilerini oluşur. Aşağıdaki tabloda açıklandığı gibi adlı tablolardaki saat ölçümleri, dakika ölçümleri ve bir depolama hesabının Blob hizmeti için kapasite erişilebilir.

| Ölçümleri düzeyi | Tablo adları | Desteklenen sürümler |
| --- | --- | --- |
| Saatlik ölçümleri, birincil konumu |$MetricsTransactionsBlob <br/>$MetricsTransactionsTable <br/> $MetricsTransactionsQueue |2013-08-15 yalnızca'den önceki sürümleri. Bu adları hala desteklenmektedir, ancak aşağıda listelenen tabloların kullanmaya geçiş yapmanız önerilir. |
| Saatlik ölçümleri, birincil konumu |$MetricsHourPrimaryTransactionsBlob <br/>$MetricsHourPrimaryTransactionsTable <br/>$MetricsHourPrimaryTransactionsQueue |2013-08-15 dahil olmak üzere tüm sürümleri. |
| Dakika ölçümleri, birincil konumu |$MetricsMinutePrimaryTransactionsBlob <br/>$MetricsMinutePrimaryTransactionsTable <br/>$MetricsMinutePrimaryTransactionsQueue |2013-08-15 dahil olmak üzere tüm sürümleri. |
| Saatlik ölçümleri, ikincil konum |$MetricsHourSecondaryTransactionsBlob <br/>$MetricsHourSecondaryTransactionsTable <br/>$MetricsHourSecondaryTransactionsQueue |2013-08-15 dahil olmak üzere tüm sürümleri. Okuma erişimli coğrafi olarak yedekli çoğaltma etkinleştirilmiş olması gerekir. |
| Dakika ölçümleri, ikincil konum |$MetricsMinuteSecondaryTransactionsBlob <br/>$MetricsMinuteSecondaryTransactionsTable <br/>$MetricsMinuteSecondaryTransactionsQueue |2013-08-15 dahil olmak üzere tüm sürümleri. Okuma erişimli coğrafi olarak yedekli çoğaltma etkinleştirilmiş olması gerekir. |
| Kapasite (yalnızca Blob hizmeti) |$MetricsCapacityBlob |2013-08-15 dahil olmak üzere tüm sürümleri. |

Storage Analytics bir depolama hesabı için etkinleştirildiğinde, bu tablolar otomatik olarak oluşturulur. Örneğin, depolama hesabı ad alanı erişilir:`https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")`

### <a name="accessing-metrics-data"></a>Ölçümleri verilerine erişme
Ölçümleri tablolarındaki tüm verileri tablo hizmeti API'leri kullanılarak erişilebilir, Azure tarafından sağlanan .NET API'lerini de dahil olmak üzere kitaplığı yönetilen. Depolama hesabının yöneticisine okuyun ve tablo varlıkları silmek, ancak oluşturabilir veya bunları güncelleştirin.

## <a name="billing-for-storage-analytics"></a>Depolama çözümlemeleri için fatura
Tüm ölçüm verilerini bir depolama hesabı Hizmetleri tarafından yazılır. Sonuç olarak, depolama analizi tarafından gerçekleştirilen her yazma işlemi faturalanabilir. Ayrıca, ölçüm verilerini tarafından kullanılan depolama alanı miktarı da faturalanabilir.

Storage Analytics tarafından gerçekleştirilen aşağıdaki eylemler Faturalanabilir şunlardır:

* Günlük için BLOB'ları oluşturmak için istek sayısı. 
* Ölçümler için tablo varlıkları oluşturmak için istek sayısı.

Bir veri bekletme ilkesi yapılandırdıysanız, depolama çözümlemeleri eski günlüğe kaydetme ve ölçüm verileri sildiğinde, silme işlemleri için sizden ücret istenmese. Ancak, bir istemciden silme işlemleri faturalanabilir. Bekletme ilkeleri hakkında daha fazla bilgi için bkz: [depolama Analytics veri Bekletme İlkesi ayarı](https://msdn.microsoft.com/library/azure/hh343263.aspx).

### <a name="understanding-billable-requests"></a>Faturalandırılabilir isteklerin anlama
Bir hesabın depolama hizmetine yapılan her isteği Faturalanabilir ya da Faturalanamayan değil. Storage Analytics istek nasıl işlendiğini gösteren bir durum iletisi de dahil olmak üzere bir hizmetine yapılan her isteği günlüğe kaydeder. Benzer şekilde, Storage Analytics ölçümleri hem hizmet hem de o hizmetinde yüzdeleri ve belirli durum iletilerinin sayısını gibi API işlemlerini depolar. Birlikte, bu özellikler, uygulamanızda geliştirmeler yapmak ve hizmetlerinizi istekleri ile ilgili sorunları tanılamak Faturalanabilir isteklerinizi çözümlemenize yardımcı olabilir. Faturalama hakkında daha fazla bilgi için bkz: [anlama Azure depolama faturalama - bant genişliği, işlemleri ve kapasite](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

Storage Analytics veri bakarken tablolarında kullanabilirsiniz [depolama Analytics günlüğe yazılan işlemler ve durum iletileri](https://msdn.microsoft.com/library/azure/hh343260.aspx) hangi istekleri Faturalanabilir belirlemek için konu. Ardından, ölçüm verileri belirli bir istek için ücret olmadığını görmek için durum iletilerini ve günlükleri karşılaştırabilirsiniz. Ayrıca, depolama birimi hizmeti veya bireysel API işlemi için araştırmak için önceki konusunda tabloları kullanabilir.

## <a name="next-steps"></a>Sonraki adımlar
### <a name="setting-up-storage-analytics"></a>Storage Analytics ayarlama
* [Azure Portal'da depolama hesabı izleme](storage-monitor-storage-account.md)
* [Etkinleştirme ve depolama çözümlemeleri yapılandırma](https://msdn.microsoft.com/library/hh360996.aspx)

### <a name="storage-analytics-logging"></a>Storage Analytics günlüğe kaydetme
* [Storage Analytics günlüğü hakkında](https://msdn.microsoft.com/library/hh343262.aspx)
* [Storage Analytics günlük biçimi](https://msdn.microsoft.com/library/hh343259.aspx)
* [Storage Analytics işlemler ve durum iletilerini günlüğe](https://msdn.microsoft.com/library/hh343260.aspx)

### <a name="storage-analytics-metrics"></a>Storage Analytics ölçümleri
* [Storage Analytics ölçümleri hakkında](https://msdn.microsoft.com/library/hh343258.aspx)
* [Storage Analytics Ölçüm tablosu şeması](https://msdn.microsoft.com/library/hh343264.aspx)
* [Storage Analytics işlemler ve durum iletilerini günlüğe](https://msdn.microsoft.com/library/hh343260.aspx)  

