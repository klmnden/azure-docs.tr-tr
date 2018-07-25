---
title: LUIS önceden oluşturulmuş varlıklar para birimi başvuru - Azure | Microsoft Docs
titleSuffix: Azure
description: Bu makalede, para birimi içeren önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: diberry
ms.openlocfilehash: b383e21f870d15818c540b79a9a56c1dd65fa342
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39236831"
---
# <a name="currency-entity"></a>Para birimi varlığı
Önceden oluşturulmuş bir para birimi varlık para birimi birçok denominations ve LUIS uygulama kültürü ne olursa olsun ülkelerde algılar. Bu varlık zaten eğitildi çünkü uygulama hedefleri için para birimi içeren örnek Konuşma ekleme gerekmez. Para birimi varlık içerisinde desteklendiği [çok kültür](luis-reference-prebuilt-entities.md). 

## <a name="types-of-currency"></a>Para birimi türü
Para birimi yönetilen engelle [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L26) Github deposu

## <a name="resolution-for-currency-entity"></a>Para birimi varlık için çözümleme
Aşağıdaki örnek, çözünürlüğünü gösterir **builtin.currency** varlık.

```JSON
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