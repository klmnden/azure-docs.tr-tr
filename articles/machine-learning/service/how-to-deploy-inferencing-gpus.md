---
title: GPU ile çıkarımı için model dağıtma
titleSuffix: Azure Machine Learning service
description: Derin öğrenme modeli için çıkarım bir GPU kullanan bir web hizmeti olarak dağıtmayı öğrenin. Bu makalede, Tensorflow modeli için Azure Kubernetes hizmeti kümesi dağıtılır. Küme, GPU özellikli bir sanal makine konak web hizmeti ve puan çıkarımı isteklerini kullanır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: vaidyas
author: csteegz
ms.reviewer: larryfr
ms.date: 05/02/2019
ms.openlocfilehash: ec71165553a1d65ff133d605bf94255100f74e6e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66388918"
---
# <a name="deploy-a-deep-learning-model-for-inference-with-gpu"></a>GPU ile çıkarımı için ayrıntılı öğrenme model dağıtma

Bir machine learning web hizmeti olarak modeli için GPU çıkarımı kullanmayı öğrenin. Çıkarım veya Puanlama modeli, dağıtılan model için tahmin, üretim veri çubuğunda en yaygın olarak kullanıldığı aşamasıdır.

Bu makalede Azure Machine Learning hizmeti örneği Tensorflow derin öğrenme modeli Azure Kubernetes Service (AKS) kümesine bir GPU etkin sanal makineye (VM) dağıtmak için nasıl kullanılacağını size öğretir. Hizmete istek gönderildiğinde, çıkarım iş yüklerini çalıştırmak için GPU modelini kullanır.

GPU üzerinde yüksek oranda paralelleştirilebilir hesaplama CPU performans avantajları sunar. GPU özellikli VM'ler için mükemmel kullanım alanları arasında derin öğrenme eğitim ve çıkarım, özellikle büyük toplu istekler için model.

Bu örnek için Azure Machine Learning modeli kaydedilmiş bir TensorFlow dağıtmayı gösterir. Aşağıdaki adımları uygulayın:

* GPU özellikli bir AKS kümesi oluşturma
* Tensorflow GPU model dağıtma

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure Machine Learning Hizmetleri çalışma
* Python distro
* Kayıtlı Tensorflow modeli kaydedildi. Modelleri kaydetme hakkında bilgi için bkz: [modelleri dağıtma](../service/how-to-deploy-and-where.md#registermodel).

Bu makale, Jupyter not defteri temel [aks'ye dağıtma Tensorflow modelleri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/production-deploy-to-aks-gpu/production-deploy-to-aks-gpu.ipynb). Jupyter not defteri modelleri kaydedilen TensorFlow kullanır ve bunları bir AKS kümesine dağıtır. Not Defteri, Gpu'lar Puanlama dosyasını ve ortam dosyası için küçük değişiklikler yaparak destekleyen altyapısı öğrenme herhangi bir makineye de uygulayabilirsiniz.  

## <a name="provision-an-aks-cluster-with-gpus"></a>Gpu'lar ile bir AKS kümesi sağlama

Azure, birçok farklı GPU seçenekleri vardır. Çıkarım için bunlardan herhangi birini kullanabilirsiniz. Bkz: [N serisi sanal makinelerin listesini](https://azure.microsoft.com/pricing/details/virtual-machines/linux/#n-series) tam dökümünü ve maliyetleri için.

Azure Machine Learning hizmeti ile AKS kullanma ile ilgili daha fazla bilgi için bkz: [nasıl dağıtılacağı ve nerede](../service/how-to-deploy-and-where.md#deploy-aks).

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
> AKS kümesi sağlanan sürece azure faturalandırır. İle işiniz bittiğinde, AKS kümenizi sildiğinizden emin olun.

## <a name="write-the-entry-script"></a>Giriş komut dosyası yazma

Aşağıdaki kod, çalışma dizini olarak kaydetmeye `score.py`. Bu dosya, hizmete gönderilen görüntüleri puanlar. Kaydedilen TensorFlow modeli yükler, girdi görüntüsünün TensorFlow oturum her POST isteğinde geçirir ve sonuçta elde edilen puanları döndürür. Diğer çıkarım çerçeveleri, farklı Puanlama dosyaları gerektirir.

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

## <a name="define-the-conda-environment"></a>Conda ortamı tanımlayın

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

## <a name="define-the-gpu-inferenceconfig-class"></a>GPU InferenceConfig sınıfı tanımlayın

Oluşturma bir `InferenceConfig` nesnesini Gpu'lar sağlar ve CUDA Docker görüntünüzü yüklendiğinden emin olmasını sağlar.

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

Daha fazla bilgi için bkz.

- [InferenceConfig sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py)
- [AksServiceDeploymentConfiguration sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aks.aksservicedeploymentconfiguration?view=azure-ml-py)

## <a name="deploy-the-model"></a>Modeli dağıtma

AKS kümenizi model dağıtabilir ve hizmetinizi oluşturmak bekleyin.

```python
aks_service = Model.deploy(ws,
                           models=[model],
                           inference_config=inference_config, 
                           deployment_config=gpu_aks_config,
                           deployment_target=aks_target,
                           name=aks_service_name)

aks_service.wait_for_deployment(show_output = True)
print(aks_service.state)
```

> [!NOTE]
> Azure Machine Learning hizmeti ile bir model dağıtma olmaz bir `InferenceConfig` nesnesini GPU bir GPU sahip olmayan bir kümeye etkin olmasını bekliyor.

Daha fazla bilgi için [Model sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

## <a name="issue-a-sample-query-to-your-deployed-model"></a>Örnek sorgu dağıtılan modelinizi sorunu

Dağıtılan modele test sorgusu gönderin. Modele bir jpeg görüntüsünü gönderdiğinizde, resmi puanlar.

```python
scoring_url = aks_service.scoring_uri
api_key = aks_service.get_key()(0)
IMAGEURL = "https://upload.wikimedia.org/wikipedia/commons/thumb/6/68/Lynx_lynx_poing.jpg/220px-Lynx_lynx_poing.jpg"

headers = {'Authorization':('Bearer '+ api_key)}
img_data = read_image_from(IMAGEURL).read()
r = requests.post(scoring_url, data = img_data, headers=headers)
```

> [!IMPORTANT]
> Gecikmeyi en aza indirmek ve aktarım hızını iyileştirme için uç nokta olarak aynı Azure bölgesinde istemcinizi olduğundan emin olun. Bu örnekte, Doğu ABD Azure bölgesinde API'leri oluşturulur.

## <a name="clean-up-the-resources"></a>Kaynakları temizleme

Bu örnekle bitirdikten sonra kaynakları silin.

> [!IMPORTANT]
> AKS kümesi ne kadar süreyle dağıtılmış temel azure faturaları. Kendisiyle tamamladıktan sonra temizlik yapmanızı emin olun.

```python
aks_service.delete()
aks_target.delete()
```

## <a name="next-steps"></a>Sonraki adımlar

* [FPGA üzerinde model dağıtma](../service/how-to-deploy-fpga-web-service.md)
* [Olan ONNX model dağıtma](../service/concept-onnx.md#deploy-onnx-models-in-azure)
* [Tensorflow DNN modellerini eğitin](../service/how-to-train-tensorflow.md)
