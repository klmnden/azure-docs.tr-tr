---
title: Önceden oluşturulmuş sıcaklık varlık
titleSuffix: Azure
description: Bu makalede sıcaklık içeren önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/04/2019
ms.author: diberry
ms.openlocfilehash: 6a6600a0294eaef9f3b8ee6e7f6f021eb5c1e60b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60709704"
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
