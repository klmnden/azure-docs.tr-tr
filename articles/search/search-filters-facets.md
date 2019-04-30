---
title: Model filtreleri arama Gezinti uygulamalar - Azure Search için
description: Filtre ölçütü kullanıcı güvenlik kimliği, coğrafi konum ya da sorgular, Azure arama, Microsoft Azure üzerinde barındırılan bulut arama hizmeti üzerinde arama sonuçları azaltmak için sayısal değerler.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 10/13/2017
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 8793f6f4d135d6099541d24aa5f5cfc0b6c21b30
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61290066"
---
# <a name="how-to-build-a-facet-filter-in-azure-search"></a>Azure Search'te bir cephe filtresi oluşturma 

Çok yönlü gezinme, kendinden yönlendirmeli Burada, uygulamanızın kullanıcı Arabirimi denetimleri için kapsam belirleme arama belgeleri (örneğin, kategorileri veya markaları) gruplarına sunar ve Azure arama, yedekleme için veri yapısı sağlar bir arama uygulamasında, sorgu sonuçları üzerinde filtreleme için kullanılır deneyimi. Bu makalede, bir çok yönlü gezinme yapısı arama deneyimi sağlamak için istediğiniz yedekleme oluşturmak için temel adımlar hızlı bir şekilde gözden geçirin. 

> [!div class="checklist"]
> * Filtreleme ve çok yönlü aramaya alanları seçin
> * Alandaki öznitelikler kümesi
> * Dizin ve yük verileri oluşturma
> * Model filtreleri bir sorguya ekleyin
> * Sonuçları işleme

Dinamik ve sorguda döndürülen modeller. Araması yanıtlarında, kendileriyle sonuçları gezinmek için kullanılan model kategorileri getirin. Modelleri ile ilgili bilgi sahibi değilseniz, aşağıdaki örnek model gezinti yapısında temsilidir.

  ![](./media/search-filters-facets/facet-nav.png)

Yeni çok yönlü gezinme ve isterseniz daha ayrıntılı bilgi? Bkz: [Azure Arama'da çok yönlü navigasyon uygulamak nasıl](search-faceted-navigation.md).

## <a name="choose-fields"></a>Alanları seçin

Modelleri koleksiyonları yanı sıra tek değer alanlar hesaplanır. En çok yönlü gezinme çalışma alanlarına sahip düşük önem düzeyi: çok az sayıda, arama topluluğunuza (örneğin, renkleri, ülke veya marka adları listesi) belgelerde boyunca yineleyin farklı değerleri. 

Model oluşturma aşağıdaki öznitelikleri TRUE olarak ayarlayarak dizini oluştururken bir alanı alanlı temelinde etkin: `filterable`, `facetable`. Yalnızca filtrelenebilir alanlardan görünüm oluşturulabilir.

Tüm [alan türü](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) , büyük olasılıkla kullanılabilen çok yönlü gezinme, "modellenebilir" işaretlenir:

+ Edm.String
+ Edm.DateTimeOffset
+ Edm.Boolean
+ Edm.Collections
+ Sayısal alan türleri: EDM.Int32, EDM.Int64, Edm.Double

Çok yönlü gezinme Edm.GeographyPoint kullanamazsınız. Modelleri, insan tarafından okunabilir metin veya sayı oluşturulur. Bu nedenle, modelleri için coğrafi koordinatları desteklenmez. Konuma göre modeli bir şehir veya bölge alan gerekir.

## <a name="set-attributes"></a>Öznitelikleri Ayarla

Bir alanın nasıl kullanıldığını kontrol dizin öznitelikleri dizindeki tek alan tanımları eklenir. Aşağıdaki örnekte, düşük önem düzeyi, model oluşturma için faydalı alanlarla oluşur: Kategori (otel, motel, hostel) kullanılmıyordu ve derecelendirmeleri. 

.NET API filtre öznitelikleri açıkça ayarlanmış olması gerekir. REST API, çok yönlü aramaya ve filtreleme, kapatmak istediğinizde öznitelikleri açıkça ayarlamak yalnızca ihtiyacınız olduğu anlamına gelir varsayılan olarak etkinleştirilir. Teknik olarak gerekli olmamasına karşın, atıfları REST örnekte açıklayıcı amacıyla göstereceğiz. 

> [!Tip]
> Performans ve depolama iyileştirme için en iyi uygulama olarak model oluşturma hiçbir zaman bir model olarak kullanılması gereken alanlar için devre dışı. Özellikle, "Modellenebilir" için dize alanları kimliği veya ürün adı gibi tek değerler için ayarlanmalıdır: önlemek için false, yanlışlıkla (ve verimsiz) çok yönlü gezinme kullanın.


```http
{
    "name": "hotels",  
    "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String", "facetable": false},
        {"name": "category", "type": "Edm.String", "filterable": true, "facetable": true},
        {"name": "tags", "type": "Collection(Edm.String)", "filterable": true, "facetable": true},
        {"name": "parkingIncluded", "type": "Edm.Boolean",  "filterable": true, "facetable": true, "sortable": false},
        {"name": "smokingAllowed", "type": "Edm.Boolean", "filterable": true, "facetable": true, "sortable": false},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32", "filterable": true, "facetable": true},
        {"name": "location", "type": "Edm.GeographyPoint"}
    ]
}
```

> [!Note]
> Bu dizin tanımını kopyalanır [REST API kullanarak Azure Search dizini oluşturma](https://docs.microsoft.com/azure/search/search-create-index-rest-api). Alan tanımları yüzeysel farklılıkları dışında aynıdır. Filtrelenebilir ve modellenebilir öznitelikler, kategori, etiketler, parkingIncluded, smokingAllowed ve derecelendirme alanları üzerinde açıkça eklenir. Uygulamada, modellenebilir için ücretsiz Edm.String Edm.Boolean ve EDM.Int32 alan türlerini ve filtrelenebilir alın. 

## <a name="build-and-load-an-index"></a>Derleme ve dizin yükleme

Ara (ve belki de belirgin) bir adım için sahip olduğu [oluşturun ve dizini doldurma](https://docs.microsoft.com/azure/search/search-create-index-dotnet#3---construct-index) sorgu formulating önce. Biz bu adımı burada bütünlük bahsedebilirsiniz. Dizin kullanılabilir olup olmadığını belirlemenin bir yolu olan dizinler listesi işaretleyerek [portalı](https://portal.azure.com).

## <a name="add-facet-filters-to-a-query"></a>Model filtreleri bir sorguya ekleyin

Uygulama kodunda tüm parçalarını arama ifadeleri, modelleri, Puanlama profillerini – herhangi bir istek düzenleyin için kullanılan filtreler dahil olmak üzere, geçerli bir sorgu belirten bir sorgu oluşturun. Aşağıdaki örnek, Konaklama, derecelendirme ve diğer kullanılmıyordu türüne göre modeli Gezinti oluşturan bir istek oluşturur.

```csharp
SearchParameters sp = new SearchParameters()
{
  ...
  // Add facets
  Facets = new List<String>() { "category", "rating", "parkingIncluded", "smokingAllowed" },
};
```

### <a name="return-filtered-results-on-click-events"></a>Filtrelenmiş sonuçlar üzerinde tıklama olayları

Filtre ifadesi modeli değeri click olayını işler. Bir kategori modeli göz önünde bulundurulduğunda, tıklayarak "motel" kategorisi aracılığıyla uygulanır bir `$filter` ifade türü Konaklama seçer. Bir kullanıcı yalnızca motels gösterilen olduğunu belirtmek için "motels" tıkladığında uygulamanın gönderdiği sonraki sorgu, $filter içerir. Kategori eq 'motels' =.

Aşağıdaki kod parçacığı, kullanıcı kategorisi modeli bir değer seçerse filtre kategorisi ekler.

```csharp
if (categoryFacet != "")
  filter = "category eq '" + categoryFacet + "'";
```
REST API kullanarak, istek olarak geliştirilmiştir `$filter=category eq 'c1'`. Birden çok değerli alan kategoriyi yapmak için aşağıdaki sözdizimini kullanın: `$filter=category/any(c: c eq 'c1')`

## <a name="tips-and-workarounds"></a>İpuçları ve geçici çözümler

### <a name="initialize-a-page-with-facets-in-place"></a>Yerinde özellikleri içeren bir sayfa Başlat

Yerinde özellikleri içeren bir sayfa başlatmak istiyorsanız, ilk model yapısı sayfası oluşturmak için sayfa başlatma işleminin bir parçası olarak bir sorgu gönderebilirsiniz.

### <a name="preserve-a-facet-navigation-structure-asynchronously-of-filtered-results"></a>Filtrelenmiş sonuçlarını zaman uyumsuz olarak model gezinti yapısında koru

Azure Search'te modeli Gezinti zorluklar modelleri yalnızca geçerli sonuçları mevcut biridir. Uygulamada, kullanıcı, geriye doğru arama içeriği alternatif yollarını keşfetmek için adımları retracing gidebilmeniz statik bir modeller kümesi korumak için yaygındır. 

Bu yaygın bir kullanım örneği olsa da, bunu bir şey değil model gezinti yapısında şu anda kullanıma hazır sağlar. Genellikle statik modelleri isteyen geliştiriciler iki filtrelenmiş sorgu yayınlayarak sınırlamayı çalışma: bir sonuçları kapsamlı, diğer statik Gezinti amacıyla modellerin listesini oluşturmak için kullanılır.

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Search'te filtreler](search-filters.md)
+ [Dizin REST API'si oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index)
+ [Search belgeleri REST API'si](https://docs.microsoft.com/rest/api/searchservice/search-documents)
