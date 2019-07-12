---
title: 'Hızlı Başlangıç: Postman ile REST API - Azure Search dizinlerini sorgulamanız oluşturma ve yükleme'
description: Postman, örnek verilerini ve tanımlarını kullanarak Azure Search REST API'lerini çağırma hakkında bilgi edinin.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: quickstart
ms.date: 07/11/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 015dd3631322978d6416041a3eea8390a72b0c17
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840235"
---
# <a name="quickstart-create-an-azure-search-index-in-postman-using-rest-apis"></a>Hızlı Başlangıç: Postman içinde REST API'lerini kullanarak bir Azure Search dizini oluşturma
> [!div class="op_single_selector"]
> * [Postman](search-get-started-postman.md)
> * [C#](search-create-index-dotnet.md)
> * [Python](search-get-started-python.md)
> * [Portal](search-get-started-portal.md)
> * [PowerShell](search-howto-dotnet-sdk.md)
>*

Araştırılacak kolay yollarından biri [Azure Search REST API'lerini](https://docs.microsoft.com/rest/api/searchservice) HTTP isteklerini oluşturmak ve yanıtları için Postman veya başka bir web testi araç kullanarak. Doğru araçlar ve bu yönergelerden yararlanarak herhangi bir kod yazmadan önce istek gönderebilir ve yanıtları görüntüleyebilirsiniz.

Bu makalede, etkileşimli istekleri formüle açıklanmaktadır. Alternatif olarak, [indirmek ve bir Postman koleksiyonunu içeri aktarma](https://github.com/Azure-Samples/azure-search-postman-samples/tree/master/Quickstart) önceden tanımlanmış istekleri kullanmak için.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta, aşağıdaki hizmetler ve Araçlar kullanılır. 

+ [Postman masaüstü uygulaması](https://www.getpostman.com/) istek Azure Search'e göndermek için kullanılır.

+ [Azure Search hizmeti oluşturma](search-create-service-portal.md) veya [mevcut bir hizmet bulma](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) geçerli aboneliğinizdeki. Bu Hızlı Başlangıç için ücretsiz bir hizmet kullanabilirsiniz. 

## <a name="get-a-key-and-url"></a>Bir anahtarı ve URL alma

REST çağrıları için her istekte hizmet URL'sinin ve bir erişim anahtarının iletilmesi gerekir. İkisini de içeren bir arama hizmeti oluşturulur. Bu nedenle aboneliğinize Azure Search hizmetini eklediyseniz gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure portalında oturum açın](https://portal.azure.com/)ve arama hizmetinizdeki **genel bakış** sayfa olduğunda URL'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. İçinde **ayarları** > **anahtarları**, hizmette tam haklarına yönelik bir yönetici anahtarını alın. Bir gece yarısında gerektiği durumlarda iş sürekliliği için sağlanan iki birbirinin yerine yönetici anahtarı mevcuttur. Ekleme, değiştirme ve silme nesneler için istekleri birincil veya ikincil anahtar kullanabilirsiniz.

![Bir HTTP uç noktası ve erişim anahtarını alma](media/search-get-started-postman/get-url-key.png "bir HTTP uç noktası ve erişim anahtarını alma")

Tüm istekleri hizmete gönderilen her istekte bir API anahtarı gerektirir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="connect-to-azure-search"></a>Azure Search'e Bağlan

Bu bölümde, Azure Search bağlantı kurmak için tercih ettiğiniz web aracı kullanın. Her araç yalnızca api anahtarını ve Content-Type bir kez girmeniz gerektiği anlamına gelir oturum için istek üst bilgisini devam ettirir.

Araçtan için (GET, POST, PUT ve benzeri) bir komut seçmek için bir URL uç noktası sağlar ve bazı görevler için isteğin gövdesindeki JSON sağlar. Arama hizmeti adı (YOUR-SEARCH-hizmet-adı), geçerli bir değerle değiştirin. Ekleme `$select=name` yalnızca her dizin adını döndürmek için. 

    https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes?api-version=2019-05-06&$select=name

HTTPS ön ekini, hizmetin adı (Bu durumda, dizinler koleksiyonu), bir nesnenin adını dikkat edin ve [api sürümü](search-api-versions.md). Gerekli, küçük harf dize olarak belirtilen api-version değeri `?api-version=2019-05-06` geçerli sürümü için. API sürümleri düzenli olarak güncelleştirilir. api-version parametresini her isteğe dahil etmeniz hangisinin kullanıldığıyla ilgili tam denetim sahibi olmanızı sağlar.  

İstek üst bilgisi oluşturma, iki öğe, içerik türü ve Azure Search için kimliğini doğrulamak için kullanılan api anahtarını içerir. Yönetici API anahtarını (YOUR-AZURE-SEARCH-ADMIN-API-KEY) geçerli bir değerle değiştirin. 

    api-key: <YOUR-AZURE-SEARCH-ADMIN-API-KEY>
    Content-Type: application/json

Postman içinde aşağıdaki ekran görüntüsü gibi görünen bir istek düzenleyin. Seçin **alma** fiili olarak bir URL girin ve tıklatın **Gönder**. Bu komut Azure Search'e bağlanan dizinler koleksiyonu okur ve başarılı bir bağlantı üzerinde 200 HTTP durum kodunu döndürür. Hizmetinizi dizinleri zaten varsa, yanıtı de dizin tanımlarını içerir.

![Postman isteği URL'si ve üst bilgi](media/search-get-started-postman/postman-url.png "Postman istek URL'si ve üst bilgisi")

## <a name="1---create-an-index"></a>1 - Dizin oluşturma

Azure Search'te verileri yüklemeden önce dizini genellikle oluşturun. [Dizin REST API oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index) bu görev için kullanılır. 

URL içerecek şekilde Genişletilmiş `hotels` dizin adı.

Postman içinde Bunu yapmak için:

1. Değiştirmek için fiil **PUT**.

2. Bu URL'yi kopyalayın `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels?api-version=2019-05-06`.

3. (Kopyalama kullanıma hazır kod aşağıda verilmiştir) dizin tanımını istek gövdesine sağlar.

4. Tıklayın **Gönder**.

![İstek gövdesi dizin JSON belgesinde](media/search-get-started-postman/postman-request.png "istek gövdesindeki dizin JSON belgesi")

### <a name="index-definition"></a>Dizin tanımı

Alanlar koleksiyonu belgenin yapısını tanımlar. Bu alanlar her belge olmalı ve her alanın veri türü olması gerekir. Dize alanları tam metin araması için kullanılır. Bu nedenle içerikte arama yapılabilmesini istiyorsanız sayısal verileri dize olarak ayarlamak isteyebilirsiniz.

Alan öznitelikleri izin verilen eylemi belirler. REST API'leri varsayılan olarak birçok eyleme izin verir. Örneğin tüm dizelerde arama, getirme, filtreleme ve modelleme özellikleri varsayılan olarak etkindir. Genellikle, yalnızca bir davranışı kapatmak istediğinizde özniteliklerini ayarlayın gerekir.

```json
{
    "name": "hotels-quickstart",  
    "fields": [
        {"name": "HotelId", "type": "Edm.String", "key": true, "filterable": true},
        {"name": "HotelName", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": true, "facetable": false},
        {"name": "Description", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "en.lucene"},
        {"name": "Category", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true},
        {"name": "Tags", "type": "Collection(Edm.String)", "searchable": true, "filterable": true, "sortable": false, "facetable": true},
        {"name": "ParkingIncluded", "type": "Edm.Boolean", "filterable": true, "sortable": true, "facetable": true},
        {"name": "LastRenovationDate", "type": "Edm.DateTimeOffset", "filterable": true, "sortable": true, "facetable": true},
        {"name": "Rating", "type": "Edm.Double", "filterable": true, "sortable": true, "facetable": true},
        {"name": "Address", "type": "Edm.ComplexType", 
        "fields": [
        {"name": "StreetAddress", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "searchable": true},
        {"name": "City", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true},
        {"name": "StateProvince", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true},
        {"name": "PostalCode", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true},
        {"name": "Country", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true}
        ]
     }
  ]
}
```

Bu isteği gönderdiğinizde dizinin başarıyla oluşturulduğunu belirten HTTP 201 yanıtı almanız gerekir. Bu eylemi portaldan doğrulayabilirsiniz ancak portal sayfasındaki yenileme aralıkları nedeniyle bilgilerin güncellenmesi bir-iki dakika sürebilir.

> [!TIP]
> HTTP 504 yanıtı alırsanız HTTPS'yi belirten URL'yi doğrulayın. HTTP 400 veya 404 yanıtı görürseniz kopyala-yapıştır hatası olmadığını doğrulamak için istek gövdesini kontrol edin. HTTP 403 genelde api anahtarı ile ilgili bir sorunu gösterir (geçersiz anahtar veya api anahtarının nasıl belirtildiğine ilişkin söz dizimi sorunu).

## <a name="2---load-documents"></a>2 - belge yükleme

Dizini oluşturma ve dizini doldurma ayrı adımlardır. Azure Search'te dizin, arama yapılabilecek ve JSON belgeleri olarak iletebileceğiniz tüm verileri içerir. [Ekleme, güncelleştirme veya silme belgeleri REST API'sini](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) bu görev için kullanılır. 

URL içerecek şekilde Genişletilmiş `docs` koleksiyonları ve `index` işlemi.

Postman içinde Bunu yapmak için:

1. Fiili **POST** olarak değiştirin.

2. Bu URL'yi kopyalayın `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels-quickstart/docs/index?api-version=2019-05-06`.

3. İstek gövdesinde JSON belgelerini (kopya kullanıma hazır kod aşağıda verilmiştir) sağlayın.

4. Tıklayın **Gönder**.

![İstek gövdesindeki JSON belgelerini](media/search-get-started-postman/postman-docs.png "istek gövdesindeki JSON belgeleri")

### <a name="json-documents-to-load-into-the-index"></a>JSON belgeleri dizine yüklemek için

İstek Gövdesi, oteller dizinine eklenecek dört belge içerir.

```json
{
    "value": [
    {
    "@search.action": "upload",
    "HotelId": "1",
    "HotelName": "Secret Point Motel",
    "Description": "The hotel is ideally located on the main commercial artery of the city in the heart of New York. A few minutes away is Time's Square and the historic centre of the city, as well as other places of interest that make New York one of America's most attractive and cosmopolitan cities.",
    "Category": "Boutique",
    "Tags": [ "pool", "air conditioning", "concierge" ],
    "ParkingIncluded": false,
    "LastRenovationDate": "1970-01-18T00:00:00Z",
    "Rating": 3.60,
    "Address": 
        {
        "StreetAddress": "677 5th Ave",
        "City": "New York",
        "StateProvince": "NY",
        "PostalCode": "10022",
        "Country": "USA"
        } 
    },
    {
    "@search.action": "upload",
    "HotelId": "2",
    "HotelName": "Twin Dome Motel",
    "Description": "The hotel is situated in a  nineteenth century plaza, which has been expanded and renovated to the highest architectural standards to create a modern, functional and first-class hotel in which art and unique historical elements coexist with the most modern comforts.",
    "Category": "Boutique",
    "Tags": [ "pool", "free wifi", "concierge" ],
    "ParkingIncluded": false,
    "LastRenovationDate": "1979-02-18T00:00:00Z",
    "Rating": 3.60,
    "Address": 
        {
        "StreetAddress": "140 University Town Center Dr",
        "City": "Sarasota",
        "StateProvince": "FL",
        "PostalCode": "34243",
        "Country": "USA"
        } 
    },
    {
    "@search.action": "upload",
    "HotelId": "3",
    "HotelName": "Triple Landscape Hotel",
    "Description": "The Hotel stands out for its gastronomic excellence under the management of William Dough, who advises on and oversees all of the Hotel’s restaurant services.",
    "Category": "Resort and Spa",
    "Tags": [ "air conditioning", "bar", "continental breakfast" ],
    "ParkingIncluded": true,
    "LastRenovationDate": "2015-09-20T00:00:00Z",
    "Rating": 4.80,
    "Address": 
        {
        "StreetAddress": "3393 Peachtree Rd",
        "City": "Atlanta",
        "StateProvince": "GA",
        "PostalCode": "30326",
        "Country": "USA"
        } 
    },
    {
    "@search.action": "upload",
    "HotelId": "4",
    "HotelName": "Sublime Cliff Hotel",
    "Description": "Sublime Cliff Hotel is located in the heart of the historic center of Sublime in an extremely vibrant and lively area within short walking distance to the sites and landmarks of the city and is surrounded by the extraordinary beauty of churches, buildings, shops and monuments. Sublime Cliff is part of a lovingly restored 1800 palace.",
    "Category": "Boutique",
    "Tags": [ "concierge", "view", "24-hour front desk service" ],
    "ParkingIncluded": true,
    "LastRenovationDate": "1960-02-06T00:00:00Z",
    "Rating": 4.60,
    "Address": 
        {
        "StreetAddress": "7400 San Pedro Ave",
        "City": "San Antonio",
        "StateProvince": "TX",
        "PostalCode": "78216",
        "Country": "USA"
        }
    }
  ]
}
```

Birkaç saniye içinde HTTP 201 yanıtını oturum listesinde görmeniz gerekir. Bu, belgelerin başarıyla oluşturulduğunu belirtir. 

207 yanıtı alırsanız en az bir belge karşıya yüklenemedi. 404 yanıtı alırsanız üst bilgi veya istek gövdesinde söz dizimi hatanız vardır. Uç noktayı `/docs/index` içerecek şekilde değiştirdiğinizden emin olun.

> [!Tip]
> Seçili veri kaynaklarında dizin oluşturma için gerekli kodları sadeleştiren ve miktarını azaltan alternatif *dizin oluşturucu* yaklaşımını kullanabilirsiniz. Daha fazla bilgi için bkz. [Dizin oluşturucu işlemleri](https://docs.microsoft.com/rest/api/searchservice/indexer-operations).


## <a name="3---search-an-index"></a>3 - Dizin arama

Dizin ve belgeler yüklenir, bunlara karşı sorgu iletebilirsiniz kullanarak [arama belgeleri REST API'si](https://docs.microsoft.com/rest/api/searchservice/search-documents).

URL, arama işleci kullanılarak belirtilen bir sorgu ifadesini içerecek şekilde genişletilir.

Postman içinde Bunu yapmak için:

1. Değiştirmek için fiil **alma**.

2. Bu URL'yi kopyalayın `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels-quickstart/docs?search=*&$count=true&api-version=2019-05-06`.

3. Tıklayın **Gönder**.

Bu sorgu, boş ve arama sonuçlarındaki belgelerin sayısını döndürür. Tıkladıktan sonra istek ve yanıt için Postman aşağıdaki ekran görüntüsüne benzer görünmelidir **Gönder**. Durum kodu 200 olmalıdır.

 ![Arama dizesiyle URL'sini alma](media/search-get-started-postman/postman-query.png "arama dizesiyle URL'sini alın")

Bir genel görünüm sözdizimi almak için birkaç diğer sorgu örnekleri deneyin. Verbatim $filter sorgular bir dize arama yapın, kapsam belirli alanları ve daha fazlası için arama sonuçları kümesini sınırlamak.

Geçerli URL aşağıdaki fiyatlarla kullanıma tıklayarak takas **Gönder** sonuçlarını görüntülemek için her zaman.

```
# Query example 1 - Search on restaurant and wifi
# Return only the HotelName, Description, and Tags fields
https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?search=restaurant wifi&$count=true&$select=HotelName,Description,Tags&api-version=2019-05-06

# Query example 2 - Apply a filter to the index to find hotels rated 4 or highter
# Returns the HotelName and Rating. Two documents match
https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?search=*&$filter=Rating gt 4&$select=HotelName,Rating&api-version=2019-05-06

# Query example 3 - Take the top two results, and show only HotelName and Category in the results
https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?search=boutique&$top=2&$select=HotelName,Category&api-version=2019-05-06

# Query example 4 - Sort by a specific field (Address/City) in ascending order
https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?search=pool&$orderby=Address/City asc&$select=HotelName, Address/City, Tags, Rating&api-version=2019-05-06
```

## <a name="get-index-properties"></a>Dizin özelliklerini alma
Ayrıca [istatistikleri alma](https://docs.microsoft.com/rest/api/searchservice/get-index-statistics) sorgulamak için belge sayısını ve boyutunu dizinlemek için: 

```
https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels-quickstart/stats?api-version=2019-05-06`
```

Ekleme `/stats` URL'nize dizin bilgilerini döndürür. Postman uygulamasında isteğinizin aşağıdakine benzer olması ve yanıtta belge sayısı ile kullanılan alanın bayt cinsinden değerinin belirtilmesi gerekir.

 ![Dizin bilgileri Al](media/search-get-started-postman/postman-system-query.png "dizin bilgileri Al")

api-version söz diziminin farklı olduğuna dikkat edin. Bu istek için api-version parametresine `?` ekleyin. `?` URL yolunu Sorgu dizesinden ayırırken & işareti ' ad = değer ' sorgu dizesinde çifti. Bu sorgu için api-version, sorgu dizesindeki ilk ve tek öğedir.

## <a name="clean-up"></a>Temizleme

Kendi aboneliğinizde çalışırken, oluşturduğunuz kaynakları hala gerekip gerekmediğini belirlemek için iyi bir fikir sonunda, bir proje var. Kaynakları sol çalışan can para maliyeti. Kaynakları tek tek silmek ya da tüm kaynak kümesini silmek için kaynak grubunu silin.

Bulabilir ve Portalı'nda kaynaklarını yönetme kullanarak **tüm kaynakları** veya **kaynak grupları** sol gezinti bölmesindeki bağlantıyı.

Ücretsiz bir hizmet kullanıyorsanız, üç dizin, dizin oluşturucular ve veri kaynağı için sınırlı olduğunu unutmayın. Bireysel öğeleri limiti altında kalmak için portalda silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

REST istemcileri plansız keşifler için çok önemlidir. REST API'lerinin nasıl çalıştığını öğrendiğinizde göre kod kullanarak ilerleyebilirsiniz. Sonraki adım için şu bağlantıya bakınız:

+ [Hızlı Başlangıç: .NET SDK'sını kullanarak bir dizin oluşturun](search-get-started-dotnet.md)
