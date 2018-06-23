---
title: Video arama API'si hızlı başlangıç | Microsoft Docs
description: Bing Video arama API'si ile çalışmaya başlamak gösterilmiştir.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 7E59692A-83A8-4F4C-B122-1F0EDC8E5C86
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 0bd0f067d64cac3ebac342ebadcfcc010a47af7b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354545"
---
# <a name="your-first-video-search-query"></a>İlk video arama sorgunuzu

İlk aramanız yapabilmeniz için önce bir Bing arama Bilişsel hizmetler abonelik anahtarı almanız gerekir. Bir anahtar almak için bkz: [deneyin Bilişsel Hizmetler](https://azure.microsoft.com/try/cognitive-services/?api=bing-video-search-api).

Video arama sonuçları almak için aşağıdaki uç noktaya bir GET isteği gönder:  
  
```
https://api.cognitive.microsoft.com/bing/v7.0/videos/search
```
   
İstek HTTPS protokolünü kullanmanız gerekir.

Tüm istekleri bir sunucusundan kaynaklanan öneririz. Bir istemci uygulaması bir parçası olarak anahtar dağıtma bir kötü amaçlı üçüncü erişmek taraf için daha fazla fırsatı sağlar. Ayrıca, bir sunucudan çağrıları yapma tek bir yükseltme noktası API gelecek sürümlerinde sağlar.

  
İstek belirtmelisiniz [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query) kullanıcının arama terimi içeren sorgu parametresi. İsteğe bağlı olsa da, istek de belirtmeniz gerekir [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#mkt) alınması için sonuçları istediğiniz Pazar tanımlayan sorgu parametresi. Listesini isteğe bağlı parametreleri gibi sorgu `pricing`, bkz: [sorgu parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query-parameters). Tüm sorgu parametre değerleri URL kodlanmış olmalıdır.  
  
İstek belirtmelisiniz [Apim abonelik anahtar Ocp](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#subscriptionkey) üstbilgi. İsteğe bağlı olsa da, aşağıdaki üst bilgiler de belirtmeniz önerilir:  
  
-   [Kullanıcı Aracısı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#useragent)  
-   [X MSEdge ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#clientid)  
-   [X arama ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#clientip)  
-   [X arama konumu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#location)  

İstemci IP ve konum üstbilgileri konumu kullanan içerik döndürmek için önemlidir.  

Tüm istek ve yanıt üstbilgileri listesi için bkz: [üstbilgileri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#headers).


## <a name="the-request"></a>İstek

Tüm üstbilgiler ve önerilen sorgu parametreleri içeren bir arama isteğine gösterir. Herhangi bir Bing API'leri çağırma ilk kez ise, istemci kimliği üstbilgisi eklemeyin. Yalnızca istemci kimliği, daha önce Bing API'si çağırdıktan ve kullanıcı ve aygıt bileşimi için bir istemci kimliği Bing döndürülen içerir. 
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Önceki istek yanıtı gösterir. Bu örnek ayrıca Bing özgü yanıt üstbilgilerini gösterir.

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

Out API deneyin. Git [Video arama API sınama Konsolu'nu](https://dev.cognitive.microsoft.com/docs/services/56b43f3ccf5ff8098cef3809/operations/58113fe5e31dac0a1ce6b0a8). 

Yanıt nesneleri kullanma hakkında daha fazla bilgi için bkz [Web videolar için arama](./search-the-web.md).

İlgili aramalar gibi bir video hakkında Öngörüler alma hakkında daha fazla ayrıntı için bkz: [Video Öngörüler](./video-insights.md).  
  
Sosyal medya üzerinde eğilim videolar hakkında daha fazla ayrıntı için bkz: [eğilim videolar](./trending-videos.md).  
