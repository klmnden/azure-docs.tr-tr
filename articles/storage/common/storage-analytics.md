---
title: Azure depolama analizi günlükleri ve ölçüm verilerini toplamak için kullan | Microsoft Docs
description: Depolama analizi, tüm depolama hizmetleri için ölçüm verileri izlemek amacıyla ve Blob, kuyruk ve tablo depolama için günlükleri toplamak için sağlar.
services: storage
author: roygara
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 03/03/2017
ms.author: rogarana
ms.component: common
ms.openlocfilehash: a99375ae961e9239e5e8ea86db8b1b9b002b10c8
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39526972"
---
# <a name="storage-analytics"></a>Depolama Analizi

Azure Depolama Analizi günlük kaydı yapar ve depolama hesabı için ölçüm verileri sağlar. Bu verileri kullanarak istekleri izleyebilir, kullanım eğilimlerini çözümleyebilir ve depolama hesabınızdaki sorunları tanılayabilirsiniz.

Depolama analizi kullanmak için onu ayrı ayrı izlemek istediğiniz her hizmet için etkinleştirmeniz gerekir. Buradan etkinleştirebilirsiniz [Azure portalı](https://portal.azure.com). Ayrıntılar için bkz [Azure Portal'da depolama hesabı izleme](storage-monitor-storage-account.md). Depolama analizi REST API veya istemci kitaplığı aracılığıyla programlı olarak da etkinleştirebilirsiniz. Kullanım [Blob hizmeti özelliklerini almak](https://msdn.microsoft.com/library/hh452239.aspx), [kuyruk hizmeti özelliklerini almak](https://msdn.microsoft.com/library/hh452243.aspx), [tablo hizmeti özelliklerini almak](https://msdn.microsoft.com/library/hh452238.aspx), ve [alma dosya hizmeti özelliklerini](https://msdn.microsoft.com/library/mt427369.aspx)her hizmet için depolama analizi etkinleştirme işlemleri.

Toplanan veriler (günlük için) iyi bilinen bir blob ve tablo hizmeti API'leri ve Blob hizmeti kullanılarak erişilebilecek (ölçüm) için iyi bilinen tablolara depolanır.

Depolama analizi, depolama hesabınız için toplam limiti bağımsızdır depolanan veri miktarına 20 TB limiti vardır. Faturalandırma ve veri saklama ilkeleri hakkında daha fazla bilgi için bkz: [depolama analizi ve faturalandırma](https://msdn.microsoft.com/library/hh360997.aspx). Depolama hesabı sınırları hakkında daha fazla bilgi için bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](storage-scalability-targets.md).

Kapsamlı bir kılavuz tanımlamak, tanılamak ve Azure depolama ile ilgili sorunları gidermek için depolama analizi ve diğer araçları kullanma hakkında bilgi için bkz: [izleme, tanılama ve sorun giderme Microsoft Azure depolama](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="about-storage-analytics-logging"></a>Depolama analizi günlük kaydı hakkında
Depolama analizi, başarılı ve başarısız istekler hakkında ayrıntılı bilgi için bir depolama hizmetine kaydeder. Bu bilgiler, tek tek istekleri izlemek için ve bir depolama hizmeti ile ilgili sorunları tanılamak için kullanılabilir. Bir en iyi çaba ilkesine göre istekleri günlüğe kaydedilir.

Depolama hizmeti etkinlik yoksa yalnızca günlük girişi oluşturulur. Örneğin, bir depolama hesabı, Blob hizmeti, ancak tablo veya kuyruk hizmetlerinin etkinlik varsa, yalnızca Blob hizmetine ilişkin günlükleri oluşturulur.

Depolama analizi günlük kaydı, Azure dosyaları için kullanılamıyor.

### <a name="logging-authenticated-requests"></a>Kimlik doğrulaması günlüğe kaydetme istekleri
Aşağıdaki türde kimliği doğrulanmış istekler kaydedilir:

* Başarılı istekler.
* İstek zaman aşımı, azaltma, ağ, yetkilendirme ve başka hatalar da dahil olmak üzere, başarısız oldu.
* Başarılı ve başarısız istekleri dahil olmak üzere paylaşılan erişim imzası (SAS), kullanarak istek sayısı.
* Analiz verilerini istekleri.

Depolama analizi kendisini günlük oluşturma veya silme gibi tarafından yapılan istekleri günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam bir listesi belgelenen [depolama analizi günlüğe yazılan işlemler ve durum iletileri](https://msdn.microsoft.com/library/hh343260.aspx) ve [depolama analizi günlük biçimi](https://msdn.microsoft.com/library/hh343259.aspx) konuları.

### <a name="logging-anonymous-requests"></a>Anonim istekler günlüğe kaydetme
Aşağıdaki türde anonim istekler kaydedilir:

* Başarılı istekler.
* Sunucu hataları.
* İstemci ve sunucu için zaman aşımı hataları.
* Hata ile başarısız olan GET istekleri 304 (değiştirilmedi) kodu.

Diğer tüm başarısız anonim istekler günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam bir listesi belgelenen [depolama analizi günlüğe yazılan işlemler ve durum iletileri](https://msdn.microsoft.com/library/hh343260.aspx) ve [depolama analizi günlük biçimi](https://msdn.microsoft.com/library/hh343259.aspx) konuları.

### <a name="how-logs-are-stored"></a>Günlükler nasıl depolanır
Tüm günlükler, depolama analizi için bir depolama hesabı etkinleştirildiğinde bu otomatik olarak oluşturulur ve $logs adlı bir kapsayıcı içinde blok blobları olarak depolanır. $Logs kapsayıcı örneğin depolama hesabının blob ad alanı bulunur: `http://<accountname>.blob.core.windows.net/$logs`. Depolama analizi etkinleştirildikten sonra bu kapsayıcı içeriğini silinebilir silinemiyor.

> [!NOTE]
> Bir kapsayıcı listeleme işlemi, gibi gerçekleştirildiğinde $logs kapsayıcı gösterilmez [ListContainers](https://msdn.microsoft.com/library/azure/dd179352.aspx) yöntemi. Doğrudan erişilmelidir. Örneğin, kullanabileceğiniz [ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx) BLOB'ları erişmeye yöntemi `$logs` kapsayıcı.
> Depolama analizi istekleri günlüğe gibi ara sonuçlar bloklar olarak yükleyeceksiniz. Düzenli olarak, depolama analizi bu blokları işleme ve bir blob olarak kullanılmasını.
> 
> 

Yinelenen kayıtları aynı saat içinde oluşturulan günlükler için mevcut olmayabilir. Bir kayıt yinelenen olup olmadığını kontrol ederek belirleyebilirsiniz **RequestId** ve **işlemi** sayı.

### <a name="log-naming-conventions"></a>Günlük adlandırma kuralları
Şu biçimde her bir günlüğe yazılır.

    <service-name>/YYYY/MM/DD/hhmm/<counter>.log

Aşağıdaki tabloda günlük adı içindeki her bir öznitelik açıklanmaktadır.

| Öznitelik | Açıklama |
| --- | --- |
| < hizmet-adı > |Depolama hizmeti adı. Örneğin: blob, tablo ve kuyruk. |
| YYYY |Günlüğü için dört basamaklı yıl. Örneğin: 2011. |
| AA |İki haneli ay günlüğü. Örneğin: 07. |
| DD |İki haneli ay günlüğü. Örneğin: 07. |
| hh |İki basamaklı bir saat, 24 saat UTC biçiminde günlükleri için başlangıç saati gösterir. Örneğin: 18. |
| aa |Günlükleri başlangıç dakika gösteren iki basamaklı bir sayı. Bu değer, depolama analizi geçerli sürümde desteklenmiyor ve değeri her zaman 00 olur. |
| <counter> |Altı basamaklı bir saat içinde süre depolama hizmeti için oluşturulan günlük bloblarını sayısını gösteren sıfır tabanlı bir sayacı. Bu sayaç 000000 başlatır. Örneğin: 000001. |

Önceki örneklerde birleştiren bir tam örnek günlük adı verilmiştir.

    blob/2011/07/31/1800/000001.log

Önceki günlüğe erişmek için kullanılan URI bir örnek verilmiştir.

    https://<accountname>.blob.core.windows.net/$logs/blob/2011/07/31/1800/000001.log

Depolama isteği günlüğe kaydedildiğinde, sonuçta elde edilen günlük adı istenen işlem tamamlandığında bir saate ilişkilendirir. Örneğin, 6:30 P.M'de bir GetBlob isteği tamamlandı 7/31/2011'de, günlük aşağıdaki ön ekine sahip yazılabilir: `blob/2011/07/31/1800/`

### <a name="log-metadata"></a>Log meta verileri
Tüm günlük bloblarını blob içerdiği günlük verileri tanımlamak için kullanılan meta verileriyle depolanır. Aşağıdaki tabloda her bir meta veri öznitelik açıklanmaktadır.

| Öznitelik | Açıklama |
| --- | --- |
| LogType |Günlük Okuma, yazma veya silme işlemleri için ilgili bilgileri içerip içermediğini açıklar. Bu değer, bir tür veya üçünü virgülle ayrılmış bir birleşimini içerebilir. Örnek 1: yazabilirsiniz. Örnek 2: Okuma, yazma; Örnek 3: Okuma, yazma, silme. |
| StartTime |En erken zamanı YYYY biçiminde günlüğündeki bir girişi-aa-ssZ. Örneğin: 2011-07-31T18:21:46Z. |
| EndTime |En son YYYY biçiminde günlüğündeki bir girişi saati-aa-ssZ. Örneğin: 2011-07-31T18:22:09Z. |
| LogVersion |Günlük biçimi sürümü. Şu anda desteklenen tek değer 1.0 ' dir. |

Aşağıdaki listede, önceki örneklerde kullanarak tam bir örnek meta veri görüntüler.

* LogType=write
* StartTime=2011-07-31T18:21:46Z
* EndTime=2011-07-31T18:22:09Z
* LogVersion = 1,0

### <a name="accessing-logging-data"></a>Günlük verilerine erişme
Tüm veriler `$logs` kapsayıcı, Blob hizmeti API'leri kullanılarak erişilebilir, Azure tarafından sağlanan .NET API'leri dahil olmak üzere yönetilen kitaplığı. Depolama hesabı yöneticisi okuyun ve günlükleri, silme, ancak oluşturabilir veya bunları güncelleştirin. Günlüğün meta verilerini hem de günlük adı için bir günlük sorgulanırken kullanılabilir. Verilen saatlik günlükleri bozuk görünmesi mümkündür, ancak meta verileri, günlük girişlerini timespan her zaman bir oturum açma belirtir. Bu nedenle, belirli bir günlük için arama yaparken, günlük adlarını ve meta verilerini bir birleşimini kullanabilirsiniz.

## <a name="about-storage-analytics-metrics"></a>Storage Analytics ölçümleri hakkında
Depolama analizi, bir depolama hizmetine istekleriyle ilgili toplu işlem istatistiklerini ve kapasite verileri içeren ölçümleri depolayabilir. İşlemler her iki API düzeyinde işlemi yanı sıra depolama hizmet düzeyinde bildirilir ve kapasitesi depolama hizmet düzeyinde bildirilir. Ölçüm verileri, depolama hizmeti kullanımını çözümleme, depolama hizmeti karşı yapılan istekleri ile ilgili sorunları tanılayın ve hizmet kullanan uygulamaların performansını artırmak için kullanılabilir.

Depolama analizi kullanmak için onu ayrı ayrı izlemek istediğiniz her hizmet için etkinleştirmeniz gerekir. Buradan etkinleştirebilirsiniz [Azure portalı](https://portal.azure.com). Ayrıntılar için bkz [Azure Portal'da depolama hesabı izleme](storage-monitor-storage-account.md). Depolama analizi REST API veya istemci kitaplığı aracılığıyla programlı olarak da etkinleştirebilirsiniz. Kullanım **hizmet özellikleri Al** her hizmet için depolama analizi etkinleştirme işlemleri.

### <a name="transaction-metrics"></a>İşlem ölçümleri
Sağlam bir veri kümesini giriş/çıkış, kullanılabilirlik, hataları, dahil olmak üzere API işlemi, istenen her depolama hizmeti için saat veya dakika aralıklarla kaydedilir ve isteği yüzdeleri kategorilere ayrılmıştır. İşlem ayrıntıları tam listesini görebilirsiniz [Storage Analytics Ölçüm tablosu şeması](https://msdn.microsoft.com/library/hh343264.aspx) konu.

İşlem verileri, iki düzeyde – hizmet düzeyi ve API işlemi düzeyi kaydedilir. Hizmet düzeyinde bile hiçbir istek hizmetine yapılan API işlemleri saatte bir tablo varlığına yazılan tüm özetleme istatistikleri istedi. API işlem düzeyinde istatistikleri o saat içinde istenen işlem, bir varlığa yalnızca yazılır.

Örneğin, gerçekleştirdiğiniz bir **GetBlob** Blob hizmeti, Storage Analytics ölçümleri hakkında daha fazla işlem isteğini kaydetmesini ve Blob hizmetine ilişkin birleşik veriler dahil yanı sıra **GetBlob** işlem. Ancak, hiçbir **GetBlob** saatte istenen işlem, bir varlık için yazılmayacak `$MetricsTransactionsBlob` bu işlem için.

İşlem ölçümlerini, kullanıcı isteklerini hem de depolama analizi tarafından kendisine yapılan istekler için kaydedilir. Örneğin, günlükleri ve tablo varlıkları yazma isteklerinin depolama analizi tarafından kaydedilir. Bu istekler nasıl faturalandırılır hakkında daha fazla bilgi için bkz. [depolama analizi ve faturalandırma](https://msdn.microsoft.com/library/hh360997.aspx).

### <a name="capacity-metrics"></a>Kapasite ölçümleri
> [!NOTE]
> Kapasite ölçümleri şu anda yalnızca Blob hizmeti için olarak kullanılabilir. Tablo hizmeti ve kuyruk hizmeti için kapasite ölçümlerini depolama analizi, gelecek sürümlerinde kullanıma sunulacak.
> 
> 

Kapasite verileri günlük olarak bir depolama hesabının Blob hizmeti için kaydedilir ve iki tablo varlıkları yazılır. Bir varlık için kullanıcı verileri istatistikler sağlar ve diğer ilgili istatistikler sağlar `$logs` blob kapsayıcısı depolama analizi tarafından kullanılır. `$MetricsCapacityBlob` Tablo, aşağıdaki İstatistikler içerir:

* **Kapasite**: bayt olarak depolama hesabının Blob hizmeti tarafından kullanılan depolama miktarı.
* **ContainerCount**: depolama hesabının Blob hizmetindeki blob kapsayıcı sayısı.
* **ObjectCount**: kaydedilen ve kaydedilmeyen blok veya sayfa blobları depolama hesabının Blob hizmetindeki sayısı.

Kapasite ölçümleri hakkında daha fazla bilgi için bkz: [Storage Analytics Ölçüm tablosu şeması](https://msdn.microsoft.com/library/hh343264.aspx).

### <a name="how-metrics-are-stored"></a>Ölçümleri nasıl depolanır
Tüm ölçüm verileri Depolama hizmetlerinin her biri için bu hizmet için ayrılmış üç tablo depolanır: işlem bilgileri için bir tablo, dakikalık işlem bilgi almak için bir tabloya ve kapasite bilgileri için başka bir tablo. Kullanım verileri depolama kapasitesi bilgileri oluşur ve işlem ve dakika işlem bilgilerini istek ve yanıt verilerini oluşur. Saat ölçümleri dakika ölçümlerini ve bir depolama hesabının Blob hizmeti için kapasite, aşağıdaki tabloda açıklandığı gibi adlı tablolarda erişilebilir.

| Ölçümleri düzeyi | Tablo adları | Desteklenen sürümler |
| --- | --- | --- |
| Saatlik ölçümlerini, birincil konum |$MetricsTransactionsBlob <br/>$MetricsTransactionsTable <br/> $MetricsTransactionsQueue |2013-08-15 yalnızca önceki sürümler. Bu adlar hala desteklendiğinden, aşağıda listelenen tablolar'ı kullanmaya geçmeniz özellikle önerilir. |
| Saatlik ölçümlerini, birincil konum |$MetricsHourPrimaryTransactionsBlob <br/>$MetricsHourPrimaryTransactionsTable <br/>$MetricsHourPrimaryTransactionsQueue |2013-08-15 dahil tüm sürümler. |
| Dakika ölçümlerini, birincil konum |$MetricsMinutePrimaryTransactionsBlob <br/>$MetricsMinutePrimaryTransactionsTable <br/>$MetricsMinutePrimaryTransactionsQueue |2013-08-15 dahil tüm sürümler. |
| Saatlik ölçümlerini, ikincil konum |$MetricsHourSecondaryTransactionsBlob <br/>$MetricsHourSecondaryTransactionsTable <br/>$MetricsHourSecondaryTransactionsQueue |2013-08-15 dahil tüm sürümler. Okuma erişimli coğrafi olarak yedekli çoğaltma etkinleştirilmiş olması gerekir. |
| Dakika ölçümlerini, ikincil konum |$MetricsMinuteSecondaryTransactionsBlob <br/>$MetricsMinuteSecondaryTransactionsTable <br/>$MetricsMinuteSecondaryTransactionsQueue |2013-08-15 dahil tüm sürümler. Okuma erişimli coğrafi olarak yedekli çoğaltma etkinleştirilmiş olması gerekir. |
| Kapasite (yalnızca Blob hizmeti) |$MetricsCapacityBlob |2013-08-15 dahil tüm sürümler. |

Depolama analizi için bir depolama hesabı etkinleştirildiğinde bu tabloların otomatik olarak oluşturulur. Örneğin, depolama hesabı, ad alanı erişilir: `https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")`

### <a name="accessing-metrics-data"></a>Ölçümleri verilerine erişme
Tabulky Metrik tüm verileri tablo hizmet API'lerini kullanarak erişilebilir, Azure tarafından sağlanan .NET API'leri dahil olmak üzere yönetilen kitaplığı. Depolama hesabı yöneticisi okuyun ve tablo varlıklarını silme, ancak oluşturabilir veya bunları güncelleştirin.

## <a name="billing-for-storage-analytics"></a>Depolama analizi faturalandırması
Tüm ölçüm verileri bir depolama hesabının hizmetler tarafından yazılır. Sonuç olarak, depolama analizi tarafından gerçekleştirilen her yazma işlemi Faturalandırılabilir. Ayrıca, ölçüm verilerini tarafından kullanılan depolama miktarı da Faturalandırılabilir.

Depolama analizi tarafından gerçekleştirilen aşağıdaki eylemler Faturalanabilir şunlardır:

* Günlüğe kaydetme için BLOB'ları oluşturmak için istek sayısı. 
* Ölçümler için tablo varlıkları oluşturmak için istek sayısı.

Veri bekletme ilkesi yapılandırdıysanız, depolama analizi, eski günlük ve ölçüm verileri sildiğinde silme işlemler için ücretlendirilmez. Ancak, bir istemciden silme işlemleri Faturalanabilir niteliktedir. Bekletme ilkeleri hakkında daha fazla bilgi için bkz: [depolama Analytics veri saklama ilkesini belirlemeden](https://msdn.microsoft.com/library/azure/hh343263.aspx).

### <a name="understanding-billable-requests"></a>Faturalandırılabilir isteklerin anlama
Bir hesabın depolama hizmetine yapılan her isteği Faturalanabilir ya da Faturalanamayan değil. Depolama analizi yapılan bir hizmete istek nasıl işlendiğini belirten bir durum iletisi de dahil olmak üzere, tek tek her isteği günlüğe kaydeder. Benzer şekilde, Storage Analytics ölçümleri hem hizmet hem de hizmet yüzdeleri ve belirli durum iletilerinin sayısı dahil olmak üzere, API işlemleri için depolar. Birlikte, bu özellikler, Faturalanabilir isteklerinizi çözümleme, uygulamanız üzerinde geliştirmeler yapmak ve hizmetlerinizi istekleri ile ilgili sorunları tanılamanıza yardımcı olabilir. Faturalama hakkında daha fazla bilgi için bkz. [anlama Azure depolama Faturalaması - bant genişliği, işlemler ve kapasite](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

Depolama analizi verilere baktığımızda, tablolardaki kullanabileceğiniz [depolama analizi günlüğe yazılan işlemler ve durum iletileri](https://msdn.microsoft.com/library/azure/hh343260.aspx) hangi istekleri Faturalanabilir belirlemek için. Ardından, günlükleri ve ölçüm verileri belirli bir istek için ücret olmadığını görmek için durum iletilerine karşılaştırabilirsiniz. Depolama hizmeti veya tek tek API işlemi için kullanılabilirlik araştırmak için önceki konu başlığında tabloları da kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
### <a name="setting-up-storage-analytics"></a>Depolama analizi ayarlama
* [Azure Portal'da depolama hesabı izleme](storage-monitor-storage-account.md)
* [Etkinleştirme ve yapılandırma depolama analizi](https://msdn.microsoft.com/library/hh360996.aspx)

### <a name="storage-analytics-logging"></a>Depolama analizi günlük kaydı
* [Depolama analizi günlük kaydı hakkında](https://msdn.microsoft.com/library/hh343262.aspx)
* [Depolama analizi günlük biçimi](https://msdn.microsoft.com/library/hh343259.aspx)
* [Depolama analizi işlemleri ve durum iletileri günlüğe kaydedilir.](https://msdn.microsoft.com/library/hh343260.aspx)

### <a name="storage-analytics-metrics"></a>Storage Analytics ölçümleri
* [Storage Analytics ölçümleri hakkında](https://msdn.microsoft.com/library/hh343258.aspx)
* [Storage Analytics Ölçüm tablosu şeması](https://msdn.microsoft.com/library/hh343264.aspx)
* [Depolama analizi işlemleri ve durum iletileri günlüğe kaydedilir.](https://msdn.microsoft.com/library/hh343260.aspx)  

