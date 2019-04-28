---
title: Azure portal - Azure Search verileri sorgulamak için arama Gezgini aracı
description: Azure Search'te sorgu dizini arama Gezgini gibi Azure portal araçlarını kullanın. Arama terimleri veya Gelişmiş söz dizimi ile tam arama dizesini girin.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 85e574a56380384b10d0916385a8816fd26c2eeb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61292479"
---
# <a name="search-explorer-for-querying-data-in-azure-search"></a>Azure Search'te veri sorgulamak için arama Gezgini 

Bu makale, mevcut bir Azure Search dizini kullanarak nasıl sorgulanacağını gösterir **arama Gezgini** Azure portalında. Hizmetinizde var olan bir dizine basit veya tam Lucene sorgu dizeleri göndermek için arama Gezgini'ni kullanabilirsiniz. 

   ![Arama Gezgini komut portalında](./media/search-explorer/search-explorer-cmd2.png "portalında arama Gezgini komutu")


Başlangıç konusunda yardım için bkz. [Aramaya Başla Gezgini](#start-search-explorer).

## <a name="basic-search-strings"></a>Temel arama dizeleri

Aşağıdaki örneklerde, yerleşik realestate örnek dizini varsayılmaktadır. Bu dizin oluşturmanıza yardımcı olması için bkz: [hızlı başlangıç: İçeri aktarma, dizin ve Azure portalındaki sorgu](search-get-started-portal.md).

### <a name="example-1---empty-search"></a>Örnek 1 - boş arama

İçeriğinizi ilk göz için boş bir arama tıklayarak yürütme **arama** sağlanan herhangi bir hüküm ile. Boş bir arama ilk sorgu olarak yararlıdır, çünkü böylece belge oluşturma inceleyebilirsiniz belgelerinin tamamını döndürür. Boş bir arama üzerinde hiçbir arama sıralamasını yoktur ve belgeler, rastgele sırayla döndürülür (`"@search.score": 1` tüm belgeler için). Varsayılan olarak, arama talebinde 50 belgeler döndürülür.

Boş bir arama eşdeğer sözdizimi `*` veya `search=*`.

   ```Input
   search=*
   ```

   **Sonuçlar**
   
   ![Boş sorgu örnek](./media/search-explorer/search-explorer-example-empty.png "Unqualified ya da boş sorgu örneği")

### <a name="example-2---free-text-search"></a>Örnek 2 - serbest metin arama

Serbest biçimli sorguları ile veya olmadan işleçler, özel bir uygulamadan Azure Search'e gönderilen kullanıcı tarafından tanımlanan sorguların benzetimi için kullanışlıdır. Sorgu terimleriyle veya ifadeleri sağladığınızda, arama sıralamasını oyuna geldiğini dikkat edin. Aşağıdaki örnek, bir serbest metin arama gösterir.

   ```Input
   Seattle apartment "Lake Washington" miele OR thermador appliance
   ```

   **Sonuçlar**

   Ctrl-F, ilgilendiğiniz belirli terimleri sonuçları içinde aramak için kullanabilirsiniz.

   ![Serbest metin sorgusu örneği](./media/search-explorer/search-explorer-example-freetext.png "serbest metin sorgusu örneği")

### <a name="example-3---count-of-matching-documents"></a>Örnek 3 - eşleşen belge sayısı 

Ekleme **$count** bir dizinde bulunan eşleşme sayısı alınamıyor. Boş bir arama üzerinde sayısı belgeleri dizindeki toplam sayısıdır. Tam bir arama sorgusu eşleşen belgelerin sayıdır.

   ```Input1
   $count=true
   ```
   **Sonuçlar**

   ![Belgeler örnek sayısı](./media/search-explorer/search-explorer-example-count.png "eşleşen belge dizinde sayısı")

### <a name="example-4---restrict-fields-in-search-results"></a>Örnek 4 - kısıtlama alanları arama sonuçları

Ekleme **$select** daha okunabilir çıkış için açıkça adlandırılmış alanları sonuçlarını sınırlamak için **arama Gezgini**. Arama dizesi tutmak ve **$count = true**, önek bağımsız değişkenlerle **&**. 

   ```Input
   search=seattle condo&$select=listingId,beds,baths,description,street,city,price&$count=true
   ```

   **Sonuçlar**

   ![Limit alanları örneği](./media/search-explorer/search-explorer-example-selectfield.png "kısıtlama alanları arama sonuçları")

### <a name="example-5---return-next-batch-of-results"></a>Örnek 5 - dönüş sonraki toplu sonuçları

Azure Search, üzerinde arama sıralamasını göre en çok 50 eşleşmesi döndürür. Sonraki eşleşen belge kümesini almak için URL'ye **$top = 100 & $skip = 50** sonuç kümesini 100 belgelere artırmak için (varsayılan değer 50, 1000 en yüksek değer), ilk 50 belgeleri atlanıyor. Bir sorgu terimine ya da sonuçları sıralanmış için ifade gibi belirli bir ölçüt sağlamanız gereken geri çağırma. Bildirim arama puanları derin azaltmak arama sonuçlarının ulaşın.

   ```Input
   search=seattle condo&$select=listingId,beds,baths,description,street,city,price&$count=true&$top=100,&$skip=50
   ```

   **Sonuçlar**

   ![Toplu arama sonuçları](./media/search-explorer/search-explorer-example-topskip.png "dönüş sonraki toplu arama sonuçları")

## <a name="filter-expressions-greater-than-less-than-equal-to"></a>Filtre ifadelerini (büyük, küçük, ona eşit)

Kullanım **$filter** parametresi, serbest metin arama yerine kesin ölçütlerini belirtmek istediğinizde. Bu örnek 3'ten büyük yatak arar: `search=seattle condo&$filter=beds gt 3&$count=true`

   ![Filtre ifadesi](./media/search-explorer/search-explorer-example-filter.png "ölçüte göre filtrele")

## <a name="order-by-expressions"></a>Order by ifadeleri

Ekleme **$orderby** arama puanı yanı sıra başka bir alana göre sonuçları sıralamak için. Bu çıkış test etmek için kullanabileceğiniz örnek ifade `search=seattle condo&$select=listingId,beds,price&$filter=beds gt 3&$count=true&$orderby=price asc`

   ![Výraz OrderBy](./media/search-explorer/search-explorer-example-ordery.png "sıralama düzenini değiştirme")

Her ikisi de **$filter** ve **$orderby** OData yapılarını ifadelerdir. Daha fazla bilgi edinmek için bkz. [OData söz dizimini filtreleme](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search).

<a name="start-search-explorer"></a>

## <a name="how-to-start-search-explorer"></a>Arama Gezgini başlatma

1. İçinde [Azure portalında](https://portal.azure.com), panodan arama hizmeti sayfasını açın veya [hizmetinizi bulma](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) Hizmet listesinde.

2. Hizmet genel bakış sayfasında tıklatın **arama Gezgini**.

   ![Arama Gezgini komut portalında](./media/search-explorer/search-explorer-cmd2.png "portalında arama Gezgini komutu")

3. Sorgu dizini seçin.

   ![Sorgu dizini seçin](./media/search-explorer/search-explorer-changeindex-se2.png "dizini seçin")

4. İsteğe bağlı olarak, API sürümünü ayarlama. Varsayılan olarak, geçerli genel kullanıma sunulan API sürümü seçilir, ancak bir önizleme seçebilir veya söz dizimi kullanmak istiyorsanız, eski sürüme özgü bir API'dir.

5. Dizin ve API sürümü seçili, arama terimleri veya tam sorgu ifadeleri arama çubuğunda girin ve tıklayın **arama** yürütülecek.

   ![Arama terimleri girin ve Ara](./media/search-explorer/search-explorer-query-string-example.png "Enter arama koşulları ve Ara")

Arama ipuçları **arama Gezgini**:

+ Belge yapısı ve içeriği tamamen görüntüleyebilmek sonuçları ayrıntılı JSON belgeleri olarak döndürülür. Sorgu ifadeleri, hangi alanların döndürülen sınırı örneklerde gösterilen kullanabilirsiniz.

+ Belgeler olarak işaretlenmiş tüm alanlardan oluşur **alınabilir** dizinde. Portalda dizin özniteliklerini görüntülemek için tıklayın *realestate-us-sample* içinde **dizinleri** arama genel bakış sayfasında listesi.

+ Serbest biçimli sorguları, bir ticari web tarayıcısında ne girebilirsiniz için benzer bir son kullanıcı deneyimi test etmek için kullanışlıdır. Örneğin, yerleşik realestate örnek dizini varsayıldığında, "Seattle apartmanlar lake washington" girebilirsiniz ve arama sonuçlarını içindeki terimler bulmak için Ctrl-F sonra kullanabilirsiniz. 

+ Azure arama tarafından desteklenen bir söz dizimi, sorgu ve filtre ifadeleri geliştirilmiştir gerekir. Varsayılan bir [basit söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search), ancak isteğe bağlı olarak kullanabileceğiniz [tam Lucene](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) daha güçlü sorgular yapmak için. [Filtre ifadelerini](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) bir OData söz dizimini olan.


## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki kaynaklar ek sorgu söz dizimi bilgileri ve örnekler içerir.

 + [Basit sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 
 + [Lucene sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 + [Lucene sorgu örnekleri](search-query-lucene-examples.md) 
 + [OData Filtre ifadesinin söz dizimi](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) 