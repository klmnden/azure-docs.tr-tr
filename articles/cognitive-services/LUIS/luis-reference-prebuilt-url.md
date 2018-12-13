---
title: URL önceden oluşturulmuş varlıklar
titleSuffix: Azure
description: Bu makalede URL'sini içeren önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: e01bcd022b5688b6cf00f63fcf4a0f06a477ebed
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53093633"
---
# <a name="url-entity"></a>URL varlığı
URL varlık URL'leri ile etki alanı adlarını veya IP adreslerini ayıklar. Bu varlık zaten eğitildi olduğundan, uygulamaya URL'ler içeren örnek Konuşma ekleme gerekmez. URL varlık içerisinde desteklendiği `en-us` yalnızca kültür. 

## <a name="types-of-urls"></a>URL türleri
URL, gelen yönetilir [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-URL.yaml) Github deposu

## <a name="resolution-for-prebuilt-url-entity"></a>Önceden oluşturulmuş URL varlık için çözümleme
Aşağıdaki örnek, çözünürlüğünü gösterir **builtin.url** varlık.

```json
{
  "query": "http://www.luis.ai is a great cognitive services example of artificial intelligence",
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
      "entity": "http://www.luis.ai",
      "type": "builtin.url",
      "startIndex": 0,
      "endIndex": 17
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [sıralı](luis-reference-prebuilt-ordinal.md), [numarası](luis-reference-prebuilt-number.md), ve [sıcaklık](luis-reference-prebuilt-temperature.md) varlıklar.