---
title: Bir dizin tanımını ve kavramlar - Azure Search oluşturma
description: Dizin terimleri ve kavramları bileşen parçalarına ve fiziksel yapısı gibi Azure Search giriş.
author: HeidiSteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.topic: conceptual
ms.date: 02/13/2019
ms.custom: seodec2018
ms.openlocfilehash: 9cd43172fc57443cc89f238e1d4ffaae45301936
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/16/2019
ms.locfileid: "56330571"
---
# <a name="create-a-basic-index-in-azure-search"></a>Azure Search'te bir temel dizin oluşturma

Azure Search'te bir *dizin* kalıcı bir deposudur *belgeleri* ve Azure Search Hizmeti filtrelenmiş ve tam metin araması için kullanılan diğer yapılar. Kavramsal olarak, bir belge, dizininizdeki aranabilir verilerin tek bir birimdir. Örneğin, bir e-ticaret satıcısında sattığı her bir öğe için bir belge, bir haber kuruluşunda her bir makale için bir belge, vb. olabilir. Bu kavramları daha çok bilinen veritabanı eşdeğerlerine eşleyen bir *dizin*, kavramsal olarak bir *tabloya* benzer ve *belgeler* de bir tablodaki *satırlarla* kabaca eşdeğerdir.

Eklediğinizde veya dizin karşıya yükleme fiziksel yapıları sağladığınız şemasını temel alan Azure Search oluşturur. Örneğin, dizininizdeki bir alanın aranabilir olarak işaretlenmişse, bu alan için ters dizin oluşturulur. Daha sonra eklediğinizde veya belgeleri karşıya yüklemesine veya Azure Search'e arama sorguları göndermek için belirli bir dizin arama hizmetinizdeki istekler gönderiyor. Belge değerlerini yüklenirken çağrılır *dizin* veya veri alımı.

Portalda bir dizin oluşturabilirsiniz [REST API](search-create-index-rest-api.md), veya [.NET SDK'sı](search-create-index-dotnet.md).

## <a name="recommended-workflow"></a>Önerilen iş akışı

Fiziksel yapılar, dizin oluşturma sırasında oluşturulduğundan gerekecektir [bırakın ve dizinleri yeniden](search-howto-reindex.md) varolan alan tanımına kullanacağıyla ilgili önemli değişiklikler yaptığınızda. Başka bir deyişle, geliştirme sırasında üzerinde sık sık yeniden planlamalısınız. Git, daha hızlı hale getirmek için verilerinizin bir alt kümesi ile çalışma oluşturur göz önünde bulundurabilirsiniz. 

Portal dizin yerine kod da önerilir. Dizin tanımı için portalda güveniyorsanız, her yeniden dizin tanımını doldurmanız gerekir. Alternatif olarak, bir aracı gibi kullanarak [Postman ve REST API'si](search-fiddler.md) geliştirme projelerini hala erken bir aşamada olduğunda, kavram kanıtı test etmek için yararlıdır. Hizmetinize güncelleştirilmiş bir şemayı kullanarak bir dizini yeniden oluşturmak için bu isteği göndermek için bir dizin tanımını istek gövdesine artımlı değişiklikler yapabilirsiniz.

## <a name="components-of-an-index"></a>Bir dizinin bileşenleri

Schematically, Azure Search dizini aşağıdaki öğelerden oluşur. 

[ *Alanlar koleksiyonunu* ](#fields-collection) genellikle burada her bir alan adlandırılır, bir dizinin en büyük bölümü yazılan ve nasıl kullanıldığını belirlemek izin verilen davranışlarla öznitelikli. Diğer öğeleri içeren [öneri Araçları](#suggesters), [Puanlama profilleri](#scoring-profiles), [Çözümleyicileri](#analyzers) özelleştirme desteklemek için bileşen parçalarına sahip ve [CORS](#cors) Seçenekler.

```json
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
      "analyzer": "name_of_analyzer_for_search_and_indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
      "searchAnalyzer": "name_of_search_analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
      "indexAnalyzer": "name_of_indexing_analyzer", (only if 'searchAnalyzer' is set and 'analyzer' is not set)
      "synonymMaps": [ "name_of_synonym_map" ] (optional, only one synonym map per field is currently supported)
    }  
  ],  
  "suggesters": [  
    {  
      "name": "name of suggester",  
      "searchMode": "analyzingInfixMatching",  
      "sourceFields": ["field1", "field2", ...]  
    }  
  ],  
  "scoringProfiles": [  
    {  
      "name": "name of scoring profile",  
      "text": (optional, only applies to searchable fields) {  
        "weights": {  
          "searchable_field_name": relative_weight_value (positive #'s),  
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
            "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)  
            "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)  
          },  
          "tag": {  
            "tagsParameter": "..." (parameter to be passed in queries to specify a list of tags to compare against target fields)  
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
```

<a name="fields-collection"></a>

## <a name="fields-collection-and-field-attributes"></a>Alanlar koleksiyonu ve alan öznitelikleri

Şemanızı tanımlarken, dizininizdeki her bir alan için ad, tür ve öznitelikler belirtmeniz gerekir. Alan türü, bu alanda depolanan verileri sınıflandırır. Öznitelikler, alanın nasıl kullanıldığını belirtmek için tek tek alanlarda ayarlanır. Aşağıdaki tablolar belirtebileceğiniz türleri ve öznitelikleri numaralandırır.

### <a name="data-types"></a>Veri türleri
| Type | Açıklama |
| --- | --- |
| *Edm.String* |Tam metin arama için isteğe bağlı olarak belirteç haline getirilebilen metin (sözcük bölünmesi, kök ayırma, vb.). |
| *Collection(Edm.String)* |Tam metin araması için isteğe bağlı olarak belirteç haline getirilebilen dize listesi. Bir koleksiyondaki öğelerin sayısında teorik bir üst sınır yoktur ancak yük boyutundaki 16 MB'lık üst sınır, koleksiyonlar için geçerlidir. |
| *Edm.Boolean* |True/false değerlerini içerir. |
| *Edm.Int32* |32 bit tamsayı değerleri. |
| *Edm.Int64* |64 bit tamsayı değerleri. |
| *Edm.Double* |Çift duyarlıklı sayısal veriler. |
| *Edm.DateTimeOffset* |OData V4 biçiminde (ör. `yyyy-MM-ddTHH:mm:ss.fffZ` ve `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`) temsil edilen tarih ve saat değerleri. |
| *Edm.GeographyPoint* |Dünya üzerindeki bir coğrafi konumu temsil eden bir nokta. |

Azure Search'ün [desteklediği veri türleri hakkında burada](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) daha ayrıntılı bilgiler edinebilirsiniz.

### <a name="index-attributes"></a>Dizin öznitelikleri
| Öznitelik | Açıklama |
| --- | --- |
| *Anahtar* |Her bir belgenin belge araması için kullanılan benzersiz kimliğini sağlayan bir dize. Tüm dizinlerin bir anahtarı olması gerekir. Yalnızca bir alan anahtar olabilir ve bunun türü Edm.String olarak ayarlanmalıdır. |
| *Alınabilir* |Bir arama sonucunda bir alanın döndürülüp döndürülemeyeceğini belirtir. |
| *Filtrelenebilir* |Alanın filtre sorgularında kullanılmasını sağlar. |
| *Sıralanabilir* |Bu alanı kullanarak bir sorgunun arama sonuçlarını sıralamasını sağlar. |
| *Modellenebilir* |Kullanıcının bağımsız filtrelemesi için [modellenmiş bir gezinmede](search-faceted-navigation.md) bir alanın kullanılmasını sağlar. Genellikle birden çok belgeyi bir araya gruplamak için kullanabileceğiniz yinelemeli değerler içeren alanlar (örneğin, tek bir marka veya hizmet kategorisine denk gelen birden çok belge) model olarak en iyi şekilde işler. |
| *Aranabilir* |Alanı tam metin aranabilir şeklinde işaretler. |

Azure Search'ün [dizin öznitelikleri hakkında burada](https://docs.microsoft.com/rest/api/searchservice/Create-Index) daha ayrıntılı bilgiler edinebilirsiniz.

## <a name="storage-implications"></a>Depolama etkileri

Öznitelikleri depolama etkisi. Aşağıdaki ekran görüntüsünde, dizin depolama düzeni özniteliklerin çeşitli birleşimler kaynaklanan bir örnektir. Dizin dayanır [yerleşik realestate örnek](search-get-started-portal.md) dizine ekleyebilir, veri kaynağı ve Portalı'nda sorgu.

Belgeleri bozulmadan saklanan şekilde tam eşleşme filtreleme ve sıralama işlemleri sorgu. Aranabilir alanları tam metin ve belirsiz aramayı etkinleştirin. Ters dizinleri aranabilir alanları için oluşturulan ve simgeleştirilmiş şartlarını doldurulur. Bir alanı işaretlemek odalı önemli dizin boyutu üzerinde etkisi yoktur.

![Dizin öznitelik seçimi temel alınarak boyut](./media/search-what-is-an-index/realestate-index-size.png "dizin öznitelik seçimi temel alınarak boyutu")

Birkaç aşağıdaki bileşimler yapay, bir nokta illuminating için kullanışlı, ancak uygun bir dizinde neden değil. Uygulamada, hiçbir zaman tek her alan için bir öneri aracı eklediğinizde veya aranabilir ancak değil alınabilir bir dizin oluşturun.

Depolama mimarisi, Azure Search'ün bir uygulama ayrıntısı kabul edilir ve uyarı değişebilir. Şu anki davranışı gelecekte kalıcı bir garanti yoktur.

## <a name="suggesters"></a>Öneri Araçları
Bir öneri aracı hangi alanlarda dizin aramaları otomatik tamamlamayı veya yazarken tamamlanan sorgu desteklemek için kullanılan tanımlayan şemayı bölümüdür. Genellikle kısmi arama dizelerini önerileri (Azure Search Hizmeti REST API'si) kullanıcı arama sorgusu yazıyor ve API, önerilen ifadelerden bir dizi döndürür gönderilir. 

Dizin tanımlayan bir öneri aracı hangi alanların yazarken tamamlanan arama terimlerini oluşturmak için kullanılan belirler. Daha fazla bilgi için [öneri Araçları eklemek](index-add-suggesters.md) yapılandırma ayrıntıları için.

## <a name="scoring-profiles"></a>Puanlama modelleri

Puanlama profili, hangi öğelerin arama sonuçlarında daha yukarıda görüntülenmesini etkilemek olanak tanıyan özel Puanlama davranışlarını tanımlayan şemayı bölümüdür. Puanlama profilleri, alan ağırlıklarını ve işlevleri yapılır. Bunları kullanmak için sorgu dizesi adına göre bir profil seçin.

Puanlama profili varsayılan bir sonuç kümesi içindeki her bir öğe için arama puanını hesaplamak için arka planda çalışır. İç, Puanlama profili adlandırılmamış kullanabilirsiniz. Alternatif olarak, özel bir profil, sorgu dizesinde belirtilmedi çağrılır, varsayılan olarak özel bir profil kullanılacak defaultScoringProfile ayarlayın.

Daha fazla bilgi için [Puanlama profillerini Ekle](index-add-scoring-profiles.md).

## <a name="analyzers"></a>Çözümleyiciler

Çözümleyiciler öğesi alanı için kullanılacak dil Çözümleyicisi adını ayarlar. İzin verilen değerleri kümesi için bkz: [dil Çözümleyicileri Azure Search'te](index-add-language-analyzers.md). Bu seçenek yalnızca aranabilir alanları ile kullanılabilir ve onu ile birlikte ayarlanamaz **searchAnalyzer** veya **indexAnalyzer**. Çözümleyicisi seçildikten sonra alan için değiştirilemez.

## <a name="cors"></a>CORS

İstemci tarafı JavaScript tarayıcı tüm çıkış noktaları arası istekleri engeller olduğundan tüm API'leri varsayılan olarak çağrılamıyor. Dizininizi çıkış noktaları arası sorguları izin vermek için CORS'yi (çıkış noktaları arası kaynak paylaşımı) ayarlayarak etkinleştirme **corsOptions** özniteliği. Güvenlik nedeniyle, yalnızca sorgu API CORS desteği. 

CORS için aşağıdaki seçenekler ayarlanabilir:

+ **allowedOrigins** (gerekli): Dizininiz için erişim verilecek çıkış noktaları listesidir. Bu kaynaklardan herhangi bir JavaScript kodu sunulan anlamına gelir (doğru api anahtarını sağladığı varsayılarak) dizininizi sorgulama için izin verilir. Her kaynak kod biçimindedir genellikle `protocol://<fully-qualified-domain-name>:<port>` rağmen `<port>` genellikle atlanır. Bkz: [çıkış noktaları arası kaynak paylaşımı (Wikipedia)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) daha fazla ayrıntı için.

  Tüm kaynaklara erişmesine izin vermek istiyorsanız, dahil `*` tek bir öğe olarak **allowedOrigins** dizisi. *Bu uygulama için üretim arama hizmetleri önerilmez* ancak genellikle geliştirme ve hata ayıklama için yararlı olur.

+ **Maxageınseconds** (isteğe bağlı): Tarayıcılar bu değeri önbellek CORS denetim öncesi yanıtlarını süresi (saniye) belirlemek için kullanın. Bu, negatif olmayan tamsayı olmalıdır. Bu değer büyük, daha iyi performans olacaktır ancak CORS İlkesi değişikliklerinin etkili olması alacaktır uzun. Ayarlanmazsa, varsayılan süre olan 5 dakika kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

Dizin oluşturma bir anlayışa sahip portalda ilk dizininizi oluşturmaya devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Bir dizin (portal) Ekle](search-create-index-portal.md)