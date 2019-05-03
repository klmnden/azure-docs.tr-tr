---
title: Azure Search (Önizleme) - Azure Search moreLikeThis
description: Azure Search REST API'SİNDE kullanıma sunulan moreLikeThis (Önizleme) özelliği için başlangıç belgeleri.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: d18069335bb20f78a5bcda85eb6fcb2a5abe75f7
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65024664"
---
# <a name="morelikethis-in-azure-search-preview"></a>moreLikeThis Azure Search (Önizleme)

`moreLikeThis=[key]` bir sorgu parametresidir [arama belgeleri API](https://docs.microsoft.com/rest/api/searchservice/search-documents) , bulduğu belgeleri belge anahtarını tarafından belirtilen belge benzer. Ne zaman bir arama isteği yapıldığında ile `moreLikeThis`, belge en iyi şekilde açıklayan verilen belgedeki ayıklanan arama terimlerini bir sorgu oluşturulur. Oluşturulan sorgu daha sonra arama istekte bulunmak için kullanılır. Varsayılan olarak, tüm aranabilir alanları içeriğini, kullanarak belirtilen kısıtlı alanları değerlendirilir `searchFields` parametresi. `moreLikeThis` Parametresi kullanılamaz arama parametresi ile `search=[string]`.

Varsayılan olarak, tüm üst düzey aranabilir alanları içeriği olarak kabul edilir. Bunun yerine belirli alanları belirtmek istiyorsanız, kullanabileceğiniz `searchFields` parametresi. 

> [!NOTE]
> `moreLikeThis` Önizleme aranabilir alt alanlar üzerinde çalışmıyor bir [karmaşık tür](search-howto-complex-data-types.md).

## <a name="examples"></a>Örnekler 

MoreLikeThis sorgu örneği aşağıda verilmiştir. Açıklama alanları tarafından belirtilen kaynak belge alanının en çok benzeyen belgeleri sorgunun bulacağı `moreLikeThis` parametresi.

```
Get /indexes/hotels/docs?moreLikeThis=1002&searchFields=description&api-version=2019-05-06-Preview
```

```
POST /indexes/hotels/docs/search?api-version=2019-05-06-Preview
    {
      "moreLikeThis": "1002",
      "searchFields": "description"
    }
```

## <a name="feature-availability"></a>Özellik kullanılabilirliği

`moreLikeThis` Parametresi yalnızca önizleme REST API'leri kullanılabilir (`api-version=2019-05-06-Preview`).

## <a name="next-steps"></a>Sonraki adımlar

Bu özellikle ilgili denemeler için herhangi bir web testi aracı kullanabilirsiniz.  Bu alıştırma için Postman'ı kullanmanızı öneririz.

> [!div class="nextstepaction"]
> [Postman kullanarak Azure Search REST API'lerini keşfetme](search-fiddler.md)