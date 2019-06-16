---
title: OData karşılaştırma işleci başvuru - Azure Search
description: OData Karşılaştırma işleçleri, eq, ne, gt, lt, ge ve Azure arama sorgularında le.
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
ms.openlocfilehash: b51bf3d77283ae828f47fdb0355d2deb43f071a1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079931"
---
# <a name="odata-comparison-operators-in-azure-search---eq-ne-gt-lt-ge-and-le"></a>Azure Search - OData karşılaştırma işleçlerinden `eq`, `ne`, `gt`, `lt`, `ge`, ve `le`

En temel işleminde bir [OData filtre ifadesinin](query-odata-filter-orderby-syntax.md) Azure Search'te bir alan belirli bir değerle karşılaştırmak sağlamaktır. Olası--karşılaştırma iki tür eşitlik karşılaştırması ve aralığı karşılaştırma. Bir alan sabit bir değerle karşılaştırmak için aşağıdaki işleçleri kullanabilirsiniz:

Eşitlik işleçleri:

- `eq`: Bir alan olup olmadığını sınamak **eşit** sabit değer
- `ne`: Bir alan olup olmadığını sınamak **eşit değil** sabit değer

Aralık işleçleri:

- `gt`: Bir alan olup olmadığını sınamak **büyüktür** sabit değer
- `lt`: Bir alan olup olmadığını sınamak **küçüktür** sabit değer
- `ge`: Bir alan olup olmadığını sınamak **büyüktür veya eşittir** sabit değer
- `le`: Bir alan olup olmadığını sınamak **ya da eşit** sabit değer

Aralık işleçleri ile birlikte kullanabileceğiniz [mantıksal işleçler](search-query-odata-logical-operators.md) bir alan bir değer aralığında olup olmadığını test etmek için. Bkz: [örnekler](#examples) bu makalenin ilerleyen bölümlerinde.

> [!NOTE]
> Tercih ederseniz, sol tarafta, işleç ve sağ taraftaki alan adı, sabit değer koyabilirsiniz. Aralık işleçler için karşılaştırma anlamını tersine çevrildi. Örneğin, sol tarafta, sabit değer ise `gt` sabit değer alanın büyük olup olmadığını test edersiniz. Bir işlevin sonucu gibi Karşılaştırılacak Karşılaştırma işleçleri kullanabilirsiniz `geo.distance`, bir değere sahip. Boole işlevleri gibi `search.ismatch`, karşılaştırma sonucu `true` veya `false` isteğe bağlıdır.

## <a name="syntax"></a>Sözdizimi

Aşağıdaki EBNF ([genişletilmiş Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) Karşılaştırma işleçleri kullanan bir OData ifade dilbilgisi tanımlar.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
comparison_expression ::=
    variable_or_function comparison_operator constant |
    constant comparison_operator variable_or_function

variable_or_function ::= variable | function_call

comparison_operator ::= 'gt' | 'lt' | 'ge' | 'le' | 'eq' | 'ne'
```

Bir etkileşimli söz dizim diyagramı görülmektedir de kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Search için OData söz dizimini diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#comparison_expression)

> [!NOTE]
> Bkz: [OData ifadesi söz dizimi başvurusu için Azure Search](search-query-odata-syntax-reference.md) tam EBNF için.

Karşılaştırma ifadeleri iki biçimi vardır. Aralarındaki tek fark, sabiti sol - veya sağ taraftaki üzerinde işlecinin görünüp görünmediğini ' dir. İşlecinin diğer tarafında bir ifade olmalıdır bir **değişkeni** veya bir işlev çağrısı. Bir değişken bir alan adı veya bir aralık değişkenine durumunda olabilir bir [lambda ifadesi](search-query-odata-collection-operators.md).

## <a name="data-types-for-comparisons"></a>Veri türleri için karşılaştırma

Karşılaştırma işlecinin her iki tarafında veri türleri uyumlu olmalıdır. Örneğin, sol tarafta türünde bir alan ise `Edm.DateTimeOffset`, sağ tarafındaki bir tarih saat sabiti olmalıdır. Sayısal veri türleri daha esnektir. Aşağıdaki tabloda açıklandığı gibi değişkenlerin ve işlevlerin herhangi bir sayısal tür başka türden sayısal, bazı kısıtlamalarla sabitler ile karşılaştırabilirsiniz.

| Değişken veya işlev türü | Sabit değer türü | Sınırlamalar |
| --- | --- | --- |
| `Edm.Double` | `Edm.Double` | Bir karşılaştırmadır konusu [için özel kurallar `NaN`](#special-case-nan) |
| `Edm.Double` | `Edm.Int64` | Sabiti dönüştürülür `Edm.Double`, sonuçta elde edilen büyük büyüklük değerlerinin duyarlık kaybı |
| `Edm.Double` | `Edm.Int32` | yok |
| `Edm.Int64` | `Edm.Double` | Karşılaştırmalar `NaN`, `-INF`, veya `INF` izin verilmez |
| `Edm.Int64` | `Edm.Int64` | yok |
| `Edm.Int64` | `Edm.Int32` | Sabiti dönüştürülür `Edm.Int64` karşılaştırma önce |
| `Edm.Int32` | `Edm.Double` | Karşılaştırmalar `NaN`, `-INF`, veya `INF` izin verilmez |
| `Edm.Int32` | `Edm.Int64` | yok |
| `Edm.Int32` | `Edm.Int32` | yok |

Türünde bir alan karşılaştırma gibi izin verilmeyen karşılaştırmalar için `Edm.Int64` için `NaN`, Azure Search REST API'sine döndüreceği bir "HTTP 400: Hatalı istek"hatası.

> [!IMPORTANT]
> Sayısal tür karşılaştırmaları esnek olmasına karşın, sabit değeri olarak değişken veya işlev için bunu karşılaştırılıyor aynı veri türünde olması karşılaştırmalar filtrelerini yazma önemle öneririz. Bu olduğunda özellikle önemlidir, kayan nokta karıştırma ve duyarlık kaybı örtük dönüştürmelerin nerede olası tamsayı değerleri.

<a name="special-case-nan"></a>

### <a name="special-cases-for-null-and-nan"></a>Özel durumlar için `null` ve `NaN`

Karşılaştırma işleçleri kullanırken, Azure Search'te tüm koleksiyon dışı alanlar büyük olasılıkla olabileceğini unutmayın `null`. Aşağıdaki tabloda her iki tarafında nereye olabilir bir karşılaştırma ifadesi tüm olası sonuçlarını gösterir. `null`:

| İşleç | Yalnızca alan veya değişken olduğunda sonucu `null` | Yalnızca sabiti olduğunda sonucu `null` | Alan veya değişken ve sabiti sonuç `null` |
| --- | --- | --- | --- |
| `gt` | `false` | HTTP 400: Hatalı istek hatası | HTTP 400: Hatalı istek hatası |
| `lt` | `false` | HTTP 400: Hatalı istek hatası | HTTP 400: Hatalı istek hatası |
| `ge` | `false` | HTTP 400: Hatalı istek hatası | HTTP 400: Hatalı istek hatası |
| `le` | `false` | HTTP 400: Hatalı istek hatası | HTTP 400: Hatalı istek hatası |
| `eq` | `false` | `false` | `true` |
| `ne` | `true` | `true` | `false` |

Özet olarak, `null` yalnızca kendisine eşit ve daha az veya diğer değerden büyüktür.

Dizininizi türünde alanlar varsa `Edm.Double` ve karşıya yüklediğiniz `NaN` bu alanların değerlerine için filtreler yazarken hesabı gerekir. Azure arama, işleme için IEEE 754 standart uygulayan `NaN` değerler ve tür değerleri karşılaştırmalar aşağıdaki tabloda gösterildiği gibi belirgin olmayan sonuçlar üretir.

| İşleç | En az bir işlenen olduğunda sonucu `NaN` |
| --- | --- |
| `gt` | `false` |
| `lt` | `false` |
| `ge` | `false` |
| `le` | `false` |
| `eq` | `false` |
| `ne` | `true` |

Özet olarak, `NaN` kendisi de dahil olmak üzere, herhangi bir değerine eşit değil.

### <a name="comparing-geo-spatial-data"></a>Jeo-uzamsal verileri karşılaştırma

Türünde bir alan doğrudan karşılaştırılamaz `Edm.GeographyPoint` sabit bir değer ile kullanabilirsiniz, ancak `geo.distance` işlevi. Bu işlev türünde bir değer döndürür `Edm.Double`, bir sayısal karşılaştırma için sabit Jeo-uzamsal koordinatları mesafe dayanan filtre uygulamak için sabiti. Bkz: [örnekler](#examples) aşağıda.

### <a name="comparing-string-data"></a>Dize verileri karşılaştırma

Dizeleri karşılaştırılabilir filtreleri kullanarak tam eşleşme için `eq` ve `ne` işleçleri. Bu karşılaştırmalar büyük/küçük harfe duyarlıdır.

## <a name="examples"></a>Örnekler

Eşleşme belgeleri nerede `Rating` alandır 3 ile 5, kapsamlı arasında:

    Rating ge 3 and Rating le 5

Eşleşme belgeleri nerede `Location` alandır değerinden 2 kilometre belirli bir enlem ve boylam:

    geo.distance(Location, geography'POINT(-122.031577 47.578581)') lt 2.0

Eşleşme belgeleri nerede `LastRenovationDate` alandır büyüktür veya eşittir 1 Ocak 2015'den itibaren gece yarısı UTC:

    LastRenovationDate ge 2015-01-01T00:00:00.000Z

Eşleşme belgeleri nerede `Details/Sku` alan değil `null`:

    Details/Sku ne null

Belgeler için en az bir oda sahip olduğu türü "Deluxe odası", burada hotels eşleşen dizenin `Rooms/Type` alan filtre tam olarak eşleşen:

    Rooms/any(room: room/Type eq 'Deluxe Room')

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Search'te filtreler](search-filters.md)
- [Azure Search için OData ifade dili genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Search için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Search belgeleri &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
