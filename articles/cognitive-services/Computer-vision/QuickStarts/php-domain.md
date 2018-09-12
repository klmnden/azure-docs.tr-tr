---
title: Görüntü İşleme API'si PHP hızlı başlangıcı alan modeli | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Bu hızlı başlangıçta, Bilişsel Hizmetler’de PHP ile Görüntü İşleme’yi kullanarak bir görüntüdeki önemli yerleri tanımlamak için alan modeli kullanacaksınız.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: 335065b45781dd2712f5416fb03a2f8726182472
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43772626"
---
# <a name="quickstart-use-a-domain-model---rest-php"></a>Hızlı Başlangıç: Alan modeli kullanma - REST, PHP

Bu hızlı başlangıçta, Görüntü İşleme kullanarak bir görüntüdeki önemli yerleri veya ünlüleri tanımlamak için alan modeli kullanacaksınız.

## <a name="prerequisites"></a>Ön koşullar

Görüntü İşleme’yi kullanmak için, bir abonelik anahtarınızın olması gerekir; bkz. [Abonelik Anahtarlarını Alma](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="recognize-landmark-request"></a>Önemli Yer Tanıma isteği

[Alana Özgü İçerik Tanıma yöntemi](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e200) ile, bir görüntüdeki belirli bir nesne kümesini tanımlayabilirsiniz. Şu anda kullanılabilir olan alana özgü iki model, _ünlüler_ ve _önemli yerler_’dir.

Örneği çalıştırmak için aşağıdaki adımları uygulayın:

1. Aşağıdaki kodu bir düzenleyicinin içine kopyalayın.
1. `<Subscription Key>` değerini geçerli abonelik anahtarınızla değiştirin.
1. Gerekirse `uriBase` değerini abonelik anahtarlarınızı aldığınız konumu kullanacak şekilde değiştirin.
1. İsteğe bağlı olarak, `imageUrl` öğesini analiz etmek istediğiniz görüntü olarak ayarlayın.
1. İsteğe bağlı olarak, Ünlüler modelini kullanmak için `domain` öğesini `celebrities` olarak ayarlayın.
1. Dosyası `.php` uzantısıyla kaydedin.
1. Dosyayı PHP desteğiyle bir tarayıcı penceresinde açın.

Aşağıdaki örnek, görüntüdeki bir önemli yeri tanımlar.

Bu örnek, PHP5 [HTTP_Request2](http://pear.php.net/package/HTTP_Request2) paketini kullanır.

```php
<html>
<head>
    <title>Analyze Domain Model Sample</title>
</head>
<body>
<?php
// Replace <Subscription Key> with a valid subscription key.
$ocpApimSubscriptionKey = '<Subscription Key>';

// You must use the same location in your REST call as you used to obtain
// your subscription keys. For example, if you obtained your subscription keys
// from westus, replace "westcentralus" in the URL below with "westus".
$uriBase = 'https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/';

// Change 'landmarks' to 'celebrities' to use the Celebrities model.
$domain = 'landmarks';

$imageUrl =
    'https://upload.wikimedia.org/wikipedia/commons/2/23/Space_Needle_2011-07-04.jpg';

require_once 'HTTP/Request2.php';

$request = new Http_Request2($uriBase . 'models/' . $domain . '/analyze');
$url = $request->getUrl();

$headers = array(
    // Request headers
    'Content-Type' => 'application/json',
    'Ocp-Apim-Subscription-Key' => $ocpApimSubscriptionKey
);
$request->setHeader($headers);

$parameters = array(
    // Request parameters
    'model' => $domain
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

## <a name="recognize-landmark-response"></a>Önemli Yer Tanıma yanıtı

JSON’da başarılı bir yanıt döndürülür, örneğin:

```json
{
    "result": {
        "landmarks": [
            {
                "name": "Space Needle",
                "confidence": 0.9998177886009216
            }
        ]
    },
    "requestId": "4d26587b-b2b9-408d-a70c-1f8121d84b0d",
    "metadata": {
        "height": 4132,
        "width": 2096,
        "format": "Jpeg"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Görüntü analiz etmek, ünlüleri ve önemli yerleri algılamak, küçük resim oluşturmak ve yazdırılan ve el yazısı metinleri ayıklamak için kullanılan Görüntü İşleme API'lerini keşfedin. Görüntü İşleme API'lerini hızlı bir şekilde denemeniz için [Open API test konsolu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) konusuna göz atın.

> [!div class="nextstepaction"]
> [Görüntü İşleme API'lerini keşfedin](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
