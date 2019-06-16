---
title: Telefon numarası önceden oluşturulmuş varlıklar
titleSuffix: Azure
description: Bu makale, telefon numarası önceden oluşturulmuş varlık bilgisi Language Understanding (LUIS) içerir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 43d0b855c25ed10b074d99b247ee56dc2ba7769b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65146193"
---
# <a name="phone-number-prebuilt-entity-for-a-luis-app"></a>Bir LUIS uygulaması için telefon numarası önceden oluşturulmuş varlık
`phonenumber` Varlık ülke kodunu içeren telefon numaralarını çeşitli ayıklar. Bu varlık zaten eğitildi olduğundan, uygulama için örnek Konuşma ekleme gerekmez. `phonenumber` Varlık içerisinde desteklendiği `en-us` yalnızca kültür. 

## <a name="types-of-a-phone-number"></a>Bir telefon numarası türü
`Phonenumber` gelen yönetilen [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-PhoneNumbers.yaml) GitHub deposu

## <a name="resolution-for-this-prebuilt-entity"></a>Önceden oluşturulmuş bu varlık için çözümleme

### <a name="api-version-2x"></a>API sürüm 2.x

Aşağıdaki örnek, çözünürlüğünü gösterir **builtin.phonenumber** varlık.

```json
{
  "query": "my mobile is 1 (800) 642-7676",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.8448457
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.8448457
    }
  ],
  "entities": [
    {
        "entity": "1 (800) 642-7676",
        "type": "builtin.phonenumber",
        "startIndex": 13,
        "endIndex": 28,
        "resolution": {
            "score": "1",
            "value": "1 (800) 642-7676"
        }
    }
  ]
}
```

### <a name="preview-api-version-3x"></a>Önizleme API sürümü 3.x

Aşağıdaki JSON ile olan `verbose` parametresini `false`:

```json
{
    "query": "my mobile is 1 (800) 642-7676",
    "prediction": {
        "normalizedQuery": "my mobile is 1 (800) 642-7676",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.592748761
            }
        },
        "entities": {
            "phonenumber": [
                "1 (800) 642-7676"
            ]
        }
    }
}
```

Aşağıdaki JSON ile olan `verbose` parametresini `true`:

```json
{
    "query": "my mobile is 1 (800) 642-7676",
    "prediction": {
        "normalizedQuery": "my mobile is 1 (800) 642-7676",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.592748761
            }
        },
        "entities": {
            "phonenumber": [
                "1 (800) 642-7676"
            ],
            "$instance": {
                "phonenumber": [
                    {
                        "type": "builtin.phonenumber",
                        "text": "1 (800) 642-7676",
                        "startIndex": 13,
                        "length": 16,
                        "score": 1,
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

Hakkında bilgi edinin [yüzdesi](luis-reference-prebuilt-percentage.md), [numarası](luis-reference-prebuilt-number.md), ve [sıcaklık](luis-reference-prebuilt-temperature.md) varlıklar. 
