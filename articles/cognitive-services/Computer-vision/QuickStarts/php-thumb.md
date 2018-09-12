---
title: 'Hızlı Başlangıç: Küçük resim oluşturma - REST, PHP - Görüntü İşleme'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Bilişsel Hizmetler’de PHP ile Görüntü İşleme kullanarak bir görüntüden küçük resim oluşturacaksınız.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: dc2d5c4e5a1a9a098ad8b70b6b7a280c4dd02299
ms.sourcegitcommit: 3d0295a939c07bf9f0b38ebd37ac8461af8d461f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "43841905"
---
# <a name="quickstart-generate-a-thumbnail---rest-php---computer-vision"></a>Hızlı Başlangıç: Küçük resim oluşturma - REST, PHP - Görüntü İşleme

Bu hızlı başlangıçta, Görüntü İşleme kullanarak bir görüntüden küçük resim oluşturacaksınız.

## <a name="prerequisites"></a>Ön koşullar

Görüntü İşleme’yi kullanmak için, bir abonelik anahtarınızın olması gerekir; bkz. [Abonelik Anahtarlarını Alma](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="get-thumbnail-request"></a>Küçük Resim Alma isteği

[Küçük Resim Alma yöntemi](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) ile, bir görüntünün küçük resmini alabilirsiniz. Giriş görüntüsünün en boy oranından farklı olabilen bir yükseklik ve genişlik belirtirsiniz. Görüntü İşleme, ilgi bölgesini akıllı bir şekilde belirlemek için akıllı kırpma özelliğini kullanır ve ilgili bölgeyi temel alan kırpma koordinatları oluşturur.

Örneği çalıştırmak için aşağıdaki adımları uygulayın:

1. Aşağıdaki kodu bir düzenleyicinin içine kopyalayın.
1. `<Subscription Key>` değerini geçerli abonelik anahtarınızla değiştirin.
1. Gerekirse `uriBase` değerini abonelik anahtarlarınızı aldığınız konumu kullanacak şekilde değiştirin.
1. İsteğe bağlı olarak, `imageUrl` öğesini analiz etmek istediğiniz görüntü olarak ayarlayın.
1. Dosyayı `.php` uzantısıyla kaydedin.
1. Dosyayı PHP desteğiyle bir tarayıcı penceresinde açın.

Bu örnek, PHP5 [HTTP_Request2](http://pear.php.net/package/HTTP_Request2) paketini kullanır.

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

## <a name="get-thumbnail-response"></a>Küçük Resim Alma yanıtı

Başarılı bir yanıt, küçük resim görüntü ikili dosyasını içerir. İstek başarısız olursa yanıt, bir hata kodu ve nelerin yanlış gittiğini belirlemeye yardımcı olması için bir ileti içerir.

## <a name="next-steps"></a>Sonraki adımlar

Görüntü analiz etmek, ünlüleri ve önemli yerleri algılamak, küçük resim oluşturmak ve yazdırılan ve el yazısı metinleri ayıklamak için kullanılan Görüntü İşleme API'lerini keşfedin. Görüntü İşleme API'lerini hızlı bir şekilde denemeniz için [Open API test konsolu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) konusuna göz atın.

> [!div class="nextstepaction"]
> [Görüntü İşleme API'lerini keşfedin](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
