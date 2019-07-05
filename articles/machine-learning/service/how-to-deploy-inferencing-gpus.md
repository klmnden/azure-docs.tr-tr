---
title: GPU ile çıkarımı için model dağıtma
titleSuffix: Azure Machine Learning service
description: Bu makalede, bir GPU özellikli Tensorflow derin öğrenme web service.service olarak model ve puanlama çıkarımı istekleri dağıtmak için Azure Machine Learning hizmetini kullanmayı öğretir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: vaidyas
author: csteegz
ms.reviewer: larryfr
ms.date: 06/01/2019
ms.openlocfilehash: 8086d059913cc61bff0bca31681368bea6d76777
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543796"
---
# <a name="deploy-a-deep-learning-model-for-inference-with-gpu"></a>GPU ile çıkarımı için ayrıntılı öğrenme model dağıtma

Bu makalede, bir GPU özellikli Tensorflow derin öğrenme modeli bir web hizmeti olarak dağıtmak için Azure Machine Learning hizmetini kullanmayı öğretir.

Modelinizi GPU özellikli çıkarım yapmak için bir Azure Kubernetes Service (AKS) kümesine dağıtın. Çıkarım veya model Puanlama, dağıtılmış bir modelinin tahmin için kullanıldığı aşamasıdır. Yüksek oranda paralelleştirilebilir hesaplama CPU teklif performans avantajlarını yerine GPU'ları kullanarak.

Bu örnek bir TensorFlow modeli kullansa da, herhangi bir makine öğrenme Puanlama dosyasını ve ortam dosyası için küçük değişiklikler yaparak GPU'ları destekleyen altyapısı için aşağıdaki adımları uygulayabilirsiniz. 

Bu makalede, aşağıdaki adımları uygulayın:

* GPU özellikli bir AKS kümesi oluşturma
* Tensorflow GPU model dağıtma
* Örnek sorgu dağıtılan modelinizi sorunu

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure Machine Learning services çalışma alanı.
* Bir Python distro.
* Kayıtlı Tensorflow modeli kaydedildi.
    * Modelleri kaydetme hakkında bilgi için bkz: [modelleri dağıtma](../service/how-to-deploy-and-where.md#registermodel).

Bu nasıl yapılır serisinin birinci bölümünü tamamlayabilirsiniz [TensorFlow modeli eğitmek nasıl](how-to-train-tensorflow.md), gerekli önkoşulları karşılamak için.

## <a name="provision-an-aks-cluster-with-gpus"></a>Gpu'lar ile bir AKS kümesi sağlama

Azure, birçok farklı GPU seçenekleri vardır. Çıkarım için bunlardan herhangi birini kullanabilirsiniz. Bkz: [N serisi sanal makinelerin listesini](https://azure.microsoft.com/pricing/details/virtual-machines/linux/#n-series) tam dökümünü ve maliyetleri için.

Azure Machine Learning hizmeti ile AKS kullanma ile ilgili daha fazla bilgi için bkz: [nasıl dağıtılacağı ve nerede](../service/how-to-deploy-and-where.md#deploy-aks).

```Python
# Choose a name for your cluster
aks_name = "aks-gpu"

# Check to see if the cluster already exists
try:
    compute_target = ComputeTarget(workspace=ws, name=aks_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    # Provision AKS cluster with GPU machine
    prov_config = AksCompute.provisioning_configuration(vm_size="Standard_NC6")

    # Create the cluster
    aks_target = ComputeTarget.create(
        workspace=ws, name=aks_name, provisioning_configuration=prov_config
    )

    aks_target.wait_for_completion(show_output=True)
```

> [!IMPORTANT]
> AKS kümesi sağlanan sürece azure faturalandırır. İle işiniz bittiğinde, AKS kümenizi sildiğinizden emin olun.

## <a name="write-the-entry-script"></a>Giriş komut dosyası yazma

Aşağıdaki kod, çalışma dizini olarak kaydetmeye `score.py`. Bu dosya, hizmete gönderilen görüntüleri puanlar. Kaydedilen TensorFlow modeli yükler, girdi görüntüsünün TensorFlow oturum her POST isteğinde geçirir ve sonuçta elde edilen puanları döndürür. Diğer çıkarım çerçeveleri, farklı Puanlama dosyaları gerektirir.

```python
import json
import numpy as np
import os
import tensorflow as tf

from azureml.core.model import Model

def init():
    global X, output, sess
    tf.reset_default_graph()
    model_root = Model.get_model_path('tf-dnn-mnist')
    saver = tf.train.import_meta_graph(os.path.join(model_root, 'mnist-tf.model.meta'))
    X = tf.get_default_graph().get_tensor_by_name("network/X:0")
    output = tf.get_default_graph().get_tensor_by_name("network/output/MatMul:0")
    
    sess = tf.Session()
    saver.restore(sess, os.path.join(model_root, 'mnist-tf.model'))

def run(raw_data):
    data = np.array(json.loads(raw_data)['data'])
    # make prediction
    out = output.eval(session=sess, feed_dict={X: data})
    y_hat = np.argmax(out, axis=1)
    return y_hat.tolist()

```
## <a name="define-the-conda-environment"></a>Conda ortamı tanımlayın

Adlı bir conda ortam dosyası oluşturma `myenv.yml` Hizmet bağımlılıklarını belirtmek için. Kullanmakta olduğunuz olduğunu belirtmek önemlidir `tensorflow-gpu` hızlandırılmış performans elde etmek için.

```yaml
name: project_environment
dependencies:
  # The python interpreter version.
  # Currently Azure ML only supports 3.5.2 and later.
- python=3.6.2

- pip:
  - azureml-defaults==1.0.43.*
- numpy
- tensorflow-gpu=1.12
channels:
- conda-forge
```

## <a name="define-the-gpu-inferenceconfig-class"></a>GPU InferenceConfig sınıfı tanımlayın

Oluşturma bir `InferenceConfig` nesnesini Gpu'lar sağlar ve CUDA Docker görüntünüzü yüklendiğinden emin olmasını sağlar.

```python
from azureml.core.model import Model
from azureml.core.model import InferenceConfig

aks_service_name ='aks-dnn-mnist'
gpu_aks_config = AksWebservice.deploy_configuration(autoscale_enabled = False, 
                                                    num_replicas = 3, 
                                                    cpu_cores=2, 
                                                    memory_gb=4)
model = Model(ws,"tf-dnn-mnist")

inference_config = InferenceConfig(runtime= "python", 
                                   entry_script="score.py",
                                   conda_file="myenv.yml", 
                                   enable_gpu=True)
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

## <a name="issue-a-sample-query-to-your-model"></a>Örnek sorgu modelinize sorunu

Dağıtılan modele test sorgusu gönderin. Modele bir jpeg görüntüsünü gönderdiğinizde, resmi puanlar. Aşağıdaki kod örneği, resimleri yüklemek için bir dış yardımcı işlevini kullanır. Pir sırasında ilgili kodu bulabilirsiniz [TensorFlow örneği github'daki](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow/utils.py). 

```python
# Used to test your webservice
from utils import load_data 

# Load test data from model training
X_test = load_data('./data/mnist/test-images.gz', False) / 255.0
y_test = load_data('./data/mnist/test-labels.gz', True).reshape(-1)

# send a random row from the test set to score
random_index = np.random.randint(0, len(X_test)-1)
input_data = "{\"data\": [" + str(list(X_test[random_index])) + "]}"

api_key = aks_service.get_keys()[0]
headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
resp = requests.post(aks_service.scoring_uri, input_data, headers=headers)

print("POST to url", aks_service.scoring_uri)
#print("input data:", input_data)
print("label:", y_test[random_index])
print("prediction:", resp.text)
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
