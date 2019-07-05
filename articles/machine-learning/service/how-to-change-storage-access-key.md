---
title: Depolama hesabı erişim anahtarlarını değiştirme
titleSuffix: Azure Machine Learning service
description: Çalışma alanınızı tarafından kullanılan Azure depolama hesabı erişim anahtarlarını değiştirme konusunda bilgi edinin. Azure Machine Learning hizmeti, veri ve modelleri depolamak için bir Azure depolama hesabı kullanır. Depolama hesabı için erişim anahtarı yeniden ürettiğinizde, Azure Machine Learning hizmetini yeni anahtarları kullanacak şekilde güncelleştirmeniz gerekir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 05/17/2019
ms.openlocfilehash: 488a032e177897caf2897ba6335f4e7f64dc0e4d
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543839"
---
# <a name="regenerate-storage-account-access-keys"></a>Depolama hesabı erişim anahtarlarını yeniden oluştur

Azure Machine Learning hizmeti tarafından kullanılan Azure depolama hesapları için erişim anahtarlarını değiştirme konusunda bilgi edinin. Azure Machine Learning, veri veya eğitilen modelleri depolamak için depolama hesapları kullanabilirsiniz.

Güvenlik nedeniyle, bir Azure depolama hesabı erişim anahtarlarını değiştirmeniz gerekebilir. Erişim anahtarı yeniden ürettiğinizde, Azure Machine Learning yeni anahtarı kullanacak şekilde güncelleştirilmesi gerekir. Azure Machine Learning depolama hesabı için iki modeli depolama ve bir veri deposu olarak kullanıyor olabilir.

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure Machine Learning hizmeti çalışma alanı. Daha fazla bilgi için [çalışma alanı oluşturma](setup-create-workspace.md) makalesi.

* [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

* [Azure Machine Learning CLI uzantısını](reference-azure-machine-learning-cli.md).

<a id="whattoupdate"></a> 

## <a name="what-needs-to-be-updated"></a>Güncelleştirilmesi gerekiyor

Depolama hesapları (depolama günlükleri, modelleri, anlık görüntüler, vb.) Azure Machine Learning hizmeti çalışma alanında ve bir veri deposu olarak kullanılabilir. Çalışma alanını güncelleştirmek için işlem tek bir Azure CLI komutunu ve olması çalıştırdık depolama anahtarını güncelleştirdikten sonra. Veri depoları güncelleştirme işlemi daha karmaşıktır ve hangi veri depoları şu anda depolama hesabı kullanarak keşfetme ve daha sonra yeniden kaydetme gerektirir.

> [!IMPORTANT]
> Azure CLI ve aynı anda Python kullanarak veri depoları kullanarak çalışma güncelleştirin. Tek veya diğer güncelleştirme yeterli değildir ve her ikisi de güncelleştirilene kadar hatalarına neden olabilir.

Veri depoları tarafından kullanılan depolama hesaplarını bulmak için aşağıdaki kodu kullanın:

```python
import azureml.core
from azureml.core import Workspace, Datastore

ws = Workspace.from_config()

default_ds = ws.get_default_datastore()
print("Default datstore: " + default_ds.name + ", storage account name: " + default_ds.account_name + ", container name: " + ds.container_name)

datastores = ws.datastores
for name, ds in datastores.items():
    if ds.datastore_type == "AzureBlob" or ds.datastore_type == "AzureFile":
        print("datastore name: " + name + ", storage account name: " + ds.account_name + ", container name: " + ds.container_name)
```

Bu kod, Azure depolama kullanan tüm kayıtlı veri depoları için görünür ve aşağıdaki bilgileri listeler:

* Veri deposu adı: Depolama hesabının kayıtlı olduğu veri deposu adı.
* Depolama hesabı adı: Azure depolama hesabı adı.
* Kapsayıcı: Bu kaydı tarafından kullanılan depolama hesabı, kapsayıcı.

Veri deposu adını, depolama hesabı adı ve kapsayıcı adı için erişim anahtarlarını yeniden oluşturma üzerinde planladığınız depolama hesabı için bir giriş varsa kaydedin.

## <a name="update-the-access-key"></a>Erişim anahtarını güncelleştir

Yeni anahtarı kullanmak için Azure Machine Learning hizmeti güncelleştirmek için aşağıdaki adımları kullanın:

> [!IMPORTANT]
> Her ikisi de CLI, Python kullanarak veri depoları ile çalışma alanı güncelleştiriliyor, tüm adımları gerçekleştirin. Her ikisi de güncelleştirilene kadar tek veya diğer güncelleştirme hatalarına neden olabilir.

1. Anahtarı yeniden oluştur. Bir erişim anahtarının yeniden oluşturulması hakkında daha fazla bilgi için bkz: [depolama hesabı yönetme](/azure/storage/common/storage-account-manage#access-keys) makalesi. Yeni anahtar kaydedin.

1. Yeni anahtar kullanılacak çalışma alanını güncelleştirmek için aşağıdaki adımları kullanın:

    1. Aşağıdaki Azure CLI komutunu kullanarak çalışma alanınızı içeren Azure aboneliği için oturum açmak için:

        ```azurecli-interactive
        az login
        ```

    1. Azure Machine Learning uzantıyı yüklemek için aşağıdaki komutu kullanın:

        ```azurecli-interactive
        az extension add -n azure-cli-ml 
        ```

    1. Yeni anahtar kullanılacak çalışma alanını güncelleştirmek için aşağıdaki komutu kullanın. Değiştirin `myworkspace` Azure Machine Learning çalışma alanı adı ve Değiştir `myresourcegroup` çalışma alanını içeren Azure kaynak grubu adı.

        ```azurecli-interactive
        az ml workspace sync-keys -w myworkspace -g myresourcegroup
        ```

        Bu komut, çalışma alanı tarafından kullanılan Azure depolama hesabı için yeni anahtarları otomatik olarak eşitler.

1. Depolama hesabını kullanan datastore(s) yeniden kaydettirin için değerleri kullanın. [güncelleştirilmesi için gerekenler](#whattoupdate) bölümü ve 1. adım aşağıdaki kodla anahtarı:

    ```python
    ds = Datastore.register_azure_blob_container(workspace=ws, 
                                              datastore_name='your datastore name', 
                                              container_name='your container name',
                                              account_name='your storage account name', 
                                              account_key='new storage account key',
                                              overwrite=True)
    ```

    Bu yana `overwrite=True` belirtilirse, bu kod, mevcut kaydın üzerine yazar ve yeni anahtar kullanacak şekilde güncelleştirir.

## <a name="next-steps"></a>Sonraki adımlar

Veri depoları kaydetme ile ilgili daha fazla bilgi için bkz: [ `Datastore` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) sınıf başvurusu.
