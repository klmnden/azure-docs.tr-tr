---
title: Dolaşım ve işbirliği Azure Machine Learning workbench'te | Microsoft Docs
description: Dolaşım ve işbirliği Machine Learning workbench'te ayarlama konusunda bilgi edinin.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 11/16/2017
ms.openlocfilehash: 0abc5e34d2bfa1cf2a9fc0569831e21ed295891c
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44296508"
---
# <a name="roaming-and-collaboration-in-azure-machine-learning-workbench"></a>Dolaşım ve işbirliği Azure Machine Learning workbench'te
Bu makalede, bilgisayarlar arasında gezici projeler ayarlama ve ekip üyeleriyle işbirliği yaparak Azure Machine Learning Workbench nasıl kullanabileceğiniz açıklanır. 

Uzak bir Git deposuna (depo) bağlantısı olan bir Azure Machine Learning projesi oluşturduğunuzda, anlık görüntüler ve proje meta veriler bulutta depolanır. Proje (dolaşım) farklı bir bilgisayardan erişmek için bulut bağlantısı kullanabilirsiniz. Takım üyelerinizle işbirliği projeye erişimi vererek yapabilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar
1. Machine Learning Workbench uygulamasını yükleyin. Bir Azure Machine Learning denemesi hesap erişimi olduğundan emin olun. Daha fazla bilgi için [Yükleme Kılavuzu](../service/quickstart-installation.md).

2. Erişim [Azure DevOps](https://www.visualstudio.com) ve projenize bağlamak için bir depo oluşturun. Daha fazla bilgi için [bir Git deposu ile bir Machine Learning Workbench projesini kullanarak](using-git-ml-project.md).

## <a name="create-a-new-machine-learning-project"></a>Yeni bir Machine Learning projesi oluşturun
Machine Learning Workbench'i açın ve ardından (örneğin, Iris adlı bir proje) yeni bir proje oluşturun. İçinde **Visualstudio.com GIT deposu URL'si** kutusunda, bir Azure DevOps Git deposu için geçerli bir URL girin. 

> [!IMPORTANT]
> Boş proje şablonu seçerseniz, kullanmayı tercih Git deposu zaten bir ana dalın olabilir. Machine Learning yalnızca ana dalı yerel olarak kopyalar. Yerel proje klasörünün aml_config klasörünü ve diğer proje meta veri dosyaları ekler. 
>
> Herhangi diğer proje şablonu, Git deponuzu seçerseniz *olamaz* ana dal zaten sahip. Aksi halde, bir hata görürsünüz. Alternatif kullanmaktır `az ml project create` komutu ile bir proje oluşturmak için bir `--force` geçin. Bu, özgün ana daldaki dosyaları siler ve bunları seçtiğiniz şablon yeni dosyalar ile değiştirir.

Proje oluşturulduktan sonra projede hiçbir betikleriyle birkaç çalışır gönderin. Bu eylem, proje durumu uzak Git deponun çalıştırma geçmişi dala uygular. 

> [!NOTE] 
> Yalnızca betik çalıştırma geçmişi dala tetikleyici yürütmeleri çalıştırır. Veri hazırlama, yürütme ve not defteri çalıştırmalarını çalıştırma geçmişi daldaki proje anlık görüntüleri tetiklemek yok.

Git kimlik doğrulaması ayarlarsanız, ana dalda da çalışabilir. Veya yeni bir dal oluşturabilirsiniz. 

Örnek: 
```
# Check current repo status.
$ git status

# Stage all changes in the current repo.
$ git add -A

# Commit changes.
$ git commit -m "my commit fixes this weird bug!"

# Push to the remote repo.
$ git push origin master
```

## <a name="roaming"></a>Dolaşım
<a name="roaming"></a>

### <a name="open-machine-learning-workbench-on-a-second-computer"></a>İkinci bir bilgisayara Machine Learning Workbench'i açın
Azure DevOps Git deposu ile projenize bağlandıktan sonra Machine Learning Workbench yüklenmiş olan herhangi bir bilgisayardan Projesi'nden erişebilirsiniz. 

Başka bir bilgisayardaki Projesi'nden erişmek için uygulamaya projesi oluşturmak için kullanılan kimlik bilgilerini kullanarak oturum açmanız gerekir. Ayrıca, aynı Machine Learning denemesi hesabı ve çalışma alanı olması gerekir. Projesi'nden başka projelerle çalışma alanında alfabetik olarak listelenir. 

### <a name="download-the-project-on-a-second-computer"></a>İkinci bir bilgisayara projenizi indirin
İkinci bir bilgisayar üzerinde çalışma alanı açıkken, tipik bir klasör simgesini Iris projeye bitişik simgesi farklıdır. İndirme simgesinin projenin içeriğini bulutta olduğundan ve projenin geçerli bilgisayarda için yüklenmek hazır olduğunu gösterir. 

![Proje oluşturma](./media/roaming-and-collaboration/downloadable-project.png)

Projesi'nden bir yüklemeye başlamak için seçin. Yükleme tamamlandığında, ikinci bilgisayara erişilmeye hazır bir projedir. 

Windows üzerinde proje C:\Users bulunduğu\\< kullanıcı adı\>\Documents\AzureML.

MacOS üzerinde proje /home/ bulunduğu\<kullanıcıadı \> /belgeler/AzureML.

Gelecekteki bir sürümde bir hedef klasör seçebilmesi işlevselliğini geliştirmek planlıyoruz. 

> [!NOTE]
> Machine Learning dizinde proje aynı tam ada sahip bir klasör varsa, yükleme başarısız olur. Bu sorunu çözmek için var olan klasörünün geçici olarak yeniden adlandırın.


### <a name="work-on-the-downloaded-project"></a>İndirilen projedeki iş 
Yeni indirilen projedeki projedeki son çalıştırmada proje durumu yansıtır. Çalıştırma gönderdiğiniz her zaman proje durumunun bir anlık görüntü Azure DevOps Git deposunda çalıştırma geçmişi dal otomatik olarak taahhüt eder. Son çalıştırma ile ilişkili anlık görüntü, ikinci bir bilgisayar üzerinde bir proje oluşturmak için kullanılır. 
 

## <a name="collaboration"></a>İş Birliği
Bir Azure DevOps Git deposuna bağlı projelerde takım üyeleriyle işbirliği yapabilir. Machine Learning denemesi hesabı, çalışma alanı ve proje için kullanıcılara izinler atayabilirsiniz. Şu anda, Azure CLI kullanarak Azure Resource Manager komutları gerçekleştirebilirsiniz. Ayrıca [Azure portalında](https://portal.azure.com). Daha fazla bilgi için [kullanıcıları eklemek için Azure portal'ı kullanmanızı](#portal).    

### <a name="use-the-command-line-to-add-users"></a>Kullanıcılar eklemek için komut satırını kullanma
Örneğin, Gamze Projesi'nden sahibidir. Alice, Bob ile projeye erişimi paylaşma ister. 

Alice seçer **dosya** menüsüne ve ardından seçer **komut istemi** menü öğesi. Komut İstemi penceresini Projesi'nden açılır. Alice, Bob için vermek istediği erişim düzeyini sonra karar verebilirsiniz. Filiz, aşağıdaki komutları çalıştırarak izinleri verir:  

```azurecli
# Find the Resource Manager ID of the Experimentation account.
az ml account experimentation show --query "id"

# Add Bob to the Experimentation account as a Contributor.
# Bob now has read/write access to all workspaces and projects under the account by inheritance.
az role assignment create --assignee bob@contoso.com --role Contributor --scope <Experimentation account Resource Manager ID>

# Find the Resource Manager ID of the workspace.
az ml workspace show --query "id"

# Add Bob to the workspace as an Owner.
# Bob now has read/write access to all projects under the workspace by inheritance. Bob can invite or remove other users.
az role assignment create --assignee bob@contoso.com --role Owner --scope <workspace Resource Manager ID>
```

Rol ataması sonra doğrudan veya devralma, Bob proje Machine Learning Workbench'te Proje listesinde görebilirsiniz. Bob, proje görmek için uygulamayı yeniden başlatmanız gerekebilir. Bob ardından indirebileceği proje açıklandığı [gezici](#roaming)ve Alice ile işbirliği yapmaya başlayın. 

Bir proje üzerinde işbirliği yapın, tüm kullanıcılar için çalıştırma geçmişine aynı uzak Git deposuna taahhüt eder. Alice çalıştırma yürütüldüğünde, Bob çalıştırma projede bir Machine Learning Workbench uygulamasını çalıştırma geçmişi bölümünde görebilirsiniz. Bob proje Alice başlatılan çalıştırmaları da kapsayan tüm run, durumuna da geri yükleyebilirsiniz. 

Proje için uzak bir Git deposu paylaşarak Alice ve Bob ayrıca ana dalda işbirliği yapabilir. Gerekirse, bunlar Ayrıca kişisel dallar oluşturabilir ve Git çekme istekleri ve birleştirme, işbirliği yapmak için kullanın. 

### <a name="use-the-azure-portal-to-add-users"></a>Kullanıcıları eklemek için Azure portalını kullanma
<a name="portal"></a>

Machine Learning denemesi hesapları, çalışma alanları ve projeler, Azure Resource Manager kaynaklarıdır. Rolleri atamak için kullanabileceğiniz **erişim denetimi** bağlantısını [Azure portalında](https://portal.azure.com). 

Kullanıcılara kullanarak eklemek istediğiniz kaynak bulmak **tüm kaynakları** görünümü. Seçin **erişim denetimi (IAM)** bağlantısını ve ardından **kullanıcı ekleme**. 

<img src="./media/roaming-and-collaboration/iam.png" width="320px">

## <a name="sample-collaboration-workflow"></a>Örnek işbirliği iş akışı
İşbirliği iş akışını göstermek için bir örnek atalım. Alice ve Bob contoso çalışanlar, Machine Learning Workbench'i kullanarak bir veri bilimi proje üzerinde işbirliği yapmak istiyorsunuz. Kimlikleri aynı Contoso Azure Active Directory (Azure AD) kiracısına ait. Alice ve Bob ele adımlar şunlardır:

1. Alice, boş bir Git deposu içinde bir Azure DevOps projesi oluşturur. Azure DevOps projesi, Contoso Azure AD kiracısı altında oluşturulan bir Azure aboneliğinde olması gerekir. 

2. Alice, her bilgisayarda bir Machine Learning denemesi hesabı, bir çalışma alanı ve bir Machine Learning Workbench projesini oluşturur. Filiz, Filiz projeyi oluşturduğunda, Azure DevOps Git deponuzun URL'sini girer.

3. Alice, proje üzerinde çalışmaya başlar. Bazı komut dosyaları oluşturur ve birkaç çalıştırma yürütür. Her çalıştırma için tüm proje klasörünün anlık görüntüsünü otomatik olarak bir Machine Learning Workbench oluşturan Azure DevOps Git deposunun çalıştırma geçmişi dalına bir işleme gönderilir.

4. Alice Süren ile memnun olur. Filiz kendi yerel ana dala değişiklikleri kaydedin ve ardından bunları Azure DevOps Git deponun ana dalına gönderin ister. Machine Learning workbench'te Proje Aç ile Filiz komut istemi penceresi açar ve ardından şu komutları girer:
    
    ```sh
    # Verify that the Git remote is pointing to the Azure DevOps Git repo.
    $ git remote -v

    # Verify that the current branch is master.
    $ git branch

    # Stage all changes.
    $ git add -A

    # Commit changes with a comment.
    $ git commit -m "this is a good milestone"

    # Push the commit to the master branch of the remote Git repo in Azure DevOps.
    $ git push
    ```

5. Alice, Bob katkıda bulunan olarak çalışma alanına ekler. Filiz Azure portalında veya kullanarak bunu yapabilirsiniz `az role assignment` , daha önce gösterildiği gibi komutu. Alice, Bob okuma/yazma Azure DevOps Git deposu izinleri de verir.

6. Bob için Machine Learning Workbench, bilgisayarda imzalar. Alice kendisiyle paylaşılan çalışma görebilir. Bu çalışma alanı altında listelenen Projesi'nden görebilir. 

7. Bob, proje adını seçer. Proje kendi bilgisayarınıza indirilir.
    * Anlık görüntü çalıştırma geçmişinde kaydedilen son çalıştırmanın bir kopyasını indirilen proje dosyalardır. Son işlemeyi ana dala değiller.
    * Hazırlanmamış değişiklikleri ana dala yönelik yerel proje klasörünü ayarlayın.

8. Bob Alice tarafından yürütülen çalıştırmaları göz atabilirsiniz. Hüseyin, tüm önceki çalıştırmaları anlık görüntüsünü geri yükleyebilirsiniz.

9. Bob Alice gönderilen en son değişiklikleri almak ve ardından farklı bir dalda çalışmak istiyor. Machine Learning workbench'te Bob, bir komut istemi penceresi açılır ve aşağıdaki komutları yürütür:

    ```sh
    # Verify that the Git remote is pointing to the Azure DevOps Git repo.
    $ git remote -v

    # Verify that the current branch is master.
    $ git branch

    # Get the latest commit in the Azure DevOps Git master branch and overwrite current files.
    $ git pull --force

    # Create a new local branch named "bob" so that Bob's work is done in the "bob" branch
    $ git checkout -b bob
    ```

10. Bob, projeyi değiştirir ve yeni çalıştırmaları gönderir. Bob dalda yapılan değişiklikler. Bob'ın çalışır, ayrıca Alice'e görünebilir.

11. Bob, peter'in değişikliklerini uzak Git deponuza gönderme hazırdır. Alice nerede çalışıyor, master dalıyla çakışmayı önlemek için de bob adlı yeni bir uzak dala, kendi iş Bob iter.

    ```sh
    # Verify that the current branch is "bob," and that it has unstaged changes.
    $ git status
    
    # Stage all changes.
    $ git add -A

    # Commit the changes with a comment.
    $ git commit -m "I found a cool new trick."

    # Create a new branch on the remote Azure DevOps Git repo, and then push the changes.
    $ git push origin bob
    ```

12. Alice, kendi kod harika yeni ux'in bahsedin için bir çekme isteği Bob uzak Git deposuna bob dalından ana dala oluşturur. Esra'nın ardından çekme isteği ana dal ile birleştirin.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [bir Git deposu ile bir Machine Learning Workbench projesini kullanarak](using-git-ml-project.md).
