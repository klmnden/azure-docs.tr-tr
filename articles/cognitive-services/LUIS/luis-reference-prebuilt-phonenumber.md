---
title: Telefon numarası önceden oluşturulmuş varlıklar
titleSuffix: Azure
description: Bu makale, telefon numarası önceden oluşturulmuş varlık bilgisi Language Understanding (LUIS) içerir.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 3c2a873514c7355cdf7fa08372e1e17409021d56
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53081073"
---
# <a name="phonenumber-entity"></a>Telefon numarası varlığı
`phonenumber` Varlık ülke kodunu içeren telefon numaralarını çeşitli ayıklar. Bu varlık zaten eğitildi olduğundan, uygulama için örnek Konuşma ekleme gerekmez. `phonenumber` Varlık içerisinde desteklendiği `en-us` yalnızca kültür. 

## <a name="types-of-phonenumber"></a>Phonenumber türleri
PhoneNumber yönetilen engelle [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-PhoneNumbers.yaml) Github deposu

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