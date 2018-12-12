---
title: Önceden oluşturulmuş varlık yüzdesi
titleSuffix: Azure
description: Bu makalede yüzdesi içeren önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: 457de95fb4a16a3a43a0cad902b7fb1469d9db8b
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53087006"
---
# <a name="percentage-entity"></a>Yüzde varlığı
Yüzde numaraları kesir olarak görünebilir `3 1/2`, veya yüzde olarak `2%`. Bu varlık zaten eğitildi çünkü uygulama ıntents percentage içeren örnek Konuşma ekleme gerekmez. Yüzde varlık içerisinde desteklendiği [çok kültür](luis-reference-prebuilt-entities.md). 

## <a name="types-of-percentage"></a>Yüzde türleri
Yüzdesi yönetilen [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L114) Github deposu

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