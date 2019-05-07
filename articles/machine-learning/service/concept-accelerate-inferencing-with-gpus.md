---
title: Derin öğrenme modeli için çıkarım GPU ile dağıtma
titleSuffix: Azure Machine Learning service
description: Derin öğrenme modeli için çıkarım bir GPU kullanan bir web hizmeti olarak dağıtmayı öğrenin. Bu makalede, Tensorflow modeli için Azure Kubernetes hizmeti kümesi dağıtılır. Küme, GPU özellikli bir sanal makine konak web hizmeti ve puan çıkarım isteklerini kullanır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: vaidyas
author: csteegz
ms.reviewer: larryfr
ms.date: 05/02/2019
ms.openlocfilehash: 5cc0fe0526937245d3ca913afc477f0259e2afd4
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65160658"
---
# <a name="how-to-do-gpu-inferencing"></a>GPU çıkarım yapma

Bir machine learning web hizmeti olarak modeli için GPU çıkarım kullanmayı öğrenin. Bu makalede, Azure Machine Learning hizmeti örneği Tensorflow derin öğrenme modeli dağıtmak için nasıl kullanılacağını öğrenin. Model hizmeti barındırmak için GPU özellikli bir VM kullanan bir Azure Kubernetes Service (AKS) kümesine dağıtılır. Hizmete istek gönderildiğinde, çıkarım gerçekleştirmek için GPU modelini kullanır.

GPU üzerinde yüksek oranda paralelleştirilebilir hesaplama CPU performans avantajları sunar. Eğitim ve çıkarım derin öğrenme modelleri (özellikle de büyük toplu istekleri) GPU'ları için mükemmel bir kullanım örnekleridir.  

Bu örnek, Azure Machine Learning için kaydedilen TensorFlow model dağıtma gösterilmektedir. 

## <a name="goals-and-prerequisites"></a>Hedefleri ve Önkoşullar

Yönergeleri izleyin:
* GPU etkin oluşturma AKS kümesi
* Tensorflow GPU ile model dağıtma

Ön koşullar:
* Azure Machine Learning services çalışma alanı
* Python
* Tensorflow SavedModel kayıtlı. Modelleri nasıl kaydedeceğinizi öğrenmek için bkz [modelleri dağıtma](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where#registermodel)

Bu makalede dayanır [aks'ye dağıtma Tensorflow modelleri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/production-deploy-to-aks-gpu/production-deploy-to-aks-gpu.ipynb), kaydedilen TensorFlow kullanan modeller ve AKS kümesini dağıtır. Ancak, Puanlama dosyasını ve ortam dosyası yapılan küçük değişikliklerle, Gpu'lar destekleyen tüm makine öğrenmesi çerçeveleri için geçerlidir.  

## <a name="provision-aks-cluster-with-gpus"></a>Gpu'lar ile AKS kümesi sağlama
Azure, her biri için çıkarım kullanılabilir, birçok farklı GPU seçenekleri vardır. Bkz: [N serisi listesini](https://azure.microsoft.com/pricing/details/virtual-machines/linux/#n-series) tam dökümünü ve maliyetleri için. 

Azure Machine Learning hizmeti ile AKS kullanma ile ilgili daha fazla bilgi için bkz: [nasıl dağıtıp burada makalesi.](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where#create-a-new-cluster)

```python
# Provision AKS cluster with GPU machine
prov_config = AksCompute.provisioning_configuration(vm_size="Standard_NC6")

# Create the cluster
aks_target = ComputeTarget.create(
    workspace=ws, name=aks_name, provisioning_configuration=prov_config
)

aks_target.wait_for_deployment()
```

> [!IMPORTANT]
> AKS kümesi sağlanan sürece azure faturalandırır. İşiniz bittiğinde, AKS kümenizi sildiğinizden emin olun, kullanarak.


## <a name="write-entry-script"></a>Giriş betik yazma

Aşağıdaki, çalışma dizininizin kaydetmeye `score.py`. Bu dosya, hizmete gönderilen görüntüleri puanlamak için kullanılır. Bu dosya modeli kaydedilmiş TensorFlow yükler ve ardından her bir POST isteği girdi görüntüsünün TensorFlow oturumuna aktarır ve elde edilen puanları döndürür.
Diğer çıkarım çerçeveleri farklı Puanlama dosyaları gerektirir.

```python
import tensorflow as tf
import numpy as np
import ujson
from azureml.core.model import Model
from azureml.contrib.services.aml_request import AMLRequest, rawhttp
from azureml.contrib.services.aml_response import AMLResponse

def init():
    global session
    global input_name
    global output_name
    
    session = tf.Session()

    model_path = Model.get_model_path('resnet50')
    model = tf.saved_model.loader.load(session, ['serve'], model_path)
    if len(model.signature_def['serving_default'].inputs) > 1:
        raise ValueError("This score.py only supports one input")
    input_name = [tensor.name for tensor in model.signature_def['serving_default'].inputs.values()][0]
    output_name = [tensor.name for tensor in model.signature_def['serving_default'].outputs.values()]
    

@rawhttp
def run(request):
    if request.method == 'POST':
        reqBody = request.get_data(False)
        resp = score(reqBody)
        return AMLResponse(resp, 200)
    if request.method == 'GET':
        respBody = str.encode("GET is not supported")
        return AMLResponse(respBody, 405)
    return AMLResponse("bad request", 500)

def score(data):
    result = session.run(output_name, {input_name: [data]})
    return ujson.dumps(result[1])

if __name__ == "__main__":
    init()
    with open("lynx.jpg", 'rb') as f: #load file for testing locally
        content = f.read()
        print(score(content))

```

## <a name="define-conda-environment"></a>Conda ortamı tanımlayın
Adlı bir conda ortam dosyası oluşturma `myenv.yml` Hizmet bağımlılıklarını belirtmek için. Kullanmakta olduğunuz olduğunu belirtmek önemlidir `tensorflow-gpu` hızlandırılmış performans elde etmek için.
```yaml
name: aml-accel-perf
channels:
  - defaults
dependencies:
  - tensorflow-gpu = 1.12
  - numpy
  - ujson
  - pip:
    - azureml-core
    - azureml-contrib-services
```

## <a name="define-gpu-inferenceconfig"></a>GPU InferenceConfig tanımlayın

Oluşturma bir [ `InferenceConfig` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) GPU etkinleştirdiğini belirtir. Bu, CUDA görüntünüzle yüklendiğini sağlayacaktır.

```python
from azureml.core.model import Model
from azureml.core.model import InferenceConfig

aks_service_name ='gpu-rn'
gpu_aks_config = AksWebservice.deploy_configuration(autoscale_enabled = False, 
                                                    num_replicas = 3, 
                                                    cpu_cores=2, 
                                                    memory_gb=4)
model = Model(ws,"resnet50")

inference_config = InferenceConfig(runtime= "python", 
                                   entry_script="score.py",
                                   conda_file="myenv.yml", 
                                   gpu_enabled=True)
```

Daha fazla bilgi için [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) ve [AksServiceDeploymentConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aks.aksservicedeploymentconfiguration?view=azure-ml-py).
## <a name="deploy-the-model"></a>Modeli dağıtma

AKS kümenizi model dağıtabilir ve hizmetinizi oluşturmak bekleyin.

```python
aks_service = Model.deploy(ws,
                           models=[model],
                           inference_config=inference_config, 
                           deployment_config=aks_config,
                           deployment_target=aks_target,
                           name=aks_service_name)

aks_service.wait_for_deployment(show_output = True)
print(aks_service.state)
```

> [!NOTE]
> Azure Machine Learning hizmeti değil bir model dağıtma bir `InferenceConfig` GPU olmadan bir kümeye GPU bekliyor.

Daha fazla bilgi için [Model](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

## <a name="issue-sample-query-to-deployed-model"></a>Sorun örnek sorgu modeli dağıtılan

Örnek sorgu dağıtılan modelinizi sorun. Bu modeli bir post isteği göndermek için herhangi bir jpeg görüntüsünü Puanlama. 

```python
scoring_url = aks_service.scoring_uri
api_key = aks_service.get_key()(0)
IMAGEURL = "https://upload.wikimedia.org/wikipedia/commons/thumb/6/68/Lynx_lynx_poing.jpg/220px-Lynx_lynx_poing.jpg"

headers = {'Authorization':('Bearer '+ api_key)}
img_data = read_image_from(IMAGEURL).read()
r = requests.post(scoring_url, data = img_data, headers=headers)
```

> [!IMPORTANT]
> Gecikme süresi ve aktarım hızını iyileştirmek için istemci uç noktası ile aynı Azure bölgesinde olmalıdır.  Şu anda API'leri, Doğu ABD Azure bölgesi oluşturulur.

## <a name="cleaning-up-the-resources"></a>Kaynakları temizleme

Kaynaklarınızı Tanıtımı ile işiniz bittiğinde silin.

> [!IMPORTANT]
> Azure, ne kadar süreyle AKS kümesi dağıtıldı göre ücretlendirir. Kendisiyle tamamladıktan sonra temizlik yapmanızı emin olun.

```python
aks_service.delete()
aks_target.delete()
```

## <a name="next-steps"></a>Sonraki adımlar

* [FPGA üzerinde model dağıtma](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-fpga-web-service)
* [Olan ONNX model dağıtma](https://docs.microsoft.com/azure/machine-learning/service/how-to-build-deploy-onnx#deploy)
* [Tensorflow DNN modellerini eğitin](https://docs.microsoft.com/azure/machine-learning/service/how-to-train-tensorflow)
