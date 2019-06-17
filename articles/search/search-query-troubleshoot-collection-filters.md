---
title: OData koleksiyon filtreleri - Azure Search sorunlarını giderme
description: Azure arama sorguları OData koleksiyon filtresi hatalarında sorun giderme.
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
ms.openlocfilehash: c7fa00c82eea03a50bae22fcb1ad16e230aa5bcb
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079632"
---
# <a name="troubleshooting-odata-collection-filters-in-azure-search"></a>Azure Search'te OData toplama filtreleri sorunlarını giderme

İçin [filtre](query-odata-filter-orderby-syntax.md) Azure Search'te toplama alanlar üzerinde kullanabileceğiniz [ `any` ve `all` işleçleri](search-query-odata-collection-operators.md) ile birlikte **lambda ifadeleri**. Bir koleksiyonun her öğesine uygulanan bir alt filtre bir lambda ifadesidir.

Filtre ifadeleri her özellik, bir lambda ifadesinde kullanılabilir. Hangi özelliklerin kullanılabilir olduğunu filtre uygulamak istediğiniz koleksiyon alanın veri türüne bağlı olarak farklılık gösterir. Bu bağlamda desteklenmiyor bir lambda ifadesinde bir özellik kullanmayı denerseniz bu hataya neden. Bu makalede, karmaşık filtre koleksiyon alanları yazmaya çalışırken bunun gibi hataları istemcilerinizle, sorunu gidermenize yardımcı olur.

## <a name="common-collection-filter-errors"></a>Genel koleksiyon filtre hataları

Aşağıdaki tabloda, bir koleksiyon filtresi yürütmeye çalışırken karşılaşabileceğiniz hatalar listelenmektedir. Bir lambda ifadesi içinde desteklenmeyen bir özellik filtre ifadeleri kullanın Bu hataları meydana gelir. Her hata hatayı önlemek için filtre nasıl yeniden bazı yönergeler verir. Tablo, bu hatadan kaçınmak daha fazla bilgi sağlar. Bu makalenin ilgili bölüm için bağlantıyı da içerir.

| Hata iletisi | Durum | Daha fazla bilgi için bkz. |
| --- | --- | --- |
| ' % S'işlevi 'IsMatch' aralık değişkeni için kullanıcının bağlı parametresi yok '. Yalnızca alan başvuruları ('any' veya 'all') lambda ifadelerinin içinde desteklenen bağlı. Lütfen filtrenizi 'IsMatch' işlev dışında lambda ifadesi, böylece değiştirin ve yeniden deneyin. | Kullanarak `search.ismatch` veya `search.ismatchscoring` bir lambda ifadesi içinde | [Karmaşık koleksiyonları filtrelemek için kurallar](#bkmk_complex) |
| Geçersiz bir lambda ifadesi. Bunun tersi Collection(Edm.String) türünde bir alan üzerinde yinelenen bir lambda ifadesinde burada bekleniyordu eşitlik ve eşitsizlik için test bulundu. ' İçin ', lütfen form 'x eq y' veya 'search.in(...)' ifadeleri kullanın. İçin 'all', lütfen kullanın deyimler form 'x ne y', 'olmayan (x eq y)' veya 'olmayan search.in(...)'. | Türünde bir alan üzerinde filtreleme `Collection(Edm.String)` | [Dize koleksiyonlarını filtreleme kuralları](#bkmk_strings) |
| Geçersiz bir lambda ifadesi. Karmaşık bir Boole ifadesi desteklenmeyen bir tür bulunamadı. ' İçin ', lütfen 'ORs, Equal', olarak da bilinen ayırt edici Normal Form ifadeler kullanın. Örneğin: '(a and b) veya (c ve d)' burada a, b, c ve d karşılaştırma veya eşitlik alt ifadeleri kullanabilirsiniz. İçin 'all', lütfen kullanın 'Equal'ın ORs', olarak da bilinen Conjunctive Normal Form ifadeler. Örneğin: '(a or b) ve (c) veya d' burada a, b, c ve d karşılaştırma ya da eşitsizlik alt ifadeler olan. Karşılaştırma ifadeleri örnekleri: ' x gt 5', ' le 2 x. Eşitlik ifadesi örneği: ' x eq 5'. Bir eşitsizlik ifade örneği: ' x ne 5'. | Türünde alanlar üzerinde filtreleme `Collection(Edm.DateTimeOffset)`, `Collection(Edm.Double)`, `Collection(Edm.Int32)`, veya `Collection(Edm.Int64)` | [Karşılaştırılabilir koleksiyonları filtrelemek için kurallar](#bkmk_comparables) |
| Geçersiz bir lambda ifadesi. Desteklenmeyen bir kullanım geo.distance() veya geo.intersects() Collection(Edm.GeographyPoint) türünde bir alan üzerinde yinelenen bir lambda ifadesi bulundu. 'Herhangi biri için', 'lt' veya 'le' işleçleri kullanarak geo.distance() karşılaştırın ve geo.intersects() kullanımı değil değilleme uygulandığını emin olun sağlayın. İçin 'all', 'gt' veya 'ge' işleçleri kullanarak geo.distance() karşılaştırın ve geo.intersects() kullanımı değilleme uygulandığını emin emin olun. | Türünde bir alan üzerinde filtreleme `Collection(Edm.GeographyPoint)` | [GeographyPoint koleksiyonları filtrelemek için kurallar](#bkmk_geopoints) |
| Geçersiz bir lambda ifadesi. Karmaşık mantıksal ifadeleri Collection(Edm.GeographyPoint) türünde alanlar üzerinde yineleme lambda ifadeleri desteklenmez. Lütfen katılın 'herhangi için' ile alt ifadeler 'veya'; 've' desteklenmiyor. İçin 'all', lütfen JOIN ile alt ifadeler 've'; 'veya' desteklenmiyor. | Türünde alanlar üzerinde filtreleme `Collection(Edm.String)` veya `Collection(Edm.GeographyPoint)` | [Dize koleksiyonlarını filtreleme kuralları](#bkmk_strings) <br/><br/> [GeographyPoint koleksiyonları filtrelemek için kurallar](#bkmk_geopoints) |
| Geçersiz bir lambda ifadesi. Bir karşılaştırma işleci (bir 'lt', 'le', 'gt' veya 'ge') bulunamadı. Eşitlik işleçleri, lambda ifadelerinde Collection(Edm.String) türünde alanlar üzerinde yineleme izin verilir. ' İçin ', lütfen ' % s'form 'x eq y' ifadeleri kullanın. İçin 'all', lütfen kullanın x ne form 'y' veya 'olmayan (x eq y)' ifadeleri. | Türünde bir alan üzerinde filtreleme `Collection(Edm.String)` | [Dize koleksiyonlarını filtreleme kuralları](#bkmk_strings) |

<a name="bkmk_examples"></a>

## <a name="how-to-write-valid-collection-filters"></a>Geçerli koleksiyon yazma filtreleri

Geçerli koleksiyon filtreleri yazmak için kuralları, her veri türü için farklıdır. Aşağıdaki bölümlerde, filtre özelliklerin desteklendiği ve hangi olmayan örnekler göstererek kurallar açıklanmaktadır:

- [Dize koleksiyonlarını filtreleme kuralları](#bkmk_strings)
- [Boole koleksiyonları filtrelemek için kurallar](#bkmk_bools)
- [GeographyPoint koleksiyonları filtrelemek için kurallar](#bkmk_geopoints)
- [Karşılaştırılabilir koleksiyonları filtrelemek için kurallar](#bkmk_comparables)
- [Karmaşık koleksiyonları filtrelemek için kurallar](#bkmk_complex)

<a name="bkmk_strings"></a>

## <a name="rules-for-filtering-string-collections"></a>Dize koleksiyonlarını filtreleme kuralları

Dize koleksiyonları için lambda ifadelerinin içinde kullanılabilecek tek bir Karşılaştırma işleçleri olan `eq` ve `ne`.

> [!NOTE]
> Azure Search'ü desteklemez `lt` / `le` / `gt` / `ge` dizeleri işleçleri içinde veya dışında bir lambda ifadesi.

Gövdesi bir `any` yalnızca gövdesi sırasında eşitlik için test edebilirsiniz bir `all` yalnızca eşitsizlik için test edebilirsiniz.

Birden fazla ifade aracılığıyla birleştirmek mümkündür `or` gövdesinde bir `any`, aracılığıyla `and` gövdesinde bir `all`. Bu yana `search.in` eşitlik denetimleri ile birleştirmek için işlev eşdeğerdir `or`, gövdesinde ayrıca izin verilen bir `any`. Buna karşılık, `not search.in` gövdesinde izin verilen bir `all`.

Örneğin, bu deyimler izin verilir:

- `tags/any(t: t eq 'books')`
- `tags/any(t: search.in(t, 'books, games, toys'))`
- `tags/all(t: t ne 'books')`
- `tags/all(t: not (t eq 'books'))`
- `tags/all(t: not search.in(t, 'books, games, toys'))`
- `tags/any(t: t eq 'books' or t eq 'games')`
- `tags/all(t: t ne 'books' and not (t eq 'games'))`

Bu ifadeler izin verilmeyen sırada:

- `tags/any(t: t ne 'books')`
- `tags/any(t: not search.in(t, 'books, games, toys'))`
- `tags/all(t: t eq 'books')`
- `tags/all(t: search.in(t, 'books, games, toys'))`
- `tags/any(t: t eq 'books' and t ne 'games')`
- `tags/all(t: t ne 'books' or not (t eq 'games'))`

<a name="bkmk_bools"></a>

## <a name="rules-for-filtering-boolean-collections"></a>Boole koleksiyonları filtrelemek için kurallar

Türü `Edm.Boolean` yalnızca destekler `eq` ve `ne` işleçleri. Bu nedenle, aynı aralık değişkeniyle denetlemek gibi yan tümceleri birleştirmeye olanak tanıyan çok mantıklı değildir `and` / `or` olduğundan, her zaman tautologies veya contradictions sunulmasını sağlar.

İzin verilen Boole koleksiyonları filtreleriyle bazı örnekleri aşağıda verilmiştir:

- `flags/any(f: f)`
- `flags/all(f: f)`
- `flags/any(f: f eq true)`
- `flags/any(f: f ne true)`
- `flags/all(f: not f)`
- `flags/all(f: not (f eq true))`

Dize koleksiyonlarından farklı olarak, tür lambda ifadesinin sınırsız işleci kullanılabilir Boolean koleksiyonları sahiptir. Her ikisi de `eq` ve `ne` gövdesinde kullanılan `any` veya `all`.

Aşağıdaki gibi ifadeler Boole koleksiyonlar için izin verilmez:

- `flags/any(f: f or not f)`
- `flags/any(f: f or f)`
- `flags/all(f: f and not f)`
- `flags/all(f: f and f eq true)`

<a name="bkmk_geopoints"></a>

## <a name="rules-for-filtering-geographypoint-collections"></a>GeographyPoint koleksiyonları filtrelemek için kurallar

Türü değerlerinin `Edm.GeographyPoint` birbirine doğrudan bir koleksiyonda karşılaştırılamaz. Bunun yerine, bunlar için parametre olarak kullanılması gereken `geo.distance` ve `geo.intersects` işlevleri. `geo.distance` İşlevi sırayla gerekir karşılaştırılabilir karşılaştırma işleçlerinden birini kullanarak bir uzaklık değeri `lt`, `le`, `gt`, veya `ge`. Bu kurallar, koleksiyon dışı Edm.GeographyPoint alanlar için de geçerlidir.

Gibi dize koleksiyonlarını `Edm.GeographyPoint` koleksiyonları nasıl Jeo-uzamsal işlevleri kullanılır ve birleştirilmiş farklı türlerdeki lambda ifadeleri için bazı kurallara sahiptir:

- Hangi Karşılaştırma işleçleri ile birlikte kullanabileceğiniz `geo.distance` işlevi lambda ifadesi türüne bağlıdır. İçin `any`, yalnızca kullanabilirsiniz `lt` veya `le`. İçin `all`, yalnızca kullanabilirsiniz `gt` veya `ge`. İfadeler içeren negate `geo.distance`, ancak karşılaştırma işleci değişikliği yapmanız (`geo.distance(...) lt x` olur `not (geo.distance(...) ge x)` ve `geo.distance(...) le x` olur `not (geo.distance(...) gt x)`).
- Gövdesinde bir `all`, `geo.intersects` işlevi negatif. Buna karşılık, gövdesinde bir `any`, `geo.intersects` işlevi gerekir değil tasarruflarını.
- Gövdesinde bir `any`, Jeo-uzamsal ifadeleri kullanarak birleştirilebilir `or`. Gövdesinde bir `all`, bu tür ifadeleri kullanılarak birleştirilebilir `and`.

Yukarıdaki sınırlamalara benzer nedenlerle dize koleksiyonlarını eşitlik/eşitsizlik sınırlamasını olarak mevcut. Bkz: [Azure Search'te toplama filtreleri anlama OData](search-query-understand-collection-filters.md) bu nedenle, daha ayrıntılı bir bakış.

Üzerinde filtre bazı örnekler şunlardır `Edm.GeographyPoint` izin verilen koleksiyonlar:

- `locations/any(l: geo.distance(l, geography'POINT(-122 49)') lt 10)`
- `locations/any(l: not (geo.distance(l, geography'POINT(-122 49)') ge 10) or geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`
- `locations/all(l: geo.distance(l, geography'POINT(-122 49)') ge 10 and not geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`

Aşağıdaki gibi ifadeler için izin verilmez `Edm.GeographyPoint` koleksiyonlar:

- `locations/any(l: l eq geography'POINT(-122 49)')`
- `locations/any(l: not geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`
- `locations/all(l: geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`
- `locations/any(l: geo.distance(l, geography'POINT(-122 49)') gt 10)`
- `locations/all(l: geo.distance(l, geography'POINT(-122 49)') lt 10)`
- `locations/any(l: geo.distance(l, geography'POINT(-122 49)') lt 10 and geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`
- `locations/all(l: geo.distance(l, geography'POINT(-122 49)') le 10 or not geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`

<a name="bkmk_comparables"></a>

## <a name="rules-for-filtering-comparable-collections"></a>Karşılaştırılabilir koleksiyonları filtrelemek için kurallar

Bu bölüm, aşağıdaki tüm veri türleri için geçerlidir:

- `Collection(Edm.DateTimeOffset)`
- `Collection(Edm.Double)`
- `Collection(Edm.Int32)`
- `Collection(Edm.Int64)`

Gibi türleri `Edm.Int32` ve `Edm.DateTimeOffset` altısı da Karşılaştırma işleçleri destekler: `eq`, `ne`, `lt`, `le`, `gt`, ve `ge`. Bu türler üzerinde lambda ifadeleri, bu işleçler birini kullanarak basit bir ifade içerebilir. Bu, her ikisi için de geçerlidir `any` ve `all`. Örneğin, bu filtrelere izin verilir:

- `ratings/any(r: r ne 5)`
- `dates/any(d: d gt 2017-08-24T00:00:00Z)`
- `not margins/all(m: m eq 3.5)`

Ancak, lambda ifadesi içinde daha karmaşık ifadeler gibi karşılaştırma ifadeleri nasıl birleştirilebilir sınırlamalar vardır:

- İçin kuralları `any`:
  - Basit eşitsizlik ifadeleri usefully diğer ifadelerle birleştirilemez. Örneğin, bu ifade izin verilir:
    - `ratings/any(r: r ne 5)`

    Ancak bu ifade değildir:
    - `ratings/any(r: r ne 5 and r gt 2)`

    ve bu ifade izin verilse de koşulları çakışıyor çünkü bu kullanışlı değildir:
    - `ratings/any(r: r ne 5 or r gt 7)`
  - Basit karşılaştırma ifadeleri içeren `eq`, `lt`, `le`, `gt`, veya `ge` birleştirilebilir `and` / `or`. Örneğin:
    - `ratings/any(r: r gt 2 and r le 5)`
    - `ratings/any(r: r le 5 or r gt 7)`
  - Karşılaştırma ifadeleri Sunucusu'yla birlikte `and` (bağlaçlar) daha fazla birleştirilebilir kullanarak `or`. Bu form, Boolean mantıksal bilinen "[ayırt edici Normal Form](https://en.wikipedia.org/wiki/Disjunctive_normal_form)" (DNF). Örneğin:
    - `ratings/any(r: (r gt 2 and r le 5) or (r gt 7 and r lt 10))`
- İçin kuralları `all`:
  - Basit eşitlik ifadeleridir usefully diğer ifadelerle birleştirilemez. Örneğin, bu ifade izin verilir:
    - `ratings/all(r: r eq 5)`

    Ancak bu ifade değildir:
    - `ratings/all(r: r eq 5 or r le 2)`

    ve bu ifade izin verilse de koşulları çakışıyor çünkü bu kullanışlı değildir:
    - `ratings/all(r: r eq 5 and r le 7)`
  - Basit karşılaştırma ifadeleri içeren `ne`, `lt`, `le`, `gt`, veya `ge` birleştirilebilir `and` / `or`. Örneğin:
    - `ratings/all(r: r gt 2 and r le 5)`
    - `ratings/all(r: r le 5 or r gt 7)`
  - Karşılaştırma ifadeleri Sunucusu'yla birlikte `or` (disjunctions) daha fazla birleştirilebilir kullanarak `and`. Bu form, Boolean mantıksal bilinen "[Conjunctive Normal Form](https://en.wikipedia.org/wiki/Conjunctive_normal_form)" (CNF). Örneğin:
    - `ratings/all(r: (r le 2 or gt 5) and (r lt 7 or r ge 10))`

<a name="bkmk_complex"></a>

## <a name="rules-for-filtering-complex-collections"></a>Karmaşık koleksiyonları filtrelemek için kurallar

Lambda ifadeleri karmaşık koleksiyonlar üzerinden çok daha esnek bir söz dizimi lambda ifadeleri daha ilkel türler koleksiyonlarının destekler. Herhangi bir filtre yapısı yalnızca iki özel durumu dışında bir kullanabileceğiniz gibi bir lambda ifadesi içinde kullanabilirsiniz.

İlk olarak işlevleri `search.ismatch` ve `search.ismatchscoring` lambda ifadelerinin içinde desteklenmez. Daha fazla bilgi için [Azure Search'te toplama filtreleri anlama OData](search-query-understand-collection-filters.md).

İkinci olarak, olmayan alanlara başvuran *bağlı* aralık değişkeni için (sözde *ücretsiz değişkenleri*) izin verilmiyor. Örneğin, aşağıdaki iki eşdeğer OData filtre ifadeleri göz önünde bulundurun:

1. `stores/any(s: s/amenities/any(a: a eq 'parking')) and details/margin gt 0.5`
1. `stores/any(s: s/amenities/any(a: a eq 'parking' and details/margin gt 0.5))`

İlk ifade izin verilir, çünkü form ikinci reddedilir `details/margin` aralık değişkenine bağlı olmayan `s`.

Bu kural, ayrıca dış kapsamda bağlı değişkenleriniz ifadelerini genişletir. Bu değişkenleri göründükleri kapsamında yer ücretsizdir. İkinci eşdeğer ifade için izin verilmiyor sırasında Örneğin, ilk ifade izin `s/name` Aralık değişkeninin kapsamında yer ücretsizdir `a`:

1. `stores/any(s: s/amenities/any(a: a eq 'parking') and s/name ne 'Flagship')`
1. `stores/any(s: s/amenities/any(a: a eq 'parking' and s/name ne 'Flagship'))`

Her zaman lambda ifadeleri yalnızca bağımlı değişkenler içeren şekilde filtreleri oluşturmak mümkün olduğundan bu sınırlama, uygulamada bir sorun olmamalıdır.

## <a name="cheat-sheet-for-collection-filter-rules"></a>Koleksiyon filtre kurallarına yönelik kopya kağıdı

Aşağıdaki tabloda her bir koleksiyon veri türü için geçerli bir filtre oluşturmak için kurallar özetlenmiştir.

[!INCLUDE [Limitations on OData lambda expressions in Azure Search](../../includes/search-query-odata-lambda-limitations.md)]

Her durum için geçerli bir filtre oluşturmak nasıl bir örnekleri için bkz: [geçerli koleksiyon filtreleri yazmak nasıl](#bkmk_examples).

Genellikle yazma filtreleri ve kuralları ilk ilkeler anlama yardımcı bunları memorizing birden fazla bkz [Azure Search'te toplama filtreleri anlama OData](search-query-understand-collection-filters.md).

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Search'te OData toplama filtreleri anlama](search-query-understand-collection-filters.md)
- [Azure Search'te filtreler](search-filters.md)
- [Azure Search için OData ifade dili genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Search için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Search belgeleri &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
