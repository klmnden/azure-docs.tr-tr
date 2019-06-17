---
title: Arama sonuçları - Bing Web araması API'si aracılığıyla sayfasına nasıl
titleSuffix: Azure Cognitive Services
description: Bing Web araması API'si Arama sonuçlarından aracılığıyla sayfasında öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: 26CA595B-0866-43E8-93A2-F2B5E09D1F3B
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 05/15/2019
ms.author: aahi
ms.openlocfilehash: a038dc2706c7cb128751630f8997851409886290
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66384802"
---
# <a name="how-to-page-through-results-from-the-bing-web-search-api"></a>Bing Web araması API'si sonuçlarını aracılığıyla sayfası

Web araması API'si çağırdığınızda, Bing, sonuçların listesini döndürür. Liste, sorgu ile ilgili sonuç toplam sayısı bir alt kümesidir. Kullanılabilir sonuçları tahmin edilen toplam sayısını almak için yanıt nesnenin erişim [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference) alan.  

Aşağıdaki örnekte gösterildiği `totalEstimatedMatches` bir Web yanıtı içeren alan.  

```json
{
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3A43CA...",
        "totalEstimatedMatches" : 262000,
        "value" : [...]
    }
}  
```

Kullanılabilir Web sayfası için kullanın [sayısı](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#count) ve [uzaklığı](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#offset) sorgu parametreleri.  

`count` Parametre sonuçları yanıtta döndürülecek sayısını belirtir. Yanıtta isteyebilir sonuçları sayısı 50'dir. Varsayılan değer 10'dur. Teslim gerçek sayı istenenden daha az olabilir.

`offset` Parametre atlanacak sonuç sayısını belirtir. `offset` Sıfır tabanlıdır ve olması gereken küçüktür (`totalEstimatedMatches` - `count`).  

Sayfa başına 15 Web sayfalarında görüntülemek istiyorsanız, ayarlarsınız `count` 15 ve `offset` ilk sayfasını almak için 0. Her bir sonraki sayfa için gerçekleştirilemediği `offset` 15 (örneğin, 15, 30) tarafından.  

Aşağıdaki örnek, 15 Web sayfalarını 45 uzaklıkta başlayan ister.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&count=15&offset=45&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```

Varsayılan `count` değeri, uygulamanız için çalışır, yalnızca belirtmenize gerek `offset` sorgu parametresi.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&offset=45&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```

Web araması API'si, Web sayfaları ve görüntü, video ve haber öğeleri içerebilir sonuçlarını döndürür. Arama sonuçları sayfası açıldığında, sayfalama [WebAnswer](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#webanswer) yanıt ve değil diğer yanıtlarını görüntüler veya haber gibi. Örneğin, ayarlarsanız `count` , 50 50 Web sayfası sonuçları ulaşırsınız, ancak başka yanıtlar da sonuçları yanıt içerebilir. Örneğin, yanıt 15 görüntüler ve 4 haber makalelerini içerebilir. Ayrıca olası sonuçlarını ilk sayfa ancak ikinci sayfada değil, haber içerebilir ya da tam tersi.   

Belirtirseniz `responseFilter` sorgu parametresi ve Web sayfalarında filtre listesinde içermez, kullanmayın `count` ve `offset` parametreleri. 

> [!NOTE]
> `TotalEstimatedAnswers` Alandır ne tahmini toplam arama sonuçları almak için geçerli sorgu sayısı.  Ayarladığınızda `count` ve `offset` parametreleri `TotalEstimatedAnswers` numarası değişebilir. 

## <a name="next-steps"></a>Sonraki adımlar

* [Bing Web araması API'si nedir](overview.md)?
