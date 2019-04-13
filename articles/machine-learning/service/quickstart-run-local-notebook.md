---
title: Kendi not defteri sunucusunda bir not defteri çalıştırma hızlı başlangıcı
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti ile çalışmaya başlama. Çalışma alanınızda denemek için kendi yerel notebook sunucusu kullanın.  Çalışma alanınızda denemeler, eğitmek ve makine öğrenimi modelleri dağıtmak için kullandığınız bulutta temel taşıdır.
ms.service: machine-learning
ms.subservice: core
ms.topic: quickstart
ms.reviewer: sgilley
author: sdgilley
ms.author: sgilley
ms.date: 03/21/2019
ms.custom: seodec18
ms.openlocfilehash: 3afea20fe02eafbf14b5162eef3a198d27140b9e
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59549144"
---
# <a name="quickstart-use-your-own-notebook-server-to-get-started-with-azure-machine-learning"></a>Hızlı Başlangıç: Azure Machine Learning'i kullanmaya başlamak için kendi notebook sunucusu kullanma

Değer'de oturum açması kodu çalıştırmak için kendi notebook sunucusu kullanın [Azure Machine Learning hizmeti çalışma alanında](concept-azure-machine-learning-architecture.md). Çalışma alanı, denemeler, eğitmek ve Machine Learning ile makine öğrenimi modelleri dağıtmak için kullandığınız bulutta temel taşıdır.

Bu hızlı başlangıçta Python ortamınızı ve Jupyter Notebook sunucusu kullanılır. SDK yükleme ile bir hızlı başlangıç için bkz: [hızlı başlangıç: Azure Machine Learning'i kullanmaya başlamak için bir bulut tabanlı bir not defteri sunucusu kullan](quickstart-run-cloud-notebook.md) 

Bu hızlı başlangıç videosu görüntüleyin:

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2G9N6]

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree) bugün.

## <a name="prerequisites"></a>Önkoşullar

* Azure Machine Learning SDK ile bir Python 3.6 Not Defteri sunucusu
* Bir Azure Machine Learning hizmeti çalışma
* Bir çalışma alanı yapılandırma dosyası (**.azureml/config.json** ).

Tüm bu önkoşulları alma [bir Azure Machine Learning hizmeti çalışma alanı oluşturma](setup-create-workspace.md#portal).


## <a name="use-the-workspace"></a>Çalışma alanını kullanma

Bir betik oluşturabilir veya çalışma alanı yapılandırma dosyanız ile aynı dizinde bir not defteri başlatın. Deneme çalıştırmalarınızın izlemek için SDK'ın temel API'leri kullanır. Bu kodu çalıştırın.

1. Bir deney, çalışma alanınızda oluşturun.
1. Tek bir değer alanına oturum açın.
1. Değerlerin bir listesini ve denemenin oturum açın.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=useWs)]

## <a name="view-logged-results"></a>Günlüğe kaydedilen sonuçları görüntüleme

Çalıştırma tamamlandığında deneme çalıştırmasının sonucunu Azure portalda görüntüleyebilirsiniz. Son çalıştırmanın sonuçlarını ızgaranın URL yazdırmak için aşağıdaki kodu kullanın:

```python
print(run.get_portal_url())
```

Bu kodu tarayıcınızda Azure portalında oturum değerleri görüntülemek için kullanabileceğiniz bir bağlantıyı döndürür.

![Azure portalında oturum değerleri](./media/quickstart-run-local-notebook/logged-values.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme 

>[!IMPORTANT]
>Kullanabileceğiniz kaynakları oluşturduğunuz diğer Machine Learning öğreticileri için ön koşulları olarak burada ve nasıl yapılır makaleleri.

Bu makalede oluşturduğunuz kaynakları kullanmayı planlamıyorsanız, tüm geçmeyecekseniz ücretlendirmeden kaçınmak için bunları silin.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=delete)]

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, denemeler ve modelleri dağıtmak için ihtiyacınız olan kaynakları oluşturdunuz. Kod içinde bir not defteri çalıştırdığınız ve bulutta çalışma alanınızdaki kodunu çalıştırma geçmişini incelediniz.

> [!div class="nextstepaction"]
> [Öğretici: Bir görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md)

Da keşfedebilirsiniz [örnekler Github'da daha gelişmiş](https://aka.ms/aml-notebooks) veya görüntüleme [SDK'sı Kullanıcı Kılavuzu](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).
