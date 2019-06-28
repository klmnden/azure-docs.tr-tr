---
title: "Hızlı Başlangıç: Bing Video arama REST API'si ve Node.js kullanarak video arayın"
titlesuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Bing Video arama REST kullanarak JavaScript API'si için video arama istekleri göndermek için kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: quickstart
ms.date: 06/26/2019
ms.author: aahi
ms.openlocfilehash: a7cfdeb69fd0df9d79424ade3823ab7f8adb4913
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67449287"
---
# <a name="quickstart-search-for-videos-using-the-bing-video-search-rest-api-and-nodejs"></a>Hızlı Başlangıç: Bing Video arama REST API'si ve Node.js kullanarak video arayın

Bu hızlı başlangıçta, Bing Video arama API'si, ilk çağrı yapmak ve bir JSON yanıtı Arama sonuçlarından görüntülemek için kullanın. Bu basit bir JavaScript uygulama API'sine HTTP video arama sorgusu gönderir ve yanıtını görüntüler. Bu uygulamanın, JavaScript'te yazılmış ve Node.js kullanır, ancak çoğu programlama dilleri ile uyumlu bir RESTful Web hizmeti API'dir. Bu örneğin kaynak kodu, ek hata işleme ve kod açıklama notları ile [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingVideoSearchv7.js)’da bulunabilir.

## <a name="prerequisites"></a>Önkoşullar

* [Node.js](https://nodejs.org/en/download/)

* JavaScript için istek Modülü
    * Bu modül kullanarak yükleyebilirsiniz. `npm install request`

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](../../../../includes/cognitive-services-bing-video-search-signup-requirements.md)]

## <a name="initialize-the-application"></a>Uygulamayı Başlat

1. Yeni bir JavaScript dosyası, sık kullandığınız IDE veya düzenleyici oluşturun. Katılık ayarlayın ve aşağıdaki gereksinim ekleyin:

    ```javascript
    'use strict';
    let https = require('https');
    ```

2. API uç noktanıza, abonelik anahtarını ve arama teriminizi değişkenleri oluşturun.

    ```javascript
    let subscriptionKey = 'enter key here';
    let host = 'api.cognitive.microsoft.com';
    let path = '/bing/v7.0/videos/search';
    let term = 'kittens';
    ```

## <a name="create-a-response-handler"></a>Yanıt işleyici oluşturma

1. Çağrılan bir işlev oluşturma `response_handler` API'sinden bir JSON yanıtı gerçekleştirilecek. Yanıt gövdesi için bir değişken oluşturun. Yanıt ekleme yaparken bir `data` bayrağı alındığında, kullanarak `response.on()`.

    ```javascript
    let response_handler = function (response) {
        let body = '';
        response.on('data', function (d) {
            body += d;
        });
    };
    ```
    
   1. Zaman `end` olan sinyal, kullanın `response.on()` bing ile ilgili üst bilgileri depolamak için (başlayarak `bingapis` veya `x-msedge-`). Ardından kullanarak JSON Ayrıştır `JSON.parse()`, bir dize dönüştürmek `JSON.stringify()`, yazdırabilirsiniz.

       ```javascript
       response.on('end', function () {
           for (var header in response.headers)
               // header keys are lower-cased by Node.js
               if (header.startsWith("bingapis-") || header.startsWith("x-msedge-"))
                    console.log(header + ": " + response.headers[header]);
           body = JSON.stringify(JSON.parse(body), null, '  ');
           //JSON Response body
           console.log(body);
       });
       ```

# <a name="create-and-send-the-search-request"></a>Oluşturma ve arama isteği gönderme

1. Çağrılan bir işlev oluşturma `bing_video_search()`. Ana bilgisayar adı ve üst bilgiler dahil olmak üzere, istek parametrelerini ekleyin. Arama teriminizi kodlama ve yol parametreniz eklemenin `?q=` parametresi. Ardından istekle Gönder `req.end()`.

    ```javascript
    let bing_video_search = function (search_term) {
      console.log('Searching videos for: ' + term);
      let request_params = {
            method : 'GET',
            hostname : host,
            path : path + '?q=' + encodeURIComponent(search_term),
            headers : {
                'Ocp-Apim-Subscription-Key' : subscriptionKey,
            }
        };
        let req = https.request(request_params, response_handler);
        req.end();
    }
    ```

## <a name="json-response"></a>JSON yanıtı

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
    "_type": "Videos",
    "instrumentation": {},
    "readLink": "https://api.cognitive.microsoft.com/api/v7/videos/search?q=kittens",
    "webSearchUrl": "https://www.bing.com/videos/search?q=kittens",
    "totalEstimatedMatches": 1000,
    "value": [
        {
            "webSearchUrl": "https://www.bing.com/videos/search?q=kittens&view=...",
            "name": "Top 10 cute kitten videos compilation",
            "description": "HELP HOMELESS ANIMALS AND WIN A PRIZE BY CHOOSING...",
            "thumbnailUrl": "https://tse4.mm.bing.net/th?id=OVP.n1aE_Oikl4MtzBb...",
            "datePublished": "2014-11-12T22:47:36.0000000",
            "publisher": [
                {
                    "name": "Fabrikam"
                }
            ],
            "creator": {
                "name": "Marcus Appel"
            },
            "isAccessibleForFree": true,
            "contentUrl": "https://www.fabrikam.com/watch?v=8HVWitAW-Qg",
            "hostPageUrl": "https://www.fabrikam.com/watch?v=8HVWitAW-Qg",
            "encodingFormat": "h264",
            "hostPageDisplayUrl": "https://www.fabrikam.com/watch?v=8HVWitAW-Qg",
            "width": 480,
            "height": 360,
            "duration": "PT3M52S",
            "motionThumbnailUrl": "https://tse4.mm.bing.net/th?id=OM.j4QyJAENJphdZQ_1501386166&pid=Api",
            "embedHtml": "<iframe width=\"1280\" height=\"720\" src=\"https://www.fabrikam.com/embed/8HVWitAW-Qg?autoplay=1\" frameborder=\"0\" allowfullscreen></iframe>",
            "allowHttpsEmbed": true,
            "viewCount": 7513633,
            "thumbnail": {
                "width": 300,
                "height": 168
            },
            "videoId": "655D98260D012432848F6558260D012432848F",
            "allowMobileEmbed": true,
            "isSuperfresh": false
        },
        . . .
    ],
    "nextOffset": 36,
    "queryExpansions": [
        {
            "text": "Kittens Meowing",
            "displayText": "Meowing",
            "webSearchUrl": "https://www.bing.com/videos/search?q=Kittens+Meowing...",
            "searchLink": "https://api.cognitive.microsoft.com/api/v7/videos/search...",
            "thumbnail": {
                "thumbnailUrl": "https://tse3.mm.bing.net/th?q=Kittens+Meowing&pid..."
            }
        },
        {
            "text": "Funny Kittens",
            "displayText": "Funny",
            "webSearchUrl": "https://www.bing.com/videos/search?q=Funny+Kittens...",
            "searchLink": "https://api.cognitive.microsoft.com/api/v7/videos/search...",
            "thumbnail": {
                "thumbnailUrl": "https://tse3.mm.bing.net/th?q=Funny+Kittens&..."
            }
        },
        . . .
    ],
    "pivotSuggestions": [
        {
            "pivot": "kittens",
            "suggestions": [
                {
                    "text": "Cat",
                    "displayText": "Cat",
                    "webSearchUrl": "https://www.bing.com/videos/search?q=Cat...",
                    "searchLink": "https://api.cognitive.microsoft.com/api/v7/videos/search?...",
                    "thumbnail": {
                        "thumbnailUrl": "https://tse3.mm.bing.net/th?q=Cat&pid=Api..."
                    }
                },
                {
                    "text": "Feral Cat",
                    "displayText": "Feral Cat",
                    "webSearchUrl": "https://www.bing.com/videos/search?q=Feral+Cat...",
                    "searchLink": "https://api.cognitive.microsoft.com/api/v7/videos/search...",
                    "thumbnail": {
                        "thumbnailUrl": "https://tse3.mm.bing.net/th?q=Feral+Cat&pid=Api&..."
                    }
                }
            ]
        }
    ],
    "relatedSearches": [
        {
            "text": "Kittens Being Born",
            "displayText": "Kittens Being Born",
            "webSearchUrl": "https://www.bing.com/videos/search?q=Kittens+Being+Born...",
            "searchLink": "https://api.cognitive.microsoft.com/api/v7/videos/search?...",
            "thumbnail": {
                "thumbnailUrl": "https://tse1.mm.bing.net/th?q=Kittens+Being+Born&pid=..."
            }
        },
        . . .
    ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfa uygulaması oluşturma](../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz. 

 [Bing Video arama API'si nedir?](../overview.md)
