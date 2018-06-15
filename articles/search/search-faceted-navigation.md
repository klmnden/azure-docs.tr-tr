---
title: Azure Search'te modellenmiş bir gezinmede gerçekleştirme | Microsoft Docs
description: Azure Search, Microsoft Azure üzerinde barındırılan bulut arama hizmeti ile tümleştirmek uygulamalara modellenmiş bir gezinmede ekleyin.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 3/10/2017
ms.author: heidist
ms.openlocfilehash: e00e875619e4ed6800f5739362ff0c52971f6f16
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32195303"
---
# <a name="how-to-implement-faceted-navigation-in-azure-search"></a>Azure Arama'da çok yönlü navigasyon
Modellenmiş bir gezinmede arama uygulamaları kendi kendine yönlendirilmiş ayrıntıya gitme gezinti sağlayan bir filtreleme mekanizması ' dir. 'Modellenmiş bir gezinmede' terimi bilinmiyor olabilir, ancak büyük olasılıkla daha önce kullanılmış. Aşağıdaki örnekte gösterildiği gibi çok yönlü gezinmeyi sonuçlara filtre uygulamak için kullanılan kategorileri'den fazla doğrudur.

 ![Azure arama iş Portal Tanıtımı][1]

Modellenmiş bir gezinmede aramak için bir alternatif giriş noktasıdır. Karmaşık arama ifadeleri el ile yazmak için uygun bir alternatif sunar. Modelleri için sıfır sonuçları almadım sağlarken aradığınızı bulmanıza yardımcı olabilir. Bir geliştirici olarak modelleri, arama gövde gezinme en yararlı arama ölçütlerini kullanıma olanak tanır. Çevrimiçi perakende uygulamalarda modellenmiş bir gezinmede genellikle markalar, Departmanlar (çocuk yerine), boyutu, fiyat, popülerliği ve derecelendirmeleri üzerinde oluşturulmuştur. 

Modellenmiş bir gezinmede uygulama arama teknolojileri arasında farklılık gösterir. Azure Search'te modellenmiş bir gezinmede sorgu zamanında, şemada daha önce öznitelikli alanları kullanılarak oluşturulur.

-   Bir sorgu göndermelidir uygulamanızı derlemeler sorgularda *modeli sorgu parametreleri* kullanılabilir modeli Bu belge sonuç kümesi için filtre değerlerini almak için.

-   Gerçekte belge sonuç kırpmaya ayarlamak, uygulama de uygulamanız gerekir bir `$filter` ifade.

Uygulama geliştirme toplu iş sorguları yapıları kod yazma meydana gelir. Birçok modellenmiş Gezinti bölmesinden beklediğiniz uygulama davranışları aralıkları tanımlama ve sayıları için model sonuçları alma için yerleşik destek dahil olmak üzere bu hizmeti tarafından sağlanır. Hizmet ayrıca yardımcı duyarlı Varsayılanları içerir yönetilmeleri Gezinti yapıları kaçının. 

## <a name="sample-code-and-demo"></a>Örnek kod ve tanıtım
Bu makalede örnek olarak bir iş arama portal kullanır. Örneğin, bir ASP.NET MVC uygulaması uygulanır.

-   Bakın ve çalışma demo adresindeki çevrimiçi test [Azure arama proje portalı Demo](http://azjobsdemo.azurewebsites.net/).

-   Koddan karşıdan [Azure-Samples bağlantıların github'da](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs).

## <a name="get-started"></a>başlarken
Geliştirme arama yeniyseniz, çok yönlü gezinmeyi düşünülecek iyi Self yönlendirilmiş arama olasılıklarını gösterir yoludur. Önceden tanımlanmış filtreleri, hızlı bir şekilde arama sonuçları noktası tıklatın eylemler arasında aşağı daraltmak için kullanılan temel alarak ayrıntıya arama deneyimi türüdür. 

### <a name="interaction-model"></a>Etkileşim modeli

Arama deneyimi modellenmiş gezinmesine sağlandığından kullanıcı eylemlerine yanıt unfold sorguları bir dizi olarak anlayarak Başlat yinelemelidir.

Genellikle periphery yerleştirilen çok yönlü gezinmeyi sağlayan uygulama sayfası başlangıç noktasıdır. Modellenmiş Gezinti ağaç yapısı, her değeri veya tıklanabilir bir metin için onay kutularını ile görülür. 

1. Azure arama için gönderilen bir sorgu modellenmiş gezinti yapısında bir veya daha fazla model sorgu parametreleri aracılığıyla belirtir. Örneğin, sorgu içerebilir `facet=Rating`, belki de ile bir `:values` veya `:sort` daha fazla sunu iyileştirmek için seçeneği.
2. Sunu katmanı istekte belirtilen modellerle kullanarak çok yönlü gezinmeyi sağlayan bir arama sayfasını işler.
3. Derecelendirme içeren modellenmiş gezinti yapısı verildiğinde, "4"'i yalnızca 4 veya daha yüksek bir derecelendirme ürünleriyle gösterilen olduğunu belirtmek için. 
4. Yanıt olarak, uygulamayı içeren bir sorgu gönderir. `$filter=Rating ge 4` 
5. Sunu katmanı yeni ölçütlerini karşılayan yalnızca bu öğeleri içeren bir azaltılmış sonuç kümesi gösteren sayfasında, güncelleştirmeleri (Bu durumda, ürün 4 derecelendirilmiş ve üstü).

Bir model için sorgu parametresi olsa da, sorgu girişi ile karıştırmayın. Sorguda seçim ölçütü olarak asla kullanılmaz. Bunun yerine, model Sorgu parametrelerinin yanıtta gelir gezinti yapısında girdi olarak düşünün. Sağladığınız her model için sorgu parametresi, Azure Search, her model değeri için kısmi sonuçlarında kaç belgelerdir değerlendirir.

Bildirim `$filter` 4. adımda. Filtre modellenmiş bir gezinmede önemli bir yönüdür. Modelleri ve filtreleri API bağımsız olmakla birlikte, düşündüğünüz deneyimi sunmak için her ikisini de gerekir. 

### <a name="app-design-pattern"></a>Uygulama tasarım deseni

Uygulama kodunda düzeni modeli sorgu parametreleri modeli sonuçları yanı sıra, bir $filter ifadesi birlikte modellenmiş bir gezinmede yapısı döndürülecek kullanmaktır.  Filtre ifadesi model değeri click olayını işler. Düşünün `$filter` arama sonuçlarının ifade gerçek kırpma arka plan kod olarak sunu katmanı döndürdü. Renkleri modeli verildiğinde, kırmızı renk tıklatarak aracılığıyla uygulanır bir `$filter` kırmızı renk olan öğeleri seçer ifade. 

### <a name="query-basics"></a>Sorgu temelleri

Azure Search'te bir isteği aracılığıyla bir veya daha fazla sorgu parametreleri belirtilir (bkz [Search belgeleri](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) her biri bir açıklaması için). Sorgu parametreleri hiçbiri gerekli değildir, ancak en az bir geçerli olması bir sorgu için sırasıyla olması gerekir.

Duyarlık ilgisiz isabet filtreleme yeteneği gerçekleştirilir gibi birine veya ikisine de bu ifadeleri anladım:

-   **Arama =**  
    Bu parametrenin değeri Arama ifadesi meydana gelir. Tek bir metin veya birden çok hüküm ve işleçleri içeren karmaşık arama ifadesi olması olabilir. Sunucuda, bir arama ifadesi tam metin arama, sıralama sırayla sonuçları döndüren koşullarını eşleşen dizini aranabilir alanlara sorgulamak için kullanılır. Ayarlarsanız `search` null, sorgu yürütme tüm dizinde olduğundan (diğer bir deyişle, `search=*`). Bu durumda, sorgu diğer öğeleri gibi bir `$filter` veya profili, Puanlama etken hangi belgeleri döndürülen etkileyen birincil `($filter`) ve hangi sırayla (`scoringProfile` veya `$orderby`).

-   **$filter =**  
    Bir filtre, belirli bir belge öznitelik değerlerine göre arama sonuçları boyutunu sınırlamak için güçlü bir mekanizmadır. A `$filter` ilk olarak değerlendirilmesi, kullanılabilir değerler ve her bir değer için karşılık gelen sayıları oluşturur olduğunu mantığı ve ardından

Karmaşık arama ifadeleri sorgu performansını azaltın. Mümkün olduğunda iyi oluşturulmuş filtre ifadeleri duyarlık artırmak ve sorgu performansını artırmak için kullanın.

Nasıl daha hassas bir filtre ekler daha iyi anlamak için bir filtre ifadesi içeren bir karmaşık arama ifadesi karşılaştırın:

-   `GET /indexes/hotel/docs?search=lodging budget +Seattle –motel +parking`
-   `GET /indexes/hotel/docs?search=lodging&$filter=City eq ‘Seattle’ and Parking and Type ne ‘motel’`

Her iki sorguları geçerlidir, ancak motels olmayan Seattle'da park ile arıyorsanız ikinci üstündedir.
-   İlk sorguyu belirtilen veya dize alanları adını, açıklamasını ve aranabilir verileri içeren herhangi bir alan gibi geçmeyen belirli sözcükleri kullanır.
-   İkinci sorguyu kesin eşleşmeler yapılandırılmış verileri arar ve çok daha doğru olması olasıdır.

Modellenmiş bir gezinmede içeren uygulamalarda, arama sonuçlarını daraltarak modellenmiş bir gezinmede yapısı üzerinden her bir kullanıcı eylemi eşlik emin olun. Sonuçları daraltmak için bir filtre ifadesi kullanın.

<a name="howtobuildit"></a>

## <a name="build-a-faceted-navigation-app"></a>Modellenmiş bir gezinmede uygulaması oluşturma
Azure Search modellenmiş bir gezinmede arama isteği oluşturur, uygulama kodunuzda uygulayın. Önceden tanımlanmış şemanızı öğelerinde modellenmiş bir gezinmede kullanır.

Önceden tanımlanmış aramanızda dizinidir `Facetable [true|false]` dizin özniteliği, etkinleştirme veya devre dışı kullanımlarını modellenmiş gezinti yapısında seçili alanları ayarlayın. Olmadan `"Facetable" = true`, model Gezinti bölmesinde bir alan kullanılamaz.

Sunu katmanı kodunuzda kullanıcı deneyimi sağlar. Label ve değerleri, onay kutularını ve sayısı gibi modellenmiş bir gezinmede bağlı bölümlerini listelenmelidir. Azure Search REST API'sini platform belirsiz, hangi dil ve platformu kullanın. Güncelleştirilmiş kullanıcı Arabirimi durumu ile artımlı yenileme her ek modeli seçtiğinizde destekleyen kullanıcı Arabirimi öğeleri içerecek şekilde önemli şeydir bakın. 

Sorgu zamanında uygulama kodunuz içeren bir istek oluşturur `facet=[string]`, modeli tarafından alana sağlayan bir istek parametresi. Sorgu birden fazla modelleri gibi olabilir `&facet=color&facet=category&facet=rating`, her biri ayrılmış bir ampersan (&) karakteri tarafından.

Uygulama kodu gerekir de oluşturmak bir `$filter` modellenmiş bir gezinmede tıklatın olayları işlemek için ifade. A `$filter` filtre ölçütü olarak model değeri kullanarak arama sonuçlarını azaltır.

Azure arama güncelleştirmeleri modellenmiş bir gezinmede yapısına yönelik işaretçinin birlikte girin bir veya daha fazla şartları temel alınarak arama sonuçlarını döndürür. Azure Search'te modellenmiş bir gezinmede modeli değerleri içeren bir tek düzeyli yapım ve her biri için kaç tane sonuçlarını bulunan sayar.

Aşağıdaki bölümlerde, her bölüm oluşturmak nasıl yakından alın.

<a name="buildindex"></a>

## <a name="build-the-index"></a>Dizini derleme
Bu dizin özniteliği aracılığıyla dizinindeki alan alanını temelinde etkin olduğunu: `"Facetable": true`.  
Büyük olasılıkla modellenmiş bir gezinmede kullanılabilir tüm alan türleri `Facetable` varsayılan olarak. Bu tür alan türleri içerir `Edm.String`, `Edm.DateTimeOffset`ve tüm sayısal alan türleri (esas olarak, tüm alan türleri modellenebilir dışında olan `Edm.GeographyPoint`, hangi kullanılamaz modellenmiş bir gezinmede). 

Dizin oluştururken, en iyi modellenmiş gezinmesine açıkça olduğunu hiçbir zaman bir model kullanılması gereken alanlar için devre dışı bırakmak için uygulamadır.  Dize alanları bir kimliği veya ürün adı gibi singleton değerleri için özel olarak, ayarlanmalı `"Facetable": false` modellenmiş bir gezinmede'nde yanlışlıkla (ve etkisiz) kullanımları engellemek için. Burada gerekmeyen kapalı olduğunu kapatma dizin boyutunu küçük tutmaya yardımcı olur ve genellikle performansı artırır.

Aşağıdaki şema boyutunu azaltmak için bazı özniteliklerini kırpılmış proje portalı Demo örnek uygulama için bir parçasıdır:

```json
{
  ...
  "name": "nycjobs",
  "fields": [
    { “name”: "id",                 "type": "Edm.String",              "searchable": false, "filterable": false, ... "facetable": false, ... },
    { “name”: "job_id",             "type": "Edm.String",              "searchable": false, "filterable": false, ... "facetable": false, ... },
    { “name”: "agency",              "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "posting_type",        "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "num_of_positions",    "type": "Edm.Int32",              "searchable": false, "filterable": true, ...  "facetable": true, ...  },
    { “name”: "business_title",      "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "civil_service_title", "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "title_code_no",       "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "level",               "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "salary_range_from",   "type": "Edm.Int32",              "searchable": false, "filterable": true, ...  "facetable": true, ...  },
    { “name”: "salary_range_to",     "type": "Edm.Int32",              "searchable": false, "filterable": true, ...  "facetable": true, ...  },
    { “name”: "salary_frequency",    "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "work_location",       "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
…
    { “name”: "geo_location",        "type": "Edm.GeographyPoint",     "searchable": false, "filterable": true, ...  "facetable": false, ... },
    { “name”: "tags",                "type": "Collection(Edm.String)", "searchable": true,  "filterable": true, ...  "facetable": true, ...  }
  ],
…
}
```

Örnek şemada gördüğünüz `Facetable` kimliği değerleri gibi modellerini olarak kullanılmaması dize alanları için devre dışı. Burada gerekmeyen kapalı olduğunu kapatma dizin boyutunu küçük tutmaya yardımcı olur ve genellikle performansı artırır.

> [!TIP]
> En iyi uygulama, her bir alan için dizin özniteliklerini tam kümesi içerir. Ancak `Facetable` kasıtlı olarak ayarlama, neredeyse tüm alanlar için varsayılan olarak her özniteliği, her bir şema kararı etkileri düşündüğünüz yardımcı olabilir açıktır. 

<a name="checkdata"></a>

## <a name="check-the-data"></a>Onay verileri
Veri Kalitesi beklediğiniz gibi olup modellenmiş bir gezinmede yapısı gerçeğe üzerinde doğrudan etkisi vardır. Ayrıca, sonuç kümesi azaltmak için filtreleri oluşturma kolaylığı etkiler.

Modeli marka veya fiyat tarafından istiyorsanız, her belge için değerler içermelidir *BrandName* ve *ProductPrice* geçerli, tutarlı ve üretken filtre seçeneği olarak.

Ne için itmek birkaç anımsatıcıları şunlardır:

* Modeli tarafından istediğiniz her alan için kendiniz Self yönlendirilmiş aramada filtre olarak uygun değerleri içerip içermediğini isteyin. Değerler kısa, açıklayıcı ve rakip seçenekler arasında NET bir seçenek sunmak için yeterince farklı olmalıdır.
* Yazım hatası veya neredeyse eşleşen değerler. Varsa, model renk ve alan değerlerini içeren turuncu ve Ornage (bir yazım hatası), renk alanına dayalı bir modeli hem de çekme.
* Karışık büyük metin de turuncu ve iki farklı değerler olarak görünen turuncu modellenmiş bir gezinmede düzensizliğe benzer zararlar verecektir. 
* Aynı değeri tek ve çoğul sürümleri, her biri için ayrı bir modelinin neden olabilir.

Hayal edebildiğiniz gibi verileri hazırlama içinde tespitlerini etkili modellenmiş bir gezinmede önemli bir yönüdür.

<a name="presentationlayer"></a>

## <a name="build-the-ui"></a>Kullanıcı Arabirimini oluşturma
Sunu katmanı çalışmasını geri hangi özellikler arama deneyimi için gereklidir, aksi takdirde eksik ve anlama gereksinimleri ortaya çıkarmaya yardımcı olabilir.

Web ya da uygulama sayfanızı modellenmiş gezinti yapısını görüntüler modellenmiş bir gezinmede bakımından kullanıcı giriş sayfasında algılar ve değiştirilen öğeler ekler. 

Web uygulamaları için AJAX artımlı değişiklikler yenilemeye izin verdiğinden sunu katmanı yaygın olarak kullanılır. ASP.NET MVC veya bir Azure Search hizmetine HTTP üzerinden bağlanan herhangi bir görsel öğe platforma da kullanabilirsiniz. Bu makale--başvurulan örnek uygulama **Azure arama proje portalı Demo** – bir ASP.NET MVC uygulaması olacak şekilde gerçekleşir.

Aşağıdaki örnekte modellenmiş bir gezinmede arama sonuçları sayfasına yerleşik olarak bulunur. Alınan aşağıdaki örnekte, `index.cshtml` dosyayı modellenmiş bir gezinmede Search'te görüntüleme gösterir durağan HTML'yi yapısı örnek uygulamasının sayfa sonuçlanır. Modelleri listesi yerleşik veya bir arama terimi göndermek ya da seçin veya bir model temizlediğinizde dinamik olarak yeniden.

```html
<div class="widget sidebar-widget jobs-filter-widget">
  <h5 class="widget-title">Filter Results</h5>
    <p id="filterReset"></p>
    <div class="widget-content">

      <h6 id="businessTitleFacetTitle">Business Title</h6>
      <ul class="filter-list" id="business_title_facets">
      </ul>

      <h6>Location</h6>
      <ul class="filter-list" id="posting_type_facets">
      </ul>

      <h6>Posting Type</h6>
      <ul class="filter-list" id="posting_type_facets"></ul>

      <h6>Minimum Salary</h6>
      <ul class="filter-list" id="salary_range_facets">
      </ul>

  </div>
</div>
```

Aşağıdaki kod parçacığını gelen `index.cshtml` sayfası, ilk model, iş başlık görüntülenecek HTML dinamik olarak oluşturur. Benzer işlevler dinamik olarak HTML diğer yönleri için oluşturun. Bir etiket ve için model sonucunda ortaya çıkan bulunan öğelerin sayısını görüntüler bir sayısı her modeli vardır.

```js
function UpdateBusinessTitleFacets(data) {
  var facetResultsHTML = '';
  for (var i = 0; i < data.length; i++) {
    facetResultsHTML += '<li><a href="javascript:void(0)" onclick="ChooseBusinessTitleFacet(\'' + data[i].Value + '\');">' + data[i].Value + ' (' + data[i].Count + ')</span></a></li>';
  }

  $("#business_title_facets").html(facetResultsHTML);
}
```

> [!TIP]
> Arama sonuçları sayfasını tasarlarken, modelleri temizlemek için bir mekanizma eklemeyi unutmayın. Onay kutusu eklerseniz, filtreleri temizlemek nasıl kolayca görebilirsiniz. Diğer düzenleri için bir içerik haritası desen veya başka bir yaratıcı yaklaşım gerekebilir. Örneğin, iş arama Portal örnek uygulamasında tıklayabilirsiniz `[X]` modeli temizlemek için seçilen modeli sonra.

<a name="buildquery"></a>

## <a name="build-the-query"></a>Sorgu oluşturma
Sorgular oluşturmak için yazma kod arama ifadeleri, modelleri, profilleri – formüle bir istek için kullanılan herhangi bir şey Puanlama filtreleri de dahil olmak üzere geçerli bir sorgu tüm parçalarını belirtmeniz gerekir. Bu bölümde, modelleri bir sorgu nerelerde ve filtreleri azaltılmış sonuç kümesi sunmak için yönleri ile nasıl kullanıldığı keşfedin.

Modelleri Bu örnek uygulamasında tam sayı olduğuna dikkat edin. Proje portalı Demo arama deneyimi modellenmiş bir gezinmede ve filtreleri tasarlanmıştır. Sayfasında modellenmiş bir gezinmede belirgin yerleşimini önem derecesini gösterir. 

Bir örnek genellikle başlamak için uygun bir yerdir. Alınan aşağıdaki örnekte, `JobsSearch.cs` dosya, model Gezinti oluşturur isteğin tabanlı iş başlık, konum, nakil türü ve Minimum maaş derlemeleri. 

```cs
SearchParameters sp = new SearchParameters()
{
  ...
  // Add facets
  Facets = new List<String>() { "business_title", "posting_type", "level", "salary_range_from,interval:50000" },
};
```

Bir model sorgu parametresi için bir alanın ayarlanır ve bağlı olarak veri türü, daha fazla parametreli içeren virgülle ayrılmış liste `count:<integer>`, `sort:<>`, `interval:<integer>`, ve `values:<list>`. Aralıkları ayarlarken değerler listesini sayısal veriler için desteklenir. Bkz: [Search belgeleri (Azure Search API)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) kullanım ayrıntıları için.

Modelleri yanı sıra, uygulamanız tarafından şeklide isteği ayrıca bir model değeri seçimine göre adayı belgeleri kümesi daraltmak için filtreleri oluşturması gerekir. Bir Bisiklet Mağazası için ipuçları soruların ister modellenmiş bir gezinmede sağlar *hangi renkler, üreticileri ve bisiklet türleri kullanılabilir?*. Filtreleme gibi sorulara yanıtlar *hangi tam bisiklet kırmızı, Sıradağlar bisiklet bu aralığı fiyat?*. Yalnızca kırmızı ürünlerini gösterilen olduğunu belirtmek için "Red" tıklattığınızda, uygulamanın gönderdiği sonraki sorgu içeren `$filter=Color eq ‘Red’`.

Aşağıdaki kod parçacığını gelen `JobsSearch.cs` sayfa eklerse Seçili iş başlık filtre uygulamak için iş başlık modelin bir değer seçin.

```cs
if (businessTitleFacet != "")
  filter = "business_title eq '" + businessTitleFacet + "'";
```

<a name="tips"></a> 

## <a name="tips-and-best-practices"></a>İpuçları ve en iyi yöntemler

### <a name="indexing-tips"></a>Dizin oluşturma ipuçları
**Bir arama kutusu kullanmazsanız dizin verimliliğini artırmak**

Uygulamanız çok yönlü gezinme özel olarak kullanıyorsa (diğer bir deyişle, hiçbir arama kutusu), alan olarak işaretleyebilirsiniz `searchable=false`, `facetable=true` daha küçük bir dizin oluşturmak için. Ayrıca, dizin oluşturma işlemi yalnızca tüm model değerlerle word kesme yok veya birden çok word değeri bileşen parçalarının dizinini oluşur.

**Hangi alanların modelleri kullanılabileceğini belirtin**

Hangi alanların bir model kullanabileceğiniz dizin şeması belirler geri çağırma. Bir alan modellenebilir olduğunu varsayarak, sorgu, modeli tarafından alanlarını belirtir. Olduğunu olduğunuz alan altına etiketi görülen değerleri sağlar. 

Her etiketin altında görüntülenen değerler dizinden alınır. Örneğin, model alan ise *renk*, ek filtreleme için kullanılabilir değerler, alanın - kırmızı, siyah ve benzeri değerleri.

Yalnızca sayısal ve tarih/saat değerleri için açıkça modeli alanında değerlerini ayarlayabilirsiniz (örneğin, `facet=Rating,values:1|2|3|4|5`). Değerler listesini modeli sonuçları ayrımı bitişik aralıkları (sayısal değerleri veya dönemlere göre ya da aralıkları) içine basitleştirmek Bu alan türleri için izin verilir. 

**Varsayılan olarak yalnızca bir düzey modellenmiş bir gezinmede olabilir** 

Belirtildiği gibi bir hiyerarşideki modelleri iç içe geçme için doğrudan desteği yoktur. Varsayılan olarak, Azure Search'te modellenmiş bir gezinmede yalnızca bir düzey filtre destekler. Ancak, geçici çözümler mevcut. Hiyerarşik modeli yapısında kodlayabilir bir `Collection(Edm.String)` ile bir giriş noktası hiyerarşisi. Bu geçici çözüm uygulandığında, bu makalenin kapsamı dışındadır olur. 

### <a name="querying-tips"></a>İpuçları sorgulama
**Alanları doğrula**

Dinamik olarak güvenilmeyen kullanıcı girişini temel alarak modelleri listesi yapılandırdıysanız, modellenmiş alanların adlarını geçerli olduğunu doğrulayın. Veya URL'leri kullanarak oluştururken adları kaçış `Uri.EscapeDataString()` .NET veya eşdeğer tercih, Platform.

### <a name="filtering-tips"></a>Filtreleme ipuçları
**Arama duyarlık filtrelerle artırın**

Filtreleri kullanın. Üzerinde güveniyorsanız yalnızca arama ifadeleri kaynaklanan tek başına, bir belge, kesin model değeri kendi alanların hiçbirini yok döndürülecek neden olabilir.

**Filtrelerle arama performansı artırma**

Filtreler arama için aday belgeleri kümesi daraltmak ve gelen sıralaması içermeyecek. Çok sayıda belgeleri varsa, detaya seçmeli modeli genellikle kullanarak, daha iyi performans sağlar.
  
**Filtre yalnızca modellenmiş alanları**

Modellenmiş ayrıntıya aşağı genellikle yalnızca model değeri belirli (modellenmiş) alanında olmayan herhangi bir yere tüm aranabilir alanları olan belgeler dahil etmek istediğiniz. Bir filtre eklemeden hedef alan eşleşen bir değeri için yalnızca modellenmiş alanında arama hizmete yönlendirerek eklenir.

**Daha fazla filtrelerle modeli sonuçları kırpma**

Model sonuçları modeli terimiyle eşleşen arama sonuçlarında bulunan belgelerdir. Aşağıdaki örnekte, arama sonuçlarında *bulut bilgi işlem*, 254 öğeleri de *iç belirtimi* bir içerik türü. Öğeleri dışlayan olmak zorunda değildir. Bir öğeyi her iki filtre ölçütlerini karşılıyorsa, her birinde sayılır. Bu çoğaltma mümkündür üzerinde olduğunu `Collection(Edm.String)` belge etiketleme uygulamak için sık kullanılan alanları.

        Search term: "cloud computing"
        Content type
           Internal specification (254)
           Video (10) 

Genel olarak, görürseniz modeli sonuçları sürekli olarak çok büyük olduğundan, öneririz arama daraltma için daha fazla seçenek kullanıcılara vermek için daha fazla filtreleri ekleyin.

### <a name="tips-about-result-count"></a>Sonuç sayısı hakkında ipuçları

**Model Gezinti içindeki öğe sayısını sınırla**

Gezinti ağacında her modellenmiş alan için bir varsayılan sınır 10 değer yoktur. Değerler listesi yönetilebilir bir boyuta tuttuğu için bu varsayılan gezinti yapıları için anlamlıdır. Saymak için bir değer atayarak Varsayılanı geçersiz kılabilirsiniz.

* `&facet=city,count:5` yalnızca sonuçlar derece üst bulunan ilk beş Şehir modeli sonucu olarak döndürüleceğini belirtir. Bir arama terimi "havaalanı" ve 32 eşleşen bir örnek sorgu göz önünde bulundurun. Sorgu belirtiyorsa `&facet=city,count:5`, yalnızca ilk beş benzersiz Şehir arama sonuçlarında en belgelerle modeli sonuçlara dahil edilir.

Model sonuçları ve arama sonuçları arasında ayrım dikkat edin. Sorguyla eşleşen tüm belgeleri arama sonuçları olabilir. Model sonuçlar her model değeri için eşleşme olur. Örnekte, arama sonuçları (örneğimizde 5) modeli sınıflandırma listesinde olmayan Şehir adları içerir. Modelleri işaretini kaldırın veya şehir yanı sıra diğer yönleri seçin çok yönlü gezinmeyi filtrelenen sonuçları görünür hale gelmiştir. 

> [!NOTE]
> Ele `count` olduğunda birden çok tür kafa karıştırıcı olabilir. Aşağıdaki tabloda terimi Azure Search API, örnek kod ve belgeleri nasıl kullanılacağı kısa bir özeti sunar. 

* `@colorFacet.count`<br/>
  Sunu kodda model sonuç sayısını görüntülemek için kullanılan model üzerinde sayısı parametre görmeniz gerekir. Model sonuçlarında sayısı modeli terim ya da aralığı eşleşen belgeleri sayısını gösterir.
* `&facet=City,count:12`<br/>
  Bir model sorgu sayısı için bir değer ayarlayabilirsiniz.  Varsayılan değer 10'dur, ancak daha yüksek veya düşük ayarlayabilirsiniz. Ayarı `count:12` 12 üst eşleşen modeli sonuçlarında belge sayısına göre alır.
* "`@odata.count`"<br/>
  Sorgu yanıt olarak, bu değer arama sonuçlarında eşleşen öğe sayısını belirtir. Ortalama olarak, arama terimiyle eşleşen öğeleri varlığı nedeniyle birleştirilmiş tüm model sonuçların toplamını daha büyük ancak herhangi bir model değeri eşleşme vardır.

**Model sonuçlarında sayılarını elde**

Modellenmiş bir sorgu için bir filtre eklediğinizde, model deyimi korumak isteyebilirsiniz (örneğin, `facet=Rating&$filter=Rating ge 4`). Teknik olarak, model = değil derecelendirme gereklidir, ancak bunu tutma döndürür derecelendirmeler için model değerleri sayar 4 ve üzeri. Örneğin, "4"'i tıklatın ve daha büyük veya eşit "4" için bir filtre sorgu içeriyorsa, 4. her bir derecelendirme için döndürülen ve daha yüksek sayar.  

**Doğru model sayılarını elde emin olun**

Belirli koşullar altında model sayıları sonuç kümelerini eşleşmiyor bulabilirsiniz (bkz [Azure Search'te (forum gönderisi) modellenmiş bir gezinmede](https://social.msdn.microsoft.com/Forums/azure/06461173-ea26-4e6a-9545-fbbd7ee61c8f/faceting-on-azure-search?forum=azuresearch)).

Model sayıları parçalama mimarisi nedeniyle tutarsız olabilir. Birden çok parça her arama dizini vardır ve her parça sonra tek bir sonuç birleştirilmiş belge sayısına göre üst N modelleri raporlar. Başkalarının daha az varken bazı parça birçok eşleşen değerleri, varsa, bazı modeli değerleri eksik olan veya altında sonuçlarında sayılan bulabilirsiniz.

Bu davranış bugün karşılaşırsanız Bu davranış herhangi bir zamanda değişebilir rağmen çevresinde yapay sayısı inflating tarafından çalışabilirsiniz:<number> tam her parça raporlama zorlamak için büyük bir sayı. Varsa sayısı değeri: değerinden büyük veya eşit alanında benzersiz değerlerin sayısını, doğru sonuçlar garanti. Ancak, belge sayısını yüksek olduğunda bulunmaktadır performans, bu nedenle bu seçeneği dikkatli kullanın.

### <a name="user-interface-tips"></a>Kullanıcı arabirimi ipuçları
**Model Gezinti bölmesinde her bir alan için etiketler ekleme**

Etiketleri tipik HTML veya formunda tanımlanır (`index.cshtml` örnek uygulamasında). Azure Search'te model Gezinti etiketleri için API yok veya diğer meta verileri yok.

<a name="rangefacets"></a>

## <a name="filter-based-on-a-range"></a>Bir aralığı tabanlı filtresi
Değerleri aralığı üzerinden olduğunu ortak arama uygulaması gerekli değildir. Aralıkları sayısal veri ve DateTime değerleri için desteklenir. Daha fazla bilgiyi her yaklaşımı hakkında [Search belgeleri (Azure Search API)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).

Azure arama, bir aralık bilgi işlem için iki yaklaşım sağlayarak aralığı yapım basitleştirir. Her iki yaklaşımın için Azure Search sağladığınız girişleri verilen uygun aralıkların oluşturur. Örneğin, 10 aralık değerleri belirtirseniz | 20 | 30, otomatik olarak oluşturur, 0-10, 10-20, 20-30 aralıkları. Uygulamanızın isteğe bağlı olarak boş aralıkları kaldırabilirsiniz. 

**Yaklaşım 1: aralığı parametresini kullanın**  
Fiyat modelleri 10 artışlarla ayarlamak için belirtmeniz gerekir: `&facet=price,interval:10`

**Yaklaşım 2: değerler listesini kullanın**  
Sayısal veriler için değerler listesi kullanabilirsiniz.  Model aralığının göz önünde bulundurun bir `listPrice` gibi çizilir alan:

  ![Örnek değerler listesi][5]

Önceki ekran görüntüsü bir model aralığı belirtmek için değerler listesini kullanın:

    facet=listPrice,values:10|25|100|500|1000|2500

Her aralık 0 bir uç nokta olarak listeden bir değer bir başlangıç noktası olarak kullanarak ve ayrık aralıklarını oluşturmak için önceki aralığı kırpılmış yerleşik olarak bulunur. Azure arama modellenmiş bir gezinmede bir parçası olarak bu işlemi yapar. Her aralık yapılandırılması için kod yazmanız gerekmez.

### <a name="build-a-filter-for-a-range"></a>Bir aralık için bir filtre oluşturun
Seçtiğiniz bir aralığı tabanlı belgeleri filtrelemek için kullanabileceğiniz `"ge"` ve `"lt"` filtre aralığın uç noktalarını tanımlayan iki parçalı ifadesi işleçler. Örneğin, 10-25 aralığının seçerseniz bir `listPrice` alanın filtre olacaktır `$filter=listPrice ge 10 and listPrice lt 25`. Filtre ifadesi örnek kodda kullanan **priceFrom** ve **priceTo** uç noktalarını ayarlamak için parametreler. 

  ![Bir değer aralığı için sorgu][6]

<a name="geofacets"></a> 

## <a name="filter-based-on-distance"></a>Uzaklık üzerinde göre filtrele
Görmek için ortak bir mağaza, Restoran ya da kendi yakınlık geçerli konumunuza göre hedef seçtiğiniz yardımcı filtreler. Bu tür filtresini modellenmiş bir gezinmede gibi görünebilir, ancak bunu yalnızca bir filtredir. Biz buraya uygulama öneriler, belirli tasarım sorunu için özellikle istiyorsunuz, bu için Bahsediyor.

Azure Search'te iki Jeo-uzamsal işlevleri vardır **geo.distance** ve **geo.intersects**.

* **Geo.distance** işlevi arasında iki nokta kilometre uzaklığını döndürür. Bir noktadan bir alan ve diğer filtre bir parçası olarak geçirilen bir sabit değer. 
* **Geo.intersects** işlevi verilen bir noktaya içinde belirli bir Çokgen ise true döndürür. Bir alan noktasıdır ve Çokgen filtre bir parçası olarak geçirilen koordinatları sabit listesi olarak belirtilir.

Filtre örneklerde bulabilirsiniz [OData ifadesi sözdizimi (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search).

<a name="tryitout"></a>

## <a name="try-the-demo"></a>Demoyu deneyin
Azure arama proje portalı Demo bu makalede başvurulan örnekler yer almaktadır.

-   Bakın ve çalışma demo adresindeki çevrimiçi test [Azure arama proje portalı Demo](http://azjobsdemo.azurewebsites.net/).

-   Koddan karşıdan [Azure-Samples bağlantıların github'da](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs).

Arama sonuçları ile çalışırken, sorgu yapım değişiklikleri URL'sini izleyin. Bu uygulama, tek tek seçerek modelleri uri'ye için gerçekleşir.

1. Tanıtım uygulamasını eşleme işlevselliğini kullanmak için Bing Haritalar anahtarından alma [Bing Haritalar Geliştirme Merkezi](https://www.bingmapsportal.com/). Varolan anahtar üzerinden yapıştırın `index.cshtml` sayfası. `BingApiKey` Ayarı `Web.config` dosya kullanılmaz. 

2. Uygulamayı çalıştırın. İsteğe bağlı tura katılın veya iletişim kutusunu kapatın.
   
3. "Analist" gibi bir arama terimi girin ve arama simgesine tıklayın. Sorgu hızlı bir şekilde yürütür.
   
   Modellenmiş bir gezinmede yapısı, Arama sonuçlarıyla olarak da döndürülür. Arama sonuçları sayfasında modellenmiş bir gezinmede yapısı her modeli sonucu sayılarını içerir. Hiçbir modelleri seçildi bu nedenle tüm eşleşen sonuç döndürmedi.
   
   ![Modelleri seçmeden önce arama sonuçları][11]

4. Bir iş başlık, konum veya en düşük maaş'ı tıklatın. Modelleri ilk Search'te null, ancak değerler aldıkları gibi arama sonuçlarını artık eşleşen öğeleri atılır.
   
   ![Modelleri seçtikten sonra arama sonuçları][12]

5. Böylece farklı sorgu davranışları deneyebilirsiniz modellenmiş sorgu temizlemek için tıklatın `[X]` modelleri temizlemek için seçilen modelleri sonra.
   
<a name="nextstep"></a>

## <a name="learn-more"></a>Daha fazla bilgi edinin
Gözcü [Azure arama derinlemesine](http://channel9.msdn.com/Events/TechEd/Europe/2014/DBI-B410). 45:25 modelleri uygulamak nasıl demo yoktur.

Tasarım ilkeleri modellenmiş gezinmesine daha fazla bilgiler için aşağıdaki bağlantıları öneririz:

* [Modellenmiş Ara tasarlama](http://www.uie.com/articles/faceted_search/)
* [Tasarım desenleri: Çok yönlü gezinme](http://alistapart.com/article/design-patterns-faceted-navigation)


<!--Anchors-->
[How to build it]: #howtobuildit
[Build the presentation layer]: #presentationlayer
[Build the index]: #buildindex
[Check for data quality]: #checkdata
[Build the query]: #buildquery
[Tips on how to control faceted navigation]: #tips
[Faceted navigation based on range values]: #rangefacets
[Faceted navigation based on GeoPoints]: #geofacets
[Try it out]: #tryitout

<!--Image references-->
[1]: ./media/search-faceted-navigation/azure-search-faceting-example.PNG
[2]: ./media/search-faceted-navigation/Facet-2-CSHTML.PNG
[3]: ./media/search-faceted-navigation/Facet-3-schema.PNG
[4]: ./media/search-faceted-navigation/Facet-4-SearchMethod.PNG
[5]: ./media/search-faceted-navigation/Facet-5-Prices.PNG
[6]: ./media/search-faceted-navigation/Facet-6-buildfilter.PNG
[7]: ./media/search-faceted-navigation/Facet-7-appstart.png
[8]: ./media/search-faceted-navigation/Facet-8-appbike.png
[9]: ./media/search-faceted-navigation/Facet-9-appbikefaceted.png
[10]: ./media/search-faceted-navigation/Facet-10-appTitle.png
[11]: ./media/search-faceted-navigation/faceted-search-before-facets.png
[12]: ./media/search-faceted-navigation/faceted-search-after-facets.png

<!--Link references-->
[Designing for Faceted Search]: http://www.uie.com/articles/faceted_search/
[Design Patterns: Faceted Navigation]: http://alistapart.com/article/design-patterns-faceted-navigation
[Create your first application]: search-create-first-solution.md
[OData expression syntax (Azure Search)]: https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search
[Azure Search Adventure Works Demo]: https://azuresearchadventureworksdemo.codeplex.com/
[http://www.odata.org/documentation/odata-version-2-0/overview/]: http://www.odata.org/documentation/odata-version-2-0/overview/ 
[Faceting on Azure Search forum post]: ../faceting-on-azure-search.md?forum=azuresearch
[Search Documents (Azure Search API)]: https://docs.microsoft.com/rest/api/searchservice/Search-Documents

