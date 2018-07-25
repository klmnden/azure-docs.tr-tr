---
title: LUIS önceden oluşturulmuş varlıklarla yaş başvuru - Azure | Microsoft Docs
titleSuffix: Azure
description: Bu makalede yaş içeren önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: diberry
ms.openlocfilehash: f93acb8bece8c66c3ed7197f1c4530011aec3f29
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39237409"
---
# <a name="age-entity"></a>Yaş varlığı
Önceden oluşturulmuş yaş varlık, hem sayısal ve gün, hafta, ay ve yıl açısından yaş değeri yakalar. Bu varlık zaten eğitildi çünkü uygulama hedefleri için yaş içeren örnek Konuşma ekleme gerekmez. Yaş varlık içerisinde desteklendiği [çok kültür](luis-reference-prebuilt-entities.md). 

## <a name="types-of-age"></a>Yaş türleri
Yaş yönetilen [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L3) Github deposu

## <a name="resolution-for-prebuilt-age-entity"></a>Önceden oluşturulmuş yaş varlık için çözümleme
Aşağıdaki örnek, çözünürlüğünü gösterir **builtin.age** varlık.

```JSON
{
  "query": "A 90 day old utilities bill is quite late.",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.8236133
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.8236133
    }
  ],
  "entities": [
    {
      "entity": "90 day old",
      "type": "builtin.age",
      "startIndex": 2,
      "endIndex": 11,
      "resolution": {
        "unit": "Day",
        "value": "90"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [para birimi](luis-reference-prebuilt-currency.md), [datetimeV2](luis-reference-prebuilt-datetimev2.md), ve [boyut](luis-reference-prebuilt-dimension.md) varlıklar. 