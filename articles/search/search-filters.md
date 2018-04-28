---
title: Azure Search'te filtreleri | Microsoft Docs
description: Kullanıcının güvenlik kimliği, dil, coğrafi konuma veya Azure arama, Microsoft Azure üzerinde barındırılan bulut arama hizmeti sorgularda arama sonuçları azaltmak için sayısal değerleri göre filtreleyin.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: heidist
ms.openlocfilehash: 9f891dbe3f051f2fb5bfd242830f3c30abede487
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="filters-in-azure-search"></a>Azure Search'te filtreler 

A *filtre* bir Azure Search sorguda kullanılan belgeler seçmek için ölçüt sağlar. Filtre uygulanmamış arama dizinde tüm belgeleri içerir. Bir filtre, bir arama sorgusu belgelerin alt kapsamları. Örneğin, bir filtre, belirli bir marka veya renk, belirli bir eşiğin üstünde fiyat noktalarda sahip yalnızca bu ürünler için tam metin araması sınırlayabilirsiniz.

Uygulama bir parçası olarak bazı arama deneyimleri filtre gereksinimleri, ancak arama'yı kullanarak kısıtlamak istediğiniz zaman filtreleri kullanabilirsiniz *değeri tabanlı* ölçütleri (kapsam aramak için ürün türü "books" kategorisi" olmayan kurgu""Tarafından Simon & Schuster"yayımlanan).

Bunun yerine amacınız belirli veri hedeflenen Search'te ise *yapıları* (müşteri incelemelerini alanına kapsam arama), aşağıda açıklanan alternatif yöntemler vardır.

## <a name="when-to-use-a-filter"></a>Bir filtreyi kullanmak ne zaman

Filtreler "modellenmiş bir gezinmede ve güvenlik filtreleri gösteren yalnızca bir kullanıcı görmek için izin verilen belgeleri Bul bana yakın", dahil olmak üzere birkaç arama deneyimleri temel. Bu deneyimler herhangi biri uygularsanız, bir filtre gereklidir. Coğrafi konum koordinatlarını sağlar arama sorgusu kullanıcı veya güvenlik kimliği istek sahibi tarafından seçilen model kategoriye ekli filtredir.

Örnek senaryolar aşağıdakileri içerir:

1. Dizindeki veri değerleri temel alarak dizininizi dilim için bir filtre kullanın. Şehir, housing türü ve s şemasıyla verilir, açıkça (, Seattle, apartman dairesi rıhtımının) ölçütlerinizi karşılayan belgeleri seçmek için bir filtre oluşturabilirsiniz. 

  Tam metin araması aynı giriş ile genellikle benzer sonuçlar verir, ancak tam bir eşleşme içeriğinde dizininiz karşı filtre koşulunun gerekir bir filtre daha kesin değildir. 

2. Bir filtre gereksinim ile arama deneyimini geliyorsa, bir filtre kullanın:

 * [Modellenmiş bir gezinmede](search-faceted-navigation.md) geri kullanıcı tarafından seçilen model kategori geçirmek için bir filtre kullanır.
 * Coğrafi arama "Bul" Benim yakın geçerli konumda koordinatlarını uygulamaları geçirmek için bir filtre kullanır. 
 * Güvenlik filtreleri güvenlik tanımlayıcıları burada dizindeki bir eşleşme belgeye erişim hakları için proxy olarak hizmet veren filtre ölçütü olarak geçirin.

3. Bir sayısal alana arama ölçütlerine istiyorsanız bir filtre kullanın. 

  Sayısal alanlar belgede alınabilir ve arama sonuçlarında görünebilir, ancak bunlar ayrı ayrı (tam metin araması tabi) aranabilir değil. Sayısal veriler üzerinde dayalı seçim ölçütleri gerekiyorsa, bir filtre kullanın.

### <a name="alternative-methods-for-reducing-scope"></a>Kapsam azaltmak için alternatif yöntemler

Filtreler Arama sonuçlarınızda daraltma etkisi istiyorsanız, yalnızca tercih ettiğiniz değildir. Bu alternatifleri hedefiniz bağlı olarak daha iyi bir uyum olabilir:

 + `searchFields` sorgu parametresi belirli alan arama pegs. Örneğin, dizininiz için İngilizce ve İspanyolca açıklamalar ayrı alanları sağlıyorsa, hangi alanlar için tam metin araması kullanmak için hedef için searchFields kullanabilirsiniz. 

+ `$select` parametre bir sonucunda eklemek için hangi alanları, çağıran uygulama göndermeden önce yanıt etkili bir şekilde kırpma belirlemek için kullanılır. Bu parametre sorgu daraltmayı ya da belge koleksiyonunu azaltmak, ancak ayrıntılı yanıt amacınız ise, bu parametre dikkate alınması gereken bir seçenektir. 

Her iki parametre hakkında daha fazla bilgi için bkz: [Search belgeleri > İstek > sorgu parametreleri](https://docs.microsoft.com/rest/api/searchservice/search-documents#request).


## <a name="filters-in-the-query-pipeline"></a>Sorgu ardışık düzeninde filtreleri

Filtre ayrıştırıcı ölçütleri giriş olarak kabul eder sorgu zaman ifade atomik Boolean ifadelere dönüştürür ve ardından dizin filtrelenebilir alanları üzerinde hesaplanan bir filtre ağacı oluşturur.  

Filtreleme belge alma için aşağı akış işleme ve ilgi Puanlama dahil edilecek hangi belgeleri niteleme arama önce gerçekleşir. Bir arama dizesi ile eşlenmiş, filtre etkili bir şekilde sonraki arama işlemi'nın yüzey alanını azaltır. Tek başına kullanıldığında (örneğin, sorgu dizesi olduğunda boş nereye `search=*`), filtre ölçütlerini olan tek giriş. 

## <a name="filter-definition"></a>Filtre tanımı

Filtreleri kullanarak geliştirilmiştir OData ifadeleri olan bir [Azure Search'te desteklenen OData V4 sözdizimi kısmı](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search). 

Her biri için bir filtre belirtebilirsiniz **arama** işlemi, ancak filtre birden çok alan, birden çok ölçüt içerebilir ve kullanırsanız, bir **IsMatch** işlevi, birden çok ifade. Çok parçalı filtre ifadesinde herhangi bir sırada koşulları belirtebilirsiniz. Belirli bir sırayla koşullarında yeniden denerseniz, performansı fark edilir hiçbir kazanç yoktur.

Bir filtre ifadesi sabit sınırı istekte üst sınırı ' dir. Filtre planlayacak tüm istek en fazla 16 MB POST veya GET için 8 KB olabilir. Filtre ifadesi yan tümcelerinde sayısına esnek sınırlar ilişkilendirmek. Bir iyi yan tümceleri yüzlerce varsa, sınırı içinde çalışan risk altındadır udur. Sınırsız boyutunun filtreleri oluşturmaz şekilde uygulamanızı tasarlama öneririz.

Aşağıdaki örnekler API'leri Prototipik filtresi tanımlarının temsil eder.

```http
# Option 1:  Use $filter for GET
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2017-11-11

# Option 2: Use filter for POST and pass it in the header
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2017-11-11
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

Aşağıdaki örneklerde filtre senaryoları için birkaç tasarım desenleri gösterilmektedir. Daha fazla fikir için bkz: [OData ifade sözdizimini > örnekler](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search#bkmk_examples).

+ Tek başına **$filter**, filtre ifadesi ilgi belgeleri tam olarak nitelemek mümkün olduğunda yararlı bir sorgu dizesi olmadan. Bir sorgu dizesi sözcük veya dil çözümleme, hiçbir Puanlama ve herhangi bir derecelendirme yoktur. Arama dizesi boş olduğuna dikkat edin.

   ```
   search=*&$filter=(baseRate ge 60 and baseRate lt 300) and accommodation eq 'Hotel' and city eq 'Nogales'
   ```

+ Sorgu dizesi birleşimi ve **$filter**, burada filtre alt oluşturur ve sorgu dizesi terim girişleri için tam metin araması filtrelenmiş alt sağlar. Bir filtre bir sorgu dizesi en yaygın kod düzeni kullanmaktır.

   ```
   search=hotels ocean$filter=(baseRate ge 60 and baseRate lt 300) and city eq 'Los Angeles'
   ```

+ Ayrılmış sorguları, bileşik "veya", her biri kendi filtre ölçütlerini (örneğin, 'köpek' içinde ' beagles') ya da 'Kat' ' siamese'. VEYA sahip ifadeleri değerlendirilir ayrı ayrı her bir geri çağırma uygulamaya gönderilen bir yanıtı halinde birleştirilmiş yanıt. Bu tasarım deseni search.ismatch işlevi ile elde edilir. Puanlama olmayan sürümünü (search.ismatch) veya Puanlama sürümünü (search.ismatchscoring) kullanabilirsiniz.

   ```
   # Match on hostels rated higher than 4 OR 5-star motels.
   $filter=search.ismatchscoring('hostel') and rating ge 4 or search.ismatchscoring('motel') and rating eq 5

   # Match on 'luxury' or 'high-end' in the description field OR on category exactly equal to 'Luxury'.
   $filter=search.ismatchscoring('luxury | high-end', 'description') or category eq 'Luxury'
   ```

Bu makalelerde kapsamlı kılavuzu için üzerinde belirli kullanım durumları izle:

+ [Model filtreleri](search-filters-facets.md)
+ [Dil filtreleri](search-filters-language.md)
+ [Güvenlik kırpma](search-security-trimming-for-azure-search.md) 

## <a name="field-requirements-for-filtering"></a>Filtrelemek için alan gereksinimleri

REST API filtrelenemez olup *üzerinde* varsayılan olarak. Filtrelenebilir alanları dizin boyutunu artırın; ayarladığınızdan emin olun `filterable=FALSE` gerçekte bir filtre kullanmayı planlıyorsanız olmayan alanlar için. Alan tanımları için ayarları hakkında daha fazla bilgi için bkz: [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index).

.NET SDK'ın filtrelenemez olup *kapalı* varsayılan olarak. Filtrelenebilir özelliğini ayarlamak için API [IsFilterable](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.isfilterableattribute). Aşağıda, kendi BaseRate alan tanımı kümesinde örnekte.

```csharp
    [IsFilterable, IsSortable, IsFacetable]
    public double? BaseRate { get; set; }
```

### <a name="reindexing-requirements"></a>Tamamlanmasından gereksinimleri

Bir alan filtrelenebilir olmayan ise ve filtrelenebilir yapmak istediğiniz, yeni bir alan eklemek veya var olan alanın yeniden gerekir. Bir alan tanımını değiştirmeyi fiziksel dizin yapısını değiştirir. Azure Search'te tüm izin verilen erişim yolları alan tanımları değiştirdiğinizde, veri yapılarını yeniden gerekir hızlı sorgu hızı için dizinlenir. 

Tek tek alanların yeniden mevcut belge anahtarı ve ilişkili değerleri her belgenin geri kalanında dokunmadan dizinine gönderen bir birleştirme işlemi gerektiren bir düşük etkisi işlemi olabilir. Yeniden gereksinim karşılaşırsanız, yönergeler için aşağıdaki bağlantılara bakın:

 + [.NET SDK kullanarak dizin oluşturma eylemleri](https://docs.microsoft.com/azure/search/search-import-data-dotnet#decide-which-indexing-action-to-use)
 + [REST API kullanarak dizin oluşturma eylemleri](https://docs.microsoft.com/azure/search/search-import-data-rest-api#decide-which-indexing-action-to-use)

## <a name="text-filter-fundamentals"></a>Metin filtresi temelleri

Metin filtreleri arama Gövde içinde değerlere göre belgeleri rasgele bazı topluluğu çekmek istediğiniz dize alanları için geçerlidir.

Dizeleri oluşan metin filtreleri, sözcük analiz veya yoktur Sözcük bölünmesi, yalnızca tam eşleşme karşılaştırmaları; bu nedenle. Örneğin, bir alan varsayın *f* "güneşli gün", içeren `$filter=f eq 'Sunny'`eşleşmiyor, ancak `$filter=f eq 'Sunny day'` olur. 

Metin dizelerini büyük/küçük harfe duyarlıdır. Hiçbir alt-büyük/küçük harf üst ortası sözcüklerin yoktur: `$filter=f eq 'Sunny day'` değil bulacaksınız ' güneşli gün ''.


| Yaklaşım | Açıklama | 
|----------|-------------|
| [Search.in()](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) | Belirli bir alan için dizelerin virgülle ayrılmış listesi sağlayan işlev. Dizeleri kapsamında sorgu için her alan için uygulanan filtre ölçütünü kapsar. <br/><br/>`search.in(f, ‘a, b, c’)` anlam olarak eşdeğerdir `f eq ‘a’ or f eq ‘b’ or f eq ‘c’`, değerler listesinin büyük olduğunda çok daha hızlı yürütür dışında.<br/><br/>Öneririz **search.in** için işlev [güvenlik filtreleri](search-security-trimming-for-azure-search.md) ve belirli bir alandaki değerlere eşleştirilmesini ham metni herhangi bir filtre oluşan için. Bu yaklaşım hızı için tasarlanmıştır. Yüz binlerce değerleri için subsecond yanıt süresi bekleyebilirsiniz. İşleve geçirebilirsiniz öğe sayısını sınırlama yoktur açık olsa da, gecikme sağladığınız dizeleri sayısı orantılı olarak artar. | 
| [Search.ismatch()](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) | Tam metin araması işlemlerini kesinlikle Boole filtresi işlemlerle aynı filtre ifadesindeki karışık olanak sağlayan bir işlev. Bir istek birden çok sorgu filtresi bileşimlerde sağlar. Bunun için de kullanabilirsiniz bir *içeren* büyük bir dizi içinde kısmi dize üzerinde filtrelemek için. |  
| [$filter = alan işleci dize](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) | Kullanıcı tanımlı bir ifade alanları, işleçler ve değerler oluşur. | 

## <a name="numeric-filter-fundamentals"></a>Sayısal filtre temelleri

Sayısal alanlar değildir `searchable` tam metin araması bağlamında. Yalnızca tam metin araması tabi dizelerdir. Örneğin, bir arama terimi 99,99 girerseniz, 99,99 fiyatlandırılır öğeleri geri alamazsınız. Bunun yerine, belgenin dize alanları 99 numarasına sahip öğeleri görürsünüz. Sayısal veri varsa, bu nedenle, siz bunları aralıkları, modelleri, grupları ve diğerleri de dahil olmak üzere filtreleri için kullanacağı varsayılır. 

Sayısal alanlar (Fiyat, boyut, SKU, kimliği) içeren belgeleri alan işaretlenmişse arama sonuçlarında bu değerleri sağlayın `retrievable`. Burada, tam metin araması sayısal alan türleri için geçerli değildir noktasıdır.

## <a name="next-steps"></a>Sonraki adımlar

Öncelikle, deneyin **arama Gezgini** sorguları göndermek için portalda **$filter** parametreleri. [Gerçek varlıklarının örnek dizin](search-get-started-portal.md) Arama çubuğuna yapıştırın, aşağıdaki sorguları filtre ilginç sonuçlar sağlar:

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

Daha fazla örnek ile çalışmak için bkz: [OData filtre ifadesi sözdizimi > örnekler](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search#bkmk_examples).

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Search'te nasıl tam metin araması çalışır](search-lucene-query-architecture.md)
+ [REST API belgelerde arama](https://docs.microsoft.com/rest/api/searchservice/search-documents)
+ [Basit sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)
+ [Lucene sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)
+ [Desteklenen veri türleri](https://docs.microsoft.com/rest/api/searchservice/supported-data-types)
