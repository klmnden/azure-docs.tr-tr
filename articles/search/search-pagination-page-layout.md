---
title: Arama sonuçları - Azure Search ile çalışmaya nasıl
description: Yapı ve arama sonuçlarını sıralamasını, belge sayısı alın ve Azure Search'te arama sonuçlarına içerik gezinti ekleyin.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: ''
ms.topic: conceptual
ms.date: 06/13/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 73f0dc98d7d2c3e7aa77f6414cbd58e58599eae7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67068822"
---
# <a name="how-to-work-with-search-results-in-azure-search"></a>Azure Search'te sonuçlarını arama ile çalışma
Bu makalede, standart arama sonuçları sayfası, toplam sayısı, belge alma, sıralama düzenleri ve gezinti gibi öğeleri ekleme hakkında yönergeler sağlanır. Veri veya arama sonuçlarındaki bilgileri katkıda sayfasıyla ilgili seçenekleri aracılığıyla belirtilen [belgede arama](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) Azure arama hizmetinize gönderilen istekler. 

REST API istekleri GET komutu, yol ve hangi istenen hizmet bildirmek sorgu parametreleri ve yanıt formüle etmek nasıl içerir. .NET SDK'da eşdeğer API'dir [DocumentSearchResult sınıfı](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.documentsearchresult-1).

Birkaç kod örneği, burada bulduğunuz bir web ön uç arabirimi şunlardır: [New York City işleri demo app](https://azjobsdemo.azurewebsites.net/) ve [CognitiveSearchFrontEnd](https://github.com/LuisCabrer/CognitiveSearchFrontEnd).

> [!NOTE]
> Geçerli bir isteği bir hizmet URL'sini ve yolu, HTTP fiili gibi öğeleri içerir `api-version`ve benzeri. Konuyu uzatmamak amacıyla, biz yalnızca için sayfalandırma ilgili söz dizimi vurgulama için örnekleri kırpılır. İstek sözdizimi hakkında daha fazla bilgi için bkz. [Azure arama hizmeti REST](https://docs.microsoft.com/rest/api/searchservice).
>

## <a name="total-hits-and-page-counts"></a>Toplam İsabet sayısı ve sayfa sayıları

Bir sorgudan döndürülen sonuçların toplam sayısını gösteren ve ardından daha küçük öbekler halinde sonuçları getireceğinden neredeyse tüm arama sayfalar için temel.

![][1]

Azure Search'te kullandığınız `$count`, `$top`, ve `$skip` bu değerleri döndürmek için parametreleri. Aşağıdaki örnekte gösterildiği bir örnek istek Toplam İsabet "çevrimiçi-katalog" adlı bir dizin döndürülen olarak `@odata.count`:

    GET /indexes/online-catalog/docs?$count=true

15 gruplardaki belgelerini alın ve ayrıca ilk sayfasına Başlangıç Toplam İsabet sayısı göster:

    GET /indexes/online-catalog/docs?search=*$top=15&$skip=0&$count=true

Sonuçları sayfalandırmayı, gerektiren her ikisi de `$top` ve `$skip`burada `$top` bir toplu işlemde döndürülecek ne kadar öğenin belirtir ve `$skip` atlamak için ne kadar öğenin belirtir. Aşağıdaki örnekte, her sayfa sonraki 15 öğeleri gösterir, artımlı atlar belirttiği `$skip` parametresi.

    GET /indexes/online-catalog/docs?search=*$top=15&$skip=0&$count=true

    GET /indexes/online-catalog/docs?search=*$top=15&$skip=15&$count=true

    GET /indexes/online-catalog/docs?search=*$top=15&$skip=30&$count=true

## <a name="layout"></a>Düzen

Arama sonuçları sayfasında, bir küçük resim, alanların bir alt kümesini ve tam ürün sayfasının bağlantısı göstermek isteyebilirsiniz.

 ![][2]

Azure Search'te, kullanacağınız `$select` ve [arama API'si isteği](https://docs.microsoft.com/rest/api/searchservice/search-documents) bu deneyim uygulamak için.

Düzeni döşenmiş için alanların bir alt kümesini döndürmek için:

    GET /indexes/online-catalog/docs?search=*&$select=productName,imageFile,description,price,rating

Görüntüleri ve medya dosyalarını doğrudan aranabilir değil ve maliyetleri azaltmak için Azure Blob Depolama gibi başka bir depolama platformu depolanması gerekir. Dizin ve belgeler, dış içerik URL adresini depolayan bir alan tanımlayın. Ardından, alan bir görüntü başvurusu olarak kullanabilirsiniz. Görüntünün URL'si belge içinde olmalıdır.

Ürün açıklaması sayfası için alınacak bir **onClick** kullanım olayı [arama belge](https://docs.microsoft.com/rest/api/searchservice/Lookup-Document) almak için bir belge anahtarında geçirilecek. Anahtarın veri türü `Edm.String`. Bu örnekte olduğu *246810*.

    GET /indexes/online-catalog/docs/246810

## <a name="sort-by-relevance-rating-or-price"></a>İlgi düzeyi, derecelendirme ve fiyat göre sırala

Genellikle ilgi düzeyi için varsayılan sıralama sıralar, ancak böylece müşterilerin hızlı bir şekilde mevcut sonuçları farklı bir derece sıra sırasını yeniden ayarlaması alternatif sıralama siparişler kullanıma hazır hale getirmek için yaygındır.

 ![][3]

Azure Search'te sıralama dayanır `$orderby` ifadesi olarak dizine alınmış tüm alanları `"Sortable": true.` bir `$orderby` yan tümcesi bir OData ifadedir. Sözdizimi hakkında daha fazla bilgi için bkz. [OData ifadesi söz dizimi filtreleri ve order by yan tümcesi için](query-odata-filter-orderby-syntax.md).

İlgi kesin Puanlama profilleri ile ilişkilendirilmiş olması gerekir. Varsayılan Puanlama, metin analizi ve istatistikleri kullanır belgeleri ile daha fazla veya daha güçlü eşleşme üzerinde bir arama terimi giderek daha yüksek puanları ile tüm sonuçları, sipariş derecelendirmesini kullanabilirsiniz.

Alternatif sıralama düzenleri, genellikle ilişkilendirilen **onClick** sıralama düzenini oluşturan bir yöntem için geri çağırma olayları. Örneğin, bu sayfa öğesi verilen:

 ![][4]

Seçili sıralama seçeneği girdi olarak kabul eder ve bu seçeneği ile ilişkili ölçütler için sıralı bir listesi döndüren bir yöntem oluşturursunuz.

 ![][5]

> [!NOTE]
> Varsayılan Puanlama birçok senaryo için yeterli olmakla birlikte, bunun yerine özel bir Puanlama profili ilgi dayandırmamaya özen öneririz. Özel bir Puanlama profili, işletmeniz için daha yararlı olan boost öğeleri için bir yol sunar. Bkz: [Puanlama profillerini Ekle](index-add-scoring-profiles.md) daha fazla bilgi için.
>

## <a name="faceted-navigation"></a>Çok yönlü gezinme

Gezintide Ara genellikle yan veya bir sayfanın üst kısmında bulunan sonuçları sayfasında, yaygın bir durumdur. Azure Search'te, bağımsız aramayı önceden tanımlanmış filtreleri temel alarak çok yönlü gezinme sağlar. Bkz: [Azure Arama'da çok yönlü gezinme](search-faceted-navigation.md) Ayrıntılar için.

## <a name="filters-at-the-page-level"></a>Sayfa düzeyi filtreleri

Çözüm tasarımınızın belirli türlerdeki içerik (örneğin, sayfanın başında listelenen bölümleri olan bir çevrimiçi satış uygulama) için ayrılmış arama sayfaları dahil, ekleyebilirsiniz bir [filtre ifadesi](search-filters.md) bir yanısıra**onClick** olay önceden filtrelenmiş bir durumda bir sayfasını açın.

Bir filtre veya arama ifadesi kaydetmeden gönderebilirsiniz. Örneğin, aşağıdaki isteği, eşleşen belgeleri döndüren marka adı üzerinde filtre uygular.

    GET /indexes/online-catalog/docs?$filter=brandname eq 'Microsoft' and category eq 'Games'

Bkz: [Search belgeleri (Azure Search API)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) hakkında daha fazla bilgi için `$filter` ifadeler.

## <a name="see-also"></a>Ayrıca Bkz.

- [Azure arama hizmeti REST API'si](https://docs.microsoft.com/rest/api/searchservice)
- [Dizin işlemleri](https://docs.microsoft.com/rest/api/searchservice/Index-operations)
- [Belge işlemlerini](https://docs.microsoft.com/rest/api/searchservice/Document-operations)
- [Azure Arama'da çok yönlü navigasyon](search-faceted-navigation.md)

<!--Image references-->
[1]: ./media/search-pagination-page-layout/Pages-1-Viewing1ofNResults.PNG
[2]: ./media/search-pagination-page-layout/Pages-2-Tiled.PNG
[3]: ./media/search-pagination-page-layout/Pages-3-SortBy.png
[4]: ./media/search-pagination-page-layout/Pages-4-SortbyRelevance.png
[5]: ./media/search-pagination-page-layout/Pages-5-BuildSort.png
