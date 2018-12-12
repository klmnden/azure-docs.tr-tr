---
title: Önceden oluşturulmuş sıralı varlık
titleSuffix: Azure
description: Bu makale, Language Understanding (LUIS) önceden oluşturulmuş sıralı varlık bilgileri içerir.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: 060314bd56d477bad3dcb3333ba02c5f4786b476
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53082773"
---
# <a name="ordinal-entity"></a>Sıralı varlık
Sıra numarası olduğu bir nesne kümesi içinde sayısal bir gösterimi: `first`, `second`, `third`. Bu varlık zaten eğitildi çünkü uygulama hedefleri için sıralı içeren örnek Konuşma ekleme gerekmez. Sıralı varlık içerisinde desteklendiği [çok kültür](luis-reference-prebuilt-entities.md). 

## <a name="types-of-ordinal"></a>Sıra türü
Sıra yönetilen [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L45) Github deposu

## <a name="resolution-for-prebuilt-ordinal-entity"></a>Önceden oluşturulmuş sıralı varlık için çözümleme
Aşağıdaki örnek, çözünürlüğünü gösterir **builtin.ordinal** varlık.

```json
{
  "query": "Order the second option",
  "topScoringIntent": {
    "intent": "OrderFood",
    "score": 0.9993253
  },
  "intents": [
    {
      "intent": "OrderFood",
      "score": 0.9993253
    },
    {
      "intent": "None",
      "score": 0.05046708
    }
  ],
  "entities": [
    {
      "entity": "second",
      "type": "builtin.ordinal",
      "startIndex": 10,
      "endIndex": 15,
      "resolution": {
        "value": "2"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [yüzdesi](luis-reference-prebuilt-percentage.md), [phonenumber](luis-reference-prebuilt-phonenumber.md), ve [sıcaklık](luis-reference-prebuilt-temperature.md) varlıklar. 