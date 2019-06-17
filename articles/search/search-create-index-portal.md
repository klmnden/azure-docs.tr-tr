---
title: Azure portal - Azure Search bir Azure Search dizini oluşturma
description: Azure portalında yerleşik dizin tasarımcıları kullanarak arama için dizin oluşturmayı öğrenin.
manager: cgronlun
author: heidisteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 02/16/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 6a2bac71c37cc750eb24e3492ecdcdf0b2333cce
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60817305"
---
# <a name="create-an-azure-search-index-in-the-portal"></a>Portalda Azure Search dizini oluşturma

Azure Search'ü içeren yerleşik bir dizin Tasarımcısı prototipleri için yararlı portalında veya oluşturma bir [arama dizini](search-what-is-an-index.md) Azure Search hizmetinizde barındırılan. Araç, şema yapımı için kullanılır. Tanımı kaydedin, boş bir dizin tam olarak Azure Search'te ifade olur. Aranabilir verileri ile yüklediğiniz nasıl size bağlıdır.

Dizin tasarımcısını dizin oluşturma için yalnızca bir yaklaşımdır. Programlı olarak kullanarak bir dizin oluşturabilirsiniz [.NET](search-create-index-dotnet.md) veya [REST](search-create-index-rest-api.md) API'leri.

## <a name="start-index-designer"></a>Dizin tasarımcısını Başlat

1. [Azure portalında](https://portal.azure.com) oturum açın ve hizmet panosunu açın. Atlama çubuğundaki **Tüm hizmetler**’e tıklayarak geçerli abonelikteki mevcut "arama hizmetleri" için arama yapabilirsiniz. 

2. Tıklayın **dizin** sayfanın üst kısmındaki komut çubuğunda bağlantı.

   ![Komut çubuğunda dizin Bağlantı Ekle](media/search-create-index-portal/add-index.png "komut çubuğunda dizin Bağlantı Ekle")

3. Azure Search dizininizi adlandırın. Dizin adları, dizin oluşturma ve sorgu işlemlerinde başvurulur. Dizin adı, dizinle yapılan bağlantılarda ve Azure Search REST API’sinde HTTP istekleri göndermek için kullanılan uç nokta URL’sinin bir parçasını oluşturur.

   * En başta bir harf kullanın.
   * Yalnızca küçük harfleri, rakamları veya kısa çizgileri ("-") kullanın.
   * Adı 60 karakterle sınırlayın.

## <a name="add-fields"></a>Alanlar ekleme

Dizin oluşturma, dizininizdeki aranabilir verileri tanımlayan bir *Alanlar koleksiyonunu* içerir. Tamamen alanlar koleksiyonu ayrı olarak karşıya yüklediğiniz belgelerin yapısını belirtir. Gerekli ve isteğe bağlı alanlar adlandırılmış ve yazılmış, alanın nasıl kullanılabileceğini belirleyen dizin öznitelikleriyle birlikte bir alanlar koleksiyonu içerir.

1. Tam olarak karşıya yüklediğiniz, ayarı belgeleri belirtmek için alanlar ekleme bir [veri türü](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) her biri için. Örneğin, belgeler bir *otel kimliği*, *otel adı*, *adresi*, *Şehir*, ve *bölge*, dizindeki her biri için karşılık gelen bir alan oluşturun. Gözden geçirme [bölümüne alan tasarım kılavuzunu](#design) özniteliklerini ayarlama konusunda Yardım.

2. Belirtin bir *anahtarı* Edm.String türünde alan. Bu alanın değerlerini her belgenin benzersiz şekilde tanımlamalıdır. Varsayılan olarak alan, *id* olarak adlandırılır, ancak [adlandırma kurallarına](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) uygun şekilde alanı yeniden adlandırabilirsiniz. Örneğin, alanlar koleksiyonu içerir, *otel kimliği*, Kiracı anahtarınızı için seçmelisiniz. Her Azure Search dizini için bir anahtar alan zorunludur ve dize olmalıdır.

3. Her bir alan özniteliklerini ayarlayın. Dizin tasarımcısını veri türü için geçersiz olan herhangi bir özniteliği dahil değildir, ancak ne içerecek şekilde önerisi yoktur. Sonraki bölümde öznitelikleri neler olduğunu anlama açısından yönergeleri inceleyin.

    Azure Search API’si belgeleri, basit bir *oteller* dizinin yer aldığı kod örneklerini içerir. Aşağıdaki ekran görüntüsünde, dizin tanımı sırasında belirtilen Fransızca Dil çözümleyicisini içeren dizin tanımını görebilirsiniz, portalda uygulama olarak yeniden oluşturabilirsiniz.

    ![Hotels tanıtım dizini](media/search-create-index-portal/field-definitions.png "Hotels tanıtım dizini")

4. İşiniz bittiğinde tıklayın **Oluştur** kaydedip dizini oluşturmak için.

<a name="design"></a>

## <a name="set-attributes"></a>Öznitelikleri Ayarla

İstediğiniz zaman yeni alanlar ekleyebilseniz de, dizinin ömrü boyunca mevcut alan tanımları kilitlenir. Bu nedenle geliştiriciler genellikle basit dizinler oluşturmak, fikirleri test etmek veya portal sayfalarını kullanarak bir ayarı bulmak için portalı kullanır. Dizini kolayca yeniden derleyebilmeniz için kod tabanlı bir yaklaşım izlerseniz, bir dizin tasarımı çerçevesinde sık sık yapılan yineleme daha verimli olur.

Dizin kaydedilmeden önce çözümleyiciler ve öneri araçları alanlarla ilişkilendirilir. Oluşturmakta olduğunuz dizin tanımınıza dil Çözümleyicileri veya öneri Araçları eklemek emin olun.

Dize alanları genellikle **Aranabilir** ve **Alınabilir** olarak işaretlenir. Arama sonuçlarını daraltmak için kullanılan alanlar arasında **Sıralanabilir**, **Filtrelenebilir** ve **Modellenebilir** alanları yer alır.

Alan öznitelikleri, bir alanın nasıl kullanıldığını; örneğin, tam metin alanında mı, çok yönlü gezinmede mi, sıralama işlemlerinde vb.’de mi kullanılıp kullanılmayacağı belirler. Aşağıdaki tabloda her bir öznitelik açıklanmaktadır.

|Öznitelik|Açıklama|  
|---------------|-----------------|  
|**aranabilir**|Tam metin aranabilir, dizin oluşturma sırasında sözcüklere bölme gibi sözcük temelli analize tabidir. Aranabilir bir alanı, "güneşli gün" gibi bir değere ayarlarsanız, dahili olarak bu "güneşli" ve "gün" belirteçlerine bölünür. Ayrıntılar için bkz. [Tam metin araması nasıl çalışır?](search-lucene-query-architecture.md)|  
|**filtrelenebilir**|**$filter** sorgularında başvurulur. `Edm.String` veya `Collection(Edm.String)` türünde filtrelenebilir alanlara sözcüklere bölme işlemi uygulanmaz, bu nedenle karşılaştırmalar yalnızca tam eşleşmeler içindir. Örneğin, böyle bir alanı "güneşli gün" olarak ayarlarsanız `$filter=f eq 'sunny'` herhangi bir eşleşme bulmaz, ancak `$filter=f eq 'sunny day'` bulur. |  
|**sıralanabilir**|Varsayılan olarak sistem sonuçları puana göre sıralar, ancak belgelerdeki alanlara göre sıralamayı yapılandırabilirsiniz. `Collection(Edm.String)` türünde alanlar **sıralanabilir** olamaz. |  
|**modellenebilir**|Genellikle kategoriye göre (örneğin, belirli bir şehirdeki oteller) isabet sayısını içeren bir arama sonuçları sunumunda kullanılır. Bu seçenek, `Edm.GeographyPoint` türünde alanlarla kullanılamaz. **Filtrelenebilir**, **sıralanabilir** veya **modellenebilir** olan `Edm.String` türünde alanlar en fazla 32 kilobayt uzunluğunda olabilir. Ayrıntılar için bkz. [Dizin Oluşturma (REST API’si)](https://docs.microsoft.com/rest/api/searchservice/create-index).|  
|**anahtar**|Dizin içindeki belgeler için benzersiz tanımlayıcı. Anahtar alan olarak tam olarak bir alan seçilmeli ve `Edm.String` türünde olmalıdır.|  
|**alınabilir**|Alanın bir arama sonucunda döndürülüp döndürülemeyeceğini belirler. Filtre, sıralama veya puanlama mekanizması olarak bir alanı (örn. *kâr marjı*) kullanmak istediğinizde, ancak alanın son kullanıcıya görünür olmasını istemediğinizde bu faydalıdır. Bu öznitelik, `key` alanları için `true` olmalıdır.|  

## <a name="next-steps"></a>Sonraki adımlar

Azure Search dizini oluşturduktan sonra bir sonraki adıma geçebilirsiniz: [aranabilir verileri dizine yükleme](search-what-is-data-import.md).

Alternatif olarak, siz de ele geçirebilir bir [dizinlere daha ayrıntılı Görünüm](search-what-is-an-index.md). Alanlar koleksiyonuna ek olarak bir dizin, çözümleyicileri, öneri araçlarını, puanlama profillerini ve CORS ayarlarını da belirtir. Portal, en yaygın öğeleri tanımlamak için sekmeli sayfalar sağlar: Alanlar, çözümleyiciler ve öneri araçları. Başka öğeler oluşturmak veya değiştirmek için REST API’sini veya .NET SDK’sını kullanabilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.

 [Tam metin araması nasıl çalışır?](search-lucene-query-architecture.md)  
 [Arama hizmeti REST API’si](https://docs.microsoft.com/rest/api/searchservice/) [.NET SDK’sı](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)

