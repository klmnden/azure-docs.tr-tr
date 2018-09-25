---
title: Azure Machine Learning CLI uzantı hakkında
description: Azure Machine Learning için CLI uzantısını öğrenme makine hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: reference
ms.reviewer: jmartens
ms.author: jordane
author: jpe316
ms.date: 09/24/2018
ms.openlocfilehash: 7e430d1b590413f497c851b687abcaa98e04d0e4
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47053870"
---
# <a name="what-is-the-azure-machine-learning-cli"></a>Azure Machine Learning CLI nedir?

Veri bilimcileri ve Azure Machine Learning hizmeti ile çalışan geliştiriciler için Azure Machine Learning komut satırı arabirimi (CLI) uzantısıdır. Hızlı bir şekilde machine learning iş akışlarını otomatikleştirin ve bunları gibi üretim ortamında yararlanmanın olanak tanır:
+ Makine öğrenimi modelleri oluşturma çalıştırın

+ Makine öğrenimi modelleri müşteri kullanım için kaydolun

+ Paketleme, dağıtma ve makine öğrenimi Modellerinizi ömrünü izleme

Bu makine öğrenimi CLI uzantısıdır [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) ve Python tabanlı üzerinde oluşturulmuş <a href="http://aka.ms/aml-sdk" target="_blank">SDK</a> Azure Machine Learning hizmeti için.

> [!NOTE]
> CLI, şu anda erken Önizleme aşamasındadır ve güncelleştirilir.

## <a name="installing-and-uninstalling"></a>Yükleme ve kaldırma

Bu komuttan önizlememiz Pypı dizin kullanarak CLI'yi yükleyebilirsiniz:
```AzureCLI
az extension add -s https://azuremlsdktestpypi.blob.core.windows.net/wheels/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1/azure_cli_ml-0.1.50-py2.py3-none-any.whl --pip-extra-index-urls  https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
```

Bu komutu kullanarak CLI'yi kaldırabilirsiniz:
```AzureCLI
az extension remove -n azure-cli-ml
```

CLI kullanarak güncelleştirebilirsiniz **Kaldır** ve **ekleme** yukarıdaki adımları.

## <a name="using-the-cli-vs-the-sdk"></a>SDK'sı ve CLI'yı kullanma
CLI'yı daha iyi Otomasyon için bir geliştirme işlemleri kişi veya sürekli tümleştirme ve teslim ardışık bir parçası olarak uygundur. Sık erişilmeyen ve yüksek oranda parametreli görevlerini işlemek için optimize edilmiştir. 

Örneklere şunlar dahildir:
- sağlama işlem
- Parametreli deneme gönderme
- Model kaydı, görüntü oluşturma
- Hizmet dağıtımı

Azure ML SDK'sını veri uzmanlarının önerilir.

## <a name="common-machine-learning-cli-commands"></a>Yaygın machine learning CLI komutları

Zengin kullanın `az ml` portalı Azure cloud Shell'i dahil olmak üzere herhangi bir komut satırı ortamı hizmetinde etkileşim kurmak için komutları.

Sık kullanılan komutlar bir örneği aşağıda verilmiştir:

### <a name="workspace-creation--compute-setup"></a>Çalışma alanı oluşturma & işlem Kurulumu

+ Bir Azure Machine Learning çalışma alanı, machine learning için en üst düzey kaynağı oluşturun.
   ```AzureCLI
   az ml workspace create -n myworkspace -g myresourcegroup
   ```

+ Bu çalışma alanı varsayılan olarak kullanmak için CLI'yı ayarlayın.
   ```AzureCLI
   az configure --defaults aml_workspace=myworkspace group=myresourcegroup
   ```

+ Bir DSVM (veri bilimi sanal makinesi) için eğitim modeller oluşturun. Dağıtılmış eğitim BatchAI kümeleri de oluşturabilirsiniz.
  ```AzureCLI
  az ml computetarget setup dsvm -n mydsvm
  ```

### <a name="experiment-submission"></a>Denemeyi gönderme
+ Bir denemeyi göndermek için bir proje (çalıştırma yapılandırması) ekleyin. Bu sayede deneme çalıştırmalarınızın izlemek için kullanılır.
  ```AzureCLI
  az ml project attach --experiment-name myhistory
  ```

+ Seçtiğiniz işlem hedef Azure Machine Learning hizmetinde bir deney olarak gönderin. Bu örnek, yerel işlem ortamınızda karşı yürütür. Bir örnek train.py betiği bulabilirsiniz [burada](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local/train.py).

  ```AzureCLI
  az ml run submit -c local train.py
  ```

+ Gönderilen denemeleri listesini görüntüleyin.
```AzureCLI
az ml history list
```

### <a name="model-registration-image-ceation--deployment"></a>Model kaydı, görüntü ceation ve dağıtım

+ Bir modeli Azure Machine Learning ile kaydedin.
  ```AzureCLI
  az ml model register -n mymodel -m mymodel.pkl  -w myworkspace -g myresourcegroup
  ```

+ Görüntü, makine öğrenimi modeli ve bağımlılıkları içerecek şekilde oluşturun. 
  ```AzureCLI
  az ml image create -n myimage -r python -m mymodel.pkl -f score.py -c myenv.yml
  ```

+ Paketlenmiş modelinizi ACI ve AKS dahil olmak üzere hedeflere dağıtın.
  ```AzureCLI
  az ml service create aci -n myaciservice -i myimage:1
  ```
    
## <a name="full-command-list"></a>Tam komut listesi
Çalıştırarak CLI uzantısını (ve bunların desteklenen parametreler) komutların tam listesini bulabilirsiniz ```az ml COMMANDNAME -h```. 
