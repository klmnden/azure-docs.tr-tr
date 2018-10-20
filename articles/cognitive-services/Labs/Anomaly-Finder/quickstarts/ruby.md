---
title: Anomali Bulucu API Ruby - Microsoft Bilişsel hizmetler ile kullanma | Microsoft Docs
description: Hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get, Ruby ve Anomali Bulucu API Bilişsel hizmetler kullanarak başlayın.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.component: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: 97b02bfc2e388757306ae6180cf66f6870437218
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49457638"
---
# <a name="use-the-anomaly-finder-api-with-ruby"></a>Anomali Bulucu API ile Ruby kullanma

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

Bu makalede bilgiler sağlanmaktadır ve kod örnekleri, hızlı bir şekilde yardımcı olması için anomali algılama sonucu zaman serisi verilerini alma görevi ile Ruby Anomali Bulucu API'sini kullanarak kullanmaya başlayın.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-anomaly-finder-api-using-ruby"></a>Anomali noktaları Ruby kullanarak Anomali Bulucu API'sini kullanmaya başlama 
[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Zaman serisi verilerini örneği
Zaman serisi veri noktaları örneği aşağıdaki gibidir,

[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-ruby-example"></a>Verileri analiz etmek ve Ruby örnek anomali puan Al

Örneği kullanarak adımlar aşağıdaki gibidir.

1. Yükleme [rest istemcisi](https://github.com/rest-client/rest-client) 'rest-istemci yükleme gem' çalıştırarak.
2. Aşağıdaki kod .rb dosya olarak kaydedin.
3. `[YOUR_SUBSCRIPTION_KEY]` değerini geçerli abonelik anahtarınızla değiştirin.
4. Değiştirin `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` örnek veya kendi veri noktaları.
5. Yürütme ve yanıtı kontrol edin.

```ruby
# https://github.com/rest-client/rest-client
require 'rest_client'

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the subscriptionKey string value with your valid subscription key.
subscription_key = '[YOUR_SUBSCRIPTION_KEY]';

endpoint = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection";

# Replace the request data with your real data.
requestData = '[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]';

header = {
    :content_type => 'application/json',
    :'Ocp-Apim-Subscription-Key' => subscription_key
}

response = RestClient::Request.execute(
    :url => endpoint,
    :method => :post,
    :verify_ssl => true,
    :payload => requestData,
    :header => header)

# You will see the response with anomaly results
puts response.body
```

### <a name="example-response"></a>Örnek yanıt

Başarılı bir yanıt JSON biçiminde döndürülür. Örnek yanıt aşağıdaki gibidir.
[!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [REST API başvurusu](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
