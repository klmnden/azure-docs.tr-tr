---
title: PersonName önceden oluşturulmuş varlık
titleSuffix: Azure Cognitive Services
description: Bu makalede personName içeren önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/24/2018
ms.author: diberry
ms.openlocfilehash: ad19aa8eef72234e034fe9c0e1905e848ace0650
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53091756"
---
# <a name="personname-entity"></a>PersonName varlığı
Önceden oluşturulmuş personName varlık kişi adlarını algılar. Bu varlık zaten eğitildi çünkü uygulama hedefleri için personName içeren örnek Konuşma ekleme gerekmez. İngilizce ve Çince personName varlık desteklenen [kültürler](luis-reference-prebuilt-entities.md).

## <a name="resolution-for-personname-entity"></a>PersonName varlık için çözümleme
Aşağıdaki örnek, çözünürlüğünü gösterir **builtin.personName** varlık.

```json
{
  "query": "Is Jill Jones in Cairo?",
  "topScoringIntent": {
    "intent": "WhereIsEmployee",
    "score": 0.762141049
  }
  "entities": [
    {
      "entity": "Jill Jones",
      "type": "builtin.personName",
      "startIndex": 3,
      "endIndex": 12
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [e-posta](luis-reference-prebuilt-email.md), [numarası](luis-reference-prebuilt-number.md), ve [sıralı](luis-reference-prebuilt-ordinal.md) varlıklar. 