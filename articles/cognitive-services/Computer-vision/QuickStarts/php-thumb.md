---
title: 'Hızlı Başlangıç: Küçük resim - REST, PHP oluşturma'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, PHP ile Görüntü İşleme API’sini kullanarak bir görüntüden küçük resim oluşturacaksınız.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: a9a3e2cb1dec8d47c468313f057e3845ea81900c
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55222380"
---
# <a name="quickstart-generate-a-thumbnail-using-the-rest-api-and-php-in-computer-vision"></a>Hızlı Başlangıç: Görüntü işleme PHP ve REST API kullanarak küçük resim oluşturma

Bu hızlı başlangıçta, Görüntü İşleme REST API’sini kullanarak bir görüntüden küçük resim oluşturacaksınız. [Küçük Resim Alma](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) yöntemi ile bir görüntünün küçük resmini alabilirsiniz. Giriş görüntüsünün en boy oranından farklı olabilen bir yükseklik ve genişlik belirtirsiniz. Görüntü işleme, akıllı bir şekilde ilgi belirlemek ve söz konusu bölgeyi temel alan kırpma koordinatları oluşturmak için akıllı kırpma kullanır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

- [PHP](https://secure.php.net/downloads.php) yüklenmiş olmalıdır.
- [Pear](https://pear.php.net) yüklenmiş olmalıdır.
- Görüntü İşleme için bir abonelik anahtarınız olması gerekir. Bir abonelik anahtarı almak için bkz. [Abonelik Anahtarları Alma](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="create-and-run-the-sample"></a>Örnek oluşturma ve çalıştırma

Örneği oluşturup çalıştırmak için aşağıdaki adımları uygulayın:

1. PHP5 [`HTTP_Request2`](https://pear.php.net/package/HTTP_Request2) paketini yükleyin.
   1. Yönetici olarak bir komut istemi penceresini açın.
   1. Şu komutu çalıştırın:

      ```console
      pear install HTTP_Request2
      ```

   1. Paket başarıyla yüklendikten sonra komut istemi penceresini kapatın.

1. Aşağıdaki kodu bir metin düzenleyicisine kopyalayın.
1. Gerektiğinde kodda aşağıdaki değişiklikleri yapın:
    1. `subscriptionKey` değerini abonelik anahtarınızla değiştirin.
    1. Gerekirse `uriBase` değerini, abonelik anahtarlarınızı aldığınız Azure bölgesinden [Küçük Resim Al](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) yönteminin uç nokta URL’si ile değiştirin.
    1. `imageUrl` değerini isterseniz küçük resmini oluşturmak istediğiniz başka bir görüntünün URL’si ile de değiştirebilirsiniz.
1. Kodu, `.php` uzantısıyla bir dosya olarak kaydedin. Örneğin, `get-thumbnail.php`.
1. PHP desteğiyle bir tarayıcı penceresini açın.
1. Dosyayı tarayıcı penceresine sürükleyip bırakın.

```php
<html>
<head>
    <title>Get Thumbnail Sample</title>
</head>
<body>
<?php
// Replace <Subscription Key> with a valid subscription key.
$ocpApimSubscriptionKey = '<Subscription Key>';

// You must use the same location in your REST call as you used to obtain
// your subscription keys. For example, if you obtained your subscription keys
// from westus, replace "westcentralus" in the URL below with "westus".
$uriBase = 'https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/';

$imageUrl =
    'https://upload.wikimedia.org/wikipedia/commons/9/94/Bloodhound_Puppy.jpg';

require_once 'HTTP/Request2.php';

$request = new Http_Request2($uriBase . 'generateThumbnail');
$url = $request->getUrl();

$headers = array(
    // Request headers
    'Content-Type' => 'application/json',
    'Ocp-Apim-Subscription-Key' => $ocpApimSubscriptionKey
);
$request->setHeader($headers);

$parameters = array(
    // Request parameters
    'width' => '100',  // Width of the thumbnail.
    'height' => '100', // Height of the thumbnail.
    'smartCropping' => 'true',
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

Küçük resmin görüntü verilerini temsil eden ikili veri halinde başarılı bir yanıt döndürülür. İstek başarısız olursa yanıt, tarayıcı penceresinde görüntülenir. Başarısız isteğin yanıtı, neyin yanlış gittiğini belirlemeye yardımcı olması için bir hata kodu ve bir ileti içerir.

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
