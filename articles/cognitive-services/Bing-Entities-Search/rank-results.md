---
title: Derecelendirme yanıtları - Bing varlık arama görüntülemek için kullanma
titlesuffix: Azure Cognitive Services
description: Derecelendirme Bing varlık arama API'si döndürdüğü yanıtları görüntülemek için nasıl kullanılacağını gösterir.
services: cognitive-services
author: v-jerkin
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: v-jerkin
ms.openlocfilehash: 4a336ccaea18ab84464f28aef170ccdc423b216d
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48814602"
---
# <a name="using-ranking-to-display-results"></a>Sıralama sonuçları görüntülemek için kullanma  

Her varlık arama yanıt içeren bir [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse) yanıt, Bing Web araması yanıt gösterilene benzer nasıl arama sonuçlarını görüntülemek belirtir. Derecelendirme yanıt kutbu, ana hat, sonuçlara ve kenar çubuğu içeriğini gruplandırır. Kutup sonucu en önemli veya tanınmış bir sonucudur ve ilk görüntülenmesi gerekir. Kenar biçimi ve kalan sonuçlarında Geleneksel ana hat görüntülenmiyorsa, ana hat içerik daha yüksek görünürlük kenar çubuğu içeriği daha sağlamanız gerekir. 
  
Her grup içindeki [öğeleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankinggroup-items) dizi içeriği görünmelidir sırasını tanımlar. Her bir öğe içinde bir yanıt sonucu belirlemek için iki yol sunar.  
  
-   `answerType` ve `resultIndex` — `answerType` alan (varlık veya yer) yanıt tanımlar ve `resultIndex` yanıt (örneğin, bir varlık) içinde bir sonuç tanımlar. Sıfır tabanlı dizindir.  
  
-   `value` — `value` Alan yanıt veya yanıt içinde bir sonuç kimliği eşleşen bir kimlik içerir. Yanıt ya da sonuçları kimliği ancak ikisini birden içerir.  
  
Kimliğini kullanarak, cevap veya bir hesaplamanın sonuçlarını kimliği derecelendirme Kimliğiyle eşleşecek şekilde gerektirir. Bir yanıt nesnesi içeriyorsa bir `id` alan, yanıtın tüm sonuçları birlikte görüntüler. Örneğin, varsa `Entities` nesne içerir `id` alanı, tüm varlıkları makaleleri birlikte görüntüleyebilirsiniz. Varsa `Entities` nesnesi içermez `id` her varlık içeriyorsa, alan bir `id` alan ve sıralama yanıt yerler sonuçları ile varlıkları karıştırır.  
  
Kullanarak `answerType` ve `resultIndex` iki adımlı bir işlemdir. İlk olarak, kullandığınız `answerType` görüntülemek için sonuçları içeren yanıt tanımlamak için. Kullanmanız `resultIndex` görüntülenecek sonuç almak için bu yanıtın sonuçları dizine için. ( `answerType` Değer alanın adıdır [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#searchresponse) nesne.) Yanıtın tüm sonuçları birlikte görüntülemek için beklenen yapıyorsanız, derecelendirme yanıt öğesi içermez `resultIndex` alan.

## <a name="ranking-response-example"></a>Örnek yanıt derecelendirmesi

Aşağıdaki örnekler [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse).
  
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

Bu derecelendirme yanıtta bağlı olarak, kenar Jimi Hendrix için ilgili iki varlık sonuçları görüntüler.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing varlık arama Öğreticisi](tutorial-bing-entities-search-single-page-app.md)
