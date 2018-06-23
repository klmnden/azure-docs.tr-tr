---
title: Duygu tanıma API'si Python Eğitmen | Microsoft Docs
description: Bilişsel hizmetler duygu tanıma API'si Python ile kullanma hakkında bilgi edinmek için Jupyter not defteri kullanın. Sonuçlarınızı popüler kitaplıklarını kullanarak görselleştirin.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 05/23/2017
ms.author: anroth
ms.openlocfilehash: 70c8ca48c651601f3d7cbb3717c32bfe112176fe
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352397"
---
# <a name="emotion-api-using-python-tutorial"></a>Python Eğitmen kullanarak API duygu tanıma

> [!IMPORTANT]
> Video API Önizleme 30 Ekim 2017 sona erer. Yeni deneyin [Video dizin oluşturucu API önizlemesi](https://azure.microsoft.com/services/cognitive-services/video-indexer/) kolayca videoların öngörüleri ayıklamak ve konuşulan sözcüklerin, yüzler, karakterler ve duygular algılayarak arama sonuçları gibi içerik bulma deneyimlerini geliştirmek üzere. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

Duygu tanıma API'si ile çalışmaya başlama kolaylaştırmak için aşağıda bağlı Jupyter Not Defteri, Python API kullanma ve bazı yaygın kitaplıkları kullanarak sonuçlarınızı görselleştirmek nasıl gösterir. 

[Not Defteri github'da bağlantı](https://github.com/Microsoft/Cognitive-Emotion-Python/blob/master/Jupyter%20Notebook/Emotion%20Analysis%20Example.ipynb)

### <a name="using-the-jupyter-notebook"></a>Jupyter Not Defteri kullanarak

Not Defteri etkileşimli olarak kullanabilmek için kopyalayın ve Jupyter'de çalıştırılması gerekir. Etkileşimli Jupyter not defterleri ile çalışmaya başlama öğrenmek için yönergeleri izleyin http://jupyter.readthedocs.org/en/latest/install.html. 

Bu not defterini kullanın duygu tanıma API'si için bir abonelik anahtarı gerekir. Ziyaret [aboneliği sayfasına](https://azure.microsoft.com/try/cognitive-services/) kaydolmak için. "Oturum açma" sayfasında, Microsoft hesabınızda oturum açmak için kullandığınız ve abone olmak ve ücretsiz anahtarlarını almak mümkün olacaktır. Kayıt işlemini tamamladıktan sonra anahtarınızı aşağıda gösterilen değişkenler bölümü yapıştırın. Birincil veya ikincil anahtarı çalışır.

```
Python Example 

#Variables

_url = 'https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize'
_key = None #Here you have to paste your primary key
_maxNumRetries = 10

```
