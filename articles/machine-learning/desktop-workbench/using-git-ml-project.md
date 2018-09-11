---
title: Bir Git deposu ile bir Azure Machine Learning Workbench projesini kullanın | Microsoft Docs
description: Bu makalede, bir Azure Machine Learning Workbench'te proje ile birlikte bir Git deposu kullanmayı açıklar.
services: machine-learning
author: hning86
ms.author: haining
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 11/18/2017
ms.openlocfilehash: 58ab1d77218595344c899dff654ba5b7a5bfb0d8
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44296645"
---
# <a name="use-a-git-repo-with-a-machine-learning-workbench-project"></a>Bir Git deposu ile bir Machine Learning Workbench projesini kullanın
Git'in Azure Machine Learning Workbench sürüm denetimi sağlayın ve veri bilimi denemenizi içinde yeniden üretilebilirliğini emin olmak için nasıl kullandığını öğrenin. Projenize bir Git deposuna (depo) bulutla ilişkilendir öğrenin.

Machine Learning Workbench, Git tümleştirmesi için tasarlanmıştır. Yeni bir proje oluşturduğunuzda, proje klasörü otomatik olarak "Git ile yerel bir Git deposu" başlatılır. İkinci, gizli yerel Git deposu da AzureMLHistory adlı bir dal oluşturulur, /\<GUID proje\>. Dal, her yürütme için proje klasörü değişiklikleri izler. 

Azure Machine Learning projesi Git deposu ile ilişkilendirerek otomatik sürüm denetimi, yerel olarak ve Uzaktan sağlar. Git deposu, Azure DevOps barındırılır. Machine Learning projesi Git deposu ile ilişkili olduğundan, uzak depo erişimi olan herkes en son kaynak kodu (dolaşım) başka bir bilgisayara yükleyebilirsiniz.  

> [!NOTE]
> Azure DevOps, Azure Machine Learning denemesi hizmeti, bağımsız olan kendi erişim denetimi listesi (ACL) vardır. Kullanıcı erişimini bir Git deposuna ve bir Machine Learning çalışma alanı veya proje arasında değişebilir. Erişim yönetmeniz gerekebilir. 
> 
> Bir takım üyesi vermek istediğiniz kod Machine Learning projenizi erişim düzeyi veya yalnızca çalışma paylaşma, kullanıcı Azure DevOps Git deposuna erişmek için doğru izinleri vermek gerekir. 

Git ile sürüm denetimi yönetmek için ana dala kullanabilir veya depoda diğer dalları oluşturun. Yerel Git deposu kullanma ve sağlandığından, uzak Git deposuna gönderin.

Bu diyagram bir Machine Learning projesi ile bir Azure DevOps Git deposu arasındaki ilişki gösterilmektedir:

![Azure Machine Learning Git](media/using-git-ml-project/aml_git.png)

Uzak bir Git deposu ile çalışmaya başlamak için aşağıdaki bölümlerde açıklanan adımları tamamlayın.

> [!NOTE]
> Şu anda Azure Machine Learning yalnızca Azure DevOps kuruluşlarına üzerinde Git depolarını destekler.

## <a name="step-1-create-a-machine-learning-experimentation-account"></a>1. Adım Bir Machine Learning denemesi hesabı oluşturun
Bir Machine Learning denemesi hesabı oluşturun ve Azure Machine Learning Workbench uygulamasını yükleyin. Daha fazla bilgi için [yükleme ve oluşturma Hızlı Başlangıç](../service/quickstart-installation.md).

## <a name="step-2-create-an-azure-devops-project-or-use-an-existing-project"></a>2. Adım Azure DevOps projesi oluşturun veya mevcut bir projeyi kullanın
İçinde [Azure portalında](https://portal.azure.com/), yeni bir proje oluşturun:
1. Seçin **+**.
2. Arama **takım projesi**.
3. Gerekli bilgileri girin:
    - **Ad**: bir takım adı.
    - **Sürüm denetimi**: seçin **Git**.
    - **Abonelik**: Machine Learning denemesi hesabı olan bir abonelik seçin.
    - **Konum**: ideal olarak, Machine Learning denemesi kaynaklarınızı yakın olan bir bölge seçin.
4. **Oluştur**’u seçin. 

![Azure portalını kullanarak bir proje oluşturma](media/using-git-ml-project/create_vsts_team.png)

Machine Learning Workbench erişmek için kullandığınız aynı Azure Active Directory (Azure AD) hesabını kullanarak oturum açtığınızdan emin olun. Aksi takdirde, sistem, Azure AD kimlik bilgilerinizi kullanarak Machine Learning Workbench erişemez. Machine Learning projesi oluşturmak için komut satırını kullanın ve Git deposuna erişmek için kişisel erişim belirteci sağlayın, bir özel durumdur. Bu makalenin ilerleyen bölümlerinde daha ayrıntılı olarak ele alır.

URL https:// doğrudan oluşturduğunuz proje için Git kullanın\<proje adı\>. visualstudio.com.

## <a name="step-3-set-up-a-machine-learning-project-and-git-repo"></a>3. Adım Bir Machine Learning projesi ve bir Git deposu ayarlama

Bir Machine Learning projesi ayarlamak için iki seçeneğiniz vardır:
- Uzak bir Git deposuna sahip bir Machine Learning projesi oluşturun
- Bir Azure DevOps Git deposu ile var olan bir Machine Learning projesi ilişkilendirmek

### <a name="create-a-machine-learning-project-that-has-a-remote-git-repo"></a>Uzak bir Git deposuna sahip bir Machine Learning projesi oluşturun
Machine Learning Workbench'i açın ve yeni bir proje oluşturun. İçinde **Git deposu** kutusunda, adım 2'den Azure DevOps Git deponuzun URL'sini girin. Genellikle şu şekilde görünür: https://\<Azure DevOps kuruluş adı\>.visualstudio.com/_git/\<proje adı\>

![Bir Git deposu olan Machine Learning projesi oluşturun](media/using-git-ml-project/create_project_with_git_rep.png)

Ayrıca, Azure komut satırı aracı (Azure CLI) kullanarak proje oluşturabilirsiniz. Kişisel erişim belirteci girme seçeneğiniz vardır. Machine Learning, Azure AD kimlik bilgilerinizi kullanarak yerine Git deposuna erişmek için bu belirteci kullanabilirsiniz:

```
# Create a new project that has a Git repo by using a personal access token.
$ az ml project create -a <Experimentation account name> -n <project name> -g <resource group name> -w <workspace name> -r <Git repo URL> --vststoken <Azure DevOps personal access token>
```

> [!IMPORTANT]
> Boş proje şablonu seçerseniz, kullanmayı tercih Git deposu zaten bir ana dalın olabilir. Machine Learning yalnızca ana dalı yerel olarak kopyalar. Yerel proje klasörünün aml_config klasörünü ve diğer proje meta veri dosyaları ekler. 
>
> Herhangi diğer proje şablonu, Git deponuzu seçerseniz *olamaz* ana dal zaten sahip. Aksi halde, bir hata görürsünüz. Alternatif kullanmaktır `az ml project create` komutu ile bir proje oluşturmak için bir `--force` geçin. Bu, özgün ana daldaki dosyaları siler ve bunları seçtiğiniz şablon yeni dosyalar ile değiştirir.

Etkin Uzak Git deposu ile tümleştirme, yeni bir Machine Learning projesi oluşturulur. Proje klasörü her zaman yerel bir Git deposu Git ile başlatılmıştır. Uzak Git deponuza işlemeleri gönderebilirsiniz, böylece uzak Git uzak Azure DevOps Git deposuna ayarlanır.

### <a name="associate-an-existing-machine-learning-project-with-an-azure-devops-git-repo"></a>Bir Azure DevOps Git deposu ile var olan bir Machine Learning projesi ilişkilendirmek
Azure DevOps Git deposu olmadan bir Machine Learning projesi oluşturun ve çalıştırma geçmişi anlık görüntüler için yerel Git deposu kullanır. Daha sonra aşağıdaki komutu kullanarak bir Azure DevOps Git deposu bu var olan Machine Learning projesi ile ilişkilendirebilirsiniz:

```azurecli
# Ensure that you are in the project path so Azure CLI has the context of your current project.
$ az ml project update --repo https://<Azure DevOps organization name>.visualstudio.com/_git/<project name>
```

> [!NOTE] 
> Kendisiyle ilişkili bir Git deposuna sahip olmayan bir Machine Learning projesi depo güncelleştirme işlemi gerçekleştirebilir. Ayrıca, bir Git deposuna bir Machine Learning ile ilişkilendirdikten sonra bunu kaldırılamıyor.

## <a name="step-4-capture-a-project-snapshot-in-the-git-repo"></a>4. Adım. Git deposunda bir projeyi anlık görüntü Yakala
Projede birkaç betik çalıştırmalarını yürütmek ve çalıştırmaları arasında bazı değişiklikleri yapın. Masaüstü uygulamasında veya Azure CLI kullanarak bunu yapabilirsiniz `az ml experiment submit` komutu. Daha fazla bilgi için [Iris sınıflandırma Öğreticisi](tutorial-classifying-iris-part-1.md). Her çalıştırma için herhangi bir dosya proje klasöründeki herhangi bir değişiklik yapılması durumunda tüm proje klasörünün anlık görüntüsünü kaydedilmiş ve AzureMLHistory adlı bir dal altında uzak Git deposuna gönderildi /\<GUID proje\>. Dallar ve işlemeler AzureMLHistory dahil olmak üzere, görüntülemek için /\<GUID proje\> dal, Azure DevOps Git deponun URL'sine gidin. 

> [!NOTE] 
> Anlık görüntü yalnızca bir betik yürütme önce kararlıdır. Şu anda, veri hazırlığı yürütme ya da bir not defteri hücre yürütme anlık görüntü tetikleme değil.

![Çalıştırma geçmişi dal](media/using-git-ml-project/run_history_branch.png)

> [!IMPORTANT] 
> Git komutlarını kullanarak geçmişi dalda çalışması yoksa, en iyisidir. Çalıştırma geçmişi ile kesintiye neden. Bunun yerine, ana dalı kullanın veya kendi Git işlemleri için diğer dalları oluşturun.

## <a name="step-5-restore-a-previous-project-snapshot"></a>5. Adım. Önceki bir proje anlık görüntü geri yükleme 
Tüm proje klasörünün anlık görüntü, Machine Learning Workbench'te bir önceki çalıştırma geçmişi durumunu geri yüklemek için:
1. Çubuk (kum saati simgesi) etkinliğinde seçin **çalıştırmaları**.
2. İçinde **çalıştırma listesi** görüntülemek için geri yüklemek istediğiniz farklı Çalıştır'ı seçin.
3. İçinde **çalıştırma ayrıntı** görüntülenecek **geri**.

![Çalıştırma geçmişi dal](media/using-git-ml-project/restore_project.png)

İsteğe bağlı olarak, Machine Learning Workbench uygulamasında Azure CLI penceresine aşağıdaki komutları kullanın:

```azurecli
# Discover the run I want to restore a snapshot from.
$ az ml history list -o table

# Restore the snapshot from a specific run.
$ az ml project restore --run-id <run ID>
```

Bu komutu çalıştırırken dikkatli olun. Bu komut yürütülürken, belirli bir çalıştırma başlatıldı, alınan anlık ile tüm proje klasörünün üzerine yazar. Projenize, geçerli dal olarak kalır. Diğer bir deyişle, **tüm değişiklikleri kaybetmek** geçerli proje klasörünüzde.  

Bu işlemi gerçekleştirmeden önce geçerli dala değişiklikleri işlemek için Git kullanmak isteyebilirsiniz.

## <a name="step-6-use-the-master-branch"></a>6. Adım. Ana dalı kullanın
Geçerli proje durumunuzu yanlışlıkla kaybetmemek için bir Git deposu'nın ana dalı (veya kendi başınıza oluşturduğunuz herhangi bir dala) projeyi kaydetmek için yoludur. Ana dalda çalışmak için komut satırından veya sık kullandığınız, Git istemci aracından Git kullanabilirsiniz. Örneğin:

```sh
# Check status to make sure you are on the master branch (or branch of your choice).
$ git status

# Stage all changes.
$ git add -A

# Commit all changes locally on the master branch.
$ git commit -m 'these are my updates so far'

# Push changes to the remote Azure DevOps Git repo master branch.
$ git push origin master
```

Şimdi, güvenli bir şekilde projeyi daha önceki bir anlık görüntüye 5. Adım'ı izleyerek geri yükleyebilirsiniz. Her zaman geri yaptığınız için yürütme ana dalda dönebilirsiniz.

## <a name="authentication"></a>Kimlik Doğrulaması
Yalnızca Machine learning'de proje anlık görüntülerini alabilir ve bunları geri yüklemek için çalıştırma geçmişi işlevlerde güveniyorsanız, Git deposu kimlik doğrulaması hakkında endişelenmeniz gerekmez. Kimlik doğrulaması, Machine Learning denemesi hizmeti katmanı tarafından işlenir.

Bununla birlikte, sürüm denetimi yönetmek için kendi Git araçlarını kullanıyorsanız, Azure DevOps uzak Git deposunda karşı kimlik doğrulamasını işlemek gerekir. Machine Learning'de uzak Git deposuna yerel depoya Git remote olarak HTTPS protokolü kullanılarak eklenir. Bu, uzak öğeye Git komutlarını (örneğin, itme veya çekme) verdiğinizde, kullanıcı adınızı ve parolanızı veya kişisel erişim belirteci sağlamanız gerektiğini anlamına gelir. Bir Azure DevOps Git deposunda bir kişisel erişim belirteci oluşturmak için yönergeleri izleyin. [kimlik doğrulaması için bir kişisel erişim belirteci kullanan](https://docs.microsoft.com/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate).

## <a name="next-steps"></a>Sonraki adımlar
- Bilgi edinmek için nasıl [proje düzenlemek için Team Data Science Process kullanma](how-to-use-tdsp-in-azure-ml.md).
