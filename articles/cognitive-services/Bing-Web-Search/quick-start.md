---
title: Web ara API hızlı başlangıç | Microsoft Docs
description: Bing Web arama API'sini kullanmaya başlamak gösterilmiştir.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 27B4B51A-D017-44C8-8E4E-9684DC553886
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 0b8c4678a518985a4be3ee426a85b0a85dd2365d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351748"
---
# <a name="your-first-bing-search-query"></a>İlk Bing arama sorgusu

İlk aramanız yapabilmeniz için önce bir Bilişsel hizmetler abonelik anahtarı almanız gerekir. Bir anahtar almak için bkz: [deneyin Bilişsel Hizmetler](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).

Web arama sonuçları almak için aşağıdaki uç noktaya bir GET isteği gönder:  
  
```
https://api.cognitive.microsoft.com/bing/v7.0/search
```  

İstek HTTPS protokolünü kullanmanız gerekir.

Tüm istekleri bir sunucusundan kaynaklanan öneririz. Bir istemci uygulaması bir parçası olarak anahtar dağıtma bir kötü amaçlı üçüncü erişmek taraf için daha fazla fırsatı sağlar. Ayrıca, bir sunucudan çağrıları yapma tek bir yükseltme noktası API gelecek sürümlerinde sağlar.  
  
İstek belirtmelisiniz [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#query) kullanıcının arama terimi içeren sorgu parametresi. İsteğe bağlı olsa da, istek de belirtmeniz gerekir [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#mkt) alınması için sonuçları istediğiniz Pazar tanımlayan sorgu parametresi. Listesini isteğe bağlı parametreleri gibi sorgu `responseFilter` ve `textDecorations`, bkz: [sorgu parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#query-parameters). Tüm sorgu parametre değerleri URL kodlanmış olmalıdır.  
  
İstek belirtmelisiniz [Apim abonelik anahtar Ocp](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#subscriptionkey) üstbilgi. İsteğe bağlı olsa da, aşağıdaki üst bilgiler de belirtmeniz önerilir:  
  
-   [Kullanıcı Aracısı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#useragent)  
-   [X MSEdge ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#clientid)  
-   [X arama ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#clientip)  
-   [X arama konumu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#location)  

İstemci IP ve konum üstbilgileri konumu kullanan içerik döndürmek için önemlidir. Örneğin, kullanıcının sorgu ise *yelken açmaya ne dersiniz + dersleri*, büyük olasılıkla konumlarına bulunan dersleri ilgi. Kullanıcının konumuna kullanılabilir dersleri içerecek şekilde sonuçları istiyorsanız, konum üstbilgisi ve isteğe bağlı olarak, istemci IP üstbilgisi eklemeniz gerekir. Sorgu Terime bir konum (örneğin, yelken açmaya ne dersiniz + dersleri İzmir ' + anahtarları) açıkça değinmektedir, daha az önemlidir. 

Tüm istek ve yanıt üstbilgileri listesi için bkz: [üstbilgileri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#headers).

## <a name="the-request"></a>İstek

Tüm üstbilgiler ve önerilen sorgu parametreleri içeren bir arama isteğine gösterir. Herhangi bir Bing API'leri çağırma ilk kez ise, istemci kimliği üstbilgisi eklemeyin. Yalnızca istemci kimliği, daha önce Bing API'si çağırdıktan ve kullanıcı ve aygıt bileşimi için bir istemci kimliği Bing döndürülen içerir. 
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+lessons+seattle&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
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
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=70BE289346...",
        "totalEstimatedMatches" : 982000,
        "value" : [{
            "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.0",
            "name" : "Seattle Sailing Club | Lessons, Membership & More",
            "url" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FE...",
            "displayUrl" : "https:\/\/seattlesailing.com",
            "snippet" : "Sailing is fun! Seattle Sailing Club makes it accessible and affordable...",
            "deepLinks" : [{
                "name" : "Learn to Sail",
                "url" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FEFDAD..."
            },
            {
                "name" : "Fleet",
                "url" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FEFDAD38...",
                "snippet" : "The Seattle Sailing Club is nothing without our fleet of excellent..."
            },
            {
                "name" : "Sailing Lessons",
                "url" : "http:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FEFDAD38...",
                "snippet" : "Sailing Lessons. Taking the first step into the sailing community..."
            },
            {
                "name" : "Membership",
                "url" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FEFDAD3...",
                "snippet" : "Seattle sailboat rental for less than the cost of moorage!"
            },
            {
                "name" : "Hours – Directions",
                "url" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED4594874...",
                "snippet" : "Contact the Seattle Sailing Club by phone or email..."
            },
            {
                "name" : "Overnight Cruising",
                "url" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FEFDAD...",
                "snippet" : "Overnight Cruising. Setting sail for a multi-day adventure..."
            }],
            "dateLastCrawled" : "2017-04-07T02:25:00"
        },
        {
            "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.1",
            "name" : "Best Sailing lessons in Seattle, WA - yelp.com",
            "url" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FEFDAD38AA...",
            "displayUrl" : "https:\/\/www.yelp.com\/search?find_desc=sailing+lessons&find_loc...",
            "snippet" : "Reviews on Sailing lessons in Seattle, WA - Seattle Sailing...",
            "dateLastCrawled" : "2017-04-01T10:49:00"
        },
        {
            "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.2",
            "name" : "Seattle Sailing with Windworks Sailing Club - Sailing ...",
            "url" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FEFDAD3...",
            "displayUrl" : "https:\/\/www.windworkssailing.com",
            "snippet" : "Seattle sailing club for sailing lessons, sailboat charters...",
            "dateLastCrawled" : "2017-04-09T03:33:00"
        },
        {
            "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.3",
            "name" : "Sailing Lessons in Downtown Seattle - SheSails Seattle",
            "url" : "http:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FEFDAD38...",
            "displayUrl" : "shesailsseattle.com",
            "snippet" : "SheSails Seattle is sailing lessons and sailboat charters in a fun...",
            "dateLastCrawled" : "2017-04-05T07:15:00"
        },
        {
            "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.4",
            "name" : "Seattle Sailing Club - 19 Photos & 27 Reviews - Boating ...",
            "url" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FE...",
            "displayUrl" : "https:\/\/www.yelp.com\/biz\/seattle-sailing-club-seattle",
            "snippet" : "27 reviews of Seattle Sailing Club \"Love the Seattle Sailing...",
            "dateLastCrawled" : "2017-04-08T14:40:00"
        },
        {
            "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.5",
            "name" : "Puget Sound Sailing - Seattle and Tacoma - PUGET SOUND ...",
            "url" : "http:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FEFDA...",
            "displayUrl" : "www.pugetsoundsailing.com",
            "snippet" : "Puget Sound Sailing Institute is an American Sailing Association...",
            "dateLastCrawled" : "2017-04-09T01:39:00"
        },
        {
            "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.6",
            "name" : "Seattle Yacht Club - Official Site",
            "url" : "http:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FE...",
            "displayUrl" : "www.seattleyachtclub.org",
            "snippet" : "Seattle Yacht Club Awarded Joe Prosser Trophy for Excellence...",
            "dateLastCrawled" : "2017-04-09T14:30:00"
        }],
        "someResultsRemoved" : true
    },
    "relatedSearches" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#RelatedSearches",
        "value" : [{
            "text" : "sailing lessons",
            "displayText" : "sailing lessons",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=70BE289346E..."
        },
        {
            "text" : "sailing schools seattle",
            "displayText" : "sailing schools seattle",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED4..."
        },
        {
            "text" : "seattle boating lessons",
            "displayText" : "seattle boating lessons",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED4..."
        },
        {
            "text" : "adult sailing lessons seattle",
            "displayText" : "adult sailing lessons seattle",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED..."
        },
        {
            "text" : "seattle sailing club",
            "displayText" : "seattle sailing club",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED459..."
        },
        {
            "text" : "seattle sailing club lessons",
            "displayText" : "seattle sailing club lessons",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=70BE289..."
        },
        {
            "text" : "sailing in seattle area",
            "displayText" : "sailing in seattle area",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED45948..."
        },
        {
            "text" : "seattle sailing lessons lake union",
            "displayText" : "seattle sailing lessons lake union",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED459..."
        }]
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
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 6,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.6"
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

## <a name="next-steps"></a>Sonraki adımlar

Out API deneyin. Git [Web arama API sınama Konsolu](https://dev.cognitive.microsoft.com/docs/services/56b43eeccf5ff8098cef3807/operations/56b4447dcf5ff8098cef380d). 

Yanıt nesneleri kullanma hakkında daha fazla bilgi için bkz [Web arama](./search-the-web.md).