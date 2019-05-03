---
title: Dizin - Azure Search arama sonuçlarında kapsamı için filtreler
description: Kullanıcı güvenlik kimliği, dil, coğrafi konum veya sorgular, Azure arama, Microsoft Azure üzerinde barındırılan bulut arama hizmeti üzerinde arama sonuçları azaltmak için sayısal değerler göre filtreleyin.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 49f971fb50d0a8a6a0dab09158f780206a4d32f1
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65024842"
---
# <a name="filters-in-azure-search"></a>Azure Search'te filtreler 

A *filtre* belgeleri bir Azure Search sorguda kullanılan seçmek için ölçütleri sağlar. Filtrelenmemiş arama dizininde tüm belgeleri içerir. Bir filtre, bir arama sorgusu belgelerin alt kapsamlar. Örneğin, bir filtre tam metin araması yalnızca ürünlerin belirli bir marka veya renk, belirli bir eşiğin üstünde fiyat noktalarında sahip kısıtlayabilirsiniz.

Bazı arama deneyimleri, uygulamanın bir parçası filtre gereksinimlerini zorunlu tuttukları ancak arama ile sınırlamak dilediğiniz filtreleri kullanabilirsiniz *değer tabanlı* ölçütleri (kapsam aramak için ürün türü "books" Kategori" kurgu olmayan""Simon & Schuster"tarafından yayımlanan).

Bunun yerine belirli veri çubuğunda hedeflenen arama amaçlıyorsanız *yapıları* (müşteri incelemeleri alanına kapsam arama), aşağıda açıklanan diğer yöntemleri vardır.

## <a name="when-to-use-a-filter"></a>Ne zaman bir filtreyi kullanın

"Çok yönlü gezinme ve güvenlik filtreleri gösteren bir kullanıcının görmek için verilen belgeleri Yakınımda Bul", dahil olmak üzere çeşitli arama deneyimleri temel filtreler. Bu deneyimler herhangi biri uygularsanız, bir filtre gereklidir. Coğrafi konum koordinatlarını sağlayan arama sorgusu kullanıcı veya güvenlik kimliği istek sahibi tarafından seçilen modeli kategoriye bağlı filtresidir.

Örnek senaryolar aşağıdakileri içerir:

1. Dizin veri değerlere göre dizininizi dilim için filtre kullanın. Şehir, konut türü ve kullanılmıyordu sahip bir şema göz önünde bulundurulduğunda, (içinde Seattle, apartman dairesi, rıhtımının) ölçütlerinizi karşılayan belgeleri açıkça seçmek için bir filtre oluşturabilir. 

   Tam metin araması aynı girişlerle genellikle şuna benzer sonuçlar üretir, ancak tam bir eşleşme içerik dizininize göre filtre döneminin gerektirir, bir filtre daha kesin. 

2. Arama deneyimi, bir filtre gereksinimiyle geliyorsa bir filtre kullanın:

   * [Çok yönlü gezinme](search-faceted-navigation.md) kullanıcı tarafından seçilen modeli kategori geri geçirmek için bir filtre kullanır.
   * Coğrafi arama, "Yakınımda Bul" geçerli konumu koordinatlarını uygulamaları geçirmek için bir filtre kullanır. 
   * Güvenlik filtreleri güvenlik tanımlayıcıları, burada dizindeki bir eşleşme belgeye erişim hakları için proxy olarak hizmet veren filtre ölçütü olarak geçirin.

3. Arama ölçütleri sayısal bir alan üzerinde filtre kullanın. 

   Sayısal alanlar belge odalı ve arama sonuçlarında görünebilir, ancak bunlar ayrı ayrı (tam metin araması tabidir) aranabilir değil. Sayısal verileri temel alan seçim ölçütü gerekirse bir filtre uygulayın.

### <a name="alternative-methods-for-reducing-scope"></a>Kapsam azaltmak için alternatif yöntemler

Filtreler arama sonuçlarınızı daraltma etkisi istiyorsanız, yalnızca seçtiğiniz değildir. Bu alternatifleri hedefiniz bağlı olarak, daha uygun olabilir:

 + `searchFields` sorgu parametresi belirli alanlarında arama pegs. Örneğin, dizininizin İngilizce ve İspanyolca açıklamaları için ayrı alanlara sağlıyorsa, hangi alanlar tam metin araması için kullanılacak hedef için searchFields kullanabilirsiniz. 

+ `$select` parametre bir sonucuna dahil edilecek hangi alanların, etkili bir şekilde çağıran uygulama için göndermeden önce yanıt kırpma belirlemek için kullanılır. Bu parametre sorguyu daraltmak etmez veya belge koleksiyonunu azaltmak, ancak ayrıntılı yanıt hedefiniz varsa bu parametre dikkate alınması gereken bir seçenektir. 

Her iki parametre hakkında daha fazla bilgi için bkz. [belge Ara > İstek > sorgu parametreleri](https://docs.microsoft.com/rest/api/searchservice/search-documents#request).


## <a name="filters-in-the-query-pipeline"></a>Sorgu işlem hattındaki filtreler

Sorgu zamanında bir filtre ayrıştırıcı ölçütleri giriş olarak kabul eder, atomik Boolean ifadeleri ifade dönüştürür ve dizin filtrelenebilir alanlara üzerinden değerlendirilir bir filtre ağacı oluşturur.  

Filtreleme, arama, hangi belgeler belge almak için aşağı akış işleme ve ilgi düzeyi Puanlama dahil edilecek uygun önce gerçekleşir. Bir arama dizesi ile birlikte kullanıldığında filtresini etkili bir şekilde sonraki arama işlemi'nın yüzey alanını azaltır. Tek başına kullanıldığında (örneğin, sorgu dizesi iken boş nerede `search=*`), filtre ölçütlerini olan tek giriş. 

## <a name="filter-definition"></a>Filtre tanımı

OData ifadeleri kullanarak geliştirilmiştir, filtrelerdir bir [alt kümesini Azure Search'te desteklenen OData V4 sözdizimi](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search). 

Her biri için bir filtre belirtebilirsiniz **arama** işlemi, ancak filtre birden çok alan, birden çok ölçüt içerebilir ve bir **IsMatch** işlevi, birden fazla ifade. Çok parçalı filtre ifadesinde, herhangi bir sırada koşulları belirtebilirsiniz. Belirli bir sırada doğrulamaları yeniden denerseniz, performansta önemli hiçbir kazanç yoktur.

Bir filtre ifadesi sabit sınırı istekte üst sınır:. Filtre tamamlanmıyorsa, tüm istek en fazla 16 MB POST veya GET için 8 KB olabilir. Bulunsa da bu sınırlar, filtre ifadesi yan tümcelerinde sayısına ilişkilendirin. Bir iyi yan tümceleri yüzlerce varsa, sınırı içinde çalışan risk altındadır udur. Sınırsız boyutunun filtreleri oluşturmaz şekilde uygulamanızı tasarlama öneririz.

Aşağıdaki örnekler birkaç API Prototipik filtre tanımlarında temsil eder.

```http
# Option 1:  Use $filter for GET
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2019-05-06

# Option 2: Use filter for POST and pass it in the header
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2019-05-06
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

```csharp
    parameters =
        new SearchParameters()
        {
            Filter = "baseRate lt 150",
            Select = new[] { "hotelId", "description" }
        };

```

## <a name="filter-design-patterns"></a>Filtre tasarım desenleri

Aşağıdaki örnekler, filtre senaryoları için çeşitli tasarım desenleri gösterir. Daha fazla fikir için bkz: [OData ifadesi söz dizimi > örnekler](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search#filter-examples).

+ Tek başına **$filter**, filtre ifadesi ilgi belgeleri tam olarak nitelemek için yararlı bir sorgu dizesi olmadan. Bir sorgu dizesi hiçbir sözlü ya da dil analizi, herhangi bir Puanlama ve hiçbir sıralama yoktur. Arama dizesi boş olduğuna dikkat edin.

   ```
   search=*&$filter=(baseRate ge 60 and baseRate lt 300) and accommodation eq 'Hotel' and city eq 'Nogales'
   ```

+ Sorgu dizesi birleşimi ve **$filter**, burada filtre alt oluşturur ve sorgu dizesi terimi girişleri tam metin araması için filtrelenmiş bir alt kümesi üzerinde sağlar. Bir sorgu dizesi ile bir filtre kullanarak en yaygın kod modelidir.

   ```
   search=hotels ocean$filter=(baseRate ge 60 and baseRate lt 300) and city eq 'Los Angeles'
   ```

+ Sorgular, ayrılmış bileşik "veya", her biri kendi filtre ölçütü (örneğin, 'köpek' içinde ' beagles') ya da 'cat' ' siamese'. VEYA sahip ifadeler değerlendirilir ayrı ayrı her bir geri çağırma uygulamaya gönderilen bir yanıt olarak bir araya yanıt. Bu tasarım deseni search.ismatch işlevi ile elde edilir. Puanlama sürümü (search.ismatch) veya Puanlama sürümünü (search.ismatchscoring) kullanabilirsiniz.

   ```
   # Match on hostels rated higher than 4 OR 5-star motels.
   $filter=search.ismatchscoring('hostel') and rating ge 4 or search.ismatchscoring('motel') and rating eq 5

   # Match on 'luxury' or 'high-end' in the description field OR on category exactly equal to 'Luxury'.
   $filter=search.ismatchscoring('luxury | high-end', 'description') or category eq 'Luxury'
   ```

Bu makalelerde kapsamlı kılavuzu için belirli kullanım durumlarını iletişim kurun:

+ [Model filtreleri](search-filters-facets.md)
+ [Dil filtreleri](search-filters-language.md)
+ [Güvenlik kırpma](search-security-trimming-for-azure-search.md) 

## <a name="field-requirements-for-filtering"></a>Alan gereksinimleri filtreleme

REST API'SİNDE filtrelenemez olup *üzerinde* varsayılan olarak. Filtrelenebilir alanlardan dizin boyutunu artırın; ayarladığınızdan emin olun `filterable=FALSE` aslında bir filtre kullanmayı planlamıyor alanlar için. Alan tanımları için ayarları hakkında daha fazla bilgi için bkz. [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index).

.NET SDK'da filtrelenemez olup *kapalı* varsayılan olarak. Filtrelenebilir özelliği ayarlamak için API [IsFilterable](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.isfilterableattribute). Kendi BaseRate kümesinde aşağıdaki örnekte tanımı alan.

```csharp
    [IsFilterable, IsSortable, IsFacetable]
    public double? BaseRate { get; set; }
```

### <a name="reindexing-requirements"></a>Tamamlanmasından gereksinimleri

Bir alan filtrelenebilir olmayan ve filtrelenebilir yapmak istiyorsanız yeni bir alan eklemek veya var olan alanın yeniden oluşturmak zorunda. Bir alan tanımı değiştirilmesi dizinin fiziksel yapısını değiştirir. Azure Search'te tüm izin verilen erişim yollarını, alan tanımları değiştirdiğinizde, veri yapılarının yeniden BIOS'ta hızlı sorgu hızı için dizine eklenir. 

Her bir alanı yeniden dizine her belgenin geri kalanında dokunmadan, mevcut belge anahtarını ve ilişkili değerleri gönderen bir birleştirme işlemi gerektiren bir düşük etki işlemi olabilir. Yeniden gereksinim karşılaşırsanız bkz [eylemleri dizin oluşturma (karşıya yükleme, birleştirme, mergeOrUpload, silme)](search-what-is-data-import.md#indexing-actions) seçeneklerin bir listesi için.


## <a name="text-filter-fundamentals"></a>Metin filtresi temelleri

Metin filtreleri arama dizini içindeki değerlere göre belgelerin bazı rastgele koleksiyonu çekmek istediğiniz, dize alanları için geçerlidir.

Dizeleri oluşan metin filtreleri için sözcük analizi veya yoktur sözcük bölme, bu nedenle karşılaştırmalar yalnızca tam eşleşme. Örneğin, bir alan varsayın *f* "güneşli gün" içeren `$filter=f eq 'Sunny'`eşleşmiyor, ancak `$filter=f eq 'Sunny day'` olur. 

Metin dizelerinin büyük/küçük harfe duyarlıdır. Hiçbir alt-büyük/küçük harf bir kelimelerin büyük harfleri yoktur: `$filter=f eq 'Sunny day'` bulmaz ' güneşli gün ''.


| Yaklaşım | Açıklama | 
|----------|-------------|
| [search.in()](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) | Belirli bir alanı için dizelerin virgülle ayrılmış listesi sağlayan bir işlev. Dizeleri kapsamda sorgu için her alana uygulanan filtre ölçütlerini kapsar. <br/><br/>`search.in(f, ‘a, b, c’)` anlamsal olarak eşdeğer olan `f eq ‘a’ or f eq ‘b’ or f eq ‘c’`dışında değerler listesinin büyük olduğunda çok daha hızlı yürütür.<br/><br/>Öneririz **search.in** için işlev [güvenlik filtreleri](search-security-trimming-for-azure-search.md) ve herhangi bir filtre, belirli bir alandaki değerlere eşleştirilecek ham metin oluşan için. Bu yaklaşım, hız için tasarlanmıştır. Yüz binlerce değerleri için subsecond yanıt süresi bekleyebilirsiniz. Açık sınırsız işlevine geçirilebilecek öğe sayısını olsa da, gecikme süresi derlemekten sağladığınız dizeleri sayısını artırır. | 
| [search.ismatch()](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) | Tam metin arama işlemleri kesinlikle Boole filtresi işlemleri aynı filtre ifadesi olan karıştırmak izin veren bir işlev. Bu, birden çok sorgu filtresi kombinasyonları için geçerli bir istek sağlar. Bunun için de kullanabilirsiniz bir *içeren* filtresi üzerinde daha büyük bir dizedeki bir kısmi dize. |  
| [$filter işleci dizesi alanı =](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) | Kullanıcı tanımlı bir ifade alanlarını, işleçlerini ve değerlerini oluşur. | 

## <a name="numeric-filter-fundamentals"></a>Sayısal filtre temelleri

Sayısal alanlar olmayan `searchable` bağlamında, tam metin araması. Yalnızca tam metin araması tabi dizelerdir. Örneğin, bir arama terimi 99,99 girerseniz, 99,99 ücretlendirilen öğeler geri alamazsınız. Bunun yerine, dize alanlarına belgenin numarası 99 olan öğeleri görürsünüz. Sayısal veri varsa, bu nedenle, bunları aralıkları, modelleri, grupları ve diğerleri dahil olmak üzere, filtreleri için kullanacağı varsayılır. 

Sayısal alanlar (Fiyat, boyutunu, SKU, kimliği) içeren belgeleri alan olarak işaretlendiğinden, bu değer arama sonuçlarında sağlamak `retrievable`. Burada nokta, tam metin araması sayısal alan türler için geçerli değil.

## <a name="next-steps"></a>Sonraki adımlar

İlk olarak, deneyin **arama Gezgini** ile sorguları göndermek için portaldaki **$filter** parametreleri. [Gerçek Emlak örnek dizini](search-get-started-portal.md) Arama çubuğuna yapıştırın, aşağıdaki sorguları filtrelenmiş ilginç sonuçlar sağlar:

```
# Geo-filter returning documents within 5 kilometers of Redmond, Washington state
# Use $count=true to get a number of hits returned by the query
# Use $select to trim results, showing values for named fields only
# Use search=* for an empty query string. The filter is the sole input

search=*&$count=true&$select=description,city,postCode&$filter=geo.distance(location,geography'POINT(-122.121513 47.673988)') le 5

# Numeric filters use comparison like greater than (gt), less than (lt), not equal (ne)
# Include "and" to filter on multiple fields (baths and bed)
# Full text search is on John Leclerc, matching on John or Leclerc

search=John Leclerc&$count=true&$select=source,city,postCode,baths,beds&$filter=baths gt 3 and beds gt 4

# Text filters can also use comparison operators
# Wrap text in single or double quotes and use the correct case
# Full text search is on John Leclerc, matching on John or Leclerc

search=John Leclerc&$count=true&$select=source,city,postCode,baths,beds&$filter=city gt 'Seattle'
```

Daha fazla örnek ile çalışmak için bkz [OData filtre ifadesinin söz dizimi > örnekler](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search#filter-examples).

## <a name="see-also"></a>Ayrıca bkz.

+ [Metin arama Azure Search'te tam nasıl çalışır](search-lucene-query-architecture.md)
+ [Search belgeleri REST API'si](https://docs.microsoft.com/rest/api/searchservice/search-documents)
+ [Basit sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)
+ [Lucene sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)
+ [Desteklenen veri türleri](https://docs.microsoft.com/rest/api/searchservice/supported-data-types)
