---
title: OData filtre başvuru - Azure Search
description: Azure arama sorgu filtresi sözdizimini OData dil referansı.
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
ms.openlocfilehash: c44645ebcbf8808cc929bfaa0c3cb0d3ee9c90cd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079918"
---
# <a name="odata-filter-syntax-in-azure-search"></a>Azure Search'te $filter OData söz dizimi

Azure Search kullanan [OData filtre ifadeleri](query-odata-filter-orderby-syntax.md) bir arama sorgusuyla tam metin arama terimleri yanı sıra ek ölçütlerini uygulamak için. Bu makalede ayrıntılı filtreler söz dizimini açıklar. Filtreleri nedir ve bunları belirli bir sorgu senaryoları hayata geçirmek için kullanma hakkında daha fazla genel bilgi için bkz: [Azure Search'te filtreler](search-filters.md).

## <a name="syntax"></a>Sözdizimi

Bir filtredir sırayla biri birden fazla deyim tarafından aşağıdaki EBNF gösterildiği olabilir bir Boole ifadesi OData dilde ([genişletilmiş Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)):

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
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
```

Bir etkileşimli söz dizim diyagramı görülmektedir de kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Search için OData söz dizimini diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#boolean_expression)

> [!NOTE]
> Bkz: [OData ifadesi söz dizimi başvurusu için Azure Search](search-query-odata-syntax-reference.md) tam EBNF için.

Boolean ifadeleri türleri şunlardır:

- Koleksiyon filtre ifadeleri kullanarak `any` veya `all`. Bu koleksiyon alanları için filtre ölçütlerini uygulamak. Daha fazla bilgi için [OData toplama işleçleri Azure Search'te](search-query-odata-collection-operators.md).
- Birleştirme işleçleri kullanarak diğer Boole ifadesine mantıksal ifadeleri `and`, `or`, ve `not`. Daha fazla bilgi için [Azure Aramadaki mantıksal işleçler OData](search-query-odata-logical-operators.md).
- Alan karşılaştırma veya aralık değişkenleri işleçleri kullanarak sabit değerleri için karşılaştırma ifadeleri `eq`, `ne`, `gt`, `lt`, `ge`, ve `le`. Daha fazla bilgi için [OData karşılaştırma işleçlerinden Azure Search'te](search-query-odata-comparison-operators.md). Karşılaştırma ifadeleri kullanarak Jeo-uzamsal koordinatları arasındaki uzaklıkları karşılaştırmak için de kullanılır `geo.distance` işlevi. Daha fazla bilgi için [OData Jeo-uzamsal işlevleri Azure Search'te](search-query-odata-geo-spatial-functions.md).
- Boole sabit değerleri `true` ve `false`. Bu sabitler bazen filtreleri, program aracılığıyla oluştururken yararlı olabilir ancak Aksi halde uygulamada kullanılmak üzere eğilimli yok.
- Boole işlevleri dahil olmak üzere, çağrı:
  - `geo.intersects`, belirli bir noktaya belirli bir Çokgen içinde olup olmadığını sınar. Daha fazla bilgi için [OData Jeo-uzamsal işlevleri Azure Search'te](search-query-odata-geo-spatial-functions.md).
  - `search.in`, bir alan veya aralık değişkeni değerlerin bir listesini her bir değeri ile karşılaştırır. Daha fazla bilgi için [OData `search.in` işlevi Azure Search'te](search-query-odata-search-in-function.md).
  - `search.ismatch` ve `search.ismatchscoring`, bir filtre bağlamında tam metin arama işlemleri yürütün. Daha fazla bilgi için [Azure Search'te tam metin arama işlevleri OData](search-query-odata-full-text-search-functions.md).
- Alan yolları veya aralık türü değişkenlerindeki `Edm.Boolean`. Örneğin, adlı bir Boolean alan dizininiz varsa, `IsEnabled` ve bu alan olduğu tüm belgeleri iade etmek istediğiniz `true`, filtre ifadesi yalnızca ad olabilir `IsEnabled`.
- Parantezlerdeki ifadeler Boolean. Parantezler kullanarak açıkça bir filtre işlem sırasını belirlemek için yardımcı olabilir. OData işleçleri varsayılan önceliği hakkında daha fazla bilgi için sonraki bölüme bakın.

### <a name="operator-precedence-in-filters"></a>Filtrelerinde İşleç önceliği

Azure Search, alt ifadeler hiçbir parantezler ile bir filtre ifadesi yazarsanız, İşleç önceliği kuralları kümesi göre değerlendirir. Bu kurallar, hangi işleçleri alt ifadeler birleştirmek için kullanılan temel alır. Aşağıdaki tabloda sırada en yüksekten en düşük öncelikliye grupları listelenmiştir:

| Grup | İşleçleri |
| --- | --- |
| Mantıksal işleçler | `not` |
| Karşılaştırma işleçleri | `eq`, `ne`, `gt`, `lt`, `ge`, `le` |
| Mantıksal işleçler | `and` |
| Mantıksal işleçler | `or` |

Yukarıdaki tabloda daha yüksek olan bir işleç "daha sıkı bir şekilde bağlanacaktır" işlenenleri diğer işleçleri için. Örneğin, `and` daha yüksek bir önceliğe değil `or`, ve Karşılaştırma işleçleri, ya da daha yüksek önceliğe, için aşağıdaki iki ifadeleri eşdeğerdir:

    Rating gt 0 and Rating lt 3 or Rating gt 7 and Rating lt 10
    ((Rating gt 0) and (Rating lt 3)) or ((Rating gt 7) and (Rating lt 10))

`not` İşleci tüm--Karşılaştırma işleçleri bile daha yüksek önceliğe sahiptir. Bunun gibi bir filtre Yaz çalışırsanız nedeni budur:

    not Rating gt 5

Bu hata iletisi alırsınız:

    Invalid expression: A unary operator with an incompatible type was detected. Found operand type 'Edm.Int32' for operator kind 'Not'.

İşleci ile ilişkili olduğundan bu hata oluşur. yalnızca `Rating` tür alanı `Edm.Int32`ve tüm karşılaştırma ifadeyle. Düzeltme işleneni eklemektir `not` parantez içinde:

    not (Rating gt 5)

<a name="bkmk_limits"></a>

### <a name="filter-size-limitations"></a>Filtre boyutu sınırlamaları

Büyüklüğü ve karmaşıklığı ile Azure Search'e göndermek için filtre ifadeleri limitleri vardır. Sınırları kabaca, filtre ifadesi yan tümcelerinde sayısını temel alır. Yan tümceleri yüzlerce varsa, sınırı aşan bir risk altında olduğunu iyi taşımaktadır. Sınırsız boyutunun filtreleri oluşturmadığını şekilde uygulamanızı tasarlama öneririz.

> [!TIP]
> Kullanarak [ `search.in` işlevi](search-query-odata-search-in-function.md) yerine uzun disjunctions eşitlik karşılaştırmaları tek bir yan tümce olarak bir işlev çağrısı sayısı bu yana filtre yan tümcesi, kaçınabilir yardımcı olabilir.

## <a name="examples"></a>Örnekler

Bir temel ücrete ile en az bir yer tüm hotels $ veya 4 üzerindeki derecelendirilir 200'den daha az bulun:

    $filter=Rooms/any(room: room/BaseRate lt 200.0) and Rating ge 4

2010'dan itibaren renovated tüm hotels "Deniz görünümü Motel" dışında bulun:

    $filter=HotelName ne 'Sea View Motel' and LastRenovationDate ge 2010-01-01T00:00:00Z

2010 veya üzeri renovated tüm hotels bulun. Datetime hazır değerinde Pasifik Standart Saati için saat dilimi bilgilerini içerir:  

    $filter=LastRenovationDate ge 2010-01-01T00:00:00-08:00

Park dahil olan ve tüm odaları İçilmez olmayan olduğu tüm hotels bulun:

    $filter=ParkingIncluded and Rooms/all(room: not room/SmokingAllowed)

 \- OR-  

    $filter=ParkingIncluded eq true and Rooms/all(room: room/SmokingAllowed eq false)

Lüks veya park içerir ve 5 derecesi sahip tüm hotels bulun:  

    $filter=(Category eq 'Luxury' or ParkingIncluded eq true) and Rating eq 5

En az bir odada tüm hotels "wifi" etiketiyle bulun (her yer depolanan etiketleri sahip olduğu bir `Collection(Edm.String)` alan):  

    $filter=Rooms/any(room: room/Tags/any(tag: tag eq 'wifi'))

Tüm odaları ile tüm hotels bulun:  

    $filter=Rooms/any()

Odaları olmayan tüm hotels bulun:

    $filter=not Rooms/any()

Belirtilen başvuru noktası 10 kilometre içindeki tüm Oteller bulmak (burada `Location` bir alanı `Edm.GeographyPoint`):

    $filter=geo.distance(Location, geography'POINT(-122.131577 47.678581)') le 10

Bir çokgenin tanımlanan belirli bir görünüm penceresinin içinde tüm Oteller bulmak (burada `Location` Edm.GeographyPoint türünde bir alan). İlk anlamı Çokgen kapatılmalıdır ve son noktası kümeleri aynı olması gerekir. Ayrıca, [noktaları saat yönünün tersi düzende listelenmelidir](https://docs.microsoft.com/rest/api/searchservice/supported-data-types#Anchor_1).

    $filter=geo.intersects(Location, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))')

"Description" alanı null olduğu tüm hotels bulun. Hiçbir zaman ayarlandıysa veya açıkça ayarlandıysa alan null olur null:  

    $filter=Description eq null

Tüm hotels adlı 'Deniz görünümü motel' veya 'Bütçe otel' eşittir bulabilirsiniz). Bu tümcecikleri boşluk içeremez ve varsayılan sınırlayıcı alandır. Alternatif bir sınırlayıcı tek tırnak işaretleri dize üçüncü parametresi olarak belirtebilirsiniz:  

    $filter=search.in(HotelName, 'Sea View motel,Budget hotel', ',')

Tüm hotels adlı eşittir 'Deniz görünümü motel' veya 'ile ayrılmış bütçe otel' Bul ' |'):  

    $filter=search.in(HotelName, 'Sea View motel|Budget hotel', '|')

Tüm odaları etiketi 'wifi' veya 'düşürürseniz sigorta atar' sahip olduğu tüm hotels bulun:

    $filter=Rooms/any(room: room/Tags/any(tag: search.in(tag, 'wifi, tub'))

Bir eşleşme ifadeleri 'ısıtılan towel raflar' veya 'dahil hairdryer' gibi bir koleksiyon içinde etiketleri bulun.

    $filter=Rooms/any(room: room/Tags/any(tag: search.in(tag, 'heated towel racks,hairdryer included', ','))

"Rıhtımının" sözcüğüyle belgeleri bulun. Bu filtre sorgusu aynıdır bir [arama isteği](https://docs.microsoft.com/rest/api/searchservice/search-documents) ile `search=waterfront`.

    $filter=search.ismatchscoring('waterfront')

Sözcük "hostel" büyük veya buna eşit 4 ya da "motel" word belgeleriyle derecelendirme ve 5'e eşit derecelendirme belgeleri bulun. Bu istek olmadan ifade uygulanamadı `search.ismatchscoring` filtre işlemleriyle kullanarak tam metin araması birleştirir olduğundan işlev `or`.

    $filter=search.ismatchscoring('hostel') and rating ge 4 or search.ismatchscoring('motel') and rating eq 5

"Lüks" sözcüğü olmadan belgeleri bulun.

    $filter=not search.ismatch('luxury')

Okyanusu ifade "Görünüm" veya 5 değerlendirmesi eşit belgeleri bulun. `search.ismatchscoring` Sorgu yalnızca alanları karşı yürütülür `HotelName` ve `Description`. Yalnızca ikinci yan tümcesi ayrım, eşleşen belgeler döndürülecek çok--ile hotels `Rating` 5'e eşit. Bu belgeleri puanı hale sıfıra eşit olan döndürülen bunlar herhangi bir ifade puanlanmış bölümlerini eşleşmedi temizleyin.

    $filter=search.ismatchscoring('"ocean view"', 'Description,HotelName') or Rating eq 5

Hotels koşulları "otel" ve "havaalanı" beşten fazla sözcük açıklamasında uzaklıkta olduğu ve tüm odaları İçilmez olmayan nerede bulun. Bu sorgu kullanan [tam Lucene sorgu dili](query-lucene-syntax.md).

    $filter=search.ismatch('"hotel airport"~5', 'Description', 'full', 'any') and not Rooms/any(room: room/SmokingAllowed)

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Search'te filtreler](search-filters.md)
- [Azure Search için OData ifade dili genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Search için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Search belgeleri &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
