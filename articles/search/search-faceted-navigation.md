---
title: Nasıl bir category hiyerarşisi - Azure Search çok yönlü navigasyon uygulamak için
description: Microsoft Azure üzerinde barındırılan bulut arama hizmeti olan Azure Search ile tümleşen uygulamaları için model Gezinti ekleyin.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: b5c7050ac006ea2500854f8f41b134895e5e0061
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541222"
---
# <a name="how-to-implement-faceted-navigation-in-azure-search"></a>Azure Arama'da çok yönlü navigasyon
Çok yönlü gezinme, kendinden yönlendirmeli detayına gitme Gezinti arama uygulamalarda sağlayan filtreleme bir mekanizmadır. ' % S'terim 'çok yönlü gezinme' bilinmiyor olabilir, ancak büyük olasılıkla daha önce kullanılmış. Aşağıdaki örnekte gösterildiği gibi çok yönlü gezinme sonuçları filtrelemek için kullanılan kategorileri başka bir şey var.

 ![Azure arama iş Portal Tanıtımı][1]

Çok yönlü gezinme, aramak için bir alternatif giriş noktasıdır. El ile karmaşık arama ifadeleri yazmak için uygun bir alternatif sunar. Modelleri, sıfır sonuçlar elde etmezsiniz sağlarken aradığınızı bulmanıza yardımcı olabilir. Bir geliştirici olarak, arama topluluğunuza gezinmek için kullanışlı olabilecek arama ölçütleri kullanıma özellikleri sağlar. Çevrimiçi satış uygulamalar, çok yönlü gezinme genellikle markaları, Departmanlar (çocuk shoes), boyutu, fiyat, özelliği sayesinde Popülerlik ve derecelendirmeler üzerinde oluşturulmuştur. 

Çok yönlü navigasyon uygulamak arama teknolojilerinden farklıdır. Azure arama'yı kullanarak daha önce Şemanızda öznitelikli alanlar sorgu zamanında çok yönlü gezinme oluşturulmuştur.

-   Uygulamanız oluşturulur, sorguda göndermelisiniz sorgularda *modeli sorgu parametreleri* kullanılabilir modeli filtre değerleri için bu belge sonuç kümesi almak için.

-   Belge sonucu gerçekten trim koymak, uygulama de uygulamanız gerekir bir `$filter` ifade.

Uygulama geliştirme çalışmalarınızı sorguları oluşturan kod yazma, toplu iş oluşturur. Modellenmiş Gezinti bölmesinden beklediğiniz uygulama davranışları birçoğu, aralıkları tanımlama ve sayıları için modeli sonuçları almak için yerleşik destek de dahil olmak üzere hizmet tarafından sağlanır. Hizmet ayrıca yardımcı mantıklı varsayılanlar içerir kullanışsız Gezinti yapıları kaçının. 

## <a name="sample-code-and-demo"></a>Örnek kod ve tanıtım
Bu makalede örnek olarak bir iş araması portalını kullanır. Örneğin, bir ASP.NET MVC uygulaması uygulanır.

-   Görebilir ve test sırasında çevrimiçi çalışma Tanıtımı [Azure arama iş Portal Tanıtımı](http://azjobsdemo.azurewebsites.net/).

-   Kodu indir [github'daki Azure örnekleri deposu](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs).

## <a name="get-started"></a>başlarken
Geliştirme arama yeniyseniz, çok yönlü gezinme düşünme en iyi bağımsız aramayı olasılıklarını gösterir yoludur. Detaya gitme arama deneyimi, arama sonuçları ve tıklama eylemler arasında aşağı hızla daraltmak için kullanılan önceden tanımlanmış filtreleri temel alarak türüdür. 

### <a name="interaction-model"></a>Etkileşim modeli

Çok yönlü bir gezinti arama deneyimi Haydi yanıt olarak kullanıcı eylemlerini unfold sorguları bir dizi olarak anlayarak başlayın yinelemelidir.

Çok yönlü gezinme, genellikle periphery üzerinde yerleştirilen sağlayan bir uygulama sayfası başlangıç noktasıdır. Çok yönlü gezinme, genellikle her değeri veya tıklanabilir bir metin için onay kutuları içeren bir ağaç yapısı olur. 

1. Azure Search için gönderilen bir sorgu, çok yönlü gezinme yapısı bir veya daha fazla model sorgu parametreleri aracılığıyla belirtir. Örneği için sorguyu içerebilir `facet=Rating`, belki de sahip bir `:values` veya `:sort` sunu iyice daraltmak için seçeneği.
2. Sunu katmanı çok yönlü gezinme, istekte belirtilen modellerle kullanarak sağlayan bir arama sayfasını işler.
3. Derecelendirme içeren bir çok yönlü gezinme yapısı göz önünde bulundurulduğunda, "4" öğesini yalnızca 4 veya daha yüksek bir derecelendirme ürünleriyle gösterilecek belirtmek için. 
4. Yanıt olarak, uygulamayı içeren bir sorgu gönderir. `$filter=Rating ge 4` 
5. Yeni ölçütleri karşılayan yalnızca öğeleri içeren daha az sonuç kümesini gösteren sayfa sunu katmanını güncelleştirir (Bu durumda 4 ürünleri derecelendirilmiş ve üstü).

Bir model için bir sorgu parametresi olsa da, sorgu girişi ile karıştırmayın. Bir sorguda seçim ölçütü olarak hiçbir zaman kullanılmaz. Bunun yerine, model Sorgu parametrelerinin yanıtta gelir gezinme yapısına girdi olarak düşünün. Sağladığınız her modeli sorgu parametresi için Azure Search, her model değeri için kısmi sonuçları kaç belgelerdir değerlendirir.

Bildirim `$filter` 4. adımda. Filtre, çok yönlü gezinme, önemli bir yönüdür. Özellikleri ve filtreleri API'de bağımsız olmakla birlikte, düşündüğünüz bir deneyim sunmak için her ikisi de gerekir. 

### <a name="app-design-pattern"></a>Uygulama tasarım deseni

Uygulama kodunda modeli sonuçları yanı sıra, $filter ifadesinin yanı sıra çok yönlü gezinme yapısı döndürmek için facet sorgu parametrelerini kullanın modelidir.  Filtre ifadesi modeli değeri click olayını işler. Düşünün `$filter` gerçek kesme ardında kod olarak ifade arama sonuçları için sunu katmanı döndürdü. Renkleri modeli göz önünde bulundurulduğunda, kırmızı renk tıklayarak aracılığıyla uygulanır bir `$filter` ifade kırmızı rengini olan öğeleri seçer. 

### <a name="query-basics"></a>Sorgu temelleri

Azure Search'te bir isteği bir veya daha fazla sorgu parametreler belirtilen (bkz [arama belgeleri](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) her biri bir açıklaması için). Sorgu parametreleri hiçbiri gerekli değil, ancak en az bir geçerli olması bir sorgu için sırada olması gerekir.

Duyarlık, ilgisiz isabet filtreleme özelliği gerçekleştirilir gibi bir veya iki bu ifadelerin anladım:

-   **search=**  
    Bu parametrenin değeri, arama ifade oluşturur. Tek bir metin ya da birden çok hüküm ve işleçleri içeren bir karmaşık arama ifadesi olabilir. Sunucu üzerinde bir arama ifadesi tam metin arama, aranabilir alanları sözcüklerle, derece sırayla sonuçları döndüren eşleşen için dizinde sorgulamak için kullanılır. Ayarlarsanız `search` null olarak sorgu yürütmesi, tüm dizini (diğer bir deyişle, `search=*`). Bu durumda, diğer öğeleri sorgu gibi bir `$filter` veya Puanlama profili, hangi belgeler döndürülür etkileyen temel unsurlar `($filter`) ve hangi sırayla (`scoringProfile` veya `$orderby`).

-   **$filter =**  
    Bir filtre, belirli bir belge öznitelik değerlerine göre arama sonuçları boyutunu sınırlamak için güçlü bir mekanizmadır. A `$filter` önce değerlendirilir, kullanılabilir değerler ve her bir değer için karşılık gelen sayıları oluşturan modelleme mantıksal ardından

Karmaşık arama ifadeleri sorgu performansını azaltır. Mümkünse, duyarlık artırmak ve sorgu performansını artırmak için iyi oluşturulmuş filtre ifadeleri kullanın.

Nasıl daha hassas bir filtre ekler daha iyi anlamak için bir filtre ifadesi içeren bir karmaşık arama ifadesi karşılaştırın:

-   `GET /indexes/hotel/docs?search=lodging budget +Seattle –motel +parking`
-   `GET /indexes/hotel/docs?search=lodging&$filter=City eq ‘Seattle’ and Parking and Type ne ‘motel’`

Her iki sorguları geçerli, ancak ikinci motels olmayan Seattle park ile arıyorsanız üstündür.
-   İlk sorgu bahsedilen veya dize alanları aranabilir verileri içeren herhangi bir alan adı ve açıklaması gibi geçmeyen bu belirli sözcükleri kullanır.
-   İkinci sorgu, yapılandırılmış veriler üzerinde kesin eşleşmeleri arar ve çok daha doğru hale gelmesi muhtemeldir.

Çok yönlü gezinme içeren uygulamalar, her bir kullanıcı eylemi çok yönlü gezinme yapısı üzerinden arama sonuçlarını daraltarak sunulduğu emin olun. Sonuçları daraltmak için bir filtre ifadesi kullanın.

<a name="howtobuildit"></a>

## <a name="build-a-faceted-navigation-app"></a>Çok yönlü gezinme uygulaması derleme
Uygulama kodunuzda arama isteği oluşturan Azure Search ile çok yönlü gezinme uygulayın. Çok yönlü gezinme, daha önceden tanımlanan, şemadaki öğelerin kullanır.

Önceden tanımlanmış aramanızda dizinidir `Facetable [true|false]` dizin öznitelikleri, etkinleştirme veya devre dışı kullanımları çok yönlü gezinme yapısına seçili alanları ayarlayın. Olmadan `"Facetable" = true`, model Gezinti bölmesindeki bir alanı kullanılamaz.

Sunu katmanı kodunuzda kullanıcı deneyimi sağlar. Bu etiket, değerleri, onay kutularını ve sayı gibi çok yönlü gezinme, bağlı bölümlerini listelemelisiniz. Azure Search REST API'sine platformu belirsiz olduğundan, hangi dilde ve istediğiniz platformu kullanın. Her ek modeli seçtiğinizde güncelleştirilmiş kullanıcı Arabirimi durumu ile artımlı yenileme destekleyen bir kullanıcı Arabirimi öğeleri dahil etmek için önemli olan olmasıdır. 

Sorgu zamanında uygulama kodunuzu içeren bir istek oluşturur `facet=[string]`, model tarafından alana sağlayan bir istek parametresi. Birden çok modelleri gibi bir sorgu olabilir `&facet=color&facet=category&facet=rating`, her biri ayrılmış bir ampersan (&) karakteri tarafından.

Uygulama kodu gerekir ayrıca oluşturmak bir `$filter` çok yönlü gezinme, tıklama olaylarını işlemek için ifade. A `$filter` model değeri filtre ölçütü olarak kullanarak arama sonuçlarını azaltır.

Azure arama güncelleştirmeleri çok yönlü gezinme yapısına yönelik işaretçinin birlikte girin bir veya daha çok terimi göre arama sonuçlarını döndürür. Azure Search'te, çok yönlü gezinme modeli değerleri içeren bir tek düzey yapım ve her biri için kaç sonuçlarını bulunan sayar.

Aşağıdaki bölümlerde, size her bölümü oluşturmak nasıl daha yakından göz atın.

<a name="buildindex"></a>

## <a name="build-the-index"></a>Dizini derleme
Model oluşturma, bu dizin özniteliği aracılığıyla dizin bir alan olarak alanı temelinde etkinleştirilir: `"Facetable": true`.  
Büyük olasılıkla çok yönlü gezinme kullanılabilir tüm alan türleri `Facetable` varsayılan olarak. Böyle alan türler `Edm.String`, `Edm.DateTimeOffset`ve tüm sayısal alan türleri (temelde, modellenebilir dışındaki tüm alan türleri olan `Edm.GeographyPoint`, hangi kullanılamaz çok yönlü gezinme). 

Dizin oluştururken, çok yönlü gezinme için en iyi uygulama, hiçbir zaman bir model olarak kullanılması gereken alanlar için açıkça modelleme kapatmak sağlamaktır.  Dize alanları kimliği veya ürün adı gibi tek değerler için özellikle ayarlanmalıdır `"Facetable": false` çok yönlü gezinme yanlışlıkla (ve verimsiz) kullanımları önlemek için. Modelleme burada gerekmeyen kapalı kapatma dizinin boyutunu küçük tutmaya yardımcı olur ve genellikle performansını artırır.

Aşağıdaki şema boyutunu azaltmak için bazı özniteliklerini kırpılmış proje portalı tanıtım örnek uygulama için bir parçasıdır:

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

Örnek şemada gördüğünüz gibi `Facetable` modelleri, kodu değerleri gibi olarak kullanılmaması dize alanları için devre dışıdır. Modelleme burada gerekmeyen kapalı kapatma dizinin boyutunu küçük tutmaya yardımcı olur ve genellikle performansını artırır.

> [!TIP]
> En iyi uygulama, her alan için dizin özniteliklerini tam kümesini içerir. Ancak `Facetable` bilerek ayarlama, neredeyse tüm alanlar için varsayılan olarak her bir öznitelik yardımcı olabilir her bir şema kararı etkilerini düşünme açıktır. 

<a name="checkdata"></a>

## <a name="check-the-data"></a>Verileri denetleme
Verilerinizi kalitesini beklediğiniz gibi çok yönlü gezinme yapısına olup gerçekleştiren üzerinde doğrudan etkisi vardır. Ayrıca, sonuç kümesi azaltmak için filtreleri oluşturma kolaylığı etkiler.

Model marka veya fiyat tarafından istiyorsanız her belge için değerler içermelidir *BrandName* ve *ProductPrice* geçerli, tutarlı ve üretken bir filtre seçeneği olarak.

Birkaç anımsatıcı temizlemek için gerekenler şunlardır:

* Model tarafından istediğiniz her alan için kendiniz kendinden yönlendirmeli arama filtre olarak uygun olan değerleri içerip içermediğini isteyin. Değerler, kısa, açıklayıcı ve birbiriyle rekabet içindeki seçenekleri arasında NET bir seçenek sunmak için yeterince farklı olmalıdır.
* Yazım hatası veya neredeyse eşleşen değerler. Varsa, renk ve alan değerlerini modeli içeren turuncu ve Ornage (bir yazım hatası), hem renk alanını temel alarak bir model seçiyordu.
* Karışık büyük metin ayrıca çok yönlü gezinme, orange ve iki farklı değerler olarak görünen turuncu ile içinde düzensizliğe benzer zararlar verecektir. 
* Aynı değeri tek ve çoğul hallerini, her biri için ayrı bir modelde neden olabilir.

Tahmin edebileceğiniz gibi verilerin hazırlanması dikkatli olmanızı etkili çok yönlü gezinme, önemli bir yönüdür.

<a name="presentationlayer"></a>

## <a name="build-the-ui"></a>Kullanıcı Arabirimini oluşturma
Sunu katmanını geri çalışmasını hangi özellikleri arama deneyimi için gereklidir, aksi takdirde eksik ve anlamak gereksinimleri ortaya çıkarmaya yardımcı olabilir.

Çok yönlü gezinme, açısından web ya da uygulama sayfanızın çok yönlü gezinme yapısına görüntüler, kullanıcı giriş sayfasında algılar ve değişen öğeleri ekler. 

Web uygulamaları için AJAX artımlı değişiklikler yenilemeye izin verdiğinden sunu katmanı yaygın olarak kullanılır. ASP.NET MVC ya da bir Azure Search hizmeti için HTTP üzerinden bağlanabilir herhangi bir görselleştirme platform de kullanabilirsiniz. Bu makale boyunca--başvurulan örnek uygulamayı **Azure arama iş Portal Tanıtımı** – bir ASP.NET MVC uygulaması olacak şekilde gerçekleşir.

Aşağıdaki örnekte, çok yönlü gezinme arama sonuçları sayfasına yerleşik olarak bulunur. Aşağıdaki örnek runbook'undan `index.cshtml` sonuçları sayfası dosyası örnek uygulamanın gösterildiği statik HTML yapısı çok yönlü gezinme Search'te görüntülemek için. Modellerin listesi oluşturulmadan veya bir arama terimi gönderin veya seçtiğinizde veya bir model Temizle dinamik olarak yeniden.

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

Aşağıdaki kod parçacığı `index.cshtml` sayfası ilk modeli, iş başlık görüntülenecek HTML dinamik olarak oluşturur. Benzer işlevler, diğer modelleri için HTML dinamik olarak oluşturun. Her model, bir etiket ve modeli sonucu için bulunan öğe sayısını görüntüleyen bir sayısı vardır.

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
> Modelleri temizlemek için bir mekanizma eklemek arama sonuçları sayfası tasarlarken unutmayın. Onay kutusu eklerseniz, filtreleri temizlemek nasıl kolayca görebilirsiniz. Diğer düzenleri için bir içerik haritası desen veya başka bir yaratıcı yaklaşım gerekebilir. Örneğin, iş araması portalını örnek uygulamada tıklayabilirsiniz `[X]` sonra modeli temizlemek için seçilen bir modeli.

<a name="buildquery"></a>

## <a name="build-the-query"></a>Sorgu oluşturma
Sorguları oluşturmak için yazdığınız kod arama ifadeleri, modelleri, Puanlama profillerini – herhangi bir istek düzenleyin için kullanılan filtreler dahil olmak üzere, geçerli bir sorgu tüm parçalarını belirtmeniz gerekir. Bu bölümde, modelleri bir sorguya nerelerde ve filtreleri daha az sonuç kümesi teslim etmek için modelleri ile nasıl kullanıldığını keşfedin.

Bu örnek uygulamasında modelleri ayrılmaz dikkat edin. Proje portalı tanıtım arama deneyimini, çok yönlü gezinme ve filtreleri geçici olarak tasarlanmıştır. Çok yönlü gezinme sayfasında tanınmış yerleşimini önem derecesini gösterir. 

Genellikle başlamak için iyi bir yerdir örneğidir. Aşağıdaki örnek runbook'undan `JobsSearch.cs` dosyası, model Gezinti oluşturan bir istek tabanlı iş başlık, konum, yayınlayarak, türü ve Minimum ücret derlemeler. 

```cs
SearchParameters sp = new SearchParameters()
{
  ...
  // Add facets
  Facets = new List<String>() { "business_title", "posting_type", "level", "salary_range_from,interval:50000" },
};
```

Modeli sorgu parametresi için bir alan olarak ayarlanır ve bağlı veri türü, daha fazla parametreli içeren virgülle ayrılmış liste tarafından `count:<integer>`, `sort:<>`, `interval:<integer>`, ve `values:<list>`. Değerler listesinde aralıkları ayarlarken sayısal veriler için desteklenir. Bkz: [Search belgeleri (Azure Search API)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) kullanım ayrıntıları için.

Modelleri yanı sıra, uygulamanız tarafından şeklide isteği ayrıca bir model değeri seçimi temel alınarak aday belge kümesini daraltmak için filtre oluşturması gerekir. Soruların ipuçları ister bisiklet deposu için çok yönlü gezinme sağlar *hangi renkleri, üreticiler ve bisiklet türleri mevcuttur?*. Filtreleme gibi soruların yanıtları *hangi tam bir bisiklet kırmızı, dağ bisikleti bu aralığı fiyat?*. Yalnızca kırmızı ürünlerini gösterilecek belirtmek için "Red"'a tıkladığınızda uygulamanın gönderdiği sonraki sorgu içeren `$filter=Color eq ‘Red’`.

Aşağıdaki kod parçacığı `JobsSearch.cs` iş başlık modeli bir değer seçerseniz sayfayı Seçili iş başlığı filtresi ekler.

```cs
if (businessTitleFacet != "")
  filter = "business_title eq '" + businessTitleFacet + "'";
```

<a name="tips"></a> 

## <a name="tips-and-best-practices"></a>İpuçları ve en iyi uygulamalar

### <a name="indexing-tips"></a>Dizin oluşturma ipuçları
**Bir arama kutusu kullanmazsanız dizin verimliliği artırın**

Uygulamanız çok yönlü gezinme özel kullanıyorsa (diğer bir deyişle, hiçbir arama kutusu), alan olarak işaretleyebilirsiniz `searchable=false`, `facetable=true` daha kompakt bir dizin üretmek için. Ayrıca, yalnızca tüm model değerlerle sözcük sonu veya birden çok sözcük değerinin bileşen bölümlerini dizin üzerinde dizin gerçekleşir.

**Hangi alanların modelleri kullanılabileceğini belirtin**

Hangi alanların bir model olarak kullanılabilir dizin şemasını belirler geri çağırma. Modellenebilir alandır varsayıldığında, sorgu, model tarafından alanlarını belirtir. Modelleme olduğunuz alanı altına etiketi görünen değerleri sağlar. 

Her etiketin altında görünen değerlerin dizininden alınır. Örneğin, model alan ise *renk*, ek filtreleme için kullanılabilir değerler, alanın - Red, Black ve benzeri değerleri.

Yalnızca sayısal ve tarih/saat değerleri için açıkça modeli alanın değerlerini ayarlayabilirsiniz (örneğin, `facet=Rating,values:1|2|3|4|5`). Değerler listesinde ayrımı modeli sonuçları bitişik aralıkların (sayısal değerleri veya zaman dilimlerine göre ya da aralıkları) içine basitleştirmek bu alanı türleri için izin verilir. 

**Varsayılan olarak bir düzey çok yönlü gezinme, yalnızca olabilir** 

Belirtildiği gibi bir hiyerarşideki modelleri iç içe geçirme için doğrudan desteği yoktur. Varsayılan olarak, Azure Arama'da çok yönlü gezinme, yalnızca bir filtre düzeyini destekler. Ancak, geçici çözümler vardır. Bir hiyerarşik modeli yapısında kodlayabilir bir `Collection(Edm.String)` bir girişle hiyerarşi gelin. Geçici çözüm bu uygulama, bu makalenin kapsamı dışındadır olduğu. 

### <a name="querying-tips"></a>İpuçları sorgulama
**Alanları doğrulama**

Dinamik olarak güvenilmeyen kullanıcı girişini temel alarak modellerin listesini oluşturmak, çok yönlü alanların adlarının geçerli olduğunu doğrulayın. Veya URL'leri kullanarak oluştururken adlarını kaçış `Uri.EscapeDataString()` .NET veya tercih ettiğiniz platformda eşdeğer.

### <a name="filtering-tips"></a>Filtreleme ipuçları
**Filtrelerle arama duyarlık artırın**

Filtreleri kullanın. Üzerinde güveniyorsanız yalnızca arama ifadeleri başına dallanma, kesin model değeri alanlarını hiçbirinde yok döndürülecek belgeye neden olabilir.

**Filtrelerle Arama performansını artırın**

Filtreler arama için aday belge kümesini daraltmak ve sıralamasından içermeyecek. Çok sayıda belgeleri varsa, detaya gitme seçmeli model genellikle kullanarak, daha iyi performans sunar.
  
**Çok yönlü alanlar yalnızca filtre**

Çok yönlü ayrıntıya inme genellikle yalnızca model değeri belirli (çok yönlü) alanında tüm aranabilir alanları arasında değil herhangi bir yerde bulunan belgeleri dahil etmek istersiniz. Bir filtre eklemeden hedef alan eşleşen bir değer için yalnızca çok yönlü alanında aranacak hizmet yönlendirerek çalışacak şekilde güçlendirir.

**Model sonuçlarını daha fazla filtre ile kırpma**

Model sonuçlarını modeli terimiyle eşleşen arama sonuçlarında bulunamadı belgelerdir. Aşağıdaki örnekte, arama sonuçlarında *bulut bilgi işlem*, 254 öğeleri de *iç belirtimi* içerik türü. Öğeleri birbirini dışlamaz. Bir öğe iki filtre ölçütlerini karşılıyorsa, her biri sayılır. Bu çoğaltma mümkündür üzerinde modelleme `Collection(Edm.String)` belge etiketleme uygulamak için sık kullanılan alanlar.

        Search term: "cloud computing"
        Content type
           Internal specification (254)
           Video (10) 

Genel olarak, bulduğunuz modeli sonuçları tutarlı bir şekilde çok büyük olduğundan, öneririz, arama daraltma için daha fazla seçenek kullanıcılara vermek için daha fazla filtre ekleyin.

### <a name="tips-about-result-count"></a>Sonuç sayısı konusunda ipuçları

**Model Gezinti öğe sayısını sınırla**

Gezinti ağacı çok yönlü her alan için varsayılan bir sınırı 10 değer yoktur. Değerler listesinde yönetilebilir bir boyuta tuttuğu bu varsayılan gezinti yapıları için anlamlıdır. Saymak için bir değer atayarak Varsayılanı geçersiz kılabilirsiniz.

* `&facet=city,count:5` yalnızca üst sıralanmış sonuçları bulunan ilk beş şehirler modeli sonucu olarak döndürüleceğini belirtir. Örnek sorgu "havaalanı" ve 32 eşleşen bir arama terimi ile göz önünde bulundurun. Sorgu belirtiyorsa `&facet=city,count:5`, arama sonuçlarında en belgelerle yalnızca ilk beş benzersiz şehirler modeli sonuçlara dahil edilir.

Modeli sonuçları ve arama sonuçları arasında ayrım dikkat edin. Arama sonuçları sorguyla eşleşen tüm belgelerdir. Her model değeri için eşleşme modeli sonuçları olur. Örnekte, arama sonuçları (Bizim örneğimizde, 5) modeli sınıflandırma listede olmayan bir şehir adları içerir. Modelleri temizleyin veya şehir yanı sıra diğer özellikleri seçin çok yönlü gezinmeyi filtrelenmiş sonuçları görünür hale gelir. 

> [!NOTE]
> Görüştükten `count` olduğunda birden fazla tür kafa karıştırıcı olabilir. Aşağıdaki tabloda, Azure arama API'si, örnek kod ve belgeleri terimi nasıl kullanıldığını kısa bir Özet sunar. 

* `@colorFacet.count`<br/>
  Sunu kodunda modeli sonuçları sayısını görüntülemek için kullanılan model üzerinde bir sayı parametresi görmeniz gerekir. Model sonuçlarında sayısı model terimi veya aralık eşleşen belgelerin sayısını gösterir.
* `&facet=City,count:12`<br/>
  Bir modeli sorgu sayısı bir değere ayarlayabilirsiniz.  Varsayılan değer 10'dur ancak bunu daha yüksek veya düşük ayarlayabilirsiniz. Ayar `count:12` 12 üst belge sayısı tarafından modeli sonuçlarında eşleşen alır.
* "`@odata.count`"<br/>
  Sorgu yanıtına, bu değer arama sonuçlarında eşleşen öğe sayısını gösterir. Ortalama olarak, birleştirilmiş, arama terimiyle eşleşen öğeleri varlığı nedeniyle modeli sonuçlarının toplamından büyük ancak herhangi bir model değeri eşleşme vardır.

**Model sonuçlarında sayılarını Al**

Çok yönlü bir sorguya bir filtre eklediğinizde, modeli deyimi korumak isteyebilirsiniz (örneğin, `facet=Rating&$filter=Rating ge 4`). Teknik olarak, model = derecelendirme gerekli olmayan, ancak bunu tutma döndürür sayıları derecelendirmeler için model değerlerinin 4 ve üzeri. Örneğin, "4"'a tıklayın ve büyük veya buna eşit "4" için bir filtre sorgusu içeriyorsa, döndürülen 4 her derecelendirmesi ve daha yüksek sayar.  

**Doğru modeli sayıları alacağınızdan emin olun**

Belirli koşullar altında sonuç kümelerini modeli sayıları eşleşmiyor bulabilirsiniz (bkz [(forum gönderisi) Azure Arama'da çok yönlü gezinme](https://social.msdn.microsoft.com/Forums/azure/06461173-ea26-4e6a-9545-fbbd7ee61c8f/faceting-on-azure-search?forum=azuresearch)).

Model sayıları parçalama mimarisi nedeniyle yanlış olabilir. Her arama dizini birden çok parça vardır ve her parça sonra tek bir sonuçta birleştirilmiş belge sayıma göre üst N modelleri bildirir. Başkalarının daha az olsa bazı parçalar çok eşleşen değerleri, varsa, bazı modeli değerleri eksik veya altında sonuçlarda sayılan bulabilirsiniz.

Bu davranış bugün karşılaşırsanız Bu davranış herhangi bir zamanda değişebilir olsa da, etrafında sayısı inflating sınırlarına göre çalışabilirsiniz:<number> tam her parçadan veri raporlama zorlamak için büyük bir sayı. Varsa sayısı değeri: değerinden büyükse veya doğru sonuçlar garantilenen alanında benzersiz değerlerin sayısını eşittir. Ancak, belge sayısını yüksek olduğunda bir performans cezası olmaz, bu seçeneği dikkatli kullanın.

### <a name="user-interface-tips"></a>Kullanıcı arabirimi ipuçları
**Model Gezinti bölmesinde her bir alan için etiketler ekleyin**

Etiketler genellikle HTML veya form içinde tanımlanan (`index.cshtml` örnek uygulamada). Model Gezinti etiketleri için Azure Search API yok veya diğer meta verileri yok.

<a name="rangefacets"></a>

## <a name="filter-based-on-a-range"></a>Bir aralığı dayanan filtre
Modelleme değerleri aralığı içinde bir genel arama uygulama gereksinimdir. Aralıkları, sayısal ve tarih/saat değerleri için desteklenir. Daha fazla bilgi edinebilirsiniz her bir yaklaşıma hakkında [Search belgeleri (Azure Search API)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).

Azure arama, bir aralık bilgi işlem için iki yaklaşım sağlayarak aralığı oluşturma basitleştirir. Azure Search, her iki yaklaşım için sağladığınız girişlere uygun aralıkların oluşturur. Örneğin, aralığı 10 değerini belirtirseniz | 20 | 30, otomatik olarak oluşturduğu aralığı 0-10, 20 10, 20-30. Uygulamanızı isteğe bağlı olarak boş bir aralık kaldırabilirsiniz. 

**Yaklaşım 1: Aralığı parametresini kullanın**  
10 ABD Doları artışlarla fiyat özellikleri ayarlamak için şunu belirtmeniz gerekir: `&facet=price,interval:10`

**Yaklaşım 2: Değerler listesini kullanın**  
Sayısal veriler için değerler listesini kullanabilirsiniz.  Model aralığının göz önünde bir `listPrice` alan, şu şekilde oluşturulur:

  ![Örnek değerler listesi][5]

Önceki ekran görüntüsüne benzer bir modeli aralığını belirtmek için değerler listesini kullanın:

    facet=listPrice,values:10|25|100|500|1000|2500

Her aralık, başlangıç noktası, bir uç nokta olarak listeden bir değer olarak 0'ı kullanarak ve sonra ayrık aralıklarını oluşturmak için önceki aralığı kırpılır oluşturulmuştur. Azure arama, çok yönlü gezinme bir parçası olarak bunları yapar. Yapılandırma her aralık için kod yazmak zorunda değildir.

### <a name="build-a-filter-for-a-range"></a>Bir aralık için bir filtre oluşturun
Seçtiğiniz bir aralığı tabanlı belge filtrelemek için kullanabileceğiniz `"ge"` ve `"lt"` filtre işleçleri aralığın uç noktalarını tanımlayan iki parçalı ifadesinde. Örneğin, aralığı 10-25 seçerseniz bir `listPrice` alanın filtre olacak `$filter=listPrice ge 10 and listPrice lt 25`. Örnek kodda, filtre ifadesi kullanan **priceFrom** ve **priceTo** uç noktaları ayarlamak için parametreleri. 

  ![Değer aralığı için sorgu][6]

<a name="geofacets"></a> 

## <a name="filter-based-on-distance"></a>Hakkında daha fazla mesafe dayanan filtre
Depolama, Restoran ve kendi yakınlık geçerli konumunuza göre hedef seçtiğiniz yardımcı görmek için ortak filtreler. Bu filtre türünü, çok yönlü gezinme gibi görünebilir, ancak bunu yalnızca bir filtredir. Biz burada olanlar için özel olarak uygulama önerileri için bu belirli tasarım sorunu arayan bahsedebilirsiniz.

Azure Search'te iki Jeo-uzamsal işlevleri vardır **geo.distance** ve **geo.intersects**.

* **Geo.distance** işlevi iki nokta mesafeyi içinde uzaklık döndürür. Bir noktada bir alandır ve diğer filtre bir parçası olarak geçirilen bir sabit değerdir. 
* **Geo.intersects** işlevi belirli bir noktaya içinde belirli bir Çokgen ise true döndürür. Bir alan noktasıdır ve Çokgen sabit liste filtresinin bir bölümü olarak geçirilen koordinatları olarak belirtilir.

Filtre örnekler bulabilirsiniz [OData ifadesi söz dizimi (Azure Search)](query-odata-filter-orderby-syntax.md).

<a name="tryitout"></a>

## <a name="try-the-demo"></a>Demoyu deneyin
Azure arama iş Portal Tanıtımı, bu makalede bahsedilen örnekler içerir.

-   Görebilir ve test sırasında çevrimiçi çalışma Tanıtımı [Azure arama iş Portal Tanıtımı](https://azjobsdemo.azurewebsites.net/).

-   Kodu indir [github'daki Azure örnekleri deposu](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs).

Arama sonuçları ile çalışırken, sorgu oluşturma değişiklikleri URL'sini izleyin. Bu uygulama, her birini seçtiğinizde modelleri URİ'ye ekler için gerçekleşir.

1. Tanıtım uygulamasını eşleme işlevselliğini kullanmak için bir Bing Haritalar anahtarını alma [Bing Haritalar geliştirme Merkezi'nde](https://www.bingmapsportal.com/). Mevcut anahtarı yapıştırın `index.cshtml` sayfası. `BingApiKey` Ayarı `Web.config` dosya kullanılmaz. 

2. Uygulamayı çalıştırın. İsteğe bağlı bir tura katılın veya iletişim kutusunu kapatın.
   
3. "Analist" gibi bir arama terimi girin ve arama simgesine tıklayın. Sorgu hızlı bir şekilde yürütür.
   
   Çok yönlü gezinme yapısı ile arama sonuçları da döndürülür. Arama sonuçları sayfasında, çok yönlü gezinme yapısına her modeli sonuç sayısını içerir. Modeli seçilir ve bu nedenle tüm eşleşen sonuç döndürülür.
   
   ![Modelleri seçmeden önce arama sonuçları][11]

4. Bir iş başlık, konum veya en düşük ücret tıklayın. Modeller ilk arama null, ancak değerlerine göre aldıkları gibi arama sonuçlarını artık eşleşen öğeleri atılır.
   
   ![Modelleri seçtikten sonra arama sonuçları][12]

5. Böylece farklı sorgu davranışları yapabileceğiniz çok yönlü sorgu temizlemek için tıklatın `[X]` özellikleri temizlemek için seçilen modelleri sonra.
   
<a name="nextstep"></a>

## <a name="learn-more"></a>Daha fazla bilgi edinin
İzleme [Azure Search'ü yakından](https://channel9.msdn.com/Events/TechEd/Europe/2014/DBI-B410). 45:25 modelleri uygulama konusunda bir demo yoktur.

Çok yönlü gezinme için tasarım ilkeleri hakkında daha fazla öngörü için aşağıdaki bağlantıları öneririz:

* [Çok yönlü arama için tasarlama](http://www.uie.com/articles/faceted_search/)
* [Tasarım desenleri: Çok yönlü navigasyon](https://alistapart.com/article/design-patterns-faceted-navigation)


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
[Design Patterns: Faceted Navigation]: https://alistapart.com/article/design-patterns-faceted-navigation
[Create your first application]: search-create-first-solution.md
[OData expression syntax (Azure Search)]: https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search
[Azure Search Adventure Works Demo]: https://azuresearchadventureworksdemo.codeplex.com/
[https://www.odata.org/documentation/odata-version-2-0/overview/]: https://www.odata.org/documentation/odata-version-2-0/overview/ 
[Faceting on Azure Search forum post]: ../faceting-on-azure-search.md?forum=azuresearch
[Search Documents (Azure Search API)]: https://docs.microsoft.com/rest/api/searchservice/Search-Documents

