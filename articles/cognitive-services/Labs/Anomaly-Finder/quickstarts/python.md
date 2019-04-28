---
title: Anomali Bulucu API Python - Microsoft Bilişsel hizmetler ile kullanma | Microsoft Docs
description: Anomali Bulucu, Bilişsel hizmetler, Python kullanarak hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get başlayın.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: df894fee3328b2199b8f7436adabfd3db62f4004
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61217301"
---
# <a name="use-the-anomaly-finder-api-with-python"></a>Anomali Bulucu API Python ile kullanma

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

Bu makalede bilgiler sağlanmaktadır ve kod örnekleri, hızlı bir şekilde yardımcı olması için anomali sonuç için zaman serisi verilerini alma görevi gerçekleştirmek için Python ile Anomali Bulucu API'yi kullanarak kullanmaya başlayın.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-anomaly-finder-api-using-python"></a>Anomali noktaları Anomali Bulucu kullanarak Python API'sini kullanmaya başlama 

[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Zaman serisi verilerini örneği

Zaman serisi veri noktaları örneği aşağıdaki gibidir.

[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-python-example"></a>Verileri analiz ve anomali noktaları Python örnek alın

Python3 yükledikten sonra detect.py adlı bir python yürütülebilir bir dosya oluşturun emin olun. Detect.PY içinde aşağıdaki kodu içermesi gerekir. Kodu yürütmeden önce değiştirmeyi unutmayın `[YOUR_SUBSCRIPTION_KEY]` değeri geçerli bir abonelik.
Değiştirin `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` , veri noktalarıyla.

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

Başarılı bir yanıt JSON biçiminde döndürülür. Örnek yanıt aşağıdaki gibidir.
[!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Python uygulaması](../tutorials/python-tutorial.md)
