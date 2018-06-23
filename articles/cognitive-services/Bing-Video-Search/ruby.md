---
title: Bilişsel hizmetler Azure, Bing Video arama API için Söyleniş hızlı başlangıç | Microsoft Docs
description: Hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get Bing Video arama API Azure üzerinde Microsoft Bilişsel Hizmetleri'ndeki kullanmaya başlayın.
services: cognitive-services
documentationcenter: ''
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: article
ms.date: 9/21/2017
ms.author: v-jerkin
ms.openlocfilehash: d621944415ec376f11a45ea96c331138ec4d6cdb
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354538"
---
# <a name="quickstart-for-bing-video-search-api-with-ruby"></a>Bing Video arama Ruby API'SİYLE için hızlı başlangıç

Bu makalede, Microsoft Bilişsel hizmetler Azure üzerinde bir parçası Bing Video arama API kullanma gösterilmektedir. Bu makalede Ruby kullanır, ancak bir RESTful Web hizmeti, HTTP isteklerini hale getirebilir ve JSON ayrıştırma programlama dili ile uyumlu API'dir. 

Kod örneği, Ruby 2.4 altında çalıştırmak için yazılmıştır.

Başvurmak [API Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference) API'leri hakkında teknik ayrıntılar için.

## <a name="prerequisites"></a>Önkoşullar

Bilmeniz gereken bir [Bilişsel Hizmetleri API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ile **Bing arama API'leri**. [Ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) Bu Hızlı Başlangıç için yeterlidir. Ücretsiz deneme sürümünüzü etkinleştirmek ya da Ücretli abonelik anahtarı Azure panonuza kullanabilir sağlanan erişim anahtarı gerekir.

## <a name="bing-video-search"></a>Bing video arama

[Bing Video arama API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference) arama motoru Bing'den video sonuçları döndürür.

1. Sık kullanılan IDE veya Düzenleyicisi içinde yeni bir Söyleniş projesi oluşturun.
2. Aşağıda sunulan kodu ekleyin.
3. Değiştir `accessKey` aboneliğiniz için geçerli bir erişim anahtarı ile değer.
4. Programını çalıştırın.

```ruby
require 'net/https'
require 'uri'
require 'json'

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the accessKey string value with your valid access key.
accessKey = "enter key here"

# Verify the endpoint URI.  At this writing, only one endpoint is used for Bing
# search APIs.  In the future, regional endpoints may be available.  If you
# encounter unexpected authorization errors, double-check this value against
# the endpoint for your Bing Search instance in your Azure dashboard.

uri  = "https://api.cognitive.microsoft.com"
path = "/bing/v7.0/videos/search"

term = "kittens"

uri = URI(uri + path + "?q=" + URI.escape(term))

puts "Searching videos for: " + term

request = Net::HTTP::Get.new(uri)
request['Ocp-Apim-Subscription-Key'] = accessKey

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request(request)
end

puts "\nRelevant Headers:\n\n"
response.each_header do |key, value|
    # header names are coerced to lowercase
    if key.start_with?("bingapis-") or key.start_with?("x-msedge-") then
        puts key + ": " + value
    end
end

puts "\nJSON Response:\n\n"
puts JSON::pretty_generate(JSON(response.body))
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