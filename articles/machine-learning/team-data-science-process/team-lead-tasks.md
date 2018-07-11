---
title: Görevler - Azure veri bilimi işlemi ekibine Liderlikte takım | Microsoft Docs
description: Görev bir veri bilimi takım projesi üzerinde Ekip Lideri için ana hat.
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
ms.openlocfilehash: 9d2043808cbd61d5e2a69cbe0f2a5a611e3afa31
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "34839767"
---
# <a name="team-lead-tasks"></a>Ekip Lideri görevleri

Bu konuda anahatları ekip lideri olan görevleri için veri bilimi ekip tamamlanması bekleniyor. Hedefi üzerinde standartlaştırır ekip işbirliği ortamı oluşturmaktır [Team Data Science Process](overview.md) (TDSP). TDSP Tahmine dayalı analiz çözümlerini ve akıllı uygulamaları etkili bir şekilde sunmak için bir Çevik, yinelemeli bir veri bilimi metodolojisidir ' dir. İşbirliği ve takım öğrenme geliştirmeye yardımcı olmak için tasarlanmıştır. Yapıları yönelik geliştirdikleri başarılı şirketler tamamen yardımcı olmak için veri bilimi girişimlerini gerekli sektörün yanı sıra hem Microsoft'un kendi analiz programlarının avantajlarından ve en iyi bir distillation işlemidir. Bu işlemi, standart personel rolleri ve veri bilimi ekibi tarafından işlenen ilişkilendirilen görevlerinin bir özetini görmek [Team Data Science Process rolleri ve görevleri](roles-tasks.md).

A **ekibine Liderlikte** kurumsal bir veri bilimi birimindeki ekibi yönetir. Takım birden çok veri bilimcileri oluşur. Yalnızca az sayıda veri bilimcileri, veri bilimi birimle için **Grup Yöneticisi** ve **ekibine Liderlikte** aynı kişiye olabilir veya kendi görev için bir yedek temsilci seçebilecek. Ancak görevlerin kendileri değiştirmeyin. Bu ortamı ayarlamak için takım liderleri tarafından tamamlanacak görevler için iş akışını aşağıdaki şekilde gösterilen:

![1](./media/team-lead-tasks/team-leads-1-creating-teams.png)

>[AZURE.NOTE] 1. ve 2 segmentini blokları görevleri barındırma platformu kod olarak Visual Studio Team Services (VSTS) kullanıyorsanız ve kendi ekibiniz için ayrı bir takım projesi istediğiniz gereklidir. Bu görevler tamamlandıktan sonra ekibinizin tüm depolar bu takım projesi altında oluşturulabilir. 

Aşağıdaki bölümde belirttiğiniz görev grubu yöneticisi tarafından karşılanan birkaç önkoşul sonra beş asıl görevler vardır (bazı isteğe bağlı) Bu öğreticinin tamamlanması. Bu görevler, bu konu başlığının bölümleri numaralı ana karşılık gelir:

1. Oluşturma bir **takım projesi** gruba ve projedeki iki takım depolarını grubun VSTS sunucusunda:
    - **ProjectTemplate depo** 
    - **TeamUtilities depo**
2. Takım temel **ProjectTemplate** depodan **GroupProjectTemplate** grubu yöneticiniz tarafından ayarlanan deposu. 
3. Takım veri ve analiz kaynaklarını oluşturun:
    - Takım özgü yardımcı programları ekleme **TeamUtilities** depo. 
    - (İsteğe bağlı) Oluşturma bir **Azure dosya depolama** takımın tamamı için yararlı olabilecek veri varlıkları depolamak için kullanılacak. 
4. (İsteğe bağlı) Azure dosya depolama bağlama **veri bilimi sanal makinesi** (DSVM) takım sağlama ve veri varlıklarını ekleyin.
5. Ayarlanan **güvenlik denetimi** göre takım üyeleri ekleme ve kendi ayrıcalıklarını yapılandırın.

>[AZURE.NOTE] VSTS kullanarak aşağıdaki yönergelerde yer TDSP takım ortamını ayarlamak için gerekli olan adımları genel çizgileriyle belirtin. Biz nasıl biz Microsoft'ta TDSP uygulama olduğu için bu görevleri VSTS ile nasıl yapılacağını belirtin. Grubunuz için başka bir kod barındırma platformu kullanılıyorsa, ekip lideri tarafından genellikle tamamlanması gereken görevler değiştirmeyin. Ancak bu görevleri tamamlamak için yol farklı zordur.

## <a name="repositories-and-directories"></a>Depoları ve dizinler

Bu konuda, kısaltılmış depoları ve dizinler için kullanılır. Bu adlar dizinlerini ve depoları işlemleri izlemenizi kolaylaştırır. Bu gösterim (**R** Git depoları ve **D** DSVM'ye yerel dizinler için) aşağıdaki bölümlerde kullanılır:

- **R1**: **GroupProjectTemplate** VSTS grubu sunucunuzda, Grup Yöneticisi ayarladığınız Git deposunda.
- **R3**: Takım **ProjectTemplate** ayarladığınız Git deposunda.
- **R4**: **TeamUtilities** ayarladığınız Git deposunda.
- **D1**: yerel dizin R1 kopyalandı ve D3 için kopyalanır.
- **D3**: yerel dizin R3 ' kopyalanabilir, özelleştirme ve R3 için kopyalanır.
- **D4**: yerel dizin R4 kopyalanan, özelleştirme ve yeniden R4 için kopyalanır.

Bu öğreticide depolar ve dizin için belirtilen adlar hedefiniz daha büyük bir veri bilimi grup içinde kendi ekibiniz için ayrı bir takım projesi oluşturmaktır varsayımına üzerinde sağlanmıştır. Ancak, ekip lideri açık diğer seçenekleri mevcuttur:

- Grubun tamamını tek bir takım projesi oluşturmayı seçebilirsiniz. Ardından tüm veri bilimi ekipleri tüm projelerden bu tek takım projesi altında olacaktır. Bunu başarmak için tek bir takım projesi oluşturmak için bu yönergeleri izlemek için bir git yönetici belirleyebilirsiniz. Bu senaryo, örneğin geçerli olabilir:
    -  birden çok veri bilimi ekipleri sahip olmayan bir küçük veri bilimi grubu 
    -  yine de grup düzeyinde sprint planlama gibi etkinliklerle arası işbirliğini en iyi duruma getirmek istediği daha büyük bir veri bilimi Grup birden çok veri bilimi ekipleri ile. 
- Takımlar team özel proje şablonları veya ekibe özgü yardımcı programlar tüm grup için tek bir takım projesi altındaki sahip olmayı seçebilirsiniz. Bu durumda, takım liderleri, takım proje şablonu depolarını ve/veya takım yardımcı programları depolarını aynı takım projesi altında oluşturmanız gerekir. Bu depolar ad *< TeamName\>ProjectTemplate* ve *< TeamName\>yardımcı programları*, örneğin, *TeamJohnProjectTemplate*ve *TeamJohnUtilities*. 

Herhangi bir durumda, takım liderleri takım üyelerinin hangi şablon ve yardımcı programlar depolar ayarlama ve proje ve yardımcı programlar depoların kopyalanması benimsemeniz bilmeniz bildirmeniz gerekir. Proje liderleri izlemelidir [bir veri bilimi takım projesi neden görevlerde](project-lead-tasks.md) projesi depolarını, tek bir takım projesi altında ayrı takım projelerine olup olmadığını veya oluşturmak için. 


## <a name="0-prerequisites"></a>0. Önkoşullar

Özetlenen, Grup Yöneticisi atanan görevlerin tamamlayarak önkoşullara uyduğunuzdan [grup yöneticisi görevleri için bir veri bilimi ekip](group-manager-tasks.md). Burada özetlemek gerekirse, aşağıdaki gereksinimleri ekip sağlama görevlerini başlamadan önce yerine getirmeniz gerekir: 

- **Grubu VSTS sunucusu** (veya barındırma platformu başka bir kod grubu hesabı), grup yöneticisi tarafından ayarlanmış.
- **GroupProjectTemplate depo** (R1) ayarlanmış grubu hesabınızdaki barındırma platformu kullanmayı planladığınız kod, grup yöneticisi tarafından.
- Size verilmiş olması **yetkili** grubu hesabınızda ekibiniz için depo oluşturun.
- Git makinenizde yüklü olması gerekir. Bir veri bilimi sanal makinesi (DSVM) kullanıyorsanız, Git önceden yüklenmiş ve hazırsınız demektir. Aksi takdirde bkz [platformları ve araçlarıyla ek](platforms-and-tools.md#appendix).  
- Kullanıyorsanız bir **Windows DSVM**, ihtiyacınız [Git Credential Manager'ı (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) makinenizde yüklü. README.md dosyasında doğru aşağı kaydırın **indirme ve yükleme** tıklayın ve bölüm *en son yükleyicisi*. Bu en son yükleyici sayfasına götürür. .Exe yükleyiciyi buradan indirin ve çalıştırın. 
- Kullanıyorsanız **Linux DSVM'sini**, bir SSH ortak anahtarı üzerinde DSVM oluşturma ve Grup VSTS sunucunuza ekleyin. SSH hakkında daha fazla bilgi için bkz: **oluşturma SSH ortak anahtarı** konusundaki [platformları ve araçlarıyla ek](platforms-and-tools.md#appendix). 
    
## <a name="1-create-a-team-project-and-repositories"></a>1. Bir takım projesini ve depo oluşturma

VSTS, kodunuzu barındırma sürüm oluşturma ve işbirliği platformu olarak kullanıyorsanız bu adımı tamamlayın. Bu bölümde, grubunuzun VSTS Server'da üç yapıları oluşturma bulunur:

- **MyTeam** vsts'de proje
- **MyProjectTemplate** depo (**R3**) üzerinde Git
- **MyTeamUtilities** depo (**R4**) üzerinde Git

### <a name="create-the-myteam-project"></a>MyTeam projesi oluşturma

- URL'de grubunuzun VSTS sunucusu giriş sayfası Git `https://<VSTS Server Name\>.visualstudio.com`. 
- Tıklayın **yeni** bir takım projesi oluşturmak için. 

    ![2](./media/team-lead-tasks/team-leads-2-create-new-team.png)

- Bir oluşturma takım proje penceresi isteyen proje adı girin (**MyTeam** Bu örnekte). Seçtiğinizden emin olun **Çevik** olarak **işlem şablonu** ve **Git** olarak **sürüm denetimi**. 

    ![3](./media/team-lead-tasks/team-leads-3-create-new-team-2.png)

- Tıklayın **proje oluştur**. Takım projeniz **MyTeam** 1 dakikadan az oluşturulur. 

- Takım projesi sonra **MyTeam** olan oluşturulan tıklayın **proje Git** düğme, takım projenizin giriş sayfasına yeniden yönlendirilmesi için. 

    ![4](./media/team-lead-tasks/team-leads-4-create-new-team-3.png)

- Görürseniz bir **Tebrikler!** açılan pencere tıklayın **kod ekleme** (kırmızı kutu içinde düğme). ' A tıklayıp **kod** (kutusunda sarı). Bu takım projenizin Git deposu sayfasına yönlendirir. 

    ![5](./media/team-lead-tasks/team-leads-5-team-project-home.png)

### <a name="create-the-myprojecttemplate-repository-r3-on-git"></a>Git (R3) MyProjectTemplate depo oluşturma

- Takım projenizin Git deposu sayfasında depo adının yanındaki aşağı oku **MyTeam**seçip **depoları Yönet...** .

    ![6](./media/team-lead-tasks/team-leads-6-rename-team-project-repo.png)

- Üzerinde **sürüm denetimi** sekmesi, takım projenizi Denetim Masası ' nı **MyTeam**, ardından **depoyu yeniden adlandır...** . 

    ![7](./media/team-lead-tasks/team-leads-7-rename-team-project-repo-2.png)

- Depoya yeni bir ad giriş **MyTeam depoyu yeniden adlandırabilir** penceresi. Bu örnekte, *MyTeamProjectTemplate*. Benzer bir şey seçebileceğiniz *< takım adınızı\>ProjectTemplate*. Tıklayın **Yeniden Adlandır** devam etmek için.

    ![8](./media/team-lead-tasks/team-leads-8-rename-team-project-repo-3.png)

### <a name="create-the-myteamutilities-repository-r4-on-git"></a>Git (R4) MyTeamUtilities depo oluşturma

- Yeni bir havuz oluşturmak için *< takım adınızı\>yardımcı programları* , takım projesi altında tıklayın **yeni havuz...**  üzerinde **sürüm denetimi** ekip projenizin Denetim Masası sekmesinde.  

    ![9](./media/team-lead-tasks/team-leads-9-create-team-utilities.png)

- İçinde **yeni depo Oluştur** , açılır penceresi bu depo için bir ad belirtin. Bu örnekte biz olarak adlandırın *MyTeamUtilities*, olduğu **R4** bizim gösterimde. Aşağıdaki gibi seçin *< takım adınızı\>yardımcı programları*. Seçtiğinizden emin olun **Git** için **türü**. ' A tıklayarak **Oluştur** devam etmek için.

    ![10](./media/team-lead-tasks/team-leads-10-create-team-utilities-2.png)

- İki yeni Git depoları, takım projesi altında oluşturulan gördüğünüzü onaylayın **MyTeam**. Bu örnekte: 

- **MyTeamProjectTemplate** (R3) 
- **MyTeamUtilities** (R4).

    ![11](./media/team-lead-tasks/team-leads-11-two-repo-in-team.png)


## <a name="2-seed-your-team-projecttemplate-and-teamutilities-repositories"></a>2. Takım ProjectTemplate ve TeamUtilities depolarını temel

Dengeli dağıtım yordam dizinleri yerel DSVM'ye Ara hazırlama siteler olarak kullanır. Özelleştirmeniz gerekirse, **ProjectTemplate** ve **TeamUtilities** depoları bazı belirli karşılamak için ekip ihtiyaçlarını, aşağıdaki yordamın sondan adımında bunu. İçeriği sağlamak için kullanılan adımlarla bir özeti aşağıda verilmiştir **MyTeamProjectTemplate** ve **MyTeamUtilities** bir veri bilimi team için depoları. Her bir adımı alt bölümlere dengeli dağıtım yordamda karşılık gelir:

- Yerel dizine grubu depoyu Kopyala: yerel D1 -> için kopyalanan R1 - takım
- Yerel dizine takım depoları kopyalayarak: R3 & yerel D3 & D4 -> için kopyalanan R4 - takım
- Grup proje şablonu içeriğini team yerel klasöre kopyalayın: D1 - D3 -> için kopyalanan içeriği
- Yerel D3 & D4 özelleştirme (isteğe bağlı)
- Yerel dizin içeriğini team depolara itme: D3 & D4 - içeriğini Ekle -> için R3 & R4 takım


### <a name="initialize-the-team-repositories"></a>Takım depolarını Başlat

Bu adımda, takım projesi şablonu deponuzu grubu proje şablonu depodan Başlat:

- **MyTeamProjectTemplate** depo (**R3**) öğesinden, **GroupProjectTemplate** (**R1**) depo


### <a name="clone-group-repositories-into-local-directories"></a>Yerel dizine grubu depoları kopyalayın

Bu yordama başlamadan için:

- Yerel makinenizde dizinleri oluşturun:
    - İçin **Windows**: **C:\GitRepos\GroupCommon** ve **C:\GitRepos\MyTeam**
    - İçin **Linux**: **GitRepos\GroupCommon** ve **GitRepos\MyTeam** giriş dizininize üzerinde 
- Dizinine değiştirin **GitRepos\GroupCommon**.
- Yerel makinenize işletim sistemi üzerinde uygun şekilde aşağıdaki komutu çalıştırın.

**Windows**

    git clone https://<Your VSTS Server name>.visualstudio.com/GroupCommon/_git/GroupProjectTemplate
    

![12](./media/team-lead-tasks/team-leads-12-create-two-group-repos.png)

**Linux**
    
    git clone ssh://<Your VSTS Server name>@<Your VSTS Server name>.visualstudio.com:22/GroupCommon/_git/GroupProjectTemplate
    
    
![13](./media/team-lead-tasks/team-leads-13-clone_two_group_repos_linux.png)

Bu komutlar kopyalama, **GroupProjectTemplate** (R1) depo yerel dizinde için Grup VSTS sunucunuzdaki **GitRepos\GroupCommon** yerel makinenizde. Kopyalama, dizin sonra **GroupProjectTemplate** (D1) dizininde oluşturulan **GitRepos\GroupCommon**. Burada, Grup Yöneticisi bir ekip projesi oluşturduktan varsayıyoruz **GroupCommon**ve **GroupProjectTemplate** bu takım projesi altında bir depodur. 


### <a name="clone-your-team-repositories-into-local-directories"></a>Takım depolarınızı yerel dizine kopyalama

Bu komutlar kopyalama, **MyTeamProjectTemplate** (R3) ve **MyTeamUtilities** (R4) depoları ekip projesi altındaki **MyTeam** içinGrupVSTSsunucunuzdaki **MyTeamProjectTemplate** (D3) ve **MyTeamUtilities** (D4) dizinlerde **GitRepos\MyTeam** yerel makinenizde. 

- Dizinine değiştirin **GitRepos\MyTeam**
- Yerel makinenize işletim sistemi üzerinde uygun olarak aşağıdaki komutları çalıştırın. 

**Windows**

    git clone https://<Your VSTS Server name>.visualstudio.com/<Your Team Name>/_git/MyTeamProjectTemplate
    git clone https://<Your VSTS Server name>.visualstudio.com/<Your Team Name>/_git/MyTeamUtilities

![14](./media/team-lead-tasks/team-leads-14-clone_two_empty_team_repos.png)
        
**Linux**
    
    git clone ssh://<Your VSTS Server name>@<Your VSTS Server name>.visualstudio.com:22/<Your Team Name>/_git/MyTeamProjectTemplate
    git clone ssh://<Your VSTS Server name>@<Your VSTS Server name>.visualstudio.com:22/<Your Team Name>/_git/MyTeamUtilities
    
![15](./media/team-lead-tasks/team-leads-15-clone_two_empty_team_repos_linux.png)

Kopyalama, iki dizin sonra **MyTeamProjectTemplate** (D3) ve **MyTeamUtilities** (D4) dizininde oluşturulur **GitRepos\MyTeam**. Biz Burada şablon ve yardımcı programlar depoları, takım projenizi adlı kabul **MyTeamProjectTemplate** ve **MyTeamUtilities**. 

### <a name="copy-the-group-project-template-content-to-the-local-team-project-template-directory"></a>Grup proje şablonu içeriğini yerel bir takım proje şablonu dizinine kopyalayın

Yerel içeriği kopyalamak için **GroupProjectTemplate** yerel klasöre (D1) **MyTeamProjectTemplate** (D3), aşağıdaki Kabuk betikleri birini çalıştırın: 

#### <a name="from-the-powershell-command-line-for-windows"></a>PowerShell için Windows komut satırı       

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_win.ps1" -outfile "tdsp_local_copy_win.ps1"
    .\tdsp_local_copy_win.ps1 2

    
![16](./media/team-lead-tasks/team-leads-16-local_copy_team_lead_new.png)

#### <a name="from-the-linux-shell-for-the-linux-dsvm"></a>İçin Linux kabuğundan **Linux DSVM'sini**
    
    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_linux.sh"
    bash tdsp_local_copy_linux.sh 2
    
![17](./media/team-lead-tasks/team-leads-17-local-copy-team-lead-linux-new.png)

Betikleri .git dizini içeriğini hariç tutun. Betikleri sağlamak ister **tamamlamak yolları** kaynak dizini D1 ve hedef dizin D3.
        

### <a name="customize-your-team-project-template-or-team-utilities-optional"></a>Takım proje şablonu veya takım hizmet programları (isteğe bağlı) özelleştirme

Özelleştirme, **MyTeamProjectTemplate** (D3) ve **MyTeamUtilities** (gerekirse, Kurulum işlemi bu aşamada D4). 

- Ekibinizin belirli gereksinimlerini karşılayacak şekilde D3 içeriğini özelleştirmek istiyorsanız, şablon belgeleri değiştirin ya da dizin yapısını değiştirebilirsiniz.

- Ekibiniz tüm takımınızla paylaşmak istediğiniz bazı yardımcı programlar geliştirdiyseniz, kopyalayın ve bu yardımcı programlar D4 dizinine yapıştırın. 


### <a name="push-local-directory-content-to-team-repositories"></a>Yerel dizin içeriğini team depolara itme

İçeriği (isteğe bağlı olarak özelleştirilmiş) yerel dizinler D3 ve D4 R3 ve R4 takım depolarını eklemek için bir Windows PowerShell konsolundan ya da Linux kabuğundan aşağıdaki git bash komutları çalıştırın. Komutları çalıştırmak **GitRepos\MyTeam\MyTeamProjectTemplate** dizin.

    git status
    git add .
    git commit -m"push from DSVM"
    git push
    
![18](./media/team-lead-tasks/team-leads-18-push-to-group-server-2.png)

Bu betik çalıştırıldığında grubunuzun VSTS sunucusunun MyTeamProjectTemplate deposundaki dosyaları neredeyse anında eşitlenir.

![19](./media/team-lead-tasks/team-leads-19-push-to-group-server-showed-up.png)

Şimdi aynı dört git komutları kümesini çalıştırmak **GitRepos\MyTeam\MyTeamUtilities** dizin. 

> [AZURE.NOTE]Bu bir Git deposuna işleme ilk kez kullanıyorsanız, genel parametrelerini yapılandırmanıza gerek *user.name* ve *user.email* çalıştırmadan önce `git commit` komutu. Aşağıdaki iki komutu çalıştırın:
        
    git config --global user.name <your name>
    git config --global user.email <your email address>
 
> Birden çok Git deposu için yürütülmekte olan, her birine işlemesini oluştururken aynı ad ve e-posta adresi kullanın. Git etkinliklerinizi birden çok depolara göre izlemek için Power BI panoları oluşturduğunuzda aynı ad ve e-posta adresi kullanarak daha sonra uygun kanıtlar.

![20](./media/team-lead-tasks/team-leads-20-git-config-name.png)


## <a name="3-create-team-data-and-analytics-resources-optional"></a>3. Takım veri ve analiz kaynaklarını (isteğe bağlı) oluşturma

Veri ve analiz kaynakları tüm takımınızla paylaşma performans ve maliyet avantajları vardır: takım üyelerinin projelerini paylaşılan kaynaklar üzerinde yürütmek, bütçeleriyle ilgili kaydetme ve daha verimli bir şekilde işbirliği yapın. Bu bölümde, Azure dosya depolama oluşturma hakkında yönergeler sağlarız. Sonraki bölümde, Azure dosya depolama bağlama yerel makinenize nasıl konusunda yönerge sağlar. Azure HDInsight Spark kümeleri, gibi Azure veri bilimi sanal makineleri, diğer kaynakları paylaşma hakkında daha fazla bilgi için bkz [platformları ve araçlarıyla](platforms-and-tools.md). Bu konu, gereksinimleriniz için uygun olan kaynaklara seçme konusunda bir veri bilimi açısından rehberlik sağlar ve ürün sayfaları ve yayımladık diğer ilgili ve kullanışlı öğreticileri için bağlar.

>[AZURE.NOTE] Veri gönderme çapraz önlemek için yavaş ve pahalı olabilir, veri merkezleri, kaynak grubu, depolama hesabı ve Azure VM (örneğin, DSVM) aynı Azure veri merkezinde olduğundan emin olun. 

Ekibiniz için Azure dosya depolama alanı oluşturmak için aşağıdaki komut dosyasını çalıştırın. Ekibiniz için Azure dosya depolama, takımınız için kullanışlı olan veri varlıkları depolamak için kullanılabilir. Komut dosyaları ister Azure hesabı ve abonelik bilgilerinizi için bu nedenle bu kimlik bilgilerini girmek için hazır olması. 

### <a name="create-azure-file-storage-with-powershell-from-windows"></a>Azure dosya depolama ile Windows Powershell'den oluşturma

Bu betik, komut satırı bir PowerShell üzerinden çalıştırın:

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/CreateFileShare.ps1" -outfile "CreateFileShare.ps1"
    .\CreateFileShare.ps1

![21](./media/team-lead-tasks/team-leads-21-create-fileshare-win.png)   

İstendiğinde Microsoft Azure hesabınızda oturum açın:

![22](./media/team-lead-tasks/team-leads-22-file-create-s1.png)

Kullanmak istediğiniz Azure aboneliğini seçin:

![23](./media/team-lead-tasks/team-leads-23-file-create-s2.png)

Veya, seçili abonelik altında yeni bir depolama hesabını seçin:

![24](./media/team-lead-tasks/team-leads-24-file-create-s3.png)

Azure dosya depolama alanı oluşturmak için adını girin. Düşük yalnızca büyük küçük harf karakterler, sayılar ve - kabul edilir:

![25](./media/team-lead-tasks/team-leads-25-file-create-s4.png)

Bağlama ve oluşturulduktan sonra bu depolama paylaşımını kolaylaştırmak için Azure dosya depolama bilgileri bir metin dosyasına kaydedin ve konumuna yolunu not edin. Özellikle, bu dosya, sonraki bölümde, Azure sanal makinelerine Azure dosya depolama alanınızı bağlamak gerekir. 

Bu metin dosyasını takım ProjectTemplate deponuza iade etmek için iyi bir uygulamadır. Dizinine koymak için önerdiğimiz **Docs\DataDictionaries**. Bu nedenle, bu veri varlığını, takımınızdaki tüm projeleri tarafından erişilebilir. 

![26](./media/team-lead-tasks/team-leads-26-file-create-s5.png)


### <a name="create-azure-file-storage-with-a-linux-script"></a>Azure dosya depolama ile bir Linux komut dosyası oluşturma

Bu betik, Linux kabuğundan çalıştırın:

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/CreateFileShare.sh"
    bash CreateFileShare.sh

Bu ekrandaki yönergeleri izleyerek Microsoft Azure hesabınızda oturum açın:

![27](./media/team-lead-tasks/team-leads-27-file-create-linux-s1.png)

Kullanmak istediğiniz Azure aboneliğini seçin:

![28](./media/team-lead-tasks/team-leads-28-file-create-linux-s2.png)

Veya, seçili abonelik altında yeni bir depolama hesabını seçin:

![29](./media/team-lead-tasks/team-leads-29-file-create-linux-s3.png)

Oluşturmak için Azure dosya depolama, yalnızca küçük harf karakter, sayı adını girin ve - kabul edilir:

![30](./media/team-lead-tasks/team-leads-30-file-create-linux-s4.png)

Oluşturulduktan sonra bu depolama alanına erişilirken kolaylaştırmak için Azure dosya depolama bilgileri bir metin dosyasına kaydedin ve konumuna yolunu not edin. Özellikle, bu dosya, sonraki bölümde, Azure sanal makinelerine Azure dosya depolama alanınızı bağlamak gerekir.

Bu metin dosyasını takım ProjectTemplate deponuza iade etmek için iyi bir uygulamadır. Dizinine koymak için önerdiğimiz **Docs\DataDictionaries**. Bu nedenle, bu veri varlığını, takımınızdaki tüm projeleri tarafından erişilebilir. 

![31](./media/team-lead-tasks/team-leads-31-file-create-linux-s5.png)


## <a name="4-mount-azure-file-storage-optional"></a>4. Azure dosya depolama bağlama (isteğe bağlı)

Azure dosya depolama başarıyla oluşturulduktan sonra aşağıdaki PowerShell ya da Linux komut dosyalarından birini kullanarak yerel makinenize bağlanabilir.

### <a name="mount-azure-file-storage-with-powershell-from-windows"></a>Azure dosya depolama bağlama ile Windows Powershell'den

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/AttachFileShare.ps1" -outfile "AttachFileShare.ps1"
    .\AttachFileShare.ps1
    
Açtıktan değil ise, ilk oturum istenir. 

Tıklayın **Enter** veya **y** dosyasını açın ve ardından Giriş bir Azure dosya depolama bilgileri varsa istendiğinde devam etmek için ***tamamlamak yolu ve adı** oluşturduğunuz dosyanın önceki adımı. Bir Azure dosya depolama bağlama bilgileri, dosya ve sonraki adıma geçmek hazır olduğunu doğrudan okunur.

![32](./media/team-lead-tasks/team-leads-32-attach-s1.png)

> [AZURE.NOTE] Azure dosya depolama bilgilerini içeren bir dosya yoksa, bu bölümün sonunda bilgileri klavye girişi için adımları sağlanır.

Ardından, sanal makineye eklenecek sürücü adını girmeniz istenir. Varolan sürücü adlarının bir listesini, ekranda yazdırılır. Zaten listede var olmayan bir sürücü adı sağlamanız gerekir.

![33](./media/team-lead-tasks/team-leads-33-attach-s2.png)

Yeni bir F sürücü makinenize başarıyla bağlı olduğunu onaylayın.

![34](./media/team-lead-tasks/team-leads-34-attach-s3.png)

**Azure dosya depolama bilgilerini el ile girmek nasıl:** bir metin dosyasını Azure dosya depolama bilgilerinizi yoksa, gerekli olan abonelik, depolama hesabı ve Azure yazmak için aşağıdaki ekranda yönergeleri izleyebilirsiniz dosya depolama bilgileri:

![35](./media/team-lead-tasks/team-leads-35-attach-s4.png)

Tür adı'nda depolama hesabı Azure dosya depolama oluşturulduğu, Azure aboneliği seçin ve Azure dosya depolama adını yazın:

![36](./media/team-lead-tasks/team-leads-36-attach-s5.png)

### <a name="mount-azure-file-storage-with-a-linux-script"></a>Azure dosya depolama bağlama ile bir Linux betiği

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/AttachFileShare.sh"
    bash AttachFileShare.sh

![37](./media/team-lead-tasks/team-leads-37-attach-s1-linux.png)

Açtıktan değil ise, ilk oturum istenir. 

Tıklayın **Enter** veya **y** dosyasını açın ve ardından Giriş bir Azure dosya depolama bilgileri varsa istendiğinde devam etmek için ***tamamlamak yolu ve adı** oluşturduğunuz dosyanın önceki adımı. Bir Azure dosya depolama bağlama bilgileri, dosya ve sonraki adıma geçmek hazır olduğunu doğrudan okunur.

![38](./media/team-lead-tasks/team-leads-38-attach-s2-linux.png)

Ardından, sanal makineye eklenecek sürücü adını girmeniz istenir. Varolan sürücü adlarının bir listesini, ekranda yazdırılır. Zaten listede var olmayan bir sürücü adı sağlamanız gerekir.

![39](./media/team-lead-tasks/team-leads-39-attach-s3-linux.png)

Yeni bir F sürücü makinenize başarıyla bağlı olduğunu onaylayın.

![40](./media/team-lead-tasks/team-leads-40-attach-s4-linux.png)

**Azure dosya depolama bilgilerini el ile girmek nasıl:** bir metin dosyasını Azure dosya depolama bilgilerinizi yoksa, gerekli olan abonelik, depolama hesabı ve Azure yazmak için aşağıdaki ekranda yönergeleri izleyebilirsiniz dosya depolama bilgileri:

- Giriş **n**.
- Azure dosya depolama önceki adımda oluşturulduğu abonelik adı dizinini seçin:

    ![41](./media/team-lead-tasks/team-leads-41-attach-s5-linux.png)

- Depolama hesabı türü ve abonelik altında Azure dosya depolama adını seçin:

    ![42](./media/team-lead-tasks/team-leads-42-attach-s6-linux.png)

- Tüm mevcut olanlardan farklı olmalıdır makinenize eklenecek sürücü adını girin:

    ![43](./media/team-lead-tasks/team-leads-43-attach-s7-linux.png)


## <a name="5-set-up-security-control-policy"></a>5. Güvenlik denetimi İlkesi ayarlama 

Grup VSTS sunucunun ana sayfadan tıklayın **dişli simgesini** sağ üst köşesinde kullanıcı adınızı yanında, ardından **güvenlik** sekmesi. Üyeleri takım çeşitli izinlerle buraya ekleyebilirsiniz.

![44](./media/team-lead-tasks/team-leads-44-add-team-members.png)

## <a name="next-steps"></a>Sonraki adımlar

Team Data Science Process tarafından tanımlanan görevleri ve rolleri ayrıntılı açıklamaları için bağlantılar şunlardır:

- [Bir veri bilimi takım için Grup yöneticisi görevleri](group-manager-tasks.md)
- [Bir veri bilimi takım için takım sağlama görevleri](team-lead-tasks.md)
- [Proje için bir veri bilimi ekibi müşteri adayı görevleri](project-lead-tasks.md)
- [Bir veri bilimi takım için proje bağımsız katılımcıları](project-ic-tasks.md)