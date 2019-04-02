---
title: V3 API'ler - Azure ile geliştirme | Microsoft Docs
description: Bu makalede, Media Services v3 ile geliştirirken varlıkları ve API'ler için geçerli kurallar açıklanmaktadır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/30/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 6bf0efd6994315d56e7b1b2447ffed9154cf5ee5
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58804879"
---
# <a name="developing-with-media-services-v3-apis"></a>V3 API'ler Media Services ile geliştirme

Bu makalede, Media Services v3 ile geliştirirken varlıkları ve API'ler için geçerli kurallar açıklanmaktadır.

## <a name="naming-conventions"></a>Adlandırma kuralları

Azure Media Services v3 kaynaklarının adları (Varlıklar, İşler, Dönüşümler gibi), Azure Resource Manager adlandırma kısıtlamalarına tabidir. Azure Resource Manager uyarınca kaynak adları her zaman benzersizdir. Bu nedenle kaynaklarınızda benzersiz tanıtıcı dizeleri (GUID gibi) kullanabilirsiniz. 

Media Services kaynak adları şu karakterleri içeremez: '<', '>', '%', '&', ':', '&#92;', '?', '/', '*', '+', '.', tek tırnak karakteri veya kontrol karakterleri. Diğer tüm karakterlere izin verilir. Bir kaynağın adı en fazla 260 karakter olabilir. 

Azure Resource Manager adlandırma hakkında daha fazla bilgi için bkz: [Adlandırma gereksinimlerini](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/resource-api-reference.md#arguments-for-crud-on-resource) ve [adlandırma kuralları](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).

## <a name="v3-api-design-principles"></a>V3 API tasarım ilkeleri

v3 API’nin temel tasarım ilkelerinden biri API’yi daha güvenli hale getirmektir. v3 API’ler **Get** veya **List** işlemlerinde gizli diziler ve kimlik bilgileri döndürmez. Anahtarlar her zaman null, boş veya yanıttan ayıklanmış olur. Gizli dizileri ve kimlik bilgilerini almak için ayrı bir eylem yöntemi çağırmanız gerekir. Bazı API’ler gizli dizileri alır ve görüntülerken diğer API'lerin bunu yapmaması durumunda, ayrı eylemler farklı RBAC güvenlik izinleri ayarlamanızı sağlar. RBAC kullanarak erişimi yönetme hakkında daha fazla bilgi için bkz. [Erişimi yönetmek için RBAC kullanma](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest).

Bu örnekleri:

* ContentKey değerler döndüren değil StreamingLocator alın.
* Kısıtlama anahtarlarını döndürme değil ContentKeyPolicy alın.
* İşlerin HTTP giriş URL'lerini (imza kaldırmak için) URL'si sorgu dizesi parçasını döndürme değil.

Bkz: [içerik anahtarı ilkesi - .NET edinme](get-content-key-policy-dotnet-howto.md) örnek.

## <a name="long-running-operations"></a>Uzun süre çalışan işlemler

Sahip olarak işaretlenen işlemler `x-ms-long-running-operation` Azure Media Services [dosyaları swagger](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/streamingservice.json) uzun süren işlemlere. 

Azure zaman uyumsuz işlemleri izleme hakkında daha fazla ayrıntı için bkz: [zaman uyumsuz işlemler](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations#monitor-status-of-operation)

Media Services, aşağıdaki uzun süre çalışan işlemler bulunur:

* Livestream oluşturma
* Livestream güncelleştir
* Delete LiveEvent
* Livestream Başlat
* Livestream Durdur
* Livestream Sıfırla
* LiveOutput oluşturma
* LiveOutput Sil
* StreamingEndpoint oluşturma
* Güncelleştirme StreamingEndpoint
* StreamingEndpoint Sil
* StreamingEndpoint Başlat
* StreamingEndpoint Durdur
* Ölçek StreamingEndpoint


## <a name="filtering-ordering-paging-of-media-services-entities"></a>Media Services varlıkların filtreleme, sıralama, sayfalama

Media Services Media Services v3 varlıklar için aşağıdaki OData sorgu seçeneklerini destekler: 

* $filter 
* $orderby 
* $top 
* $skiptoken 

İşleç açıklaması:

* EQ eşit =
* Ne = eşit değil
* Ge = büyüktür veya eşittir
* Le = küçüktür veya eşittir
* Gt = büyüktür
* Lt = kısa

Datetime türü bir varlık özellikleri her zaman UTC biçimindedir.

### <a name="page-results"></a>Sonuçlar sayfası

Sorgu yanıtına fazla öğe içeriyorsa, hizmet döndürür bir "\@odata.nextLink" sonraki sonuç sayfasını alınacağı özellik. Bu kullanılabilir sonuç kümesinin tamamı aracılığıyla sayfası. Sayfa boyutunu yapılandıramazsınız. Sayfa boyutunu varlık türüne göre değişir. ayrı ayrı bölümlerde ayrıntıları için lütfen okuyun.

Varlıkları oluşturduysanız veya çalışırken disk belleği koleksiyonu aracılığıyla silindi (Bu değişiklikleri indirilmedi koleksiyonun parçası değilse) döndürülen sonuçlarda değişiklikler yansıtılır. 

> [!TIP]
> Koleksiyon listeleme ve belirli bir sayfa bağımlı olmadan her zaman sonraki bağlantısını kullanmanız gerekir.

### <a name="assets"></a>Varlıklar

#### <a name="filteringordering"></a>Filtreleme ve sıralama

Aşağıdaki tablo nasıl filtreleme ve sıralama seçenekleri uygulanabilir gösterir [varlık](https://docs.microsoft.com/rest/api/media/assets) özellikleri: 

|Ad|Filtre|Sipariş verme|
|---|---|---|
|id|||
|ad|eq, gt, lt| Artan veya azalan|
|properties.alternateId |EQ||
|properties.assetId |EQ||
|Properties.Container |||
|Properties.Created| eq, gt, lt| Artan veya azalan|
|Properties.Description |||
|properties.lastModified |||
|properties.storageAccountName |||
|properties.storageEncryptionFormat | ||
|type|||

Aşağıdaki C# örneği, oluşturulan tarih filtreleri:

```csharp
var odataQuery = new ODataQuery<Asset>("properties/created lt 2018-05-11T17:39:08.387Z");
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName, odataQuery);
```

#### <a name="pagination"></a>Sayfalandırma 

Sayfalandırma her dört etkin sıralama düzenleri desteklenir. Şu anda, sayfa boyutu 1000'dir.

##### <a name="c-example"></a>C# örneği

Aşağıdaki C# örneği, hesaptaki tüm varlıkları aracılığıyla listeleme gösterilmiştir.

```csharp
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.Assets.ListNextAsync(currentPage.NextPageLink);
}
```

##### <a name="rest-example"></a>REST örneği

$Skiptoken kullanıldığı aşağıdaki örneği göz önünde bulundurun. Değiştirdiğiniz emin *amstestaccount* hesap adınız ve küme *api sürümü* en son sürüme değeri.

Varlıklar listesi bu gibi istenmişse:

```
GET  https://management.azure.com/subscriptions/00000000-3761-485c-81bb-c50b291ce214/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01 HTTP/1.1
x-ms-client-request-id: dd57fe5d-f3be-4724-8553-4ceb1dbe5aab
Content-Type: application/json; charset=utf-8
```

Bir yanıt şuna benzer ulaşırsınız:

```
HTTP/1.1 200 OK
 
{
"value":[
{
"name":"Asset 0","id":"/subscriptions/00000000-3761-485c-81bb-c50b291ce214/resourceGroups/mediaresources/providers/Microsoft.Media/mediaservices/amstestaccount/assets/Asset 0","type":"Microsoft.Media/mediaservices/assets","properties":{
"assetId":"00000000-5a4f-470a-9d81-6037d7c23eff","created":"2018-12-11T22:12:44.98Z","lastModified":"2018-12-11T22:15:48.003Z","container":"asset-98d07299-5a4f-470a-9d81-6037d7c23eff","storageAccountName":"amsdevc1stoaccount11","storageEncryptionFormat":"None"
}
},
// lots more assets
{
"name":"Asset 517","id":"/subscriptions/00000000-3761-485c-81bb-c50b291ce214/resourceGroups/mediaresources/providers/Microsoft.Media/mediaservices/amstestaccount/assets/Asset 517","type":"Microsoft.Media/mediaservices/assets","properties":{
"assetId":"00000000-912e-447b-a1ed-0f723913b20d","created":"2018-12-11T22:14:08.473Z","lastModified":"2018-12-11T22:19:29.657Z","container":"asset-fd05a503-912e-447b-a1ed-0f723913b20d","storageAccountName":"amsdevc1stoaccount11","storageEncryptionFormat":"None"
}
}
],"@odata.nextLink":"https:// management.azure.com/subscriptions/00000000-3761-485c-81bb-c50b291ce214/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01&$skiptoken=Asset+517"
}
```

Ardından, bir get isteği göndererek sonraki sayfaya istek:

```
https://management.azure.com/subscriptions/00000000-3761-485c-81bb-c50b291ce214/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01&$skiptoken=Asset+517
```

Daha fazla diğer örnekler için bkz [varlıklar - liste](https://docs.microsoft.com/rest/api/media/assets/list)

### <a name="content-key-policies"></a>İçerik Anahtar İlkeleri

#### <a name="filteringordering"></a>Filtreleme ve sıralama

Aşağıdaki tabloda bu seçeneklerin nasıl uygulanabilir gösterilmektedir [içerik anahtar ilkeleri](https://docs.microsoft.com/rest/api/media/contentkeypolicies) özellikleri: 

|Ad|Filtre|Sipariş verme|
|---|---|---|
|id|||
|ad|Eq, ne, ge, le, gt, lt|Artan veya azalan|
|Properties.Created |Eq, ne, ge, le, gt, lt|Artan veya azalan|
|Properties.Description |Eq, ne, ge, le, gt, lt||
|properties.lastModified|Eq, ne, ge, le, gt, lt|Artan veya azalan|
|Properties.Options |||
|properties.policyId|Eq, ne||
|type|||

#### <a name="pagination"></a>Sayfalandırma

Sayfalandırma her dört etkin sıralama düzenleri desteklenir. Şu anda, sayfa boyutu 10'dur.

Aşağıdaki C# örnek gösterir tüm numaralandırma **içerik anahtar ilkeleri** hesabı.

```csharp
var firstPage = await MediaServicesArmClient.ContentKeyPolicies.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.ContentKeyPolicies.ListNextAsync(currentPage.NextPageLink);
}
```

Diğer örnekler için bkz [içerik anahtar ilkeleri - liste](https://docs.microsoft.com/rest/api/media/contentkeypolicies/list)

### <a name="jobs"></a>İşler

#### <a name="filteringordering"></a>Filtreleme ve sıralama

Aşağıdaki tabloda bu seçeneklerin nasıl uygulanabilir gösterilmektedir [işleri](https://docs.microsoft.com/rest/api/media/jobs) özellikleri: 

| Ad    | Filtre                        | Sipariş verme |
|---------|-------------------------------|-------|
| ad                    | EQ            | Artan veya azalan|
| Properties.State        | Eq, ne        |                         |
| Properties.Created      | gt, lt, le ge| Artan veya azalan|
| properties.lastModified | gt, lt, le ge | Artan veya azalan| 


#### <a name="pagination"></a>Sayfalandırma

Sayfalandırma işleri, Media Services v3 sürümünde desteklenir.

Aşağıdaki C# örnek hesabında, işlere numaralandırma gösterir.

```csharp            
List<string> jobsToDelete = new List<string>();
var pageOfJobs = client.Jobs.List(config.ResourceGroup, config.AccountName, "Encode");

bool exit;
do
{
    foreach (Job j in pageOfJobs)
    {
        jobsToDelete.Add(j.Name);
    }

    if (pageOfJobs.NextPageLink != null)
    {
        pageOfJobs = client.Jobs.ListNext(pageOfJobs.NextPageLink);
        exit = false;
    }
    else
    {
        exit = true;
    }
}
while (!exit);

```

Diğer örnekler için bkz [işler - liste](https://docs.microsoft.com/rest/api/media/jobs/list)

### <a name="streaming-locators"></a>Akış Bulucuları

#### <a name="filteringordering"></a>Filtreleme ve sıralama

Aşağıdaki tabloda bu seçeneklerin StreamingLocator özelliklerine nasıl uygulanabilir gösterilmektedir: 

|Ad|Filtre|Sipariş verme|
|---|---|---|
|id |||
|ad|Eq, ne, ge, le, gt, lt|Artan veya azalan|
|properties.alternativeMediaId  |||
|properties.assetName   |||
|properties.contentKeys |||
|Properties.Created |Eq, ne, ge, le, gt, lt|Artan veya azalan|
|properties.defaultContentKeyPolicyName |||
|properties.endTime |Eq, ne, ge, le, gt, lt|Artan veya azalan|
|properties.startTime   |||
|properties.streamingLocatorId  |||
|properties.streamingPolicyName |||
|type   |||

#### <a name="pagination"></a>Sayfalandırma

Sayfalandırma her dört etkin sıralama düzenleri desteklenir. Şu anda, sayfa boyutu 10'dur.

Aşağıdaki C# örneği, hesaptaki tüm StreamingLocators aracılığıyla listeleme gösterilmiştir.

```csharp
var firstPage = await MediaServicesArmClient.StreamingLocators.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.StreamingLocators.ListNextAsync(currentPage.NextPageLink);
}
```

Diğer örnekler için bkz [akış bulucuları - liste](https://docs.microsoft.com/rest/api/media/streaminglocators/list)

### <a name="streaming-policies"></a>Akış İlkeleri

#### <a name="filteringordering"></a>Filtreleme ve sıralama

Aşağıdaki tabloda bu seçeneklerin StreamingPolicy özelliklerine nasıl uygulanabilir gösterilmektedir: 

|Ad|Filtre|Sipariş verme|
|---|---|---|
|id|||
|ad|Eq, ne, ge, le, gt, lt|Artan veya azalan|
|properties.commonEncryptionCbcs|||
|properties.commonEncryptionCenc|||
|Properties.Created |Eq, ne, ge, le, gt, lt|Artan veya azalan|
|properties.defaultContentKeyPolicyName |||
|properties.envelopeEncryption|||
|properties.noEncryption|||
|type|||

#### <a name="pagination"></a>Sayfalandırma

Sayfalandırma her dört etkin sıralama düzenleri desteklenir. Şu anda, sayfa boyutu 10'dur.

Aşağıdaki C# örneği, hesaptaki tüm StreamingPolicies aracılığıyla listeleme gösterilmiştir.

```csharp
var firstPage = await MediaServicesArmClient.StreamingPolicies.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.StreamingPolicies.ListNextAsync(currentPage.NextPageLink);
}
```

Diğer örnekler için bkz [akış ilkeleri - liste](https://docs.microsoft.com/rest/api/media/streamingpolicies/list)


### <a name="transform"></a>Dönüşüm

#### <a name="filteringordering"></a>Filtreleme ve sıralama

Aşağıdaki tabloda bu seçeneklerin nasıl uygulanabilir gösterilmektedir [dönüştüren](https://docs.microsoft.com/rest/api/media/transforms) özellikleri: 

| Ad    | Filtre                        | Sipariş verme |
|---------|-------------------------------|-------|
| ad                    | EQ            | Artan veya azalan|
| Properties.Created      | gt, lt, le ge| Artan veya azalan|
| properties.lastModified | gt, lt, le ge | Artan veya azalan|

## <a name="next-steps"></a>Sonraki adımlar

[Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)
