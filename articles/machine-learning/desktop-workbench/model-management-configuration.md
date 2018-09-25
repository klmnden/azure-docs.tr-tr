---
title: Azure Machine Learning Model Yönetimi Kurulumu ve yapılandırması | Microsoft Docs
description: Bu belgede adımlar ve kavramlar açıklanır ayarlama ve Azure Machine Learning'de Model yönetimi yapılandırma katılan.
services: machine-learning
author: raymondlaghaeian
ms.author: raymondl
manager: hjerez
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 08/29/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 6660657141cc5aac532d121b61c7c8db6a24ccda
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46968722"
---
# <a name="model-management-setup"></a>Model Yönetimi Kurulumu

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 



## <a name="overview"></a>Genel Bakış
Bu belge, dağıtmak ve makine öğrenimi Modellerinizi web Hizmetleri olarak yönetmek için Azure ML model Yönetimi kullanmaya başlamanıza yardımcı olur. 

Azure ML model Yönetimi'ni kullanarak, verimli bir şekilde dağıtın ve birkaç SparkML, Keras, TensorFlow, Microsoft Bilişsel Araç Seti veya Python gibi çerçeveler kullanılarak oluşturulan makine öğrenimi modellerini yönetin. 

Bu belgenin sonunda, model yönetim ortamınızı ayarlama ve makine öğrenimi Modellerinizi dağıtmak için hazır olması mümkün olması gerekir.

## <a name="what-you-need-to-get-started"></a>Başlamak için ihtiyacınız olanlar
Bu kılavuzun dışına en elde etmek için Modellerinizi için dağıttığınız bir Azure aboneliğine sahip erişimi olmalıdır.
CLI'yı Azure Machine Learning Workbench ve üzerinde önceden yüklü olarak gelen [Azure Dsvm'leri](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-virtual-machine-overview).

## <a name="using-the-cli"></a>CLI kullanarak
Workbench'ten komut satırı arabirimi (Clı'ler) kullanmak için **dosya** -] **açık komut satırı arabirimi**. 

Üzerinde bir veri bilimi sanal makinesi, bağlanın ve komut istemini açın. Tür `az ml -h` seçenekleri görmek için. Komutlara ilişkin daha fazla ayrıntı için kullanın help bayrağı.

Diğer tüm sistemlerde Clı'yi yüklemek gerekir.

### <a name="installing-or-updating-on-windows"></a>Yükleme (veya güncelleştirme) Windows üzerinde

Python'dan yükleme https://www.python.org/. Instalace pip SE seçtiğinizden emin olun.

Yönetici olarak çalıştır'ı kullanarak bir komut istemi açın ve aşağıdaki komutları çalıştırın:

```cmd
pip install azure-cli
pip install azure-cli-ml
```
 
>[!NOTE]
>Önceki bir sürümü varsa, bunu önce aşağıdaki komutu kullanarak kaldırın:
>

```cmd
pip uninstall azure-cli-ml
```

### <a name="installing-or-updating-on-linux"></a>Yükleme (veya güncelleştirme) Linux'ta
Komut satırından aşağıdaki komutu çalıştırın ve yönergeleri izleyin:

```bash
sudo -i
pip install azure-cli
pip install azure-cli-ml
```

Yükleme tamamlandıktan sonra aşağıdaki komutu çalıştırın:

```bash
sudo /opt/microsoft/azureml/initial_setup.sh
```

>[!NOTE]
>Oturumu Kapat ve değişikliklerin etkili olması SSH oturumunuz için yeniden oturum açın.
>Yukarıdaki komutları Clı'leri DSVM üzerinde daha önceki bir sürümünü güncelleştirmek için kullanabilirsiniz.
>

## <a name="deploying-your-model"></a>Model dağıtma
Clı'yi Modellerinizi web Hizmetleri olarak dağıtmak için kullanın. Web hizmetleri yerel olarak veya bir kümeye dağıtabilirsiniz.

İle yerel bir dağıtımı başlatın, model ve koda, ardından çalıştığını doğrulama üretim ölçek kullanmak için bir kümeye dağıtın.

Başlamak için dağıtım ortamı oluşturmanız gerekir. Bir ortam Kurulumu olduğu zaman görev. Kurulum tamamlandıktan sonra ortamı sonraki dağıtımlar için yeniden kullanabilirsiniz. Daha fazla ayrıntı için aşağıdaki bölüme bakın.

Ortam Kurulumu tamamlanırken:
- Azure'da oturum açmanız istenir. Oturum açmak için bir web tarayıcısı kullanarak https://aka.ms/devicelogin ve kimlik doğrulaması için sağlanan kod girin.
- Kimlik doğrulama işlemi sırasında kimlik doğrulaması yapmak bir hesap istenir. Önemli: geçerli bir Azure aboneliği ve kaynak hesabı oluşturulamadı. yeterli izinlere sahip bir hesap seçin - oturum açma tamamlandıktan sonra abonelik bilgilerinizi sunulur ve ile devam etmek istiyor olup isteniyor Seçili hesap.

### <a name="environment-setup"></a>Ortam Kurulumu
Kurulum işlemini başlatmak için aşağıdaki komutu girerek ortam sağlayıcısını kaydetmeniz gerekir:

```azurecli
az provider register -n Microsoft.MachineLearningCompute
```

#### <a name="local-deployment"></a>Yerel dağıtım
Dağıtma ve web hizmetinizi yerel makinede test etmek için aşağıdaki komutu kullanarak yerel bir ortam ayarlayın:

```azurecli
az ml env setup -l [location of Azure Region, e.g. eastus2] -n [your environment name] [-g [existing resource group]]
```
>[!NOTE] 
>Yerel web hizmeti dağıtımı gerektirir, yerel makinede Docker yüklemek için. 
>

Yerel ortam Kurulum komutu aboneliğinizde aşağıdaki kaynakları oluşturur:
- Bir kaynak grubu (sağlanmadığında)
- Bir depolama hesabı
- Azure Container Registry (ACR)
- Application Insights

Kurulum başarıyla tamamlandıktan sonra aşağıdaki komutu kullanarak kullanılacak ortamı ayarlayın:

```azurecli
az ml env set -n [environment name] -g [resource group]
```

#### <a name="cluster-deployment"></a>Küme dağıtımı
Küme dağıtımı, yüksek ölçekli üretim senaryoları için kullanın. Bu ACS Kubernetes kümesine düzenleyicisi olarak ayarlar. ACS kümesi, web hizmeti çağrıları için daha büyük bir aktarım hızı işlemek için genişletilebilir.

Web hizmetiniz bir üretim ortamına dağıtmak için öncelikle aşağıdaki komutu kullanarak ortamı ayarlayın:

```azurecli
az ml env setup -c --name [your environment name] --location [Azure region e.g. eastus2] [-g [resource group]]
```

Küme ortamı Kurulum komutu aboneliğinizde aşağıdaki kaynakları oluşturur:
- Bir kaynak grubu (sağlanmadığında)
- Bir depolama hesabı
- Azure Container Registry (ACR)
- Azure Container Service (ACS) kümesinde Kubernetes dağıtımı
- Application Insights

Kaynak grubu, depolama hesabı ve ACR hızlı bir şekilde oluşturulur. ACS dağıtımı 20 dakikaya kadar sürebilir. 

Kurulum tamamlandıktan sonra bu dağıtım için kullanılacak ortamı gerekir. Aşağıdaki komutu kullanın:

```azurecli
az ml env set -n [environment name] -g [resource group]
```

>[!NOTE] 
> Sonraki dağıtımlarda, ortam oluşturulduktan sonra yalnızca yeniden kullanmak için yukarıdaki kümesi komutunu kullanmanız gerekir.
>

>[!NOTE] 
>Bir HTTPS uç noktası oluşturmak için bir SSL sertifikası kullanarak bir küme oluşturma sırasında belirtin az ml env Kurulum sertifika adı ve--sertifika pem seçenekleri. Bu küme belirtilen sertifika kullanılarak güvenli https üzerinde isteklerine hizmet vermeye ayarlar. Kurulum tamamlandıktan sonra küme FQDN için bir CNAME DNS kaydı oluşturun.

### <a name="create-an-account"></a>Hesap oluşturun
Modelleri dağıtmak için bir hesap gereklidir. Hesap başına bir kez gerçekleştirmeniz gereken ve birden çok dağıtımlarında aynı hesabı kullanabilirsiniz.

Yeni bir hesap oluşturmak için aşağıdaki komutu kullanın:

```azurecli
az ml account modelmanagement create -l [Azure region, e.g. eastus2] -n [your account name] -g [resource group name] --sku-instances [number of instances, e.g. 1] --sku-name [Pricing tier for example S1]
```

Var olan bir hesap için aşağıdaki komutu kullanın:
```azurecli
az ml account modelmanagement set -n [your account name] -g [resource group it was created in]
```

### <a name="deploy-your-model"></a>Modelinizi dağıtma
Şimdi kaydedilmiş modelinizi bir web hizmeti olarak dağıtmaya hazırsınız. 

```azurecli
az ml service create realtime --model-file [model file/folder path] -f [scoring file e.g. score.py] -n [your service name] -s [schema file e.g. service_schema.json] -r [runtime for the Docker container e.g. spark-py or python] -c [conda dependencies file for additional python packages]
```

### <a name="next-steps"></a>Sonraki Adımlar
Galerideki birçok örneklerinden birini deneyin.
