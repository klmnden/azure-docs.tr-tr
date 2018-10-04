---
title: Anomali algılama Python uygulaması - Microsoft Bilişsel hizmetler | Microsoft Docs
description: Microsoft Bilişsel hizmetler Anomali algılama API'sini kullanan bir Python not defteri keşfedin. Özgün veri noktaları API'ye gönderin ve beklenen değerini ve anomali noktalarını alın.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: 87cd9e976d231291ad13acecf188cfd668d692b6
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248236"
---
# <a name="anomaly-detection-python-application"></a>Anomali algılama Python uygulaması

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

Öğretici Python'da Anomali algılama API'sini kullanmayı ve popüler kitaplıklarını kullanarak, sonuçların görselleştirilmesi nasıl gösterir. Öğreticiyi çalıştırmak için Jupyter kullanarak ve kendi verilerinizi abonelik anahtarınız ile çalışıyor. Etkileşimli Jupyter not defterleri ile çalışmaya başlama konusunda bilgi için bkz [Jupyter belgeleri](http://jupyter.readthedocs.io/en/latest/index.html). 

## <a name="prerequisites"></a>Önkoşullar

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Anomali algılama için abone ve bir abonelik anahtarı edinirler 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="download-the-example-code"></a>Örnek kodu indirin

1. Gidin [github'da öğretici not defteri](https://github.com/MicrosoftAnomalyDetection/python-sample).
2. Kopyala veya indir öğreticisi için yeşil düğmesine tıklayın. 

## <a name="opening-the-tutorial-notebook-in-jupyter"></a>Jupyter'de öğretici not defterini açarak

1. Bir komut istemi açın ve klasör python örneğine gidin.
2. Komut Jupyter not defteri Jupyter başlatacak komut istemi çalıştırın.
3. Jupyter penceresinde tıklayarak <em>Anomali algılama API'si Example.ipynb</em> öğretici not defterini açın.   

## <a name="running-the-tutorial"></a>Öğreticiyi çalıştırdıktan

Bu not defterini kullanma Anomali algılama API'si için bir abonelik anahtarı gerekir. Kaydolmak için abonelik sayfasını ziyaret edin. "Oturum açma" sayfasında, Microsoft hesabınızda oturum açarken kullandığınız ve abone olma ve anahtarlarınızı almak mümkün olacaktır. Kayıt işlemini tamamladıktan sonra değişkenler bölümü (aşağıda çoğaltılamaz) not defterinin anahtarınızı yapıştırın. Birincil veya ikincil anahtarı çalışır. Anahtar bir dize olmak için tırnak içine aldığınızdan emin olun.

```Python

            # Variables
            endpoint = 'https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection'
            subscription_key = None #Here you have to paste your primary key

```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [REST API başvurusu](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
