---
title: Azure Machine Learning modeli Yönetim Kurulumu ve yapılandırması | Microsoft Docs
description: Bu belgede adımları ve kavramları açıklamaktadır ayarlama ve Azure Machine Learning modeli yönetimini yapılandırma katılan.
services: machine-learning
author: aashishb
ms.author: aashishb
manager: hjerez
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 12/6/2017
ms.openlocfilehash: 0859031ac26b061861aa51dce1093f2fe4350935
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="model-management-setup"></a>Model Yönetimi Kurulumu

Bu belge, Azure ML model yönetim dağıtmak ve yönetmek, machine learning web Hizmetleri olarak modelleri için kullanmaya alır. 

Azure ML model Yönetimi'ni kullanarak verimli bir şekilde dağıtın ve çerçeveleri SparkML, Keras, TensorFlow, Microsoft Bilişsel Araç Seti veya Python dahil olmak üzere çeşitli kullanılarak oluşturulan Machine Learning modellerini yönetin. 

Bu belgenin sonuna ayarlayabilir ve hazır, machine learning modellerini dağıtmak için model yönetim ortamına sahip yapabiliyor olmanız gerekir.

## <a name="what-you-need-to-get-started"></a>Başlamak için ihtiyacınız olanlar
Bu kılavuz yararlanmak için bir Azure aboneliği veya Modellerinizi için dağıtabileceğiniz bir kaynak grubu katkıda bulunan erişiminiz olması.
CLI Azure Machine Learning çalışma ekranı ve üzerinde önceden yüklü olarak gelen [Azure DSVMs](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-virtual-machine-overview).

## <a name="using-the-cli"></a>CLI kullanarak
Çalışma ekranı komut satırı arabirimlerinden (CLIs) kullanmak için tıklatın **dosya** -> **komut istemini açın**. 

Üzerinde veri bilimi sanal makine, bağlanın ve komut istemini açın. Tür `az ml -h` seçenekleri görmek için. Komutlar hakkında daha fazla ayrıntı için kullanmak Yardım bayrağı.

Diğer tüm sistemlerde CLIs yüklemeniz gerekir.

### <a name="installing-or-updating-on-windows"></a>Yükleme (veya güncelleştirme) Windows

Gelen Python yüklemek https://www.python.org/. PIP yüklemek için seçtiğinizden emin olun.

Yönetici olarak çalıştır'ı kullanarak bir komut istemi açın ve aşağıdaki komutları çalıştırın:

```cmd
pip install -r https://aka.ms/az-ml-o16n-cli-requirements-file
```

### <a name="installing-or-updating-on-linux"></a>Yükleme (veya güncelleştirme) Linux
Komut satırından şu komutu çalıştırın ve yönergeleri izleyin:

```bash
sudo -i
pip install -r https://aka.ms/az-ml-o16n-cli-requirements-file
```

### <a name="configuring-docker-on-linux"></a>Docker Linux üzerinde yapılandırma
Kök olmayan kullanıcılar tarafından kullanılması için Linux'ta Docker yapılandırmak için buradaki yönergeleri izleyin: [yükleme sonrası adımlar Linux için](https://docs.docker.com/engine/installation/linux/linux-postinstall/)

>[!NOTE]
> Linux DSVM üzerinde Docker doğru şekilde yapılandırmak için aşağıdaki komut dosyasını çalıştırabilirsiniz. **Oturumu kapatın ve komut dosyasını çalıştırdıktan sonra tekrar oturum açmak unutmayın.**
>```
>sudo /opt/microsoft/azureml/initial_setup.sh
>```

## <a name="deploying-your-model"></a>Model dağıtma
CLI modelleri web Hizmetleri olarak dağıtmak için kullanın. Web Hizmetleri, yerel olarak veya bir küme dağıtılabilir.

Yerel bir dağıtımı ile'ı başlatın, modeli ve kod, ardından çalıştığını doğrulamak üretim ölçeği kullanmak için bir kümeye dağıtın.

Başlatmak için dağıtım ortamı kurmanız gerekir. Bir ortamı kurulması saat görevi. Kurulum tamamlandıktan sonra ortam sonraki dağıtımlar için yeniden kullanabilirsiniz. Daha fazla ayrıntı için aşağıdaki bölüme bakın.

Ortam Kurulumu tamamlanırken:
- Azure'da oturum açın istenir. Oturum açmak için sayfayı açmak için bir web tarayıcısı kullanın https://aka.ms/devicelogin ve kimlik doğrulaması için sağlanan kod girin.
- Kimlik doğrulama işlemi sırasında kimlik doğrulaması yapmak bir hesap istenir. Önemli: geçerli bir Azure aboneliğinizin ve hesaptaki kaynakları oluşturmak için yeterli izinlere sahip bir hesap seçin. Oturum açma tamamlandığında, abonelik bilgilerinizi sunulur ve seçili hesabıyla devam etmek istiyor isteyip sorulur.

### <a name="environment-setup"></a>Ortam Kurulumu
Kurulum işlemi başlatmak için aşağıdaki komutları girerek birkaç ortam sağlayıcıları kaydetmeniz gerekir:

```azurecli
az provider register -n Microsoft.MachineLearningCompute
az provider register -n Microsoft.ContainerRegistry
az provider register -n Microsoft.ContainerService
```
#### <a name="local-deployment"></a>Yerel dağıtım
Dağıtma ve web hizmetinizi yerel makine üzerinde test etmek için aşağıdaki komutu kullanarak yerel bir ortamı ayarlayın. Kaynak grubu adı isteğe bağlıdır.

```azurecli
az ml env setup -l [Azure Region, e.g. eastus2] -n [your environment name] [-g [existing resource group]]
```
>[!NOTE] 
>Yerel web hizmeti dağıtımı Docker yerel makinede yüklemenizi gerektirir. 
>

Yerel ortamdaki Kurulum komutu aşağıdaki kaynaklar, aboneliğinizde oluşturur:
- Bir kaynak grubu (sağlanmadığında veya sağlanan adı yoksa)
- bir depolama hesabı
- Bir Azure kapsayıcı kayıt defteri (ACR)
- Bir uygulama Öngörüler hesabı

Kurulum başarıyla tamamlandıktan sonra aşağıdaki komutu kullanarak kullanılacak ortamı ayarlayın:

```azurecli
az ml env set -n [environment name] -g [resource group]
```

#### <a name="cluster-deployment"></a>Küme dağıtımı
Küme dağıtımı, büyük ölçekli üretim senaryoları için kullanın. Bu Kubernetes ACS kümeyle orchestrator ayarlar. ACS küme, web hizmeti çağrıları için büyük verimi işlemek için genişletilebilir.

Web hizmetiniz için bir üretim ortamında dağıtmak için önce aşağıdaki komutu kullanarak ortamını ayarlarken ayarlayın:

```azurecli
az ml env setup --cluster -n [your environment name] -l [Azure region e.g. eastus2] [-g [resource group]]
```

Küme ortamı Kurulum komutu aşağıdaki kaynaklar, aboneliğinizde oluşturur:
- Bir kaynak grubu (sağlanmadığında veya sağlanan adı yoksa)
- bir depolama hesabı
- Bir Azure kapsayıcı kayıt defteri (ACR)
- Bir Azure kapsayıcı Hizmeti'ni (ACS) küme üzerinde Kubernetes dağıtımı
- Bir uygulama Öngörüler hesabı

>[!IMPORTANT]
> Bir küme ortamında başarılı bir şekilde oluşturmak için olmasını Azure abonelik veya kaynak grubu üzerinde katılımcı erişim gerekir.

Kaynak grubu, depolama hesabı ve ACR hızlı bir şekilde oluşturulur. ACS dağıtımı 20 dakikaya kadar sürebilir. 

Devam eden küme sağlama durumunu denetlemek için aşağıdaki komutu kullanın:

```azurecli
az ml env show -n [environment name] -g [resource group]
```

Kurulum tamamlandıktan sonra bu dağıtım için kullanılacak ortamı gerekir. Aşağıdaki komutu kullanın:

```azurecli
az ml env set -n [environment name] -g [resource group]
```

>[!NOTE] 
> Sonraki dağıtımları için ortamı oluşturulduktan sonra yalnızca onu tekrar için yukarıdaki kümesi komutunu kullanmanız gerekir.
>

### <a name="create-a-model-management-account"></a>Bir Model yönetim hesabı oluşturun
Bir model yönetim hesabı modelleri dağıtmak için gereklidir. Abonelik başına bu bir kez yapmanız gerekir ve birden çok dağıtım aynı hesapta yeniden kullanabilirsiniz.

Yeni bir hesap oluşturmak için aşağıdaki komutu kullanın:

```azurecli
az ml account modelmanagement create -l [Azure region, e.g. eastus2] -n [your account name] -g [resource group name] --sku-instances [number of instances, e.g. 1] --sku-name [Pricing tier for example S1]
```

Var olan bir hesap kullanmak için aşağıdaki komutu kullanın:
```azurecli
az ml account modelmanagement set -n [your account name] -g [resource group it was created in]
```

Bu işlem sonucunda ortamı hazır ve model yönetim hesabı yönetmek ve Machine Learning modellerini dağıtmak için gereken özellikleri sağlamak için oluşturulan (bkz [Azure Machine Learning modeli Yönetim](model-management-overview.md) için bir Genel Bakış).

## <a name="next-steps"></a>Sonraki adımlar

* Yerel makine veya bir küme üzerinde çalıştırmak için web hizmetlerini dağıtma hakkında yönergeler için devam etmek için [makine öğrenimi modeline bir Web hizmeti olarak dağıtma](model-management-service-deploy.md).
* Galeri birçok örneklerinde birini deneyin.
