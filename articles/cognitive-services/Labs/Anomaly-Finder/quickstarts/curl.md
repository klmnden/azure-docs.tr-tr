---
title: Anomali Bulucu API cURL - Microsoft Bilişsel hizmetler ile kullanma | Microsoft Docs
description: Hızlı bir şekilde yardımcı olmak için bilgi alın, Bilişsel Hizmetler'e cURL ve Anomali Bulucu API'sini kullanmaya başlama.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: 7a4996c83d57b34fcfcff43650b21359acb4e65e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61094910"
---
# <a name="use-the-anomaly-finder-api-with-curl"></a>Anomali Bulucu API ile cURL kullanın.

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

Bu makalede bilgiler sağlanmaktadır ve kod örnekleri, hızlı bir şekilde yardımcı olmak için zaman serisi verilerini anomali sonucu alma görevi gerçekleştirmek için cURL ile Anomali Bulucu API'yi kullanarak kullanmaya başlayın.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-the-anomaly-finder-api-using-curl"></a>Anomali Bulucu cURL kullanarak API anomali noktalarını alma 

[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Zaman serisi verilerini örneği

Zaman serisi veri noktaları örneği aşağıdaki gibidir.

[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-curl-example"></a>Verileri analiz ve anomali noktaları cURL örnek alın

Örneği kullanarak adımlar aşağıdaki gibidir.

1. `[YOUR_SUBSCRIPTION_KEY]` değerini geçerli abonelik anahtarınızla değiştirin.
2. Değiştirin `[YOUR_REGION]` abonelik anahtarlarınızın aldığınız yere konumu kullanmak üzere.
3. Değiştirin `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` örnek veya kendi veri noktaları.
4. Yürütme ve yanıtı kontrol edin.

```cURL

curl -v -X POST "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection"
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: [YOUR_SUBSCRIPTION_KEY]"
--data-ascii "[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]" 

```

### <a name="example-response"></a>Örnek yanıt
Başarılı bir yanıt JSON biçiminde döndürülür. Örnek yanıt aşağıdaki gibidir: [!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [REST API başvurusu](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
