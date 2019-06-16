---
title: OData koleksiyon filtreleri - Azure Search anlama
description: OData koleksiyon filtreleri Azure arama sorguları nasıl çalıştığını anlama.
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
ms.openlocfilehash: 7af1b0ab95d04249d6d74e3324dbeb30eda6e1de
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079606"
---
# <a name="understanding-odata-collection-filters-in-azure-search"></a>Azure Search'te OData toplama filtreleri anlama

İçin [filtre](query-odata-filter-orderby-syntax.md) Azure Search'te toplama alanlar üzerinde kullanabileceğiniz [ `any` ve `all` işleçleri](search-query-odata-collection-operators.md) ile birlikte **lambda ifadeleri**. Lambda ifadeleri başvuran Boolean ifadeler olan bir **aralık değişkeni**. `any` Ve `all` işleçleridir benzer bir `for` döngü, döngü değişkeninin ve döngü gövdesi lambda ifadesiyle rolünü alma olan Aralık değişkeniyle çoğu programlama dillerinde. Aralık değişkeni koleksiyonu yineleme döngüsü sırasında "mevcut" değerini alır.

En az olan kavramsal olarak şekli at. Gerçekte, Azure Search çok farklı bir şekilde nasıl filtreler uygulayan `for` iş döngüsü. İdeal olarak, bu fark için görünmez, ancak belirli durumlarda bu değildir. Sonuç, lambda ifadeleri yazarken izlemek zorunda kural yoktur.

Bu makalede, neden toplama filtreleri için kuralları nasıl Azure Search bu filtreler çalıştırır inceleyerek mevcut açıklanmaktadır. Gelişmiş Filtreler karmaşık lambda ifadeleri ile yazıyorsanız, bu makalede filtreleri mümkündür, Anlayışınızı oluşturmanın faydalı bulabilirsiniz ve neden.

Kuralları örnekler de dahil olmak üzere toplama filtreleri şunlardır bkz bilgi [sorun giderme OData toplama filtreleri Azure Search'te](search-query-troubleshoot-collection-filters.md).

## <a name="why-collection-filters-are-limited"></a>Neden toplama filtreleri sınırlıdır

Koleksiyonlar tüm türleri için desteklenen tüm filtre özellikler neden üç temel neden şunlardır:

1. Yalnızca belirli işleçler, belirli veri türleri için desteklenir. Örneğin, bu Boolean değerleri karşılaştırmak için doesn't make Sense `true` ve `false` kullanarak `lt`, `gt`ve benzeri.
1. Azure Search'ü desteklemez **bağıntılı arama** türünde alanlar üzerinde `Collection(Edm.ComplexType)`.
1. Azure Search kullanan filtreler tüm koleksiyonları dahil olmak üzere, veri türleri üzerinde yürütmek için dizinleri ters.

İlk neden EDM türü sistem ve OData dil nasıl tanımlandığını, yalnızca bir sonucu var. Son iki, bu makalenin geri kalanında daha ayrıntılı açıklanmıştır.

## <a name="correlated-versus-uncorrelated-search"></a>Bağıntılı uncorrelated arama

Birden çok filtre ölçütlerini karmaşık nesne koleksiyonu uygularken ölçütlerdir **bağıntılı** uygulandıkları beri *koleksiyondaki her bir nesnenin*. Örneğin, aşağıdaki filtre 100'den daha az bir ücret en az bir lüks odada hotels döndürür:

    Rooms/any(room: room/Type eq 'Deluxe Room' and room/BaseRate lt 100)

Filtreleme olduysa *uncorrelated*, yukarıdaki filtre, burada bir oda haline deluxe ve farklı bir yer olan temel hotels Oranı 100'den küçük döndürebilir. Mıydı mantıklı, her iki yan tümceleri lambda ifadesinin yani aynı aralık değişkeni için geçerli olduğundan `room`. Bu tür filtreleri bağıntılı nedeni budur.

Ancak, tam metin araması için belirli bir aralık değişkenine başvurmak için hiçbir yolu yoktur. Verilecek fielded arama kullanıyorsanız bir [tam Lucene sorgu](query-lucene-syntax.md) bunu ister:

    Rooms/Type:deluxe AND Rooms/Description:"city view"

bir açıklama burada bir oda deluxe ve "Şehir görünümü" farklı bir oda bahsetmeleri hotels geri alabilirsiniz. Örneğin, aşağıdaki belge ile `Id` , `1` sorgu eşleşir:

```json
{
  "value": [
    {
      "Id": "1",
      "Rooms": [
        { "Type": "deluxe", "Description": "Large garden view suite" },
        { "Type": "standard", "Description": "Standard city view room" }
      ]
    },
    {
      "Id": "2",
      "Rooms": [
        { "Type": "deluxe", "Description": "Courtyard motel room" }
      ]
    }
  ]
}
```

Neden olan `Rooms/Type` başvuran tüm çözümlenen koşullarını `Rooms/Type` tüm belge ve benzer şekilde için alan `Rooms/Description`aşağıdaki tabloda gösterildiği gibi.

Nasıl `Rooms/Type` tam metin araması için depolanır:

| Terim `Rooms/Type` | Belge Kimliği |
| --- | --- |
| Deluxe | 1, 2 |
| standart | 1 |

Nasıl `Rooms/Description` tam metin araması için depolanır:

| Terim `Rooms/Description` | Belge Kimliği |
| --- | --- |
| courtyard | 2 |
| city | 1 |
| bahçesi | 1 |
| Büyük | 1 |
| motel | 2 |
| Oda | 1, 2 |
| standart | 1 |
| Paketi | 1 |
| görünüm | 1 |

Bu nedenle yukarıdaki filtre, temelde diyor "eşleşen bir oda sahip olduğu belgeleri `Type` 'Deluxe odası' eşittir ve **, aynı odada** sahip `BaseRate` 100", arama sorgu "eşleşme belgeleri nerede `Rooms/Type`"deluxe" bir dönemi kapsar ve `Rooms/Description` "Şehir görünümü" deyimi vardır. İkinci durumda, alanlar eşleştirilebilir bireysel odaları kavramı yoktur.

> [!NOTE]
> Görmek istiyorsanız, Azure Search için eklenen bağıntılı arama desteği, lütfen oylayın [bu Uservoice öğesi](https://feedback.azure.com/forums/263029-azure-search/suggestions/37735060-support-correlated-search-on-complex-collections).

## <a name="inverted-indexes-and-collections"></a>Ters dizinleri ve koleksiyonları

İster basit koleksiyonları için daha çok daha az kısıtlamalar lambda ifadeleri karmaşık koleksiyonlar üzerinden olduğunu fark etmiş olabilirsiniz `Collection(Edm.Int32)`, `Collection(Edm.GeographyPoint)`ve benzeri. Basit koleksiyonların koleksiyon olarak hiç depolanmaz, ancak Azure Search karmaşık koleksiyonları gerçek alt belge koleksiyonları depolar. olmasıdır.

Örneğin, filtrelenebilir dize koleksiyonu alanı gibi düşünün `seasons` bir çevrimiçi satış şirketi için dizin içinde. Bu dizin için karşıya yüklenen bazı belgeler şuna benzeyebilir:

```json
{
  "value": [
    {
      "id": "1",
      "name": "Hiking boots",
      "seasons": ["spring", "summer", "fall"]
    },
    {
      "id": "2",
      "name": "Rain jacket",
      "seasons": ["spring", "fall", "winter"]
    },
    {
      "id": "3",
      "name": "Parka",
      "seasons": ["winter"]
    }
  ]
}
```

Değerlerini `seasons` alan adı verilen bir yapıda depolanan bir **dizin ters**, aşağıdakine benzer aradığı:

| Terim | Belge Kimliği |
| --- | --- |
| Yay | 1, 2 |
| Yaz | 1 |
| Kalan | 1, 2 |
| Kış | 2, 3 |

Bu veri yapısını, büyük bir hızla ile bir soruyu yanıtlamak için tasarlanmıştır: Belirli bir dönem içinde hangi belgelerin görünüyor mu? Bu soruyu yanıtlarken daha gibi bir düz eşitlik kontrolüne göre bir döngü bir koleksiyon üzerinde çalışır. Aslında, bu neden dize koleksiyonları, Azure Search yalnızca sağlar, `eq` içinde bir lambda ifadesi için karşılaştırma işleci olarak `any`.

Eşitlik oluşturmayı, sonraki nasıl, aynı aralık değişkeniyle birden çok eşitlik denetimleri birleştirmek mümkündür göz atacağız `or`. Cebir sayesinde çalışır ve [miktar belirleyiciler dağıtarak özelliğini](https://en.wikipedia.org/wiki/Existential_quantification#Negation). Bu ifade:

    seasons/any(s: s eq 'winter' or s eq 'fall')

eşdeğerdir:

    seasons/any(s: s eq 'winter') or seasons/any(s: s eq 'fall')

ve her iki `any` alt ifadeler verimli bir şekilde yürütülebilir ters dizini kullanma. Ayrıca, performanstan için [miktar belirleyiciler olumsuzlama Kanunu](https://en.wikipedia.org/wiki/Existential_quantification#Negation), bu ifade:

    seasons/all(s: s ne 'winter' and s ne 'fall')

eşdeğerdir:

    not seasons/any(s: s eq 'winter' or s eq 'fall')

kullanmak mümkün mü neden olduğu `all` ile `ne` ve `and`.

> [!NOTE]
> Ayrıntılar bu belgenin kapsamı dışında olsa da, bu aynı ilkeler uzatmak [Jeo-uzamsal noktaları koleksiyonları için uzaklık ve kesişimi testleri](search-query-odata-geo-spatial-functions.md) de. Bu nedenle, bileşenidir `any`:
>
> - `geo.intersects` negatif olamaz
> - `geo.distance` kullanarak karşılaştırıldığında gerekir `lt` veya `le`
> - ifadeler birlikte, ile `or`değil `and`
>
> Ters kurallar için geçerli `all`.

Çok çeşitli ifadeleri izin verilen koleksiyon üzerinde veri filtreleme desteği yazdığında `lt`, `gt`, `le`, ve `ge` işleçler gibi `Collection(Edm.Int32)` örneğin. Özellikle kullanabilirsiniz `and` yanı `or` içinde `any`, temel alınan karşılaştırma ifadeleri birleştirilir sürece **aralığı karşılaştırmalar** kullanarak `and`, hangi sonra daha fazla kullanılarak birleştirilen `or`. Boolean ifadeleri, bu yapı adlı [ayırt edici Normal Form (DNF)](https://en.wikipedia.org/wiki/Disjunctive_normal_form), aksi takdirde "ORs, Equal" bilinir. Buna karşılık, lambda ifadeleri için `all` türleri olmalıdır bu veriler için [Conjunctive Normal Form (CNF)](https://en.wikipedia.org/wiki/Conjunctive_normal_form), aksi takdirde "Equal biri OR"özniteliklerine bilinen. Azure arama, bunları yürütebilir çünkü tür aralığı karşılaştırmaları sağlar kullanarak ters dizinleri verimli bir şekilde yalnızca yapabildiğiniz gibi dizeler için hızlı terim araması.

Özet olarak, kural için bir lambda ifadesinde izin verilenden karşısında şunlardır:

- İçinde `any`, *pozitif denetimleri* her zaman, eşitlik gibi aralığı karşılaştırmalar izin `geo.intersects`, veya `geo.distance` ile karşılaştırıldığında `lt` veya `le` (düşünün "gibi olarak eşleşme" uzaklık denetimini geldiğinde eşitlik).
- İçinde `any`, `or` her zaman izin verilir. Kullanabileceğiniz `and` Equal ORs (DNF) yalnızca aralığı denetimleri ve yalnızca ifade edebilirsiniz veri türleri için kullanın.
- İçinde `all`, kuralları--yalnızca tersine *negatif denetimleri* izin verilen, kullanabileceğiniz `and` her zaman kullanabileceğiniz `or` kaldırmalı, ORs (CNF) olarak ifade edilen aralık denetimleri için yalnızca.

Uygulamada, bunlar yine de kullanmak en olası filtreleri türleridir. Olasılıklara ancak sınırları anlamak yine de yararlıdır.

Hangi tür filtre izin verildiğini ve hangi olmayan belirli örnekleri için bkz: [geçerli koleksiyon filtreleri yazmak nasıl](search-query-troubleshoot-collection-filters.md#bkmk_examples).

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Search'te OData toplama filtreleri sorunlarını giderme](search-query-troubleshoot-collection-filters.md)
- [Azure Search'te filtreler](search-filters.md)
- [Azure Search için OData ifade dili genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Search için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Search belgeleri &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
