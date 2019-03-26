---
title: Azure portalında depolama ölçümlerini etkinleştirme | Microsoft Docs
description: Blob, kuyruk, tablo ve Dosya Hizmetleri için depolama ölçümlerini etkinleştirme
services: storage
author: roygara
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 02/14/2017
ms.author: rogarana
ms.subservice: common
ms.openlocfilehash: 3fc2ebe30a9be685c62e46e351e3958eefdd6ac7
ms.sourcegitcommit: 72cc94d92928c0354d9671172979759922865615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58417760"
---
# <a name="enabling-azure-storage-metrics-and-viewing-metrics-data"></a>Azure depolama ölçümlerini etkinleştirme ve ölçüm verilerini görüntüleme
[!INCLUDE [storage-selector-portal-enable-and-view-metrics](../../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a>Genel Bakış
Depolama ölçümleri, yeni bir depolama hesabı oluşturduğunuzda varsayılan olarak etkindir. Aracılığıyla izlemek yapılandırabileceğiniz [Azure portalında](https://portal.azure.com) veya Windows PowerShell veya program aracılığıyla bir depolama istemci kitaplıkları aracılığıyla.

Bir ölçüm verileri saklama süresini yapılandırabilirsiniz: Bu ölçüm ve depolanması için gereken alanı, ücretleri için ne kadar depolama hizmeti tutar belirler. Genellikle, daha kısa bir saklama süresi için saatlik ölçümlerini daha dakika ölçümlerini dakika ölçümlerini için gereken önemli ek alanı kullanmanız gerekir. Verileri analiz etmek ve çevrimdışı analiz veya Raporlama amaçları için tutmak istediğiniz tüm ölçümleri indirmek için yeterli zamana sahip olacak bir bekletme dönemi seçin. Ayrıca ölçüm verileri depolama hesabınızdan indirdiğiniz için faturalandırılırsınız olduğunu unutmayın.

## <a name="how-to-enable-metrics-using-the-azure-portal"></a>Azure portalını kullanarak ölçümleri etkinleştirme
Ölçümlerde etkinleştirmek için bu adımları [Azure portalında](https://portal.azure.com):

1. Depolama hesabınıza gidin.
1. Seçin **tanılama** gelen **menü** bölmesi.
1. Emin **durumu** ayarlanır **üzerinde**.
1. Ölçümleri izlemek istediğiniz hizmetleri seçin.
1. Ne kadar süreyle ölçümleri korumak ve veri günlük belirtmek için bir bekletme ilkesi belirtin.
1. **Kaydet**’i seçin.

[Azure portalında](https://portal.azure.com) , depolama hesabınızdaki; dakika ölçümlerini yapılandırmak şu anda olarak etkinleştirmez, PowerShell kullanarak dakika ölçümlerini etkinleştirmeniz gerekir veya programlama yoluyla.

## <a name="how-to-enable-metrics-using-powershell"></a>PowerShell kullanarak ölçümleri etkinleştirme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure PowerShell cmdlet geçerli ayarları almak için Get-AzStorageServiceMetricsProperty ve Set-AzStorageServiceMetricsProperty cmdlet'ini kullanarak depolama hesabınızda depolama ölçümleri yapılandırmak için yerel makinenizde PowerShell'i kullanabilirsiniz Geçerli ayarları değiştirmek için.

Depolama ölçümleri denetim cmdlet'ler, aşağıdaki parametreleri kullanın:

* MetricsType: olası saat ve dakika değerleri.
* ServiceType: Blob, kuyruk ve tablo olası değerler şunlardır.
* MetricsLevel: hiçbiri, hizmet ve ServiceAndApi olası değerler şunlardır.

Örneğin, aşağıdaki komutu için varsayılan depolama hesabınızdaki Blob hizmetine dakika ölçümlerini üzerinde bekletme dönemi beş gün olarak ayarlanmış olan geçer:

```powershell
$storagecontext = New-AzStorageContext -StorageAccountName <storageaccountname> -StorageAccountKey <storageaccountkey>

Set-AzStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5`  -Context $storagecontext.context
```

Aşağıdaki komut geçerli saat ölçümleri düzeyi ve bekletme gün boyunca varsayılan depolama hesabınızdaki blob hizmetine alır:

```powershell
Get-AzStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob
```

Azure PowerShell cmdlet'lerini, Azure aboneliğiniz ile çalışmak için yapılandırma ve kullanılacak varsayılan depolama hesabı seçme hakkında daha fazla bilgi için bkz: [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview).

## <a name="how-to-enable-storage-metrics-programmatically"></a>Program aracılığıyla depolama ölçümlerini etkinleştirme
Aşağıdaki C# kod parçacığı, Ölçümler ve .NET için depolama istemci kitaplığı kullanarak Blob hizmeti için günlüğe kaydetmeyi etkinleştirmek gösterilmektedir:

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

## <a name="viewing-storage-metrics"></a>Depolama ölçümlerini görüntüleme
Depolama hesabınız izlemek için Storage Analytics ölçümleri yapılandırdıktan sonra depolama analizi ölçümleri bir dizi depolama hesabındaki iyi bilinen tablolara kaydeder. Saatlik ölçümlerini görüntülemek için grafik yapılandırabileceğiniz [Azure portalında](https://portal.azure.com):

1. Depolama hesabınıza gidin [Azure portalında](https://portal.azure.com).
1. Seçin **ölçümleri** içinde **menü** ölçümleri görüntülemek istediğiniz hizmet için bölmesi.
1. Seçin **Düzenle** yapılandırmak istediğiniz grafiği.
1. İçinde **grafiği Düzenle** bölmesinde **zaman aralığı**, **grafik türü**ve görüntülenen grafiğinde gösterilmesini istediğiniz ölçümleri.
1. **Tamam**’ı seçin

Uzun vadeli depolama için ölçümleri indirmek için veya yerel olarak analiz etmek istiyorsanız, yapmanız:

* Bu tablolarda farkındadır ve görüntülemenize ve bunları indirmenize olanak sağlayacak bir araç kullanın.
* Özel bir uygulama veya okuyun ve tabloları depolamak için kod yazın.

Depolama gözatma çok sayıda üçüncü taraf araçları bu tablolarda farkında olduğundan ve doğrudan görüntüleyebilirsiniz.
Bkz: [Azure depolama istemci araçları](storage-explorers.md) kullanılabilen araçların listesi için.

> [!NOTE]
> ' In 0.8.0 sürümünden başlayarak [Microsoft Azure Depolama Gezgini](https://storageexplorer.com/), görüntüleyin ve analiz tabulky Metrik indirin.
>
>

Analytics tablolarını program aracılığıyla erişmek için depolama hesabınızdaki tüm tabloları listelerseniz analytics tabloları görünmez unutmayın. Bunları doğrudan adıyla erişmek veya kullanabilirsiniz [CloudAnalyticsClient API](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.analytics.cloudanalyticsclient.aspx) tablo adlarının sorgulamak için .NET İstemci Kitaplığı'nda.

### <a name="hourly-metrics"></a>Saat ölçümleri
* $MetricsHourPrimaryTransactionsBlob
* $MetricsHourPrimaryTransactionsTable
* $MetricsHourPrimaryTransactionsQueue

### <a name="minute-metrics"></a>Dakika ölçümleri
* $MetricsMinutePrimaryTransactionsBlob
* $MetricsMinutePrimaryTransactionsTable
* $MetricsMinutePrimaryTransactionsQueue

### <a name="capacity"></a>Kapasite
* $MetricsCapacityBlob

Bu tablolar için şemalar tam ayrıntılarını bulabilirsiniz [Storage Analytics Ölçüm tablosu şeması](https://msdn.microsoft.com/library/azure/hh343264.aspx). Aşağıdaki örnek satırlar yalnızca bir sütun alt kümesi kullanılabilir gösterir, ancak bu ölçümler depolama ölçümleri kaydeder şekilde önemli özelliklerinden bazıları gösterilmektedir:

| PartitionKey | RowKey | Zaman damgası | TotalRequests | TotalBillableRequests | Totalıngress | TotalEgress | Kullanılabilirlik | AverageE2ELatency | AverageServerLatency | PercentSuccess |
| --- |:---:| ---:| --- | --- | --- | --- | --- | --- | --- | --- |
| 20140522T1100 |Kullanıcı; Tüm |2014-05-22T11:01:16.7650250Z |7 |7 |4003 |46801 |100 |104.4286 |6.857143 |100 |
| 20140522T1100 |Kullanıcı; QueryEntities |2014-05-22T11:01:16.7640250Z |5 |5 |2694 |45951 |100 |143.8 |7.8 |100 |
| 20140522T1100 |Kullanıcı; QueryEntity |2014-05-22T11:01:16.7650250Z |1 |1 |538 |633 |100 |3 |3 |100 |
| 20140522T1100 |Kullanıcı; UpdateEntity |2014-05-22T11:01:16.7650250Z |1 |1 |771 |217 |100 |9 |6 |100 |

Bu örnek dakika ölçümlerini veriler, bölüm anahtarı, saat dakika çözünürlükte kullanır. Satır anahtarını, satır içinde depolanan bilgi türünü tanımlar. Satır anahtarı bilgileri, erişim türünü ve istek türü iki parça oluşur:

* Erişim kullanıcı veya sistem, burada kullanıcı tüm kullanıcı isteklerini depolama hizmetini ifade eder ve depolama analizi tarafından yapılan istekleri sistem başvurduğu türüdür.
* İstek türü tüm o durumda bir Özet satırı olan veya belirli bir API'yi QueryEntity veya UpdateEntity gibi tanımlar değil.

Yukarıdaki örnek veriler (11: 00'da başlar) tek bir dakika için tüm kayıtları gösterir, istekleri sayısı QueryEntities isteklerinin sayısı artı QueryEntity sayısı için en fazla yedi toplam olan Ekle UpdateEntity istekleri kullanıcı: All satırda gösterilir. Hesaplayarak kullanıcı: All satırda ortalama uçtan uca gecikme süresi 104.4286 benzer şekilde, türetebilirsiniz ((143.8 * 5) + 3 + 9) / 7.

Lütfen unutmayın **Blob saatlik ölçüm ayarlarına** her ikisi de uygulanır **Blob kapasite ölçüm** ($MetricsCapacityBlob) ve **saatlik işlem ölçümlerini Blob** ($ MetricsHourPrimaryTransactionsBlob). Her ikisi de etkinleştirilir veya birlikte devre dışı ve aynı bekletme ilkesini kullanın.

## <a name="metrics-alerts"></a>Ölçüm uyarıları
Uyarıları Ayarlama dikkate almanız gereken [Azure portalında](https://portal.azure.com) şekilde depolama ölçümlerini otomatik olarak depolama hizmetlerinizi davranışını önemli değişiklikler hakkında bilgi edinebilirsiniz. Bu ölçüm verileri sınırlandırılmış biçimde indirmek için Depolama Gezgini aracını kullanırsanız, verileri analiz etmek için Microsoft Excel'i kullanabilirsiniz. Bkz: [Azure depolama istemci araçları](storage-explorers.md) kullanılabilir depolama alanı Gezgini Araçlar listesi. Uyarıları yapılandırabilirsiniz **uyarı kuralları** bölmesi altında erişilebilir **izleme** depolama hesabı menü bölmesinde.

> [!IMPORTANT]
> Bir depolama olay ve karşılık gelen saat veya dakika ölçüm verilerini ne zaman kaydedilir arasında bir gecikme olabilir. Veri birkaç dakika dakika ölçümlerini açarken, aynı anda yazılabilir. Önceki dakika işlemlerden sonra geçerli dakika için hareket halinde bir araya getirilebilir. Bu durumda, uyarı hizmetinin tüm kullanılabilir ölçüm verilerini beklenmedik bir şekilde tetikleme uyarılarına yol açabilir ve yapılandırılmış uyarı aralığı için yüklenmemiş olabilir.
>

## <a name="accessing-metrics-data-programmatically"></a>Ölçüm verileri program aracılığıyla erişme
Aşağıdaki kod bir dizi dakika dakika ölçümlerini erişen ve sonuçları bir konsol penceresinde görüntüler örnek C# kodunu gösterir. Azure depolama kitaplığı depolama ölçümleri tablolarında erişme basitleştirir CloudAnalyticsClient sınıfı içeren sürüm 4 kullanır.

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

## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a>Depolama ölçümleri etkinleştirdiğinizde, hangi ücretleri, uygulanır?
Yazma istekleri ölçümler için tablo varlıkları oluşturmak için tüm Azure depolama işlemleri için geçerli standart fiyatlarıyla ücretlendirilir.

Bir istemci için ölçüm verileri okuma ve silme istekleri de standart fiyatlar üzerinden Faturalanabilir niteliktedir. Veri bekletme ilkesi yapılandırdıysanız, Azure depolama eski ölçüm verileri sildiğinde, ücretlendirilmez. Ancak, analiz verilerini silmeniz halinde, hesabınızı silme işlemleri için ücret alınır.

Tabulky Metrik tarafından kullanılan kapasite de Faturalanabilir: ölçüm verilerini depolamak için kullanılan kapasite miktarı tahmin etmek için aşağıdakileri kullanabilirsiniz:

* Sonra her saat bir hizmet, her hizmetin her API yararlanıyorsa, hem hizmet hem de API düzeyi Özet etkinleştirdiyseniz yaklaşık 148 KB veri saatte ölçümleri işlem tablolarda depolanır.
* Sonra her saat bir hizmet, her hizmetin her API yararlanıyorsa, yalnızca hizmet düzeyi Özet etkinleştirdiyseniz yaklaşık 12 KB veri saatte ölçümleri işlem tablolarda depolanır.
* Bloblar için kapasite tablosunda (kullanıcı için günlükleri kabul sağlanan) her gün eklenen iki satır vardır: her gün bu tablonun boyutunu en fazla yaklaşık 300 bayt artırır, bu anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar
[Depolama günlük verilerine erişme ve günlüğü etkinleştirme](/rest/api/storageservices/Enabling-Storage-Logging-and-Accessing-Log-Data)
