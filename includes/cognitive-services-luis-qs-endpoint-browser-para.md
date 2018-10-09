---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: luis
ms.topic: include
ms.custom: include file
ms.date: 08/16/2018
ms.author: diberry
ms.openlocfilehash: e507a7c45e286473abe9b9e4365e80fb29eba2a4
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47044048"
---
Bir LUIS tahmin uç noktasının ne döndüğünü anlamak için bir tahmin sonucunu bir Web tarayıcısında görüntüleyin. Ortak bir uygulamayı sorgulamak için kendi anahtarınız ve uygulama kimliğiniz olması gerekir. Ortak IoT uygulama kimliği `df67dcdb-c37d-46af-88e1-8b97951ca1c2`, birinci adımda URL'nin bir parçası olarak verilmiştir.

Bir **GET** uç nokta isteğinin URL'sinin biçimi:

```JSON
https://<region>.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>?subscription-key=<YOUR-KEY>&q=<user-utterance>
```

1. Ortak IoT uygulamasının uç noktasının biçimi: `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=<YOUR_KEY>&q=turn on the bedroom light`

    URL'yi kopyalayın ve `<YOUR_KEY>` değerini anahtarınız ile değiştirin.

2. URL'yi bir tarayıcı penceresine yapıştırıp Enter tuşuna basın. Tarayıcıda LUIS'in `HomeAutomation.TurnOn` amacını en önemli amaç olarak ve `HomeAutomation.Room` varlığını `bedroom` değeriyle algıladığını gösteren bir JSON sonucu görüntülenir.

    ```JSON
    {
      "query": "turn on the bedroom light",
      "topScoringIntent": {
        "intent": "HomeAutomation.TurnOn",
        "score": 0.809439957
      },
      "entities": [
        {
          "entity": "bedroom",
          "type": "HomeAutomation.Room",
          "startIndex": 12,
          "endIndex": 18,
          "score": 0.8065475
        }
      ]
    }
    ```

3. URL'deki `q=` parametresinin değerini `turn off the living room light` olarak değiştirip Enter tuşuna basın. Sonuç şimdi LUIS'in `HomeAutomation.TurnOff` amacını en önemli amaç olarak ve `HomeAutomation.Room` varlığını `living room` değeriyle algıladığını gösterir. 

    ```JSON
    {
      "query": "turn off the living room light",
      "topScoringIntent": {
        "intent": "HomeAutomation.TurnOff",
        "score": 0.984057844
      },
      "entities": [
        {
          "entity": "living room",
          "type": "HomeAutomation.Room",
          "startIndex": 13,
          "endIndex": 23,
          "score": 0.9619945
        }
      ]
    }
    ```
