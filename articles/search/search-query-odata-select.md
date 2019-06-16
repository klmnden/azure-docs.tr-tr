---
title: OData seçme başvuru - Azure Search
description: İçin OData dil başvurusu, Azure arama sorguları söz dizimi seçin.
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
ms.openlocfilehash: 9383ae725fffac55854488ffbc6aeb161ae7e0c2
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079684"
---
# <a name="odata-select-syntax-in-azure-search"></a>Azure Search'te OData $select söz dizimi

 Kullanabileceğiniz [OData **$select** parametre](query-odata-filter-orderby-syntax.md) hangi alanlar Azure Search arama sonuçları dahil etmek için. Bu makalede söz dizimi **$select** ayrıntılı. Nasıl kullanılacağı hakkında daha fazla genel bilgi için **$select** arama sonuçları sunma görürsünüz [sonuçları Azure Search'te arama ile çalışmaya nasıl](search-pagination-page-layout.md).

## <a name="syntax"></a>Sözdizimi

**$Select** parametresini, hangi alanların her belge için sorgu sonuç kümesinde döndürülen belirler. Aşağıdaki EBNF ([genişletilmiş Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) için dilbilgisine tanımlar **$select** parametresi:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
select_expression ::= '*' | field_path(',' field_path)*

field_path ::= identifier('/'identifier)*
```

Bir etkileşimli söz dizim diyagramı görülmektedir de kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Search için OData söz dizimini diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#select_expression)

> [!NOTE]
> Bkz: [OData ifadesi söz dizimi başvurusu için Azure Search](search-query-odata-syntax-reference.md) tam EBNF için.

**$Select** parametre iki biçimde gelir:

1. Tek bir yıldız (`*`), tüm getirilebilir alanlar döndürülmelidir, gösteren veya
1. Alan yolları virgülle ayrılmış listesi, hangi alanların tanımlayan döndürülmelidir.

İkinci form kullanırken, yalnızca listede getirilebilir alanlar belirtebilirsiniz.

Alt alanları açıkça belirtmeden karmaşık bir alan listesi sorgu sonuç kümesinde alınabilir tüm alt alanları dahil edilir. Örneğin, dizininizin olduğunu varsayalım bir `Address` alanına `Street`, `City`, ve `Country` alınabilir tüm alt alan. Belirtirseniz `Address` içinde **$select**, sorgu sonuçları tüm üç alt alanları dahil edilir.

## <a name="examples"></a>Örnekler

Dahil `HotelId`, `HotelName`, ve `Rating` sonuçlarında, üst düzey alanları yanı sıra `City` alt alanı `Address`:

    $select=HotelId, HotelName, Rating, Address/City

Bir örnek sonuç şuna benzeyebilir:

```json
{
  "HotelId": "1",
  "HotelName": "Secret Point Motel",
  "Rating": 4,
  "Address": {
    "City": "New York"
  }
}
```

Dahil `HotelName` tüm alt alanlarının yanı sıra sonuçları en üst düzey alanında `Address`ve `Type` ve `BaseRate` her bir nesnenin alt alanlar `Rooms` koleksiyonu:

    $select=HotelName, Address, Rooms/Type, Rooms/BaseRate

Bir örnek sonuç şuna benzeyebilir:

```json
{
  "HotelName": "Secret Point Motel",
  "Rating": 4,
  "Address": {
    "StreetAddress": "677 5th Ave",
    "City": "New York",
    "StateProvince": "NY",
    "Country": "USA",
    "PostalCode": "10022"
  },
  "Rooms": [
    {
      "Type": "Budget Room",
      "BaseRate": 9.69
    },
    {
      "Type": "Budget Room",
      "BaseRate": 8.09
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Search'te sonuçlarını arama ile çalışma](search-pagination-page-layout.md)
- [Azure Search için OData ifade dili genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Search için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Search belgeleri &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
