---
title: Anomali algılama Python uygulaması - Microsoft Bilişsel hizmetler | Microsoft Docs
description: Microsoft Bilişsel Hizmetleri'nde Anomali algılama API'sini kullanan bir Python not defteri keşfedin. API için özgün veri noktaları göndermek ve beklenen değer ve anomali noktaları alabilirsiniz.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: d35f41ddab21aa155376ad52ff4084298dab8fc5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353938"
---
# <a name="anomaly-detection-python-application"></a>Anomali algılama Python uygulama

Öğretici, Python içinde Anomali algılama API kullanmayı ve popüler kitaplıkları kullanarak sonuçlarınızı görselleştirmek nasıl gösterir. Öğretici çalıştırmak için Jupyter kullanarak ve kendi verilerinizi abonelik anahtarınızı ile çalışıyor. Etkileşimli Jupyter not defterleri ile çalışmaya başlama öğrenmek için bkz [Jupyter belgelerine](http://jupyter.readthedocs.io/en/latest/index.html). 

## <a name="prerequisites"></a>Önkoşullar

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Anomali algılama abone olma ve aboneliği anahtarı alma 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="download-the-example-code"></a>Örnek kodu indirme

1. Gidin [github'da öğretici dizüstü](https://github.com/MicrosoftAnomalyDetection/python-sample).
2. Kopyalama veya öğreticiyi indirmek için yeşil düğmeyi tıklatın. 

## <a name="opening-the-tutorial-notebook-in-jupyter"></a>Eğitmen dizüstü Jupyter'de açma

1. Bir komut istemi açın ve klasör python örneğine gidin.
2. Komut Jupyter not defteri Jupyter başlayacak komut istemi çalıştırın.
3. Jupyter penceresinde tıklayın <em>Anomali algılama API Example.ipynb</em> öğretici not defteri açın.   

## <a name="running-the-tutorial"></a>Öğretici çalıştırma

Bu not defterini kullanmak için Anomali algılama API abonelik anahtarı gerekir. Kaydolmak için abonelik sayfasını ziyaret edin. "Oturum açma" sayfasında, Microsoft hesabınızda oturum açmak için kullandığınız ve abone olmak ve anahtarlarınızı almak mümkün olacaktır. Kayıt işlemini tamamladıktan sonra anahtarınızı (aşağıda çoğaltılamaz) not defteri değişkenleri bölümünde yapıştırın. Birincil veya ikincil anahtarı çalışır. Anahtar bir dize yapmak için tırnak içine aldığınızdan emin olun.

```Python

            # Variables
            endpoint = 'https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection'
            subscription_key = None #Here you have to paste your primary key

```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [REST API başvurusu](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
