---
title: Karmaşık veri türlerini modelleme - Azure Search hakkında
description: İç içe ya da hiyerarşik veri yapılarını ComplexType ve koleksiyonları veri türlerini kullanarak, Azure Search dizini modellenebilir.
author: brjohnstmsft
manager: jlembicz
ms.author: brjohnst
tags: complex data types; compound data types; aggregate data types
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/13/2019
ms.custom: seodec2018
ms.openlocfilehash: 10400b0342fbe8667b22fea82c6446713d019e0d
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65597342"
---
# <a name="how-to-model-complex-data-types-in-azure-search"></a>Azure Search karmaşık veri türlerini modelleme hakkında

Bazen bir Azure Search dizinini doldurmak için kullanılan dış veri kümeleri, hiyerarşik veya iç içe substructures içerir. Örnekleri tek bir müşteri, birden çok renkler ve boyutlar, tek bir kitap için birden çok yazarları gibi tek bir SKU için birden çok konumda ve telefon numaralarını içerir ve benzeri. Koşulları modelleme araçlarındaki olarak adlandırılan bu yapıları görebileceğiniz *karmaşık veri türlerini*, *bileşik veri türleri*, *bileşik veri türleri*, veya *toplama veri türleri*. Azure Search terminolojisinde, bir karmaşık tür alt öğe (alt alanlar) içeren bir basit veya karmaşık kendileri alandır. Benzer şekilde yapılandırılmış veri türü bir programlama dili budur. Karmaşık alanlar belge tek bir nesnede temsil eden tek alanlar veya bir koleksiyonu temsil eden bir nesne dizisi olabilir.

Azure arama, karmaşık türler ve Koleksiyonlar yerel olarak destekler. Birlikte, bu tür bir Azure Search dizini neredeyse tüm iç içe geçmiş JSON yapısındaki modeli sağlar. Azure arama API'lerinin önceki sürümlerinde, yalnızca satır kümeleri alınamadı düzleştirilmiş. En yeni sürümde dizininizi artık daha yakından kaynak verilere karşılık gelebilir. Diğer bir deyişle, veri kaynağınızı karmaşık türler varsa dizininizi karmaşık türler de olabilir.

Başlamak için önerilir [Hotels veri kümesi](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/README.md), içinde yük **verileri içeri aktarma** Azure portalındaki Sihirbazı. Sihirbaz, karmaşık türler kaynak olarak algılar ve algılanan yapıları tabanlı bir dizin şemasını önerir.

> [!Note]
> Karmaşık türler için destek sunulmuştur `api-version=2019-05-06`. 
>
> Arama çözümünüzü düzleştirilmiş veri kümesi bir koleksiyondaki önceki geçici çözümler üzerinde oluşturulursa, yeni API sürümünde desteklenen gibi karmaşık türler dahil etmek için dizininizi değiştirmeniz gerekir. API sürümleri yükseltme hakkında daha fazla bilgi için bkz. [en yeni REST API sürümüne yükseltme](search-api-migration.md) veya [en yeni .NET SDK'sı sürümüne yükseltin](search-dotnet-sdk-migration-version-9.md).

## <a name="example-of-a-complex-structure"></a>Karmaşık bir yapısı örneği

Basit ve karmaşık alanları aşağıdaki JSON belgesini oluşur. Gibi karmaşık alanları `Address` ve `Rooms`, alt alanlar içerir. `Address` Belge içindeki tek bir nesne olduğundan bu alt alanlar için değerler tek bir kümesi vardır. Buna karşılık, `Rooms` birden fazla kendi alt alanlar için değerler, her nesne için bir koleksiyonu vardır.

```json
{
  "HotelId": "1",
  "HotelName": "Secret Point Motel",
  "Description": "Ideally located on the main commercial artery of the city in the heart of New York.",
  "Address": {
    "StreetAddress": "677 5th Ave",
    "City": "New York",
    "StateProvince": "NY"
  },
  "Rooms": [
    {
      "Description": "Budget Room, 1 Queen Bed (Cityside)",
      "Type": "Budget Room",
      "BaseRate": 96.99,
    },
    {
      "Description": "Deluxe Room, 2 Double Beds (City View)",
      "Type": "Deluxe Room",
      "BaseRate": 150.99,
    },
  ]
}
```

## <a name="creating-complex-fields"></a>Karmaşık alanları oluşturma

Portal, kullanabileceğiniz herhangi bir dizin tanımını ile gibi [REST API](https://docs.microsoft.com/rest/api/searchservice/create-index), veya [.NET SDK'sı](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index?view=azure-dotnet) karmaşık türler içeren bir şema oluşturun. 

Aşağıdaki örnek bir JSON dizin şeması ile basit alanlar, koleksiyonlar ve karmaşık türler gösterir. Karmaşık bir tür içindeki her alt alan bir türe sahiptir ve özniteliklere sahip olabilir, yalnızca gibi üst düzey alanları dikkat edin. Şema, yukarıdaki örnek verilere karşılık gelir. `Address` (bir otel bir adresi olan) bir koleksiyon değil karmaşık bir alandır. `Rooms` (bir otel birçok odaları sahiptir) karmaşık bir toplama alandır.

<!---
For indexes used in a [push-model data import](search-what-is-data-import.md) strategy, where you are pushing a JSON data set to an Azure Search index, you can only have the basic syntax shown here: single complex types like `Address`, or a `Collection(Edm.ComplexType)` like `Rooms`. You cannot have complex types nested inside other complex types in an index used for push-model data ingestion.

Indexers are a different story. When defining an indexer, in particular one used to build a knowledge store, your index can have nested complex types. An indexer is able to hold a chain of complex data structures in-memory, and when it includes a skillset, it can support highly complex data forms. For more information and an example, see [How to get started with knowledge store](knowledge-store-howto.md).
-->

```json
{
  "name": "hotels",
  "fields": [
    { "name": "HotelId", "type": "Edm.String", "key": true, "filterable": true  },
    { "name": "HotelName", "type": "Edm.String", "searchable": true, "filterable": false },
    { "name": "Description", "type": "Edm.String", "searchable": true, "analyzer": "en.lucene" },
    { "name": "Address", "type": "Edm.ComplexType",
      "fields": [
        { "name": "StreetAddress", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "searchable": true },
        { "name": "City", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
        { "name": "StateProvince", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true }
      ]
    },
    { "name": "Rooms", "type": "Collection(Edm.ComplexType)",
      "fields": [
        { "name": "Description", "type": "Edm.String", "searchable": true, "analyzer": "en.lucene" },
        { "name": "Type", "type": "Edm.String", "searchable": true },
        { "name": "BaseRate", "type": "Edm.Double", "filterable": true, "facetable": true }
      ]
    }
  ]
}
```

## <a name="updating-complex-fields"></a>Karmaşık alanlar güncelleştiriliyor

Tüm [kuralları ölçeklemek](search-howto-reindex.md) geçerli alanlar, karmaşık alanlar için genel olarak hala geçerli. Birkaç Dinleyicilerinizden ana kuralları Burada, bir alan ekleme bir dizini yeniden oluşturma gerektirmez, ancak en son değişiklikleri yapın.

### <a name="structural-updates-to-the-definition"></a>Tanımına yapısal güncelleştirmeler

Bir dizini yeniden oluşturma gerek kalmadan herhangi bir zamanda karmaşık bir alan için yeni alt alanlar ekleyebilirsiniz. "ZipCode" Örneğin, ekleme `Address` veya için "s" `Rooms` izin, yalnızca bir dizine en üst düzey bir alan eklemeye benzer. Açıkça verilerinizi güncelleştirerek bu alanları doldurmak kadar varolan belgeleri yeni alanlar için null bir değer var.

Karmaşık bir tür içindeki her alt alan bir türe sahiptir ve özniteliklere sahip olabilir, yalnızca gibi üst düzey alanları dikkat edin.

### <a name="data-updates"></a>Veri güncelleştirmeleri

Bir dizinde varolan belgeleri karşıya yükleme eylemiyle güncelleştirme karmaşık ve basit alanlar için aynı şekilde çalışır; tüm alanları değiştirilir. Ancak, birleştirme (veya var olan bir belgeyi uygulandığında mergeOrUpload) aynı tüm alanlar genelinde çalışmaz. Özellikle, birleştirme, bir koleksiyondaki öğeleri birleştirme özelliği yok. Bu, ilkel türler, yanı sıra karmaşık koleksiyonları geçerlidir. Bir koleksiyonu güncellemek için tam koleksiyon değerini almak için değişiklikleri yapın ve ardından yeni bir koleksiyon dizin API isteğe ekleyin.


## <a name="searching-complex-fields"></a>Karmaşık arama alanları

Serbest biçimli arama ifadeleri, karmaşık türleri ile beklendiği gibi çalışmayabilir. Aranabilir alan ya da belgede herhangi bir alt alanı eşleşirse, belge bir eşleştirme olup. 

Daha fazla sorgu get, birden çok hüküm ve işleçler varsa ve bazı terimler mümkün olduğu gibi belirtilen alan adlarını sahip ince [Lucene sözdizimi](query-lucene-syntax.md). Örneğin, bu sorgu iki terim, "İstanbul" eşleştirmeye çalışır ve "OR" Adres alanının iki alt alanlara göre:

```json
search=Address/City:Portland AND Address/State:OR
```

Böyle sorgular için tam metin araması uncorrelated (filtreleri, aksine karmaşık bir koleksiyonun alt alanları üzerinde sorgular kullanarak ilişkili olabilir veya bütün bir bağıntılı alt sorguya SQL gibi). Bu, yukarıdaki Lucene sorgu "Portland, Maine" içeren belgeleri yanı sıra "Portland, Oregon" ve diğer şehirlerin Oregon döndürecekti anlamına gelir. "Geçerli alt belgeye" kavramı bu nedenle, her yan tümcesi tüm belgeyi belirtilen alanın tüm değerlerini karşı değerlendirilir olmasıdır. 

 

## <a name="selecting-complex-fields"></a>Karmaşık alanları seçme

`$select` Parametresi hangi alanların arama sonuçlarında döndürülen seçmek için kullanılır. Karmaşık bir alan belirli alt alanları seçmek için bu parametreyi kullanmak için üst alanı ve eğik çizgiyle ayrılmış alt alanı ekleyin (`/`).

```json
$select=HotelName, Address/City, Rooms/BaseRate
```

Arama sonuçlarında istiyorsanız alanları dizinde alınabilir işaretlenmelidir. Yalnızca alınabilir işaretlenmiş alanlar kullanılabilir bir `$select` deyimi. 


## <a name="filter-facet-and-sort-complex-fields"></a>Filtre, sıralama karmaşık alanlarını ve modeli

Aynı [OData yolu sözdizimi](query-odata-filter-orderby-syntax.md) filtreleme için kullanılan ve fielded aramaları da kullanılabilir modelleme, sıralama ve arama talebinde alanı seçmek için. Karmaşık türler için alt alanları olarak sıralanabilir olarak işaretlenebilir veya modellenebilir yöneten kurallar uygulanır. 

### <a name="faceting-sub-fields"></a>Alt alanlar model oluşturma 

Türü olmadığı sürece, herhangi bir alt alanı modellenebilir işaretlenebilir `Edm.GeographyPoint` veya `Collection(Edm.GeographyPoint)`. 

Belge sayısını çok yönlü gezinme yapısı için iade edildiğinde sayıları üst belge (bir otel), göreli bir karmaşık koleksiyonu (odaları) içindeki iç içe geçmiş belgeler için değildir. Örneğin, bir otel 20 odaları türü "paketi" olduğunu varsayalım. Bu facet parametresini verilen `facet=Rooms/Type`, model sayısı üst belge (Oteller) için bir tane olması ve alt belgeleri (odaları) Ara değil. 

### <a name="sorting-complex-fields"></a>Sıralama karmaşık alanları

Sıralama işlemi (Oteller) ve alt belgeler değil (odaları) için geçerlidir. Odaları gibi bir karmaşık tür koleksiyonu olduğunda odaları üzerinde hiç sıralayamazsınız hayata geçirmek önemlidir. Aslında, herhangi bir koleksiyon sıralama yapılamaz. 

Sıralama işlemi çalışma alanları tek değerli olduğunda basit bir alan olarak veya bir karmaşık tür alt alanı olarak. Örneğin, `$orderby=Address/ZipCode` otel başına yalnızca bir posta kodu olduğundan karmaşık türü sıralanabilir. 

Sıralama etrafında kuralları Dinleyicilerinizden, içinde bir dizin alanı olarak filtrelenebilir ve sıralanabilir şekilde işaretlenmelidir bir `$orderby` deyimi. 

## <a name="next-steps"></a>Sonraki adımlar

 Deneyin [Hotels veri kümesi](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/README.md) içinde **verileri içeri aktarma** Sihirbazı. Verilere erişmek için Benioku belgesinde sağlanan Cosmos DB bağlantı bilgileri gerekir. 
 
 El ile bu bilgileri kullanarak ilk adımınız sihirbazında yeni bir Azure Cosmos DB veri kaynağı oluşturmaktır. Hedef dizin sayfasına geldiğinizde daha fazla üzerinde Sihirbazı'nda, bir dizin ile karmaşık türler görürsünüz. Oluşturun ve bu dizin yükleme ve sonra yeni yapısını anlamak için sorgular yürütün.

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: içeri aktarma, dizin oluşturma ve sorgular portal Sihirbazı](search-get-started-portal.md)