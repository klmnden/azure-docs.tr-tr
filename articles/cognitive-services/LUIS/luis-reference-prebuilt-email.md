---
title: LUIS önceden oluşturulmuş varlıklarla e-posta başvuru - Azure | Microsoft Docs
titleSuffix: Azure
description: Bu makalede, e-posta içeren önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: diberry
ms.openlocfilehash: 5f2ff9ef8e06c747558d795b52423d494824a746
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39236739"
---
# <a name="email-entity"></a>E-posta varlığı
Bir utterance tüm e-posta adresinden e-posta ayıklama içerir. Bu varlık zaten eğitildi çünkü içeren e-posta uygulaması hedefleri için örnek Konuşma ekleme gerekmez. E-posta varlık içerisinde desteklendiği `en-us` yalnızca kültür. 

## <a name="resolution-for-prebuilt-email"></a>Önceden oluşturulmuş bir e-posta için çözümleme
Aşağıdaki örnek, çözünürlüğünü gösterir **builtin.email** varlık.

```JSON
{
  "query": "please send the information to patti.owens@microsoft.com",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.811592042
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.811592042
    }
  ],
  "entities": [
    {
      "entity": "patti.owens@microsoft.com",
      "type": "builtin.email",
      "startIndex": 31,
      "endIndex": 55
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [numarası](luis-reference-prebuilt-number.md), [sıralı](luis-reference-prebuilt-ordinal.md), ve [yüzdesi](luis-reference-prebuilt-percentage.md). 