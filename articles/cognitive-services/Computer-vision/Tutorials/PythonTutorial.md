---
title: Görüntü işlemleri - Python
titlesuffix: Azure Cognitive Services
description: Jupyter notebook’ları kullanarak Python ile Görüntü İşleme API’sinin nasıl kullanılacağını öğrenin. Popüler kitaplıkları kullanarak sonuçlarınızı görselleştirin.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 07/03/2019
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: b18f41db772c1c214bdf48c9bb765b1ce7e6d2c2
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604150"
---
# <a name="computer-vision-api-jupyter-notebook"></a>Bilgisayar görüntü işleme API'si Jupyter not defteri

Bu kılavuzda, Python görüntü işleme API'sini kullanmayı ve popüler kitaplıklarını kullanarak, sonuçların görselleştirilmesi nasıl gösterir. Öğreticiyi çalıştırmak için Jupyter kullanacaksınız. Etkileşimli Jupyter not defterleri ile çalışmaya başlama konusunda bilgi için bkz [Jupyter belgeleri](https://jupyter.readthedocs.io/en/latest/index.html).

## <a name="prerequisites"></a>Önkoşullar

- [Python 2.7+ veya 3.5+](https://www.python.org/downloads/)
- [pip](https://pip.pypa.io/en/stable/installing/) aracı
- [Jupyter Notebook](https://jupyter.org/install) yüklü

## <a name="open-the-notebook-in-jupyter"></a>İçinde Jupyter not defterini açın 

1. [Cognitive Vision Python](https://github.com/Microsoft/Cognitive-Vision-Python) GitHub deposuna gidin. 
2. Depoyu kopyalamak veya indirmek için yeşil düğmeye tıklayın. 
3. Bir komut istemi açın ve **Cognitive-Vision-Python\Jupyter Notebook** klasörüne gidin.
1. Komut isteminden `pip install requests opencv-python numpy matplotlib` komutunu çalıştırarak gerekli tüm kitaplıklara sahip olduğunuzdan emin olun.
1. Komut isteminden `jupyter notebook` komutunu çalıştırarak Jupyter’i başlatın.
1. Jupyter penceresinde _Computer Vision API Example.ipynb_ seçeneğine tıklayarak öğretici not defterini açın.

## <a name="run-the-notebook"></a>Not defterini çalıştırma

Bu not defterini kullanmak için, Görüntü İşleme API’si için bir abonelik anahtarı gerekir. Kaydolmak için [Abonelik sayfasını](https://azure.microsoft.com/try/cognitive-services/) ziyaret edin. **Oturum aç** sayfasında oturum açmak için Microsoft hesabınızı kullanın, böylece abone olup ücretsiz anahtar alabilirsiniz. Kayıt işlemini tamamladıktan sonra anahtarınızı not defterinin `Variables` bölümüne yapıştırın (aşağıda oluşturulmuştur). Birincil veya ikincil anahtar çalışacaktır. Anahtarı dize haline getirmek için tırnak içine aldığınızdan emin olun.

Ayrıca `_region` alanının aboneliğinize karşılık gelen bölgeyle eşleştiğinden de emin olmanız gerekir.

```python
# Variables
_region = 'westcentralus'  # Here you enter the region of your subscription
_url = 'https://{}.api.cognitive.microsoft.com/vision/v2.0/analyze'.format(
    _region)
_key = None  # Here you have to paste your primary key
_maxNumRetries = 10
```

Öğreticiyi çalıştırdığınızda analiz etmek için hem URL’den hem de yerel depolama alanından görüntüleri eklemeniz mümkün olur. Betik, görüntüleri ve analiz bilgilerini not defterinde görüntüler.
