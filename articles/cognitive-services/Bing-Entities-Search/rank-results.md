---
title: Derecelendirme yanıtları - Bing varlık arama görüntülemek için kullanma
titlesuffix: Azure Cognitive Services
description: Bing varlık arama API'si döndürdüğü yanıtları görüntülemek için sıralama'ı kullanmayı öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: 9e2a4075436145a0cc185b7ab1b406fa8d27b8e3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60309343"
---
# <a name="using-ranking-to-display-entity-search-results"></a>Varlık arama sonuçlarını görüntülemek için sıralama kullanma  

Her varlık arama yanıt içeren bir [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse) nasıl Bing varlık arama API'si tarafından döndürülen arama sonuçlarını görüntülemek belirten yanıt. Derecelendirme yanıt kutbu, ana hat, sonuçlara ve kenar çubuğu içeriğini gruplandırır. Kutup sonucu en önemli veya tanınmış bir sonucudur ve ilk görüntülenmesi gerekir. Kenar biçimi ve kalan sonuçlarında Geleneksel ana hat görüntülenmiyorsa, ana hat içerik daha yüksek görünürlük kenar çubuğu içeriği daha sağlamanız gerekir. 
  
Her grup içindeki [öğeleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankinggroup-items) dizi içeriği görünmelidir sırasını tanımlar. Her bir öğe içinde bir yanıt sonucu belirlemek için iki yol sunar.  
 

|Alan | Açıklama  |
|---------|---------|
|`answerType` ve `resultIndex` | `answerType` Yanıt (varlık veya yer) tanımlar ve `resultIndex` sonucu, yanıt (örneğin, bir varlık) içinde tanımlar. Dizin 0'da başlar.|
|`value`    | `value` Bir yanıt veya yanıt içinde bir sonuç kimliği eşleşen bir kimlik içerir. Yanıt ya da sonuçları kimliği ancak ikisini birden içerir. |
  
Kullanarak `answerType` ve `resultIndex` iki adımlı bir işlemdir. İlk olarak, `answerType` görüntülemek için sonuçları içeren yanıt tanımlamak için. Ardından `resultIndex` görüntülenecek sonuç almak için bu yanıtın sonuçları dizine için. ( `answerType` Değer alanın adıdır [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#searchresponse) nesne.) Yanıtın tüm sonuçları birlikte görüntülemek için beklenen yapıyorsanız, derecelendirme yanıt öğesi içermez `resultIndex` alan.

Kimliğini kullanarak, cevap veya bir hesaplamanın sonuçlarını kimliği derecelendirme Kimliğiyle eşleşecek şekilde gerektirir. Bir yanıt nesnesi içeriyorsa bir `id` alan, yanıtın tüm sonuçları birlikte görüntüler. Örneğin, varsa `Entities` nesne içerir `id` alanı, tüm varlıkları makaleleri birlikte görüntüleyebilirsiniz. Varsa `Entities` nesnesi içermez `id` her varlık içeriyorsa, alan bir `id` alan ve sıralama yanıt yerler sonuçları ile varlıkları karıştırır.  
  
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
> [Tek sayfalı web uygulaması oluşturma](tutorial-bing-entities-search-single-page-app.md)
