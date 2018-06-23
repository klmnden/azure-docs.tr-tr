---
title: Duygu tanıma API'si cURL hızlı başlangıç | Microsoft Docs
description: Hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get duygu tanıma API'si Bilişsel hizmetler cURL ile kullanmaya başlayın.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 05/23/2017
ms.author: anroth
ms.openlocfilehash: d36a191d3589702d676e694715f34ea1c924e2d3
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352385"
---
# <a name="emotion-api-curl-quick-start"></a>Duygu tanıma API'si cURL hızlı başlangıç

> [!IMPORTANT]
> Video API Önizleme 30 Ekim 2017 sona erer. Yeni deneyin [Video dizin oluşturucu API önizlemesi](https://azure.microsoft.com/services/cognitive-services/video-indexer/) kolayca videoların öngörüleri ayıklamak ve konuşulan sözcüklerin, yüzler, karakterler ve duygular algılayarak arama sonuçları gibi içerik bulma deneyimlerini geliştirmek üzere. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

Bu makalede bilgiler sağlar ve hızlı bir şekilde yardımcı olmak için kod örnekleri, kullanımına başlamanıza [duygu tanıma API'si tanıması yöntemi](https://dev.projectoxford.ai/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) görüntünün bir veya daha fazla kişiler tarafından ifade duygular tanımak için cURL ile. 

## <a name="prerequisite"></a>Önkoşul
* Ücretsiz abonelik anahtarınızı alın [burada](https://azure.microsoft.com/try/cognitive-services/)

## <a name="recognize-emotions-curl-example-request"></a>Örnek istek duygular cURL tanı

> [!NOTE]
> Abonelik anahtarlarınızı elde etmek için kullanılan yazarken, REST çağrısı, aynı konuma kullanmanız gerekir. Abonelik anahtarlarınızı westcentralus edindiyseniz, örneğin, "westus" URL'de "westcentralus" ile değiştirin.

```json
@ECHO OFF

curl -v -X POST "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize"
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: {subscription key}"

--data-ascii "{body}" 
```

## <a name="recognize-emotions-sample-response"></a>Tanı duygular örnek yanıt
Başarılı bir çağrı yüz girişleri dizisi ve azalan düzende yüz dikdörtgen boyutuna göre derece ilişkili duygu puanlarını döndürür. Boş bir yanıt hiçbir yüzeyleri algıladığını belirtir. Bir duygu giriş aşağıdaki alanları içerir:
* faceRectangle - yüz görüntüdeki dikdörtgen konumu.
* puanları - görüntüdeki her yüz duygu puanlarını. 

```json
application/json 
[
  {
    "faceRectangle": {
      "left": 68,
      "top": 97,
      "width": 64,
      "height": 97
    },
    "scores": {
      "anger": 0.00300731952,
      "contempt": 5.14648448E-08,
      "disgust": 9.180124E-06,
      "fear": 0.0001912825,
      "happiness": 0.9875571,
      "neutral": 0.0009861537,
      "sadness": 1.889955E-05,
      "surprise": 0.008229999
    }
  }
]

