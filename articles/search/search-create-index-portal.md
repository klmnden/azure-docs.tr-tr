---
title: Dizin oluşturma (portal - Azure Search) | Microsoft Docs
description: Azure Portal’ı kullanarak bir dizin oluşturun.
manager: cgronlun
author: heidisteen
ms.service: search
ms.devlang: NA
ms.topic: quickstart
ms.date: 06/20/2017
ms.author: heidist
ms.openlocfilehash: ab0352b8c830e875afc9b1d1b006ba4d2a512d7a
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="create-an-azure-search-index-using-the-azure-portal"></a>Azure Portal’ı kullanarak bir Azure Search dizini oluşturma
> [!div class="op_single_selector"]
> * [Genel Bakış](search-what-is-an-index.md)
> * [Portal](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

Azure portalında yerleşik dizin tasarımcısını kullanarak, Azure Search hizmetinizde çalıştırılacak bir [arama dizininin](search-what-is-an-index.md) prototipini veya kendisini oluşturun. 

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide, bir [Azure aboneliği](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) ve [Azure Search hizmeti](search-create-service-portal.md) kullanıldığı varsayılır.  

## <a name="find-your-search-service"></a>Arama hizmetinizi bulma
1. Azure portalı sayfasında oturum açın ve [aboneliğiniz için arama hizmetleri](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) bölümünü gözden geçirin
2. Azure Search hizmetinizi seçin.

## <a name="name-the-index"></a>Dizini adlandırma

1. Sayfanın en üstündeki komut çubuğunda **Dizin ekle** düğmesine tıklayın.
2. Azure Search dizininizi adlandırın. 
   * En başta bir harf kullanın.
   * Yalnızca küçük harfleri, rakamları veya kısa çizgileri ("-") kullanın.
   * Adı 60 karakterle sınırlayın.

  Dizin adı, dizinle yapılan bağlantılarda ve Azure Search REST API’sinde HTTP istekleri göndermek için kullanılan uç nokta URL’sinin bir parçasını oluşturur.

## <a name="define-the-fields-of-your-index"></a>Dizininizin alanlarını tanımlama

Dizin oluşturma, dizininizdeki aranabilir verileri tanımlayan bir *Alanlar koleksiyonunu* içerir. Daha açık belirtmek gerekirse, ayrı olarak karşıya yüklediğiniz belgelerin yapısını belirtir. Alanlar koleksiyonu, alanın nasıl kullanılabileceğini belirleyen dizin öznitelikleriyle birlikte, adlandırılan ve yazılan zorunlu ve isteğe bağlı alanları içerir.

1. **Dizin Ekle** dikey penceresinde **Alanlar >** seçeneğine tıklayarak alan tanımı dikey penceresini kaydırarak açın. 

2. Edm.String türünde oluşturulan *anahtar* alanını kabul edin. Varsayılan olarak alan, *id* olarak adlandırılır, ancak [adlandırma kurallarına](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) uygun şekilde alanı yeniden adlandırabilirsiniz. Her Azure Search dizini için bir anahtar alan zorunludur ve dize olmalıdır.

3. Karşıya yükleyeceğiniz belgeleri tam olarak belirtmek için alanlar ekleyin. Belgeler bir *kimlik*, *otel adı*, *adres*, *şehir* ve *bölgeden* oluşuyorsa, dizindeki her biri için karşılık gelen bir alan oluşturun. Özniteliklerin ayarlanmasına yardımcı olması için [aşağıdaki bölümde yer alan tasarım kılavuzunu](#design) gözden geçirin.

4. İsteğe bağlı olarak, filtre ifadelerinde dahili olarak kullanılan alanlar ekleyin. Alandaki öznitelikler, alanları arama işlemlerinden dışlayacak şekilde ayarlanabilir.

5. Tamamlandığında, kaydedip dizini oluşturmak için **Tamam**’a tıklayın.

## <a name="tips-for-adding-fields"></a>Alan ekleme ipuçları

Portalda dizin oluşturma işlemi yoğun klavye kullanımı gerektirir. Bu iş akışını izleyerek adımları en aza indirin:

1. İlk olarak, adları girip veri türlerini ayarlayarak alan listesini derleyin.

2. Ardından, her bir özniteliğin üst kısmındaki onay kutularını kullanarak tüm alanlara yönelik ayarı toplu etkinleştirin ve zorunlu olmayan birkaç alanın kutusunu seçerek işaretini kaldırın. Örneğin, dize alanları genellikle aranabilir. Bu nedenle, **Alınabilir** ve **Aranabilir** seçeneğine tıklayarak hem arama sonuçlarında alanın değerlerini döndürebilir hem de alanda tam metin aramasına izin verebilirsiniz. 

<a name="design"></a>
## <a name="design-guidance-for-setting-attributes"></a>Öznitelikleri ayarlamaya yönelik tasarım kılavuzu

İstediğiniz zaman yeni alanlar ekleyebilseniz de, dizinin ömrü boyunca mevcut alan tanımları kilitlenir. Bu nedenle geliştiriciler genellikle basit dizinler oluşturmak, fikirleri test etmek veya portal sayfalarını kullanarak bir ayarı bulmak için portalı kullanır. Dizini kolayca yeniden derleyebilmeniz için kod tabanlı bir yaklaşım izlerseniz, bir dizin tasarımı çerçevesinde sık sık yapılan yineleme daha verimli olur.

Dizin kaydedilmeden önce çözümleyiciler ve öneri araçları alanlarla ilişkilendirilir. Dizin tanımınıza dil çözümleyicileri veya öneri araçları eklemek için her sekmeli sayfaya tıkladığınızdan emin olun.

Dize alanları genellikle **Aranabilir** ve **Alınabilir** olarak işaretlenir.

Arama sonuçlarını daraltmak için kullanılan alanlar arasında **Sıralanabilir**, **Filtrelenebilir** ve **Modellenebilir** alanları yer alır.

Alan öznitelikleri, bir alanın nasıl kullanıldığını; örneğin, tam metin alanında mı, çok yönlü gezinmede mi, sıralama işlemlerinde vb.’de mi kullanılıp kullanılmayacağı belirler. Aşağıdaki tabloda her bir öznitelik açıklanmaktadır.

|Öznitelik|Açıklama|  
|---------------|-----------------|  
|**aranabilir**|Tam metin aranabilir, dizin oluşturma sırasında sözcüklere bölme gibi sözcük temelli analize tabidir. Aranabilir bir alanı, "güneşli gün" gibi bir değere ayarlarsanız, dahili olarak bu "güneşli" ve "gün" belirteçlerine bölünür. Ayrıntılar için bkz. [Tam metin araması nasıl çalışır?](search-lucene-query-architecture.md)|  
|**filtrelenebilir**|**$filter** sorgularında başvurulur. `Edm.String` veya `Collection(Edm.String)` türünde filtrelenebilir alanlara sözcüklere bölme işlemi uygulanmaz, bu nedenle karşılaştırmalar yalnızca tam eşleşmeler içindir. Örneğin, böyle bir alanı "güneşli gün" olarak ayarlarsanız `$filter=f eq 'sunny'` herhangi bir eşleşme bulmaz, ancak `$filter=f eq 'sunny day'` bulur. |  
|**sıralanabilir**|Varsayılan olarak sistem sonuçları puana göre sıralar, ancak belgelerdeki alanlara göre sıralamayı yapılandırabilirsiniz. `Collection(Edm.String)` türünde alanlar **sıralanabilir** olamaz. |  
|**modellenebilir**|Genellikle kategoriye göre (örneğin, belirli bir şehirdeki oteller) isabet sayısını içeren bir arama sonuçları sunumunda kullanılır. Bu seçenek, `Edm.GeographyPoint` türünde alanlarla kullanılamaz. **Filtrelenebilir**, **sıralanabilir** veya **modellenebilir** olan `Edm.String` türünde alanlar en fazla 32 kilobayt uzunluğunda olabilir. Ayrıntılar için bkz. [Dizin Oluşturma (REST API’si)](https://docs.microsoft.com/rest/api/searchservice/create-index).|  
|**anahtar**|Dizin içindeki belgeler için benzersiz tanımlayıcı. Anahtar alan olarak tam olarak bir alan seçilmeli ve `Edm.String` türünde olmalıdır.|  
|**alınabilir**|Alanın bir arama sonucunda döndürülüp döndürülemeyeceğini belirler. Filtre, sıralama veya puanlama mekanizması olarak bir alanı (örn. *kâr marjı*) kullanmak istediğinizde, ancak alanın son kullanıcıya görünür olmasını istemediğinizde bu faydalıdır. Bu öznitelik, `key` alanları için `true` olmalıdır.|  

## <a name="create-the-hotels-index-used-in-example-api-sections"></a>Örnek API bölümlerinde kullanılan oteller dizinini oluşturma

Azure Search API’si belgeleri, basit bir *oteller* dizinin yer aldığı kod örneklerini içerir. Aşağıdaki ekran görüntülerinde, dizin tanımı sırasında belirtilen Fransızca dil çözümleyicisini içeren dizin tanımını görebilirsiniz. Portalda uygulama olarak bunu yeniden oluşturabilirsiniz.

![](./media/search-create-index-portal/field-definitions.png)

![](./media/search-create-index-portal/set-analyzer.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure Search dizini oluşturduktan sonra bir sonraki adıma geçebilirsiniz: [aranabilir verileri dizine yükleme](search-what-is-data-import.md).

Alternatif olarak, dizinlere daha ayrıntılı şekilde göz atabilirsiniz. Alanlar koleksiyonuna ek olarak bir dizin, çözümleyicileri, öneri araçlarını, puanlama profillerini ve CORS ayarlarını da belirtir. Portal, en yaygın öğeleri tanımlamak için sekmeli sayfalar sağlar: Alanlar, çözümleyiciler ve öneri araçları. Başka öğeler oluşturmak veya değiştirmek için REST API’sini veya .NET SDK’sını kullanabilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.

 [Tam metin araması nasıl çalışır?](search-lucene-query-architecture.md)  
 [Arama hizmeti REST API’si](https://docs.microsoft.com/rest/api/searchservice/) [.NET SDK’sı](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)

