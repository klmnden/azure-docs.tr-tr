---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: include
ms.custom: include file
ms.date: 08/16/2018
ms.author: diberry
ms.openlocfilehash: dae56e05f01e83f05e75fdf378c0c50679d18728
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42820268"
---
LUIS tahmin uç nokta ne döndürür anlamak için bir web tarayıcısında tahmin sonuç görüntüleyin. Genel Uygulama sorgulamak için kendi anahtarını ve uygulama kimliğini gerekir Ortak IOT uygulama kimliği `df67dcdb-c37d-46af-88e1-8b97951ca1c2`, Birinci adımdaki URL'nin bir parçası olarak sağlanır.

URL biçimi bir **alma** uç nokta isteği:

```JSON
https://<region>.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>?subscription-key=<YOUR-KEY>&q=<user-utterance>
```

1. Genel IOT uygulama uç noktası bu biçimdedir: `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=<YOUR_KEY>&q=turn on the bedroom light`

    URL'yi kopyalayın ve anahtarınızı değerinin yerine `<YOUR_KEY>`.

2. URL'yi bir tarayıcı penceresine yapıştırıp Enter tuşuna basın. LUIS algıladığını belirten bir JSON sonucu tarayıcı görüntüler `HomeAutomation.TurnOn` üst hedefi olarak hedefi ve `HomeAutomation.Room` değerle varlık `bedroom`.

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

3. URL'deki `q=` parametresinin değerini `turn off the living room light` olarak değiştirip Enter tuşuna basın. Sonuç artık LUIS algılandığını gösterir `HomeAutomation.TurnOff` üst hedefi olarak hedefi ve `HomeAutomation.Room` değerle varlık `living room`. 

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
