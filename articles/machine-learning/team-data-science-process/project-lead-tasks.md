---
title: Team Data Science Process içinde proje için görevleri sağlama
description: Anahat bir proje sağlama görevlerin bir veri bilimi takım projesi üzerinde tamamlanması bekleniyor.
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/13/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 00b1b58a39724951f2d5e4e688df8eb178654bbb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65952831"
---
# <a name="tasks-for-the-project-lead-in-the-team-data-science-process"></a>Team Data Science Process içinde proje için görevleri sağlama

Bu öğretici anahatlarını proje lideri olan görevler proje takımlarının tamamlanması bekleniyor. Hedefi üzerinde standartlaştırır ekip işbirliği ortamı oluşturmaktır [Team Data Science Process](overview.md) (TDSP). TDSP, etkinlikleri, bulut tabanlı ve Tahmine dayalı analiz çözümleri verimli bir şekilde yürütmek için yapılandırılmış bir dizi sağlayan Microsoft tarafından geliştirilen bir çerçevedir. Bu işlemi, standart personel rolleri ve veri bilimi ekibi tarafından işlenen ilişkilendirilen görevlerinin bir özetini görmek [Team Data Science Process rolleri ve görevleri](roles-tasks.md).

A **proje sağlama** belirli veri bilimi proje üzerinde tek tek veri bilimcileri günlük etkinliklerini yönetir. Bu ortamı ayarlamak için proje liderleri olarak tamamlanacak görevler için iş akışını aşağıdaki şekilde gösterilen:

![1](./media/project-lead-tasks/project-leads-1-tdsp-creating-projects.png)

Bu konu, şu anda proje liderleri için bu iş akışı görevleri 1,2 ve 6 kapsar.

> [!NOTE]
> Azure DevOps aşağıdaki yönergeleri kullanarak bir proje için TDSP takım ortamını ayarlamak için gerekli olan adımları genel çizgileriyle belirtin. Biz, biz Microsoft'ta TDSP nasıl uygulama olduğundan, Azure DevOps ile bu görevleri gerçekleştirmek üzere nasıl belirtin. Grubunuz için başka bir kod barındırma platformu kullanılıyorsa, ekip lideri tarafından genellikle tamamlanması gereken görevler değiştirmeyin. Ancak bu görevleri tamamlamak için yol farklı zordur.


## <a name="repositories-and-directories"></a>Depoları ve dizinler

Bu öğreticide, kısaltılmış depoları ve dizinler için kullanılır. Bu adlar dizinlerini ve depoları işlemleri izlemenizi kolaylaştırır. Bu gösterim (Git depoları için R) ve D DSVM'ye yerel dizinleri için aşağıdaki bölümlerde kullanılır:

- **R3**: Takım **ProjectTemplate** , ekip lideri ayarlanmış bir Git deposunda.
- **R5**: Proje Git deposunu projeniz için ayarlayın.
- **D3**: Yerel dizin R3 kopyalandı.
- **D5**: Yerel dizin R5 kopyalandı.


## <a name="0-prerequisites"></a>0. Önkoşullar

Özetlenen, Grup Yöneticisi atanan görevlerin tamamlayarak önkoşullara uyduğunuzdan [grup yöneticisi görevleri için bir veri bilimi ekip](group-manager-tasks.md) ve müşteri adayı özetlenen takım için [ekip sağlama görevleri için bir veri bilimi takım](team-lead-tasks.md). 

Burada özetlemek gerekirse, aşağıdaki gereksinimleri ekip sağlama görevlerini başlamadan önce yerine getirmeniz gerekir: 

- **Azure DevOps Services grubunda** (veya grup hesabı barındırma kodunu herhangi bir platform üzerinde), Grup Yöneticisi ayarlandı.
- **TeamProjectTemplate depo** (R3) ayarlanmış grubu hesabınız kapsamında kullanmayı planladığınız barındırma kodunu platformunda, ekip lideri tarafından.
- Size verilmiş olması **yetkili** depoları ekibiniz için Grup hesabınızı oluşturmak için Ekip Lideri tarafından.
- Git makinenizde yüklü olması gerekir. Bir veri bilimi sanal makinesi (DSVM) kullanıyorsanız, Git önceden yüklenmiş ve hazırsınız demektir. Aksi takdirde bkz [platformları ve araçlarıyla ek](platforms-and-tools.md#appendix).  
- Kullanıyorsanız bir **Windows DSVM**, ihtiyacınız [Git Credential Manager'ı (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) makinenizde yüklü. README.md dosyasında doğru aşağı kaydırın **indirme ve yükleme** tıklayın ve bölüm *en son yükleyicisi*. Bu en son yükleyici sayfasına götürür. .Exe yükleyiciyi buradan indirin ve çalıştırın. 
- Kullanıyorsanız **Linux DSVM'sini**, bir SSH ortak anahtarı üzerinde DSVM oluşturma ve Grup Azure DevOps hizmetlerinizi ekleyin. SSH hakkında daha fazla bilgi için bkz: **oluşturma SSH ortak anahtarı** konusundaki [platformları ve araçlarıyla ek](platforms-and-tools.md#appendix). 


## <a name="1-create-a-project-repository-r5"></a>1. Proje deposu (R5) oluşturun

- Oturum açma konumunda Grup Azure DevOps Services *https://\<Azure DevOps Hizmetleri adı\>. visualstudio.com*. 
- Altında **son projeler ve takımlar**, tıklayın **Gözat**. Açılır bir pencere, Azure DevOps hizmetler tüm projeleri listeler. 

    ![2](./media/project-lead-tasks/project-leads-2-create-project-repo.png)

- Proje deponuzu oluşturma olacak proje adına tıklayın. Bu örnekte, tıklayın **MyTeam**. 
- ' A tıklayarak **Navigate** projenin ana sayfaya yönlendirilmek için **MyTeam**:

    ![3](./media/project-lead-tasks/project-leads-3-create-project-repo-2.png)

- Tıklayın **kod üzerinde işbirliği** git giriş sayfasına projenizin yönlendirilecek.  

    ![4](./media/project-lead-tasks/project-leads-4-create-project-repo-3.png)

- Sol üst köşedeki aşağı oka tıklatın ve seçin **+ yeni havuz**. 
    
    ![5](./media/project-lead-tasks/project-leads-5-create-project-repo-4.png)

- İçinde **yeni depo Oluştur** penceresinde proje git deponuz için bir ad girin. Seçtiğinizden emin olun **Git** Havuz türü olarak. Bu örnekte, adını kullanıyoruz *DSProject1*. 

    ![6](./media/project-lead-tasks/project-leads-6-create-project-repo-5.png)

- Oluşturmak için ***DSProject1*** proje git deposu, tıklayın **Oluştur**.


## <a name="2-seed-the-dsproject1-project-repository"></a>2. Çekirdek DSProject1 proje deposu

Burada için çekirdek görevdir **DSProject1** proje şablonu deponuzu (R3) (R5) deposundan proje. Dengeli dağıtım yordam D3 ve D5 dizinleri yerel DSVM'ye Ara hazırlama siteler olarak kullanır. Özet olarak, dengeli dağıtım yoludur: R3 -> D3 -> D5 -> R5.

Özelleştirmeniz gerekirse, **DSProject1** bazı belirli karşılamak için proje deposuna proje gereksinimleriniz, aşağıdaki yordamın sondan adımında bunu. İçeriği sağlamak için kullanılan adımlarla bir özeti aşağıda verilmiştir **DSProject1** proje deposu. Her bir adımı alt bölümlere dengeli dağıtım yordamda karşılık gelir:

- Proje şablonu depoyu yerel dizine kopyala: yerel D3 -> için kopyalanan R3 - takım.
- Yerel bir dizine DSProject1 depoyu Kopyala: yerel D5 -> için kopyalanan R5 - takım.
- Kopyalanan proje şablonu içeriği DSProject1 depo yerel kopyasını kopyalayın:  D3 - D5 -> için kopyalanan içeriği.
- (İsteğe bağlı) Yerel D5 özelleştirme.
- Takım depolarını yerel DSProject1 içeriği gönderin: D5 - içeriğini team R5 -> için ekleyin.


### <a name="clone-your-project-template-repository-r3-to-a-directory-d3-on-your-local-machine"></a>Proje şablonu deponuza (R3) bir ' % s'dizini (D3) yerel makinenize kopyalayın.

Yerel makinenizde bir dizin oluşturun:

- *C:\GitRepos\MyTeamCommon* Windows için 
- *$home/GitRepos/MyTeamCommon* Linux

Bu dizine geçin. Daha sonra proje şablonu deposunu yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın. 

**Windows**
            
    git clone <the HTTPS URL of the TeamProjectTemplate repository>
    
Azure DevOps kod barındırma platformu olarak genellikle kullanıyorsanız *HTTPS URL'si proje şablonu deponuzun* olan:

 ***https://\<Azure DevOps Hizmetleri adı\>.visualstudio.com/\<projenizin adına\>/_git/\<proje şablonu depo adınızı\>***. 

Bu örnekte, sunuyoruz:

***https://mysamplegroup.visualstudio.com/MyTeam/_git/MyTeamProjectTemplate***. 

![7](./media/project-lead-tasks/project-leads-7-clone-team-project-template.png)
            
**Linux**

    git clone <the SSH URL of the TeamProjectTemplate repository>
        
![8](./media/project-lead-tasks/project-leads-8-clone-team-project-template-linux.png)

Azure DevOps kod barındırma platformu olarak genellikle kullanıyorsanız *SSH proje şablonu depo URL'si* olan:

***SSH: / /\<Azure DevOps Hizmetleri adı\>\@\<Azure DevOps Hizmetleri adı\>.visualstudio.com:22/\<proje adınız > /_git/\<, proje şablonu Depo adı\>.*** 

Bu örnekte, sunuyoruz:

***ssh://mysamplegroup\@mysamplegroup.visualstudio.com:22/MyTeam/_git/MyTeamProjectTemplate***. 

### <a name="clone-dsproject1-repository-r5-to-a-directory-d5-on-your-local-machine"></a>Yerel makinenizde bir dizin (D5) (R5) DSProject1 deposunu kopyalayın

Dizini **GitRepos**, ve, proje depoyu yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın. 

**Windows**
            
    git clone <the HTTPS URL of the Project repository>

![9](./media/project-lead-tasks/project-leads-9-clone-project-repository.png)

Azure DevOps kod barındırma platformu olarak genellikle kullanıyorsanız _proje depo HTTPS URL'si_ olduğu ***https://\<Azure DevOps Hizmetleri adı\>.visualstudio.com/\<Proje adınız > /_git/ < proje depo adınızı\>***. Bu örnekte, sahip olduğumuz ***https://mysamplegroup.visualstudio.com/MyTeam/_git/DSProject1***.

**Linux**

    git clone <the SSH URL of the Project repository>

![10](./media/project-lead-tasks/project-leads-10-clone-project-repository-linux.png)

Azure DevOps genellikle kod barındırma platformu olarak kullanıyorsanız, _SSH proje depo URL'si_ _ssh olduğu: / / < Azure DevOps Hizmetleri adı\>@< Azure DevOps Hizmetleri adı\>.visualstudio.com:22/ < proje adınız\>/\_git / < proje depo adınızı\>. Bu örnekte, sahip olduğumuz ***ssh://mysamplegroup\@mysamplegroup.visualstudio.com:22/MyTeam/_git/DSProject1***.

### <a name="copy-contents-of-d3-to-d5"></a>D5 için D3 içeriğini kopyalayın 

Artık yerel makinenize içeriğini kopyalayın gerekiyor. _D3_ için _D5_, dışındaki git meta verilerde .git dizini. Aşağıdaki komut, işi yapar. Dizinleri doğru ve tam yollarını yazın emin olun. Ekibiniz için bir kaynak klasörü olduğu (_D3_); projeniz için bir hedef klasör olduğu (_D5_).    

**Windows**
    
    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_win.ps1" -outfile "tdsp_local_copy_win.ps1"
    .\tdsp_local_copy_win.ps1 -role 3
    
![11](./media/project-lead-tasks/project-leads-11-local-copy-project-lead-new.png)

Şimdi, gördüğünüz _DSProject1_ klasöründe (.git hariç) tüm dosyalar gelen kopyalanır _MyTeamProjectTemplate_.

![12](./media/project-lead-tasks/project-leads-12-teamprojectTemplate_copied_to_local.png)

**Linux**
            
    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_linux.sh"
    bash tdsp_local_copy_linux.sh 3
        
![13](./media/project-lead-tasks/project-leads-13-local_copy_project_lead_linux_new.png)

Şimdi, gördüğünüz _DSProject1_ klasöründe (.git meta verilerde) dışındaki tüm dosyaları gelen kopyalanır _MyTeamProjectTemplate_.

![14](./media/project-lead-tasks/project-leads-14-teamprojectTemplate_copied_to_local_linux_new.png)


### <a name="customize-d5-if-you-need-to-optional"></a>(İsteğe bağlı) gerekiyorsa D5 Özelleştirme

Projenize bazı belirli dizinleri veya belgeler gerekiyorsa (önceki adımda D5 dizinine kopyalanır), proje şablonundan alma olanlar dışındaki D5 içeriği artık özelleştirebilirsiniz. 

### <a name="add-contents-of-dsproject1-in-d5-to-r5-on-your-group-azure-devops-services"></a>DSProject1 içeriğini R5 Grup Azure DevOps hizmetlerinizi şirket içinde D5 ekleyin

Şimdi içeriği itin gerekir **_DSProject1_** için _R5_ grubunuzun Azure DevOps Services projenizde depo. 


- Dizinine değiştirin **D5**. 
- İçeriği eklemek için aşağıdaki git komutlarını kullanmak **D5** için **R5**. Komutlar, hem Windows hem de Linux sistemleri için aynıdır. 
    
    Git durumu git ekleyin.
    Git commit -m "win DSVM gelen anında iletme" git gönderimi
    
- Anında iletme ve değişikliği işleyin. 

> [!NOTE]
> Bu bir Git deposuna işleme ilk kez kullanıyorsanız, genel parametrelerini yapılandırmanıza gerek *user.name* ve *user.email* çalıştırmadan önce `git commit` komutu. Aşağıdaki iki komutu çalıştırın:
        
    git config --global user.name <your name>
    git config --global user.email <your email address>
 
> Birden çok Git deposu için yürütülmekte olan, bunların tümünün arasında aynı adı ve e-posta adresi kullanın. Git etkinliklerinizi birden çok depolara göre izlemek için Power BI panoları oluşturduğunuzda aynı ad ve e-posta adresi kullanarak daha sonra uygun kanıtlar.

![15](./media/project-lead-tasks/project-leads-15-git-config-name.png)


## <a name="6-create-and-mount-azure-file-storage-as-project-resources-optional"></a>6. Oluşturma ve (isteğe bağlı) project kaynakları olarak Azure dosya depolama bağlama

Azure dosya depolama, veri paylaşımı oluşturmak istiyorsanız, proje gibi ham veriler veya projeniz için tüm proje üyeleri aynı veri kümelerine erişim, birden çok Dsvm'leri sahiptir. böylece oluşturulan özellikler 3 ve 4. Bölüm'ndaki yönergeleri izleyin [ Müşteri adayı görev bir veri bilimi takım için takım](team-lead-tasks.md). 


## <a name="next-steps"></a>Sonraki adımlar

Team Data Science Process tarafından tanımlanan görevleri ve rolleri ayrıntılı açıklamaları için bağlantılar şunlardır:

- [Bir veri bilimi takım için Grup yöneticisi görevleri](group-manager-tasks.md)
- [Bir veri bilimi takım için takım sağlama görevleri](team-lead-tasks.md)
- [Proje için bir veri bilimi ekibi müşteri adayı görevleri](project-lead-tasks.md)
- [Bir veri bilimi takım için proje bağımsız katılımcıları](project-ic-tasks.md)
