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
ms.date: 07/08/2019
ms.custom: seodec18
ms.openlocfilehash: 406797203a99df7e805e08ee7589148599eeffce
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67670709"
---
# <a name="quickstart-use-your-own-notebook-server-to-get-started-with-azure-machine-learning"></a>Hızlı Başlangıç: Azure Machine Learning'i kullanmaya başlamak için kendi notebook sunucusu kullanma

Azure Machine Learning hizmeti ile çalışmaya başlama için kendi Python ortamını ve Jupyter Notebook sunucusu kullanın.  SDK yükleme ile bir hızlı başlangıç için bkz: [hızlı başlangıç: Azure Machine Learning'i kullanmaya başlamak için bir bulut tabanlı bir not defteri sunucusu kullanmak](quickstart-run-cloud-notebook.md).

Bu hızlı başlangıçta nasıl kullanabileceğinizi gösteren [Azure Machine Learning hizmeti çalışma alanında](concept-azure-machine-learning-architecture.md) makine öğrenimi denemelerini izlemek için. Python kodu, çalıştıracak değerleri çalışma alanına oturum açın.

Bu hızlı başlangıç videosu görüntüleyin:

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2G9N6]

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree) bugün.

## <a name="prerequisites"></a>Önkoşullar

* Azure Machine Learning SDK ile bir Python 3.6 Not Defteri sunucusu
* Bir Azure Machine Learning hizmeti çalışma
* Bir çalışma alanı yapılandırma dosyası ( **.azureml/config.json**).

Tüm bu önkoşulları alma [bir Azure Machine Learning hizmeti çalışma alanı oluşturma](setup-create-workspace.md#sdk).



## <a name="use-the-workspace"></a>Çalışma alanını kullanma

Bir betik oluşturabilir veya bir not defteri, çalışma alanı yapılandırma dosyası ile aynı dizinde Başlat ( **.azureml/config.json**).

### <a name="attach-to-workspace"></a>Çalışma alanına ekleme

Bu kod, çalışma alanınıza eklemek için yapılandırma dosyasından bilgilerini okur.

```
from azureml.core import Workspace

ws = Workspace.from_config()
```

### <a name="log-values"></a>Günlük değerleri

Deneme çalıştırmalarınızın izlemek için SDK'ın temel API'leri kullanır. Bu kodu çalıştırın.

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
