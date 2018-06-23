---
title: Görüntüleri arama API'si hızlı başlangıç | Microsoft Docs
description: Bing görüntüleri arama API'si ile çalışmaya başlamak gösterilmiştir.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: BE2B2F8C-20B5-4E0B-AEC8-EAD505BA4C85
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 9e211cf5acd17ab80948d0b7161bdd2a9220c4a6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351622"
---
# <a name="your-first-images-search-query"></a>Sorgu ilk görüntülerinizi arama

İlk aramanız yapabilmeniz için önce bir Bing arama Bilişsel hizmetler abonelik anahtarı almanız gerekir. Bir anahtar almak için bkz: [deneyin Bilişsel Hizmetler](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api).

Resim arama sonuçları almak için aşağıdaki uç noktaya bir GET isteği gönder:  
  
```
https://api.cognitive.microsoft.com/bing/v7.0/images/search
```
  
İstek HTTPS protokolünü kullanmanız gerekir.

Tüm istekleri bir sunucusundan kaynaklanan öneririz. Bir istemci uygulaması bir parçası olarak anahtar dağıtma bir kötü amaçlı üçüncü erişmek taraf için daha fazla fırsatı sağlar. Ayrıca, bir sunucudan çağrıları yapma tek bir yükseltme noktası API gelecek sürümlerinde sağlar.

İstek belirtmelisiniz [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query) kullanıcının arama terimi içeren sorgu parametresi. İsteğe bağlı olsa da, istek de belirtmeniz gerekir [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#mkt) alınması için sonuçları istediğiniz Pazar tanımlayan sorgu parametresi. Listesini isteğe bağlı parametreleri gibi sorgu `freshness` ve `size`, bkz: [sorgu parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query-parameters). Tüm sorgu parametre değerleri URL kodlanmış olmalıdır.  
  
İstek belirtmelisiniz [Apim abonelik anahtar Ocp](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#subscriptionkey) üstbilgi. İsteğe bağlı olsa da, aşağıdaki üst bilgiler de belirtmeniz önerilir:  
  
-   [Kullanıcı Aracısı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#useragent)  
-   [X MSEdge ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#clientid)  
-   [X arama ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#clientip)  
-   [X arama konumu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#location)  

İstemci IP ve konum üstbilgileri konumu kullanan içerik döndürmek için önemlidir.  

Tüm istek ve yanıt üstbilgileri listesi için bkz: [üstbilgileri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#headers).

## <a name="the-request"></a>İstek

Tüm üstbilgiler ve önerilen sorgu parametreleri içeren bir arama isteğine gösterir. Eğer öyleyse ilk zaman herhangi bir Bing API'leri çağırma, istemci kimliği üstbilgisi dahil etmeyin. Yalnızca istemci kimliği, daha önce Bing API'si çağırdıktan ve kullanıcı ve aygıt bileşimi için bir istemci kimliği Bing döndürülen içerir. 
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Önceki istek yanıtı gösterir.

```json
{
    "_type" : "Images",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=73118C8B4E3...",
    "totalEstimatedMatches" : 838,
    "value" : [
        {
            "name" : "File:Rich Passage Minto Sailing Dinghy.jpg - Wikipedia",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=73118C8B4E3...",
            "thumbnailUrl" : "https:\/\/tse1.mm.bing.net\/th?id=OIP.GNarK7m...",
            "datePublished" : "2011-10-29T11:26:00",
            "contentUrl" : "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3...",
            "hostPageUrl" : "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3687...",
            "contentSize" : "79239 B",
            "encodingFormat" : "jpeg",
            "hostPageDisplayUrl" : "en.wikipedia.org\/wiki\/File:Rich_Passage...",
            "width" : 526,
            "height" : 688,
            "thumbnail" : {
                "width" : 229,
                "height" : 300
            },
            "imageInsightsToken" : "ccid_GNarK7ma*mid_CCF85447ADA6...",
            "insightsSourcesSummary" : {
                "shoppingSourcesCount" : 0,
                "recipeSourcesCount" : 0
            },
            "imageId" : "CCF85447ADA6FFF9E96E7DF0B796F7A86E34593",
            "accentColor" : "376094"
        },
        . . .
    ],
    "queryExpansions" : [
        {
            "text" : "Small Sailing Dinghies",
            "displayText" : "Small",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=73118C8B4...",
            "searchLink" : "https:\/\/api.cognitive.microsoft.com\/api...",
            "thumbnail" : {
                "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?q=..."
            }
        },
        . . .
    ],
    "nextOffset" : 10,
    "pivotSuggestions" : [
        {
            "pivot" : "sailing",
            "suggestions" : []
        },
        {
            "pivot" : "dinghies",
            "suggestions" : [
                {
                    "text" : "Sailing Tender",
                    "displayText" : "Tender",
                    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=73118...",
                    "searchLink" : "https:\/\/api.cognitive.microsoft.com\/api...",
                    "thumbnail" : {
                        "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?q=..."
                    }
                },
                . . .
            ]
        }
    ],
    "displayShoppingSourcesBadges" : false,
    "displayRecipeSourcesBadges" : true
}
```

## <a name="next-steps"></a>Sonraki adımlar

Out API deneyin. Git [görüntü arama API sınama Konsolu'nu](https://dev.cognitive.microsoft.com/docs/services/8336afba49a84475ba401758c0dbf749/operations/571fab09dbe2d933e891028f). 

Yanıt nesneleri kullanma hakkında daha fazla bilgi için bkz [Web arama](./search-the-web.md).

Görüntü veya tanındı kişiler görüntüde yer aldığı web sayfaları gibi bir görüntü ile ilgili Öngörüler alma hakkında daha fazla ayrıntı için bkz: [görüntü Öngörüler](./image-insights.md).  
  
Sosyal medya üzerinde eğilim görüntüleri hakkında daha fazla ayrıntı için bkz: [eğilim görüntüleri](./trending-images.md).  
