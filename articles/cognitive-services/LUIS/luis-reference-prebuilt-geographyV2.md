---
title: Coğrafya V2 önceden oluşturulmuş varlık - LUIS başvurusu
titleSuffix: Azure Cognitive Services
description: Bu makalede geographyV2 içeren önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/24/2018
ms.author: diberry
ms.openlocfilehash: 3559bc02944f88f486104d4d9553f0c45a1f1754
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46983421"
---
# <a name="geographyv2-entity"></a>GeographyV2 varlık
Önceden oluşturulmuş geographyV2 varlık yerler algılar. Bu varlık zaten eğitildi çünkü GeographyV2 içeren uygulama hedefleri için örnek Konuşma ekleme gerekmez. GeographyV2 varlık İngilizce olarak desteklenen [kültür](luis-reference-prebuilt-entities.md).

## <a name="subtypes"></a>Alt türleri
Coğrafi konumları subtypes vardır:

|Alt tür|Amaç|
|--|--|
|`poi`|ilgi noktası|
|`city`|Şehir adı|
|`countryRegion`|Ülke veya bölgesinin adı|
|`continent`|Kıta adı|
|`state`|Eyalet veya bölge adı|


## <a name="resolution-for-geographyv2-entity"></a>GeographyV2 varlık için çözümleme
Aşağıdaki örnek, çözünürlüğünü gösterir **builtin.geographyV2** varlık.

```JSON
{
    "query": "Carol is visiting the sphinx in gizah egypt in africa before heading to texas",
    "topScoringIntent": {
        "intent": "None",
        "score": 0.8008023
    },
    "intents": [
        {
            "intent": "None",
            "score": 0.8008023
        }
    ],
    "entities": [
        {
            "entity": "the sphinx",
            "type": "builtin.geographyV2.poi",
            "startIndex": 18,
            "endIndex": 27
        },
        {
            "entity": "gizah",
            "type": "builtin.geographyV2.city",
            "startIndex": 32,
            "endIndex": 36
        },
        {
            "entity": "egypt",
            "type": "builtin.geographyV2.countryRegion",
            "startIndex": 38,
            "endIndex": 42
        },
        {
            "entity": "africa",
            "type": "builtin.geographyV2.continent",
            "startIndex": 47,
            "endIndex": 52
        },
        {
            "entity": "texas",
            "type": "builtin.geographyV2.state",
            "startIndex": 72,
            "endIndex": 76
        },
        {
            "entity": "carol",
            "type": "builtin.personName",
            "startIndex": 0,
            "endIndex": 4
        }
    ]
} 
```

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [e-posta](luis-reference-prebuilt-email.md), [numarası](luis-reference-prebuilt-number.md), ve [sıralı](luis-reference-prebuilt-ordinal.md) varlıklar. 