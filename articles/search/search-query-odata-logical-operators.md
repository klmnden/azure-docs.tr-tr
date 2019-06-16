---
title: OData mantıksal işleç başvuru - Azure Search
description: OData mantıksal işleçler ve, veya ve değil, Azure arama sorguları.
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
ms.openlocfilehash: de93765117b4cafe70e5ad277e32ca0a1fa8d90a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079775"
---
# <a name="odata-logical-operators-in-azure-search---and-or-not"></a>Azure Search - OData mantıksal işleçler `and`, `or`, `not`

[OData filtre ifadeleri](query-odata-filter-orderby-syntax.md) Azure Search'te vermesi Boolean ifadeler olan `true` veya `false`. Bir dizi yazarak karmaşık bir filtre yazabilirsiniz [daha basit filtreleri](search-query-odata-comparison-operators.md) gelen mantıksal işleçleri kullanarak oluşturma ve [Boolean Cebir](https://en.wikipedia.org/wiki/Boolean_algebra):

- `and`: Değerlendiren ikili işleç `true` hem kendi sol ve sağ alt ifadeler sonucunu verirse `true`.
- `or`: Değerlendiren ikili işleç `true` ya da kendi sol veya sağ alt ifadeler birini değerlendirilen `true`.
- `not`: Birli işleç olarak değerlendirilen `true` kendi alt ifade değerlendirilirse `false`ve tersi.

Bunlar, birlikte [toplama işleçleri `any` ve `all` ](search-query-odata-collection-operators.md), çok karmaşık arama ölçütleri ifade edebilirsiniz filtreleri oluşturmak olanak sağlar.

## <a name="syntax"></a>Sözdizimi

Aşağıdaki EBNF ([genişletilmiş Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) mantıksal işleçlerini kullanan bir OData ifade dilbilgisi tanımlar.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
logical_expression ::=
    boolean_expression ('and' | 'or') boolean_expression
    | 'not' boolean_expression
```

Bir etkileşimli söz dizim diyagramı görülmektedir de kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Search için OData söz dizimini diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#logical_expression)

> [!NOTE]
> Bkz: [OData ifadesi söz dizimi başvurusu için Azure Search](search-query-odata-syntax-reference.md) tam EBNF için.

Mantıksal ifadeler iki tür vardır: ikili (`and`/`or`), iki alt ifadeler ve birli olduğu (`not`), burada yalnızca bir tane vardır. Alt ifadeler, Boolean ifadeleri herhangi bir türde olabilir:

- Türünde alanlar veya aralık değişkenleri `Edm.Boolean`
- Tür değerleri döndüren işlevleri `Edm.Boolean`, gibi `geo.intersects` veya `search.ismatch`
- [Karşılaştırma ifadeleri](search-query-odata-comparison-operators.md), gibi `rating gt 4`
- [Koleksiyon ifadeleri](search-query-odata-collection-operators.md), gibi `Rooms/any(room: room/Type eq 'Deluxe Room')`
- Boole sabit değerleri `true` veya `false`.
- Diğer mantıksal ifadeleri kullanarak oluşturulan `and`, `or`, ve `not`.

> [!IMPORTANT]
> Bazı durumlarda, burada alt ifade tüm türleri ile kullanılabilir `and` / `or`, özellikle iç lambda ifadeleri. Bkz: [OData toplama işleçleri Azure Search'te](search-query-odata-collection-operators.md#limitations) Ayrıntılar için.

### <a name="logical-operators-and-null"></a>Mantıksal işleçler ve `null`

İşlevler ve karşılaştırma gibi çoğu Boolean ifadeler üretemiyor `null` değerleri ve mantıksal işleçler uygulanamaz `null` doğrudan değişmez değeri (örneğin, `x and null` izin verilmiyor). Ancak, Boolean alanlar olabilir `null`, bu nedenle, nasıl dikkat etmeniz gerekir `and`, `or`, ve `not` işleçleri davranır null saklanacaktır. Bu aşağıdaki tabloda özetlenen burada `b` bir alanı `Edm.Boolean`:

| İfade | Sonuç, `b` olduğu `null` |
| --- | --- |
| `b` | `false` |
| `not b` | `true` |
| `b eq true` | `false` |
| `b eq false` | `false` |
| `b eq null` | `true` |
| `b ne true` | `true` |
| `b ne false` | `true` |
| `b ne null` | `false` |
| `b and true` | `false` |
| `b and false` | `false` |
| `b or true` | `true` |
| `b or false` | `false` |

Bir Boolean alan `b` görünür tek başına bir filtre ifadesinde yazılmış gibi davrandığını `b eq true`, dolayısıyla `b` olan `null`, ifadenin değerlendirdiği `false`. Benzer şekilde, `not b` gibi davranan `not (b eq true)`değerlendirilir, bu nedenle `true`. Bu şekilde `null` alanları aynı şekilde davranır `false`. Bu kullanan diğer ifadelerle birlikte kullanıldığında davranışları ile tutarlıdır `and` ve `or`yukarıdaki tabloda gösterilen şekilde. Buna karşın, doğrudan bir karşılaştırma `false` (`b eq false`) için değerlendirilecek olan `false`. Diğer bir deyişle, `null` eşit değildir `false`rağmen Boolean ifadeleri gibi davranır.

## <a name="examples"></a>Örnekler

Eşleşme belgeleri nerede `rating` alandır 3 ile 5, kapsamlı arasında:

    rating ge 3 and rating le 5

Eşleşme belgeleri nerede tüm öğeleri `ratings` alan 3'den küçük ya da 5 büyüktür:

    ratings/all(r: r lt 3 or r gt 5)

Eşleşme belgeleri nerede `location` alanı içinde belirtilen Çokgen ve belge "Genel" terimi içermiyor.

    geo.intersects(location, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))') and not search.ismatch('public')

Belgeler Vancouver, Kanada, Oteller için eşleşen bir lüks oda tabana sahip olduğu değeri 160'tan oranı:

    Address/City eq 'Vancouver' and Address/Country eq 'Canada' and Rooms/any(room: room/Type eq 'Deluxe Room' and room/BaseRate lt 160)

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Search'te filtreler](search-filters.md)
- [Azure Search için OData ifade dili genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Search için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Search belgeleri &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
