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
ms.openlocfilehash: 1cb12bdc362955da907fb5a5ed64c2a1a43fdc32
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53140872"
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