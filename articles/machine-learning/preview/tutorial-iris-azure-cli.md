---
title: "Azure Machine Learning Önizleme özellikleri - komut satırı arabirimi için öğretici makale | Microsoft Docs"
description: "Bu öğretici ilerlemesi tüm adımlar bir Iris sınıflandırma uçtan uca komut satırı arabiriminden tamamlamak için gerekli."
services: machine-learning
author: ahgyger
ms.author: ahgyger, ritbhat
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 10/15/2017
ms.openlocfilehash: 453c774c97b77dd7829a50fa5e5668d06f817a1d
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2017
---
# <a name="tutorial-classifying-iris-using-the-command-line-interface"></a>Öğretici: komut satırı arabirimi kullanarak Iris sınıflandırma
Azure Machine Learning hizmetleri (önizleme) uzman veri bilimcilerinin bulut ölçeğinde veri hazırlamasını, deney geliştirmesini ve model dağıtmasını sağlayan tümleşik, uçtan uca ve genişmiş analiz çözümüdür.

Bu öğreticide, Azure Machine Learning Önizleme özellikleri için komut satırı arabirimi (CLI) araçlarını kullanmayı öğrenin: 
> [!div class="checklist"]
> * Bir deneme hesabı ayarlamanız ve bir çalışma alanı oluşturma
> * Proje oluşturma
> * Birden çok işlem hedefi için bir deneme gönderme
> * Yükseltme ve eğitilen model kaydetme
> * Yeni veri Puanlama amacıyla bir web hizmetini dağıtma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar
- Bir Azure aboneliği ve kaynaklar bu abonelikte oluşturma izni erişmeniz gerekir. 
- İzleyerek Azure makine Learing çalışma ekranı uygulamayı yüklemek gereken [yükleme ve hızlı başlangıç oluşturma](quickstart-installation.md). 

  >[!NOTE]
  >Yalnızca Azure Machine Learning çalışma ekranı yerel olarak yüklemeniz gerekir. Hesap oluşturulduktan sonra Azure Machine Learning çalışma ekranı yüklemek, başlıklı bölümlerdeki adımları izleyin ve adımları bu makalede komut satırı tarafından yapılan yeni bir proje oluşturun yeterlidir.
 
## <a name="getting-started"></a>Başlarken
Azure Machine Learning komut satırı arabirimi (CLI) bir uçtan uca veri bilimi akışı için gerekli tüm görevleri gerçekleştirmenizi sağlar. CLI araçlarını aşağıdaki yöntemlerle erişebilirsiniz:

### <a name="option-1-launch-azure-ml-cli-from-azure-ml-workbench-log-in-dialog-box"></a>Seçenek 1. Azure ML CLI Azure ML çalışma ekranı oturum açma iletişim kutusundan başlatma
Azure ML çalışma ekranı ve oturum açma ilk kez başlattığınızda ve bir deneme hesabı için erişim zaten yoksa, şu ekranla sunulur:

![hiçbir hesap bulunamadı](media/tutorial-iris-azure-cli/no_account_found.png)

Tıklayın **komut satırı penceresinde** komut satırı penceresinde başlatmak için iletişim kutusuna bağlantı.

### <a name="option-2-launch-azure-ml-cli-from-azure-ml-workbench-app"></a>Seçenek 2. Azure ML CLI Azure ML çalışma ekranı uygulamasını başlatma
Bir deneme hesabı için erişim zaten varsa, başarılı bir şekilde oturum. Ve ardından tıklayarak komut satırı penceresi açabilir **dosya** --> **komut istemi açın** menüsü.

### <a name="option-3-enable-azure-ml-cli-in-an-arbitrary-command-line-window"></a>Seçenek 3. Azure ML CLI rasgele bir komut satırı penceresinde etkinleştir
Azure ML CLI komut satırı penceresinde de etkinleştirebilirsiniz. Sadece bir komut penceresini başlatın ve aşağıdaki komutları girin:

```sh
# Windows Command Prompt
set PATH=%LOCALAPPDATA%\amlworkbench\Python;%LOCALAPPDATA%\amlworkbench\Python\Scripts;%PATH%

# Windows PowerShell
$env:Path = $env:LOCALAPPDATA+"\amlworkbench\Python;"+$env:LOCALAPPDATA+"\amlworkbench\Python\Scripts;"+$env:Path

# macOS Bash Shell
PATH=$HOME/Library/Caches/AmlWorkbench/Python/bin:$PATH
```
Değişikliği kalıcı yapmak için kullanabileceğiniz `SETX` Windows. MacOS için kullandığınız `setenv`.

>[!TIP]
>Yukarıdaki ortam değişkenleri ayarlayarak, sık kullanılan terminal penceresinde Azure CLI etkinleştirebilirsiniz.

## <a name="step-1-log-in-to-azure"></a>1. Adım Azure'da oturum açma
CLI AMLWorkbench uygulamadan açmak için ilk adımdır (Dosya > komut istemi açın). Bunun yapılması, doğru python ortamı kullanırız ve kullanılabilir ML CLI komutları sahibiz sağlar. 

Biz sonra sağ bağlam erişmek ve Azure kaynaklarınızı yönetmek için CLI ayarlamanız gerekir.
 
```azure-cli
# log in
$ az login

# list all subscriptions
$ az account list -o table

# set the current subscription
$ az account set -s <subscription id or name>
```

## <a name="step-2-create-a-new-azure-machine-learning-experimentation-account-and-workspace"></a>2. Adım Yeni bir Azure Machine Learning deneme hesabı ve çalışma alanı oluşturma
Yeni bir deneme hesabı ve yeni bir çalışma alanı oluşturmaya başlayın. Bkz: [Azure Machine Learning kavramları](overview-general-concepts.md) deneme hesapları ve çalışma alanları hakkında daha fazla ayrıntı için.

> [!NOTE]
> Deneme hesapları deneme çalışmalarınız çıkışları depolamak için kullanılan bir depolama hesabı gerektirir. Depolama hesabı adı ile ilişkili bir url olduğundan Azure içinde genel olarak benzersiz olması gerekir. Mevcut bir depolama hesabını belirtmezseniz, deneme hesabınızın adını yeni bir depolama hesabı oluşturmak için kullanılır. Benzersiz bir ad kullandığınızdan emin olun veya gibi bir hata iletisiyle karşılaşırsınız _"adlı depolama hesabı \<storage_account_name > zaten alındı."_ Alternatif olarak, kullanabileceğiniz `--storage` mevcut bir depolama hesabını sağlamak için bağımsız değişken.

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

## <a name="step-2a-optional-share-a-workspace-with-co-worker"></a>Adım 2.a (isteğe bağlı) ile birlikte çalışan bir çalışma alanı paylaşma
Buraya bir iş arkadaşı ile çalışma alanına erişim paylaşma keşfedin. Bir deneme hesabı veya bir proje erişimini paylaşmak için adımlar aynı olacaktır. Yalnızca Azure kaynak kimliği alma şekilde güncelleştirilmesi gerekir.

```azure-cli
# find the workspace Azure Resource ID
$az ml workspace show --name <workspace name> --account <experimentation account name> --resource-group <resource group name>

# add Bob to this workspace as a new owner
$az role assignment create --assignee bob@contoso.com --role owner --scope <workspace Azure Resource ID>
```

> [!TIP]
> `bob@contoso.com`Yukarıdaki komutta geçerli bir olmalıdır Azure AD kimliğinde geçerli aboneliğe ait olduğu için dizin.

## <a name="step-3-create-a-new-project"></a>3. Adım Yeni bir proje oluşturma
Bizim sonraki adım, yeni bir proje oluşturmaktır. Yeni bir proje ile çalışmaya başlamak için birkaç yolu vardır.

### <a name="create-a-new-blank-project"></a>Yeni boş bir proje oluşturun

```azure-cli
# create a new project
$ az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path>
```

### <a name="create-a-new-project-with-a-default-project-template"></a>Varsayılan proje şablonu ile yeni bir proje oluşturun
Yeni bir proje ile varsayılan bir şablon oluşturabilirsiniz.

```azure-cli
$ az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path> --template
```

### <a name="create-a-new-project-associated-with-a-cloud-git-repository"></a>Git deposu Bulutu ile ilişkili yeni bir proje oluşturma
VSTS (Visual Studio Team hizmeti) Git deposu ile ilişkili yeni bir proje oluşturabilir. Bir deneme gönderilen her zaman, bir anlık görüntü tüm proje klasörünün uzak Git deposuna taahhüt eder. Bkz: [kullanarak Git deposu Azure Machine Learning çalışma ekranı projesi ile](using-git-ml-project.md) daha fazla ayrıntı için.

> [!NOTE]
> Azure Machine Learning yalnızca VSTS oluşturulan boş Git depoları destekler.

```azure-cli
$ az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path> --repo <VSTS repo URL>
```
> [!TIP]
> "Depo URL'si geçersiz olabilir veya kullanıcı erişimi sahip olmayabilir" bir hata alıyorsanız, bir güvenlik belirteci VSTS oluşturabilirsiniz (altında _güvenlik_, _kişisel erişim belirteçleri eklemek_ menüsü) ve `--vststoken`projenizi oluştururken bağımsız değişkeni. 

### <a name="sample_create"></a>Bir örnek alarak yeni bir proje oluşturun
Bu örnekte, bir örnek proje şablon olarak kullanarak yeni bir proje oluşturun.

```azure-cli
# List the project samples, find the Classifying Iris sample
$ az ml project sample list

# Create a new project from the sample
az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path> --template-url https://github.com/MicrosoftDocs/MachineLearningSamples-Iris
```
Projeniz oluşturulduktan sonra kullanmak `cd` komutu proje dizini girin.

## <a name="step-4-run-the-training-experiment"></a>Adım 4 eğitim denemeyi çalıştırın 
Aşağıdaki adımlar Iris örnek projeyle sahip olduğunuzu varsaymaktadır (bkz [çevrimiçi bir örnek alarak yeni bir proje oluşturun](#sample_create)).

### <a name="prepare-your-environment"></a>Ortamınızı hazırlama 
Iris örnek için biz matplotlib yüklemeniz gerekir.

```azure-cli
$ pip install matplotlib
```

###  <a name="submit-the-experiment"></a>Denemeyi gönderme

```azure-cli
# Execute the file
$ az ml experiment submit --run-configuration local iris_sklearn.py
```

### <a name="iterate-on-your-experiment-with-descending-regularization-rates"></a>Denemenizi regularization oranları azalan ile yineleme
Bazı yaratıcılık ile farklı regularization hızı denemeler gönderen bir Python komut dosyası bir araya basittir. (Sağ proje yolunu işaret edecek şekilde dosyayı düzenleyin gerekebilir.)

```azure-cli
$ python run.py
```

## <a name="step-5-view-run-history"></a>5. Adım. Çalıştırma geçmişini görüntüleme
Komutu aşağıdaki yürütülen tüm önceki çalışmalarını listeler. 

```azure-cli
$ az ml history list -o table
```
Yukarıdaki komut çalıştıran bu projeye ait tüm metinler listesini görüntüler. Kesinlik ve regularization oranı ölçümleri çok listelendiğini görebilirsiniz. Bu yapma, en iyi tanımlamak kolay çalıştırın listeden.

## <a name="step-5a-view-attachment-created-by-a-given-run"></a>Verilen çalışması tarafından oluşturulan 5.a görünüm ek adım 
Belirli bir çalışma ile ilişkili Eki görüntülemek için çalıştırma geçmişi bilgileri komutu kullanabilirsiniz. Yukarıdaki listeden belirli bir çalışma çalışma kimliğini bulun.

```azure-cli
$ az ml history info --run <run id> --artifact driver_log
```

Bir çalışma yapıları indirmek için komutu kullanabilirsiniz:

```azure-cli
# Stream a given attachment 
$ az ml history info --run <run id> --artifact <artifact location>
```

## <a name="step-6-promote-artifacts-of-a-run"></a>6. Adım. Bir çalışma yapıları Yükselt 
Biz üretime dağıtılacak Puanlama bir web hizmeti oluşturmak için kullanmak istediğiniz şekilde biri biz yapılan çalıştırır, daha iyi bir AUC sahiptir. Bunu yapmak için önce bir varlığa yapıları yükseltmek ihtiyacımız.

```azure-cli
$ az ml history promote --run <run id> --artifact-path outputs/model.pkl --name model.pkl
```

Bu oluşturur bir `assets` proje dizininiz klasöründe bir `model.pkl.link` dosyası. Bu bağlantı dosyası yükseltilen bir varlık başvurmak için kullanılır.

## <a name="step-7-download-the-files-to-be-operationalized"></a>7. Adım. Kullanıma hazır hale getirilmiş için dosyaları indirme
Şimdi biz bizim tahmin web hizmeti oluşturmak için kullanabilmeniz için yükseltilen modeli indirmek ihtiyacımız. 

```azure-cli
$ az ml asset download --link-file assets\pickle.link -d asset_download
```

## <a name="step-8-setup-your-model-management-environment"></a>8. adım. Model yönetim ortamı Kurulumu 
Web hizmetleri dağıtmak için bir ortamı oluşturuyoruz. Biz, web hizmeti Docker kullanarak yerel makinede çalıştırabilirsiniz. Veya yüksek ölçekli işlemleri için bir ACS kümeye dağıtın. 

```azure-cli
# Create new local operationalization environment
$ az ml env setup -l <supported Azure region> -n <env name>
# Once setup is complete, set your environment for current context
$ az ml env set -g <resource group name> -n <env name>
```

## <a name="step-9-create-a-model-management-account"></a>9. adım. Bir model yönetim hesabı oluşturun 
Bir model yönetim hesabı dağıtma ve üretim Modellerinizi izlemek için gereklidir. 

```azure-cli
$ az ml account modelmanagement create -n <model management account name> -g <resource group name> -l <supported Azure region>
```

## <a name="step-10-create-a-web-service"></a>10. adım. Web hizmeti oluşturma
Ardından dağıttığımız modeli kullanarak bir tahmini döndüren bir web hizmeti oluşturuyoruz. 

```azure-cli
$ az ml service create realtime -m asset_download/model.pkl -f score.py -r python –n <web service name>
```

## <a name="step-10-run-the-web-service"></a>10. adım. Web hizmeti çalıştırın
Önceki adımda çıktısından web hizmeti kimliği kullanarak, web hizmetini çağırmak ve test. 

```azure-cli
# Get web service usage infomration
$ az ml service usage realtime -i <web service id>

# Call the web service with the run command:
$ az ml service run realtime -i <web service id> -d <input data>
```

## <a name="deleting-all-the-resources"></a>Tüm kaynakları silme 
Şimdi bu öğreticide oluşturduk, tüm kaynakları çalışmaya devam etmek istemiyorsanız silerek tamamlandı! 

Bunu yapmak için biz yalnızca tüm KAYNAKLARIMIZI bulunduran kaynak grubunu silin. 

```azure-cli
az group delete --name <resource group name>
```

## <a name="next-steps"></a>Sonraki Adımlar
Bu öğreticide, Azure Machine Learning Önizleme özellikleri kullanmak öğrendiniz 
> [!div class="checklist"]
> * Bir deneme hesabı kurmak, çalışma alanı oluşturma
> * Projeleri oluşturma
> * Birden çok işlem hedef denemeleri gönder
> * Yükseltme ve eğitilen model kaydetme
> * Model yönetimi için bir model yönetim hesabı oluşturun
> * Bir web hizmeti dağıtmak için bir ortamı oluşturun
> * Web hizmeti ve yeni verilerle puanı dağıtma