---
title: LUIS önceden oluşturulmuş varlıklarla e-posta başvuru - Azure | Microsoft Docs
titleSuffix: Azure
description: Bu makalede, e-posta içeren önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 51e6a5da0d757023bd7cd1f61c77387a77b77de4
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55869068"
---
# <a name="email-prebuilt-entity-for-a-luis-app"></a>Bir LUIS uygulaması için önceden oluşturulmuş varlık e-posta
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
