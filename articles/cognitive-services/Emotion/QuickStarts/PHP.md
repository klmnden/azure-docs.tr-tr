---
title: "Hızlı Başlangıç: Görüntü - duygu tanıma API'si, PHP yüzlerini üzerinde duyguları tanıma"
titlesuffix: Azure Cognitive Services
description: PHP ile Duygu Tanıma API'sini kullanmaya başlamanıza yardımcı olacak bilgiler ve kod örnekleri edinin.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: quickstart
ms.date: 05/23/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: 3acf47ee26974ddff4f6063eef95bb29be61fce3
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55215504"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>Hızlı Başlangıç: Görüntüdeki yüzleri üzerinde duyguları tanıma için bir uygulama oluşturun.

> [!IMPORTANT]
> Duygu Tanıma API'si 15 Şubat 2019 tarihinde kullanım dışı bırakılacaktır. Duygu tanıma özelliği [Yüz Tanıma API'sinin](https://docs.microsoft.com/azure/cognitive-services/face/) bir parçası olarak genel kullanıma sunulmuştur. 

Bu makalede bir görüntüdeki bir veya daha fazla kişinin duygularını tanımak için PHP ile [Duygu Tanıma API'si Recognize metodunu](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) kullanmaya başlamanıza yardımcı olacak bilgiler ve kod örnekleri bulunmaktadır.

## <a name="prerequisite"></a>Önkoşul
* [Buradan](https://azure.microsoft.com/try/cognitive-services/) ücretsiz Abonelik Anahtarınızı alın

## <a name="recognize-emotions-php-example-request"></a>Duygu Tanıma PHP Örnek İsteği

REST URL'sini abonelik anahtarlarınızı aldığınız konumu kullanacak şekilde değiştirin, gövdeyi test etmek istediğiniz görüntünün URL'sini yazarak güncelleştirin ve "Ocp-Apim-Subscription-Key" değerinin yerine geçerli abonelik anahtarınızı yazın.

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

## <a name="recognize-emotions-sample-response"></a>Duygu Tanıma Örneği Yanıtı
Başarılı bir çağrı, yüz dikdörtgenine göre büyükten küçüğe doğru sıralanmış şekilde yüz girişlerinden ve ilgili duygu puanlarından oluşan bir dizi döndürür. Yanıtın boş olması hiç yüz algılanmadığını gösterir. Duygu girişi aşağıdaki alanları içerir:
* faceRectangle - Dikdörtgen şeklinde yüzün görüntü içindeki konumu.
* scores - Görüntüdeki her bir yüzün duygu puanı.

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
