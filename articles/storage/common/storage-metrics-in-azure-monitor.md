---
title: Azure İzleyici'de Azure depolama ölçümleri | Microsoft Docs
description: Azure İzleyici'den sunulan yeni ölçümler hakkında bilgi edinin.
services: storage
author: fhryo-msft
ms.service: storage
ms.topic: article
ms.date: 09/05/2017
ms.author: fryu
ms.subservice: common
ms.openlocfilehash: e9d11f7426a70d058daa75466b977e47e6e33ee8
ms.sourcegitcommit: f24b62e352e0512dfa2897362021b42e0cb9549d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59505779"
---
# <a name="azure-storage-metrics-in-azure-monitor"></a>Azure İzleyici’de Azure Depolama ölçümleri

Azure depolama ölçümleri ile izleme istekleri olan kullanım eğilimlerini çözümleyebilir ve depolama hesabınızla ilgili sorunları tanılayın.

Azure İzleyici, farklı Azure Hizmetleri genelinde izleme için birleştirilmiş bir kullanıcı arabirimi sağlar. Daha fazla bilgi için [Azure İzleyici](../../monitoring-and-diagnostics/monitoring-overview.md). Azure depolama, Azure İzleyici ölçüm verilerini Azure İzleyici platforma göndererek tümleştirir.

## <a name="access-metrics"></a>Erişim ölçümleri

Azure İzleyici ölçümlerine erişim birden çok yol sağlar. Bunları erişebileceğiniz [Azure portalında](https://portal.azure.com), Event Hubs gibi analiz çözümleri ve Azure İzleyici API'leri (REST ve .NET). Daha fazla bilgi için [Azure İzleyici ölçümleri](../../monitoring-and-diagnostics/monitoring-overview-metrics.md).

Ölçümler, varsayılan olarak etkindir ve son 93 günün verilerini erişebilir. Uzun bir süre saklamak istiyorsanız ölçüm verileri bir Azure depolama hesabına arşivleyebilir. Bu yapılandırılan [tanılama ayarları](../../azure-monitor/platform/diagnostic-logs-overview.md) Azure İzleyici'de.

### <a name="access-metrics-in-the-azure-portal"></a>Azure portalında erişim ölçümleri

Azure portalında zaman içinde ölçümleri izleyebilirsiniz. Aşağıdaki örnek nasıl görüntüleneceğini gösterir **işlemleri** hesap düzeyinde.

![Azure portalında ölçümleri erişme ekran görüntüsü](./media/storage-metrics-in-azure-monitor/access-metrics-in-portal.png)

Boyutlar destekleyen ölçümler için istenen boyut değeri Metrik filtreleyebilirsiniz. Aşağıdaki örnek nasıl görüntüleneceğini gösterir **işlemleri** hesap düzeyinde değerlerini seçerek belirli bir işlemi **API adı** boyut.

![Azure portalında boyutlu ölçümler erişme ekran görüntüsü](./media/storage-metrics-in-azure-monitor/access-metrics-in-portal-with-dimension.png)

### <a name="access-metrics-with-the-rest-api"></a>REST API'si ile erişim ölçümleri

Azure İzleyicisi'nin sağladığı [REST API'leri](/rest/api/monitor/) ölçüm tanımı ve değerleri okunamıyor. Bu bölümde, depolama ölçümlerini okuma işlemini göstermektedir. Kaynak Kimliği, tüm REST API'leri kullanılır. Depolama Hizmetleri için kaynak kimliği anlama, daha fazla bilgi için lütfen okuyun.

Aşağıdaki örnek nasıl kullanılacağını gösterir [ArmClient](https://github.com/projectkudu/ARMClient) REST API ile test etmeyi kolaylaştırmak için komut satırına.

#### <a name="list-account-level-metric-definition-with-the-rest-api"></a>REST API'si ile hesap düzeyinde ölçüm tanımını listeleme

Aşağıdaki örnek, hesap düzeyinde ölçüm tanımını listeleme gösterilmiştir:

```
# Login to Azure and enter your credentials when prompted.
> armclient login

> armclient GET /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/providers/microsoft.insights/metricdefinitions?api-version=2018-01-01

```

Blob, tablo, dosya veya kuyruk ölçüm tanımlarını listelemek istiyorsanız, API ile her hizmet için farklı kaynak kimliklerini belirtmeniz gerekir.

Yanıta JSON biçiminde ölçüm tanımı içerir:

```Json
{
  "value": [
    {
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/providers/microsoft.insights/metricdefinitions/UsedCapacity",
      "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}",
      "category": "Capacity",
      "name": {
        "value": "UsedCapacity",
        "localizedValue": "Used capacity"
      },
      "isDimensionRequired": false,
      "unit": "Bytes",
      "primaryAggregationType": "Average",
      "metricAvailabilities": [
        {
          "timeGrain": "PT1M",
          "retention": "P30D"
        },
        {
          "timeGrain": "PT1H",
          "retention": "P30D"
        }
      ]
    },
    ... next metric definition
  ]
}

```

#### <a name="read-account-level-metric-values-with-the-rest-api"></a>REST API ile salt okunur hesap düzeyinde ölçüm değerleri

Aşağıdaki örnek, hesap düzeyinde ölçüm verileri okumak gösterilmektedir:

```
> armclient GET "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/providers/microsoft.insights/metrics?metricnames=Availability&api-version=2018-01-01&aggregation=Average&interval=PT1H"

```

Blob, tablo, dosya veya kuyruk için ölçüm değerleri okumak istiyorsanız, yukarıdaki örnekte, her hizmet için farklı kaynak kimliklerini API ile belirtmeniz gerekir.

Şu yanıtı JSON biçiminde ölçüm değerleri içerir:

```Json
{
  "cost": 0,
  "timespan": "2017-09-07T17:27:41Z/2017-09-07T18:27:41Z",
  "interval": "PT1H",
  "value": [
    {
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/providers/Microsoft.Insights/metrics/Availability",
      "type": "Microsoft.Insights/metrics",
      "name": {
        "value": "Availability",
        "localizedValue": "Availability"
      },
      "unit": "Percent",
      "timeseries": [
        {
          "metadatavalues": [],
          "data": [
            {
              "timeStamp": "2017-09-07T17:27:00Z",
              "average": 100.0
            }
          ]
        }
      ]
    }
  ]
}

```

### <a name="access-metrics-with-the-net-sdk"></a>.NET SDK'sı ile erişim ölçümleri

Azure İzleyicisi'nin sağladığı [.NET SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/) ölçüm tanımı ve değerleri okunamıyor. [Örnek kod](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/) SDK'sı ile farklı parametreler kullanma işlemini gösterir. Kullanmanız gereken `0.18.0-preview` veya sonraki sürümü için depolama ölçümleri. Kaynak Kimliği, .NET SDK kullanılır. Depolama Hizmetleri için kaynak kimliği anlama, daha fazla bilgi için lütfen okuyun.

Aşağıdaki örnek, Azure İzleyici .NET SDK'sı depolama ölçümlerini okuma için nasıl kullanılacağını gösterir.

#### <a name="list-account-level-metric-definition-with-the-net-sdk"></a>.NET SDK'sı ile hesap düzeyinde ölçüm tanımını listeleme

Aşağıdaki örnek, hesap düzeyinde ölçüm tanımını listeleme gösterilmiştir:

```csharp
    public static async Task ListStorageMetricDefinition()
    {
        // Resource ID for storage account
        var resourceId = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}";
        var subscriptionId = "{SubscriptionID}";
        // How to identify Tenant ID, Application ID and Access Key: https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/
        var tenantId = "{TenantID}";
        var applicationId = "{ApplicationID}";
        var accessKey = "{AccessKey}";

        // Using metrics in Azure Monitor is currently free. However, if you use additional solutions ingesting metrics data, you may be billed by these solutions. For example, you are billed by Azure Storage if you archive metrics data to an Azure Storage account. Or you are billed by Operation Management Suite (OMS) if you stream metrics data to OMS for advanced analysis.
        MonitorManagementClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;
        IEnumerable<MetricDefinition> metricDefinitions = await readOnlyClient.MetricDefinitions.ListAsync(resourceUri: resourceId, cancellationToken: new CancellationToken());

        foreach (var metricDefinition in metricDefinitions)
        {
            //Enumrate metric definition:
            //    Id
            //    ResourceId
            //    Name
            //    Unit
            //    MetricAvailabilities
            //    PrimaryAggregationType
            //    Dimensions
            //    IsDimensionRequired
        }
    }

```

Blob, tablo, dosya veya kuyruk ölçüm tanımlarını listelemek istiyorsanız, API ile her hizmet için farklı kaynak kimliklerini belirtmeniz gerekir.

#### <a name="read-metric-values-with-the-net-sdk"></a>.NET SDK'sı ile okuma ölçüm değerleri

Aşağıdaki örnek nasıl okunduğunu gösterir `UsedCapacity` hesap düzeyinde veri:

```csharp
    public static async Task ReadStorageMetricValue()
    {
        // Resource ID for storage account
        var resourceId = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}";
        var subscriptionId = "{SubscriptionID}";
        // How to identify Tenant ID, Application ID and Access Key: https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/
        var tenantId = "{TenantID}";
        var applicationId = "{ApplicationID}";
        var accessKey = "{AccessKey}";

        MonitorClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;

        Microsoft.Azure.Management.Monitor.Models.Response Response;

        string startDate = DateTime.Now.AddHours(-3).ToUniversalTime().ToString("o");
        string endDate = DateTime.Now.ToUniversalTime().ToString("o");
        string timeSpan = startDate + "/" + endDate;

        Response = await readOnlyClient.Metrics.ListAsync(
            resourceUri: resourceId,
            timespan: timeSpan,
            interval: System.TimeSpan.FromHours(1),
            metricnames: "UsedCapacity",

            aggregation: "Average",
            resultType: ResultType.Data,
            cancellationToken: CancellationToken.None);

        foreach (var metric in Response.Value)
        {
            //Enumrate metric value
            //    Id
            //    Name
            //    Type
            //    Unit
            //    Timeseries
            //        - Data
            //        - Metadatavalues
        }
    }

```

Blob, tablo, dosya veya kuyruk için ölçüm değerleri okumak istiyorsanız, yukarıdaki örnekte, her hizmet için farklı kaynak kimliklerini API ile belirtmeniz gerekir.

#### <a name="read-multi-dimensional-metric-values-with-the-net-sdk"></a>.NET SDK ile çok boyutlu ölçüm değerleri okuyun

Çok boyutlu ölçümler için belirli bir boyut değeri ölçüm verilerini okumak istiyorsanız, meta veri filtresini tanımlamanız gerekir.

Aşağıdaki örnek, birden çok boyut destekleyen ölçüm ölçüm verileri okumak gösterilmektedir:

```csharp
    public static async Task ReadStorageMetricValueTest()
    {
        // Resource ID for blob storage
        var resourceId = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/blobServices/default";
        var subscriptionId = "{SubscriptionID}";
        // How to identify Tenant ID, Application ID and Access Key: https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/
        var tenantId = "{TenantID}";
        var applicationId = "{ApplicationID}";
        var accessKey = "{AccessKey}";

        MonitorManagementClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;

        Microsoft.Azure.Management.Monitor.Models.Response Response;

        string startDate = DateTime.Now.AddHours(-3).ToUniversalTime().ToString("o");
        string endDate = DateTime.Now.ToUniversalTime().ToString("o");
        string timeSpan = startDate + "/" + endDate;
        // It's applicable to define meta data filter when a metric support dimension
        // More conditions can be added with the 'or' and 'and' operators, example: BlobType eq 'BlockBlob' or BlobType eq 'PageBlob'
        ODataQuery<MetadataValue> odataFilterMetrics = new ODataQuery<MetadataValue>(
            string.Format("BlobType eq '{0}'", "BlockBlob"));

        Response = readOnlyClient.Metrics.List(
                        resourceUri: resourceId,
                        timespan: timeSpan,
                        interval: System.TimeSpan.FromHours(1),
                        metricnames: "BlobCapacity",
                        odataQuery: odataFilterMetrics,
                        aggregation: "Average",
                        resultType: ResultType.Data);

        foreach (var metric in Response.Value)
        {
            //Enumrate metric value
            //    Id
            //    Name
            //    Type
            //    Unit
            //    Timeseries
            //        - Data
            //        - Metadatavalues
        }
    }

```

## <a name="understanding-resource-id-for-services-in-azure-storage"></a>Azure Storage Hizmetleri için kaynak kimliği anlama

Kaynak Kimliği, azure'da bir kaynağın benzersiz bir tanımlayıcıdır. Ölçüm tanımları veya değerleri okumak için Azure İzleyici REST API'sini kullandığınızda, çalışmak istediğiniz kaynak için kaynak Kimliğini kullanmanız gerekir. Kaynak Kimliği şablonu şu biçimdedir:

```
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

Depolama ölçümleri hem depolama hesabı düzeyinde hem de Azure İzleyici ile hizmet düzeyi sağlar. Örneğin, yalnızca Blob Depolama için ölçümleri de alabilirsiniz. Her düzey kendi kaynak kimliğinin, bu düzey için ölçümleri almak için kullanılır.

### <a name="resource-id-for-a-storage-account"></a>Bir depolama hesabı kaynak kimliği

Bir depolama hesabı kaynak kimliği belirlemek için biçimi gösterir.

```
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}
```

### <a name="resource-id-for-the-storage-services"></a>Depolama Hizmetleri için kaynak kimliği

Aşağıdaki Depolama hizmetlerinin her biri için kaynak Kimliğini belirtme biçimi gösterir.

* BLOB hizmeti kaynak kimliği
```
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/blobServices/default
```
* Tablo hizmeti kaynak kimliği
```
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/tableServices/default
```
* Kuyruk hizmeti kaynak kimliği
```
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/queueServices/default
```
* Dosya hizmeti kaynak kimliği
```
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/fileServices/default
```

### <a name="resource-id-in-azure-monitor-rest-api"></a>Azure İzleyici REST API kaynak kimliği

Azure İzleyici REST API'sini çağırmak çalışırken kullanılan düzeni göstermektedir.

```
GET {resourceId}/providers/microsoft.insights/metrics?{parameters}
```

## <a name="capacity-metrics"></a>Kapasite ölçümleri
Kapasite ölçüm değerleri her saat için Azure İzleyici gönderilir. Değerleri günlük olarak yenilenir. Zaman dilimi ölçüm değerleri sunulduğu zaman aralığını tanımlar. Desteklenen zaman dilimi için tüm kapasite ölçümleri bir (PT1H) saattir.

Azure depolama, Azure İzleyici'de aşağıdaki kapasite ölçümleri sağlar.

### <a name="account-level"></a>Hesap düzeyi

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
| UsedCapacity | Depolama hesabı tarafından kullanılan depolama miktarı. Standart depolama hesapları için blob, tablo, dosya ve kuyruk tarafından kullanılan kapasitesi toplamıdır. Premium depolama hesapları ve Blob Depolama hesapları için BlobCapacity ile aynı olur. <br/><br/> Birim: Bayt <br/> Toplama türü: Ortalama <br/> Değeri örneği: 1024 |

### <a name="blob-storage"></a>Blob depolama

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
| BlobCapacity | Blob Depolama depolama hesabında kullanılan toplamı. <br/><br/> Birim: Bayt <br/> Toplama türü: Ortalama <br/> Değeri örneği: 1024 <br/> Boyut: BlobType ([tanımı](#metrics-dimensions)) |
| BLOB sayısı    | Depolama hesabında blob nesne sayısı. <br/><br/> Birim: Sayı <br/> Toplama türü: Ortalama <br/> Değeri örneği: 1024 <br/> Boyut: BlobType ([tanımı](#metrics-dimensions)) |
| ContainerCount    | Depolama hesabındaki kapsayıcıları sayısı. <br/><br/> Birim: Sayı <br/> Toplama türü: Ortalama <br/> Değeri örneği: 1024 |
| IndexCapacity     | ADLS Gen2 hiyerarşik dizin tarafından kullanılan depolama miktarı <br/><br/> Birim: Bayt <br/> Toplama türü: Ortalama <br/> Değeri örneği: 1024 |

### <a name="table-storage"></a>Table Storage

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
| TableCapacity | Depolama hesabı olarak kullanılan tablo depolama miktarı. <br/><br/> Birim: Bayt <br/> Toplama türü: Ortalama <br/> Değeri örneği: 1024 |
| TableCount   | Depolama hesabındaki tablolar sayısı. <br/><br/> Birim: Sayı <br/> Toplama türü: Ortalama <br/> Değeri örneği: 1024 |
| TableEntityCount | Depolama hesabındaki tablo varlıklarının sayısı. <br/><br/> Birim: Sayı <br/> Toplama türü: Ortalama <br/> Değeri örneği: 1024 |

### <a name="queue-storage"></a>Kuyruk depolama

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
| QueueCapacity | Depolama hesabı tarafından kullanılan kuyruk depolama miktarı. <br/><br/> Birim: Bayt <br/> Toplama türü: Ortalama <br/> Değeri örneği: 1024 |
| QueueCount   | Depolama hesabındaki sıraların sayısı. <br/><br/> Birim: Sayı <br/> Toplama türü: Ortalama <br/> Değeri örneği: 1024 |
| QueueMessageCount | Depolama hesabındaki süresi dolmamış kuyruk ileti sayısı. <br/><br/>Birim: Sayı <br/> Toplama türü: Ortalama <br/> Değeri örneği: 1024 |

### <a name="file-storage"></a>Dosya depolama

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
| FileCapacity | Depolama hesabı tarafından kullanılan dosya depolama miktarı. <br/><br/> Birim: Bayt <br/> Toplama türü: Ortalama <br/> Değeri örneği: 1024 |
| FileCount   | Depolama hesabındaki dosya sayısı. <br/><br/> Birim: Sayı <br/> Toplama türü: Ortalama <br/> Değeri örneği: 1024 |
| FileShareCount | Dosya sayısı depolama hesabında paylaşır. <br/><br/> Birim: Sayı <br/> Toplama türü: Ortalama <br/> Değeri örneği: 1024 |

## <a name="transaction-metrics"></a>İşlem ölçümleri

İşlem ölçümlerini her istek, Azure Depolama'dan Azure İzleyici için bir depolama hesabına gönderilir. Depolama hesabınızda hiç etkinlik olması durumunda olacaktır veri üzerinde işlem ölçümlerini dönem içinde. Tüm işlem ölçümlerini hesabı hem de hizmet düzeyinde (Blob Depolama, tablo depolama, Azure dosyaları ve kuyruk depolama) kullanılabilir. Zaman dilimi ölçüm değerleri sunulan zaman aralığını tanımlar. Tüm işlem ölçümlerini için desteklenen zaman grains PT1H ve PT1M ' dir.

Azure depolama, Azure İzleyici'de aşağıdaki işlem ölçümlerini sağlar.

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
| İşlemler | Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız istekleri ve hata üreten istekleri içerir. <br/><br/> Birim: Sayı <br/> Toplama türü: Toplam <br/> Geçerli boyut: ResponseType, GeoType ApiName ve kimlik doğrulaması ([tanımı](#metrics-dimensions))<br/> Değeri örneği: 1024 |
| Giriş | Giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir. <br/><br/> Birim: Bayt <br/> Toplama türü: Toplam <br/> Geçerli boyut: GeoType ApiName ve kimlik doğrulaması ([tanımı](#metrics-dimensions)) <br/> Değeri örneği: 1024 |
| Çıkış | Çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz. <br/><br/> Birim: Bayt <br/> Toplama türü: Toplam <br/> Geçerli boyut: GeoType ApiName ve kimlik doğrulaması ([tanımı](#metrics-dimensions)) <br/> Değeri örneği: 1024 |
| SuccessServerLatency | Azure Depolama tarafından gerçekleştirilen başarılı bir isteği işlemek için kullanılan ortalama süre. Bu değer, Başarı E2E Gecikme Süresi’nde belirtilen ağ gecikme süresini içermez. <br/><br/> Birim: Milisaniye <br/> Toplama türü: Ortalama <br/> Geçerli boyut: GeoType ApiName ve kimlik doğrulaması ([tanımı](#metrics-dimensions)) <br/> Değeri örneği: 1024 |
| Başarı E2e | Bir depolama hizmetine yapılan başarılı isteklerin veya belirtilen API işleminin ortalama uçtan uca gecikme süresi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir. <br/><br/> Birim: Milisaniye <br/> Toplama türü: Ortalama <br/> Geçerli boyut: GeoType ApiName ve kimlik doğrulaması ([tanımı](#metrics-dimensions)) <br/> Değeri örneği: 1024 |
| Kullanılabilirlik | Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik, toplam Faturalandırılabilir isteklerin değeri ve beklenmeyen hata üreten bu istekleri dahil ilgili istek sayısına göre bölme göre hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır. <br/><br/> Birim: Yüzde <br/> Toplama türü: Ortalama <br/> Geçerli boyut: GeoType ApiName ve kimlik doğrulaması ([tanımı](#metrics-dimensions)) <br/> Değeri örneği: 99.99 |

## <a name="metrics-dimensions"></a>Ölçümleri boyutları

Azure İzleyicisi'nde ölçümler için boyut şu Azure Storage'ı destekler.

| Boyut Adı | Açıklama |
| ------------------- | ----------------- |
| BlobType | Yalnızca Blob ölçümler için blob türü. Desteklenen değerler şunlardır: **BlockBlob** ve **PageBlob**. Ekleme Blob BlockBlob içinde bulunur. |
| ResponseType | İşlem yanıt türü. Kullanılabilir değerler şunlardır: <br/><br/> <li>ServerOtherError: Açıklananlar hariç diğer tüm sunucu tarafı hatalar </li> <li> ServerBusyError: HTTP 503 durum kodu döndüren, kimliği doğrulanmış istek. </li> <li> ServerTimeoutError: HTTP 500 durum kodu döndüren, zaman aşımına uğramış ve kimliği doğrulanmış istek. Zaman aşımı bir sunucu hatası nedeniyle gerçekleşti. </li> <li> AuthorizationError: Yetkisiz veri erişimi veya yetkilendirme hatası nedeniyle başarısız olmuş bir kimliği doğrulanmış istek. </li> <li> NetworkError: Ağ hataları nedeniyle başarısız olmuş bir kimliği doğrulanmış istek. Çoğunlukla bir istemci, zaman aşımı süre sonundan önce bağlantıyı erkenden kapattığında gerçekleşir. </li> <li>    ClientThrottlingError: İstemci tarafı azaltma hatası. </li> <li> ClientTimeoutError: HTTP 500 durum kodu döndüren, zaman aşımına uğramış ve kimliği doğrulanmış istek. İstemcinin ağ zaman aşımı veya istek zaman aşımı depolama hizmetinin beklediğinden düşük bir değere ayarlanmışsa beklenen bir zaman aşımıdır. Aksi takdirde, bir ServerTimeoutError olarak bildirilir. </li> <li> ClientOtherError: Açıklananlar hariç diğer tüm istemci tarafı hatalar. </li> <li> Başarılı: Başarılı İstek. </li> <li> SuccessWithThrottling: SMB istemcisi ilk girişimleri içinde kısıtlanan ancak denemeden sonra başarılı başarılı istek.|
| GeoType | Birincil veya ikincil kümeden işlem. Kullanılabilir değerler, birincil ve ikincil içerir. Okuma erişimli coğrafi olarak yedekli Storage(RA-GRS) nesneleri ikincil kiracıdan okurken uygulanır. |
| ApiName | İşlem adı. Örneğin: <br/> <li>CreateContainer</li> <li>DeleteBlob</li> <li>GetBlob</li> Tüm işlem adları için bkz [belge](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages). |
| Authentication | İşlemlerde kullanılan kimlik doğrulaması türü. Kullanılabilir değerler şunlardır: <br/> <li>AccountKey: İşlem, depolama hesabı anahtarı ile kimlik doğrulaması yapılır.</li> <li>SAS: İşlem, paylaşılan erişim imzaları ile kimlik doğrulaması yapılır.</li> <li>OAuth: İşlem, OAuth erişim belirteçleri ile doğrulanır.</li> <li>Anonim: İşlem anonim olarak istenir. Bu denetim öncesi isteği içermez.</li> <li>AnonymousPreflight: Denetim öncesi isteği bir işlemdir.</li> |

Ölçümleri destekleyen boyutları için karşılık gelen bir ölçüm değerleri görmek için boyut değerini belirtmeniz gerekir. Örneğin bakarsanız **işlemleri** değeri başarılı yanıtlar için filtre uygulamak gereken **ResponseType** ile Boyut **başarı**. Veya bakarsanız **BLOB sayısı** değeri filtrelemek ihtiyacınız blok blobu için **BlobType** ile Boyut **BlockBlob**.

## <a name="service-continuity-of-legacy-metrics"></a>Hizmet sürekliliğini eski ölçüm

Eski ölçümler, yönetilen Azure İzleyici ölçümleri ile paralel kullanılabilir. Azure depolama ölçümleri eski hizmette sonlandırılana kadar destek aynı kalmasını sağlar.

## <a name="faq"></a>SSS

**Yeni ölçümler, Klasik depolama hesabı destekliyor mu?**

Hayır, Azure İzleyici yalnızca destek Azure Resource Manager depolama hesaplarında yeni ölçümler. Depolama hesaplarında ölçümleri kullanmak istiyorsanız, Azure Resource Manager depolama hesaplarına geçirmek gerekir. Bkz: [Azure Resource Manager'a geçiş](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-overview).

**Yönetilen diskler veya yönetilmeyen diskler için Azure depolama ölçümleri destekliyor mu?**

Hayır, Azure işlem ölçümleri disklerde destekler. Bkz: [makale](https://azure.microsoft.com/blog/per-disk-metrics-managed-disks/) daha fazla ayrıntı için.

**Harita ve yeni ölçümler ile klasik ölçüm geçirmek nasıl?**

İçinde yeni ölçümler ile klasik ölçüm arasında ayrıntılı eşleme bulabilirsiniz [Azure depolama ölçümleri geçiş](./storage-metrics-migration.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici](../../monitoring-and-diagnostics/monitoring-overview.md)
