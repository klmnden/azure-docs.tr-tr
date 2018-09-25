---
title: LUIS önceden oluşturulmuş varlıklarla sayı başvuru - Azure | Microsoft Docs
titleSuffix: Azure
description: Bu makale numarası önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS) içerir.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: diberry
ms.openlocfilehash: 4d14a3490d87693a4785111035b62e7ae59ee669
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47041855"
---
# <a name="number-entity"></a>Sayı varlığı
Hangi sayısal değerleri ölçme, express ve bilgi parçalarını tanımlamak için kullanılan birçok yolu vardır. Bu makalede yalnızca bazı olası örnekler yer almaktadır. LUIS, kullanıcı konuşma farklılığı yorumlar ve tutarlı bir sayısal değerleri döndürür. Bu varlık zaten eğitildi çünkü uygulama hedefleri için numarası içeren örnek Konuşma ekleme gerekmez. 

## <a name="types-of-number"></a>Sayı türleri
Sayı yönetilen [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml) Github deposu

## <a name="examples-of-number-resolution"></a>Sayı çözümleme örnekleri

| İfade        | Varlık   | Çözüm |
| ------------- |:----------------:| --------------:|
| ```one thousand times```  | ```"one thousand"``` |   ```"1000"```      | 
| ```1,000 people```        | ```"1,000"```    |   ```"1000"```      |
| ```1/2 cup```         | ```"1 / 2"```    |    ```"0.5"```      |
|  ```one half the amount```     | ```"one half"```     |    ```"0.5"```      |
| ```one hundred fifty orders``` | ```"one hundred fifty"``` | ```"150"``` |
| ```one hundred and fifty books``` | ```"one hundred and fifty"``` | ```"150"```|
| ```a grade of one point five```| ```"one point five"``` |  ```"1.5"``` |
| ```buy two dozen eggs```    | ```"two dozen"``` | ```"24"``` |


LUIS, tanınan bir değer içeren bir **`builtin.number`** varlıkta `resolution` alanını JSON yanıtı döndürür.

## <a name="resolution-for-prebuilt-number"></a>Önceden oluşturulmuş numaralı çözümleme
Aşağıdaki örnek, çözüm için "iki düzine" utterance 24, değeri içeren bir JSON yanıtı, luıs'den gösterir.

```JSON
{
  "query": "order two dozen eggs",
  "topScoringIntent": {
    "intent": "OrderFood",
    "score": 0.105443209
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.105443209
    },
    {
      "intent": "OrderFood",
      "score": 0.9468431361
    },
    {
      "intent": "Help",
      "score": 0.000399122015
    },
  ],
  "entities": [
    {
      "entity": "two dozen",
      "type": "builtin.number",
      "startIndex": 6,
      "endIndex": 14,
      "resolution": {
        "value": "24"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [para birimi](luis-reference-prebuilt-currency.md), [sıralı](luis-reference-prebuilt-ordinal.md), ve [yüzdesi](luis-reference-prebuilt-percentage.md). 