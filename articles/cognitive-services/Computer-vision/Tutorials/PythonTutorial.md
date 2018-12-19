---
title: 'Öğretici: Görüntü işlemleri - Python'
titlesuffix: Azure Cognitive Services
description: Jupyter notebook’ları kullanarak Python ile Görüntü İşleme API’sinin nasıl kullanılacağını öğrenin. Popüler kitaplıkları kullanarak sonuçlarınızı görselleştirin.
services: cognitive-services
author: KellyDF
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: tutorial
ms.date: 11/06/2018
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: 55e5168c36eb16944142c2deaae9bc94d60d2fe4
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2018
ms.locfileid: "53582023"
---
# <a name="tutorial-computer-vision-api-python"></a>Öğretici: Bilgisayar görüntü işleme API'si Python

Bu öğreticide, Python’da Görüntü İşleme API’sinin nasıl kullanılacağı ve popüler kitaplıklar kullanılarak sonuçlarınızın nasıl görselleştirileceği gösterilir. Öğreticiyi çalıştırmak için Jupyter kullanacaksınız. Etkileşimli Jupyter notebook’larını nasıl kullanmaya başlayacağınızı öğrenmek için [Jupyter Belgeleri](https://jupyter.readthedocs.io/en/latest/index.html)’ne bakın.

## <a name="prerequisites"></a>Önkoşullar

- [Python 2.7+ veya 3.5+](https://www.python.org/downloads/)
- [pip](https://pip.pypa.io/en/stable/installing/) aracı
- [Jupyter Notebook](https://jupyter.org/install) yüklü

## <a name="open-the-tutorial-notebook-in-jupyter"></a>Öğretici Notebook'unu Jupyter’de Açma 

1. [Cognitive Vision Python](https://github.com/Microsoft/Cognitive-Vision-Python) GitHub deposuna gidin. 
2. Depoyu kopyalamak veya indirmek için yeşil düğmeye tıklayın. 
3. Bir komut istemi açın ve **Cognitive-Vision-Python\Jupyter Notebook** klasörüne gidin.
1. Komut isteminden `pip install requests opencv-python numpy matplotlib` komutunu çalıştırarak gerekli tüm kitaplıklara sahip olduğunuzdan emin olun.
1. Komut isteminden `jupyter notebook` komutunu çalıştırarak Jupyter’i başlatın.
1. Jupyter penceresinde _Computer Vision API Example.ipynb_ seçeneğine tıklayarak öğretici not defterini açın.

## <a name="run-the-tutorial"></a>Öğreticiyi Çalıştırma

Bu not defterini kullanmak için, Görüntü İşleme API’si için bir abonelik anahtarı gerekir. Kaydolmak için [Abonelik sayfasını](https://azure.microsoft.com/try/cognitive-services/) ziyaret edin. **Oturum aç** sayfasında oturum açmak için Microsoft hesabınızı kullanın, böylece abone olup ücretsiz anahtar alabilirsiniz. Kayıt işlemini tamamladıktan sonra anahtarınızı not defterinin `Variables` bölümüne yapıştırın (aşağıda oluşturulmuştur). Birincil veya ikincil anahtar çalışacaktır. Anahtarı dize haline getirmek için tırnak içine aldığınızdan emin olun.

Ayrıca `_region` alanının aboneliğinize karşılık gelen bölgeyle eşleştiğinden de emin olmanız gerekir.

```python
# Variables
_region = 'westcentralus' #Here you enter the region of your subscription
_url = 'https://{}.api.cognitive.microsoft.com/vision/v2.0/analyze'.format(_region)
_key = None #Here you have to paste your primary key
_maxNumRetries = 10
```

Öğreticiyi çalıştırdığınızda analiz etmek için hem URL’den hem de yerel depolama alanından görüntüleri eklemeniz mümkün olur. Betik, görüntüleri ve analiz bilgilerini not defterinde görüntüler.