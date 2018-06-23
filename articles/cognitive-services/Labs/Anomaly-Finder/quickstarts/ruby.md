---
title: Ruby - Microsoft Bilişsel hizmetler Anomali Bulucu API kullanma | Microsoft Docs
description: Hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get Ruby ve Anomali Bulucu API Bilişsel Hizmetleri'nde kullanmaya başlayın.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: ca4754514ba5012f7e9e28981d0869d174561fb3
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353326"
---
# <a name="use-the-anomaly-finder-api-with-ruby"></a>Anomali Bulucu API ile Söyleniş kullanın

Bu makalede bilgiler sağlanmaktadır ve hızlı bir şekilde yardımcı olmak için kod örnekleri Anomali Bulucu API ile Söyleniş anomali algılama sonucu zaman serisi veri alma görevi gerçekleştirmek için kullanmaya başlayın.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-anomaly-finder-api-using-ruby"></a>Anomali noktaları Ruby kullanarak Anomali Bulucu API'si ile Başlarken 
[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Zaman serisi veri örneği
Zaman serisi veri noktaları örneği aşağıdaki gibidir, [!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-ruby-example"></a>Verileri çözümlemek ve anomali noktaları Söyleniş örnek alma

Örneği kullanarak adımlar aşağıdaki gibidir.

1. Yükleme [rest istemcisi](https://github.com/rest-client/rest-client) 'yükleme rest-istemci gem' çalıştırarak.
2. Kod .rb dosyası olarak kaydedin.
3. Değiştir `[YOUR_SUBSCRIPTION_KEY]` değeri geçerli bir abonelik anahtarınızı ile.
4. Değiştir `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` örnek veya kendi veri noktaları.
5. Yürütme ve yanıt denetleyin.

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

Başarılı yanıt JSON döndürülür. Örnek yanıt aşağıdaki gibidir.
[!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [REST API başvurusu](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
