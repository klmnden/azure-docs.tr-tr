---
title: Yükleme ve üst görevler - Azure Machine Learning CLI'yi kullanma
description: Azure Machine Learning görevlerinde öğrenme en yaygın makine için CLI'yı yükleyip kullanmayı öğrenin.
services: machine-learning
author: haining
ms.author: haining
manager: cgronlun
ms.reviewer: mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/10/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 06e85845d41b240638a5b5b4d75d64fd460a99bf
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46953336"
---
# <a name="install-and-use-the-machine-learning-cli-for-top-tasks-in-azure-machine-learning"></a>Yükleme ve Azure Machine Learning, üst görev için CLI öğrenme machine kullanma

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

Azure Machine Learning hizmeti, tümleşik, uçtan uca veri bilimi ve Gelişmiş analiz çözümüdür. Uzman veri bilimcilerinin veri hazırlamasını, deney geliştirmesini ve bulut ölçeğinde modeller dağıtmak için Azure Machine Learning hizmetini kullanabilirsiniz. 

Azure Machine Learning komut satırı arabirimi (CLI) olan yapabilecekleriniz ile sağlar:
+ Çalışma alanı ve projelerinizi yönetme
+ İşlem hedefleri ayarlayın
+ Eğitim denemeleri çalıştırmak
+ Geçmişi Görüntüle ve son çalıştırmalar için ölçümleri
+ Modellerinizi web Hizmetleri olarak üretime dağıtın
+ Üretim modelleri ve hizmetleri yönetmek

Bu makale size kolaylık sağlamak için en kullanışlı CLI komutları bazılarını gösterir. 

![Azure Machine Learning CLI](media/cli-for-azure-machine-learning/flow.png)

## <a name="what-you-need-to-get-started"></a>Başlamak için ihtiyacınız olanlar

Bir Azure aboneliği veya Modellerinizi dağıtabileceğiniz bir kaynak grubu için katkıda bulunan erişmeniz gerekir. Ayrıca, Azure Machine Learning Workbench CLI'yı çalıştırmak için yüklemeniz gerekir. 

>[!IMPORTANT]
>CLI ile Azure Machine Learning hizmeti farklı teslim [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest), Azure kaynaklarını yönetmek için kullanılır.

## <a name="get-and-start-cli"></a>Alın ve CLI başlayın

Bu CLI almak için buradan indirilebilir Azure Machine Learning Workbench'i yükleyin:
    + Windows - https://aka.ms/azureml-wb-msi 
    + MacOS- https://aka.ms/azureml-wb-dmg 

CLI'yı başlatmak için:
+ Azure Machine Learning Workbench'te Başlat menüsünden CLI **Dosya -> komut istemini açın.**

## <a name="get-command-help"></a>Komut Yardımı alın 

CLI komutlarını kullanma hakkında ek bilgi alabileceğiniz `--debug` veya `--help` gibi komutlar sonra `az ml <xyz> --debug` burada `<xyz>` komut adı. Örneğin:
```azurecli
az ml computetarget --debug 

az ml experiment --help
```

## <a name="common-cli-tasks-for-azure-machine-learning"></a>Azure Machine Learning için ortak CLI görevleri 

Bu bölümde, CLI ile gerçekleştirebileceğiniz en yaygın görevler hakkında bilgi dahil olmak üzere:
+ [İşlem hedeflerini ayarlama](#target)
+ [Uzaktan yürütme için iş gönderme](#jobs)
+ [Jupyter not defterleri ile çalışma](#jupyter)
+ [Çalıştırma geçmişleri ile etkileşim kurma](#history)
+ [Kullanıma hazır hale getirmek için ortamınızı yapılandırma](#o16n)

<a name="target"></a>

### <a name="set-up-a-compute-target"></a>Bir işlem hedefine ayarlayın

Makine öğrenimi modeli de dahil olmak üzere farklı hedefler hesaplayabilirsiniz:
+ Yerel modda
+ bir veri bilimi sanal makinesi (DSVM) içinde
+ bir HDInsight kümesi üzerinde

Bir veri bilimi sanal makinesi hedefi eklemek için:
```azurecli
az ml computetarget attach remotedocker -n <target name> -a <ip address or FQDN> -u <username> -w <password>
``` 

Bir HDInsight hedefi eklemek için:
```azurecli
az ml computetarget attach cluster -n <target name> -a <cluster name, e.g. myhdicluster-ssh.azurehdinsight.net> -u <ssh username> -w <ssh password>
```

İçinde **aml_config** klasöründe conda bağımlılıklarını değiştirebilirsiniz. 

Ayrıca, PySpark, Python veya Python'da bir GPU DSVM ile çalışabilir. 

Python çalışma modu tanımlamak için:
+ Python için ekleme `Framework:Python` içinde `<target name>.runconfig` 

+ PySpark için ekleme `Framework:PySpark` içinde `<target name>.runconfig` 

+ GPU DSVM Python için
    1. Ekleme `Framework:Python` içinde `<target name>.runconfig` 

    1. Ayrıca, ekleme `baseDockerImage: microsoft/mmlspark:plus-gpu-0.9.9 and nvidiaDocker:true` içinde `<target name>.compute`

İşlem hedef hazırlamak için:
```azurecli
az ml experiment prepare -c <target name>
```

>[!TIP]
>Aboneliğinizi göstermek için:<br/>
>`az account show`<br/>
>
>Aboneliğinizi ayarlamak için:<br/>
>`az account set –s "my subscription name" `

<a name="jobs"></a>

### <a name="submit-remote-jobs"></a>Uzak işlerini gönderme

Bir uzak hedefi için bir iş göndermek için:
```azurecli
az ml experiment submit -c <target name> myscript.py
```

<a name="jupyter"></a>

### <a name="work-with-jupyter-notebooks"></a>Jupyter not defterleri ile çalışma

Jupyter not defteri başlatmak için:
```azurecli
az ml notebook start
```

Bu komut localhost Jupyter not defteri başlatır. Yerel Python 3 Çekirdek seçerek çalışma veya uzaktan, sanal çekirdek seçerek çalışma `<target name>`.

<a name="history"></a>

### <a name="interact-with-and-explore-the-run-history"></a>Etkileşim kurma ve çalıştırma geçmişini keşfedin

Çalıştırma geçmişini listelemek için:
```azurecli
az ml history list -o table
```

Tüm tamamlanan çalıştırmalar listelemek için:
```azurecli
az ml history list --query "[?status=='Completed']" -o table
```

Bulunacak en yüksek doğruluk oranıyla çalıştırır:
```azurecli
az ml history list --query "@[?Accuracy != null] | max_by(@, &Accuracy).Accuracy"
```

Her çalıştırma tarafından oluşturulan dosyaların da indirebilirsiniz. 

Klasörüne yapılandırma çıkışları kaydedilmiş bir modeli yükseltmek için:
```azurecli
az ml history promote -r <run id> -ap outputs/model.pkl -n <model name>
```

Bu model indirmek için:
```azurecli
az ml asset download -l assets/model.pkl.link -d <model folder path>
```

<a name="o16n"></a>

### <a name="configure-your-environment-to-operationalize"></a>Kullanıma hazır hale getirmek için ortamınızı yapılandırma

Kullanıma hazır hale getirme ortamınızı ayarlamak için oluşturmanız gerekir:

> [!div class="checklist"]
> * Bir kaynak grubu 
> * Bir depolama hesabı
> * Azure Container Registry (ACR)
> * Bir Application Insight hesap
> * Azure Container Service (ACS) kümesinde Kubernetes dağıtımı


Bir Docker kapsayıcısında test etmek için yerel bir dağıtımı ayarlamak için:
```azurecli
az ml env setup -l <region, e.g. eastus2> -n <env name> -g <resource group name>
```

Kubernetes ile ACS kümesini ayarlamak için:
```azurecli
az ml env setup -l <region, e.g. eastus2> -n <env name> -g <resource group name> --cluster
```

Dağıtım durumunu izlemek için:
```azurecli
az ml env show -n <environment name> -g <resource group name>
```

Kullanılması gereken ortamı için:
```azurecli
az ml env set -n <environment name> -g <resource group name>
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalelerin biri ile başlayın: 
+ [Yükleme ve Azure Machine Learning'i kullanmaya başlayın](quickstart-installation.md)
+ [Sınıflandırma Iris veri öğretici: Bölüm 1](tutorial-classifying-iris-part-1.md)

Şu makalelerden birini daha derinlemesine inceleyelim:
+ [Model Yönetimi CLI komutları](model-management-cli-reference.md)
