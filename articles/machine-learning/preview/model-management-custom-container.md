---
title: Azure ML modelleri dağıtmak için kullanılan kapsayıcı görüntüsünü özelleştirme | Microsoft Docs
description: Bu makalede Azure Machine Learning modellerini için bir kapsayıcı görüntüsünün nasıl özelleştirileceğini açıklar
services: machine-learning
author: tedway
ms.author: tedway, raymondl
manager: mwinkle
ms.reviewer: mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 3/26/2018
ms.openlocfilehash: 858c8933565aeeb22dc1b685082dab2c6481737b
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="customize-the-container-image-used-for-azure-ml-models"></a>Azure ML modelleri için kullanılan kapsayıcı görüntüsünü özelleştirme

Bu makalede, Azure Machine Learning modellerini için bir kapsayıcı görüntüsünün nasıl özelleştirileceğini açıklar.  Azure ML çalışma ekranı kapsayıcıları makine öğrenimi modellerini dağıtmak için kullanır. Modeller, bağımlılıklarıyla birlikte dağıtılır ve Azure ML model, bağımlılıkları ve ilişkili dosyaları görüntüden oluşturur.

## <a name="how-to-customize-the-docker-image"></a>Docker görüntüsünün nasıl özelleştirileceğini
Azure ML kullanarak dağıtır Docker görüntüsünü özelleştirme:

1. Depenencies.yml dosya: gelen yüklenebilir bağımlılıkları yönetmek için [Pypı]( https://pypi.python.org/pypi), çalışma ekranı proje conda_dependencies.yml dosyasından kullanın veya kendinizinkini oluşturun. Bu mod PIP yüklenebilir Python Bağımlılıkların yüklenmesi önerilen yaklaşımdır.

   Örnek CLI komut:
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
        
2. Dosya bir Docker adımlar: Bu seçeneği kullanarak, dağıtılan görüntüye Pypı yüklenemez bağımlılıkları yükleyerek özelleştirin. 

   Dosya, bir DockerFile gibi Docker yükleme adımlarını eklemeniz gerekir. Aşağıdaki komutları dosyasında izin verilir: 

    ENV, ARG, ETİKET, ÇALIŞTIRMAK, KULLANIMA SUNMA

   Örnek CLI komut:
   ```azurecli
   az ml image create -n <my Image Name> --manifest-id <my Manifest ID> --docker-file <myDockerStepsFileName> 
   ```

   Görüntü, bildirim ve hizmet komutları docker dosya bayrağı kabul eder.

   Docker adımları dosyası örneği:
   ```docker
   # Install tools required to build the project
   RUN apt-get update && apt-get install -y git libxml2-dev
   # Install library dependencies
   RUN dep ensure -vendor-only
   ```

> [!NOTE]
> Azure ML kapsayıcıları için temel görüntü Ubuntu ve değiştirilemez. Farklı bir temel görüntü belirtirseniz, göz ardı edilir.

## <a name="next-steps"></a>Sonraki adımlar
Kapsayıcı görüntünüzü özelleştirdiğiniz, büyük ölçekli kullanmak için bir kümeye dağıtabilirsiniz.  Küme web hizmet dağıtımı için ayarlama hakkında daha fazla bilgi için bkz: [Model Yönetim Yapılandırması](deployment-setup-configuration.md). 
