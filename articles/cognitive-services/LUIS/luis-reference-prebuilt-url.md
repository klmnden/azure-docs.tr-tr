---
title: HALUK önceden oluşturulmuş varlıklar url başvuru - Azure | Microsoft Docs
titleSuffix: Azure
description: Bu makalede URL'si içeren önceden oluşturulmuş varlık bilgilerini dil anlama (HALUK).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 4eacf564a295a568a3e2c8d2f44ad0af3fbbe258
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321963"
---
# <a name="url-entity"></a>URL varlık
URL varlık URL'leri etki alanı adlarını veya IP adresleri ile ayıklar. Bu varlık zaten eğitildi olduğundan, uygulamaya URL'leri içeren örnek utterances eklemeniz gerekmez. URL varlık desteklenir `en-us` yalnızca kültür. 

## <a name="types-of-urls"></a>Tür URL'leri
URL, gelen yönetilir [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-URL.yaml) Github deposunu

## <a name="resolution-for-prebuilt-url-entity"></a>Önceden oluşturulmuş URL varlık için çözümleme
Aşağıdaki örnek, çözünürlüğünü gösterir **builtin.url** varlık.

```JSON
{
  "query": "http://www.luis.ai is a great cognitive services example of artificial intelligence",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.781975448
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.781975448
    }
  ],
  "entities": [
    {
      "entity": "http://www.luis.ai",
      "type": "builtin.url",
      "startIndex": 0,
      "endIndex": 17
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [sıralı](luis-reference-prebuilt-ordinal.md), [numarası](luis-reference-prebuilt-number.md), ve [sıcaklık](luis-reference-prebuilt-temperature.md) varlıklar.