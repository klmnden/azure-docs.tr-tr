---
title: "Azure Search'te arama sonuçları sayfası nasıl | Microsoft Docs"
description: "Sayfa numaralandırma Azure Search'te, Microsoft Azure üzerinde barındırılan bulut arama hizmeti."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
ms.assetid: a0a1d315-8624-4cdf-b38e-ba12569c6fcc
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/29/2016
ms.author: heidist
ms.openlocfilehash: 1054e15a2751c53aad5dbc8054c4cec41102dee9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-page-search-results-in-azure-search"></a>Azure Arama'da arama sonuçlarını sayfalandırma
Bu makale, toplam sayıları, belge alma, sıralamalar ve gezinti gibi bir arama sonuçları sayfasının standart öğeleri uygulamak için Azure Search Hizmeti REST API kullanma hakkında yönergeler sağlar.

Aşağıda belirtilen her durumda, verileri veya arama sonuçları sayfasını bilgileri katkıda sayfasıyla ilgili seçenekleri aracılığıyla belirtilen [arama belge](http://msdn.microsoft.com/library/azure/dn798927.aspx) Azure arama hizmetinize gönderilen istekleri. İstekleri GET komutu, yol ve ne istenen hizmet bildirmek sorgu parametreleri ile nasıl yanıt formüle içerir.

> [!NOTE]
> Geçerli bir istek bir hizmet URL'si ve yol, HTTP fiili gibi öğeleri içerir `api-version`ve benzeri. Konuyu uzatmamak amacıyla, yalnızca sayfa numaralandırma için uygun olan söz dizimini vurgulamak için örnekler kırpılır. Lütfen bakın [Azure Search Hizmeti REST API'si](http://msdn.microsoft.com/library/azure/dn798935.aspx) isteği sözdizimi hakkında ayrıntılar için belgelere.
> 
> 

## <a name="total-hits-and-page-counts"></a>Toplam İsabet ve sayfa sayıları
Bir sorgudan döndürülen sonuç sayısı toplam gösteren ve ardından daha küçük parçalar sonuçları getireceğinden neredeyse tüm arama sayfalar için temel.

![][1]

Azure Search'te kullandığınız `$count`, `$top`, ve `$skip` bu değer döndürmek için parametreler. Aşağıdaki örnek olarak döndürülen toplam isabet sayısı için bir örnek istek gösterir `@OData.count`:

        GET /indexes/onlineCatalog/docs?$count=true

Belge 15 gruplarındaki almak ve ayrıca ilk sayfasına başlangıç İsabetli göster:

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

Sonuçları sayfa numaralandırma, gerektiren her ikisi de `$top` ve `$skip`, burada `$top` bir toplu işlemde döndürmek için kaç tane öğelerini belirtir ve `$skip` atlamak için kaç tane öğelerini belirtir. Aşağıdaki örnekte, her sayfa sonraki 15 öğeleri gösterir artımlı atlar belirttiği `$skip` parametresi.

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=15&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=30&$count=true

## <a name="layout"></a>Düzen
Arama sonuçları sayfasında, bir küçük resim, alanlarının alt kümesini ve tam ürün sayfasına bir bağlantı göstermek isteyebilirsiniz.

 ![][2]

Azure Search'te kullanacağınız `$select` ve bu deneyimi uygulamak için bir arama komutu.

Alanların döşeli düzeni için bir alt döndürmek için:

        GET /indexes/ onlineCatalog/docs?search=*&$select=productName,imageFile,description,price,rating 

Görüntüleri ve medya dosyalarını doğrudan aranabilir değildir ve maliyetleri azaltmak için Azure Blob Depolama alanı gibi başka bir depolama Platform depolanması gerekir. Dizin ve belgeleri dış içeriği URL adresini depolayan bir alan tanımlayın. Daha sonra alan bir görüntü başvuru olarak kullanabilirsiniz. Resim URL'si belgede olmalıdır.

Bir ürün açıklama sayfasına almak için bir **onClick** olay, kullanım [arama belge](http://msdn.microsoft.com/library/azure/dn798929.aspx) almak için belge anahtarında geçirmek için. Anahtarın veri türü `Edm.String`. Bu örnek, *246810*. 

        GET /indexes/onlineCatalog/docs/246810

## <a name="sort-by-relevance-rating-or-price"></a>İlgi, derecelendirme veya fiyat göre sırala
Genellikle ilgi için varsayılan sıralama siparişleri ancak böylece müşteriler hızla varolan sonuçları farklı bir sıralama düzeni içinde sırasını yeniden ayarlaması alternatif sıralama siparişleri kullanıma hazır olmak için yaygın bir durumdur.

 ![][3]

Azure Search'te sıralama dayanır `$orderby` olarak dizin oluşturulmuş tüm alanlar için ifade`"Sortable": true.`

İlgi kesinlikle profilleri Puanlama ile ilişkilidir. Puanlama varsayılan metin analizi ve İstatistikler kullanır daha fazla veya daha güçlü eşleşme belgelerle bir arama terimi giderek daha yüksek puanları ile tüm sonuçları, sipariş derecelendirmek için kullanabilirsiniz.

Alternatif sıralamalar genellikle ilişkili olan **onClick** sıralama düzenini derlemeler bir yönteme geri arama olayları. Örneğin, bu sayfa öğesi verilen:

 ![][4]

Seçili sıralama seçeneği giriş olarak kabul eder ve sıralı bir listesi için bu seçeneği ile ilişkili ölçütler döndüren bir yöntem oluşturursunuz.

 ![][5]

> [!NOTE]
> Varsayılan Puanlama birçok senaryo için yeterli olmakla birlikte, bunun yerine özel bir Puanlama profili ilgi alma öneririz. Özel bir Puanlama profili işinize daha faydalı artırma öğeleri için bir yol sağlar. Bkz: [Puanlama profili Ekle](http://msdn.microsoft.com/library/azure/dn798928.aspx) daha fazla bilgi için. 
> 
> 

## <a name="faceted-navigation"></a>Çok yönlü gezinme
Arama gezinti genellikle yan veya bir sayfanın üst kısmında bulunan bir sonuçlar sayfası üzerinde yaygındır. Azure Search'te modellenmiş bir gezinmede önceden tanımlanmış filtrelere göre kendi kendine yönlendirilmiş arama sağlar. Bkz: [Azure Search'te modellenmiş bir gezinmede](search-faceted-navigation.md) Ayrıntılar için.

## <a name="filters-at-the-page-level"></a>Sayfa düzeyinde filtreleri
Çözüm tasarımınızın belirli türlerdeki içerik (örneğin, sayfanın en üstünde listelenen bölümleri olan bir çevrimiçi perakende uygulama) için ayrılmış arama sayfalar dahil, bir filtre ifadesi yanında ekleyebilirsiniz bir **onClick** olay makale bir durumda bir sayfayı açın. 

Bir filtre ile veya olmadan arama ifadesi gönderebilirsiniz. Örneğin, aşağıdaki isteği marka, eşleşen belgeleri döndüren adına filtre uygular.

        GET /indexes/onlineCatalog/docs?$filter=brandname eq ‘Microsoft’ and category eq ‘Games’

Bkz: [Search belgeleri (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) hakkında daha fazla bilgi için `$filter` ifadeler.

## <a name="see-also"></a>Ayrıca Bkz.
* [Azure Search Hizmeti REST API'si](http://msdn.microsoft.com/library/azure/dn798935.aspx)
* [Dizin işlemleri](http://msdn.microsoft.com/library/azure/dn798918.aspx)
* [Belge işlemleri](http://msdn.microsoft.com/library/azure/dn800962.aspx)
* [Video ve Azure Search öğreticileri](search-video-demo-tutorial-list.md)
* [Azure Search'te modellenmiş bir gezinmede](search-faceted-navigation.md)

<!--Image references-->
[1]: ./media/search-pagination-page-layout/Pages-1-Viewing1ofNResults.PNG
[2]: ./media/search-pagination-page-layout/Pages-2-Tiled.PNG
[3]: ./media/search-pagination-page-layout/Pages-3-SortBy.png
[4]: ./media/search-pagination-page-layout/Pages-4-SortbyRelevance.png
[5]: ./media/search-pagination-page-layout/Pages-5-BuildSort.png 
