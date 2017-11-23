---
title: "Bir Azure Machine çalışma ekranı proje Learning Git deposuna kullanarak | Microsoft Docs"
description: "Bu makale bir Azure Machine Learning çalışma ekranı proje ile birlikte bir Git deposu kullanmayı açıklar."
services: machine-learning
author: hning86
ms.author: haining
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/18/2017
ms.openlocfilehash: 0cd447a52964578dd2348a786dd57a45ea87516e
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2017
---
# <a name="using-git-repository-with-an-azure-machine-learning-workbench-project"></a>Git deposu bir Azure Machine Learning çalışma ekranı project ile kullanma
Bu belge, Azure Machine Learning çalışma ekranı Git sürüm denetimi sağlar ve veri bilimi denemenizi içinde yeniden Üretilebilirlik sağlamak için kullanma hakkında bilgi sağlar. Projenizin bulutla Git deposu ilişkilendirmek yönergeler de sağlanır.

## <a name="introduction"></a>Giriş
Azure Machine Learning çalışma ekranı Yukarı Git tümleştirmesi sıfırdan tasarlanmıştır. Yeni proje oluşturma, proje klasöründen otomatik olarak "Git-yerel Git deposu (depo) başlatıldı" değildir. Bu sırada, ikinci bir gizli yerel Git deposu adlı bir dal ile de oluşturulur _AzureMLHistory / < project_GUID >_ her yürütme için proje klasörünü değişiklikleri izlemek için. 

Azure ML proje bir Visual Studio Team hizmet (VSTS) proje içinde barındırılan bir Git deposu ile ilişkilendirme otomatik sürüm denetimi, hem yerel hem de uzaktan sağlar. Bu ilişkilendirme uzak depoyu erişimi olan herkes son kaynak kodunu (gezici) başka bir bilgisayara yüklemek olanak sağlar.  

> [!NOTE]
> VSTS Azure Machine Learning deneme hizmetten bağımsızdır kendi erişim denetimi listesi vardır. Kullanıcı erişimi ve bir Azure ML çalışma alanı veya proje Git deposuna arasında farklılık gösterebilir ve yönetilmesi gerekir. Çalışma alanı paylaşmak istiyorsanız Azure ML projenizi kodu düzeyi erişim, ek olarak yalnızca dahil olmak üzere bir ekip üyesine ile paylaşmak için açıkça him/her VSTS Git deposu için uygun erişim vermeniz gerekir. 

Git ile da sürüm denetimi açıkça kullanarak yönetmek mümkündür _ana_ dallanma veya üzerinde deposu oluşturma dala göre. Uzak Git deposuna sağlanan varsa gönderebilir ve yalnızca yerel Git deposu kullanabilirsiniz.

Bu diyagramda VSTS Git deposu ile Azure ML projesinde arasındaki ilişkiyi göstermektedir:

![AML Git](media/using-git-ml-project/aml_git.png)

Uzak bir Git deposu kullanmaya başlamak için bu temel yönergeleri izleyin.

> [!NOTE]
> Şu anda, Azure Machine Learning VSTS hesaplarında yalnızca Git depoları destekler. Genel Git depoları (örneğin, GitHub ve vb.) için destek gelecekte planlanmaktadır.

## <a name="step-1-create-an-azure-ml-experimentation-account"></a>1. Adım Azure ML deneme hesabı oluşturma
Yoksa zaten, bir Azure ML deneme hesabı oluşturun ve Azure ML çalışma ekranı uygulamasını yükleyin. İçinde daha fazla ayrıntı görmek [yükleyin ve hızlı başlangıç oluşturma](quickstart-installation.md).

## <a name="step-2-create-a-team-project-or-use-an-existing-team-project"></a>2. Adım Takım projesi oluşturma veya varolan bir takım projesine kullanın
Gelen [Azure portal](https://portal.azure.com/), yeni bir **takım projesi**.
1. ' Yi tıklatın**+**
2. Arama **"Takım projesi"**
3. Gerekli bilgileri girin.
    - : Bir ekip adı.
    - Sürüm denetimi: **Git**
    - Abonelik: Azure Machine Learning deneme hesabıyla bir.
    - Konumu: Azure Machine Learning deneme kaynaklarınızı yakın olan bir bölgede ideal olarak kalır.
4. **Oluştur**'a tıklayın. 

![Azure portalından bir takım projesi oluşturma](media/using-git-ml-project/create_vsts_team.png)

Azure Machine Learning çalışma ekranı erişmek için kullandığınız aynı Azure Active Directory (AAD) hesabıyla oturum emin olun. Aksi takdirde, Azure ML proje oluşturma ve Git deposuna erişmek için kişisel erişim belirteci sağlama için komut satırı kullanmadığınız sürece sistem AAD kimlik bilgilerinizi kullanarak erişemiyor. Bu daha sonra daha fazla.

Takım projesi oluşturulduktan sonra sonraki adıma geçmeye hazırsınız.

Doğrudan yeni oluşturduğunuz takım projesine gitmek için bir URL'dir `https://<team_project_name>.visualstudio.com`.

## <a name="step-3-create-a-new-azure-ml-project-with-a-remote-git-repo"></a>3. Adım Uzak bir Git deposu ile yeni bir Azure ML projesi oluşturma
Azure ML çalışma ekranı başlatın ve yeni bir proje oluşturun. Git deposu metin kutusuna, adım 2'den alma VSTS Git deposu URL'si ile doldurun. Genellikle şu şekildedir:`http://<vsts_account_name>.visualstudio.com/_git/<project_name>`

![Git deposu ile Azure ML projesi oluşturma](media/using-git-ml-project/create_project_with_git_rep.png)

Komut satırı aracını kullanarak proje de oluşturabilirsiniz. Kişisel erişim belirteci vermesini seçeneğiniz vardır. Azure ML üzerinde AAD kimlik bilgileriniz kalmak yerine, sizin adınıza Git deposuna erişmek için bu belirteci kullanabilirsiniz:

```
# create a new project with a Git repo and personal access token.
$ az ml project create -a <experimentation account name> -n <project name> -g <resource group name> -w <workspace name> -r <Git repo URL> --vststoken <VSTS personal access token>
```
> [!IMPORTANT]
> Boş proje şablonu seçerseniz, seçtiğiniz zaten Git deposu varsa, bu normaldir bir _ana_ dal. Azure ML yalnızca klonlar _ana_ yerel olarak dallandırma ve ekleme `aml_config` klasörü ve diğer meta veri dosyaları yerel proje klasöre proje. Ancak herhangi bir proje şablonu seçerseniz, Git deposu zaten gerekir bir _ana_ şube veya bir hata görürsünüz. Alternatif kullanmaktır `az ml project create` proje oluşturma ve sağlama için komut satırı aracı bir `--force` geçin. Bu, özgün ana dala dosyalarını siler ve bunları seçtiğiniz şablona yeni dosyalar ile değiştirin.

Şimdi yeni bir Azure ML proje ile uzak Git deposu tümleştirme etkin ve hazırsınız oluşturulur. Proje klasöründeki her zaman yerel bir Git deposu Git ile başlatılmıştır. Ve Git _uzak_ uzak VSTS Git deposuna işlemeleri uzak Git deposu gönderilemez şekilde ayarlanır.

## <a name="step-3a-associate-an-existing-azure-ml-project-with-a-vsts-git-repo"></a>Adım 3a. Mevcut bir Azure ML projesini VSTS Git deposu ile ilişkilendirme
İsteğe bağlı olarak, Azure ML projeyi VSTS Git deposuna olmadan da oluşturabilir ve yalnızca yerel Git deposu çalıştırma geçmişi anlık görüntüleri için kullanır. Ve aşağıdaki komutu kullanarak bu var olan Azure ML proje ile daha sonra VSTS Git deposuna ilişkilendirebilirsiniz:

```azurecli
# make sure you are in the project path so CLI has context of your current project
$ az ml project update --repo http://<vsts_account_name>.visualstudio.com/_git/<project_name>
```

> [!NOTE] 
> Yalnızca ilişkili bir Git deposuna sahip değilse Azure ML projesinde güncelleştirme deposu işlemi gerçekleştirebilirsiniz. Ve Git deposuna ilişkilendirildiğinde kaldırılamaz.

## <a name="step-4-capture-project-snapshot-in-git-repo"></a>4. Adım. Git deposu içinde proje anlık görüntü yakalama
Projede birkaç komut dosyasını çalıştırır yürütebilir artık, bazı değişiklikler ortası çalıştırır olun. Masaüstü uygulaması, veya CLI kullanarak bunu yapabilirsiniz `az ml experiment submit` komutu. Daha fazla ayrıntı için izleyebileceğiniz [sınıflandırma Iris Öğreticisi](tutorial-classifying-iris-part-1.md). Her çalıştırma için proje klasöründeki tüm dosyalar yapılan herhangi bir değişiklik varsa tüm proje klasörünün bir anlık görüntü kaydedilen ve adlı bir dal altında uzaktaki Git deposuna içine gönderilen `AzureMLHistory/<Project_GUID>`. VSTS Git deposu URL'sini göz atarak dallar ve işlemlerini görüntülemek ve bu dal bulun. 

> [!NOTE] 
> Anlık görüntü yalnızca bir komut dosyası yürütme önce kararlıdır. Şu anda veri hazırlığı yürütme ya da bir not defteri hücre yürütme anlık görüntü tetiklemez.

![çalıştırma geçmişi şube](media/using-git-ml-project/run_history_branch.png)

> [!IMPORTANT] 
> Geçmiş dalında Git komutları kullanarak kendiniz çalışmamasının en iyisidir. Bunun yapılması çalıştırma geçmişi uğraşmanız. Ana dala kullanabilir veya diğer dalların kullanılabilirliği etkilenmeden yerine kendi Git işlemleri için oluşturabilirsiniz.

## <a name="step-5-restore-a-previous-project-snapshot"></a>5. Adım. Önceki bir proje anlık görüntü geri yükleme 
Tüm proje klasöründeki Azure ML çalışma ekranından, bir önceki çalıştırma geçmişi proje durumu anlık görüntü durumuna geri yüklemek için:
1. Tıklayın **çalışır** etkinliğinde (cam saatlik simgesi) çubuğu.
2. Gelen **çalışma listesi** görüntülemek için geri yüklemek istediğiniz Çalıştır'ı tıklatın.
3. Gelen **çalıştırmak ayrıntı** görüntülemek için tıklayın **geri**.

![çalıştırma geçmişi şube](media/using-git-ml-project/restore_project.png)

Alternatif olarak, Azure ML çalışma ekranı CLI penceresinden aşağıdaki komutu kullanabilirsiniz.

```azurecli
# discover the run I want to restore snapshot from:
$ az ml history list -o table

# restore the snapshot from a particular run
$ az ml project restore --run-id <run_id>
```

Bu komutu yürüterek biz tüm proje klasörünü belirli çalıştıran başlayacağı zamana olduğunda alınan anlık görüntü ile üzerine yazar. Ancak, projenizin geçerli dalı üzerinde kalır. Bu, olacağı anlamına gelir **tüm değişiklikleri kaybetmek** geçerli proje klasörünüzdeki. Bu nedenle bu komutu çalıştırdığınızda, lütfen çok dikkatli olun. Yukarıdaki işlemi gerçekleştirmeden önce geçerli dal değişikliklerinizi uygulamak için Git için isteyebilirsiniz. Önce daha fazla ayrıntı için bkz.

## <a name="step-6-use-the-master-branch"></a>6. Adım. Ana dala kullanın
Yanlışlıkla önlemek için bir geçerli proje durumu kaybetme ana dala (veya kendi başınıza oluşturduğunuz herhangi bir dal) proje gerçekleştirmeyi Git deposuna yoludur. Git doğrudan ana dala üzerinde çalışması için komut satırını (veya diğer sık kullanılan Git istemci seçeceğiniz araç) de kullanabilirsiniz. Örneğin:

```sh
# check status to make sure you are on the master branch (or branch of your choice)
$ git status

# stage all changes
$ git add -A

# commit all changes locally on the master branch
$ git commit -m 'these are my updates so far'

# push changes into the remote VSTS Git repo master branch.
$ git push origin master
```

Proje güvenli bir şekilde daha önceki bir anlık görüntüye geri yükleyebilirsiniz artık aşağıdaki adım 5 ', her zaman geri uygulanmak üzere, yalnızca gelebilir olduğunu bilmek ana dal yapılan.

## <a name="authentication"></a>Kimlik Doğrulaması
Varsa yalnızca proje anlık görüntüleri almak için Azure ML çalıştırma geçmişi işlevlerde kullanır ve bunları geri yükleme, Git deposuna kimlik doğrulaması hakkında endişelenmeniz gerekmez. Bu gerçekleştirilecek Deneme hizmet katmanı tarafından önemli.

Ancak, sürüm denetimi yönetmek için kendi Git araçları kullanırsanız, düzgün şekilde VSTS üzerinde uzak Git deposu karşı kimlik doğrulamasını işleyecek gerekir. Azure ML içinde uzak Git deposu yerel depoya Git HTTPS protokolünü kullanarak uzak eklenir. Git sorun olduğunda bunun anlamı, kullanıcı adı ve parola veya kişisel erişim belirteci sağlamanız gereken uzaktan (gibi İtme veya çekme), komutları. İzleyin [bu yönergeleri](https://docs.microsoft.com/vsts/accounts/use-personal-access-tokens-to-authenticate) bir VSTS Git deposuna kişisel erişim belirteci oluşturmak için.

## <a name="next-steps"></a>Sonraki adımlar
Takım veri bilimi işlemi proje yapınızı düzenlemek için bkz: nasıl kullanacağınızı öğrenin [TDSP sahip bir proje yapısı](how-to-use-tdsp-in-azure-ml.md)
