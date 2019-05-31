---
title: Resim arama API'si hızlı başlangıç | Microsoft Docs
description: Bing görüntü arama API'sini kullanmaya başlama işlemini gösterir.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: BE2B2F8C-20B5-4E0B-AEC8-EAD505BA4C85
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: article
ms.date: 03/04/2019
ms.author: scottwhi
ms.openlocfilehash: 006b611bd2dcc94b1b651de7789e165a5c45a17b
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66383766"
---
# <a name="your-first-images-search-query"></a>Sorgu ilk görüntülerinizi arama

İlk çağrınızı yapmadan önce bir Bing Arama Bilişsel Hizmetler abonelik anahtarı almanız gerekir. Anahtar almak için bkz. [Bilişsel Hizmetleri Deneme](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api).  Ayrıca bkz: [Bilişsel hizmetler fiyatlandırması - Bing arama API'si](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

Görüntü arama sonuçlarını almak için aşağıdaki uç noktaya bir GET isteği gönder:  
  
```
https://api.cognitive.microsoft.com/bing/v7.0/images/search
```
  
İstek, HTTPS protokolünü kullanmalıdır.

Tüm isteklerin bir sunucudan gönderilmesini öneririz. Anahtarı bir istemci uygulamanın parçası olarak dağıtmak, kötü amaçlı bir üçüncü tarafa anahtara erişmek için daha fazla fırsat sunar. Ayrıca bir sunucudan çağrı yapmak API'nin gelecek sürümleri için tek bir yükseltme noktası sağlar.

İstek kullanıcının arama terimini içeren [q](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#query) sorgu parametresini belirtmelidir. Tercihe bağlı olmakla birlikte, istek, sonuçların gelmesini istediğiniz pazarı tanımlayan [mkt](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#mkt) sorgu parametresini de belirtmelidir. `freshness` ve `size` gibi isteğe bağlı sorgu parametrelerinin bir listesi için bkz. [Sorgu Parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#query-parameters). Tüm sorgu parametre değerleri URL olarak kodlanmış olmalıdır.  
  
İstek [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#subscriptionkey) üstbilgisini belirtmelidir. İsteğe bağlı olmakla birlikte şu üstbilgileri de belirtmeniz önerilir:  
  
-   [User-Agent](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#useragent)  
-   [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#clientid)  
-   [X-Search-ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#clientip)  
-   [X-Search-Location](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#location)  

İstemci IP'si ve konum üstbilgileri konuma duyarlı içerik döndürmek için önemlidir.  

Tüm istek ve yanıt üstbilgilerinin bir listesi için bkz. [Üstbilgiler](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#headers).

## <a name="the-request"></a>İstek

Aşağıda önerilen tüm sorgu parametrelerini ve üst bilgilerini içeren bir arama isteği gösterilmektedir. Bu ise ilk kez herhangi bir Bing API'leri çağırma, istemci kimliği üst bilgisi içermez. İstemci kimliğini yalnızca önceden bir Bing API'sini çağırdıysanız ve Bing, kullanıcı ve cihaz birleşimi için bir istemci kimliği döndürdüyse dahil edin. 
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Aşağıda, bir önceki isteğin yanıtı gösterilmektedir.

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

API’yi deneyin. Git [resim arama API'si sınama Konsolu'nu](https://dev.cognitive.microsoft.com/docs/services/8336afba49a84475ba401758c0dbf749/operations/571fab09dbe2d933e891028f). 

Yanıt nesnelerini kullanma ile ilgili ayrıntılar için bkz. [Web’de arama yapma](./search-the-web.md).

Görüntüyü ya da tanındı kişiler görüntüsüne dahil etme web sayfaları gibi bir görüntü ile ilgili Öngörüler alma hakkında daha fazla bilgi için bkz [resim öngörüleri](./image-insights.md).  
  
Sosyal medyada popüler görüntüleri hakkında daha fazla bilgi için bkz. [popüler resimler](./trending-images.md).  
