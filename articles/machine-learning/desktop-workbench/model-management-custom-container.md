---
title: Azure ML modelleri dağıtmak için kullanılan kapsayıcı görüntüsünü özelleştirmeniz | Microsoft Docs
description: Bu makalede Azure Machine Learning modelleri için bir kapsayıcı görüntüsünün nasıl özelleştirileceğini açıklar.
services: machine-learning
author: tedway
ms.author: tedway
manager: mwinkle
ms.reviewer: mldocs, raymondl
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 3/26/2018
ms.openlocfilehash: 7879cf1891e071da1a0ad3ddfc30f90fc7be8ca5
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35651259"
---
# <a name="customize-the-container-image-used-for-azure-ml-models"></a>Azure ML modelleri için kullanılan kapsayıcı görüntüsünü özelleştirme

Bu makalede, Azure Machine Learning modelleri için bir kapsayıcı görüntüsünün nasıl özelleştirileceğini açıklar.  Azure ML Workbench, kapsayıcılar, makine öğrenimi modelleri dağıtmak için kullanır. Modelleri, bağımlılıklarıyla birlikte dağıtılır ve Azure ML model, bağımlılıkları ve ilişkili dosyaları bir görüntüden oluşturur.

## <a name="how-to-customize-the-docker-image"></a>Docker görüntüsünü özelleştirme
Azure ML kullanarak dağıtır Docker görüntüsünü Özelleştir:

1. A `dependencies.yml` dosya: gelen yüklenebilir bağımlılıkları yönetmek için [Pypı]( https://pypi.python.org/pypi), kullanabileceğiniz `conda_dependencies.yml` dosya Workbench projeden veya kendinizinkini oluşturun. Bu kolayca yüklenebilir Python Bağımlılıkların yüklenmesi için önerilen yaklaşımdır.

   Örnek CLI komutunu:
   ```azurecli
   az ml image create -n <my Image Name> --manifest-id <my Manifest ID> -c amlconfig\conda_dependencies.yml
   ```

   Örnek conda_dependencies dosyası: 
   ```yaml
   name: project_environment
   dependencies:
      - python=3.5.2
      - scikit-learn
      - ipykernel=4.6.1
      
      - pip:
        - azure-ml-api-sdk==0.1.0a11
        - matplotlib
   ```
        
2. Bir Docker dosyası adımlar: Bu seçeneği kullanarak, dağıtılan görüntüdeki Pypı yüklenemez bağımlılıkları yükleyerek özelleştirin. 

   Dosya bir DockerFile gibi Docker yükleme adımlarını eklemeniz gerekir. Aşağıdaki komutlar, dosyada izin verilir: 

    ENV ARG, ETİKET ÇALIŞTIR KULLANIMA SUNMA

   Örnek CLI komutunu:
   ```azurecli
   az ml image create -n <my Image Name> --manifest-id <my Manifest ID> --docker-file <myDockerStepsFileName> 
   ```

   Görüntü, bildirim ve hizmet komutları kabul eder, docker dosya bayrağı.

   Docker adımları dosyası örneği:
   ```docker
   # Install tools required to build the project
   RUN apt-get update && apt-get install -y git libxml2-dev
   # Install library dependencies
   RUN dep ensure -vendor-only
   ```

> [!NOTE]
> Azure ML kapsayıcılar için temel görüntü Ubuntu olduğu ve değiştirilemez. Farklı bir temel görüntü belirtirseniz, göz ardı edilir.

## <a name="next-steps"></a>Sonraki adımlar
Kapsayıcı görüntünüzü özelleştirdiğinize göre büyük ölçekli kullanmak için bir kümeye dağıtabilirsiniz.  Web hizmeti dağıtımı için bir küme ayarlama hakkında daha fazla ayrıntı için bkz. [Model Yönetim Yapılandırması](deployment-setup-configuration.md). 
