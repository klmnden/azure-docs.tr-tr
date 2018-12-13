---
title: Anahtar cümlesi önceden oluşturulmuş varlık
titleSuffix: Azure
description: Bu makalede anahtar cümlesi içeren önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: 684bba0c2f0c6fbaf05ce25ce543da413f1b71e2
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53141229"
---
# <a name="keyphrase-prebuilt-entity-for-a-luis-app"></a>anahtar cümlesi LUIS uygulaması için önceden oluşturulmuş varlık
anahtar cümlesi, anahtar ifadeleri çeşitli bir utterance ayıklar. Uygulamaya anahtar cümlesi içeren örnek Konuşma ekleme gerekmez. anahtar cümlesi varlık içerisinde desteklendiği [çok kültür](luis-language-support.md#languages-supported) parçası olarak [metin analizi](../text-analytics/overview.md) özellikleri. 

## <a name="resolution-for-prebuilt-keyphrase-entity"></a>Önceden oluşturulmuş bir anahtar cümlesi varlık için çözümleme
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
