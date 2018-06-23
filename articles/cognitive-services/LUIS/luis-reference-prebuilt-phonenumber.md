---
title: HALUK önceden oluşturulmuş varlıklar telefon numarası başvuru - Azure | Microsoft Docs
titleSuffix: Azure
description: Bu makale, telefon numarası önceden oluşturulmuş varlık bilgisi dil anlama (HALUK) içerir.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 0f72b807b9b0ec110a80d67babb1c45902b8c810
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321886"
---
# <a name="phonenumber-entity"></a>PhoneNumber varlık
`phonenumber` Varlık telefon numaraları ülke kodu da dahil olmak üzere çeşitli ayıklar. Bu varlık zaten eğitildi olduğundan, örnek utterances uygulamaya eklemek gerekmez. `phonenumber` Varlık desteklenir `en-us` yalnızca kültür. 

## <a name="types-of-phonenumber"></a>Phonenumber türleri
PhoneNumber gelen yönetilir [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-PhoneNumbers.yaml) Github deposunu

## <a name="resolution-for-prebuilt-phonenumber-entity"></a>Önceden oluşturulmuş phonenumber varlık için çözümleme
Aşağıdaki örnek, çözünürlüğünü gösterir **builtin.phonenumber** varlık.

```JSON
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

Hakkında bilgi edinin [yüzde](luis-reference-prebuilt-percentage.md), [numarası](luis-reference-prebuilt-number.md), ve [sıcaklık](luis-reference-prebuilt-temperature.md) varlıklar. 