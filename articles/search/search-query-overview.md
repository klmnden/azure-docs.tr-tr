---
title: Sorgu türleri ve oluşturma - Azure Search
description: Filtre uygulamak için parametreleri kullanarak Azure Search, arama sorgusu oluşturmak için temel bilgileri seçin ve sonuçları sıralamak.
author: HeidiSteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.topic: conceptual
ms.date: 03/25/2019
ms.custom: seodec2018
ms.openlocfilehash: cfc9b44963f6880e97859bc7ab77bff12d258471
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58500177"
---
# <a name="how-to-compose-a-query-in-azure-search"></a>Azure Search'te bir sorgu oluşturmak nasıl

Azure Search'te bir sorgu tam bir gidiş dönüş işlemi belirtimi ' dir. İstekte parametreler belgelere dizin, yürütme yönergeleriyle altyapısı ve yanıt şekillendirmek için yönergeler için bulmak için eşleştirme ölçütü belirtin. 

Bir sorgu isteği kapsam, arama yapma, sıralama veya filtreleme için iade vb. için hangi alanların hangi alanların olduğunu belirten zengin bir yapıdır. Belirtilmezse, rastgele sırayla kümesi puanlanmayan bir sonuç döndüren bir tam metin arama işlemi olarak tüm aranabilir alanları karşı bir sorgu çalıştırır.

## <a name="apis-and-tools-for-testing"></a>API'ler ve test araçları

Aşağıdaki tabloda sorguları gönderme aracı tabanlı yaklaşımlar ve API'ları listeler.

| Yöntemi | Açıklama |
|-------------|-------------|
| [Arama Gezgini (portal)](search-explorer.md) | Arama çubuğu ve dizin ve API sürümü seçimleri için seçenekler sağlar. Sonuçlar, JSON belgeleri olarak döndürülür. <br/>[Daha fazla bilgi edinin.](search-get-started-portal.md#query-index) | 
| [Postman veya fiddler'ı](search-fiddler.md) | Web test araçları, REST çağrılarını formulating için harika bir seçenektir. REST API, Azure arama'yı olası her işlemi destekler. Bu makalede, bir HTTP isteği üst bilgisi ve gövdesi istekleri Azure Search'e göndermek için ayarlama konusunda bilgi edinin.  |
| [Searchındexclient (.NET)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient?view=azure-dotnet) | Bir Azure Search dizinini sorgulama için kullanılan istemci.  <br/>[Daha fazla bilgi edinin.](search-howto-dotnet-sdk.md#core-scenarios)  |
| [Search belgeleri (REST API'si)](https://docs.microsoft.com/rest/api/searchservice/search-documents) | GET veya POST yöntemleri için ek giriş sorgu parametrelerini kullanarak, bir dizin üzerinde.  |

## <a name="a-first-look-at-query-requests"></a>Sorgu istekleri ilk göz

Örnekler, yeni kavramları tanıtımı için kullanışlıdır. Temsili bir sorgu oluşturulmuş gibi [REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents), bu örnek hedefleri [Emlak tanıtım dizin](search-get-started-portal.md) ve ortak parametreleri içerir.

```
{
    "queryType": "simple" 
    "search": "seattle townhouse* +\"lake\"",
    "searchFields": "description, city",
    "count": "true",
    "select": "listingId, street, status, daysOnMarket, description",
    "top": "10",
    "orderby": "daysOnMarket"
}
```

+ **`queryType`** Azure Search'te olabilir ve Ayrıştırıcıyı ayarlar [varsayılan Basit Sorgu ayrıştırıcı](search-query-simple-examples.md) (tam metin araması için ideal), veya [tam Lucene sorgu ayrıştırıcısına](search-query-lucene-examples.md) normal ifadeler gibi gelişmiş sorgu yapıları için kullanılan , yakınlık araması, belirsiz ve joker karakter search.

+ **`search`** eşleşme ölçütlerini, genellikle metnin genellikle eşlik Boole işleçleri tarafından sağlar. Tek tek başına koşulları *terimi* sorgular. Tırnak işareti içine alınmış çok parçalı sorgular *anahtar tümcecik* sorgular. Arama olarak tanımlanmamış olabilir **`search=*`**, ancak büyük olasılıkla koşulları, ifadeler ve işleçler örnekte görünen ne benzer oluşur.

+ **`searchFields`** sorgu yürütme belirli alanlarla sınırlandırmak için isteğe bağlı, kullanılır.

Yanıtlar aynı zamanda sorguya dahil parametreleri şeklinde. Örnekte, sonuç kümesi yer alandan oluşur **`select`** deyimi. Bu sorguda yalnızca en üst 10 isabet sayısı döndürülür ancak **`count`** kaç belgeler genel eşleşen söyler. Bu sorgu, satırlar daysOnMarket göre sıralanır.

Azure arama'yı istekte sağlanan bir API anahtarı kullanılarak kimlik doğrulaması bir dizini karşı sorgu yürütme her zaman olur. İstek üst bilgilerinde, KALAN her ikisi de sağlanır.

### <a name="how-to-run-this-query"></a>Bu sorgu çalıştırma

Bu sorguyu çalıştırmak için kullanın [arama Gezgini ve tanıtım Emlak dizini](search-get-started-portal.md). 

Bu sorgu dizesi explorer'ın arama çubuğuna yapıştırabilirsiniz: `search=seattle townhouse +lake&searchFields=description, city&$count=true&$select=listingId, street, status, daysOnMarket, description&$top=10&$orderby=daysOnMarket`

## <a name="how-query-operations-are-enabled-by-the-index"></a>Dizine göre sorgu işlemleri nasıl etkinleştirilir

Dizin tasarımı ve tasarım Azure Search'te sıkıca sorgu. Önden bilmek önemli bir olgu olan *dizin şeması*, her bir alan özniteliklerinde ile sorgu yapı türünü belirler. 

Bir alan olup olmadığını bir alanda dizin özniteliklerini ayarlayın - izin verilen işlemler *aranabilir* dizinde *alınabilir* sonuçlarında *sıralanabilir*,  *filtrelenebilir*ve böyle devam eder. Örnek Sorgu dizesinde `"$orderby": "daysOnMarket"` daysOnMarket alan olarak işaretlendiğinden yalnızca çalışır *sıralanabilir* dizin şemasında. 

![Dizin tanımı Emlak örnek](./media/search-query-overview/realestate-sample-index-definition.png "dizin tanımı Emlak örnek")

Yukarıdaki ekran görüntüsünde, Emlak örneği için dizin özniteliklerini, kısmi bir listesidir. Portalda tüm dizin şemasını görüntüleyebilirsiniz. Dizin öznitelikleri hakkında daha fazla bilgi için bkz: [dizin REST API oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index).

> [!Note]
> Bazı sorgu işlevselliği, dizin genelinde yerine alan başına temelinde etkinleştirilir. Bu özellikler şunları içerir: [eş anlamlı eşler](search-synonyms.md), [özel çözümleyiciler](index-add-custom-analyzers.md), [öneri aracı oluşturur (otomatik tamamlama ve önerilen sorgular)](index-add-suggesters.md), [mantıksal Puanlama Sonuçları sıralama için](index-add-scoring-profiles.md).

## <a name="elements-of-a-query-request"></a>Bir sorgu isteği öğeleri

Sorgular, her zaman tek bir dizinde yönlendirilir. Dizinleri katamaz veya bir sorgu hedefi olarak özel veya geçici veri yapılarını oluşturun. 

Gerekli bir sorgu isteği öğelerinde bulunan aşağıdaki bileşenleri içerir:

+ Sabit ve kullanıcı tanımlı bileşenlerini içeren bir URL ifade edilen hizmet uç noktası ve dizin belge koleksiyonu: **`https://<your-service-name>.search.windows.net/indexes/<your-index-name>/docs`**
+ **`api-version`** (Yalnızca REST) API'ın birden fazla sürümü her zaman kullanılabilir olduğu için gereklidir. 
+ **`api-key`**, bir sorgu veya yönetici api anahtarını hizmetiniz için istek kimliğini doğrular.
+ **`queryType`**, basit veya tam yerleşik varsayılan basit söz dizimi kullanılıyorsa, atlanabilir.
+ **`search`** veya **`filter`** eşleşme ölçütlerini boş bir arama gerçekleştirmek istiyorsanız, belirtilmemiş olabilen sağlar. Her iki sorgu türleri basit ayrıştırıcının açısından ele alınmıştır, ancak daha gelişmiş sorgular, karmaşık sorgu ifadeleri geçirmek için arama parametresi gerektirir.

Diğer tüm arama parametreleri isteğe bağlıdır. Öznitelikleri tam listesi için bkz. [dizin oluşturma (REST)](https://docs.microsoft.com/rest/api/searchservice/create-index). İşleme sırasında parametre nasıl kullanıldığı bir daha yakından bakış için bkz: [Azure Search'te tam metin araması nasıl çalışır](search-lucene-query-architecture.md).

## <a name="choose-a-parser-simple--full"></a>Bir Ayrıştırıcı seçin: Basit | tam

Azure Search, Apache Lucene en üstünde yer alan ve genel ve özel sorguları işlemek için iki sorgu Çözümleyicileri arasında seçmenizi sağlar. Basit Ayrıştırıcıyı kullanarak istekleri şeklide kullanarak [Basit Sorgu söz dizimi](query-simple-syntax.md), serbest biçimli metin sorgularda verimliliği ve hızı için varsayılan olarak seçili. Bu söz dizimi AND, OR, NOT dahil olmak üzere ortak arama işleçlerini, tümcecik, sonek ve öncelik işleçleri destekler.

[Tam Lucene sorgu söz dizimi](query-Lucene-syntax.md#bkmk_syntax), eklediğinizde, etkin `queryType=full` isteğine bir parçası olarak geliştirilen yaygın olarak benimsenen ve açıklayıcı sorgu dilini kullanıma sunan [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html). Tam sözdizimi basit söz dizimi genişletir. Basit sözdizimi için yazdığınız herhangi bir sorgu tam Lucene çözümleyici altında çalışır. 

Aşağıdaki örnekler noktası gösterir: aynı sorgu, ancak farklı queryType ayarlarla farklı sonuçlar getirebilir. İlk sorgu `^3` arama teriminin bir parçası olarak kabul edilir.

```
queryType=simple&search=mountain beach garden ranch^3&searchFields=description&$count=true&$select=listingId, street, status, daysOnMarket, description&$top=10&$orderby=daysOnMarket
```

Aynı sorgu tam Lucene Ayrıştırıcıyı kullanarak bu belirli terimini içeren sonuçlarının arama sıralamasını artırıyor "ranch" üzerinde alan boost yorumlar.

```
queryType=full&search=mountain beach garden ranch^3&searchFields=description&$count=true&$select=listingId, street, status, daysOnMarket, description&$top=10&$orderby=daysOnMarket
```

<a name="types-of-queries"></a>

## <a name="types-of-queries"></a>Sorgu türleri

Sorgu türleri geniş bir Azure Search'ü destekler. 

| Sorgu türü | Kullanım | Örnekler ve daha fazla bilgi |
|------------|--------|-------------------------------|
| Serbest biçimli metin arama | Arama parametresi ve iki ayrıştırıcı| Bir veya daha çok terimi tüm tam metin araması tarar *aranabilir* dizininizdeki alanları ve çalışmak için Google veya Bing gibi bir arama motoru beklediğiniz gibi çalışır. Tam metin araması giriş örnektir.<br/><br/>Tam metin araması (varsayılan) standart olarak Lucene çözümleyici kullanarak metin analizi gibi "" remove durdurma sözcükleri olan tüm koşulları için küçük uygulanır. Varsayılan geçersiz kılma [İngilizce olmayan Çözümleyicileri](index-add-language-analyzers.md#language-analyzer-list) veya [özelleştirilmiş dilden Çözümleyicileri](index-add-custom-analyzers.md#AnalyzerTable) metin analizi değiştirin. Bir örnek [anahtar sözcüğü](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html) bir alanın tüm içeriği tek bir belirteç kabul eder. Bu, posta kodları, kimlikleri ve bazı ürün adları gibi veriler için kullanışlıdır. | 
| Filtrelenen arama | [OData filtre ifadesinin](query-odata-filter-orderby-syntax.md) ve ya da çözümleyici | Filtre sorgularını tüm üzerinde bir boolean ifadesinin değerlendirme *filtrelenebilir* dizin alanları. Arama, bir filtre sorgusu büyük küçük harf duyarlılığı dize alanları dahil olmak üzere, bir alanın tam içeriğini eşleştirir. Filtre sorgularını OData söz diziminde ifade edilen başka bir farktır. <br/>[Filtre ifadesi örneği](search-query-simple-examples.md#example-3-filter-queries) |
| Coğrafi arama | [Edm.GeographyPoint türü](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) alanda, filtre ifadesi ve ya da çözümleyici | "Yakınımda Bul" için kullanılan ya da harita tabanlı bir Edm.GeographyPoint sahip bir alanda depolanmış koordinatları arama denetimleri. <br/>[Coğrafi arama örneği](search-query-simple-examples.md#example-5-geo-search)|
| Aralık arama | Filtre ifadesi ve basit ayrıştırıcı | Azure Search'te, aralık sorguları, filtre parametresi kullanılarak oluşturulur. <br/>[Aralık filtresi örnek](search-query-simple-examples.md#example-4-range-filters) | 
| [İçi alan filtreleme](query-lucene-syntax.md#bkmk_fields) | Arama parametresi ve tam ayrıştırıcı | Tek bir alan hedefleyen bir bileşik sorgu ifadesi oluşturun. <br/>[İçi alan filtreleme örneği](search-query-lucene-examples.md#example-2-intra-field-filtering) |
| [Belirsiz arama](query-lucene-syntax.md#bkmk_fuzzy) | Arama parametresi ve tam ayrıştırıcı | Üzerinde benzer bir yapı olması veya yazım koşulları eşleşir. <br/>[Belirsiz arama örneği](search-query-lucene-examples.md#example-3-fuzzy-search) |
| [Yakınlık araması](query-lucene-syntax.md#bkmk_proximity) | Arama parametresi ve tam ayrıştırıcı | Birbirine yakın olan bir belgede bulur koşulları. <br/>[Yakınlık araması örneği](search-query-lucene-examples.md#example-4-proximity-search) |
| [Terim artırma](query-lucene-syntax.md#bkmk_termboost) | Arama parametresi ve tam ayrıştırıcı | Başkalarının içermeyen göreli artırmalı terimi içeriyorsa, daha yüksek bir belge sıralar. <br/>[Terim artırma örneği](search-query-lucene-examples.md#example-5-term-boosting) |
| [Normal ifade araması](query-lucene-syntax.md#bkmk_regex) | Arama parametresi ve tam ayrıştırıcı | Normal bir ifadenin içeriğine göre eşleşir. <br/>[Normal ifade örneği](search-query-lucene-examples.md#example-6-regex) |
|  [joker karakter veya önek arama](query-lucene-syntax.md#bkmk_wildcard) | Arama parametresi ve tam ayrıştırıcı | Eşleşme tabanlı bir ön ek ve tilde (`~`) veya tek bir karakter (`?`). <br/>[Joker karakter araması örneği](search-query-lucene-examples.md#example-7-wildcard-search) |

## <a name="manage-search-results"></a>Arama sonuçlarını yönetme 

.NET API kullanırsanız, serileştirme yerleşik olan ancak sorgu sonuçları REST API'si, JSON belgeleri olarak aktarılır. Sonuçları yanıtı için belirli alanları seçerek sorgu parametreleri ayarlayarak biçimlendirebilirsiniz.

Sorgu parametreleri, sonuç aşağıdaki yollarla kümesi yapısı için kullanılabilir:

+ Sınırlama veya sonuçları (varsayılan olarak 50) belge sayısı toplu işleme
+ Sonuçların dahil edileceği alanları seçme
+ Sıralama düzenini ayarlama
+ Arama sonuçlarını gövdesinde koşulları eşleştirme dikkat çekmek için ekleme isabet vurgular.

### <a name="tips-for-unexpected-results"></a>Beklenmeyen sonuçlar için ipuçları

Bazen, madde temini ve sonuçları yapısı beklenmeyen. Sorgu sonuçlarını görmek beklediğiniz değil olduğunda sonuçlarını iyileştirmek için bu sorgu değişiklikleri deneyebilirsiniz:

+ Değişiklik **`searchMode=any`** (varsayılan) **`searchMode=all`** ölçütlerden herhangi birine yerine tüm ölçütleri eşleşme istemek için. Boole işleçleri eklendiğinde bu özellikle doğrudur sorgu.

+ Sorgu tekniği, metin veya sözcük temelli analize gereklidir, ancak sorgu türünü dil işleme ışığının değiştirin. Tam metin araması, metin veya yazım hataları, tekil çoğul sözcük biçimlerini ve hatta düzensiz fiilleri veya isimleri için sözcük analizi autocorrects. Bazı sorgular gibi belirsiz veya joker karakter araması, metin analizi değil, işlem hattı ayrıştırma sorgunun parçası. Bazı senaryolarda, normal ifadeler geçici bir çözüm olarak kullanılır. 

### <a name="paging-results"></a>Disk belleği sonuçları
Azure Search, arama sonuçlarının sayfalanması uygulamasını kolaylaştırır. Kullanarak **`top`** ve **`skip`** parametreleri, arama sonuçları kümesini, yönetilebilir almanıza olanak, alt kümeler, sipariş arama isteklerini sorunsuz verebilir iyi arama kullanıcı Arabirimi uygulamalarını kolayca etkinleştirin. Bu daha küçük sonuç alt kümelerini alırken, tüm arama sonuçları kümesindeki belge sayısını da alabilirsiniz.

[Azure Search'te arama sonuçlarını numaralandırma](search-pagination-page-layout.md) makalesinde arama sonuçlarının numaralanması hakkında daha fazla bilgi alabilirsiniz.

### <a name="ordering-results"></a>Sonuçları sıralama
Bir arama sorgusunun sonuçları alınırken, Azure Search'ün sonuçları belirli bir alandaki değerlere göre sıralayarak sunmasını isteyebilirsiniz. Varsayılan olarak Azure Search, her bir belgenin [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)'den türetilen arama puanı sıralamasını temel alarak arama sonuçlarını sıralar.

Azure Search arama puanı dışında bir değere göre sıralı kullanabileceğiniz sonuçlarınızı döndürmek isterseniz **`orderby`** arama parametresi. Değerini belirtebileceğiniz **`orderby`** alan adları ve çağrıları dahil etmek için parametre [  **`geo.distance()` işlevi** ](query-odata-filter-orderby-syntax.md) Jeo-uzamsal değerler için. Her deyim tarafından izlenebilir `asc` sonuçları artan sırada istendiğini belirtmek için ve **`desc`** sonuçları azalan sırada istendiğini belirtmek için. Artan sıralama varsayılandır.


### <a name="hit-highlighting"></a>İsabet vurgulama
Azure Search'te arama sonuçlarının arama sorgusuyla eşleşen tam bölümü vurgulama kullanarak kolaylaştırılmıştır **`highlight`**, **`highlightPreTag`**, ve **`highlightPostTag`** parametreleri. Hangi *aranabilir* alanların eşleşen metninin vurgulanacağının yanı sıra Azure Search'ün döndürdüğü eşleşen metnin başına ve sonuna eklenecek dize etiketlerini tam olarak belirtebilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.

+ [Nasıl tam metin araması (sorgu mimarisi ayrıştırma) Azure Search'te çalışır](search-lucene-query-architecture.md)
+ [Arama Gezgini](search-explorer.md)
+ [. NET'te sorgulama](search-query-dotnet.md)
+ [KALAN sorgulama](search-create-index-rest-api.md)
