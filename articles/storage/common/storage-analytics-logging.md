---
title: Azure depolama analizi günlük kaydı
description: Azure depolama karşı yapılan isteklerin ayrıntılarını günlüğe kaydetme hakkında bilgi edinin.
services: storage
author: fhryo-msft
ms.service: storage
ms.topic: article
ms.date: 03/11/2019
ms.author: fryu
ms.subservice: common
ms.openlocfilehash: 09a5a6d823240b724e6ec88de38df068a58982d9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61483511"
---
# <a name="azure-storage-analytics-logging"></a>Azure depolama analizi günlük kaydı

Depolama analizi, başarılı ve başarısız istekler hakkında ayrıntılı bilgi için bir depolama hizmetine kaydeder. Bu bilgiler, tek tek istekleri izlemek için ve bir depolama hizmeti ile ilgili sorunları tanılamak için kullanılabilir. Bir en iyi çaba ilkesine göre istekleri günlüğe kaydedilir.

 Depolama analizi günlük depolama hesabınız için varsayılan olarak etkin değildir. İçinde etkinleştirebilirsiniz [Azure portalında](https://portal.azure.com/); Ayrıntılar için bkz: [Azure portalında depolama hesabı izleme](/azure/storage/storage-monitor-storage-account). Depolama analizi REST API veya istemci kitaplığı aracılığıyla programlı olarak da etkinleştirebilirsiniz. Kullanım [Blob hizmeti özelliklerini almak](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API), [kuyruk hizmeti özelliklerini almak](https://docs.microsoft.com/rest/api/storageservices/Get-Queue-Service-Properties), ve [tablo hizmeti özelliklerini almak](https://docs.microsoft.com/rest/api/storageservices/Get-Table-Service-Properties) her hizmet için depolama analizi etkinleştirme işlemleri.

 Hizmet uç noktasına karşı yapılan istekler varsa, günlük girişi oluşturulur. Örneğin, Blob hizmetine ilişkin günlükleri bir depolama hesabı, Blob uç noktası ancak kendi tablo veya kuyruk uç etkinliği varsa oluşturulur.

> [!NOTE]
>  Depolama analizi günlük kaydı şu anda yalnızca Blob, kuyruk ve tablo hizmetler için kullanılabilir. Ancak, premium depolama hesabı desteklenmiyor.

## <a name="requests-logged-in-logging"></a>İstekleri oturum açanlar
### <a name="logging-authenticated-requests"></a>Kimlik doğrulaması günlüğe kaydetme istekleri

 Aşağıdaki türde kimliği doğrulanmış istekler kaydedilir:

- Başarılı istekler
- Başarısız istek, zaman aşımı, azaltma, ağ, yetkilendirme ve başka hatalar da dahil olmak üzere
- Paylaşılan erişim imzası (SAS) veya başarılı ve başarısız istekleri dahil olmak üzere, OAuth kullanarak istekleri
- Analiz veri istekleri

  Depolama analizi kendisini günlük oluşturma veya silme gibi tarafından yapılan istekleri günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam bir listesi belgelenen [depolama analizi günlüğe yazılan işlemler ve durum iletileri](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) ve [depolama analizi günlük biçimi](/rest/api/storageservices/storage-analytics-log-format) konuları.

### <a name="logging-anonymous-requests"></a>Anonim istekler günlüğe kaydetme

 Aşağıdaki türde anonim istekler kaydedilir:

- Başarılı istekler
- Sunucu hataları
- İstemci ve sunucu zaman aşımı hataları
- 304 (değiştirilmedi) hata koduyla başarısız GET istekleri

  Diğer tüm başarısız anonim istekler günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam bir listesi belgelenen [depolama analizi günlüğe yazılan işlemler ve durum iletileri](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) ve [depolama analizi günlük biçimi](/rest/api/storageservices/storage-analytics-log-format) konuları.

## <a name="how-logs-are-stored"></a>Günlükler nasıl depolanır

Tüm günlükler adlı bir kapsayıcıda blok bloblarını depolanır `$logs`, otomatik olarak oluşturulan depolama hesabı için depolama analizi etkin olduğunda. `$logs` Kapsayıcı bulunduğu depolama hesabının blob ad alanı örnek: `http://<accountname>.blob.core.windows.net/$logs`. Depolama analizi etkinleştirildikten sonra bu kapsayıcı içeriğini silinebilir silinemiyor. Bir kapsayıcıya doğrudan gitmek için depolama tarama aracı kullanırsanız, günlük verilerinizi içeren tüm blobları görürsünüz.

> [!NOTE]
>  `$logs` Kapsayıcı listesi kapsayıcıları işlemi gibi bir kapsayıcı listeleme işlemi gerçekleştirildiğinde görüntülenmez. Doğrudan erişilmelidir. Örneğin, BLOB'ları erişmeye Blobları listeleme işlemi kullanabilirsiniz `$logs` kapsayıcı.

Depolama analizi istekleri günlüğe gibi ara sonuçlar bloklar olarak yükleyeceksiniz. Düzenli olarak, depolama analizi bu blokları işleme ve bir blob olarak kullanılmasını. Günlük verilerine, BLOB'ları görünmesi bir saate kadar sürebilir **$logs** kapsayıcı için depolama hizmeti günlük yazıcılar aktarır sıklığı. Yinelenen kayıtları aynı saat içinde oluşturulan günlükler için mevcut olmayabilir. Bir kayıt yinelenen olup olmadığını kontrol ederek belirleyebilirsiniz **RequestId** ve **işlemi** sayı.

Her saat için birden çok dosya ile günlük veri hacmi yüksek varsa, günlük blob meta veri alanları inceleyerek içerdiği verileri belirlemek için blob meta verilerini kullanabilirsiniz. Bazen olabileceğinden bir gecikme veriler günlük dosyalarına yazılır olsa da kullanışlıdır: blob adı yerine daha doğru bir göstergesi blob içeriğinin blob meta verilerini sağlar.

Çoğu depolama gözatma araçları BLOB meta verilerini görüntülemek etkinleştirme; Ayrıca, PowerShell kullanarak bu bilgileri okuyabilirsiniz veya programlama yoluyla. Aşağıdaki PowerShell kod parçacığı bir zaman belirtmek için adı ve meta verileri içeren yalnızca günlükler tanımlamak için günlük BLOB listesini filtreleme ilişkin bir örnektir **yazma** operations.  

 ```powershell
 Get-AzureStorageBlob -Container '$logs' |  
 Where-Object {  
     $_.Name -match 'table/2014/05/21/05' -and   
     $_.ICloudBlob.Metadata.LogType -match 'write'  
 } |  
 ForEach-Object {  
     "{0}  {1}  {2}  {3}" –f $_.Name,   
     $_.ICloudBlob.Metadata.StartTime,   
     $_.ICloudBlob.Metadata.EndTime,   
     $_.ICloudBlob.Metadata.LogType  
 }  
 ```  

Blobları program aracılığıyla listeleme hakkında daha fazla bilgi için bkz: [Blob kaynaklarını numaralandırma](http://msdn.microsoft.com/library/azure/hh452233.aspx) ve [ayarlama ve alma özellikleri ve meta verileri Blob kaynakları için](http://msdn.microsoft.com/library/azure/dd179404.aspx).  

### <a name="log-naming-conventions"></a>Günlük adlandırma kuralları

 Şu biçimde her bir günlüğe yazılır:

 `<service-name>/YYYY/MM/DD/hhmm/<counter>.log`

 Aşağıdaki tabloda günlük adı içindeki her bir öznitelik açıklanmaktadır:

|Öznitelik|Açıklama|
|---------------|-----------------|
|`<service-name>`|Depolama hizmeti adı. Örneğin: `blob`, `table`, veya `queue`|
|`YYYY`|Günlüğü için dört basamaklı yıl. Örneğin, `2011`|
|`MM`|İki haneli ay günlüğü için. Örneğin, `07`|
|`DD`|İki basamaklı gün için günlük. Örneğin, `31`|
|`hh`|Günlükler için başlangıç saati gösteren iki basamaklı saat, 24 saati UTC biçiminde. Örneğin, `18`|
|`mm`|Günlükleri başlangıç dakika gösteren iki basamaklı bir sayı. **Not:**  Bu değer, depolama analizi geçerli sürümde desteklenmiyor ve değeri her zaman olacaktır `00`.|
|`<counter>`|Altı basamaklı bir saat içinde süre depolama hizmeti için oluşturulan günlük bloblarını sayısını gösteren sıfır tabanlı bir sayacı. Bu sayaç başlar `000000`. Örneğin, `000001`|

 Yukarıdaki örneklerde birleştiren bir tam örnek günlük adı verilmiştir:

 `blob/2011/07/31/1800/000001.log`

 Yukarıdaki günlük erişmek için kullanılan URI bir örnek verilmiştir:

 `https://<accountname>.blob.core.windows.net/$logs/blob/2011/07/31/1800/000001.log`

 Depolama isteği günlüğe kaydedildiğinde, sonuçta elde edilen günlük adı istenen işlem tamamlandığında bir saate ilişkilendirir. Örneğin, günlük 6:30 PM 31/7/2011'adresindeki GetBlob isteği tamamlanmışsa, aşağıdaki ön ekine sahip yazılacak: `blob/2011/07/31/1800/`

### <a name="log-metadata"></a>Log meta verileri

 Tüm günlük bloblarını blob içerdiği günlük verileri tanımlamak için kullanılan meta verileriyle depolanır. Aşağıdaki tabloda, her meta veri özniteliği açıklanmaktadır:

|Öznitelik|Açıklama|
|---------------|-----------------|
|`LogType`|Günlük Okuma, yazma veya silme işlemleri için ilgili bilgileri içerip içermediğini açıklar. Bu değer, bir tür veya üçünü virgülle ayrılmış bir birleşimini içerebilir.<br /><br /> Örnek 1: `write`<br /><br /> Örnek 2: `read,write`<br /><br /> Örnek 3: `read,write,delete`|
|`StartTime`|En erken zamanı biçiminde günlüğündeki bir girişi `YYYY-MM-DDThh:mm:ssZ`. Örneğin, `2011-07-31T18:21:46Z`|
|`EndTime`|En son saati biçiminde günlüğündeki bir girişi `YYYY-MM-DDThh:mm:ssZ`. Örneğin, `2011-07-31T18:22:09Z`|
|`LogVersion`|Günlük biçimi sürümü.|

 Aşağıdaki listede, Yukarıdaki örneklerde kullanarak tam bir örnek meta veri görüntüler:

-   `LogType=write`
-   `StartTime=2011-07-31T18:21:46Z`
-   `EndTime=2011-07-31T18:22:09Z`
-   `LogVersion=1.0`

## <a name="enable-storage-logging"></a>Depolama günlük kaydını etkinleştirme

Azure portalı, PowerShell ve depolama SDK'ları ile depolama günlük kaydını etkinleştirebilirsiniz.

### <a name="enable-storage-logging-using-the-azure-portal"></a>Azure portalını kullanarak depolama günlük kaydını etkinleştirme  

Azure Portalı'nda **tanılama ayarları (Klasik)** dikey penceresine denetim depolama günlüğü, erişilebilir **izleme (Klasik)** bir depolama hesabının bölümünü **menü dikey penceresi** .

Günlüğe kaydetmek istediğiniz depolama hizmetleri ve günlüğe kaydedilen verileri saklama süresini (gün cinsinden) belirtebilirsiniz.  

### <a name="enable-storage-logging-using-powershell"></a>PowerShell kullanarak depolama günlük kaydını etkinleştirme  

 Depolama günlüğü, depolama hesabınızdaki Azure PowerShell cmdlet'ini kullanarak yapılandırmak için yerel makinenizde PowerShell'i kullanabilirsiniz **Get-AzureStorageServiceLoggingProperty** cmdlet'ivegeçerliayarlarıalmakiçin **Set-AzureStorageServiceLoggingProperty** geçerli ayarları değiştirmek için.  

 Depolama günlük kaydı denetimi cmdlet'lerini bir **LoggingOperations** oturum için istek türleri virgülle ayrılmış listesini içeren bir dize parametresi. Üç olası istek türleri **okuma**, **yazma**, ve **Sil**. Günlüğe kaydetmeyi geçiş yapmak için değerini kullanın. **hiçbiri** için **LoggingOperations** parametresi.  

 Günlüğünü okuma için aşağıdaki komutu geçer, yazma ve saklama süresi beş gün olarak ayarlanmış varsayılan depolama hesabınızdaki kuyruk hizmeti isteklerinde Sil:  

```powershell
Set-AzureStorageServiceLoggingProperty -ServiceType Queue -LoggingOperations read,write,delete -RetentionDays 5  
```  

 Aşağıdaki komut, tablo hizmeti, varsayılan depolama hesabı için günlük kaydını devre dışı geçer:  

```powershell
Set-AzureStorageServiceLoggingProperty -ServiceType Table -LoggingOperations none  
```  

 Azure PowerShell cmdlet'lerini, Azure aboneliğiniz ile çalışmak için yapılandırma ve kullanılacak varsayılan depolama hesabı seçme hakkında daha fazla bilgi için bkz: [Azure PowerShell'i yükleme ve yapılandırma işlemini](https://azure.microsoft.com/documentation/articles/install-configure-powershell/).  

### <a name="enable-storage-logging-programmatically"></a>Depolamayı programlı olarak günlüğü etkinleştir  

 Azure portal veya Azure PowerShell Cmdlet'lerine denetim günlüğü depolama kullanmanın yanı sıra, Azure depolama API'lerden birini kullanabilirsiniz. Örneğin, bir .NET dil kullanıyorsanız, depolama istemcisi kitaplığı kullanabilirsiniz.  

 Sınıfları **CloudBlobClient**, **CloudQueueClient**, ve **CloudTableClient** tüm yöntemlerine sahip **SetServiceProperties** ve **SetServicePropertiesAsync** süren bir **ServiceProperties** bir parametre olarak nesne. Kullanabileceğiniz **ServiceProperties** depolama günlük yapılandırma nesnesi. Örneğin, aşağıdaki C# parçacığı günlüğe kaydedilir ve sıra günlüğü saklama süresini nasıl değiştirileceğini gösterir:  

```csharp
var storageAccount = CloudStorageAccount.Parse(connStr);  
var queueClient = storageAccount.CreateCloudQueueClient();  
var serviceProperties = queueClient.GetServiceProperties();  

serviceProperties.Logging.LoggingOperations = LoggingOperations.All;  
serviceProperties.Logging.RetentionDays = 2;  

queueClient.SetServiceProperties(serviceProperties);  
```  

 Depolama günlüğü yapılandırmak için bir .NET dili kullanma hakkında daha fazla bilgi için bkz. [depolama istemci kitaplığı başvurusu](https://msdn.microsoft.com/library/azure/dn261237.aspx).  

 Depolama REST API kullanarak oturum yapılandırma hakkında genel bilgi için bkz. [etkinleştirme ve yapılandırma depolama analizi](https://msdn.microsoft.com/library/azure/hh360996.aspx).  

## <a name="download-storage-logging-log-data"></a>Günlük veri günlüğü depolama indirin

 Günlük verilerinizi analiz edin ve görüntülemek için bir yerel makineye ilgilendiğiniz günlük verileri içeren BLOB'ları indirmelisiniz. Birçok depolama tarama araçlarını depolama hesabınızdan blobları indirmek etkinleştirme; komut satırı Azure kopyalama aracı sağlanan Azure depolama ekibi de kullanabilirsiniz (**AzCopy**) günlük verilerinizi indirilemedi.  

 İlgilendiğiniz günlük verilerini indirdiğinizden emin olun ve aynı günlük verilerini birden çok kez yüklenmesini önlemek için:  

-   Tarih kullanın ve zaman adlandırma kuralı, BLOB'ları izlemek için günlük verileri içeren BLOB'ları için birden çok kez aynı verilerin yeniden yüklenmesini önlemek analiz için önce indirilip.  

-   Meta veriler, blob indirmek için gereken tam blob tanımlamak için günlük verileri tutan belirli bir süre belirlemek için günlük verileri içeren BLOB'ları üzerinde kullanın.  

> [!NOTE]
>  AzCopy Azure SDK'ın bir parçasıdır, ancak her zaman en son sürümü karşıdan yükleyebileceğiniz [ https://aka.ms/AzCopy ](https://aka.ms/AzCopy). Varsayılan olarak, AzCopy klasöre yüklenir **C:\Program Files (x86) \Microsoft SDKs\Windows Azure\AzCopy**, ve bir komut istemi veya PowerShell penceresinde aracı çalıştırmak denemeden önce bu klasörü yolunuza eklemeniz gerekir.  

 Aşağıdaki örnek, kuyruk hizmeti saat 11: 00-20 Mayıs 2014'te 09 AM ve 10'da başlayan için günlük verileri nasıl indirebileceğiniz gösterir. **/S** parametresi neden olur tarihler ve saatler günlük dosya adlarında; dayalı bir yerel klasör yapısını oluşturmak AzCopy **/V** parametresi neden olur; ayrıntılı çıktı üretmek AzCopy **/Y** parametresi, tüm yerel dosyaları üzerine yazmak AzCopy neden olur. Değiştirin **< yourstorageaccount\>**  değiştirme ve depolama hesabı adı ile **< yourstoragekey\>**  depolama hesabınızın anahtarıyla.  

```
AzCopy 'http://<yourstorageaccount>.blob.core.windows.net/$logs/queue'  'C:\Logs\Storage' '2014/05/20/09' '2014/05/20/10' '2014/05/20/11' /sourceKey:<yourstoragekey> /S /V /Y  
```  

 AzCopy, ayrıca bazı yararlı parametreler nasıl karşıdan yüklerken dosyaların son değiştirilme saati ayarlar denetleyen sahiptir ve olup daha eski veya yerel makinenizde zaten mevcut olan tüm dosyaları daha yeni olan dosyaları indirmek dener. Uygulamayı yeniden başlatılabilir modunda da çalıştırabilirsiniz. Tüm Ayrıntılar için Yardım'ı çalıştırarak görüntülemek **AzCopy /?** komutu.  

 Günlük verilerinizi programlama yoluyla indirmek nasıl bir örnek için blog gönderisine bakın [Windows Azure depolama günlüğü: İstekleri izlemek için depolama günlüklerini kullanarak](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/08/03/windows-azure-storage-logging-using-logs-to-track-storage-requests.aspx) ve arama sayfasında "DumpLogs" sözcüğü için.  

 Günlük verilerinizi yüklendiğinde, günlük girişlerini dosyalarında görüntüleyebilirsiniz. Bu günlük dosyaları, Microsoft Message Analyzer dahil olmak üzere diğer birçok günlük araçları ayrıştırmak için okuma sınırlandırılmış metin biçimi kullanın (daha fazla bilgi için kılavuzuna bakın [izleme Diagnosing ve sorun giderme Microsoft Azure depolama](storage-monitoring-diagnosing-troubleshooting.md)). Biçimlendirme, filtreleme, sıralama, günlük dosyalarının içeriğini arama ad farklı özellikleri farklı araçları vardır. Depolama günlüğü, günlük dosyası biçimi ve içerik hakkında daha fazla bilgi için bkz. [depolama analizi günlük biçimi](/rest/api/storageservices/storage-analytics-log-format) ve [depolama analizi günlüğe yazılan işlemler ve durum iletileri](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages).

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama analizi günlük biçimi](/rest/api/storageservices/storage-analytics-log-format)
* [Depolama analizi işlemleri ve durum iletileri günlüğe kaydedilir.](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages)
* [Storage Analytics ölçümleri (Klasik)](storage-analytics-metrics.md)