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
ms.date: 02/28/2018
ms.author: diberry
ms.openlocfilehash: e214870e74b2680555cf1d116862c568cc5068b1
ms.sourcegitcommit: c712cb5c80bed4b5801be214788770b66bf7a009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57213133"
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
