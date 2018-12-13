---
title: Önceden oluşturulmuş sıcaklık varlık
titleSuffix: Azure
description: Bu makalede sıcaklık içeren önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 348030d888383c497d80259b279056d8ff892bfe
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53165531"
---
# <a name="temperature-prebuilt-entity-for-a-luis-app"></a>Sıcaklık LUIS uygulaması için önceden oluşturulmuş varlık
Sıcaklık sıcaklık türleri çeşitli ayıklar. Bu varlık zaten eğitildi çünkü uygulama sıcaklık içeren örnek Konuşma ekleme gerekmez. Sıcaklık varlık içerisinde desteklendiği [çok kültür](luis-reference-prebuilt-entities.md). 

## <a name="types-of-temperature"></a>Sıcaklık türleri
Sıcaklık yönetilen engelle [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L819) GitHub deposu

## <a name="resolution-for-prebuilt-temperature-entity"></a>Önceden oluşturulmuş sıcaklık varlık için çözümleme
Aşağıdaki örnek, çözünürlüğünü gösterir **builtin.temperature** varlık.

```json
{
  "query": "set the temperature to 30 degrees",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.85310787
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.85310787
    }
  ],
  "entities": [
    {
      "entity": "30 degrees",
      "type": "builtin.temperature",
      "startIndex": 23,
      "endIndex": 32,
      "resolution": {
        "unit": "Degree",
        "value": "30"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [yüzdesi](luis-reference-prebuilt-percentage.md), [numarası](luis-reference-prebuilt-number.md), ve [yaş](luis-reference-prebuilt-age.md) varlıklar. 