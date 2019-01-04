---
title: Varlıklar Media Services - Azure | Microsoft Docs
description: Bu makalede, varlıklar nedir ve Azure Media Services tarafından nasıl kullanıldıkları bir açıklama sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 01/01/2018
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 8507d51f0d4d49d89fc24b38ed73df7488261daa
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2019
ms.locfileid: "53969584"
---
# <a name="assets"></a>Varlıklar

Azure Media Services, bir [varlık](https://docs.microsoft.com/rest/api/media/assets) dijital dosyalar (video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı alt yazı dosyaları dahil) ve bu dosyalar hakkındaki meta veriler içerir. Dijital dosyalar bir varlığa karşıya yüklendikten sonra medya kodlama, akış, içerik iş akışları çözümleme Hizmetleri'nde kullanılabilir. Daha fazla bilgi için [varlıklarına dijital dosyaları karşıya yükleme](#upload-digital-files-into-assets) bölümüne bakın.

Bir varlık, bir blob kapsayıcısını eşlendi [Azure depolama hesabı](storage-account-concept.md) ve varlık içindeki dosyalara kapsayıcıdaki blok blobları olarak depolanır. Media Services hesabı genel amaçlı v2 kullandığında Blob katmanları destekler (GPv2) depolama. GPv2 ile dosyalarını taşıyabilirsiniz [seyrek erişimli veya arşiv depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers). **Arşiv** depolama (örneğin, bunlar kodlanmış sonra) artık gerekli olmadığında kaynak dosyalarını arşivlemek için uygundur.

**Arşiv** kodlama iş çıktısı bir çıkış blob kapsayıcısında koyulmuş ve depolama katmanı yalnızca zaten kodlanmış çok büyük kaynak dosyaları için önerilir. Bir varlık ve akış veya içeriğinizi çözümlemek için kullanım ile ilişkilendirmek istediğiniz çıkış kapsayıcıdaki blobları bulunması gereken bir **etkin** veya **seyrek erişimli** depolama katmanı.

## <a name="asset-definition"></a>Varlık tanımı

Aşağıdaki tabloda, varlık özelliklerini gösterir ve bunların tanımlarının verir.

|Ad|Açıklama|
|---|---|
|id|Kaynak için tam kaynak kimliği.|
|ad|Kaynak adı.|
|properties.alternateId |Varlık alternatif kimliği.|
|properties.assetId |Varlık Kimliği|
|Properties.Container |Varlık blob kapsayıcısının adı.|
|Properties.Created |Varlık oluşturma tarihi.<br/> DateTime her zaman UTC biçimindedir.|
|Properties.Description|Varlık açıklaması.|
|properties.lastModified |En son değiştirildiği tarih varlığı. <br/> DateTime her zaman UTC biçimindedir.|
|properties.storageAccountName |Depolama hesabı adı.|
|properties.storageEncryptionFormat |Varlık şifeleme biçimi. Bir None veya MediaStorageEncryption.|
|type|Kaynak türü.|

Tam tanımı için bkz: [varlıklar](https://docs.microsoft.com/rest/api/media/assets).

## <a name="upload-digital-files-into-assets"></a>Dijital dosyalar varlıklarına karşıya yükleme

Genel medya Hizmetleri iş akışlarını karşıya yükleme, kodlama ve bir dosya akışı biridir. Bu bölümde, genel adımlar anlatılmaktadır.

1. Yeni bir "Giriş" varlık oluşturmak için Media Services v3 API'sini kullanın. Bu işlem, Media Services hesabınızla ilişkili depolama hesabında bir kapsayıcı oluşturur. API, kapsayıcı adı döndürür (örneğin, `"container": "asset-b8d8b68a-2d7f-4d8c-81bb-8c7bbbe67ee4"`).
   
    Bir varlık ile ilişkilendirmek istediğiniz bir blob kapsayıcısı zaten varsa, bir varlık oluştururken, kapsayıcı adı belirtebilirsiniz. Media Services şu anda yalnızca BLOB kapsayıcı kök ve değil dosya adında yolları destekler. Bu nedenle, bir kapsayıcı "input.mp4" dosya adıyla birlikte çalışır. Ancak, "videos/inputs/input.mp4" dosya adıyla bir kapsayıcı çalışmaz.

    Doğrudan herhangi bir depolama hesabı ve aboneliğinizde haklarınız kapsayıcı yüklemek için Azure CLI'yı kullanabilirsiniz. <br/>Kapsayıcı adı benzersiz olmalı ve depolama adlandırma yönergeleri izleyin. Ad, biçimlendirme medya Hizmetleri varlık kapsayıcı adı (varlık GUID'si) izleyin gerekmez. 
    
    ```azurecli
    az storage blob upload -f /path/to/file -c MyContainer -n MyBlob
    ```
2. Varlık kapsayıcısının içine dijital dosyalar karşıya yüklemek için kullanılacak olan okuma-yazma izinlerine sahip bir SAS URL'sini alın. Media Services API'sine kullanabileceğiniz [varlık kapsayıcı URL'lerin listesi](https://docs.microsoft.com/rest/api/media/assets/listcontainersas).
3. Azure depolama API veya SDK'larını (örneğin, [depolama REST API'si](../../storage/common/storage-rest-api-auth.md), [JAVA SDK'sı](../../storage/blobs/storage-quickstart-blobs-java-v10.md), veya [.NET SDK'sı](../../storage/blobs/storage-quickstart-blobs-dotnet.md)) dosyaları varlık kapsayıcıya yüklemek için. 
4. Media Services v3 API'ler, Dönüşüm ve bir iş Varlığınızı "Giriş" işlemi oluşturmak için kullanın. Daha fazla bilgi için [dönüşümler ve işler](transform-concept.md).
5. "Çıktı" varlığına içerikten Stream.

> [!TIP]
> Gösteren tam bir .NET örneği için nasıl yapılır: varlık oluşturma, depolama kapsayıcısını varlığın yazılabilir bir SAS URL'si almak, SAS URL'sini kullanarak depolama kapsayıcısına dosya karşıya yükleme, bkz [yerel bir dosyadan iş girdisi oluşturma](job-input-from-local-file-how-to.md).

### <a name="create-a-new-asset"></a>Yeni varlık oluşturma

#### <a name="rest"></a>REST

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{amsAccountName}/assets/{assetName}?api-version=2018-07-01
```

REST örnek için bkz: [REST ile bir varlık oluşturun](https://docs.microsoft.com/rest/api/media/assets/createorupdate#examples) örnek.

Bu örnek nasıl oluşturulacağını gösterir **istek gövdesi** açıklaması, kapsayıcı adı, depolama hesabı ve diğer bilgileri gibi yararlı bilgileri belirtebileceğiniz.

#### <a name="curl"></a>cURL

```cURL
curl -X PUT \
  'https://management.azure.com/subscriptions/00000000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Media/mediaServices/amsAccountName/assets/myOutputAsset?api-version=2018-07-01' \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "properties": {
    "description": "",
  }
}'
```

#### <a name="net"></a>.NET

```csharp
 Asset asset = await client.Assets.CreateOrUpdateAsync(resourceGroupName, accountName, assetName, new Asset());
```

Tam bir örnek için bkz: [yerel bir dosyadan iş girdisi oluşturma](job-input-from-local-file-how-to.md). Media Services v3 sürümünde bir işin girdisini de HTTPS URL'lerden oluşturulabilir (bkz [bir HTTPS URL'si iş girdisi oluşturma](job-input-from-http-how-to.md)).

## <a name="filtering-ordering-paging"></a>Filtreleme, sıralama, sayfalama

Media Services için varlıklar aşağıdaki OData sorgu seçeneklerini destekler: 

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

### <a name="filteringordering"></a>Filtreleme ve sıralama

Aşağıdaki tabloda bu seçeneklerin varlık özelliklerine nasıl uygulanabilir gösterilmektedir: 

|Ad|Filtre|Sipariş verme|
|---|---|---|
|id|||
|ad|Desteklediği Özel Uygulamalar: Eq, Gt, Lt|Desteklediği Özel Uygulamalar: Artan veya azalan|
|properties.alternateId |Desteklediği Özel Uygulamalar: EQ||
|properties.assetId |Desteklediği Özel Uygulamalar: EQ||
|Properties.Container |||
|Properties.Created|Desteklediği Özel Uygulamalar: Eq, Gt, Lt| Desteklediği Özel Uygulamalar: Artan veya azalan|
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

### <a name="pagination"></a>Sayfalandırma

Sayfalandırma her dört etkin sıralama düzenleri desteklenir. Şu anda, sayfa boyutu 1000'dir.

> [!TIP]
> Koleksiyon listeleme ve belirli bir sayfa bağımlı olmadan her zaman sonraki bağlantısını kullanmanız gerekir.

Sorgu yanıtına fazla öğe içeriyorsa, hizmet döndürür bir "\@odata.nextLink" sonraki sonuç sayfasını alınacağı özellik. Bu kullanılabilir sonuç kümesinin tamamı aracılığıyla sayfası. Sayfa boyutunu yapılandıramazsınız. 

Varlıklar oluşturduysanız veya çalışırken disk belleği koleksiyonu aracılığıyla silindi (Bu değişiklikleri indirilmedi koleksiyonun parçası değilse) döndürülen sonuçlarda değişiklikler yansıtılır. 

#### <a name="c-example"></a>C# örneği

Aşağıdaki C# örneği, hesaptaki tüm varlıkları aracılığıyla listeleme gösterilmiştir.

```csharp
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.Assets.ListNextAsync(currentPage.NextPageLink);
}
```

#### <a name="rest-example"></a>REST örneği

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

## <a name="storage-side-encryption"></a>Depolama tarafında şifreleme

Bekleyen veri varlıklarınızı korumanın varlıklar tarafından depolama tarafı şifrelemesi şifrelenmelidir. Aşağıdaki tabloda, depolama tarafı şifrelemesi Media Services'de nasıl çalıştığı gösterilmektedir:

|Şifreleme seçeneği|Açıklama|Media Services v2|Media Services v3|
|---|---|---|---|
|Media Services'ı depolama şifrelemesi|Media Services tarafından yönetilen AES-256 şifreleme anahtarı|Desteklenen<sup>(1)</sup>|Desteklenmeyen<sup>(2)</sup>|
|[Bekleyen veriler için depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Sunucu tarafı şifrelemesi, Azure Depolama tarafından sunulan anahtarı Azure tarafından veya müşteri tarafından yönetilen|Desteklenen|Desteklenen|
|[Depolama istemci tarafı şifreleme](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Azure depolama, anahtar Kasası'nda müşteri tarafından yönetilen bir kiracı anahtarı tarafından sunulan istemci tarafı şifreleme|Desteklenmiyor|Desteklenmiyor|

<sup>1</sup> sırada Media Services, işleme içeriğinin desteklemez, açıkta/herhangi bir biçimde şifreleme olmadan, bunu yapmanız bu nedenle önerilmez.

<sup>2</sup> , Media Services v3 (AES-256 şifreleme) depolama şifrelemesi, yalnızca varlıklarınızı Media Services v2 ile oluşturulduğunda için geriye dönük uyumluluk desteklenir. Var olan depolama ile v3 çalışır anlamı varlıklar şifreli ancak yenilerini oluşturulmasına izin vermez.

## <a name="next-steps"></a>Sonraki adımlar

[Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)

[Medya arasındaki farklar Hizmetleri v2 ve v3](migrate-from-v2-to-v3.md)
