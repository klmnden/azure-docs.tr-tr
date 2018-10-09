---
title: "Hızlı Başlangıç: Bing Video Arama API'si"
titlesuffix: Azure Cognitive Services
description: Bing Video Arama API'sini kullanmaya başlamayı göstermektedir.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: quickstart
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: abeeec95755a566216ac65b2edf5c831a8ab93b6
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47225523"
---
# <a name="quickstart-your-first-video-search-query"></a>Hızlı Başlangıç: İlk video arama sorgunuz

İlk çağrınızı yapmadan önce bir Bing Arama Bilişsel Hizmetler abonelik anahtarı almanız gerekir. Anahtar alma için bkz: [Bilişsel Hizmetleri Deneme](https://azure.microsoft.com/try/cognitive-services/?api=bing-video-search-api).

Video arama sonuçlarını almak için aşağıdaki uç noktaya bir GET isteği gönderirsiniz:  
  
```
https://api.cognitive.microsoft.com/bing/v7.0/videos/search
```
   
İstek, HTTPS protokolünü kullanmalıdır.

Tüm isteklerin bir sunucudan gönderilmesini öneririz. Anahtarı bir istemci uygulamanın parçası olarak dağıtmak, kötü amaçlı bir üçüncü tarafa anahtara erişmek için daha fazla fırsat sunar. Ayrıca bir sunucudan çağrı yapmak API'nin gelecek sürümleri için tek bir yükseltme noktası sağlar.

  
İstek kullanıcının arama terimini içeren [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query) sorgu parametresini belirtmelidir. Tercihe bağlı olmakla birlikte, istek, sonuçların gelmesini istediğiniz pazarı tanımlayan [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#mkt) sorgu parametresini de belirtmelidir. `pricing` gibi isteğe bağlı sorgu parametrelerinin bir listesi için bkz. [Sorgu Parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query-parameters). Tüm sorgu parametre değerleri URL olarak kodlanmış olmalıdır.  
  
İstek [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#subscriptionkey) üstbilgisini belirtmelidir. İsteğe bağlı olmakla birlikte şu üstbilgileri de belirtmeniz önerilir:  
  
-   [User-Agent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#useragent)  
-   [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#clientid)  
-   [X-Search-ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#clientip)  
-   [X-Search-Location](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#location)  

İstemci IP'si ve konum üstbilgileri konuma duyarlı içerik döndürmek için önemlidir.  

Tüm istek ve yanıt üstbilgilerinin bir listesi için bkz. [Üstbilgiler](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#headers).


## <a name="the-request"></a>İstek

Aşağıda önerilen tüm sorgu parametrelerini ve üstbilgilerini içeren bir arama isteği gösterilmektedir. Bing API'lerinden birini ilk kez çağırıyorsanız istemci kimliği üst bilgisini eklemeyin. İstemci kimliğini yalnızca önceden bir Bing API'sini çağırdıysanız ve Bing, kullanıcı ve cihaz birleşimi için bir istemci kimliği döndürdüyse dahil edin. 
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Aşağıda, bir önceki isteğin yanıtı gösterilmektedir. Örnek ayrıca Bing'e özgü yanıt üstbilgilerini de göstermektedir.

```
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D5694...",
    "totalEstimatedMatches" : 1000,
    "value" : [
        {
            "name" : "How to sail - What to Wear for Dinghy Sailing",
            "description" : "An informative video on what to wear when...",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D56...",
            "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OVP.DYWCvh...",
            "datePublished" : "2014-03-04T11:51:53",
            "publisher" : [
                {
                    "name" : "Fabrikam"
                }
            ],
            "creator" : {
                "name" : "Marcus Appel"
            },
            "contentUrl" : "https:\/\/www.fabrikam.com\/watch?v=vzmPjHZ--g",
            "hostPageUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D56944...",
            "encodingFormat" : "h264",
            "hostPageDisplayUrl" : "https:\/\/www.fabrikam.com\/watch?v=vzmPjBZ--g",
            "width" : 1280,
            "height" : 720,
            "duration" : "PT2M47S",
            "motionThumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?id=OM.Y6...",
            "embedHtml" : "<iframe width=\"1280\" height=\"720\" src=\"https:...><\/iframe>",
            "allowHttpsEmbed" : true,
            "viewCount" : 8743,
            "thumbnail" : {
                "width" : 300,
                "height" : 168
            },
            "videoId" : "6DB795E11A6E3CBAAD636DB795E11E3CBAAD63",
            "allowMobileEmbed" : true,
            "isSuperfresh" : false
        },
        . . .
    ],
    "nextOffset" : 0,
    "pivotSuggestions" : [
        {
            "pivot" : "sailing",
            "suggestions" : []
        },
        {
            "pivot" : "dinghies",
            "suggestions" : [
                {
                    "text" : "Sailing Cruising",
                    "displayText" : "Cruising",
                    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF754...",
                    "searchLink" : "https:\/\/api.cognitive.microsoft.com...",
                    "thumbnail" : {
                        "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?q=Sailing..."
                    }
                },
                . . .
            ]
        }
    ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

API'yi deneyin. [Video Arama API'si Test Konsolu](https://dev.cognitive.microsoft.com/docs/services/56b43f3ccf5ff8098cef3809/operations/58113fe5e31dac0a1ce6b0a8)'na gidin. 

Yenıt nesnelerini kullanmanın ayrıntıları için bkz. [Web'de Video arama](./search-the-web.md).

Bir video hakkında ilgili aramalar gibi içgörüler elde etmenin ayrıntıları için bkz. [Video İçgörüleri](./video-insights.md).  
  
Sosyal medyada popüler olan videoların ayrıntıları hakkında bkz. [Popüler Videolar](./trending-videos.md).  
