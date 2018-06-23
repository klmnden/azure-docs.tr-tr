---
title: Bilgisayar görme API Python Eğitmen | Microsoft Docs
description: Microsoft Bilişsel Hizmetleri'nde Jupyter not defterlerini kullanarak bilgisayar görme API Python ile kullanmayı öğrenin. Popüler kitaplıkları kullanarak sonuçları görselleştirebilirsiniz.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 02/25/2017
ms.author: kefre
ms.openlocfilehash: a093c2d066e70a8daf1fe1cd33ccf794ecb196af
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352378"
---
# <a name="computer-vision-api-python-tutorial"></a>Bilgisayar görme API Python Eğitmen

Bu öğretici, Python bilgisayar görme API kullanma ve bazı yaygın kitaplıkları kullanarak sonuçlarınızı görselleştirmek nasıl gösterir. Jupyter öğretici çalıştırmak için kullanın. Etkileşimli Jupyter not defterleri ile çalışmaya başlama öğrenmek için bkz: [Jupyter Documementation](http://jupyter.readthedocs.io/en/latest/index.html). 

### <a name="opening-the-tutorial-notebook-in-jupyter"></a>Eğitmen dizüstü Jupyter'de açma 

1. Gidin [github'da öğretici dizüstü](https://github.com/Microsoft/Cognitive-Vision-Python). 
2. Kopyalama veya öğreticiyi indirmek için yeşil düğmeyi tıklatın. 
3. Bir komut istemi açın ve klasöre gidin _görme Python master\Jupyter Cognitive dizüstü_. 
4. Komutu çalıştırın **jupyter not defteri** komut isteminden. Bu Jupyter başlatır.
5. Jupyter penceresinde tıklayın _bilgisayar görme API Example.ipynb_ öğretici not defterini açmak için 

### <a name="running-the-tutorial"></a>Öğretici çalıştırma

Bu not defterini kullanmak için bilgisayar görme API için bir abonelik anahtarı gerekir. Ziyaret [aboneliği sayfasına](https://azure.microsoft.com/try/cognitive-services/) kaydolmak için. "Oturum açma" sayfasında, Microsoft hesabınızda oturum açmak için kullandığınız ve abone olmak ve ücretsiz anahtarlarını almak mümkün olacaktır. Kayıt işlemini tamamladıktan sonra anahtarınızı (aşağıda çoğaltılamaz) not defteri değişkenleri bölümünde yapıştırın. Birincil veya ikincil anahtarı çalışır. Anahtar bir dize yapmak için tırnak içine aldığınızdan emin olun.

```python
# Variables

_url = 'https://westcentralus.api.cognitive.microsoft.com/vision/v1/analyses'
_key = None #Here you have to paste your primary key
_maxNumRetries = 10
```
