---
title: URL önceden oluşturulmuş varlıklar
titleSuffix: Azure
description: Bu makalede URL'sini içeren önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/04/2019
ms.author: diberry
ms.openlocfilehash: 5fb62c38bde98d946694790adb860240eaa59fa9
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57530186"
---
# <a name="url-prebuilt-entity-for-a-luis-app"></a>URL bir LUIS uygulaması için önceden oluşturulmuş varlık
URL varlık URL'leri ile etki alanı adlarını veya IP adreslerini ayıklar. Bu varlık zaten eğitildi olduğundan, uygulamaya URL'ler içeren örnek Konuşma ekleme gerekmez. URL varlık içerisinde desteklendiği `en-us` yalnızca kültür. 

## <a name="types-of-urls"></a>URL türleri
URL, gelen yönetilir [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-URL.yaml) GitHub deposu

## <a name="resolution-for-prebuilt-url-entity"></a>Önceden oluşturulmuş URL varlık için çözümleme
Aşağıdaki örnek, çözünürlüğünü gösterir **builtin.url** varlık.

```json
{
  "query": "https://www.luis.ai is a great cognitive services example of artificial intelligence",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.781975448
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.781975448
    }
  ],
  "entities": [
    {
      "entity": "https://www.luis.ai",
      "type": "builtin.url",
      "startIndex": 0,
      "endIndex": 17
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [sıralı](luis-reference-prebuilt-ordinal.md), [numarası](luis-reference-prebuilt-number.md), ve [sıcaklık](luis-reference-prebuilt-temperature.md) varlıklar.
