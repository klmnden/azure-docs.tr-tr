---
title: Yükleme ve en önemli görevler - Azure Machine Learning CLI kullanma
description: Azure Machine Learning görevlerinde öğrenme en yaygın makine için CLI yükleyip öğrenin.
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
ms.openlocfilehash: 33a1665c8f09efae88c831172199fca3e0b7634d
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="install-and-use-the-machine-learning-cli-for-top-tasks-in-azure-machine-learning"></a>Yükleme ve Azure Machine Learning üst görevler için CLI öğrenme makine kullanma

Azure Machine Learning services, tümleşik, uçtan uca veri bilimi ve Gelişmiş analiz çözümü. Profesyonel veri bilimcilerine Azure Machine Learning Hizmetleri verileri hazırlama, denemeler geliştirmek ve bulut ölçeğinde modelleri dağıtmak için kullanabilirsiniz. 

Azure Machine Learning hangi yapabilecekleriniz ile komut satırı arabirimi (CLI) sağlar:
+ Çalışma alanı ve projeleri yönetme
+ İşlem hedefleri ayarlayın
+ Eğitim denemeleri çalıştırın
+ Geçmişini görüntüleme ve geçmiş çalışmaları için ölçümleri
+ Web Hizmetleri olarak üretime modelleri dağıtma
+ Üretim modelleri ve hizmetlerini yönetebilir

Bu makale size kolaylık sağlamak için en kullanışlı CLI komutlarının bazıları gösterir. 

![Azure Machine Learning CLI](media/cli-for-azure-machine-learning/flow.png)

## <a name="what-you-need-to-get-started"></a>Başlamak için ihtiyacınız olanlar

Bir Azure aboneliği veya burada Modellerinizi dağıtabilmeniz için bir kaynak grubu için katkıda bulunan erişmeniz gerekir. Ayrıca, CLI çalıştırmak için Azure Machine Learning çalışma ekranı yüklemeniz gerekir. 

>[!IMPORTANT]
>Azure Machine Learning hizmetleriyle teslim CLI farklıdır [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest), hangi Azure kaynaklarını yönetmek için kullanılır.

## <a name="get-and-start-cli"></a>Alın ve CLI başlayın

Bu CLI almak için buradan yüklenebilir Azure Machine Learning çalışma yükleyin:
    + Windows - https://aka.ms/azureml-wb-msi 
    + MacOS- https://aka.ms/azureml-wb-dmg 

CLI başlatmak için:
+ Azure Machine Learning çalışma içinde menüsünde CLI başlatma **Dosya -> komut istemini açın.**

## <a name="get-command-help"></a>Komut için Yardım alın 

CLI komutları kullanarak hakkında ek bilgi alabileceğiniz `--debug` veya `--help` gibi komutları sonra `az ml <xyz> --debug` burada `<xyz>` komutu adı. Örneğin:
```azurecli
az ml computetarget --debug 

az ml experiment --help
```

## <a name="common-cli-tasks-for-azure-machine-learning"></a>Azure Machine Learning için ortak CLI görevleri 

Bu bölümde, CLI ile gerçekleştirebileceğiniz en yaygın görevleri hakkında bilgi de dahil olmak üzere:
+ [İşlem hedeflerini ayarlama](#target)
+ [Uzaktan yürütme işleri gönderme](#jobs)
+ [Jupyter not defterleri ile çalışma](#jupyter)
+ [Çalıştırma geçmişlerini ile etkileşim kurma](#history)
+ [Faaliyete ortamınızı yapılandırma](#o16n)

<a name="target"></a>

### <a name="set-up-a-compute-target"></a>Bir işlem hedefi ayarlama

Farklı hedefler dahil, modelde öğrenme makinenizi hesaplayabilirsiniz:
+ Yerel modda
+ bir veri bilimi VM (DSVM)
+ bir Hdınsight kümesine

Bir veri bilimi VM hedef eklemek için:
```azurecli
az ml computetarget attach remotedocker -n <target name> -a <ip address or FQDN> -u <username> -w <password>
``` 

Bir Hdınsight hedef eklemek için:
```azurecli
az ml computetarget attach cluster -n <target name> -a <cluster name, e.g. myhdicluster-ssh.azurehdinsight.net> -u <ssh username> -w <ssh password>
```

İçinde **aml_config** klasörünü conda bağımlılıkları değiştirebilirsiniz. 

Ayrıca, PySpark, Python veya Python GPU DSVM içinde çalışabilir. 

Python çalışma modu tanımlamak için:
+ Python için ekleme `Framework:Python` içinde `<target name>.runconfig` 

+ PySpark için ekleme `Framework:PySpark` içinde `<target name>.runconfig` 

+ GPU DSVM, Python için
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

### <a name="submit-remote-jobs"></a>Uzak işleri gönderme

Uzak hedefe işi göndermek için:
```azurecli
az ml experiment submit -c <target name> myscript.py
```

<a name="jupyter"></a>

### <a name="work-with-jupyter-notebooks"></a>Jupyter not defterleri ile çalışma

Jupyter not defteri başlatmak için:
```azurecli
az ml notebook start
```

Bu komut localhost Jupyter not defteri başlatır. Yerel Python 3 Çekirdek seçerek iş veya çekirdek seçerek uzak VM'nizi iş `<target name>`.

<a name="history"></a>

### <a name="interact-with-and-explore-the-run-history"></a>Etkileşim ve çalıştırma geçmişi keşfedin

Çalıştırma geçmişi listelemek için:
```azurecli
az ml history list -o table
```

Tüm tamamlanan çalıştırır listelemek için:
```azurecli
az ml history list --query "[?status=='Completed']" -o table
```

Bulmak için en iyi doğruluk ile çalışır:
```azurecli
az ml history list --query "@[?Accuracy != null] | max_by(@, &Accuracy).Accuracy"
```

Ayrıca, her çalıştırmayı tarafından oluşturulan dosyalar indirebilirsiniz. 

Klasör çıktılarında kaydedilmiş bir modeli yükseltmek için:
```azurecli
az ml history promote -r <run id> -ap outputs/model.pkl -n <model name>
```

Bu model karşıdan yüklemek için:
```azurecli
az ml asset download -l assets/model.pkl.link -d <model folder path>
```

<a name="o16n"></a>

### <a name="configure-your-environment-to-operationalize"></a>Ortamınızı faaliyete şekilde yapılandırma

Operationalization ortamınızı ayarlamak için oluşturmanız gerekir:

> [!div class="checklist"]
> * Bir kaynak grubu 
> * bir depolama hesabı
> * Bir Azure kapsayıcı kayıt defteri (ACR)
> * Bir uygulama Insight hesabı
> * Bir Azure kapsayıcı Hizmeti'ni (ACS) küme üzerinde Kubernetes dağıtımı


Docker kapsayıcısı içinde test etmek için yerel bir dağıtım ayarlamak için:
```azurecli
az ml env setup -l <region, e.g. eastus2> -n <env name> -g <resource group name>
```

Bir ACS küme Kubernetes ile ayarlamak için:
```azurecli
az ml env setup -l <region, e.g. eastus2> -n <env name> -g <resource group name> --cluster
```

Dağıtım durumunu izlemek için:
```azurecli
az ml env show -n <environment name> -g <resource group name>
```

Kullanılması gereken ortam ayarlamak için:
```azurecli
az ml env set -n <environment name> -g <resource group name>
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makaleler biri ile başlayın: 
+ [Yükleyin ve Azure Machine Learning kullanmaya başlama](quickstart-installation.md)
+ [Sınıflandırma Iris veri öğretici: Bölüm 1](tutorial-classifying-iris-part-1.md)

Bu makaleler biriyle derinliklerine:
+ [Modelleri yönetmek için CLI komutları](model-management-cli-reference.md)