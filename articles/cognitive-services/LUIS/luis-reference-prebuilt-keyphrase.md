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
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: a543e60c6e77ed9fdb825ad6cb2a936119677671
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52334593"
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
