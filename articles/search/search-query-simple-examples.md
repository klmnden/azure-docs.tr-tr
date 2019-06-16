---
title: "\"Basit\" arama söz dizimi - Azure Search kullanarak sorgu örnekleri"
description: Tam metin araması, filtre arama, coğrafi arama, çok yönlü arama ve bir Azure Search dizinini sorgulama için kullanılan diğer sorgu dizeleri için basit bir sorgu örnekleri.
author: HeidiSteen
manager: cgronlun
tags: Simple query analyzer syntax
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 0c47212e51725e7d4a173c441709dca739d4e357
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65024542"
---
# <a name="query-examples-using-the-simple-search-syntax-in-azure-search"></a>"Basit" arama söz dizimi kullanarak Azure Search'te sorgu örnekleri

[Basit Sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) Azure Search dizini tam metin arama sorguları yürütmek varsayılan sorgu ayrıştırıcı çağırır. Basit Sorgu Çözümleyicisi, hızlı ve Azure Search, tam metin araması, filtrelenmiş ve çok yönlü arama ve coğrafi arama gibi yaygın senaryoları ele alır. Bu makalede, basit söz dizimi kullanırken kullanılabilir sorgu işlemleri gösteren örnekler adım.

Diğer sorgu söz dizimi [tam Lucene](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), daha karmaşık sorgu yapıları gibi destekleyici belirsiz ve joker karakter araması, işlemek için ek zaman alabilir. Daha fazla bilgi ve tam söz dizimi gösteren örnekler için bkz. [Lucene sözdizimi sorgu örnekleri](search-query-lucene-examples.md).

## <a name="formulate-requests-in-postman"></a>Postman'deki istekleri düzenleme

Aşağıdaki örnekler tarafından sağlanan bir veri kümesini temel alan işleri kullanılabilir oluşan NYC işleri arama dizini yararlanarak [New York City OpenData](https://nycopendata.socrata.com/) girişim. Bu veriler, geçerli ya da tam düşünülmemelidir. Diğer bir deyişle, bir Azure aboneliği veya Azure Search'ün bu sorguları deneyin gerekmez Microsoft tarafından sağlanan bir korumalı alan hizmeti dizinidir.

Ne gerekiyor, Postman veya HTTP GET isteği verme eşdeğer bir aracı değil. Daha fazla bilgi için [REST istemcileri ile Araştır](search-fiddler.md).

### <a name="set-the-request-header"></a>İstek üst bilgisini ayarlayın

1. İstek üstbilgisinde ayarlanan **Content-Type** için `application/json`.

2. Ekleme bir **api anahtarını**ve bu dize olarak ayarlayın: `252044BE3886FE4A8E3BAA4F595114BB`. NYC işleri dizini barındırma korumalı alan arama hizmeti için bir sorgu anahtarı budur.

İstek üstbilgisi belirttikten sonra onu tüm sorgular bu makalede, yalnızca takas tekrar kullanabilirsiniz **arama =** dize. 

  ![Postman isteği üst bilgisi](media/search-query-lucene-examples/postman-header.png)

### <a name="set-the-request-url"></a>İstek URL'sini ayarlayın

Azure arama uç noktası ve arama dizesini içeren URL'yi ile eşleştirilmiş bir GET komutu isteğidir.

  ![Postman isteği üst bilgisi](media/search-query-lucene-examples/postman-basic-url-request-elements.png)

URL'si birleşimi aşağıdaki öğeleri içerir:

+ **`https://azs-playground.search.windows.net/`** bir korumalı alan arama hizmeti, Azure Search geliştirme ekibi tarafından korunur. 
+ **`indexes/nycjobs/`** Bu hizmetin dizinlerini koleksiyonunda NYC işleri dizinidir. İstekte hizmet adını ve dizin gereklidir.
+ **`docs`** documents koleksiyonunu içeren tüm aranabilir içeriği. İstek üst bilgisinde sağlanan sorgunuzun api anahtarını, yalnızca documents koleksiyonunu hedefleyen okuma işlemleri üzerinde çalışır.
+ **`api-version=2019-05-06`** her istekte gerekli bir parametre olan api-version, ayarlar.
+ **`search=*`** ilk sorgu null sorgu dizesi, ilk 50 sonuçları döndüren (varsayılan).

## <a name="send-your-first-query"></a>İlk sorgunuzu Gönder

Bir doğrulama adımı aşağıdaki isteği GET yapıştırın ve tıklayın **Gönder**. Sonuçları ayrıntılı JSON belgeleri olarak döndürülür. Tüm alanları ve tüm değerleri görmenize olanak sağlayan tüm belgeler döndürülür.

Bu URL'yi bir doğrulama adımı olarak ve belge yapısı görüntülemek için bir REST istemcisi yapıştırın.

  ```http
  https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&search=*
  ```

Sorgu dizesi **`search=*`** , belirtilmeyen bir arama null veya boş aramaya eşdeğerdir. Özellikle kullanışlı değildir, ancak yapabileceğiniz basit arama olur.

İsteğe bağlı olarak ekleyebileceğiniz **`$count=true`** arama ölçütleriyle eşleşen belgelerin sayısını döndürmek için URL. Üzerinde bir boş bir arama dizesi (yaklaşık 2800 NYC işleri söz konusu olduğunda) dizindeki tüm belgelerin budur.

## <a name="how-to-invoke-simple-query-parsing"></a>Basit Sorgu ayrıştırma çağırmak nasıl

Etkileşimli sorgular için herhangi bir şey belirtmeniz gerekmez: basit bir varsayılandır. Kodda, daha önce çağırdıysanız **queryType = full** tam sorgu söz dizimi için varsayılan sıfırlayabiliyor **queryType = basit**.

## <a name="example-1-field-scoped-query"></a>Örnek 1: Alan kapsamlı sorgu

Bu ilk örneği, ayrıştırıcı özgü değildir, ancak ilk sorgu temel kavramı tanıtmak için Biz bu ile neden: kapsama. Bu örnek, sorgu yürütme ve yalnızca birkaç belirli alanları yanıta kapsamlar. Postman veya arama Gezgini araç olduğunda, okunabilir bir JSON yanıtı nasıl haberdar olmak önemlidir. 

Konuyu uzatmamak amacıyla, sorgu hedefleyen yalnızca *business_title* alan ve yalnızca iş başlıkları döndürülür belirtir. Söz dizimi **searchFields** business_title alan yalnızca, sorgu yürütme kısıtlamak için ve **seçin** yanıt olarak hangi alanların ekleneceğini belirlemek için.

### <a name="partial-query-string"></a>Kısmi bir sorgu dizesi

```http
searchFields=business_title&$select=business_title&search=*
```

Virgülle ayrılmış bir liste içinde birden çok alan ile aynı sorgu aşağıdadır.

```http
search=*&searchFields=business_title, posting_type&$select=business_title, posting_type
```

### <a name="full-url"></a>Tam URL

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&searchFields=business_title&$select=business_title&search=*
```

Bu sorgu için yanıt, aşağıdaki ekran görüntüsüne benzer görünmelidir.

  ![Postman örnek yanıt](media/search-query-lucene-examples/postman-sample-results.png)

Yanıt arama puanı fark etmiş olabilirsiniz. 1 Tekdüzen puanları olduğunda hiçbir sıralama veya arama değil, tam metin araması olduğundan veya hiçbir ölçüt uygulandığı nedeniyle oluşur. Hiçbir ölçüt null arama için satırlar rastgele sırayla geri dönün. Gerçek bir ölçüt eklediğinizde, arama puanları anlamlı değerlere evrim Geçiren görürsünüz.

## <a name="example-2-look-up-by-id"></a>Örnek 2: Kimliğe göre arayın

Bu örnek bir bit alışılmadık olduğu, ancak arama davranışlarını değerlendirirken, neden, yer veya Eşleşmeyenler sonuçlara dahil anlamak için belirli bir belge tüm içeriğini incelemek isteyebilirsiniz. Tek bir belge sunabilen döndürmek için bir [arama işlemi](https://docs.microsoft.com/rest/api/searchservice/lookup-document) belge kimliği geçirmek için

Tüm belgeleri, benzersiz bir tanımlayıcıya sahip. Bir arama sorgusu söz diziminin denemek için kullanmak üzere bulabilmek belge kimlikleri listesini başta döndürür. NYC işleri için tanımlayıcılar içinde depolanan `id` alan.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&searchFields=id&$select=id&search=*
```

Sonraki örnek bir arama sorgusu dayalı belirli bir belge döndürme olan `id` önceki yanıtta ilk göründüğü "9E1E3AF9-0660-4E00-AF51-9B654925A2D5". Aşağıdaki sorgu, yalnızca seçili alanları, tüm belgeyi döndürür. 

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs/9E1E3AF9-0660-4E00-AF51-9B654925A2D5?api-version=2019-05-06&$count=true&search=*
```

## <a name="example-3-filter-queries"></a>Örnek 3: Sorguları filtreleme

[Filtre söz dizimi](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search#filter-examples) ile kullanabileceğiniz bir OData ifade **arama** veya tek başına. Filtre ifadesi ilgi belgeleri tam olarak nitelemek mümkün olduğunda, bir arama parametresi olmadan bir tek başına filtre yararlıdır. Bir sorgu dizesi hiçbir sözlü ya da dilbilimsel analiz yoktur yok (1 olan tüm puanları) Puanlama ve hiçbir sıralaması. Arama dizesi boş olduğuna dikkat edin.

```http
POST /indexes/nycjobs/docs/search?api-version=2019-05-06
    {
      "search": "",
      "filter": "salary_frequency eq 'Annual' and salary_range_from gt 90000",
      "select": "select=job_id, business_title, agency, salary_range_from",
      "count": "true"
    }
```

Birlikte kullanıldığında, filtre öncelikle tüm dizine uygulanır ve ardından arama filtre sonuçlarına gerçekleştirilir. Filtreler arama sorgusunun işlemesi gereken belge kümesini azalttığından, sorgu performansını iyileştirmeye yönelik kullanışlı bir teknik olabilir.

  ![Filtre sorgusu yanıtı](media/search-query-simple-examples/filtered-query.png)

Bu GET kullanarak Postman'da denemek istiyorsanız, bu dizesinde yapıştırabilirsiniz:

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&$select=job_id,business_title,agency,salary_range_from&search=&$filter=salary_frequency eq 'Annual' and salary_range_from gt 90000
```

Filtre ve arama birleştirmek için başka bir güçlü yollarından biri sayesinde **`search.ismatch*()`** , burada kullanabileceğiniz bir arama sorgusu filtredeki bir filtre ifadesi. Bu filtre ifadesi bir joker karakter kullanan *planı* business_title terimi planı, planner, planlama ve diğerleri dahil olmak üzere seçin.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&$select=job_id,business_title,agency&search=&$filter=search.ismatch('plan*', 'business_title', 'full', 'any')
```

İşlevi hakkında daha fazla bilgi için bkz. ["Filtre örneklerde" search.ismatch](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search#filter-examples).

## <a name="example-4-range-filters"></a>Örnek 4: Aralık filtresi

Aralık filtresi aracılığıyla desteklenir **`$filter`** ifadeleri herhangi bir veri türü için. Aşağıdaki örnekler, sayısal ve dize alanları üzerinde arayın. 

Veri türleri aralık filtreleri önemlidir ve sayısal veri sayısal alanları ve dize verilerini dize alanları olduğunda en iyi şekilde çalışır. Sayısal dizeler Azure Search'te karşılaştırılabilir olmadığı için dize alanları sayısal veri aralıkları için uygun değil. 

Aşağıdaki örnekler (sayısal aralık metin aralığı tarafından izlenen) okunabilirlik için POST biçiminde şunlardır:

```http
POST /indexes/nycjobs/docs/search?api-version=2019-05-06
    {
      "search": "",
      "filter": "num_of_positions ge 5 and num_of_positions lt 10",
      "select": "job_id, business_title, num_of_positions, agency",
      "orderby": "agency",
      "count": "true"
    }
```
  ![Sayısal aralık için Aralık filtresi](media/search-query-simple-examples/rangefilternumeric.png)


```http
POST /indexes/nycjobs/docs/search?api-version=2019-05-06
    {
      "search": "",
      "filter": "business_title ge 'A*' and business_title lt 'C*'",
      "select": "job_id, business_title, agency",
      "orderby": "business_title",
      "count": "true"
    }
```

  ![Aralık filtresi için metin aralığı](media/search-query-simple-examples/rangefiltertext.png)

Ayrıca bu Postman kullanarak GET deneyebileceğiniz:

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&search=&$filter=num_of_positions ge 5 and num_of_positions lt 10&$select=job_id, business_title, num_of_positions, agency&$orderby=agency&$count=true
```

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&search=&$filter=business_title ge 'A*' and business_title lt 'C*'&$select=job_id, business_title, agency&$orderby=business_title&$count=true
```

> [!NOTE]
> Modelleme değerleri aralığı içinde bir genel arama uygulama gereksinimdir. Daha fazla bilgi ve model Gezinti yapılar için filtre oluşturmaya ilişkin örnekler için bkz. ["Filtre tabanlı bir aralıkta" içinde *çok yönlü navigasyon uygulamak nasıl*](search-faceted-navigation.md#filter-based-on-a-range).

## <a name="example-5-geo-search"></a>Örnek 5: Coğrafi arama

Örnek dizini enlem ve boylam koordinatları geo_location alanıyla içerir. Bu örnekte [geo.distance işlevi](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search#filter-examples) filtre uygulayan bir başlangıç noktası çevresi içindeki belgeler üzerinde out sağlayan bir rastgele uzaklık (olan mesafeyi kilometre cinsinden). Son değerini azaltın veya sorguyu'nın yüzey alanını genişletmek için sorgu (4) ayarlayabilirsiniz.

Aşağıdaki örnek, okunabilirlik için POST biçiminde aynıdır:

```http
POST /indexes/nycjobs/docs/search?api-version=2019-05-06
    {
      "search": "",
      "filter": "geo.distance(geo_location, geography'POINT(-74.11734 40.634384)') le 4",
      "select": "job_id, business_title, work_location",
      "count": "true"
    }
```
Daha okunabilir sonuçlar için iş kimliği, iş unvanı ve iş konumunuz dahil etmek için arama sonuçları atılır. Başlangıç koordinatları dizininde (Bu durumda, Staten Adası iş konumuna. rastgele bir belgeden edinilen

Ayrıca bu Postman kullanarak GET deneyebileceğiniz:

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&search=&$select=job_id, business_title, work_location&$filter=geo.distance(geo_location, geography'POINT(-74.11734 40.634384)') le 4
```

## <a name="example-6-search-precision"></a>Örnek 6: Arama duyarlık

Terim, bağımsız olarak değerlendirilen tek terimleri, bunları, muhtemelen çoğunu sorgulardır. Tümcecik sorguları tırnak işareti içine alınmış ve bir verbatim dizesi değerlendirilir. Duyarlık eşleşmenin işleçler ve searchMode tarafından denetlenir.

Örnek 1: **`&search=fire`** burada tüm sözcük yangın belgedeki yere eşleşmeleri 150 sonuçları döndürür.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&search=fire
```

Örnek 2: **`&search=fire department`** 2002 sonuçlarını döndürür. Eşleşme yangın veya bölüm içeren belgeleri döndürülür.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&search=fire department
```

Örnek 3: **`&search="fire department"`** 82 sonuçlarını döndürür. Her iki terim verbatim bir arama dizesini tırnak işaretleri içine kapsayan olduğu ve eşleşme parçalanmış koşulları birleşik koşullarını oluşan dizininde bulunur. Bu gibi bir arama neden açıklıyor **`search=+fire +department`** eşdeğer değildir. Her iki terim gerekiyor, ancak için bağımsız olarak taranır. 

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&search="fire department"
```

## <a name="example-7-booleans-with-searchmode"></a>Örnek 7: SearchMode ile Boole değerleri

Basit söz dizimi karakter biçiminde Boole işleçleri destekler (`+, -, |`). İle kesinlik ve geri çağırırsanız, bir denge searchMode parametresi bildiren `searchMode=any` geri çağırma öncelik tanıdığından (herhangi bir ölçüte uyan niteleyen sonuç kümesi için bir belge), ve `searchMode=all` öncelik tanıdığından duyarlık (tüm ölçütleri eşleştirilmelidir). Varsayılan `searchMode=any`, hangi kullanılabilir birden çok işleç bir sorgu yığın, kafa karıştırıcı ve daha dar sonuçları yerine daha geniş alınıyor. Burada sonuçlarında "içeren değil" tüm belgeleri NOT ile özellikle, böyle bir terim.

(Any) varsayılan searchMode kullanarak 2800 belgeler döndürülür: birden çok parça içeren "Metrotech Center" terimi olmayan tüm belgelerin yanı sıra "fire departmanı" terimi.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&searchMode=any&search="fire department"  -"Metrotech Center"
```

  ![modu her arama](media/search-query-simple-examples/searchmodeany.png)

Değiştirme için searchMode `all` bir toplu etkisi ölçütlere zorunlu kılan ve tüm "fire departman", bu işleri Metrotech merkezi adresten eksi ifadesini içeren belgeleri oluşan daha küçük sonuç kümesi - 21 belgeleri - döndürür.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&searchMode=all&search="fire department"  -"Metrotech Center"
```
  ![Tüm arama modu](media/search-query-simple-examples/searchmodeall.png)

## <a name="example-8-structuring-results"></a>8\. örnek: Yapılandırma sonuçları

Her batch ve sıralama düzeni döndürülen belgelerin sayısını alanlar aramaya olan birkaç parametre denetimi sonuçlanır. Bu örnek, önceki örneklerde, birkaçını sonuçları belirli alanlara kullanarak sınırlama resurfaces **$select** deyimi ve 82 eşleşme dönmeden verbatim arama ölçütü 

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"
```
Önceki örnekte eklenen, başlığa göre sıralayabilirsiniz. Bu sıralama civil_service_title olduğundan çalışır *sıralanabilir* dizinde.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"&$orderby=civil_service_title
```

Disk belleği sonuçları kullanılarak gerçekleştirilir **$top** parametresi, bu durumda ilk 5 belgeleri döndüren:

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"&$orderby=civil_service_title&$top=5&$skip=0
```

Sonraki 5 almak için ilk batch atla:

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"&$orderby=civil_service_title&$top=5&$skip=5
```

## <a name="next-steps"></a>Sonraki adımlar
Kodunuzda sorguları belirtmeyi deneyin. Aşağıdaki bağlantıları arama sorguları hem .NET hem de varsayılan basit söz dizimi kullanarak REST API için nasıl yapılacağını açıklar.

* [Azure Search .NET SDK kullanarak dizininizi sorgulama](search-query-dotnet.md)
* [Azure Search REST API kullanarak dizininizi sorgulama](search-create-index-rest-api.md)

Ek söz dizimi başvurusu, sorgu mimarisi ve örnekleri aşağıdaki bağlantılarda bulunabilir:

+ [Lucene sözdizimi sorgu örnekleri Gelişmiş sorgular oluşturmak için](search-query-lucene-examples.md)
+ [Metin arama Azure Search'te tam nasıl çalışır](search-lucene-query-architecture.md)
+ [Basit sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)
+ [Tam Lucene sorgu](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)
+ [Filtre ve Orderby söz dizimi](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search)
