---
title: "Bir Azure Machine Learning çalışma ekranı projeyle Git deposuna kullanın | Microsoft Docs"
description: "Bu makalede Azure Machine Learning çalışma ekranı projesi ile birlikte bir Git deposu kullanımı açıklanmaktadır."
services: machine-learning
author: hning86
ms.author: haining
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/18/2017
ms.openlocfilehash: f4f1112fe68bdb2a26f68b3da08fe97f9d22d3b7
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="use-a-git-repo-with-a-machine-learning-workbench-project"></a>Bir Machine Learning çalışma ekranı projesiyle Git deposuna kullanın
Git Azure Machine Learning çalışma ekranı sürüm denetimi sağlar ve veri bilimi denemenizi içinde yeniden Üretilebilirlik sağlamak için nasıl kullandığını öğrenin. Projenizin bulutla Git deposu (depo) ilişkilendir öğrenin.

Machine Learning çalışma ekranı Git tümleştirmesi için tasarlanmıştır. Yeni bir proje oluşturduğunuzda, proje klasöründen otomatik olarak "Git-yerel bir Git deposu başlatıldı". Bir ikinci, gizli yerel Git deposu Ayrıca, AzureMLHistory adlı bir dal ile oluşturulan /\<GUID proje\>. Dal, her yürütme için proje klasörünü değişiklikleri izler. 

Azure Machine Learning proje Git deposu ile ilişkilendirme otomatik sürüm denetimi, yerel olarak ve Uzaktan sağlar. Git deposu Visual Studio Team Services (Team Services) barındırılıyor. Machine Learning proje Git deposu ile ilişkili olduğundan, uzak bağlantıların erişimi olan herkes son kaynak kodunu (gezici) başka bir bilgisayara yükleyebilirsiniz.  

> [!NOTE]
> Team Services, Azure Machine Learning deneme hizmetten bağımsızdır kendi erişim denetimi listesi (ACL) vardır. Kullanıcı erişimi bir Git deposu ve Machine Learning çalışma alanı veya proje arasında değişebilir. Erişimi yönetmek üzere gerekebilir. 
> 
> Bir ekip üyesine vermek isteyip istemediğinizi kod Machine Learning projeniz için erişim düzeyi veya yalnızca çalışma paylaşma, kullanıcı bir Team Services Git deposuna erişmek için doğru izinleri vermeniz gerekir. 

Sürüm denetimi Git ile yönetmek için ana dala kullanabilir veya bağlantıların bulunması dala oluşturun. Yerel Git deposu kullanın ve sağlandığından, uzak Git deposuna Gönder.

Bu diyagramda Team Services Git deposuna ve Machine Learning proje arasındaki ilişkiyi göstermektedir:

![Azure Machine Learning Git](media/using-git-ml-project/aml_git.png)

Uzak bir Git deposu ile çalışmaya başlamak için aşağıdaki bölümlerde açıklanan adımları tamamlayın.

> [!NOTE]
> Şu anda, Azure Machine Learning yalnızca Team Services hesaplarını üzerinde Git depoları destekler. GitHub gibi genel Git depoları için destek gelecek için planlanan.

## <a name="step-1-create-a-machine-learning-experimentation-account"></a>1. Adım Bir Machine Learning deneme hesabı oluşturma
Bir Machine Learning deneme hesabı oluşturmak ve Azure Machine Learning çalışma ekranı uygulamasını yükleyin. Daha fazla bilgi için bkz: [yükleyin ve hızlı başlangıç oluşturma](quickstart-installation.md).

## <a name="step-2-create-a-team-project-or-use-an-existing-team-project"></a>2. Adım Takım projesi oluşturma veya varolan bir takım projesine kullanın
İçinde [Azure portal](https://portal.azure.com/), yeni bir takım projesi oluşturun:
1. Seçin  **+** .
2. Arama **takım projesi**.
3. Gerekli bilgileri girin:
    - **Ad**: bir ekip adı.
    - **Sürüm denetimi**: seçin **Git**.
    - **Abonelik**: bir Machine Learning deneme hesabına sahip bir abonelik seçin.
    - **Konum**: ideal olarak, Machine Learning deneme kaynaklarınızı yakın olan bir bölge seçin.
4. **Oluştur**’u seçin. 

![Azure portalında bir takım projesi oluşturma](media/using-git-ml-project/create_vsts_team.png)

Machine Learning çalışma ekranı erişmek için kullandığınız aynı Azure Active Directory (Azure AD) hesabı kullanarak oturum emin olun. Aksi takdirde, sistem, Azure AD kimlik bilgilerinizi kullanarak Machine Learning çalışma ekranı erişemiyor. Machine Learning projesi oluşturmak için komut satırını kullanın ve Git deposuna erişmek için kişisel erişim belirteci kaynağı, bir özel durumdur. Biz bu makalenin sonraki bölümlerinde daha ayrıntılı ele alınmıştır.

Doğrudan oluşturduğunuz takım projesine gitmek için URL https:// kullanmak\<ekip projesi adını\>. visualstudio.com.

## <a name="step-3-set-up-a-machine-learning-project-and-git-repo"></a>3. Adım Bir Machine Learning proje ve Git deposu ayarlama

Bir Machine Learning projesi ayarlamak için iki seçeneğiniz vardır:
- Uzak bir Git deposuna sahip bir Machine Learning projesi oluşturma
- Mevcut bir Machine Learning projesini Team Services Git deposuna ile ilişkilendirme

### <a name="create-a-machine-learning-project-that-has-a-remote-git-repo"></a>Uzak bir Git deposuna sahip bir Machine Learning projesi oluşturma
Machine Learning çalışma ekranı açın ve yeni bir proje oluşturun. İçinde **Git deposuna** kutusunda, adım 2'den Team Services Git deposu URL'sini girin. Genellikle şöyle görünür: https://\<Team Services hesabı adı\>.visualstudio.com/_git/\<proje adı\>

![Bir Git deposuna sahip bir Machine Learning projesi oluşturma](media/using-git-ml-project/create_project_with_git_rep.png)

Projeyi Azure komut satırı aracı (Azure CLI) kullanarak da oluşturabilirsiniz. Kişisel erişim belirteci girme seçeneğiniz vardır. Machine Learning, Azure AD kimlik bilgilerinizi kullanarak yerine Git deposuna erişmek için bu belirteci kullanabilirsiniz:

```
# Create a new project that has a Git repo by using a personal access token.
$ az ml project create -a <Experimentation account name> -n <project name> -g <resource group name> -w <workspace name> -r <Git repo URL> --vststoken <Team Services personal access token>
```

> [!IMPORTANT]
> Boş proje şablonu seçerseniz, kullanmayı seçerseniz Git deposu zaten ana dala sahip olabilir. Machine Learning yalnızca yerel ana dala klonlar. Aml_config klasörü ve diğer proje meta veri dosyaları için yerel proje klasörünü ekler. 
>
> Tüm diğer proje şablonu, Git deposuna seçerseniz *olamaz* zaten bir ana dala sahip. Aşması durumunda, bir hata görürsünüz. Alternatif kullanmaktır `az ml project create` komutu ile projesi oluşturmak için bir `--force` geçin. Bu dosyaları özgün ana dal siler ve bunları seçtiğiniz şablon yeni dosyalar ile değiştirir.

Etkin Uzak Git deposu ile tümleştirme, yeni bir Machine Learning projesi oluşturulur. Proje klasöründeki her zaman yerel bir Git deposu Git ile başlatılmıştır. Uzak Git deposuna yürütmelerini gönderdiğiniz şekilde uzak Git uzak Team Services Git deposuna ayarlanır.

### <a name="associate-an-existing-machine-learning-project-with-a-team-services-git-repo"></a>Mevcut bir Machine Learning projesini Team Services Git deposuna ile ilişkilendirme
Bir Team Services Git deposuna olmadan Machine Learning proje oluşturma ve çalıştırma geçmişi anlık görüntüler için yerel Git deposu kullanır. Daha sonra aşağıdaki komutu kullanarak bu varolan Machine Learning projeyle Team Services Git deposuna ilişkilendirebilirsiniz:

```azurecli
# Ensure that you are in the project path so Azure CLI has the context of your current project.
$ az ml project update --repo https://<Team Services account name>.visualstudio.com/_git/<project name>
```

> [!NOTE] 
> Kendisiyle ilişkili bir Git deposuna sahip olmayan bir Machine Learning proje güncelleştirme deposu işlemi gerçekleştirebilir. Ayrıca, bir Git deposu Machine Learning ile ilişkilendirdikten sonra onu kaldıramazsınız.

## <a name="step-4-capture-a-project-snapshot-in-the-git-repo"></a>4. Adım. Git deposu içinde proje anlık görüntü yakalama
Projede birkaç komut dosyasını çalıştırır yürütün ve çalışmaları arasında bazı değişiklikler yapın. Masaüstü uygulamasında veya Azure CLI kullanarak bunu yapabilirsiniz `az ml experiment submit` komutu. Daha fazla bilgi için bkz: [sınıflandırma Iris Öğreticisi](tutorial-classifying-iris-part-1.md). Her çalıştırma için proje klasöründeki herhangi bir dosya herhangi bir değişiklik yaptıysanız tüm proje klasörünün bir anlık görüntü kaydedilen ve AzureMLHistory adlı bir dal altında uzaktaki Git deposuna gönderilen /\<GUID proje\>. Dal ve işlemeleri AzureMLHistory dahil olmak üzere, görüntülemek için /\<GUID proje\> şube Team Services Git deposuna URL'sine gidin. 

> [!NOTE] 
> Anlık görüntü yalnızca bir komut dosyası yürütme önce kararlıdır. Şu anda veri hazırlığı yürütme ya da bir not defteri hücre yürütme anlık görüntü tetiklemek değil.

![çalıştırma geçmişi şube](media/using-git-ml-project/run_history_branch.png)

> [!IMPORTANT] 
> Git komutları kullanarak geçmiş dalında çalışmaya yok en iyisidir. İle çalıştırma geçmişi etkileyebilir. Bunun yerine, ana dala kullanabilir veya kendi Git işlemleri için dala oluşturabilirsiniz.

## <a name="step-5-restore-a-previous-project-snapshot"></a>5. Adım. Önceki bir proje anlık görüntü geri yükleme 
Tüm proje klasörünü, Machine Learning çalışma ekranı anlık görüntü bir önceki çalıştırma geçmişi durumunu geri yüklemek için:
1. Etkinliğin (kum saati simgesi) çubuğu seçin **çalışır**.
2. İçinde **çalışma listesi** görüntülemek için geri yüklemek istediğiniz Çalıştır'ı seçin.
3. İçinde **çalıştırmak ayrıntı** görünümü, select **geri**.

![çalıştırma geçmişi şube](media/using-git-ml-project/restore_project.png)

İsteğe bağlı olarak, Machine Learning çalışma ekranı Azure CLI penceresinde aşağıdaki komutları kullanın:

```azurecli
# Discover the run I want to restore a snapshot from.
$ az ml history list -o table

# Restore the snapshot from a specific run.
$ az ml project restore --run-id <run ID>
```

Bu komutu çalıştırırken dikkatli olun. Bu komutu yürütmek tüm proje klasörünü belirli çalıştıran başlayacağı zamana olduğunda alınan anlık üzerine yazar. Projenizi geçerli dalı kalır. Bunun anlamı, **tüm değişiklikleri kaybetmek** geçerli proje klasörünüzdeki.  

Bu işlemi gerçekleştirmeden önce geçerli dal değişikliklerinizi uygulamak için Git kullanmak isteyebilirsiniz.

## <a name="step-6-use-the-master-branch"></a>6. Adım. Ana dala kullanın
Geçerli proje durumu yanlışlıkla kaybetmemek için bir proje Git deposuna ana dala (veya kendi başınıza oluşturduğunuz herhangi bir dal) gerçekleştirmeyi yoludur. Ana dala üzerinde çalışması için komut satırından veya, sık kullanılan Git istemci aracından Git kullanabilirsiniz. Örneğin:

```sh
# Check status to make sure you are on the master branch (or branch of your choice).
$ git status

# Stage all changes.
$ git add -A

# Commit all changes locally on the master branch.
$ git commit -m 'these are my updates so far'

# Push changes to the remote Team Services Git repo master branch.
$ git push origin master
```

Şimdi, güvenli bir şekilde projenin daha önceki bir anlık görüntüye adım 5 tamamlayarak geri yükleyebilirsiniz. Her zaman geri yaptığınız yürütme üzerinde ana dala gelebilir.

## <a name="authentication"></a>Kimlik Doğrulaması
Proje anlık görüntülerini almak ve bunları geri yüklemek için makine öğrenme çalıştırma geçmişi işlevlerde üzerinde yalnızca güveniyorsanız, Git deposuna kimlik doğrulaması hakkında endişelenmeniz gerekmez. Kimlik doğrulaması, makine öğrenme Deneme hizmet katmanı tarafından işlenir.

Ancak, sürüm denetimi yönetmek için kendi Git araçları kullanırsanız, uzak Git deposu Team Services karşı kimlik doğrulamasını işleyecek gerekir. Machine Learning içinde uzak Git deposu yerel depoya Git remote olarak HTTPS protokolü kullanılarak eklenir. Bu, Git komutlarını (örneğin, anında iletme veya çekme) için uzaktan verdiğinizde, kullanıcı adınızı ve parolanızı veya kişisel erişim belirteci sağlamanız gerektiğini anlamına gelir. Bir Team Services Git deposuna kişisel erişim belirteci oluşturmak için'ndaki yönergeleri izleyin [kişisel erişim belirteci kimlik doğrulaması kullanmasını](https://docs.microsoft.com/vsts/accounts/use-personal-access-tokens-to-authenticate).

## <a name="next-steps"></a>Sonraki adımlar
- Bilgi edinmek için nasıl [proje yapınızı düzenlemek için takım veri bilimi işlemi kullanın](how-to-use-tdsp-in-azure-ml.md).
