---
title: Azure Search (Önizleme) - Azure Search moreLikeThis
description: Azure Search REST API'SİNDE kullanıma sunulan moreLikeThis (Önizleme) özelliği için başlangıç belgeleri.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 10/27/2016
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: d55a6d883e0dcd5ad4b1c1584b76bae06e6c742a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61283382"
---
# <a name="morelikethis-in-azure-search-preview"></a>moreLikeThis Azure Search (Önizleme)

`moreLikeThis=[key]` bir sorgu parametresidir [arama API'si](https://docs.microsoft.com/rest/api/searchservice/search-documents). Belirterek `moreLikeThis` parametresinde bir arama sorgusu belge anahtarı tarafından belirtilen belge benzer belgeler bulabilirsiniz. Ne zaman bir arama isteği yapıldığında ile `moreLikeThis`, belge en iyi şekilde açıklayan verilen belgedeki ayıklanan arama terimlerini bir sorgu oluşturulur. Oluşturulan sorgu daha sonra arama istekte bulunmak için kullanılır. Varsayılan olarak, tüm içeriğini `searchable` sürece alanlar olarak kabul `searchFields` parametresi alanları kısıtlamak için kullanılır. `moreLikeThis` Parametresi kullanılamaz arama parametresi ile `search=[string]`.

## <a name="examples"></a>Örnekler 

MoreLikeThis sorgu örneği aşağıda verilmiştir. Açıklama alanları tarafından belirtilen kaynak belge alanının en çok benzeyen belgeleri sorgunun bulacağı `moreLikeThis` parametresi.

```
Get /indexes/hotels/docs?moreLikeThis=1002&searchFields=description&api-version=2016-09-01-Preview
```

```
POST /indexes/hotels/docs/search?api-version=2016-09-01-Preview
    {
      "moreLikeThis": "1002",
      "searchFields": "description"
    }
```

## <a name="feature-availability"></a>Özellik kullanılabilirliği

MoreLikeThis özelliği şu anda Önizleme aşamasındadır ve önizleme API-sürümlerinde, yalnızca desteklenen `2015-02-28-Preview` ve `2016-09-01-Preview`. İsteğinde belirtilen API sürümü, genel kullanıma (GA) birleştirmek ve aynı uygulamada API'leri Önizleme mümkün olmasıdır. Bunları üretim uygulamalarında kullanılması önerilmez için ancak Önizleme API'leri SLA ve Özellikler altında değildir, değişebilir.
