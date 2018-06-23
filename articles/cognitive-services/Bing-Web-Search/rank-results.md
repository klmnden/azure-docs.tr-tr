---
title: Derecelendirme web yanıtları görüntülemek için kullanma | Microsoft Docs
description: Derecelendirme Bing Web arama API döndürür yanıtları görüntülemek için nasıl kullanılacağını gösterir.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: BBF87972-B6C3-4910-BB52-DE90893F6C71
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 750146f3bb28b94594a71733b68f092880360c5a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354569"
---
# <a name="using-ranking-to-display-results"></a>Sıralama sonuçları görüntülemek için kullanma  

Her arama yanıtı içeren bir [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse) nasıl arama sonuçlarını görüntülemek belirten yanıt. Sonuçları sıralama yanıt gruplar tarafından mainline içerik ve kenar içerik geleneksel bir arama sonuçları sayfası için. Kenar biçimi ve sonuçları bir geleneksel mainline görüntülenmiyorsa kenar çubuğu içeriğini daha mainline içerik daha yüksek görünürlük sağlamanız gerekir.  
  
Her grup içindeki (mainline veya kenar), [öğeleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankinggroup-items) dizi içeriği görünmelidir sırasını tanımlar. Her öğe içinde bir yanıt sonucu tanımlamak için aşağıdaki iki yöntem sağlar.  
  
-   `answerType` ve `resultIndex` — `answerType` alan (örneğin, Web sayfası veya haber) yanıt tanımlar ve `resultIndex` yanıt (örneğin, bir haber makalesi) içinde bir sonuç tanımlar. Sıfır tabanlı dizin olabilir.  
  
-   `value` — `value` Alan bir yanıt veya yanıt içinde bir sonuç kodu eşleşen bir Kimliğini içerir. Yanıt veya sonuçları kimliği ancak ikisini içerir.  
  
Kimliğini kullanarak, yalnızca bir yanıt veya sonuçlarını birini Kimliğini derecelendirme Kimliğiyle eşleşmesi gerekir çünkü kullanmak daha basittir. Bir yanıt nesne içeriyorsa, bir `id` alan, yanıtın tüm sonuçları birlikte görüntüler. Örneğin, varsa `News` nesnesi içerir `id` alanı, haber makaleleri birlikte görüntüler. Varsa `News` nesne içermez `id` her haber makaleleri içeriyorsa, alan bir `id` alan ve derecelendirme yanıt diğer yanıtlar alınan sonuçlarla haber makalelerini karıştırır.  
  
Kullanarak `answerType` ve `resultIndex` biraz daha karmaşıktır. Kullandığınız `answerType` görüntülenecek sonuçlarını içeren yanıt tanımlamak için. Ardından, kullandığınız `resultIndex` yanıtın sonuçları görüntülemek için sonuç almak için aracılığıyla dizin. ( `answerType` Değerdir alanın adını [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#searchresponse) nesnesi.) Birlikte yanıtın tüm sonuçları görüntülemek için gereken, derecelendirme yanıt öğesi içermeyen `resultIndex` alan.  

## <a name="ranking-response-example"></a>Yanıt örnek sıralaması

Aşağıdaki örnek gösterir [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse). Web yanıt içermemesi bir `id` alanı, tek tek bir derecelendirme göre tüm Web sayfalarını görüntülemek (her Web sayfası içeren bir `id` alan). Ve resim, video ve ilişkili aramaları yanıtlar içerdiğinden `id` alanı, her bir derecelendirme göre Bu yanıtlar birlikte sonuçlarını görüntülemek.
  
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
    },  
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
  
Bu derecelendirme yanıtta bağlı olarak, mainline aşağıdaki arama sonuçları görüntüler:  
  
-   İlk Web sayfası sonucu 
-   Tüm görüntüleri  
-   İkinci ve üçüncü Web sayfası sonuçları  
-   Tüm videoları  
-   4, 5 ve 6 Web sayfası sonuçları  
  
Ve kenar aşağıdaki arama sonuçları görüntüler:  
  
-   Tüm ilgili aramalar  
  

## <a name="next-steps"></a>Sonraki adımlar

Unranked sonuçları yükseltme hakkında daha fazla bilgi için bkz: [değil sıralanır yanıtlar yükseltme](./filter-answers.md#promoting-answers-that-are-not-ranked).

Yanıt dereceli yanıtlar sayısını sınırlama hakkında daha fazla bilgi için bkz: [yanıtlar yanıt sayısını sınırlama](./filter-answers.md#limiting-the-number-of-answers-in-the-response).

Sıralama sonuçları görüntülemek için kullandığı bir C# örnek için bkz: [C# derecelendirme Öğreticisi](./csharp-ranking-tutorial.md).