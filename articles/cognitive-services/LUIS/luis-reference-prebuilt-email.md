---
title: LUIS önceden oluşturulmuş varlıklarla e-posta başvuru - Azure | Microsoft Docs
titleSuffix: Azure
description: Bu makalede, e-posta içeren önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 07fe8a0d48d59da89ad76f1648b75332d23005a2
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53074093"
---
# <a name="email-entity"></a>E-posta varlığı
Bir utterance tüm e-posta adresinden e-posta ayıklama içerir. Bu varlık zaten eğitildi çünkü içeren e-posta uygulaması hedefleri için örnek Konuşma ekleme gerekmez. E-posta varlık içerisinde desteklendiği `en-us` yalnızca kültür. 

## <a name="resolution-for-prebuilt-email"></a>Önceden oluşturulmuş bir e-posta için çözümleme
Aşağıdaki örnek, çözünürlüğünü gösterir **builtin.email** varlık.

```json
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