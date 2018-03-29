---
title: Azure portalında depolama ölçümlerini etkinleştirme | Microsoft Docs
description: Blob, kuyruk, tablo ve Dosya Hizmetleri için depolama ölçümlerini etkinleştirme
services: storage
documentationcenter: ''
author: roygara
manager: jeconnoc
editor: tysonn
ms.assetid: 0407adfc-2a41-4126-922d-b76e90b74563
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/14/2017
ms.author: rogarana
ms.openlocfilehash: 0caa4eff80877ad4bf8d501a276e82922b1a84c7
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="enabling-azure-storage-metrics-and-viewing-metrics-data"></a>Azure Storage ölçümlerini etkinleştirme ve ölçüm verilerini görüntüleme
[!INCLUDE [storage-selector-portal-enable-and-view-metrics](../../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a>Genel Bakış
Depolama ölçümleri, yeni bir depolama hesabı oluşturduğunuzda, varsayılan olarak etkindir. Aracılığıyla izlemeyi yapılandırmadan [Azure portal](https://portal.azure.com) veya Windows PowerShell veya depolama istemci kitaplıklarından birini aracılığıyla programlı olarak.

Ölçüm verileri için bekletme süresi yapılandırabilirsiniz: Bu dönem için ne kadar depolama hizmeti ölçümleri ve bunları depolamak için alan için gerekli ücretleri tutar belirler. Genellikle, daha kısa bir bekletme dönemi saatlik ölçümleri daha dakika ölçümünün dakika ölçümleri için gereken önemli ek boşluk nedeniyle kullanmanız gerekir. Verileri çözümlemek ve çevrimdışı analiz veya Raporlama amaçları için tutmak istediğiniz ölçümleri karşıdan yüklemek için yeterli zamana sahip olacağı şekilde bir bekletme dönemi seçin. Ayrıca ölçümleri veri depolama hesabınızdan indirme için faturalandırılır olduğunu unutmayın.

## <a name="how-to-enable-metrics-using-the-azure-portal"></a>Azure portalını kullanarak ölçümlerini etkinleştirme
Ölçümlerini etkinleştirmek için aşağıdaki adımları izleyin [Azure portal](https://portal.azure.com):

1. Depolama hesabınıza gidin.
1. Seçin **tanılama** gelen **menü** bölmesi.
1. Emin **durum** ayarlanır **üzerinde**.
1. Ölçümleri, izlemek istediğiniz hizmetleri seçin.
1. Ölçümleri korumak ve verileri günlüğe kaydetmek için ne kadar süreyle belirtmek için bir bekletme ilkesi belirtin.
1. **Kaydet**’i seçin.

[Azure portal](https://portal.azure.com) minute ölçümleri depolama hesabınızdaki; yapılandırmanızı şu anda olarak etkinleştirmez PowerShell kullanarak dakika ölçümlerini etkinleştirmeniz gerekir veya program aracılığıyla.

## <a name="how-to-enable-metrics-using-powershell"></a>PowerShell kullanarak ölçümlerini etkinleştirme
Geçerli ayarları değiştirmek için geçerli ayarları almak için Get-AzureStorageServiceMetricsProperty Azure PowerShell cmdlet'ini ve Set-AzureStorageServiceMetricsProperty cmdlet'ini kullanarak depolama hesabınız depolama ölçümleri yapılandırmak için yerel makinenizde PowerShell'i kullanabilirsiniz.

Depolama Ölçümleri kontrol cmdlet'leri aşağıdaki parametreleri kullanın:

* MetricsType: olası saat ve dakika değerleri.
* ServiceType: Blob, kuyruk ve tablo olası değerler şunlardır.
* MetricsLevel: olası değerler, hizmeti ve ServiceAndApi yok.

Örneğin, aşağıdaki komutu varsayılan depolama hesabınızdaki Blob hizmeti için dakika ölçümler üzerinde bekletme dönemi beş gün olarak ayarlanmış olan geçer:

```powershell
Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5`
```

Aşağıdaki komut, geçerli saatlik ölçümleri düzeyi ve saklama gün varsayılan depolama hesabınızdaki blob hizmeti için alır:

```powershell
Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob
```

Azure aboneliğinizi ve kullanmak için varsayılan depolama hesabını seçmek nasıl çalışmak için Azure PowerShell cmdlet'lerini nasıl yapılandırılacağı hakkında bilgi için bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).

## <a name="how-to-enable-storage-metrics-programmatically"></a>Depolama ölçümleri programlı olarak etkinleştirme
Aşağıdaki C# kod parçacığında, ölçümleri ve .NET için depolama istemci kitaplığı kullanılarak Blob hizmeti için günlüğe kaydetmeyi etkinleştirmek gösterilmektedir:

```csharp
//Parse the connection string for the storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

// Create service client for credentialed access to the Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Enable Storage Analytics logging and set retention policy to 10 days.
ServiceProperties properties = new ServiceProperties();
properties.Logging.LoggingOperations = LoggingOperations.All;
properties.Logging.RetentionDays = 10;
properties.Logging.Version = "1.0";

// Configure service properties for metrics. Both metrics and logging must be set at the same time.
properties.HourMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
properties.HourMetrics.RetentionDays = 10;
properties.HourMetrics.Version = "1.0";

properties.MinuteMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
properties.MinuteMetrics.RetentionDays = 10;
properties.MinuteMetrics.Version = "1.0";

// Set the default service version to be used for anonymous requests.
properties.DefaultServiceVersion = "2015-04-05";

// Set the service properties.
blobClient.SetServiceProperties(properties);
```

## <a name="viewing-storage-metrics"></a>Depolama ölçümleri görüntüleme
Depolama hesabınız izlemek için Storage Analytics ölçümleri yapılandırdıktan sonra Storage Analytics ölçümleri depolama hesabındaki iyi bilinen tablo kümesini kaydeder. Saatlik ölçümlerini görüntülemek üzere grafik yapılandırabilirsiniz [Azure portal](https://portal.azure.com):

1. Depolama hesabınıza gidin [Azure portal](https://portal.azure.com).
1. Seçin **ölçümleri** içinde **menü** ölçümleri görüntülemek istediğiniz hizmet için bölmesi.
1. Seçin **Düzenle** yapılandırmak istediğiniz grafik.
1. İçinde **grafiği Düzenle** bölmesinde, **zaman aralığı**, **grafik türü**ve görüntülenmesini istediğiniz grafikte ölçümleri.
1. **Tamam**’ı seçin

Uzun vadeli depolama için ölçümler indirmek için veya yerel olarak analiz etmek istiyorsanız, için gerekir:

* Bu tabloları farkındadır ve görüntülemek ve bunları indirmek sağlayacak bir araç kullanın.
* Özel bir uygulama ya da okuma ve tabloları depolamak için komut dosyası yazma.

Çok sayıda üçüncü taraf araçları depolama gözatma bu tabloları farkında olduğundan ve doğrudan görüntülemenizi sağlar.
Bkz: [Azure Storage istemci araçları](storage-explorers.md) kullanılabilen araçlar listesi.

> [!NOTE]
> Sürümüyle 0.8.0 başlangıç [Microsoft Azure Storage Gezgini](http://storageexplorer.com/), görüntüleyebilir ve analytics ölçümleri tabloları indirin.
> 
> 

Analytics tabloları programlı olarak erişmek için depolama hesabınızdaki tüm tabloları listelerseniz analytics tabloları görünmez unutmayın. Doğrudan adıyla erişmesine veya kullanmak [CloudAnalyticsClient API](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.analytics.cloudanalyticsclient.aspx) tablo adları sorgulamak için .NET İstemci Kitaplığı'nda.

### <a name="hourly-metrics"></a>Saatlik ölçümleri
* $MetricsHourPrimaryTransactionsBlob
* $MetricsHourPrimaryTransactionsTable
* $MetricsHourPrimaryTransactionsQueue

### <a name="minute-metrics"></a>Dakika ölçümleri
* $MetricsMinutePrimaryTransactionsBlob
* $MetricsMinutePrimaryTransactionsTable
* $MetricsMinutePrimaryTransactionsQueue

### <a name="capacity"></a>Kapasite
* $MetricsCapacityBlob

Bu tablolar şemalardan tam Ayrıntılar bulabilirsiniz [Storage Analytics Ölçüm tablosu şeması](https://msdn.microsoft.com/library/azure/hh343264.aspx). Aşağıdaki örnek satır, yalnızca bir sütun alt kümesi kullanılabilir göster ancak bu ölçümleri Storage ölçümleri kaydeder şekilde önemli özelliklerinden bazıları gösterilmektedir:

| PartitionKey | RowKey | Zaman damgası | TotalRequests | TotalBillableRequests | Totalıngress | TotalEgress | Kullanılabilirlik | AverageE2ELatency | AverageServerLatency | PercentSuccess |
| --- |:---:| ---:| --- | --- | --- | --- | --- | --- | --- | --- |
| 20140522T1100 |Kullanıcı; Tüm |2014-05-22T11:01:16.7650250Z |7 |7 |4003 |46801 |100 |104.4286 |6.857143 |100 |
| 20140522T1100 |Kullanıcı; QueryEntities |2014-05-22T11:01:16.7640250Z |5 |5 |2694 |45951 |100 |143.8 |7.8 |100 |
| 20140522T1100 |Kullanıcı; QueryEntity |2014-05-22T11:01:16.7650250Z |1 |1 |538 |633 |100 |3 |3 |100 |
| 20140522T1100 |user;UpdateEntity |2014-05-22T11:01:16.7650250Z |1 |1 |771 |217 |100 |9 |6 |100 |

Bu örnek dakika ölçümleri verilerde bölüm anahtarı süreyi dakika çözünürlükte kullanır. Satır anahtarını satır içinde depolanan bilgi türünü tanımlar. Satır anahtarı bilgileri, erişim türünü ve istek türü iki parça oluşur:

* Access kullanıcı veya sistem, burada depolama birimi hizmeti tüm kullanıcı isteklerini kullanıcı başvurduğu ve depolama analizi tarafından yapılan istekleri sistem başvurduğu türüdür.
* İstek bir Özet satırı tüm hangi durumda olduğu veya QueryEntity veya UpdateEntity gibi belirli API tanımlayan türüdür.

Yukarıdaki örnek verileri (11: 00'da başlayarak) tek bir dakika için tüm kayıtları gösterir, QueryEntities istek sayısı artı QueryEntity sayısı isteklerinin sayısı şekilde UpdateEntity kadar yedi, toplam olan eklemek ister kullanıcı: tüm satırda gösterilen. Hesaplayarak kullanıcı: tüm satırda ortalama uçtan uca gecikme 104.4286 benzer şekilde, türetilemeyeceğini ((143.8 * 5) + 3 + 9) / 7.

## <a name="metrics-alerts"></a>Ölçümleri uyarıları
Uyarıları Ayarlama göz önünde bulundurmanız gerekir [Azure portal](https://portal.azure.com) şekilde depolama ölçümleri otomatik olarak depolama hizmetlerinizi davranışlarındaki önemli değişiklikler hakkında bilgi edinebilirsiniz. Bu ölçüm verilerini sınırlandırılmış biçimde indirmek için bir Depolama Gezgini aracı kullanırsanız, Microsoft Excel verileri çözümlemek için kullanabilirsiniz. Bkz: [Azure Storage istemci araçları](storage-explorers.md) kullanılabilir Depolama Gezgini araçları listesi. Uyarılar yapılandırabilirsiniz **uyarı kuralları** bölmesi altında erişilebilir **izleme** depolama hesabı menü bölmesinde.

> [!IMPORTANT]
> Bir depolama olayı ve karşılık gelen saat veya dakika ölçüm verilerini ne zaman kaydedilir arasında bir gecikme olabilir. Dakika ölçümleri günlüğe kaydedildiğinde, birkaç dakika veri aynı anda yazılabilir. Önceki dakika hareketleri geçerli dakikada hareket halinde toplanabilecek. Bu gerçekleştiğinde, uyarı hizmetini tüm kullanılabilir ölçüm verilerini beklenmedik bir şekilde tetikleme uyarıları açabilir yapılandırılmış uyarı aralığı olmayabilir.
>

## <a name="accessing-metrics-data-programmatically"></a>Ölçüm verileri programlı olarak erişme
Aşağıdaki liste için bir aralığı dakika dakika ölçümleri erişir ve sonuçları bir konsol penceresi görüntüler örnek C# kodunu gösterir. Azure Storage kitaplığı depolama ölçümleri tablolarda erişme basitleştirir CloudAnalyticsClient sınıfı içeren 4 sürümünü kullanır.

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
        // (StartsWith is not supported using LINQ with Azure table storage)
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

## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a>Depolama ölçümleri etkinleştirdiğinizde, hangi, ücretlendirme?
Tüm Azure depolama işlemleri için geçerli standart tarifelere ölçümleri için tablo varlıkları oluşturmak için istekler ücretlendirilen yazma.

Ölçüm verilerini bir istemci tarafından okuma ve silme istekleri de standart oranlarda faturalanabilir. Bir veri bekletme ilkesi yapılandırdıysanız, Azure Storage eski ölçüm verileri sildiğinde, sizden ücret istenmese. Ancak, analiz verileri silerseniz, hesabınızı silme işlemleri için ücret kesilir.

Ölçümleri tabloları tarafından kullanılan kapasitesi de Faturalanabilir: ölçüm verilerini depolamak için kullanılan kapasite miktarı tahmin etmek için aşağıdakileri kullanın:

* Sonra her saat her hizmetin her API bir hizmet kullanırsa, hem hizmet hem de API düzey özeti etkinleştirdiyseniz, yaklaşık 148 KB veri saatte ölçümleri işlem tablolarında depolanır.
* Sonra her saat her hizmetin her API bir hizmet kullanırsa, yalnızca hizmet düzeyi Özet etkinleştirdiyseniz, yaklaşık olarak 12 KB veri saatte ölçümleri işlem tablolarda depolanır.
* BLOB'lar için kapasite tablosunda (kullanıcı, günlükleri için seçti sağlanan) her gün eklenen iki satır vardır: Bu her gün bu tablonun boyutunu tarafından yaklaşık 300 bayt kadar arttığını gösterir.

## <a name="next-steps"></a>Sonraki adımlar
[Günlüğe kaydetme ve günlük verilerine erişme depolama etkinleştirme](/rest/api/storageservices/Enabling-Storage-Logging-and-Accessing-Log-Data)
