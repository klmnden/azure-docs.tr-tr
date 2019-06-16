---
title: OData search.score işlevi başvuru - Azure Search
description: Azure arama sorgularında OData search.score işlevi.
ms.date: 06/13/2019
services: search
ms.service: search
ms.topic: conceptual
author: brjohnstmsft
ms.author: brjohnst
ms.manager: cgronlun
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: dc444216c4677b9970b867e92aa5ae259a197220
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079697"
---
# <a name="odata-searchscore-function-in-azure-search"></a>OData `search.score` Azure Search işlevi

Azure arama ' bir sorgu gönderdiğinizde [ **$orderby** parametre](search-query-odata-orderby.md), döndürülmesini sonuçları azalan sırada ilgi puanı göre sıralanır. Hatta kullandığınızda **$orderby**, ilgi puanı TIES ayırmak için varsayılan olarak kullanılacak. Ancak, bazen ilgi puanı giderici ilk sıralama ölçütleri ve bazı diğer ölçütlere kullanmak kullanışlıdır. `search.score` İşlevi, bunu yapmanızı sağlar.

## <a name="syntax"></a>Sözdizimi

Sözdizimi `search.score` içinde **$orderby** olduğu `search.score()`. İşlev `search.score` herhangi bir parametre almaz. İle kullanılabilir `asc` veya `desc` gibi diğer yan tümcesinde sıralama düzeni belirticisi **$orderby** parametresi. Ayrıca, sıralama ölçütü listesinde her yerde görünebilir.

## <a name="example"></a>Örnek

Hotels göre azalan düzende sıralamak `search.score` ve `rating`ve böylece aynı dereceye sahip iki hotels en yakındakine listede ilk sıradaysa ardından artan düzende uzaklık tarafından verilen koordinatlarından:

    search.score() desc,rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Search için OData ifade dili genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Search için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Search belgeleri &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
