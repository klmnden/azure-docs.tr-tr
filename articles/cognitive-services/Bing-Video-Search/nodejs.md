---
title: Bilişsel hizmetler Azure, Bing Video arama API için node.js hızlı başlangıç | Microsoft Docs
description: Hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get Bing Video arama API Azure üzerinde Microsoft Bilişsel Hizmetleri'ndeki kullanmaya başlayın.
services: cognitive-services
documentationcenter: ''
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: article
ms.date: 9/21/2017
ms.author: v-jerkin
ms.openlocfilehash: be9cbd56a1db1f9df78ec6f1c378d803c0a37151
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354533"
---
# <a name="quickstart-for-bing-video-search-api-with-nodejs"></a>Bing Video arama API Node.js ile için hızlı başlangıç

Bu makalede, Microsoft Bilişsel hizmetler Azure üzerinde bir parçası Bing Video arama API kullanma gösterilmektedir. Bu makalede Node.js kullanır, ancak bir RESTful Web hizmeti, HTTP isteklerini hale getirebilir ve JSON ayrıştırma programlama dili ile uyumlu API'dir. 

Örnek JavaScript'te yazılmış ve Node.js 6 altında çalışır.

Başvurmak [API Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) API'leri hakkında teknik ayrıntılar için.

## <a name="prerequisites"></a>Önkoşullar

Bilmeniz gereken bir [Bilişsel Hizmetleri API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ile **Bing arama API'leri**. [Ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) Bu Hızlı Başlangıç için yeterlidir. Ücretsiz deneme sürümünüzü etkinleştirmek ya da Ücretli abonelik anahtarı Azure panonuza kullanabilir sağlanan erişim anahtarı gerekir.

## <a name="bing-video-search"></a>Bing video arama

[Bing Video arama API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference) arama motoru Bing'den video sonuçları döndürür.

1. Sık kullanılan IDE veya Düzenleyicisi içinde yeni bir Node.js projesi oluşturun.
2. Aşağıda sunulan kodu ekleyin.
3. Değiştir `subscriptionKey` aboneliğiniz için geçerli bir erişim anahtarı ile değer.
4. Programını çalıştırın.

```javascript
'use strict';

let https = require('https');

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
let subscriptionKey = 'enter key here';

// Verify the endpoint URI.  At this writing, only one endpoint is used for Bing
// search APIs.  In the future, regional endpoints may be available.  If you
// encounter unexpected authorization errors, double-check this host against
// the endpoint for your Bing Search instance in your Azure dashboard.
let host = 'api.cognitive.microsoft.com';
let path = '/bing/v7.0/videos/search';

let term = 'kittens';

let response_handler = function (response) {
    let body = '';
    response.on('data', function (d) {
        body += d;
    });
    response.on('end', function () {
        console.log('\nRelevant Headers:\n');
        for (var header in response.headers)
            // header keys are lower-cased by Node.js
            if (header.startsWith("bingapis-") || header.startsWith("x-msedge-"))
                 console.log(header + ": " + response.headers[header]);
        body = JSON.stringify(JSON.parse(body), null, '  ');
        console.log('\nJSON Response:\n');
        console.log(body);
    });
    response.on('error', function (e) {
        console.log('Error: ' + e.message);
    });
};

let bing_video_search = function (search) {
  console.log('Searching videos for: ' + term);
  let request_params = {
        method : 'GET',
        hostname : host,
        path : path + '?q=' + encodeURIComponent(search),
        headers : {
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };

    let req = https.request(request_params, response_handler);
    req.end();
}
bing_video_search(term);
```

**Yanıt**

Başarılı yanıt JSON'da, aşağıdaki örnekte gösterildiği gibi verilir: 

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
> [Disk belleği videolar](paging-videos.md)
> [Resizing ve küçük resim görüntüleri kırpma](resize-and-crop-thumbnails.md)

## <a name="see-also"></a>Ayrıca bkz. 

 [Web videolar için arama](search-the-web.md) [deneyin](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/)