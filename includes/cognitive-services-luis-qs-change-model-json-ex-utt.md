---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: include
ms.custom: include file
ms.date: 08/16/2018
ms.author: diberry
ms.openlocfilehash: 419f15901b665b43b850922f77bd32d7aac8d3a2
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42920522"
---
Örnek konuşma dosyası **utterances.json**, belirli bir biçimdedir. 

`text` Alan örnek utterance metni içerir. `intentName` Alan LUIS uygulaması mevcut bir amaca adı gelmelidir. `entityLabels` alanı gereklidir. Herhangi bir varlık etiketi istemiyorsanız, boş bir dizi sağlar.

EntityLabels dizisi boş değilse `startCharIndex` ve `endCharIndex` başvurulan varlık işaretlemek gereken `entityName` alan. Dizin sıfır, "S" alanı ile Seattle ve büyük harf s önce üst örnekte 6 başvurduğu anlamına gelir. Başlamak veya bitiş etiketi boşlukla metin konuşma eklemek için API çağrısı başarısız olur.

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