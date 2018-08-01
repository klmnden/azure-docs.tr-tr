---
title: Azure Machine Learning Model Yönetimi Kurulumu ve yapılandırması | Microsoft Docs
description: Bu belgede adımlar ve kavramlar açıklanır ayarlama ve Azure Machine Learning'de Model yönetimi yapılandırma katılan.
services: machine-learning
author: aashishb
ms.author: aashishb
manager: hjerez
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 12/6/2017
ms.openlocfilehash: 150114184f6f04f22aa9da409758daa6a0d175b5
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39369054"
---
# <a name="model-management-setup"></a>Model Yönetimi Kurulumu

Bu belge, dağıtmak ve makine öğrenimi Modellerinizi web Hizmetleri olarak yönetmek için Azure ML model Yönetimi kullanmaya başlamanıza yardımcı olur. 

Azure ML model Yönetimi'ni kullanarak, verimli bir şekilde dağıtın ve birkaç SparkML, Keras, TensorFlow, Microsoft Bilişsel Araç Seti veya Python gibi çerçeveler kullanılarak oluşturulan makine öğrenimi modellerini yönetin. 

Bu belgenin sonunda, model yönetim ortamınızı ayarlama ve makine öğrenimi Modellerinizi dağıtmak için hazır olması mümkün olması gerekir.

## <a name="what-you-need-to-get-started"></a>Başlamak için ihtiyacınız olanlar
En iyi bu kılavuz için bir Azure aboneliği veya Modellerinizi için dağıtabileceğiniz bir kaynak grubu Katılımcısı erişimi olması gerekir.
CLI'yı Azure Machine Learning Workbench ve üzerinde önceden yüklü olarak gelen [Azure Dsvm'leri](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-virtual-machine-overview).

## <a name="using-the-cli"></a>CLI kullanarak
Workbench'ten komut satırı arabirimi (Clı'ler) kullanmak için **dosya** -> **komut istemini Aç**. 

Üzerinde bir veri bilimi sanal makinesi, bağlanın ve komut istemini açın. Tür `az ml -h` seçenekleri görmek için. Komutlara ilişkin daha fazla ayrıntı için kullanın help bayrağı.

Diğer tüm sistemlerde Clı'yi yüklemek gerekir.

>[!NOTE]
> Bir Jupyter not defteri üzerinde bir Linux DSVM'sini, Azure CLI ve Azure ML CLI komut biçimiyle erişebilirsiniz.  **Bir Linux DSVM'sini bir Jupyter not defteri için özellikle budur**.  Not defterinde geçerli Python çekirdek bu komutlara erişmek (örneğin conda `py35` ortamı)
>```
>import sys
>! {sys.executable} -m azure.cli login
>! {sys.executable} -m azure.cli ml -h
>```

### <a name="installing-or-updating-on-windows"></a>Yükleme (veya güncelleştirme) Windows üzerinde

Python'dan yükleme https://www.python.org/. Instalace pip SE seçtiğinizden emin olun.

Yönetici olarak çalıştır'ı kullanarak bir komut istemi açın ve aşağıdaki komutları çalıştırın:

```cmd
pip install -r https://aka.ms/az-ml-o16n-cli-requirements-file
```

### <a name="installing-or-updating-on-linux"></a>Yükleme (veya güncelleştirme) Linux'ta
Komut satırından aşağıdaki komutu çalıştırın ve yönergeleri izleyin:

```bash
sudo -i
pip install -r https://aka.ms/az-ml-o16n-cli-requirements-file
```

### <a name="configuring-docker-on-linux"></a>Linux'ta Docker'ı yapılandırma
Kök olmayan kullanıcılar tarafından kullanılması için Linux'ta Docker yapılandırmak için yönergeleri izleyin: [Linux için yükleme sonrası adımlar](https://docs.docker.com/engine/installation/linux/linux-postinstall/)

>[!NOTE]
> Bir Linux DSVM'sini üzerinde Docker doğru şekilde yapılandırmak için aşağıdaki betiği çalıştırabilirsiniz. **Oturumu kapatın ve betiği çalıştırdıktan sonra tekrar oturum açmayı unutmayın.**
>```
>sudo /opt/microsoft/azureml/initial_setup.sh
>```

## <a name="deploying-your-model"></a>Model dağıtma
Modellerinizi web Hizmetleri olarak dağıtmak için CLI'yı kullanın. Web hizmetleri yerel olarak veya bir kümeye dağıtabilirsiniz.

İle yerel bir dağıtımı başlatın, model ve koda, ardından çalıştığını doğrulama üretim ölçek kullanmak için bir kümeye dağıtın.

Başlamak için dağıtım ortamı oluşturmanız gerekir. Bir ortam Kurulumu olduğu zaman görev. Kurulum tamamlandıktan sonra ortamı sonraki dağıtımlar için yeniden kullanabilirsiniz. Daha fazla ayrıntı için aşağıdaki bölüme bakın.

Ortam Kurulumu tamamlanırken:
- Azure'da oturum açmanız istenir. Oturum açmak için bir web tarayıcısı kullanarak https://aka.ms/devicelogin ve kimlik doğrulaması için sağlanan kod girin.
- Kimlik doğrulama işlemi sırasında kimlik doğrulaması yapmak bir hesap istenir. Önemli: geçerli bir Azure aboneliği ve kaynak hesabı oluşturmak için yeterli izinlere sahip bir hesap seçin. Oturum açma tamamlandıktan sonra abonelik bilgilerinizi sunulur ve seçili hesapla devam etmek istiyor olup isteniyor.

### <a name="environment-setup"></a>Ortam Kurulumu
Kurulum işlemini başlatmak için aşağıdaki komutları girerek birkaç ortam sağlayıcılarını kaydetme gerekir:

```azurecli
az provider register -n Microsoft.MachineLearningCompute
az provider register -n Microsoft.ContainerRegistry
az provider register -n Microsoft.ContainerService
```
#### <a name="local-deployment"></a>Yerel dağıtım
Dağıtma ve web hizmetinizi yerel makinede test etmek için aşağıdaki komutu kullanarak yerel bir ortamı ayarlayın. Kaynak grubu adı isteğe bağlıdır.

```azurecli
az ml env setup -l [Azure Region, e.g. eastus2] -n [your environment name] [-g [existing resource group]]
```
>[!NOTE] 
>Yerel web hizmeti dağıtımı, Docker yerel makinede yüklemenizi istiyor. 
>

Yerel ortam Kurulum komutu aboneliğinizde aşağıdaki kaynakları oluşturur:
- Bir kaynak grubu (sağlanmadığında veya sağlanan ad mevcut değilse)
- Bir depolama hesabı
- Azure Container Registry (ACR)
- Bir Application ınsights hesabı

Kurulum başarıyla tamamlandıktan sonra aşağıdaki komutu kullanarak kullanılacak ortamı ayarlayın:

```azurecli
az ml env set -n [environment name] -g [resource group]
```

#### <a name="cluster-deployment"></a>Küme dağıtımı
Küme dağıtımı, yüksek ölçekli üretim senaryoları için kullanın. Bu ACS Kubernetes kümesine düzenleyicisi olarak ayarlar. ACS kümesi, web hizmeti çağrıları için daha büyük bir aktarım hızı işlemek için genişletilebilir.

Web hizmetiniz bir üretim ortamına dağıtmak için öncelikle aşağıdaki komutu kullanarak ortamı ayarlayın:

```azurecli
az ml env setup --cluster -n [your environment name] -l [Azure region e.g. eastus2] [-g [resource group]]
```

Küme ortamı Kurulum komutu aboneliğinizde aşağıdaki kaynakları oluşturur:
- Bir kaynak grubu (sağlanmadığında veya sağlanan ad mevcut değilse)
- Bir depolama hesabı
- Azure Container Registry (ACR)
- Azure Container Service (ACS) kümesinde Kubernetes dağıtımı
- Bir Application ınsights hesabı

>[!IMPORTANT]
> Bir küme ortamında başarıyla oluşturmak için Azure abonelik veya kaynak grubu üzerinde katkıda bulunan erişimine sahip olması gerekir.

Kaynak grubu, depolama hesabı ve ACR hızlı bir şekilde oluşturulur. ACS dağıtımı 20 dakikaya kadar sürebilir. 

Bir devam eden küme sağlama durumunu denetlemek için aşağıdaki komutu kullanın:

```azurecli
az ml env show -n [environment name] -g [resource group]
```

Kurulum tamamlandıktan sonra bu dağıtım için kullanılacak ortamı gerekir. Aşağıdaki komutu kullanın:

```azurecli
az ml env set -n [environment name] -g [resource group]
```

>[!NOTE] 
> Sonraki dağıtımlarda, ortam oluşturulduktan sonra yalnızca yeniden kullanmak için yukarıdaki kümesi komutunu kullanmanız gerekir.
>

### <a name="create-a-model-management-account"></a>Model Yönetimi hesabı oluşturma
Modelleri dağıtmak için bir model Yönetimi hesabı gereklidir. Abonelik başına bir kez gerçekleştirmeniz gereken ve birden çok dağıtımlarında aynı hesabı kullanabilirsiniz.

Yeni bir hesap oluşturmak için aşağıdaki komutu kullanın:

```azurecli
az ml account modelmanagement create -l [Azure region, e.g. eastus2] -n [your account name] -g [resource group name] --sku-instances [number of instances, e.g. 1] --sku-name [Pricing tier for example S1]
```

Var olan bir hesap için aşağıdaki komutu kullanın:
```azurecli
az ml account modelmanagement set -n [your account name] -g [resource group it was created in]
```

Bu işlem sonucunda, ortamı hazırdır ve model Yönetimi hesabı yönetme ve makine öğrenimi modellerini dağıtmak için gereken özellikleri sağlamak için oluşturulan (bkz [Azure Machine Learning Model Yönetimi](model-management-overview.md) için bir Genel Bakış).

## <a name="next-steps"></a>Sonraki adımlar

* Bir yerel makine veya bir küme üzerinde çalıştırmak için web hizmetlerini dağıtma hakkında yönergeler için devam etmek için [makine öğrenme modeli bir Web hizmeti olarak dağıtma](model-management-service-deploy.md).
* Galerideki birçok örneklerinden birini deneyin.
