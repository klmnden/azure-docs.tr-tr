---
title: Azure Machine Learning donanım hızlandırmasını için FPGA paketi
description: Azure Machine Learning kullanıcılar için kullanılabilir python paketlerini hakkında bilgi edinin.
ms.service: machine-learning
ms.component: studio
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: routlaw
author: rloutlaw
ms.date: 05/07/2018
ms.openlocfilehash: e680ef34be1d5dae2942c432de5e81fe620bbdc4
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34832987"
---
# <a name="azure-machine-learning-hardware-acceleration-package"></a>Azure Machine Learning donanım hızlandırma paketi

Azure Machine Learning donanım hızlandırmasını paket Python PIP yüklenebilir için bir veri bilimcileri ve AI geliştiricilerin hızla sağlayan Azure Machine Learning uzantıdır:

+ ResNet 50 quantized sürümüyle Featurize görüntüleri

+ Bu özellikleri temel sınıflandırıcı eğitme

+ Modellere dağıtmak [alan programlanabilir kapısı diziler (FPGA)](concept-accelerate-with-fpgas.md) son derece düşük gecikme süresi inferencing için azure'da

## <a name="prerequisites"></a>Önkoşullar

1. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

1. Bir Azure Machine Learning modeli yönetim hesabı oluşturmanız gerekir. Hesap oluşturma hakkında daha fazla bilgi için bkz: [Azure Machine Learning Quickstart ve çalışma ekranı yükleme](../service/quickstart-installation.md) belge. 

1. Paketin yüklenmesi gerekir. 

 
## <a name="how-to-install-the-package"></a>Paket yükleme

1. En son sürümünü karşıdan yükleyip [Git](https://git-scm.com/downloads).

2. Yükleme [Anaconda (Python 3.6)](https://conda.io/miniconda.html)

3. Önceden yapılandırılmış bir Anaconda ortamı indirmek için Git isteminden şu komutu kullanın:

    ```
    git clone https://aka.ms/aml-real-time-ai
    ```
5. Ortamı oluşturmak için açık bir **Anaconda komut istemi** ve aşağıdaki komutu kullanın:

    ```
    conda env create -f aml-real-time-ai/environment.yml
    ```

6. Ortam etkinleştirmek için aşağıdaki komutu kullanın:

    ```
    conda activate amlrealtimeai
    ```

## <a name="sample-code"></a>Örnek kod

Bu örnek kod, bir model için bir FPGA dağıtmak için SDK kullanılarak üzerinden açıklanmaktadır.

1. Paket içeri aktarın:
   ```python
   import amlrealtimeai
   from amlrealtimeai import resnet50
   ```

1. Ön işleme görüntü:
   ```python 
   from amlrealtimeai.resnet50.model import LocalQuantizedResNet50
   model_path = os.path.expanduser('~/models')
   model = LocalQuantizedResNet50(model_path)
   print(model.version)
   ```

1. Featurize görüntüler:
   ```python 
   from amlrealtimeai.resnet50.model import LocalQuantizedResNet50
   model_path = os.path.expanduser('~/models')
   model = LocalQuantizedResNet50(model_path)
   print(model.version)
   ```

1. Bir sınıflandırıcı oluşturun:
   ```python
   model.import_graph_def(include_featurizer=False)
   print(model.classifier_input)
   print(model.classifier_output)
   ```

1. Hizmet tanımı oluşturun:
   ```python
   from amlrealtimeai.pipeline import ServiceDefinition, TensorflowStage, BrainWaveStage
   save_path = os.path.expanduser('~/models/save')
   service_def_path = os.path.join(save_path, 'service_def.zip')

   service_def = ServiceDefinition()
   service_def.pipeline.append(TensorflowStage(tf.Session(), in_images, image_tensors))
   service_def.pipeline.append(BrainWaveStage(model))
   service_def.pipeline.append(TensorflowStage(tf.Session(), model.classifier_input, model.classifier_output))
   service_def.save(service_def_path)
   print(service_def_path)
   ```
 
1. Modelin bir FPGA üzerinde çalıştırmak için hazırlayın:
   ```python
   from amlrealtimeai import DeploymentClient

   subscription_id = "<Your Azure Subscription ID>"
   resource_group = "<Your Azure Resource Group Name>"
   model_management_account = "<Your AzureML Model Management Account Name>"

   model_name = "resnet50-model"
   service_name = "quickstart-service"

   deployment_client = DeploymentClient(subscription_id, resource_group, model_management_account)
   ```

1. Bir FPGA üzerinde çalıştırmak için modeli dağıtın:
   ```python
   service = deployment_client.get_service_by_name(service_name)
   model_id = deployment_client.register_model(model_name, service_def_path)

   if(service is None):
      service = deployment_client.create_service(service_name, model_id)    
   else:
      service = deployment_client.update_service(service.id, model_id)
   ```

1. İstemci oluşturun:
    ```python
   from amlrealtimeai import PredictionClient
   client = PredictionClient(service.ipAddress, service.port)  
   ```

1. API çağrısı:
   ```python
   image_file = R'C:\path_to_file\image.jpg'
   results = client.score_image(image_file)
   ```

## <a name="reporting-issues"></a>Raporlama konuları

Kullanım [Forumu](https://aka.ms/aml-forum) paketiyle karşılaştığınız sorunları bildirmek için.

## <a name="next-steps"></a>Sonraki adımlar

[Model bir FPGA üzerinde bir web hizmeti olarak dağıtma](how-to-deploy-fpga-web-service.md)