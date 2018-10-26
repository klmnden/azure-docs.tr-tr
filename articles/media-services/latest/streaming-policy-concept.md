---
title: Azure Media Services ilkeleri akış | Microsoft Docs
description: Bu makalede, akış ilkeleri nelerdir ve Azure Media Services tarafından nasıl kullanıldıkları bir açıklama sağlar.
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
ms.openlocfilehash: c5f441fef95989e5c82586d96fc6c10e00a9627c
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50085504"
---
# <a name="streaming-policies"></a>Akış İlkeleri

Azure Media Services v3 sürümünde akış ilkeleri akış protokolleri ve şifreleme seçeneklerini, StreamingLocators tanımlamanıza olanak tanır. Akış oluşturduğunuz ilke adını belirtebilir veya önceden tanımlanmış akış ilkelerden birini kullanın. Önceden tanımlanmış akış ilkeleri şu anda kullanılabilir: 'Predefined_DownloadOnly', 'Predefined_ClearStreamingOnly', 'Predefined_DownloadAndClearStreaming', 'Predefined_ClearKey', 'Predefined_MultiDrmCencStreaming' ve ' Predefined_ MultiDrmStreaming'.

> [!IMPORTANT]
> Özel [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies)’yi kullanırken Media Service hesabınız için bu tür ilkelerin sınırlı bir kümesini tasarlamanız ve aynı şifreleme seçenekleri ve protokoller gerekli olduğunda StreamingLocators için bunları kullanmanız gerekir. Media Service hesabınızda StreamingPolicy girişlerinin sayısı için bir kota bulunur. Her StreamingLocator için yeni bir StreamingPolicy oluşturmamanız gerekir.

## <a name="streamingpolicy-definition"></a>StreamingPolicy tanımı

Aşağıdaki tabloda StreamingPolicy'nın özelliklerini gösterir ve bunların tanımlarının sağlar.

|Ad|Açıklama|
|---|---|
|id|Kaynak için tam kaynak kimliği.|
|ad|Kaynak adı.|
|properties.commonEncryptionCbcs|CommonEncryptionCbcs yapılandırması|
|properties.commonEncryptionCenc|CommonEncryptionCenc yapılandırması|
|Properties.Created |Akış ilkesi oluşturma zamanı|
|properties.defaultContentKeyPolicyName |Geçerli akışı İlkesi tarafından kullanılan varsayılan ContentKey|
|properties.envelopeEncryption  |EnvelopeEncryption yapılandırması|
|properties.noEncryption|Şifreleme yok yapılandırmaları|
|type|Kaynak türü.|

Tam tanımı için bkz [akış ilkeleri](https://docs.microsoft.com/rest/api/media/streamingpolicies).

## <a name="filtering-ordering-paging"></a>Filtreleme, sıralama, sayfalama

Media Services için akış ilkeleri aşağıdaki OData sorgu seçeneklerini destekler: 

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
|properties.commonEncryptionCbcs|||
|properties.commonEncryptionCenc|||
|Properties.Created |Eq, ne, ge, le, gt, lt|Artan veya azalan|
|properties.defaultContentKeyPolicyName |||
|properties.envelopeEncryption|||
|properties.noEncryption|||
|type|||

### <a name="pagination"></a>Sayfalandırma

Sayfalandırma her dört etkin sıralama düzenleri desteklenir. Şu anda, sayfa boyutu 10'dur.

> [!TIP]
> Koleksiyon listeleme ve belirli bir sayfa bağımlı olmadan her zaman sonraki bağlantısını kullanmanız gerekir.

Sorgu yanıtına fazla öğe içeriyorsa, hizmet döndürür bir "\@odata.nextLink" sonraki sonuç sayfasını alınacağı özellik. Bu kullanılabilir sonuç kümesinin tamamı aracılığıyla sayfası. Sayfa boyutunu yapılandıramazsınız. 

StreamingPolicy oluşturduysanız veya çalışırken disk belleği koleksiyonu sildi (Bu değişiklikleri indirilmedi koleksiyonu içinde parçasıysa.) değişiklikler döndürülen sonuçlarda yansıtılır 

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

## <a name="next-steps"></a>Sonraki adımlar

[Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)
