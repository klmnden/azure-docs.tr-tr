---
title: Önceden oluşturulmuş varlık geçerlilik süresi
titleSuffix: Azure
description: Bu makalede yaş içeren önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: b8239688000f0ce32ca2c2be054b1443bbb698b5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65069129"
---
# <a name="age-prebuilt-entity-for-a-luis-app"></a>Bir LUIS uygulaması için önceden oluşturulmuş varlık geçerlilik süresi
Önceden oluşturulmuş yaş varlık, hem sayısal ve gün, hafta, ay ve yıl açısından yaş değeri yakalar. Bu varlık zaten eğitildi çünkü uygulama hedefleri için yaş içeren örnek Konuşma ekleme gerekmez. Yaş varlık içerisinde desteklendiği [çok kültür](luis-reference-prebuilt-entities.md). 

## <a name="types-of-age"></a>Yaş türleri
Yaş yönetilen [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L3) GitHub deposu

## <a name="resolution-for-prebuilt-age-entity"></a>Önceden oluşturulmuş yaş varlık için çözümleme

### <a name="api-version-2x"></a>API sürüm 2.x

Aşağıdaki örnek, çözünürlüğünü gösterir **builtin.age** varlık.

```json
{
  "query": "A 90 day old utilities bill is quite late.",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.8236133
  },
  "entities": [
    {
      "entity": "90 day old",
      "type": "builtin.age",
      "startIndex": 2,
      "endIndex": 11,
      "resolution": {
        "unit": "Day",
        "value": "90"
      }
    }
  ]
}
```

### <a name="preview-api-version-3x"></a>Önizleme API sürümü 3.x

Aşağıdaki JSON ile olan `verbose` parametresini `false`:

```json
{
    "query": "A 90 day old utilities bill is quite late.",
    "prediction": {
        "normalizedQuery": "a 90 day old utilities bill is quite late.",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.558252
            }
        },
        "entities": {
            "age": [
                {
                    "number": 90,
                    "unit": "Day"
                }
            ]
        }
    }
}
```

Aşağıdaki JSON ile olan `verbose` parametresini `true`:

```json
{
    "query": "A 90 day old utilities bill is quite late.",
    "prediction": {
        "normalizedQuery": "a 90 day old utilities bill is quite late.",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.558252
            }
        },
        "entities": {
            "age": [
                {
                    "number": 90,
                    "unit": "Day"
                }
            ],
            "$instance": {
                "age": [
                    {
                        "type": "builtin.age",
                        "text": "90 day old",
                        "startIndex": 2,
                        "length": 10,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor"
                    }
                ]
            }
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [para birimi](luis-reference-prebuilt-currency.md), [datetimeV2](luis-reference-prebuilt-datetimev2.md), ve [boyut](luis-reference-prebuilt-dimension.md) varlıklar. 
