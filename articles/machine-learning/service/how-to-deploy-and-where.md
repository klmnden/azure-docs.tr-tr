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
ms.date: 05/31/2019
ms.custom: seoapril2019
ms.openlocfilehash: dcb90eb8ee25b8b0c780006f3555a5a9b815ffdd
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67514250"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Azure Machine Learning hizmeti ile modelleri dağıtma

Machine learning modeli Azure bulutta veya IOT Edge cihazları için bir web hizmeti olarak dağıtmayı öğrenin. 

İş akışı bağımsız olarak, benzer [olduğu dağıttığınızda](#target) modelinizi:

1. Modeli kaydedin.
1. Dağıtmaya hazırlanma (varlıklar, kullanım, hedef işlem belirt)
1. Model işlem hedefine dağıtın.
1. Web hizmeti olarak da bilinir dağıtılan modeli test edin.

Dağıtım iş akışı içinde ilgili kavramları hakkında daha fazla bilgi için bkz. [yönetin, dağıtın ve izleyin modeller Azure Machine Learning hizmeti ile](concept-model-management-and-deployment.md).

## <a name="prerequisites"></a>Önkoşullar

- Bir model. Eğitilen bir modelin izniniz yok, modeli kullandığınız & bağımlılığı dosyaları sağlanan içinde [Bu öğreticide](https://aka.ms/azml-deploy-cloud).

- [Machine Learning hizmeti için Azure CLI uzantısı](reference-azure-machine-learning-cli.md), [Azure Machine Learning Python SDK'sı](https://aka.ms/aml-sdk), veya [Azure Machine Learning Visual Studio Code uzantısı](how-to-vscode-tools.md).

## <a id="registermodel"></a> Modelinizi kaydetme

Modelinizi bir veya daha fazla dosyaların için kayıtlı modeli mantıksal kapsayıcı. Örneğin, birden çok dosyasında depolanan bir model varsa, bunları çalışma alanında tek bir model olarak kaydedebilirsiniz. Kayıt sonrasında sonra indirin veya kayıtlı modeli dağıtabilir ve kaydedilmiş tüm dosyalar alırsınız.

Makine öğrenimi modellerini Azure Machine Learning çalışma alanınızda kaydedilir. Azure Machine Learning hizmetinden gelebilir veya başka bir yere gelebilir. Aşağıdaki örnekler, bir model dosyasından kaydetme göstermektedir:

### <a name="register-a-model-from-an-experiment-run"></a>Denemeyi çalıştırma modelden kaydetme

+ **SDK'sını kullanarak Scikit-öğrenme örneği**
  ```python
  model = run.register_model(model_name='sklearn_mnist', model_path='outputs/sklearn_mnist_model.pkl')
  print(model.name, model.id, model.version, sep='\t')
  ```

  > [!TIP]
  > Birden çok dosya model kaydı içerecek şekilde, `model_path` dosyaları içeren dizine.

+ **CLI kullanarak**

  ```azurecli-interactive
  az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment
  ```

  > [!TIP]
  > Birden çok dosya model kaydı içerecek şekilde, `--asset-path` dosyaları içeren dizine.

+ **VS Code'u kullanarak**

  Herhangi bir model dosyaları veya klasörleri kullanarak modelleri kaydetme [VS Code](how-to-vscode-tools.md#deploy-and-manage-models) uzantısı.

### <a name="register-an-externally-created-model"></a>Harici olarak oluşturulmuş bir modeli kaydedin

[!INCLUDE [trusted models](../../../includes/machine-learning-service-trusted-model.md)]

Harici olarak oluşturulmuş bir model sunarak kaydedebileceğiniz bir **yerel yol** modeli. Bir klasör veya tek bir dosyayı sağlayabilir.

+ **Python SDK'sı ile ONNX örnek:**
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

  > [!TIP]
  > Birden çok dosya model kaydı içerecek şekilde, `model_path` dosyaları içeren dizine.

+ **CLI kullanarak**
  ```azurecli-interactive
  az ml model register -n onnx_mnist -p mnist/model.onnx
  ```

  > [!TIP]
  > Birden çok dosya model kaydı içerecek şekilde, `-p` dosyaları içeren dizine.

**Tahmini Süre**: Yaklaşık 10 saniye.

Daha fazla bilgi için başvuru belgeleri için bkz. [Model sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

Dış Azure Machine Learning hizmeti modelleriyle çalışma hakkında daha fazla bilgi eğitim için bkz: [mevcut bir model dağıtma](how-to-deploy-existing-model.md).

<a name="target"></a>

## <a name="choose-a-compute-target"></a>İşlem hedefi seçin

Aşağıdaki hedefleri, işlem veya işlem kaynakları, web hizmeti dağıtımınız barındırmak için kullanılabilir. 

[!INCLUDE [aml-compute-target-deploy](../../../includes/aml-compute-target-deploy.md)]

## <a name="prepare-to-deploy"></a>Dağıtmaya hazırlanma

Bir web hizmeti olarak dağıtalım için çıkarım yapılandırması oluşturun (`InferenceConfig`) ve bir dağıtım yapılandırması. Çıkarım veya Puanlama modeli, dağıtılan model için tahmin, üretim veri çubuğunda en yaygın olarak kullanıldığı aşamasıdır. Çıkarım yapılandırmada modelinizi sunmak için gerekli bağımlılıkları ve betikleri belirtin. Dağıtım yapılandırması nasıl işlem hedef model hizmet ayrıntılarını belirtin.


### <a id="script"></a> 1. Giriş betik & bağımlılıkları tanımlayın

Giriş betik, dağıtılan web hizmetine gönderilen verileri alır ve modele geçirir. Sonra modeli tarafından döndürülen yanıtı alır ve istemciye döndürür. **Kendi modeline özgü olduğundan ve betiğin**; model bekliyor ve döndüren veri anlamanız gerekir.

Betik, yükleme ve çalıştırmayı iki işlev içerir:

* `init()`: Genellikle bu işlev, genel bir nesnesine modeli yükler. Bu işlev, yalnızca web hizmeti için Docker kapsayıcı başlatıldığında bir kez çalıştırılır.

* `run(input_data)`: Bu işlev, giriş verileri temel alan bir değer tahmin modelini kullanır. Genellikle girişler ve çıkışlar farklı çalıştır JSON seri hale getirme ve serinin için kullanın. Ayrıca, ham ikili verileri ile çalışabilirsiniz. Veri modeline göndermeden önce veya istemciye döndürmeden önce dönüştürebilirsiniz.

#### <a name="what-is-getmodelpath"></a>Get_model_path nedir?

Bir modeli kaydettiğinizde, kayıt defteri modelde yönetmek için kullanılan bir model adı sağlayın. Bu ada sahip kullandığınız [Model.get_model_path()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#get-model-path-model-name--version-none---workspace-none-) model dosyaları yerel dosya sistemindeki yolunu almak için. Bu API, bir klasör veya dosyaları koleksiyonunu kaydederseniz, bu dosyaları içeren dizine yolunu döndürür.

Bir modeli kaydettiğinizde, bu model, yerel olarak veya hizmet dağıtımı sırasında yerleştirildiği için karşılık gelen bir ad verin.

Aşağıdaki örnekte bir yol tek dosya adlı döndüreceği `sklearn_mnist_model.pkl` (adıyla kaydedildi `sklearn_mnist`):

```python
model_path = Model.get_model_path('sklearn_mnist')
``` 

#### <a name="optional-automatic-swagger-schema-generation"></a>(İsteğe bağlı) Otomatik Swagger şema oluşturma

Otomatik olarak web hizmetiniz için bir şema oluşturmak, giriş örneği sağlar ve/veya çıkış için tanımlanan bir tür nesne türü ve örnek oluşturucusu otomatik olarak şema oluşturmak için kullanılır. Azure Machine Learning hizmeti daha sonra oluşturur bir [Openapı](https://swagger.io/docs/specification/about/) dağıtımı sırasında web hizmeti için (Swagger) belirtimi.

Aşağıdaki türleri şu anda desteklenir:

* `pandas`
* `numpy`
* `pyspark`
* Standart Python nesnesi

Şeması oluşturma kullanmak için dahil `inference-schema` conda ortam dosyanızdaki Paket. Aşağıdaki örnekte `[numpy-support]` giriş betik numpy parametre türü kullandığından: 

#### <a name="example-dependencies-file"></a>Örnek bağımlılıklar dosyası
Aşağıdaki YAML çıkarımı için Conda bağımlılıkları dosyasının örneğidir.

```YAML
name: project_environment
dependencies:
  - python=3.6.2
  - pip:
    - azureml-defaults
    - scikit-learn==0.20.0
    - inference-schema[numpy-support]
```

Otomatik şeması oluşturma, giriş komut dosyanızı kullanmak istiyorsanız **gerekir** alma `inference-schema` paketleri. 

Giriş tanımlayın ve örnek biçimlerde çıkış `input_sample` ve `output_sample` değişkenleri, web hizmeti için istek ve yanıt formatları temsil eder. Girdide bu örnekleri kullanın ve işlev dekoratörler üzerinde çıkışını `run()` işlevi. Scikit-aşağıdaki örnekte şeması oluşturma kullandığını öğrenin.

> [!TIP]
> Hizmet dağıtıldıktan sonra kullanın `swagger_uri` şema JSON belgesi alınacağını özelliği.

#### <a name="example-entry-script"></a>Örnek giriş betiği

Aşağıdaki örnek, kabul edin ve JSON verilerini döndürmek gösterilmektedir:

```python
#example: scikit-learn and Swagger
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

#### <a name="example-script-with-dictionary-input-support-consumption-from-power-bi"></a>Sözlük girişi (Power BI destek tüketim) ile örnek betiği

Aşağıdaki örnek, girdi verisi olarak tanımlamak gösterilmiştir < anahtar: değer > veri çerçevesini kullanarak sözlük. Bu yöntem dağıtılan web hizmetinden Power BI'ı kullanma için desteklenir ([Power BI web hizmetini kullanma hakkında daha fazla edinin](https://docs.microsoft.com/power-bi/service-machine-learning-integration)):

```python
import json
import pickle
import numpy as np
import pandas as pd
import azureml.train.automl
from sklearn.externals import joblib
from azureml.core.model import Model

from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.numpy_parameter_type import NumpyParameterType
from inference_schema.parameter_types.pandas_parameter_type import PandasParameterType

def init():
    global model
    model_path = Model.get_model_path('model_name')   # replace model_name with your actual model name, if needed
    # deserialize the model file back into a sklearn model
    model = joblib.load(model_path)

input_sample = pd.DataFrame(data=[{
              "input_name_1": 5.1,         # This is a decimal type sample. Use the data type that reflects this column in your data
              "input_name_2": "value2",    # This is a string type sample. Use the data type that reflects this column in your data
              "input_name_3": 3            # This is a integer type sample. Use the data type that reflects this column in your data
            }])

output_sample = np.array([0])              # This is a integer type sample. Use the data type that reflects the expected result

@input_schema('data', PandasParameterType(input_sample))
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

Çıkarım yapılandırma Öngörüler bulunmak üzere modelinizi yapılandırılması açıklanmaktadır. Aşağıdaki örnek, çıkarım yapılandırmasının nasıl oluşturulacağını gösterir. Bu yapılandırma, çalışma zamanı, giriş betik ve conda ortam dosyası (isteğe bağlı olarak) belirtir:

```python
inference_config = InferenceConfig(runtime= "python",
                                   entry_script="x/y/score.py",
                                   conda_file="env/myenv.yml")
```

Daha fazla bilgi için [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) sınıf başvurusu.

Çıkarım yapılandırmasıyla özel Docker görüntüsü kullanma hakkında daha fazla bilgi için bkz. [özel Docker görüntüsü kullanarak bir model dağıtma](how-to-deploy-custom-docker-image.md).

### <a name="cli-example-of-inferenceconfig"></a>CLI örneği InferenceConfig

Aşağıdaki JSON belgesi, machine learning CLI ile kullanmak için örnek bir çıkarımı yapılandırma verilmiştir:

```JSON
{
   "entryScript": "x/y/score.py",
   "runtime": "python",
   "condaFile": "env/myenv.yml",
   "sourceDirectory":"C:/abc",
}
```

Bu dosyada, aşağıdaki varlıkları geçerlidir:

* __entryScript__: Görüntü için çalıştırılacak kodu içeren yerel dosya yolu.
* __Çalışma zamanı__: Görüntüyü kullanmak için hangi çalışma zamanı. Geçerli desteklenen çalışma zamanları şunlardır: 'spark-py' ve 'python'.
* __condaFile__ (isteğe bağlı): Görüntü için kullanılacak bir conda ortam tanımı içeren yerel dosya yolu.
* __extraDockerFileSteps__ (isteğe bağlı): Görüntüyü oluşturan ayarlarken çalıştırmak için ek Docker adımlar içeren yerel dosya yolu.
* __sourceDirectory__ (isteğe bağlı): Görüntüyü oluşturmak için tüm dosyaları içeren klasörlere yolu.
* __enableGpu__ (isteğe bağlı): GPU etkinleştirme gerekip gerekmediğini, görüntüyü destekler. GPU görüntüyü Azure Container Instances, Azure Machine Learning işlem, Azure sanal makineler ve Azure Kubernetes hizmeti gibi Microsoft Azure Hizmetleri kullanılmalıdır. Varsayılan değeri False'tur.
* __baseImage__ (isteğe bağlı): Özel bir görüntü, temel görüntü olarak kullanılacak. Temel görüntü belirtilmezse, temel görüntü kapatıp belirli bir çalışma zamanı parametre tabanlı kullanılır.
* __baseImageRegistry__ (isteğe bağlı): Temel görüntü içeren görüntü kayıt.
* __cudaVersion__ (isteğe bağlı): GPU desteğe ihtiyaç duyan görüntüler için yüklemek için CUDA sürümü. GPU görüntüyü Azure Container Instances, Azure Machine Learning işlem, Azure sanal makineler ve Azure Kubernetes hizmeti gibi Microsoft Azure Hizmetleri kullanılmalıdır. Desteklenen sürümler 9.0 9.1 ve 10.0 ' dir. 'Enable_gpu' olarak ayarlanırsa '9.1 için' varsayılan olarak.

Bu varlıklar için parametreleri eşleme [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) sınıfı.

Aşağıdaki komut üç rol CLI kullanarak bir model dağıtma gösterilmektedir:

```azurecli-interactive
az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json
```

Bu örnekte, yapılandırma aşağıdaki öğeleri içerir:

* Çıkarım için gerekli varlıkları içeren bir dizin
* Bu model Python gerektirir
* [Giriş betik](#script), dağıtılmış hizmette gönderilen web isteklerini işlemek için kullanılır
* Çıkarım için gereken Python paketlerini tanımlayan conda dosyası

Çıkarım yapılandırmasıyla özel Docker görüntüsü kullanma hakkında daha fazla bilgi için bkz. [özel Docker görüntüsü kullanarak bir model dağıtma](how-to-deploy-custom-docker-image.md).

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

### <a name="optional-profile-your-model"></a>İsteğe bağlı: Modelinizi profil
Modelinizi bir hizmeti dağıtmadan önce en iyi CPU ve bellek gereksinimlerini belirlemek için profil isteyebilirsiniz. CLI veya SDK'sını kullanarak modelinizi profili yapabilirsiniz.

Daha fazla bilgi için SDK'sı belgelerimize burada kontrol edebilirsiniz: https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#profile-workspace--profile-name--models--inference-config--input-data-

Model profil oluşturma sonuçları bir çalışma nesnesi olarak gönderilir.
Modeli profili şemadaki ayrıntıları şurada bulunabilir: https://docs.microsoft.com/python/api/azureml-core/azureml.core.profile.modelprofile?view=azure-ml-py

## <a name="deploy-to-target"></a>Hedefe dağıtım

### <a id="local"></a> Yerel dağıtım

Yerel olarak dağıtmak için ihtiyacınız **Docker'ın yüklü** yerel makinenizde.

+ **SDK'sını kullanma**

  ```python
  deployment_config = LocalWebservice.deploy_configuration(port=8890)
  service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config)
  service.wait_for_deployment(show_output = True)
  print(service.state)
  ```

+ **CLI kullanarak**

  ```azurecli-interactive
  az ml model deploy -m sklearn_mnist:1 -ic inferenceconfig.json -dc deploymentconfig.json
  ```

### <a id="aci"></a> Azure Container Instances (DEVTEST)

Bir web hizmeti bir veya daha aşağıdaki koşullardan biri Modellerinizi dağıtmak için Azure Container Instances kullanmak doğrudur:
- Hızlı bir şekilde dağıtın ve modelinizi doğrulama gerekir.
- Geliştirilmekte olan bir modeli test edersiniz. 

ACI için kotaları ve bölge kullanılabilirliği görmek için bkz [kotaları ve Azure Container Instances için bölge kullanılabilirliği](https://docs.microsoft.com/azure/container-instances/container-instances-quotas) makalesi.

+ **SDK'sını kullanma**

  ```python
  deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
  service = Model.deploy(ws, "aciservice", [model], inference_config, deployment_config)
  service.wait_for_deployment(show_output = True)
  print(service.state)
  ```

+ **CLI kullanarak**

  ```azurecli-interactive
  az ml model deploy -m sklearn_mnist:1 -n aciservice -ic inferenceconfig.json -dc deploymentconfig.json
  ```


+ **VS Code'u kullanarak**

  İçin [VS Code ile Modellerinizi dağıtın](how-to-vscode-tools.md#deploy-and-manage-models) ACI kapsayıcı çalışma sırasında oluşturulur çünkü önceden test etmek için bir ACI kapsayıcı oluşturmanıza gerek kalmaz.

Daha fazla bilgi için başvuru belgeleri için bkz. [AciWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py) ve [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py) sınıfları.

### <a id="aks"></a>Azure Kubernetes hizmeti (geliştirme ve test ve üretim)

Mevcut bir AKS kümesi kullanmak veya Azure Machine Learning SDK'sı, CLI veya Azure portalını kullanarak yeni bir tane oluşturun.

<a id="deploy-aks"></a>

Ekli bir AKS kümesi zaten varsa, kendisine dağıtabilirsiniz. Henüz oluşturduğunuz veya bir AKS kümesi bağlı işleme izleyin <a href="#create-attach-aks">yeni bir AKS kümesi oluşturma</a>.

+ **SDK'sını kullanma**

  ```python
  aks_target = AksCompute(ws,"myaks")
  # If deploying to a cluster configured for dev/test, ensure that it was created with enough
  # cores and memory to handle this deployment configuration. Note that memory is also used by
  # things such as dependencies and AML components.
  deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
  service = Model.deploy(ws, "aksservice", [model], inference_config, deployment_config, aks_target)
  service.wait_for_deployment(show_output = True)
  print(service.state)
  print(service.get_logs())
  ```

+ **CLI kullanarak**

  ```azurecli-interactive
  az ml model deploy -ct myaks -m mymodel:1 -n aksservice -ic inferenceconfig.json -dc deploymentconfig.json
  ```

+ **VS Code'u kullanarak**

  Ayrıca [VS Code uzantısı AKS'ye dağıtma](how-to-vscode-tools.md#deploy-and-manage-models), ancak AKS kümeleri önceden yapılandırmanız gerekecektir.

AKS dağıtımı ve otomatik olarak ölçeklendirme hakkında daha fazla bilgi edinin [AksWebservice.deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice) başvuru.

#### Yeni bir AKS kümesi oluşturma<a id="create-attach-aks"></a>
**Tahmini Süre**: Yaklaşık 20 dakika.

Oluşturma veya bir AKS kümesi tek bir süredir eklemek, çalışma alanınız için işler. Bu kümeye birden çok dağıtımlar için yeniden kullanabilirsiniz. Küme veya onu içeren kaynak grubunu silerseniz, yeni bir kümeye dağıtmak için gerektiğinde oluşturmanız gerekir. Çalışma alanınıza bağlı birden çok AKS kümesi olabilir.

Geliştirme, doğrulama ve sınama için AKS kümesi oluşturmak istiyorsanız, ayarladığınız `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST` kullanırken [ `provisioning_configuration()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py). Bu ayar bir kümesi, yalnızca bir düğümün sahip olur.

> [!IMPORTANT]
> Ayar `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST` üretim trafiği işlemek için uygun olmayan bir AKS kümesi oluşturur. Çıkarım kez üretim için oluşturulan bir kümede daha uzun olabilir. Hataya dayanıklılık ayrıca geliştirme ve test kümeleri için garanti edilmez.
>
> Geliştirme/test için oluşturulmuş kümeleri en az iki sanal CPU kullanmanızı öneririz.

Aşağıdaki örnek yeni bir Azure Kubernetes hizmeti kümesinin nasıl oluşturulacağını gösterir:

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Use the default configuration (you can also provide parameters to customize this).
# For example, to create a dev/test cluster, use:
# prov_config = AksCompute.provisioning_configuration(cluster_purpose = AksComputee.ClusterPurpose.DEV_TEST)
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

Daha fazla bilgi için `cluster_purpose` parametresi bkz [AksCompute.ClusterPurpose](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py) başvuru.

> [!IMPORTANT]
> İçin [ `provisioning_configuration()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py), büyüktür veya eşittir 12 sanal CPU'lara göre vm_size çarpılan agent_count emin olmanız gerekir daha sonra agent_count ve vm_size, özel değerleri seçin. Örneğin, bir vm_size 4 sanal CPU'lar varsa, "Standard_D3_v2" birini kullanırsanız, 3 veya daha büyük bir agent_count seçmeniz gerekir.
>
> Azure Machine Learning SDK'sı, AKS kümesini ölçeklendirme destek sağlamaz. Kümedeki düğümler ölçeklendirmek için Azure portalında AKS kümenizin kullanıcı arabirimini kullanın. Yalnızca VM boyutu değil kümenin düğüm sayısını değiştirebilirsiniz.

#### <a name="attach-an-existing-aks-cluster"></a>Mevcut bir AKS kümesi ekleme
**Tahmini süre:** Yaklaşık 5 dakika.

AKS kümesini Azure aboneliğinizde zaten ve sürüm 1.12. ##, görüntünüzü dağıtmak için kullanın.

> [!WARNING]
> Bir AKS kümesi bir çalışma alanına eklenirken ayarlayarak kümeyi nasıl kullanacağını tanımlayabilirsiniz `cluster_purpose` parametresi.
>
> Ayarlanmamış olması halinde `cluster_purpose` parametresi veya set `cluster_purpose = AksCompute.ClusterPurpose.FAST_PROD`, sonra kümeye en az 12 sanal CPU'lar kullanılabilir olması gerekir.
>
> Ayarlarsanız `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST`, sonra kümeyi 12 sanal CPU'lar olması gerekmez. Ancak, geliştirme/test için yapılandırılan bir kümeniz üretim düzeyinde trafik için uygun olmayacaktır ve çıkarım sürelerini artırabilir.

Aşağıdaki kod, mevcut bir AKS 1.12 eklemek gösterilmektedir. ## çalışma kümesi:

```python
from azureml.core.compute import AksCompute, ComputeTarget
# Set the resource group that contains the AKS cluster and the cluster name
resource_group = 'myresourcegroup'
cluster_name = 'mycluster'

# Attach the cluster to your workgroup. If the cluster has less than 12 virtual CPUs, use the following instead:
# attach_config = AksCompute.attach_configuration(resource_group = resource_group,
#                                         cluster_name = cluster_name,
#                                         cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST)
attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                         cluster_name = cluster_name)
aks_target = ComputeTarget.attach(ws, 'mycompute', attach_config)
```

Daha fazla bilgi için `attack_configuration()`, bkz: [AksCompute.attach_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-) başvuru.

Daha fazla bilgi için `cluster_purpose` parametresi bkz [AksCompute.ClusterPurpose](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py) başvuru.

## <a name="consume-web-services"></a>Web hizmetlerini kullanma

Çeşitli programlama dillerini istemci uygulamaları oluşturmak için her dağıtılan web hizmeti bir REST API'si sağlar. Hizmetiniz için kimlik doğrulamasını etkinleştirdiyseniz, istek üstbilgisindeki bir belirteç olarak bir hizmet anahtarı sağlamanız gerekir.

### <a name="request-response-consumption"></a>İstek-yanıt tüketim

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


### <a id="azuremlcompute"></a> Batch çıkarımı
Azure Machine Learning işlem hedefleri oluşturulur ve Azure Machine Learning hizmeti tarafından yönetilir. Azure Machine Learning işlem hatlarını gelen toplu tahmin için kullanılabilir.

Bir Azure Machine Learning işlem ile batch çıkarım kılavuzu için okuma [Batch Öngörüler çalıştırma nasıl](how-to-run-batch-predictions.md) makalesi.

### <a id="iotedge"></a> IOT Edge çıkarımı
Uca dağıtma desteği Önizleme aşamasındadır. Daha fazla bilgi için [Azure Machine Learning bir IOT Edge modülü olarak dağıtma](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning) makalesi.


## <a id="update"></a> Web Hizmetleri güncelleştirmesi

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

## <a name="continuous-model-deployment"></a>Sürekli model dağıtımı 

Modelleri için Machine Learning uzantısını kullanarak sürekli dağıtım yapabilirsiniz [Azure DevOps](https://azure.microsoft.com/services/devops/). Azure DevOps için Machine Learning uzantısı kullanarak, Azure Machine Learning hizmeti çalışma alanında yeni bir machine learning modeli kaydedildiğinde bir dağıtım işlem hattı tetikleyebilirsiniz. 

1. Kaydolun [Azure işlem hatları](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops), hangi mümkün kılar sürekli tümleştirme ve teslim bulut uygulamanızın herhangi bir platform/any. Azure işlem hatları [ML ardışık düzen ' farklı](concept-ml-pipelines.md#compare). 

1. [Azure DevOps projesi oluşturun.](https://docs.microsoft.com/azure/devops/organizations/projects/create-project?view=azure-devops)

1. Yükleme [Azure işlem hatları için Machine Learning uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-air-aiagility.vss-services-azureml&targetId=6756afbe-7032-4a36-9cb6-2771710cadc2&utm_source=vstsproduct&utm_medium=ExtHubManageList) 

1. Kullanım __bağlantılara hizmet__ tüm yapıtlar erişmek için Azure Machine Learning hizmeti çalışma alanında bir hizmet sorumlusu bağlantı ayarlamak için. Proje Ayarları'na gidin, hizmet bağlantıları ve Azure Resource Manager'ı seçin.

    ![Görünüm hizmet bağlantısı](media/how-to-deploy-and-where/view-service-connection.png) 

1. AzureMLWorkspace olarak tanımlamak __kapsam düzeyi__ ve sonraki parametreleri doldurun.

    ![azure resource manager görüntüle](media/how-to-deploy-and-where/resource-manager-connection.png)

1. Ardından, Azure işlem hatları kullanarak makine öğrenimi modelinizi sürekli olarak dağıtmak için işlem hatları altında seçin __yayın__. Yeni yapıt ekleme, önceki adımda oluşturulan hizmet bağlantı ve AzureML modeli yapıt seçin. Bir dağıtımı tetiklemek için sürümü ve modeli seçin. 

    ![Select AzureMLmodel yapıt](media/how-to-deploy-and-where/enable-modeltrigger-artifact.png)

1. Model, model yapıt tetikleyici etkinleştirin. Tetikleyici, her zaman belirtilen sürümü (yani açarak en yeni sürümü) çalışma alanınızda Bu modelin kaydıdır, bir Azure DevOps yayın işlem hattı tetiklenir. 

    ![etkinleştirme modeli tetikleyicisi](media/how-to-deploy-and-where/set-modeltrigger.png)

Örnek projeler ve örnekler için kullanıma [MLOps depo](https://github.com/Microsoft/MLOps)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Dağıtılmış bir web hizmetini silmek için kullanın `service.delete()`.
Kayıtlı bir model silmek için kullanın `model.delete()`.

Daha fazla bilgi için başvuru belgeleri için bkz. [WebService.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#delete--), ve [Model.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#delete--).

## <a name="next-steps"></a>Sonraki adımlar
* [Özel Docker görüntüsü kullanarak bir model dağıtma](how-to-deploy-custom-docker-image.md)
* [Dağıtım sorunlarını giderme](how-to-troubleshoot-deployment.md)
* [Azure Machine Learning web hizmetleri SSL ile güvenli hale getirme](how-to-secure-web-service.md)
* [Bir web hizmeti olarak ML modeli kullanma](how-to-consume-web-service.md)
* [Azure Machine Learning Modellerinizi Application Insights ile izleme](how-to-enable-app-insights.md)
* [Üretimde modelleri için veri toplama](how-to-enable-data-collection.md)

