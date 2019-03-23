---
title: PowerShell ve REST API - Azure Search kullanarak dizin sorgulama oluşturma ve yükleme
description: Oluşturma, yüklemek ve PowerShell, Invoke-RestMethod ve Azure Search REST API'sini kullanarak dizin sorgulama.
ms.date: 03/15/2019
author: heidisteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: 9e1b6fc0dc4e6a6c2c191960fa061c810e3a2e79
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58372123"
---
# <a name="quickstart-create-an-azure-search-index-using-powershell-and-the-rest-api"></a>Hızlı Başlangıç: PowerShell ve REST API kullanarak Azure Search dizini oluşturma
> [!div class="op_single_selector"]
> * [PowerShell (REST)](search-create-index-rest-api.md)
> * [C#](search-create-index-dotnet.md)
> * [Postman (REST)](search-fiddler.md)
> * [Portal](search-create-index-portal.md)
> 

Bu makalede size bir Azure Search sorgulama oluşturma ve yükleme işleminde size [dizin](search-what-is-an-index.md) PowerShell kullanarak ve [Azure arama hizmeti REST API'si](https://docs.microsoft.com/rest/api/searchservice/). Dizin tanımını ve aranabilir içeriği sağlanan istek gövdesinde doğru biçimlendirilmiş JSON içeriği.

## <a name="prerequisites"></a>Önkoşullar

[Azure Search hizmeti oluşturma](search-create-service-portal.md) veya [mevcut bir hizmet bulma](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) geçerli aboneliğinizdeki. Bu Hızlı Başlangıç için ücretsiz bir hizmet kullanabilirsiniz. Diğer Önkoşullar aşağıdaki öğeleri ekleyin.

[PowerShell 5.1 veya üstü](https://github.com/PowerShell/PowerShell)kullanarak [Invoke-RestMethod](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Utility/Invoke-RestMethod) sıralı ve etkileşimli adımlar.

Yönetici ve URL uç noktasını alın, arama hizmetinizin api anahtarı. İkisini de içeren bir arama hizmeti oluşturulur. Bu nedenle aboneliğinize Azure Search hizmetini eklediyseniz gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. Azure portalında arama hizmetinizin **genel bakış** sayfa olduğunda URL'yi alın. Örnek uç nokta https gibi görünebilir:\//my-service-name.search.windows.net.

2. İçinde **ayarları** > **anahtarları**, hizmette tam haklarına yönelik bir yönetici anahtarını alın. Bir gece yarısında gerektiği durumlarda iş sürekliliği için sağlanan iki birbirinin yerine yönetici anahtarı mevcuttur. Ekleme, değiştirme ve silme nesneler için istekleri birincil veya ikincil anahtar kullanabilirsiniz.

   ![Bir HTTP uç noktası ve erişim anahtarını alma](media/search-fiddler/get-url-key.png "bir HTTP uç noktası ve erişim anahtarını alma")

   Tüm istekleri hizmete gönderilen her istekte bir API anahtarı gerektirir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="connect-to-azure-search"></a>Azure Search'e Bağlan

PowerShell'de, oluşturma bir **$headers** content-type ve API anahtarı depolamak için nesne. Yalnızca bu başlığı oturum süresi boyunca ayarlamalı gerekir, ancak her istek için ekleyeceksiniz. 

```powershell
$headers = @{
   'api-key' = '<your-admin-api-key>'
   'Content-Type' = 'application/json' 
   'Accept' = 'application/json' }
```

Oluşturma bir **$url** hizmetin belirten nesne dizinler koleksiyonu. `mydemo` Hizmet adı, bir yer tutucu olarak tasarlanmıştır. Bu örnekte boyunca geçerli bir abonelikte bir geçerli arama hizmeti ile değiştirin.

```powershell
$url = "https://mydemo.search.windows.net/indexes?api-version=2017-11-11"
```

Çalıştırma **Invoke-RestMethod** hizmetine bir GET isteği gönderir ve bağlantıyı doğrulamak için. Ekleme **ConvertTo-Json** böylece gönderilen geri hizmetinden yanıtları görüntüleyebilirsiniz.

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

Portal kullanmıyorsanız, verileri yüklemeden önce bir dizin hizmette mevcut olması gerekir. Bu adım, dizini tanımlayan ve hizmetine gönderir. [Dizin oluşturma (REST API'si)](https://docs.microsoft.com/rest/api/searchservice/create-index) Bu adım için kullanılır.

Bir dizinin gerekli öğeler, bir ad ve bir alanlar koleksiyonu içerir. Alanlar koleksiyonu yapısını tanımlayan bir *belge*. Her alanın bir adı, türü ve nasıl kullanıldığını belirleyen özniteliklere sahip (örneğin, tam metin olup aranabilir, filtrelenebilir veya arama sonuçlarında alınabilir). Bir dizinin türü alanlardan biri içinde `Edm.String` olarak belirlenmesi gerekir *anahtar* belge kimliği.

Bu dizin, "hotels" adlı ve aşağıda gördüğünüz alan tanımı yok. Dizin tanımını belirtir bir [dil Çözümleyicisi](index-add-language-analyzers.md) için `description_fr` bir sonraki örnekte ekleyeceğiz Fransızca metin depolamaya yönelik tasarlandığından alan.

Bu örnek oluşturmak için PowerShell içinde yapıştırın bir **$body** dizin şemasını içeren nesne.

```powershell
$body = @"
{
    "name": "hotels",  
    "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String", "facetable": false},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean", "sortable": false},
        {"name": "smokingAllowed", "type": "Edm.Boolean", "sortable": false},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
    ]
}
"@
```

Hizmetinizde dizinler koleksiyonu için URI ayarlayın ve *hotels* dizini.

```powershell
$url = "https://mydemo.search.windows.net/indexes/hotels?api-version=2017-11-11"
```

Komutla çalıştırın **$url**, **$headers**, ve **$body** hizmette dizini oluşturmak için. 

```powershell
Invoke-RestMethod -Uri $url -Headers $headers -Method Put -Body $body | ConvertTo-Json
```
Sonuçlar (ilk iki alanı kısaltma kesilmiş) şuna benzer görünmelidir:

```
{
    "@odata.context":  "https://mydemo.search.windows.net/$metadata#indexes/$entity",
    "@odata.etag":  "\"0x8D6A99E2DED96B0\"",
    "name":  "hotels",
    "defaultScoringProfile":  null,
    "fields":  [
                   {
                       "name":  "hotelId",
                       "type":  "Edm.String",
                       "searchable":  false,
                       "filterable":  true,
                       "retrievable":  true,
                       "sortable":  false,
                       "facetable":  false,
                       "key":  true,
                       "indexAnalyzer":  null,
                       "searchAnalyzer":  null,
                       "analyzer":  null,
                       "synonymMaps":  ""
                   },
                   {
                       "name":  "baseRate",
                       "type":  "Edm.Double",
                       "searchable":  false,
                       "filterable":  true,
                       "retrievable":  true,
                       "sortable":  true,
                       "facetable":  true,
                       "key":  false,
                       "indexAnalyzer":  null,
                       "searchAnalyzer":  null,
                       "analyzer":  null,
                       "synonymMaps":  ""
                   },
. . .
```

> [!Tip]
> Doğrulama için ayrıca Portalı'nda dizinleri listeyi kontrol edin, veya görmek için hizmet bağlantı doğrulamak için kullanılan komutu yeniden çalıştırın *hotels* dizin dizinler koleksiyonu içinde listelenen.

<a name="load-documents"></a>

## <a name="2---load-documents"></a>2 - belge yükleme

Belgeleri göndermek için dizininizin URL uç noktasına bir HTTP POST isteği kullanın. Bu görev için REST API [ekleme, güncelleştirme veya silme belgeleri](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents).

Bu örnek oluşturmak için PowerShell içinde yapıştırın bir **$body** karşıya yüklemek istediğiniz belgeleri içeren nesne. 

Bu istek, iki tam ve kısmi bir kayıt içerir. Eksik belgeler karşıya kısmi kaydı gösterir. `@search.action` Parametresi, dizin oluşturma nasıl yapıldığını belirtir. Geçerli değerler, karşıya yükleme, birleştirme, mergeOrUpload ve silmeyi içerir. MergeOrUpload davranışı hotelId için yeni bir belge ya da oluşturur = 3 veya zaten varsa, içeriği güncelleştirir.

```powershell
$body = @"
{
    "value": [
        {
            "@search.action": "upload",
            "hotelId": "1",
            "baseRate": 199.0,
            "description": "Best hotel in town",
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
            "@search.action": "mergeOrUpload",
            "hotelId": "3",
            "baseRate": 129.99,
            "description": "Close to town hall and the river"
        }
    ]
}
"@
```

Uç nokta kümesine *hotels* docs koleksiyonu ve dizin işlemi (dizinleri/hotels/docs/dizin) içerir.

```powershell
$url = "https://mydemo.search.windows.net/indexes/hotels/docs/index?api-version=2017-11-11"
```

Komutla çalıştırın **$url**, **$headers**, ve **$body** belgeleri Oteller dizinine yüklenemiyor.

```powershell
Invoke-RestMethod -Uri $url -Headers $headers -Method Post -Body $body | ConvertTo-Json
```
Sonuçları şu örneğe benzemelidir. Durum kodu 201 görmeniz gerekir. Tüm durum kodları açıklaması için bkz: [HTTP durum kodları (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).

```
{
    "@odata.context":  "https://mydemo.search.windows.net/indexes(\u0027hotels\u0027)/$metadata#Collection(Microsoft.Azure.Search.V2017_11_11.IndexResult)",
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
                  }
              ]
}
```

## <a name="3---search-an-index"></a>3 - Dizin arama

Bu adım bir dizin kullanarak nasıl sorgulanacağını gösterir [arama belgeleri API](https://docs.microsoft.com/rest/api/searchservice/search-documents).

Uç nokta kümesine *hotels* docs koleksiyonu ve ekleme bir **arama** parametresini kullanarak sorgu dizelerini içerir. Bu dize boş bir aramadır ve tüm belgelerin unranked bir listesini döndürür.

```powershell
$url = 'https://mydemo.search.windows.net/indexes/hotels/docs?api-version=2017-11-11&search=*'
```

Göndermek için kullanılan komut çalıştırma **$url** hizmeti.

```powershell
Invoke-RestMethod -Uri $url -Headers $headers | ConvertTo-Json
```

Sonuç aşağıdaki çıktıya benzer olmalıdır.

```
{
    "@odata.context":  "https://mydemo.search.windows.net/indexes(\u0027hotels\u0027)/$metadata#docs(*)",
    "value":  [
                  {
                      "@search.score":  1.0,
                      "hotelId":  "1",
                      "baseRate":  199.0,
                      "description":  "Best hotel in town",
                      "description_fr":  null,
                      "hotelName":  "Fancy Stay",
                      "category":  "Luxury",
                      "tags":  "pool view wifi concierge",
                      "parkingIncluded":  false,
                      "smokingAllowed":  false,
                      "lastRenovationDate":  "2010-06-27T00:00:00Z",
                      "rating":  5,
                      "location":  "@{type=Point; coordinates=System.Object[]; crs=}"
                  },
                  {
                      "@search.score":  1.0,
                      "hotelId":  "2",
                      "baseRate":  79.99,
                      "description":  "Cheapest hotel in town",
                      "description_fr":  null,
                      "hotelName":  "Roach Motel",
                      "category":  "Budget",
                      "tags":  "motel budget",
                      "parkingIncluded":  true,
                      "smokingAllowed":  true,
                      "lastRenovationDate":  "1982-04-28T00:00:00Z",
                      "rating":  1,
                      "location":  "@{type=Point; coordinates=System.Object[]; crs=}"
                  },
                  {
                      "@search.score":  1.0,
                      "hotelId":  "3",
                      "baseRate":  129.99,
                      "description":  "Close to town hall and the river",
                      "description_fr":  null,
                      "hotelName":  null,
                      "category":  null,
                      "tags":  "",
                      "parkingIncluded":  null,
                      "smokingAllowed":  null,
                      "lastRenovationDate":  null,
                      "rating":  null,
                      "location":  null
                  }
              ]
}
```

Bir genel görünüm sözdizimi almak için birkaç diğer sorgu örnekleri deneyin. Verbatim $filter sorgular bir dize arama yapın, kapsam belirli alanları ve daha fazlası için arama sonuçları kümesini sınırlamak.

```powershell
# Query example 1
# Search the entire index for the term 'budget'
# Return only the `hotelName` field, "Roach hotel"
$url = 'https://mydemo.search.windows.net/indexes/hotels/docs?api-version=2017-11-11&search=budget&$select=hotelName'

# Query example 2 
# Apply a filter to the index to find hotels cheaper than $150 per night
# Returns the `hotelId` and `description`. Two documents match.
$url = 'https://mydemo.search.windows.net/indexes/hotels/docs?api-version=2017-11-11&search=*&$filter=baseRate lt 150&$select=hotelId,description'

# Query example 3
# Search the entire index, order by a specific field (`lastRenovationDate`) in descending order
# Take the top two results, and show only `hotelName` and `lastRenovationDate`
$url = 'https://mydemo.search.windows.net/indexes/hotels/docs?api-version=2017-11-11&search=*&$top=2&$orderby=lastRenovationDate desc&$select=hotelName,lastRenovationDate'
```
## <a name="clean-up"></a>Temizleme 

Artık ihtiyacınız kalmadığında, dizin silmeniz gerekir. Ücretsiz bir hizmet için üç dizin sınırlıdır. Diğer öğreticileri geçebilirsiniz, etkin olarak kullanmadığınız tüm dizinleri silmek isteyebilirsiniz.

```powershell
# Set the URI to the hotel index
$url = 'https://mydemo.search.windows.net/indexes/hotels?api-version=2017-11-11'

# Delete the index
Invoke-RestMethod -Uri $url -Headers $headers -Method Delete
```

## <a name="next-steps"></a>Sonraki adımlar

Fransız açıklamaları dizine eklemeyi deneyin. Aşağıdaki örnek, Fransızca dizeler içerir ve ek arama işlemleri gösterilmektedir. MergeOrUpload oluşturmak veya var olan alanları eklemek için kullanın. Aşağıdaki dizeleri UTF-8 olarak kodlanmış olması gerekir.

```json
{
    "value": [
        {
            "@search.action": "mergeOrUpload",
            "hotelId": "1",
            "description_fr": "Meilleur hôtel en ville"
        },
        {
            "@search.action": "merge",
            "hotelId": "2",
            "description_fr": "Hôtel le moins cher en ville"
        }
    ]
}
```
