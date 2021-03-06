---
title: Arama sonuçları - Bing Web araması API'si filtreleme
titleSuffix: Azure Cognitive Services
description: Filtre ve Bing Web araması API'si arama sonuçları görüntüleme hakkında bilgi edinin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: 8B837DC2-70F1-41C7-9496-11EDFD1A888D
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 07/08/2019
ms.author: scottwhi
ms.openlocfilehash: a89d73b63680415aa8e516926b8e1d6c59ffbbad
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67626013"
---
# <a name="filtering-the-answers-that-the-search-response-includes"></a>Arama yanıtı içeren yanıtlar filtreleme  

Web sorguladığınızda, Bing arama bulduğu tüm ilgili içeriği döndürür. Örneğin, arama sorgusu "yelken açmaya ne dersiniz + dinghies" ise, yanıt aşağıdaki soruların yanıtlarını içerebilir:

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

## <a name="query-parameters"></a>Sorgu parametreleri

Bing tarafından döndürülen yanıtların filtrelemek için kullanmak aşağıda API'nin çağrılması durumunda sorgu parametreleri.  

### <a name="responsefilter"></a>ResponseFilter

Bing yanıta (örneğin görüntü, video ve haber) içeren bir yanıt türleri kullanarak filtreleyebilirsiniz [responseFilter](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#responsefilter) yanıtlar virgülle ayrılmış bir listesi olan sorgu parametresi. Bing için ilgili içeriğin bulursa bir yanıt yanıt olarak dahil edilir. 

Görüntüleri gibi yanıtından belirli yanıtlar dışlamak için önüne ekleyin bir `-` karakter yanıt türü. Örneğin:

```
&responseFilter=-images,-videos
```

Aşağıdakileri nasıl kullanılacağını gösterir `responseFilter` isteği görüntüleri, videolar ve Haberler Yelkenli dinghies. Sorgu dizesini kodlayın, virgül, %2 C değiştirin.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&responseFilter=images%2Cvideos%2Cnews&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location:  47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Aşağıda, bir önceki sorgunun yanıtı gösterilmektedir. İlgili video ve haber sonuçları Bing bulamadı çünkü yanıt onları içermez.

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

Bing video ve haber sonuçları önceki yanıtta döndürmedi olsa da, video ve haber içeriği yok gelmez. Yalnızca sayfanın bunları eklemediğiniz anlamına gelir. Ancak, varsa, [sayfa](./paging-webpages.md) daha fazla sonuç, sonraki sayfalarda büyük olasılıkla bunları verilebilir. Ayrıca, eğer [Video arama API'si](../bing-video-search/search-the-web.md) ve [haber arama API'si](../bing-news-search/search-the-web.md) doğrudan uç noktaları yanıt sonuçları büyük olasılıkla içerecektir.

Kullanarak önerilmez `responseFilter` tek bir API'den sonuçları elde etmek için. Tek bir Bing API içerikten istiyorsanız doğrudan bu API'ye çağrı. Örneğin, yalnızca görüntüleri almak için resim arama API'si uç noktaya bir istek gönderin `https://api.cognitive.microsoft.com/bing/v7.0/images/search` veya diğer [görüntüleri](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#endpoints) uç noktaları. Tek bir API çağırma yalnızca performansı artırmak için önemli olduğu ancak daha zengin sonuçları içerik özel API'ler sunar. Örneğin, sonuçları filtrelemek için Web araması API'si kullanılabilir değil filtreleri kullanabilirsiniz.  

### <a name="site"></a>Site

Belirli bir etki alanına ait arama sonuçlarını almak için dahil `site:` sorgu, sorgu dizesi parametresi.  

```
https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us
```

> [!NOTE]
> Kullanırsanız sorguya bağlı olarak `site:` sorgu işleci yok yanıt bağımsız olarak, yetişkinlere yönelik içerik içerebilir olasılığını [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#safesearch) ayarı. `site:` işlecini yalnızca sitenin içeriği hakkında bilgi sahibiyseniz ve senaryonuz, yetişkinlere yönelik içeriğin mevcut olma ihtimalini destekliyorsa kullanın.

### <a name="freshness"></a>Yenilik

Belirli bir dönemde Bing bulunan Web sayfalarının web yanıt sonuçlarını sınırlamak için ayarlanmış [güncellik](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#freshness) sorgu parametresi için büyük küçük harf duyarsız aşağıdaki değerlerden biri:

* `Day` — Bing son 24 saat içinde bulunan Web sayfalarının döndürür
* `Week` — Bing son 7 gün içinde bulunan Web sayfalarının döndürür
* `Month` — Son 30 gün içinde bulunan Web sayfalarının döndürür

Bu parametre ayrıca formunda, özel bir tarih aralığı için ayarlayabilir `YYYY-MM-DD..YYYY-MM-DD`. 

`https://<host>/bing/v7.0/search?q=ipad+updates&freshness=2019-02-01..2019-05-30`

Tek bir tarih sonuçlarını sınırlamak için belirli bir tarihe güncellik parametresini ayarlayın:

`https://<host>/bing/v7.0/search?q=ipad+updates&freshness=2019-02-04`

Sonuçları Bing filtre ölçütlerinizle eşleşen Web sayfalarını sayısını istediğiniz Web sayfaları (veya Bing döndüren varsayılan numarası) sayısından daha az ise belirtilen dönemini dışında Web sayfalarının içerebilir.

## <a name="limiting-the-number-of-answers-in-the-response"></a>Yanıtlar yanıt sayısını sınırlandırma

Bing birden çok yanıt türü içinde JSON yanıtı döndürebilir. Örneğin, sorgu, *yelken açmaya ne dersiniz + dinghies*, Bing döndürebilir `webpages`, `images`, `videos`, ve `relatedSearches`.

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

Bu Bing yanıtlarını sayısını sınırlamak için kümesi (Web sayfalarını ve görüntüleri), ilk iki yanıtlarını döndürür [answerCount](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#answercount) sorgu parametresi için 2.

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

Eklerseniz `responseFilter` sorgu parametresi önceki sorgu ve Web sayfalarını ve haber, yanıtın içerdiği yalnızca Web sayfalarını haber önem derecesi değil çünkü kümesi.

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

## <a name="promoting-answers-that-are-not-ranked"></a>Değil sıralanır yanıtlar yükseltiliyor

Bing döndüren bir sorgu için yanıtlar sıralanmış üst Web sayfaları, görüntü, video ve relatedSearches ise bu yanıtlar yanıt verilebilir. Ayarlarsanız [answerCount](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#answercount) iki (2), Bing döndürür dereceli iki yanıtlar için: Web sayfaları ve görüntüler. Bing görüntü ve video yanıta dahil etmek istiyorsanız, belirtin [Yükselt](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#promote) sorgu parametresi ve resimler ve videolar için ayarlayın.

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&answerCount=2&promote=images%2Cvideos&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location:  47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Yukarıdaki isteğin yanıtını verilmiştir. Bing üst iki yanıt birbiri, Web sayfaları ve görüntü döndürür ve videoları yanıt yükseltir.

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

Ayarlarsanız `promote` kişilerinin sıralı bir yanıt olmadığından haberler için haber yanıt yanıt içermeyen&mdash;yanıtlar sıralanmış yalnızca yükseltebilirsiniz.

Yükseltmek istediğiniz yanıtları karşı sayılmaz `answerCount` sınırı. Örneğin, haber görüntüleri ve videoları dereceli yanıtları olan ve ayarlarsanız `answerCount` 1 ve `promote` haberler için yanıt haberleri ve görüntüleri içerir. Veya dereceli yanıtları videoları, resimleri ve haberler, videolar ve Haberler yanıtı içerir.

Kullanmanızı `promote` yalnızca belirtirseniz `answerCount` sorgu parametresi.
