---
title: Azure Search'te modeli filtreleri | Microsoft Docs
description: "Filtre ölçütü kullanıcının güvenlik kimliği, dil, coğrafi konuma veya Azure arama, Microsoft Azure üzerinde barındırılan bulut arama hizmeti sorgularda arama sonuçları azaltmak için sayısal değerleri."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
ms.assetid: 
ms.service: search
ms.devlang: 
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/13/2017
ms.author: heidist
ms.openlocfilehash: 3480fbbecf59fe985103fe39ec27fef2668b3c0a
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="how-to-build-a-facet-filter-in-azure-search"></a>Azure Search'te bir model filtresi oluşturma 

Çok yönlü gezinme, kendi kendine yönlendirilmiş Burada, uygulamanız (örneğin, kategori veya markalar) belgelerin gruplarına kapsam arama için kullanıcı Arabirimi denetimlerini sunar ve Azure Search sağlar veri yapısı geri arama uygulamasında, sorgu sonuçlarını filtreleme için kullanılır deneyimi. Bu makalede, hızlı bir şekilde sağlamak istediğiniz arama deneyimi yedekleme modellenmiş bir gezinmede yapısı oluşturmak için temel adımları gözden geçirin. 

> [!div class="checklist"]
> * Filtreleme ve olduğunu için alanları seçin
> * Alan özelliklerini ayarla
> * Dizin ve yük verileri oluşturma
> * Model filtreleri sorguya ekleme
> * Sonuçlarını işleme

Dinamik ve bir sorgu üzerinde döndürülen modeller. Arama yanıtları bunlarla sonuçları gezinmek için kullanılan model kategorileri duruma getirin. Modelleri ile alışık değilseniz, aşağıdaki örnekte bir model gezinti yapısında çizimi ' dir.

  ![](./media/search-filters/facet-nav.png)

Yeni çok yönlü gezinme ve istediğiniz daha ayrıntılı bilgi? Bkz: [modellenmiş bir gezinmede Azure Search'te uygulamak nasıl](search-faceted-navigation.md).

## <a name="choose-fields"></a>Alanları seçin

Modelleri koleksiyonları yanı sıra tek değer alanları hesaplanabilir. En iyi modellenmiş Gezinti bölmesinde çalışma alanlarına sahip düşük önem düzeyi: belgeleri, arama gövde (örneğin, bir listesi renkler, ülke veya marka) boyunca yineleyin farklı değerleri küçük sayısı. 

Olduğunu, bir alan alanını temelinde etkinleştirilmişse, TRUE olarak aşağıdaki öznitelikler ayarlayarak dizin oluşturduğunuzda: `filterable`, `facetable`. Yalnızca filtrelenebilir alanlardan görünüm oluşturulabilir.

Tüm [alan türü](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) , büyük olasılıkla kullanılabilirdi içinde modellenmiş bir gezinmede "modellenebilir" işaretlenmiş:

+ Edm.String
+ Edm.DateTimeOffset
+ Edm.Boolean
+ Edm.Collections
+ Sayısal alan türleri: EDM.Int32, EDM.Int64, Edm.Double

Modellenmiş bir gezinmede Edm.GeographyPoint kullanamazsınız. Modelleri İnsan okunabilir metin veya sayı oluşturulur. Bu nedenle, modelleri coğrafi koordinatlarına için desteklenmez. Konuma göre modeli için bir şehir veya bölge alanı gerekir.

## <a name="set-attributes"></a>Özelliklerini ayarla

Bir alanın nasıl kullanıldığını kontrol dizin öznitelikleri, dizinde tek alan tanımları eklenir. Aşağıdaki örnekte, düşük önem düzeyi, olduğunu için yararlı olan alanlar oluşur: kategorisi (otel, motel, hostel), s ve derecelendirmeleri. 

.NET API filtreleme öznitelikleri açıkça ayarlanmış olması gerekir. REST API, olduğunu ve filtreleme, yalnızca kapatmak istediğinizde özniteliklerini açıkça ayarlamak ihtiyaç duyacağınız anlamına gelir varsayılan olarak etkinleştirilir. Teknik olarak gerekli olmamasına karşın, nitelikleri aşağıdaki REST örnekte açıklayıcı amacıyla gösteriyoruz. 

> [!Tip]
> Performans ve depolama iyileştirme için en iyi uygulama olarak olduğunu hiçbir zaman bir model kullanılması gereken alanlar için kapatın. Özellikle, dize alanları bir kimliği veya ürün adı gibi singleton değerleri için "Modellenebilir" olarak ayarlanmalıdır: önlemek için false kendi yanlışlıkla (ve etkisiz) modellenmiş bir gezinmede kullanın.


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
> Bu dizin tanımı kopyalanır [REST API kullanarak Azure Search dizini oluşturma](https://docs.microsoft.com/azure/search/search-create-index-rest-api). Alan tanımları yüzeysel farklılıklar dışında aynıdır. Filtrelenebilir ve modellenebilir öznitelikleri, kategori, etiketler, parkingIncluded, smokingAllowed ve derecelendirme alanları açıkça eklenir. Uygulamada, filtrelenebilir almak ve modellenebilir için Edm.String, Edm.Boolean ve EDM.Int32 alan türlerine boş. 

## <a name="build-and-load-an-index"></a>Derleme ve dizin yükleme

Bir ara (ve muhtemelen belirgin) için sahip adımdır [oluşturmak ve dizinini doldurmak](https://docs.microsoft.com/azure/search/search-create-index-dotnet#create-the-index) bir sorgu formulating önce. Bu adım burada eksiksiz olması için ayarlardan söz eder. Dizin kullanılabilir olup olmadığını belirlemek için bir yoldur dizinler listesi denetleyerek [portal](https://portal.azure.com).

## <a name="add-facet-filters-to-a-query"></a>Model filtreleri sorguya ekleme

Uygulama kodunda arama ifadeleri, modelleri, profilleri – formüle bir istek için kullanılan herhangi bir şey Puanlama filtreleri de dahil olmak üzere geçerli bir sorgu tüm parçalarını belirten bir sorgu oluşturun. Aşağıdaki örnek Konaklama, derecelendirme ve diğer s türüne göre model Gezinti oluşturur isteğin oluşturur.

```csharp
SearchParameters sp = new SearchParameters()
{
  ...
  // Add facets
  Facets = new List<String>() { "category", "rating", "parkingIncluded", "smokingAllowed" },
};
```

### <a name="return-filtered-results-on-click-events"></a>Dönüş filtrelenmiş sonuçlar tıklama olayları

Filtre ifadesi model değeri click olayını işler. Bir kategori modeli verildiğinde, "motel" kategorisi tıklatarak aracılığıyla uygulanır bir `$filter` türü konaklamaları seçer ifade. Bir kullanıcı yalnızca motels gösterilen olduğunu belirtmek için "motels" tıkladığında, uygulamanın gönderdiği sonraki sorgu $filter içeren kategori eq 'motels' =.

Kullanıcı kategorisi modelin bir değer seçerse aşağıdaki kod parçacığını kategori filtresi ekler.

```csharp
if (categoryFacet != "")
  filter = "category eq '" + categoryFacet + "'";
```
REST API kullanarak, istek olarak geliştirilmiştir `$filter=category eq 'c1'`. Kategoriyi birden çok değerli alan yapmak için aşağıdaki sözdizimini kullanın:`$filter=category/any(c: c eq 'c1')`

## <a name="tips-and-workarounds"></a>İpuçları ve geçici çözümler

### <a name="initialize-a-page-with-facets-in-place"></a>Bir sayfa yerinde modellerle başlatma

Modellerle yerinde bir sayfayı başlatmak istiyorsanız, ilk model yapısını sayfasıyla oluşturmak için sayfa başlatma bir parçası olarak bir sorgu gönderebilirsiniz.

### <a name="preserve-a-facet-navigation-structure-asynchronously-of-filtered-results"></a>Model gezinti yapısında filtrelenmiş sonuçlarını zaman uyumsuz olarak koru

Azure Search'te model Gezinti zorluklar modelleri yalnızca geçerli sonuçlarını mevcut biridir. Uygulamada, kullanıcı, geriye doğru arama içerik alternatif yollarını keşfetmek için adımları retracing böylece modelleri statik bir dizi korumak için yaygın bir sorundur. 

Bu genel bir kullanım örneği olsa da, bunu bir şey değil model gezinti yapısında şu anda Giden kutusu sağlar. Genellikle statik modelleri isteyen geliştiriciler iki filtrelenmiş sorguları vererek sınırlamaya geçici çalışma: biri sonuçları kapsamlı, diğer statik Gezinti amacıyla modelleri listesini oluşturmak için kullanılır.

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Search'te filtreleri](search-filters.md)
+ [Dizin REST API'si oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index)
+ [REST API belgelerde arama](https://docs.microsoft.com/rest/api/searchservice/search-documents)

