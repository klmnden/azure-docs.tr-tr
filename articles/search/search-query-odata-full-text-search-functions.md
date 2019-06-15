---
title: OData tam metin araması işlevi başvuru - Azure Search
description: Search.ismatch ve Azure arama sorgularında search.ismatchscoring OData tam metin arama işlevleri.
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
ms.openlocfilehash: 158312a7afe88e7b9885376c5d28b01958acbbfb
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079814"
---
# <a name="odata-full-text-search-functions-in-azure-search---searchismatch-and-searchismatchscoring"></a>Azure Search - OData tam metin arama işlevleri `search.ismatch` ve `search.ismatchscoring`

Azure arama, tam metin araması bağlamında destekler [OData filtre ifadeleri](query-odata-filter-orderby-syntax.md) aracılığıyla `search.ismatch` ve `search.ismatchscoring` işlevleri. Bu işlevler, tam metin araması katı Boole yalnızca üst düzey kullanarak mümkün olmayan bir yolla filtreleme ile birleştirmek izin `search` parametresinin [arama API'si](https://docs.microsoft.com/rest/api/searchservice/search-documents).

> [!NOTE]
> `search.ismatch` Ve `search.ismatchscoring` işlevleri filtreleri, desteklenen yalnızca [arama API'si](https://docs.microsoft.com/rest/api/searchservice/search-documents). İçinde desteklenmeyen [Öner](https://docs.microsoft.com/rest/api/searchservice/suggestions) veya [otomatik tamamlama](https://docs.microsoft.com/rest/api/searchservice/autocomplete) API'leri.

## <a name="syntax"></a>Sözdizimi

Aşağıdaki EBNF ([genişletilmiş Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) Dilbilgisi tanımlar `search.ismatch` ve `search.ismatchscoring` İşlevler:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
search_is_match_call ::=
    'search.ismatch'('scoring')?'(' search_is_match_parameters ')'

search_is_match_parameters ::=
    string_literal(',' string_literal(',' query_type ',' search_mode)?)?

query_type ::= "'full'" | "'simple'"

search_mode ::= "'any'" | "'all'"
```

Bir etkileşimli söz dizim diyagramı görülmektedir de kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Search için OData söz dizimini diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#search_is_match_call)

> [!NOTE]
> Bkz: [OData ifadesi söz dizimi başvurusu için Azure Search](search-query-odata-syntax-reference.md) tam EBNF için.

### <a name="searchismatch"></a>search.ismatch

`search.ismatch` İşlevi bir tam metin arama sorgusu bir filtre ifadesi bir parçası olarak değerlendirir. Arama sorgusuyla eşleşen belgelerin sonuç kümesinde döndürülür. Bu işlevin aşağıdaki aşırı kullanılabilir:

- `search.ismatch(search)`
- `search.ismatch(search, searchFields)`
- `search.ismatch(search, searchFields, queryType, searchMode)`

Parametreleri aşağıdaki tabloda tanımlanmıştır:

| Parametre adı | Tür | Açıklama |
| --- | --- | --- |
| `search` | `Edm.String` | Arama sorgusu (her ikisinde [basit](query-simple-syntax.md) veya [tam](query-lucene-syntax.md) Lucene sorgu söz dizimi). |
| `searchFields` | `Edm.String` | Virgülle ayrılmış bir liste içinde arama yapmak aranabilir alanları; Varsayılan olarak dizindeki tüm aranabilir alanları. Kullanırken [arama fielded](query-lucene-syntax.md#bkmk_fields) içinde `search` parametresi alan Lucene sorgu geçersiz kılma tanımlayıcıları bu parametrede belirtilen herhangi bir alan. |
| `queryType` | `Edm.String` | `'simple'` veya `'full'`; varsayılan olarak `'simple'`. Hangi sorgu dili kullanıldı belirtir `search` parametresi. |
| `searchMode` | `Edm.String` | `'any'` veya `'all'`, varsayılan olarak `'any'`. Tüm arama, koşulları olup olmadığını gösteren `search` parametresi eşleşen, belgeyi bir eşleşme olarak saymak için. Kullanırken [Lucene Boole işleçleri](query-lucene-syntax.md#bkmk_boolean) içinde `search` parametresi, bunlar önceliklidir Bu parametre. |

Yukarıdaki tüm parametreleri karşılık gelen eşdeğerdir [istek parametrelerini arama API'si arama](https://docs.microsoft.com/rest/api/searchservice/search-documents).

`search.ismatch` İşlevi türünde bir değer döndürür `Edm.Boolean`, diğer filtre alt ifadeleri kullanarak bir Boole değeri oluşturan olanak tanıyan [mantıksal işleçler](search-query-odata-logical-operators.md).

> [!NOTE]
> Azure Search'ü kullanarak desteklemiyor `search.ismatch` veya `search.ismatchscoring` lambda ifadeleri iç. Başka bir deyişle, tam metin arama sonuçları aynı nesne üzerinde katı filtre eşleşme ile ilişkilendirebilmek nesne koleksiyonları üzerinde yazma filtreleri mümkün değildir. Bu sınırlama yanı sıra örnekleri hakkında daha fazla bilgi için bkz. [Azure Search'te toplama filtreleri sorun giderme](search-query-troubleshoot-collection-filters.md). Neden ilişkin daha ayrıntılı bilgi için bu sınırlama var, bkz: [Azure Search'te toplama filtreleri anlama](search-query-understand-collection-filters.md).


### <a name="searchismatchscoring"></a>Search.ismatchscoring

`search.ismatchscoring` Gibi işlev `search.ismatch` işlevinin döndürdükleriyle `true` tam metin arama sorgusuyla eşleşen belgeler için bir parametre olarak geçirilir. İlgi eşleşen belgelerin puan fark aralarında `search.ismatchscoring` sorgu durumunda sırada genel belge puanı katkıda `search.ismatch`, belge puanı değiştirilmez. Bu işlevin aşağıdaki aşırı gereksinimlerine aynı parametrelerle kullanılabilir `search.ismatch`:

- `search.ismatchscoring(search)`
- `search.ismatchscoring(search, searchFields)`
- `search.ismatchscoring(search, searchFields, queryType, searchMode)`

Hem `search.ismatch` ve `search.ismatchscoring` işlevleri aynı filtre ifadesinde kullanılabilir.

## <a name="examples"></a>Örnekler

"Rıhtımının" sözcüğüyle belgeleri bulun. Bu filtre sorgusu aynıdır bir [arama isteği](https://docs.microsoft.com/rest/api/searchservice/search-documents) ile `search=waterfront`.

    search.ismatchscoring('waterfront')

Sözcük "hostel" büyük veya buna eşit 4 ya da "motel" word belgeleriyle derecelendirme ve 5'e eşit derecelendirme belgeleri bulun. Unutmayın, bu isteği değil ifade edilemez olmadan `search.ismatchscoring` işlevi.

    search.ismatchscoring('hostel') and Rating ge 4 or search.ismatchscoring('motel') and Rating eq 5

"Lüks" sözcüğü olmadan belgeleri bulun.

    not search.ismatch('luxury')

Okyanusu ifade "Görünüm" veya 5 değerlendirmesi eşit belgeleri bulun. `search.ismatchscoring` Sorgu yalnızca alanları karşı yürütülür `HotelName` ve `Rooms/Description`.

Yalnızca ikinci yan tümcesi ayrım, eşleşen belgeler döndürülecek çok--ile hotels `Rating` 5'e eşit. Bunu yapmak için bu belgelere herhangi bir ifade puanlanmış bölümleri ile eşleşmedi, sıfıra eşit puanı döndürülür temizleyin.

    search.ismatchscoring('"ocean view"', 'Rooms/Description,HotelName') or Rating eq 5

Koşulları "otel" ve "havaalanı" birbirinden 5 sözcük ise açıklaması içinde olduğu ve burada İçilmez en az izin verilmez bulmanın bazı odaları. Bu sorgu kullanan [tam Lucene sorgu dili](query-lucene-syntax.md).

    search.ismatch('"hotel airport"~5', 'Description', 'full', 'any') and Rooms/any(room: not room/SmokingAllowed)

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Search'te filtreler](search-filters.md)
- [Azure Search için OData ifade dili genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Search için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Search belgeleri &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
