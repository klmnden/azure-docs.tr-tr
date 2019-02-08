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
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: fa2163b37bda5beba7c36bfdff0778c4062c5546
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55863424"
---
# <a name="phonenumber-prebuilt-entity-for-a-luis-app"></a>PhoneNumber LUIS uygulaması için önceden oluşturulmuş varlık
`phonenumber` Varlık ülke kodunu içeren telefon numaralarını çeşitli ayıklar. Bu varlık zaten eğitildi olduğundan, uygulama için örnek Konuşma ekleme gerekmez. `phonenumber` Varlık içerisinde desteklendiği `en-us` yalnızca kültür. 

## <a name="types-of-phonenumber"></a>Phonenumber türleri
PhoneNumber yönetilen engelle [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-PhoneNumbers.yaml) GitHub deposu

## <a name="resolution-for-prebuilt-phonenumber-entity"></a>Önceden oluşturulmuş phonenumber varlık için çözümleme
Aşağıdaki örnek, çözünürlüğünü gösterir **builtin.phonenumber** varlık.

```json
{
  "query": "my mobile is 00 44 161 1234567",
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
      "entity": "00 44 161 1234567",
      "type": "builtin.phonenumber",
      "startIndex": 13,
      "endIndex": 29,
      "resolution": {
        "value": "00 44 161 1234567"
      }
    }
  ]
}
```


## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [yüzdesi](luis-reference-prebuilt-percentage.md), [numarası](luis-reference-prebuilt-number.md), ve [sıcaklık](luis-reference-prebuilt-temperature.md) varlıklar. 
