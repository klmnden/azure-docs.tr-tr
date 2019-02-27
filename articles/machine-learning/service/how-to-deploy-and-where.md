---
title: Modelleri web Hizmetleri olarak dağıtma
titleSuffix: Azure Machine Learning service
description: 'Nasıl ve nerede bilgi dahil olmak üzere Azure Machine Learning hizmeti Modellerinizi dağıtmak için: Azure Container Instances, Azure Kubernetes hizmeti, Azure IOT Edge ve alanda programlanabilir kapı dizileri.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: 19d34e76c73c5ec2472d3eacddc01d6aebb6b9fb
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56889112"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Azure Machine Learning hizmeti ile modelleri dağıtma

Azure Machine Learning hizmeti SDK'sını kullanarak eğitilen modelinizi dağıtabileceğiniz çeşitli yöntemler sağlar. Bu belgede, modelinizin Azure bulutta veya IOT Edge cihazları için bir web hizmeti olarak dağıtmayı öğrenin.

> [!IMPORTANT]
> Model bir web hizmeti olarak dağıtırken, çıkış noktaları arası kaynak paylaşımı (CORS) şu anda desteklenmiyor.

Modelleri için aşağıdaki işlem hedeflerine dağıtabilirsiniz:

| Hedef işlem | Dağıtım türü | Açıklama |
| ----- | ----- | ----- |
| [Azure Kubernetes Service'i (AKS)](#aks) | Gerçek zamanlı çıkarımı | Büyük ölçekli üretim dağıtımları için idealdir. Otomatik ölçeklendirme ve hızlı yanıt süresi sağlar. |
| Azure ML işlemi | Batch çıkarımı | Batch tahmin, sunucusuz bir işlem üzerinde çalıştırın. Normal veya düşük öncelikli Vm'lere destekler. |
| [Azure Container Instances (ACI)](#aci) | Test Etme | Geliştirme veya test için iyidir. **Üretim iş yükleri için uygun değildir.** |
| [Azure IoT Edge](#iotedge) | (Önizleme) IOT Modülü | IOT cihazlarında modelleri dağıtın. Çıkarım cihazda'olmuyor. |
| [Alanda programlanabilir kapı dizileri (FPGA)](#fpga) | (Önizleme) Web hizmeti | Gerçek zamanlı çıkarım için son derece düşük gecikme süresi. |

Tüm işlem hedeflerine yönelik bir model dağıtma işlemini benzer:

1. Eğitme ve modeli kaydedin.
1. Yapılandırma ve modeli kullanan bir görüntüyü kaydedin.
1. Resim bir işlem hedefine dağıtın.
1. Dağıtımı test etme

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Kwk3]


Dağıtım iş akışı içinde ilgili kavramları hakkında daha fazla bilgi için bkz. [yönetin, dağıtın ve izleyin modeller Azure Machine Learning hizmeti ile](concept-model-management-and-deployment.md).

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](http://aka.ms/AMLFree) bugün.

- Bir Azure Machine Learning hizmeti çalışma alanında ve yüklü Python için Azure Machine Learning SDK'sı. Kullanarak şu önkoşul olarak gerekenleri edinin öğrenin [Azure Machine Learning Hızlı Başlangıç ile çalışmaya başlama](quickstart-get-started.md).

- Eğitilen bir modeli. Eğitilen bir modelin yoksa içindeki adımları kullanın [eğitme modelleri](tutorial-train-models-with-aml.md) eğitmek ve bir Azure Machine Learning hizmeti ile kaydetme öğretici.

    > [!NOTE]
    > Azure Machine Learning hizmeti ile Python 3'te yüklenebilen herhangi bir genel model çalışabilir ancak bu belgedeki örnekler pickle biçiminde depolanan bir modeli kullanarak gösterir.
    > 
    > ONNX modelleri kullanma hakkında daha fazla bilgi için bkz. [ONNX ve Azure Machine Learning](how-to-build-deploy-onnx.md) belge.

## <a id="registermodel"></a> Eğitilen bir modeli kaydedin

Model kayıt depolamak ve eğitilen Modellerinizi Azure bulutunda düzenlemek için bir yoldur. Modeller Azure Machine Learning hizmeti çalışma alanınızda kaydedilir. Azure Machine Learning veya başka bir hizmet kullanarak modeli eğitilir. Bir model dosyasından kaydetmek için aşağıdaki kodu kullanın:

```python
from azureml.core.model import Model

model = Model.register(model_path = "model.pkl",
                       model_name = "Mymodel",
                       tags = {"key": "0.1"},
                       description = "test",
                       workspace = ws)
```

**Tahmini Süre**: Yaklaşık 10 saniye.

Daha fazla bilgi için başvuru belgeleri için bkz. [Model sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

## <a id="configureimage"></a> Oluşturma ve bir görüntüyü kaydedin

Dağıtılan modellerinde bir görüntü olarak paketlenir. Görüntü modeli çalıştırmak için gerekli olan bağımlılıklar içerir.

İçin **Azure Container Instance**, **Azure Kubernetes hizmeti**, ve **Azure IOT Edge** dağıtımları [azureml.core.image.ContainerImage](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py) sınıfı, bir görüntü yapılandırması oluşturmak için kullanılır. Görüntü yapılandırma, ardından yeni bir Docker görüntüsü oluşturmak için kullanılır. 

Aşağıdaki kod, yeni bir görüntü yapılandırmasının nasıl oluşturulacağını gösterir:

```python
from azureml.core.image import ContainerImage

# Image configuration
image_config = ContainerImage.image_configuration(execution_script = "score.py",
                                                 runtime = "python",
                                                 conda_file = "myenv.yml",
                                                 description = "Image with ridge regression model",
                                                 tags = {"data": "diabetes", "type": "regression"}
                                                 )
```

**Tahmini Süre**: Yaklaşık 10 saniye.

Bu örnekte aşağıdaki tabloda açıklanan önemli parametreleri:

| Parametre | Açıklama |
| ----- | ----- |
| `execution_script` | Hizmete gönderilen istekleri almak için kullanılan bir Python betiği belirtir. Bu örnekte, komut dosyası içinde yer alan `score.py` dosya. Daha fazla bilgi için [yürütme betik](#script) bölümü. |
| `runtime` | Görüntü Python kullandığını gösterir. Diğer seçenek `spark-py`, Apache Spark ile Python kullanır. |
| `conda_file` | Conda ortam dosyası sağlamak için kullanılır. Bu dosya, dağıtılmış bir modelinin conda ortamı tanımlar. Bu dosya oluşturma hakkında daha fazla bilgi için bkz. [bir ortam dosyası (myenv.yml) oluşturma](tutorial-deploy-models-with-aml.md#create-environment-file). |

Daha fazla bilgi için başvuru belgeleri için bkz. [ContainerImage sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py)

### <a id="script"></a> Betik yürütme

Yürütme komut dağıtılan görüntüye gönderilen verileri alır ve modele geçirir. Sonra modeli tarafından döndürülen yanıtı alır ve istemciye döndürür. Betik, modelinize özeldir; Bunu, modelin bekliyor ve döndüren veri anlamanız gerekir. Betik, genellikle yük ve model çalıştırma iki işlev de içerir:

* `init()`: Genellikle bu işlev, genel bir nesnesine modeli yükler. Bu işlev, Docker kapsayıcısı başlatıldığında tek bir kez çalıştırılır. 

* `run(input_data)`: Bu işlev, giriş verileri temel alan bir değer tahmin modelini kullanır. Genellikle girişler ve çıkışlar farklı çalıştır JSON seri hale getirme ve serinin için kullanın. Ayrıca, ham ikili verileri ile çalışabilirsiniz. Veri modeline göndermeden önce veya istemciye döndürmeden önce dönüştürebilirsiniz. 

#### <a name="working-with-json-data"></a>JSON verileriyle çalışma

Aşağıdaki örnek betik, kabul eder ve JSON verilerini döndürür. `run` İşlevi JSON verileri model bekler ve ardından döndürmeden önce JSON yanıtı dönüştürür bir biçime dönüştürür:

```python
# import things required by this script
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

## <a id="deploy"></a> Görüntüyü dağıtmak

Dağıtıma aldığınızda, dağıttığınız işlem hedef bağlı olarak biraz farklı bir işlemdir. Bilgileri dağıtma hakkında bilgi edinmek için aşağıdaki bölümlerdeki kullanın:

* [Azure Container Instances](#aci)
* [Azure Kubernetes hizmeti](#aks)
* [Project Brainwave (alanda programlanabilir kapı dizileri)](#fpga)
* [Azure IOT Edge cihazları](#iotedge)

> [!NOTE]
> Zaman **bir web hizmeti olarak dağıtma**, kullanabileceğiniz üç dağıtım yöntemi vardır:
>
> | Yöntem | Notlar |
> | ----- | ----- |
> | [deploy_from_image](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-from-image-workspace--name--image--deployment-config-none--deployment-target-none-) | Modeli kaydedin ve bu yöntem kullanmadan önce bir görüntü oluşturmanız gerekir. |
> | [Dağıtma](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-workspace--name--model-paths--image-config--deployment-config-none--deployment-target-none-) | Bu yöntemi kullanırken, modeli kaydedin veya görüntü oluşturmanız gerekmez. Ancak model veya görüntü adını kontrol edemezsiniz veya ilişkili etiketleri ve açıklamaları. |
> | [deploy_from_model](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-from-model-workspace--name--models--image-config--deployment-config-none--deployment-target-none-) | Bu yöntemi kullanırken, bir görüntü oluşturmak gerekmez. Ancak, oluşturulan görüntünün adını denetime sahip değilsiniz. |
>
> Bu örneklerde belge kullanım `deploy_from_image`.

### <a id="aci"></a> Azure Container Instances'a (DEVTEST) dağıtma

Bir web hizmeti bir veya daha aşağıdaki koşullardan biri Modellerinizi dağıtmak için Azure Container Instances kullanmak doğrudur:

- Hızlı bir şekilde dağıtın ve modelinizi doğrulama gerekir. ACI dağıtım 5 dakikadan daha kısa bir süre içinde tamamlanır.
- Geliştirilmekte olan bir modeli test edersiniz. ACI için kotaları ve bölge kullanılabilirliği görmek için bkz [kotaları ve Azure Container Instances için bölge kullanılabilirliği](https://docs.microsoft.com/azure/container-instances/container-instances-quotas) belge.

Azure Container Instances'a dağıtmak için aşağıdaki adımları kullanın:

1. Dağıtım Yapılandırması tanımlayın. Aşağıdaki örnek, bir CPU Çekirdeği ve 1 GB bellek kullanan yapılandırması tanımlar:

    [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-deploy-to-aci/how-to-deploy-to-aci.py?name=configAci)]

2. Oluşturulan görüntüyü dağıtmak için [görüntüyü oluşturmaya](#createimage) bölümü bu belgede aşağıdaki kodu kullanın:

    [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-deploy-to-aci/how-to-deploy-to-aci.py?name=option3Deploy)]

    **Tahmini Süre**: Yaklaşık 3 dakika.

Daha fazla bilgi için başvuru belgeleri için bkz. [AciWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py) ve [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py) sınıfları.

### <a id="aks"></a> Azure Kubernetes Service'e (üretim) dağıtma

Modelinizi ölçekli üretim web hizmeti olarak dağıtmak için Azure Kubernetes Service (AKS) kullanın. Mevcut bir AKS kümesi kullanmak veya Azure Machine Learning SDK'sı, CLI veya Azure portalını kullanarak yeni bir tane oluşturun.

Olan bir AKS kümesi oluşturma işlemi için çalışma süresi. Bu kümeye birden çok dağıtımlar için yeniden kullanabilirsiniz. Kümeyi silmeniz halinde, sonraki açışınızda dağıtmanız gerekir. yeni bir küme oluşturmanız gerekir.

Azure Kubernetes hizmeti, aşağıdaki özellikleri sağlar:

* Otomatik ölçeklendirme
* Günlüğe kaydetme
* Model veri koleksiyonu
* Web hizmetlerinizi hızlı yanıt süresi

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

AKS kümesini Azure aboneliğinizde zaten ve sürüm 1.11. *, görüntünüzü dağıtmak için kullanın. Aşağıdaki kod, varolan bir kümenin çalışma alanınıza eklemek gösterilmektedir:

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

### <a id="fpga"></a> Azure ML işlem ile çıkarımı

Azure ML bilgisayar hedefine oluşturulur ve Azure Machine Learning hizmeti tarafından yönetilir. Azure ML işlem hatları gelen toplu tahmin için kullanılabilir.

Bir Azure ML işlem ile batch çıkarım kılavuzu için okuma [Batch Öngörüler çalıştırma nasıl](how-to-run-batch-predictions.md) belge.


### <a id="fpga"></a> Alanda programlanabilir kapı dizileri (FPGA) dağıtma

Project Brainwave gerçek zamanlı çıkarım istekleri için çok düşük gecikme süresine ulaşmanız mümkün kılar. Project Brainwave alanda programlanabilir kapı dizileri Azure bulutta dağıtılan derin sinir ağı (DNN) hızlandırır. Yaygın olarak kullanılan Dnn'leri aktarımlı öğrenme için featurizers olarak kullanılabilir mi ya kendi verilerinizden özelleştirilebilir ağırlıklara sahip eğitilir.

Project Brainwave kullanarak bir model dağıtımına ilişkin bir kılavuz için bkz [bir FPGA Dağıt](how-to-deploy-fpga-web-service.md) belge.

### <a id="iotedge"></a> Azure IOT Edge için dağıtma

Azure IOT Edge cihazı, bir Linux veya Azure IOT Edge çalışma zamanı çalışan Windows tabanlı bir cihaz örneğidir. Makine öğrenimi modelleri için bu cihazları IOT Edge modülleri dağıtılabilir. IOT Edge cihazına bir model dağıtımına model bulut işleme için veri göndermek zorunda yerine doğrudan kullanmak cihazın verir. Daha hızlı yanıt süreleri ve daha az veri aktarımı olursunuz.

Azure IOT Edge modülleri, bir kapsayıcı kayıt defterinden cihazınıza dağıtılır. Görüntü modelinizden oluşturduğunuzda, çalışma alanınız için kapsayıcı kayıt defterinde depolanır.

#### <a name="set-up-your-environment"></a>Ortamınızı ayarlama

* Bir geliştirme ortamı. Daha fazla bilgi için [bir geliştirme ortamı yapılandırma](how-to-configure-environment.md) belge.

* Bir [Azure IOT hub'ı](../../iot-hub/iot-hub-create-through-portal.md) Azure aboneliğinizdeki. 

* Eğitilen bir modeli. Bir model eğitip ilişkin bir örnek için bkz: [bir Azure Machine Learning ile görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md) belge. Üzerinde önceden eğitilen bir modelin kullanılabilir [Azure IOT Edge GitHub deposunda için AI Toolkit](https://github.com/Azure/ai-toolkit-iot-edge/tree/master/IoT%20Edge%20anomaly%20detection%20tutorial).

#### <a name="prepare-the-iot-device"></a>IOT cihazı hazırlama
IOT hub'ı oluşturmalı ve bir cihazı kaydetmek veya sahip olduğunuz bir yeniden [bu betik](https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/createNregister).

``` bash
ssh <yourusername>@<yourdeviceip>
sudo wget https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/createNregister
sudo chmod +x createNregister
sudo ./createNregister <The Azure subscriptionID you want to use> <Resourcegroup to use or create for the IoT hub> <Azure location to use e.g. eastus2> <the Hub ID you want to use or create> <the device ID you want to create>
```

"Cs" sonra elde edilen bağlantı dizesini kaydedin: "{Bu dizeyi kopyalayın}".

Cihazınızı indirerek başlatmak [bu betik](https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/installIoTEdge) bir UbuntuX64 IOT Edge düğüm veya aşağıdaki komutları çalıştırmak için DSVM:

```bash
ssh <yourusername>@<yourdeviceip>
sudo wget https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/installIoTEdge
sudo chmod +x installIoTEdge
sudo ./installIoTEdge
```

IOT Edge düğüm, IOT Hub'ınız için bağlantı dizesini almak hazırdır. Satır için konum ```device_connection_string:``` ve teklifler arası üst bağlantı dizesini yapıştırın.

Cihazınızı kaydedemedik ve izleyerek IOT çalışma zamanı yükleme konusunda da bilgi alabilirsiniz [hızlı başlangıç: Bir Linux x64 cihaza, ilk IOT Edge modülü dağıtmak](../../iot-edge/quickstart-linux.md) belge.


#### <a name="get-the-container-registry-credentials"></a>Kapsayıcı kayıt defteri kimlik bilgilerini alma
Cihazınız için IOT Edge modülü dağıtmak için Azure IOT kapsayıcı kayıt defterine docker görüntüleri depolayan Azure Machine Learning hizmeti için kimlik bilgileri gerekir.

Kolayca gerekli kapsayıcı kayıt defteri kimlik bilgilerini iki yolla alabilir:

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

#### <a name="deploy-the-model-to-the-device"></a>Cihaz için model dağıtma

Bir model çalıştırarak kolayca dağıtım yapabilir [bu betik](https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/deploymodel) Yukarıdaki adımlarda aşağıdaki bilgileri sağlayarak: kapsayıcı kayıt defteri adı, kullanıcı adı, parola, görüntü konumu URL'si, istenen dağıtım adı, IOT hub'ı adı ve oluşturduğunuz cihaz kimliği. Bu sanal Makineye aşağıdaki adımları izleyerek yapabilirsiniz: 

```bash 
wget https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/deploymodel
sudo chmod +x deploymodel
sudo ./deploymodel <ContainerRegistryName> <username> <password> <imageLocationURL> <DeploymentID> <IoTHubname> <DeviceID>
```

Alternatif olarak, adımları izleyebilirsiniz [dağıtma Azure IOT Edge modülleri Azure portalından](../../iot-edge/how-to-deploy-modules-portal.md) cihazınıza görüntüsünü dağıtmak üzere belge. Yapılandırırken __kayıt defteri ayarları__ aygıtı için __oturum açma sunucusu__, __kullanıcıadı__, ve __parola__ çalışma alanınız için kapsayıcı kayıt defteri.

> [!NOTE]
> Azure IOT ile bilmiyorsanız, hizmeti ile çalışmaya başlama bilgi için aşağıdaki belgelere bakın:
>
> * [Hızlı Başlangıç: İlk IOT Edge modülü bir Linux cihazına dağıtma](../../iot-edge/quickstart-linux.md)
> * [Hızlı Başlangıç: İlk IOT Edge modülü bir Windows cihazına dağıtma](../../iot-edge/quickstart.md)


## <a name="testing-web-service-deployments"></a>Web hizmeti dağıtımları test etme

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

## <a name="clean-up"></a>Temizleme

Dağıtılmış bir web hizmetini silmek için kullanın `service.delete()`.

Görüntüyü silmek için kullanın `image.delete()`.

Kayıtlı bir model silmek için kullanın `model.delete()`.

Daha fazla bilgi için başvuru belgeleri için bkz. [WebService.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#delete--), [Image.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.image(class)?view=azure-ml-py#delete--), ve [Model.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#delete--).

## <a name="troubleshooting"></a>Sorun giderme

* __Dağıtım sırasında bir hata varsa__, kullanın `service.get_logs()` hizmet günlükleri görüntülemek için. Günlüğe kaydedilen bilgileri hatanın nedenini gösterir.

* Günlükleri yönlendiren bir hata içeriyor olabilir __günlük düzeyi ayarlamak için hata ayıklama__. Günlük tutma düzeyini ayarlamaya Puanlama komut dosyası aşağıdaki satırları bir görüntü oluşturup ardından görüntüyü kullanarak bir hizmet oluşturma ekleyin:

    ```python
    import logging
    logging.basicConfig(level=logging.DEBUG)
    ```

    Bu değişiklik, ek günlük kaydını etkinleştirir ve daha fazla bilgi neden hatanın oluştuğu döndürebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Machine Learning web hizmetleri SSL ile güvenli hale getirme](how-to-secure-web-service.md)
* [Bir web hizmeti olarak ML modeli kullanma](how-to-consume-web-service.md)
* [Batch Öngörüler çalıştırma](how-to-run-batch-predictions.md)
* [Azure Machine Learning Modellerinizi Application Insights ile izleme](how-to-enable-app-insights.md)
* [Üretimde modelleri için veri toplama](how-to-enable-data-collection.md)
* [Azure Machine Learning hizmeti SDK'sı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)
* [Azure Machine Learning hizmeti ile Azure sanal ağları kullanın.](how-to-enable-virtual-network.md)
* [Öneri sistemleri oluşturma için en iyi yöntemler](https://github.com/Microsoft/Recommenders)
* [Azure üzerinde gerçek zamanlı bir öneri API'si oluşturun](https://docs.microsoft.com/azure/architecture/reference-architectures/ai/real-time-recommendation)
