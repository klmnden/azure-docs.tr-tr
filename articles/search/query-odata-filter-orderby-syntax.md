---
title: OData ifadesi söz dizimi filtreleri ve sipariş tarafından tümceleri - Azure Search için
description: Filtre ve order by deyimi Azure arama sorguları için OData söz dizimi.
ms.date: 03/27/2019
services: search
ms.service: search
ms.topic: conceptual
author: Brjohnstmsft
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
ms.openlocfilehash: ab98c3be75fb59603be66ee84e0d288de56cdc91
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58648512"
---
# <a name="odata-expression-syntax-for-filters-and-order-by-clauses-in-azure-search"></a>OData ifadesi söz dizimi filtreleri ve Azure Search order by yan tümceleri

Azure Search'ü destekleyen bir alt kümesi için OData ifadesi söz dizimi **$filter** ve **$orderby** ifadeler. Filtre ifadeleri ayrıştırma, belirli alanlarında arama sınırlamak veya dizin taramaları sırasında kullanılan eşleştirme ölçütü ekleme sorgusu sırasında değerlendirilir. Order by ifadeleri bir sonradan işleme adımı bir sonuç kümesi uygulanır. Filtreler hem order by ifadeleri dahil edilecek bir OData sözdizimi bağımsız olarak uymak bir sorgu isteği [basit](query-simple-syntax.md) veya [tam](query-lucene-syntax.md) sorgu söz dizimi kullanılan bir **arama** parametre. Bu makalede, filtreleri ve sıralama ifadeleri kullanılan OData ifadeler için başvuru belgeleri sağlar.

## <a name="filter-syntax"></a>Filtre söz dizimi

A **$filter** ifade tek başına tam olarak ifade edilen bir sorgu olarak çalıştırmak veya ek parametrelere sahip bir sorgu daraltın. Aşağıdaki örnekler birkaç önemli senaryolar gösterir. İlk örnekte, madde temini sorgunun bir filtredir.


```POST
POST /indexes/hotels/docs/search?api-version=2017-11-11
    {
      "filter": "(baseRate ge 60 and baseRate lt 300) or hotelName eq 'Fancy Stay'"
    }
```

Başka bir ortak kullanım örneği filtreleri Birleşik Modelleme, burada sorgu yüzey kullanıcı tabanlı model Gezinti seçime göre filtre azaltır şöyledir:

```POST
POST /indexes/hotels/docs/search?api-version=2017-11-11
    {
      "search": "test",
      "facets": [ "tags", "baseRate,values:80|150|220" ],
      "filter": "rating eq 3 and category eq 'Motel'"
    }
```

### <a name="filter-operators"></a>Filtre işleçleri  

- Mantıksal işleçler (ve, veya değil).  

- Karşılaştırma ifadeleri (`eq, ne, gt, lt, ge, le`). Dize karşılaştırmaları büyük/küçük harfe duyarlıdır.  

- Desteklenen sabitleri [varlık veri modeli](https://docs.microsoft.com/dotnet/framework/data/adonet/entity-data-model) (EDM) türleri (bkz [desteklenen veri türleri &#40;Azure Search&#41; ](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) desteklenen türlerinin bir listesi için). Sabit koleksiyon türleri desteklenmez.  

- Alan adlarına başvurular. Yalnızca `filterable` filtre ifadelerinde alanlar kullanılabilir.  

- `any` Hiçbir parametre olmadan. Bu tür bir alan olup olmadığını test `Collection(Edm.String)` tüm öğeleri içerir.  

- `any` ve `all` sınırlı bir lambda ifadesi desteği. 
    
  -   `any/all` türünde alanlar üzerinde desteklenen `Collection(Edm.String)`. 
    
  -   `any` yalnızca basit eşitlik ifadeleridir veya ile kullanılabilir bir `search.in` işlevi. Örneğin oluşmalıdır ve tek bir alan ve bir değişmez değer arasında bir karşılaştırma basit ifadeler `Title eq 'Magna Carta'`.
    
  -   `all` yalnızca basit eşitsizlik ifadeleri ile veya kullanılabilir bir `not search.in`.   

- Jeo-uzamsal işlevler `geo.distance` ve `geo.intersects`. `geo.distance` İşlevi iki nokta mesafeyi içinde uzaklık döndürür, geçirilen filtre bir parçası olarak bir alan ve bir sabit olan bir oluşturuluyor. `geo.intersects` Belirli bir noktaya içinde belirli bir Çokgen ise, bir alan noktasıdır ve Çokgen filtresinin bir bölümü olarak geçirilen bir sabit olarak belirtildiğinden işlev true döndürür.  

  Çokgen depolanan bir iki boyutlu yüzeydir noktaları dizisi olarak bir sınırlayıcı tanımlama halka (aşağıdaki örneğe bakın). Çokgen kapatılması, yani ilk gerekir ve son noktası kümeleri aynı olması gerekir. [Bir çokgenin noktaları saat yönünün tersi sırada olması gerekir](https://docs.microsoft.com/rest/api/searchservice/supported-data-types#Anchor_1).

  `geo.distance` Azure Search'te mesafeyi kilometre cinsinden uzaklığı döndürür. Bu, genellikle uzaklıkları ölçütleri dönüş OData Jeo-uzamsal işlemleri destekleyen diğer hizmetlerden farklıdır.  

  > [!NOTE]  
  >  GEO.distance filtrede kullanırken, sabit kullanarak işlevi tarafından döndürülen uzaklık karşılaştırmalıdır `lt`, `le`, `gt`, veya `ge`. İşleçler `eq` ve `ne` uzaklıkları karşılaştırılırken desteklenmez. Örneğin, bir doğru kullanımı geo.distance budur: `$filter=geo.distance(location, geography'POINT(-122.131577 47.678581)') le 5`.  

- `search.in` İşlevi verilen dize alanı verilen değerlerin listesini birine eşit olup olmadığını test eder. Bu ayrıca any veya all, tek bir dize koleksiyonu alanı değer verilen değerlerin listesini ile karşılaştırmak için kullanılabilir. Alan ve listedeki her değer arasındaki aynı şekilde olarak büyük küçük harfe duyarlı bir biçimde belirlenir `eq` işleci. Bu nedenle bir ifade ister `search.in(myfield, 'a, b, c')` eşdeğerdir `myfield eq 'a' or myfield eq 'b' or myfield eq 'c'`dışında `search.in` çok daha iyi performans verir. 

   İlk parametre olarak `search.in` işlev, dizeyi alan başvurusu (veya bir aralık değişkeni bir dize koleksiyonu alanı durumda üzerinden burada `search.in` içinde kullanılan bir `any` veya `all` ifadesi). 
  
   İkinci parametre değerleri, boşluk ve/veya virgülle ayrılmış listesini içeren bir dizedir. 
  
   Üçüncü parametresi, burada her karakter dizesinin veya bu dizenin alt ayırıcı olarak ikinci parametre değerleri listesi ayrıştırılırken değerlendirilir bir dizedir. Değerlerinizin bu karakterleri içerdiğinden boşluk ve virgül dışındaki ayırıcıları kullanmanız gerekiyorsa, isteğe bağlı üçüncü bir parametre belirtebilirsiniz `search.in`. 

  > [!NOTE]   
  > Bazı senaryolar, çok sayıda sabit değerler karşı bir alan karşılaştırma gerektirir. Örneğin, güvenlik kırpma filtrelerle uygulama Belge Kimliği alanı kimlikleri okuma erişimi izni isteyen kullanıcı için bir listesiyle karşılaştıran gerektirebilir. Bu gibi senaryolarda kullanarak öneririz `search.in` eşitlik ifadeleridir, daha karmaşık bir ayrım yerine işlevi. Örneğin, `search.in(Id, '123, 456, ...')` yerine `Id eq 123 or Id eq 456 or ....`. 
  >
  > Kullanırsanız `search.in`, ikinci parametre, yüzlerce veya binlerce değerlerinin listesini içerdiğinde saniyenin altındaki yanıt süresi bekleyebilirsiniz. Öğe sayısı için geçirebilirsiniz açık sınırsız olduğunu unutmayın `search.in`, ancak yine de en büyük istek boyutuyla sınırlıdır. Ancak, değerler sayısı arttıkça gecikme çıkarılır.

- `search.ismatch` İşlevi, arama sorgusu bir filtre ifadesi bir parçası olarak değerlendirir. Arama sorgusuyla eşleşen belgelerin sonuç kümesinde döndürülür. Bu işlevin aşağıdaki aşırı kullanılabilir:
  - `search.ismatch(search)`
  - `search.ismatch(search, searchFields)`
  - `search.ismatch(search, searchFields, queryType, searchMode)`

  burada: 
  
  - `search`: arama sorgusu (her ikisinde [basit](query-simple-syntax.md) veya [tam](query-lucene-syntax.md) sorgu söz dizimi). 
  - `queryType`: "Basit" veya "tam" varsayılan "Basit". Hangi sorgu dili kullanıldı belirtir `search` parametresi.
  - `searchFields`: tüm aranabilir alanları dizindeki Varsayılanları, içinde arama yapmak aranabilir alanların virgülle ayrılmış listesi.    
  - `searchMode`: "tümü" veya "tüm" varsayılan "herhangi bir". Arama terimleriyle tüm belgeyi bir eşleşme olarak saymak için eşlenmesi gereken olup olmadığını gösterir.

  Yukarıdaki tüm parametreleri karşılık gelen eşdeğerdir [istek parametrelerini arama](https://docs.microsoft.com/rest/api/searchservice/search-documents).

- `search.ismatchscoring` Gibi işlev `search.ismatch` işlev, parametre olarak geçen arama sorgusuyla eşleşen belgeler için true değerini döndürür. İlgi eşleşen belgelerin puan fark aralarında `search.ismatchscoring` sorgu durumunda sırada genel belge puanı katkıda `search.ismatch`, belge puanı değiştirilmez. Bu işlevin aşağıdaki aşırı gereksinimlerine aynı parametrelerle kullanılabilir `search.ismatch`:
  - `search.ismatchscoring(search)`
  - `search.ismatchscoring(search, searchFields)`
  - `search.ismatchscoring(search, searchFields, queryType, searchMode)`

  `search.ismatch` Ve `search.ismatchscoring` birbirleriyle ve filtre Cebir geri kalanı ile tam olarak dikgen işlevleri. Başka bir deyişle, her iki işlev aynı filtre ifadesinde kullanılabilir. 

### <a name="geospatial-queries-and-polygons-spanning-the-180th-meridian"></a>Jeo-uzamsal sorgular ve 180th meridyen kapsayan çokgenler  
 180th meridyen (yakın tarih çizgisi) içeren bir sorgu formulating kitaplıkları için birçok Jeo-uzamsal sorgu geçerli off-limits veya iki tane meridyen her iki tarafındaki içine Çokgen bölme gibi bir çözüm gerektirir.  

 Azure Search'te 180 derecelik boylam dahil Jeo-uzamsal sorguları sorgu şekildir dikdörtgen ve boylam ve enlem boyunca bir kılavuz düzeni, koordinatları Hizala beklendiği gibi çalıştığından (örneğin, `geo.intersects(location, geography'POLYGON((179 65,179 66,-179 66,-179 65,179 65))'`). Aksi takdirde, olmayan veya hizalanmamış şekiller için bölünmüş Çokgen yaklaşımı göz önünde bulundurun.  

<a name="bkmk_limits"></a>

## <a name="filter-size-limitations"></a>Filtre boyutu sınırlamaları 

 Büyüklüğü ve karmaşıklığı ile Azure Search'e göndermek için filtre ifadeleri limitleri vardır. Sınırları kabaca, filtre ifadesi yan tümcelerinde sayısını temel alır. Bir iyi yan tümceleri yüzlerce varsa, sınırı içinde çalışan risk altındadır udur. Sınırsız boyutunun filtreleri oluşturmaz şekilde uygulamanızı tasarlama öneririz.  


## <a name="filter-examples"></a>Filtre örnekleri  

 Bir temel ücrete ile tüm hotels $ veya 4 üzerindeki derecelendirilir 200'den daha az bulun:  

```
$filter=baseRate lt 200.0 and rating ge 4
```

 2010'dan itibaren renovated tüm hotels "Roach Motel" dışında bulun:  

```
$filter=hotelName ne 'Roach Motel' and lastRenovationDate ge 2010-01-01T00:00:00Z
```

 Bir temel ücrete $200'den daha az ile 2010, Pasifik Standart Saati için saat dilimi bilgilerini içeren bir datetime hazır değerinde beri renovated tüm hotels bulun:  

```
$filter=baseRate lt 200 and lastRenovationDate ge 2010-01-01T00:00:00-08:00
```

 Dahil edilen park ve İçilmez izin verme tüm hotels bulun:  

```
$filter=parkingIncluded and not smokingAllowed
```

 \- OR-  

```
$filter=parkingIncluded eq true and smokingAllowed eq false
```

 Lüks veya park içerir ve 5 derecesi sahip tüm hotels bulun:  

```
$filter=(category eq 'Luxury' or parkingIncluded eq true) and rating eq 5
```

 (Her otel Collection(Edm.String) alanında depolanan etiketleri sahip olduğu) tüm hotels "wifi" etiketiyle bulun:  

```
$filter=tags/any(t: t eq 'wifi')
```

 "Motel" etiketi olmayan tüm hotels bulun:  

```
$filter=tags/all(t: t ne 'motel')
```

 Tüm etiketlere sahip tüm hotels bulun:  

```
$filter=tags/any()
```

Etiket tüm hotels bulun:  

```
$filter=not tags/any()
```


 (Konum Edm.GeographyPoint türünde bir alan olduğu) belirli bir başvuru noktası 10 kilometre içindeki tüm hotels bulun:  

```
$filter=geo.distance(location, geography'POINT(-122.131577 47.678581)') le 10
```

 (Konum Edm.GeographyPoint türünde bir alan olduğu) tüm hotels Çokgen tanımlanan belirli bir görünüm penceresinin içinde bulun. Çokgen kapalı olduğunu unutmayın (ilk ve son noktası kümeleri aynı olmalıdır) ve [noktaları saat yönünün tersi düzende listelenmelidir](https://docs.microsoft.com/rest/api/searchservice/supported-data-types#Anchor_1).

```
$filter=geo.intersects(location, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))')
```

 Ya da "description" alanında değeri olmayan tüm Oteller bulmak veya değeri açıkça olduğunu kümesinde null:  

```
$filter=description eq null
```

Tüm hotels adlı 'Roach motel' veya 'Bütçe otel' eşittir bulabilirsiniz). İfadeler, varsayılan sınırlayıcı olduğu alanları içerir. Üçüncü bir dize parametresi olarak specicfy tek tırnak içinde başka bir sınırlayıcı yapabilecekleriniz:  

```
$filter=search.in(name, 'Roach motel,Budget hotel', ',')
```

Tüm hotels adlı eşit ya da Roach motel Bul ' veya 'ile ayrılmış bütçe otel' ' |'):  

```
$filter=search.in(name, 'Roach motel|Budget hotel', '|')
```

Etiket 'wifi' veya 'havuzu' tüm hotels bulun:  

```
$filter=tags/any(t: search.in(t, 'wifi, pool'))
```

Bir eşleşme ifadeleri 'ısıtılan towel raflar' veya 'dahil hairdryer' gibi bir koleksiyon içinde etiketleri bulun. 

```
$filter=tags/any(t: search.in(t, 'heated towel racks,hairdryer included', ','))
```

Etiket 'motel' veya 'kabini' olmadan tüm hotels bulun:  

```
$filter=tags/all(t: not search.in(t, 'motel, cabin'))
```

"Rıhtımının" sözcüğüyle belgeleri bulun. Bu filtre sorgusu aynıdır bir [arama isteği](https://docs.microsoft.com/rest/api/searchservice/search-documents) ile `search=waterfront`.

```
$filter=search.ismatchscoring('waterfront')
```

Sözcük "hostel" büyük veya buna eşit 4 ya da "motel" word belgeleriyle derecelendirme ve 5'e eşit derecelendirme belgeleri bulun. Unutmayın, bu isteği değil ifade edilemez olmadan `search.ismatchscoring` işlevi.

```
$filter=search.ismatchscoring('hostel') and rating ge 4 or search.ismatchscoring('motel') and rating eq 5
```

"Lüks" sözcüğü olmadan belgeleri bulun.

```
$filter=not search.ismatch('luxury') 
```

Okyanusu ifade "Görünüm" veya 5 değerlendirmesi eşit belgeleri bulun. `search.ismatchscoring` Sorgu yalnızca alanları karşı yürütülür `hotelName` ve `description`.
Not, yalnızca ikinci yan tümcesi ayrım, eşleşen belgelerin çok döndürülecek - derecelendirme hotels 5'e eşit. Bu belgeler Temizle sağlamak için herhangi bir ifade puanlanmış bölümlerini eşleşmedi, sıfıra eşit puanı döndürülür.

```
$filter=search.ismatchscoring('"ocean view"', 'description,hotelName') or rating eq 5
```

Koşulları "otel" ve "havaalanı" birbirinden 5 sözcük ise açıklaması içinde olduğu ve burada İçilmez izin belgeleri bulun. Bu sorgu kullanan [tam Lucene sorgu dili](query-lucene-syntax.md).

```
$filter=search.ismatch('"hotel airport"~5', 'description', 'full', 'any') and not smokingAllowed 
```

## <a name="order-by-syntax"></a>Sipariş tarafından söz dizimi

**$Orderby** parametreyi kabul eden, formun en fazla 32 ifadelerin virgülle ayrılmış bir liste `sort-criteria [asc|desc]`. Sıralama ölçütü adını olabilir bir `sortable` alan veya öğelerine yönelik çağrıdan `geo.distance` veya `search.score` işlevleri. Kullanabilirsiniz `asc` veya `desc` sıralama düzenini açıkça belirtmek için. Varsayılan artan.

Birden çok belge aynı sıralama ölçütü varsa ve `search.score` işlevi kullanılamıyor (örneğin, bir sayısal göre sıralarsanız `rating` tüm alan ve üç belgeler sahip bir derecelendirme 4), TIES bozulmuş belge puana göre azalan sırada. Belge puanlar (örneğin, istekte belirtilen tam metin araması sorgu olduğunda) aynı olduğunda, göreli sıralamasını bağlı belgelerin belirsiz ise.
 
Birden çok sıralama ölçütleri belirtebilirsiniz. Son sıralama ifadeleri sırasını belirler. Örneğin, derecelendirmesine göre ve ardından, puana göre azalan düzende sıralamak için söz dizimi olacaktır `$orderby=search.score() desc,rating desc`.

Sözdizimi `geo.distance` içinde **$orderby** olduğundan aynı olan **$filter**. Kullanırken `geo.distance` içinde **$orderby**, uygulandığı alanın türü olmalıdır `Edm.GeographyPoint` ve ayrıca olmalıdır `sortable`.  

Sözdizimi `search.score` içinde **$orderby** olduğu `search.score()`. İşlev `search.score` herhangi bir parametre almaz.  
 

## <a name="order-by-examples"></a>Sipariş tarafından örnekleri

Taban fiyat göre artan sıralama hotels:

```
$orderby=baseRate asc
```

Otel, derecelendirme, daha sonra temel ücrete göre artan azalan sıralama (artan varsayılan olduğunu unutmayın):

```
$orderby=rating desc,baseRate
```

Hotels derecelendirme, ardından belirli ortak ordinates uzaklık artan azalan düzende sıralanır:

```
$orderby=rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc
```

Hotels search.score ve Derecelendirmeye göre azalan düzende ve ardından aynı dereceye sahip iki hotels arasında en yakındakine ilk sırada olacak şekilde artan sırada uzaklık tarafından verilen koordinatlarından Sırala:

```
$orderby=search.score() desc,rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc
```
<a name="bkmk_unsupported"></a>

## <a name="unsupported-odata-syntax"></a>Desteklenmeyen OData söz dizimi

-   Aritmetik ifadeler  

-   İşlevler (dışındaki uzaklık ve Jeo-uzamsal işlevler kesişip)  

-   `any/all` rastgele lambda ifadeleri  

## <a name="see-also"></a>Ayrıca bkz.  

+ [Azure Arama'da çok yönlü navigasyon](search-faceted-navigation.md) 
+ [Azure Search'te filtreler](search-filters.md) 
+ [Search belgeleri &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) 
+ [Lucene sorgu söz dizimi](query-lucene-syntax.md)
+ [Azure Search'te Basit Sorgu söz dizimi](query-simple-syntax.md)   
