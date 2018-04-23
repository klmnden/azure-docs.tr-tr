---
title: Azure Machine Learning önizleme özellikleri öğretici makalesi - Komut Satırı Arabirimi | Microsoft Docs
description: Bu öğreticide komut satırı arabiriminden uçtan uca bir Süsen sınıflandırmasını tamamlamak için gereken tüm adımlarda yol gösterilir.
services: machine-learning
author: ahgyger
ms.author: ahgyger
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: tutorial
ms.date: 10/15/2017
ms.openlocfilehash: 05238c27a5654ae24c619b52d769abbf90b940e7
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="tutorial-classifying-iris-using-the-command-line-interface"></a>Öğretici: Komut satırı arabirimini kullanarak Süsen Sınıflandırması
Azure Machine Learning hizmetleri (önizleme) uzman veri bilimcilerinin bulut ölçeğinde veri hazırlamasını, deney geliştirmesini ve model dağıtmasını sağlayan tümleşik, uçtan uca ve gelişmiş bir analiz çözümüdür.

Bu öğreticide, Azure Machine Learning önizleme özelliklerinde komut satırı arabirimini (CLI) kullanarak şunları yapmayı öğrenirsiniz: 
> [!div class="checklist"]
> * Deneme hesabını ayarlama ve çalışma alanı oluşturma
> * Proje oluşturma
> * Birden çok bilgisayar hedefine deneme gönderme
> * Eğitilen bir modeli yükseltme ve kaydetme
> * Yeni verileri puanlamak için bir web hizmeti dağıtma

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:
- Bir Azure aboneliğinde kaynakları oluşturmak için bu aboneliğe ve izinlerine erişin. 
  
  Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- Azure Machine Learning Workbench uygulaması, [Hızlı Başlangıç: Azure Machine Learning hizmetlerini yükleme ve başlatma](../service/quickstart-installation.md) altında açıklandığı gibi yüklenir. 

  >[!IMPORTANT]
  >Azure Machine Learning hizmet hesaplarını oluşturmayın çünkü bu işlemi bu makalede CLI kullanarak yapacaksınız.
 
## <a name="getting-started"></a>Başlarken
Azure Machine Learning komut satırı arabirimi (CLI) uçtan uca bir veri bilimi iş akışı için gereken tüm görevleri gerçekleştirmenize olanak tanır. CLI araçlarına şu yollarla erişebilirsiniz:

### <a name="option-1-launch-azure-ml-cli-from-azure-ml-workbench-log-in-dialog-box"></a>1. Seçenek Azure ML CLI'yi Azure ML Workbench oturum açma iletişim kutusundan başlatma
İlk kez Azure ML Workbench'i başlatıp oturum açtığınızda, henüz bir Deneme Hesabına erişiminiz yoksa aşağıdaki ekran gösterilir:

![hesap bulunamadı](media/tutorial-iris-azure-cli/no_account_found.png)

Komut satırı penceresini başlatmak için iletişim kutusunda **Komut Satırı Penceresi** bağlantısına tıklayın.

### <a name="option-2-launch-azure-ml-cli-from-azure-ml-workbench-app"></a>2. Seçenek Azure ML CLI'yi Azure ML Workbench uygulamasından başlatma
Zaten bir Deneme Hesabına erişiminiz varsa, başarılı bir şekilde oturum açabilirsiniz. Sonra da **Dosya** --> **Komut İstemini Aç** menüsüne tıklayarak komut satırı penceresini açabilirsiniz.

### <a name="option-3-enable-azure-ml-cli-in-an-arbitrary-command-line-window"></a>3. Seçenek Azure ML CLI'yi rastgele bir komut satırı penceresinde etkinleştirme
Azure ML CLI'yi herhangi bir komut satırı penceresinde de etkinleştirebilirsiniz. Bunu yapmak için bir komut penceresi başlatın ve aşağıdaki komutları girin:

```sh
# Windows Command Prompt
set PATH=%LOCALAPPDATA%\amlworkbench\Python;%LOCALAPPDATA%\amlworkbench\Python\Scripts;%PATH%

# Windows PowerShell
$env:Path = $env:LOCALAPPDATA+"\amlworkbench\Python;"+$env:LOCALAPPDATA+"\amlworkbench\Python\Scripts;"+$env:Path

# macOS Bash Shell
PATH=$HOME/Library/Caches/AmlWorkbench/Python/bin:$PATH
```
Değişikliği kalıcı hale getirmek için Windows'da `SETX` kullanabilirsiniz. macOS'ta ise `setenv` kullanabilirsiniz.

>[!TIP]
>Önündeki ortam değişkenlerini ayarlayarak Azure CLI'yi sık kullandığınız terminal penceresinde etkinleştirebilirsiniz.

## <a name="step-1-log-in-to-azure"></a>1. Adım Azure'da oturum açma
İlk adım AMLWorkbench Uygulamasından (Dosya > Komut İstemini Aç) CLI'yi açmaktır. Bunu yaparak doğru Python ortamını kullandığınızdan ve ML CLI komutlarının kullanılabildiğinden emin olursunuz. 

Şimdi, Azure kaynaklarına erişmek ve bunları yönetmek için CLI'nizde doğru bağlamı ayarlayabilirsiniz.
 
```azure-cli
# log in
$ az login

# list all subscriptions
$ az account list -o table

# set the current subscription
$ az account set -s <subscription id or name>
```

## <a name="step-2-create-a-new-azure-machine-learning-experimentation-account-and-workspace"></a>2. Adım Yeni bir Azure Machine Learning Denemesi Hesabı ve Çalışma Alanı oluşturma

Bu adımda, yeni bir Deneme hesabı ve yeni bir çalışma alanı oluşturursunuz. Deneme hesapları ve çalışma alanları hakkındaki diğer ayrıntılar için bkz. [Azure Machine Learning kavramları](overview-general-concepts.md).

> [!NOTE]
> Deneme hesaplarına, deneme çalıştırmalarınızın çıkışlarını depolamak için kullanılan bir depolama hesabı gerekir. Depolama hesabı adı Azure'da genel olarak benzersiz olmalıdır çünkü bu adla ilişkilendirilmiş bir url vardır. Mevcut bir depolama hesabı belirtmezseniz, deneme hesabı adınız kullanılarak yeni depolama hesabı oluşturulur. Benzersiz bir ad kullandığınızdan emin olun, yoksa _"\<depolama_hesabı_adı> adlı depolama hesabı zaten alınmış"_ gibi bir hata alırsınız. Alternatif olarak, mevcut bir depolama hesabı sağlamak için `--storage` bağımsız değişkenini kullanabilirsiniz.

```azure-cli
# create a resource group 
$ az group create --name <resource group name> --location <supported Azure region>

# create a new experimentation account with a new storage account
$ az ml account experimentation create --name <experimentation account name> --resource-group <resource group name>

# create a new experimentation account with an existing storage account
$ az ml account experimentation create --name <experimentation account name>  --resource-group <resource group name> --storage <storage account Azure Resource ID>

# create a workspace in the experimentation account
az ml workspace create --name <workspace name> --account <experimentation account name> --resource-group <resource group name>
```

## <a name="step-2a-optional-share-a-workspace-with-co-worker"></a>2.a Adımı (isteğe bağlı) Çalışma alanını iş arkadaşıyla paylaşma
Burada, çalışma alanı erişiminin bir iş arkadaşıyla nasıl paylaşılacağını keşfedebilirsiniz. Deneme hesabına veya projeye erişimi paylaşma adımları aynı olabilir. Yalnızca Azure Kaynak Kimliğini alma yönteminin güncelleştirilmesi gerekebilir.

```azure-cli
# find the workspace Azure Resource ID
$az ml workspace show --name <workspace name> --account <experimentation account name> --resource-group <resource group name>

# add Bob to this workspace as a new owner
$az role assignment create --assignee bob@contoso.com --role owner --scope <workspace Azure Resource ID>
```

> [!TIP]
> Yukarıdaki komutta kullanılan `bob@contoso.com`, geçerli aboneliğin ait olduğu dizinde geçerli bir Azure AD kimliği olmalıdır.

## <a name="step-3-create-a-new-project"></a>3. Adım Yeni bir proje oluşturma
Sonraki adımımız yeni proje oluşturmaktır. Yeni projeye başlamanın birkaç yolu vardır.

### <a name="create-a-new-blank-project"></a>Yeni boş bir proje oluşturma

```azure-cli
# create a new project
$ az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path>
```

### <a name="create-a-new-project-with-a-default-project-template"></a>Varsayılan proje şablonuyla yeni proje oluşturma
Varsayılan şablonla yeni bir proje oluşturabilirsiniz.

```azure-cli
$ az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path> --template
```

### <a name="create-a-new-project-associated-with-a-cloud-git-repository"></a>Bulut Git deposuyla ilişkilendirilmiş yeni proje oluşturma
VSTS (Visual Studio Team Service) Git deposuyla ilişkilendirilmiş yeni bir proje oluşturabilirsiniz. Deneme her gönderildiğinde, tüm proje klasörünün anlık görüntüsü uzak Git deposuna işlenir. Diğer ayrıntılar için bkz. [Azure Machine Learning Workbench projesiyle Git deposunu kullanma](using-git-ml-project.md).

> [!NOTE]
> Azure Machine Learning yalnızca VSTS'de oluşturulmuş boş Git depolarını destekler.

```azure-cli
$ az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path> --repo <VSTS repo URL>
```
> [!TIP]
> "Depo url'si geçersiz olabilir veya kullanıcının erişimi olmayabilir" hatasını alıyorsanız, VSTS'de bir güvenlik belirteci oluşturabilir (_Güvenlik_ altında, _Kişisel erişim belirteçleri ekle_ menüsü) ve projenizi oluştururken `--vststoken` bağımsız değişkenini kullanabilirsiniz. 

### <a name="sample_create"></a>Örnekten yeni proje oluşturma
Bu örnekte, şablon olarak bir örnek proje kullanıp yeni proje oluşturursunuz.

```azure-cli
# List the project samples, find the Classifying Iris sample
$ az ml project sample list

# Create a new project from the sample
az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path> --template-url https://github.com/MicrosoftDocs/MachineLearningSamples-Iris
```
Projeniz oluşturulduktan sonra, `cd` komutunu kullanarak proje dizinine girin.

## <a name="step-4-run-the-training-experiment"></a>4. Adım Eğitim denemesini çalıştırma 
Aşağıdaki adımlarda Iris örneğiyle bir projeniz olduğu varsayılır (bkz. [Çevrimiçi örnekten yeni proje oluşturma](#sample_create)).

### <a name="prepare-your-environment"></a>Ortamınızı hazırlama 
Iris örneği için, matplotlib'i yüklemelisiniz.

```azure-cli
$ pip install matplotlib
```

###  <a name="submit-the-experiment"></a>Denemeyi gönderme

```azure-cli
# Execute the file
$ az ml experiment submit --run-configuration local iris_sklearn.py
```

### <a name="iterate-on-your-experiment-with-descending-regularization-rates"></a>Azalan düzenleme hızlarıyla denemenizi yineleme
Biraz yaratıcılıkla, denemeleri farklı düzenleme hızlarıyla gönderen Python betiğini bir araya getirmek basit bir işlemdir. (Dosyayı doğru proje yoluna işaret edecek şekilde düzenlemeniz gerekebilir.)

```azure-cli
$ python run.py
```

## <a name="step-5-view-run-history"></a>5. Adım. Çalıştırma geçmişini görüntüleme
Aşağıdaki komut yürütülen tüm önceki çalıştırmaları listeler. 

```azure-cli
$ az ml history list -o table
```
Önceki komut çalıştırıldığında, bu projeye ait tüm çalıştırmalar listelenir. Doğruluk ve düzenleme hızı ölçümlerinin de listelendiğini görebilirsiniz. Bu, listeden en iyi çalıştırmanın belirlenmesini kolaylaştırır.

## <a name="step-5a-view-attachment-created-by-a-given-run"></a>5.a Adımı Belirli bir çalıştırma tarafından oluşturulan eki görüntüleme 
Belirli bir çalıştırmayla ilişkilendirilmiş eki görüntülemek için, çalıştırma geçmişinin info komutunu kullanabilirsiniz. Önceki listede belirli bir çalıştırmanın çalıştırma kimliğini bulun.

```azure-cli
$ az ml history info --run <run id> --artifact driver_log
```

Çalıştırmadan yapıtları indirmek için aşağıdaki komutu kullanabilirsiniz:

```azure-cli
# Stream a given attachment 
$ az ml history info --run <run id> --artifact <artifact location>
```

## <a name="step-6-promote-artifacts-of-a-run"></a>6. Adım. Çalıştırmanın yapıtlarını yükseltme 
Çalıştırmalardan birinin AUC'si daha iyidir, dolayısıyla üretime dağıtmak üzere bir puanlama web hizmeti oluştururken bu çalıştırma kullanılır. Bunu yapmak için, önce yapıtları bir varlığa yükseltmelisiniz.

```azure-cli
$ az ml history promote --run <run id> --artifact-path outputs/model.pkl --name model.pkl
```

Bu işlem, proje dizininizde `model.pkl.link` dosyasını içeren bir `assets` klasörü oluşturur. Bu bağlantı dosyası yükseltilen varlığa başvurmak için kullanılır.

## <a name="step-7-download-the-files-to-be-operationalized"></a>7. Adım. Kullanıma hazır hale getirilecek dosyaları indirme
Yükseltilen modeli indirin. Böylelikle bir tahmin web hizmeti oluştururken bu modeli kullanabilirsiniz. 

```azure-cli
$ az ml asset download --link-file assets\pickle.link -d asset_download
```

## <a name="step-8-set-up-your-model-management-environment"></a>8. Adım Model yönetim ortamınızı ayarlama 
Web hizmetlerini dağıtmak için bir ortam oluşturun. Docker kullanarak web hizmetini yerel makinede çalıştırabilirsiniz. İsterseniz, büyük ölçekli işlemler için bunu bir ACS kümesine de dağıtabilirsiniz. 

```azure-cli
# Create new local operationalization environment
$ az ml env setup -l <supported Azure region> -n <env name>
# Once setup is complete, set your environment for current context
$ az ml env set -g <resource group name> -n <env name>
```

## <a name="step-9-create-a-model-management-account"></a>9. Adım Model yönetimi hesabı oluşturma 
Model yönetimi hesabı, üretimde modellerinizi dağıtmak ve izlemek için gereklidir. 

```azure-cli
$ az ml account modelmanagement create -n <model management account name> -g <resource group name> -l <supported Azure region>
```

## <a name="step-10-create-a-web-service"></a>10. Adım Web hizmeti oluşturma
Dağıttığınız modeli kullanarak tahmin döndüren bir web hizmeti oluşturun. 

```azure-cli
$ az ml service create realtime -m asset_download/model.pkl -f score_iris.py -r python –n <web service name>
```

## <a name="step-11-run-the-web-service"></a>11. Adım Web hizmetini çalıştırma
Önceki adımın çıkışındaki web hizmeti kimliğini kullanarak web hizmetini çağırın ve test edin. 

```azure-cli
# Get web service usage infomration
$ az ml service usage realtime -i <web service id>

# Call the web service with the run command:
$ az ml service run realtime -i <web service id> -d <input data>
```

## <a name="step-12-deleting-all-the-resources"></a>12. Adım Tüm kaynakları silme 
Şimdi, oluşturulmuş olan ve üzerinde çalışmak istemediğiniz tüm kaynakları silerek bu öğreticiyi tamamlayalım. 

Bunu yapmak için, kaynakları barındıran kaynak grubunu silin. 

```azure-cli
az group delete --name <resource group name>
```

## <a name="next-steps"></a>Sonraki Adımlar
Bu öğreticide, Azure Machine Learning'i kullanarak şunları yapmayı öğrendiniz: 
> [!div class="checklist"]
> * Deneme hesabını ayarlama, çalışma alanını oluşturma
> * Projeleri oluşturma
> * Birden çok bilgisayar hedefine denemeleri gönderme
> * Eğitilen bir modeli yükseltme ve kaydetme
> * Model yönetimi için bir model yönetimi hesabı oluşturma
> * Web hizmetlerini dağıtmak için bir ortam oluşturma
> * Web hizmetini dağıtma ve yeni verilerle puanlama yapma
