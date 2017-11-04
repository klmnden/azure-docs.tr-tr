---
title: "Azure Search'te (Önizleme) moreLikeThis | Microsoft Docs"
description: "Azure Search REST API'sini açığa moreLikeThis (Önizleme) özelliği, ön belgeleri."
services: search
documentationCenter: na
authors: mhko
manager: jlembicz
editor: na
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: nateko
ms.openlocfilehash: 20f6a1166d47799f15275cb5b6a9ea4934ae57e2
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="morelikethis-in-azure-search-preview"></a>moreLikeThis Azure Search'te (Önizleme)

`moreLikeThis=[key]`bir sorgu parametresidir [arama API](https://docs.microsoft.com/rest/api/searchservice/search-documents). Belirterek `moreLikeThis` bir arama sorgusu parametresi, belge anahtarı tarafından belirtilen belge benzer belgeleri bulabilirsiniz. Ne zaman bir arama isteği yapıldığında ile `moreLikeThis`, bir sorgu belge en iyi şekilde açıklayan verilen belgedeki ayıklanan arama terimleri ile oluşturulur. Oluşturulan sorgu daha sonra arama isteği yapmak için kullanılır. Varsayılan olarak, tüm içeriğini `searchable` sürece alanlar olarak kabul `searchFields` parametresi alanları kısıtlamak için kullanılır. `moreLikeThis` Parametresi arama parametresiyle kullanılamaz `search=[string]`.

## <a name="examples"></a>Örnekler 

Aşağıda, bir moreLikeThis sorgu örneğidir. Sorgu tanımı alanları tarafından belirtilen kaynak belge alan en çok benzeyen belgeleri bulur `moreLikeThis` parametresi.

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

MoreLikeThis özelliği şu anda önizlemede değil ve yalnızca önizleme API-sürümlerde, desteklenen `2015-02-28-Preview` ve `2016-09-01-Preview`. İstekte API sürümü belirtildiğinden, genel olarak kullanılabilir (GA) birleştirmek ve aynı uygulamada API'leri önizlemesini mümkündür. Üretim uygulamalarında kullanma önermiyoruz şekilde ancak API SLA ve Özellikler altında olmayan Önizleme, değişebilir.