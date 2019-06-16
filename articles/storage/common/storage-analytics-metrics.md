---
title: Azure Storage analytics ölçümleri (Klasik)
description: Azure depolama ölçümleri kullanmayı öğrenin.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 03/11/2019
ms.author: normesta
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: f0dfed10190685c1d51822b8bec2b3c80cea7bb2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65153933"
---
# <a name="azure-storage-analytics-metrics-classic"></a>Azure Storage analytics ölçümleri (Klasik)

Depolama analizi, bir depolama hizmetine istekleriyle ilgili toplu işlem istatistiklerini ve kapasite verileri içeren ölçümleri depolayabilir. İşlemler her iki API düzeyinde işlemi yanı sıra depolama hizmet düzeyinde bildirilir ve kapasitesi depolama hizmet düzeyinde bildirilir. Ölçüm verileri, depolama hizmeti kullanımını çözümleme, depolama hizmeti karşı yapılan istekleri ile ilgili sorunları tanılayın ve hizmet kullanan uygulamaların performansını artırmak için kullanılabilir.  

 Storage Analytics ölçümleri, yeni depolama hesapları için varsayılan olarak etkindir. Ölçümlerde yapılandırabileceğiniz [Azure portalında](https://portal.azure.com/); Ayrıntılar için bkz: [Azure portalında depolama hesabı izleme](/azure/storage/storage-monitor-storage-account). Depolama analizi REST API veya istemci kitaplığı aracılığıyla programlı olarak da etkinleştirebilirsiniz. Depolama analizi için her hizmetini etkinleştirmek için hizmeti özelliklerini ayarla işlemleri kullanın.  

> [!NOTE]
> Storage Analytics ölçümleri Blob, kuyruk, tablo ve Dosya Hizmetleri için kullanılabilir.
> Storage Analytics ölçümleri artık Klasik ölçümleridir. Microsoft, kullanılmasını önerir [Azure İzleyici'de depolama ölçümleri](storage-metrics-in-azure-monitor.md) Storage Analytics ölçümleri yerine.

## <a name="transaction-metrics"></a>İşlem ölçümleri  
 Sağlam bir veri kümesini giriş/çıkış, kullanılabilirlik, hataları, dahil olmak üzere API işlemi, istenen her depolama hizmeti için saat veya dakika aralıklarla kaydedilir ve isteği yüzdeleri kategorilere ayrılmıştır. İşlem ayrıntıları tam listesini görebilirsiniz [Storage Analytics Ölçüm tablosu şeması](/rest/api/storageservices/storage-analytics-metrics-table-schema) konu.  

 İşlem verileri, iki düzeyde – hizmet düzeyi ve API işlemi düzeyi kaydedilir. Hizmet düzeyinde bile hiçbir istek hizmetine yapılan API işlemleri saatte bir tablo varlığına yazılan tüm özetleme istatistikleri istedi. API işlem düzeyinde istatistikleri o saat içinde istenen işlem, bir varlığa yalnızca yazılır.  

 Örneğin, gerçekleştirdiğiniz bir **GetBlob** Blob hizmeti, Storage Analytics ölçümleri hakkında daha fazla işlem isteğini kaydetmesini ve Blob hizmetine ilişkin birleşik veriler dahil yanı sıra **GetBlob** işlem. Ancak, hiçbir **GetBlob** saatte istenen işlem, bir varlık için yazılmayacak *$MetricsTransactionsBlob* bu işlem için.  

 İşlem ölçümlerini, kullanıcı isteklerini hem de depolama analizi tarafından kendisine yapılan istekler için kaydedilir. Örneğin, günlükleri ve tablo varlıkları yazma isteklerinin depolama analizi tarafından kaydedilir.

## <a name="capacity-metrics"></a>Kapasite ölçümleri  

> [!NOTE]
>  Kapasite ölçümleri şu anda yalnızca Blob hizmeti için olarak kullanılabilir.

 Kapasite verileri günlük olarak bir depolama hesabının Blob hizmeti için kaydedilir ve iki tablo varlıkları yazılır. Bir varlık için kullanıcı verileri istatistikler sağlar ve diğer ilgili istatistikler sağlar `$logs` blob kapsayıcısı depolama analizi tarafından kullanılır. *$MetricsCapacityBlob* tablo, aşağıdaki İstatistikler içerir:  

- **Kapasite**: Bayt olarak depolama hesabının Blob hizmeti tarafından kullanılan depolama miktarı.  
- **ContainerCount**: Depolama hesabının Blob hizmetindeki blob kapsayıcı sayısı.  
- **ObjectCount**: İşlenmiş ve kaydedilmemiş blok veya sayfa blobları depolama hesabının Blob hizmetindeki sayısı.  

  Kapasite ölçümleri hakkında daha fazla bilgi için bkz: [Storage Analytics Ölçüm tablosu şeması](/rest/api/storageservices/storage-analytics-metrics-table-schema).  

## <a name="how-metrics-are-stored"></a>Ölçümleri nasıl depolanır  

 Tüm ölçüm verileri Depolama hizmetlerinin her biri için bu hizmet için ayrılmış üç tablo depolanır: işlem bilgileri için bir tablo, dakikalık işlem bilgi almak için bir tabloya ve kapasite bilgileri için başka bir tablo. Kullanım verileri depolama kapasitesi bilgileri oluşur ve işlem ve dakika işlem bilgilerini istek ve yanıt verilerini oluşur. Saat ölçümleri, dakika ölçümlerini ve kapasite için bir depolama hesabının Blob hizmeti aşağıdaki tabloda açıklandığı gibi adlı tablolar erişilebilir.  

|Ölçümleri düzeyi|Tablo adları|Desteklenen sürümleri için|  
|-------------------|-----------------|----------------------------|  
|Saatlik ölçümlerini, birincil konum|-   $MetricsTransactionsBlob<br />-$MetricsTransactionsTable<br />-   $MetricsTransactionsQueue|2013-08-15 yalnızca önceki sürümler. Bu adlar hala desteklendiğinden, aşağıda listelenen tablolar'ı kullanmaya geçmeniz özellikle önerilir.|  
|Saatlik ölçümlerini, birincil konum|-   $MetricsHourPrimaryTransactionsBlob<br />-$MetricsHourPrimaryTransactionsTable<br />-   $MetricsHourPrimaryTransactionsQueue<br />-   $MetricsHourPrimaryTransactionsFile|Tüm sürümleri. Dosya hizmeti ölçümler için destek, yalnızca sürüm 2015-04-05 kullanılabilir ve üzerinde desteklenir.|  
|Dakika ölçümlerini, birincil konum|-   $MetricsMinutePrimaryTransactionsBlob<br />-$MetricsMinutePrimaryTransactionsTable<br />-   $MetricsMinutePrimaryTransactionsQueue<br />-   $MetricsMinutePrimaryTransactionsFile|Tüm sürümleri. Dosya hizmeti ölçümler için destek, yalnızca sürüm 2015-04-05 kullanılabilir ve üzerinde desteklenir.|  
|Saatlik ölçümlerini, ikincil konum|-   $MetricsHourSecondaryTransactionsBlob<br />-$MetricsHourSecondaryTransactionsTable<br />-   $MetricsHourSecondaryTransactionsQueue|Tüm sürümleri. Okuma erişimli coğrafi olarak yedekli çoğaltma etkinleştirilmiş olması gerekir.|  
|Dakika ölçümlerini, ikincil konum|-   $MetricsMinuteSecondaryTransactionsBlob<br />-$MetricsMinuteSecondaryTransactionsTable<br />-   $MetricsMinuteSecondaryTransactionsQueue|Tüm sürümleri. Okuma erişimli coğrafi olarak yedekli çoğaltma etkinleştirilmiş olması gerekir.|  
|Kapasite (yalnızca Blob hizmeti)|$MetricsCapacityBlob|Tüm sürümleri.|  

 Depolama analizi depolama hizmet uç noktası için etkinleştirildiğinde bu tabloların otomatik olarak oluşturulur. Depolama hesabı, ad alanı örneğin erişilir: `https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")`. Tabulky Metrik listeleme işlemi ile görüntülenmez ve doğrudan tablo adı erişilmelidir.  

## <a name="enable-metrics-using-the-azure-portal"></a>Azure portalını kullanarak ölçümlerini etkinleştir
Ölçümlerde etkinleştirmek için bu adımları [Azure portalında](https://portal.azure.com):

1. Depolama hesabınıza gidin.
1. Seçin **tanılama ayarları (Klasik)** gelen **menü** bölmesi.
1. Emin **durumu** ayarlanır **üzerinde**.
1. Ölçümleri izlemek istediğiniz hizmetleri seçin.
1. Ne kadar süreyle ölçümleri korumak ve veri günlük belirtmek için bir bekletme ilkesi belirtin.
1. **Kaydet**’i seçin.

[Azure portalında](https://portal.azure.com) , depolama hesabınızdaki; dakika ölçümlerini yapılandırmak şu anda olarak etkinleştirmez, PowerShell kullanarak dakika ölçümlerini etkinleştirmeniz gerekir veya programlama yoluyla.

> [!NOTE]
>  Azure portalında şu anda depolama hesabınızdaki dakika ölçümlerini yapılandırmanızı etkinleştirmez olduğunu unutmayın. PowerShell kullanarak dakika ölçümlerini etkinleştirmeniz gerekir veya programlama yoluyla.

## <a name="enable-storage-metrics-using-powershell"></a>PowerShell kullanarak depolama ölçümlerini etkinleştir  
Azure PowerShell cmdlet'ini kullanarak, depolama hesabınızda depolama ölçümleri yapılandırmak için yerel makinenizde PowerShell'i kullanabilirsiniz **Get-AzureStorageServiceMetricsProperty** cmdlet'ivegeçerliayarlarıalmakiçin **Set-AzureStorageServiceMetricsProperty** geçerli ayarları değiştirmek için.  

Depolama ölçümleri denetim cmdlet'ler, aşağıdaki parametreleri kullanın:  

* **ServiceType**, olası değer **Blob**, **kuyruk**, **tablo**, ve **dosya**.
* **MetricsType**, olası değerler **saat** ve **dakika**.  
* **MetricsLevel**, olası değerler şunlardır:
* **Hiçbiri**: İzleme devre dışı bırakır.
* **Hizmet**: Giriş/Çıkış, kullanılabilirlik, gecikme süresi ve blob, kuyruk, tablo ve Dosya Hizmetleri için toplanır başarı yüzdeleri gibi ölçümleri toplar.
* **ServiceAndApi**: Hizmet ölçümlerini ek olarak, aynı Azure depolama hizmeti API'sindeki her bir depolama işlemi için ölçüm kümesini toplar.

Örneğin, aşağıdaki komutu için varsayılan depolama hesabınızdaki blob hizmetine dakika ölçümlerini üzerinde bekletme dönemi beş gün olarak ayarlanmış olan geçer:  

```  
Set-AzureStorageServiceMetricsProperty -MetricsType Minute   
-ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5  
```  

Aşağıdaki komut geçerli saat ölçümleri düzeyi ve bekletme gün boyunca varsayılan depolama hesabınızdaki blob hizmetine alır:  

```  
Get-AzureStorageServiceMetricsProperty -MetricsType Hour   
-ServiceType Blob  
```  

Azure PowerShell cmdlet'lerini, Azure aboneliğiniz ile çalışmak için yapılandırma ve kullanılacak varsayılan depolama hesabı seçme hakkında daha fazla bilgi için bkz: [Azure PowerShell'i yükleme ve yapılandırma işlemini](https://azure.microsoft.com/documentation/articles/install-configure-powershell/).  

## <a name="enable-storage-metrics-programmatically"></a>Program aracılığıyla depolama ölçümlerini etkinleştir  
Azure portal veya Azure PowerShell Cmdlet'lerine denetim depolama ölçümleri kullanmanın yanı sıra, Azure depolama API'lerden birini kullanabilirsiniz. Örneğin, bir .NET dil kullanıyorsanız, depolama istemcisi kitaplığı kullanabilirsiniz.  

Sınıfları **CloudBlobClient**, **CloudQueueClient**, **CloudTableClient**, ve **CloudFileClient** tüm yöntemler gibi sahiptir **SetServiceProperties** ve **SetServicePropertiesAsync** süren bir **ServiceProperties** bir parametre olarak nesne. Kullanabileceğiniz **ServiceProperties** depolama ölçümlerini yapılandırma nesnesi. Örneğin, aşağıdaki C# parçacığı saatlik kuyruk ölçümleri ölçümleri düzeyi ve bekletme gün değiştirme gösterir:  

```csharp
var storageAccount = CloudStorageAccount.Parse(connStr);  
var queueClient = storageAccount.CreateCloudQueueClient();  
var serviceProperties = queueClient.GetServiceProperties();  

serviceProperties.HourMetrics.MetricsLevel = MetricsLevel.Service;  
serviceProperties.HourMetrics.RetentionDays = 10;  

queueClient.SetServiceProperties(serviceProperties);  
```  

Depolama ölçümleri yapılandırmak için bir .NET dili kullanma hakkında daha fazla bilgi için bkz. [.NET için depolama istemci Kitaplığı](https://msdn.microsoft.com/library/azure/mt347887.aspx).  

Depolama REST API kullanarak ölçümlerini yapılandırma hakkında genel bilgi için bkz. [etkinleştirme ve yapılandırma depolama analizi](/rest/api/storageservices/Enabling-and-Configuring-Storage-Analytics).  

##  <a name="viewing-storage-metrics"></a>Depolama ölçümlerini görüntüleme  
Depolama hesabınız izlemek için Storage Analytics ölçümleri yapılandırdıktan sonra depolama analizi ölçümleri bir dizi depolama hesabındaki iyi bilinen tablolara kaydeder. Saatlik ölçümlerini görüntülemek için grafik yapılandırabileceğiniz [Azure portalında](https://portal.azure.com):

1. Depolama hesabınıza gidin [Azure portalında](https://portal.azure.com).
1. Seçin **ölçümler (Klasik)** içinde **menü** ölçümleri görüntülemek istediğiniz hizmet için dikey pencere.
1. Yapılandırmak istediğiniz bir grafiğe tıklayın.
1. İçinde **grafiği Düzenle** dikey penceresinde **zaman aralığı**, **grafik türü**ve görüntülenen grafiğinde gösterilmesini istediğiniz ölçümleri.

İçinde **izleme (Klasik)** bölümünde Azure portalında depolama hesabınızın menü dikey pencerenin yapılandırabileceğiniz [uyarı kuralları](#metrics-alerts), e-posta gönderme gibi belirli bir ölçüm olduğunda sizi bilgilendirmek üzere uyarılar ulaştığında bir belirli bir değer.

Uzun vadeli depolama için ölçümleri indirmek için veya yerel olarak analiz etmek istiyorsanız, bir araç kullanın veya tabloları okumak için biraz kod yazalım. Dakika ölçümlerini analiz indirmeniz gerekir. Tablolar, depolama hesabınızdaki tüm tabloları listeleyin, ancak bunları doğrudan adını kullanarak erişebileceğiniz görünmez. Birçok depolama Tarama Aracı bu tablolarda farkında olduğundan ve bunları doğrudan görüntüleyebilirsiniz (bkz [Azure depolama istemci araçları](/azure/storage/storage-explorers) kullanılabilen araçların listesi için).

||||  
|-|-|-|  
|**Ölçümler**|**Tablo adları**|**Notlar**|  
|Saat ölçümleri|$MetricsHourPrimaryTransactionsBlob<br /><br /> $MetricsHourPrimaryTransactionsTable<br /><br /> $MetricsHourPrimaryTransactionsQueue<br /><br /> $MetricsHourPrimaryTransactionsFile|2013-08-15 ile önceki sürümlerde, bu tablolar olarak bilinirdi:<br /><br /> $MetricsTransactionsBlob<br /><br /> $MetricsTransactionsTable<br /><br /> $MetricsTransactionsQueue<br /><br /> Dosya hizmeti için ölçümleri, 2015-04-05 sürümüyle kullanılabilir başlıyoruz.|  
|Dakika ölçümleri|$MetricsMinutePrimaryTransactionsBlob<br /><br /> $MetricsMinutePrimaryTransactionsTable<br /><br /> $MetricsMinutePrimaryTransactionsQueue<br /><br /> $MetricsMinutePrimaryTransactionsFile|PowerShell kullanarak yalnızca etkinleştirilebilir veya programlama yoluyla.<br /><br /> Dosya hizmeti için ölçümleri, 2015-04-05 sürümüyle kullanılabilir başlıyoruz.|  
|Kapasite|$MetricsCapacityBlob|Yalnızca BLOB hizmeti.|  

Bu tablolar için şemalar tam ayrıntılarını bulabilirsiniz [Storage Analytics Ölçüm tablosu şeması](/rest/api/storageservices/storage-analytics-metrics-table-schema). Aşağıdaki örnek satırlar yalnızca bir sütun alt kümesi kullanılabilir gösterir, ancak bu ölçümler depolama ölçümleri kaydeder şekilde önemli özelliklerinden bazıları gösterilmektedir:  

||||||||||||  
|-|-|-|-|-|-|-|-|-|-|-|  
|**partitionKey**|**RowKey**|**Zaman damgası**|**TotalRequests**|**TotalBillableRequests**|**Totalıngress**|**TotalEgress**|**Kullanılabilirlik**|**AverageE2ELatency**|**AverageServerLatency**|**PercentSuccess**|  
|20140522T1100|Kullanıcı; Tüm|2014-05-22T11:01:16.7650250Z|7|7|4003|46801|100|104.4286|6.857143|100|  
|20140522T1100|Kullanıcı; QueryEntities|2014-05-22T11:01:16.7640250Z|5|5|2694|45951|100|143.8|7.8|100|  
|20140522T1100|Kullanıcı; QueryEntity|2014-05-22T11:01:16.7650250Z|1|1|538|633|100|3|3|100|  
|20140522T1100|Kullanıcı; UpdateEntity|2014-05-22T11:01:16.7650250Z|1|1|771|217|100|9|6|100|  

Bu örnek dakika ölçümlerini veriler, bölüm anahtarı, saat dakika çözünürlükte kullanır. Satır anahtarı satır içinde depolanan bilgi türünü tanımlar ve bu bilgileri, erişim türünü ve istek türü iki parça oluşur:  

-   Erişim türü geçerli **kullanıcı** veya **sistem**burada **kullanıcı** tüm kullanıcı isteklerini depolama hizmetini ifade eder ve **sistem** başvurur Depolama analizi tarafından yapılan istekleri.  

-   İstek türü geçerli **tüm** bu durumda bir Özet satırı olan veya belirli API gibi tanımlar **QueryEntity** veya **UpdateEntity**.  

Yukarıdaki örnek veriler (11: 00'da başlayan fiyatlarla), bir tek dakika kadar tüm kayıtları gösterir sayısını **QueryEntities** istekleri sayısı **QueryEntity** istekleri sayısı  **UpdateEntity** istekleri en fazla yedi toplam olan ekleme gösterilen **kullanıcı: All** satır. Benzer şekilde, üzerinde bir ortalama uçtan uca gecikme süresi 104.4286 türetebilirsiniz **kullanıcı: All** hesaplama satır ((143.8 * 5) + 3 + 9) / 7.  

## <a name="metrics-alerts"></a>Ölçüm uyarıları
Uyarıları Ayarlama dikkate almanız gereken [Azure portalında](https://portal.azure.com) böylece size, depolama hizmetleri de önemli değişiklikler otomatik olarak bildirilir. Bu ölçüm verileri sınırlandırılmış biçimde indirmek için Depolama Gezgini aracını kullanırsanız, verileri analiz etmek için Microsoft Excel'i kullanabilirsiniz. Bkz: [Azure depolama istemci araçları](/azure/storage/storage-explorers) kullanılabilir depolama alanı Gezgini Araçlar listesi. Uyarıları yapılandırabilirsiniz **uyarı (Klasik)** dikey penceresinde altında erişilebilir **izleme (Klasik)** depolama hesabı menü dikey penceresinde.

> [!IMPORTANT]
> Bir depolama olay ve karşılık gelen saat veya dakika ölçüm verilerini ne zaman kaydedilir arasında bir gecikme olabilir. Dakika ölçümlerini söz konusu olduğunda, aynı anda birkaç dakikalık veri yazılabilir. Bu işlem için geçerli dakika için hareket halinde toplanmakta olan önceki dakika neden olabilir. Bu durumda, uyarı hizmetinin tüm kullanılabilir ölçüm verilerini beklenmedik bir şekilde tetikleme uyarılarına yol açabilir ve yapılandırılmış uyarı aralığı için yüklenmemiş olabilir.
>

## <a name="accessing-metrics-data-programmatically"></a>Ölçüm verileri program aracılığıyla erişme  
Aşağıdaki kod bir dizi dakika dakika ölçümlerini erişen ve sonuçları bir konsol penceresinde görüntüler örnek C# kodunu gösterir. Kod örneği, Azure depolama istemci kitaplığı sürümü kullanır 4.x ya da daha sonra içeren **CloudAnalyticsClient** depolama ölçümleri tablolarında erişme basitleştirir sınıfı.  

```csharp
private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)  
{  
 // Convert the dates to the format used in the PartitionKey  
 var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");  
 var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");  

 var services = Enum.GetValues(typeof(StorageService));  
 foreach (StorageService service in services)  
 {  
     Console.WriteLine("Minute Metrics for Service {0} from {1} to {2} UTC", service, start, end);  
     var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);  
     var t = analyticsClient.GetMinuteMetricsTable(service);  
     var opContext = new OperationContext();  
     var query =  
             from entity in metricsQuery  
             // Note, you can't filter using the entity properties Time, AccessType, or TransactionType  
             // because they are calculated fields in the MetricsEntity class.  
             // The PartitionKey identifies the DataTime of the metrics.  
             where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0   
             select entity;  

     // Filter on "user" transactions after fetching the metrics from Table Storage.  
     // (StartsWith is not supported using LINQ with Azure Table storage)  
     var results = query.ToList().Where(m => m.RowKey.StartsWith("user"));  
     var resultString = results.Aggregate(new StringBuilder(), (builder, metrics) => builder.AppendLine(MetricsString(metrics, opContext))).ToString();  
     Console.WriteLine(resultString);  
 }  
}  

private static string MetricsString(MetricsEntity entity, OperationContext opContext)  
{  
 var entityProperties = entity.WriteEntity(opContext);  
 var entityString =  
         string.Format("Time: {0}, ", entity.Time) +  
         string.Format("AccessType: {0}, ", entity.AccessType) +  
         string.Format("TransactionType: {0}, ", entity.TransactionType) +  
         string.Join(",", entityProperties.Select(e => new KeyValuePair<string, string>(e.Key.ToString(), e.Value.PropertyAsObject.ToString())));  
 return entityString;  
}  
```  

## <a name="billing-on-storage-metrics"></a>Depolama ölçümleri faturalandırmayla  
Yazma istekleri ölçümler için tablo varlıkları oluşturmak için tüm Azure depolama işlemleri için geçerli standart fiyatlarıyla ücretlendirilir.  

Bir istemci tarafından ölçüm verileri okuma ve silme isteklerinin de standart fiyatlar üzerinden Faturalanabilir niteliktedir. Veri bekletme ilkesi yapılandırdıysanız, Azure depolama eski ölçüm verileri sildiğinde, ücretlendirilmez. Ancak, analiz verilerini silmeniz halinde, hesabınızı silme işlemleri için ücret alınır.  

Tabulky Metrik tarafından kullanılan kapasiteyi, ayrıca Faturalanabilir olur. Ölçüm verilerini depolamak için kullanılan kapasite miktarı tahmin etmek için aşağıdakileri kullanabilirsiniz:  

-   Sonra her saat bir hizmet, her hizmetin her API yararlanıyorsa, her iki hizmet ve API düzeyi Özet etkinleştirdiyseniz yaklaşık 148 KB veri saatte ölçümleri işlem tablolarda depolanır.  
-   Her bir saat olduğu varsa, her API hizmetinde bir hizmet kullanır ve ardından yalnızca hizmet düzeyi özeti etkinleştirdiyseniz yaklaşık 12 KB veri işlem tabulky Metrik her saat depolanır.  
-   Kabul günlükleri için açma sağlanan bloblar için kapasite tablosunda her gün eklenen iki satır vardır. Bu, her gün, bu tablonun boyutunu tarafından yaklaşık 300 bayt kadar arttığını gösterir.

## <a name="next-steps"></a>Sonraki adımlar
* [Bir depolama hesabı izleme](https://www.windowsazure.com/manage/services/storage/how-to-monitor-a-storage-account/)   
* [Storage Analytics Ölçüm tablosu şeması](/rest/api/storageservices/storage-analytics-metrics-table-schema)   
* [Depolama analizi işlemleri ve durum iletileri günlüğe kaydedilir.](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages)   
* [Depolama analizi günlük kaydı](storage-analytics-logging.md)
