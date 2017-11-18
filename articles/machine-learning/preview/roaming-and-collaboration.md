---
title: "Makine gezici ve Azure işbirliği çalışma ekranı öğrenme | Microsoft Docs"
description: "Bilinen sorunların listesi ve gidermenize yardımcı olması için bir kılavuz"
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/16/2017
ms.openlocfilehash: 856348c07a198a8c53c6661441d5c49196ef3af5
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="roaming-and-collaboration-in-azure-machine-learning-workbench"></a>Gezici ve Azure Machine Learning çalışma ekranı işbirliği
Bu belge, nasıl Azure Machine Learning çalışma ekranı kodunuza etkinleştir işbirliğiyle yanı sıra makineler üzerinden projelerinizin dolaşan yardımcı olabileceği aracılığıyla açıklanmaktadır. 

Uzak bir Git deposu (depo) bağlantı ile bir Azure Machine Learning projesi oluşturduğunuzda, proje meta verilerini ve anlık görüntüleri bulutta depolanır. Bulut bağlantı, farklı bir bilgisayardan (gezici) proje erişmenize olanak tanır. Ayrıca, böylece işbirliğini etkinleştirme çalışma arkadaşlarınız için erişim verebilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar
İlk olarak, bir deneme hesabı için erişim ile Azure Machine Learning çalışma ekranı yükleyin. İzleyin [Yükleme Kılavuzu](quickstart-installation.md) daha fazla ayrıntı için.

İkinci olarak, erişim [Visual Studio Team System](https://www.visualstudio.com) ve projenize bağlamak için bir depo oluşturun. Git hakkında ayrıntılı bilgi için başvuru [kullanarak Git deposuna Azure Machine Learning çalışma ekranı projesi ile](using-git-ml-project.md) makalesi.

## <a name="create-a-new-azure-machine-learning-project"></a>Yeni bir Azure Machine Learning projesi oluşturma
Azure Machine Learning çalışma ekranı başlatın ve yeni bir proje oluşturun (örneğin, _iris_). Dolgu **Visualstudio.com GIT deposu URL'sini** geçerli bir VSTS Git deposu URL'si kutusuyla. 
>[!IMPORTANT]
>Git deposu üzerinde okuma/yazma erişimi yoksa proje oluşturma başarısız olur ve Git deposuna boş değil, yani ana dala zaten sahip.

Proje oluşturulduktan sonra proje içindeki herhangi bir komut dosyası birkaç çalışır gönderin. Bu eylem, uzak Git deposu 's çalıştırma geçmişi dala proje durumunu kaydeder. 

Kurulum Git kimlik doğrulaması varsa, yapabilir açıkça ana dala çalışmayabilir, veya bir dalı oluşturabilirsiniz. 

Örneğin: 
```
# check current repo status
$ git status

# stage all changes in the current repo
$ git add -A

# commit changes
$ git commit -m "my commit fixes this weird bug!"

# push to remote repo.
$ git push origin master
```

## <a name="roaming"></a>Dolaşım
<a name="roaming"></a>

### <a name="open-azure-machine-learning-workbench-on-second-machine"></a>Azure Machine Learning çalışma ekranı ikinci makinede açın
VSTS Git deposu projenizi ile bağlantılı sonra erişebilirsiniz _iris_ Azure Machine Learning çalışma ekranı yüklediğiniz herhangi bir bilgisayardan projesi. 

İris proje başka bir bilgisayara erişmek için uygulama için proje oluşturulurken kullanılan kimlik bilgileriyle oturum gerekir. Ayrıca, aynı deneme hesabı ve çalışma alanına gidin gerekir. _İris_ proje çalışma alanı içindeki diğer projelerle alfabetik olarak listelenir. 

### <a name="download-project-on-second-machine"></a>İkinci makinede projenizi indirin
Çalışma alanı ikinci açıldığında makine, bitişik simgesi _iris_ projedir normal klasörü simgesinden farklı. Yükleme simgesi projenin içerik bulutta ve geçerli makineye indirilmesi gerekir belirtir. 

![Proje oluşturma](./media/roaming-and-collaboration/downloadable-project.png)

Tıklayarak _iris_ proje karşıdan yükleme eylemi başlatır. While, yükleme tamamlandığında kısa sonra projeyi ikinci bilgisayarda erişilebilmesi hazırdır. 

Windows üzerinde olduğu`C:\Users\<username>\Documents\AzureML`

MacOS üzerinde aşağıda verilmiştir:`/home/<username>/Documents/AzureML`

Bir hedef klasör seçin olanak tanımak için işlevselliğini geliştirmek planlıyoruz gelecekteki bir sürümde. 

>Azure ML dizininde projesi indirme başarısız tam aynı ada sahip bir klasör için durum meydana unutmayın. Süresi olan, bu sorunu çözmek için varolan bir klasörü yeniden adlandırmanız gerekir.


### <a name="work-on-the-downloaded-project"></a>İndirilen projedeki çalışma 
Yeni indirilen projedeki projesinde son çalıştırma itibariyle proje durumunu yansıtır. Bir farklı çalıştır gönderme her zaman proje durumunun anlık görüntüsü VSTS Git deposuna çalıştırma geçmişi dalında için otomatik olarak taahhüt eder. İkinci bilgisayara proje başlatılırken son çalıştırma ile ilişkili anlık görüntü kullanırız. 
 

## <a name="collaboration"></a>İş Birliği
Kodunuza VSTS Git deposu için bağlı projelerde ile işbirliği yapabilir. Deneme hesabı, çalışma ve proje kullanıcılara izinleri atayabilirsiniz. Şu anda Azure CLI kullanarak Azure Resource Manager komutları gerçekleştirebilirsiniz. Aynı zamanda [Azure portal](https://portal.azure.com). Bkz: [bölümden](#portal).    

### <a name="using-command-line-to-add-users"></a>Kullanıcı eklemek için komut satırını kullanarak
Bir örnek sağlar. Alice th e_Iris_ proje sahibi ve paylaşım erişimi ile Bob istediği söyleyin. 

Alice tıkladığı **dosya** menü ve seçer **komut istemi** menü öğesi için yapılandırılmış komut istemi başlatmak için _iris_ projesi. Alice ardından aşağıdaki komutları yürüterek Bob için vermek istediği fo erişimi hangi düzeyi karar verebilirsiniz.  

```azurecli
# Find ARM ID of the experimnetation account
az ml account experimentation show --query "id"

# Add Bob to the Experimentation Account as a Contributor.
# Bob now has read/write access to all workspaces and projects under the Account by inheritance.
az role assignment create --assignee bob@contoso.com --role Contributor --scope <experimentation account ARM ID>

# Find ARM ID of the workspace
az ml workspace show --query "id"

# Add Bob to the workspace as an Owner.
# Bob now has read/write access to all projects under the Workspace by inheritance. And he can invite or remove others.
az role assignment create --assignee bob@contoso.com --role Owner --scope <workspace ARM ID>
```

Rol ataması sonra doğrudan veya devralma, Bob çalışma ekranı proje listesi projesinde görebilirsiniz. Uygulama projesi görmek için yeniden başlatma gerekebilir. Bob sonra yükleyebilirler proje açıklandığı gibi [bölüm gezici](#roaming) ve Alice ile işbirliği yapın. 

Çalıştırma geçmişi bir projede işbirliği tüm kullanıcılar için aynı uzak Git deposuna taahhüt eder. Bu nedenle Alice Çalıştır yürütüldüğünde, Bob Çalıştır çalışma ekranı uygulama projesinde çalıştırma geçmişi bölümünde görebilirsiniz. Bob, Alice tarafından başlatılan çalıştığı dahil olmak üzere çalışma durumuna proje da geri yükleyebilirsiniz. 

Projesi için uzak bir Git deposu paylaşımı Alice ve Bob ayrıca ana dala üzerinde işbirliği yapmak etkinleştirir. Gerekiyorsa, bunlar Ayrıca kişisel dalları oluşturmak ve Git çekme istekleri ve birleştirmeler işbirliği yapmak için kullanın. 

### <a name="using-azure-portal-to-add-users"></a>Kullanıcı eklemek için Azure portalını kullanma
<a name="portal"></a>

Azure Machine Learning deneme hesapları, çalışma alanları ve projeleri, Azure Resource Manager kaynaklarıdır. Erişim denetimi bağlantıyı kullanabilirsiniz [Azure portal](https://portal.azure.com) rolleri atamak için. 

Tüm kaynaklardan görüntülemek için kullanıcılar eklemek için aradığınız kaynak bulunamıyor. Erişim denetimini sayfada (IAM) bağlantısına tıklayın. Kullanıcı ekle 

<img src="./media/roaming-and-collaboration/iam.png" width="320px">

## <a name="sample-collaboration-workflow"></a>Örnek işbirliği iş akışı
İşbirliği akışını göstermek için şimdi bir örnek yol. Contoso çalışanlar Alice ve Bob Azure ML çalışma ekranı kullanarak bir veri bilimi projede işbirliği ister. Kimliklerini aynı Contoso Azure AD kiracısına ait.

1. Alice, ilk VSTS projede boş bir Git deposu oluşturur. Bu VSTS proje Contoso AAD kiracısı altında oluşturulan bir Azure aboneliği dinamik. 

2. Alice sonra her bilgisayarda bir Azure ML deneme hesabı, bir çalışma alanı ve bir Azure ML çalışma ekranı projesi oluşturur. Proje oluşturulurken aynen Git deposu URL'sini sağlar.

3. Alice proje üzerinde çalışmaya başlar. Kendisinin bazı komut dosyaları oluşturur ve birkaç çalıştırır yürütür. Her çalıştırma için tüm proje klasörünün anlık bir yürütme olarak çalışma ekranı tarafından oluşturulan VSTS Git deposuna çalıştırma geçmişi dalı uygulamasına otomatik olarak gönderilir.

4. Alice ile iş sürüyor mutluluk sunulmuştur. Yerel kendi değişikliği kaydetmek istediği _ana_ dal ve VSTS Git deposuna iter _ana_ dal. Proje Aç Bunu yapmak için aynen Azure ML çalışma ekranı komut istemi penceresinden başlatır ve aşağıdaki komutları sorunlar:
    
    ```sh
    # verify the Git remote is pointing to the VSTS Git repo
    $ git remote -v

    # verify that the current branch is master
    $ git branch

    # stage all changes
    $ git add -A

    # commit changes with a comment
    $ git commit -m "this is a good milestone"

    # push the commit to the master branch of the remote Git repo in VSTS
    $ git push
    ```

5. Alice, katılımcısı olarak çalışma alanına Bob sonra ekler. Aynen Azure portal veya kullanarak bunu yapabilirsiniz `az role assignment` komutu göstermeye üstünde. Kendisi ayrıca Bob okuma/erişim VSTS Git deposuna yazma verir.

6. Bob, bilgisayarda artık Azure ML çalışma ekranı içinde günlüğe kaydeder. Çalışma alanı ona ile paylaşılan Alice ve bu çalışma alanı altında listelenen proje görebilir. 

7. Bob proje adına göre tıklar ve projeyi bilgisayarına yüklenir.
    
    a. En son anlık görüntü kopyalarını çalıştırma geçmişi kaydedilen çalıştırmak indirilen projedeki dosyalarıdır. Son yürütme ana dala üzerinde değiller.
    
    b. Yerel proje klasöründeki ayarlanan _ana_ unstaged değişikliklerle dal.

8. Bob, artık Alice ve tüm önceki çalışmalarını geri yükleme görüntüsünü tarafından yürütülen çalıştırır göz atabilirsiniz.

9. Bob Alice tarafından gönderilen en son değişiklikleri almak ve farklı bir dalı üzerinde çalışmaya başlamak istiyor. Bu nedenle kendisi Azure ML çalışma ekranından komut istemi penceresi açar ve aşağıdaki komutları çalıştırır:

    ```sh
    # verify the Git remote is pointing to the VSTS Git repo
    $ git remote -v

    # verify that the current branch is master
    $ git branch

    # get the latest commit in VSTS Git master branch and overwrite current files
    $ git pull --force

    # create a new local branch named "bob" so Bob's work is done on the "bob" branch
    $ git checkout -b bob
    ```

10. Bob, artık projeyi değiştirir ve yeni çalıştırır gönderin. Değişiklikler üzerinde yapılır _bob_ dal. Ve Bob'un çalıştırır de Alice'e görünür hale gelmiştir.

11. Bob, değişiklikleri uzak Git deposu göndermek artık hazırdır. Çakışma olmaması için _ana_ Alice nerede çalıştığını dal, kendisi de adlı yeni bir uzaktan dalı kendi iş göndermek karar _bob_.

    ```sh
    # verify that the current branch is "bob" and it has unstaged changes
    $ git status
    
    # stage all changes
    $ git add -A

    # commit them with a comment
    $ git commit -m "I found a cool new trick."

    # create a new branch on the remote VSTS Git repo, and push changes
    $ git push origin bob
    ```

12. Bob sonra öğrenebilirsiniz Alice yeni cool eli hakkında kendi kodunda ve bir çekme isteği uzak Git deposu oluşturur _bob_ dallanma _ana_ dal. Ve Alice sonra içine çekme isteği birleştirme _ana_ dal.

## <a name="next-steps"></a>Sonraki adımlar
Git ile Azure ML çalışma ekranı kullanma hakkında daha fazla bilgi edinin: [Azure Machine Learning çalışma ekranı projesi ile kullanarak Git deposu](using-git-ml-project.md)