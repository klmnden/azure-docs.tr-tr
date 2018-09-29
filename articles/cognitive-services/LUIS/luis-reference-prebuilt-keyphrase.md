---
title: LUIS önceden oluşturulmuş varlıklar anahtar cümlesi başvuru - Azure | Microsoft Docs
titleSuffix: Azure
description: Bu makalede anahtar cümlesi içeren önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/09/2018
ms.author: diberry
ms.openlocfilehash: 4133b7c7c3fabbe92a3208c567d7b4c6c2c27283
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434407"
---
# <a name="keyphrase-entity"></a>keyPhrase varlığı
anahtar cümlesi, anahtar ifadeleri çeşitli bir utterance ayıklar. Uygulamaya anahtar cümlesi içeren örnek Konuşma ekleme gerekmez. anahtar cümlesi varlık içerisinde desteklendiği [çok kültür](luis-language-support.md#languages-supported) parçası olarak [metin analizi](../text-analytics/overview.md) özellikleri. 

## <a name="resolution-for-prebuilt-keyphrase-entity"></a>Önceden oluşturulmuş bir anahtar cümlesi varlık için çözümleme
Aşağıdaki örnek, çözünürlüğünü gösterir **builtin.keyPhrase** varlık.

```JSON
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
