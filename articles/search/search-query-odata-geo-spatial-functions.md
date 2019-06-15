---
title: OData Jeo-uzamsal işlevi başvuru - Azure Search
description: OData Jeo-uzamsal İşlevler, geo.distance ve Azure arama sorgularında geo.intersects.
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
ms.openlocfilehash: 0ce63ab1143c784eb3e10f47c20ef2b5034d63a7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079801"
---
# <a name="odata-geo-spatial-functions-in-azure-search---geodistance-and-geointersects"></a>Azure Search - OData Jeo-uzamsal işlevleri `geo.distance` ve `geo.intersects`

Azure arama, Jeo-uzamsal sorguları destekleyen [OData filtre ifadeleri](query-odata-filter-orderby-syntax.md) aracılığıyla `geo.distance` ve `geo.intersects` işlevleri. `geo.distance` İşlevi iki nokta mesafeyi içinde uzaklık döndürür, tek bir alan veya aralık değişkenini ve sabit olan bir olan geçirilen filtre bir parçası olarak. `geo.intersects` İşlevinin döndürdükleriyle `true` belirli bir noktaya içinde belirli bir Çokgen ise, burada bir alan veya aralık değişkeni noktasıdır ve Çokgen filtresinin bir bölümü olarak geçirilen bir sabit olarak belirtilir.

`geo.distance` İşlevi de kullanılabilir [ **$orderby** parametre](search-query-odata-orderby.md) arama sonuçları belirli bir noktaya mesafe göre sıralamak için. Sözdizimi `geo.distance` içinde **$orderby** olduğundan aynı olan **$filter**. Kullanırken `geo.distance` içinde **$orderby**, uygulandığı alanın türü olmalıdır `Edm.GeographyPoint` ve ayrıca olmalıdır **sıralanabilir**.

## <a name="syntax"></a>Sözdizimi

Aşağıdaki EBNF ([genişletilmiş Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) Dilbilgisi tanımlar `geo.distance` ve `geo.intersects` işlevlerin yanı sıra, Jeo-uzamsal değerler üzerinde çalışır:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
geo_distance_call ::=
    'geo.distance(' variable ',' geo_point ')'
    | 'geo.distance(' geo_point ',' variable ')'

geo_point ::= "geography'POINT(" lon_lat ")'"

lon_lat ::= float_literal ' ' float_literal

geo_intersects_call ::=
    'geo.intersects(' variable ',' geo_polygon ')'

/* You need at least four points to form a polygon, where the first and
last points are the same. */
geo_polygon ::=
    "geography'POLYGON((" lon_lat ',' lon_lat ',' lon_lat ',' lon_lat_list "))'"

lon_lat_list ::= lon_lat(',' lon_lat)*
```

Bir etkileşimli söz dizim diyagramı görülmektedir de kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Search için OData söz dizimini diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#geo_distance_call)

> [!NOTE]
> Bkz: [OData ifadesi söz dizimi başvurusu için Azure Search](search-query-odata-syntax-reference.md) tam EBNF için.

### <a name="geodistance"></a>GEO.distance

`geo.distance` İşlev türünde iki parametre alır `Edm.GeographyPoint` ve döndüren bir `Edm.Double` arasındaki mesafeyi kilometre cinsinden uzaklık değeri. Bu, genellikle uzaklıkları ölçütleri dönüş OData Jeo-uzamsal işlemleri destekleyen diğer hizmetlerden farklıdır.

Parametrelerden biri için `geo.distance` bir Coğrafya noktası sabit olması gerekir ve bir alan yolu olması gerekir (veya bir aralık değişkeni bir filtre söz konusu olduğunda türünde bir alan üzerinde yineleme `Collection(Edm.GeographyPoint)`). Bu parametre sırası önemli değildir.

Coğrafya noktası sabiti biçimindedir `geography'POINT(<longitude> <latitude>)'`, enlem ve boylam sayısal sabitlere olduğu.

> [!NOTE]
> Kullanırken `geo.distance` bir filtrede kullanarak sabit işlevi tarafından döndürülen uzaklık karşılaştırmalıdır `lt`, `le`, `gt`, veya `ge`. İşleçler `eq` ve `ne` uzaklıkları karşılaştırılırken desteklenmez. Örneğin, bu doğru kullanımını, `geo.distance`: `$filter=geo.distance(location, geography'POINT(-122.131577 47.678581)') le 5`.

### <a name="geointersects"></a>GEO.intersects

`geo.intersects` İşlev türünün bir değişkeni alır `Edm.GeographyPoint` ve sabit `Edm.GeographyPolygon` ve döndüren bir `Edm.Boolean`  --  `true` noktasının Çokgen sınırları içinde olup olmadığını `false` Aksi takdirde.

Çokgen sınırlayıcı halkası tanımlama noktaları dizisi olarak depolanan bir iki boyutlu yüzeydir (bkz [örnekler](#examples) aşağıda). Çokgen kapatılması, yani ilk gerekir ve son noktası kümeleri aynı olması gerekir. [Bir çokgenin noktaları saat yönünün tersi sırada olması gerekir](https://docs.microsoft.com/rest/api/searchservice/supported-data-types#Anchor_1).

### <a name="geo-spatial-queries-and-polygons-spanning-the-180th-meridian"></a>Jeo-uzamsal sorgular ve 180th meridyen kapsayan çokgenler

180th meridyen (yakın tarih çizgisi) içeren bir sorgu formulating kitaplıkları için birçok Jeo-uzamsal sorgu geçerli off-limits veya iki tane meridyen her iki tarafındaki içine Çokgen bölme gibi bir çözüm gerektirir.

Azure Search'te 180 derecelik boylam dahil Jeo-uzamsal sorguları sorgu şekildir dikdörtgen ve boylam ve enlem boyunca bir kılavuz düzeni, koordinatları Hizala beklendiği gibi çalıştığından (örneğin, `geo.intersects(location, geography'POLYGON((179 65, 179 66, -179 66, -179 65, 179 65))'`). Aksi takdirde, olmayan veya hizalanmamış şekiller için bölünmüş Çokgen yaklaşımı göz önünde bulundurun.  

### <a name="geo-spatial-functions-and-null"></a>Jeo-uzamsal işlevleri ve `null`

Azure Search'te türünde alanlar diğer tüm koleksiyon dışı alanlar gibi `Edm.GeographyPoint` içerebilir `null` değerleri. Azure Search değerlendirirken `geo.intersects` bir alan için `null`, sonuç her zaman olacaktır `false`. Davranışını `geo.distance` bu durumda bağlam üzerinde bağlıdır:

- Filtreler de `geo.distance` , bir `null` alan sonuçlarında `null`. Bu belge, çünkü eşleşmez anlamına gelir `null` karşılaştırıldığında her null olmayan değer değerlendirilen `false`.
- Kullanarak sonuçları sıralarken **$orderby**, `geo.distance` , bir `null` alan olası en büyük uzaklık sonuçlanır. Böyle bir alana belgelerle sıralama diğerlerini daha düşük olduğunda sıralama yönünü `asc` olduğunu (varsayılan), kullanılan ve diğerlerini yönü olduğunda daha yüksek `desc`.

## <a name="examples"></a>Örnekler

### <a name="filter-examples"></a>Filtre örnekleri

Belirtilen başvuru noktası 10 kilometre içindeki tüm Oteller bulmak (konum türünde bir alan olduğu `Edm.GeographyPoint`):

    geo.distance(location, geography'POINT(-122.131577 47.678581)') le 10

Bir çokgenin tanımlanan belirli bir görünüm penceresinin içinde tüm Oteller bulmak (konum türünde bir alan olduğu `Edm.GeographyPoint`). Çokgen kapalı olduğunu unutmayın (ilk ve son noktası kümeleri aynı olmalıdır) ve [noktaları saat yönünün tersi düzende listelenmelidir](https://docs.microsoft.com/rest/api/searchservice/supported-data-types#Anchor_1).

    geo.intersects(location, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))')

### <a name="order-by-examples"></a>Sipariş tarafından örnekleri

Sıralama göre azalan sırada hotels `rating`, ardından uzaklık tarafından verilen koordinatlarından artan:

    rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc

Hotels göre azalan düzende sıralamak `search.score` ve `rating`ve böylece aynı dereceye sahip iki hotels en yakındakine listede ilk sıradaysa ardından artan düzende uzaklık tarafından verilen koordinatlarından:

    search.score() desc,rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Search'te filtreler](search-filters.md)
- [Azure Search için OData ifade dili genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Search için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Search belgeleri &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
