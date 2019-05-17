---
title: "Hızlı Başlangıç: Python ve REST API'ler - Azure Search'ü"
description: Oluşturma, yükleme ve Python, Jupyter not defterleri ve Azure Search REST API'sini kullanarak dizin sorgulama.
ms.date: 05/15/2019
author: heidisteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: 1ab6bb069f60f4d2dbb4cfaecda54c3c2ef20adc
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65806439"
---
# <a name="quickstart-create-an-azure-search-index-using-jupyter-python-notebooks"></a>Hızlı Başlangıç: Jupyter Python not defterlerini kullanarak bir Azure Search dizini oluşturma
> [!div class="op_single_selector"]
> * [Python (REST)](search-get-started-python.md)
> * [PowerShell (REST)](search-create-index-rest-api.md)
> * [C#](search-create-index-dotnet.md)
> * [Postman (REST)](search-fiddler.md)
> * [Portal](search-create-index-portal.md)
> 

Jupyter not defteri oluşturur, yükler ve Azure Search sorgular derleme [dizin](search-what-is-an-index.md) Python kullanarak ve [Azure arama hizmeti REST API'lerini](https://docs.microsoft.com/rest/api/searchservice/). Bu makalede, derleme Not adım adım açıklanmaktadır. İsteğe bağlı olarak, tamamlanmış bir not defteri çalıştırabilirsiniz. Bir kopyasını indirmek için Git [Azure-Search-python-samples deposuna](https://github.com/Azure-Samples/azure-search-python-samples).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun ve [Azure Search hizmetine kaydolun](search-create-service-portal.md).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta, aşağıdaki hizmetler ve Araçlar kullanılır. 

+ [Azure Search hizmeti oluşturma](search-create-service-portal.md) veya [mevcut bir hizmet bulma](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) geçerli aboneliğinizdeki. Bu Hızlı Başlangıç için ücretsiz bir hizmet kullanabilirsiniz. 

+ [Anaconda 3.x](https://www.anaconda.com/distribution/#download-section), sağlama Python 3.x ve Jupyter not defterleri.

## <a name="get-a-key-and-url"></a>Bir anahtarı ve URL alma

REST çağrıları için her istekte hizmet URL'sinin ve bir erişim anahtarının iletilmesi gerekir. İkisini de içeren bir arama hizmeti oluşturulur. Bu nedenle aboneliğinize Azure Search hizmetini eklediyseniz gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure portalında oturum açın](https://portal.azure.com/)ve arama hizmetinizdeki **genel bakış** sayfa olduğunda URL'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. İçinde **ayarları** > **anahtarları**, hizmette tam haklarına yönelik bir yönetici anahtarını alın. Bir gece yarısında gerektiği durumlarda iş sürekliliği için sağlanan iki birbirinin yerine yönetici anahtarı mevcuttur. Ekleme, değiştirme ve silme nesneler için istekleri birincil veya ikincil anahtar kullanabilirsiniz.

![Bir HTTP uç noktası ve erişim anahtarını alma](media/search-fiddler/get-url-key.png "bir HTTP uç noktası ve erişim anahtarını alma")

Tüm istekleri hizmete gönderilen her istekte bir API anahtarı gerektirir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="connect-to-azure-search"></a>Azure Search'e Bağlan

Jupyter not defterini açın ve yerel iş istasyonunuzu bağlantısından hizmetinizde dizin listesi isteyerek doğrulayın. Anaconda3 ile Windows Anaconda Gezgin bir not defteri başlatmak için kullanabilirsiniz.

1. Yeni bir Python3 not defteri oluşturun.

1. İlk hücrenin JSON ile çalışma ve HTTP isteklerini formulating için kullanılan kitaplıkları yükleyin.

   ```python
   import json
   import requests
   from pprint import pprint
   ```

1. İkinci hücresinde her istekte sabitleri olacak istek öğeleri girin. Arama hizmeti adı (YOUR-SEARCH-hizmet-adı) ve yönetici API anahtarını (YOUR-ADMIN-API-KEY) geçerli değerlerle değiştirin. 

   ```python
    endpoint = 'https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/'
    api_version = '?api-version=2019-05-06'
    headers = {'Content-Type': 'application/json',
           'api-key': '<YOUR-ADMIN-API-KEY>' }
   ```

1. Üçüncü hücrede istek düzenleyin. Bu GET isteği, arama hizmetinizin dizinler koleksiyonu hedefleyen ve ad özelliği seçer.

   ```python
   url = endpoint + "indexes" + api_version + "&$select=name"
   response  = requests.get(url, headers=headers)
   index_list = response.json()
   pprint(index_list)
   ```

1. Her adım çalıştırın. Dizin yoksa, yanıt dizinlerin bir listesini içerir. Aşağıdaki ekran görüntüsünde, azureblob dizini ve realestate-us-sample dizin hizmeti içerir.

   ![Python betiğini Jupyter not defteri ile HTTP istekleri için Azure Search](media/search-get-started-python/connect-azure-search.png "Python betiğini Jupyter not defteri ile HTTP istekleri için Azure Search")

   Boş dizin koleksiyonu bu yanıtı döndürür: `{'@odata.context': 'https://mydemo.search.windows.net/$metadata#indexes(name)', 'value': []}`

> [!Tip]
> Ücretsiz bir hizmet üç dizin, dizin oluşturucular ve veri kaynakları için sınırlı olursunuz. Bu hızlı başlangıçta her birini oluşturur. Daha fazla işlenmesini önce yeni nesneler oluşturmak için yer olduğundan emin olun.

## <a name="1---create-an-index"></a>1 - Dizin oluşturma

Portal kullanmıyorsanız, verileri yüklemeden önce bir dizin hizmette mevcut olması gerekir. Bu adımı kullanan [dizin REST API oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index) bir dizin şemasını hizmetine göndermek için

Alanlar koleksiyonu yapısını tanımlayan bir *belge*. Bir dizinin gerekli öğeler, bir ad ve bir alanlar koleksiyonu içerir. Her alanın bir adı, türü ve nasıl kullanıldığını belirleyen özniteliklere sahip (örneğin, tam metin olup aranabilir, filtrelenebilir veya arama sonuçlarında alınabilir). Bir dizinin türü alanlardan biri içinde `Edm.String` olarak belirlenmesi gerekir *anahtar* belge kimliği.

Bu dizin, "hotels-py" olarak adlandırılır ve aşağıda gördüğünüz alan tanımlarına sahip. Daha büyük bir alt kümesidir [Oteller dizinini](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/hotels/Hotels_IndexDefinition.JSON) diğer izlenecek yollarında kullanılır. Biz, bu hızlı başlangıçta kısaltma kırpılır.


1. Sonraki hücreye aşağıdaki örnekte şema sağlamak üzere bir hücreye yapıştırın. 

    ```python
    index_schema = {
       "name": "hotels-py",  
       "fields": [
         {"name": "HotelId", "type": "Edm.String", "key": "true", "filterable": "true"},
         {"name": "HotelName", "type": "Edm.String", "searchable": "true", "filterable": "false", "sortable": "true", "facetable": "false"},
         {"name": "Description", "type": "Edm.String", "searchable": "true", "filterable": "false", "sortable": "false", "facetable": "false", "analyzer": "en.lucene"},
         {"name": "Description_fr", "type": "Edm.String", "searchable": "true", "filterable": "false", "sortable": "false", "facetable": "false", "analyzer": "fr.lucene"},
         {"name": "Category", "type": "Edm.String", "searchable": "true", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "Tags", "type": "Collection(Edm.String)", "searchable": "true", "filterable": "true", "sortable": "false", "facetable": "true"},
         {"name": "ParkingIncluded", "type": "Edm.Boolean", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "LastRenovationDate", "type": "Edm.DateTimeOffset", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "Rating", "type": "Edm.Double", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "Address", "type": "Edm.ComplexType", 
         "fields": [
         {"name": "StreetAddress", "type": "Edm.String", "filterable": "false", "sortable": "false", "facetable": "false", "searchable": "true"},
         {"name": "City", "type": "Edm.String", "searchable": "true", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "StateProvince", "type": "Edm.String", "searchable": "true", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "PostalCode", "type": "Edm.String", "searchable": "true", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "Country", "type": "Edm.String", "searchable": "true", "filterable": "true", "sortable": "true", "facetable": "true"}
        ]
       }
      ]
    }
    ```

2. Başka bir hücreye istek düzenleyin. Bu PUT İsteği, arama hizmetinizin dizinler koleksiyonu hedefleyen ve önceki adımda sağladığınız dizin şemasını bağlı bir dizin oluşturur.

   ```python
   url = endpoint + "indexes" + api_version
   response  = requests.post(url, headers=headers, json=index_schema)
   index = response.json()
   pprint(index)
   ```

3. Her adım çalıştırın.

   Yanıt şeması JSON gösterimini içerir. Daha fazla yanıt görmenize olanak tanıyan aşağıdaki ekran görüntüsünde dizin şemasını bölümlerini kırpar.

    ![Dizin oluşturma isteği](media/search-get-started-python/create-index.png "dizin oluşturma isteği")

> [!Tip]
> Doğrulama için ayrıca Portalı'nda dizinleri listeyi kontrol edin, veya görmek için hizmet bağlantı isteğini yeniden *hotels-py* dizin dizinler koleksiyonu içinde listelenen.

<a name="load-documents"></a>

## <a name="2---load-documents"></a>2 - belge yükleme

Belgeleri göndermek için dizininizin URL uç noktasına bir HTTP POST isteği kullanın. REST API [ekleme, güncelleştirme veya silme belgeleri](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents). Belgeler kaynaklanan gelen [HotelsData](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/hotels/HotelsData_toAzureSearch.JSON) GitHub üzerinde.

1. Yeni bir hücreye dizin şemaya uygun üç belgeler sağlar. Her belge için karşıya yükleme eylemi belirtin.

    ```python
    documents = {
        "value": [
        {
        "@search.action": "upload",
        "HotelId": "1",
        "HotelName": "Secret Point Motel",
        "Description": "The hotel is ideally located on the main commercial artery of the city in the heart of New York. A few minutes away is Time's Square and the historic centre of the city, as well as other places of interest that make New York one of America's most attractive and cosmopolitan cities.",
        "Description_fr": "L'hôtel est idéalement situé sur la principale artère commerciale de la ville en plein cœur de New York. A quelques minutes se trouve la place du temps et le centre historique de la ville, ainsi que d'autres lieux d'intérêt qui font de New York l'une des villes les plus attractives et cosmopolites de l'Amérique.",
        "Category": "Boutique",
        "Tags": [ "pool", "air conditioning", "concierge" ],
        "ParkingIncluded": "false",
        "LastRenovationDate": "1970-01-18T00:00:00Z",
        "Rating": 3.60,
        "Address": {
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
        "Description_fr": "L'hôtel est situé dans une place du XIXe siècle, qui a été agrandie et rénovée aux plus hautes normes architecturales pour créer un hôtel moderne, fonctionnel et de première classe dans lequel l'art et les éléments historiques uniques coexistent avec le confort le plus moderne.",
        "Category": "Boutique",
        "Tags": [ "pool", "free wifi", "concierge" ],
        "ParkingIncluded": "false",
        "LastRenovationDate": "1979-02-18T00:00:00Z",
        "Rating": 3.60,
        "Address": {
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
        "Description_fr": "L'hôtel est situé dans une place du XIXe siècle, qui a été agrandie et rénovée aux plus hautes normes architecturales pour créer un hôtel moderne, fonctionnel et de première classe dans lequel l'art et les éléments historiques uniques coexistent avec le confort le plus moderne.",
        "Category": "Resort and Spa",
        "Tags": [ "air conditioning", "bar", "continental breakfast" ],
        "ParkingIncluded": "true",
        "LastRenovationDate": "2015-09-20T00:00:00Z",
        "Rating": 4.80,
        "Address": {
            "StreetAddress": "3393 Peachtree Rd",
            "City": "Atlanta",
            "StateProvince": "GA",
            "PostalCode": "30326",
            "Country": "USA"
        }
      }
     ]
    }
    ```

2. Başka bir hücreye istek düzenleyin. Bu POST isteği, Oteller-py dizini docs koleksiyonunu hedefleyen ve önceki adımda sağlanan belgelerinin gönderir.

   ```python
   url = endpoint + "indexes/hotels-py/docs/index" + api_version
   response  = requests.post(url, headers=headers, json=documents)
   index_content = response.json()
   pprint(index_content)
   ```

3. Belgeleri arama hizmetinizdeki dizin göndermek için her adımı çalıştırın. Sonuçları şu örneğe benzemelidir. 

   ```
   {'@odata.context': "https://mydemo.search.windows.net/indexes('hotels-py')/$metadata#Collection(Microsoft.Azure.Search.V2019_05_06.IndexResult)",
    'value': [{'errorMessage': None,
            'key': '1',
            'status': True,
            'statusCode': 201},
           {'errorMessage': None,
            'key': '2',
            'status': True,
            'statusCode': 201},
           {'errorMessage': None,
            'key': '3',
            'status': True,
            'statusCode': 201}]}
     ```


## <a name="3---search-an-index"></a>3 - Dizin arama

Bu adım bir dizin kullanarak nasıl sorgulanacağını gösterir [arama belgeleri REST API'si](https://docs.microsoft.com/rest/api/searchservice/search-documents).


1. Yeni bir hücreye bir sorgu ifadesini girin. Aşağıdaki örnek koşulları "hotels" ve "wifi" arar. Ayrıca döndürür bir *sayısı* eşleşen, belgelerin ve *seçer* hangi alanların arama sonuçlarına dahil.

   ```python
   searchstring = '&search=hotels wifi&$count=true&$select=HotelId,HotelName'
   ```

2. Bir istek düzenleyin. Bu GET isteği, Oteller-py dizini docs koleksiyonunu hedefleyen ve önceki adımda belirtilen sorgu ekler.

   ```python
   url = endpoint + "indexes/hotels-py/docs" + api_version + searchstring
   response  = requests.get(url, headers=headers, json=searchstring)
   query = response.json()
   pprint(query)
   ```

   Sonuç aşağıdaki çıktıya benzer olmalıdır.

   ```
   {'@odata.context': "https://mydemo.search.windows.net/indexes('hotels-py')/$metadata#docs(*)",
    '@odata.count': 3,
    'value': [{'@search.score': 1.0,
               'HotelId': '1',
               'HotelName': 'Secret Point Motel'},
              {'@search.score': 1.0,
               'HotelId': '2',
               'HotelName': 'Twin Dome Motel'},
              {'@search.score': 1.0,
               'HotelId': '3',
               'HotelName': 'Triple Landscape Hotel'}]}
   ```

3. Bir genel görünüm sözdizimi almak için birkaç diğer sorgu örnekleri deneyin. Bir filtre uygulamak, ilk iki sonucu alın, belirli bir alana göre sıralamak veya 

   + `searchstring = '&search=*&$filter=Rating gt 4&$select=HotelId,HotelName,Description'`

   + `searchstring = '&search=hotel&$top=2&$select=HotelId,HotelName,Description'`

   + `searchstring = '&search=pool&$orderby=Address/City&$select=HotelId, HotelName, Address/City, Address/StateProvince'`

## <a name="clean-up"></a>Temizleme 

Artık ihtiyacınız kalmadığında, dizin silmeniz gerekir. Ücretsiz bir hizmet için üç dizin sınırlıdır. Etkin bir şekilde diğer öğreticiler için yer açmak için kullandığınız değil tüm dizinleri silmek isteyebilirsiniz.

   ```python
  url = endpoint + "indexes/hotels-py" + api_version
  response  = requests.delete(url, headers=headers)
   ```

Dizin silme işlemi mevcut dizinler listesini döndüren ve doğrulayabilirsiniz. Hotels-py yoksa, istek başarılı bildirin.

```python
url = endpoint + "indexes" + api_version + "&$select=name"

response  = requests.get(url, headers=headers)
index_list = response.json()
pprint(index_list)
```

## <a name="next-steps"></a>Sonraki adımlar

Sorgu söz dizimi ve senaryoları hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Temel sorgu oluşturma](search-query-overview.md)
