---
title: OData search.in işlevi başvuru - Azure Search
description: Azure arama sorgularında OData search.in işlevi.
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
ms.openlocfilehash: f72a59aac448796cf15220e15a3c8a4f12803bb5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079736"
---
# <a name="odata-searchin-function-in-azure-search"></a>OData `search.in` Azure Search işlevi

Sık karşılaşılan bir senaryodur [OData filtre ifadeleri](query-odata-filter-orderby-syntax.md) her belge tek bir alanda birçok değerlerden birine eşit olup olmadığını denetlemek yollardan biridir. Örneğin, bazı uygulamaları nasıl uygulamak budur [güvenlik kırpma](search-security-trimming-for-azure-search.md) --asıl kimlikleri sorgu verme kullanıcıyı temsil eden bir listesiyle bir veya daha fazla asıl kimlikleri içeren bir alanda denetleyerek. Bu gibi bir sorguda kullanmak için yazma için tek yönlü [ `eq` ](search-query-odata-comparison-operators.md) ve [ `or` ](search-query-odata-logical-operators.md) işleçleri:

    group_ids/any(g: g eq '123' or g eq '456' or g eq '789')

Ancak, kullanarak bunu yazmak için bir kısa bir yol yoktur `search.in` işlevi:

    group_ids/any(g: search.in(g, '123, 456, 789'))

> [!IMPORTANT]
> Daha kısa ve kolay okunur, kullanarak yanı sıra `search.in` de sağlar [performans avantajlarının](#bkmk_performance) ve belirli önler [boyut sınırlamaları filtre](search-query-odata-filter.md#bkmk_limits) yüzlerce veya binlerce değerleri filtreye eklenecek. Bu nedenle, kullanarak öneririz `search.in` eşitlik ifadeleridir, daha karmaşık bir ayrım yerine.

> [!NOTE]
> OData standardı 4.01 sürümünü kısa bir süre önce tanıtılan [ `in` işleci](http://docs.oasis-open.org/odata/odata/v4.01/cs01/part2-url-conventions/odata-v4.01-cs01-part2-url-conventions.html#_Toc505773230), benzer davranışlara sahip `search.in` Azure Search'te işlevi. Kullanmanız gerekir ancak, Azure Search bu işleci desteklemez `search.in` işlevini.

## <a name="syntax"></a>Sözdizimi

Aşağıdaki EBNF ([genişletilmiş Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) Dilbilgisi tanımlar `search.in` işlevi:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
search_in_call ::=
    'search.in(' variable ',' string_literal(',' string_literal)? ')'
```

Bir etkileşimli söz dizim diyagramı görülmektedir de kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Search için OData söz dizimini diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#search_in_call)

> [!NOTE]
> Bkz: [OData ifadesi söz dizimi başvurusu için Azure Search](search-query-odata-syntax-reference.md) tam EBNF için.

`search.in` Verilen değerlerin listesini birine eşit aralık değişkeni veya işlevi verilen dize alanı olup olmadığını test eder. Değişkeni ve listedeki her değer arasındaki aynı şekilde olarak büyük küçük harfe duyarlı bir biçimde belirlenir `eq` işleci. Bu nedenle bir ifade ister `search.in(myfield, 'a, b, c')` eşdeğerdir `myfield eq 'a' or myfield eq 'b' or myfield eq 'c'`dışında `search.in` çok daha iyi performans verir.

İki aşırı yükleme `search.in` işlevi:

- `search.in(variable, valueList)`
- `search.in(variable, valueList, delimiters)`

Parametreleri aşağıdaki tabloda tanımlanmıştır:

| Parametre adı | Tür | Açıklama |
| --- | --- | --- |
| `variable` | `Edm.String` | Bir dize alan başvurusu (veya bir aralık değişkeni bir dize koleksiyonu alanı durumda üzerinden burada `search.in` içinde kullanılan bir `any` veya `all` ifadesi). |
| `valueList` | `Edm.String` | Eşleştirilecek değerleri ayrılmış listesini içeren bir dize `variable` parametresi. Varsa `delimiters` parametresi belirtilmezse, boşluk ve virgül varsayılan sınırlayıcı olarak kullanılır. |
| `delimiters` | `Edm.String` | Burada her karakter olarak kabul ayırıcı olarak ayrıştırılırken bir dize `valueList` parametresi. Bu parametrenin varsayılan değeri `' ,'` tüm değerleri boşluk ve/veya aralarındaki virgüller ile ayrılması anlamına gelir. Değerlerinizin bu karakterleri içerdiğinden boşluk ve virgül dışındaki ayırıcıları kullanmanız gerekiyorsa, gibi başka bir sınırlayıcı belirtin `'|'` bu parametrede. |

<a name="bkmk_performance"></a>

### <a name="performance-of-searchin"></a>Performansı `search.in`

Kullanırsanız `search.in`, ikinci parametre, yüzlerce veya binlerce değerlerinin listesini içerdiğinde saniyenin altındaki yanıt süresi bekleyebilirsiniz. Öğe sayısı için geçirebilirsiniz açık bir sınır yoktur `search.in`, ancak yine de en büyük istek boyutuyla sınırlıdır. Ancak, değerler sayısı arttıkça gecikme çıkarılır.

## <a name="examples"></a>Örnekler

Tüm hotels adlı 'Deniz görünümü motel' veya 'Bütçe otel' eşittir bulun. İfadeler, varsayılan sınırlayıcı olduğu alanları içerir. Alternatif bir sınırlayıcı tek tırnak işaretleri dize üçüncü parametresi olarak belirtebilirsiniz:  

    search.in(HotelName, 'Sea View motel,Budget hotel', ',')

Tüm hotels adlı eşittir 'Deniz görünümü motel' veya 'ile ayrılmış bütçe otel' Bul ' |'):

    search.in(HotelName, 'Sea View motel|Budget hotel', '|')

Etiket 'wifi' veya 'düşürürseniz sigorta atar' odaları ile tüm hotels bulun:

    Rooms/any(room: room/Tags/any(tag: search.in(tag, 'wifi, tub')))

Bir eşleşme ifadeleri 'ısıtılan towel raflar' veya 'dahil hairdryer' gibi bir koleksiyon içinde etiketleri bulun.

    Rooms/any(room: room/Tags/any(tag: search.in(tag, 'heated towel racks,hairdryer included', ','))

Etiket 'motel' veya 'kabini' olmadan tüm hotels bulun:

    Tags/all(tag: not search.in(tag, 'motel, cabin'))

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Search'te filtreler](search-filters.md)
- [Azure Search için OData ifade dili genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Search için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Search belgeleri &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
