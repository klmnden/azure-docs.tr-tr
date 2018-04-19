---
title: "Azure Machine Learning modeli Yönetim Kurulumu ve yapılandırması | Microsoft Docs"
description: "Bu belgede adımları ve kavramları açıklamaktadır ayarlama ve Azure Machine Learning modeli yönetimini yapılandırma katılan."
services: machine-learning
author: raymondlaghaeian
ms.author: raymondl
manager: hjerez
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 08/29/2017
ms.openlocfilehash: 45ddd4dc6fb5559c020706e2784158b1319f9b52
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
---
# <a name="model-management-setup"></a>Model Yönetimi Kurulumu

## <a name="overview"></a>Genel Bakış
Bu belge, Azure ML model yönetim dağıtmak ve yönetmek, machine learning web Hizmetleri olarak modelleri için kullanmaya alır. 

Azure ML model Yönetimi'ni kullanarak verimli bir şekilde dağıtın ve çerçeveleri SparkML, Keras, TensorFlow, Microsoft Bilişsel Araç Seti veya Python dahil olmak üzere çeşitli kullanılarak oluşturulan Machine Learning modellerini yönetin. 

Bu belgenin sonuna ayarlayabilir ve hazır, machine learning modellerini dağıtmak için model yönetim ortamına sahip yapabiliyor olmanız gerekir.

## <a name="what-you-need-to-get-started"></a>Başlamak için ihtiyacınız olanlar
Bu kılavuzun en dışında almak için Modellerinizi için dağıtabileceğiniz bir Azure aboneliğine sahip erişiminiz olması.
CLI Azure Machine Learning çalışma ekranı ve üzerinde önceden yüklü olarak gelen [Azure DSVMs](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-virtual-machine-overview).

## <a name="using-the-cli"></a>CLI kullanarak
Çalışma ekranı komut satırı arabirimlerinden (CLIs) kullanmak için tıklatın **dosya** -] **açık komut satırı arabirimi**. 

Üzerinde veri bilimi sanal makine, bağlanın ve komut istemini açın. Tür `az ml -h` seçenekleri görmek için. Komutlar hakkında daha fazla ayrıntı için kullanmak Yardım bayrağı.

Diğer tüm sistemlerde CLIs yüklemeniz gerekir.

### <a name="installing-or-updating-on-windows"></a>Yükleme (veya güncelleştirme) Windows

Python https://www.python.org/ yükleyin. PIP yüklemek için seçtiğinizden emin olun.

Yönetici olarak çalıştır'ı kullanarak bir komut istemi açın ve aşağıdaki komutları çalıştırın:

```cmd
pip install azure-cli
pip install azure-cli-ml
```
 
>[!NOTE]
>Önceki bir sürümü varsa, önce aşağıdaki komutu kullanarak kaldırın:
>

```cmd
pip uninstall azure-cli-ml
```

### <a name="installing-or-updating-on-linux"></a>Yükleme (veya güncelleştirme) Linux
Komut satırından şu komutu çalıştırın ve yönergeleri izleyin:

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
>Oturumu kapatın ve değişikliklerin etkili olması SSH oturumunuzun oturum geri dönün.
>Yukarıdaki komutları DSVM üzerinde CLIs önceki bir sürümünü güncelleştirmek için kullanabilirsiniz.
>

## <a name="deploying-your-model"></a>Model dağıtma
CLIs modelleri web Hizmetleri olarak dağıtmak için kullanın. Web Hizmetleri, yerel olarak veya bir küme dağıtılabilir.

Yerel bir dağıtımı ile'ı başlatın, modeli ve kod, ardından çalıştığını doğrulamak üretim ölçeği kullanmak için bir kümeye dağıtın.

Başlatmak için dağıtım ortamı kurmanız gerekir. Bir ortamı kurulması saat görevi. Kurulum tamamlandıktan sonra ortam sonraki dağıtımlar için yeniden kullanabilirsiniz. Daha fazla ayrıntı için aşağıdaki bölüme bakın.

Ortam Kurulumu tamamlanırken:
- Azure'da oturum açın istenir. Oturum açmak için sayfayı https://aka.ms/devicelogin açın ve kimlik doğrulaması için sağlanan kod girmek için bir web tarayıcısı kullanın.
- Kimlik doğrulama işlemi sırasında kimlik doğrulaması yapmak bir hesap istenir. Önemli: geçerli bir Azure aboneliğinizin ve kaynakları hesabı oluşturmak için yeterli izinlere sahip bir hesap seçin - oturum aç tamamlandığında, abonelik bilgilerinizi sunulur ve devam etmek istiyor olup olmadığını istenir Seçilen hesap.

### <a name="environment-setup"></a>Ortam Kurulumu
Kurulum işlemi başlatmak için aşağıdaki komutu girerek ortamı sağlayıcısı kaydetmeniz gerekir:

```azurecli
az provider register -n Microsoft.MachineLearningCompute
```

#### <a name="local-deployment"></a>Yerel dağıtım
Dağıtma ve web hizmetinizi yerel makine üzerinde test etmek için aşağıdaki komutu kullanarak yerel bir ortamı ayarlayın:

```azurecli
az ml env setup -l [location of Azure Region, e.g. eastus2] -n [your environment name] [-g [existing resource group]]
```
>[!NOTE] 
>Yerel web hizmeti dağıtımı gerektirir, Docker yerel makineye yüklemek için. 
>

Yerel ortamdaki Kurulum komutu aşağıdaki kaynaklar, aboneliğinizde oluşturur:
- Bir kaynak grubu (sağlanmadığında)
- bir depolama hesabı
- Bir Azure kapsayıcı kayıt defteri (ACR)
- Application Insights

Kurulum başarıyla tamamlandıktan sonra aşağıdaki komutu kullanarak kullanılacak ortamı ayarlayın:

```azurecli
az ml env set -n [environment name] -g [resource group]
```

#### <a name="cluster-deployment"></a>Küme dağıtımı
Küme dağıtımı, büyük ölçekli üretim senaryoları için kullanın. Bu Kubernetes ACS kümeyle orchestrator ayarlar. ACS küme, web hizmeti çağrıları için büyük verimi işlemek için genişletilebilir.

Web hizmetiniz için bir üretim ortamında dağıtmak için önce aşağıdaki komutu kullanarak ortamını ayarlarken ayarlayın:

```azurecli
az ml env setup -c --name [your environment name] --location [Azure region e.g. eastus2] [-g [resource group]]
```

Küme ortamı Kurulum komutu aşağıdaki kaynaklar, aboneliğinizde oluşturur:
- Bir kaynak grubu (sağlanmadığında)
- bir depolama hesabı
- Bir Azure kapsayıcı kayıt defteri (ACR)
- Bir Azure kapsayıcı Hizmeti'ni (ACS) küme üzerinde Kubernetes dağıtımı
- Application Insights

Kaynak grubu, depolama hesabı ve ACR hızlı bir şekilde oluşturulur. ACS dağıtımı 20 dakikaya kadar sürebilir. 

Kurulum tamamlandıktan sonra bu dağıtım için kullanılacak ortamı gerekir. Aşağıdaki komutu kullanın:

```azurecli
az ml env set -n [environment name] -g [resource group]
```

>[!NOTE] 
> Sonraki dağıtımları için ortamı oluşturulduktan sonra yalnızca onu tekrar için yukarıdaki kümesi komutunu kullanmanız gerekir.
>

>[!NOTE] 
>Bir HTTPS uç noktası oluşturmak için bir SSL sertifikası kullanarak bir küme oluştururken belirtin az ml env Kurulum sertifika adı ve--cert pem seçenekleri. Bu küme güvenliği sağlanan sertifika kullanılarak https üzerinde isteklere yanıt ayarlar. Kurulum tamamlandıktan sonra kümenin FQDN işaret eden bir CNAME DNS kaydı oluşturun.

### <a name="create-an-account"></a>Bir hesap oluşturun
Bir hesap modelleri dağıtmak için gereklidir. Hesap başına bu bir kez yapmanız gerekir ve birden çok dağıtım aynı hesapta yeniden kullanabilirsiniz.

Yeni bir hesap oluşturmak için aşağıdaki komutu kullanın:

```azurecli
az ml account modelmanagement create -l [Azure region, e.g. eastus2] -n [your account name] -g [resource group name] --sku-instances [number of instances, e.g. 1] --sku-name [Pricing tier for example S1]
```

Var olan bir hesap kullanmak için aşağıdaki komutu kullanın:
```azurecli
az ml account modelmanagement set -n [your account name] -g [resource group it was created in]
```

### <a name="deploy-your-model"></a>Model dağıtma
Artık kaydedilmiş modelinizi bir web hizmeti olarak dağıtmaya hazır olursunuz. 

```azurecli
az ml service create realtime --model-file [model file/folder path] -f [scoring file e.g. score.py] -n [your service name] -s [schema file e.g. service_schema.json] -r [runtime for the Docker container e.g. spark-py or python] -c [conda dependencies file for additional python packages]
```

### <a name="next-steps"></a>Sonraki Adımlar
Galeri birçok örneklerinde birini deneyin.
