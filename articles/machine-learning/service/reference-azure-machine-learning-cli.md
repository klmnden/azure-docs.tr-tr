---
title: Machine learning CLI uzantısını kullanma
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning CLI uzantı hakkında Azure CLI için öğrenin. Azure CLI'yı Azure buluttaki kaynaklar ile çalışmanıza olanak sağlayan bir platformlar arası komut satırı yardımcı programıdır. Machine Learning uzantısı, Azure Machine Learning hizmeti ile çalışmanıza olanak sağlar.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: jordane
author: jpe316
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: e1e94c2301cdbacf2ade037fe04cc8359ed06598
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53078200"
---
# <a name="use-the-cli-extension-for-azure-machine-learning-service"></a>Azure Machine Learning hizmeti için CLI uzantısını kullanma

Azure Machine Learning CLI uzantısıdır [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest), Azure platformu için platformlar arası komut satırı arabirimi. Bu uzantı, komut satırından Azure Machine Learning hizmeti ile çalışmaya yönelik komutları sağlar. Makine öğrenimi iş akışlarını otomatikleştiren betikler oluşturmak sağlar. Örneğin, aşağıdaki eylemleri gerçekleştirin betikleri oluşturabilirsiniz:

+ Makine öğrenimi modelleri oluşturma çalıştırın

+ Makine öğrenimi modelleri müşteri kullanım için kaydolun

+ Paketlemeyi, dağıtmayı ve makine öğrenimi Modellerinizi ömrünü izleme

CLI, Azure Machine Learning SDK'sı yerine değil. Yüksek oranda parametreli görevler gibi işlemek için optimize edilmiştir tamamlayıcı bir araçtır:

* İşlem kaynakları oluşturma

* Parametreli deneme gönderme

* Model kaydı

* Görüntü oluşturma

* Hizmet dağıtımı

## <a name="prerequisites"></a>Önkoşullar

* [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

> [!NOTE]
> CLI kullanmak için Azure aboneliğiniz olmalıdır. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://aka.ms/AMLfree) oluşturun.

## <a name="install-the-extension"></a>Uzantıyı yükleme

Machine Learning CLI uzantısını yüklemek için aşağıdaki komutu kullanın:

```azurecli-interactive
az extension add -s https://azuremlsdktestpypi.blob.core.windows.net/wheels/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1/azure_cli_ml-1.0.2-py2.py3-none-any.whl --pip-extra-index-urls  https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
```

Sorulduğunda, `y` uzantıyı yüklemek için.

Uzantı yüklü olduğunu doğrulamak için ML özgü alt komutları listesini görüntülemek için aşağıdaki komutu kullanın:

```azurecli-interactive
az ml -h
```

> [!TIP]
> Yapmanız gerekenler uzantıyı güncelleştirmek için __kaldırmak__ ve ardından __yüklemek__ bu. Bu, en son sürümünü yükler.

## <a name="remove-the-extension"></a>Uzantıyı kaldırın

CLI uzantısını kaldırmak için aşağıdaki komutu kullanın:

```azurecli-interactive
az extension remove -n azure-cli-ml
```

## <a name="resource-management"></a>Kaynak yönetimi

Aşağıdaki komutları, Azure Machine Learning tarafından kullanılan kaynakları yönetmek için CLI'yı kullanmayı göstermektedir.


+ Bir Azure Machine Learning hizmeti çalışma alanında oluşturun:

    ```azurecli-interactive
    az ml workspace create -n myworkspace -g myresourcegroup
    ```

+ Varsayılan çalışma alanı ayarlayın:

    ```azurecli-interactive
    az configure --defaults aml_workspace=myworkspace group=myresourcegroup
    ```

+ Eğitim dağıtılmış yönetilen işlem hedefi oluşturmak:

    ```azurecli-interactive
    az ml computetarget create amlcompute -n mycompute --max_nodes 4 --size Standard_NC6
    ```

* Hedef yönetilen bilgi işlem güncelleştirin:

    ```azurecli-interactive
    az ml computetarget update --name mycompute --workspace –-group --max_nodes 4 --min_nodes 2 --idle_time 300
    ```

* Eğitim veya dağıtım için bir yönetilmeyen işlem hedefine ekleyin:

    ```azurecli-interactive
    az ml computetarget attach aks -n myaks -i myaksresourceid -g myrg -w myworkspace
    ```

## <a name="experiments"></a>Denemeler

Aşağıdaki komutları denemeleri ile çalışmak için CLI kullanma işlemini gösterir:

* Bir deney göndermeden önce (yapılandırmayı Çalıştır) bir proje ekleyin:

    ```azurecli-interactive
    az ml project attach --experiment-name myhistory
    ```

* Çalıştırma denemenizi başlatın. Bu komutu kullanırken bir işlem hedefini belirtin. Bu örnekte, `local` kullanarak modeli eğitmek için yerel bilgisayarı `train.py` betiği:

    ```azurecli-interactive
    az ml run submit -c local train.py
    ```

* Gönderilen denemeleri listesini görüntüleyin:

    ```azurecli-interactive
    az ml history list
    ```

## <a name="model-registration-image-creation--deployment"></a>Model kaydı, görüntü oluşturma ve dağıtım

Aşağıdaki komutlar, eğitilen bir modeli kaydedin ve ardından bunu bir üretim hizmeti dağıtacağız göstermektedir:

+ Bir modeli Azure Machine Learning ile kaydedin:

  ```azurecli-interactive
  az ml model register -n mymodel -m sklearn_regression_model.pkl
  ```

+ Makine öğrenimi modeli ve bağımlılıklar içeren bir görüntü oluşturun: 

  ```azurecli-interactive
  az ml image create container -n myimage -r python -m mymodel:1 -f score.py -c myenv.yml
  ```

+ Bir resim bir işlem hedefine dağıtın:

  ```azurecli-interactive
  az ml service create aci -n myaciservice --image-id myimage:1
  ```
