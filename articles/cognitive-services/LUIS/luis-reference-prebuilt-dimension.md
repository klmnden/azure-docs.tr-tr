---
title: HALUK önceden oluşturulmuş varlıklar boyut başvuru - Azure | Microsoft Docs
titleSuffix: Azure
description: Bu makalede içeren boyut önceden oluşturulmuş varlık bilgilerini dil anlama (HALUK).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 3b2758f1d68ae3659f2e43cad555d327b21f8732
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321984"
---
# <a name="dimension-entity"></a>Boyut varlık
Önceden oluşturulmuş boyut varlık HALUK uygulama kültür bakılmaksızın boyutlar çeşitli türlerde algılar. Bu varlık zaten eğitildi çünkü uygulama hedefleri boyutlar içeren örnek utterances eklemek gerekmez. Boyut varlık desteklenir [çok kültür](luis-reference-prebuilt-entities.md). 

## <a name="types-of-dimension"></a>Boyut türleri

Boyut yönetilen [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml) Github deposunu


## <a name="resolution-for-dimension-entity"></a>Boyut varlık için çözümleme
Aşağıdaki örnek, çözünürlüğünü gösterir **builtin.dimension** varlık.

```JSON
{
  "query": "it takes more than 10 1/2 miles of cable and wire to hook it all up , and 23 computers.",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.762141049
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.762141049
    }
  ],
  "entities": [
    {
      "entity": "10 1/2 miles",
      "type": "builtin.dimension",
      "startIndex": 19,
      "endIndex": 30,
      "resolution": {
        "unit": "Mile",
        "value": "10.5"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [e-posta](luis-reference-prebuilt-email.md), [numarası](luis-reference-prebuilt-number.md), ve [sıralı](luis-reference-prebuilt-ordinal.md) varlıklar. 