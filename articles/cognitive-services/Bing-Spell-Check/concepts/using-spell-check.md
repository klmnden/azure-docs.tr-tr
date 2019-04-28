---
title: Bing Yazım Denetimi API’sini kullanma
titlesuffix: Azure Cognitive Services
description: Bing yazım denetimi modları, ayarları ve API için ilgili diğer bilgileri hakkında bilgi edinin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: overview
ms.date: 02/20/2019
ms.author: aahi
ms.openlocfilehash: 9544337ef1322e52cbdf123bb48d283485a8c7dd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60896147"
---
# <a name="using-the-bing-spell-check-api"></a>Bing Yazım Denetimi API’sini kullanma

Bing yazım denetimi API'si bağlamsal dil bilgisi ve yazım denetimi kullanma hakkında bilgi edinmek için bu makaleyi kullanın. Sözlüğe dayalı kural kümeleri üzerinde yazım denetleyicileri kullanır, ancak makine öğrenimi ve istatistiksel makine çevirisi doğru ve bağlamsal düzeltmeleri sağlamak için Bing yazım denetleyicisi yararlanır. 

## <a name="spell-check-modes"></a>Yazım denetimi modları

API, `Proof` ve `Spell` olmak üzere iki denetleme modunu destekler.  Örnekleri [burada](https://azure.microsoft.com/services/cognitive-services/spell-check/) deneyebilirsiniz.

### <a name="proof---for-documents"></a>Kanıt - belgeleri 

`Proof`, varsayılan moddur. `Proof` yazım denetimi modu, belge oluşturmaya yardımcı olmak için büyük/küçük harf kullanımını düzeltme ve temel noktalama işaretleri ekleme gibi çeşitli özellikler sunarak en kapsamlı denetimleri sağlar. Ancak bu mod yalnızca en-US (İngilizce-Amerika Birleşik Devletleri), es-ES (İspanyolca) ve pt-BR (Portekizce) pazarlarında kullanılabilir. (Not: İspanyolca ve Portekizce için yalnızca beta sürümünde sunulur). Diğer tüm pazarlar için mode sorgu parametresini Spell olarak ayarlayın. 

> [!NOTE]
> Sorgu metni uzunluğu 4096 aşarsa, 4096 karakterle kesilecek sonra işlenir. 

### <a name="spell----for-web-searchesqueries"></a>-Web aramaları/sorgular için yazım

`Spell`, daha iyi arama sonuçları döndürmek için daha agresif şekilde çalışır. `Spell` modu, çoğu yazım hatasını bulur ancak `Proof` modunda yakalanan, gibi bazı dil bilgisi hatalarını (örneğin, büyük/küçük harf kullanımı ve yinelenen sözcükler ile ilgili hatalar) saptayamaz.

> [!NOTE]
> * Maksimum desteklenen sorgu uzunluğu aşağıda verilmiştir. Sorgu en fazla uzunluğu aşıyor, sorgu ve sonuçları değiştirilmeyecek.
>    * Aşağıdaki dil kodları 130 karakter: tr, de, es, fr, pl, pt, sv, ru, nl, nb, tr-tr, it, zh, ko. 
>    * diğer tüm 65 karakter.
> * Yazım modu köşeli ayraç karakterleri desteklemez (`[` ve `]`), sorgu ve tutarsız sonuçlara neden olabilir. Bunları Yazım modunu kullanırken, sorgularından kaldırma öneririz.

## <a name="market-setting"></a>Pazar ayarı

A [pazara kod](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#market-codes) ile belirtilmelidir `mkt` sorgu parametresi isteğinizdeki. API, aksi durumda bir varsayılan piyasaya isteğin IP adresine göre kullanır.


## <a name="http-post-and-get-support"></a>HTTP POST ve GET desteği

Hem HTTP POST hem de HTTP GET, API tarafından desteklenir. Bunlardan hangisini kullanacağınız, denetlemeyi planladığınız metnin uzunluğuna göre değişir. Dizelerin tümü 1.500 karakterden kısaysa GET kullanırsınız. Ancak, 10.000 karaktere kadar uzunluktaki dizeleri desteklemek istiyorsanız POST kullanırsınız. Metin dizesi tüm UTF-8 karakterlerini içerebilir.

Sıradaki örnekte, bir metin dizesinin yazımının ve dil bilgisinin denetlenmesine yönelik bir POST isteği gösterilmektedir. [Mode](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#mode) sorgu parametresi bütünlük açısından örneğe dahil edilmiştir. (`mode` varsayılan olarak Denetleme olduğundan, bu parametre dışarıda bırakılabilirdi.) [Text](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#text) sorgu parametresinde, denetlenecek dize bulunur.
  
```  
POST https://api.cognitive.microsoft.com/bing/v7.0/spellcheck?mode=proof&mkt=en-us HTTP/1.1  
Content-Type: application/x-www-form-urlencoded  
Content-Length: 47  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
 
text=when+its+your+turn+turn,+john,+come+runing  
``` 

HTTP GET kullanmanız halinde, URL'nin sorgu dizesine `text` sorgu parametresini dahil etmeniz gerekir
  
Aşağıda, bir önceki isteğin yanıtı gösterilmektedir. Yanıt, [SpellCheck](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#spellcheck) nesnesi içerir. 
  
```json
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
  
[flaggedTokens](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#flaggedtokens) alanında, API'nin [text](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#text) dizesinde bulduğu yazım ve dil bilgisi hataları listelenir. `token` alanı, değiştirilecek sözcüğü içerir. `text` dizesindeki belirteci bulmak için `offset` alanında sıfır temelli uzaklığı kullanırsınız. Ardından, söz konusu konumdaki sözcüğü `suggestion` alanındaki sözcükle değiştirirsiniz. 

`type` alanı RepeatedToken olsa da belirteci `suggestion` ile değiştirirsiniz, ancak muhtemelen sondaki boşluğu kaldırmanız gerekir.

## <a name="throttling-requests"></a>İstekleri azaltma

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Bing yazım denetimi API'si nedir?](../overview.md)
- [Bing Yazım Denetimi API’si v7 Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference)
