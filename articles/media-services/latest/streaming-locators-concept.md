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
ms.date: 12/20/2018
ms.author: juliako
ms.openlocfilehash: 658843fd5acbe0d4e29947e99c00edf4909fe9f4
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53742755"
---
# <a name="streaming-locators"></a>Akış Bulucuları

Kayıttan yürütme için kullanabileceğiniz bir URL ile istemcilerinize kodlanmış video veya ses dosyası sağlamanız gerekir, oluşturmanız gerekir. bir [akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators) ve akış URL'leri oluşturun. Daha fazla bilgi için [dosya Stream](stream-files-dotnet-quickstart.md).

## <a name="streaminglocator-definition"></a>StreamingLocator tanımı

Aşağıdaki tabloda StreamingLocator'ın özelliklerini gösterir ve bunların tanımlarının sağlar.

|Ad|Açıklama|
|---|---|
|id |Kaynak için tam kaynak kimliği.|
|ad|Kaynak adı.|
|properties.alternativeMediaId|Bu akış Bulucusu alternatif ortam kimliği.|
|properties.assetName|Varlık adı|
|properties.contentKeys|Bu akış Bulucu tarafından kullanılan ContentKeys.|
|Properties.Created|Akış Bulucusu oluşturma zamanı.|
|properties.defaultContentKeyPolicyName|Bu akış Bulucu tarafından kullanılan ContentKeyPolicy varsayılan adı.|
|properties.endTime|Akış Bulucu bitiş saati.|
|properties.startTime|Akış Bulucu başlangıç zamanı.|
|properties.streamingLocatorId|Akış Bulucu StreamingLocatorId.|
|properties.streamingPolicyName |Bu akış Bulucu tarafından kullanılan akış ilke adı. Akış oluşturduğunuz ilke adı belirtin veya önceden tanımlanmış akış ilkelerden birini kullanabilirsiniz. Önceden tanımlanmış akış kullanılabilir ilkeleri şunlardır: 'Predefined_DownloadOnly', 'Predefined_ClearStreamingOnly', 'Predefined_DownloadAndClearStreaming', 'Predefined_ClearKey', 'Predefined_MultiDrmCencStreaming' ve 'Predefined_MultiDrmStreaming'|
|type|Kaynak türü.|

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

Diğer örnekler için bkz [akış bulucuları - liste](https://docs.microsoft.com/rest/api/media/streaminglocators/list)

## <a name="next-steps"></a>Sonraki adımlar

[Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)
