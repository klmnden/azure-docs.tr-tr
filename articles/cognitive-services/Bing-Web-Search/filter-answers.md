---
title: Bing döndüren web yanıtları filtreleme | Microsoft Docs
description: ResponseFilter Bing Web arama API döndürür yanıtlar filtrelemek için nasıl kullanılacağını gösterir.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 8B837DC2-70F1-41C7-9496-11EDFD1A888D
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 01/12/2017
ms.author: scottwhi
ms.openlocfilehash: a5ee6241630ee24a05c2c4b932453bd7946a7508
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352900"
---
# <a name="filtering-the-answers-that-the-search-response-includes"></a>Arama yanıtı içeren yanıtlar filtreleme  

Web sorguladığınızda, Bing, düşündüğü tüm içeriği aramayı ilgili döndürür. Örneğin, arama sorgusu "yelken açmaya ne dersiniz + dinghies" ise, yanıt aşağıdaki yanıtlar içerebilir:

```json
{
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3A43C...",
        "totalEstimatedMatches" : 262000,
        "value" : [...]
    },
    "images" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images",
        "readLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=sail...",
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3A43CA5CA6464E5D...",
        "isFamilyFriendly" : true,
        "value" : [...]
    },
    "rankingResponse" : {
        "mainline" : {
            "items" : [...]
        }
    }
}    
```

İçerik resim, video ve haber gibi belirli türlerdeki ilginizi çekiyorsa kullanarak yalnızca bu yanıtlar isteyebilir [responseFilter](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#responsefilter) sorgu parametresi. Bing belirtilen yanıtlar için ilgili içeriğin bulursa, onu Bing döndürür. Yanıt filtresi yanıtlar, virgülle ayrılmış bir listesidir. Aşağıdaki nasıl kullanılacağını gösterir `responseFilter` isteği resim, video ve Yelkenli dinghies haberler için. Sorgu dizesi kodlarken virgül %2 C'ye değiştirin.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&responseFilter=images%2Cvideos%2Cnews&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location:  47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Önceki sorgu yanıtı gösterir. Gördüğünüz gibi yanıt bunları içermeyen şekilde Bing ilgili video ve haber sonuçları bulunamadı.

```json
{
    "_type" : "SearchResponse",
    "images" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images",
        "readLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=sail...",
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3AD78B183C56456C...",
        "isFamilyFriendly" : true,
        "value" : [...]
    },
    "rankingResponse" : {
        "mainline" : {
            "items" : [{
                "answerType" : "Images",
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images"
                }
            }]
        }
    }
}
```

Bing önceki yanıtta video ve haber sonuç döndürmedi rağmen video ve haber içerik yok gelmez. Bu, yalnızca sayfa bunları eklemediniz anlamına gelir. Ancak, varsa, [sayfa](./paging-webpages.md) daha fazla sonuç, sihirbazın sonraki sayfalarında olasılıkla bunları içerir. Ayrıca, çağırırsanız [Video arama API](../bing-video-search/search-the-web.md) ve [haber arama API](../bing-news-search/search-the-web.md) doğrudan uç noktaları yanıt sonuçları olasılıkla içerecektir. 

Kullanımından önerilmez `responseFilter` tek bir API öğesinden sonuçları elde etmek için. Tek bir Bing API'si içerikten istiyorsanız, bu API doğrudan çağırabilir. Örneğin, yalnızca görüntüleri almak için görüntü arama API uç noktası için bir istek göndermesini `https://api.cognitive.microsoft.com/bing/v7.0/images/search` veya başka biri [görüntüleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#endpoints) uç noktaları. Tek API çağırma yalnızca performans nedenleriyle önemli ancak içeriği özgü API'ları daha zengin sonuçlar sunar. Örneğin, Web arama sonuçlarını filtrelemek için API için kullanılabilir değil filtreleri kullanabilirsiniz.  
  
Arama Sonuçları belirli bir etki alanından almak için dahil `site:` sorgu dizesinde sorgu işleci.  

```
https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us
```

> [!NOTE] 
> Sorgu kullanırsanız, bağlı olarak `site:` sorgu işleci yanıt bağımsız olarak, yetişkinlere yönelik içeriğe içerebilir fırsat yok [güvenli arama](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#safesearch) ayarı. Kullanmanız gereken `site:` yalnızca sitedeki içeriğin farkında ve senaryonuz yetişkinlere yönelik içeriğe olasılığını destekler. 
  
## <a name="limiting-the-number-of-answers-in-the-response"></a>Yanıtlar yanıt sayısını sınırlandırma

Bing yanıtlar üzerinde sıralamasına göre yanıtı içerir. Örneğin, sorgu, *yelken açmaya ne dersiniz + dinghies*, Bing döndürür `webpages`, `images`, `videos`, ve `relatedSearches`.

```json
{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "sailing dinghies"
    },
    "webPages" : {...},
    "images" : {...},
    "relatedSearches" : {...},
    "videos" : {...},
    "rankingResponse" : {...}
}
```

Bu Bing yanıtlar sayısını sınırlamak için kümesi (Web sayfalarını ve görüntüleri), üstteki iki yanıtlarını döndürür [answerCount](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#answercount) sorgu parametresi 2. 

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&answerCount=2&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location:  47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Yanıt yalnızca içeriyor `webPages` ve `images`.

```json
{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "sailing dinghies"
    },
    "webPages" : {...},
    "images" : {...},
    "rankingResponse" : {...}
}
```

Eklerseniz `responseFilter` sorgu parametresi önceki sorgu ve Web sayfalarını ve haberler, yanıt içerdiği yalnızca Web sayfalarının haber değil derece çünkü kümesi.

```json
{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "sailing dinghies"
    },
    "webPages" : {...},
    "rankingResponse" : {...}
}
```

## <a name="promoting-answers-that-are-not-ranked"></a>Değil sıralanır yanıtlar yükseltme

Bir sorgu için Bing döndürür yanıtlar derece üst Web sayfaları, resim, video ve relatedSearches varsa, yanıt Bu yanıtlar içerir. Ayarlarsanız [answerCount](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#answercount) iki (2), Bing döndürür dereceli iki yanıtlar için: Web sayfalarını ve görüntüler. Bing görüntüler ve videolar yanıta dahil etmek istiyorsanız, belirtin [Yükselt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#promote) sorgu parametresi ve görüntüleri ve videolar ayarlayın. 

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&answerCount=2&promote=images%2Cvideos&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location:  47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Yukarıdaki isteğinin yanıtı verilmiştir. Bing üst iki yanıtlar, Web sayfalarını ve görüntüleri döndürür ve videolar yanıt yükseltir.

```json
{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "sailiing dinghies"
    },
    "webPages" : {...},
    "images" : {...},
    "videos" : {...},
    "rankingResponse" : {...}
}
```

Ayarlarsanız `promote` dereceli yanıt olmadığından haberleri ile haber yanıt yanıt içermeyen&mdash;yanıtlar derece yalnızca yükseltebilirsiniz.

Yükseltmek istediğiniz yanıtları karşı sayılmaz `answerCount` sınırı. Örneğin, dereceli yanıtlar Haberler, görüntüler ve videolar ve ayarladığınız `answerCount` 1 ve `promote` haberleri ile yanıtı, Haberler ve görüntüleri içeriyor. Veya dereceli yanıtlar videoları, görüntüler ve haber varsa, videoları ve haber yanıtı içerir.

Kullanabilirsiniz `promote` yalnızca belirtirseniz `answerCount` sorgu parametresi.
