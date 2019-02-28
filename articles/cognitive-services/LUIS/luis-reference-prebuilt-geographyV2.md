---
title: Coğrafya V2 önceden oluşturulmuş varlık
titleSuffix: Azure Cognitive Services
description: Bu makalede geographyV2 içeren önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: 17f612f2ee6c7d27dcec9f72ed3df1ed418eb3d2
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56961624"
---
# <a name="geographyv2-prebuilt-entity-for-a-luis-app"></a>GeographyV2 LUIS uygulaması için önceden oluşturulmuş varlık
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

```json
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
