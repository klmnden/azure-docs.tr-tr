---
title: Donanım hızlandırma FPGA paketi
titleSuffix: Azure Machine Learning service
description: Bu makalede kullanım dışı bırakılmıştır.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: reference
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 05/07/2018
ms.custom: seodec18
ROBOTS: NOINDEX
ms.openlocfilehash: 44c0818d7ddb418e34e6e124ddc0b27460bb24a0
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2018
ms.locfileid: "53262087"
---
# <a name="azure-machine-learning-hardware-acceleration-package"></a>Azure Machine Learning donanım hızlandırma paket

>[!Note]
>**Bu makalede kullanım dışı bırakılmıştır.** Bu FPGA paket kullanım dışı bırakıldı. Azure ML SDK'sı için bu işlevleri için destek eklendi. Bu paket için destek artımlı olarak sona erer. [Destek zaman çizelgesini görüntüleyin](overview-what-happened-to-workbench.md#timeline). Bilgi hakkında güncelleştirilmiş [FPGA Destek](concept-accelerate-with-fpgas.md).

Veri bilimcileri ve yapay ZEKA geliştiricilerine hızla sağlayan Azure Machine Learning için Python bir kolayca yüklenebilir uzantısı Azure Machine Learning donanım hızlandırma pakettir:

+ ResNet-50 quantized sürümüyle özellik kazandırın görüntüleri

+ Bu özelliklere göre sınıflandırıcılar eğitin

+ Model için [alan programlanabilir kapı dizileri (FPGA)](concept-accelerate-with-fpgas.md) Ultra düşük gecikme süresi çıkarım için azure'da

## <a name="prerequisites"></a>Önkoşullar

>[!Note]
>Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](http://aka.ms/AMLFree) bugün.

1. Bir Azure Machine Learning Model Yönetimi hesabı. Hesap oluşturmayla ilgili daha fazla bilgi için [Azure Machine Learning hızlı ve Workbench'i yükleme](../desktop-workbench/quickstart-installation.md) belge. 

1. Paketin yüklenmesi gerekir. 

 
## <a name="how-to-install-the-package"></a>Paket yükleme

1. En son sürümünü indirip [Git](https://git-scm.com/downloads).

2. Yükleme [Anaconda (Python 3.6)](https://conda.io/miniconda.html)

   Önceden yapılandırılmış bir Anaconda ortam indirmek için Git isteminden aşağıdaki komutu kullanın:

    ```
    git clone https://aka.ms/aml-real-time-ai
    ```
1. Ortam oluşturmak için açık bir **Anaconda istemi** ve aşağıdaki komutu kullanın:

    ```
    conda env create -f aml-real-time-ai/environment.yml
    ```

1. Ortam etkinleştirmek için aşağıdaki komutu kullanın:

    ```
    conda activate amlrealtimeai
    ```

## <a name="sample-code"></a>Örnek kod

Bu örnek kod SDK'sını kullanarak bir FPGA için model dağıtma adımları gösterilmektedir.

1. Paketi içeri aktarın:
   ```python
   import amlrealtimeai
   from amlrealtimeai import resnet50
   ```

1. Görüntünün önceden işleyebilir:
   ```python 
   from amlrealtimeai.resnet50.model import LocalQuantizedResNet50
   model_path = os.path.expanduser('~/models')
   model = LocalQuantizedResNet50(model_path)
   print(model.version)
   ```

1. Özellik kazandırın görüntüler:
   ```python 
   from amlrealtimeai.resnet50.model import LocalQuantizedResNet50
   model_path = os.path.expanduser('~/models')
   model = LocalQuantizedResNet50(model_path)
   print(model.version)
   ```

1. Sınıflandırıcı oluşturma:
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
 
1. Model bir FPGA üzerinde çalıştırmak için hazırlayın:
   ```python
   from amlrealtimeai import DeploymentClient

   subscription_id = "<Your Azure Subscription ID>"
   resource_group = "<Your Azure Resource Group Name>"
   model_management_account = "<Your AzureML Model Management Account Name>"

   model_name = "resnet50-model"
   service_name = "quickstart-service"

   deployment_client = DeploymentClient(subscription_id, resource_group, model_management_account)
   ```

1. Bir FPGA üzerinde çalıştırmak için model dağıtma:
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

Kullanım [Forumu](https://aka.ms/aml-forum-service) paketiyle karşılaştığınız sorunları bildirmek için.

## <a name="next-steps"></a>Sonraki adımlar

[Model bir FPGA üzerinde bir web hizmeti olarak dağıtma](how-to-deploy-fpga-web-service.md)
