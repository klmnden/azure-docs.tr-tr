---
title: OData ifadesi söz dizimi başvurusu - Azure Search
description: Resmi dilbilgisi ve dizilim belirtimi Azure arama sorguları OData ifadeler için.
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
ms.openlocfilehash: cccfb749af07d1deeeda6e94de9c2cd5ce5396f3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079671"
---
# <a name="odata-expression-syntax-reference-for-azure-search"></a>Azure Search için OData ifadesi söz dizimi başvurusu

Azure Search kullanan [OData ifadeleri](http://docs.oasis-open.org/odata/odata/v4.01/odata-v4.01-part2-url-conventions.html) API boyunca parametre olarak. OData ifadeler için en yaygın olarak kullanılan `$orderby` ve `$filter` parametreleri. Bu ifadeler, birden çok yan tümceleri, İşlevler ve işleçler içeren karmaşık olabilir. Ancak, daha basit OData ifadeleri yolları birçok Azure Search REST API bölümlerinde kullanılan özellik ister. Örneğin, yol ifadelerini API'sindeki zaman listeleme alt alanlar gibi her yerde, karmaşık alanlarının alt alanlara başvurmak için kullanılan bir [öneri aracı](index-add-suggesters.md), [işlevi Puanlama](index-add-scoring-profiles.md), `$select` parametresi , hatta [fielded Search'te Lucene sorgu](query-lucene-syntax.md).

Bu makalede, bu formları resmi dilbilgisi kullanarak OData ifadelerin açıklanır. Ayrıca bir [etkileşimli çizimin](#syntax-diagram) dilbilgisi görsel olarak keşfedin yardımcı olmak için.

## <a name="formal-grammar"></a>Resmi dilbilgisi

Biz bir EBNF kullanarak Azure Search tarafından desteklenen OData dil kümesini açıklayın ([genişletilmiş Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) Dilbilgisi. Kurallar, "yukarıdan aşağıya", en karmaşık ifadeleri ile başlayan ve daha basit ifadelere bozucu listelenir. Üst kısmında, Azure Search REST API'sine belirli parametrelere karşılık gelir dilbilgisi kurallar şunlardır:

- [`$filter`](search-query-odata-filter.md), tarafından tanımlanan `filter_expression` kuralı.
- [`$orderby`](search-query-odata-orderby.md), tarafından tanımlanan `order_by_expression` kuralı.
- [`$select`](search-query-odata-select.md), tarafından tanımlanan `select_expression` kuralı.
- Alan tarafından tanımlanan yollar `field_path` kuralı. Alan yolları API'si kullanılır. Bir dizinin en üst düzey ya da alanları ya da bir veya daha fazla alt alanlarla başvurabilir [karmaşık alan](search-howto-complex-data-types.md) üst öğelerinden.

EBNF'ye bir gözatılabilir sonra [söz dizim diyagramı görülmektedir](https://en.wikipedia.org/wiki/Syntax_diagram) dilbilgisi ve ilişkileri kurallarını arasında etkileşimli olarak keşfetmek sağlar.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
/* Top-level rules */

filter_expression ::= boolean_expression

order_by_expression ::= order_by_clause(',' order_by_clause)*

select_expression ::= '*' | field_path(',' field_path)*

field_path ::= identifier('/'identifier)*


/* Shared base rules */

identifier ::= [a-zA-Z_][a-zA-Z_0-9]*


/* Rules for $orderby */

order_by_clause ::= (field_path | sortable_function) ('asc' | 'desc')?

sortable_function ::= geo_distance_call | 'search.score()'


/* Rules for $filter */

boolean_expression ::=
    collection_filter_expression
    | logical_expression
    | comparison_expression
    | boolean_literal
    | boolean_function_call
    | '(' boolean_expression ')'
    | variable

/* This can be a range variable in the case of a lambda, or a field path. */
variable ::= identifier | field_path

collection_filter_expression ::=
    field_path'/all(' lambda_expression ')'
    | field_path'/any(' lambda_expression ')'
    | field_path'/any()'

lambda_expression ::= identifier ':' boolean_expression

logical_expression ::=
    boolean_expression ('and' | 'or') boolean_expression
    | 'not' boolean_expression

comparison_expression ::= 
    variable_or_function comparison_operator constant | 
    constant comparison_operator variable_or_function

variable_or_function ::= variable | function_call

comparison_operator ::= 'gt' | 'lt' | 'ge' | 'le' | 'eq' | 'ne'


/* Rules for constants and literals */

constant ::=
    string_literal
    | date_time_offset_literal
    | integer_literal
    | float_literal
    | boolean_literal
    | 'null'

string_literal ::= "'"([^'] | "''")*"'"

date_time_offset_literal ::= date_part'T'time_part time_zone

date_part ::= year'-'month'-'day

time_part ::= hour':'minute(':'second('.'fractional_seconds)?)?

zero_to_fifty_nine ::= [0-5]digit

digit ::= [0-9]

year ::= digit digit digit digit

month ::= '0'[1-9] | '1'[0-2]

day ::= '0'[1-9] | [1-2]digit | '3'[0-1]

hour ::= [0-1]digit | '2'[0-3]

minute ::= zero_to_fifty_nine

second ::= zero_to_fifty_nine

fractional_seconds ::= integer_literal

time_zone ::= 'Z' | sign hour':'minute

sign ::= '+' | '-'

/* In practice integer literals are limited in length to the precision of
the corresponding EDM data type. */
integer_literal ::= digit+

float_literal ::=
    sign? whole_part fractional_part? exponent?
    | 'NaN'
    | '-INF'
    | 'INF'

whole_part ::= integer_literal

fractional_part ::= '.'integer_literal

exponent ::= 'e' sign? integer_literal

boolean_literal ::= 'true' | 'false'


/* Rules for functions */

function_call ::=
    geo_distance_call |
    boolean_function_call

geo_distance_call ::=
    'geo.distance(' variable ',' geo_point ')'
    | 'geo.distance(' geo_point ',' variable ')'

geo_point ::= "geography'POINT(" lon_lat ")'"

lon_lat ::= float_literal ' ' float_literal

boolean_function_call ::=
    geo_intersects_call |
    search_in_call |
    search_is_match_call

geo_intersects_call ::=
    'geo.intersects(' variable ',' geo_polygon ')'

/* You need at least four points to form a polygon, where the first and
last points are the same. */
geo_polygon ::=
    "geography'POLYGON((" lon_lat ',' lon_lat ',' lon_lat ',' lon_lat_list "))'"

lon_lat_list ::= lon_lat(',' lon_lat)*

search_in_call ::=
    'search.in(' variable ',' string_literal(',' string_literal)? ')'

/* Note that it is illegal to call search.ismatch or search.ismatchscoring
from inside a lambda expression. */
search_is_match_call ::=
    'search.ismatch'('scoring')?'(' search_is_match_parameters ')'

search_is_match_parameters ::=
    string_literal(',' string_literal(',' query_type ',' search_mode)?)?

query_type ::= "'full'" | "'simple'"

search_mode ::= "'any'" | "'all'"
```

## <a name="syntax-diagram"></a>Söz dizimi diyagramı

Azure arama tarafından desteklenen OData dil dilbilgisi görsel olarak araştırmak için etkileşimli söz dizim diyagramı görülmektedir deneyin:

> [!div class="nextstepaction"]
> [Azure Search için OData söz dizimini diyagramı](https://azuresearch.github.io/odata-syntax-diagram/)

## <a name="see-also"></a>Ayrıca bkz.  

- [Azure Search'te filtreler](search-filters.md)
- [Search belgeleri &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
- [Lucene sorgu söz dizimi](query-lucene-syntax.md)
- [Azure Search'te Basit Sorgu söz dizimi](query-simple-syntax.md)
