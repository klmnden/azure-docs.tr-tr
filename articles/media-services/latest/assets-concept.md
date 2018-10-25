---
title: Azure Media Services varlıklar | Microsoft Docs
description: Bu makalede, varlıklar nedir ve Azure Media Services tarafından nasıl kullanıldıkları bir açıklama sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: 62cc4634a0f76b0562d5b3c1355a7442fc5cf989
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49985242"
---
# <a name="assets"></a>Varlıklar

Bir **varlık** dijital dosyalar (video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı alt yazı dosyaları dahil) ve bu dosyalar hakkındaki meta veriler içerir. Dijital dosyalar bir varlığa karşıya yüklendikten sonra medya kodlama ve iş akışları akış Hizmetleri'nde kullanılabilir.

Bir varlık, bir blob kapsayıcısını eşlendi [Azure depolama hesabı](storage-account-concept.md) ve varlık içindeki dosyalara kapsayıcıdaki blok blobları olarak depolanır. Varlık dosyaları depolama SDK'sı istemcileri kullanarak kapsayıcılardaki etkileşim kurabilirsiniz.

Azure Media Services hesabı genel amaçlı v2 kullandığında Blob katmanları destekler (GPv2) depolama. GPv2 ile seyrek dosyaları veya soğuk depolama taşıyabilirsiniz. Soğuk depolama (örneğin, bunlar kodlanmıştır. sonra) artık gerekli olmadığında kaynak dosyalarını arşivlemek için uygundur

Media Services v3 sürümünde, iş girdisi, varlıklar veya http (s) URL'leri oluşturulabilir. İşiniz için bir giriş olarak kullanılabilecek bir varlık oluşturmak için bkz [yerel bir dosyadan iş girdisi oluşturma](job-input-from-local-file-how-to.md).

Ayrıca, hakkında bilgi edinin [depolama hesapları Media Services](storage-account-concept.md) ve [dönüşümler ve işler](transform-concept.md).

## <a name="asset-definition"></a>Varlık tanımı

Aşağıdaki tabloda, varlık özelliklerini gösterir ve bunların tanımlarının verir.

|Ad|Tür|Açıklama|
|---|---|---|
|id|dize|Kaynak için tam kaynak kimliği.|
|ad|dize|Kaynak adı.|
|properties.alternateId |dize|Varlık alternatif kimliği.|
|properties.assetId |dize|Varlık Kimliği|
|Properties.Container |dize|Varlık blob kapsayıcısının adı.|
|Properties.Created |dize|Varlık oluşturma tarihi.|
|Properties.Description |dize|Varlık açıklaması.|
|properties.lastModified |dize|En son değiştirildiği tarih varlığı.|
|properties.storageAccountName |dize|Depolama hesabı adı.|
|properties.storageEncryptionFormat |AssetStorageEncryptionFormat |Varlık şifeleme biçimi. Bir None veya MediaStorageEncryption.|
|type|dize|Kaynak türü.|

Tam tanımı için bkz [varlıklar](https://docs.microsoft.com/rest/api/media/assets).

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
|ad|Destekler: Eq, Gt, Lt|Destekleyen: artan ve azalan|
|properties.alternateId |Destekleyen: Eq||
|properties.assetId |Destekleyen: Eq||
|Properties.Container |||
|Properties.Created|Destekler: Eq, Gt, Lt| Destekler: Artan ve azalan düzende|
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

Varlıklar oluşturduysanız veya çalışırken disk belleği koleksiyonu sildi (Bu değişiklikleri indirilmedi koleksiyonu içinde parçasıysa.) değişiklikler döndürülen sonuçlarda yansıtılır 

Aşağıdaki C# örneği, hesaptaki tüm varlıkları aracılığıyla listeleme gösterilmiştir.

```csharp
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.Assets.ListNextAsync(currentPage.NextPageLink);
}
```

Diğer örnekler için bkz [varlıklar - liste](https://docs.microsoft.com/rest/api/media/assets/list)

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
