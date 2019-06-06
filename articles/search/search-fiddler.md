---
title: "Hızlı Başlangıç: Postman ve REST API'ler - Azure Search'ü"
description: Postman, örnek verilerini ve tanımlarını kullanarak Azure Search REST API'lerini çağırma hakkında bilgi edinin.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: quickstart
ms.date: 05/16/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: eaf594286e5ffc101ad2d24e808fcb806998d053
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66471569"
---
# <a name="quickstart-explore-azure-search-rest-apis-using-postman"></a>Hızlı Başlangıç: Postman kullanarak Azure Search REST API'lerini keşfetme
> [!div class="op_single_selector"]
> * [Postman](search-fiddler.md)
> * [C#](search-create-index-dotnet.md)
> * [Python](search-get-started-python.md)
> * [Portal](search-get-started-portal.md)
> * [PowerShell](search-howto-dotnet-sdk.md)
>*

Araştırılacak kolay yollarından biri [Azure Search REST API'lerini](https://docs.microsoft.com/rest/api/searchservice) HTTP isteklerini oluşturmak ve yanıtları için Postman veya başka bir web testi araç kullanarak. Doğru araçlar ve bu yönergelerden yararlanarak herhangi bir kod yazmadan önce istek gönderebilir ve yanıtları görüntüleyebilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun ve [Azure Search hizmetine kaydolun](search-create-service-portal.md).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta, aşağıdaki hizmetler ve Araçlar kullanılır. 

+ [Azure Search hizmeti oluşturma](search-create-service-portal.md) veya [mevcut bir hizmet bulma](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) geçerli aboneliğinizdeki. Bu Hızlı Başlangıç için ücretsiz bir hizmet kullanabilirsiniz. 

+ [Postman masaüstü uygulaması](https://www.getpostman.com/) veya [Telerik Fiddler](https://www.telerik.com/fiddler) istek Azure Search'e göndermek için kullanılır.

## <a name="get-a-key-and-url"></a>Bir anahtarı ve URL alma

REST çağrıları için her istekte hizmet URL'sinin ve bir erişim anahtarının iletilmesi gerekir. İkisini de içeren bir arama hizmeti oluşturulur. Bu nedenle aboneliğinize Azure Search hizmetini eklediyseniz gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure portalında oturum açın](https://portal.azure.com/)ve arama hizmetinizdeki **genel bakış** sayfa olduğunda URL'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. İçinde **ayarları** > **anahtarları**, hizmette tam haklarına yönelik bir yönetici anahtarını alın. Bir gece yarısında gerektiği durumlarda iş sürekliliği için sağlanan iki birbirinin yerine yönetici anahtarı mevcuttur. Ekleme, değiştirme ve silme nesneler için istekleri birincil veya ikincil anahtar kullanabilirsiniz.

![Bir HTTP uç noktası ve erişim anahtarını alma](media/search-fiddler/get-url-key.png "bir HTTP uç noktası ve erişim anahtarını alma")

Tüm istekleri hizmete gönderilen her istekte bir API anahtarı gerektirir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="connect-to-azure-search"></a>Azure Search'e Bağlan

Bu bölümde, Azure Search bağlantı kurmak için tercih ettiğiniz web aracı kullanın. Her araç yalnızca api anahtarını ve Content-Type bir kez girmeniz gerektiği anlamına gelir oturum için istek üst bilgisini devam ettirir.

Araçtan için (GET, POST, PUT ve benzeri) bir komut seçmek için bir URL uç noktası sağlar ve bazı görevler için isteğin gövdesindeki JSON sağlar. Arama hizmeti adı (YOUR-SEARCH-hizmet-adı), geçerli bir değerle değiştirin. 

    https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes?api-version=2019-05-06

HTTPS ön ekini, hizmetin adı (Bu durumda, dizinler koleksiyonu), bir nesnenin adını dikkat edin ve [api sürümü](search-api-versions.md). Gerekli, küçük harf dize olarak belirtilen api-version değeri `?api-version=2019-05-06` geçerli sürümü için. API sürümleri düzenli olarak güncelleştirilir. api-version parametresini her isteğe dahil etmeniz hangisinin kullanıldığıyla ilgili tam denetim sahibi olmanızı sağlar.  

İstek üst bilgisi oluşturma, iki öğe, içerik türü ve Azure Search için kimliğini doğrulamak için kullanılan api anahtarını içerir. Yönetici API anahtarını (YOUR-ADMIN-API-KEY) geçerli bir değerle değiştirin. 

    api-key: <YOUR-ADMIN-API-KEY>
    Content-Type: application/json

Postman içinde aşağıdaki ekran görüntüsü gibi görünen bir istek düzenleyin. Seçin **alma** fiili olarak bir URL girin ve tıklatın **Gönder**. Bu komut Azure Search'e bağlanan dizinler koleksiyonu okur ve başarılı bir bağlantı üzerinde 200 HTTP durum kodunu döndürür. Hizmetinizi dizinleri zaten varsa, yanıtı de dizin tanımlarını içerir.

![Postman isteği üst bilgisi][6]

## <a name="1---create-an-index"></a>1 - Dizin oluşturma

Azure Search'te verileri yüklemeden önce dizini genellikle oluşturun. [Dizin REST API oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index) bu görev için kullanılır. 

URL içerecek şekilde Genişletilmiş `hotels` dizin adı.

Postman içinde Bunu yapmak için:

1. Değiştirmek için fiil **PUT**.

2. Bu URL'yi kopyalayın `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels?api-version=2019-05-06`.

3. (Aşağıda istek gövdesinde gösterilmiştir) dizin tanımını sağlar.

4. Tıklayın **Gönder**.

![Postman isteği gövdesi][8]

### <a name="index-definition"></a>Dizin tanımı

Alanlar koleksiyonu belgenin yapısını tanımlar. Bu alanlar her belge olmalı ve her alanın veri türü olması gerekir. Dize alanları tam metin araması için kullanılır. Bu nedenle içerikte arama yapılabilmesini istiyorsanız sayısal verileri dize olarak ayarlamak isteyebilirsiniz.

Alan öznitelikleri izin verilen eylemi belirler. REST API'leri varsayılan olarak birçok eyleme izin verir. Örneğin tüm dizelerde arama, getirme, filtreleme ve modelleme özellikleri varsayılan olarak etkindir. Genellikle, yalnızca bir davranışı kapatmak istediğinizde özniteliklerini ayarlayın gerekir.

          {
         "name": "hotels",  
         "fields": [
           {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
           {"name": "baseRate", "type": "Edm.Double"},
           {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
           {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
           {"name": "hotelName", "type": "Edm.String"},
           {"name": "category", "type": "Edm.String"},
           {"name": "tags", "type": "Collection(Edm.String)"},
           {"name": "parkingIncluded", "type": "Edm.Boolean"},
           {"name": "smokingAllowed", "type": "Edm.Boolean"},
           {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
           {"name": "rating", "type": "Edm.Int32"},
           {"name": "location", "type": "Edm.GeographyPoint"}
          ]
         }

Bu isteği gönderdiğinizde dizinin başarıyla oluşturulduğunu belirten HTTP 201 yanıtı almanız gerekir. Bu eylemi portaldan doğrulayabilirsiniz ancak portal sayfasındaki yenileme aralıkları nedeniyle bilgilerin güncellenmesi bir-iki dakika sürebilir.

> [!TIP]
> HTTP 504 yanıtı alırsanız HTTPS'yi belirten URL'yi doğrulayın. HTTP 400 veya 404 yanıtı görürseniz kopyala-yapıştır hatası olmadığını doğrulamak için istek gövdesini kontrol edin. HTTP 403 genelde api anahtarı ile ilgili bir sorunu gösterir (geçersiz anahtar veya api anahtarının nasıl belirtildiğine ilişkin söz dizimi sorunu).

## <a name="2---load-documents"></a>2 - belge yükleme

Dizini oluşturma ve dizini doldurma ayrı adımlardır. Azure Search'te dizin, arama yapılabilecek ve JSON belgeleri olarak iletebileceğiniz tüm verileri içerir. [Ekleme, güncelleştirme veya silme belgeleri REST API'sini](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) bu görev için kullanılır. 

URL içerecek şekilde Genişletilmiş `docs` koleksiyonları ve `index` işlemi.

Postman içinde Bunu yapmak için:

1. Fiili **POST** olarak değiştirin.

2. Bu URL'yi kopyalayın `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels/docs/index?api-version=2019-05-06`.

3. JSON belgelerini (aşağıdaki istek gövdesinde gösterilmiştir) sağlayın.

4. Tıklayın **Gönder**.

![Postman isteği yükü][10]

### <a name="json-documents-to-load-into-the-index"></a>JSON belgeleri dizine yüklemek için

İstek Gövdesi, oteller dizinine eklenecek dört belge içerir.

         {
         "value": [
         {
             "@search.action": "upload",
             "hotelId": "1",
             "baseRate": 199.0,
             "description": "Best hotel in town",
             "description_fr": "Meilleur hôtel en ville",
             "hotelName": "Fancy Stay",
             "category": "Luxury",
             "tags": ["pool", "view", "wifi", "concierge"],
             "parkingIncluded": false,
             "smokingAllowed": false,
             "lastRenovationDate": "2010-06-27T00:00:00Z",
             "rating": 5,
             "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
           },
           {
             "@search.action": "upload",
             "hotelId": "2",
             "baseRate": 79.99,
             "description": "Cheapest hotel in town",
             "description_fr": "Hôtel le moins cher en ville",
             "hotelName": "Roach Motel",
             "category": "Budget",
             "tags": ["motel", "budget"],
             "parkingIncluded": true,
             "smokingAllowed": true,
             "lastRenovationDate": "1982-04-28T00:00:00Z",
             "rating": 1,
             "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
           },
           {
             "@search.action": "upload",
             "hotelId": "3",
             "baseRate": 279.99,
             "description": "Surprisingly expensive",
             "hotelName": "Dew Drop Inn",
             "category": "Bed and Breakfast",
             "tags": ["charming", "quaint"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
           },
           {
             "@search.action": "upload",
             "hotelId": "4",
             "baseRate": 220.00,
             "description": "This could be the one",
             "hotelName": "A Hotel for Everyone",
             "category": "Basic hotel",
             "tags": ["pool", "wifi"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
           }
          ]
         }

Birkaç saniye içinde, oturum listesinde bir HTTP 200 yanıtı görmeniz gerekir. Bu, belgelerin başarıyla oluşturulduğunu belirtir. 

207 yanıtı alırsanız en az bir belge karşıya yüklenemedi. 404 yanıtı alırsanız üst bilgi veya istek gövdesinde söz dizimi hatanız vardır. Uç noktayı `/docs/index` içerecek şekilde değiştirdiğinizden emin olun.

> [!Tip]
> Seçili veri kaynaklarında dizin oluşturma için gerekli kodları sadeleştiren ve miktarını azaltan alternatif *dizin oluşturucu* yaklaşımını kullanabilirsiniz. Daha fazla bilgi için bkz. [Dizin oluşturucu işlemleri](https://docs.microsoft.com/rest/api/searchservice/indexer-operations).


## <a name="3---search-an-index"></a>3 - Dizin arama

Dizin ve belgeler yüklenir, bunlara karşı sorgu iletebilirsiniz kullanarak [arama belgeleri REST API'si](https://docs.microsoft.com/rest/api/searchservice/search-documents).

URL, arama işleci kullanılarak belirtilen bir sorgu dizesi dahil etmek için genişletilir.

Postman içinde Bunu yapmak için:

1. Değiştirmek için fiil **alma**.

2. Bu URL'yi kopyalayın `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels/docs?search=motel&$count=true&api-version=2019-05-06`.

3. Tıklayın **Gönder**.

Bu sorgu "motel" terimini arar ve arama sonuçlarındaki belgelerin sayısını döndürür. Tıkladıktan sonra istek ve yanıt için Postman aşağıdaki ekran görüntüsüne benzer görünmelidir **Gönder**. Durum kodu 200 olmalıdır.

 ![Postman sorgusu yanıtı][11]


## <a name="get-index-properties"></a>Dizin özelliklerini alma
Ayrıca, belge sayısını ve depolama tüketimini almak için sistem bilgilerini sorgulayabilirsiniz: `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels/stats?api-version=2019-05-06`

Postman uygulamasında isteğinizin aşağıdakine benzer olması ve yanıtta belge sayısı ile kullanılan alanın bayt cinsinden değerinin belirtilmesi gerekir.

 ![Postman sistem sorgusu][12]

api-version söz diziminin farklı olduğuna dikkat edin. Bu istek için api-version parametresine `?` ekleyin. `?` URL yolunu Sorgu dizesinden ayırırken & işareti ' ad = değer ' sorgu dizesinde çifti. Bu sorgu için api-version, sorgu dizesindeki ilk ve tek öğedir.

Bu API hakkında daha fazla bilgi için bkz. [alma dizin istatistiklerini REST API](https://docs.microsoft.com/rest/api/searchservice/get-index-statistics).


## <a name="use-fiddler"></a>Fiddler'ı kullanma

Bu bölümde, önceki bölümlerde, Fiddler ekran görüntüleri ve yönergeleri ile eşdeğerdir

### <a name="connect-to-azure-search"></a>Azure Search'e Bağlan

Aşağıdaki ekran görüntüsü gibi görünen bir istek düzenleyin. Seçin **alma** fiili olarak. Fiddler `User-Agent=Fiddler` ekler. Altındaki yeni satırlara iki ek istek üst bilgisi yapıştırabilirsiniz. Hizmetinize ait yönetici erişim anahtarını kullanarak hizmetinizin içerik türünü ve API anahtarını dahil edin.

Hedef için bu URL'yi değiştirilmiş bir sürümünü kopyalayın: `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes?api-version=2019-05-06`

![Fiddler isteği üst bilgisi][1]

> [!Tip]
> Fazlalık, ilgisiz HTTP etkinliğini gizlemek için web trafiği devre dışı bırakın. Fiddler'ın içinde **dosya** menüsünden devre dışı **trafik yakalama**. 

### <a name="1---create-an-index"></a>1 - Dizin oluşturma

Değiştirmek için fiil **PUT**. Bu URL'yi değiştirilmiş bir sürümünü kopyasında: `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels?api-version=2019-05-06`. Dizin tanımını istek gövdesine yukarıda sağlanan kopyalayın. Sayfanız aşağıdaki ekran görüntüsüne benzer görünmelidir. Tıklayın **yürütme** üst sağ tamamlanan isteği gönderin.

![Fiddler isteği gövdesi][7]

### <a name="2---load-documents"></a>2 - belge yükleme

Fiili **POST** olarak değiştirin. URL'yi `/docs/index` içerecek şekilde değiştirin. İstek gövdesi, aşağıdaki ekran görüntüsüne benzer belgeler kopyalayın ve ardından isteği yürütün.

![Fiddler isteği yükü][9]

### <a name="tips-for-running-our-sample-queries-in-fiddler"></a>Örnek sorgularımızı Fiddler'da çalıştırma ipuçları

Aşağıdaki örnek sorgu [arama belgeleri REST API'si](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) makalesi. Bu makaledeki örnek sorguların çoğu, Fiddler'da izin verilmeyen boşluklar içerir. Sorgu dizesini yapıştırmadan ve Fiddler'da sorguyu denemeden önce, her bir boşluğu + karakteri ile değiştirin.

**Boşluklar değiştirilmeden önce (in lastRenovationDate desc):**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2019-05-06

**Boşluklar + ile değiştirildikten sonra (in lastRenovationDate+desc):**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2019-05-06

### <a name="tips-for-viewing-index-statistic-in-fiddler"></a>Dizin istatistiklerini Fiddler'da görüntülemek için ipuçları

Fiddler'da **Inspectors** (Denetçiler) sekmesine tıklayın, **Headers** (Üst Bilgiler) sekmesine tıklayın ve ardından JSON biçimini seçin. Belge sayısını ve depolama boyutunu (KB) görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

REST istemcileri plansız keşifler için çok önemlidir. REST API'lerinin nasıl çalıştığını öğrendiğinizde göre kod kullanarak ilerleyebilirsiniz. Sonraki adımları için aşağıdaki bağlantılara bakın:

+ [Hızlı Başlangıç: .NET SDK'sını kullanarak bir dizin oluşturun](search-create-index-dotnet.md)
+ [Hızlı Başlangıç: PowerShell kullanarak bir dizini (REST) oluşturma](search-create-index-rest-api.md)

<!--Image References-->
[1]: ./media/search-fiddler/fiddler-url.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png
[6]: ./media/search-fiddler/postman-url.png
[7]: ./media/search-fiddler/fiddler-request.png
[8]: ./media/search-fiddler/postman-request.png
[9]: ./media/search-fiddler/fiddler-docs.png
[10]: ./media/search-fiddler/postman-docs.png
[11]: ./media/search-fiddler/postman-query.png
[12]: ./media/search-fiddler/postman-system-query.png
