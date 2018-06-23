---
title: HALUK önceden oluşturulmuş varlıklar para birimi başvuru - Azure | Microsoft Docs
titleSuffix: Azure
description: Bu makalede para birimi içeren önceden oluşturulmuş varlık bilgilerini dil anlama (HALUK).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 3e20642f6734b0247d23db1a63317eb8b4a96b5e
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321851"
---
# <a name="currency-entity"></a>Para birimi varlık
Önceden oluşturulmuş para birimi varlık para birimi birçok denominations ve HALUK uygulama kültür bakılmaksızın ülkelerde algılar. Bu varlık zaten eğitildi çünkü uygulama hedefleri para birimi içeren örnek utterances eklemek gerekmez. Para birimi varlık desteklenir [çok kültür](luis-reference-prebuilt-entities.md). 

## <a name="types-of-currency"></a>Para birimi türü
Para birimi gelen yönetilir [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L26) Github deposunu

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