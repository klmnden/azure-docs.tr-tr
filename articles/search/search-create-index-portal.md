---
title: "Bir dizin (portalı - Azure Search) oluşturma | Microsoft Docs"
description: "Azure Portalı'nı kullanarak bir dizin oluşturun."
services: search
manager: jhubbard
author: heidisteen
documentationcenter: 
ms.assetid: 
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 06/20/2017
ms.author: heidist
ms.openlocfilehash: a7d98ab0937a7d3f932d5df34c19ae091129804e
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="create-an-azure-search-index-using-the-azure-portal"></a>Azure Portal kullanarak Azure Search dizini oluşturma
> [!div class="op_single_selector"]
> * [Genel Bakış](search-what-is-an-index.md)
> * [Portal](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

Prototip için Azure portalında yerleşik dizin designer'ı kullanın veya oluşturma bir [arama dizini](search-what-is-an-index.md) Azure Search hizmetinizde çalıştırmak için. 

## <a name="prerequisites"></a>Ön koşullar

Bu makalede varsayar bir [Azure aboneliği](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) ve [Azure Search Hizmeti](search-create-service-portal.md).  

## <a name="find-your-search-service"></a>Arama hizmetinizi bulma
1. Azure portal sayfasında oturum açın ve gözden [arama hizmetleri aboneliğiniz için](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)
2. Azure Search hizmetinizi seçin.

## <a name="name-the-index"></a>Dizin adı

1. Tıklatın **Ekle dizin** sayfanın üst kısmındaki komut çubuğunda düğmesi.
2. Azure Search dizininizi olarak adlandırın. 
   * Bir harf ile başlamalıdır.
   * Yalnızca küçük harf, rakam veya kesik çizgi kullanın ("-").
   * Ad 60 karakter sınırlayın.

  Dizin adı bağlantıları dizine ve Azure Search REST API'sini HTTP istekleri göndermek için kullanılan uç nokta URL'SİNİN bir parçası haline gelir.

## <a name="define-the-fields-of-your-index"></a>Dizininizi alanlarını tanımlayın

Dizin oluşturma içeren bir *alanlar koleksiyonu* , dizininizdeki aranabilir verileri tanımlar. Daha açık belirtmek gerekirse, ayrı ayrı karşıya belgelerinin yapısını belirtir. Alanlar koleksiyonu adlı ve, alanın nasıl kullanılacağını belirlemek için dizin öznitelikleri ile yazılan gerekli ve isteğe bağlı alanları içerir.

1. İçinde **eklemek dizin** dikey penceresinde tıklatın **alanları >** alan tanımı dikey penceresini kaydırarak açmak. 

2. Oluşturulan kabul *anahtar* türü Edm.String alan. Varsayılan olarak, alanın adlı *kimliği* ancak dize karşılayan sürece adlandırabilirsiniz [adlandırma kurallarını](https://docs.microsoft.com/rest/api/searchservice/Naming-rules). Her Azure Search dizini için bir anahtar alanı zorunludur ve bir dize olmalıdır.

3. Karşıya yükleyecek belgeleri tam olarak belirlemek için alanları ekleyin. Belgeler oluşur, bir *kimliği*, *otel adı*, *adresi*, *Şehir*, ve *bölge*, oluşturma bir dizindeki her biri için karşılık gelen alan. Gözden geçirme [Tasarım Kılavuzu bölümünde](#design) özniteliklerini ayarlama hakkında Yardım için.

4. İsteğe bağlı olarak kullanılan herhangi bir alan dahili olarak filtre ifadelerinde ekleyin. Alan öznitelikleri alanları arama işlemlerinin çıkarmak için ayarlanabilir.

5. Tamamlandığında, tıklatın **Tamam** kaydedip dizini oluşturun.

## <a name="tips-for-adding-fields"></a>Alanlar ekleyerek ipuçları

Portalda bir dizin oluşturma klavye yoğun bağlıdır. Adımları bu iş akışı izleyerek en aza indirir:

1. İlk olarak, adları girerek ve veri türleri ayarlama alan listesi oluşturun.

2. Ardından, toplu onay kutularının her bir üst öznitelik kullanım tüm alanlar için ayarını etkinleştirin ve onu gerektirmeyen az alan kutularını seçmeli olarak temizleyin. Örneğin, dize alanları genellikle aranamaz. Bu nedenle, tıklatabilir **alınabilir** ve **aranabilir** alanın tam metin araması izin yanı sıra hem de arama sonuçlarında alanın değerini döndürür. 

<a name="design"></a>
## <a name="design-guidance-for-setting-attributes"></a>Öznitelikleri ayarlamak için Tasarım Kılavuzu

Herhangi bir zamanda yeni alanlar ekleyebilirsiniz, ancak varolan alan tanımları dizini ömrü boyunca kilitli olduğunu. Bu nedenle, geliştiriciler genellikle portal Basit Dizin oluşturma, test fikirleri veya bir kurma aramak için portal sayfalarını kullanarak kullanın. Bir dizin tasarımı üzerinden sık yineleme dizini kolayca yeniden böylece kod tabanlı bir yaklaşım izlerseniz daha verimli olur.

Dizin kaydedilmeden önce Çözümleyicileri ve ilgili alanları ile ilişkilendirilir. Dil Çözümleyicileri veya ilgili dizin tanımınızı eklemek için her sekmeli sayfasında aracılığıyla tıklattığınızdan emin olun.

Dize alanları olarak genellikle işaretlenir **aranabilir** ve **alınabilir**.

Arama sonuçlarını daraltmak için kullanılan alanları içeren **sıralanabilir**, **Filterable**, ve **modellenebilir**.

Alan öznitelikleri nasıl bir alan, onu tam metin araması, modellenmiş bir gezinmede, sıralama işlemi ve benzeri kullanılan gibi kullanıldığını belirler. Aşağıdaki tabloda her özniteliği açıklanmaktadır.

|Öznitelik|Açıklama|  
|---------------|-----------------|  
|**aranabilir**|Tam metin dizin oluşturma sırasında sözcük bölme gibi sözcük analiz tabi aranabilir. Aranabilir alan "güneşli gün" gibi bir değer ayarlarsanız, dahili olarak, Güneşli tek tek belirteçleri"" ve "gün" olarak bölünür. Ayrıntılar için bkz [nasıl tam metin arama works](search-lucene-query-architecture.md).|  
|**filtrelenebilir**|Başvurulan **$filter** sorgular. Türünde filtrelenebilir alanlar `Edm.String` veya `Collection(Edm.String)` yalnızca tam eşleşme için karşılaştırmaları; bu nedenle Sözcük bölünmesi, uygulanabilecek değil. Örneğin, bu tür bir alana f "güneşli gün", ayarlarsanız `$filter=f eq 'sunny'` herhangi bir eşleşme bulur ancak `$filter=f eq 'sunny day'` olur. |  
|**sıralanabilir**|Varsayılan olarak sistem sonuçları puana göre sıralar, ancak belgeleri alanlara göre sıralama yapılandırabilirsiniz. Türünde alanlar `Collection(Edm.String)` olamaz **sıralanabilir**. |  
|**modellenebilir**|Genellikle bir isabet sayısı (örneğin, belirli bir şehirde Oteller) kategoriye içeren sunu arama sonuçlarının kullanılır. Bu seçenek türü alanlarla kullanılamaz `Edm.GeographyPoint`. Türünde alanlar `Edm.String` olan **filtrelenebilir**, **sıralanabilir**, veya **modellenebilir** en çok 32 kilobaytı uzunluğunda olabilir. Ayrıntılar için bkz [Create Index (REST API'si)](https://docs.microsoft.com/rest/api/searchservice/create-index).|  
|**anahtarı**|Dizin içinde belgeleri için benzersiz tanımlayıcı. Anahtar alan olarak yalnızca bir alanın seçtiniz ve türünde olmalıdır `Edm.String`.|  
|**alınabilir**|Alan bir arama sonucunda döndürülen olup olmadığını belirler. Bir alan kullanmak istediğinizde bu kullanışlıdır (gibi *Kar marjı*) bir filtre, sıralama veya mekanizması, Puanlama ancak sağlamadığı alanının son kullanıcı için görünür olmasını istiyorsunuz. Bu öznitelik olmalıdır `true` için `key` alanları.|  

## <a name="create-the-hotels-index-used-in-example-api-sections"></a>Örnek API bölümlerde kullanılan Oteller dizini oluşturma

Azure Search API belgelerine içeren basit bir özelliğe sahip olan kod örnekleri *Oteller* dizini. Aşağıdaki ekran görüntülerinde dizin tanımı sırasında belirtilen Fransızca Dil Çözümleyicisi dahil olmak üzere dizin tanımı görebilirsiniz, alıştırmada portalında olarak yeniden oluşturabilirsiniz.

![](./media/search-create-index-portal/field-definitions.png)

![](./media/search-create-index-portal/set-analyzer.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure Search dizini oluşturduktan sonra sonraki adıma geçebilirsiniz: [aranabilir verileri dizine yüklemek](search-what-is-data-import.md).

Alternatif olarak, dizinleri daha derin göz ele geçirebilir. Alanlar koleksiyonu ek olarak, bir dizin çözümleyiciler, ilgili, Puanlama profilleri ve CORS ayarları belirtir. En yaygın öğeleri tanımlamak için portal sekmeli sayfaları sağlar: alanları, Çözümleyicileri ve ilgili. Oluşturmak veya diğer öğeleri değiştirmek için REST API veya .NET SDK'sını kullanabilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.

 [Tam metin araması nasıl çalışır?](search-lucene-query-architecture.md)  
 [Arama hizmeti REST API'si](https://docs.microsoft.com/rest/api/searchservice/) [.NET SDK'sı](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)

