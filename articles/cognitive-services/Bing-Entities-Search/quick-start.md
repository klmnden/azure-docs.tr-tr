---
title: Varlıkları arama API'si hızlı başlangıç | Microsoft Docs
description: Bing varlıkları arama API'si ile çalışmaya başlamak gösterilmiştir.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: B206A254-B7E9-49FF-AFD5-87B1E4D6D30B
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 07/06/2017
ms.author: scottwhi
ms.openlocfilehash: 12031d2447920c7e2d6180f35cf4fb29aa1b6150
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351526"
---
# <a name="making-your-first-entities-request"></a>İlk varlıklarınızı isteği yapma

Varlık arama API için Bing arama sorgusu gönderir ve varlıkları ve basamak içeren sonuçları alır. Yerleştir sonuçları Restoran, otel veya diğer yerel işletmeler içerir. Bir yerde için (örneğin, Restoran bana yakın) listesi isteyebilir veya sorgu yerel iş adını belirtebilirsiniz. Varlık sonuçları, kişiler, yerler veya öğeleri içerir. Bu bağlamda yerdir turistik yerleri, durumları, ülkelerde vb. 

## <a name="first-steps"></a>İlk adımlar

İlk aramanız yapabilmeniz için önce bir Bilişsel hizmetler abonelik anahtarı almanız gerekir. Bir anahtar almak için bkz: [deneyin Bilişsel Hizmetler](https://azure.microsoft.com/try/cognitive-services/?api=bing-entities-search-api). (Varlıkları arama API üstünde görünür durumda değilse tıklatın **arama** sekmesinde ve onu görene kadar aşağı kaydırın.)

## <a name="the-endpoint"></a>Uç nokta

Varlık alma ve arama sonuçlarını yerleştirmek için aşağıdaki uç noktaya bir GET isteği gönder:  
  
```
https://api.cognitive.microsoft.com/bing/v7.0/entities
```

İstek HTTPS protokolünü kullanmanız gerekir.

Tüm istekleri bir sunucusundan kaynaklanan öneririz. Bir istemci uygulaması bir parçası olarak anahtar dağıtma erişmek kötü amaçlı bir üçüncü taraf için daha fazla fırsatı sağlar. Ayrıca, bir sunucudan çağrıları yapma tek bir yükseltme noktası API gelecek sürümlerinde sağlar.

## <a name="specifying-query-parameters-and-headers"></a>Sorgu parametrelerinin ve üst bilgileri belirtme

İstek belirtmelisiniz [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#query) kullanıcının arama terimi içeren sorgu parametresi. İstek de belirtmeniz gerekir [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#mkt) alınması için sonuçları istediğiniz Pazar tanımlayan sorgu parametresi. İsteğe bağlı sorgu parametrelerin listesi için bkz: [sorgu parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#query-parameters). URL tüm sorgu parametreleri kodlayın.  
  
İstek belirtmelisiniz [Apim abonelik anahtar Ocp](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#subscriptionkey) üstbilgi. İsteğe bağlı olsa da, aşağıdaki üst bilgiler de belirtmeniz önerilir:  
  
-   [Kullanıcı Aracısı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#useragent)  
-   [X MSEdge ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#clientid)  
-   [X MSEdge ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#clientip)  
-   [X arama konumu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#location)  

İstemci IP ve konum üstbilgileri konumu kullanan içerik döndürmek için önemlidir.  

Tüm istek ve yanıt üstbilgileri listesi için bkz: [üstbilgileri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#headers).

## <a name="the-request"></a>İstek

Tüm üstbilgiler ve önerilen sorgu parametreleri içeren bir varlık isteği gösterir. 

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/entities?q=mount+rainier&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Herhangi bir Bing API'leri çağırma ilk kez ise, istemci kimliği üstbilgisi eklemeyin. Yalnızca istemci kimliği, daha önce Bing API'si çağırdıktan ve kullanıcı ve aygıt bileşimi için bir istemci kimliği Bing döndürülen içerir.

## <a name="the-response"></a>Yanıt

Önceki istek yanıtı gösterir. Bu örnek ayrıca Bing özgü yanıt üstbilgilerini gösterir. Yanıt nesnesini hakkında daha fazla bilgi için bkz: [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#searchresponse).

```
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "mount rainier"
    },
    "entities" : {
        "queryScenario" : "DominantEntity",
        "value" : [{
            "contractualRules" : [{
                "_type" : "ContractualRules\/LicenseAttribution",
                "targetPropertyName" : "description",
                "mustBeCloseToContent" : true,
                "license" : {
                    "name" : "CC-BY-SA",
                    "url" : "http:\/\/creativecommons.org\/licenses\/by-sa\/3.0\/"
                },
                "licenseNotice" : "Text under CC-BY-SA license"
            },
            {
                "_type" : "ContractualRules\/LinkAttribution",
                "targetPropertyName" : "description",
                "mustBeCloseToContent" : true,
                "text" : "en.wikipedia.org",
                "url" : "http:\/\/en.wikipedia.org\/wiki\/Mount_Rainier"
            },
            {
                "_type" : "ContractualRules\/MediaAttribution",
                "targetPropertyName" : "image",
                "mustBeCloseToContent" : true,
                "url" : "http:\/\/en.wikipedia.org\/wiki\/Mount_Rainier"
            }],
            "webSearchUrl" : "https:\/\/www.bing.com\/search?q=Mount%20Rainier...",
            "name" : "Mount Rainier",
            "image" : {
                "name" : "Mount Rainier",
                "thumbnailUrl" : "https:\/\/www.bing.com\/th?id=A21890c0e1f...",
                "provider" : [{
                    "_type" : "Organization",
                    "url" : "http:\/\/en.wikipedia.org\/wiki\/Mount_Rainier"
                }],
                "hostPageUrl" : "http:\/\/upload.wikimedia.org\/wikipedia...",
                "width" : 110,
                "height" : 110
            },
            "description" : "Mount Rainier, Mount Tacoma, or Mount Tahoma is the highest...",
            "entityPresentationInfo" : {
                "entityScenario" : "DominantEntity",
                "entityTypeHints" : ["Attraction"],
                "entityTypeDisplayHint" : "Mountain"
            },
            "bingId" : "9ae3e6ca-81ea-6fa1-ffa0-42e1d78906"
        }]
    }
}
```



## <a name="next-steps"></a>Sonraki adımlar

Out API deneyin. Git [varlıkları arama konsol test API](https://dev.cognitive.microsoft.com/docs/services/7a3fb374be374859a823b79fd938cc65/). 

Yanıt nesneleri kullanma hakkında daha fazla bilgi için bkz [Web varlıkları ve yerler için arama](./search-the-web.md).

Okuduğunuzdan emin olun [Bing kullanın ve görüntü gereksinimleri](./use-display-requirements.md) arama sonuçlarını kullanma hakkında kurallardan herhangi birinin kesmeyin şekilde.
