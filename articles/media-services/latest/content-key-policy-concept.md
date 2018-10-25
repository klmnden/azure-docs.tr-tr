---
title: Azure Media Services anahtar ilkeleri içerik | Microsoft Docs
description: Bu makalede, içerik anahtarı ilkeleri nelerdir ve Azure Media Services tarafından nasıl kullanıldıkları bir açıklama sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/22/2018
ms.author: juliako
ms.openlocfilehash: 9a5ef8df9b1ca87430fb5e8d1da94f1899c4a856
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49985871"
---
# <a name="content-key-policies"></a>İçerik Anahtar İlkeleri

Azure Media Services, depolama, işleme ve teslim üzerinden bilgisayarınıza çıkışında medyanızdaki güvenliğini sağlamak için kullanabilirsiniz. Media Services sayesinde, Gelişmiş Şifreleme Standardı (AES-128) veya üç ana dijital hak yönetimi (DRM) sistemlerinden ile dinamik olarak şifrelenmiş canlı ve isteğe bağlı içerik teslim edebilirsiniz: Microsoft PlayReady ve Google Widevine Apple FairPlay. Media Services de AES anahtarları ve DRM sunmaya yönelik bir hizmet sağlar (PlayReady, Widevine ve FairPlay) lisansları yetkili istemcilere.

Azure Media Services v3 sürümünde içerik anahtar ilkeleri, Media Services anahtar teslim bileşen aracılığıyla istemciler sonlandırmak için içerik anahtarını nasıl teslim edildiğini belirtmek etkinleştirin. Daha fazla bilgi için [içerik korumaya genel bakış](content-protection-overview.md).

## <a name="contentkeypolicies-definition"></a>ContentKeyPolicies tanımı

Aşağıdaki tabloda ContentKeyPolicy'nın özelliklerini gösterir ve bunların tanımlarının sağlar.

|Ad|Tür|Açıklama|
|---|---|---|
|id|dize|Kaynak için tam kaynak kimliği.|
|ad|dize|Kaynak adı.|
|Properties.Created |dize|İlke oluşturma tarihi|
|Properties.Description |dize|İlke için bir açıklama.|
|properties.lastModified    |dize|İlkeyi son değiştirilme tarihi|
|Properties.Options |ContentKeyPolicyOption]|Anahtar ilkesi seçenekleri.|
|properties.policyId    |dize|Eski bir ilke kimliği|
|type   |dize|Kaynak türü.|

Tam tanımı için bkz [içerik anahtar ilkeleri](https://docs.microsoft.com/rest/api/media/contentkeypolicies).

## <a name="filtering-ordering-paging"></a>Filtreleme, sıralama, sayfalama

Media Services için ContentKeyPolicies aşağıdaki OData sorgu seçeneklerini destekler: 

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

Aşağıdaki tabloda bu seçeneklerin StreamingPolicy özelliklerine nasıl uygulanabilir gösterilmektedir: 

|Ad|Filtre|Sipariş verme|
|---|---|---|
|id|||
|ad|Eq, ne, ge, le, gt, lt|Artan veya azalan|
|Properties.Created |Eq, ne, ge, le, gt, lt|Artan veya azalan|
|Properties.Description |Eq, ne, ge, le, gt, lt||
|properties.lastModified    |Eq, ne, ge, le, gt, lt|Artan veya azalan|
|Properties.Options |||
|properties.policyId    |Eq, ne||
|type   |||

### <a name="pagination"></a>Sayfalandırma

Sayfalandırma her dört etkin sıralama düzenleri desteklenir. Şu anda, sayfa boyutu 10'dur.

> [!TIP]
> Koleksiyon listeleme ve belirli bir sayfa bağımlı olmadan her zaman sonraki bağlantısını kullanmanız gerekir.

Sorgu yanıtına fazla öğe içeriyorsa, hizmet döndürür bir "\@odata.nextLink" sonraki sonuç sayfasını alınacağı özellik. Bu kullanılabilir sonuç kümesinin tamamı aracılığıyla sayfası. Sayfa boyutunu yapılandıramazsınız. 

StreamingPolicy oluşturduysanız veya çalışırken disk belleği koleksiyonu sildi (Bu değişiklikleri indirilmedi koleksiyonu içinde parçasıysa.) değişiklikler döndürülen sonuçlarda yansıtılır 

Aşağıdaki C# örneği, hesaptaki tüm ContentKeyPolicies aracılığıyla listeleme gösterilmiştir.

```csharp
var firstPage = await MediaServicesArmClient.ContentKeyPolicies.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.ContentKeyPolicies.ListNextAsync(currentPage.NextPageLink);
}
```

Diğer örnekler için bkz [içerik anahtar ilkeleri - liste](https://docs.microsoft.com/rest/api/media/contentkeypolicies/list)

## <a name="next-steps"></a>Sonraki adımlar

[AES-128 dinamik şifreleme ve anahtar teslim hizmetini kullanma](protect-with-aes128.md)

[DRM dinamik şifreleme ve lisans teslimat hizmeti kullanın](protect-with-drm.md)
