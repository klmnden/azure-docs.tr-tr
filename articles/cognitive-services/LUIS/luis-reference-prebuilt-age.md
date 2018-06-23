---
title: HALUK önceden oluşturulmuş varlıklar başvuru - Azure geçerlilik süresi | Microsoft Docs
titleSuffix: Azure
description: Bu makalede yaş içeren önceden oluşturulmuş varlık bilgilerini dil anlama (HALUK).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 59732469cf0d1e55643f3977958ec34a887130d3
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321991"
---
# <a name="age-entity"></a>Geçerlilik süresi varlık
Önceden oluşturulmuş yaş varlık geçerlilik değerinin hem sayısal ve gün, hafta, ay ve yıl bakımından yakalar. Bu varlık zaten eğitildi çünkü uygulama hedefleri yaşın içeren örnek utterances eklemek gerekmez. Geçerlilik süresi varlık desteklenir [çok kültür](luis-reference-prebuilt-entities.md). 

## <a name="types-of-age"></a>Geçerlilik süresi türleri
Geçerlilik süresi yönetilen [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L3) Github deposunu

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