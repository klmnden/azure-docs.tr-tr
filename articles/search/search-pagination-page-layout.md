---
title: Azure Search'te arama sonuçları sayfası nasıl | Microsoft Docs
description: Microsoft Azure üzerinde barındırılan bulut arama hizmeti olan Azure Search sayfalandırma.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 08/29/2016
ms.author: heidist
ms.openlocfilehash: 8953be2be77c14a82294e56ac60b8bc993ec6c2f
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39527078"
---
# <a name="how-to-page-search-results-in-azure-search"></a>Azure Arama'da arama sonuçlarını sayfalandırma
Bu makalede, standart arama sonuçları sayfası, toplam sayısı, belge alma, sıralama düzenleri ve gezinti gibi öğeleri uygulamak için Azure arama hizmeti REST API'si kullanma hakkında yönergeler sağlanır.

Aşağıda belirtilen her durumda, veri veya bilgi arama sonuçları sayfası için katkı sayfasıyla ilgili seçenekleri aracılığıyla belirtilen [belgede arama](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) Azure arama hizmetinize gönderilen istekler. GET komutu, yol ve ne istenen hizmet bildirmek sorgu parametreleri ve nasıl yanıt formüle etmek istekleri içerir.

> [!NOTE]
> Geçerli bir isteği bir hizmet URL'sini ve yolu, HTTP fiili gibi öğeleri içerir `api-version`ve benzeri. Konuyu uzatmamak amacıyla, biz yalnızca için sayfalandırma ilgili söz dizimi vurgulama için örnekleri kırpılır. Lütfen [Azure arama hizmeti REST API'si](https://docs.microsoft.com/rest/api/searchservice) isteği söz dizimi hakkında ayrıntılar için belgeleri.
> 
> 

## <a name="total-hits-and-page-counts"></a>Toplam İsabet sayısı ve sayfa sayıları
Bir sorgudan döndürülen sonuçların toplam sayısını gösteren ve ardından daha küçük öbekler halinde sonuçları getireceğinden neredeyse tüm arama sayfalar için temel.

![][1]

Azure Search'te kullandığınız `$count`, `$top`, ve `$skip` bu değerleri döndürmek için parametreleri. Aşağıdaki örnek, bir örnek istek olarak döndürülen toplam isabet sayısı için gösterir `@OData.count`:

        GET /indexes/onlineCatalog/docs?$count=true

15 gruplardaki belgelerini alın ve ayrıca ilk sayfasına Başlangıç Toplam İsabet sayısı göster:

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

Sonuçları sayfalandırmayı, gerektiren her ikisi de `$top` ve `$skip`burada `$top` bir toplu işlemde döndürülecek ne kadar öğenin belirtir ve `$skip` atlamak için ne kadar öğenin belirtir. Aşağıdaki örnekte, her sayfa sonraki 15 öğeleri gösterir, artımlı atlar belirttiği `$skip` parametresi.

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=15&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=30&$count=true

## <a name="layout"></a>Düzen
Arama sonuçları sayfasında, bir küçük resim, alanların bir alt kümesini ve tam ürün sayfasının bağlantısı göstermek isteyebilirsiniz.

 ![][2]

Azure Search'te, kullanacağınız `$select` ve bu deneyimi uygulamak için bir arama komutu.

Düzeni döşenmiş için alanların bir alt kümesini döndürmek için:

        GET /indexes/ onlineCatalog/docs?search=*&$select=productName,imageFile,description,price,rating 

Görüntüleri ve medya dosyalarını doğrudan aranabilir değil ve maliyetleri azaltmak için Azure Blob Depolama gibi başka bir depolama platformu depolanması gerekir. Dizin ve belgeler, dış içerik URL adresini depolayan bir alan tanımlayın. Ardından, alan bir görüntü başvurusu olarak kullanabilirsiniz. Görüntünün URL'si belge içinde olmalıdır.

Ürün açıklaması sayfası için alınacak bir **onClick** kullanım olayı [arama belge](https://docs.microsoft.com/rest/api/searchservice/Lookup-Document) almak için bir belge anahtarında geçirilecek. Anahtarın veri türü `Edm.String`. Bu örnekte olduğu *246810*. 

        GET /indexes/onlineCatalog/docs/246810

## <a name="sort-by-relevance-rating-or-price"></a>İlgi düzeyi, derecelendirme ve fiyat göre sırala
Genellikle ilgi düzeyi için varsayılan sıralama sıralar, ancak böylece müşterilerin hızlı bir şekilde mevcut sonuçları farklı bir derece sıra sırasını yeniden ayarlaması alternatif sıralama siparişler kullanıma hazır hale getirmek için yaygındır.

 ![][3]

Azure Search'te sıralama dayanır `$orderby` ifadesi olarak dizine alınmış tüm alanları `"Sortable": true.`

İlgi kesin Puanlama profilleri ile ilişkilendirilmiş olması gerekir. Varsayılan Puanlama, metin analizi ve istatistikleri kullanır belgeleri ile daha fazla veya daha güçlü eşleşme üzerinde bir arama terimi giderek daha yüksek puanları ile tüm sonuçları, sipariş derecelendirmesini kullanabilirsiniz.

Alternatif sıralama düzenleri, genellikle ilişkilendirilen **onClick** sıralama düzenini oluşturan bir yöntem için geri çağırma olayları. Örneğin, bu sayfa öğesi verilen:

 ![][4]

Seçili sıralama seçeneği girdi olarak kabul eder ve bu seçeneği ile ilişkili ölçütler için sıralı bir listesi döndüren bir yöntem oluşturursunuz.

 ![][5]

> [!NOTE]
> Varsayılan Puanlama birçok senaryo için yeterli olmakla birlikte, bunun yerine özel bir Puanlama profili ilgi dayandırmamaya özen öneririz. Özel bir Puanlama profili, işletmeniz için daha yararlı olan boost öğeleri için bir yol sunar. Bkz: [Puanlama profili Ekle](https://docs.microsoft.com/rest/api/searchservice/Add-scoring-profiles-to-a-search-index) daha fazla bilgi için. 
> 
> 

## <a name="faceted-navigation"></a>Çok yönlü gezinme
Gezintide Ara genellikle yan veya bir sayfanın üst kısmında bulunan sonuçları sayfasında, yaygın bir durumdur. Azure Search'te, bağımsız aramayı önceden tanımlanmış filtreleri temel alarak çok yönlü gezinme sağlar. Bkz: [Azure Arama'da çok yönlü gezinme](search-faceted-navigation.md) Ayrıntılar için.

## <a name="filters-at-the-page-level"></a>Sayfa düzeyi filtreleri
Çözüm tasarımınızın belirli türlerdeki içerik (örneğin, sayfanın başında listelenen bölümleri olan bir çevrimiçi satış uygulama) için ayrılmış arama sayfaları dahil, bir filtre ifadesi yanı sıra ekleyebilirsiniz bir **onClick**olay önceden filtre uygulanmış bir durumda bir sayfasını açın. 

Bir filtre veya arama ifadesi kaydetmeden gönderebilirsiniz. Örneğin, aşağıdaki isteği, eşleşen belgeleri döndüren marka adı üzerinde filtre uygular.

        GET /indexes/onlineCatalog/docs?$filter=brandname eq ‘Microsoft’ and category eq ‘Games’

Bkz: [Search belgeleri (Azure Search API)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) hakkında daha fazla bilgi için `$filter` ifadeler.

## <a name="see-also"></a>Ayrıca Bkz.
* [Azure arama hizmeti REST API'si](https://docs.microsoft.com/rest/api/searchservice)
* [Dizin işlemleri](https://docs.microsoft.com/rest/api/searchservice/Index-operations)
* [Belge işlemlerini](https://docs.microsoft.com/rest/api/searchservice/Document-operations)
* [Azure Arama'da çok yönlü navigasyon](search-faceted-navigation.md)

<!--Image references-->
[1]: ./media/search-pagination-page-layout/Pages-1-Viewing1ofNResults.PNG
[2]: ./media/search-pagination-page-layout/Pages-2-Tiled.PNG
[3]: ./media/search-pagination-page-layout/Pages-3-SortBy.png
[4]: ./media/search-pagination-page-layout/Pages-4-SortbyRelevance.png
[5]: ./media/search-pagination-page-layout/Pages-5-BuildSort.png 
