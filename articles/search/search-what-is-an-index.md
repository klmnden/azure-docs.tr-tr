---
title: Bir dizin tanımını ve kavramlar - Azure Search oluşturma
description: Dizin terimleri ve kavramları bileşen parçalarına ve fiziksel yapısı gibi Azure Search giriş.
author: HeidiSteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.custom: seodec2018
ms.openlocfilehash: 0a6a5b0e3957141b9ea17a378a7cbeff33a0124e
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67485193"
---
# <a name="create-a-basic-index-in-azure-search"></a>Azure Search'te bir temel dizin oluşturma

Azure Search'te bir *dizin* kalıcı bir deposudur *belgeleri* ve Azure Search Hizmeti filtrelenmiş ve tam metin araması için kullanılan diğer yapılar. Kavramsal olarak, bir belge, dizininizdeki aranabilir verilerin tek bir birimdir. Örneğin, bir e-ticaret satıcısında sattığı her bir öğe için bir belge, bir haber kuruluşunda her bir makale için bir belge, vb. olabilir. Bu kavramları daha çok bilinen veritabanı eşdeğerlerine eşleyen bir *dizin*, kavramsal olarak bir *tabloya* benzer ve *belgeler* de bir tablodaki *satırlarla* kabaca eşdeğerdir.

Eklediğinizde veya dizin karşıya yükleme fiziksel yapıları sağladığınız şemasını temel alan Azure Search oluşturur. Örneğin, dizininizdeki bir alanın aranabilir olarak işaretlenmişse, bu alan için ters dizin oluşturulur. Daha sonra eklediğinizde veya belgeleri karşıya yüklemesine veya Azure Search'e arama sorguları göndermek için belirli bir dizin arama hizmetinizdeki istekler gönderiyor. Belge değerlerini yüklenirken çağrılır *dizin* veya veri alımı.

Portalda bir dizin oluşturabilirsiniz [REST API](search-create-index-rest-api.md), veya [.NET SDK'sı](search-create-index-dotnet.md).

## <a name="recommended-workflow"></a>Önerilen iş akışı

En doğru dizin tasarımı ulaşan, genellikle birden fazla yineleme ile elde edilir. Araçlar ve API'ler bir birleşimini kullanarak hızla tasarımı son haline getir yardımcı olabilir.

1. Kullanıp kullanamadığını belirlemek bir [dizin oluşturucu](search-indexer-overview.md#supported-data-sources). Dış verilerinizin desteklenen veri kaynaklarından biri, prototip olabilir ve dizini kullanarak yük [ **verileri içeri aktarma** ](search-import-data-portal.md) Sihirbazı.

2. Kullanamıyorsanız **verileri içeri aktarma**, yine de [portalda ilk dizin oluşturma](search-create-index-portal.md), alanları, veri türleri ekleme ve atama denetimleri kullanarak öznitelikleri **Add Index** Sayfa. Portal hangi özniteliklerin farklı veri türleri için kullanılabilen gösterir. Dizin tasarımı yeniyseniz, bu yararlı olur.

   ![Öznitelikleri veri türüne göre gösteren Ekle dizin sayfası](media/search-create-index-portal/field-attributes.png "öznitelikleri veri türüne göre gösteren Ekle dizin sayfası")
  
   Tıkladığınızda **Oluştur**, tüm dizininizi destekleyen fiziksel yapıları arama hizmetiniz oluşturulur.

3. Dizin şeması kullanarak indirin [alma dizin REST API](https://docs.microsoft.com/rest/api/searchservice/get-index) ve bir web testi gibi araç [Postman](search-get-started-postman.md). Artık bir JSON temsili portalda oluşturulan bir dizin var. 

   Bu noktada kod tabanlı bir yaklaşımda geçiyorsunuz. Önceden oluşturulmuş bir dizin düzenleyemediğiniz portalı yineleme için uygun değil. Ancak, kalan görevlerin Postman ve REST kullanabilirsiniz.

4. [Dizininizi verilerle yük](search-what-is-data-import.md). Azure Search, JSON belgelerini kabul eder. Verilerinizi programlama yoluyla yüklemek için Postman isteği yükü JSON belgeleri ile kullanabilirsiniz. Bu adım, JSON olarak verilerinizi kolayca belirtilmiyorsa en yoğun işçilik olacaktır.

5. Dizininizi sorgulama, sonuçları inceleyin ve beklediğiniz sonuçları görmek başlayana kadar dizin şeması hakkında daha fazla yineleme. Kullanabileceğiniz [ **arama Gezgini** ](search-explorer.md) veya Postman dizininizi sorgulama için.

6. Kod üzerinde tasarımınızı yinelemek için kullanmaya devam edin.  

Fiziksel yapılar, hizmette oluşturulduğundan [bırakarak ve dizinleri yeniden](search-howto-reindex.md) varolan alan tanımına kullanacağıyla ilgili önemli değişiklikler yaptığınızda gereklidir. Başka bir deyişle, geliştirme sırasında üzerinde sık sık yeniden planlamalısınız. Git, daha hızlı hale getirmek için verilerinizin bir alt kümesi ile çalışma oluşturur göz önünde bulundurabilirsiniz. 

Portal bir yaklaşım yerine kod yinelemeli tasarım için önerilir. Dizin tanımı için portalda güveniyorsanız, her yeniden dizin tanımını doldurmanız gerekir. Alternatif olarak, gibi araçları [Postman ve REST API'si](search-get-started-postman.md) geliştirme projelerini hala erken bir aşamada olduğunda, kavram kanıtı test etmek için yararlıdır. Bir dizin tanımını istek gövdesine artımlı değişiklikler yapmak ve sonra güncelleştirilmiş bir şemayı kullanarak bir dizini yeniden oluşturmak için hizmetinize istek gönderin.

## <a name="components-of-an-index"></a>Bir dizinin bileşenleri

Schematically, Azure Search dizini aşağıdaki öğelerden oluşur. 

[ *Alanlar koleksiyonunu* ](#fields-collection) genellikle burada her bir alan adlandırılır, bir dizinin en büyük bölümü yazılan ve nasıl kullanıldığını belirlemek izin verilen davranışlarla öznitelikli. Diğer öğeleri içeren [öneri Araçları](#suggesters), [Puanlama profilleri](#scoring-profiles), [Çözümleyicileri](#analyzers) özelleştirme desteklemek için bileşen parçalarına sahip [CORS](#cors) ve [şifreleme anahtarı](#encryption-key) seçenekleri.

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
  },
  "encryptionKey":(optional){
    "keyVaultUri": "azure_key_vault_uri",
    "keyVaultKeyName": "name_of_azure_key_vault_key",
    "keyVaultKeyVersion": "version_of_azure_key_vault_key",
    "accessCredentials":(optional){
      "applicationId": "azure_active_directory_application_id",
      "applicationSecret": "azure_active_directory_application_authentication_key"
    }
  }
}
```

<a name="fields-collection"></a>

## <a name="fields-collection-and-field-attributes"></a>Alanlar koleksiyonu ve alan öznitelikleri

Şemanızı tanımlarken, dizininizdeki her bir alan için ad, tür ve öznitelikler belirtmeniz gerekir. Alan türü, bu alanda depolanan verileri sınıflandırır. Öznitelikler, alanın nasıl kullanıldığını belirtmek için tek tek alanlarda ayarlanır. Aşağıdaki tablolar belirtebileceğiniz türleri ve öznitelikleri numaralandırır.

### <a name="data-types"></a>Veri türleri
| Tür | Açıklama |
| --- | --- |
| *Edm.String* |(Sözcük bölünmesi, dallanma ve diğerleri) tam metin araması için isteğe bağlı olarak getirilebilen metin. |
| *Collection(Edm.String)* |Tam metin araması için isteğe bağlı olarak belirteç haline getirilebilen dize listesi. Bir koleksiyondaki öğelerin sayısında teorik bir üst sınır yoktur ancak yük boyutundaki 16 MB'lık üst sınır, koleksiyonlar için geçerlidir. |
| *Edm.Boolean* |True/false değerlerini içerir. |
| *Edm.Int32* |32 bit tamsayı değerleri. |
| *Edm.Int64* |64 bit tamsayı değerleri. |
| *Edm.Double* |Çift duyarlıklı sayısal veriler. |
| *Edm.DateTimeOffset* |Tarih saat değerlerini temsil edilen OData V4 biçiminde (örneğin, `yyyy-MM-ddTHH:mm:ss.fffZ` veya `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`). |
| *Edm.GeographyPoint* |Dünya üzerindeki bir coğrafi konumu temsil eden bir nokta. |

Azure Search'ün [desteklediği veri türleri hakkında burada](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) daha ayrıntılı bilgiler edinebilirsiniz.

### <a name="index-attributes"></a>Dizin öznitelikleri

Dizininizde yalnızca bir alanın olmalıdır belirlenmiş olarak bir **anahtar** her belgenin benzersiz olarak tanımlayan alan.

Diğer öznitelikler, bir uygulamada bir alanın nasıl kullanıldığını belirler. Örneğin, **aranabilir** özniteliği bir tam metin aramasına dahil edilmesi gereken her alanı atanır. 

Bir dizin oluşturmak için kullandığınız API'leri, değişen davranışa sahip. İçin [REST API'leri](https://docs.microsoft.com/rest/api/searchservice/Create-Index), özelliklerin çoğu, varsayılan olarak etkin olan (örneğin, **aranabilir** ve **alınabilir** dize alanları için true) ve genellikle yalnızca bunları verilirse gerekir bunları kapatmak istiyor. .NET SDK için bunun tersi de geçerlidir. Açıkça ayarlamayın herhangi bir özellik hakkında özellikle etkinleştirmediğiniz sürece karşılık gelen arama davranışını devre dışı bırakmak için varsayılandır.

| Öznitelik | Açıklama |
| --- | --- |
| `key` |Her bir belgenin belge araması için kullanılan benzersiz kimliğini sağlayan bir dize. Tüm dizinlerin bir anahtarı olması gerekir. Yalnızca bir alan anahtar olabilir ve bunun türü Edm.String olarak ayarlanmalıdır. |
| `retrievable` |Bir arama sonucunda bir alanın döndürülüp döndürülemeyeceğini belirtir. |
| `filterable` |Alanın filtre sorgularında kullanılmasını sağlar. |
| `Sortable` |Bu alanı kullanarak bir sorgunun arama sonuçlarını sıralamasını sağlar. |
| `facetable` |Kullanıcının bağımsız filtrelemesi için [modellenmiş bir gezinmede](search-faceted-navigation.md) bir alanın kullanılmasını sağlar. Genellikle birden çok belgeyi bir araya gruplamak için kullanabileceğiniz yinelemeli değerler içeren alanlar (örneğin, tek bir marka veya hizmet kategorisine denk gelen birden çok belge) model olarak en iyi şekilde işler. |
| `searchable` |Alanı tam metin aranabilir şeklinde işaretler. |


## <a name="storage-implications"></a>Depolama etkileri

Öznitelikleri depolama etkisi. Aşağıdaki ekran görüntüsünde özniteliklerin çeşitli birleşimleri arasından kaynaklanan dizin depolama desenleri gösterir.

Dizin dayanır [yerleşik Emlak örnek](search-get-started-portal.md) dizine ekleyebilir, veri kaynağı ve Portalı'nda sorgu. Dizin şemaları gösterilmese dizin adını temel alarak öznitelikleri çıkarabilir. Örneğin, *realestate-aranabilir* dizininin **aranabilir** seçili özniteliği ve başka bir şey *realestate-alınabilir* dizininin  **alınabilir** seçili özniteliği ve hiçbir şey başka ve benzeri.

![Dizin öznitelik seçimi temel alınarak boyut](./media/search-what-is-an-index/realestate-index-size.png "dizin öznitelik seçimi temel alınarak boyutu")

Bu dizin çeşitleri yapay olsa da, biz onlara öznitelikleri depolama nasıl etkileyeceğini geniş karşılaştırmalar için başvurabilir. Ayar yapar **alınabilir** dizin boyutunu artırın? Hayır. Alanları ekleme yoksa bir **öneri aracı** dizin boyutunu artırın? Evet.

Filtreleme ve sıralama desteği dizinleri orantılı olarak yalnızca tam metin aramayı destekleyen dizinleri büyüktür. Nedeni bu filtreleme ve sıralama sorguyu temel tam eşleşme olduğundan belgeler bozulmadan depolanır. Buna karşılık, aranabilir alanları tam metin ve belirsiz destekleyen tüm belgeleri daha az alan tüketen parçalanmış koşullarıyla doldurulur ters kullanım dizinler, arayın.

> [!Note]
> Depolama mimarisi, Azure Search'ün bir uygulama ayrıntısı kabul edilir ve uyarı değişebilir. Şu anki davranışı gelecekte kalıcı bir garanti yoktur.

## <a name="suggesters"></a>Öneri Araçları
Bir öneri aracı hangi alanlarda dizin aramaları otomatik tamamlamayı veya yazarken tamamlanan sorgu desteklemek için kullanılan tanımlayan şemayı bölümüdür. Genellikle, kısmi arama dizelerini gönderilen [önerileri (REST API'si)](https://docs.microsoft.com/rest/api/searchservice/suggestions) kullanıcı arama sorgusu yazıyor ve API, önerilen ifadelerden bir dizi döndürür. 

Alanlar için bir öneri aracı eklenmiş, yazarken tamamlanan arama terimlerini oluşturmak için kullanılır. Arama koşulları dizin oluşturma sırasında oluşturulur ve ayrı olarak depolanır. Öneri aracı yapısı oluşturma hakkında daha fazla bilgi için bkz. [öneri Araçları eklemek](index-add-suggesters.md).

## <a name="scoring-profiles"></a>Puanlama modelleri

A [Puanlama profili](index-add-scoring-profiles.md) olan özel tanımlayan şemanın bir bölümünü Puanlama davranışları hangi öğelerin arama sonuçlarında daha yukarıda görünür, etkilemek bildiren. Puanlama profilleri, alan ağırlıklarını ve işlevleri yapılır. Bunları kullanmak için sorgu dizesi adına göre bir profil seçin.

Puanlama profili varsayılan bir sonuç kümesi içindeki her bir öğe için arama puanını hesaplamak için arka planda çalışır. İç, Puanlama profili adlandırılmamış kullanabilirsiniz. Alternatif olarak, kümesinin **defaultScoringProfile** özel bir profil varsayılan olarak kullanmak için özel bir profil sorgu dizesinde belirtilmediğinde çağrılır.

## <a name="analyzers"></a>Çözümleyiciler

Çözümleyiciler öğesi alanı için kullanılacak dil Çözümleyicisi adını ayarlar. Çözümleyiciler kullanabileceğiniz çeşitli hakkında daha fazla bilgi için bkz. [çözümleyiciler için bir Azure Search dizini ekleme](search-analyzers.md). Çözümleyicileri yalnızca aranabilir alanları ile kullanılabilir. Çözümleyici bir alan atandıktan sonra dizini yeniden sürece değiştirilemez.

## <a name="cors"></a>CORS

İstemci tarafı JavaScript tarayıcı tüm çıkış noktaları arası istekleri engeller olduğundan tüm API'leri varsayılan olarak çağrılamıyor. Dizininizi çıkış noktaları arası sorguları izin vermek için CORS'yi (çıkış noktaları arası kaynak paylaşımı) ayarlayarak etkinleştirme **corsOptions** özniteliği. Güvenlik nedeniyle, yalnızca sorgu API CORS desteği. 

CORS için aşağıdaki seçenekler ayarlanabilir:

+ **allowedOrigins** (gerekli): Dizininiz için erişim verilecek çıkış noktaları listesidir. Bu kaynaklardan herhangi bir JavaScript kodu sunulan anlamına gelir (doğru api anahtarını sağladığı varsayılarak) dizininizi sorgulama için izin verilir. Her kaynak kod biçimindedir genellikle `protocol://<fully-qualified-domain-name>:<port>` rağmen `<port>` genellikle atlanır. Bkz: [çıkış noktaları arası kaynak paylaşımı (Wikipedia)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) daha fazla ayrıntı için.

  Tüm kaynaklara erişmesine izin vermek istiyorsanız, dahil `*` tek bir öğe olarak **allowedOrigins** dizisi. *Bu uygulama için üretim arama hizmetleri önerilmez* ancak genellikle geliştirme ve hata ayıklama için yararlı olur.

+ **Maxageınseconds** (isteğe bağlı): Tarayıcılar bu değeri önbellek CORS denetim öncesi yanıtlarını süresi (saniye) belirlemek için kullanın. Bu, negatif olmayan tamsayı olmalıdır. Bu değer büyük, daha iyi performans olacaktır ancak CORS İlkesi değişikliklerinin etkili olması alacaktır uzun. Ayarlanmazsa, varsayılan süre olan 5 dakika kullanılır.

## <a name="encryption-key"></a>Şifreleme anahtarı

Şifreli olarak tüm Azure search dizinlerini varsayılan olarak Microsoft tarafından yönetilen anahtarlarla şifrelenir, ancak dizin yapılandırılabilir **müşteri tarafından yönetilen anahtarlar** anahtar Kasası'nda. Daha fazla bilgi için bkz. [Azure Search'te şifreleme anahtarlarını yönetmek](search-security-manage-encryption-keys.md).

## <a name="next-steps"></a>Sonraki adımlar

Dizin oluşturma bir anlayışa sahip portalda ilk dizininizi oluşturmaya devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Bir dizin (portal) Ekle](search-create-index-portal.md)
