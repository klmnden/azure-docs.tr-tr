---
title: Bing Yazım Denetimi API'si nedir?
titlesuffix: Azure Cognitive Services
description: Bing Yazım Denetimi API'si, bağlamsal yazım denetimi için makine öğrenimi ve istatistiksel makine çevirisinden yararlanır.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: overview
ms.date: 05/03/2018
ms.author: nolachar
ms.openlocfilehash: c15af0dcebdfcbe984d47b5c06f213e516ae3914
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55149780"
---
# <a name="what-is-bing-spell-check-api"></a>Bing Yazım Denetimi API'si nedir?

Bing Yazım Denetimi API'si, bağlamsal dil bilgisi ve yazım denetimi gerçekleştirmenize olanak sağlar.

Sıradan yazım denetleyicileri ile Bing'in üçüncü nesil yazım denetleyicisi arasındaki fark nedir? Mevcut yazım denetleyicilerinin çalışma biçimi; yazımı ve dil bilgisini, düzenli olarak güncelleştirilen ve genişletilen sözlük temelli kural kümelerine göre doğrulamaya dayanır. Başka bir deyişle, yazım denetleyicisinin verimliliği, yalnızca temel alınan sözlüğe ve sözlüğü destekleyen editör kadrosuna bağlıdır. Bu yazım denetleyicisi türüne Microsoft Word ve diğer sözcük işlemcilerden aşinasınız.

Buna karşılık, Bing, sürekli olarak gelişen ve yüksek düzeyde bağlamsal olan bir algoritmayı dinamik bir şekilde eğitmek için makine öğrenimi ve istatistiksel makine çevirisinden yararlanan web tabanlı bir yazım denetleyicisi geliştirmiştir. Yazım denetleyicisi, web aramalarından ve belgelerden oluşan dev bir topluluğu temel alır.

Bu yazım denetleyicisi tüm sözcük işleme senaryolarının üstesinden gelebilir:

- Argo ve konuşma diline özgü ifadeleri belirler
- Bağlam içinde sık yapılan ad hatalarını tanır
- Sözcük bölme sorunlarını tek bayrakla düzeltir
- Bağlamdaki eş seslileri ve saptanması zor olan diğer hataları düzeltebilir
- Ortaya çıkan yeni markaları, dijital eğlence içeriklerini ve popüler ifadeleri anında destekler
- "See" (görmek) ve "sea" (deniz) gibi söyleniş biçimi aynı ancak anlamı ve yazımı farklı sözcükler.

## <a name="spell-check-modes"></a>Yazım denetimi modları

API, `Proof` ve `Spell` olmak üzere iki denetleme modunu destekler.  Örnekleri [burada](https://azure.microsoft.com/services/cognitive-services/spell-check/) deneyebilirsiniz.
### <a name="proof---for-documents-scenario"></a>Denetleme (belge senaryosu için)
`Proof`, varsayılan moddur. `Proof` yazım denetimi modu, belge oluşturmaya yardımcı olmak için büyük/küçük harf kullanımını düzeltme ve temel noktalama işaretleri ekleme gibi çeşitli özellikler sunarak en kapsamlı denetimleri sağlar. Ancak bu mod yalnızca en-US (İngilizce-Amerika Birleşik Devletleri), es-ES (İspanyolca) ve pt-BR (Portekizce) pazarlarında kullanılabilir. (Not: İspanyolca ve Portekizce için yalnızca beta sürümünde sunulur). Diğer tüm pazarlar için mode sorgu parametresini Spell olarak ayarlayın. 
<br /><br/>**NOT:**   Sorgu metni uzunluğu 4096 aşarsa, 4096 karakterle kesilecek sonra işlenir. 
### <a name="spell----for-web-searchesqueries-scenario"></a>Yazım (Web araması/sorgusu senaryosu için)
`Spell`, daha iyi arama sonuçları döndürmek için daha agresif şekilde çalışır. `Spell` modu, çoğu yazım hatasını bulur ancak `Proof` modunda yakalanan, gibi bazı dil bilgisi hatalarını (örneğin, büyük/küçük harf kullanımı ve yinelenen sözcükler ile ilgili hatalar) saptayamaz.

> [!NOTE]
> * Maksimum desteklenen sorgu uzunluğu aşağıda verilmiştir. Sorgu en fazla uzunluğu aşıyor, sorgu ve sonuçları değiştirilmeyecek.
>    * Aşağıdaki dil kodları 130 karakter: tr, de, es, fr, pl, pt, sv, ru, nl, nb, tr-tr, it, zh, ko. 
>    * diğer tüm 65 karakter.
> * Yazım modu köşeli ayraç karakterleri desteklemez (`[` ve `]`), sorgu ve tutarsız sonuçlara neden olabilir. Bunları Yazım modunu kullanırken, sorgularından kaldırma öneririz.

## <a name="market-setting"></a>Pazar ayarı
Pazarın, istek URL'sinde belirtilmesi gerekir. Aksi halde; yazım denetleyici, IP adresine göre varsayılan pazarı dikkate alır.


## <a name="post-vs-get"></a>POST ve GET

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
  
```  
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

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Sonraki adımlar

İlk isteğinizi hızlı bir şekilde başlatmak için bkz. [İlk İsteğinizi Yapma](quickstarts/csharp.md).

[Bing Yazım Denetimi API'si Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference)'nu inceleyin. Başvuruda arama sonuçlarını istemek için kullanabileceğiniz uç noktaların, üst bilgilerin ve sorgu parametrelerinin listesinin yanı sıra yanıt nesnelerinin tanımları yer alır. 

Sonuçların kullanımı ile ilgili kurallara uygun hareket etmek için [Bing Kullanım ve Görüntüleme Gereksinimleri](./useanddisplayrequirements.md)'ni okuduğunuzdan emin olun.
