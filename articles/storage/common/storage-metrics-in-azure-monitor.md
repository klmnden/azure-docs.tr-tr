---
title: Azure Storage ölçümleri Azure İzleyicisi'nde | Microsoft Docs
description: Azure izleyiciden sunulan yeni ölçümleri hakkında bilgi edinin.
services: storage
documentationcenter: na
author: fhryo-msft
manager: cbrooks
editor: fhryo-msft
ms.assetid: ''
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 09/05/2017
ms.author: fryu
ms.openlocfilehash: b1d82f9b527a62109e0301907b87bd683f9912af
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="azure-storage-metrics-in-azure-monitor"></a>Azure İzleyici’de Azure Depolama ölçümleri

Azure depolama ölçümlerini ile kullanım eğilimleri, trace istekleri, çözümleme ve depolama hesabınızla sorunlarını tanılamak.

Azure İzleyicisi farklı Azure Hizmetleri genelinde izleme için birleştirilmiş kullanıcı arabirimleri sağlar. Daha fazla bilgi için bkz: [Azure İzleyici](../../monitoring-and-diagnostics/monitoring-overview.md). Azure depolama Azure İzleyici platformuna ölçüm verileri göndererek Azure İzleyici tümleştirir.

## <a name="access-metrics"></a>Erişim ölçümleri

Azure monitör, erişim ölçümleri için birden çok yollar sağlar. Bunlardan erişebilirsiniz [Azure portal](https://portal.azure.com), operasyon Management Suite ve Event Hubs gibi analiz çözümleri ve Azure İzleyici API'leri (REST ve .net). Daha fazla bilgi için bkz: [Azure İzleyici ölçümleri](../../monitoring-and-diagnostics/monitoring-overview-metrics.md).

Ölçümleri varsayılan olarak etkindir ve son 30 gün veri erişebilir. Uzun bir süre için verileri korumak gerekiyorsa, ölçüm verilerini bir Azure depolama hesabı arşivleyebilirsiniz. Bu yapılandırılan [tanılama ayarlarını](../../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings) Azure İzleyicisi'nde.

### <a name="access-metrics-in-the-azure-portal"></a>Azure portalında erişim ölçümleri

Azure portalında zamanla ölçümleri izleyebilirsiniz. Aşağıdaki örnekte nasıl görüntüleneceğini gösterir **UsedCapacity** hesap düzeyinde.

![Azure portalında ölçümleri erişme ekran görüntüsü](./media/storage-metrics-in-azure-monitor/access-metrics-in-portal.png)

Boyutlar destekleyen ölçümleri için istenen boyut değeriyle filtre gerekir. Aşağıdaki örnekte nasıl görüntüleneceğini gösterir **işlemleri** ile hesap düzeyinde **başarı** yanıt türü.

![Azure portalında boyutla ölçümleri erişme ekran görüntüsü](./media/storage-metrics-in-azure-monitor/access-metrics-in-portal-with-dimension.png)

### <a name="access-metrics-with-the-rest-api"></a>REST API ile erişim ölçümleri

Azure İzleyicisi'nin sağladığı [REST API'leri](/rest/api/monitor/) ölçüm tanımı ve değerleri okunamıyor. Bu bölümde depolama ölçümleri gösterilmiştir. Kaynak Kimliği tüm REST API'leri kullanılır. Daha fazla bilgi için lütfen okuyun [depolama hizmetleri için kaynak kimliği anlama](#understanding-resource-id-for-services-in-storage).

Aşağıdaki örnekte nasıl kullanılacağını gösterir [ArmClient](https://github.com/projectkudu/ARMClient) REST API ile test etmeyi kolaylaştırmak için komut satırında.

#### <a name="list-account-level-metric-definition-with-the-rest-api"></a>Hesap düzeyinde ölçüm tanımı REST API ile listesi

Aşağıdaki örnek, hesap düzeyinde ölçüm tanımı listesinde gösterilmiştir:

```
# Login to Azure and enter your credentials when prompted.
> armclient login

> armclient GET /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/providers/microsoft.insights/metricdefinitions?api-version=2017-05-01-preview

```

Blob, tablo, dosya veya sıra ölçüm tanımlarını listesinde istiyorsanız, her hizmet için farklı kaynak kimlikleri API ile belirtmeniz gerekir.

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

#### <a name="read-account-level-metric-values-with-the-rest-api"></a>REST API ile okuma hesabı düzeyinde ölçüm değerleri

Aşağıdaki örnek, hesap düzeyinde ölçüm verileri okumak gösterilmektedir:

```
> armclient GET "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/providers/microsoft.insights/metrics?metric=Availability&api-version=2017-05-01-preview&aggregation=Average&interval=PT1H"

```

Blob, tablo, dosya veya sıra, ölçüm değerleri okumak istiyorsanız, yukarıdaki örnek, her hizmet için farklı kaynak kimlikleri API ile belirtmeniz gerekir.

Aşağıdaki yanıtı JSON biçiminde ölçüm değerlerini içerir:

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

### <a name="access-metrics-with-the-net-sdk"></a>.Net SDK ile erişim ölçümleri

Azure İzleyicisi'nin sağladığı [.Net SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/) ölçüm tanımı ve değerleri okunamıyor. [Örnek koduna](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/) farklı parametrelerle SDK kullanmayı gösterir. Kullanmanız gereken `0.18.0-preview` veya sonraki sürümü için depolama ölçümleri. Kaynak kimliği .net SDK kullanılır. Daha fazla bilgi için lütfen okuyun [depolama hizmetleri için kaynak kimliği anlama](#understanding-resource-id-for-services-in-storage).

Aşağıdaki örnek, depolama ölçümleri Azure Monitor .net SDK kullanmayı gösterir.

#### <a name="list-account-level-metric-definition-with-the-net-sdk"></a>Hesap düzeyinde ölçüm tanımı .net SDK ile listesi

Aşağıdaki örnek, hesap düzeyinde ölçüm tanımı listesinde gösterilmiştir:

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
        MonitorClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;
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

Blob, tablo, dosya veya sıra ölçüm tanımlarını listesinde istiyorsanız, her hizmet için farklı kaynak kimlikleri API ile belirtmeniz gerekir.

#### <a name="read-metric-values-with-the-net-sdk"></a>.Net SDK ile okuma ölçüm değerleri

Aşağıdaki örnekte nasıl okunacağını gösterir `UsedCapacity` hesabı düzeyindeki veriler:

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

        string startDate = DateTime.Now.AddHours(-3).ToString("o");
        string endDate = DateTime.Now.ToString("o");
        string timeSpan = startDate + "/" + endDate;

        Response = await readOnlyClient.Metrics.ListAsync(
            resourceUri: resourceId,
            timespan: timeSpan,
            interval: System.TimeSpan.FromHours(1),
            metric: "UsedCapacity",

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

Blob, tablo, dosya veya sıra, ölçüm değerleri okumak istiyorsanız, yukarıdaki örnek, her hizmet için farklı kaynak kimlikleri API ile belirtmeniz gerekir.

#### <a name="read-multi-dimensional-metric-values-with-the-net-sdk"></a>Çok boyutlu ölçüm değerleri .net SDK ile okuma

Çok boyutlu ölçümleri için ölçüm verilerini belirli boyut değerini okumaya istiyorsanız meta veri filtresini tanımlamanız gerekir.

Aşağıdaki örnek, birden çok boyut destekleme ölçüm ölçüm verileri okumak gösterilmektedir:

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

        MonitorClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;

        Microsoft.Azure.Management.Monitor.Models.Response Response;

        string startDate = DateTime.Now.AddHours(-3).ToString("o");
        string endDate = DateTime.Now.ToString("o");
        string timeSpan = startDate + "/" + endDate;
        // It's applicable to define meta data filter when a metric support dimension
        // More conditions can be added with the 'or' and 'and' operators, example: BlobType eq 'BlockBlob' or BlobType eq 'PageBlob'
        ODataQuery<MetadataValue> odataFilterMetrics = new ODataQuery<MetadataValue>(
            string.Format("BlobType eq '{0}'", "BlockBlob"));

        Response = readOnlyClient.Metrics.List(
                        resourceUri: resourceId,
                        timespan: timeSpan,
                        interval: System.TimeSpan.FromHours(1),
                        metric: "BlobCapacity",
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

## <a name="understanding-resource-id-for-services-in-azure-storage"></a>Azure depolama hizmetleri için kaynak kimliği anlama

Kaynak Kimliği, Azure kaynak benzersiz kimliğidir. Ölçümleri tanımları ya da değerleri okumak için Azure İzleyici REST API kullanırken, çalışmak istediğiniz kaynağı için kaynak kimliği kullanmanız gerekir. Kaynak Kimliği şablonu şu biçimdedir:

`
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
`

Depolama ölçümleri depolama hesabı düzeyi ve Azure İzleyicisi ile hizmet düzeyi sağlar. Örneğin, yalnızca Blob storage için ölçümleri alabilir. Her düzey, yalnızca o düzeyini ölçümleri almak için kullanılan kendi kaynak kimliği vardır.

### <a name="resource-id-for-a-storage-account"></a>Bir depolama hesabı için kaynak kimliği

Kaynak kimliği için bir depolama hesabı belirtmek için biçimi gösterir.

`
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}
`

### <a name="resource-id-for-the-storage-services"></a>Storage Hizmetleri için kaynak kimliği

Her Depolama Hizmetleri için kaynak Kimliğini belirtme biçimi gösterir.

* BLOB hizmeti kaynak kimliği `
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/blobServices/default
`
* Tablo hizmeti kaynak kimliği `
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/tableServices/default
`
* Sıra hizmeti kaynak kimliği `
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/queueServices/default
`
* Dosya hizmeti kaynak kimliği `
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/fileServices/default
`

### <a name="resource-id-in-azure-monitor-rest-api"></a>REST API Azure izleyicisinde kaynak kimliği

Aşağıdaki Azure İzleyici REST API'si çağrılırken kullanılan deseni gösterir.

`
GET {resourceId}/providers/microsoft.insights/metrics?{parameters}
`

## <a name="capacity-metrics"></a>Kapasite ölçümleri
Kapasite ölçümleri değerleri Azure İzleyicisi saatte gönderilir. Değerleri günlük olarak yenilenir. Zaman birimi ölçüm değerleri sunulduğu zaman aralığını tanımlar. Tüm kapasite ölçümlerini için desteklenen zaman çizgisi bir (PT1H) saattir.

Azure depolama Azure İzleyicisi'nde aşağıdaki kapasite ölçümleri sağlar.

### <a name="account-level"></a>Hesap düzeyi

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
| UsedCapacity | Depolama hesabı tarafından kullanılan depolama alanı miktarı. Standart depolama hesapları için blob, tablo, dosya ve kuyruk tarafından kullanılan kapasitenin toplamıdır. Premium depolama hesapları ve Blob storage hesapları için onu BlobCapacity aynıdır. <br/><br/> Birim: bayt <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 |

### <a name="blob-storage"></a>Blob depolama

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
| BlobCapacity | Depolama hesabında kullanılan Blob Depolama toplamı. <br/><br/> Birim: bayt <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 <br/> Boyut: BlobType ([tanımı](#metrics-dimensions)) |
| BLOB sayısı    | Depolama hesabında depolanan blob nesnelerin sayısı. <br/><br/> Birim: sayısı <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 <br/> Boyut: BlobType ([tanımı](#metrics-dimensions)) |
| ContainerCount    | Depolama hesabı kapsayıcı sayısı. <br/><br/> Birim: sayısı <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 |

### <a name="table-storage"></a>Table Storage

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
| TableCapacity | Depolama hesabı tarafından kullanılan tablo depolama alanı miktarı. <br/><br/> Birim: bayt <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 |
| TableCount   | Depolama hesabı tablolarda sayısı. <br/><br/> Birim: sayısı <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 |
| TableEntityCount | Depolama hesabındaki tablo varlıkları sayısı. <br/><br/> Birim: sayısı <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 |

### <a name="queue-storage"></a>Kuyruk depolama

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
| QueueCapacity | Kuyruk depolama depolama hesabı tarafından kullanılan miktarı. <br/><br/> Birim: bayt <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 |
| QueueCount   | Depolama hesabındaki sıraların sayısı. <br/><br/> Birim: sayısı <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 |
| QueueMessageCount | Depolama hesabındaki süresi dolmamış sıra iletilerinin sayısı. <br/><br/>Birim: sayısı <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 |

### <a name="file-storage"></a>Dosya depolama

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
| FileCapacity | Depolama hesabı tarafından kullanılan dosya depolama alanı miktarı. <br/><br/> Birim: bayt <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 |
| FileCount   | Depolama hesabındaki dosya sayısı. <br/><br/> Birim: sayısı <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 |
| FileShareCount | Dosya sayısını ve depolama hesabında paylaşır. <br/><br/> Birim: sayısı <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 |

## <a name="transaction-metrics"></a>İşlem ölçümlerini

İşlem ölçümlerini Azure depolama biriminden dakikada Azure İzleyicisi için gönderilir. Tüm işlem ölçümlerini hesabı ve hizmet düzeyinde (Blob storage, Table storage, Azure dosyaları ve kuyruk depolama) kullanılabilir. Zaman birimi ölçüm değerleri sunulan zaman aralığını tanımlar. Tüm işlem ölçümlerini desteklenen saat grains PT1H ve PT1M'dır.

Azure depolama Azure İzleyicisi'nde aşağıdaki işlem ölçümlerini sağlar.

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
| İşlemler | Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız istekleri ve hata üreten istekleri içerir. <br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Geçerli boyutlar: ResponseType, GeoType, ApiName ([tanımı](#metrics-dimensions))<br/> Değer örneği: 1024 |
| Giriş | Giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir. <br/><br/> Birim: bayt <br/> Toplama türü: toplam <br/> Geçerli boyutlar: GeoType, ApiName ([tanımı](#metrics-dimensions)) <br/> Değer örneği: 1024 |
| Çıkış | Çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz. <br/><br/> Birim: bayt <br/> Toplama türü: toplam <br/> Geçerli boyutlar: GeoType, ApiName ([tanımı](#metrics-dimensions)) <br/> Değer örneği: 1024 |
| SuccessServerLatency | Azure Depolama tarafından gerçekleştirilen başarılı bir isteği işlemek için kullanılan ortalama süre. Bu değer, Başarı E2E Gecikme Süresi’nde belirtilen ağ gecikme süresini içermez. <br/><br/> Birim: milisaniye <br/> Toplama türü: ortalama <br/> Geçerli boyutlar: GeoType, ApiName ([tanımı](#metrics-dimensions)) <br/> Değer örneği: 1024 |
| SuccessE2ELatency | Bir depolama hizmetine yapılan başarılı isteklerin veya belirtilen API işleminin ortalama uçtan uca gecikme süresi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir. <br/><br/> Birim: milisaniye <br/> Toplama türü: ortalama <br/> Geçerli boyutlar: GeoType, ApiName ([tanımı](#metrics-dimensions)) <br/> Değer örneği: 1024 |
| Kullanılabilirlik | Depolama Birimi hizmeti veya belirtilen API işlemi için yüzdesi. Kullanılabilirlik toplam Faturalandırılabilir isteklerin değerini almak ve beklenmeyen hatalar üretilen bu istekleri dahil olmak üzere, geçerli isteklerin sayısına bölünerek hesaplanır. Depolama hizmet veya belirtilen API işlemi için daha az kullanılabilirlik tüm beklenmeyen hatalar sonucu. <br/><br/> Birim: yüzde <br/> Toplama türü: ortalama <br/> Geçerli boyutlar: GeoType, ApiName ([tanımı](#metrics-dimensions)) <br/> Değer örneği: 99,99 |

## <a name="metrics-dimensions"></a>Ölçümleri boyutları

Azure Storage ölçümleri Azure İzleyicisi'nde boyutlarını aşağıdaki destekler.

| Boyut Adı | Açıklama |
| ------------------- | ----------------- |
| BlobType | Blob ölçümünün yalnızca blob türü. Desteklenen değerler **BlockBlob** ve **PageBlob**. Append Blob BlockBlob içinde bulunur. |
| ResponseType | İşlem yanıt türü. Kullanılabilir değerler şunlardır: <br/><br/> <li>ServerOtherError: Diğer tüm sunucu tarafı hataları açıklanan olanlar dışındaki </li> <li> ServerBusyError: bir HTTP 503 durum kodunu döndürdü isteğin kimliği. </li> <li> ServerTimeoutError: bir HTTP 500 durum kodunu döndürdü zaman aşımına uğradı kimliği doğrulanmış istek. Zaman aşımı nedeniyle bir sunucu hatası oluştu. </li> <li> AuthorizationError: veriler veya bir Yetkilendirme hatası yetkisiz erişim nedeniyle başarısız oldu kimliği doğrulanmış isteği. </li> <li> NetworkError: ağ hataları nedeniyle başarısız kimliği doğrulanmış isteği. Bir istemci zamanından önce bir bağlantı zaman aşımı geçerliliği sona ermeden önce kapandığında en yaygın olarak gerçekleşir. </li> <li>    ClientThrottlingError: İstemci-tarafı azaltma hata oluştu. </li> <li> ClientTimeoutError: bir HTTP 500 durum kodunu döndürdü zaman aşımına uğradı kimliği doğrulanmış istek. İstemcinin ağ zaman aşımı veya isteği zaman aşımı depolama hizmeti tarafından beklenenden daha düşük bir değere ayarlanırsa, beklenen bir zaman aşımı var. Aksi takdirde, ServerTimeoutError bildirilir. </li> <li> ClientOtherError: Diğer tüm istemci-tarafı hataları açıklanan olanlar dışındaki. </li> <li> BAŞARI: Başarılı İstek|
| GeoType | Birincil veya ikincil kümeden işlem. Kullanılabilir değerler, birincil ve ikincil içerir. Nesneleri ikincil kiracısı okunurken okuma erişimi coğrafi olarak yedekli Storage(RA-GRS) için geçerlidir. |
| apiName | İşlemin adı. Örneğin: <br/> <li>CreateContainer</li> <li>DeleteBlob</li> <li>GetBlob</li> Tüm işlem adları için bkz: [belge](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages#logged-operations.md). |

Ölçümleri destekleyen boyutlar için karşılık gelen ölçüm değerleri görmek için boyut değerini belirtmeniz gerekir. Örneğin, bakarsanız **işlemleri** değeri başarılı yanıtlar için filtre uygulamak gereken **ResponseType** ile Boyut **başarı**. Veya bakarsanız **BLOB sayısı** değeri blok blobu için filtre uygulamak gereken **BlobType** ile Boyut **BlockBlob**.

## <a name="service-continuity-of-legacy-metrics"></a>Hizmet sürekliliğini eski ölçümleri

Eski ölçümleri Azure yönetilen İzleyici ölçümleri ile paralel olarak kullanılabilir. Azure Storage eski ölçümleri hizmette sonlanana kadar destek aynı tutar.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici](../../monitoring-and-diagnostics/monitoring-overview.md)
