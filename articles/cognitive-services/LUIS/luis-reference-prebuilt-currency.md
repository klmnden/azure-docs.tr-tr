---
title: Önceden oluşturulmuş CurrentY varlık
titleSuffix: Azure
description: Bu makalede, para birimi içeren önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: f3d4c8aeabb33e9f0f8b53f63c60c8d6ca702a83
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55876683"
---
# <a name="currency-prebuilt-entity-for-a-luis-app"></a>Para birimi önceden oluşturulmuş bir varlık için bir LUIS uygulaması
Önceden oluşturulmuş bir para birimi varlık para birimi birçok denominations ve LUIS uygulama kültürü ne olursa olsun ülkelerde algılar. Bu varlık zaten eğitildi çünkü uygulama hedefleri için para birimi içeren örnek Konuşma ekleme gerekmez. Para birimi varlık içerisinde desteklendiği [çok kültür](luis-reference-prebuilt-entities.md). 

## <a name="types-of-currency"></a>Para birimi türü
Para birimi yönetilen engelle [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L26) GitHub deposu

## <a name="resolution-for-currency-entity"></a>Para birimi varlık için çözümleme
Aşağıdaki örnek, çözünürlüğünü gösterir **builtin.currency** varlık.

```json
{
  "query": "search for items under $10.99",
  "topScoringIntent": {
    "intent": "SearchForItems",
    "score": 0.926173568
  },
  "intents": [
    {
      "intent": "SearchForItems",
      "score": 0.926173568
    },
    {
      "intent": "None",
      "score": 0.07376878
    }
  ],
  "entities": [
    {
      "entity": "$10.99",
      "type": "builtin.currency",
      "startIndex": 23,
      "endIndex": 28,
      "resolution": {
        "unit": "Dollar",
        "value": "10.99"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [datetimeV2](luis-reference-prebuilt-datetimev2.md), [boyut](luis-reference-prebuilt-dimension.md), ve [e-posta](luis-reference-prebuilt-email.md) varlıklar. 
