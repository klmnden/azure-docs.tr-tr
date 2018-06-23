---
title: Duygu tanıma API'si PHP hızlı başlangıç | Microsoft Docs
description: Hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get duygu tanıma API'si PHP Bilişsel hizmetler ile birlikte kullanmaya başlayın.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 05/23/2017
ms.author: anroth
ms.openlocfilehash: bb3f3bc1421e7c65a1fa6095f22f499415c50890
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352391"
---
# <a name="emotion-api-php-quick-start"></a>Duygu tanıma API'si PHP hızlı başlangıç

> [!IMPORTANT]
> Video API Önizleme 30 Ekim 2017 sona erer. Yeni deneyin [Video dizin oluşturucu API önizlemesi](https://azure.microsoft.com/services/cognitive-services/video-indexer/) kolayca videoların öngörüleri ayıklamak ve konuşulan sözcüklerin, yüzler, karakterler ve duygular algılayarak arama sonuçları gibi içerik bulma deneyimlerini geliştirmek üzere. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

Bu makalede bilgiler sağlar ve hızlı bir şekilde yardımcı olmak için kod örnekleri, PHP kullanımına başlamanıza ve [duygu tanıma API'si tanıması yöntemi](https://dev.projectoxford.ai/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) görüntünün bir veya daha fazla kişiler tarafından ifade duygular tanımak için. 

## <a name="prerequisite"></a>Önkoşul
* Ücretsiz abonelik anahtarınızı alın [burada](https://azure.microsoft.com/try/cognitive-services/)

## <a name="recognize-emotions-php-example-request"></a>Tanı duygular PHP örnek istek

Abonelik anahtarlarınızı burada aldığınız konumu kullanmak için REST URL'sini, test etmek istediğiniz görüntünün bir URL'ye gövdesi değiştirmek ve "Ocp-Apim-Subscription-Key" değeri geçerli bir abonelik anahtarınızla değiştirin.

```php
<?php
// This sample uses the Apache HTTP client from HTTP Components (http://hc.apache.org/httpcomponents-client-ga/)
require_once 'HTTP/Request2.php';

// NOTE: You must use the same region in your REST call as you used to obtain your subscription keys.
//   For example, if you obtained your subscription keys from westcentralus, replace "westus" in the 
//   URL below with "westcentralus".
$request = new Http_Request2('https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize');
$url = $request->getUrl();

$headers = array(
    // Request headers
    'Content-Type' => 'application/json',

    // NOTE: Replace the "Ocp-Apim-Subscription-Key" value with a valid subscription key.
    'Ocp-Apim-Subscription-Key' => '13hc77781f7e4b19b5fcdd72a8df7156',
);

$request->setHeader($headers);

$parameters = array(
    // Request parameters
);

$url->setQueryVariables($parameters);

$request->setMethod(HTTP_Request2::METHOD_POST);

// Request body
$request->setBody('{"url": "http://www.example.com/images/image.jpg"}');

try
{
    $response = $request->send();
    echo $response->getBody();
}
catch (HttpException $ex)
{
    echo $ex;
}

?>
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

