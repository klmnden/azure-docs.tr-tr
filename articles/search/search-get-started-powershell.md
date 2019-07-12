---
title: "PowerShell hızlı başlangıç: Kullanarak Azure Search REST API'lerini - Azure Search dizinlerini sorgulamanız oluşturma ve yükleme"
description: Dizin oluşturma, veri yükleme ve PowerShell'in kullanarak sorguları çalıştırma açıklanmaktadır Invoke-RestMethod ve Azure Search REST API'si.
ms.date: 07/11/2019
author: heidisteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: c8a49fe5d334b5752b9272e480fb2502a980b0a4
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840165"
---
# <a name="quickstart-create-an-azure-search-index-in-powershell-using-rest-apis"></a>Hızlı Başlangıç: REST API'lerini kullanarak PowerShell'de Azure Search dizini oluşturma
> [!div class="op_single_selector"]
> * [PowerShell (REST)](search-create-index-rest-api.md)
> * [C#](search-create-index-dotnet.md)
> * [Postman (REST)](search-get-started-postman.md)
> * [Python](search-get-started-python.md)
> * [Portal](search-create-index-portal.md)
> 

Bu makalede PowerShell kullanarak Azure Search dizini sorgulama oluşturma ve yükleme sürecinde yardımcı olur ve [Azure Search REST API'lerini](https://docs.microsoft.com/rest/api/searchservice/). Bu makalede, PowerShell komutlarını etkileşimli olarak çalışacak şekilde açıklanmaktadır. Alternatif olarak, [karşıdan yükleyip bir Powershell betiği çalıştırmanız](https://github.com/Azure-Samples/azure-search-powershell-samples/tree/master/Quickstart) , aynı işlemleri gerçekleştirir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta, aşağıdaki hizmetler ve Araçlar kullanılır. 

+ [PowerShell 5.1 veya üstü](https://github.com/PowerShell/PowerShell)kullanarak [Invoke-RestMethod](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Utility/Invoke-RestMethod) sıralı ve etkileşimli adımlar.

+ [Azure Search hizmeti oluşturma](search-create-service-portal.md) veya [mevcut bir hizmet bulma](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) geçerli aboneliğinizdeki. Bu Hızlı Başlangıç için ücretsiz bir hizmet kullanabilirsiniz. 

## <a name="get-a-key-and-url"></a>Bir anahtarı ve URL alma

REST çağrıları için her istekte hizmet URL'sinin ve bir erişim anahtarının iletilmesi gerekir. İkisini de içeren bir arama hizmeti oluşturulur. Bu nedenle aboneliğinize Azure Search hizmetini eklediyseniz gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure portalında oturum açın](https://portal.azure.com/)ve arama hizmetinizdeki **genel bakış** sayfa olduğunda URL'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

2. İçinde **ayarları** > **anahtarları**, hizmette tam haklarına yönelik bir yönetici anahtarını alın. Bir gece yarısında gerektiği durumlarda iş sürekliliği için sağlanan iki birbirinin yerine yönetici anahtarı mevcuttur. Ekleme, değiştirme ve silme nesneler için istekleri birincil veya ikincil anahtar kullanabilirsiniz.

![Bir HTTP uç noktası ve erişim anahtarını alma](media/search-get-started-postman/get-url-key.png "bir HTTP uç noktası ve erişim anahtarını alma")

Tüm istekleri hizmete gönderilen her istekte bir API anahtarı gerektirir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="connect-to-azure-search"></a>Azure Search'e Bağlan

1. PowerShell'de, oluşturma bir **$headers** content-type ve API anahtarı depolamak için nesne. Yönetici API anahtarını (YOUR-ADMIN-API-KEY) arama hizmetiniz için geçerli bir anahtarla değiştirin. Yalnızca bu başlığı oturum süresi boyunca ayarlamalı gerekir, ancak her istek için ekleyeceksiniz. 

    ```powershell
    $headers = @{
    'api-key' = '<YOUR-ADMIN-API-KEY>'
    'Content-Type' = 'application/json' 
    'Accept' = 'application/json' }
    ```

2. Oluşturma bir **$url** hizmetin belirten nesne dizinler koleksiyonu. Hizmet adı (YOUR-SEARCH-hizmet-adı), geçerli bir arama hizmeti ile değiştirin.

    ```powershell
    $url = "https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes?api-version=2019-05-06&$select=name"
    ```

3. Çalıştırma **Invoke-RestMethod** hizmetine bir GET isteği gönderir ve bağlantıyı doğrulamak için. Ekleme **ConvertTo-Json** böylece gönderilen geri hizmetinden yanıtları görüntüleyebilirsiniz.

    ```powershell
    Invoke-RestMethod -Uri $url -Headers $headers | ConvertTo-Json
    ```

   Hizmet boştur ve dizin varsa sonuçları aşağıdaki örneğe benzerdir. Aksi takdirde, dizin tanımlarını JSON temsili görürsünüz.

    ```
    {
        "@odata.context":  "https://mydemo.search.windows.net/$metadata#indexes",
        "value":  [

                ]
    }
    ```

## <a name="1---create-an-index"></a>1 - Dizin oluşturma

Portal kullanmıyorsanız, verileri yüklemeden önce bir dizin hizmette mevcut olması gerekir. Bu adım, dizini tanımlayan ve hizmetine gönderir. [Dizin REST API oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index) Bu adım için kullanılır.

Bir dizinin gerekli öğeler, bir ad ve bir alanlar koleksiyonu içerir. Alanlar koleksiyonu yapısını tanımlayan bir *belge*. Her alanın bir adı, türü ve nasıl kullanıldığını belirleyen özniteliklere sahip (örneğin, tam metin olup aranabilir, filtrelenebilir veya arama sonuçlarında alınabilir). Bir dizinin türü alanlardan biri içinde `Edm.String` olarak belirlenmesi gerekir *anahtar* belge kimliği.

Bu dizin, "hotels-quickstart" olarak adlandırılır ve aşağıda gördüğünüz alan tanımı yok. Daha büyük bir alt kümesidir [Oteller dizinini](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/hotels/Hotels_IndexDefinition.JSON) diğer izlenecek yollarında kullanılır. Biz, bu hızlı başlangıçta kısaltma kırpılır.

1. Bu örnek oluşturmak için PowerShell içinde yapıştırın bir **$body** dizin şemasını içeren nesne.

    ```powershell
    $body = @"
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
    "@
    ```

2. Hizmetinizde dizinler koleksiyonu için URI ayarlayın ve *hotels-quickstart* dizini.

    ```powershell
    $url = "https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart?api-version=2019-05-06"
    ```

3. Komutla çalıştırın **$url**, **$headers**, ve **$body** hizmette dizini oluşturmak için. 

    ```powershell
    Invoke-RestMethod -Uri $url -Headers $headers -Method Put -Body $body | ConvertTo-Json
    ```

    Sonuçlar (ilk iki alanı kısaltma kesilmiş) şuna benzer görünmelidir:

    ```
    {
        "@odata.context":  "https://mydemo.search.windows.net/$metadata#indexes/$entity",
        "@odata.etag":  "\"0x8D6EDE28CFEABDA\"",
        "name":  "hotels-quickstart",
        "defaultScoringProfile":  null,
        "fields":  [
                    {
                        "name":  "HotelId",
                        "type":  "Edm.String",
                        "searchable":  true,
                        "filterable":  true,
                        "retrievable":  true,
                        "sortable":  true,
                        "facetable":  true,
                        "key":  true,
                        "indexAnalyzer":  null,
                        "searchAnalyzer":  null,
                        "analyzer":  null,
                        "synonymMaps":  ""
                    },
                    {
                        "name":  "HotelName",
                        "type":  "Edm.String",
                        "searchable":  true,
                        "filterable":  false,
                        "retrievable":  true,
                        "sortable":  true,
                        "facetable":  false,
                        "key":  false,
                        "indexAnalyzer":  null,
                        "searchAnalyzer":  null,
                        "analyzer":  null,
                        "synonymMaps":  ""
                    },
    . . .
    ```

> [!Tip]
> Doğrulama için portal dizinler listesinde denetleyebilirsiniz.

<a name="load-documents"></a>

## <a name="2---load-documents"></a>2 - belge yükleme

Belgeleri göndermek için dizininizin URL uç noktasına bir HTTP POST isteği kullanın. Bu görev için REST API [ekleme, güncelleştirme veya silme belgeleri](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents).

1. Bu örnek oluşturmak için PowerShell içinde yapıştırın bir **$body** karşıya yüklemek istediğiniz belgeleri içeren nesne. 

    Bu istek, iki tam ve kısmi bir kayıt içerir. Eksik belgeler karşıya kısmi kaydı gösterir. `@search.action` Parametresi, dizin oluşturma nasıl yapıldığını belirtir. Geçerli değerler, karşıya yükleme, birleştirme, mergeOrUpload ve silmeyi içerir. MergeOrUpload davranışı hotelId için yeni bir belge ya da oluşturur = 3 veya zaten varsa, içeriği güncelleştirir.

    ```powershell
    $body = @"
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
    "@
    ```

1. Uç nokta kümesine *hotels-quickstart* docs koleksiyonu ve dizin işlemi (dizinleri/hotels-hızlı başlangıç/docs/index) içerir.

    ```powershell
    $url = "https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs/index?api-version=2019-05-06"
    ```

1. Komutla çalıştırın **$url**, **$headers**, ve **$body** hotels-quickstart dizine belgeleri yüklenemedi.

    ```powershell
    Invoke-RestMethod -Uri $url -Headers $headers -Method Post -Body $body | ConvertTo-Json
    ```
    Sonuçları şu örneğe benzemelidir. Görmelisiniz bir [201 durum kodunu](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).

    ```
    {
        "@odata.context":  "https://mydemo.search.windows.net/indexes(\u0027hotels-quickstart\u0027)/$metadata#Collection(Microsoft.Azure.Search.V2019_05_06.IndexResult)",
        "value":  [
                    {
                        "key":  "1",
                        "status":  true,
                        "errorMessage":  null,
                        "statusCode":  201
                    },
                    {
                        "key":  "2",
                        "status":  true,
                        "errorMessage":  null,
                        "statusCode":  201
                    },
                    {
                        "key":  "3",
                        "status":  true,
                        "errorMessage":  null,
                        "statusCode":  201
                    },
                    {
                        "key":  "4",
                        "status":  true,
                        "errorMessage":  null,
                        "statusCode":  201
                    }
                ]
    }
    ```

## <a name="3---search-an-index"></a>3 - Dizin arama

Bu adım bir dizin kullanarak nasıl sorgulanacağını gösterir [arama belgeleri API](https://docs.microsoft.com/rest/api/searchservice/search-documents).

Arama $urls tek tırnak işareti kullanın emin olun. Sorgu dizelerini içerir **$** karakterleri ve çıkarabilirsiniz tüm dize tek tırnak işaretleri arasına alınmışsa, bunları kaçış gerek kalmadan...

1. Uç nokta kümesine *hotels-quickstart* docs koleksiyonu ve ekleme bir **arama** bir sorgu dizesi içinde geçirilecek parametre. 
  
   Bu dize boş bir arama yürütür (arama = *), bir unranked listesi döndüren (arama puanı 1.0 =) rastgele belgeleri. Varsayılan olarak, Azure Search, aynı anda 50 eşleşmesi döndürür. Yapılandırılmış olarak, bu sorgu tüm belge yapısı ve değerleri döndürür. Ekleme **$count = true** sonuçlarda tüm belgelerin sayısını almak için.

    ```powershell
    $url = 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?api-version=2019-05-06&search=*&$count=true'
    ```

1. Göndermek için kullanılan komut çalıştırma **$url** hizmeti.

    ```powershell
    Invoke-RestMethod -Uri $url -Headers $headers | ConvertTo-Json
    ```

    Sonuç aşağıdaki çıktıya benzer olmalıdır.

    ```
    {
    "@odata.context":  "https://mydemo.search.windows.net/indexes(\u0027hotels-quickstart\u0027)/$metadata#docs(*)",
    "@odata.count":  4,
    "value":  [
                  {
                      "@search.score":  0.1547872,
                      "HotelId":  "2",
                      "HotelName":  "Twin Dome Motel",
                      "Description":  "The hotel is situated in a  nineteenth century plaza, which has been expanded and renovated to the highest architectural standards to create a modern, functional and first-class hotel in which art and unique historical elements coexist with the most modern comforts.",
                      "Category":  "Boutique",
                      "Tags":  "pool free wifi concierge",
                      "ParkingIncluded":  false,
                      "LastRenovationDate":  "1979-02-18T00:00:00Z",
                      "Rating":  3.6,
                      "Address":  "@{StreetAddress=140 University Town Center Dr; City=Sarasota; StateProvince=FL; PostalCode=34243; Country=USA}"
                  },
                  {
                      "@search.score":  0.009068266,
                      "HotelId":  "3",
                      "HotelName":  "Triple Landscape Hotel",
                      "Description":  "The Hotel stands out for its gastronomic excellence under the management of William Dough, who advises on and oversees all of the Hotel\u0027s restaurant services.",
                      "Category":  "Resort and Spa",
                      "Tags":  "air conditioning bar continental breakfast",
                      "ParkingIncluded":  true,
                      "LastRenovationDate":  "2015-09-20T00:00:00Z",
                      "Rating":  4.8,
                      "Address":  "@{StreetAddress=3393 Peachtree Rd; City=Atlanta; StateProvince=GA; PostalCode=30326; Country=USA}"
                  },
                . . . 
    ```

Bir genel görünüm sözdizimi almak için birkaç diğer sorgu örnekleri deneyin. Verbatim $filter sorgular bir dize arama yapın, kapsam belirli alanları ve daha fazlası için arama sonuçları kümesini sınırlamak.

```powershell
# Query example 1
# Search the entire index for the terms 'restaurant' and 'wifi'
# Return only the HotelName, Description, and Tags fields
$url = 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?api-version=2019-05-06&search=restaurant wifi&$count=true&$select=HotelName,Description,Tags'

# Query example 2 
# Apply a filter to the index to find hotels rated 4 or highter
# Returns the HotelName and Rating. Two documents match.
$url = 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?api-version=2019-05-06&search=*&$filter=Rating gt 4&$select=HotelName,Rating'

# Query example 3
# Take the top two results, and show only HotelName and Category in the results
$url = 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?api-version=2019-05-06&search=boutique&$top=2&$select=HotelName,Category'

# Query example 4
# Sort by a specific field (Address/City) in ascending order

$url = 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?api-version=2019-05-06&search=pool&$orderby=Address/City asc&$select=HotelName, Address/City, Tags, Rating'
```
## <a name="clean-up"></a>Temizleme 

Kendi aboneliğinizde çalışırken, oluşturduğunuz kaynakları hala gerekip gerekmediğini belirlemek için iyi bir fikir sonunda, bir proje var. Kaynakları sol çalışan can para maliyeti. Kaynakları tek tek silmek ya da tüm kaynak kümesini silmek için kaynak grubunu silin.

Bulabilir ve Portalı'nda kaynaklarını yönetme kullanarak **tüm kaynakları** veya **kaynak grupları** sol gezinti bölmesindeki bağlantıyı.

Ücretsiz bir hizmet kullanıyorsanız, üç dizin, dizin oluşturucular ve veri kaynağı için sınırlı olduğunu unutmayın. Bireysel öğeleri limiti altında kalmak için portalda silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure Search içerik erişmek ve oluşturmak için temel iş akışı aracılığıyla adım için PowerShell kullanılır. Azure veri kaynağından dizin oluşturma gibi daha gelişmiş senaryoları geçmeden aklınızda kavramlarla öneririz;

> [!div class="nextstepaction"]
> [REST Öğreticisi: Dizin ve yarı yapılandırılmış verileri (JSON blobları) Azure Search'te arama](search-semi-structured-data.md)