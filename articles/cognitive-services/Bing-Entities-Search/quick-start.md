---
title: "Hızlı başlangıç: Bing Varlık Arama API'si"
description: Bing Varlık Arama API'sini kullanmaya başlamayı göstermektedir.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: quickstart
ms.date: 07/06/2017
ms.author: scottwhi
ms.openlocfilehash: ffc9ebb21c6646b1a39af4659053adf4157d204b
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48813966"
---
# <a name="quickstart-making-your-first-bing-entity-search-request"></a>Hızlı başlangıç: İlk Bing Varlık Arama isteğiniz oluşturma

Bing Varlık Arama API'si, Bing'e bir arama sorgusu gönderip varlıkları ve yerleri içeren sonuçlar alır. Yer sonuçları restoranlar, oteller veya diğer yerel işletmeleri kapsar. Yerler için sorguda yerel işletmenin adı belirtilebilir veya liste isteği (yakınımdaki restoranlar gibi) gönderilebilir. Varlık sonuçları kişileri, yerleri veya nesneleri kapsar. Bu bağlamda yer turistik noktalar, şehirler, ülkeler gibi konumlar olabilir. 

## <a name="first-steps"></a>İlk adımlar

İlk çağrınızı yapmadan önce bir Bilişsel Hizmetler abonelik anahtarı almanız gerekir. Anahtar alma için bkz: [Bilişsel Hizmetleri Deneme](https://azure.microsoft.com/try/cognitive-services/?api=bing-entities-search-api). (Varlık Arama API'si en üstte görünmüyorsa **Ara** sekmesine gidin ve bulana kadar sayfayı kaydırın.)

## <a name="the-endpoint"></a>Uç nokta

Varlık ve yer arama sonuçlarını almak için aşağıdaki uç noktaya bir GET isteği gönderin:  
  
```
https://api.cognitive.microsoft.com/bing/v7.0/entities
```

İstek, HTTPS protokolünü kullanmalıdır.

Tüm isteklerin bir sunucudan gönderilmesini öneririz. Anahtarı bir istemci uygulamanın parçası olarak dağıtmak, kötü amaçlı bir üçüncü tarafa anahtara erişmek için daha fazla fırsat sunar. Ayrıca bir sunucudan çağrı yapmak API'nin gelecek sürümleri için tek bir yükseltme noktası sağlar.

## <a name="specifying-query-parameters-and-headers"></a>Sorgu parametrelerini ve üst bilgileri belirtme

İstek kullanıcının arama terimini içeren [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#query) sorgu parametresini belirtmelidir. İstek, sonuçların gelmesini istediğiniz pazarı tanımlayan [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#mkt) sorgu parametresini de belirtmelidir. İsteğe bağlı sorgu parametrelerinin bir listesi için bkz. [Sorgu Parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#query-parameters). Tüm sorgu parametrelerini URL'de kodlayın.  
  
İstek [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#subscriptionkey) üstbilgisini belirtmelidir. İsteğe bağlı olmakla birlikte şu üstbilgileri de belirtmeniz önerilir:  
  
-   [User-Agent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#useragent)  
-   [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#clientid)  
-   [X-MSEdge-ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#clientip)  
-   [X-Search-Location](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#location)  

İstemci IP'si ve konum üstbilgileri konuma duyarlı içerik döndürmek için önemlidir.  

Tüm istek ve yanıt üstbilgilerinin bir listesi için bkz. [Üstbilgiler](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#headers).

## <a name="the-request"></a>İstek

Aşağıda önerilen tüm sorgu parametrelerini ve üstbilgilerini içeren bir varlık isteği gösterilmektedir. 

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/entities?q=mount+rainier&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Bing API'lerinden birini ilk kez çağırıyorsanız istemci kimliği üst bilgisini eklemeyin. İstemci kimliğini yalnızca önceden bir Bing API'sini çağırdıysanız ve Bing, kullanıcı ve cihaz birleşimi için bir istemci kimliği döndürdüyse dahil edin.

## <a name="the-response"></a>Yanıt

Aşağıda, bir önceki isteğin yanıtı gösterilmektedir. Örnek ayrıca Bing'e özgü yanıt üstbilgilerini de göstermektedir. Yanıt nesnesi hakkında bilgi için bkz. [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#searchresponse).

```
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "mount rainier"
    },
    "entities" : {
        "queryScenario" : "DominantEntity",
        "value" : [{
            "contractualRules" : [{
                "_type" : "ContractualRules\/LicenseAttribution",
                "targetPropertyName" : "description",
                "mustBeCloseToContent" : true,
                "license" : {
                    "name" : "CC-BY-SA",
                    "url" : "http:\/\/creativecommons.org\/licenses\/by-sa\/3.0\/"
                },
                "licenseNotice" : "Text under CC-BY-SA license"
            },
            {
                "_type" : "ContractualRules\/LinkAttribution",
                "targetPropertyName" : "description",
                "mustBeCloseToContent" : true,
                "text" : "en.wikipedia.org",
                "url" : "http:\/\/en.wikipedia.org\/wiki\/Mount_Rainier"
            },
            {
                "_type" : "ContractualRules\/MediaAttribution",
                "targetPropertyName" : "image",
                "mustBeCloseToContent" : true,
                "url" : "http:\/\/en.wikipedia.org\/wiki\/Mount_Rainier"
            }],
            "webSearchUrl" : "https:\/\/www.bing.com\/search?q=Mount%20Rainier...",
            "name" : "Mount Rainier",
            "image" : {
                "name" : "Mount Rainier",
                "thumbnailUrl" : "https:\/\/www.bing.com\/th?id=A21890c0e1f...",
                "provider" : [{
                    "_type" : "Organization",
                    "url" : "http:\/\/en.wikipedia.org\/wiki\/Mount_Rainier"
                }],
                "hostPageUrl" : "http:\/\/upload.wikimedia.org\/wikipedia...",
                "width" : 110,
                "height" : 110
            },
            "description" : "Mount Rainier, Mount Tacoma, or Mount Tahoma is the highest...",
            "entityPresentationInfo" : {
                "entityScenario" : "DominantEntity",
                "entityTypeHints" : ["Attraction"],
                "entityTypeDisplayHint" : "Mountain"
            },
            "bingId" : "9ae3e6ca-81ea-6fa1-ffa0-42e1d78906"
        }]
    }
}
```



## <a name="next-steps"></a>Sonraki adımlar

API'yi deneyin. [Varlık Arama API'si Test Konsolu](https://dev.cognitive.microsoft.com/docs/services/7a3fb374be374859a823b79fd938cc65/)'na gidin. 

Yanıt nesnelerini kullanmanın ayrıntıları için bkz. [Web'de varlık ve yer arama](./search-the-web.md).

Arama sonuçlarını kullanma kurallarına uygun hareket ettiğinizden emin olmak için [Bing Kullanım ve Görüntüleme Gereksinimleri](./use-display-requirements.md)'ni okumayı unutmayın.
