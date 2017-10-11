---
title: "Azure Search Hizmeti REST API sürümü 2015-02-28-Önizleme | Microsoft Docs"
description: "Azure Search Hizmeti REST API sürümü 2015-02-28-Önizleme doğal dil Çözümleyicileri ve moreLikeThis aramaları gibi Deneysel özellikler içerir."
services: search
documentationcenter: na
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 3dba3bf8-9c83-42f6-82bc-04727bd11037
ms.service: search
ms.devlang: rest-api
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 05/01/2017
ms.author: brjohnst
ms.openlocfilehash: e6ad5c964bfa8421be2706cb4015980e01a271b7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="azure-search-service-rest-api-version-2015-02-28-preview"></a>Azure Search Hizmeti REST API'si: Sürüm 2015-02-28-Önizleme
Bu makale için başvuru belgesidir `api-version=2015-02-28-Preview`. Bu önizleme geçerli genel olarak kullanılabilir sürüm genişletir [api-version = 2015-02-28](https://msdn.microsoft.com/library/dn798935.aspx), aşağıdaki Deneysel özellikleri sağlayarak:

* `moreLikeThis`sorgu parametresi olarak [Search belgeleri](#SearchDocs) API. Başka bir özel belge için uygun olan diğer belgeleri bulur.

Birkaç ek bölümlerini `2015-02-28-Preview` REST API ayrı olarak belgelenmiştir. Bunlar:

* [Puanlama profili](search-api-scoring-profiles-2015-02-28-preview.md)
* [Dizin Oluşturucular](search-api-indexers-2015-02-28-preview.md)

Azure Search hizmeti birden çok sürümlerinde kullanılabilir. Lütfen [arama hizmet sürümü oluşturma](http://msdn.microsoft.com/library/azure/dn864560.aspx) Ayrıntılar için.

## <a name="apis-in-this-document"></a>Bu belgedeki API'leri
Azure Search Hizmeti API'si API işlemleri için iki URL sözdizimleri destekler: Basit ve OData (bkz [OData (Azure Search API) için destek](http://msdn.microsoft.com/library/azure/dn798932.aspx) Ayrıntılar için). Aşağıdaki listede basit sözdizimi gösterilmektedir.

[Dizin oluşturma](#CreateIndex)

    POST /indexes?api-version=2015-02-28-Preview

[Dizini Güncelleştir](#UpdateIndex)

    PUT /indexes/[index name]?api-version=2015-02-28-Preview

[Dizin Al](#GetIndex)

    GET /indexes/[index name]?api-version=2015-02-28-Preview

[Dizinleri listeleme](#ListIndexes)

    GET /indexes?api-version=2015-02-28-Preview

[Dizin istatistikleri Al](#GetIndexStats)

    GET /indexes/[index name]/stats?api-version=2015-02-28-Preview

[Testi Çözümleyicisi](#TestAnalyzer)

    POST /indexes/[index name]/analyze?api-version=2015-02-28-Preview

[Dizin silme](#DeleteIndex)

    DELETE /indexes/[index name]?api-version=2015-02-28-Preview

[Ekleme, silme ve dizin içindeki verileri güncelleştirme](#AddOrUpdateDocuments)

    POST /indexes/[index name]/docs/index?api-version=2015-02-28-Preview

[Belge ara](#SearchDocs)

    GET /indexes/[index name]/docs?[query parameters]
    POST /indexes/[index name]/docs/search?api-version=2015-02-28-Preview

[Arama belge](#LookupAPI)

     GET /indexes/[index name]/docs/[key]?[query parameters]

[Count belgeleri](#CountDocs)

    GET /indexes/[index name]/docs/$count?api-version=2015-02-28-Preview

[Öneriler](#Suggestions)

    GET /indexes/[index name]/docs/suggest?[query parameters]
    POST /indexes/[index name]/docs/suggest?api-version=2015-02-28-Preview

- - -
<a name="IndexOps"></a>

## <a name="index-operations"></a>Dizin işlemleri
Oluşturun ve Azure Search hizmeti belirtilen dizin kaynak basit HTTP isteklerini (POST, GET, PUT, DELETE) aracılığıyla dizinlerde yönetin. Dizin oluşturmak için önce dizin şemasını tanımlayan bir JSON belgesi gönderin. Şema dizini, kendi veri türleri ve nasıl (örneğin, tam metin araması, filtre, sıralama veya olduğunu) kullanılabilmesi için alan tanımlar. Ayrıca dizin davranışını yapılandırmak için Puanlama profilleri, ilgili ve diğer öznitelikleri tanımlar.

Aşağıdaki örnek, bir çizimi iki dillerde tanımlanan açıklama alanı ile otel bilgi aramak için kullanılan bir şema sağlar. Öznitelikler alanın nasıl kullanılacağına nasıl kontrol dikkat edin. Örneğin `hotelId` belge anahtarı olarak kullanılan (`"key": true`) ve tam metin aramalardan dışlandı (`"searchable": false`).

    {
    "name": "hotels",  
    "fields": [
      {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
      {"name": "baseRate", "type": "Edm.Double"},
      {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
      {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
      {"name": "hotelName", "type": "Edm.String"},
      {"name": "category", "type": "Edm.String"},
      {"name": "tags", "type": "Collection(Edm.String)"},
      {"name": "parkingIncluded", "type": "Edm.Boolean"},
      {"name": "smokingAllowed", "type": "Edm.Boolean"},
      {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
      {"name": "rating", "type": "Edm.Int32"},
      {"name": "location", "type": "Edm.GeographyPoint"}
     ],
     "suggesters": [
      {
       "name": "sg",
       "searchMode": "analyzingInfixMatching",
       "sourceFields": ["hotelName"]
      }
     ]
    }

Dizin oluşturulduktan sonra dizinini doldurmak belgeleri yükleyeceksiniz. Bkz: [ekleme veya güncelleştirme belgeler](#AddOrUpdateDocuments) bu sonraki adım için.

Azure Search'te dizin oluşturma için video giriş için bkz [kanal 9 bulut kapak bölüm Azure Search üzerinde](http://go.microsoft.com/fwlink/p/?LinkId=511509).

<a name="CreateIndex"></a>

## <a name="create-index"></a>Dizin Oluşturma
Bir dizin, düzenleme ve belgeler Azure arama, bir tablo veritabanındaki kayıtları nasıl düzenler için benzer arama birincil yoludur. Her dizin tüm (alan adları, veri türleri ve özellikleri) dizin şemasıyla uyumlu, ancak dizinler de diğer arama davranışları tanımlamak ek yapıları (ilgili, Puanlama profilleri ve CORS seçenekleri) belirtmeniz belgeleri koleksiyonu vardır.

Bir HTTP POST veya PUT İsteği kullanarak Azure Search hizmeti içinde yeni bir dizin oluşturabilirsiniz. İstek gövdesine dizin ve yapılandırma bilgileri belirten bir JSON Şeması ' dir.

    POST https://[service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Alternatif olarak, PUT kullanın ve URI üzerinde dizin adı belirtin. Dizin yoksa, oluşturulur.

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]

Dizin oluşturma depolanır ve arama işlemlerinin kullanılan belgelerinin yapısını belirler. Dizin doldurma ayrı bir işlemdir. Bu adım için kullanabileceğiniz bir [dizin oluşturucu](https://msdn.microsoft.com/library/azure/mt183328.aspx) (desteklenen veri kaynakları için kullanılabilir) veya bir [Ekle, güncelleştirme veya silme belgeleri](https://msdn.microsoft.com/library/azure/dn798930.aspx) işlemi. Belgeleri gönderilen ters dizin oluşturulur.

**Not**: fiyatlandırma katmanı tarafından dizinleri izin verilen maksimum sayısı değişir. Ücretsiz hizmeti 3 adede kadar dizinler sağlar. Standart hizmeti arama hizmeti başına 50 dizinleri sağlar. Bkz: [sınırları ve kısıtlamaları](http://msdn.microsoft.com/library/azure/dn798934.aspx) Ayrıntılar için.

**İstek**

HTTPS tüm hizmet istekleri için gereklidir. **Create Index** isteği POST veya PUT yöntemini kullanan olacak oluşturulan. POST kullanırken, dizin şeması tanımı birlikte istek gövdesinde bir dizin adı sağlamanız gerekir. PUT ile dizin adı URL bir parçasıdır. Dizin yoksa, oluşturulur. Zaten varsa, yeni tanımına güncelleştirilir.

Dizin adı gerekir küçük olması, bir harf veya sayı ile başlamalı, hiçbir eğik çizgi veya nokta olan ve 128 karakterden kısa olmalıdır. Tireler ardışık olmayan sürece dizin adı bir harf veya sayı ile başlattıktan sonra kalan adının herhangi harf, sayı ve kısa çizgi, içerebilir.

`api-version` Gereklidir. Bkz: [arama hizmet sürümü oluşturma](http://msdn.microsoft.com/library/azure/dn864560.aspx) kullanılabilir sürümlerin listesi için.

**İstek üstbilgileri**

Aşağıdaki listede gerekli ve isteğe bağlı İstek üstbilgilerinin açıklanmaktadır.

* `Content-Type`: Gerekli. Bu ayar`application/json`
* `api-key`: Gerekli. `api-key` İçin kullanılır
* isteğin arama hizmetiniz için kimlik doğrulaması. Hizmetinize benzersiz bir dize değeri değil. **Create Index** isteği içermelidir bir `api-key` üstbilgi yönetici anahtarınızı (aksine, bir sorgu anahtarı) ayarlayın.

İstek URL'si oluşturmak için hizmet adı da gerekir. Her iki hizmet adını alabilir ve `api-key` Azure Portalı'nda, hizmet panosundan. Bkz: [portalda Azure Search hizmeti oluşturma](search-create-service-portal.md) sayfa gezinti Yardım.

<a name="RequestData"></a>
**İstek gövdesi sözdizimi**

Bu dizin, veri türleri, öznitelikler yanı sıra isteğe bağlı bir sorgu zamanında eşleşen belgeleri Puanlama amacıyla kullanılan profilleri Puanlama listesi içine beslenecek belgeleri içindeki veri alanlarını listesini içeren bir şema tanımı istek gövdesi içeriyor .

Bir POST isteği için dizin adı istek gövdesinde belirtmeniz gerektiğini unutmayın.

Yalnızca olabilir bir anahtar alanı dizinde. Bu bir dize alanı olması gerekir. Bu alan dizini içinde depolanan her belge için benzersiz tanımlayıcıyı temsil eder.

Bir dizinin ana bölümleri şunlardır:

* `name`
* `fields`Bu ad, veri türü ve bu alan izin verilen eylemleri tanımlayan özellikleri dahil olmak üzere bu dizinine beslenecek.
* `suggesters`otomatik tamamlamayı veya yazarken tamamlanan sorgular için kullanılır.
* `scoringProfiles`Derecelendirme özel arama puanı için kullanılır. Bkz: [Puanlama profili Ekle](https://msdn.microsoft.com/library/azure/dn798928.aspx) Ayrıntılar için.
* `analyzers`, `charFilters`, `tokenizers`, `tokenFilters` belgeler/sorgularınızı dizine ve arama yapılabilir belirteçlere nasıl ayrılır tanımlamak için kullanılır. Bkz: [analiz Azure Search'te](https://aka.ms//azsanalysis) Ayrıntılar için.
* `defaultScoringProfile`davranışları Puanlama varsayılan üzerine yazmak için kullanılır.
* `corsOptions`dizininizi çıkış noktaları arası sorguları izin vermek için.

İstek yükünde yapılandırılması söz dizimi aşağıdaki gibidir. Örnek istek, bu konuda daha üzerinde sağlanır.

    {
      "name": (optional on PUT; required on POST) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false,              
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }

**Dizin öznitelikleri**

Aşağıdaki öznitelikler, dizin oluşturma sırasında ayarlanabilir. Puanlama ve puanlama profilleri hakkında daha fazla bilgi için bkz [eklemek profilleri Puanlama](https://msdn.microsoft.com/library/azure/dn798928.aspx).

`name`-Alanın adını ayarlar.

`type`-Alanın veri türünü ayarlar.

`searchable`-Alan tam metin arama yapabilir işaretler. Başka bir deyişle, dizin oluşturma sırasında sözcük bölme gibi analiz yapılacaktır. Ayarlarsanız bir `searchable` "güneşli gün", dahili olarak bu gibi bir değer alanına bölme tek tek belirteçlere "güneşli" ve "gün". Bu, bu koşulları için tam metin araması sağlar. Türünde alanlar `Edm.String` veya `Collection(Edm.String)` olan `searchable` varsayılan olarak. Diğer türleri alanlarının olamaz `searchable`.

* **Not**: `searchable` alanları Azure Search tam metin araması için alan değeri ek parçalanmış sürümünü depolayacak beri dizininizdeki ek boşluk kullanma. Dizininizde alanı kaydetmek istediğiniz ve arama dahil edilecek bir alan olması gerekmez, ayarlamak `searchable` için `false`.

`filterable`-İçinde başvurulacak alan verir `$filter` sorgular. `filterable`farklı `searchable` dizeleri işlenme içinde. Türünde alanlar `Edm.String` veya `Collection(Edm.String)` olan `filterable` yalnızca tam eşleşme için karşılaştırmaları; bu nedenle Sözcük bölünmesi, uygulanabilecek değil. Örneğin, böyle bir alan ayarlarsanız `f` "güneşli Day" `$filter=f eq 'sunny'` herhangi bir eşleşme bulur ancak `$filter=f eq 'sunny day'` olur. Tüm alanlar `filterable` varsayılan olarak.

`sortable`-Varsayılan sistem sonuçları puana göre sıralar, ancak birçok deneyimlerinde kullanıcıların belgeleri alanlara göre sıralamayı isteyeceksiniz. Türünde alanlar `Collection(Edm.String)` olamaz `sortable`. Diğer tüm alanlar `sortable` varsayılan olarak.

`facetable`-Genellikle kategorisini (örneğin, dijital kamera ve bakın isabet marka tarafından megapiksel tarafından fiyat, vb. göre arayın.) tarafından isabet sayısı içeren sunu arama sonuçlarının kullanılır. Bu seçenek türü alanlarla kullanılamaz `Edm.GeographyPoint`. Diğer tüm alanlar `facetable` varsayılan olarak.

* **Not**: türünde alanlar `Edm.String` olan `filterable`, `sortable`, veya `facetable` 32 KB cinsinden uzunluğu en fazla olabilir. Bu tür alanları tek arama terimi olarak kabul edilir ve Azure Search'te bir terim uzunluğu en fazla 32 KB olduğundan budur. Tek bir dize alanında bu daha fazla metni depolamak gerekiyorsa, açık bir şekilde ayarlamanız gerekir `filterable`, `sortable`, ve `facetable` için `false` , dizin tanımında.
* **Not**: bir alan ayarlamak yukarıdaki öznitelikleri hiçbiri varsa `true` (`searchable`, `filterable`, `sortable`, veya`facetable`) alan etkili bir şekilde ters dizinden çıkarılır. Bu seçenek, sorgularda kullanılmaz, ancak arama sonuçlarında gerekli alanlar için kullanışlıdır. Bu tür alanları dizinden hariç performansı artırır.

`key`-Alanın dizini içinde belgeleri için benzersiz tanımlayıcı içeren olarak işaretler. Yalnızca bir alanın seçilen, olarak `key` alanı ve türü olması gerektiği `Edm.String`. Anahtar alanları için doğrudan aracılığıyla belgeleri aramak için kullanılabilir [arama API](#LookupAPI).

`retrievable`-Alan bir arama sonucunda döndürülebilecek olup olmadığını belirler.  Alanı (örneğin, kenar boşluğu) kullanmak bir filtre olarak sıralama veya mekanizması Puanlama istediğiniz, ancak son kullanıcı için görünür olacak alanı istemediğiniz durumlarda kullanışlıdır. Bu öznitelik olmalıdır `true` için `key` alanları.

`analyzer`-Arama süresini ve dizin oluşturma zamanında alan için kullanılacak çözümleyici adını ayarlar. İzin verilen değerler için bkz [çözümleyiciler](https://msdn.microsoft.com/library/mt605304.aspx). Bu seçenek yalnızca kullanılabilir `searchable` alanları ve ayarlanamaz birlikte ya da `searchAnalyzer` veya `indexAnalyzer`.  Çözümleyicisi seçildikten sonra alan için değiştirilemez.

`searchAnalyzer`-Arama zaman alan için kullanılan Çözümleyicisi adını ayarlar. İzin verilen değerler için bkz [çözümleyiciler](https://msdn.microsoft.com/library/mt605304.aspx). Bu seçenek yalnızca kullanılabilir `searchable` alanları. İle birlikte ayarlanmalıdır `indexAnalyzer` ve ile birlikte ayarlanamaz `analyzer` seçeneği. Bu çözümleyici varolan alan güncelleştirilebilir.

`indexAnalyzer`-Alan için dizin oluşturma zamanında kullanılan Çözümleyicisi adını ayarlar. İzin verilen değerler için bkz [çözümleyiciler](https://msdn.microsoft.com/library/mt605304.aspx). Bu seçenek yalnızca kullanılabilir `searchable` alanları. İle birlikte ayarlanmalıdır `searchAnalyzer` ve ile birlikte ayarlanamaz `analyzer` seçeneği. Çözümleyicisi seçildikten sonra alan için değiştirilemez.

`suggesters`-Arama modu ve öneriler için içerik kaynağı olan alanları ayarlar. Bkz: [ilgili](#Suggesters) Ayrıntılar için.

`scoringProfiles`-Hangi öğelerin arama sonuçlarında daha yüksek görünür etkilemek sağlayan özel Puanlama davranışları tanımlar. Puanlama profili, alan ağırlığı ve işlevleri yapılır. Bkz: [eklemek profilleri Puanlama](https://msdn.microsoft.com/library/azure/dn798928.aspx) Puanlama profili kullanılan öznitelikler hakkında daha fazla bilgi.

<!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Dil desteği**

Aranabilir alanlara analiz uygulanabilecek en sık Sözcük bölünmesi, metin normalleştirme ve koşulları filtreleme ilgilidir. Varsayılan olarak Azure arama aranabilir alanlara sahip analiz [Apache Lucene standart Çözümleyicisi](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) aşağıdaki elemanlara metni keser["Unicode metin kesimleme"](http://unicode.org/reports/tr29/) kuralları. Ayrıca, standart Çözümleyicisi tüm karakterleri küçük harfe formlarına dönüştürür. Dizinlenmiş belgeleri ve arama terimlerini analiz dizin oluşturma ve sorgu işleme sırasında gidin.

Azure arama, çeşitli dillerde destekler. Her dil için belirli bir dil özellikleri hesapları bir standart metin Çözümleyicisi gerektirir. Azure arama çözümleyiciler iki tür sunar:

* 35 çözümleyiciler Lucene tarafından yedeklenir.
* Office ve Bing kullanılan teknoloji işleme özel Microsoft doğal dil desteğiyle 50 Çözümleyicileri.

Bazı geliştiriciler Lucene daha tanıdık, basit, açık kaynak çözümü tercih edebilirsiniz. Lucene çözümleyiciler hızlıdır, ancak Microsoft çözümleyiciler Gelişmiş lemmatization, word (dillerde Almanca, Danca, Felemenkçe, İsveççe, Norveççe, Estonca, bitiş, Macarca, Slovakça gibi) decompounding ve varlık tanıma (URL'ler gibi özellikleri e-postalar, tarihler, sayılar). Mümkünse, hangisinin daha iyi uygun olduğuna karar vermek için hem Microsoft hem de Lucene çözümleyiciler karşılaştırmaları çalıştırmanız gerekir.

***Nasıl Karşılaştır***

İngilizce için Lucene Çözümleyicisi standart Çözümleyicisi genişletir. ('S sondaki) iyelik sözcükleri kaldırır, göre kaynaklanan geçerlidir [bağlantısı kaynaklanan algoritması](http://tartarus.org/~martin/PorterStemmer/)ve İngilizce kaldırır [durdurma sözcükleri](http://en.wikipedia.org/wiki/Stop_words).

Buna karşılık, Microsoft analyzer kaynaklanan yerine lemmatization gerçekleştirir. Bunu işleyebilir bükümlü ve düzensiz sözcük formlarını daha iyi ne daha ilgili arama sonuçlarında sonuçları anlamına gelir (izleme modülünü 7 [Azure arama MVA sunu](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) daha fazla ayrıntı için).

İle Microsoft çözümleyiciler dizin ortalama iki ila üç kat daha yavaş Lucene eşdeğerlerine dil bağlı olarak daha açıktır. Arama performansını önemli ölçüde ortalama boyutu sorgularında etkilenmemesi gerekir.

***Yapılandırma***

Dizin tanımı'ndaki her bir alan için ayarladığınız `analyzer` hangi dil ve satıcı belirten Çözümleyicisi adına özelliği. Aynı Çözümleyicisi dizin oluşturma ve bu alan için arama uygulanır.
Örneğin, yana birimi aynı dizinde mevcut İngilizce, Fransızca ve İspanyolca otel açıklamaları için ayrı alanları olabilir. Kullanım ['searchFields' sorgu parametresi](#SearchQueryParameters) karşı sorgularınızda aramak için hangi dile özgü alanı belirtmek için. Dahil sorgu örnekler gözden geçirebilirsiniz `analyzer` özelliğinde [Search belgeleri](#SearchDocs). 

***Çözümleyici listesi***

Lucene ve Microsoft Çözümleyicisi adları ile birlikte desteklenen dillerin listesi aşağıdadır.

<table style="font-size:12">
    <tr>
        <th>Dil</th>
        <th>Microsoft Çözümleyicisi adı</th>
        <th>Lucene Çözümleyicisi adı</th>
    </tr>
    <tr>
        <td>Arapça</td>
        <td>ar.Microsoft</td>
        <td>ar.lucene</td>        
    </tr>
    <tr>
        <td>Ermenice</td>
        <td></td>
        <td>hy.lucene</td>
      </tr>
    <tr>
        <td>Bangla</td>
        <td>Bn.Microsoft</td>
        <td></td>
    </tr>
      <tr>
        <td>Bask dili</td>
        <td></td>
        <td>EU.lucene</td>
    </tr>
      <tr>
         <td>Bulgarca</td>
        <td>BG.Microsoft</td>
        <td>BG.lucene</td>
      </tr>
      <tr>
        <td>Katalanca</td>
        <td>CA.Microsoft</td>
        <td>CA.lucene</td>          
      </tr>
    <tr>
        <td>Basitleştirilmiş Çince</td>
        <td>zh-Hans.microsoft</td>
        <td>zh-Hans.lucene</td>        
    </tr>
    <tr>
        <td>Geleneksel Çince</td>
        <td>zh-Hant.microsoft</td>
        <td>zh-Hant.lucene</td>        
    <tr>
    <tr>
        <td>Hırvatça</td>
        <td>hr.Microsoft</td>
        <td/></td>
    </tr>
    <tr>
        <td>Çekçe</td>
        <td>cs.Microsoft</td>
        <td>cs.lucene</td>        
    </tr>    
    <tr>
        <td>Danca</td>
        <td>da.Microsoft</td>
        <td>da.lucene</td>        
    </tr>    
    <tr>
        <td>Hollanda dili</td>
        <td>NL.Microsoft</td>
        <td>NL.lucene</td>    
    </tr>    
    <tr>
        <td>Türkçe</td>        
        <td>en.Microsoft</td>
        <td>en.lucene</td>        
    </tr>
    <tr>
        <td>Estonca</td>
        <td>et.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Fince</td>
        <td>Fi.Microsoft</td>
        <td>Fi.lucene</td>        
    </tr>    
    <tr>
        <td>Fransızca</td>
        <td>fr.Microsoft</td>
        <td>fr.lucene</td>        
    </tr>
    <tr>
        <td>Galiçya lehçesi</td>
        <td></td>
        <td>GL.lucene</td>        
      </tr>
    <tr>
        <td>Almanca</td>
        <td>de.Microsoft</td>
        <td>de.lucene</td>        
    </tr>
    <tr>
        <td>Yunanca</td>
        <td>el.Microsoft</td>
        <td>el.lucene</td>        
    </tr>
    <tr>
        <td>Gucerat dili</td>
        <td>gu.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>İbranice</td>
        <td>He.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Hintçe</td>
        <td>Hi.Microsoft</td>
        <td>Hi.lucene</td>        
    </tr>
    <tr>
        <td>Macarca</td>        
        <td>hu.Microsoft</td>
        <td>hu.lucene</td>
    </tr>
    <tr>
        <td>İzlanda dili</td>
        <td>is.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Endonezya dili (Bahasa)</td>
        <td>id.Microsoft</td>
        <td>id.lucene</td>        
    </tr>
    <tr>
        <td>İrlanda dili</td>
        <td></td>
          <td>GA.lucene</td>
    </tr>
    <tr>
        <td>İtalyanca</td>
        <td>it.Microsoft</td>
        <td>it.lucene</td>        
    </tr>
    <tr>
        <td>Japonca</td>
        <td>ja.Microsoft</td>
        <td>ja.lucene</td>

    </tr>
    <tr>
        <td>Kannada dili</td>
        <td>Ka.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Kore dili</td>
        <td>Ko.Microsoft</td>
        <td>Ko.lucene</td>
    </tr>
    <tr>
        <td>Letonca</td>        
        <td>LV.Microsoft</td>
        <td>LV.lucene</td>    
    </tr>
    <tr>
        <td>Litvanca</td>
        <td>lt.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Malayalam dili</td>
        <td>ML.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Malay (Latin)</td>
        <td>MS.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Marathi</td>
        <td>Mr.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Norveççe</td>
        <td>NB.Microsoft</td>
        <td>No.lucene</td>        
    </tr>
      <tr>
        <td>Farsça</td>
        <td></td>
        <td>FA.lucene</td>        
      </tr>
    <tr>
        <td>Lehçe</td>
        <td>PL.Microsoft</td>
        <td>PL.lucene</td>        
    </tr>
    <tr>
        <td>Portekizce (Brezilya)</td>
        <td>PT Br.microsoft</td>
        <td>PT Br.lucene</td>        
    </tr>
    <tr>
        <td>Portekizce (Portekiz)</td>
        <td>PT Pt.microsoft</td>        
        <td>PT Pt.lucene</td>
    </tr>
    <tr>
        <td>Pencap dili</td>
        <td>Pa.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Rumence</td>
        <td>Ro.Microsoft</td>
        <td>Ro.lucene</td>
    </tr>
    <tr>
        <td>Rusça</td>
        <td>RU.Microsoft</td>
        <td>RU.lucene</td>    
    </tr>
    <tr>
        <td>Sırpça (Kiril)</td>
        <td>SR-cyrillic.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Sırpça (Latin)</td>
        <td>SR-latin.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Slovakça</td>
        <td>SK.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Slovence</td>
        <td>SL.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>İspanyolca</td>
        <td>Es.Microsoft</td>
        <td>Es.lucene</td>
    </tr>
    <tr>
        <td>İsveç dili</td>
        <td>sv.Microsoft</td>
        <td>sv.lucene</td>
    </tr>

    <tr>
        <td>Tamil dili</td>
        <td>ta.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Telugu dili</td>
        <td>te.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Tay dili</td>
        <td>TH.Microsoft</td>
        <td>TH.lucene</td>
    </tr>
    <tr>
        <td>Türkçe</td>
        <td>tr.Microsoft</td>
        <td>tr.lucene</td>        
    </tr>
    <tr>
        <td>Ukrayna dili</td>
        <td>UK.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Urduca</td>
        <td>Your.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Vietnam dili</td>
        <td>vi.Microsoft</td>
        <td></td>
    </tr>
    <td colspan="3">Ayrıca Azure Search dilden bağımsız çözümleyicisi yapılandırmalarını sağlar</td>
    <tr>
        <td>Standart ASCII Katlama</td>
        <td>standardasciifolding.lucene</td>
        <td>
        <ul>
            <li>Unicode metin kesimleme (standart belirteç Oluşturucu)</li>
            <li>ASCII kırılma filtresi - ilk 127 ASCII karakter kümesi ASCII eşdeğerlerine ait olmayan Unicode karakterler dönüştürür. Bu, Aksan kaldırmak için kullanışlıdır.</li>
        </ul>
        </td>
    </tr>
</table>

İle ek açıklama adları olan tüm çözümleyiciler <i>lucene</i> tarafından sağlanmıştır [Apache Lucene'nın dil Çözümleyicileri](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html). Filtre Katlama ASCII hakkında daha fazla bilgi bulunabilir [burada](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).

**Öneri araçları**

A `suggester` hangi alanların bir dizin aramaları otomatik tamamlama desteklemek için kullanılacağını tanımlar. Kısmi arama dizelerini genellikle gönderilir [önerileri API](#Suggestions) kullanıcı bir arama sorgusu yazıyor ve API önerilen tümcecikleri kümesini döndürür. Hangi alanların yazarken tamamlanan arama terimleri oluşturmak için kullanılan dizinde tanımladığınız bir öneri aracı belirler. Bkz: [ilgili](#Suggesters) yapılandırma ayrıntıları için.

**Puanlama modelleri**

A `scoringProfile` hangi öğelerin arama sonuçlarında daha yüksek görünür etkilemek sağlayan özel Puanlama davranışları tanımlar. Puanlama profili, alan ağırlığı ve işlevleri yapılır. Bunları kullanmak için sorgu dizesi adına göre bir profili belirtin.

Arka planda bir sonuç kümesi bulunan her öğe için bir arama puanı hesaplamak için bir varsayılan profili Puanlama çalışır. İç, Puanlama profili adlandırılmamış kullanabilirsiniz. Alternatif olarak, kümesinin `defaultScoringProfile` özel bir profil varsayılan olarak kullanmak üzere özel bir profil sorgu dizesinde belirtilmediğinde çağrılır.

Bkz: [arama dizini (Azure Search Hizmeti REST API'si) Puanlama profili Ekle](search-api-scoring-profiles-2015-02-28-preview.md) Ayrıntılar için.

**CORS seçenekleri**

Tarayıcı tüm çıkış noktaları arası istekleri engeller istemci tarafı Javascript herhangi API'leri varsayılan olarak çağrılamaz. CORS (çıkış noktaları arası kaynak paylaşımı) ayarlayarak etkinleştir `corsOptions` dizininizi çıkış noktaları arası sorgularına izin vermek için öznitelik. Yalnızca bu sorguyu güvenlik nedeniyle CORS desteğini API'leri unutmayın. CORS için aşağıdaki seçenekler ayarlanabilir:

* `allowedOrigins`(gerekli): Bu bir dizininize erişim izni verilecek çıkış noktaları listesidir. Bu çıkış noktalarından sunulacak tüm Javascript kodlarının bu anlamına gelir (doğru API anahtarını verdiği varsayılarak) dizininizi sorgulama izin verilmez. Her kaynak kod biçimidir genellikle `protocol://fully-qualified-domain-name:port` ancak bağlantı noktası çoğunlukla yazılmaz. Bkz: [bu makalede](http://go.microsoft.com/fwlink/?LinkId=330822) daha fazla ayrıntı için.
  * Tüm kaynaklara erişmesine izin vermek istiyorsanız, dahil `*` tek bir öğe olarak `allowedOrigins` dizi. Unutmayın **bu yöntem üretim arama hizmetleri için önerilmez.** Ancak, geliştirme veya hata ayıklama için yararlı olabilir.
* `maxAgeInSeconds`(isteğe bağlı): tarayıcılar bu değer önbellek CORS denetim öncesi yanıtlarını süresi (saniye) belirlemek için kullanın. Bu, negatif olmayan bir tamsayı olmalıdır. Bu değer büyük, daha iyi performans olur ancak CORS İlkesi değişikliklerinin etkili olması alacaktır uzun. Ayarlanmazsa, varsayılan süre olan 5 dakika kullanılır.

<a name="CreateUpdateIndexExample"></a>
**İstek gövdesi örneği**

    {
      "name": "hotels",  
      "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String"},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean"},
        {"name": "smokingAllowed", "type": "Edm.Boolean"},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["hotelName"]
        }
      ]
    }

**Yanıt**

Başarılı bir istek için: "201 oluşturuldu".

Varsayılan olarak yanıt gövdesi JSON için oluşturulan dizin tanımını içerir. Varsa `Prefer` istek üstbilgisi olarak ayarlanmış `return=minimal`, yanıt gövdesi boş olur ve başarı durumunu kodu olacaktır "204 İçerik yok" yerine "201 oluşturuldu". Bu, olup PUT veya POST dizini oluşturmak için kullanılan bağımsız olarak geçerlidir.

**Açıklamalar**

Şu anda, sınırlı dizin şeması güncelleştirmelerini desteği yoktur. Alan türlerinin değiştirilmesi gibi yeniden dizinlemeyi gerektiren hiçbir şema güncelleştirmesi şu anda desteklenmiyor. Var olan alanları değiştirilmiş veya silinemez ancak herhangi bir zamanda mevcut bir dizine yeni alanlar eklenebilir. Yeni bir alan eklediğinizde, dizindeki tüm mevcut belgeleri otomatik olarak bu alan için bir null değer sahip olacaktır. Yeni belgeler dizin için eklenene kadar hiçbir ek depolama alanı tüketilebilir.

<a name="Suggesters"></a>

## <a name="suggesters"></a>Öneri Araçları
Arama kutusuna girilen kısmi dize girişleri yanıta olası arama terimlerini listesini sağlayan yazarken tamamlanan veya otomatik tamamlama sorgusu özelliği, Azure Search'te önerileri özelliğidir. Büyük olasılıkla Sorgu önerileri ticari web arama motorları kullanılırken fark: Bing içinde ".NET" yazarak üreten koşulları listesini ".NET 4.5", ".NET Framework 3,5", vb.. Arama hizmeti REST API kullanırken, özel bir Azure Search uygulamada önerileri uygulamak için şunlar gerekir:

* Ekleyerek önerilerini etkinleştirmek bir **öneri aracı** adı, arama modu ve alanların listesi için yazarken tamamlanan vermiş dizininizdeki yapım çağrılır. Kısmi arama dizesi "Sea" yazarak bir kaynak alanı olarak "ŞehirAdı" belirtirseniz, örneğin, "Seattle", "Sahil" ve "(üçü gerçek Şehir adlardır) Seatac Sorgu önerileri kullanıcıya sunulan" neden olur.
* Öneriler çağırarak çağırma [önerileri API](#Suggestions) , uygulama kodunuzda. Genellikle kısmi arama dizelerini kullanıcı bir arama sorgusu yazıyor ve bu API önerilen tümcecikleri kümesini döndürür hizmetine gönderilir.

Bu makalede nasıl yapılandırılacağı açıklanmaktadır bir **öneri aracı**. Da gözden geçirmelisiniz [önerileri API](#Suggestions) bir öneri Aracı nasıl kullanıldığı hakkında bilgi.

**Kullanım**

`Suggesters`Dizin ve iş veya belirli belgeleri yerine gevşek koşulları tümce önermek üzere kullanıldığında en iyi oluşturulur. En iyi adayı başlıklar, adları ve bir öğe tanımlayabilir diğer görece kısa tümceleri alanlardır. Kategoriler ve etiketler gibi yinelenen alanları veya çok uzun alanları açıklamaları veya açıklamalar alanları gibi daha az etkili olur.

Dizin tanımı bir parçası olarak, tek bir öneri aracı için ekleyebilirsiniz `suggesters` koleksiyonu. Bir öneri aracı tanımlayan özellikleri şunlardır:

* `name`: Öneri aracı adı. Öneri aracı adını çağrılırken kullanmak `suggest` API.
* `searchMode`: Aday tümcecikleri aramak için kullanılan stratejisi. Şu anda desteklenen tek mod `analyzingInfixMatching`, başında veya ortasında cümleleri esnek eşleştirilmesini gerçekleştirir.
* `sourceFields`: Öneriler için içerik kaynağı olan bir veya daha fazla alanları bir listesi. Yalnızca türünde alanlar `Edm.String` ve `Collection(Edm.String)` kaynakları önerileri için olabilir. Ayarlama özel bir dil Çözümleyicisi sahip olmayan alanlar kullanılabilir.

**Öneri aracı örneği**

Bir öneri aracı dizini bir parçasıdır. Yalnızca bir öneri aracı varolabilir `suggesters` alanlar koleksiyonu yanında geçerli sürümü koleksiyonunda ve `scoringProfiles`.

        {
          "name": "hotels",
          "fields": [
             . . .
           ],
          "suggesters": [
            {
            "name": "sg",
            "searchMode": "analyzingInfixMatching",
            "sourceFields: ["hotelName", "category"]
            }
          ],
          "scoringProfiles": [
             . . .
          ]
        }

> [!NOTE]
> Azure Search, genel Önizleme sürümü kullandıysanız `suggesters` daha eski bir boolean özelliği değiştirir (`"suggestions": false`), yalnızca desteklenen önek önerileri için kısa dizeleri (3-25 karakter). Kendi değiştirme `suggesters`, destekler infix başında veya ortasında alan içeriği, bir arama dizelerini hatalar için daha iyi tolerans ile eşleşen terimleri bulduğu eşleşen. Genel olarak kullanılabilir sürümünden başlayarak, bu artık yalnızca API önerileri uygulamasıdır. Eski `suggestions` sunulmuştur özelliği `api-version=2014-07-31-Preview` bu sürümde çalışmaya devam eder, ancak içinde çalışmıyor `2015-02-28` veya Azure Search'ın sonraki sürümleri.
> 
> 

<a name="UpdateIndex"></a>

## <a name="update-index"></a>Dizini Güncelleştir
Bir HTTP PUT İsteği kullanarak Azure Search'te içinde varolan bir dizin güncelleştirebilirsiniz. Güncelleştirmeler için varolan şema yeni alanlar ekleyerek, CORS seçenekleri değiştirme ve puanlama profilleri değiştirme içerebilir. Bkz: [eklemek profilleri Puanlama](https://msdn.microsoft.com/library/azure/dn798928.aspx) daha fazla bilgi için. İstek URI'si güncelleştirmek için dizin adını belirtin:

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Önemli:** arama dizinini yeniden oluşturmayı gerektirmeyen işlemleri için dizin şeması güncelleştirmeler için destek sınırlıdır. Alan türlerinin değiştirilmesi gibi yeniden dizinlemeyi gerektiren hiçbir şema güncelleştirmesi şu anda desteklenmiyor. Var olan alanları değiştirilmiş veya silinemez ancak herhangi bir zamanda yeni alanlar eklenebilir. Aynı durum geçerlidir `suggesters`. Yeni alanlar eklenebilir bir öneri aracı zaman alanları eklenir, ancak alanları öğesinden kaldırılamaz `suggesters` ve var olan alanları eklenemez `suggesters`.

Yeni bir alan için bir dizin eklerken, dizindeki tüm mevcut belgeleri otomatik olarak bu alan için bir null değer sahip olacaktır. Yeni belgeler dizin için eklenene kadar hiçbir ek depolama alanı tüketilebilir.

**İstek**

HTTPS tüm hizmet istekleri için gereklidir. **Güncelleştirme dizin** isteği HTTP PUT kullanarak oluşturulur. PUT ile dizin adı URL bir parçasıdır. Dizin yoksa, oluşturulur. Dizini zaten varsa, yeni tanımına güncelleştirilir.

Dizin adı gerekir küçük olması, bir harf veya sayı ile başlamalı, hiçbir eğik çizgi veya nokta olan ve 128 karakterden kısa olmalıdır. Tireler ardışık olmayan sürece dizin adı bir harf veya sayı ile başlattıktan sonra kalan adının herhangi harf, sayı ve kısa çizgi, içerebilir.

`api-version=[string]`(gerekli). Önizleme sürümü `api-version=2015-02-28-Preview`. Bkz: [arama hizmet sürümü oluşturma](http://msdn.microsoft.com/library/azure/dn864560.aspx) Ayrıntılar ve diğer sürümleri için.

**İstek üstbilgileri**

Aşağıdaki listede gerekli ve isteğe bağlı İstek üstbilgilerinin açıklanmaktadır.

* `Content-Type`: Gerekli. Bu ayar`application/json`
* `api-key`: Gerekli. `api-key` İsteğin arama hizmetiniz için kimlik doğrulaması için kullanılır. Hizmetinize benzersiz bir dize değeri değil. **Güncelleştirme dizin** isteği içermelidir bir `api-key` üstbilgi yönetici anahtarınızı (aksine, bir sorgu anahtarı) ayarlayın.

İstek URL'si oluşturmak için hizmet adı da gerekir. Hizmet adını alabilir ve `api-key` Azure Portalı'nda, hizmet panosundan. Bkz: [portalda Azure Search hizmeti oluşturma](search-create-service-portal.md) sayfa gezinti Yardım.

**İstek gövdesi sözdizimi**

Varolan bir dizini güncelleştirirken gövdesi özgün şema tanımı artı eklediğiniz yeni alanlar yanı sıra, ilgili ve CORS seçenekleri değiştirilmiş Puanlama profilini varsa eklemeniz gerekir. Puanlama profili ve CORS seçenekleri değiştirme değil, dizin oluşturulduğu gelen özgün eklemeniz gerekir. Genel güncelleştirmeleri için kullanılacak en iyi düzeni olan bir GET ile dizin tanımı almak için değiştirin, ardından PUT ile güncelleştirin.

Dizin oluşturmak için kullanılan şema sözdizimi burada kolaylık olması için yeniden oluşturulur. Bkz: [Create Index](#CreateIndex) daha fazla ayrıntı için.

    {
      "name": (optional) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false, 
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }


**Yanıt**

Başarılı bir istek için: "204 İçerik yok".

Varsayılan olarak yanıt gövdesi boş olur. Ancak, varsa `Prefer` istek üstbilgisi olarak ayarlanmış `return=representation`, yanıt gövdesi güncelleştirildi dizin tanımı JSON içerir. Bu durumda, başarı durum kodu olacak "200 Tamam".

**Dizin tanımı ile özel çözümleyiciler güncelleştiriliyor**

Bir analyzer, bir belirteç Oluşturucu, bir belirteç filtre veya char filtre tanımlandıktan sonra değiştirilemez. Yeni bir tane varsa, yalnızca mevcut bir dizine eklenebilir `allowIndexDowntime` bayrağı ayarlanmış dizin güncelleştirme isteğinde doğru: 

`PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true`

Bu işlem, dizini çevrimdışı, dizin oluşturma neden en az birkaç saniye ve sorgu isteği başarısız sokar unutmayın. Performans ve yazma kullanılabilirlik dizininin dizin güncelleştirildikten sonra birkaç dakika engelli ya da çok büyük dizinler için daha uzun olabilir.

<a name="ListIndexes"></a>

## <a name="list-indexes"></a>Liste dizinler
**Listesi dizinleri** işlemi listesi döndürülür dizinleri şu anda Azure Search hizmetinizin.

    GET https://[service name].search.windows.net/indexes?api-version=[api-version]
    api-key: [admin key]

**İstek**

HTTPS tüm hizmet istekleri için gereklidir. **Listesi dizinleri** isteği GET yöntemini kullanan olacak oluşturulan.

`api-version=[string]`(gerekli). Önizleme sürümü `api-version=2015-02-28-Preview`. Bkz: [arama hizmet sürümü oluşturma](http://msdn.microsoft.com/library/azure/dn864560.aspx) Ayrıntılar ve diğer sürümleri için.

**İstek üstbilgileri**

Aşağıdaki listede gerekli ve isteğe bağlı İstek üstbilgilerinin açıklanmaktadır.

* `api-key`: Gerekli. `api-key` İsteğin arama hizmetiniz için kimlik doğrulaması için kullanılır. Hizmetinize benzersiz bir dize değeri değil. **Listesi dizinler** isteği içermelidir bir `api-key` bir yönetici anahtarı (aksine, bir sorgu anahtarı) ayarlayın.

İstek URL'si oluşturmak için hizmet adı da gerekir. Hizmet adını alabilir ve `api-key` Azure Portalı'nda, hizmet panosundan. Bkz: [portalda Azure Search hizmeti oluşturma](search-create-service-portal.md) sayfa gezinti Yardım.

**İstek gövdesi**

yok.

**Yanıt**

Durum kodu: 200 Tamam için başarılı bir yanıt döndürdü.

Bir örnek yanıt gövdesi şöyledir:

    {
      "value": [
        {
          "name": "Books",
          "fields": [
            {"name": "ISBN", ...},
            ...
          ]
        },
        {
          "name": "Games",
          ...
        },
        ...
      ]
    }

Yalnızca ilgilendiğiniz özellikleri aşağıya doğru yanıt filtreleyebilirsiniz unutmayın. Örneğin, yalnızca dizin adlarının bir listesini istiyorsanız, OData kullanın `$select` sorgu seçeneği:

    GET /indexes?api-version=2015-02-28-Preview&$select=name

Bu durumda, yukarıdaki örnek yanıttan şu şekilde görünür:

    {
      "value": [
        {"name": "Books"},
        {"name": "Games"},
        ...
      ]
    }

Bu, dizinleri çok arama hizmetiniz varsa, bant genişliğinden tasarruf için yararlı bir tekniktir.

<a name="GetIndex"></a>

## <a name="get-index"></a>Dizin Al
**Alma dizin** işlemi Azure aramadan dizin tanımını alır.

    GET https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**İstek**

HTTPS istekleri için gereklidir. **Alma dizin** isteği GET yöntemini kullanan olacak oluşturulan.

İstek URI'SİNDEKİ [dizin adı] dizinleri koleksiyondan döndürmek için hangi dizinini belirtir.

`api-version=[string]`(gerekli). Önizleme sürümü `api-version=2015-02-28-Preview`. Bkz: [arama hizmet sürümü oluşturma](http://msdn.microsoft.com/library/azure/dn864560.aspx) Ayrıntılar ve diğer sürümleri için.

**İstek üstbilgileri**

Aşağıdaki listede gerekli ve isteğe bağlı İstek üstbilgilerinin açıklanmaktadır.

* `api-key``api-key` İsteğin arama hizmetiniz için kimlik doğrulaması için kullanılır. Hizmetinize benzersiz bir dize değeri değil. **Alma dizin** isteği içermelidir bir `api-key` bir yönetici anahtarı (aksine, bir sorgu anahtarı) ayarlayın.

İstek URL'si oluşturmak için hizmet adı da gerekir. Hizmet adını alabilir ve `api-key` Azure Portalı'nda, hizmet panosundan. Bkz: [portalda Azure Search hizmeti oluşturma](search-create-service-portal.md) sayfa gezinti Yardım.

**İstek gövdesi**

yok.

**Yanıt**

Durum kodu: 200 Tamam için başarılı bir yanıt döndürdü.

Örnek JSON içinde [oluşturma ve bir dizin güncelleştirme](#CreateUpdateIndexExample) yanıt yükü ilişkin bir örnek.

<a name="DeleteIndex"></a>

## <a name="delete-index"></a>Dizin Sil
**Silmek dizin** işlemi, Azure Search hizmetinizin dizin ve ilişkili belgeleri kaldırır. Dizin adı Azure portalında hizmet panosundan veya API'sinden elde edebilirsiniz. Bkz: [listesi dizinleri](#ListIndexes) Ayrıntılar için.

    DELETE https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**İstek**

HTTPS istekleri için gereklidir. **Silmek dizin** isteği DELETE yöntemini kullanan olacak oluşturulan.

İstek URI'SİNDEKİ [dizin adı] dizinleri koleksiyonundan silmek için hangi dizinini belirtir.

`api-version=[string]`(gerekli). Önizleme sürümü `api-version=2015-02-28-Preview`. Bkz: [arama hizmet sürümü oluşturma](http://msdn.microsoft.com/library/azure/dn864560.aspx) Ayrıntılar ve diğer sürümleri için.

**İstek üstbilgileri**

Aşağıdaki listede gerekli ve isteğe bağlı İstek üstbilgilerinin açıklanmaktadır.

* `api-key`: Gerekli. `api-key` İsteğin arama hizmetiniz için kimlik doğrulaması için kullanılır. Hizmet URL'nizi benzersiz bir dize değeri değil. **Silmek dizin** isteği içermelidir bir `api-key` üstbilgi yönetici anahtarınızı (aksine, bir sorgu anahtarı) ayarlayın.

İstek URL'si oluşturmak için hizmet adı da gerekir. Hizmet adını alabilir ve `api-key` Azure Portalı'nda, hizmet panosundan. Bkz: [portalda Azure Search hizmeti oluşturma](search-create-service-portal.md) sayfa gezinti Yardım.

**İstek gövdesi**

yok.

**Yanıt**

Durum kodu: 204 Hayır içerik için başarılı bir yanıt döndürülür.

<a name="GetIndexStats"></a>

## <a name="get-index-statistics"></a>Dizin istatistikleri Al
**Dizin istatistikleri almak** işlemi, geçerli dizin ek depolama alanı kullanımı için bir belge sayısı Azure aramadan döndürür.

    GET https://[service name].search.windows.net/indexes/[index name]/stats?api-version=[api-version]
    api-key: [admin key]

> [!NOTE]
> Belge sayısını ve depolama boyutunu istatistiklerle değil gerçek zamanlı olarak birkaç dakikada toplanır. Bu nedenle, bu API tarafından döndürülen istatistikleri son dizin oluşturma işlemleri tarafından yapılan değişiklikler neden yansıtmayabilir.
> 
> 

**İstek**

HTTPS tüm hizmetleri istekler için gereklidir. **Dizin istatistikleri almak** isteği GET yöntemini kullanan olacak oluşturulan.

İstek URI'SİNDEKİ [dizin adı] belirtilen dizin için dizin istatistiği hizmete bildirir.

`api-version=[string]`(gerekli). Önizleme sürümü `api-version=2015-02-28-Preview`. Bkz: [arama hizmet sürümü oluşturma](http://msdn.microsoft.com/library/azure/dn864560.aspx) Ayrıntılar ve diğer sürümleri için.

**İstek üstbilgileri**

Aşağıdaki listede gerekli ve isteğe bağlı İstek üstbilgilerinin açıklanmaktadır.

* `api-key``api-key` İsteğin arama hizmetiniz için kimlik doğrulaması için kullanılır. Hizmetinize benzersiz bir dize değeri değil. **Dizin istatistikleri almak** isteği içermelidir bir `api-key` bir yönetici anahtarı (aksine, bir sorgu anahtarı) ayarlayın.

İstek URL'si oluşturmak için hizmet adı da gerekir. Hizmet adını alabilir ve `api-key` Azure Portalı'nda, hizmet panosundan. Bkz: [portalda Azure Search hizmeti oluşturma](search-create-service-portal.md) sayfa gezinti Yardım.

**İstek gövdesi**

yok.

**Yanıt**

Durum kodu: 200 Tamam için başarılı bir yanıt döndürdü.

Yanıt gövdesi aşağıdaki biçimdedir:

    {
      "documentCount": number,
      "storageSize": number (size of the index in bytes)
    }

<a name="TestAnalyzer"></a>

## <a name="test-analyzer"></a>Testi Çözümleyicisi
**Analiz API** nasıl bir Çözümleyicisi belirteçlere metni keser. gösterir.

    POST https://[service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**İstek**

HTTPS tüm hizmetleri istekler için gereklidir. **Analiz API** isteği POST yöntemini kullanan olacak oluşturulan.

`api-version=[string]`(gerekli). Önizleme sürümü `api-version=2015-02-28-Preview`. Bkz: [arama hizmet sürümü oluşturma](http://msdn.microsoft.com/library/azure/dn864560.aspx) Ayrıntılar ve diğer sürümleri için.

**İstek üstbilgileri**

Aşağıdaki listede gerekli ve isteğe bağlı İstek üstbilgilerinin açıklanmaktadır.

* `api-key``api-key` İsteğin arama hizmetiniz için kimlik doğrulaması için kullanılır. Hizmetinize benzersiz bir dize değeri değil. **Analiz API** isteği içermelidir bir `api-key` bir yönetici anahtarı (aksine, bir sorgu anahtarı) ayarlayın.

Dizin adı ve istek URL'si oluşturmak için hizmet adı da gerekir. Hizmet adını alabilir ve `api-key` Azure Portalı'nda, hizmet panosundan. Bkz: [portalda Azure Search hizmeti oluşturma](search-create-service-portal.md) sayfa gezinti Yardım.

**İstek gövdesi**

    {
      "text": "Text to analyze",
      "analyzer": "analyzer_name"
    }

or

    {
      "text": "Text to analyze",
      "tokenizer": "tokenizer_name",
      "tokenFilters": (optional) [ "token_filter_name" ],
      "charFilters": (optional) [ "char_filter_name" ]
    }

`analyzer_name`, `tokenizer_name`, `token_filter_name` Ve `char_filter_name` önceden tanımlanmış veya özel çözümleyiciler, tokenizers, belirteç filtreleri ve char filtreleri dizini için geçerli adlar olması gerekir. Sözcük çözümleme işlemi hakkında daha fazla bilgi için [analiz Azure Search'te](https://aka.ms/azsanalysis).

**Yanıt**

Durum kodu: 200 Tamam için başarılı bir yanıt döndürdü.

Yanıt gövdesi aşağıdaki biçimdedir:

    {
      "tokens": [
        {
          "token": string (token),
          "startOffset": number (index of the first character of the token),
          "endOffset": number (index of the last character of the token),
          "position": number (position of the token in the input text)
        },
        ...
      ]
    }

**API örnek Çözümle**

**İstek**

    {
      "text": "Text to analyze",
      "analyzer": "standard"
    }

**Yanıt**

    {
      "tokens": [
        {
          "token": "text",
          "startOffset": 0,
          "endOffset": 4,
          "position": 0
        },
        {
          "token": "to",
          "startOffset": 5,
          "endOffset": 7,
          "position": 1
        },
        {
          "token": "analyze",
          "startOffset": 8,
          "endOffset": 15,
          "position": 2
        }
      ]
    }

- - -
<a name="DocOps"></a>

## <a name="document-operations"></a>Belge işlemleri
Azure Search'te bir dizine bulutta depolanan ve hizmete karşıya JSON belgelerini kullanarak doldurulur. Karşıya yüklediğiniz tüm belgeleri arama verilerinizi gövde kapsar. Belgeleri karşıya gibi bazıları arama terimleri simgeleştirilmiş alanlar içerebilir. `/docs` Azure Search API URL kesimi dizini belgelerde koleksiyonunu temsil eder. Karşıya yükleme gibi koleksiyon üzerinde gerçekleştirilen tüm işlemler birleştirme, silme veya belgeleri Al sorgulama yerleştirin bağlamında tek bir dizin, bu nedenle URL'leri bu işlemlerin her zaman ile başlar için `/indexes/[index name]/docs` için belirtilen dizin adı.

Uygulama kodunuz için Azure Search karşıya yüklemek için JSON belgeleri ya da oluşturmanız gerekir veya kullanabileceğiniz bir [dizin oluşturucu](https://msdn.microsoft.com/library/dn946891.aspx) veri kaynağı Azure SQL Database veya Azure Cosmos DB ise belgeleri yüklemek için. Genellikle, dizinleri sağlayan bir tek veri kümesinden doldurulur.

Arama yapmak istediğiniz her bir öğe için bir belge sahip planlamanız gerekir. Film kiralama uygulamasının film başına tek bir belgenin olabilir, bir mağaza uygulaması SKU başına tek bir belgenin olabilir, bir çevrimiçi eğitim malzemesi uygulamasının indirmelere başına tek bir belgenin olabilir, araştırma kesin bir belgeyi her akademik kağıt olabilir kendi Depo ve benzeri.

Belgeler bir veya daha fazla alan oluşur. Alanları Azure Search tarafından arama terimleri simgeleştirilmiş yanı sıra dışı simgeleştirilmiş metin veya filtreleri veya Puanlama profilleri kullanılabilir metin dışı değerleri içerebilir. Adları, veri türleri ve her bir alan için desteklenen arama özellikleri, dizin şemasına göre belirlenir. Her dizin şemasında alanlardan biri bir kimliği olarak işaretlenmesi gerekir ve her bir belgenin bu belge dizini içinde benzersiz olarak tanıtan ID alanı için bir değer olmalıdır. Diğer tüm belge alanlarını isteğe bağlıdır ve sol belirtilmezse bir null değeri varsayılan olarak alır. Null değerler arama dizini yer almayan unutmayın.

Belgeleri yüklemeden önce dizin hizmeti oluşturmuş olmalısınız. Bkz: [Create Index](#CreateIndex) birinci adım hakkında ayrıntılı bilgi için.

<a name="AddOrUpdateDocuments"></a>

## <a name="add-update-or-delete-documents"></a>Ekleme, güncelleştirme, belgeleri veya silme
Karşıya yüklediğiniz, HTTP POST kullanılarak belirtilen bir dizinden birleştirme, birleştirme veya karşıya yükleme veya delete belgeleri. Güncelleştirmeler için çok sayıda (toplu iş başına 1000 belge) veya toplu iş başına yaklaşık 16 MB kadar belgelerin toplu işleme önerilir.

    POST https://[service name].search.windows.net/indexes/[index name]/docs/index?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**İstek**

HTTPS tüm hizmet istekleri için gereklidir. Karşıya yüklediğiniz, HTTP POST kullanılarak belirtilen bir dizinden birleştirme, birleştirme veya karşıya yükleme veya delete belgeleri.

İstek URI'si [dizin adı] içeren belgeleri gönderme hangi dizin belirtme. Bu gibi durumlarda, bir dizin belgelere yalnızca aynı anda nakledebilirsiniz.

`api-version=[string]`(gerekli). Önizleme sürümü `api-version=2015-02-28-Preview`. Bkz: [arama hizmet sürümü oluşturma](http://msdn.microsoft.com/library/azure/dn864560.aspx) Ayrıntılar ve diğer sürümleri için.

**İstek üstbilgileri**

Aşağıdaki listede gerekli ve isteğe bağlı İstek üstbilgilerinin açıklanmaktadır.

* `Content-Type`: Gerekli. Bu ayar`application/json`
* `api-key`: Gerekli. `api-key` İsteğin arama hizmetiniz için kimlik doğrulaması için kullanılır. Hizmetinize benzersiz bir dize değeri değil. **Ekleme belgeleri** isteği içermelidir bir `api-key` üstbilgi yönetici anahtarınızı (aksine, bir sorgu anahtarı) ayarlayın.

İstek URL'si oluşturmak için hizmet adı da gerekir. Hizmet adını alabilir ve `api-key` Azure Portalı'nda, hizmet panosundan. Bkz: [portalda Azure Search hizmeti oluşturma](search-create-service-portal.md) sayfa gezinti Yardım.

**İstek gövdesi**

İstek gövdesini dizine alınacak bir veya daha fazla belgeler içerir. Belgeler, benzersiz bir anahtar tarafından tanımlanır. Her bir eylem ile ilişkili bir belgedir: karşıya yükleme, birleştirme, mergeOrUpload veya Sil. Karşıya yükleme istekleri belge verileri anahtar/değer çiftleri kümesi olarak eklemeniz gerekir.

    {
      "value": [
        {
          "@search.action": "upload (default) | merge | mergeOrUpload | delete",
          "key_field_name": "unique_key_of_document", (key/value pair for key field from index schema)
          "field_name": field_value (key/value pairs matching index schema)
            ...
        },
        ...
      ]
    }

> [!NOTE]
> Belge anahtarları yalnızca harf, sayı, kısa çizgi içerebilir ("-"), alt çizgi ("_") ve eşittir işareti ("="). Daha fazla ayrıntı için bkz: [adlandırma kurallarını](https://msdn.microsoft.com/library/azure/dn857353.aspx).
> 
> 

**Belge eylemleri**

* `upload`: Bir karşıya yükleme eylem varsa, yeni ve güncelleştirilmiş/değiştirileceği ise belge ekleneceği bir "upsert" benzer. Tüm alanları güncelleştirme durumda yerini aldığını unutmayın.
* `merge`: Birleştirme var olan bir belgeyi belirtilen alanlarla güncelleştirir. Belge mevcut değilse birleştirme işlemi başarısız olur. Birleştirmede belirttiğiniz herhangi bir alan belgede var olan alanın yerini alır. Buna `Collection(Edm.String)` türünde alanlar dahildir. Örneğin, belgeyi bir alan değeri olan "etiketler" içeriyorsa `["budget"]` ve değeriyle bir birleştirme yürütme `["economy", "pool"]` "etiketleri", "etiketler" alanının son değeri olacaktır `["economy", "pool"]`. İçinde **değil** olması `["budget", "economy", "pool"]`.
* `mergeOrUpload`: gibi davranır `merge` belirtilen anahtara sahip bir belge dizinde zaten mevcutsa. Belge mevcut değilse gibi davranır `upload` yeni bir belgeyle.
* `delete`: Delete belirtilen belgeyi dizinden kaldırır. Belirttiğiniz tüm alanlar içinde olduğunu unutmayın bir `delete` işlemi anahtar alanı dışında yok sayılacak. Bir belgeden tek bir alanı kaldırmak istiyorsanız, kullanmak `merge` yeterlidir ve bunun yerine alanını açıkça ayarlayın `null`.

**Yanıt**

Durum kodu 200 (Tamam) tüm öğeleri başarıyla eklenmemiş anlamı başarılı bir yanıt için döndürülür. Bu belirtilir `status` özelliği ayarlanıyor tüm öğeleri de için true olarak `statusCode` 201 (için yeni yüklenen belgeleri için) veya 200 (için birleştirilmiş veya Silinen Belge) ayarlanan özelliği:

    {
      "value": [
        {
          "key": "unique_key_of_new_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 201
        },
        {
          "key": "unique_key_of_merged_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        },
        {
          "key": "unique_key_of_deleted_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        }
      ]
    }  

En az bir öğenin dizine alınması başarısız olduğunda 207 (çok durumu) durum kodu döndürülür. Dizine öğeleriniz `status` alanını false olarak ayarlayın. `errorMessage` Ve `statusCode` özellikleri dizin hatanın nedenini gösterir:

    {
      "value": [
        {
          "key": "unique_key_of_document_1",
          "status": false,
          "errorMessage": "The search service is too busy to process this document. Please try again later.",
          "statusCode": 503
        },
        {
          "key": "unique_key_of_document_2",
          "status": false,
          "errorMessage": "Document not found.",
          "statusCode": 404
        },
        {
          "key": "unique_key_of_document_3",
          "status": false,
          "errorMessage": "Index is temporarily unavailable because it was updated with the 'allowIndexDowntime' flag set to 'true'. Please try again later.",
          "statusCode": 422
        }
      ]
    }  

Aşağıdaki tabloda yanıtta döndürülen çeşitli belge başına durum kodları açıklanmaktadır. Başkalarının geçici hata koşulları belirtmek karşın bazı sorunları isteği kendisi ile olduğunu unutmayın. İkinci bir gecikmeden sonra yeniden denemeniz gerekir.

<table style="font-size:12">
    <tr>
        <th>Durum kodu</th>
        <th>Anlamı</th>
        <th>Yeniden denenebilir</th>
        <th>Notlar</th>
    </tr>
    <tr>
        <td>200</td>
        <td>Belge başarıyla değiştirilmiş veya silinmiş.</td>
        <td>yok</td>
        <td>Silme işlemlerine olan <a href="https://en.wikipedia.org/wiki/Idempotence">ıdempotent</a>. Diğer bir deyişle, bir belge anahtarı dizinde mevcut değilse, bu anahtara sahip bir silme işlemini denemeden 200 durum koduna neden olur.</td>
    </tr>
    <tr>
        <td>201</td>
        <td>Belge başarıyla oluşturuldu.</td>
        <td>yok</td>
        <td></td>
    </tr>
    <tr>
        <td>400</td>
        <td>Belgedeki dizine alınmasını engelleyen bir hata vardı.</td>
        <td>Hayır</td>
        <td>Yanıt hata iletisinde belgeyle nerede olduğunu belirtir.</td>
    </tr>
    <tr>
        <td>404</td>
        <td>Verilen anahtar dizininde mevcut olmadığından belge birleştirilemeyen.</td>
        <td>Hayır</td>
        <td>Bu hata yüklemeler için yeni belgeler oluşturma ve olduklarından silme işlemi için oluşmaz oluşmaz <a href="https://en.wikipedia.org/wiki/Idempotence">ıdempotent</a>.</td>
    </tr>
    <tr>
        <td>409</td>
        <td>Bir belge dizin çalışılırken bir sürüm çakışması algılandı.</td>
        <td>Evet</td>
        <td>Bu durum, birden çok kez aynı anda aynı belge dizini oluşturmak çalıştığınız ortaya çıkar.</td>
    </tr>
    <tr>
        <td>422</td>
        <td>'AllowIndexDowntime' bayrağı 'true' olarak ayarlanmış güncelleştirilmediğinden dizin geçici olarak kullanılamıyor.</td>
        <td>Evet</td>
        <td></td>
    </tr>
    <tr>
        <td>503</td>
        <td>Arama hizmetinizi büyük olasılıkla ağır yük nedeniyle geçici olarak kullanılamıyor.</td>
        <td>Evet</td>
        <td>Kodunuzu bu durumda yeniden denemeden önce beklemesi gereken veya hizmet olmadığından uzatma risk.</td>
    </tr>
</table> 

**Not**: istemci kodunuzun sık 207 yanıtı karşılaşırsa, olası bir nedeni sistem yük altında olmasıdır. Bu denetleyerek doğrulayabilirsiniz `statusCode` 503 özelliği. Bu durumda olup olmadığını, öneririz ***dizin oluşturma isteklerinin azaltma***. Aksi durumda, trafik dizin subside değil, sistem 503 hataları olan tüm istekleri reddetme başlayabilirsiniz.

Durum kodu 429 dizin başına belge sayısı Kotanızı aştınız gösterir. Yeni bir dizin oluşturabilir veya için daha yüksek kapasite limitlerini yükseltin.

**Örnek:**

    {
      "value": [
        {
          "@search.action": "upload",
          "hotelId": "1",
          "baseRate": 199.0,
          "description": "Best hotel in town",
          "description_fr": "Meilleur hôtel en ville",
          "hotelName": "Fancy Stay",
          "category": "Luxury",
          "tags": ["pool", "view", "wifi", "concierge"],
          "parkingIncluded": false,
          "smokingAllowed": false,
          "lastRenovationDate": "2010-06-27T00:00:00Z",
          "rating": 5,
          "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
        },
        {
          "@search.action": "upload",
          "hotelId": "2",
          "baseRate": 79.99,
          "description": "Cheapest hotel in town",
          "description_fr": "Hôtel le moins cher en ville",
          "hotelName": "Roach Motel",
          "category": "Budget",
          "tags": ["motel", "budget"],
          "parkingIncluded": true,
          "smokingAllowed": true,
          "lastRenovationDate": "1982-04-28T00:00:00Z",
          "rating": 1,
          "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
        },
        {
          "@search.action": "merge",
          "hotelId": "3",
          "baseRate": 279.99,
          "description": "Surprisingly expensive",
          "lastRenovationDate": null
        },
        {
          "@search.action": "delete",
          "hotelId": "4"
        }
      ]
    }
- - -
<a name="SearchDocs"></a>

## <a name="search-documents"></a>Belge ara
A **arama** işlemi bir GET veya POST talebi olarak verilir ve eşleşen belgeleri seçmek için ölçüt vermek parametreleri belirtir.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**POST yerine GET kullanma zamanı**

Kullandığınızda, HTTP GET çağırmak için **arama** API, gereken isteği URL uzunluğu 8 KB'yi aşamaz unutmayın. Çoğu uygulama için yeterince genellikle budur. Ancak, bazı uygulamalar çok büyük sorgular veya OData filtre ifadeleri üretir. Daha büyük filtreleri ve sorguları get'ten sağladığından bu uygulamalar için HTTP POST kullanılarak daha iyi bir seçimdir. POST ile koşulları veya bir sorgu yan tümcelerinde sayısını sınırlama, POST isteği boyut sınırını yaklaşık 16 MB olduğundan ham sorgu boyutu faktördür.

> [!NOTE]
> POST isteği boyut sınırı çok büyük olsa bile arama sorguları ve filtre ifadeleri rasgele karmaşık olamaz. Bkz: [Lucene sorgu söz dizimi](https://msdn.microsoft.com/library/mt589323.aspx) ve [OData ifade sözdizimini](https://msdn.microsoft.com/library/dn798921.aspx) arama sorgusu ve filtre karmaşıklık sınırlamalar hakkında daha fazla bilgi.
> 
> 

**İstek**

HTTPS istekleri için gereklidir. **Arama** isteği GET veya POST yöntemleri kullanılarak olacak oluşturulan.

İsteğin URI sorgu parametreleri eşleşen tüm belgeler için hangi dizine belirtir. Parametreleri GET istekleri durumunda sorgu dizesinde belirtilen ve gövde POST söz konusu olduğunda istek ister.

GET isteklerini oluştururken en iyi uygulama, unutmayın [URL kodlama](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) REST API doğrudan çağrılırken belirli sorgu parametreleri. İçin **arama** bu işlemleri içerir:

* `$filter`
* `facet`
* `highlightPreTag`
* `highlightPostTag`
* `search`
* `moreLikeThis`

URL kodlaması yalnızca yukarıdaki sorgu parametreleri önerilir. Varsa, yanlışlıkla URL kodlama tüm sorgu dizesi (sonra her şeyi?), istekleri bozar.

Ayrıca, URL kodlaması yalnızca GET kullanarak doğrudan REST API çağrılırken gereklidir. Hiçbir URL kodlaması çağrılırken gereklidir **arama** POST, kullanma ya da kullanırken [.NET istemci kitaplığını](https://msdn.microsoft.com/library/dn951165.aspx), sizin için URL kodlaması işler.

<a name="SearchQueryParameters"></a>
**Sorgu parametreleri**

**Arama** sorgu ölçütlerini sağlayan ve ayrıca arama davranışını belirtin birkaç parametre kabul eder. Bu parametreler URL'deki sorgu dizesi çağrılırken sağladığınız **arama** GET aracılığıyla ve çağrılırken istek gövdesinde JSON özellikleri olarak **arama** POST ile. Bazı parametreler sözdizimi GET ve POST arasında biraz farklıdır. Bu farklılıklar, geçerli aşağıda belirtilmiştir:

`search=[string]`(isteğe bağlı) - Aranacak metin. Tüm `searchable` sürece alanları varsayılan olarak aranır `searchFields` belirtilir. Arama yaparken `searchable` alanları, arama metni simgeleştirilmiş, birden çok koşulları boşluk ile ayrılabilir şekilde (örneğin: `search=hello world`). Her terim eşleştirmek için kullanın `*` (Boole filtresi sorgular için yararlı olabilir). Bu parametrenin atlanması ayarı aynı etkiye sahip `*`. Bkz: [Basit Sorgu söz dizimi](https://msdn.microsoft.com/library/dn798920.aspx) arama söz dizimi üzerinde özellikleri için.

* **Not**: sonuçları bazen üzerinden sorgulanırken şaşırtıcı olabilir `searchable` alanları. Belirteç Oluşturucu İngilizce metin kesme, virgüller numaraları, vb. gibi ortak durumlarında mantığı içerir. Örneğin, `search=123,456` virgül İngilizce büyük sayılar için bin ayırıcı olarak kullanıldığından 123 ve 456, tek tek terimleri yerine tek bir terim 123,456 ile eşleşir. Bu nedenle, koşullarını ayırmak için noktalama yerine boşluk kullanılmasını öneririz `search` parametresi.

`searchMode=any|all`(isteğe bağlı, varsayılan olarak `any`) - arama terimleri bir bölümünü veya tamamını belgeyi bir eşleşme olarak saymak için eşleşmesi gereken olup olmadığını.

`searchFields=[string]`(isteğe bağlı) - Belirtilen metni aramak için virgülle ayrılmış bir alan adları listesi. Hedef alan işaretli, olarak `searchable`.

`queryType=simple|full`(isteğe bağlı, varsayılan olarak `simple`) - simgelerini gibi sağlayan bir basit sorgu dilini kullanma "Basit" arama metni kümesine değerlendirdiğinde +, * ve "". Sorguları, tüm aranabilir alanları değerlendirilir (veya alanlarını belirtilen `searchFields`) varsayılan olarak her belgedeki. Sorgu türü ayarlandığında `full` arama metni alana özgü ve ağırlıklı aramaları sağlayan Lucene sorgu dili kullanarak yorumlanır. Bkz: [Basit Sorgu söz dizimi](https://msdn.microsoft.com/library/dn798920.aspx) ve [Lucene sorgu söz dizimi](https://msdn.microsoft.com/library/mt589323.aspx) arama sözdizimleri üzerinde özellikleri için. 

> [!NOTE]
> Sorgu dili benzer işlevselliği sunan $filter lehinde desteklenmiyor Lucene aramada aralığı.
> 
> 

`moreLikeThis=[key]`(isteğe bağlı) **Önemli:** bu özellik yalnızca kullanılabilir `2015-02-28-Preview`. Bu seçenek metin arama parametre içeren bir sorguda kullanılamaz `search=[string]`. `moreLikeThis` Parametresi belge anahtarı tarafından belirtilen belge benzer belgeleri bulur. Ne zaman bir arama isteği yapıldığında ile `moreLikeThis`, arama terimleri listesi sıklığı ve kaynak belgedeki terimlerin rarity temel alınarak oluşturulur. Bu koşullar, daha sonra isteği yapmak için kullanılır. Varsayılan olarak, tüm içeriğini `searchable` sürece alanlar olarak kabul `searchFields` hangi alanların aranacağını kısıtlamak için kullanılır.  

`$skip=#`(isteğe bağlı) - atlamak için; arama sonuçları sayısı 100. 000 ' büyük olamaz. Belgeleri sırayla taraması gereken ancak kullanamazsınız `$skip` bu sınırlama nedeniyle kullanmayı `$orderby` tamamen sıralı anahtar ve `$filter` bir aralık sorgu yerine.

> [!NOTE]
> Çağrılırken **arama** POST kullanarak, bu parametre adlı `skip` yerine `$skip`.
> 
> 

`$top=#`(isteğe bağlı) - almak için arama sonuçları sayısı. Bu ile birlikte kullanılabilir `$skip` arama sonuçlarının istemci-tarafı Sayfalaması uygulamak üzere.

> [!NOTE]
> Çağrılırken **arama** POST kullanarak, bu parametre adlı `top` yerine `$top`.
> 
> 

`$count=true|false`(isteğe bağlı, varsayılan olarak `false`)-sonuçları toplam hata sayısı fetch belirtir. Eşleşen tüm belgeleri sayısıdır `search` ve `$filter` yoksayılıyor parametreleri `$top` ve `$skip`. Bu değeri ayarlamak `true` bir performans etkisi olabilir. Döndürülen sayısı yaklaşık olduğuna dikkat edin.

> [!NOTE]
> Çağrılırken **arama** POST kullanarak, bu parametre adlı `count` yerine `$count`.
> 
> 

`$orderby=[string]`(isteğe bağlı) - sonuçlarına göre sıralamak için ifadeleri virgülle ayrılmış listesi. Her bir ifadenin bir alan adı veya yapılan bir çağrı olabilir `geo.distance()` işlevi. Her deyim tarafından izlenebilir `asc` belirtildiği için artan ve `desc` azalan belirtmek için. Varsayılan, artan. TIES belgeleri eşleşmiyor puan tarafından ayrılmış olarak gösterilir. Öyle değilse `$orderby` belirtilmemişse, varsayılan sıralama düzeni belge eşleşmiyor puan göre azalan. Bir sınır için 32 yan `$orderby`.

> [!NOTE]
> Çağrılırken **arama** POST kullanarak, bu parametre adlı `orderby` yerine `$orderby`.
> 
> 

`$select=[string]`(isteğe bağlı) - almak için bir alanlar virgülle ayrılmış listesi. Belirtilmezse, şema olarak alınabilir işaretlenmiş tüm alanlar dahil edilir. Bu parametre ayarlayarak tüm alanları açıkça isteyebilir `*`.

> [!NOTE]
> Çağrılırken **arama** POST kullanarak, bu parametre adlı `select` yerine `$select`.
> 
> 

`facet=[string]`(sıfır veya daha fazla) - modeli tarafından bir alan. Dize virgülle ayrılmış olarak ifade edilen olduğunu özelleştirmek için parametreler isteğe bağlı olarak içerebilir `name:value` çiftleri. Geçerli Parametreler şunlardır:

* `count`(en fazla sayıda modeli şartı; varsayılan değer 10). Maksimum yok yoktur, ancak özellikle çok sayıda benzersiz koşulları modellenmiş alan içeriyorsa, daha yüksek değerleri karşılık gelen bir performans cezasına sebep.
  * Örneğin: `facet=category,count:5` modeli sonuçlarında ilk beş kategorileri alır.  
  * **Not**: varsa `count` parametre benzersiz koşulları sayısından daha az ve sonuçları doğru olmayabilir. Olduğunu sorgular parça dağıtılan şekilde nedeni budur. Artan `count` genellikle terim sayıları, ancak performans maliyeti doğruluğu artırır.
* `sort`(birini `count` sıralamak için *azalan* sayıma göre `-count` sıralamak için *artan* sayıma göre `value` sıralamak için *artan* değeri ya da `-value` sıralamak için *azalan* değeriyle)
  * Örneğin: `facet=category,count:3,sort:count` her şehir adı belgelerle sayısına göre azalan sırada modeli sonuçlarında ilk üç kategorileri alır. Örneğin, en iyi üç kategorileri şunlardır: Bütçe, Motel ve lüks ve bütçe 5 isabet varsa, 6 Motel sahip ve 4 lüks sahip, ardından demet sırayla olacaktır Motel, bütçe, lüks.
  * Örneğin: `facet=rating,sort:-value` değerine göre azalan sırada tüm olası derecelendirmeler aralıklarını üretir. Derecelendirmeleri 1'den 5'e, örneğin, demet sıralanır 5, 4, 3, 2, kaç tane belgeleri her derecelendirme eşleşen yedeklemiş 1.
* `values`(kanal ayrılmış sayısal veya `Edm.DateTimeOffset` değerleri model girdi değerleri dinamik bir dizi belirtme)
  * Örneğin: `facet=baseRate,values:10|20` üç demet oluşturur: biri kadar temel oranı 0 ancak 10, 10 için bir tane kadar dahil değildir, ancak 20 dahil değil, diğeri 20 veya daha yüksek.
  * Örneğin: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` iki demet oluşturur: biri Şubat 2010'dan önce renovated Oteller ve Otel için bir renovated Şubat 2010 veya üzeri 1.
* `interval`(sayı, 0'dan büyük tamsayı aralığı veya `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` tarih saat değerleri için)
  * Örneğin: `facet=baseRate,interval:100` demet boyutu 100 temel oranı aralıklarını temel alarak oluşturur. Örneğin, temel oranları tüm $60 ve $600 arasında varsa, olacaktır 0-100, 100-200, 200-300, 400 300, 400-500 ve 500-600 aralıklarını.
  * Örneğin: `facet=lastRenovationDate,interval:year` bir demet zaman Oteller renovated her yıl üretir.
* `timeoffset`([+-] hh: mm, [+-] ssdd, veya [+-] hh) `timeoffset` isteğe bağlıdır. İle yalnızca birleştirilebilir `interval` seçeneği ve yalnızca türünde bir alan uygulandığında `Edm.DateTimeOffset`. Değeri, zaman sınırları ayarı hesabına UTC saat konumu belirtir.
  * Örneğin: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` 01:00:00 UTC (hedef saat diliminde gece yarısı) konumunda başlayan günlük sınır kullanır
* **Not**: `count` ve `sort` aynı modeli belirtiminde birleştirilebilir, ancak ile birleştirilemez `interval` veya `values`, ve `interval` ve `values` birlikte birleştirilemez.
* **Not**: tarih saat aralığı nedeni modellerinin hesaplanan UTC saat tabanlı `timeoffset` belirtilmedi. Örneğin: için `facet=lastRenovationDate,interval:day`, günlük sınır 00:00:00 UTC başlatır. 

> [!NOTE]
> Çağrılırken **arama** POST kullanarak, bu parametre adlı `facets` yerine `facet`. Ayrıca, bunu burada her ayrı modeli ifade dizesidir dizeleri bir JSON dizisi olarak belirtin.
> 
> 

`$filter=[string]`(isteğe bağlı) - standart OData sözdiziminde yapılandırılmış arama ifadesi.

> [!NOTE]
> Çağrılırken **arama** POST kullanarak, bu parametre adlı `filter` yerine `$filter`.
> 
> 

`highlight=[string]`(isteğe bağlı) - virgülle ayrılmış bir alan adları isabet için kullanılan bir dizi vurgular. Yalnızca `searchable` alanları isabet vurgulama için kullanılabilir.

`highlightPreTag=[string]`(isteğe bağlı, varsayılan olarak `<em>`) - bir dize vurgular isabet başına etiketi. İle ayarlanmalıdır `highlightPostTag`.

> [!NOTE]
> Çağrılırken **arama** GET kullanarak, URL'deki ayrılmış karakterler (örneğin, % &#23; yerine) yüzde olarak kodlanmış olmalıdır.
> 
> 

`highlightPostTag=[string]`(isteğe bağlı, varsayılan olarak `</em>`)-vurgular isabet ekler bir dize etiketi. İle ayarlanmalıdır `highlightPreTag`.

> [!NOTE]
> Çağrılırken **arama** GET kullanarak, URL'deki ayrılmış karakterler (örneğin, % &#23; yerine) yüzde olarak kodlanmış olmalıdır.
> 
> 

`scoringProfile=[string]`(isteğe bağlı) - değerlendirmek için bir Puanlama profili adıyla aynı sonuçları sıralamak için eşleşen belgelerin puanlarını.

`scoringParameter=[string]`(sıfır veya daha fazla) - bir Puanlama işlevi tanımlanan her parametre için değerler belirtir (örneğin, `referencePointParameter`) biçimini kullanarak `name-value1,value2,...`.

* Örneğin, Puanlama profili "mylocation" adlı bir parametre ile bir işlev tanımlıyorsa sorgu dizesi seçeneği olması `&scoringParameter=mylocation--122.2,44.8`. İkinci tire ilk değer (Bu örnekte boylam) bir parçası olsa da ilk tire değer listesinden adını ayırır.
* Puanlama parametrelerini etiketi, artırmanın virgüller, içerebilir gibi böyle bir değer tek tırnak kullanılarak listesinde kaçış. Değer kendilerini tek tırnak içermiyorsa, katlama tarafından kaçış
  * Örneğin, "Hello, O'Brien" ve "Smith", sorgu "mytag" adlı bir parametreye artırmanın bir etiketi olması ve etiket artırmak istiyorsanız değerleri dize seçenek olacaktır `&scoringParameter=mytag-'Hello, O''Brien',Smith`. Tırnak işaretleri yalnızca olduğuna dikkat edin virgül içeren değerler için gereklidir.

> [!NOTE]
> Çağrılırken **arama** POST kullanarak, bu parametre adlı `scoringParameters` yerine `scoringParameter`. Bir JSON dizisini ayrı bir olduğu her bir dize belirttiğiniz ayrıca `name-values` çifti.
> 
> 

`minimumCoverage`(isteğe bağlı, 100 varsayılanlara)-0 ile 100 sırada başarılı bildirilecek sorgu için bir arama sorgusu tarafından ele alınması gereken dizin yüzdesini gösteren arasında bir sayı. Varsayılan olarak, tüm dizini kullanılabilir olmalıdır veya `Search` 503 HTTP durum kodunu döndürür. Ayarlarsanız `minimumCoverage` ve `Search` başarılı, HTTP 200 dönün ve içeren bir `@search.coverage` sorguda eklenmiştir dizin yüzdesini gösteren yanıt değeri.

> [!NOTE]
> Bu parametre için bir değer 100'den daha düşük ayarı yalnızca bir çoğaltma hizmetleriyle için bile arama kullanılabilirlik sağlamak için yararlı olabilir. Ancak, tüm eşleşen belgeleri arama sonuçlarında mevcut olması garanti. Arama geri çağırma önemlidir kullanılabilirlik uygulamanızı daha sonra çıkmak en iyisidir `minimumCoverage` zaman, varsayılan değer 100.
> 
> 

`api-version=[string]`(gerekli). Önizleme sürümü `api-version=2015-02-28-Preview`. Bkz: [arama hizmet sürümü oluşturma](http://msdn.microsoft.com/library/azure/dn864560.aspx) Ayrıntılar ve diğer sürümleri için.

Not: Bu işlem için `api-version` olup, çağrı bağımsız olarak URL'deki sorgu parametresi olarak belirtilen **arama** GET veya POST ile.

**İstek üstbilgileri**

Aşağıdaki listede gerekli ve isteğe bağlı İstek üstbilgilerinin açıklanmaktadır.

* `api-key``api-key` İsteğin arama hizmetiniz için kimlik doğrulaması için kullanılır. Hizmet URL'nizi benzersiz bir dize değeri değil. **Arama** isteği, bir yönetici anahtarını veya sorgu anahtarı belirtebilirsiniz `api-key`.

İstek URL'si oluşturmak için hizmet adı da gerekir. Hizmet adını alabilir ve `api-key` Azure Portalı'nda, hizmet panosundan. Bkz: [portalda Azure Search hizmeti oluşturma](search-create-service-portal.md) sayfa gezinti Yardım.

**İstek gövdesi**

GET için: yok.

POST için:

    {
      "count": true | false (default),
      "facets": [ "facet_expression_1", "facet_expression_2", ... ],
      "filter": "odata_filter_expression",
      "highlight": "highlight_field_1, highlight_field_2, ...",
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 100),
      "moreLikeThis": "document_key" (mutually exclusive with "search" parameter),
      "orderby": "orderby_expression",
      "scoringParameters": [ "scoring_parameter_1", "scoring_parameter_2", ... ],
      "scoringProfile": "scoring_profile_name",
      "search": "simple_query_expression",
      "searchFields": "field_name_1, field_name_2, ...",
      "searchMode": "any" (default) | "all",
      "select": "field_name_1, field_name_2, ...",
      "skip": # (default 0),
      "top": #
    }

**Kısmi arama yanıtları devamlılığı**

Bazen Azure Search tüm istenen sonuçları tek bir arama yanıtta döndüremez. Bu sorgu çok fazla belgeleri değil belirterek isteğinde bulunduğunda gibi farklı nedenlerle oluşabilir `$top` veya için bir değer belirterek `$top` , çok büyük. Böyle durumlarda, Azure Search içerecektir `@odata.nextLink` yanıt gövdesi içinde ek açıklama ve ayrıca `@search.nextPageParameters` bir POST isteği ise. Bu ek açıklamalar değerlerini arama yanıtı sonraki bölümü almak için başka bir arama isteğine formüle için kullanabilirsiniz. Bu adlı bir ***devamlılık*** özgün araması genellikle istek ve ek açıklamalar denir ***devamlılık belirteçleri***. Bkz: [örnek](#SearchResponse) bu açıklamalarının ve yanıt gövdesi içinde nerede göründüklerinden sözdizimi hakkında bilgi. 

Azure Search devamlılık belirteçleri neden döndürebilir nedeniyle uygulama özgü ve değiştirilebilir. Sağlam istemcileri her zaman durumlarda beklenenden daha az belgeler döndürülür ve belgeleri alınırken devam etmek için devamlılık belirteci eklenmiştir işlemeye hazır olmalıdır. Ayrıca, aynı HTTP yöntemi özgün istek devam edebilmeniz için kullanmanız gerektiğini unutmayın. Bir GET isteği gönderirse, örneğin, gönderdiğiniz herhangi bir devamlılık isteğini de GET kullanmanız gerekir (ve sonrası için benzer şekilde).

<a name="SearchResponse"></a>
**Yanıt**

Durum kodu: 200 Tamam için başarılı bir yanıt döndürdü.

    {
      "@odata.count": # (if $count=true was provided in the query),
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "@search.facets": { (if faceting was specified in the query)
        "facet_field": [
          {
            "value": facet_entry_value (for non-range facets),
            "from": facet_entry_value (for range facets),
            "to": facet_entry_value (for range facets),
            "count": number_of_documents
          }
        ],
        ...
      },
      "@search.nextPageParameters": { (request body to fetch the next page of results if not all results could be returned in this response and Search was called with POST)
        "count": ... (value from request body if present),
        "facets": ... (value from request body if present),
        "filter": ... (value from request body if present),
        "highlight": ... (value from request body if present),
        "highlightPreTag": ... (value from request body if present),
        "highlightPostTag": ... (value from request body if present),
        "minimumCoverage": ... (value from request body if present),
        "moreLikeThis": ... (value from request body if present),
        "orderby": ... (value from request body if present),
        "scoringParameters": ... (value from request body if present),
        "scoringProfile": ... (value from request body if present),
        "search": ... (value from request body if present),
        "searchFields": ... (value from request body if present),
        "searchMode": ... (value from request body if present),
        "select": ... (value from request body if present),
        "skip": ... (page size plus value from request body if present),
        "top": ... (value from request body if present minus page size),
      },
      "value": [
        {
          "@search.score": document_score (if a text query was provided),
          "@search.highlights": {
            field_name: [ subset of text, ... ],
            ...
          },
          key_field_name: document_key,
          field_name: field_value (retrievable fields or specified projection),
          ...
        },
        ...
      ],
      "@odata.nextLink": (URL to fetch the next page of results if not all results could be returned in this response; Applies to both GET and POST)
    }

**Örnekler:**

Ek örnekler bulabilirsiniz [OData ifade sözdizimini Azure arama için](https://msdn.microsoft.com/library/azure/dn798921.aspx) sayfası.

1)    Arama dizini tarihe göre azalan sırada sıralanır.

    GET /indexes/hotels/docs? arama = * & $orderby lastRenovationDate desc & api-version = 2015-02-28-Önizleme =

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Önizleme {"Ara": "*", "orderby": "lastRenovationDate desc"}

2)    Modellenmiş bir arama dizinini aramak ve modelleri için kategoriler, derecelendirme, etiketleri yanı sıra belirli aralıklara baseRate öğeleriyle almak:

    GET /indexes/hotels/docs? arama = test & modeli = kategori & modeli = derecelendirme & modeli etiketleri & modeli = baseRate, değerleri: 80 = | 150 | 220 & API sürümü 2015-02-28-Önizleme =

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Önizleme {"Ara": "test", "modelleri": ["kategorisi", "Derecelendirme", "etiketler", "baseRate, değerleri: 80 | 150 | 220"]}

3)    Bir filtre kullanarak daraltmak önceki modellenmiş sorgu sonuçları üzerinde kullanıcı tıklattıktan sonra 3 ve kategori "Motel" derecelendirme:

    GET /indexes/hotels/docs? arama = test & modeli etiketleri & modeli = baseRate, değerleri: 80 = | 150 | 220 & $filter & derecelendirme eq 3 ve kategori eq 'Motel' api-version = 2015-02-28-Önizleme =

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Önizleme {"Ara": "test", "modelleri": ["etiketleri", "baseRate, değerleri: 80 | 150 | 220"], "filtre": "Derecelendirme eq 3 ve kategori eq 'Motel'"}

4) Modellenmiş bir aramada bir sorgudan döndürülen benzersiz koşulları bir üst sınır ayarlayın. Varsayılan değer 10'dur, ancak artırabilir ya da bu değeri kullanılarak azaltmak `count` parametresini `facet` özniteliği:

    GET /indexes/hotels/docs? arama = test & modeli Şehir, sayısı: 5 & api-version = 2015-02-28-Önizleme =

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Önizleme {"Ara": "test", "modelleri": ["Şehir, sayısı: 5"]}

5)    Belirli alanları dizin arama; Dile özgü alanı:

    GET /indexes/hotels/docs? arama hôtel & searchFields = description_fr & api-version = 2015-02-28-Önizleme =

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Önizleme {"Ara": "hôtel", "searchFields": "description_fr"}

6) Dizin arasında birden çok alan arayın. Örneğin, depolamak ve tüm aynı dizin içinde birden çok dilde aranabilir alanlara sorgulayabilirsiniz.  İngilizce ve Fransızca açıklamaları aynı belgede birlikte bulunması durumunda veya tamamını sorgu sonuçlarında döndürebilirsiniz:

    GET /indexes/hotels/docs? arama otel & searchFields = açıklama, description_fr & api-version = 2015-02-28-Önizleme =

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Önizleme {"Ara": "otel", "searchFields": "açıklama, description_fr"}

Aynı anda yalnızca bir dizin sorgulanabilir unutmayın. Bir seferde bir sorgu planlamıyorsanız her dil için birden çok dizin oluşturmayın.

7)    Disk belleği - Get öğe 1 sayfasının (sayfa boyutu Dur 10):

    GET /indexes/hotels/docs? arama = * & $skip = 0 & $top = 10 & api-version = 2015-02-28-Önizleme

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Önizleme {"Ara": "*", "Atla": 0, "üst": 10}

8)    Disk belleği - Get öğe 2 sayfasının (sayfa boyutu Dur 10):

    GET /indexes/hotels/docs? arama = * & $skip = 10 & $top = 10 & api-version = 2015-02-28-Önizleme

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Önizleme {"Ara": "*", "Atla": "üst" 10: 10}

9)    Belirli bir alan kümesi Al:

    GET /indexes/hotels/docs? arama = * & $select hotelName, açıklama ve api-version = 2015-02-28-Önizleme =

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Önizleme {"Ara": "*", "Seç": "hotelName, açıklaması"}

10)  Bir özel filtre ifadesi eşleşen belgeler Al:

    GET /indexes/hotels/docs? $filter = (baseRate ge 60 ve baseRate lt 300) veya hotelName eq 'Süslü kalmak' & api-version = 2015-02-28-Önizleme

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Önizleme {"filtre": "(baseRate ge 60 ve baseRate lt 300) veya hotelName eq 'Süslü Kal'"}

11) İsabet vurgular dizin ve return parçaları arama

    GET /indexes/hotels/docs? arama bir şey = & vurgulayın açıklama & api-version = 2015-02-28-Önizleme =

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Önizleme {"Ara": "bir şey", "highlight": "Açıklama"}

12) Dizin arama ve yakın bir başvuru çıktığınızda uzağına konumdan sıralanmış belgeleri döndürür

    GET /indexes/hotels/docs? arama bir şey = & $orderby=geo.distance (konum, geography'POINT(-122.12315 47.88121)') & api-version = 2015-02-28-Önizleme

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Önizleme {"Ara": "bir şey", "orderby": "geo.distance (konum, geography'POINT(-122.12315 47.88121)')"}

13) "Coğrafi" adlı bir Puanlama profili olduğunu varsayarak dizininde iki uzaklık Puanlama işlevleri ile arama, "currentLocation" ve "lastLocation" adlı bir parametreyi tanımlayan bir adı verilen bir parametre tanımlama

    GET /indexes/hotels/docs? arama bir şey = & scoringProfile = coğrafi & scoringParameter currentLocation--122.123,44.77233 & scoringParameter = lastLocation--121.499,44.2113 & api-version = 2015-02-28-Önizleme =

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Önizleme {"Ara": "bir şey", "scoringProfile": "coğrafi", "scoringParameters": ["currentLocation--122.123,44.77233", "lastLocation--121.499,44.2113"]}

14) Dizini kullanarak bulmanın [Basit Sorgu söz dizimi](https://msdn.microsoft.com/library/dn798920.aspx). Bu sorgu burada koşulları "rahatlık" ve "Konum" ancak değil "motel" aranabilir alanları içeren Oteller döndürür:

    GET /indexes/hotels/docs? arama = rahatlık + konumu-motel & searchMode = all & api-version = 2015-02-28-Önizleme

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Önizleme {"Ara": "rahatlık + konumu-motel", "searchMode": "tümü"}

Kullanımına dikkat edin `searchMode=all` üstünde. Bu parametreyi dahil olmak üzere varsayılan değerini geçersiz kılar `searchMode=any`, sağlama, `-motel` "Ve değil" yerine "Veya değil" anlamına gelir. Olmadan `searchMode=all`"Veya, arama sonuçları kısıtlayan yerine genişletir değil" alın ve bu, bazı kullanıcılara counter-intuitive olabilir.

15) Dizini kullanarak bulmanın [lucene sorgu söz dizimi](https://msdn.microsoft.com/library/mt589323.aspx). Bu sorgu hotels "Bütçe" terimi kategori alanı, içerdiği ve "son renovated" ifadesini içeren tüm aranabilir alanları döndürür. "En son renovated" ifadesini içeren belgeleri yüksek terim artırma değeri (3) sonucunda derecelendirilir.

    GET /indexes/hotels/docs? arama Kategori: Bütçe = ve \"son renovated\"^ 3 & searchMode = all & api-version = 2015-02-28-Önizleme & querytype tam =

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Önizleme {"Ara": "Kategori: Bütçe ve \"son renovated\"^ 3", "queryType": "tam", "searchMode": "tümü"}

<a name="LookupAPI"></a>

## <a name="lookup-document"></a>Arama belge
**Arama belge** işlemi Azure aramadan belge alır. Bu, belirli arama sonucu kullanıcı ve bu belge hakkında belirli diğer ayrıntıları aramak istediğinizde kullanışlıdır.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/[key]?[query parameters]
    api-key: [admin or query key]

**İstek**

HTTPS istekleri için gereklidir. **Arama belge** isteği oluşturulmasını gibi.

    GET /indexes/[index name]/docs/key?[query parameters]

Alternatif olarak, anahtar arama için geleneksel OData sözdizimini kullanabilirsiniz:

    GET /indexes('[index name]')/docs('[key]')?[query parameters]

İstek URI'si [dizin adı] içerir ve [Temel], hangi dizinden almak için hangi belge belirtme. Ayrıca, aynı anda yalnızca bir belge alabilir. Kullanım **arama** tek bir istekte birden çok belge alınamıyor.

**Sorgu parametreleri**

`$select=[string]`(isteğe bağlı) - almak için bir alanlar virgülle ayrılmış listesi. Belirtilmemiş veya kümesine `*`, şemada alınabilir olarak işaretlenmiş tüm alanlar projeksiyon dahil edilir.

`api-version=[string]`(gerekli). Önizleme sürümü `api-version=2015-02-28-Preview`. Bkz: [arama hizmet sürümü oluşturma](http://msdn.microsoft.com/library/azure/dn864560.aspx) Ayrıntılar ve diğer sürümleri için.

Not: Bu işlem için `api-version` sorgu parametresi olarak belirtilir.

**İstek üstbilgileri**

Aşağıdaki listede gerekli ve isteğe bağlı İstek üstbilgilerinin açıklanmaktadır.

* `api-key``api-key` İsteğin arama hizmetiniz için kimlik doğrulaması için kullanılır. Hizmet URL'nizi benzersiz bir dize değeri değil. **Arama belge** isteği, bir yönetici anahtarını veya sorgu anahtarı belirtebilirsiniz `api-key`.

İstek URL'si oluşturmak için hizmet adı da gerekir. Hizmet adını alabilir ve `api-key` Azure Portalı'nda, hizmet panosundan. Bkz: [portalda Azure Search hizmeti oluşturma](search-create-service-portal.md) sayfa gezinti Yardım.

**İstek gövdesi**

yok.

**Yanıt**

Durum kodu: 200 Tamam için başarılı bir yanıt döndürdü.

    {
      field_name: field_value (fields matching the default or specified projection)
    }

**Örnek**

Arama anahtarı '2' olan belge

    GET /indexes/hotels/docs/2?api-version=2015-02-28-Preview

Arama 'OData sözdizimini kullanarak 3' anahtarına sahip belge:

    GET /indexes('hotels')/docs('3')?api-version=2015-02-28-Preview

<a name="CountDocs"></a>

## <a name="count-documents"></a>Count belgeleri
**Sayısı belgeleri** işlemi arama dizini belgelerde sayısını alır. `$count` Sözdizimi OData protokolü bir parçasıdır.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/$count?api-version=[api-version]
    Accept: text/plain
    api-key: [admin or query key]

**İstek**

HTTPS istekleri için gereklidir. **Sayısı belgeleri** isteği GET yöntemini kullanan olacak oluşturulan.

İstek URI'SİNDEKİ [dizin adı] belirtilen dizin belgeleri koleksiyonundaki tüm öğelerin sayısını döndürülecek hizmetin söyler.

`api-version=[string]`(gerekli). Önizleme sürümü `api-version=2015-02-28-Preview`. Bkz: [arama hizmet sürümü oluşturma](http://msdn.microsoft.com/library/azure/dn864560.aspx) Ayrıntılar ve diğer sürümleri için.

**İstek üstbilgileri**

Aşağıdaki listede gerekli ve isteğe bağlı İstek üstbilgilerinin açıklanmaktadır.

* `Accept`: Bu değer ayarlamak `text/plain`.
* `api-key``api-key` İsteğin arama hizmetiniz için kimlik doğrulaması için kullanılır. Hizmet URL'nizi benzersiz bir dize değeri değil. **Sayısı belgeleri** isteği, bir yönetici anahtarını veya sorgu anahtarı belirtebilirsiniz `api-key`.

İstek URL'si oluşturmak için hizmet adı da gerekir. Hizmet adını alabilir ve `api-key` Azure Portalı'nda, hizmet panosundan. Bkz: [portalda Azure Search hizmeti oluşturma](search-create-service-portal.md) sayfa gezinti Yardım.

**İstek gövdesi**

yok.

**Yanıt**

Durum kodu: 200 Tamam için başarılı bir yanıt döndürdü.

Yanıt gövdesi, düz metin olarak biçimlendirilmiş bir tamsayı olarak sayısı değeri içerir.

<a name="Suggestions"></a>

## <a name="suggestions"></a>Öneriler
**Önerileri** işlemi kısmi arama girişinize göre önerileri alır. Bu genellikle, arama kutularında kullanıcılar arama terimi girerek yazarken tamamlanan öneriler sağlamak için kullanılır.

Öneri istekleri hedef belgeler, birden çok adayı belge giriş aynı arama eşleşiyorsa önerilen metin yinelenebilir şekilde öneren adresindeki hedefleyin. Kullanabileceğiniz `$select` , her bir öneri kaynağı belgedir yapıp yapmadığını (belge anahtar dahil) diğer belge alanları alınamadı.

A **önerileri** işlem bir GET veya POST isteği verildi.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/suggest?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/suggest?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**POST yerine GET kullanma zamanı**

Kullandığınızda, HTTP GET çağırmak için **önerileri** API, gereken isteği URL uzunluğu 8 KB'yi aşamaz unutmayın. Çoğu uygulama için yeterince genellikle budur. Ancak, bazı uygulamalar çok büyük sorgular, özellikle OData filtre ifadeleri üretir. GET daha büyük filtreleri sağladığından bu uygulamalar için HTTP POST kullanılarak daha iyi bir seçimdir. POST ile bir filtre yan tümcelerinde POST isteği boyut sınırını yaklaşık 16 MB olduğundan ham filtre dizesi boyutu sınırlama faktörü sayısıdır.

> [!NOTE]
> POST isteği boyut sınırı çok büyük olsa da, filtre ifadeleri rasgele karmaşık olamaz. Bkz: [OData ifade sözdizimini](https://msdn.microsoft.com/library/dn798921.aspx) filtre karmaşıklık sınırlamalar hakkında daha fazla bilgi.
> 
> 

**İstek**

HTTPS istekleri için gereklidir. **Önerileri** isteği GET veya POST yöntemleri kullanılarak olacak oluşturulan.

İsteğin URI sorgu için dizin adını belirtir. Kısmen giriş arama terimi gibi parametreleri GET istekleri durumunda sorgu dizesinde belirtilen ve gövde POST söz konusu olduğunda istek ister.

GET isteklerini oluştururken en iyi uygulama, unutmayın [URL kodlama](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) REST API doğrudan çağrılırken belirli sorgu parametreleri. İçin **önerileri** bu işlemleri içerir:

* `$filter`
* `highlightPreTag`
* `highlightPostTag`
* `search`

URL kodlaması yalnızca yukarıdaki sorgu parametreleri önerilir. Varsa, yanlışlıkla URL kodlama tüm sorgu dizesi (sonra her şeyi?), istekleri bozar.

Ayrıca, URL kodlaması yalnızca GET kullanarak doğrudan REST API çağrılırken gereklidir. Hiçbir URL kodlaması çağrılırken gereklidir **önerileri** POST, kullanma ya da kullanırken [.NET istemci kitaplığını](https://msdn.microsoft.com/library/dn951165.aspx), sizin için URL kodlaması işler.

**Sorgu parametreleri**

**Öneriler** sorgu ölçütlerini sağlayan ve ayrıca arama davranışını belirtin birkaç parametre kabul eder. Bu parametreler URL'deki sorgu dizesi çağrılırken sağladığınız **önerileri** GET aracılığıyla ve çağrılırken istek gövdesinde JSON özellikleri olarak **önerileri** POST ile. Bazı parametreler sözdizimi GET ve POST arasında biraz farklıdır. Bu farklılıklar, geçerli aşağıda belirtilmiştir:

`search=[string]`-sorguları önermek üzere kullanmak için arama metni. En az 1 karakter ve en fazla 100 karakter olmalıdır.

`highlightPreTag=[string]`(isteğe bağlı) - bir dize etiketi, isabet aramak için başına. İle ayarlanmalıdır `highlightPostTag`.

> [!NOTE]
> Çağrılırken **önerileri** GET kullanarak, URL'deki ayrılmış karakterler (örneğin, % &#23; yerine) yüzde olarak kodlanmış olmalıdır.
> 
> 

`highlightPostTag=[string]`(isteğe bağlı) - bir dize etiketi, isabet aramak için ekler. İle ayarlanmalıdır `highlightPreTag`.

> [!NOTE]
> Çağrılırken **önerileri** GET kullanarak, URL'deki ayrılmış karakterler (örneğin, % &#23; yerine) yüzde olarak kodlanmış olmalıdır.
> 
> 

`suggesterName=[string]`-belirtildiği gibi öneri aracı adını `suggesters` dizin tanımı parçası olan koleksiyonu. A `suggester` hangi alanlar için önerilen sorgu terimlerinin taranır belirler. Bkz: [ilgili](#Suggesters) Ayrıntılar için.

`fuzzy=[boolean]`(isteğe bağlı, varsayılan = false)-olsa bile bir yedek veya eksik karakter arama metni olarak ayarlandığında bu API true olarak öneriler bulacaksınız. Bu bazı senaryolarda daha iyi bir deneyim sağlarken benzer bir öneri aramalar daha yavaş ve daha fazla kaynak tüketmesine maliyeti performanslarının gelir.

`searchFields=[string]`(isteğe bağlı): Belirtilen arama metni aramak için virgülle ayrılmış bir alan adları listesi. Hedef alan önerileri için etkinleştirilmesi gerekir.

`$top=#`(isteğe bağlı, varsayılan = 5)-almak için öneriler sayısı. 1 ile 100 arasında bir sayı olmalıdır.

> [!NOTE]
> Çağrılırken **önerileri** POST kullanarak, bu parametre adlı `top` yerine `$top`.
> 
> 

`$filter=[string]`(isteğe bağlı) - belgeleri filtreleyen bir ifade için öneriler kabul edilir.

> [!NOTE]
> Çağrılırken **önerileri** POST kullanarak, bu parametre adlı `filter` yerine `$filter`.
> 
> 

`$orderby=[string]`(isteğe bağlı) - sonuçlarına göre sıralamak için ifadeleri virgülle ayrılmış listesi. Her bir ifadenin bir alan adı veya yapılan bir çağrı olabilir `geo.distance()` işlevi. Her deyim tarafından izlenebilir `asc` belirtildiği için artan ve `desc` azalan belirtmek için. Varsayılan, artan. Bir sınır için 32 yan `$orderby`.

> [!NOTE]
> Çağrılırken **önerileri** POST kullanarak, bu parametre adlı `orderby` yerine `$orderby`.
> 
> 

`$select=[string]`(isteğe bağlı) - almak için bir alanlar virgülle ayrılmış listesi. Belirtilmezse, yalnızca belge anahtarını ve öneri metni döndürülür. Bu parametre ayarlayarak tüm alanları açıkça isteyebilir `*`.

> [!NOTE]
> Çağrılırken **önerileri** POST kullanarak, bu parametre adlı `select` yerine `$select`.
> 
> 

`minimumCoverage`(isteğe bağlı, varsayılan olarak 80)-0 ve öneriler sorgu başarılı bildirilecek sorgu için sırasıyla tarafından ele alınması gereken dizin yüzdesini gösteren 100 arasında bir sayı. Varsayılan olarak, dizin % 80'en az kullanılabilir olmalıdır veya `Suggest` 503 HTTP durum kodunu döndürür. Ayarlarsanız `minimumCoverage` ve `Suggest` başarılı, HTTP 200 dönün ve içeren bir `@search.coverage` sorguda eklenmiştir dizin yüzdesini gösteren yanıt değeri.

> [!NOTE]
> Bu parametre için bir değer 100'den daha düşük ayarı yalnızca bir çoğaltma hizmetleriyle için bile arama kullanılabilirlik sağlamak için yararlı olabilir. Ancak, tüm eşleşen öneri sonuçlarında mevcut olması garanti. Geri çağırma önemlidir kullanılabilirlik uygulamanızı daha sonra değil düşürmek en iyisidir `minimumCoverage` varsayılan değerini 80 aşağıda.
> 
> 

`api-version=[string]`(gerekli). Önizleme sürümü `api-version=2015-02-28-Preview`. Bkz: [arama hizmet sürümü oluşturma](http://msdn.microsoft.com/library/azure/dn864560.aspx) Ayrıntılar ve diğer sürümleri için.

Not: Bu işlem için `api-version` olup, çağrı bağımsız olarak URL'deki sorgu parametresi olarak belirtilen **önerileri** GET veya POST ile.

**İstek üstbilgileri**

Gerekli ve isteğe bağlı İstek üstbilgilerinin aşağıdaki listede açıklanmaktadır

* `api-key``api-key` İsteğin arama hizmetiniz için kimlik doğrulaması için kullanılır. Hizmet URL'nizi benzersiz bir dize değeri değil. **Önerileri** isteği, bir yönetici anahtarını veya sorgu anahtarı olarak belirtebilirsiniz `api-key`.

İstek URL'si oluşturmak için hizmet adı da gerekir. Hizmet adını alabilir ve `api-key` Azure Portalı'nda, hizmet panosundan. Bkz: [portalda Azure Search hizmeti oluşturma](search-create-service-portal.md) sayfa gezinti Yardım.

**İstek gövdesi**

GET için: yok.

POST için:

    {
      "filter": "odata_filter_expression",
      "fuzzy": true | false (default),
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 80),
      "orderby": "orderby_expression",
      "search": "partial_search_input",
      "searchFields": "field_name_1, field_name_2, ...",
      "select": "field_name_1, field_name_2, ...",
      "suggesterName": "suggester_name",
      "top": # (default 5)
    }

**Yanıt**

Durum kodu: 200 Tamam için başarılı bir yanıt döndürdü.

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
        },
        ...
      ]
    }

Dizinin her bir öğesinde bulunan alanları almak için yansıtma seçeneği kullandıysanız:

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
          <other projected data fields>
        },
        ...
      ]
    }

**Örnek**

Kısmi arama giriş 'lux' olduğu 5 önerileri Al

    GET /indexes/hotels/docs/suggest?search=lux&$top=5&suggesterName=sg&api-version=2015-02-28-Preview

    POST /indexes/hotels/docs/suggest?api-version=2015-02-28-Preview
    {
      "search": "lux",
      "top": 5,
      "suggesterName": "sg"
    }
