---
title: Machine learning CLI uzantısı
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning CLI uzantı hakkında Azure CLI için öğrenin. Azure CLI'yı Azure buluttaki kaynaklar ile çalışmanıza olanak sağlayan bir platformlar arası komut satırı yardımcı programıdır. Machine Learning uzantısı, Azure Machine Learning hizmeti ile çalışmanıza olanak sağlar.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: jordane
author: jpe316
ms.date: 04/30/2019
ms.custom: seodec18
ms.openlocfilehash: 2992ec9f43aac9e0d80c5e42873d26ac3a9c3fd1
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64916987"
---
# <a name="use-the-cli-extension-for-azure-machine-learning-service"></a>Azure Machine Learning hizmeti için CLI uzantısını kullanma

Azure Machine Learning CLI uzantısıdır [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest), Azure platformu için platformlar arası komut satırı arabirimi. Bu uzantı, komut satırından Azure Machine Learning hizmeti ile çalışmaya yönelik komutları sağlar. Bu, makine öğrenimi iş akışlarını otomatik hale getirmenizi sağlar. Örneğin, aşağıdaki eylemleri gerçekleştirebilirsiniz:

+ Makine öğrenimi modelleri oluşturma çalıştırın

+ Makine öğrenimi modelleri müşteri kullanım için kaydolun

+ Paketlemeyi, dağıtmayı ve makine öğrenimi Modellerinizi ömrünü izleme

CLI, Azure Machine Learning SDK'sı yerine değil. Yüksek oranda parametreli kendileri de otomasyon için uygun görevleri işlemek için optimize edilmiştir tamamlayıcı bir araçtır.

## <a name="prerequisites"></a>Önkoşullar

* CLI kullanmak için Azure aboneliğiniz olmalıdır. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree) bugün.

* [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

## <a name="install-the-extension"></a>Uzantıyı yükleme

Machine Learning CLI uzantısını yüklemek için aşağıdaki komutu kullanın:

```azurecli-interactive
az extension add -n azure-cli-ml
```

> [!TIP]
> Aşağıdaki komutları ile kullanabileceğiniz örnek dosyaları bulunabilen [burada](http://aka.ms/azml-deploy-cloud).

Sorulduğunda, `y` uzantıyı yüklemek için.

Uzantı yüklü olduğunu doğrulamak için ML özgü alt komutları listesini görüntülemek için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml -h
```

## <a name="remove-the-extension"></a>Uzantıyı kaldırma

CLI uzantısını kaldırmak için aşağıdaki komutu kullanın:

```azurecli-interactive
az extension remove -n azure-cli-ml
```

## <a name="resource-management"></a>Kaynak yönetimi

Aşağıdaki komutları, Azure Machine Learning tarafından kullanılan kaynakları yönetmek için CLI'yı kullanmayı göstermektedir.

+ Zaten bir yoksa, bir kaynak grubu oluşturun:

    ```azurecli-interactive
    az group create -n myresourcegroup -l westus2
    ```

+ Bir Azure Machine Learning hizmeti çalışma alanında oluşturun:

    ```azurecli-interactive
    az ml workspace create -w myworkspace -g myresourcegroup
    ```

+ Bir çalışma alanı yapılandırması CLI bağlamsal tanıma etkinleştirmek için bir klasöre bağlayın.

    ```azurecli-interactive
    az ml folder attach -w myworkspace -g myresourcegroup
    ```

+ Azure blob kapsayıcısı, bir veri deposu olarak ekleyin.

    ```azurecli-interactive
    az ml datastore attach-blob  -n datastorename -a accountname -c containername
    ```

+ Bir AKS kümesi bir işlem hedefi olarak ekleyin.

    ```azurecli-interactive
    az ml computetarget attach aks -n myaks -i myaksresourceid -g myrg -w myworkspace
    ```

+ Yeni bir AMLcompute hedefi oluşturma

    ```azurecli-interactive
    az ml computetarget create amlcompute -n cpu --min-nodes 1 --max-nodes 1 -s STANDARD_D3_V2
    ```
    
## <a id="experiments"></a>Çalıştırma denemeleri

+ Bir çalışma alanı yapılandırması CLI bağlamsal tanıma etkinleştirmek için bir klasöre bağlayın.

    ```azurecli-interactive
    az ml folder attach -w myworkspace -g myresourcegroup
    ```

* Çalıştırma denemenizi başlatın. Bu komutu kullanırken runconfig dosyasının adını belirtin (metinden önce \*dosya sisteminizin arıyorsanız .runconfig) karşı - c parametresi.

    ```azurecli-interactive
    az ml run submit-script -c local -e testexperiment train.py
    ```

* Denemeleri listesini görüntüleyin:

    ```azurecli-interactive
    az ml experiment list
    ```

## <a name="model-registration-profiling-deployment"></a>Model kaydı, profil oluşturma, dağıtım

Aşağıdaki komutlar, eğitilen bir modeli kaydedin ve ardından bunu bir üretim hizmeti dağıtacağız göstermektedir:

+ Bir modeli Azure Machine Learning ile kaydedin:

  ```azurecli-interactive
  az ml model register -n mymodel -p sklearn_regression_model.pkl
  ```

+ Modelinizi AKS'ye dağıtma

  ```azurecli-interactive
  az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json --dc deploymentconfig.json
  ```
