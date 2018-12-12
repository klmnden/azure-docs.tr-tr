---
title: Azure Machine Learning Hizmetleri Azure not defterleri kullanma
description: Örnek Not Defterleri Azure not defterleri ile kullanabileceğiniz bir Azure Machine Learning Hizmetleri için genel bakış.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 0dc4fc31-ae1c-422c-ac34-7b025e6651b4
ms.service: notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: 1508ccc151aaafd7e0c0b46345c4dcb1a5b7b03c
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53083674"
---
# <a name="use-azure-machine-learning-services-in-a-notebook"></a>Azure Machine Learning Hizmetleri bir not defteri kullanma

Azure not defterleri ile çalışmak için gerekli ortam ile önceden yapılandırılmış gelen [Azure Machine Learning Hizmetleri](/azure/machine-learning/service/). Not defterlerini hesabınızda çeşitli Machine Learning senaryolarını keşfetmek için örnek proje bir kolayca kopyalayabilirsiniz.

## <a name="clone-the-sample-into-your-account"></a>Hesabınızda örneği kopyalayın

1. Oturum [Azure not defterleri](https://notebooks.azure.com/).
1. Seçin **Projelerim** Proje panosuna gidin.
1. Seçin **karşıya GitHub deposunu** (yukarı ok) düğmesini açın **karşıya Github deposu** açılan.
1. Açılan pencerede girin `Azure/MachineLearningNotebooks` içinde **GitHub deposu**, proje için bir ad sağlayın **proje adı** "Gibi Azure ML Hizmetleri", bir tanımlayıcı sağlar **proje kimliği**temizleyin **genel** isterseniz, ardından **alma**.

    ![Azure Machine Learning not defteri örnek not defterleri hesabınızda içeri aktarma](media/azureml-import-project.png)

1. Bir veya iki dakika sonra Azure not defterleri, otomatik olarak yeni projenin panoya alır.

## <a name="run-a-sample-notebook"></a>Bir örnek not defteri çalıştırma

1. Seçin **00 - configuration.ipynb** not defterinin yapılandırma bölümü başlatın ve bir Azure Machine Learning çalışma alanı oluşturmak için kendi yönergelerini izleyin.

    - Azure not defterleri gerekli Python paketlerini içerdiğinden, kod parçacığının yalnızca 2. adımda Azure ML SDK sürümünü doğrulamak için Önkoşullar çalıştırabilirsiniz.

1. Yapılandırma tamamlandıktan sonra seçin **01.getting çalışmaya** On üç farklı örnek not defterleri içeren klasöre gidin, her biri açıklayıcıdır.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning Hizmetleri belgeleri çeşitli içinde not defterleri ile Machine Learning hizmeti çalışmada size kılavuzluk diğer kaynaklara içerir:

- [Hızlı Başlangıç: Kullanımı Azure Machine Learning'i kullanmaya başlamak Python](https://docs.microsoft.com/azure/machine-learning/service/quickstart-create-workspace-with-python)
- [#1 öğretici: Azure Machine Learning hizmeti ile bir görüntü sınıflandırma modeli eğitme](https://docs.microsoft.com/azure/machine-learning/service/tutorial-train-models-with-aml)
- [Öğretici #2: bir görüntü sınıflandırma modeli Azure Container örneği (ACI) dağıtma](https://docs.microsoft.com/azure/machine-learning/service/tutorial-deploy-models-with-aml)
- [Öğretici: Azure Machine Learning hizmetindeki otomatik machine learning ile bir sınıflandırma modeli eğitme](https://docs.microsoft.com/azure/machine-learning/service/tutorial-auto-train-models)

Ayrıca belgelerine bakın [Python için Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).
