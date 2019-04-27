---
title: Önceden oluşturulmuş varlıklarla boyut
titleSuffix: Azure
description: Bu makalede içeren önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS) boyut.
services: cognitive-services
ms.custom: seodec18
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 02/28/2019
ms.author: diberry
ms.openlocfilehash: 9099cdbb91e41998065d953b9d48b3b501df7c10
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60712772"
---
# <a name="dimension-prebuilt-entity-for-a-luis-app"></a>Bir LUIS uygulaması için önceden oluşturulmuş varlık boyut
Önceden oluşturulmuş boyut varlık LUIS uygulama kültürü ne olursa olsun, Boyutlar çeşitli türlerde algılar. Bu varlık zaten eğitildi çünkü uygulama ıntents boyutlar içeren örnek Konuşma ekleme gerekmez. Boyut varlık içerisinde desteklendiği [çok kültür](luis-reference-prebuilt-entities.md). 

## <a name="types-of-dimension"></a>Boyut türü

Boyut yönetilen [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml) GitHub deposu


## <a name="resolution-for-dimension-entity"></a>Boyut varlık için çözümleme
Aşağıdaki örnek, çözünürlüğünü gösterir **builtin.dimension** varlık.

```json
{
  "query": "it takes more than 10 1/2 miles of cable and wire to hook it all up , and 23 computers.",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.762141049
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.762141049
    }
  ],
  "entities": [
    {
      "entity": "10 1/2 miles",
      "type": "builtin.dimension",
      "startIndex": 19,
      "endIndex": 30,
      "resolution": {
        "unit": "Mile",
        "value": "10.5"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [e-posta](luis-reference-prebuilt-email.md), [numarası](luis-reference-prebuilt-number.md), ve [sıralı](luis-reference-prebuilt-ordinal.md) varlıklar. 
