---
title: Önceden oluşturulmuş varlıklarla boyut
titleSuffix: Azure
description: Bu makalede içeren önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS) boyut.
services: cognitive-services
ms.custom: seodec18
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: 0ed5d68905353cc0b99a8ce5e1d1b6bfde9d1b87
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53166056"
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