---
title: Nasıl ve nerede modelleri dağıtma
titleSuffix: Azure Machine Learning service
description: 'Nasıl ve nerede bilgi dahil olmak üzere Azure Machine Learning hizmeti Modellerinizi dağıtmak için: Azure Container Instances, Azure Kubernetes hizmeti, Azure IOT Edge ve alanda programlanabilir kapı dizileri.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 05/02/2019
ms.custom: seoapril2019
ms.openlocfilehash: 1da232c2a81c9989cc78eccf1be97b5d75a48666
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65024480"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Azure Machine Learning hizmeti ile modelleri dağıtma

Machine learning modeli Azure bulutta veya IOT Edge cihazları için bir web hizmeti olarak dağıtmayı öğrenin. Bu belgedeki bilgiler aşağıdaki işlem hedeflere dağıtın öğretir:

| Hedef işlem | Dağıtım türü | Açıklama |
| ----- | ----- | ----- |
| [Yerel web hizmeti](#local) | Test/hata ayıklama | Sınırlı test etme ve sorun giderme için uygundur.
| [Azure Kubernetes Service (AKS)](#aks) | Gerçek zamanlı çıkarımı | Büyük ölçekli üretim dağıtımları için idealdir. Otomatik ölçeklendirme ve hızlı yanıt süresi sağlar. |
| [Azure Container Instances (ACI)](#aci) | Test Etme | Düşük ölçek, CPU tabanlı iş yükleri için uygundur. |
| [Azure Machine Learning işlem](how-to-run-batch-predictions.md) | (Önizleme) Batch çıkarımı | Toplu Puanlama sunucusuz bir işlem üzerinde çalıştırın. Normal veya düşük öncelikli sanal makineleri destekler. |
| [Azure IoT Edge](#iotedge) | (Önizleme) IOT Modülü | Dağıtma ve IOT cihazlarında ML modelleri hizmet. |

## <a name="deployment-workflow"></a>Dağıtım iş akışı

Tüm işlem hedeflerine yönelik bir model dağıtma işlemini benzer:

1. Model kaydedin.
1. Model dağıtma.
1. Test modellere dağıtıldı.

Dağıtım iş akışı içinde ilgili kavramları hakkında daha fazla bilgi için bkz. [yönetin, dağıtın ve izleyin modeller Azure Machine Learning hizmeti ile](concept-model-management-and-deployment.md).

## <a name="prerequisites-for-deployment"></a>Dağıtım önkoşulları

- Bir model. Eğitilen bir modelin izniniz yok, modeli kullandığınız & bağımlılığı dosyaları sağlanan içinde [Bu öğreticide](http://aka.ms/azml-deploy-cloud).

- [Machine Learning hizmeti için Azure CLI uzantısı](reference-azure-machine-learning-cli.md), veya [Azure Machine Learning Python SDK'sı](https://aka.ms/aml-sdk).

## <a id="registermodel"></a> Makine öğrenme modeli kaydedin

Model kayıt depolamak ve eğitilen Modellerinizi Azure bulutunda düzenlemek için bir yoldur. Modeller Azure Machine Learning hizmeti çalışma alanınızda kaydedilir. Model, Azure Machine Learning kullanarak veya başka bir yerde eğitilmiş modelden alınan düşünürler. Aşağıdaki örnekler, bir model dosyasından kaydetme göstermektedir:

### <a name="register-a-model-from-an-experiment-run"></a>Denemeyi çalıştırma modelden kaydetme

**CLI ile örnek Scikit-öğrenme**
```azurecli-interactive
az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment
```
**SDK'sını kullanma**
```python
model = run.register_model(model_name='sklearn_mnist', model_path='outputs/sklearn_mnist_model.pkl')
print(model.name, model.id, model.version, sep='\t')
```

### <a name="register-an-externally-created-model"></a>Harici olarak oluşturulmuş bir modeli kaydedin
Harici olarak oluşturulmuş bir model sunarak kaydedebileceğiniz bir **yerel yol** modeli. Bir klasör veya tek bir dosyayı sağlayabilir.

**Python SDK'sı ile ONNX örnek:**
```python
onnx_model_url = "https://www.cntk.ai/OnnxModels/mnist/opset_7/mnist.tar.gz"
urllib.request.urlretrieve(onnx_model_url, filename="mnist.tar.gz")
!tar xvzf mnist.tar.gz

model = Model.register(workspace = ws,
                       model_path ="mnist/model.onnx",
                       model_name = "onnx_mnist",
                       tags = {"onnx": "demo"},
                       description = "MNIST image classification CNN from ONNX Model Zoo",)
```

**CLI kullanarak**
```azurecli-interactive
az ml model register -n onnx_mnist -p mnist/model.onnx
```

**Tahmini Süre**: Yaklaşık 10 saniye.

Daha fazla bilgi için başvuru belgeleri için bkz. [Model sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

## <a name="how-to-deploy"></a>Dağıtma

Bir web hizmeti olarak dağıtalım için çıkarım yapılandırması oluşturun (`InferenceConfig`) ve bir dağıtım yapılandırması. Çıkarım yapılandırmada modelinizi sunmak için gerekli bağımlılıkları ve betikleri belirtin. Dağıtım yapılandırması nasıl işlem hedef model hizmet ayrıntılarını belirtin.


### <a id="script"></a> 1. Giriş betik & bağımlılıkları tanımlayın

Giriş betik, dağıtılan web hizmetine gönderilen verileri alır ve modele geçirir. Sonra modeli tarafından döndürülen yanıtı alır ve istemciye döndürür. **Kendi modeline özgü olduğundan ve betiğin**; model bekliyor ve döndüren veri anlamanız gerekir.

Betik, yükleme ve çalıştırmayı iki işlev içerir:

* `init()`: Genellikle bu işlev, genel bir nesnesine modeli yükler. Bu işlev, yalnızca web hizmeti için Docker kapsayıcı başlatıldığında bir kez çalıştırılır.

* `run(input_data)`: Bu işlev, giriş verileri temel alan bir değer tahmin modelini kullanır. Genellikle girişler ve çıkışlar farklı çalıştır JSON seri hale getirme ve serinin için kullanın. Ayrıca, ham ikili verileri ile çalışabilirsiniz. Veri modeline göndermeden önce veya istemciye döndürmeden önce dönüştürebilirsiniz.

#### <a name="optional-automatic-swagger-schema-generation"></a>(İsteğe bağlı) Otomatik Swagger şema oluşturma

Otomatik olarak web hizmetiniz için bir şema oluşturmak, giriş örneği sağlar ve/veya çıkış için tanımlanan bir tür nesne türü ve örnek oluşturucusu otomatik olarak şema oluşturmak için kullanılır. Azure Machine Learning hizmeti daha sonra oluşturur bir [Openapı](https://swagger.io/docs/specification/about/) dağıtımı sırasında web hizmeti için (Swagger) belirtimi.

Aşağıdaki türleri şu anda desteklenir:

* `pandas`
* `numpy`
* `pyspark`
* Standart Python nesnesi

Şeması oluşturma kullanmak için dahil `inference-schema` conda ortam dosyanızdaki Paket. Aşağıdaki örnekte `[numpy-support]` giriş betik numpy parametre türü kullandığından: 

#### <a name="example-dependencies-file"></a>Örnek bağımlılıklar dosyası
Çıkarım için Conda bağımlılıkları dosyasının bir örnek verilmiştir.
```python
name: project_environment
dependencies:
  - python=3.6.2
  - pip:
    - azureml-defaults
    - scikit-learn
    - inference-schema[numpy-support]
```

Otomatik şeması oluşturma, giriş komut dosyanızı kullanmak istiyorsanız **gerekir** alma `inference-schema` paketleri. 

Giriş tanımlayın ve örnek biçimlerde çıkış `input_sample` ve `output_sample` değişkenleri, web hizmeti için istek ve yanıt formatları temsil eder. Girdide bu örnekleri kullanın ve işlev dekoratörler üzerinde çıkışını `run()` işlevi. Scikit-aşağıdaki örnekte şeması oluşturma kullandığını öğrenin.

> [!TIP]
> Hizmet dağıtıldıktan sonra kullanın `swagger_uri` şema JSON belgesi alınacağını özelliği.

#### <a name="example-entry-script"></a>Örnek giriş betiği

Aşağıdaki örnek, kabul edin ve JSON verilerini döndürmek gösterilmektedir:

**Scikit-öğrenme örnek Swagger oluşturma ile:**
```python
import json
import numpy as np
from sklearn.externals import joblib
from sklearn.linear_model import Ridge
from azureml.core.model import Model

from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.numpy_parameter_type import NumpyParameterType

def init():
    global model
    # note here "sklearn_regression_model.pkl" is the name of the model registered under
    # this is a different behavior than before when the code is run locally, even though the code is the same.
    model_path = Model.get_model_path('sklearn_regression_model.pkl')
    # deserialize the model file back into a sklearn model
    model = joblib.load(model_path)

input_sample = np.array([[10,9,8,7,6,5,4,3,2,1]])
output_sample = np.array([3726.995])

@input_schema('data', NumpyParameterType(input_sample))
@output_schema(NumpyParameterType(output_sample))
def run(data):
    try:
        result = model.predict(data)
        # you can return any datatype as long as it is JSON-serializable
        return result.tolist()
    except Exception as e:
        error = str(e)
        return error
```

Daha fazla örnek komut dosyası için aşağıdaki örneklere bakın:

* Pytorch: [https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch)
* TensorFlow: [https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow)
* Keras: [https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras)
* ONNX: [https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx/](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx/)
* İkili verileri karşı Puanlama: [Bir web hizmetini kullanma](how-to-consume-web-service.md)

### <a name="2-define-your-inferenceconfig"></a>2. InferenceConfig tanımlayın

Çıkarım yapılandırma Öngörüler bulunmak üzere modelinizi yapılandırılması açıklanmaktadır. Aşağıdaki örnek, çıkarım yapılandırmasının nasıl oluşturulacağını gösterir:

```python
inference_config = InferenceConfig(source_directory="C:/abc",
                                   runtime= "python",
                                   entry_script="x/y/score.py",
                                   conda_file="env/myenv.yml")
```

Bu örnekte, yapılandırma aşağıdaki öğeleri içerir:

* Çıkarım yapmak için gereken varlıkları içeren bir dizin
* Bu model Python gerektirir
* [Giriş betik](#script), dağıtılmış hizmette gönderilen web isteklerini işlemek için kullanılır
* Çıkarım çalıştırmak için gereken Python paketlerini tanımlayan conda dosyası

InferenceConfig işlevler hakkında daha fazla bilgi için bkz. [Gelişmiş Yapılandırma](#advanced-config) bölümü.

### <a name="3-define-your-deployment-configuration"></a>3. Dağıtım yapılandırmanızı tanımlayın

Dağıtmadan önce dağıtım yapılandırması tanımlamanız gerekir. Dağıtım Yapılandırması, web hizmetini barındıran işlem hedefine özeldir. Örneğin, yerel olarak dağıtırken hizmet istekleri kabul eder bağlantı noktası belirtmeniz gerekir.

İşlem kaynağı oluşturmanız gerekebilir. Örneğin, henüz yoksa bir Azure Kubernetes hizmeti çalışma alanınızla ilişkili.

Aşağıdaki tabloda, her işlem hedefi için bir dağıtım yapılandırması oluşturma örneği verilmiştir:

| Hedef işlem | Dağıtım yapılandırma örneği |
| ----- | ----- |
| Yerel | `deployment_config = LocalWebservice.deploy_configuration(port=8890)` |
| Azure Container Örneği | `deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |
| Azure Kubernetes Service | `deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |

Aşağıdaki bölümlerde, dağıtım yapılandırması oluşturun ve web hizmeti dağıtmak için kullanmak nasıl ekleyebileceğiniz gösterilmektedir.

## <a name="where-to-deploy"></a>Dağıtılacağı yeri

### <a id="local"></a> Yerel olarak dağıtma

Bu bölümdeki örneklerde kullanım [deploy_from_image](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-from-model-workspace--name--models--image-config--deployment-config-none--deployment-target-none-), bir dağıtım yapmadan önce modeli ve görüntü kayıt gerektirir. Diğer dağıtım yöntemleri hakkında daha fazla bilgi için bkz. [dağıtma](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-workspace--name--model-paths--image-config--deployment-config-none--deployment-target-none-) ve [deploy_from_model](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-from-model-workspace--name--models--image-config--deployment-config-none--deployment-target-none-).

**Yerel olarak dağıtmak için Docker'ın yerel makinenizde yüklü olması gerekir.**

**SDK'sını kullanma**

```python
deployment_config = LocalWebservice.deploy_configuration(port=8890)
service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config)
service.wait_for_deployment(show_output = True)
print(service.state)
```

**CLI kullanarak**

```azurecli-interactive
az ml model deploy -m sklearn_mnist:1 -ic inferenceconfig.json -dc deploymentconfig.json
```

### <a id="aci"></a> Azure Container Instances'a (DEVTEST) dağıtma

Bir web hizmeti bir veya daha aşağıdaki koşullardan biri Modellerinizi dağıtmak için Azure Container Instances kullanmak doğrudur:
- Hızlı bir şekilde dağıtın ve modelinizi doğrulama gerekir.
- Geliştirilmekte olan bir modeli test edersiniz. 

ACI için kotaları ve bölge kullanılabilirliği görmek için bkz [kotaları ve Azure Container Instances için bölge kullanılabilirliği](https://docs.microsoft.com/azure/container-instances/container-instances-quotas) makalesi.

**SDK'sını kullanma**

```python
deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
service = Model.deploy(ws, "aciservice", [model], inference_config, deployment_config)
service.wait_for_deployment(show_output = True)
print(service.state)
```

**CLI kullanarak**

```azurecli-interactive
az ml model deploy -m sklearn_mnist:1 -n aciservice -ic inferenceconfig.json -dc deploymentconfig.json
```

Daha fazla bilgi için başvuru belgeleri için bkz. [AciWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py) ve [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py) sınıfları.

### <a id="aks"></a> Azure Kubernetes Service'e (üretim) dağıtma

Mevcut bir AKS kümesi kullanmak veya Azure Machine Learning SDK'sı, CLI veya Azure portalını kullanarak yeni bir tane oluşturun.


> [!IMPORTANT]
> Olan bir AKS kümesi oluşturma işlemi için çalışma süresi. Bu kümeye birden çok dağıtımlar için yeniden kullanabilirsiniz.
> Oluşturulmamış veya iliştirilmiş bir AKS, küme Git <a href="#create-attach-aks">burada</a>.

#### AKS'ye dağıtma <a id="deploy-aks"></a>

AKS Azure ML CLI ile dağıtım yapabilirsiniz:
```azurecli-interactive
az ml model deploy -ct myaks -m mymodel:1 -n aksservice -ic inferenceconfig.json -dc deploymentconfig.json
```

Python SDK'yı da kullanabilirsiniz:
```python
aks_target = AksCompute(ws,"myaks")
deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
service = Model.deploy(ws, "aksservice", [model], inference_config, deployment_config, aks_target)
service.wait_for_deployment(show_output = True)
print(service.state)
print(service.get_logs())
```

Otomatik ölçeklendirme, dahil olmak üzere AKS dağıtımınızı yapılandırma hakkında daha fazla bilgi için bkz. [AksWebservice.deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice) başvuru.

**Tahmini süre:** Yaklaşık 5 dakika.

#### Oluşturun veya bir AKS kümesi ekleme <a id="create-attach-aks"></a>
Oluşturma veya ekleme bir AKS kümesi bir **bir süre işlem** çalışma alanınız için. Bir küme çalışma alanınız ile ilişkilendirildikten sonra birden çok dağıtım için kullanabilirsiniz. 

Küme veya onu içeren kaynak grubunu silerseniz, yeni bir kümeye dağıtmak için gerektiğinde oluşturmanız gerekir.

##### <a name="create-a-new-aks-cluster"></a>Yeni bir AKS kümesi oluşturma
Ayarı hakkında daha fazla bilgi için `autoscale_target_utilization`, `autoscale_max_replicas`, ve `autoscale_min_replicas`, bkz: [AksWebservice.deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py#deploy-configuration-autoscale-enabled-none--autoscale-min-replicas-none--autoscale-max-replicas-none--autoscale-refresh-seconds-none--autoscale-target-utilization-none--collect-model-data-none--auth-enabled-none--cpu-cores-none--memory-gb-none--enable-app-insights-none--scoring-timeout-ms-none--replica-max-concurrent-requests-none--max-request-wait-time-none--num-replicas-none--primary-key-none--secondary-key-none--tags-none--properties-none--description-none-) başvuru.
Aşağıdaki örnek yeni bir Azure Kubernetes hizmeti kümesinin nasıl oluşturulacağını gösterir:

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Use the default configuration (you can also provide parameters to customize this)
prov_config = AksCompute.provisioning_configuration()

aks_name = 'myaks'
# Create the cluster
aks_target = ComputeTarget.create(workspace = ws,
                                    name = aks_name,
                                    provisioning_configuration = prov_config)

# Wait for the create process to complete
aks_target.wait_for_completion(show_output = True)
```

Azure Machine Learning SDK'sı dışında bir AKS kümesi oluşturma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
* [AKS kümesi oluşturma](https://docs.microsoft.com/cli/azure/aks?toc=%2Fazure%2Faks%2FTOC.json&bc=%2Fazure%2Fbread%2Ftoc.json&view=azure-cli-latest#az-aks-create)
* [(Portal) AKS kümesi oluşturma](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal?view=azure-cli-latest)


> [!IMPORTANT]
> İçin [ `provisioning_configuration()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py), büyüktür veya eşittir 12 sanal CPU'lara göre vm_size çarpılan agent_count emin olmanız gerekir daha sonra agent_count ve vm_size, özel değerleri seçin. Örneğin, bir vm_size 4 sanal CPU'lar varsa, "Standard_D3_v2" birini kullanırsanız, 3 veya daha büyük bir agent_count seçmeniz gerekir.

**Tahmini Süre**: Yaklaşık 20 dakika.

##### <a name="attach-an-existing-aks-cluster"></a>Mevcut bir AKS kümesi ekleme

AKS kümesini Azure aboneliğinizde zaten ve sürüm 1.12. ## ve en az 12 sanal CPU'lara sahip, görüntünüzü dağıtmak için kullanın. Aşağıdaki kod, mevcut bir AKS 1.12 eklemek gösterilmektedir. ## çalışma kümesi:

```python
from azureml.core.compute import AksCompute, ComputeTarget
# Set the resource group that contains the AKS cluster and the cluster name
resource_group = 'myresourcegroup'
cluster_name = 'mycluster'

# Attach the cluster to your workgroup
attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                         cluster_name = cluster_name)
aks_target = ComputeTarget.attach(ws, 'mycompute', attach_config)
```

## <a name="consume-web-services"></a>Web hizmetlerini kullanma
Çeşitli programlama dillerini istemci uygulamaları oluşturmak için her dağıtılan web hizmeti bir REST API'si sağlar. Hizmetiniz için kimlik doğrulamasını etkinleştirdiyseniz, istek üstbilgisindeki bir belirteç olarak bir hizmet anahtarı sağlamanız gerekir.

Python hizmetinizdeki çağırmak nasıl bir örnek aşağıda verilmiştir:
```python
import requests
import json

headers = {'Content-Type':'application/json'}

if service.auth_enabled:
    headers['Authorization'] = 'Bearer '+service.get_keys()[0]

print(headers)
    
test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10], 
    [10,9,8,7,6,5,4,3,2,1]
]})

response = requests.post(service.scoring_uri, data=test_sample, headers=headers)
print(response.status_code)
print(response.elapsed)
print(response.json())
```

Daha fazla bilgi için [istemci uygulamalarının webservices'a kullanması için oluşturma](how-to-consume-web-service.md).

## <a id="update"></a> Web hizmetini güncelleştirmek

Yeni bir modeli oluşturduğunuzda, yeni modeli kullanmak istediğiniz her hizmeti el ile güncelleştirmeniz gerekir. Web hizmetini güncelleştirmek için `update` yöntemi. Aşağıdaki kod, yeni modeli kullanmak için web hizmetini güncelleştirmek gösterilmektedir:

```python
from azureml.core.webservice import Webservice
from azureml.core.model import Model

# register new model
new_model = Model.register(model_path = "outputs/sklearn_mnist_model.pkl",
                       model_name = "sklearn_mnist",
                       tags = {"key": "0.1"},
                       description = "test",
                       workspace = ws)

service_name = 'myservice'
# Retrieve existing service
service = Webservice(name = service_name, workspace = ws)

# Update to new model(s)
service.update(models = [new_model])
print(service.state)
print(service.get_logs())
```

## <a name="clean-up"></a>Temizleme
Dağıtılmış bir web hizmetini silmek için kullanın `service.delete()`.
Kayıtlı bir model silmek için kullanın `model.delete()`.

Daha fazla bilgi için başvuru belgeleri için bkz. [WebService.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#delete--), ve [Model.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#delete--).

## Gelişmiş Yapılandırma ayarları <a id="advanced-config"></a>

### <a id="customimage"></a> Özel temel görüntü kullanma

Dahili olarak InferenceConfig model ve hizmet tarafından gerekli olan diğer varlıkları içeren bir Docker görüntüsü oluşturur. Belirtilmezse, varsayılan temel görüntü kullanılır.

Çıkarım yapılandırmanızı ile kullanılacak bir görüntü oluştururken, görüntünün aşağıdaki gereksinimleri karşılaması gerekir:

* Ubuntu 16.04 veya büyük.
* Conda 4.5. # veya büyük.
* Python 3.5. # veya 3.6. #.

Özel görüntü kullanmak için ayarlanmış `base_image` görüntünün adresine çıkarımı yapılandırmanın özelliği. Aşağıdaki örnek, hem bir genel ve özel Azure kapsayıcısı kayıt defterinden bir görüntüyü kullanmak gösterilmektedir:

```python
# use an image available in public Container Registry without authentication
inference_config.base_image = "mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda"

# or, use an image available in a private Container Registry
inference_config.base_image = "myregistry.azurecr.io/mycustomimage:1.0"
inference_config.base_image_registry.address = "myregistry.azurecr.io"
inference_config.base_image_registry.username = "username"
inference_config.base_image_registry.password = "password"
```

Aşağıdaki görüntüde URI'ler Microsoft tarafından sağlanan görüntüleri içindir ve kullanıcı adı veya parola değerini sağlamadan kullanılabilir:

* `mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda`
* `mcr.microsoft.com/azureml/onnxruntime:v0.4.0`
* `mcr.microsoft.com/azureml/onnxruntime:v0.4.0-cuda10.0-cudnn7`
* `mcr.microsoft.com/azureml/onnxruntime:v0.4.0-tensorrt19.03`

Bu görüntüleri kullanmak için ayarlanmış `base_image` Yukarıdaki listeden bir URI. Ayarlama `base_image_registry.address` için `mcr.microsoft.com`.

> [!IMPORTANT]
> CUDA veya TensorRT kullanan Microsoft görüntüleri yalnızca Microsoft Azure hizmetleri üzerinde kullanılmalıdır.

Kendi görüntülerinizi Azure Container Registry'ye yükleme ile ilgili daha fazla bilgi için bkz: [özel bir Docker kapsayıcı kayıt defterine ilk görüntünüzü itme](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-docker-cli).

Azure Machine Learning işlem modelinizi eğitildi kullanıyorsa __1.0.22 sürüm veya daha büyük__ Azure Machine Learning SDK'sının eğitim sırasında bir görüntü oluşturulur. Aşağıdaki örnek, bu görüntünün nasıl kullanılacağını gösterir:

```python
# Use an image built during training with SDK 1.0.22 or greater
image_config.base_image = run.properties["AzureML.DerivedImageName"]
```

## <a name="other-inference-options"></a>Diğer çıkarımı seçenekleri

### <a id="azuremlcompute"></a> Batch çıkarımı
Azure Machine Learning işlem hedefleri oluşturulur ve Azure Machine Learning hizmeti tarafından yönetilir. Azure Machine Learning işlem hatlarını gelen toplu tahmin için kullanılabilir.

Bir Azure Machine Learning işlem ile batch çıkarım kılavuzu için okuma [Batch Öngörüler çalıştırma nasıl](how-to-run-batch-predictions.md) makalesi.

## <a id="iotedge"></a> IOT Edge üzerinde çıkarımı
Uca dağıtma desteği Önizleme aşamasındadır. Daha fazla bilgi için [Azure Machine Learning bir IOT Edge modülü olarak dağıtma](https://docs.microsoft.com/en-us/azure/iot-edge/tutorial-deploy-machine-learning) makalesi.

## <a name="next-steps"></a>Sonraki adımlar
* [Dağıtım sorunlarını giderme](how-to-troubleshoot-deployment.md)
* [Azure Machine Learning web hizmetleri SSL ile güvenli hale getirme](how-to-secure-web-service.md)
* [Bir web hizmeti olarak ML modeli kullanma](how-to-consume-web-service.md)
* [Azure Machine Learning Modellerinizi Application Insights ile izleme](how-to-enable-app-insights.md)
* [Üretimde modelleri için veri toplama](how-to-enable-data-collection.md)
