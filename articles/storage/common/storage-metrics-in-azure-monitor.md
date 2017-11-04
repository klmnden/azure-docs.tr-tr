---
title: "Azure Storage ölçümleri Azure İzleyicisi'nde | Microsoft Docs"
description: "Azure izleyiciden sunulan yeni ölçümleri hakkında bilgi edinin."
services: storage
documentationcenter: na
author: fhryo-msft
manager: cbrooks
editor: fhryo-msft
ms.assetid: 
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 09/05/2017
ms.author: fryu
ms.openlocfilehash: d30a99044e335723e5d2c4bbd71fab7e4fd51145
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-storage-metrics-in-azure-monitor-preview"></a>Azure depolama ölçümlerini Azure İzleyicisi'ni (Önizleme)

Azure depolama ölçümlerini ile kullanım eğilimleri, trace istekleri, çözümleme ve depolama hesabınızla sorunlarını tanılamak.

Azure İzleyicisi farklı Azure Hizmetleri genelinde izleme için birleştirilmiş kullanıcı arabirimleri sağlar. Daha fazla bilgi için bkz: [Azure İzleyici](../../monitoring-and-diagnostics/monitoring-overview.md). Azure depolama Azure İzleyici platformuna ölçüm verileri göndererek Azure İzleyici tümleştirir.

## <a name="access-metrics"></a>Erişim ölçümleri

Azure monitör, erişim ölçümleri için birden çok yollar sağlar. Bunlardan erişebilirsiniz [Azure portal](https://portal.azure.com), operasyon Management Suite ve olay hub'ı gibi analiz çözümleri ve Azure İzleyici API'leri (REST ve .net). Daha fazla bilgi için bkz: [Azure İzleyici ölçümleri](../../monitoring-and-diagnostics/monitoring-overview-metrics.md).

Ölçümleri varsayılan olarak etkindir ve en son 30 günlük veri erişebilir. Uzun bir süre için verileri korumak gerekiyorsa, ölçüm verilerini bir Azure depolama hesabı arşivleyebilirsiniz. Bu yapılandırılan [tanılama ayarlarını](../../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings) Azure İzleyicisi'nde.

### <a name="access-metrics-in-the-azure-portal"></a>Azure portalında erişim ölçümleri

Azure portalında zamanla ölçümleri izleyebilirsiniz. Aşağıdaki nasıl görüntüleneceğini gösteren bir örnektir **UsedCapacity** hesap düzeyinde.

![Azure portalında ölçümleri erişme ekran görüntüsü](./media/storage-metrics-in-azure-monitor/access-metrics-in-portal.png)

Boyutlar destekleyen ölçümleri için istenen boyut değeriyle filtre gerekir. Aşağıdaki nasıl görüntüleneceğini gösteren bir örnektir **işlemleri** ile hesap düzeyinde **başarı** yanıt türü.

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

## <a name="billing-for-metrics"></a>Ölçümler için fatura

Azure İzleyicisi'nde ölçümleri kullanarak şu anda ücretsizdir. Ölçüm verilerini alma ek çözümler kullanırsanız, ancak, bu çözümleri tarafından fatura. Örneğin, bir Azure depolama hesabı ölçüm verilerini arşivlerseniz Azure Storage göre faturalandırılır. Veya Gelişmiş analiz için OMS ölçümleri veri akış sahipse işlemi Yönetim Paketi (OMS) göre faturalandırılır.

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

* BLOB hizmeti kaynak kimliği`
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/blobServices/default
`
* Tablo hizmeti kaynak kimliği`
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/tableServices/default
`
* Sıra hizmeti kaynak kimliği`
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/queueServices/default
`
* Dosya hizmeti kaynak kimliği`
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/fileServices/default
`

### <a name="resource-id-in-azure-monitor-rest-api"></a>REST API Azure izleyicisinde kaynak kimliği

Aşağıdaki Azure İzleyici REST API'si çağrılırken kullanılan deseni gösterir.

`
GET {resourceId}/providers/microsoft.insights/metrics?{parameters}
`

## <a name="capacity-metrics"></a>Kapasite ölçümleri

Kapasite ölçümleri değerleri Azure İzleyicisi saatte gönderilir. Değer yenilenir günlük. Zaman birimi ölçüm değerleri sunulduğu zaman aralığını tanımlar. Tüm kapasite ölçümlerini için desteklenen zaman çizgisi bir (PT1H) saattir.

Azure depolama Azure İzleyicisi'nde aşağıdaki kapasite ölçümleri sağlar.

### <a name="account-level"></a>Hesap düzeyi

| Ölçüm adı | Açıklama |
| ------------------- | ----------------- |
| UsedCapacity | Depolama hesabı tarafından kullanılan depolama alanı miktarı. Standart depolama hesapları için blob, tablo, dosya ve kuyruk tarafından kullanılan kapasitenin toplamıdır. Premium depolama hesapları ve Blob storage hesapları için bu BlobCapacity aynıdır. <br/><br/> Birim: bayt <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 |

### <a name="blob-storage"></a>Blob depolama

| Ölçüm adı | Açıklama |
| ------------------- | ----------------- |
| BlobCapacity | Depolama hesabında kullanılan Blob Depolama toplamı. <br/><br/> Birim: bayt <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 <br/> Boyut: BlobType ([tanımı](#metrics-dimensions)) |
| BLOB sayısı    | Depolama hesabında depolanan blob nesnelerin sayısı. <br/><br/> Birim: sayısı <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 <br/> Boyut: BlobType ([tanımı](#metrics-dimensions)) |
| ContainerCount    | Depolama hesabı kapsayıcı sayısı. <br/><br/> Birim: sayısı <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 |

### <a name="table-storage"></a>Table Storage

| Ölçüm adı | Açıklama |
| ------------------- | ----------------- |
| TableCapacity | Depolama hesabı tarafından kullanılan tablo depolama alanı miktarı. <br/><br/> Birim: bayt <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 |
| TableCount   | Depolama hesabı tablolarda sayısı. <br/><br/> Birim: sayısı <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 |
| TableEntityCount | Depolama hesabındaki tablo varlıkları sayısı. <br/><br/> Birim: sayısı <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 |

### <a name="queue-storage"></a>Kuyruk depolama

| Ölçüm adı | Açıklama |
| ------------------- | ----------------- |
| QueueCapacity | Kuyruk depolama depolama hesabı tarafından kullanılan miktarı. <br/><br/> Birim: bayt <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 |
| QueueCount   | Depolama hesabındaki sıraların sayısı. <br/><br/> Birim: sayısı <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 |
| QueueMessageCount | Depolama hesabındaki süresi dolmamış sıra iletilerinin sayısı. <br/><br/>Birim: sayısı <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 |

### <a name="file-storage"></a>Dosya depolama

| Ölçüm adı | Açıklama |
| ------------------- | ----------------- |
| FileCapacity | Depolama hesabı tarafından kullanılan dosya depolama alanı miktarı. <br/><br/> Birim: bayt <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 |
| FileCount   | Depolama hesabındaki dosya sayısı. <br/><br/> Birim: sayısı <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 |
| FileShareCount | Dosya sayısını ve depolama hesabında paylaşır. <br/><br/> Birim: sayısı <br/> Toplama türü: ortalama <br/> Değer örneği: 1024 |

## <a name="transaction-metrics"></a>İşlem ölçümlerini

İşlem ölçümlerini Azure depolama biriminden dakikada Azure İzleyicisi için gönderilir. Tüm işlem ölçümlerini hesabı ve hizmet düzeyinde (Blob storage, Table storage, Azure dosyaları ve kuyruk depolama) kullanılabilir. Zaman birimi ölçüm değerleri sunulan zaman aralığını tanımlar. Tüm işlem ölçümlerini desteklenen saat grains PT1H ve PT1M'dır.

Azure depolama Azure İzleyicisi'nde aşağıdaki işlem ölçümlerini sağlar.

| Ölçüm adı | Açıklama |
| ------------------- | ----------------- |
| İşlemler | Depolama hizmet ya da belirtilen API işlemi için yapılan isteklerin sayısı. Bu sayı, hataları üretilen istekleri yanı sıra başarılı ve başarısız istekleri içerir. <br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Geçerli boyutlar: ResponseType, GeoType, ApiName ([tanımı](#metrics-dimensions))<br/> Değer örneği: 1024 |
| Giriş | Giriş verileri miktarını. Bu sayı, Azure içinde giriş yanı sıra Azure Storage içine bir dış istemcinin giriş içerir. <br/><br/> Birim: bayt <br/> Toplama türü: toplam <br/> Geçerli boyutlar: GeoType, ApiName ([tanımı](#metrics-dimensions)) <br/> Değer örneği: 1024 |
| Çıkış | Çıkış veri miktarı. Bu sayı, çıkış Azure içinde yanı sıra Azure Storage içine dış bir istemciden çıkış içerir. Sonuç olarak, bu sayı Faturalanabilir çıkış yansıtmaz. <br/><br/> Birim: bayt <br/> Toplama türü: toplam <br/> Geçerli boyutlar: GeoType, ApiName ([tanımı](#metrics-dimensions)) <br/> Değer örneği: 1024 |
| SuccessServerLatency | Başarılı bir isteği işlemek için Azure Storage tarafından kullanılan ortalama süre. Bu değer SuccessE2ELatency içinde belirtilen ağ gecikme süresi dahil değildir. <br/><br/> Birim: milisaniye <br/> Toplama türü: ortalama <br/> Geçerli boyutlar: GeoType, ApiName ([tanımı](#metrics-dimensions)) <br/> Değer örneği: 1024 |
| SuccessE2ELatency | Bir depolama birimi hizmeti veya belirtilen API işlemi yapılan başarılı istekleri ortalama uçtan uca gecikme. Bu değer, istek okuma, bir yanıt gönderir ve yanıtın bildirim almak için Azure Storage içinde gerekli işleme süresini içerir. <br/><br/> Birim: milisaniye <br/> Toplama türü: ortalama <br/> Geçerli boyutlar: GeoType, ApiName ([tanımı](#metrics-dimensions)) <br/> Değer örneği: 1024 |
| Kullanılabilirlik | Depolama Birimi hizmeti veya belirtilen API işlemi için yüzdesi. Kullanılabilirlik toplam Faturalandırılabilir isteklerin değerini almak ve beklenmeyen hatalar üretilen bu istekleri dahil olmak üzere, geçerli isteklerin sayısına bölünerek hesaplanır. Depolama hizmet veya belirtilen API işlemi için daha az kullanılabilirlik tüm beklenmeyen hatalar sonucu. <br/><br/> Birim: yüzde <br/> Toplama türü: ortalama <br/> Geçerli boyutlar: GeoType, ApiName ([tanımı](#metrics-dimensions)) <br/> Değer örneği: 99,99 |

## <a name="metrics-dimensions"></a>Ölçümleri boyutları

Azure Storage ölçümleri Azure İzleyicisi'nde boyutlarını aşağıdaki destekler.

| Boyut adı | Açıklama |
| ------------------- | ----------------- |
| BlobType | Blob ölçümünün yalnızca blob türü. Desteklenen değerler **BlockBlob** ve **PageBlob**. Append Blob BlockBlob içinde bulunur. |
| ResponseType | İşlem yanıt türü. Kullanılabilir değerler şunlardır: <br/><br/> <li>ServerOtherError: Diğer tüm sunucu tarafı hataları açıklanan olanlar dışındaki </li> <li> ServerBusyError: bir HTTP 503 durum kodunu döndürdü isteğin kimliği. (Henüz desteklenmez) </li> <li> ServerTimeoutError: bir HTTP 500 durum kodunu döndürdü zaman aşımına uğradı kimliği doğrulanmış istek. Zaman aşımı nedeniyle bir sunucu hatası oluştu. </li> <li> ThrottlingError: (ServerBusyError ve ClientThrottlingError desteklenen sonra kaldırılır) istemci ve sunucu tarafı azaltma hata toplamı </li> <li> AuthorizationError: veriler veya bir Yetkilendirme hatası yetkisiz erişim nedeniyle başarısız oldu kimliği doğrulanmış isteği. </li> <li> NetworkError: ağ hataları nedeniyle başarısız kimliği doğrulanmış isteği. Bir istemci zamanından önce bir bağlantı zaman aşımı geçerliliği sona ermeden önce kapandığında en yaygın olarak gerçekleşir. </li> <li>  ClientThrottlingError: (henüz desteklenmez) istemci-tarafı azaltma hata </li> <li> ClientTimeoutError: bir HTTP 500 durum kodunu döndürdü zaman aşımına uğradı kimliği doğrulanmış istek. İstemcinin ağ zaman aşımı veya isteği zaman aşımı depolama hizmeti tarafından beklenenden daha düşük bir değere ayarlanırsa, beklenen bir zaman aşımı var. Aksi takdirde, ServerTimeoutError bildirilir. </li> <li> ClientOtherError: Diğer tüm istemci-tarafı hataları açıklanan olanlar dışındaki. </li> <li> BAŞARI: Başarılı İstek|
| GeoType | Birincil veya ikincil kümeden işlem. Kullanılabilir değerler, birincil ve ikincil içerir. Nesneleri ikincil kiracısı okunurken okuma erişimi coğrafi olarak yedekli Storage(RA-GRS) için geçerlidir. |
| apiName | İşlemin adı. Örneğin: <br/> <li>CreateContainer</li> <li>DeleteBlob</li> <li>GetBlob</li> Tüm işlem adları için bkz: [belge](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages#logged-operations.md). |

Ölçümleri destekleme boyutları için karşılık gelen ölçüm değerleri görmek için boyut değerini belirtmeniz gerekir. Örneğin, bakarsanız **işlemleri** değeri başarılı yanıtlar için filtre uygulamak gereken **ResponseType** ile Boyut **başarı**. Veya bakarsanız **BLOB sayısı** değeri blok blobu için filtre uygulamak gereken **BlobType** ile Boyut **BlockBlob**.

## <a name="service-continuity-of-legacy-metrics"></a>Hizmet sürekliliğini eski ölçümleri

Eski ölçümleri Azure yönetilen İzleyici ölçümleri ile paralel olarak kullanılabilir. Azure Storage eski ölçümleri hizmette sonlanana kadar destek aynı tutar. Azure yönetilen İzleyici ölçümleri resmi sürüm sonra biz bitiş planı Duyurusu.

## <a name="see-also"></a>Ayrıca Bkz.

* [Azure İzleyici](../../monitoring-and-diagnostics/monitoring-overview.md)
