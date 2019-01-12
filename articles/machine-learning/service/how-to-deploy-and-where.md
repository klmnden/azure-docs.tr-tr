---
title: Modelleri web Hizmetleri olarak dağıtma
titleSuffix: Azure Machine Learning service
description: 'Nasıl ve nerede bilgi dahil olmak üzere Azure Machine Learning hizmeti Modellerinizi dağıtmak için: Azure Container Instances, Azure Kubernetes hizmeti, Azure IOT Edge ve alanda programlanabilir kapı dizileri.'
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: 2c71b0abd5069aeb00b63fde8b76e5bb0fc0beda
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2019
ms.locfileid: "54246441"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Azure Machine Learning hizmeti ile modelleri dağıtma

Azure Machine Learning hizmeti SDK'sını kullanarak eğitilen modelinizi dağıtabileceğiniz çeşitli yöntemler sağlar. Bu belgede, modelinizin Azure bulutta veya IOT edge cihazları için bir web hizmeti olarak dağıtmayı öğrenin.

Modelleri için aşağıdaki işlem hedeflerine dağıtabilirsiniz:

| Hedef işlem | Dağıtım türü | Açıklama |
| ----- | ----- | ----- |
| [Azure Container Instances (ACI)](#aci) | Web hizmeti | Hızlı Dağıtım. Geliştirme veya test için iyidir. |
| [Azure Kubernetes Service'i (AKS)](#aks) | Web hizmeti | Büyük ölçekli üretim dağıtımları için idealdir. Otomatik ölçeklendirme ve hızlı yanıt süresi sağlar. |
| [Azure IoT Edge](#iotedge) | IOT Modülü | IOT cihazlarında modelleri dağıtın. Çıkarım cihazda'olmuyor. |
| [Alanda programlanabilir kapı dizileri (FPGA)](#fpga) | Web hizmeti | Gerçek zamanlı çıkarım için son derece düşük gecikme süresi. |

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Kwk3]

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure Machine Learning hizmeti çalışma alanında ve yüklü Python için Azure Machine Learning SDK'sı. Kullanarak şu önkoşul olarak gerekenleri edinin öğrenin [Azure Machine Learning Hızlı Başlangıç ile çalışmaya başlama](quickstart-get-started.md).

- Eğitilen bir modelin iki pickle içinde (`.pkl`) veya ONNX (`.onnx`) biçimi. Eğitilen bir modelin yoksa içindeki adımları kullanın [eğitme modelleri](tutorial-train-models-with-aml.md) eğitmek ve bir Azure Machine Learning hizmeti ile kaydetme öğretici.

- Kod bölümleri varsayımında `ws` , machine learning çalışma alanı başvuruyor. Örneğin, `ws = Workspace.from_config()`.

## <a name="deployment-workflow"></a>Dağıtım iş akışı

Tüm işlem hedeflerine yönelik bir model dağıtma işlemini benzer:

1. Bir model eğitip.
1. Modeli kaydedin.
1. Bir görüntü yapılandırması oluşturun.
1. Görüntü oluşturun.
1. Resim bir işlem hedefine dağıtın.
1. Dağıtımı test etme
1. (İsteğe bağlı) Yapıları temizleyin.

    * Zaman **bir web hizmeti olarak dağıtma**, üç dağıtım seçeneği vardır:

        * [Dağıtma](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-workspace--name--model-paths--image-config--deployment-config-none--deployment-target-none-): Bu yöntemi kullanırken, modeli kaydedin veya görüntü oluşturmanız gerekmez. Ancak model veya görüntü adını kontrol edemezsiniz veya ilişkili etiketleri ve açıklamaları.
        * [deploy_from_model](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-from-model-workspace--name--models--image-config--deployment-config-none--deployment-target-none-): Bu yöntemi kullanırken, bir görüntü oluşturmak gerekmez. Ancak, oluşturulan görüntünün adını denetime sahip değilsiniz.
        * [deploy_from_image](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-from-image-workspace--name--image--deployment-config-none--deployment-target-none-): Modeli kaydedin ve bu yöntem kullanmadan önce bir görüntü oluşturun.

        Bu örneklerde belge kullanım `deploy_from_image`.

    * Zaman **bir IOT Edge modülü dağıtma**, modeli kaydedin ve görüntü oluşturmanız gerekir.

## <a name="register-a-model"></a>Bir modeli kaydedin

Eğitilen modelleri yalnızca dağıtılabilir. Azure Machine Learning veya başka bir hizmet kullanarak modeli eğitilir. Bir model dosyasından kaydetmek için aşağıdaki kodu kullanın:

```python
from azureml.core.model import Model

model = Model.register(model_path = "model.pkl",
                       model_name = "Mymodel",
                       tags = {"key": "0.1"},
                       description = "test",
                       workspace = ws)
```

> [!NOTE]
> Örnekte pickle dosya olarak depolanan bir model kullanarak gösterir, ancak aynı zamanda kullanılan ONNX modelleri kullanabilirsiniz. ONNX modelleri kullanma hakkında daha fazla bilgi için bkz. [ONNX ve Azure Machine Learning](how-to-build-deploy-onnx.md) belge.

Daha fazla bilgi için başvuru belgeleri için bkz. [Model sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

## <a id="configureimage"></a> Bir görüntü yapılandırması oluşturma

Dağıtılan modellerinde bir görüntü olarak paketlenir. Görüntü modeli çalıştırmak için gerekli olan bağımlılıklar içerir.

İçin **Azure Container Instance**, **Azure Kubernetes hizmeti**, ve **Azure IOT Edge** dağıtımları `azureml.core.image.ContainerImage` sınıfı, bir görüntü yapılandırması oluşturmak için kullanılır. Görüntü yapılandırma, ardından yeni bir Docker görüntüsü oluşturmak için kullanılır. 

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

Bu yapılandırmayı kullanan bir `score.py` geçirmek için dosyayı modele ister. Bu dosya iki işlevleri içerir:

* `init()`: Genellikle bu işlev, genel bir nesnesine modeli yükler. Bu işlev, Docker kapsayıcısı başlatıldığında tek bir kez çalıştırılır. 

* `run(input_data)`: Bu işlev, giriş verileri temel alan bir değer tahmin modelini kullanır. Çalıştırmanın girişleri ve çıkışlarında serileştirme ve seriden çıkarma için normal olarak JSON kullanılır ama başka biçimler de desteklenir.

Bir örnek `score.py` bkz [görüntü sınıflandırma Öğreticisi](tutorial-deploy-models-with-aml.md#make-script). ONNX model kullanan bir örnek için bkz: [ONNX ve Azure Machine Learning](how-to-build-deploy-onnx.md) belge.

`conda_file` Parametre conda ortam dosyası sağlamak amacıyla kullanılır. Bu dosya, dağıtılmış bir modelinin conda ortamı tanımlar. Bu dosya oluşturma hakkında daha fazla bilgi için bkz. [bir ortam dosyası (myenv.yml) oluşturma](tutorial-deploy-models-with-aml.md#create-environment-file).

Daha fazla bilgi için başvuru belgeleri için bkz. [ContainerImage sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py)

## <a id="createimage"></a> Görüntü oluşturma

Görüntü yapılandırması oluşturulduktan sonra bir görüntü oluşturmak için kullanabilirsiniz. Bu görüntü, çalışma alanınız için kapsayıcı kayıt defterinde depolanır. Oluşturulduktan sonra birden fazla hizmet için aynı görüntüsü dağıtabilirsiniz.

```python
# Create the image from the image configuration
image = ContainerImage.create(name = "myimage", 
                              models = [model], #this is the model object
                              image_config = image_config,
                              workspace = ws
                              )
```

**Tahmini Süre**: Yaklaşık 3 dakika.

Aynı ada sahip birden fazla görüntü kayıt yaptırdığınızda görüntüleri otomatik olarak tutulur. İlk resim gibi kayıtlı `myimage` kimliği atanır `myimage:1`. Sonraki bir görüntü olarak Kaydet `myimage`, yeni görüntüyü kimliğidir `myimage:2`.

Daha fazla bilgi için başvuru belgeleri için bkz. [ContainerImage sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py).

## <a name="deploy-the-image"></a>Görüntüyü dağıtmak

Dağıtıma aldığınızda, dağıttığınız işlem hedef bağlı olarak biraz farklı bir işlemdir. Bilgileri dağıtma hakkında bilgi edinmek için aşağıdaki bölümlerdeki kullanın:

* [Azure Container Instances](#aci)
* [Azure Kubernetes hizmeti](#aks)
* [Project Brainwave (alanda programlanabilir kapı dizileri)](#fpga)
* [Azure IOT Edge cihazları](#iotedge)

### <a id="aci"></a> Azure Container Instances'a dağıtma

Bir web hizmeti bir veya daha aşağıdaki koşullardan biri Modellerinizi dağıtmak için Azure Container Instances kullanmak doğrudur:

- Hızlı bir şekilde dağıtın ve modelinizi doğrulama gerekir. ACI dağıtım 5 dakikadan daha kısa bir süre içinde tamamlanır.
- Geliştirilmekte olan bir modeli test edersiniz. ACI için kotaları ve bölge kullanılabilirliği görmek için bkz [kotaları ve Azure Container Instances için bölge kullanılabilirliği](https://docs.microsoft.com/azure/container-instances/container-instances-quotas) belge.

Azure Container Instances'a dağıtmak için aşağıdaki adımları kullanın:

1. Dağıtım Yapılandırması tanımlayın. Aşağıdaki örnek, bir CPU Çekirdeği ve 1 GB bellek kullanan yapılandırması tanımlar:

    [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-deploy-to-aci/how-to-deploy-to-aci.py?name=configAci)]

2. Oluşturulan görüntüyü dağıtmak için [görüntüyü oluşturmaya](#createimage) bölümü bu belgede aşağıdaki kodu kullanın:

    [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-deploy-to-aci/how-to-deploy-to-aci.py?name=option3Deploy)]

    **Tahmini Süre**: Yaklaşık 3 dakika.

    > [!TIP]
    > Dağıtım sırasında bir hata varsa, kullanmak `service.get_logs()` AKS hizmeti günlükleri görüntülemek için. Günlüğe kaydedilen bilgileri hatanın nedenini gösterir.

Daha fazla bilgi için başvuru belgeleri için bkz. [AciWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py) ve [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py) sınıfları.

### <a id="aks"></a> Azure Kubernetes hizmetine dağıtın

Modelinizi ölçekli üretim web hizmeti olarak dağıtmak için Azure Kubernetes Service (AKS) kullanın. Mevcut bir AKS kümesi kullanmak veya Azure Machine Learning SDK'sı, CLI veya Azure portalını kullanarak yeni bir tane oluşturun.

Olan bir AKS kümesi oluşturma işlemi için çalışma süresi. Bu kümeye birden çok dağıtımlar için yeniden kullanabilirsiniz. Kümeyi silmeniz halinde, sonraki açışınızda dağıtmanız gerekir. yeni bir küme oluşturmanız gerekir.

Azure Kubernetes hizmeti, aşağıdaki özellikleri sağlar:

* Otomatik ölçeklendirme
* Günlüğe kaydetme
* Model veri koleksiyonu
* Web hizmetlerinizi hızlı yanıt süresi

Azure Kubernetes Service'e dağıtmak için aşağıdaki adımları kullanın:

1. Bir AKS kümesi oluşturmak için aşağıdaki kodu kullanın:

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

    > [!TIP]
    > AKS kümesini Azure aboneliğinizde zaten ve sürüm 1.11. *, görüntünüzü dağıtmak için kullanın. Aşağıdaki kod, varolan bir kümenin çalışma alanınıza eklemek gösterilmektedir:
    >
    > ```python
    > from azureml.core.compute import AksCompute, ComputeTarget
    > # Set the resource group that contains the AKS cluster and the cluster name
    > resource_group = 'myresourcegroup'
    > cluster_name = 'mycluster'
    > 
    > # Attatch the cluster to your workgroup
    > attach_config = AksCompute.attach_configuration(resource_group = resource_group,
    >                                          cluster_name = cluster_name)
    > aks_target = ComputeTarget.attach(ws, 'mycompute', attach_config)
    > 
    > # Wait for the operation to complete
    > aks_target.wait_for_completion(True)
    > ```

2. Oluşturulan görüntüyü dağıtmak için [görüntüyü oluşturmaya](#createimage) bölümü bu belgede aşağıdaki kodu kullanın:

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

    > [!TIP]
    > Dağıtım sırasında bir hata varsa, kullanmak `service.get_logs()` AKS hizmeti günlükleri görüntülemek için. Günlüğe kaydedilen bilgileri hatanın nedenini gösterir.

Daha fazla bilgi için başvuru belgeleri için bkz. [AksWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py) ve [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice(class)?view=azure-ml-py) sınıfları.

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

Cihazınızı indirerek başlatmak [bu betik](https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/installIoTEdge) bir UbuntuX64 IOT edge düğüm veya aşağıdaki komutları çalıştırmak için DSVM:

```bash
ssh <yourusername>@<yourdeviceip>
sudo wget https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/installIoTEdge
sudo chmod +x installIoTEdge
sudo ./installIoTEdge
```

IOT Edge düğüm, IOT Hub'ınız için bağlantı dizesini almak hazırdır. Satır için konum ```device_connection_string:``` ve teklifler arası üst bağlantı dizesini yapıştırın.

Cihazınızı kaydedemedik ve izleyerek IOT çalışma zamanı adım adım yükleme konusunda da bilgi alabilirsiniz [hızlı başlangıç: Bir Linux x64 cihaza, ilk IOT Edge modülü dağıtmak](../../iot-edge/quickstart-linux.md) belge.


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

## <a name="update-the-web-service"></a>Web hizmetini güncelleştirmek

Web hizmetini güncelleştirmek için `update` yöntemi. Aşağıdaki kod, yeni görüntüyü kullanarak web hizmetini güncelleştirmek gösterilmektedir:

```python
from azureml.core.webservice import Webservice

service_name = 'aci-mnist-3'
# Retrieve existing service
service = Webservice(name = service_name, workspace = ws)

# point to a different image
new-image = Image(workspace = ws, id="myimage2:1")

# Update the image used by the service
service.update(image = new-image)
print(service.state)
```

> [!NOTE]
> Web hizmeti bir görüntüsünü güncelleştirdiğinizde otomatik olarak güncelleştirilmez. Ayrıca, yeni görüntüyü kullanmak istediğiniz her hizmet el ile güncelleştirmeniz gerekir.

## <a name="clean-up"></a>Temizleme

Dağıtılmış bir web hizmetini silmek için kullanın `service.delete()`.

Görüntüyü silmek için kullanın `image.delete()`.

Kayıtlı bir model silmek için kullanın `model.delete()`.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Machine Learning web hizmetleri SSL ile güvenli hale getirme](how-to-secure-web-service.md)
* [Bir web hizmeti olarak ML modeli kullanma](how-to-consume-web-service.md)
* [Batch Öngörüler çalıştırma](how-to-run-batch-predictions.md)
* [Azure Machine Learning hizmeti ile Azure sanal ağları kullanın.](how-to-enable-virtual-network.md)