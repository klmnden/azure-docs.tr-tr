---
title: Azure Container Instances'a (ACI için) - Azure Machine Learning Web hizmetlerini dağıtma
description: Azure Container Instances'a (ACI) üzerinde Azure Machine Learning hizmeti ile bir web hizmeti olarak eğitilen bir modelin dağıtmayı öğrenin. Bu makalede, bir model üzerinde ACI dağıtmanın üç farklı yolu gösterilmektedir. Kod ve dağıtım adlandırma bölümlerini denetimi satır sayısı farklı.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: raymondl
author: raymondlaghaeian
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: 5a62d4b0b324d8b2536e408132210f07f08e8bb8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46958705"
---
# <a name="deploy-web-services-to-azure-container-instances"></a>Azure Container Instances'a Web hizmetlerini dağıtma 

Eğitilen model üzerinde bir web hizmeti olarak dağıtabilirsiniz [Azure Container Instances](https://azure.microsoft.com/services/container-instances/) (ACI) [Azure Kubernetes hizmeti](https://azure.microsoft.com/services/kubernetes-service/) (AKS), IOT edge cihazı veya [alan programlanabilir kapı dizileri (FPGA)](concept-accelerate-with-fpgas.md) 

ACI, genellikle AKS ucuz ve kod satırıyla 4-6 ayarlanabilir. ACI, test dağıtımları için mükemmel bir seçenektir. Daha sonra yüksek ölçekli üretim kullanımı için web hizmetleri ve modelleri kullanıma hazır olduğunuzda, yapabilecekleriniz [bunları AKS'ye dağıtma](how-to-deploy-to-aks.md).

Bu makalede, bir model üzerinde ACI dağıtmanın üç farklı yolu gösterilmektedir. Kod ve dağıtım adlandırma bölümlerini denetimi satır sayısı farklı. En az miktarda kod ve çoğu kod ile yöntemi ve denetimi ile yöntemden ACI Seçenekler şunlardır:

* Model dosyası kullanarak dağıtım `Webservice.deploy()` 
* Kayıtlı modeli kullanarak dağıtım `Webservice.deploy_from_model()`
* Yansıma kullanarak kayıtlı model dağıtma `Webservice.deploy_from_image()`

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


## <a name="prerequisites"></a>Önkoşullar

- Bir Azure Machine Learning çalışma alanı ve yüklü Python için Azure Machine Learning SDK'sı. Kullanarak şu önkoşul olarak gerekenleri edinin öğrenin [Azure Machine Learning Hızlı Başlangıç ile çalışmaya başlama](quickstart-get-started.md).

- Azure Machine Learning çalışma alanı nesnesi

    ```python
    from azureml.core import Workspace
    ws = Workspace.from_config()
    ```

- Dağıtmak için bir model. Takip ettiğiniz sırada oluşturulan model örneklerde bu belgedeki "[bir model Eğitip](tutorial-train-models-with-aml.md)" öğretici. Bu model kullanmazsanız, model adına başvurmak için adımları değiştirin.  Modelinizi çalıştırmak için kendi Puanlama betiğine yazılacak gerekir.


## <a name="configure-an-image"></a>Görüntü yapılandırma

Model dosyaları depolamak için kullanılan bir Docker görüntüsü yapılandırın.
1. Puanlama betiğine (score.py) oluşturma [bu yönergeleri kullanarak](tutorial-deploy-models-with-aml.md#create-scoring-script)

1. Bir ortam dosyası (myenv.yml) oluşturma [bu yönergeleri kullanarak](tutorial-deploy-models-with-aml.md#create-environment-file) 

1. Docker görüntüsünü Python SDK'yı kullanarak aşağıdaki gibi yapılandırmak için bu iki dosyayı kullanın:

    ```python
    from azureml.core.image import ContainerImage

    image_config = ContainerImage.image_configuration(execution_script = "score.py",
                                                      runtime = "python",
                                                      conda_file = "myenv.yml",
                                                      description = "Image with mnist model",
                                                      tags = {"data": "mnist", "type": "classification"}
                                                     )
    ```

## <a name="configure-the-aci-container"></a>ACI kapsayıcı yapılandırma

ACI kapsayıcı yapılandırma CPU sayısını ve ACI kapsayıcı için gerekli RAM miktarını gigabayt olarak belirtin. Varsayılan bir Çekirdeği ve 1 gigabayt RAM birçok modelleri için yeterli olur. Düşünüyorsanız görüntü daha sonra yeniden oluşturun ve hizmet dağıtmanız gerekir.  

```python
from azureml.core.webservice import AciWebservice

aciconfig = AciWebservice.deploy_configuration(cpu_cores = 1, 
                                               memory_gb = 1, 
                                               tags = {"data": "mnist", "type": "classification"},
                                               description = 'Handwriting recognition')
```

## <a name="register-a-model"></a>Bir modeli kaydedin

> Eğer bu önkoşulu atlayabilirsiniz [model dosyasındaki dağıtma](#deploy-from-model-file) (`Webservice.deploy()`).

Bir model kullanmak için kaydetme [ `Webservice.deploy_from_model` ](#deploy-from-registered-model) veya [ `Webservice.deploy_from_image` ](#deploy-from-image). Veya kayıtlı bir model zaten varsa, şimdi al.

### <a name="retrieve-a-registered-model"></a>Kayıtlı bir model alma
Azure Machine Learning, modeli eğitmek için kullanırsanız, model çalışma alanınızda zaten kaydedilmiş olabilir.  Örneğin, son adımı, [bir model eğitip](tutorial-train-models-with-aml.md) öğretici] model kayıtlı.  Ardından, dağıtmak için kayıtlı modeli de alın.

```python
from azureml.core.model import Model

model_name = "sklearn_mnist"
model=Model(ws, model_name)
```
  
### <a name="register-a-model-file"></a>Bir model dosyasını kaydetme

Modelinizi başka bir yerde oluşturulduysa, yine de çalışma alanınıza kaydedebilirsiniz.  Bir model, model dosyası kaydetmek için (`sklearn_mnist_model.pkl` Bu örnekte) geçerli çalışma dizininde olması gerekir. Ardından bu dosyayı adlı model olarak kaydetme `sklearn_mnist` ile çalışma alanında `Model.register()`.
    
```python
from azureml.core.model import Model

model_name = "sklearn_mnist"
model = Model.register(model_path = "sklearn_mnist_model.pkl",
                        model_name = model_name,
                        tags = {"data": "mnist", "type": "classification"},
                        description = "Mnist handwriting recognition",
                        workspace = ws)
```


## <a name="option-1-deploy-from-model-file"></a>1. seçenek: model dosyasındaki dağıtma

Bir model dosyasındaki dağıtmanın seçeneği en az miktarda kod yazmak için de sunar bileşenlerden adlandırma denetim en az miktarda gerektirir. Bu seçenek, bir model dosyası ile başlar ve sizin için çalışma alanına kaydeder.  Ancak, model adını veya etiketleri veya açıklama ilişkilendirmek için.  

Bu seçenek, SDK yöntemiyle, Webservice.deploy() kullanır.  

**Tahmini Süre**: dağıtma yaklaşık 6-7 dakika sürer.

1. Yerel çalışma dizininizde model dosyası olduğundan emin olun.

1. Önkoşul model dosyası, score.py ve değişiklik açın `init()` bölümünü:

    ```python
    def init():
        global model
        # retreive the local path to the model using the model name
        model_path = Model.get_model_path('sklearn_mnist_model.pkl')
        model = joblib.load(model_path)
    ```

1. Model dosyanızı dağıtın.

    ```python
    from azureml.core.webservice import Webservice
    
    service_name = 'aci-mnist-1'
    service = Webservice.deploy(deployment_config = aciconfig,
                                    image_config = image_config,
                                    model_paths = ['sklearn_mnist_model.pkl'],
                                    name = service_name,
                                    workspace = ws)
    
    service.wait_for_deployment(show_output = True)
    print(service.state)
    ```

1. Artık [web hizmetini test](#test-web-service).

## <a name="option-2-deploy-from-registered-model"></a>2. seçenek: kayıtlı modelden dağıtma

Kayıtlı model dosyası dağıtma seçeneği, daha fazla birkaç satır kod alır ve çıkışları adlandırma üzerinde bazı denetim sağlar. Bu seçenek zaten kayıtlı bir modeli dağıtmak için kullanışlı bir yoldur.  Ancak, Docker görüntüsünü yeniden adlandırılamıyor.  

Bu seçenek, SDK yöntemiyle, Webservice.deploy_from_model() kullanır.

**Tahmini Süre**: Bu seçenek ile dağıtımı, yaklaşık 8 birkaç dakika sürer.

1. Docker kapsayıcısı ve ACI kapsayıcı yapılandırmanız ve kayıtlı modeli belirtmek için kodu çalıştırın.

    ```python
    from azureml.core.webservice import Webservice

    service_name = 'aci-mnist-2'
    service = Webservice.deploy_from_model(deployment_config = aciconfig,
                                           image_config = image_config,
                                           models = [model], # this is the registered model object
                                           name = service_name,
                                           workspace = ws)
    service.wait_for_deployment(show_output = True)
    print(service.state)
    ```

1. Artık [web hizmetini test](#test-web-service).

## <a name="option-3-deploy-from-image"></a>Seçenek 3: görüntüsünü dağıtma

Kayıtlı model dağıtma (`model`) kullanarak `Webservice.deploy_from_image()`. Bu yöntem, Docker görüntüsünü ayrı ayrı oluşturmanız ve ardından bu görüntüden dağıtmanızı sağlar.

1. Derleme ve Docker görüntüsünü kullanarak çalışma alanı altında kaydetme `ContainerImage.create()`

    Bu yöntem, ayrı bir adımla oluşturarak görüntünün üzerinde daha fazla denetim sağlar.  Kayıtlı modeli (`model`) görüntüsünden.
    
    ```python
    from azureml.core.image import ContainerImage
    
    image = ContainerImage.create(name = "myimage1",
                                  models = [model], # this is the registered model object
                                  image_config = image_config,
                                  workspace = ws)
    
    image.wait_for_creation(show_output = True)
    ```
**Tahmini Süre**: yaklaşık 3 dakika.

1. Kullanarak bir hizmet olarak Docker görüntüsü dağıtma `Webservice.deploy_from_image()`

    Artık görüntüyü ACI'ya dağıtma.  
    
    ```python
    from azureml.core.webservice import Webservice
    
    service_name = 'aci-mnist-3'
    service = Webservice.deploy_from_image(deployment_config = aciconfig,
                                                image = image,
                                                name = service_name,
                                                workspace = ws)
    service.wait_for_deployment(show_output = True)
    print(service.state)
    ```   
 
**Tahmini Süre**: yaklaşık 3 dakika.

Bu yöntem, oluşturma ve dağıtım bileşenlerde adlandırma üzerinde en fazla denetimi sağlar.

Şimdi web hizmetini test edebilirsiniz.

## <a name="test-the-web-service"></a>Web hizmetini test edin

Web hizmeti yöntem ne olursa olsun, kullanılan aynıdır.  Öngörüler elde etmek için kullanmak `run` hizmetinin yöntemi.  

```python
# Load Data
import os
import urllib

os.makedirs('./data', exist_ok = True)

urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz', filename = './data/test-images.gz')

from utils import load_data
X_test = load_data('./data/test-images.gz', False) / 255.0

from sklearn import datasets
import numpy as np
import json

# find 5 random samples from test set
n = 5
sample_indices = np.random.permutation(X_test.shape[0])[0:n]

test_samples = json.dumps({"data": X_test[sample_indices].tolist()})
test_samples = bytes(test_samples, encoding = 'utf8')

# predict using the deployed model
prediction = service.run(input_data = test_samples)
print(prediction)
```


## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu web hizmetini kullanmak üzere etmeyecekseniz, herhangi bir ücret ödememeniz silin.

```python
service.delete()
```

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [Azure Kubernetes hizmetine dağıtın](how-to-deploy-to-aks.md) büyük ölçekli bir dağıtım için. 
