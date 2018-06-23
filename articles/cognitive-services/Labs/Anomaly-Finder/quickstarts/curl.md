---
title: CURL - Microsoft Bilişsel hizmetler Anomali Bulucu API kullanma | Microsoft Docs
description: Get bilgilerini hızla yardımcı olmak için cURL ve Anomali Bulucu API Bilişsel Hizmetleri'ni kullanarak Başlarken.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: 3c1d791b8c0478715b4ffa93cd7dfa43f9be4586
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353315"
---
# <a name="use-the-anomaly-finder-api-with-curl"></a>CURL ile Anomali Bulucu API kullanın

Bu makalede bilgiler sağlar ve hızla yardımcı olmak için kod örnekleri Anomali Bulucu API ile cURL anomali sonucu zaman serisi veri alma görevi gerçekleştirmek için kullanmaya başlayın.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-the-anomaly-finder-api-using-curl"></a>Anomali noktaları Anomali Bulucu cURL kullanarak API ile Başlarken 

[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Zaman serisi veri örneği

Zaman serisi veri noktaları örneği aşağıdaki gibidir.

[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-curl-example"></a>Verileri çözümlemek ve anomali noktaları cURL örnek alma

Örneği kullanarak adımlar aşağıdaki gibidir.

1. Değiştir `[YOUR_SUBSCRIPTION_KEY]` değeri geçerli bir abonelik anahtarınızı ile.
2. Değiştir `[YOUR_REGION]` elde burada abonelik anahtarlarınızı konumu kullanmak üzere.
3. Değiştir `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` örnek veya kendi veri noktaları.
4. Yürütme ve yanıt denetleyin.

```cURL

curl -v -X POST "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection"
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: [YOUR_SUBSCRIPTION_KEY]"
--data-ascii "[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]" 

```

### <a name="example-response"></a>Örnek yanıt
Başarılı yanıt JSON döndürülür. Örnek yanıt aşağıdaki gibidir: [!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [REST API başvurusu](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
