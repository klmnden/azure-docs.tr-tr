---
title: HALUK önceden oluşturulmuş varlıklar numara başvurusu - Azure | Microsoft Docs
titleSuffix: Azure
description: Bu makale numarası önceden oluşturulmuş varlık bilgisi dil anlama (HALUK) içerir.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: aa0b389a0694a3b742259fd42bed08055fbbadbe
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321865"
---
# <a name="number-entity"></a>Sayı varlık
İçinde sayısal değerler ölçme, express ve bilgi parçalarını tanımlamak için kullanılan birçok yolu vardır. Bu makalede, yalnızca bazı olası örnekler yer almaktadır. HALUK kullanıcı utterances Çeşitlemeler yorumlar ve tutarlı sayısal değerleri döndürür. Bu varlık zaten eğitildi olduğundan, uygulama hedefleri sayıya içeren örnek utterances eklemeniz gerekmez. 

## <a name="types-of-number"></a>Sayı türleri
Sayı yönetilen [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml) Github deposunu

## <a name="examples-of-number-resolution"></a>Sayı çözümleme örnekleri

| utterance        | Varlık   | Çözüm |
| ------------- |:----------------:| --------------:|
| ```one thousand times```  | ```"one thousand"``` |   ```"1000"```      | 
| ```1,000 people```        | ```"1,000"```    |   ```"1000"```      |
| ```1/2 cup```         | ```"1 / 2"```    |    ```"0.5"```      |
|  ```one half the amount```     | ```"one half"```     |    ```"0.5"```      |
| ```one hundred fifty orders``` | ```"one hundred fifty"``` | ```"150"``` |
| ```one hundred and fifty books``` | ```"one hundred and fifty"``` | ```"150"```|
| ```a grade of one point five```| ```"one point five"``` |  ```"1.5"``` |
| ```buy two dozen eggs```    | ```"two dozen"``` | ```"24"``` |


HALUK tanınan değerini içeren bir **`builtin.number`** varlık `resolution` döndürdüğü JSON yanıt alanı.

## <a name="resolution-for-prebuilt-number"></a>Önceden oluşturulmuş numaralı çözümleme
Aşağıdaki örnek, "iki düzine" utterance değeri 24, çözümü içeren HALUK, JSON yanıttan gösterir.

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

Hakkında bilgi edinin [para birimi](luis-reference-prebuilt-currency.md), [sıralı](luis-reference-prebuilt-ordinal.md), ve [yüzde](luis-reference-prebuilt-percentage.md). 