---
title: Modeller Azure Machine Learning hizmetinden dağıtmaya | Microsoft Docs
description: Azure Kubernetes hizmeti Azure Machine Learning hizmetinden alınan bir model dağıtma konusunda bilgi edinin. Modeli web hizmeti olarak dağıtılır. Azure Kubernetes hizmeti, büyük ölçekli üretim iş yükleri için uygundur.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: raymondl
author: raymondlaghaeian
manager: cgronlun
ms.reviewer: larryfr
ms.date: 09/24/2018
ms.openlocfilehash: 5db0efd825a655143828e7ccea73704516893ea2
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52845272"
---
# <a name="how-to-deploy-models-from-azure-machine-learning-service-to-azure-kubernetes-service"></a>Azure Kubernetes Service için Azure Machine Learning hizmetinden modelleri dağıtma

Büyük ölçekli üretim senaryoları için Azure Kubernetes Service (AKS) için model dağıtabilirsiniz. Azure Machine Learning, var olan bir AKS kümesi veya yeni bir küme dağıtımı sırasında oluşturulan kullanabilirsiniz. Model için İSTEYİN, web hizmeti olarak dağıtılır.

AKS'ye dağıtma, otomatik ölçeklendirme, günlüğe kaydetme, model veri koleksiyonu ve web hizmetiniz için hızlı yanıt süresi sağlar.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://aka.ms/AMLfree) oluşturun.

- Bir Azure Machine Learning hizmeti çalışma alanında, yüklü Python için betikleri ve Azure Machine Learning SDK'sını içeren yerel bir dizin. Kullanarak şu önkoşul olarak gerekenleri edinin öğrenin [bir geliştirme ortamı yapılandırma](how-to-configure-environment.md) belge.

- Eğitilen makine öğrenme modeli. Yoksa, bkz. [görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md) öğretici.

## <a name="initialize-the-workspace"></a>Çalışma alanını Başlat

Çalışma alanı başlatmak için yükleme `config.json` çalışma alanı bilgilerinizi içeren dosya.

```python
from azureml.coreazureml import Workspace

# Load existing workspace from the config file info.
ws = Workspace.from_config()
```

## <a name="register-the-model"></a>Modeli kaydedin

Mevcut modelini kaydettirmek için model yolunu, açıklama ve etiket belirtin.

```python
from azureml.core.model import Model

model = Model.register(model_path = "model.pkl", # this points to a local file
                        model_name = "best_model", # this is the name the model is registered as
                        tags = {"data": "diabetes", "type": "regression"},
                        description = "Ridge regression model to predict diabetes",
                        workspace = ws)
```

## <a name="create-a-docker-image"></a>Docker görüntüsü oluşturma

Azure Kubernetes hizmeti, Docker görüntüleri kullanır. Görüntüyü oluşturmak için aşağıdaki adımları kullanın:

1. Görüntü yapılandırmak için Puanlama betiğine ve ortam dosyası oluşturmanız gerekir. Betik ve ortam dosyası oluşturma örneği için görüntü sınıflandırma örneği aşağıdaki bölümlere bakın:

    * [Puanlama betiğine (score.py) oluşturma](tutorial-deploy-models-with-aml.md#create-scoring-script)

        > [!IMPORTANT]
        > Puanlama betiği istemcilerden gönderilen verileri alır ve puanlama modeli geçirir. Betik ve model veri yapısı belgeleyin. Bu belgeleri olan şey web hizmeti kullanmak üzere bir istemci oluştururken kolaylaştırır.

    * [Bir ortam dosyası (myenv.yml) oluşturma](tutorial-deploy-models-with-aml.md#create-environment-file) 

   Aşağıdaki örnek, görüntü yapılandırmak için bu dosyaları kullanır:

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

1. Görüntüyü oluşturmak için model ve görüntü yapılandırmasını kullanın. Bu işlemin tamamlanması yaklaşık 5 dakika sürebilir:

    ```python
    image = ContainerImage.create(name = "myimage1",
                                    # this is the model object
                                    models = [model],
                                    image_config = image_config,
                                    workspace = ws)

    # Wait for the create process to complete
    image.wait_for_creation(show_output = True)
    ```

## <a name="create-the-aks-cluster"></a>AKS kümesi oluşturma

Aşağıdaki kod parçacığı, AKS kümesi oluşturma işlemi gösterilmektedir. Bu işlemin tamamlanması yaklaşık 20 dakika sürer:

> [!IMPORTANT]
> Olan bir AKS kümesi oluşturma işlemi için çalışma süresi. Oluşturulduktan sonra bu küme birden çok dağıtım için yeniden kullanabilirsiniz. Küme veya onu içeren kaynak grubunu silerseniz, sonraki açışınızda dağıtmanız gerekir. yeni bir küme oluşturmanız gerekir.


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

### <a name="attach-existing-aks-cluster-optional"></a>(İsteğe bağlı) mevcut AKS kümesi ekleme

Azure aboneliğinizde var olan bir AKS kümesi varsa, görüntünüzü dağıtmak için kullanabilirsiniz. Aşağıdaki kod parçacığı, bir küme çalışma alanınıza eklemek gösterilmektedir. 

> [!IMPORTANT]
> Yalnızca sürüm 1.11.3 AKS desteklenir.

```python
# Get the resource id from https://porta..azure.com -> Find your resource group -> click on the Kubernetes service -> Properties
resource_id = '/subscriptions/<your subscription id>/resourcegroups/<your resource group>/providers/Microsoft.ContainerService/managedClusters/<your aks service name>'

# Set to the name of the cluster
cluster_name='my-existing-aks' 

# Attatch the cluster to your workgroup
aks_target = AksCompute.attach(workspace=ws, name=cluster_name, resource_id=resource_id)

# Wait for the operation to complete
aks_target.wait_for_completion(True)
```

## <a name="deploy-your-web-service"></a>Web hizmetini dağıtma

Aşağıdaki kod parçacığı AKS kümeye görüntüsünü dağıtmayı gösterir:

```python
from azureml.core.webservice import Webservice, AksWebservice

# Set configuration and service name
aks_config = AksWebservice.deploy_configuration()
aks_service_name ='aks-service-1'
# Deploy from image
aks_service = Webservice.deploy_from_image(workspace = ws, 
                                            name = aks_service_name,
                                            image = image,
                                            deployment_config = aks_config,
                                            deployment_target = aks_target)
# Wait for the deployment to complete
aks_service.wait_for_deployment(show_output = True)
print(aks_service.state)
```

> [!TIP]
> Dağıtım sırasında bir hata varsa, kullanmak `aks_service.get_logs()` AKS hizmeti günlükleri görüntülemek için. Günlüğe kaydedilen bilgileri hatanın nedenini gösterir.

## <a name="test-the-web-service"></a>Web hizmetini test edin

Kullanım `aks_service.run()` web hizmeti test etmek için. Aşağıdaki kod parçacığı, veri hizmetine iletmek ve tahmin görüntülemek gösterilmektedir:

```python
import json

test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10], 
    [10,9,8,7,6,5,4,3,2,1]
]})
test_sample = bytes(test_sample,encoding = 'utf8')

prediction = aks_service.run(input_data = test_sample)
print(prediction)
```

## <a name="update-the-web-service"></a>Web hizmetini güncelleştirmek

Web hizmetini güncelleştirmek için `update` yöntemi. Aşağıdaki kod, yeni görüntüyü kullanarak web hizmetini güncelleştirmek gösterilmektedir:

```python
from azureml.core.webservice import Webservice

service_name = 'aci-mnist-3'
# Retrieve existing service
service = Webservice(name = service_name, workspace = ws)
# Update the image used by the service
service.update(image = new-image)
print(service.state)
```

> [!NOTE]
> Web hizmeti bir görüntüsünü güncelleştirdiğinizde otomatik olarak güncelleştirilmez. Ayrıca, yeni görüntüyü kullanmak istediğiniz her hizmet el ile güncelleştirmeniz gerekir.

## <a name="cleanup"></a>Temizleme

Hizmet, görüntü ve model silmek için aşağıdaki kod parçacığını kullanın:

```python
aks_service.delete()
image.delete()
model.delete()
```

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [ML Model dağıtılan web hizmeti olarak Tüket](how-to-consume-web-service.md).