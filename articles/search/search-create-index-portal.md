---
title: Azure portal - Azure Search bir Azure Search dizini oluşturma
description: Azure portalında yerleşik dizin tasarımcıları kullanarak arama için dizin oluşturmayı öğrenin.
manager: cgronlun
author: heidisteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 4bba8b41418dadad1b241d60ab0b7aeee4c046d7
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53316718"
---
# <a name="how-to-create-an-azure-search-index-using-the-azure-portal"></a>Azure portalını kullanarak Azure Search dizini oluşturma

Azure Search'ü içeren yerleşik bir dizin Tasarımcısı prototipleri için yararlı portalında veya oluşturma bir [arama dizini](search-what-is-an-index.md) Azure Search hizmetinizde barındırılan. Araç, şema yapımı için kullanılır. Tanımı kaydedin, boş bir dizin tam olarak Azure Search'te ifade olur. Aranabilir verileri ile yüklediğiniz nasıl size bağlıdır.

Dizin tasarımcısını dizin oluşturma için yalnızca bir yaklaşımdır. Programlı olarak kullanarak bir dizin oluşturabilirsiniz [.NET](search-create-index-dotnet.md) veya [REST](search-create-index-rest-api.md) API'leri.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, bir [Azure aboneliği](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) ve [Azure Search hizmeti](search-create-service-portal.md) kullanıldığı varsayılır.

## <a name="open-index-designer-and-name-an-index"></a>Dizin tasarımcısını açın ve dizin adı

1. [Azure portalında](https://portal.azure.com) oturum açın ve hizmet panosunu açın. Atlama çubuğundaki **Tüm hizmetler**’e tıklayarak geçerli abonelikteki mevcut "arama hizmetleri" için arama yapabilirsiniz. 

2.  Sayfanın en üstündeki komut çubuğunda **Dizin ekle** düğmesine tıklayın.

3. Azure Search dizininizi adlandırın. Dizin adları, dizin oluşturma ve sorgu işlemlerinde başvurulur. Dizin adı, dizinle yapılan bağlantılarda ve Azure Search REST API’sinde HTTP istekleri göndermek için kullanılan uç nokta URL’sinin bir parçasını oluşturur.

   * En başta bir harf kullanın.
   * Yalnızca küçük harfleri, rakamları veya kısa çizgileri ("-") kullanın.
   * Adı 60 karakterle sınırlayın.

## <a name="define-the-fields-of-your-index"></a>Dizininizin alanlarını tanımlama

Dizin oluşturma, dizininizdeki aranabilir verileri tanımlayan bir *Alanlar koleksiyonunu* içerir. Tamamen alanlar koleksiyonu ayrı olarak karşıya yüklediğiniz belgelerin yapısını belirtir. Gerekli ve isteğe bağlı alanlar adlandırılmış ve yazılmış, alanın nasıl kullanılabileceğini belirleyen dizin öznitelikleriyle birlikte bir alanlar koleksiyonu içerir.

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

