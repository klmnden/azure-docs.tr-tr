---
title: 'Öğretici: Görüntü İşleme API’si Python'
titlesuffix: Azure Cognitive Services
description: Jupyter notebook’ları kullanarak Python ile Görüntü İşleme API’sinin nasıl kullanılacağını öğrenin. Popüler kitaplıkları kullanarak sonuçlarınızı görselleştirin.
services: cognitive-services
author: KellyDF
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: tutorial
ms.date: 02/25/2017
ms.author: kefre
ms.openlocfilehash: 046250d3d2142badaac35490eff27bcac220fea9
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49344906"
---
# <a name="tutorial-computer-vision-api-python"></a>Öğretici: Görüntü İşleme API’si Python

Bu öğreticide, Python’da Görüntü İşleme API’sinin nasıl kullanılacağı ve bazı popüler kitaplıklar kullanılarak sonuçlarınızın nasıl görselleştirileceği gösterilmektedir. Öğreticiyi çalıştırmak için Jupyter kullanın. Etkileşimli Jupyter notebook’ları nasıl kullanmaya başlayacağınızı öğrenmek için [Jupyter Belgeleri](http://jupyter.readthedocs.io/en/latest/index.html)’ne bakın. 

## <a name="open-the-tutorial-notebook-in-jupyter"></a>Öğretici Notebook'unu Jupyter’de Açma 

1. [GitHub’da öğretici notebook](https://github.com/Microsoft/Cognitive-Vision-Python)’una gidin. 
2. Öğreticiyi kopyalayıp indirmek için yeşil düğmeye tıklayın. 
3. Bir komut istemini açın ve _Cognitive-Vision-Python-master\Jupyter Notebook_ klasörüne gidin. 
4. Komut isteminden **jupyter notebook** komutunu çalıştırın. Böylece Jupyter başlatılır.
5. Jupyter penceresinde _Computer Vision API Example.ipynb_ seçeneğine tıklayarak öğretici not defterini açın 

## <a name="run-the-tutorial"></a>Öğreticiyi Çalıştırma

Bu not defterini kullanmak için, Görüntü İşleme API’si için bir abonelik anahtarı gerekir. Kaydolmak için [Abonelik sayfasını](https://azure.microsoft.com/try/cognitive-services/) ziyaret edin. “Oturum aç” sayfasında oturum açmak için Microsoft hesabınızı kullanın, böylece abone olup ücretsiz anahtar alabilirsiniz. Kayıt işlemini tamamladıktan sonra anahtarınızı not defterinin değişkenler bölümüne yapıştırın (aşağıda oluşturulmuştur). Birincil veya ikincil anahtar çalışır. Anahtarı dize haline getirmek için tırnak içine aldığınızdan emin olun.

```python
# Variables

_url = 'https://westcentralus.api.cognitive.microsoft.com/vision/v1/analyses'
_key = None #Here you have to paste your primary key
_maxNumRetries = 10
```
