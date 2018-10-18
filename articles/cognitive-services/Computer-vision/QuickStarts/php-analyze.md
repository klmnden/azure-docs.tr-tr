---
title: 'Hızlı Başlangıç: Uzak görüntüyü analiz etme - REST, PHP - Görüntü İşleme'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, PHP ile Görüntü İşleme API’si kullanarak bir görüntüyü analiz edeceksiniz.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: pafarley
ms.openlocfilehash: d9e65b5fa42cc9026f8b1e3f18901f988c38cc46
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49341013"
---
# <a name="quickstart-analyze-a-remote-image-using-the-rest-api-and-php-in-computer-vision"></a>Hızlı Başlangıç: Görüntü İşleme’de REST API ve PHP’yi kullanarak uzak görüntüyü analiz etme

Bu hızlı başlangıçta, Görüntü İşleme’nin REST API’sini kullanarak görsel özellikleri ayıklamak için uzakta depolanan bir görüntüyü analiz edeceksiniz. [Görüntü Analizi](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) yöntemi ile, görüntü içeriğini temel alarak görsel özellikleri ayıklayabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

- [PHP](https://secure.php.net/downloads.php) yüklenmiş olmalıdır.
- [Pear](https://pear.php.net) yüklenmiş olmalıdır.
- Görüntü İşleme için bir abonelik anahtarınız olması gerekir. Bir abonelik anahtarı almak için bkz. [Abonelik Anahtarları Alma](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="create-and-run-the-sample"></a>Örnek oluşturma ve çalıştırma

Örneği oluşturup çalıştırmak için aşağıdaki adımları uygulayın:

1. PHP5 [`HTTP_Request2`](http://pear.php.net/package/HTTP_Request2) paketini yükleyin.
   1. Yönetici olarak bir komut istemi penceresini açın.
   1. Şu komutu çalıştırın:

      ```console
      pear install HTTP_Request2
      ```

   1. Paket başarıyla yüklendikten sonra komut istemi penceresini kapatın.

1. Aşağıdaki kodu bir metin düzenleyicisine kopyalayın.
1. Gerektiğinde kodda aşağıdaki değişiklikleri yapın:
    1. `subscriptionKey` değerini abonelik anahtarınızla değiştirin.
    1. Gerekirse `uriBase` değerini [Görüntü Analizi](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) yönteminin abonelik anahtarlarınızı aldığınız Azure bölgesinden uç nokta URL'si ile değiştirin.
    1. İsteğe bağlı olarak `imageUrl` değerini, analiz etmek istediğiniz başka bir görüntünün URL’si ile değiştirin.
    1. İsteğe bağlı olarak `language` istek parametresinin değerini farklı bir dil ile değiştirin.
1. Kodu, `.php` uzantısıyla bir dosya olarak kaydedin. Örneğin, `analyze-image.php`.
1. PHP desteğiyle bir tarayıcı penceresini açın.
1. Dosyayı tarayıcı penceresine sürükleyip bırakın.

```php
<html>
<head>
    <title>Analyze Image Sample</title>
</head>
<body>
<?php
// Replace <Subscription Key> with a valid subscription key.
$ocpApimSubscriptionKey = '<Subscription Key>';

// You must use the same location in your REST call as you used to obtain
// your subscription keys. For example, if you obtained your subscription keys
// from westus, replace "westcentralus" in the URL below with "westus".
$uriBase = 'https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/';

$imageUrl = 'http://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg';

require_once 'HTTP/Request2.php';

$request = new Http_Request2($uriBase . '/analyze');
$url = $request->getUrl();

$headers = array(
    // Request headers
    'Content-Type' => 'application/json',
    'Ocp-Apim-Subscription-Key' => $ocpApimSubscriptionKey
);
$request->setHeader($headers);

$parameters = array(
    // Request parameters
    'visualFeatures' => 'Categories,Description',
    'details' => '',
    'language' => 'en'
);
$url->setQueryVariables($parameters);

$request->setMethod(HTTP_Request2::METHOD_POST);

// Request body parameters
$body = json_encode(array('url' => $imageUrl));

// Request body
$request->setBody($body);

try
{
    $response = $request->send();
    echo "<pre>" .
        json_encode(json_decode($response->getBody()), JSON_PRETTY_PRINT) . "</pre>";
}
catch (HttpException $ex)
{
    echo "<pre>" . $ex . "</pre>";
}
?>
</body>
</html>
```

## <a name="examine-the-response"></a>Yanıtı inceleme

Başarılı bir yanıt JSON biçiminde döndürülür. Örnek web sitesi aşağıdaki örneğe benzer şekilde başarılı bir yanıtı ayrıştırıp tarayıcı penceresinde görüntüler:

```json
{
  "categories": [
    {
      "name": "outdoor_water",
      "score": 0.9921875,
      "detail": {
        "landmarks": []
      }
    }
  ],
  "description": {
    "tags": [
      "nature",
      "water",
      "waterfall",
      "outdoor",
      "rock",
      "mountain",
      "rocky",
      "grass",
      "hill",
      "covered",
      "hillside",
      "standing",
      "side",
      "group",
      "walking",
      "white",
      "man",
      "large",
      "snow",
      "grazing",
      "forest",
      "slope",
      "herd",
      "river",
      "giraffe",
      "field"
    ],
    "captions": [
      {
        "text": "a large waterfall over a rocky cliff",
        "confidence": 0.916458423253597
      }
    ]
  },
  "requestId": "ebf5a1bc-3ba2-4c56-99b4-bbd20ba28705",
  "metadata": {
    "height": 959,
    "width": 1280,
    "format": "Jpeg"
  }
}
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse dosyayı silin ve sonra PHP5 `HTTP_Request2` paketini kaldırın. Paketi kaldırmak için aşağıdaki adımları uygulayın:

1. Yönetici olarak bir komut istemi penceresini açın.
2. Şu komutu çalıştırın:

   ```console
   pear uninstall HTTP_Request2
   ```

3. Paket başarıyla kaldırıldıktan sonra komut istemi penceresini kapatın.

## <a name="next-steps"></a>Sonraki adımlar

Görüntü analiz etmek, ünlüleri ve yer işaretlerini algılamak, küçük resim oluşturmak ve yazdırılan ve el yazısı metinleri ayıklamak için kullanılan Görüntü İşleme API’sini keşfedin. Görüntü İşleme API'sini hızlı bir şekilde denemeniz için [Open API test konsolu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) konusuna bakın.

> [!div class="nextstepaction"]
> [Görüntü İşleme API’sini keşfetme](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
