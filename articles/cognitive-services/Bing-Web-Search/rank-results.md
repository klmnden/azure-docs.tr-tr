---
title: Arama sonuçları - Bing Web araması API'si görüntülenecek sıralamasına kullanma
titleSuffix: Azure Cognitive Services
description: Bing Web araması API'si arama sonuçlarını görüntülemek için sıralama'ı kullanmayı öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: BBF87972-B6C3-4910-BB52-DE90893F6C71
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 03/17/2019
ms.author: scottwhi
ms.openlocfilehash: 677f6089f649aae720a6303a7e1512e3c7ebeca7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66390130"
---
# <a name="how-to-use-ranking-to-display-bing-web-search-api-results"></a>Bing Web araması API'si sonuçlarını görüntülemek için sıralama kullanma  

Her arama yanıt içeren bir [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#rankingresponse) nasıl arama sonuçlarını görüntülemek belirten yanıt. Derecelendirme yanıt sonuçlarını gruplar tarafından mainline içerik ve Kenar çubuğunda içerik geleneksel bir arama sonuçları sayfası için. Kenar biçimi ve sonuçları geleneksel bir ana hat görüntülenmiyorsa, ana hat içerik daha yüksek görünürlük kenar çubuğu içeriği daha sağlamanız gerekir.  

Her grup içindeki (mainline veya kenar), [öğeleri](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#rankinggroup-items) dizi içeriği görünmelidir sırasını tanımlar. Her bir öğe içinde bir yanıt sonucu belirlemek için aşağıdaki iki yoldan sağlar.  

-   `answerType` ve `resultIndex` — `answerType` alan tanımlar (örneğin, Web sayfası veya haber) yanıt ve `resultIndex` yanıt (örneğin, bir haber makale) içinde bir sonuç tanımlar. Sıfır tabanlı dizindir.  

-   `value` — `value` Alan yanıt veya yanıt içinde bir sonuç kimliği eşleşen bir kimlik içerir. Yanıt ya da sonuçları kimliği ancak ikisini birden içerir.  

Kimliğini kullanarak, yalnızca cevap veya bir hesaplamanın sonuçlarını kimliği derecelendirme Kimliğiyle eşleşecek şekilde gerektiğinden kullanmak daha basittir. Bir yanıt nesnesi içeriyorsa bir `id` alan, yanıtın tüm sonuçları birlikte görüntüler. Örneğin, varsa `News` nesne içeren `id` alan, haber makalelerini birlikte görüntüleyebilirsiniz. Varsa `News` nesnesi içermez `id` her haber makalesinin içeriyorsa, alan bir `id` alan ve sıralama yanıt başka yanıtlar alınan sonuçlarla haber makalelerini karıştırır.  

Kullanarak `answerType` ve `resultIndex` biraz daha karmaşıktır. Kullandığınız `answerType` görüntülemek için sonuçları içeren yanıt tanımlamak için. Ardından, kullandığınız `resultIndex` aracılığıyla yanıtın sonuçları görüntülemek için bir sonuç almak için dizin. ( `answerType` Değer alanın adıdır [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#searchresponse) nesne.) Yanıtın tüm sonuçları birlikte görüntülemek için beklenen yapıyorsanız, derecelendirme yanıt öğesi içermez `resultIndex` alan.  

## <a name="ranking-response-example"></a>Örnek yanıt derecelendirmesi

Aşağıdaki örnekler [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#rankingresponse). Web yanıtı nedeni bir `id` alanı ayrı ayrı derecelere dayanan tüm Web sayfalarını görüntüleme (her Web sayfasını içeren bir `id` alan). Ve görüntüleri, videolar ve ilgili aramalar cevapları dahil ettiğimiz `id` alan, her birinin derecelere dayanan yanıtlayabilir birlikte sonuçları görüntüle.

```json
{  
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=96C4CF214...",
        "totalEstimatedMatches" : 835000,
        "value" : [
            {
                "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.0",
                "name" : "Motor Sports - Live at the race track ...",
                "url" : "http:\/\/www.bing.com\/cr?IG=96C4CF214A0A43...",
                "displayUrl" : "www.contoso.com\/usa\/eventsandracing\/motorsport",
                "snippet" : "Here you will find detailed information about racing...",
                "deepLinks" : [{
                    "name" : "Customer Racing",
                    "url" : "http:\/\/www.bing.com\/cr?IG=96C4CF214A0A43...",
                    "snippet" : "Customer racing news; General news..."
            },
            . . .  
        ]  
    }],  
    "images" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images",
        "readLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images...",
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=96C4CF214A...",
        "isFamilyFriendly" : true,
        "value" : [
            {
                "name" : "2016 Supercar Wallpapers",
                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=96C4...",
                "thumbnailUrl" : "https:\/\/tse1.mm.bing.net\/th?id=OIP...",
                "datePublished" : "2017-03-25T11:14:00",
                "contentUrl" : "http:\/\/www.contoso.com\/wall...",
                "hostPageUrl" : "http:\/\/www.bing.com\/cr?IG=96C4CF214...",
                "contentSize" : "373283 B",
                "encodingFormat" : "jpeg",
                "hostPageDisplayUrl" : "http:\/\/www.contoso.com\/lmp-...",
                "width" : 1920,
                "height" : 1080,
                "thumbnail" : {
                    "width" : 300,
                    "height" : 168
                },
                "insightsSourcesSummary" : {
                    "shoppingSourcesCount" : 0,
                    "recipeSourcesCount" : 0
                }
            },
            . . .  
        ]  
    },  
    "relatedSearches" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#RelatedSearches",
        "value" : [
            {
                "text" : "vintage racing teams",
                "displayText" : "vintage racing teams",
                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=96C4CF2..."
            },
            . . .  
        ]  
    },  
    "videos" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Videos",
        "readLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/videos...",
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=96C4CF214A...",
        "isFamilyFriendly" : true,
        "value" : [
            {
                "name" : "Why We Race",
                "description" : "A new era begins in motorsports this weekend...",
                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=96C4CF2...",
                "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OVP.Vo1...",
                "datePublished" : "2014-01-25T16:31:48",
                "publisher" : [
                    {
                        "name" : "Fabrikam"
                    }
                ],
                "contentUrl" : "https:\/\/www.fabrikam.com\/watch?v=oL...",
                "hostPageUrl" : "https:\/\/www.bing.com\/cr?IG=96C4CF214...",
                "encodingFormat" : "mp4",
                "hostPageDisplayUrl" : "https:\/\/www.fabrikam.com\/watch?v=oLAZgD...",
                "width" : 480,
                "height" : 360,
                "duration" : "PT2M42S",
                "motionThumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OM...",
                "embedHtml" : "<iframe width=\"1280\" height=\"720\" src=\"http:\/\/www.you...<\/iframe>",
                "allowHttpsEmbed" : true,
                "viewCount" : 47325,
                "thumbnail" : {
                    "width" : 300,
                    "height" : 168
                },
                "allowMobileEmbed" : true,
                "isSuperfresh" : false
            },
            . . .  
        ]  
    },  
    "rankingResponse" : {
        "mainline" : {
            "items" : [{
                "answerType" : "WebPages",
                "resultIndex" : 0,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.0"
                }
            },
            {
                "answerType" : "Images",
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 1,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.1"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 2,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.2"
                }
            },
            {
                "answerType" : "Videos",
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Videos"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 3,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.3"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 4,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.4"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 5,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.5"
                }
            }]
        },
        "sidebar" : {
            "items" : [{
                "answerType" : "RelatedSearches",
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#RelatedSearches"
                }
            }]
        }
    }
}  
```  

Bu derecelendirme yanıtta bağlı olarak, ana hat aşağıdaki arama sonuçları görüntüler:  

-   İlk Web sayfası sonucu
-   Tüm görüntüleri  
-   İkinci ve üçüncü bir Web sayfası sonuçları  
-   Tüm videoları  
-   4, 5 ve 6 Web sayfası sonuçları  

Ve Kenar çubuğunda aşağıdaki arama sonuçları görüntüler:  

-   Tüm ilgili aramalar  


## <a name="next-steps"></a>Sonraki adımlar

Unranked sonuçları yükseltme hakkında daha fazla bilgi için bkz: [değil sıralanır yanıtlar yükseltme](./filter-answers.md#promoting-answers-that-are-not-ranked).

Yanıt dereceli yanıtlar sayısını sınırlandırma hakkında daha fazla bilgi için bkz: [yanıtlar yanıt sayısını sınırlama](./filter-answers.md#limiting-the-number-of-answers-in-the-response).

Sıralama sonuçları görüntülemek için kullanan bir C# örnek için bkz. [C# derecelendirme Öğreticisi](./csharp-ranking-tutorial.md).
