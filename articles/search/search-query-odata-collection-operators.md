---
title: OData koleksiyon işleci başvuru - Azure Search
description: OData toplama işleçleri, tüm ve lambda ifadelerinde Azure arama sorguları.
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
ms.openlocfilehash: 7afafe158732b14ebe314eeee5d015acddc55b72
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079944"
---
# <a name="odata-collection-operators-in-azure-search---any-and-all"></a>Azure Search - OData toplama işleçleri `any` ve `all`

Yazarken bir [OData filtre ifadesinin](query-odata-filter-orderby-syntax.md) Azure Search ile kullanmak için genellikle koleksiyon alanlarını filtrelemek yararlıdır. Kullanarak bunu gerçekleştirebilirsiniz `any` ve `all` işleçleri.

## <a name="syntax"></a>Sözdizimi

Aşağıdaki EBNF ([genişletilmiş Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) kullanan bir OData ifade dilbilgisi tanımlar `any` veya `all`.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
collection_filter_expression ::=
    field_path'/all(' lambda_expression ')'
    | field_path'/any(' lambda_expression ')'
    | field_path'/any()'

lambda_expression ::= identifier ':' boolean_expression
```

Bir etkileşimli söz dizim diyagramı görülmektedir de kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Search için OData söz dizimini diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#collection_filter_expression)

> [!NOTE]
> Bkz: [OData ifadesi söz dizimi başvurusu için Azure Search](search-query-odata-syntax-reference.md) tam EBNF için.

Koleksiyonları filtrelemek ifade üç tür vardır.

- İlk ikisi, koleksiyonun her öğesine bir lambda ifadesinin biçiminde belirtilen bir koşulu uygulama koleksiyonu alana üzerinden yineleme yapma.
  - Bir ifade kullanarak `all` döndürür `true` koşul koleksiyonunun her öğesi için doğru olması durumunda.
  - Bir ifade kullanarak `any` döndürür `true` koşul koleksiyon en az bir öğe için doğru olması durumunda.
- Üçüncü formu koleksiyonunun kullandığı filtre `any` olmadan bir koleksiyon alanın boş olup olmadığını test etmek için bir lambda ifadesi. Koleksiyon herhangi bir öğe olup olmadığını döndürür `true`. Koleksiyon boş değilse döndürür `false`.

A **lambda ifadesi** bir koleksiyonda bir programlama dilinde bir döngü gövdesinin filtre gibidir. Bir değişkeni tanımlar adlı **aralık değişkeni**, koleksiyon yineleme sırasında geçerli öğe tutan. Ayrıca, filtre ölçütünü koleksiyonundaki her öğe için aralık değişkeni için geçerli olan başka bir Boole ifadesi tanımlar.

## <a name="examples"></a>Örnekler

Eşleşme belgeleri `tags` "wifi" dizesi tam olarak alan içerir:

    tags/any(t: t eq 'wifi')

Eşleşme belgeleri nerede her öğenin `ratings` alan 3 ile 5, kapsamlı arasında döner:

    ratings/all(r: r ge 3 and r le 5)

Burada herhangi bir coğrafi koordinatları içinde eşleşen belgelerin `locations` içinde belirtilen Çokgen bir alandır:

    locations/any(loc: geo.intersects(loc, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))

Eşleşme belgeleri nerede `rooms` boş alan:

    not rooms/any()

Burada tüm odalar için eşleşen belgelerin `rooms/amenities` alan "tv" içerir ve `rooms/baseRate` 100'den az:

    rooms/all(room: room/amenities/any(a: a eq 'tv') and room/baseRate lt 100.0)

## <a name="limitations"></a>Sınırlamalar

Filtre ifadeleri her özellik, bir lambda ifadesinin gövdesi içinde kullanılabilir. Sınırlamalar, filtre uygulamak istediğiniz koleksiyon alanın veri türüne bağlı olarak farklılık gösterir. Aşağıdaki tabloda sınırlamaları özetlenmektedir.

[!INCLUDE [Limitations on OData lambda expressions in Azure Search](../../includes/search-query-odata-lambda-limitations.md)]

Bu sınırlamalar yanı sıra örnekleri hakkında daha fazla bilgi için bkz. [Azure Search'te toplama filtreleri sorun giderme](search-query-troubleshoot-collection-filters.md). Neden hakkında daha ayrıntılı bilgi için bu sınırlamalar var, bkz: [Azure Search'te toplama filtreleri anlama](search-query-understand-collection-filters.md).

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Search'te filtreler](search-filters.md)
- [Azure Search için OData ifade dili genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Search için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Search belgeleri &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
