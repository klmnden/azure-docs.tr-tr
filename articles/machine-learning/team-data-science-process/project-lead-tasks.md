---
title: Ekip Görevleri - Azure veri bilimi işlem proje sağlama | Microsoft Docs
description: Veri bilimi takım projesi üzerinde proje lideri görevlerde ana hattı.
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: deguhath
ms.openlocfilehash: 58c5826240b7c49ba29c0d8e86a2896e3ce2f7f7
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34838407"
---
# <a name="project-lead-tasks"></a>Proje sağlama görevleri

Bu öğretici anahatları proje lideri olan görevleri güncelleştirmesini proje ekibi tamamlanması bekleniyor. Amaç üzerinde standartlaştıran ekip işbirliği ortamı belirtmektir [takım veri bilimi işlemi](overview.md) (TDSP). TDSP etkinlikleri bulut tabanlı, Tahmine dayalı analiz çözümleri verimli bir şekilde çalıştırmak için yapılandırılmış bir dizi sağlayan Microsoft tarafından geliştirilen bir çerçevedir. Bu işlem üzerinde Standartlaştırma personel rolleri ve veri bilimi ekibi tarafından işlenen ilişkilendirilen görevlerinin bir özetini görmek [takım veri bilimi işlemi rolleri ve görevleri](roles-tasks.md).

A **proje neden** ayrı veri bilimcilerine belirli veri bilimi projedeki günlük etkinliklerini yönetir. İş akışı bu ortamı ayarlamak için Proje müşteri adayları tarafından tamamlanması gereken görevler için aşağıdaki resimde gösterilen:

![1](./media/project-lead-tasks/project-leads-1-tdsp-creating-projects.png)

Bu konu, şu anda bu iş akışı için Proje müşteri adayları, görevleri 1,2 ve 6 kapsar.

>[AZURE.NOTE] Visual Studio Team Services (VSTS) aşağıdaki yönergeleri kullanarak bir proje için bir TDSP takım ortamı kurmak için gerekli adımları ana hatlarını vermektedir. Biz, biz Microsoft'taki TDSP nasıl uygulamak olduğundan bu görevlerin VSTS ile nasıl gerçekleştirileceğini belirtin. Grubunuz için başka bir kod barındırma platformu kullanılıyorsa, ekip lideri tarafından genellikle tamamlanması gereken görevler değiştirmeyin. Ancak bu görevleri tamamlamak için yol farklı olacak.


## <a name="repositories-and-directories"></a>Depoları ve dizinler

Bu öğretici depoları ve dizinleri kısaltılmış adlarını kullanır. Bu adları depoları ve dizinler arasında işlemleri izlemek kolaylaştırır. Bu gösterim (R Git depoları için) ve D, DSVM yerel dizinleri için aşağıdaki bölümlerde kullanılır:

- **R3**: Takım **ProjectTemplate** Ekip Lideri ayarlamak Git deposunu.
- **R5**: Git projeniz için Kurulum projesi havuzda.
- **D3**: yerel dizin R3 kopyalanabilir.
- **D5**: yerel dizin R5 kopyalanabilir.


## <a name="0-prerequisites"></a>0. Önkoşullar

Özetlenen, Grup Yöneticisi atanan görevleri tamamlayarak önkoşullara uyduğunuzdan [grup yöneticisi görevleri için bir veri bilimi ekibi](group-manager-tasks.md) ve için özetlenen sağlama takım [veri bilimi ekibinEkipsağlamagörevleri](team-lead-tasks.md). 

Burada özetlemek için aşağıdaki gereksinimleri ekip sağlama görevleri başlamadan önce yerine getirmeniz gerekir: 

- **Grup VSTS sunucu** (veya grubu hesabı kodu barındıran herhangi bir platform üzerinde), grup yöneticisi tarafından ayarlanmış.
- **TeamProjectTemplate depo** (R3) ayarlanmış Grup hesabınızın altında kod barındırma platformunda kullanmayı düşünüyorsanız, ekip lideri tarafından.
- Size verilmiş olması **yetkili** depoları ekibiniz için Grup hesabınızı oluşturmak için Ekip Lideri tarafından.
- Makinenizde Git yüklenmesi gerekir. Bir veri bilimi sanal makine (DSVM) kullanıyorsanız, Git önceden yüklenmiş ve hazırsınız. Aksi takdirde bkz [platformları ve araçlarına ek](platforms-and-tools.md#appendix).  
- Kullanıyorsanız bir **Windows DSVM**, olmasına gerek [Git kimlik bilgisi Yöneticisi (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) makinenize yüklü. README.md dosyasında doğru aşağı kaydırın **yükleyip** 'ye tıklayın *son yükleyici*. Bu son yükleyici sayfasına götürür. .Exe yükleyici buradan indirin ve çalıştırın. 
- Kullanıyorsanız **Linux DSVM**, bir SSH ortak anahtarı, DSVM üzerinde oluşturun ve Grup VSTS sunucunuzu ekleyin. SSH hakkında daha fazla bilgi için bkz: **oluşturma SSH ortak anahtarı** bölümüne [platformları ve araçlarına ek](platforms-and-tools.md#appendix). 


## <a name="1-create-a-project-repository-r5"></a>1. Proje deposu (R5) oluşturma

- Oturum açtığınızda, Grup VSTS sunucuda *https://\<VSTS sunucu adı\>. visualstudio.com*. 
- Altında **son projeler & takımlar**, tıklatın **Gözat**. Açılır bir pencere VSTS sunucu üzerindeki tüm ekip projeleri listeler. 

    ![2](./media/project-lead-tasks/project-leads-2-create-project-repo.png)

- Proje deposu oluşturma olacak ekip projesi adını tıklatın. Bu örnekte **MyTeam**. 
- Ardından **Bul** takım projesi giriş sayfasına yönlendirilmesine **MyTeam**:

    ![3](./media/project-lead-tasks/project-leads-3-create-project-repo-2.png)

- Tıklatın **kodu işbirliği** takım projenizin git giriş sayfasına yönlendirilecek.  

    ![4](./media/project-lead-tasks/project-leads-4-create-project-repo-3.png)

- Sol üst köşesinde aşağı oku tıklatın ve seçin **+ yeni bir havuz**. 
    
    ![5](./media/project-lead-tasks/project-leads-5-create-project-repo-4.png)

- İçinde **yeni bir havuz oluşturma** penceresinde, proje git deposu için bir ad girin. Seçtiğinizden emin olun **Git** depo türü. Bu örnekte, kullanırız adı *DSProject1*. 

    ![6](./media/project-lead-tasks/project-leads-6-create-project-repo-5.png)

- Oluşturmak için ***DSProject1*** proje git deposu'ye tıklayın **oluşturma**.


## <a name="2-seed-the-dsproject1-project-repository"></a>2. Çekirdek DSProject1 proje deposu

Burada için çekirdek görevdir **DSProject1** takım projesi şablonu deponuza (R3) proje depodan (R5). Dengeli dağıtım yordam D3 ve D5 dizinlere, yerel DSVM üzerinde Ara hazırlama siteler olarak kullanır. Özet olarak, dengeli dağıtım yoludur: R3 -> D3 D5 -> R5 ->.

Özelleştirmeniz gerekiyorsa, **DSProject1** bazı belirli karşılamak için proje deposu proje ihtiyaçlarını, aşağıdaki yordamı sondan adımında bunu. İçeriği oluşturmak için kullanılan adımlarla bir özeti aşağıda verilmiştir **DSProject1** proje deposu. Tek tek adımları dengeli dağıtım yordamdaki alt bölümleri karşılık gelir:

- Kopya takım projesi şablonu yerel dizin depoya: klonlanmış yerel D3 -> için R3 - ekip.
- Yerel bir dizine kopya DSProject1 deposu: klonlanmış yerel D5 -> için R5 - ekip.
- Kopyalanan takım projesi şablon içeriği DSProject1 deposu yerel kopyasını kopyalayın: D3 - kopyalanır D5 -> içeriği.
- (İsteğe bağlı) Özelleştirme yerel D5.
- Takım depoları için yerel DSProject1 anında içerik: D5 - içeriği eklemek takım R5 ->.


### <a name="clone-your-team-project-template-repository-r3-to-a-directory-d3-on-your-local-machine"></a>Takım projesi şablonu deponuza (R3) yerel makinenizde bir dizin (D3) kopyalayın.

Yerel makinenizde bir dizin oluşturun:

- *C:\GitRepos\MyTeamCommon* Windows için 
- *$home/GitRepos/MyTeamCommon* Linux için

Bu dizine geçin. Ardından, takım projesi şablonu deponuza yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın. 

**Windows**
            
    git clone <the HTTPS URL of the TeamProjectTemplate repository>
    
Genellikle, kod barındırma platformu olarak VSTS kullanıyorsanız, *takım projesi şablonu deponuz HTTPS URL'sini* olan:

 ***https://\<VSTS sunucu adı\>.visualstudio.com/\<takım projenizin adına\>/_git/\<, takım projesi şablonu depo adını\>***. 

Bu örnekte, biz vardır:

***https://mysamplegroup.visualstudio.com/MyTeam/_git/MyTeamProjectTemplate***. 

![7](./media/project-lead-tasks/project-leads-7-clone-team-project-template.png)
            
**Linux**

    git clone <the SSH URL of the TeamProjectTemplate repository>
        
![8](./media/project-lead-tasks/project-leads-8-clone-team-project-template-linux.png)

Genellikle, kod barındırma platformu olarak VSTS kullanıyorsanız, *SSH takım projesi şablonu depo URL'si* değil:

***SSH: / /\<VSTS sunucu adı\>@\<VSTS sunucu adı\>.visualstudio.com:22/\<takım projesi adınız > /_git/\<takım projesi şablon depo adı \>.*** 

Bu örnekte, biz vardır:

***ssh://mysamplegroup@mysamplegroup.visualstudio.com:22/MyTeam/_git/MyTeamProjectTemplate***. 

### <a name="clone-dsproject1-repository-r5-to-a-directory-d5-on-your-local-machine"></a>Yerel makinenizde bir dizine (D5) DSProject1 depoyu (R5) kopyalayın

Değişiklik dizinine **GitRepos**, ve yerel makinenize proje deponuza kopyalamak için aşağıdaki komutu çalıştırın. 

**Windows**
            
    git clone <the HTTPS URL of the Project repository>

![9](./media/project-lead-tasks/project-leads-9-clone-project-repository.png)

VSTS kod barındırma platformu olarak genellikle kullanıyorsanız _proje depo HTTPS URL'si_ olan ***https://\<VSTS sunucu adı\>.visualstudio.com/\<bilgisayarınızı ekibi Proje adı > /_git/ < proje deposu adınızı\>***. Bu örnekte, sahibiz ***https://mysamplegroup.visualstudio.com/MyTeam/_git/DSProject1***.

**Linux**

    git clone <the SSH URL of the Project repository>

![10](./media/project-lead-tasks/project-leads-10-clone-project-repository-linux.png)

Genellikle, kod barındırma platformu olarak VSTS kullanıyorsanız, _SSH proje depo URL'si_ _ssh olduğu: / / < VSTS sunucu adı\>@< VSTS sunucu adı\>.visualstudio.com:22/<Your Team Project Name> / \_git / < proje deposu adınızı\>. Bu örnekte, sahibiz ***ssh://mysamplegroup@mysamplegroup.visualstudio.com:22/MyTeam/_git/DSProject1***.

### <a name="copy-contents-of-d3-to-d5"></a>İçin D5 D3 içeriğini kopyalayın 

Yerel makinenizde içeriğini kopyalamanız gerekir. Şimdi _D3_ için _D5_, hariç .git dizininde git meta verileri. Aşağıdaki betikler iş yapın. Dizinlere doğru ve tam yollarda yazdığınızdan emin olun. Kaynak klasördür ekibiniz için (_D3_); hedef klasördür projeniz için bir (_D5_).    

**Windows**
    
    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_win.ps1" -outfile "tdsp_local_copy_win.ps1"
    .\tdsp_local_copy_win.ps1 -role 3
    
![11](./media/project-lead-tasks/project-leads-11-local-copy-project-lead-new.png)

Görebilirsiniz artık _DSProject1_ klasöründe (.git hariç) tüm dosyaları gelen kopyalanır _MyTeamProjectTemplate_.

![12](./media/project-lead-tasks/project-leads-12-teamprojectTemplate_copied_to_local.png)

**Linux**
            
    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_linux.sh"
    bash tdsp_local_copy_linux.sh 3
        
![13](./media/project-lead-tasks/project-leads-13-local_copy_project_lead_linux_new.png)

Görebilirsiniz artık _DSProject1_ klasöründe (dışında meta verilerde .git) tüm dosyaları gelen kopyalanır _MyTeamProjectTemplate_.

![14](./media/project-lead-tasks/project-leads-14-teamprojectTemplate_copied_to_local_linux_new.png)


### <a name="customize-d5-if-you-need-to-optional"></a>(İsteğe bağlı) gerekiyorsa D5 Özelleştirme

Projenizi bazı belirli dizinleri ya da belgeler gerekirse, (D5 dizininize önceki adımda kopyaladığınız), takım projesi şablondan alma dışındaki, D5 içeriğini şimdi özelleştirebilirsiniz. 

### <a name="add-contents-of-dsproject1-in-d5-to-r5-on-your-group-vsts-server"></a>DSProject1 içeriğini içinde D5 R5 için Grup VSTS sunucunuzda ekleme

Şimdi içeriği göndermek gereken **_DSProject1_** için _R5_ takım projenizin grubunuzun VSTS sunucuda deposunda. 


- Değiştirme dizinine **D5**. 
- İçeriği eklemek için aşağıdaki git komutlarını kullanın **D5** için **R5**. Komutları, Windows ve Linux sistemleri için aynıdır. 
    
    Git durum git ekleyin.
    Git yürütme -m "gönderme temelli win DSVM gelen" git itme
    
- Değişiklik ve anında iletme uygulayın. 

>[AZURE.NOTE] Bir Git deposuna yürüttükten ilk kez kullanıyorsanız, genel parametreleri yapılandırmanız gereken *user.name* ve *user.email* çalıştırmadan önce `git commit` komutu. Aşağıdaki iki komutu çalıştırın:
        
    git config --global user.name <your name>
    git config --global user.email <your email address>
 
> Birden çok Git depoları için yürütülmekte olan, bunların tümünün arasında aynı ad ve e-posta adresi kullanın. Powerbı panolar üzerinde birden çok depoları Git etkinlikleri izlemenize oluştururken aynı ad ve e-posta adresini kullanarak daha sonra uygun kanıtlar.

![15](./media/project-lead-tasks/project-leads-15-git-config-name.png)


## <a name="6-create-and-mount-azure-file-storage-as-project-resources-optional"></a>6. Oluşturma ve Azure dosya depolama proje kaynakları (isteğe bağlı) bağlama

Veri paylaşımı için Azure dosya depolama alanı oluşturmak istiyorsanız, projenin gibi ham veriler veya tüm proje üyeleri aynı veri kümeleri için birden çok DSVMs erişimi böylece projeniz için oluşturulan özellikler bölüm 3 ve 4'ndaki yönergeleri izleyin [ Bir veri bilimi ekibi için sağlama görevleri ekip](team-lead-tasks.md). 


## <a name="next-steps"></a>Sonraki adımlar

Burada, rolleri ve görevleri takım veri bilimi işlem tarafından tanımlanan daha ayrıntılı açıklamaları bağlantıları verilmiştir:

- [Bir veri bilimi ekibi için Grup yöneticisi görevleri](group-manager-tasks.md)
- [Bir veri bilimi ekibi için takım sağlama görevleri](team-lead-tasks.md)
- [Proje veri bilimi ekibi için sağlama görevleri](project-lead-tasks.md)
- [Proje veri bilimi ekibi için tek tek Katkıda Bulunanlar](project-ic-tasks.md)