---
title: İçerik anahtarı ilkelerinde medya Hizmetleri - Azure | Microsoft Docs
description: Bu makalede, içerik anahtarı ilkeleri nelerdir ve Azure Media Services tarafından nasıl kullanıldıkları bir açıklama sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 12/20/2018
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: f12632b20d516c81e21a50cfdda7e40d4163afc1
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53742227"
---
# <a name="content-key-policies"></a>İçerik Anahtar İlkeleri

Azure Media Services, depolama, işleme ve teslim üzerinden bilgisayarınıza çıkışında medyanızdaki güvenliğini sağlamak için kullanabilirsiniz. Media Services sayesinde, Gelişmiş Şifreleme Standardı (AES-128) veya üç ana dijital hak yönetimi (DRM) sistemlerinden ile dinamik olarak şifrelenmiş canlı ve isteğe bağlı içerik teslim edebilirsiniz: Microsoft PlayReady, Google Widevine ve FairPlay Apple. Media Services de AES anahtarları ve DRM sunmaya yönelik bir hizmet sağlar (PlayReady, Widevine ve FairPlay) lisansları yetkili istemcilere.

Azure Media Services v3 içinde bir [içerik anahtarı ilke](https://docs.microsoft.com/rest/api/media/contentkeypolicies) istemcileri Media Services anahtar teslim bileşen aracılığıyla sonlandırmak için içerik anahtarını nasıl teslim edildiğini belirtmenize olanak sağlar. Daha fazla bilgi için [içerik korumaya genel bakış](content-protection-overview.md).

Aynı ContentKeyPolicy tüm varlıklarınızı yeniden olduğunu önerilir. Anahtar döndürme yapmak istiyorsanız sonra ya da yeni bir ContentKeyPolicyOption belirteç kısıtlamasına sahip mevcut ContentKeyPolicy yeni anahtarlarla ekleyebileceğiniz şekilde ContentKeyPolicies güncelleştirilebilir. Ya da birincil doğrulama anahtarı ve diğer doğrulama anahtarları mevcut ilke ve seçenek listesini güncelleştirebilirsiniz. Bu, güncelleştirme ve güncelleştirilmiş ilke çekme anahtar teslim önbellekleri 15 dakika kadar sürebilir.

## <a name="contentkeypolicy-definition"></a>ContentKeyPolicy tanımı

Aşağıdaki tabloda ContentKeyPolicy'nın özelliklerini gösterir ve bunların tanımlarının sağlar.

|Ad|Açıklama|
|---|---|
|id|Kaynak için tam kaynak kimliği.|
|ad|Kaynak adı.|
|Properties.Created |İlke oluşturma tarihi|
|Properties.Description |İlke için bir açıklama.|
|properties.lastModified|İlkeyi son değiştirilme tarihi|
|Properties.Options |Anahtar ilkesi seçenekleri.|
|properties.policyId|Eski bir ilke kimliği|
|type|Kaynak türü.|

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

Aşağıdaki tabloda bu seçeneklerin ContentKeyPolicies özelliklerine nasıl uygulanabilir gösterilmektedir: 

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

### <a name="pagination"></a>Sayfalandırma

Sayfalandırma her dört etkin sıralama düzenleri desteklenir. Şu anda, sayfa boyutu 10'dur.

> [!TIP]
> Koleksiyon listeleme ve belirli bir sayfa bağımlı olmadan her zaman sonraki bağlantısını kullanmanız gerekir.

Sorgu yanıtına fazla öğe içeriyorsa, hizmet döndürür bir "\@odata.nextLink" sonraki sonuç sayfasını alınacağı özellik. Bu kullanılabilir sonuç kümesinin tamamı aracılığıyla sayfası. Sayfa boyutunu yapılandıramazsınız. 

ContentKeyPolicies oluşturduysanız veya çalışırken disk belleği koleksiyonu sildi (Bu değişiklikleri indirilmedi koleksiyonu içinde parçasıysa.) değişiklikler döndürülen sonuçlarda yansıtılır 

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
