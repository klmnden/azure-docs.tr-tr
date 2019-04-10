---
title: Nasıl ve nerede modelleri dağıtma
titleSuffix: Azure Machine Learning service
description: 'Nasıl ve nerede bilgi dahil olmak üzere Azure Machine Learning hizmeti Modellerinizi dağıtmak için: Azure Container Instances, Azure Kubernetes hizmeti, Azure IOT Edge ve alanda programlanabilir kapı dizileri.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 04/02/2019
ms.custom: seoapril2019
ms.openlocfilehash: a6ef53d56fa293791658b37b16cbaff94aee6ef3
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59280902"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Azure Machine Learning hizmeti ile modelleri dağıtma

Bu belgede, modelinizin Azure bulutta veya IOT Edge cihazları için bir web hizmeti olarak dağıtmayı öğrenin. 

## <a name="compute-targets-for-deployment"></a>Dağıtım için hedef işlem

Azure Machine Learning SDK'sı, eğitilen model aşağıdaki konumlara dağıtmak için kullanın:

| Hedef işlem | Dağıtım türü | Açıklama |
| ----- | ----- | ----- |
| [Azure Kubernetes Hizmeti (AKS)](#aks) | Gerçek zamanlı çıkarımı | Büyük ölçekli üretim dağıtımları için idealdir. Otomatik ölçeklendirme ve hızlı yanıt süresi sağlar. |
| [Azure Machine Learning işlem (amlcompute)](#azuremlcompute) | Batch çıkarımı | Batch tahmin, sunucusuz bir işlem üzerinde çalıştırın. Normal veya düşük öncelikli sanal makineleri destekler. |
| [Azure Container Instances (ACI)](#aci) | Test Etme | Geliştirme veya test için iyidir. **Üretim iş yükleri için uygun değildir.** |
| [Azure IoT Edge](#iotedge) | (Önizleme) IOT Modülü | IOT cihazlarında modelleri dağıtın. Çıkarım cihazda'olmuyor. |
| [Alanda programlanabilir kapı dizileri (FPGA)](#fpga) | (Önizleme) Web hizmeti | Gerçek zamanlı çıkarım için son derece düşük gecikme süresi. |

## <a name="deployment-workflow"></a>Dağıtım iş akışı

Tüm işlem hedeflerine yönelik bir model dağıtma işlemini benzer:

1. Eğitme ve modeli kaydedin.
1. Yapılandırma ve modeli kullanan bir görüntüyü kaydedin.
1. Resim bir işlem hedefine dağıtın.
1. Dağıtımı test etme

Aşağıdaki videoda, Azure Container Instances'a dağıtma gösterilmektedir:

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Kwk3]


Dağıtım iş akışı içinde ilgili kavramları hakkında daha fazla bilgi için bkz. [yönetin, dağıtın ve izleyin modeller Azure Machine Learning hizmeti ile](concept-model-management-and-deployment.md).

## <a name="prerequisites-for-deployment"></a>Dağıtım önkoşulları

[!INCLUDE [aml-prereq](../../../includes/aml-prereq.md)]

- Eğitilen bir modeli. Eğitilen bir modelin yoksa içindeki adımları kullanın [eğitme modelleri](tutorial-train-models-with-aml.md) eğitmek ve bir Azure Machine Learning hizmeti ile kaydetme öğretici.

    > [!NOTE]
    > Azure Machine Learning hizmeti ile Python 3'te yüklenebilen herhangi bir genel model çalışabilir ancak bu belgedeki örnekler Python pickle biçiminde depolanan bir modeli kullanarak gösterir.
    >
    > ONNX modelleri kullanma hakkında daha fazla bilgi için bkz. [ONNX ve Azure Machine Learning](how-to-build-deploy-onnx.md) belge.

## <a id="registermodel"></a> Eğitilen bir modeli kaydedin

Model kayıt depolamak ve eğitilen Modellerinizi Azure bulutunda düzenlemek için bir yoldur. Modeller Azure Machine Learning hizmeti çalışma alanınızda kaydedilir. Azure Machine Learning veya başka bir hizmet kullanarak modeli eğitilir. Aşağıdaki kod dosyasından bir modeli kaydedin, bir ad, etiketler ve bir açıklama ayarlayın gösterilmektedir:

```python
from azureml.core.model import Model

model = Model.register(model_path = "outputs/sklearn_mnist_model.pkl",
                       model_name = "sklearn_mnist",
                       tags = {"key": "0.1"},
                       description = "test",
                       workspace = ws)
```

**Tahmini Süre**: Yaklaşık 10 saniye.

Model kaydediliyor ilişkin bir örnek için bkz [görüntü sınıflandırıcı eğitme](tutorial-train-models-with-aml.md).

Daha fazla bilgi için başvuru belgeleri için bkz. [Model sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

## <a id="configureimage"></a> Oluşturma ve bir görüntüyü kaydedin

Dağıtılan modellerinde bir görüntü olarak paketlenir. Görüntü modeli çalıştırmak için gerekli olan bağımlılıklar içerir.

İçin **Azure Container Instance**, **Azure Kubernetes hizmeti**, ve **Azure IOT Edge** dağıtımları [azureml.core.image.ContainerImage](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py) sınıfı, bir görüntü yapılandırması oluşturmak için kullanılır. Görüntü yapılandırma, ardından yeni bir Docker görüntüsü oluşturmak için kullanılır.

Görüntü yapılandırması oluştururken kullanabilirsiniz bir __varsayılan görüntü__ Azure Machine Learning hizmeti tarafından sağlanan veya __özel görüntü__ sağladığınız.

Aşağıdaki kod, yeni bir görüntü yapılandırmasının nasıl oluşturulacağını gösterir:

```python
from azureml.core.image import ContainerImage

# Image configuration
image_config = ContainerImage.image_configuration(execution_script = "score.py",
                                                 runtime = "python",
                                                 conda_file = "myenv.yml"}
                                                 )
```

**Tahmini Süre**: Yaklaşık 10 saniye.

Bu örnekte aşağıdaki tabloda açıklanan önemli parametreleri:

| Parametre | Açıklama |
| ----- | ----- |
| `execution_script` | Hizmete gönderilen istekleri almak için kullanılan bir Python betiği belirtir. Bu örnekte, komut dosyası içinde yer alan `score.py` dosya. Daha fazla bilgi için [yürütme betik](#script) bölümü. |
| `runtime` | Görüntü Python kullandığını gösterir. Diğer seçenek `spark-py`, Apache Spark ile Python kullanır. |
| `conda_file` | Conda ortam dosyası sağlamak için kullanılır. Bu dosya, dağıtılmış bir modelinin conda ortamı tanımlar. Bu dosya oluşturma hakkında daha fazla bilgi için bkz. [bir ortam dosyası (myenv.yml) oluşturma](tutorial-deploy-models-with-aml.md#create-environment-file). |

Bir görüntü yapılandırması oluşturma örneği için bkz: [görüntü sınıflandırıcı dağıtma](tutorial-deploy-models-with-aml.md).

Daha fazla bilgi için başvuru belgeleri için bkz. [ContainerImage sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py)

### <a id="customimage"></a> Özel görüntü kullanma

Özel bir görüntü kullanırken, görüntünün aşağıdaki gereksinimleri karşılaması gerekir:

* Ubuntu 16.04 veya büyük.
* Conda 4.5. # veya büyük.
* Python 3.5. # veya 3.6. #.

Özel görüntü kullanmak için ayarlanmış `base_image` adresine görüntünün görüntü yapılandırmanın özelliği. Aşağıdaki örnek, hem bir genel ve özel Azure kapsayıcısı kayıt defterinden bir görüntüyü kullanmak gösterilmektedir:

```python
# use an image available in public Container Registry without authentication
image_config.base_image = "mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda"

# or, use an image available in a private Container Registry
image_config.base_image = "myregistry.azurecr.io/mycustomimage:1.0"
image_config.base_image_registry.address = "myregistry.azurecr.io"
image_config.base_image_registry.username = "username"
image_config.base_image_registry.password = "password"
```

Bir Azure Container Registry'ye görüntü karşıya daha fazla bilgi için bkz: [özel bir Docker kapsayıcı kayıt defterine ilk görüntünüzü itme](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-docker-cli).

Azure Machine Learning işlem modelinizi eğitildi kullanıyorsa __1.0.22 sürüm veya daha büyük__ Azure Machine Learning SDK'sının eğitim sırasında bir görüntü oluşturulur. Aşağıdaki örnek, bu görüntünün nasıl kullanılacağını gösterir:

```python
# Use an image built during training with SDK 1.0.22 or greater
image_config.base_image = run.properties["AzureML.DerivedImageName"]
```

### <a id="script"></a> Betik yürütme

Yürütme komut dağıtılan görüntüye gönderilen verileri alır ve modele geçirir. Sonra modeli tarafından döndürülen yanıtı alır ve istemciye döndürür. **Kendi modeline özgü olduğundan ve betiğin**; model bekliyor ve döndüren veri anlamanız gerekir. Bir görüntü sınıflandırma modeli ile çalışan bir örnek betik için bkz [görüntü sınıflandırıcı dağıtma](tutorial-deploy-models-with-aml.md).

Betik, yükleme ve çalıştırmayı iki işlev içerir:

* `init()`: Genellikle bu işlev, genel bir nesnesine modeli yükler. Bu işlev, Docker kapsayıcısı başlatıldığında tek bir kez çalıştırılır.

* `run(input_data)`: Bu işlev, giriş verileri temel alan bir değer tahmin modelini kullanır. Genellikle girişler ve çıkışlar farklı çalıştır JSON seri hale getirme ve serinin için kullanın. Ayrıca, ham ikili verileri ile çalışabilirsiniz. Veri modeline göndermeden önce veya istemciye döndürmeden önce dönüştürebilirsiniz.

#### <a name="working-with-json-data"></a>JSON verileriyle çalışma

Aşağıdaki örnek betik, kabul eder ve JSON verilerini döndürür. `run` İşlevi JSON verileri model bekler ve ardından döndürmeden önce JSON yanıtı dönüştürür bir biçime dönüştürür:

```python
%%writefile score.py
import json
import numpy as np
import os
import pickle
from sklearn.externals import joblib
from sklearn.linear_model import LogisticRegression
from azureml.core.model import Model

# load the model
def init():
    global model
    # retrieve the path to the model file using the model name
    model_path = Model.get_model_path('sklearn_mnist')
    model = joblib.load(model_path)

# Passes data to the model and returns the prediction
def run(raw_data):
    data = np.array(json.loads(raw_data)['data'])
    # make prediction
    y_hat = model.predict(data)
    return json.dumps(y_hat.tolist())
```

#### <a name="working-with-binary-data"></a>İkili verilerle çalışma

Modelinizi kabul ediyorsa __ikili verileri__, kullanın `AMLRequest`, `AMLResponse`, ve `rawhttp`. Aşağıdaki örnek betik, ikili verileri kabul eder ve POST istekleri için ters bayt döndürür. GET istekleri için yanıt gövdesinde tam URL'yi döndürür:

```python
from azureml.contrib.services.aml_request  import AMLRequest, rawhttp
from azureml.contrib.services.aml_response import AMLResponse

def init():
    print("This is init()")

# Accept and return binary data
@rawhttp
def run(request):
    print("This is run()")
    print("Request: [{0}]".format(request))
    # handle GET requests
    if request.method == 'GET':
        respBody = str.encode(request.full_path)
        return AMLResponse(respBody, 200)
    # handle POST requests
    elif request.method == 'POST':
        reqBody = request.get_data(False)
        respBody = bytearray(reqBody)
        respBody.reverse()
        respBody = bytes(respBody)
        return AMLResponse(respBody, 200)
    else:
        return AMLResponse("bad request", 500)
```

> [!IMPORTANT]
> `azureml.contrib` Ad değişiklikleri sık olarak hizmeti geliştirmek için çalışıyoruz. Bu nedenle, bu ad alanındaki herhangi bir şey önizleme olarak kabul ve tamamen Microsoft tarafından desteklenmiyor.
>
> Bu, yerel geliştirme ortamınıza test etmeniz, bileşenleri yükleyebilirsiniz `contrib` aşağıdaki komutu kullanarak ad alanı:
> ```shell
> pip install azureml-contrib-services
> ```

### <a id="createimage"></a> Bir görüntüyü kaydedin

Görüntü yapılandırması oluşturulduktan sonra görüntünün kaydetmek için kullanabilirsiniz. Bu görüntü, çalışma alanınız için kapsayıcı kayıt defterinde depolanır. Oluşturulduktan sonra birden fazla hizmet için aynı görüntüsü dağıtabilirsiniz.

```python
# Register the image from the image configuration
image = ContainerImage.create(name = "myimage",
                              models = [model], #this is the model object
                              image_config = image_config,
                              workspace = ws
                              )
```

**Tahmini Süre**: Yaklaşık 3 dakika.

Aynı ada sahip birden fazla görüntü kayıt yaptırdığınızda görüntüleri otomatik olarak tutulur. İlk resim gibi kayıtlı `myimage` kimliği atanır `myimage:1`. Sonraki bir görüntü olarak Kaydet `myimage`, yeni görüntüyü kimliğidir `myimage:2`.

Daha fazla bilgi için başvuru belgeleri için bkz. [ContainerImage sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py).

## <a id="deploy"></a> Bir web hizmeti olarak dağıtma

Dağıtıma aldığınızda, dağıttığınız işlem hedef bağlı olarak biraz farklı bir işlemdir. Bilgileri dağıtma hakkında bilgi edinmek için aşağıdaki bölümlerdeki kullanın:

| Hedef işlem | Dağıtım türü | Açıklama |
| ----- | ----- | ----- |
| [Azure Kubernetes Hizmeti (AKS)](#aks) | Web hizmeti (gerçek zamanlı çıkarımı)| Büyük ölçekli üretim dağıtımları için idealdir. Otomatik ölçeklendirme ve hızlı yanıt süresi sağlar. |
| [Azure ML işlemi](#azuremlcompute) | Web hizmeti (Batch çıkarımı)| Batch tahmin, sunucusuz bir işlem üzerinde çalıştırın. Normal veya düşük öncelikli sanal makineleri destekler. |
| [Azure Container Instances (ACI)](#aci) | Web hizmeti (geliştirme/test)| Geliştirme veya test için iyidir. **Üretim iş yükleri için uygun değildir.** |
| [Azure IoT Edge](#iotedge) | (Önizleme) IOT Modülü | IOT cihazlarında modelleri dağıtın. Çıkarım cihazda'olmuyor. |
| [Alanda programlanabilir kapı dizileri (FPGA)](#fpga) | (Önizleme) Web hizmeti | Gerçek zamanlı çıkarım için son derece düşük gecikme süresi. |

> [!IMPORTANT]
> Model bir web hizmeti olarak dağıtırken, çıkış noktaları arası kaynak paylaşımı (CORS) şu anda desteklenmiyor.

Bu bölümdeki örneklerde kullanım [deploy_from_image](https://docs.microsoft.com/en-us/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-from-model-workspace--name--models--image-config--deployment-config-none--deployment-target-none-), bir dağıtım yapmadan önce modeli ve görüntü kayıt gerektirir. Diğer dağıtım yöntemleri hakkında daha fazla bilgi için bkz. [dağıtma](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-workspace--name--model-paths--image-config--deployment-config-none--deployment-target-none-) ve [deploy_from_model](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-from-model-workspace--name--models--image-config--deployment-config-none--deployment-target-none-).

### <a id="aci"></a> Azure Container Instances'a (DEVTEST) dağıtma

Bir web hizmeti bir veya daha aşağıdaki koşullardan biri Modellerinizi dağıtmak için Azure Container Instances kullanmak doğrudur:

- Hızlı bir şekilde dağıtın ve modelinizi doğrulama gerekir. ACI dağıtım 5 dakikadan daha kısa bir süre içinde tamamlanır.
- Geliştirilmekte olan bir modeli test edersiniz. ACI için kotaları ve bölge kullanılabilirliği görmek için bkz [kotaları ve Azure Container Instances için bölge kullanılabilirliği](https://docs.microsoft.com/azure/container-instances/container-instances-quotas) belge.

Azure Container Instances'a dağıtmak için aşağıdaki adımları kullanın:

1. Dağıtım Yapılandırması tanımlayın. Bu yapılandırma, modelinizi gereksinimlerine bağlıdır. Aşağıdaki örnek, bir CPU Çekirdeği ve 1 GB bellek kullanan yapılandırması tanımlar:

    [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-deploy-to-aci/how-to-deploy-to-aci.py?name=configAci)]

2. Oluşturulan görüntüyü dağıtmak için [görüntüyü oluşturmaya](#createimage) bölümü bu belgede aşağıdaki kodu kullanın:

    [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-deploy-to-aci/how-to-deploy-to-aci.py?name=option3Deploy)]

    **Tahmini Süre**: Yaklaşık 5 dakika.

Daha fazla bilgi için başvuru belgeleri için bkz. [AciWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py) ve [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py) sınıfları.

### <a id="aks"></a> Azure Kubernetes Service'e (üretim) dağıtma

Modelinizi ölçekli üretim web hizmeti olarak dağıtmak için Azure Kubernetes Service (AKS) kullanın. Mevcut bir AKS kümesi kullanmak veya Azure Machine Learning SDK'sı, CLI veya Azure portalını kullanarak yeni bir tane oluşturun.

Olan bir AKS kümesi oluşturma işlemi için çalışma süresi. Bu kümeye birden çok dağıtımlar için yeniden kullanabilirsiniz.

> [!IMPORTANT]
> Kümeyi silmeniz halinde, sonraki açışınızda dağıtmanız gerekir. yeni bir küme oluşturmanız gerekir.

Azure Kubernetes hizmeti, aşağıdaki özellikleri sağlar:

* Otomatik ölçeklendirme
* Günlüğe kaydetme
* Model veri koleksiyonu
* Web hizmetlerinizi hızlı yanıt süresi
* TLS sonlandırma
* Authentication

#### <a name="autoscaling"></a>Otomatik ölçeklendirme

Otomatik ölçeklendirme ayarı denetlenebilir `autoscale_target_utilization`, `autoscale_min_replicas`, ve `autoscale_max_replicas` AKS için web hizmeti. Aşağıdaki örnek, otomatik ölçeklendirmeyi etkinleştirmek üzere gösterilmektedir:

```python
aks_config = AksWebservice.deploy_configuration(autoscale_enabled=True,
                                                autoscale_target_utilization=30,
                                                autoscale_min_replicas=1,
                                                autoscale_max_replicas=4)
```

Yukarı/Aşağı ölçeklendirme kararları geçerli kapsayıcı çoğaltmaları kullanımını dışına dayanır. (Bir istek işleme) meşgul olduğu yineleme sayısı ve toplam ayrılmış olan geçerli kullanımı geçerli yineleme sayısı. Hedef kullanım bu sayıyı aşarsa, daha sonra daha fazla çoğaltma oluşturulur. Ardından düşükse, çoğaltmaları azaltılır. Varsayılan olarak, hedef kullanımı % 70'tir.

Kararları çoğaltmaları eklemek için yapılan ve hızlı bir şekilde (yaklaşık 1 saniye) uygulanır. Uzun (yaklaşık 1 dakika) çoğaltmaları kaldırmak için kararlar alın. Başa çıkabilir yeni istekler geldiğinde durumunda bu davranışı bir dakika boşta çoğaltmaları etrafında tutar.

Aşağıdaki kodu kullanarak gerekli çoğaltmaları hesaplayabilirsiniz:

```python
from math import ceil
# target requests per second
targetRps = 20
# time to process the request (in seconds)
reqTime = 10
# Maximum requests per container
maxReqPerContainer = 1
# target_utilization. 70% in this example
targetUtilization = .7

concurrentRequests = targetRps * reqTime / targetUtilization

# Number of container replicas
replicas = ceil(concurrentRequests / maxReqPerContainer)
```

Ayarı hakkında daha fazla bilgi için `autoscale_target_utilization`, `autoscale_max_replicas`, ve `autoscale_min_replicas`, bkz: [AksWebservice.deploy_configuration](https://docs.microsoft.com/en-us/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py#deploy-configuration-autoscale-enabled-none--autoscale-min-replicas-none--autoscale-max-replicas-none--autoscale-refresh-seconds-none--autoscale-target-utilization-none--collect-model-data-none--auth-enabled-none--cpu-cores-none--memory-gb-none--enable-app-insights-none--scoring-timeout-ms-none--replica-max-concurrent-requests-none--max-request-wait-time-none--num-replicas-none--primary-key-none--secondary-key-none--tags-none--properties-none--description-none-) başvuru.

#### <a name="create-a-new-cluster"></a>Yeni küme oluşturma

Yeni bir Azure Kubernetes Service kümesi oluşturmak için aşağıdaki kodu kullanın:

> [!IMPORTANT]
> Olan bir AKS kümesi oluşturma işlemi için çalışma süresi. Oluşturulduktan sonra bu küme birden çok dağıtım için yeniden kullanabilirsiniz. Küme veya onu içeren kaynak grubunu silerseniz, sonraki açışınızda dağıtmanız gerekir. yeni bir küme oluşturmanız gerekir.
> İçin [ `provisioning_configuration()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py), büyüktür veya eşittir 12 sanal CPU'lara göre vm_size çarpılan agent_count emin olmanız gerekir daha sonra agent_count ve vm_size, özel değerleri seçin. Örneğin, bir vm_size 4 sanal CPU'lar varsa, "Standard_D3_v2" birini kullanırsanız, 3 veya daha büyük bir agent_count seçmeniz gerekir.

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Use the default configuration (you can also provide parameters to customize this)
prov_config = AksCompute.provisioning_configuration()

aks_name = 'aml-aks-1'
# Create the cluster
aks_target = ComputeTarget.create(workspace = ws,
                                    name = aks_name,
                                    provisioning_configuration = prov_config)

# Wait for the create process to complete
aks_target.wait_for_completion(show_output = True)
print(aks_target.provisioning_state)
print(aks_target.provisioning_errors)
```

**Tahmini Süre**: Yaklaşık 20 dakika.

#### <a name="use-an-existing-cluster"></a>Var olan bir küme kullanın

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

# Wait for the operation to complete
aks_target.wait_for_completion(True)
```

**Tahmini Süre**: Yaklaşık 3 dakika.

Azure Machine Learning SDK'sı dışında bir AKS kümesi oluşturma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [AKS kümesi oluşturma](https://docs.microsoft.com/cli/azure/aks?toc=%2Fen-us%2Fazure%2Faks%2FTOC.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json&view=azure-cli-latest#az-aks-create)
* [(Portal) AKS kümesi oluşturma](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal?view=azure-cli-latest)

#### <a name="deploy-the-image"></a>Görüntüyü dağıtmak

Oluşturulan görüntüyü dağıtmak için [görüntüyü oluşturmaya](#createimage) bölümü Azure Kubernetes sunucu kümesine bu belgenin aşağıdaki kodu kullanın:

```python
from azureml.core.webservice import Webservice, AksWebservice

# Set configuration and service name
aks_config = AksWebservice.deploy_configuration()
aks_service_name ='aks-service-1'
# Deploy from image
service = Webservice.deploy_from_image(workspace = ws,
                                            name = aks_service_name,
                                            image = image,
                                            deployment_config = aks_config,
                                            deployment_target = aks_target)
# Wait for the deployment to complete
service.wait_for_deployment(show_output = True)
print(service.state)
```

**Tahmini Süre**: Yaklaşık 3 dakika.

Daha fazla bilgi için başvuru belgeleri için bkz. [AksWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py) ve [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice.webservice?view=azure-ml-py) sınıfları.

### <a id="azuremlcompute"></a> Azure ML işlem ile çıkarımı

Azure ML bilgisayar hedefine oluşturulur ve Azure Machine Learning hizmeti tarafından yönetilir. Azure ML işlem hatları gelen toplu tahmin için kullanılabilir.

Bir Azure ML işlem ile batch çıkarım kılavuzu için okuma [Batch Öngörüler çalıştırma nasıl](how-to-run-batch-predictions.md) belge.


### <a id="fpga"></a> Alanda programlanabilir kapı dizileri (FPGA) dağıtma

Project Brainwave gerçek zamanlı çıkarım istekleri için çok düşük gecikme süresine ulaşmanız mümkün kılar. Project Brainwave alanda programlanabilir kapı dizileri Azure bulutta dağıtılan derin sinir ağı (DNN) hızlandırır. Yaygın olarak kullanılan Dnn'leri aktarımlı öğrenme için featurizers olarak kullanılabilir mi ya kendi verilerinizden özelleştirilebilir ağırlıklara sahip eğitilir.

Project Brainwave kullanarak bir model dağıtımına ilişkin bir kılavuz için bkz [bir FPGA Dağıt](how-to-deploy-fpga-web-service.md) belge.

## <a name="define-schema"></a>Şema tanımlayın

Özel dekoratörler için kullanılabilir [Openapı](https://swagger.io/docs/specification/about/) belirtimi oluşturma ve giriş web hizmetini dağıtırken, işleme yazın. İçinde `score.py` dosyası tanımlanan bir tür nesnelerden biri için girişi ve/veya çıktı oluşturucuda bir örnek sağlar ve türü ve örnek şema otomatik olarak oluşturmak için kullanılır. Aşağıdaki türleri şu anda desteklenir:

* `pandas`
* `numpy`
* `pyspark`
* Standart Python

Gerekli bağımlılıkları için öncelikle olun `inference-schema` paket dahil edilir, `env.yml` conda ortam dosyası. Bu örnekte `numpy` tür parametresi için şema, bu nedenle ek pip `[numpy-support]` de yüklenir.

```python
%%writefile myenv.yml
name: project_environment
dependencies:
  - python=3.6.2
  - pip:
    - azureml-defaults
    - scikit-learn
    - inference-schema[numpy-support]
```

Ardından, değişiklik `score.py` içeri aktarmak için dosya `inference-schema` paketleri. Giriş tanımlayın ve örnek biçimlerde çıkış `input_sample` ve `output_sample` değişkenleri, web hizmeti için istek ve yanıt formatları temsil eder. Girdide bu örnekleri kullanın ve işlev dekoratörler üzerinde çıkışını `run()` işlevi.

```python
%%writefile score.py
import json
import numpy as np
import os
import pickle
from sklearn.externals import joblib
from sklearn.linear_model import LogisticRegression
from azureml.core.model import Model

from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.numpy_parameter_type import NumpyParameterType


def init():
    global model
    model_path = Model.get_model_path('sklearn_mnist')
    model = joblib.load(model_path)


input_sample = np.array([[1.8]])
output_sample = np.array([43638.88])

@input_schema('data', NumpyParameterType(input_sample))
@output_schema(NumpyParameterType(output_sample))
def run(raw_data):
    data = np.array(json.loads(raw_data)['data'])
    y_hat = model.predict(data)
    return json.dumps(y_hat.tolist())
```

Normal bir görüntü kayıt ve web hizmeti dağıtım işlemi ile güncelleştirilmiş aşağıdaki sonra `score.py` dosya, Swagger URI hizmetten alınamıyor. Bu URI isteyen döndürecektir `swagger.json` dosya.

```python
service.wait_for_deployment(show_output=True)
print(service.swagger_uri)
```



Yeni bir görüntü oluşturduğunuzda, yeni görüntüyü kullanmak istediğiniz her hizmeti el ile güncelleştirmeniz gerekir. Web hizmetini güncelleştirmek için `update` yöntemi. Aşağıdaki kod, yeni görüntüyü kullanarak web hizmetini güncelleştirmek gösterilmektedir:

```python
from azureml.core.webservice import Webservice
from azureml.core.image import Image

service_name = 'aci-mnist-3'
# Retrieve existing service
service = Webservice(name = service_name, workspace = ws)

# point to a different image
new_image = Image(workspace = ws, id="myimage2:1")

# Update the image used by the service
service.update(image = new_image)
print(service.state)
```

Daha fazla bilgi için başvuru belgeleri için bkz. [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py) sınıfı.

## <a name="test-web-service-deployments"></a>Test web hizmeti dağıtımları

Bir web hizmeti dağıtımı test etmek için kullanabileceğiniz `run` Webservice nesnesinin yöntemi. Aşağıdaki örnekte, bir JSON belgesi bir web hizmeti olarak ayarlandıysa ve sonucu görüntülenir. Gönderilen verilerin ne model eşleşmelidir. Bu örnekte, veri biçimi ailelere modeli tarafından beklenen giriş eşleşir.

```python
import json

test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10],
    [10,9,8,7,6,5,4,3,2,1]
]})
test_sample = bytes(test_sample,encoding = 'utf8')

prediction = service.run(input_data = test_sample)
print(prediction)
```

Web hizmeti bir REST API olduğundan, çeşitli programlama dillerini istemci uygulamalar oluşturabilirsiniz. Daha fazla bilgi için [istemci uygulamalarının webservices'a kullanması için oluşturma](how-to-consume-web-service.md).

## <a id="update"></a> Web hizmetini güncelleştirmek

Yeni bir görüntü oluşturduğunuzda, yeni görüntüyü kullanmak istediğiniz her hizmeti el ile güncelleştirmeniz gerekir. Web hizmetini güncelleştirmek için `update` yöntemi. Aşağıdaki kod, yeni görüntüyü kullanarak web hizmetini güncelleştirmek gösterilmektedir:

```python
from azureml.core.webservice import Webservice
from azureml.core.image import Image

service_name = 'aci-mnist-3'
# Retrieve existing service
service = Webservice(name = service_name, workspace = ws)

# point to a different image
new_image = Image(workspace = ws, id="myimage2:1")

# Update the image used by the service
service.update(image = new_image)
print(service.state)
```

Daha fazla bilgi için başvuru belgeleri için bkz. [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py) sınıfı.

## <a id="iotedge"></a> Azure IOT Edge için dağıtma

Azure IOT Edge cihazı, bir Linux veya Azure IOT Edge çalışma zamanı çalışan Windows tabanlı bir cihaz örneğidir. Azure IOT hub'ı kullanarak, makine öğrenimi modelleri IOT Edge modülleri bu cihazlara dağıtabilirsiniz. IOT Edge cihazına bir model dağıtımına model bulut işleme için veri göndermek zorunda yerine doğrudan kullanmak cihazın verir. Daha hızlı yanıt süreleri ve daha az veri aktarımı olursunuz.

Azure IOT Edge modülleri, bir kapsayıcı kayıt defterinden cihazınıza dağıtılır. Görüntü modelinizden oluşturduğunuzda, çalışma alanınız için kapsayıcı kayıt defterinde depolanır.

> [!IMPORTANT]
> Bu bölümdeki bilgiler, zaten Azure IOT Hub ve Azure IOT Edge modülleri ile ilgili bilgi sahibi olduğunuzu varsayar. Bu bölümdeki bilgiler, bazıları, ancak Azure Machine Learning hizmetine özel işlem çoğunu edge cihazına dağıtmak için Azure IOT hizmeti'olmuyor.
>
> Azure Iot'yi kullanmaya alışkın değilseniz bkz [Azure IOT temel konuları](https://docs.microsoft.com/azure/iot-fundamentals/) ve [Azure IOT Edge](https://docs.microsoft.com/azure/iot-edge/) temel bilgi. Sonra belirli işlemleri hakkında daha fazla bilgi için bu bölümdeki diğer bağlantıları kullanın.

### <a name="set-up-your-environment"></a>Ortamınızı ayarlama

* Bir geliştirme ortamı. Daha fazla bilgi için [bir geliştirme ortamı yapılandırma](how-to-configure-environment.md) belge.

* Bir [Azure IOT hub'ı](../../iot-hub/iot-hub-create-through-portal.md) Azure aboneliğinizdeki.

* Eğitilen bir modeli. Bir model eğitip ilişkin bir örnek için bkz: [bir Azure Machine Learning ile görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md) belge. Üzerinde önceden eğitilen bir modelin kullanılabilir [Azure IOT Edge GitHub deposunda için AI Toolkit](https://github.com/Azure/ai-toolkit-iot-edge/tree/master/IoT%20Edge%20anomaly%20detection%20tutorial).

### <a id="getcontainer"></a> Kapsayıcı kayıt defteri kimlik bilgilerini alma

Cihazınız için IOT Edge modülü dağıtmak için Azure IOT kapsayıcı kayıt defterine docker görüntüleri depolayan Azure Machine Learning hizmeti için kimlik bilgileri gerekir.

Kimlik bilgilerini iki yolla alabilirsiniz:

+ **Azure portalında**:

  1. [Azure Portal](https://portal.azure.com/signin/index) oturum açın.

  1. Azure Machine Learning hizmeti çalışma alanınıza gidin ve seçin __genel bakış__. Kapsayıcı kayıt defteri ayarları'na gidin, seçin __kayıt defteri__ bağlantı.

     ![Bir kapsayıcı kayıt defteri girdisinin görüntüsü](./media/how-to-deploy-and-where/findregisteredcontainer.png)

  1. Kapsayıcı kayıt defterinde seçmek **erişim anahtarlarını** ve yönetici kullanıcıyı etkinleştirin.

     ![Erişim anahtarları ekran görüntüsü](./media/how-to-deploy-and-where/findaccesskey.png)

  1. İçin değerleri kaydedin **oturum açma sunucusu**, **kullanıcıadı**, ve **parola**.

+ **Bir Python betiği ile**:

  1. Bir kapsayıcı oluşturmak için yukarıda çalıştırılan koddan sonra aşağıdaki Python betiği kullanın:

     ```python
     # Getting your container details
     container_reg = ws.get_details()["containerRegistry"]
     reg_name=container_reg.split("/")[-1]
     container_url = "\"" + image.image_location + "\","
     subscription_id = ws.subscription_id
     from azure.mgmt.containerregistry import ContainerRegistryManagementClient
     from azure.mgmt import containerregistry
     client = ContainerRegistryManagementClient(ws._auth,subscription_id)
     result= client.registries.list_credentials(resource_group_name, reg_name, custom_headers=None, raw=False)
     username = result.username
     password = result.passwords[0].value
     print('ContainerURL{}'.format(image.image_location))
     print('Servername: {}'.format(reg_name))
     print('Username: {}'.format(username))
     print('Password: {}'.format(password))
     ```
  1. ContainerURL, servername, kullanıcı adı ve parola için değerleri kaydedin.

     IOT Edge cihaz özel kapsayıcı kayıt defterinizde görüntülerine erişim sağlamak bu kimlik bilgileri gereklidir.

### <a name="prepare-the-iot-device"></a>IOT cihazı hazırlama

Cihazınızı Azure IOT Hub'ınızla kaydolmak ve ardından cihaza IOT Edge çalışma zamanı yükleyin. Bu işlemle ilgili bilgi sahibi değilseniz bkz [hızlı başlangıç: Bir Linux x64 cihaza, ilk IOT Edge modülü dağıtmak](../../iot-edge/quickstart-linux.md).

Bir cihaz kaydetme, diğer yöntemler şunlardır:

* [Azure portal](https://docs.microsoft.com/azure/iot-edge/how-to-register-device-portal)
* [Azure CLI](https://docs.microsoft.com/azure/iot-edge/how-to-register-device-cli)
* [Visual Studio Code](https://docs.microsoft.com/azure/iot-edge/how-to-register-device-vscode)

### <a name="deploy-the-model-to-the-device"></a>Cihaz için model dağıtma

Model aygıta dağıtmak için topladığınız kayıt defteri bilgileri kullanın. [kapsayıcı kayıt defteri kimlik bilgilerini alma](#getcontainer) bölümü modülü dağıtımı ile IOT Edge modülleri için adımlar. Örneğin, [dağıtma Azure IOT Edge modülleri Azure portalından](../../iot-edge/how-to-deploy-modules-portal.md), yapılandırmanız gereken __kayıt defteri ayarları__ cihaz için. Kullanım __oturum açma sunucusu__, __kullanıcıadı__, ve __parola__ çalışma kapsayıcı kayıt defteriniz için.

Kullanarak da dağıtabilirsiniz [Azure CLI](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-modules-cli) ve [Visual Studio Code](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-modules-vscode).

## <a name="clean-up"></a>Temizleme

Dağıtılmış bir web hizmetini silmek için kullanın `service.delete()`.

Görüntüyü silmek için kullanın `image.delete()`.

Kayıtlı bir model silmek için kullanın `model.delete()`.

Daha fazla bilgi için başvuru belgeleri için bkz. [WebService.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#delete--), [Image.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.image(class)?view=azure-ml-py#delete--), ve [Model.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#delete--).

## <a name="next-steps"></a>Sonraki adımlar

* [Dağıtım sorunlarını giderme](how-to-troubleshoot-deployment.md)
* [Azure Machine Learning web hizmetleri SSL ile güvenli hale getirme](how-to-secure-web-service.md)
* [Bir web hizmeti olarak ML modeli kullanma](how-to-consume-web-service.md)
* [Batch Öngörüler çalıştırma](how-to-run-batch-predictions.md)
* [Azure Machine Learning Modellerinizi Application Insights ile izleme](how-to-enable-app-insights.md)
* [Üretimde modelleri için veri toplama](how-to-enable-data-collection.md)
* [Azure Machine Learning hizmeti SDK'sı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)
* [Azure Machine Learning hizmeti ile Azure sanal ağları kullanın.](how-to-enable-virtual-network.md)
* [Öneri sistemleri oluşturma için en iyi yöntemler](https://github.com/Microsoft/Recommenders)
* [Azure üzerinde gerçek zamanlı bir öneri API'si oluşturun](https://docs.microsoft.com/azure/architecture/reference-architectures/ai/real-time-recommendation)
