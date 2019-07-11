---
title: Zaman serisi verilerinizle Anomali algılayıcısı API kullanma
titleSuffix: Azure Cognitive Services
description: Toplu olarak veya akış verileri üzerinde verilerinizdeki anormallikleri algılamak öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: article
ms.date: 03/26/2019
ms.author: aahi
ms.openlocfilehash: 551196815004cb047680e2ae2f8dbe32186c1a0c
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67721801"
---
# <a name="how-to-use-the-anomaly-detector-api-on-your-time-series-data"></a>Nasıl yapılır: Zaman serisi verilerinizle Anomali algılayıcısı API kullanın  

[Anomali algılayıcısı API](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect) anomali algılama için iki yöntem sunar. Ya da bir toplu iş olarak zamanlarınızı algılayabilir serisi ya verilerinizi en son veri noktası durumunu anomali algılama tarafından oluşturulur. Algılama modelini anomali sonuçları yanı sıra her veri noktasının beklenen değeri ve üst ve alt anomali algılama sınırları döndürür. Bu değerler, bir dizi normal değerleri veya verilerdeki anomaliler görselleştirmek için kullanabilirsiniz.

## <a name="anomaly-detection-modes"></a>Anomali algılama modları 

Algılama modu Anomali algılayıcısı API'sini sağlar: toplu işlem ve akış.

> [!NOTE]
> Aşağıdaki İstek URL'leri, aboneliğiniz için uygun uç nokta ile birleştirilmelidir. Örneğin, `https://westus2.api.cognitive.microsoft.com/anomalydetector/v1.0/timeseries/entire/detect`


### <a name="batch-detection"></a>Batch algılama

Belirtilen zaman aralığı üzerinde veri noktaları toplu boyunca anomalileri algılamak için aşağıdaki istek URI'si ile zaman serisi verilerinin kullanın: 

`/timeseries/entire/detect`. 

Zaman serisi verilerinizi aynı anda göndererek API tüm seri kullanarak bir model oluşturmak ve birlikte her bir veri noktası analiz edin.  

### <a name="streaming-detection"></a>Akış algılama

Sürekli olarak veri akışı üzerinde anomalileri algılamak için en son veri noktasıyla aşağıdaki istek URI'si kullanın: 

`/timeseries/last/detect'`. 

Bunları oluştururken yeni veri noktaları göndererek verilerinizi gerçek zamanlı olarak izleyebilirsiniz. Gönderdiğiniz veri noktalarıyla bir model oluşturulur ve bu API zaman serisi en son tarihli bir anomali olup olmadığını belirler.

## <a name="adjusting-lower-and-upper-anomaly-detection-boundaries"></a>Alt ve üst anomali algılama sınırlarını ayarlama

Varsayılan olarak, anomali algılama için üst ve alt sınırları kullanılarak hesaplanır `expectedValue`, `upperMargin`, ve `lowerMargin`. Farklı sınırları gerekiyorsa, uygulama öneririz bir `marginScale` için `upperMargin` veya `lowerMargin`. Sınırları şu şekilde hesaplanır:

|Sınır  |Hesaplama  |
|---------|---------|
|`upperBoundary` | `expectedValue + (100 - marginScale) * upperMargin`        |
|`lowerBoundary` | `expectedValue - (100 - marginScale) * lowerMargin`        |

Aşağıdaki örnekler farklı sensitivities bir Anomali algılayıcısı API sonucunu göstermektedir.

### <a name="example-with-sensitivity-at-99"></a>Duyarlılık 99 adresindeki örneğiyle

![Varsayılan duyarlılık](../media/sensitivity_99.png)

### <a name="example-with-sensitivity-at-95"></a>Duyarlılık 95 adresindeki örneğiyle

![99 duyarlılık](../media/sensitivity_95.png)

### <a name="example-with-sensitivity-at-85"></a>Duyarlılık 85 adresindeki örneğiyle

![85 duyarlılık](../media/sensitivity_85.png)

## <a name="next-steps"></a>Sonraki Adımlar

* [Anomali algılayıcısı API nedir?](../overview.md)
* [Hızlı Başlangıç: Anomali algılayıcısı REST API kullanarak, zaman serisi verilerinde görülen anomalileri algılayın](../quickstarts/detect-data-anomalies-csharp.md)