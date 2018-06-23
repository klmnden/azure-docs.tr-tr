---
title: Derecelendirme yanıtları görüntülemek için kullanma | Microsoft Docs
description: Derecelendirme Bing varlık arama API döndürür yanıtları görüntülemek için nasıl kullanılacağını gösterir.
services: cognitive-services
author: v-jerkin
manager: ehansen
ms.assetid: BBF87972-B6C3-4910-BB52-DE90893F6C71
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 12/12/2017
ms.author: v-jerkin
ms.openlocfilehash: ff5b004aaa863dbdfc460a774a5dfd658ce52537
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351430"
---
# <a name="using-ranking-to-display-results"></a>Sıralama sonuçları görüntülemek için kullanma  

Her varlığın arama yanıtı içeren bir [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-entities-v7-reference#rankingresponse) yanıt, benzer bir Bing Web araması yanıt, arama sonuçlarının nasıl görüntülemelidir belirtir. Derecelendirme yanıt kutbu'na, mainline, içine sonuçları ve kenar çubuğu içeriğini gruplandırır. Kutbu'na sonucu en önemli veya belirgin sonuç ve ilk görüntülenmesi gerekir. Kalan sonuçlarında geleneksel mainline ve kenar biçimi görüntülenmiyorsa kenar çubuğu içeriğini daha mainline içerik daha yüksek görünürlük sağlamanız gerekir. 
  
Her grup içindeki [öğeleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankinggroup-items) dizi içeriği görünmelidir sırasını tanımlar. Her öğe içinde bir yanıt sonucu tanımlamak için iki yöntem sunar.  
  
-   `answerType` ve `resultIndex` — `answerType` alan yanıt (varlık veya Yerleştir) tanımlar ve `resultIndex` yanıt (örneğin, bir varlık) içinde bir sonuç tanımlar. Sıfır tabanlı dizin olabilir.  
  
-   `value` — `value` Alan bir yanıt veya yanıt içinde bir sonuç kodu eşleşen bir Kimliğini içerir. Yanıt veya sonuçları kimliği ancak ikisini içerir.  
  
Kimliğini kullanarak bir yanıt veya sonuçlarını birini Kimliğini derecelendirme Kimliğiyle eşleşmesi gerekir. Bir yanıt nesne içeriyorsa, bir `id` alan, yanıtın tüm sonuçları birlikte görüntüler. Örneğin, varsa `Entities` nesnesi içerir `id` alan, tüm varlıkları makaleleri birlikte görüntüler. Varsa `Entities` nesne içermez `id` her varlık içeriyorsa, alan bir `id` alan ve derecelendirme yanıt yerler sonuçlarıyla varlıkları karıştırır.  
  
Kullanarak `answerType` ve `resultIndex` iki adımlı bir işlemdir. İlk olarak, kullandığınız `answerType` görüntülenecek sonuçlarını içeren yanıt tanımlamak için. Kullandığınız sonra `resultIndex` görüntülenecek sonuç almak için bu yanıtın sonuçları dizine için. ( `answerType` Değerdir alanın adını [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#searchresponse) nesnesi.) Birlikte yanıtın tüm sonuçları görüntülemek için gereken, derecelendirme yanıt öğesi içermeyen `resultIndex` alan.

## <a name="ranking-response-example"></a>Yanıt örnek sıralaması

Aşağıdaki örnek gösterir [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse).
  
```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "Jimi Hendrix"
  },
  "entities": { ... },
  "rankingResponse": {
    "sidebar": {
      "items": [
        {
          "answerType": "Entities",
          "resultIndex": 0,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Entities.0"
          }
        },
        {
          "answerType": "Entities",
          "resultIndex": 1,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Entities.1"
          }
        }
      ]
    }
  }
}
```

Bu derecelendirme yanıtta bağlı olarak, kenar Jimi Hendrix ilgili iki varlık sonuçları görüntüler.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing varlık arama Öğreticisi](tutorial-bing-entities-search-single-page-app.md)
