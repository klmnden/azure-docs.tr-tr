---
title: Azure Media Services Bulucular akış | Microsoft Docs
description: Bu makalede, akış Bulucuyu nedir ve Azure Media Services tarafından nasıl kullanıldıkları bir açıklama sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/16/2018
ms.author: juliako
ms.openlocfilehash: 1757ca84e7390f1ecd2d6d1e90a085372d3e4c57
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49381290"
---
# <a name="streaming-locators"></a>Akış Bulucuları

İstemcilerinize kodlanmış video veya ses dosyalarını oynatmak için kullanabileceğiniz bir URL sağlamak için oluşturmanız gerekir bir [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) ve akış URL'leri oluşturun. Daha fazla bilgi için [dosya Stream](stream-files-dotnet-quickstart.md).

## <a name="streaminglocator-definition"></a>StreamingLocator tanımı

Aşağıdaki tabloda StreamingLocator'ın özelliklerini gösterir ve bunların tanımlarının sağlar.

|Ad|Tür|Açıklama|
|---|---|---|
|id |dize|Kaynak için tam kaynak kimliği.|
|ad   |dize|Kaynak adı.|
|properties.alternativeMediaId  |dize|Bu akış Bulucusu alternatif ortam kimliği.|
|properties.assetName   |dize|Varlık adı|
|properties.contentKeys |StreamingLocatorContentKey]|Bu akış Bulucu tarafından kullanılan ContentKeys.|
|Properties.Created |dize|Akış Bulucusu oluşturma zamanı.|
|properties.defaultContentKeyPolicyName |dize|Bu akış Bulucu tarafından kullanılan ContentKeyPolicy varsayılan adı.|
|properties.endTime |dize|Akış Bulucu bitiş saati.|
|properties.startTime   |dize|Akış Bulucu başlangıç zamanı.|
|properties.streamingLocatorId  |dize|Akış Bulucu StreamingLocatorId.|
|properties.streamingPolicyName |dize|Bu akış Bulucu tarafından kullanılan akış ilke adı. Akış oluşturduğunuz ilke adı belirtin veya önceden tanımlanmış akış ilkelerden birini kullanabilirsiniz. Önceden tanımlanmış akış ilkeleri kullanılabilir: 'Predefined_DownloadOnly', 'Predefined_ClearStreamingOnly', 'Predefined_DownloadAndClearStreaming', 'Predefined_ClearKey', 'Predefined_MultiDrmCencStreaming' ve ' Predefined_ MultiDrmStreaming'|
|type   |dize|Kaynak türü.|

Tam tanımı için bkz [akış bulucuları](https://docs.microsoft.com/rest/api/media/streaminglocators).

## <a name="filtering-ordering-paging"></a>Filtreleme, sıralama, sayfalama

Media Services, aşağıdaki OData sorgu seçenekleri için akış Bulucuyu destekler: 

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

### <a name="pagination"></a>Sayfalandırma

Sayfalandırma her dört etkin sıralama düzenleri desteklenir. Şu anda, sayfa boyutu 10'dur.

> [!TIP]
> Koleksiyon listeleme ve belirli bir sayfa bağımlı olmadan her zaman sonraki bağlantısını kullanmanız gerekir.

Sorgu yanıtına fazla öğe içeriyorsa, hizmet döndürür bir "\@odata.nextLink" sonraki sonuç sayfasını alınacağı özellik. Bu kullanılabilir sonuç kümesinin tamamı aracılığıyla sayfası. Sayfa boyutunu yapılandıramazsınız. 

StreamingLocators oluşturduysanız veya çalışırken disk belleği koleksiyonu sildi (Bu değişiklikleri indirilmedi koleksiyonu içinde parçasıysa.) değişiklikler döndürülen sonuçlarda yansıtılır 

Aşağıdaki C# örneği, hesaptaki tüm StreamingLocators aracılığıyla listeleme gösterilmiştir.

```csharp
var firstPage = await MediaServicesArmClient.StreamingLocators.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.StreamingLocators.ListNextAsync(currentPage.NextPageLink);
}
```

Diğer örnekler için bkz [akış bulucuları - liste](https://docs.microsoft.com/rest/api/media/streaminglocators/streaminglocators_list)

## <a name="next-steps"></a>Sonraki adımlar

[Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)
