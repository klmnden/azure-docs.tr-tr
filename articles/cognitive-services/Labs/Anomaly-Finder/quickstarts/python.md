---
title: Python - Microsoft Bilişsel hizmetler Anomali Bulucu API kullanma | Microsoft Docs
description: Hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get Anomali Bulucu Bilişsel hizmetler Python ile kullanmaya başlayın.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: c14916b0644edab613b298d6e71f8bbb9a6bb804
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353309"
---
# <a name="use-the-anomaly-finder-api-with-python"></a>Anomali Bulucu API Python ile kullanma

Bu makalede bilgiler sağlanmaktadır ve hızla yardımcı olmak için kod örnekleri Anomali Bulucu API ile Python anomali sonucu için zaman serisi veri alma görevi gerçekleştirmek için kullanmaya başlayın.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-anomaly-finder-api-using-python"></a>Python kullanarak Anomali Bulucu API'si ile anomali noktalarını alma 

[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Zaman serisi veri örneği

Zaman serisi veri noktaları örneği aşağıdaki gibidir.

[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-python-example"></a>Verileri çözümlemek ve anomali noktaları Python örnek alma

Python3 yükledikten sonra detect.py adlı bir python yürütülebilir dosyası oluşturun emin olun. Detect.PY içinde aşağıdaki kodu içermelidir. Kod çalıştırmadan önce değiştirmek unutmayın `[YOUR_SUBSCRIPTION_KEY]` değeri geçerli bir abonelik anahtarınızı ile.
Değiştir `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` veri noktaları.

```python
import requests
import json


def detect(url, subscription_key, request_data):
    headers = {'Content-Type': 'application/json', 'Ocp-Apim-Subscription-Key': subscription_key}
    response = requests.post(url, data=json.dumps(request_data), headers=headers)
    if response.status_code == 200:
        return json.loads(response.content.decode("utf-8"))
    else:
        print(response.status_code)
        raise Exception(response.text)


sample_data = "[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]"
endpont = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection"
subscription_key = "[YOUR_SUBSCRIPTION_KEY]"

result = detect(endpont, subscription_key, sample_data)
print(result)

```

### <a name="example-response"></a>Örnek yanıt

Başarılı yanıt JSON döndürülür. Örnek yanıt aşağıdaki gibidir.
[!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Python uygulaması](../tutorials/python-tutorial.md)
