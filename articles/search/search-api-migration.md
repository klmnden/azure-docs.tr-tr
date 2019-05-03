---
title: En son Azure arama hizmeti REST API'si sürüme - Azure Search yükseltin
description: API sürümleri farklılıkları gözden geçirin ve hangi işlemlerin en yeni Azure Search Hizmeti REST API sürümü için mevcut kodu geçirilmesi için gereken bilgi edinin.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: brjohnst
ms.openlocfilehash: 286d8bbc01b5916e842c196aed5a49ef1c76bc3c
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025194"
---
# <a name="upgrade-to-the-latest-azure-search-service-rest-api-version"></a>En son Azure Search Hizmeti REST API sürümüne yükseltme
Önceki bir sürümünü kullanıyorsanız, [Azure arama hizmeti REST API'si](https://docs.microsoft.com/rest/api/searchservice/), bu makalede 2019-05-06 en sunuldu API sürümünü kullanmak için uygulamanızı yükseltmenize yardımcı olur.

REST API sürümü 2019-05-06 bazı değişiklikler daha önceki sürümlerin içerir. Bu çoğunlukla geriye dönük uyumlu yayımlanır; dolayısıyla, kod değiştirme önce kullandığınız bağlı olarak hangi sürümün yalnızca en az çaba istemeniz gerekir. [Yükseltme adımları](#UpgradeSteps) yeni özellikleri kullanmak için gereken kod değişikliklerini açıklar.

> [!NOTE]
> Bir Azure Search Hizmeti örneğinin öncekileri dahil olmak üzere bir REST API aralığı sürümleri destekler. Bu API sürümlerini kullanmaya devam edebilirsiniz, ancak yeni özellikler erişebilmesi için kodunuzu en yeni sürüme geçirme öneririz.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2019-05-06"></a>2019-05-06 sürümünde yenilikler nelerdir?
Sürüm 2019-05-06 en yeni genel kullanıma sunulan Azure arama hizmeti REST API'si sürümüdür. Bu API sürümünde genel olarak kullanılabilir duruma geçti özellikler şunlardır:

* [Otomatik Tamamlama](index-add-suggesters.md) bir kısmen belirtilen terim giriş tamamlayan bir typeahead özelliğidir.

* [Karmaşık türler](search-howto-complex-data-types.md) yapılandırılmış nesne verileri Azure Search dizini için yerel destek sağlar.

* [Ayrıştırma modları JsonLines](search-howto-index-json-blobs.md), parçası Azure Blob dizin oluşturma, bir arama belgeyi bir satır başı karakteri tarafından ayrılmış JSON varlık başına oluşturur.

* [Bilişsel arama](cognitive-search-concept-intro.md) Bilişsel hizmetler, yapay ZEKA zenginleştirme altyapıları yararlanan bağımsız dizinlemesini sağlar.

Birkaç Önizleme özellik sürümlerine genel kullanıma sunulan bu güncelleştirme ile çakışacak. Yeni Önizleme özellikleri listesini gözden geçirmek için bkz: [arama REST API sürümü 2019-05-06-Preview](search-api-preview.md).

## <a name="breaking-changes"></a>Yeni değişiklikler

Varolan kodu aşağıdaki işlevler içeren, api-version üzerinde bozar 2019-05-06 =.

### <a name="indexer-for-azure-cosmos-db---datasource-is-now-type-cosmosdb"></a>Azure Cosmos DB için-dizin oluşturucu veri kaynağı olan artık "type": "cosmosdb"

Kullanıyorsanız bir [Cosmos DB dizinleyici](search-howto-index-cosmosdb.md ), bulacağınızı biliyorsanız `"type": "documentdb"` için `"type": "cosmosdb"`.

### <a name="indexer-execution-result-errors-no-longer-have-status"></a>Dizin Oluşturucu yürütme sonucu hataları artık durumuna sahip

Dizin Oluşturucu yürütme hatası yapısını vardı bir `status` öğesi. Yararlı bilgiler sağlayan değil çünkü bu öğe kaldırıldı.

### <a name="indexer-data-source-api-no-longer-returns-connection-strings"></a>Dizin Oluşturucu veri kaynağı API artık bağlantı dizelerini döndürür

API'SİNDEN 2019-05-06 ve 2019-05-06-Preview ve sonraki sürümlerde, API veri kaynağının sürümleri artık döndürür bağlantı dizelerini içinde herhangi bir REST işlem yanıtı. Önceki API sürümlerinde, POST, kullanılarak oluşturulan veri kaynakları için Azure Search döndürülen **201** bağlantı dizesi düz metin içinde yer alan OData yanıtı ardından.

### <a name="named-entity-recognition-cognitive-skill-is-now-discontinued"></a>Adlandırılmış varlık tanıma bilişsel beceri artık kullanımdan kaldırıldı

Eğer [adı varlık tanıma](cognitive-search-skill-named-entity-recognition.md) beceri kodunuzda çağrı başarısız olur. Değiştirme işlevselliği [varlık tanıma](cognitive-search-skill-entity-recognition.md). Beceri başvuru ile başka bir değişiklik değiştirin olması gerekir. API imza iki sürümü de aynı değil. 

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Yükseltme adımları
Bir önceki GA sürümden yükseltiyorsanız, 2017-11-11 veya 2016-09-01, büyük olasılıkla kodunuza dışında sürüm numarasını değiştirmek için değişiklik gerekmez. Kodu değiştirmek gerekebilir yalnızca durumlar ne zaman şunlardır:

* Tanınmayan özellikleri içinde bir API yanıt döndürüldüğünde kod başarısız olur. Varsayılan olarak, uygulamanızın anlamadığı özellikleri yok saymanız gerekir.

* Kodunuzu API isteklerinin devam ediyorsa ve bunları yeni API sürümüne yeniden dener. Uygulamanızı arama API'den döndürülen devamlılık belirteçleri devam ederse, örneğin, bu gerçekleşebilir (daha fazla bilgi için Ara `@search.nextPageParameters` içinde [arama API'si başvurusu](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)).

Bu durumlarda ya da sizin için geçerliyse, kodunuzu buna göre değiştirmeniz gerekebilir. Kullanmaya başlamak istemediğiniz sürece Aksi takdirde, hiçbir değişiklik gerekli olmamalıdır [yeni özellikler](#WhatsNew) 2019-05-06 sürümü.

Bir önizleme API sürümüne yükseltme yapıyorsanız, yukarıdaki de geçerlidir, ancak ayrıca bazı Önizleme özellikleri 2019-05-06 sürümünde mevcut olmayan farkında olmalıdır:

* ["Daha böyle" sorguları](search-more-like-this.md) yalnızca önizleme özelliği devam etmektedir.

Kodunuzu bu özellikleri kullanıyorsa, bunların kullanım kaldırmadan API sürümü 2019-05-06 yükseltmek mümkün olmayacaktır.

> [!IMPORTANT]
> Önizleme API'leri, test ve değerlendirme içindir ve üretim ortamlarında kullanılmamalıdır.
> 

### <a name="upgrading-complex-types"></a>Karmaşık türler yükseltme

Kodunuzu eski Önizleme ile API sürümlerini 2017-11-11-Preview veya 2016-09-01-Preview karmaşık türleri kullanıyorsa, 2019-05-06 dikkat etmeniz gerekir sürümünde yeni ve değiştirilen bazı sınırlamalar vardır:

+ Alt alanları ve dizin başına karmaşık koleksiyon sayısına derinliğini barındırabileceğiniz düşürdü. Önizleme API sürümlerini kullanıp bu limitlerin dizinleri oluşturduysanız, güncelleştirme veya bunları yeniden yapmaya API sürümü 2019-05-06 kullanarak başarısız olur. Bu sizin için geçerliyse, yeni sınırları içinde uygun ve ardından dizininizi yeniden şemanızı yeniden tasarlamanız gerekir.

+ Api sürümü 2019-05-06 belge başına karmaşık koleksiyonu öğelerinin sayısı yeni sınırı yoktur. Dizinleri limitlerin Önizleme API sürümlerini kullanıp bu belgelerle oluşturduysanız, api sürümü 2019-05-06 kullanarak bu verileri yeniden dizin oluşturma denemesi başarısız olur. Bu sizin için geçerliyse, önce verilerinizi ölçeklemek belge başına karmaşık koleksiyon öğelerinin sayısını azaltmak gerekir.

Daha fazla bilgi için [için Azure Search hizmet sınırları](search-limits-quotas-capacity.md).

### <a name="how-to-upgrade-an-old-complex-type-structure"></a>Eski bir karmaşık tür yapı yükseltme

Kodunuzu eski Önizleme API sürümlerinden birini karmaşık türler kullanan, şuna benzer bir dizin tanımı biçimi kullanarak:

```json
{
  "name": "hotels",  
  "fields": [
    { "name": "HotelId", "type": "Edm.String", "key": true, "filterable": true },
    { "name": "HotelName", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": true, "facetable": false },
    { "name": "Description", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "en.microsoft" },
    { "name": "Description_fr", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.microsoft" },
    { "name": "Category", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
    { "name": "Tags", "type": "Collection(Edm.String)", "searchable": true, "filterable": true, "sortable": false, "facetable": true, "analyzer": "tagsAnalyzer" },
    { "name": "ParkingIncluded", "type": "Edm.Boolean", "filterable": true, "sortable": true, "facetable": true },
    { "name": "LastRenovationDate", "type": "Edm.DateTimeOffset", "filterable": true, "sortable": true, "facetable": true },
    { "name": "Rating", "type": "Edm.Double", "filterable": true, "sortable": true, "facetable": true },
    { "name": "Address", "type": "Edm.ComplexType" },
    { "name": "Address/StreetAddress", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "searchable": true },
    { "name": "Address/City", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
    { "name": "Address/StateProvince", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
    { "name": "Address/PostalCode", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
    { "name": "Address/Country", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
    { "name": "Location", "type": "Edm.GeographyPoint", "filterable": true, "sortable": true },
    { "name": "Rooms", "type": "Collection(Edm.ComplexType)" }, 
    { "name": "Rooms/Description", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "en.lucene" },
    { "name": "Rooms/Description_fr", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene" },
    { "name": "Rooms/Type", "type": "Edm.String", "searchable": true },
    { "name": "Rooms/BaseRate", "type": "Edm.Double", "filterable": true, "facetable": true },
    { "name": "Rooms/BedOptions", "type": "Edm.String", "searchable": true },
    { "name": "Rooms/SleepsCount", "type": "Edm.Int32", "filterable": true, "facetable": true },
    { "name": "Rooms/SmokingAllowed", "type": "Edm.Boolean", "filterable": true, "facetable": true },
    { "name": "Rooms/Tags", "type": "Collection(Edm.String)", "searchable": true, "filterable": true, "facetable": true, "analyzer": "tagsAnalyzer" }
  ]
}  
```

Dizin alanları tanımlamak için yeni bir ağaç benzer biçimde API Sürüm 2017-11-11-Preview sürümünde kullanıma sunulmuştur. Yeni biçiminde her karmaşık bir alan, alt alanlarından tanımlandığı bir alanlar koleksiyonu vardır. API sürümü 2019-05-06, bu yeni biçim özel olarak kullanılır ve oluşturma veya eski biçimi kullanarak bir dizin güncelleştirme girişimi başarısız olur. Eski biçimi kullanılarak oluşturulan dizinleri varsa, API sürümü 2019-05-06 kullanılarak yönetilebilir önce bunları yeni biçime güncelleştirmeniz API Sürüm 2017-11-11-Preview'ı kullanmak gerekir.

API Sürüm 2017-11-11-Preview'ı kullanarak aşağıdaki adımlarla yeni biçime "Düz" dizinleri güncelleştirebilirsiniz:

1. Dizininizi almak için bir GET isteği gerçekleştirir. Zaten bir yeni biçiminde olması durumunda, hazırsınız.

2. "Düz" biçim dizinden yeni biçimine çevirir. Olduğundan örnek kod kullanılabilir bu makalenin yazıldığı sırada bu kod yazmak zorunda kalırsınız.

3. Yeni biçime dizini güncelleştirecek bir PUT isteği gerçekleştirir. Bu güncelleştirme dizin API tarafından izin verilmiyor bu yana herhangi bir dizin alanları aranabilirliğini/filterability gibi ayrıntılarını değil değiştirdiğinizden emin olun.

> [!NOTE]
> Eski "Düz" biçiminde Azure portalından oluşturulan dizinleri yönetmek mümkün değildir. Lütfen dizinlerinizi "Düz" gösterimden en yakın zamanda "ağacı" gösterimine yükseltin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Search Hizmeti REST API başvuru belgelerini gözden geçirin. Sorunlarla karşılaşırsanız, bize yardım üzerinde isteyin [StackOverflow](https://stackoverflow.com/) veya [desteğe](https://azure.microsoft.com/support/community/?product=search).

> [!div class="nextstepaction"]
> [Arama hizmeti REST API Başvurusu](https://docs.microsoft.com/rest/api/searchservice/)

