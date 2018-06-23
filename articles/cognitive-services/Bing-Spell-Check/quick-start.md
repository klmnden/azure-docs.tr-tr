---
title: Bing yazım denetleme API'si hızlı başlangıç | Microsoft Docs
description: Bing yazım denetleme API'si ile çalışmaya başlamak gösterilmiştir.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: AF8EB1F0-386D-4555-9354-735611435F04
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: article
ms.date: 06/21/2016
ms.author: scottwhi
ms.openlocfilehash: cae8353e5be6e70eca90e5995b29b6774fc7d6a9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351502"
---
# <a name="your-first-spell-check-request"></a>İlk yazım istek denetleyin

İlk aramanız yapabilmeniz için önce bir Bilişsel hizmetler abonelik anahtarı almanız gerekir. Bir anahtar almak için bkz: [deneyin Bilişsel Hizmetler](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api).

Bir metin dizesi yazım ve dilbilgisi hataları denetlemek için aşağıdaki uç noktasına bir GET isteği gönder:  
  
```
https://api.cognitive.microsoft.com/bing/v5.0/spellcheck
```

> [!NOTE]
> V7 Önizleme uç noktası:
> 
> ```
> https://api.cognitive.microsoft.com/bing/v7.0/spellcheck
> ```  
  
İstek HTTPS protokolünü kullanmanız gerekir.

Tüm istekleri bir sunucusundan kaynaklanan öneririz. Bir istemci uygulaması bir parçası olarak anahtar dağıtma bir kötü amaçlı üçüncü erişmek taraf için daha fazla fırsatı sağlar. Ayrıca, bir sunucudan çağrıları yapma tek bir yükseltme noktası API gelecek sürümlerinde sağlar.

İstek belirtmelisiniz [metin](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#text) sağlaması için metin dizesini içeren sorgu parametresi. İstek, ayrıca isteğe bağlı olsa da belirtmeniz gerekir [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#mkt) alınması için sonuçları istediğiniz Pazar tanımlayan sorgu parametresi. Listesini isteğe bağlı parametreleri gibi sorgu `mode`, bkz: [sorgu parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#query-parameters). Tüm sorgu parametre değerleri URL kodlanmış olmalıdır.  
  
İstek belirtmelisiniz [Apim abonelik anahtar Ocp](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#subscriptionkey) üstbilgi. İsteğe bağlı olsa da, aşağıdaki üst bilgiler de belirtmeniz önerilir:  
  
-   [Kullanıcı Aracısı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#useragent)  
-   [X MSEdge ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#clientid)  
-   [X arama ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#clientip)  
-   [X arama konumu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#location)  

Tüm istek ve yanıt üstbilgileri listesi için bkz: [üstbilgileri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#headers).

## <a name="the-request"></a>İstek

Tüm üstbilgiler ve önerilen sorgu parametreleri içeren bir isteği gösterir. Herhangi bir Bing API'leri çağırma ilk kez ise, istemci kimliği üstbilgisi eklemeyin. Yalnızca istemci kimliği, daha önce Bing API'si çağırdıktan ve kullanıcı ve aygıt bileşimi için bir istemci kimliği Bing döndürülen içerir. 
  
```  
GET https://api.cognitive.microsoft.com/bing/v5.0/spellcheck?text=when+its+your+turn+turn,+john,+come+runing&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

> [!NOTE]
> V7 Önizleme isteği:

> ```  
> GET https://api.cognitive.microsoft.com/bing/v7.0/spellcheck?text=when+its+your+turn+turn,+john,+come+runing&mkt=en-us HTTP/1.1
> Ocp-Apim-Subscription-Key: 123456789ABCDE  
> X-MSEdge-ClientIP: 999.999.999.999  
> X-Search-Location: lat:47.60357;long:-122.3295;re:100  
> X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
> Host: api.cognitive.microsoft.com  
> ```  

Önceki istek yanıtı gösterir. Bu örnek ayrıca Bing özgü yanıt üstbilgilerini gösterir.

```
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{  
    "_type" : "SpellCheck",  
    "flaggedTokens" : [{  
        "offset" : 5,  
        "token" : "its",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "it's",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 25,  
        "token" : "john",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "John",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 19,  
        "token" : "turn",  
        "type" : "RepeatedToken",  
        "suggestions" : [{  
            "suggestion" : "",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 35,  
        "token" : "runing",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "running",  
            "score" : 1  
        }]  
    }]  
}  
```  


## <a name="next-steps"></a>Sonraki adımlar

Out API deneyin. Git [yazım denetimi API'si sınama Konsolu'nu](https://dev.cognitive.microsoft.com/docs/services/56e73033cf5ff80c2008c679/operations/57855119bca1df1c647bc358). 

Yanıt nesneleri kullanma hakkında daha fazla bilgi için bkz [yazım denetimi metin dizelerini](./proof-text.md).

