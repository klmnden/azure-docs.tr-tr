---
title: Önceden oluşturulmuş varlık yüzdesi
titleSuffix: Azure
description: Bu makalede yüzdesi içeren önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: 0e06857fe45b81bcb6696c29b6a3e07a6587a95b
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55874677"
---
# <a name="percentage-prebuilt-entity-for-a-luis-app"></a>Yüzde bir LUIS uygulaması için önceden oluşturulmuş varlık
Yüzde numaraları kesir olarak görünebilir `3 1/2`, veya yüzde olarak `2%`. Bu varlık zaten eğitildi çünkü uygulama ıntents percentage içeren örnek Konuşma ekleme gerekmez. Yüzde varlık içerisinde desteklendiği [çok kültür](luis-reference-prebuilt-entities.md). 

## <a name="types-of-percentage"></a>Yüzde türleri
Yüzdesi yönetilen [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L114) GitHub deposu

## <a name="resolution-for-prebuilt-percentage-entity"></a>Önceden oluşturulmuş yüzdesi varlık için çözümleme
Aşağıdaki örnek, çözünürlüğünü gösterir **builtin.percentage** varlık.

```json
{
  "query": "set a trigger when my stock goes up 2%",
  "topScoringIntent": {
    "intent": "SetTrigger",
    "score": 0.971157849
  },
  "intents": [
    {
      "intent": "SetTrigger",
      "score": 0.971157849
    }
  ],
  "entities": [
    {
      "entity": "2%",
      "type": "builtin.percentage",
      "startIndex": 36,
      "endIndex": 37,
      "resolution": {
        "value": "2%"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [sıralı](luis-reference-prebuilt-ordinal.md), [numarası](luis-reference-prebuilt-number.md), ve [sıcaklık](luis-reference-prebuilt-temperature.md) varlıklar. 
