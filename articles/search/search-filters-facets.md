---
title: Model filtreleri arama Gezinti uygulamalar - Azure Search için
description: Filtre ölçütü kullanıcı güvenlik kimliği, coğrafi konum ya da sorgular, Azure arama, Microsoft Azure üzerinde barındırılan bulut arama hizmeti üzerinde arama sonuçları azaltmak için sayısal değerler.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 5/13/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 8dffc5b87aefe23953d3a74f1d96b5ee03e0315d
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65597380"
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

Modelleri koleksiyonları yanı sıra tek değer alanlar hesaplanır. En çok yönlü gezinme çalışma alanlarına sahip düşük önem düzeyi: çok az sayıda, arama topluluğunuza (örneğin, renkleri, ülkeler/bölgeler veya marka adları listesi) belgelerde boyunca yineleyin farklı değerleri. 

Modelleme ayarlayarak dizini oluştururken bir alanı alanlı temelinde etkin `facetable` özniteliğini `true`. Genellikle, ayrıca ayarlamalısınız `filterable` özniteliğini `true` arama uygulamanız, son kullanıcının seçtiği özellikleri üzerinde bu alanlarda filtre uygulayabilirsiniz böylece gibi alanları için. 

Herhangi bir REST API kullanarak dizini oluştururken [alan türü](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) , büyük olasılıkla kullanılabilen çok yönlü gezinme, olarak işaretlenmiş `facetable` varsayılan olarak:

+ `Edm.String`
+ `Edm.DateTimeOffset`
+ `Edm.Boolean`
+ Sayısal alan türleri: `Edm.Int32`, `Edm.Int64`, `Edm.Double`
+ Yukarıdaki türler (örneğin, `Collection(Edm.String)` veya `Collection(Edm.Double)`)

Kullanamazsınız `Edm.GeographyPoint` veya `Collection(Edm.GeographyPoint)` modellenmiş gezinti alanları. Modelleri, düşük kardinaliteyle alanlarda en iyi çalışır. Coğrafi koordinatları çözümlemesi nedeniyle, nadir herhangi iki ortak ordinates sağlanan bir veri kümesinde eşit olacaktır. Bu nedenle, modelleri için coğrafi koordinatları desteklenmez. Konuma göre modeli bir şehir veya bölge alan gerekir.

## <a name="set-attributes"></a>Öznitelikleri Ayarla

Bir alanın nasıl kullanıldığını kontrol dizin öznitelikleri dizindeki tek alan tanımları eklenir. Aşağıdaki örnekte, düşük önem düzeyi, model oluşturma için faydalı alanlarla oluşur: `category` (otel, motel, hostel) `tags`, ve `rating`. Bu alanların `filterable` ve `facetable` öznitelikler aşağıdaki örnekte açıkça kümesi için yalnızca tanım amaçlıdır. 

> [!Tip]
> Performans ve depolama iyileştirme için en iyi uygulama olarak model oluşturma hiçbir zaman bir model olarak kullanılması gereken alanlar için devre dışı. Dize alanları için kimliği veya ürün adı gibi benzersiz değerleri özellikle ayarlanmalıdır `"facetable": false` çok yönlü gezinme yanlışlıkla (ve verimsiz) kullanımları önlemek için.


```json
{
  "name": "hotels",  
  "fields": [
    { "name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false },
    { "name": "baseRate", "type": "Edm.Double" },
    { "name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false },
    { "name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene" },
    { "name": "hotelName", "type": "Edm.String", "facetable": false },
    { "name": "category", "type": "Edm.String", "filterable": true, "facetable": true },
    { "name": "tags", "type": "Collection(Edm.String)", "filterable": true, "facetable": true },
    { "name": "parkingIncluded", "type": "Edm.Boolean",  "filterable": true, "facetable": true, "sortable": false },
    { "name": "smokingAllowed", "type": "Edm.Boolean", "filterable": true, "facetable": true, "sortable": false },
    { "name": "lastRenovationDate", "type": "Edm.DateTimeOffset" },
    { "name": "rating", "type": "Edm.Int32", "filterable": true, "facetable": true },
    { "name": "location", "type": "Edm.GeographyPoint" }
  ]
}
```

> [!Note]
> Bu dizin tanımını kopyalanır [REST API kullanarak Azure Search dizini oluşturma](https://docs.microsoft.com/azure/search/search-create-index-rest-api). Alan tanımları yüzeysel farklılıkları dışında aynıdır. `filterable` Ve `facetable` öznitelikleri üzerinde açıkça eklenir `category`, `tags`, `parkingIncluded`, `smokingAllowed`, ve `rating` alanları. Uygulamada, `filterable` ve `facetable` bu alanlardaki varsayılan olarak REST API kullanırken etkinleştirilmesi. .NET SDK'sı kullanırken, bu öznitelikler açıkça etkinleştirilmesi gerekir.

## <a name="build-and-load-an-index"></a>Derleme ve dizin yükleme

Ara (ve belki de belirgin) bir adım için sahip olduğu [oluşturun ve dizini doldurma](https://docs.microsoft.com/azure/search/search-create-index-dotnet#3---construct-index) sorgu formulating önce. Biz bu adımı burada bütünlük bahsedebilirsiniz. Dizin kullanılabilir olup olmadığını belirlemenin bir yolu olan dizinler listesi işaretleyerek [portalı](https://portal.azure.com).

## <a name="add-facet-filters-to-a-query"></a>Model filtreleri bir sorguya ekleyin

Uygulama kodunda tüm parçalarını arama ifadeleri, modelleri, Puanlama profillerini – herhangi bir istek düzenleyin için kullanılan filtreler dahil olmak üzere, geçerli bir sorgu belirten bir sorgu oluşturun. Aşağıdaki örnek, Konaklama, derecelendirme ve diğer kullanılmıyordu türüne göre modeli Gezinti oluşturan bir istek oluşturur.

```csharp
var sp = new SearchParameters()
{
    ...
    // Add facets
    Facets = new[] { "category", "rating", "parkingIncluded", "smokingAllowed" }.ToList()
};
```

### <a name="return-filtered-results-on-click-events"></a>Filtrelenmiş sonuçlar üzerinde tıklama olayları

Son kullanıcı bir model değeri tıkladığında, kullanıcının amacını gerçekleştirmek için tıklama olayı işleyicisi bir filtre ifadesi kullanmanız gerekir. Verilen bir `category` unsuruna ile gerçekleştirilir "motel" kategorisi tıklayarak bir `$filter` ifade türü Konaklama seçer. Bir kullanıcı yalnızca motels gösterilen olduğunu belirtmek için "motel" tıkladığında uygulamanın gönderdiği sonraki sorgu içeren `$filter=category eq 'motel'`.

Aşağıdaki kod parçacığı, kullanıcı kategorisi modeli bir değer seçerse filtre kategorisi ekler.

```csharp
if (!String.IsNullOrEmpty(categoryFacet))
    filter = $"category eq '{categoryFacet}'";
```

Kullanıcı bir model değeri gibi bir toplama alan için tıkladığında, `tags`, değer "havuzu" Örneğin, aşağıdaki filtre söz dizimi uygulamanızın kullanması gerekir: `$filter=tags/any(t: t eq 'pool')`

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
