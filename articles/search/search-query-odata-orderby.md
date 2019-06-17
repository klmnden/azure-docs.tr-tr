---
title: OData sırası tarafından referans - Azure Search
description: Azure arama sorguları sözdiziminde sırası tarafından OData dil referansı.
ms.date: 06/13/2019
services: search
ms.service: search
ms.topic: conceptual
author: Brjohnstmsft
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
ms.openlocfilehash: 1ced35dc73e6d596fbeda32590ab0b69df396c5c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079762"
---
# <a name="odata-orderby-syntax-in-azure-search"></a>Azure Search'te $orderby OData söz dizimi

 Kullanabileceğiniz [OData **$orderby** parametre](query-odata-filter-orderby-syntax.md) Azure Search'te arama sonuçları için bir özel sıralama düzeni uygulamak için. Bu makalede söz dizimi **$orderby** ayrıntılı. Nasıl kullanılacağı hakkında daha fazla genel bilgi için **$orderby** arama sonuçları sunma görürsünüz [sonuçları Azure Search'te arama ile çalışmaya nasıl](search-pagination-page-layout.md).

## <a name="syntax"></a>Sözdizimi

**$Orderby** parametreyi kabul eden en fazla 32 virgülle ayrılmış bir listesini **order by yan tümcesi**. Order by yan tümcesi söz dizimi aşağıdaki EBNF tarafından açıklanan ([genişletilmiş Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)):

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
order_by_clause ::= (field_path | sortable_function) ('asc' | 'desc')?

sortable_function ::= geo_distance_call | 'search.score()'
```

Bir etkileşimli söz dizim diyagramı görülmektedir de kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Search için OData söz dizimini diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#order_by_clause)

> [!NOTE]
> Bkz: [OData ifadesi söz dizimi başvurusu için Azure Search](search-query-odata-syntax-reference.md) tam EBNF için.

Sıralama ölçütü, isteğe bağlı olarak bir sıralama yönü tarafından izlenen her bir yan tümceye sahip (`asc` artan için veya `desc` azalan için). Yön belirtmezseniz varsayılan artan düzendedir. Sıralama ölçütü yolunu olabilir bir `sortable` alan veya öğelerine yönelik çağrıdan [ `geo.distance` ](search-query-odata-geo-spatial-functions.md) veya [ `search.score` ](search-query-odata-search-score-function.md) işlevleri.

Birden çok belge aynı sıralama ölçütü varsa ve `search.score` işlevi kullanılmaz (örneğin, bir sayısal göre sıralarsanız `Rating` tüm alan ve üç belgeler sahip bir derecelendirme 4), TIES bozulmuş belge puana göre azalan sırada. Belge puanlar (örneğin, istekte belirtilen tam metin araması sorgu olduğunda) aynı olduğunda, göreli sıralamasını bağlı belgelerin belirsiz ise.

Birden çok sıralama ölçütleri belirtebilirsiniz. Son sıralama ifadeleri sırasını belirler. Örneğin, derecelendirmesine göre ve ardından, puana göre azalan düzende sıralamak için söz dizimi olacaktır `$orderby=search.score() desc,Rating desc`.

Sözdizimi `geo.distance` içinde **$orderby** olduğundan aynı olan **$filter**. Kullanırken `geo.distance` içinde **$orderby**, uygulandığı alanın türü olmalıdır `Edm.GeographyPoint` ve ayrıca olmalıdır `sortable`.

Sözdizimi `search.score` içinde **$orderby** olduğu `search.score()`. İşlev `search.score` hiçbir parametre almaz.

## <a name="examples"></a>Örnekler

Taban fiyat göre artan sıralama hotels:

    $orderby=BaseRate asc

Otel, derecelendirme, daha sonra temel ücrete göre artan azalan sıralama (artan varsayılan olduğunu unutmayın):

    $orderby=Rating desc,BaseRate

Otel, derecelendirme, daha sonra uzaklık tarafından verilen koordinatlarından artan azalan düzende sıralanır:

    $orderby=Rating desc,geo.distance(Location, geography'POINT(-122.131577 47.678581)') asc

Hotels search.score ve Derecelendirmeye göre azalan düzende ve artan düzende uzaklık tarafından verilen koordinatlarından sıralayın. Aynı ilgi puanları ile iki hotels ve derecelendirmeleri arasında en yakındakine ilk listelenir:

    $orderby=search.score() desc,Rating desc,geo.distance(Location, geography'POINT(-122.131577 47.678581)') asc

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Search'te sonuçlarını arama ile çalışma](search-pagination-page-layout.md)
- [Azure Search için OData ifade dili genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Search için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Search belgeleri &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
