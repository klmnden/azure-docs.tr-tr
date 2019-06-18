---
title: OData dil genel bakış - Azure Search
description: OData dil genel bakış için filtreleri seçin ve Azure arama sorguları için sipariş tarafından.
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
ms.openlocfilehash: 166c23088fe0388199ca51efde05153bb5697e38
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67063694"
---
# <a name="odata-language-overview-for-filter-orderby-and-select-in-azure-search"></a>OData dil genel bakış için `$filter`, `$orderby`, ve `$select` Azure Search'te

Azure Search'ü destekleyen bir alt kümesi için OData ifadesi söz dizimi **$filter**, **$orderby**, ve **$select** ifadeler. Filtre ifadeleri ayrıştırma, belirli alanlarında arama sınırlamak veya dizin taramaları sırasında kullanılan eşleştirme ölçütü ekleme sorgusu sırasında değerlendirilir. Order by ifadeleri bir sonuç döndürülür belgelere sıralanacak kümesi üzerinde bir sonradan işleme adımı uygulanır. Seç ifadeleri sonuç kümesinde hangi belge alanların ekleneceğini belirler. Bu ifadelerin söz dizimi kodundan [basit](query-simple-syntax.md) veya [tam](query-lucene-syntax.md) sorgu kullanılan söz dizimi **arama** parametresi sözdizimi, bazı çakışma olsa alanlara başvuran.

Bu makalede, filtreler, order by ve select deyimleri içinde kullanılan OData ifade dili genel bir bakış sağlar. Dil "aşağıdan yukarıya", en temel öğeleri ile başlayan ve bunlar üzerinde oluşturmaya sunulur. Üst düzey sözdizimi her parametre için ayrı bir makalede açıklanmıştır:

- [$filter sözdizimi](search-query-odata-filter.md)
- [$orderby söz dizimi](search-query-odata-orderby.md)
- [$select söz dizimi](search-query-odata-select.md)

Ortak öğeler için son derece karmaşık ifadeler aralığı OData basit, ancak tüm paylaşın. Azure Search'te bir OData ifadenin en temel bölümleri şunlardır:

- **Alan yolları**, dizininizin belirli alanlarına bakın.
- **Sabitler**, belirli bir veri türünün değişmez değerler olduğu.

> [!NOTE]
> Azure Search terminolojisinde farklıdır [OData standardı](https://www.odata.org/documentation/) birkaç yolla. Dediğimiz bir **alan** Azure Search'te adlı bir **özelliği** odata'da ve benzer şekilde **alan yolu** karşı **özellik yolu**. Bir **dizin** içeren **belgeleri** Azure Search'te daha genel odata'da başvurulan bir **varlık kümesi** içeren **varlıkları**. Azure arama terimleri, bu başvuru kullanılır.

## <a name="field-paths"></a>Alan yolları

Aşağıdaki EBNF ([genişletilmiş Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) alan yolları dilbilgisi tanımlar.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
field_path ::= identifier('/'identifier)*

identifier ::= [a-zA-Z_][a-zA-Z_0-9]*
```

Bir etkileşimli söz dizim diyagramı görülmektedir de kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Search için OData söz dizimini diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#field_path)

> [!NOTE]
> Bkz: [OData ifadesi söz dizimi başvurusu için Azure Search](search-query-odata-syntax-reference.md) tam EBNF için.

Bir alan yolu bir veya daha fazla oluşur **tanımlayıcıları** eğik çizgiyle ayrılmış. Her bir tanımlayıcının bir ASCII harf veya alt çizgi ile başlamalı ve yalnızca ASCII harf, rakam veya alt çizgi içeren bir karakter dizisi ' dir. Büyük veya küçük harf olabilir.

Bir tanımlayıcı ya da bir alanın veya için adına başvurabilir bir **aralık değişkeni** bağlamında bir [koleksiyon ifadesi](search-query-odata-collection-operators.md) (`any` veya `all`) bir filtrede. Geçerli öğe koleksiyonunun temsil eden bir döngü değişkeni gibi bir aralık değişkendir. Karmaşık koleksiyonlar için değişkenin alt alanlara başvurmak için alan yollarını kullanabilirsiniz neden olan bir nesne, bu değişkeni temsil eder. Bu, çok sayıda programlama dilinde nokta gösterimi için benzerdir.

Aşağıdaki tabloda yer alan yollarının örnekler gösterilmektedir:

| Alan yolu | Açıklama |
| --- | --- |
| `HotelName` | Dizinin en üst düzey bir alana başvuruyor |
| `Address/City` | Başvurduğu `City` alt dizindeki; karmaşık bir alanın alan `Address` türünde `Edm.ComplexType` Bu örnekte |
| `Rooms/Type` | Başvurduğu `Type` ; dizini bir karmaşık koleksiyonu alanında alt alanı `Rooms` türünde `Collection(Edm.ComplexType)` Bu örnekte |
| `Stores/Address/Country` | Başvurduğu `Country` alt alanı `Address` ; dizini bir karmaşık koleksiyonu alanında alt alanı `Stores` türünde `Collection(Edm.ComplexType)` ve `Address` türünde `Edm.ComplexType` Bu örnekte |
| `room/Type` | Başvurduğu `Type` alt alanı `room` örneğin filtre ifadesinde aralık değişkeni `Rooms/any(room: room/Type eq 'deluxe')` |
| `store/Address/Country` | Başvurduğu `Country` alt alanı `Address` alt alanı `store` örneğin filtre ifadesinde aralık değişkeni `Stores/any(store: store/Address/Country eq 'Canada')` |

Bir alan yolu anlamını bağlama bağlı olarak farklılık gösterir. Filtreleri, bir alan yolu değerine başvuran bir *tek örnek* geçerli belgedeki bir alanın. Diğer bağlamlarda gibi **$orderby**, **$select**, veya [fielded Search'te Lucene tamamını](query-lucene-syntax.md#bkmk_fields), alan için bir alan yolu gösterir. Bu fark, alan yolları filtreleri nasıl kullanacağınız için bazı sonuçları vardır.

Alan yolu göz önünde bulundurun `Address/City`. Bir filtre, bu geçerli belgede, "San Francisco" gibi tek bir şehri ifade eder. Buna karşılık, `Rooms/Type` başvurduğu `Type` ("standart", "ikinci odası vb. için deluxe" ilk odası için gibi) birçok odaları alt alan. Bu yana `Rooms/Type` anlamına gelmez bir *tek örnek* alt alan `Type`, doğrudan bir filtre kullanılamaz. Bunun yerine, yer türüne filtrelemek için kullanacağınız bir [lambda ifadesi](search-query-odata-collection-operators.md) böyle bir aralık değişkeni ile:

    Rooms/any(room: room/Type eq 'deluxe')

Bu örnekte, aralık değişkeni `room` görünür `room/Type` alan yolu. Bu şekilde `room/Type` geçerli belge geçerli odada türünü ifade eder. Tek bir örneğini budur `Type` doğrudan filtrede kullanılabilmesi için alt alan.

### <a name="using-field-paths"></a>Alan yolları kullanma

Alan yolları birçok parametrelerinde kullanılan [Azure Search API](https://docs.microsoft.com/rest/api/searchservice/). Aşağıdaki tabloda, burada kullanılabilirler her yerde yanı sıra, bunların kullanımını herhangi bir kısıtlama listelenmektedir:

| API | Parametre adı | Kısıtlamalar |
| --- | --- | --- |
| [Oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index) veya [güncelleştirme](https://docs.microsoft.com/rest/api/searchservice/update-index) dizini | `suggesters/sourceFields` | None |
| [Oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index) veya [güncelleştirme](https://docs.microsoft.com/rest/api/searchservice/update-index) dizini | `scoringProfiles/text/weights` | Yalnızca başvurabilir **aranabilir** alanları |
| [Oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index) veya [güncelleştirme](https://docs.microsoft.com/rest/api/searchservice/update-index) dizini | `scoringProfiles/functions/fieldName` | Yalnızca başvurabilir **filtrelenebilir** alanları |
| [Search](https://docs.microsoft.com/rest/api/searchservice/search-documents) | `search` zaman `queryType` olduğu `full` | Yalnızca başvurabilir **aranabilir** alanları |
| [Search](https://docs.microsoft.com/rest/api/searchservice/search-documents) | `facet` | Yalnızca başvurabilir **modellenebilir** alanları |
| [Search](https://docs.microsoft.com/rest/api/searchservice/search-documents) | `highlight` | Yalnızca başvurabilir **aranabilir** alanları |
| [Search](https://docs.microsoft.com/rest/api/searchservice/search-documents) | `searchFields` | Yalnızca başvurabilir **aranabilir** alanları |
| [Öner](https://docs.microsoft.com/rest/api/searchservice/suggestions) ve [otomatik tamamlama](https://docs.microsoft.com/rest/api/searchservice/autocomplete) | `searchFields` | Yalnızca bir parçası olan alanlara başvurabilir bir [öneri aracı](index-add-suggesters.md) |
| [Arama](https://docs.microsoft.com/rest/api/searchservice/search-documents), [Öner](https://docs.microsoft.com/rest/api/searchservice/suggestions), ve [otomatik tamamlama](https://docs.microsoft.com/rest/api/searchservice/autocomplete) | `$filter` | Yalnızca başvurabilir **filtrelenebilir** alanları |
| [Arama](https://docs.microsoft.com/rest/api/searchservice/search-documents) ve [önerin](https://docs.microsoft.com/rest/api/searchservice/suggestions) | `$orderby` | Yalnızca başvurabilir **sıralanabilir** alanları |
| [Arama](https://docs.microsoft.com/rest/api/searchservice/search-documents), [Öner](https://docs.microsoft.com/rest/api/searchservice/suggestions), ve [arama](https://docs.microsoft.com/rest/api/searchservice/lookup-document) | `$select` | Yalnızca başvurabilir **alınabilir** alanları |

## <a name="constants"></a>Sabitleri

OData sabitlerle değişmez değerleri olan bir verilen [varlık veri modeli](https://docs.microsoft.com/dotnet/framework/data/adonet/entity-data-model) (EDM) türü. Bkz: [desteklenen veri türleri](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) Azure Search'te desteklenen türleri listesi. Sabit koleksiyon türleri desteklenmez.

Aşağıdaki tabloda, her bir Azure Search tarafından desteklenen veri türleri için sabitleri örneklerini gösterir:

| Veri türü | Örnek sabitleri |
| --- | --- |
| `Edm.Boolean` | `true`, `false` |
| `Edm.DateTimeOffset` | `2019-05-06T12:30:05.451Z` |
| `Edm.Double` | `3.14159`, `-1.2e7`, `NaN`, `INF`, `-INF` |
| `Edm.GeographyPoint` | `geography'POINT(-122.131577 47.678581)'` |
| `Edm.GeographyPolygon` | `geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'` |
| `Edm.Int32` | `123`, `-456` |
| `Edm.Int64` | `283032927235` |
| `Edm.String` | `'hello'` |

Aşağıdaki EBNF ([genişletilmiş Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) yukarıdaki tabloda gösterilen sabitleri çoğu için dilbilgisine tanımlar. Jeo-uzamsal türlerinin dilbilgisi bulunabilir [OData Jeo-uzamsal işlevleri Azure Search'te](search-query-odata-geo-spatial-functions.md).

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
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
```

Bir etkileşimli söz dizim diyagramı görülmektedir de kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Search için OData söz dizimini diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#constant)

> [!NOTE]
> Bkz: [OData ifadesi söz dizimi başvurusu için Azure Search](search-query-odata-syntax-reference.md) tam EBNF için.

## <a name="building-expressions-from-field-paths-and-constants"></a>Alan yolları ve sabitleri ifade derleme

Alan yolları ve sabitler en temel bir OData ifadesiyle parçasıdır, ancak zaten tam ifadeler kendilerini oldukları. Aslında, **$select** parametresi Azure Search'te, alan yolları virgülle ayrılmış bir listesini ancak hiçbir şey ve **$orderby** çok daha karmaşık değil **$select**. Türünde bir alan seçerseniz `Edm.Boolean` dizininizdeki, yalnızca bu alan yolu olan bir filtre da yazabilirsiniz. Sabitler `true` ve `false` benzer şekilde geçerli filtrelerdir.

Ancak, çoğu zaman birden fazla alan ve sabiti bakın daha karmaşık ifadeler gerekir. Bu ifadeler, parametre bağlı olarak farklı şekillerde oluşturulur.

Aşağıdaki EBNF ([genişletilmiş Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) için dilbilgisine tanımlar **$filter**, **$orderby**, ve **$select** parametreleri. Bu alan yolları ve sabitleri bakın daha basit ifadeler oluşturulur:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
filter_expression ::= boolean_expression

order_by_expression ::= order_by_clause(',' order_by_clause)*

select_expression ::= '*' | field_path(',' field_path)*
```

Bir etkileşimli söz dizim diyagramı görülmektedir de kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Search için OData söz dizimini diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#filter_expression)

> [!NOTE]
> Bkz: [OData ifadesi söz dizimi başvurusu için Azure Search](search-query-odata-syntax-reference.md) tam EBNF için.

**$Orderby** ve **$select** parametrelerdir hem daha basit ifadeler virgülle ayrılmış listesi. **$Filter** oluşan bir Boole ifadesi daha basit bir alt ifade bir parametredir. Bu alt ifadeler gibi mantıksal işleçleri kullanılarak birleştirilir [ `and`, `or`, ve `not` ](search-query-odata-logical-operators.md), Karşılaştırma işleçleri gibi [ `eq`, `lt`, `gt`ve benzeri](search-query-odata-comparison-operators.md)ve koleksiyon işleçler gibi [ `any` ve `all` ](search-query-odata-collection-operators.md).

**$Filter**, **$orderby**, ve **$select** parametreleri incelediniz daha ayrıntılı için aşağıdaki makalelere bakın:

- [Azure Search'te $filter OData söz dizimi](search-query-odata-filter.md)
- [Azure Search'te $orderby OData söz dizimi](search-query-odata-orderby.md)
- [Azure Search'te OData $select söz dizimi](search-query-odata-select.md)

## <a name="see-also"></a>Ayrıca bkz.  

- [Azure Arama'da çok yönlü navigasyon](search-faceted-navigation.md)
- [Azure Search'te filtreler](search-filters.md)
- [Search belgeleri &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
- [Lucene sorgu söz dizimi](query-lucene-syntax.md)
- [Azure Search'te Basit Sorgu söz dizimi](query-simple-syntax.md)
