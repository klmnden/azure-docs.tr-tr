---
title: Anahtar cümlesi önceden oluşturulmuş varlık
titleSuffix: Azure
description: Bu makalede anahtar cümlesi içeren önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 988609f411ad405b0f1dc244b23fb6db446136a2
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "65071993"
---
# <a name="keyphrase-prebuilt-entity-for-a-luis-app"></a>anahtar cümlesi LUIS uygulaması için önceden oluşturulmuş varlık
anahtar cümlesi, anahtar ifadeleri çeşitli bir utterance ayıklar. Uygulamaya anahtar cümlesi içeren örnek Konuşma ekleme gerekmez. anahtar cümlesi varlık içerisinde desteklendiği [çok kültür](luis-language-support.md#languages-supported) parçası olarak [metin analizi](../text-analytics/overview.md) özellikleri. 

## <a name="resolution-for-prebuilt-keyphrase-entity"></a>Önceden oluşturulmuş bir anahtar cümlesi varlık için çözümleme

### <a name="api-version-2x"></a>API sürüm 2.x

Aşağıdaki örnek, çözünürlüğünü gösterir **builtin.keyPhrase** varlık.

```json
{
  "query": "where is the educational requirements form for the development and engineering group",
  "topScoringIntent": {
    "intent": "GetJobInformation",
    "score": 0.182757929
  },
  "entities": [
    {
      "entity": "development",
      "type": "builtin.keyPhrase",
      "startIndex": 51,
      "endIndex": 61
    },
    {
      "entity": "educational requirements",
      "type": "builtin.keyPhrase",
      "startIndex": 13,
      "endIndex": 36
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [yüzdesi](luis-reference-prebuilt-percentage.md), [numarası](luis-reference-prebuilt-number.md), ve [yaş](luis-reference-prebuilt-age.md) varlıklar.
