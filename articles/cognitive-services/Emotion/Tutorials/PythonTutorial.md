---
title: "Öğretici: Görüntünün - duygu tanıma API'si, Python yüz temel duyguları tanıma"
titlesuffix: Azure Cognitive Services
description: Duygu Tanıma API'sini Python ile kullanmayı öğrenmek için bir Jupyter notebook kullanın. Popüler kitaplıkları kullanarak sonuçlarınızı görselleştirin.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: tutorial
ms.date: 05/23/2017
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 9e03e5d534fa741def2010a21120edc3d7169964
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56237476"
---
# <a name="tutorial-use-the-emotion-api-with-a-jupyter-notebook--python"></a>Öğretici: Duygu tanıma API'si, bir Jupyter not defteri ve Python ile kullanın.

> [!IMPORTANT]
> Duygu Tanıma API'si 15 Şubat 2019 tarihinde kullanım dışı bırakılacaktır. Duygu tanıma özelliği [Yüz Tanıma API'sinin](https://docs.microsoft.com/azure/cognitive-services/face/) bir parçası olarak genel kullanıma sunulmuştur. 

Duygu Tanıma API'sini kullanmaya başlamanızı kolaylaştırmak için aşağıda bağlantısı verilen Jupyter notebook'ta API'yi Python'da kullanma ve bazı popüler kitaplıkları kullanarak sonuçlarınızı görselleştirme adımları gösterilmektedir.

[GitHub'daki notebook'un bağlantısı](https://github.com/Microsoft/Cognitive-Emotion-Python/blob/master/Jupyter%20Notebook/Emotion%20Analysis%20Example.ipynb)

### <a name="using-the-jupyter-notebook"></a>Jupyter Notebook'u kullanma

Notebook'u etkileşimli bir şekilde kullanmak için kopyalayıp Jupyter'de çalıştırmanız gerekir. Etkileşimli Jupyter notebook’ları nasıl kullanmaya başlayacağınızı öğrenmek için http://jupyter.readthedocs.org/en/latest/install.html sayfasındaki yönergeleri izleyin.

Bu not defterini kullanmak için, Duygu Tanıma API'si için bir abonelik anahtarı gerekir. Kaydolmak için [Abonelik sayfasını](https://azure.microsoft.com/try/cognitive-services/) ziyaret edin. “Oturum aç” sayfasında oturum açmak için Microsoft hesabınızı kullanın, böylece abone olup ücretsiz anahtar alabilirsiniz. Kayıt işlemini tamamladıktan sonra anahtarınızı aşağıda gösterilen bölüme yapıştırın. Birincil veya ikincil anahtar çalışır.

```
Python Example

#Variables

_url = 'https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize'
_key = None #Here you have to paste your primary key
_maxNumRetries = 10

```
