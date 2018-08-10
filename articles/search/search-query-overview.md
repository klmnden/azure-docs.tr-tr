---
title: Sorgu türleri ve Azure Search birleşimde | Microsoft Docs
description: Filtre uygulamak için parametreleri kullanarak Azure Search, arama sorgusu oluşturmak için temel bilgileri seçin ve sonuçları sıralamak.
author: HeidiSteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.topic: conceptual
ms.date: 08/03/2018
ms.openlocfilehash: 098718293cda1699fb07e09fa81af94a95bbdeca
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39715167"
---
# <a name="query-types-and-composition-in-azure-search"></a>Sorgu türleri ve Azure Search oluşturma

Azure Search'te sorgu oluşturma olduğu bir isteğin tam bir belirtimi: eşleşen ölçütleri yanı sıra, sorgu yürütme yönlendirerek ve yanıt şekillendirmek için parametreleri. Bir istek, sıralama veya filtreleme, geri dönmek için hangi alanların dahil vb. için hangi alanların belirtir. Belirtilmezse, rastgele sırayla kümesi puanlanmayan bir sonuç döndüren bir tam metin arama işlemi olarak tüm aranabilir alanları karşı bir sorgu çalıştırır.

## <a name="introduction-by-example"></a>Örneğe göre giriş

Örnekler arasındaki temel kavramları göstermek için kullanışlıdır. Kullanarak ifade aşağıdaki örnekte, [arama belgeleri REST API'si](https://docs.microsoft.com/rest/api/searchservice/search-documents), istek ve yanıt bildirir. Azure arama'yı istekte sağlanan bir API anahtarı kullanılarak kimlik doğrulaması bir dizini karşı sorgu yürütme her zaman olur. 

```
{  
    "queryType": "simple" 
    "search": "seattle townhouse* +\"lake\"", 
    "searchFields": "description, city",  
    "count": "true", 
    "select": "listingId, street, status, daysOnMarket, description",
    "top": "10",
    "orderby": "listingId"
 } 
```
Temsili bir sorgu olarak bu örnekte, birkaç önemli yönüyle sonuç kümesi şekillendirme ayrıştırıcı girişleri, gelen sorgu tanımı gösterilmektedir. İstekte sağlanan bir API anahtarı kullanılarak kimlik doğrulaması bir dizini karşı sorgu yürütme her zaman olur. 

Bu sorguyu çalıştırmak için kullandığınız [arama Gezgini ve tanıtım Emlak dizini](search-get-started-portal.md). Bu sorgu dizesi explorer'ın arama çubuğuna yapıştırabilirsiniz: `search=seattle townhouse +lake&searchFields=description, city&$count=true&$select=listingId, street, status, daysOnMarket, description&$top=10&orderby=listingId`

**Dizin arama**

+ Sorgu ayrıştırıcı bir seçimdir, aracılığıyla ayarlanan `queryType`. Çoğu geliştirici varsayılan kullanmak [basit ayrıştırıcı](search-query-simple-examples.md) için tam metin araması, ancak [tam Lucene](search-query-lucene-examples.md) ayrıştırma, belirsiz arama veya normal ifadeler gibi özelleştirilmiş sorgu formlar için gereklidir.
+ Dizin içindeki belgeler üzerinde eşleştirme ölçütü aracılığıyla ayarlanır `search` parametresi. Arama olarak tanımlanmamış olabilir `search=*`, ancak daha büyük olasılıkla oluşur koşulları, ifadeler ve işleçler benzer ne örnekte gösterilir.
+ Kapsam tüm dizinde olabilir ya da gösterildiği gibi özel alanları `searchFields`.

**Yanıt yapılandırma**

Diğer parametreler örnekte sorgunun sonuçlarının ilgilidir:

+ `count` belge sayısı bu eşleşen bir sorgu.
+ `select` Yanıtta döndürülen alanlarla sınırlandırır.
+ `top` satır veya yanıtta döndürülen belgelerin sınırlar. Varsayılan değer 50'dir; Örnek azaltan 10.
+ `orderby` bir alana göre sonuçları sıralar.

**Dizin özniteliklerini işlemleriyle etkinleştirme**

Dizin tasarımı ve tasarım Azure Search'te sıkıca sorgu. Burada gösterilmez, ancak bir kritik Önden bilmek, noktasıdır *dizin şeması*, her bir alan özniteliklerinde ile sorgu yapı türünü belirler. Bir alan belirleme özniteliklerinde dizin bir alan olup olmadığını işlemleri - izin verilen *aranabilir* dizinde *alınabilir* sonuçlarında *sıralanabilir*, * filtrelenebilir*ve böyle devam eder. Örnekte, `"orderby": "listingId"` listingId alan olarak işaretlenmişse yalnızca çalışır *sıralanabilir* dizin şemasında. Dizin öznitelikleri hakkında daha fazla bilgi için bkz: [dizin REST API oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index).

Alan başına temelinde işlemlerine izin dizin tanımını sorgu yürütme bildiren yalnızca bir yoludur. Dizinde etkin diğer özellikleri şunlardır:

+ [Eş anlamlıları](https://docs.microsoft.com/rest/api/searchservice/synonym-map-operations)
+ [Metin (dil) analizi](https://docs.microsoft.com//rest/api/searchservice/language-support) ve [özel analiz](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search)
+ [Öneri aracı yapıları](https://docs.microsoft.com/rest/api/searchservice/suggesters) otomatik tamamlama ve otomatik öneri etkinleştir
+ [Puanlama profilleri](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) arama sonuçları sıralama için mantığı eklemek

Yukarıdaki özellikleri sorgu yürütme işlemi sırasında uygulanacak ancak, bu alandaki öznitelikler yerine sorgu parametreleri olarak kodunuza genel olarak uygulanır.

<a name="types-of-queries"></a>

## <a name="types-of-queries-search-and-filter"></a>Sorgu türleri: arama ve filtreleme

Tanıtım örnek arama parametresi tarafından arama ölçütleri Altyapısı'na iletilir olarak belirlenmiştir. Uygulamada, iki ana sorgu türü vardır: `search` ve `filter`. 

+ `search` bir veya daha çok terimi tüm sorguları taramak *aranabilir* dizininizdeki alanları ve çalışmak için Google veya Bing gibi bir arama motoru beklediğiniz gibi çalışır. Giriş kullanımı örnekleri `search` parametresi.

+ `filter` sorguları tüm üzerinde bir boolean ifadesinin değerlendirme *filtrelenebilir* dizin alanları. Farklı `search`, `filter` sorgu büyük küçük harf duyarlılığı dize alanları dahil olmak üzere, bir alanın tam içeriğini eşleştirir.

Arama ve filtre birlikte veya ayrı olarak kullanabilirsiniz. Filtre ifadesi ilgi belgeleri tam olarak nitelemek için bir sorgu dizesi olmadan tek başına bir filtre kullanışlıdır. Bir sorgu dizesi hiçbir sözlü ya da dil analizi, herhangi bir Puanlama ve hiçbir sıralama yoktur. Arama dizesi boş olduğuna dikkat edin.

```
POST /indexes/nycjobs/docs/search?api-version=2017-11-11  
    {  
      "search": "",
      "filter": "salary_frequency eq 'Annual' and salary_range_from gt 90000",
      "count": "true"
    }
```

Birlikte kullanıldığında, filtre öncelikle tüm dizine uygulanır ve ardından arama filtre sonuçlarına gerçekleştirilir. Filtreler arama sorgusunun işlemesi gereken belge kümesini azalttığından, sorgu performansını iyileştirmeye yönelik kullanışlı bir teknik olabilir.

Filtre ifadeleri için söz dizimi, [OData filtre dilinin](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search) bir alt kümesidir. Arama sorguları için ya da kullanabilirsiniz [Basitleştirilmiş söz dizimi](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search) veya [Lucene sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search) aşağıda ele alınmıştır.


## <a name="choose-a-syntax-simple-or-full"></a>Bir söz dizimi seçin: Basit veya tam

Azure Search, Apache Lucene en üstünde yer alan ve genel ve özel sorguları işlemek için iki sorgu Çözümleyicileri arasında seçmenizi sağlar. Tipik arama istekleri şeklide varsayılan kullanılarak [Basit Sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search). Bu söz dizimi AND, OR, NOT dahil olmak üzere ortak arama işleçlerini, tümcecik, sonek ve öncelik işleçleri destekler.

[Lucene sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_syntax), eklediğinizde, etkin `queryType=full` isteğine bir parçası olarak geliştirilen yaygın olarak benimsenen ve açıklayıcı sorgu dilini kullanıma sunan [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html). Bu sorgu söz dizimini kullanarak özel sorgular sağlar:

+ [Alan kapsamlı sorgular](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_fields)
+ [Belirsiz arama](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_fuzzy)
+ [Yakınlık araması](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_proximity)
+ [Terim artırma](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_termboost)
+ [Normal ifade araması](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_regex)
+ [joker karakter araması](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_wildcard)

Boole işleçleri, çoğunlukla her iki sözdizimi, tam Lucene ek biçimlerde aynı şunlardır:

+ [Basit söz diziminde Boole işleçleri](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search#operators-in-simple-search)
+ [Boole işleçleri tam Lucene sözdizimi](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_boolean)

## <a name="required-and-optional-elements"></a>Gerekli ve isteğe bağlı öğeler

Sorgular, her zaman tek bir dizinde yönlendirilir. Dizinleri katamaz veya bir sorgu hedefi olarak özel veya geçici veri yapılarını oluşturun. 

Azure Search'e arama istekleri gönderirken, uygulamanızın arama kutusuna yazılan gerçek sözcüklerin yanı sıra belirtilebilecek birkaç parametre bulunur. Bu sorgu parametreleri, [tam metin arama deneyiminde](search-lucene-query-architecture.md) biraz daha derin denetim elde etmenizi sağlar.

Gerekli bir sorgu isteği öğelerinde bulunan aşağıdaki bileşenleri içerir:

+ İfade, uç nokta ve dizin belgeler koleksiyon hizmeti bir URL olarak burada `https://<your-service-name>.search.windows.net/indexes/<your-index-name>/docs`.
+ API sürümü (yalnızca REST) olarak ifade edilen `api-version`
+ sorgu veya yönetici API anahtarını, olarak ifade edilen `api-key`
+ Sorgu dizesi olarak ifade edilen `search`, olabilen belirtilmeyen boş bir arama gerçekleştirmek istiyorsanız. Yalnızca bir filtre ifadesi olarak da gönderebilirsiniz `$filter`.
+ `queryType`, basit veya tam varsayılan basit söz dizimi kullanmak istiyorsanız, atlanabilir.

Diğer tüm arama parametreleri isteğe bağlıdır.

## <a name="manage-search-results"></a>Arama sonuçlarını yönetme 

.NET API kullanırsanız, serileştirme yerleşik olan ancak sorgu sonuçları REST API'si, JSON belgeleri olarak aktarılır. Sonuçlar üzerindeki sorgu, parametreleri ayarlayarak sonuç için belirli alanları seçerek şekillendirebileceğinize

Sorgu parametreleri, sonuç aşağıdaki yollarla kümesi yapısı için kullanılabilir:

+ Sınırlama veya sonuçları (varsayılan olarak 50) belge sayısı toplu işleme
+ Sonuçların dahil edileceği alanları seçme
+ Sıralama düzenini ayarlama
+ Arama sonuçlarını gövdesinde koşulları eşleştirme dikkat çekmek için ekleme isabet vurgular.

### <a name="tips-for-unexpected-results"></a>Beklenmeyen sonuçlar için ipuçları

Bazen, madde temini ve sonuçları yapısı beklenmeyen. Sorgu sonuçlarını görmek beklediğiniz değil olduğunda sonuçlarını iyileştirmek için bu sorgu değişiklikleri deneyebilirsiniz:

+ Değişiklik `searchMode=any` (varsayılan) `searchMode=all` ölçütlerden herhangi birine yerine tüm ölçütleri eşleşme istemek için. Boole işleçleri eklendiğinde bu özellikle doğrudur sorgu.

+ Sorgu tekniği, metin veya sözcük temelli analize gereklidir, ancak sorgu türünü dil işleme ışığının değiştirin. Tam metin araması'nda, metin veya sözcük temelli analize otomatik olarak yazım hatalarını, tekil çoğul sözcük biçimlerini ve hatta düzensiz fiilleri veya isimleri için düzeltir. Bazı sorgular gibi belirsiz veya joker karakter araması, metin analizi değil, işlem hattı ayrıştırma sorgunun parçası. Bazı senaryolarda, normal ifadeler geçici bir çözüm olarak kullanılır. 

### <a name="paging-results"></a>Disk belleği sonuçları
Azure Search, arama sonuçlarının sayfalanması uygulamasını kolaylaştırır. `top` ve `skip` parametrelerini kullanarak, tüm arama sonuçları kümesini, iyi arama kullanıcı arabirimi uygulamalarını kolayca etkinleştiren yönetilebilir ve sıralı alt kümeler halinde almanızı sağlayan arama isteklerini sorunsuz bir şekilde gönderebilirsiniz. Bu daha küçük sonuç alt kümelerini alırken, tüm arama sonuçları kümesindeki belge sayısını da alabilirsiniz.

[Azure Search'te arama sonuçlarını numaralandırma](search-pagination-page-layout.md) makalesinde arama sonuçlarının numaralanması hakkında daha fazla bilgi alabilirsiniz.

### <a name="ordering-results"></a>Sonuçları sıralama
Bir arama sorgusunun sonuçları alınırken, Azure Search'ün sonuçları belirli bir alandaki değerlere göre sıralayarak sunmasını isteyebilirsiniz. Varsayılan olarak Azure Search, her bir belgenin [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)'den türetilen arama puanı sıralamasını temel alarak arama sonuçlarını sıralar.

Azure Search'ün sonuçlarınızı arama puanı dışında bir değere göre sıralayarak döndürmesini istiyorsanız `orderby` arama parametresini kullanabilirsiniz. `orderby` parametresinin değerini, alan adlarını ve jeo-uzamsal değerler için [`geo.distance()` işlevine](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search) çağrıları içerecek şekilde belirtebilirsiniz. Her bir ifadenin ardından, sonuçların artan sıralamada istendiğini belirtmek için `asc`, sonuçların azalan sıralamada istendiğini belirtmek için ise `desc` gelebilir. Artan sıralama varsayılandır.


### <a name="hit-highlighting"></a>İsabet vurgulama
Azure Search'te arama sonuçlarının arama sorgusuyla tam olarak eşleşen kısmının vurgulanması `highlight`, `highlightPreTag` ve `highlightPostTag` parametreleri kullanılarak kolaylaştırılır. Hangi *aranabilir* alanların eşleşen metninin vurgulanacağının yanı sıra Azure Search'ün döndürdüğü eşleşen metnin başına ve sonuna eklenecek dize etiketlerini tam olarak belirtebilirsiniz.

## <a name="apis-and-tools-for-testing"></a>API'ler ve test araçları

Aşağıdaki tabloda sorguları gönderme aracı tabanlı yaklaşımlar ve API'ları listeler.

| Yöntemi | Açıklama |
|-------------|-------------|
| [Searchındexclient (.NET)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient?view=azure-dotnet) | Bir Azure Search dizinini sorgulama için kullanılan istemci.  <br/>[Daha fazla bilgi edinin.](search-howto-dotnet-sdk.md#core-scenarios)  |
| [Search belgeleri (REST API'si)](https://docs.microsoft.com/rest/api/searchservice/search-documents) | GET veya POST yöntemleri için ek giriş sorgu parametrelerini kullanarak, bir dizin üzerinde.  |
| [Fiddler veya Postman diğer HTTP test aracı](search-fiddler.md) | İstek üst bilgisi ve gövdesi sorgular Azure Search'e göndermek için nasıl ayarlanacağı açıklanır.  |
| [Azure portalında arama Gezgini](search-explorer.md) | Arama çubuğu ve dizin ve API sürümü seçimleri için seçenekler sağlar. Sonuçlar, JSON belgeleri olarak döndürülür. <br/>[Daha fazla bilgi edinin.](search-get-started-portal.md#query-index) | 

## <a name="see-also"></a>Ayrıca bkz.

+ [Nasıl tam metin araması (sorgu mimarisi ayrıştırma Azure Search'te çalışır](search-lucene-query-architecture.md)
+ [Arama Gezgini](search-explorer.md)
+ [. NET'te sorgulama](search-query-dotnet.md)
+ [KALAN sorgulama](search-query-rest-api.md)
