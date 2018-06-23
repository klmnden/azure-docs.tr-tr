---
title: HALUK önceden oluşturulmuş varlıklar sıcaklık başvuru - Azure | Microsoft Docs
titleSuffix: Azure
description: Bu makalede sıcaklık içeren önceden oluşturulmuş varlık bilgilerini dil anlama (HALUK).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 3cc16e7ec87775407c4261655d8f680cc0903e81
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321844"
---
# <a name="temperature-entity"></a>Sıcaklık varlığı
Sıcaklık çeşitli sıcaklık türlerini ayıklar. Bu varlık zaten eğitildi çünkü uygulamaya sıcaklık içeren örnek utterances eklemek gerekmez. Sıcaklık varlığı desteklenir [çok kültür](luis-reference-prebuilt-entities.md). 

## <a name="types-of-temperature"></a>Sıcaklık türleri
Sıcaklık yönetilen [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L819) Github deposunu

## <a name="resolution-for-prebuilt-temperature-entity"></a>Önceden oluşturulmuş sıcaklık varlığı için çözümleme
Aşağıdaki örnek, çözünürlüğünü gösterir **builtin.temperature** varlık.

```JSON
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

Hakkında bilgi edinin [yüzde](luis-reference-prebuilt-percentage.md), [numarası](luis-reference-prebuilt-number.md), ve [yaş](luis-reference-prebuilt-age.md) varlıklar. 