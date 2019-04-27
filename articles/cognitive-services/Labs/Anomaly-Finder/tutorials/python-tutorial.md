---
title: 'Öğretici: Anomali algılama ve Python'
titlesuffix: Azure Cognitive Services
description: Anomali Algılama API'sini kullanan bir Python notebook'u keşfedin. Özgün veri noktalarını API'ye gönderin ve beklenen değerle anomali noktalarını alın.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: tutorial
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: 5a2fb54658599e0500944aaae9225f314277f9da
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60838379"
---
# <a name="tutorial-anomaly-detection-with-python-application"></a>Öğretici: Python uygulaması ile anomali algılama

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

Bu öğreticide, Python’da Anomali Algılama API’sinin nasıl kullanılacağı ve popüler kitaplıklar kullanılarak sonuçlarınızın nasıl görselleştirileceği gösterilmektedir. Öğreticiyi çalıştırmak için Jupyter kullanın ve kendi abonelik anahtarınızı kullanarak elinizdeki verilerle deneme yapın. Etkileşimli Jupyter notebook’ları nasıl kullanmaya başlayacağınızı öğrenmek için [Jupyter Belgeleri](https://jupyter.readthedocs.io/en/latest/index.html)’ne bakın. 

## <a name="prerequisites"></a>Önkoşullar

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Anomali Algılama için abone olun ve abonelik anahtarını alın 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="download-the-example-code"></a>Örnek kodu indirin

1. [GitHub’da öğretici notebook](https://github.com/MicrosoftAnomalyDetection/python-sample)’una gidin.
2. Öğreticiyi kopyalayıp indirmek için yeşil düğmeye tıklayın. 

## <a name="opening-the-tutorial-notebook-in-jupyter"></a>Jupyter’de öğretici notebook’unu açma

1. Bir komut istemi açın ve python-sample klasörüne gidin.
2. Komut isteminden Jupyter notebook komutunu çalıştırarak Jupyter'in açılmasını sağlayın.
3. Jupyter penceresinde <em>Anomaly Detection API Example.ipynb</em> seçeneğine tıklayarak öğretici not defterini açın.   

## <a name="running-the-tutorial"></a>Öğreticiyi çalıştırma

Bu notebook'u kullanmak için, Anomali Algılama API’si için bir abonelik anahtarı gerekir. Kaydolmak için Abonelik sayfasını ziyaret edin. “Oturum aç” sayfasında oturum açmak için Microsoft hesabınızı kullanın, böylece abone olup anahtarlarınızı alabilirsiniz. Kayıt işlemini tamamladıktan sonra anahtarınızı not defterinin değişkenler bölümüne yapıştırın (aşağıda oluşturulmuştur). Birincil veya ikincil anahtar çalışır. Anahtarı dize haline getirmek için tırnak içine aldığınızdan emin olun.

```Python

            # Variables
            endpoint = 'https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection'
            subscription_key = None #Here you have to paste your primary key

```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [REST API başvurusu](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
