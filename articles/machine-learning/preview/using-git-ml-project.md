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
ms.date: 11/16/2017
ms.openlocfilehash: c91eadd69eaf16b2496f4d7247e5b0121904e172
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="using-git-repository-with-an-azure-machine-learning-workbench-project"></a>Git deposu bir Azure Machine Learning çalışma ekranı project ile kullanma
Bu belge, Azure Machine Learning çalışma ekranı Git veri bilimi denemenizi içinde yeniden Üretilebilirlik emin olmak için kullanma hakkında bilgi sağlar. Projenizin bulutla Git deposu ilişkilendirmek yönergeler de sağlanır.

## <a name="introduction"></a>Giriş
Azure Machine Learning çalışma ekranı Yukarı Git tümleştirmesi sıfırdan tasarlanmıştır. İkinci bir gizli yerel Git deposu adlı bir dal ile de oluşturulurken yeni bir proje oluştururken, proje klasöründen otomatik olarak "Git-yerel Git deposu (depo) başlatıldı" _AzureMLHistory / < project_GUID >_ için Her yürütme için proje klasörünü değişiklikleri takip edin. 

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


> [!TIP]
> Azure Machine Learning çalışma ekranı erişmek için kullanılan Azure Active Directory (AAD) hesabıyla oturum açmak emin olun. Aksi takdirde, yeni takım projesi son yanlış Kiracı kimliği altında yukarı ve Azure Machine Learning bulamayabilir. Bu durumda, komut satırı arabirimini kullanın ve VSTS belirteci sağlamanız gerekir.

Takım projesi oluşturulduktan sonra sonraki adıma geçmeye hazırsınız.

Doğrudan yeni oluşturduğunuz takım projesine gitmek için bir URL'dir `https://<team_project_name>.visualstudio.com`.

> [!NOTE]
> Şu anda, Azure Machine Learning yalnızca boş Git depoları hiçbir ana dala ile destekler. Komut satırı arabiriminden kullandığınız zorla bağımsız değişkeni, ana dala silmelisiniz. 

## <a name="step-3-create-a-new-azure-ml-project-with-a-remote-git-repo"></a>3. Adım Uzak bir Git deposu ile yeni bir Azure ML projesi oluşturma
Azure ML çalışma ekranı başlatın ve yeni bir proje oluşturun. Git deposu metin kutusuna, adım 2'den alma VSTS Git deposu URL'si ile doldurun. Genellikle şu şekildedir:`http://<vsts_account_name>.visualstudio.com/_git/<project_name>`

![Git deposu ile Azure ML projesi oluşturma](media/using-git-ml-project/create_project_with_git_rep.png)

Şimdi yeni bir Azure ML proje ile uzak Git deposu tümleştirme etkin ve hazırsınız oluşturulur. Proje klasöründeki her zaman yerel bir Git deposu Git ile başlatılmıştır. Ve Git _uzak_ uzak VSTS Git deposuna işlemeleri uzak Git deposu gönderilemez şekilde ayarlanır.

## <a name="step-3a-associate-an-existing-azure-ml-project-with-a-vsts-git-repo"></a>Adım 3a. Mevcut bir Azure ML projesini VSTS Git deposu ile ilişkilendirme
İsteğe bağlı olarak, Azure ML projeyi VSTS Git deposuna olmadan da oluşturabilir ve yalnızca yerel Git deposu çalıştırma geçmişi anlık görüntüleri için kullanır. Ve aşağıdaki komutu kullanarak bu var olan Azure ML proje ile daha sonra VSTS Git deposuna ilişkilendirebilirsiniz:

```azurecli
# make sure you are in the project path so CLI has context of your current project
$ az ml project update --repo http://<vsts_account_name>.visualstudio.com/_git/<project_name>
```

## <a name="step-4-capture-project-snapshot-in-git-repo"></a>4. Adım. Git deposu içinde proje anlık görüntü yakalama
Projede birkaç çalıştırır yürütebilir artık, bazı değişiklikler ortası çalıştırır olun. Masaüstü uygulaması, veya CLI kullanarak bunu yapabilirsiniz `az ml experiment submit` komutu. Daha fazla ayrıntı için izleyebileceğiniz [sınıflandırma Iris Öğreticisi](tutorial-classifying-iris-part-1.md). Her çalıştırma için proje klasöründeki tüm dosyalar yapılan herhangi bir değişiklik varsa tüm proje klasörünün bir anlık görüntü kaydedilen ve adlı bir dal altında uzaktaki Git deposuna içine gönderilen `AzureMLHistory/<Project_GUID>`. VSTS Git deposu URL'sini göz atarak dallar ve işlemlerini görüntülemek ve bu dal bulun. 

![çalıştırma geçmişi şube](media/using-git-ml-project/run_history_branch.png)

Daha iyi Not değil çalıştırma geçmişi dalında kendiniz. Bunun yapılması ile çalıştırma geçmişi uğraşmanız. Ana dala kullanabilir veya diğer dalların kullanılabilirliği etkilenmeden yerine kendi Git işlemleri için oluşturabilirsiniz.

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

Bu komutu yürüterek biz tüm proje klasörünü belirli çalıştıran başlayacağı zamana olduğunda alınan anlık görüntü ile üzerine yazar. Ancak, projenizin geçerli dalı üzerinde kalır. Bu, olacağı anlamına gelir **tüm değişiklikleri kaybetmek** geçerli proje klasörünüzdeki. Bu nedenle bu komutu çalıştırdığınızda, lütfen çok dikkatli olun.

## <a name="step-6-use-the-master-branch"></a>6. Adım. Ana dala kullanın
Yanlışlıkla önlemek için bir geçerli proje durumu kaybetme ana dala (veya kendi başınıza oluşturduğunuz herhangi bir dal) proje gerçekleştirmeyi Git deposuna yoludur. Git doğrudan ana dala üzerinde çalışması için komut satırını (veya diğer sık kullanılan Git istemci seçeceğiniz araç) de kullanabilirsiniz. Örneğin:

```
# make sure you are on the master branch (or branch of your choice)
$ git checkout master

# stage all changes
$ git add -A

# commit all changes locally on the master branch
$ git commit -m 'this is my updates so far'

# push changes into the remote VSTS Git repo master branch.
$ git push origin master
```

Proje güvenli bir şekilde daha önceki bir anlık görüntüye geri yükleyebilirsiniz artık aşağıdaki adım 5 ', her zaman geri uygulanmak üzere, yalnızca gelebilir olduğunu bilmek ana dal yapılan.

## <a name="authentication"></a>Kimlik Doğrulaması
Varsa yalnızca proje anlık görüntüleri almak için Azure ML çalıştırma geçmişi işlevlerde kullanır ve bunları geri yükleme, Git deposuna kimlik doğrulaması hakkında endişelenmeniz gerekmez. Bu gerçekleştirilecek Deneme hizmet katmanı tarafından önemli.

Ancak, sürüm denetimi yönetmek için kendi Git araçları kullanırsanız, düzgün şekilde VSTS üzerinde uzak Git deposu karşı kimlik doğrulamasını işleyecek gerekir. Diğer bir deyişle, yerel bilgisayarda Git deposu ile kimlik doğrulaması Git komutları o uzak Git deposu karşı yayımlayabilmesi ayarlamak gerekir. 

Bunu yapmak için en kolay yoludur bir SSH anahtarı çifti oluşturma ve Git deposuna güvenlik ayarları için ortak anahtar bölümü yüklemek için.

### <a name="generate-ssh-key"></a>SSH anahtarı oluşturma 
İlk şimdi bilgisayarınızda SSH anahtarları çifti oluşturur.

#### <a name="on-windows"></a>Windows:
Git GUI masaüstü uygulaması Windows ve altında başlatma _yardımcı_ menüsünde tıklatın _Göster SSH anahtarı_.

![SSH anahtarı](media/using-git-ml-project/git_gui.png)

SSH Pano'ya kopyalayın.

#### <a name="on-macos"></a>MacOS üzerinde:
Komut kabuğu'ndan hızlı adımlar:
```
# generate the SSH key
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

# start the SSH agent in the background
$ eval "$(ssh-agent -s)"

# add newly generated SSH key to the SSH agent
$ ssh-add -K ~/.ssh/id_rsa

# display the public key so you can copy it.
$ more ~/.ssh/id_rsa.pub
```
Daha fazla ayrıntı adımları bulunabilir [GitHub makalede](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).

### <a name="upload-public-key-to-git-repo"></a>Ortak anahtar Git deposuna karşıya yükleme
Visual Studio hesap sayfanız gidin: https://<vsts_account_name>.visualstudio.com ve oturum açma, ardından güvenliği, avatar altında.

SSH ortak anahtarını ekleyin, önceki adımda aldığınız SSH ortak anahtarını yapıştırın ve bir ad verin. Burada birden çok anahtar ekleyebilirsiniz.

Artık gerekli başka açık kimlik doğrulaması ile Git komutları Kaldır depoyu yerel olarak karşı uygulayabilirsiniz.

### <a name="read-more"></a>Daha fazla bilgi edinin
Lütfen bu iki makaleler (her iki yaklaşım çalışabilirsiniz) VSTS içinde uzak Git deposu için yerel kimlik doğrulamasının nasıl etkinleştirileceği hakkında daha fazla ayrıntı için izleyin.
- [SSH anahtar kimlik doğrulaması kullanma](https://www.visualstudio.com/en-us/docs/git/use-ssh-keys-to-authenticate)
- [Git kimlik bilgisi yöneticileri kullanın](https://www.visualstudio.com/en-us/docs/git/set-up-credential-managers)


## <a name="next-steps"></a>Sonraki adımlar
Takım veri bilimi işlemi proje yapınızı düzenlemek için bkz: nasıl kullanacağınızı öğrenin [TDSP sahip bir proje yapısı](how-to-use-tdsp-in-azure-ml.md)
