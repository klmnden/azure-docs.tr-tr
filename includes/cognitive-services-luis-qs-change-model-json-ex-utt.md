---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 08/16/2018
ms.author: diberry
ms.openlocfilehash: 2317e0b8bfe01f94989412db7c0c4560b2ca728f
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66124139"
---
Örnek konuşma dosyası **utterances.json** belirli bir biçimdedir. 

`text` alanı, örnek konuşmanın metnini içerir. `intentName` alanı, LUIS uygulaması içindeki mevcut bir amacın adına karşılık gelmelidir. `entityLabels` alanı gereklidir. Herhangi bir varlığı etiketlemek istemiyorsanız, boş bir dizi girin.

entityLabels dizisi boş değilse `startCharIndex` ve `endCharIndex` değerlerinin `entityName` alanında başvurulan varlığı işaretlemesi gerekir. Dizin sıfır tabanlıdır, yani üstteki örnekte 6 sayısı Seattle sözcüğünün baş harfi S'den önceki boşluğa değil S harfine karşılık gelmektedir. Etiketi metindeki bir boşlukla başlatır veya bitirirseniz, konuşmaları eklemek için yapılan API çağrısı başarısız olur.

```JSON
[
  {
    "text": "go to Seattle today",
    "intentName": "BookFlight",
    "entityLabels": [
      {
        "entityName": "Location::LocationTo",
        "startCharIndex": 6,
        "endCharIndex": 12
      }
    ]
  },
  {
    "text": "purple dogs are difficult to work with",
    "intentName": "BookFlight",
    "entityLabels": []
  }
]
```
