---
title: "Gezici ve Azure Machine Learning çalışma ekranı işbirliği | Microsoft Docs"
description: "Gezici ve Machine Learning çalışma ekranı işbirliği ayarlama öğrenin."
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/16/2017
ms.openlocfilehash: 137608007716452ec6468f1e13f494b095a11cb0
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="roaming-and-collaboration-in-azure-machine-learning-workbench"></a>Gezici ve Azure Machine Learning çalışma ekranı işbirliği
Bu makalede, Azure Machine Learning çalışma ekranı bilgisayarlar arasında gezici projeler ayarlama ve ekip üyeleri ile birlikte çalışmak için nasıl kullanabileceğinizi açıklar. 

Uzak bir Git deposu (depo) bağlantısı olan bir Azure Machine Learning projesi oluşturduğunuzda, proje meta verilerini ve anlık görüntüleri bulutta depolanır. Bulut bağlantı (gezici) farklı bir bilgisayardan projeye erişmek için kullanabilirsiniz. Ekip üyeleri ile işbirliği projesi için erişim vererek yapabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar
1. Machine Learning çalışma ekranı uygulamasını yükleyin. Bir Azure Machine Learning deneme hesabına erişim izni olduğundan emin olun. Daha fazla bilgi için bkz: [Yükleme Kılavuzu](quickstart-installation.md).

2. Erişim [Visual Studio Team Services](https://www.visualstudio.com) (Team Services) ve ardından projenize bağlamak için bir depo oluşturun. Daha fazla bilgi için bkz: [bir Git deposuna bir Machine Learning çalışma ekranı projesiyle kullanarak](using-git-ml-project.md).

## <a name="create-a-new-machine-learning-project"></a>Yeni bir Machine Learning projesi oluşturma
Machine Learning çalışma ekranı açın ve ardından yeni bir proje (örneğin, iris adlı bir proje) oluşturun. İçinde **Visualstudio.com GIT deposu URL'sini** kutusunda, bir Team Services Git deposuna için geçerli bir URL girin. 

> [!IMPORTANT]
> Boş proje şablonu seçerseniz, kullanmayı seçerseniz Git deposu zaten ana dala sahip olabilir. Machine Learning yalnızca yerel ana dala klonlar. Aml_config klasörü ve diğer proje meta veri dosyaları için yerel proje klasörünü ekler. 
>
> Tüm diğer proje şablonu, Git deposuna seçerseniz *olamaz* zaten bir ana dala sahip. Aşması durumunda, bir hata görürsünüz. Alternatif kullanmaktır `az ml project create` komutu ile projesi oluşturmak için bir `--force` geçin. Bu dosyaları özgün ana dal siler ve bunları seçtiğiniz şablon yeni dosyalar ile değiştirir.

Proje oluşturulduktan sonra projede olan herhangi bir betiği birkaç çalışır gönderin. Bu eylem proje durumu uzak Git deposu 's çalıştırma geçmişi dala kaydeder. 

> [!NOTE] 
> Yalnızca komut dosyası çalıştırma geçmişi dalı için tetikleyici yürütme çalışır. Veri yürütme ve dizüstü prep çalıştırır proje anlık görüntüleri çalıştırma geçmişi dal tetiklemek yok.

Git kimlik doğrulamasını ayarlarsanız, ana dala da çalışabilir. Veya yeni bir dalı oluşturabilirsiniz. 

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

### <a name="open-machine-learning-workbench-on-a-second-computer"></a>İkinci bir bilgisayara açık Machine Learning çalışma ekranı
Team Services Git deposuna projenizi ile bağlandıktan sonra Machine Learning çalışma ekranı yüklü olan herhangi bir bilgisayardan iris proje erişebilirsiniz. 

Başka bir bilgisayarda iris proje erişmek için uygulamaya projesi oluşturmak için kullanılan kimlik bilgilerini kullanarak oturumunuzu gerekir. Ayrıca aynı makine öğrenme deneme hesabı ve çalışma alanı olması gerekir. İris proje çalışma alanındaki diğer projelerle alfabetik olarak listelenir. 

### <a name="download-the-project-on-a-second-computer"></a>İkinci bir bilgisayara projenizi indirin
Çalışma alanı ikinci bilgisayarda açık olduğunda iris projeye bitişik normal klasörü simgesinden farklı simgedir. Yükleme simgesi proje içeriğinin bulutta olduğunu ve projenin geçerli bilgisayara yüklenmek için hazır olduğunu gösterir. 

![Proje oluşturma](./media/roaming-and-collaboration/downloadable-project.png)

Bir yüklemeye başlamak için iris projesini seçin. Yükleme tamamlandığında, proje ikinci bilgisayarda erişilebilmesi hazırdır. 

Windows üzerinde proje C:\Users bulunduğu\\< kullanıcı adı\>\Documents\AzureML.

MacOS üzerinde proje /home/ bulunur\<kullanıcıadı \> /belgeler/AzureML.

Bir hedef klasör seçebilmeniz için işlevselliğini geliştirmek planlıyoruz gelecekteki bir sürümde. 

> [!NOTE]
> Machine Learning dizininde projeyi tam aynı ada sahip bir klasörünüz varsa, yükleme başarısız olur. Bu sorunu çözmek için geçici olarak mevcut klasörünü yeniden adlandırın.


### <a name="work-on-the-downloaded-project"></a>İndirilen projedeki çalışma 
Yeni indirilen projedeki projesinde son çalıştırma proje durumunu yansıtır. Bir farklı çalıştır gönderme her zaman proje durumunun anlık görüntüsü Team Services Git deposuna çalıştırma geçmişi dalında için otomatik olarak taahhüt eder. Son çalıştırma ile ilişkili anlık görüntü ikinci bilgisayara proje örneği oluşturmak için kullanılır. 
 

## <a name="collaboration"></a>İş Birliği
Takım üyeleri bir Team Services Git deposuna bağlı projelerde ile işbirliği yapabilir. Machine Learning deneme hesabı, çalışma ve proje için kullanıcılara izinler atayabilirsiniz. Şu anda, Azure CLI kullanarak Azure Resource Manager komutları gerçekleştirebilirsiniz. Aynı zamanda [Azure portal](https://portal.azure.com). Daha fazla bilgi için bkz: [kullanıcıları eklemek için Azure portal'ı kullanmanızı](#portal).    

### <a name="use-the-command-line-to-add-users"></a>Kullanıcı eklemek için komut satırını kullanın
Örnek olarak, Alice iris proje sahibi. Alice, Bob ile projesi için erişim izni paylaşmak istiyor. 

Alice seçer **dosya** menüsüne ve ardından seçer **komut istemi** menü öğesi. Komut İstemi penceresini iris proje ile açılır. Alice, Bob için vermek istediği erişim düzeyini sonra karar verebilirsiniz. Aynen aşağıdaki komutları çalıştırarak izinleri verir:  

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

Rol ataması sonra doğrudan veya devralma tarafından Bob Machine Learning çalışma ekranı proje listesi projesinde görebilirsiniz. Bob, projeyi görmek için uygulamayı yeniden başlatmanız gerekebilir. Bob sonra yükleyebilirler proje açıklandığı gibi [gezici](#roaming)ve Alice ile işbirliği yapmak başlayın. 

Çalıştırma geçmişi bir projede işbirliği tüm kullanıcılar için aynı uzak Git deposuna taahhüt eder. Alice Çalıştır yürütüldüğünde, Bob Çalıştır Machine Learning çalışma ekranı uygulama projesinde çalıştırma geçmişi bölümünde görebilirsiniz. Bob proje Alice kullanmaya çalıştığında dahil olmak üzere tüm çalışma durumuna da geri yükleyebilirsiniz. 

Projesi için uzak bir Git deposu paylaşarak Alice ve Bob ayrıca ana dala işbirliği yapabilir. Gerekiyorsa, bunlar Ayrıca kişisel dalları oluşturmak ve Git çekme istekleri ve birleştirmeler işbirliği yapmak için kullanın. 

### <a name="use-the-azure-portal-to-add-users"></a>Kullanıcı eklemek için Azure portalını kullanın
<a name="portal"></a>

Makine öğrenme deneme hesapları, çalışma alanları ve projeleri Azure Resource Manager kaynaklarıdır. Rolleri atamak için kullanabileceğiniz **erişim denetimi** bağlamak [Azure portal](https://portal.azure.com). 

Kullanıcılara kullanarak eklemek istediğiniz kaynak bulmak **tüm kaynakları** görünümü. Seçin **erişim denetimi (IAM)** bağlamak ve ardından **kullanıcıları eklemek**. 

<img src="./media/roaming-and-collaboration/iam.png" width="320px">

## <a name="sample-collaboration-workflow"></a>Örnek işbirliği iş akışı
İşbirliği iş akışını göstermek için şimdi bir örnek yol. Contoso çalışanlar Alice ve Bob Machine Learning çalışma ekranı kullanarak bir veri bilimi projede işbirliği ister. Kimlikleri aynı Contoso Azure Active Directory (Azure AD) kiracıya ait. Alice ve ele Bob adımlar şunlardır:

1. Alice Team Services projede boş bir Git deposu oluşturur. Team Services proje Contoso Azure AD Kiracı altında oluşturulan bir Azure aboneliğiniz olmalıdır. 

2. Alice, her bilgisayarda bir Machine Learning deneme hesabı, bir çalışma alanı ve bir Machine Learning çalışma ekranı projesi oluşturur. Aynen proje oluşturduğunda, aynen Team Services Git deposu URL'sini girer.

3. Alice proje üzerinde çalışmaya başlar. Kendisinin bazı komut dosyaları oluşturur ve birkaç çalıştırır yürütür. Her çalıştırma için tüm proje klasörünün anlık bir Machine Learning çalışma ekranı oluşturur Team Services Git deposuna çalıştırma geçmişi dalının bir yürütme otomatik olarak gönderilir.

4. Alice ile iş sürüyor memnun olur. Yerel ana dal kendi değişiklikleri uygulayın ve ardından bunları Team Services Git deposuna ana dala gönderme istediği. Proje açıkken Machine Learning çalışma ekranı, aynen komut istemi penceresi açılır ve bu komutları girer:
    
    ```sh
    # Verify that the Git remote is pointing to the Team Services Git repo.
    $ git remote -v

    # Verify that the current branch is master.
    $ git branch

    # Stage all changes.
    $ git add -A

    # Commit changes with a comment.
    $ git commit -m "this is a good milestone"

    # Push the commit to the master branch of the remote Git repo in Team Services.
    $ git push
    ```

5. Alice, Bob Katılımcısı olarak çalışma alanına ekler. Aynen Azure portalında veya kullanarak bunu yapabilirsiniz `az role assignment` , daha önce gösterildiği gibi komutu. Alice, Bob okuma/yazma izinleri Team Services Git deposuna da verir.

6. Bob için Machine Learning çalışma ekranı bilgisayarını üzerinde imzalar. Alice ona ile paylaşılan çalışma görebilir. Bu çalışma alanı altında listelenen iris proje görebilir. 

7. Bob proje adı seçer. Proje bilgisayarına yüklenir.
    * İndirilen projedeki dosyaları çalıştırma geçmişi kaydedilen son çalıştırma anlık bir kopyasını alır. Son yürütme ana dala üzerinde değiller.
    * Yerel proje klasöründeki unstaged değişiklikleri ana dala ayarlanır.

8. Bob Alice tarafından yürütülen çalıştırır göz atabilirsiniz. Daha önceki tüm çalıştırır anlık görüntüleri geri yükleyebilirsiniz.

9. Bob Alice gönderilen en son değişiklikleri almak ve daha sonra farklı bir şube çalışmaya başlamak istiyor. Machine Learning çalışma ekranı, Bob bir komut istemi penceresi açar ve aşağıdaki komutları çalıştırır:

    ```sh
    # Verify that the Git remote is pointing to the Team Services Git repo.
    $ git remote -v

    # Verify that the current branch is master.
    $ git branch

    # Get the latest commit in the Team Services Git master branch and overwrite current files.
    $ git pull --force

    # Create a new local branch named "bob" so that Bob's work is done in the "bob" branch
    $ git checkout -b bob
    ```

10. Bob projeyi değiştirir ve yeni çalıştırır gönderir. Bob dalı üzerinde yapılan değişiklikler. Bob'ın çalıştığı aynı zamanda Alice'e görünür hale gelmiştir.

11. Bob uzak Git deposuna kendi değişiklikleri göndermek hazırdır. Alice nerede çalışıyor, ana dala ile çakışmayı önlemek için aynı zamanda bob adlı yeni bir uzaktan dalı için kendi iş Bob iter.

    ```sh
    # Verify that the current branch is "bob," and that it has unstaged changes.
    $ git status
    
    # Stage all changes.
    $ git add -A

    # Commit the changes with a comment.
    $ git commit -m "I found a cool new trick."

    # Create a new branch on the remote Team Services Git repo, and then push the changes.
    $ git push origin bob
    ```

12. Alice, kodu cool yeni eli hakkında bilgi için bir çekme isteği Bob uzak ana dala Git deposuna bob şube gelen oluşturur. Alice, ardından çekme isteği ana dala birleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinmek [bir Git deposuna bir Machine Learning çalışma ekranı projesiyle kullanarak](using-git-ml-project.md).
