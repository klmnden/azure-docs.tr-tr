---
title: Anomali Bulucu API PHP - Microsoft Bilişsel hizmetler ile kullanma | Microsoft Docs
description: Anomali Bulucu, Bilişsel hizmetler, PHP kullanarak hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get başlayın.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.component: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: cb149d7a0a1eef2c096a3562f5d0256deca19527
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49458080"
---
# <a name="use-the-anomaly-finder-api-with-php"></a>Anomali Bulucu API PHP ile kullanma

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

Bu makalede bilgiler sağlanmaktadır ve kod örnekleri, hızlı bir şekilde yardımcı olması için anomali sonuç için zaman serisi verilerini alma görevi gerçekleştirmek için PHP ile Anomali Bulucu API'yi kullanarak kullanmaya başlayın.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-anomaly-finder-api-using-php"></a>Anomali noktaları PHP kullanarak Anomali Bulucu API'sini kullanmaya başlama
[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Zaman serisi verilerini örneği
Zaman serisi verilerini örneği aşağıdaki gibidir.
[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-php-example"></a>Verileri analiz etmek ve örnek PHP anomali puan Al
1. `[YOUR_SUBSCRIPTION_KEY]` değerini geçerli abonelik anahtarınızla değiştirin.
2. Değiştirin `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` örnek veya kendi veri noktaları.
3. Yürütme ve yanıtı kontrol edin.

```PHP
<?php
# This sample uses the Apache HTTP client from HTTP components (http://hc.apache.org/httpcomponents-client-ga/)
require_once 'HTTP/Request2.php';

$request = new HTTP_Request2('https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection');
$url = $request->getUrl();

$requestData = '[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]';

$headers = array(
    # Request headers
    'Content-Type' => 'application/json',
    # NOTE: Replace the "Ocp-Apim-Subscription-Key" value with a valid subscription key.
    'Ocp-Apim-Subscription-Key' => '[YOUR_SUBSCRIPTION_KEY]',
);

$request->setHeader($headers);

$request->setMethod(HTTP_Request2::METHOD_POST);

# Request body
$request->setBody($requestData);

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

### <a name="example-response"></a>Örnek yanıt

Başarılı bir yanıt JSON biçiminde döndürülür. Örnek yanıt aşağıdaki gibidir.
[!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [REST API başvurusu](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
