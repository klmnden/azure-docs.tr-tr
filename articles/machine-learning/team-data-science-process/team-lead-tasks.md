---
title: Ekip Görevleri - Azure veri bilimi işlem Ekip Lideri | Microsoft Docs
description: Veri bilimi takım projesi üzerinde bir takım lideri görevlerde ana hattı.
documentationcenter: ''
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: bradsev;
ms.openlocfilehash: 995ad557eb06e545b1813e1f4631e243a98830b3
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="team-lead-tasks"></a>Ekip sağlama görevleri

Bu konuda anahatları ekip lideri olan görevleri tamamlamak için kendi veri bilimi ekibi bekleniyordu. Amaç üzerinde standartlaştıran ekip işbirliği ortamı belirtmektir [takım veri bilimi işlemi](overview.md) (TDSP). TDSP Tahmine dayalı analiz çözümleri ve akıllı uygulamalar verimli bir şekilde teslim etmek için Çevik, yinelemeli veri bilimi Metodoloji ' dir. İşbirliği ve takım öğrenme geliştirmeye yardımcı olmak için tasarlanmıştır. En iyi uygulamaları distillation işlemdir ve analizi programlarını yararları yapıları her iki Microsoft şirketler tam yardımcı olmak için veri bilimi girişimleri başarılı uygulanması için gerekli sektörün yanı sıra unutmayın. Bu işlem üzerinde Standartlaştırma personel rolleri ve veri bilimi ekibi tarafından işlenen ilişkilendirilen görevlerinin bir özetini görmek [takım veri bilimi işlemi rolleri ve görevleri](roles-tasks.md).

A **Ekip Lideri** veri bilimi birimi bir grupta bir kuruluşun yönetir. Bir takım birden çok veri bilimcilerine oluşur. Az sayıda veri bilimcilerine ile veri bilimi biriminin **Grup Yöneticisi** ve **Ekip Lideri** aynı kişi olabilir veya bir yedek görevini atayabilirsiniz. Ancak görevlerin kendileri değiştirmeyin. İş akışı bu ortamı ayarlamak için takım müşteri adayları tarafından tamamlanması gereken görevler için aşağıdaki resimde gösterilen:

![1](./media/team-lead-tasks/team-leads-1-creating-teams.png)

>[AZURE.NOTE] 1 ve Şekil 2 blokları görevleri barındırma platformu kodu olarak Visual Studio Team Services (VSTS) kullanıyorsanız ve ayrı takım projesi için kendi takım olmasını istediğiniz gereklidir. Bu görevler tamamlandıktan sonra bu takım projesi altındaki tüm depoları ekibinizin oluşturulabilir. 

Aşağıdaki bölümde belirtilen görevleri grup yöneticisi tarafından karşılanan birkaç önkoşul sonra beş asıl görevler vardır (bazı isteğe bağlı), bu öğreticide tamamlayın. Bu görevleri bu konuda bölümlerini numaralı ana karşılık gelir:

1. Oluşturma bir **takım projesi** grubu ve iki takım depoları projesinde grubun VSTS sunucusunda:
    - **ProjectTemplate depo** 
    - **TeamUtilities depo**
2. Takım çekirdek **ProjectTemplate** depodan **GroupProjectTemplate** Grup yöneticiniz tarafından ayarlanan deposu. 
3. Takım verileri ve çözümlemeler kaynaklarını oluşturun:
    - Takım özgü yardımcı programlarına eklemek **TeamUtilities** deposu. 
    - (İsteğe bağlı) Oluşturma bir **Azure dosya depolama** tüm ekip için yararlı olabilecek veri varlıkları depolamak için kullanılacak. 
4. (İsteğe bağlı) Azure dosya depolama birimine bağlama **veri bilimi sanal makine** (DSVM) ekibin sağlama ve veri varlıklarını ekleyin.
5. Ayarlanan **güvenlik denetimi** göre takım üyeleri ekleme ve kendi ayrıcalıklarını yapılandırın.

>[AZURE.NOTE] VSTS aşağıdaki yönergeleri kullanarak TDSP takım ortamı kurmak için gerekli adımları ana hatlarını vermektedir. Biz, biz Microsoft'taki TDSP nasıl uygulamak olduğundan bu görevlerin VSTS ile nasıl gerçekleştirileceğini belirtin. Grubunuz için başka bir kod barındırma platformu kullanılıyorsa, ekip lideri tarafından genellikle tamamlanması gereken görevler değiştirmeyin. Ancak bu görevleri tamamlamak için yol farklı olacak.

## <a name="repositories-and-directories"></a>Depoları ve dizinler

Bu konuda depoları ve dizinleri kısaltılmış adlarını kullanır. Bu adları depoları ve dizinler arasında işlemleri izlemek kolaylaştırır. Bu gösterim (**R** Git depoları için ve **D** , DSVM yerel dizinleri için) aşağıdaki bölümlerde kullanılır:

- **R1**: **GroupProjectTemplate** VSTS Grup sunucunuz üzerinde Grup Yöneticisi ayarlanan Git deposunu.
- **R3**: Takım **ProjectTemplate** ayarladığınız Git deposunu.
- **R4**: **TeamUtilities** ayarladığınız Git deposunu.
- **D1**: yerel dizin R1 kopyalanabilen ve D3 için kopyalanır.
- **D3**: yerel dizin R3 kopyalanması, özelleştirme ve R3 için kopyalanır.
- **D4**: yerel dizin R4 kopyalanması, özelleştirme ve geri R4 kopyalanır.

Bu öğreticide depoları ve dizinler için belirtilen adları hedefiniz ayrı takım projesi için kendi takım daha büyük bir veri bilimi grup içinde belirtmektir varsayımına üzerinde belirtildi. Ancak ekip lideri açık diğer seçenekler vardır:

- Grubun tamamı, tek bir takım projesi oluşturmak seçebilirsiniz. Ardından tüm veri bilimi ekipleri tüm projeleri bu tek bir takım projesi altında olacaktır. Bunu başarmak için tek bir takım projesi oluşturmak için bu yönergeleri izlemek için bir git Yöneticisi belirleyebilirsiniz. Bu senaryo, örneğin, geçerli olabilir:
    -  birden çok veri bilimi ekipleri sahip olmayan bir küçük veri bilimi grubu 
    -  yine de arası ekip işbirliği grup düzeyinde sprint planlama gibi etkinliklerle en iyi duruma getirmek istediği daha büyük bir veri bilimi Grup birden çok veri bilimi ekipleri ile. 
- Takımları team özgü proje şablonları veya takım özgü yardımcı programları tüm grup için tek bir takım projesi altında sahip olmayı seçebilirsiniz. Bu durumda, takım müşteri adayları, takım projesi şablonu depoları ve/veya takım yardımcı programları depoları aynı takım projesi altında oluşturmanız gerekir. Bu depoları ad *< TeamName\>ProjectTemplate* ve *< TeamName\>yardımcı programları*; Örneğin, *TeamJohnProjectTemplate*ve *TeamJohnUtilities*. 

Herhangi bir durumda, takım müşteri adayları ayarlama ve proje ve yardımcı programlar depoları kopyalama benimsemek için hangi şablonu ve yardımcı programlar depoları biliyorsanız, takım üyeleri izin vermeniz gerekiyor. Proje müşteri adayları izlemelidir [veri bilimi takım projesi sağlama görevlerde](project-lead-tasks.md) proje depoları ayrı takım projeleri olup olmadığını altında ya da tek bir takım projesi oluşturmak için. 


## <a name="0-prerequisites"></a>0. Önkoşullar

Özetlenen, Grup Yöneticisi atanan görevleri tamamlayarak önkoşullara uyduğunuzdan [grup yöneticisi görevleri için bir veri bilimi ekibi](group-manager-tasks.md). Burada özetlemek için aşağıdaki gereksinimleri ekip sağlama görevleri başlamadan önce yerine getirmeniz gerekir: 

- **Grup VSTS sunucu** (veya barındırma platformu başka bir kod grup hesabına), grup yöneticisi tarafından ayarlanmış.
- **GroupProjectTemplate depo** (R1) ayarlanmış Grup hesabınızdaki kullanmayı düşünüyorsanız platform barındırma kodu, grup yöneticisi tarafından.
- Size verilmiş olması **yetkili** ekibiniz için depoları oluşturmak için Grup hesabınızda.
- Makinenizde Git yüklenmesi gerekir. Bir veri bilimi sanal makine (DSVM) kullanıyorsanız, Git önceden yüklenmiş ve hazırsınız. Aksi takdirde bkz [platformları ve araçlarına ek](platforms-and-tools.md#appendix).  
- Kullanıyorsanız bir **Windows DSVM**, olmasına gerek [Git kimlik bilgisi Yöneticisi (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) makinenize yüklü. README.md dosyasında doğru aşağı kaydırın **yükleyip** 'ye tıklayın *son yükleyici*. Bu son yükleyici sayfasına götürür. .Exe yükleyici buradan indirin ve çalıştırın. 
- Kullanıyorsanız **Linux DSVM**, bir SSH ortak anahtarı, DSVM üzerinde oluşturun ve Grup VSTS sunucunuzu ekleyin. SSH hakkında daha fazla bilgi için bkz: **oluşturma SSH ortak anahtarı** bölümüne [platformları ve araçlarına ek](platforms-and-tools.md#appendix). 
    
## <a name="1-create-a-team-project-and-repositories"></a>1. Bir takım projesi ve havuzları oluşturma

Sürüm oluşturma ve işbirliği platformu barındırma kodunuzu olarak VSTS kullanıyorsanız bu adımı tamamlayın. Bu bölüm, grubunuza VSTS Server'da üç yapıları oluşturma vardır:

- **MyTeam** VSTS projesinde
- **MyProjectTemplate** deposu (**R3**) üzerinde Git
- **MyTeamUtilities** deposu (**R4**) üzerinde Git

### <a name="create-the-myteam-project"></a>MyTeam projesi oluşturma

- Grubunuzun VSTS sunucu giriş URL'sindeki gidin `https://<VSTS Server Name\>.visualstudio.com`. 
- Tıklatın **yeni** bir takım projesi oluşturmak için. 

    ![2](./media/team-lead-tasks/team-leads-2-create-new-team.png)

- Bir oluşturma takım projesi penceresi, proje adı giriş ister (**MyTeam** Bu örnekte). Seçtiğinizden emin olun **Çevik** olarak **işlem şablonu** ve **Git** olarak **sürüm denetimi**. 

    ![3](./media/team-lead-tasks/team-leads-3-create-new-team-2.png)

- Tıklatın **proje oluştur**. Takım projenizin **MyTeam** değerinden 1 dakika içinde oluşturulur. 

- Takım projesi sonra **MyTeam** olan oluşturulan, tıklatın **proje Git** takım projenizin giriş sayfasına yönlendirilmesi için düğmeyi. 

    ![4](./media/team-lead-tasks/team-leads-4-create-new-team-3.png)

- Görürseniz bir **Tebrikler!** açılan pencerede tıklatın **kodu ekleyin** (kırmızı kutu düğmesini). Aksi takdirde tıklatın **kod** (kutusunda sarı). Takım projenizin Git deposu sayfasına yönlendirir. 

    ![5](./media/team-lead-tasks/team-leads-5-team-project-home.png)

### <a name="create-the-myprojecttemplate-repository-r3-on-git"></a>Git üzerinde MyProjectTemplate deposu (R3) oluşturma

- Takım projenizin Git deposu sayfasında deposu adının yanındaki aşağı oka tıklayın **MyTeam**seçip **depoları Yönet...** .

    ![6](./media/team-lead-tasks/team-leads-6-rename-team-project-repo.png)

- Üzerinde **sürüm denetimi** sekmesini takım projenizin Denetim Masası ' nı **MyTeam**seçeneğini belirleyip **yeniden adlandır deposu...** . 

    ![7](./media/team-lead-tasks/team-leads-7-rename-team-project-repo-2.png)

- Depoya yeni bir ad girin **MyTeam depo yeniden adlandırma** penceresi. Bu örnekte, *MyTeamProjectTemplate*. Aşağıdakine benzer seçebilirsiniz *< takım adınızı\>ProjectTemplate*. Tıklatın **yeniden adlandırma** devam etmek için.

    ![8](./media/team-lead-tasks/team-leads-8-rename-team-project-repo-3.png)

### <a name="create-the-myteamutilities-repository-r4-on-git"></a>Git üzerinde MyTeamUtilities deposu (R4) oluşturma

- Yeni bir havuz oluşturmak için *< takım adınızı\>yardımcı programları* takım projenizin altında tıklatın **yeni bir havuz...**  üzerinde **sürüm denetimi** takım projenizin Denetim Masası'ndaki sekmesi.  

    ![9](./media/team-lead-tasks/team-leads-9-create-team-utilities.png)

- İçinde **yeni bir havuz oluşturma** , açılır pencere bu depo için bir ad sağlayın. Bu örnekte, biz olarak adlandırın *MyTeamUtilities*, olduğu **R4** bizim gösteriminde. Aşağıdakine benzer seçin *< takım adınızı\>yardımcı programları*. Seçtiğinizden emin olun **Git** için **türü**. Ardından **oluşturma** devam etmek için.

    ![10](./media/team-lead-tasks/team-leads-10-create-team-utilities-2.png)

- Takım projenizin altında oluşturulan iki yeni Git depoları gördüğünüzü onaylayın **MyTeam**. Bu örnekte: 

- **MyTeamProjectTemplate** (R3) 
- **MyTeamUtilities** (R4).

    ![11](./media/team-lead-tasks/team-leads-11-two-repo-in-team.png)


## <a name="2-seed-your-team-projecttemplate-and-teamutilities-repositories"></a>2. Takım ProjectTemplate ve TeamUtilities depoları çekirdek

Dengeli dağıtım yordam dizinleri, yerel DSVM üzerinde Ara hazırlama siteler olarak kullanır. Özelleştirmeniz gerekiyorsa, **ProjectTemplate** ve **TeamUtilities** bazı belirli karşılamak için depoları ekip ihtiyaçlarını, aşağıdaki yordamı sondan adımında bunu. İçeriği oluşturmak için kullanılan adımlarla bir özeti aşağıda verilmiştir **MyTeamProjectTemplate** ve **MyTeamUtilities** depoları veri bilimi ekibi için. Tek tek adımları dengeli dağıtım yordamdaki alt bölümleri karşılık gelir:

- Kopya grubu yerel dizin depoya: klonlanmış yerel D1 -> için R1 - ekip
- Takım depoları yerel dizine kopyalayın: R3 & klonlanmış yerel D3 & D4 -> için R4 - ekip
- Grup proje şablonu içeriğini yerel takım klasörüne kopyalayın: D1 - içeriğini kopyaladığınız D3 -> için
- Yerel D3 & D4 (isteğe bağlı) özelleştirme
- Yerel dizin içeriğini team depoları itme: D3 & D4 - içeriği Ekle -> R3 & R4 ekip


### <a name="initialize-the-team-repositories"></a>Takım depoları başlatma

Bu adımda, takım projesi şablonu deponuz grup proje şablonu depodan başlatın:

- **MyTeamProjectTemplate** deposu (**R3**) gelen, **GroupProjectTemplate** (**R1**) depo


### <a name="clone-group-repositories-into-local-directories"></a>Grup depoları yerel dizine kopyalama

Bu yordama başlamadan için:

- Yerel makinenizde dizinler oluşturun:
    - İçin **Windows**: **C:\GitRepos\GroupCommon** ve **C:\GitRepos\MyTeam**
    - İçin **Linux**: **GitRepos\GroupCommon** ve **GitRepos\MyTeam** giriş dizininize üzerinde 
- Değiştirme dizinine **GitRepos\GroupCommon**.
- Yerel makinenin işletim sisteminde uygun şekilde aşağıdaki komutu çalıştırın.

**Windows**

    git clone https://<Your VSTS Server name>.visualstudio.com/GroupCommon/_git/GroupProjectTemplate
    

![12](./media/team-lead-tasks/team-leads-12-create-two-group-repos.png)

**Linux**
    
    git clone ssh://<Your VSTS Server name>@<Your VSTS Server name>.visualstudio.com:22/GroupCommon/_git/GroupProjectTemplate
    
    
![13](./media/team-lead-tasks/team-leads-13-clone_two_group_repos_linux.png)

Bu komutlar kopyalama, **GroupProjectTemplate** (R1) depo, Grup VSTS sunucusunda yerel bir dizine **GitRepos\GroupCommon** yerel makinenizde. Kopyalama, dizin sonra **GroupProjectTemplate** (D1) dizininde oluşturulan **GitRepos\GroupCommon**. Burada, Grup Yöneticisi bir takım projesi oluşturulmuş varsayıyoruz **GroupCommon**ve **GroupProjectTemplate** bu takım projesi altındaki depodur. 


### <a name="clone-your-team-repositories-into-local-directories"></a>Takım depoları yerel dizine kopyalama

Bu komutlar kopyalama, **MyTeamProjectTemplate** (R3) ve **MyTeamUtilities** (R4) depoları takım projenizin altında **MyTeam** içinGrupVSTSsunucunuzda **MyTeamProjectTemplate** (D3) ve **MyTeamUtilities** (D4) dizinlerde **GitRepos\MyTeam** yerel makinenizde. 

- Değiştirme dizinine **GitRepos\MyTeam**
- Yerel makinenin işletim sisteminde uygun olarak aşağıdaki komutları çalıştırın. 

**Windows**

    git clone https://<Your VSTS Server name>.visualstudio.com/<Your Team Name>/_git/MyTeamProjectTemplate
    git clone https://<Your VSTS Server name>.visualstudio.com/<Your Team Name>/_git/MyTeamUtilities

![14](./media/team-lead-tasks/team-leads-14-clone_two_empty_team_repos.png)
        
**Linux**
    
    git clone ssh://<Your VSTS Server name>@<Your VSTS Server name>.visualstudio.com:22/<Your Team Name>/_git/MyTeamProjectTemplate
    git clone ssh://<Your VSTS Server name>@<Your VSTS Server name>.visualstudio.com:22/<Your Team Name>/_git/MyTeamUtilities
    
![15](./media/team-lead-tasks/team-leads-15-clone_two_empty_team_repos_linux.png)

Kopyalama, iki dizini sonra **MyTeamProjectTemplate** (D3) ve **MyTeamUtilities** (D4) dizininde oluşturulan **GitRepos\MyTeam**. Biz Burada şablon ve yardımcı programlar depoları takım projenizin adlı kabul **MyTeamProjectTemplate** ve **MyTeamUtilities**. 

### <a name="copy-the-group-project-template-content-to-the-local-team-project-template-directory"></a>Grup proje şablonu içeriğini yerel takım projesi şablonu dizinine kopyalayın

Yerel içeriği kopyalamasına izin **GroupProjectTemplate** (D1) klasörünü ve yerel **MyTeamProjectTemplate** (D3), aşağıdaki Kabuk komut dosyalarından birini çalıştırın: 

#### <a name="from-the-powershell-command-line-for-windows"></a>PowerShell Windows için komut satırı       

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_win.ps1" -outfile "tdsp_local_copy_win.ps1"
    .\tdsp_local_copy_win.ps1 2

    
![16](./media/team-lead-tasks/team-leads-16-local_copy_team_lead_new.png)

#### <a name="from-the-linux-shell-for-the-linux-dsvm"></a>İçin Linux Kabuğu'ndan **Linux DSVM**
    
    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_linux.sh"
    bash tdsp_local_copy_linux.sh 2
    
![17](./media/team-lead-tasks/team-leads-17-local-copy-team-lead-linux-new.png)

Komut dosyalarını .git dizinin içeriğini çıkarın. Komut dosyaları sağlamak ister **tamamlamak yolları** kaynak dizin D1 ve hedef dizin D3.
        

### <a name="customize-your-team-project-template-or-team-utilities-optional"></a>Takım projesi şablonu veya takım hizmet programları (isteğe bağlı) özelleştirme

Özelleştirme, **MyTeamProjectTemplate** (D3) ve **MyTeamUtilities** (gerekirse, Kurulum işlemi bu aşamada D4). 

- Ekibinizin özel ihtiyaçlarını karşılamak için D3 içeriğini özelleştirmek istiyorsanız, şablon belgelerini değiştirme veya dizin yapısını değiştirin.

- Ekibinizin tamamını takımınızla paylaşmak istediğiniz bazı yardımcı programları geliştirdiyseniz, kopyalayın ve bu yardımcı programları D4 dizinine yapıştırın. 


### <a name="push-local-directory-content-to-team-repositories"></a>Yerel dizin içeriğini team depoları için anında iletme

İçeriği (isteğe bağlı olarak özelleştirilmiş) yerel dizinler D3 ve D4 R3 ve R4 takım depoları eklemek için bir Windows PowerShell konsolundan ya da Linux Kabuğu'ndan aşağıdaki git bash komutları çalıştırın. Komutları çalıştırmak **GitRepos\MyTeam\MyTeamProjectTemplate** dizini.

    git status
    git add .
    git commit -m"push from DSVM"
    git push
    
![18](./media/team-lead-tasks/team-leads-18-push-to-group-server-2.png)

Bu komut dosyasını çalıştırdığınızda grubunuzun VSTS sunucusunun MyTeamProjectTemplate depo dosyalarında neredeyse anında eşitlenir.

![19](./media/team-lead-tasks/team-leads-19-push-to-group-server-showed-up.png)

Şimdi aynı dört git komutları çalıştırarak **GitRepos\MyTeam\MyTeamUtilities** dizini. 

> [AZURE.NOTE]Bir Git deposuna yürüttükten ilk kez kullanıyorsanız, genel parametreleri yapılandırmanız gereken *user.name* ve *user.email* çalıştırmadan önce `git commit` komutu. Aşağıdaki iki komutu çalıştırın:
        
    git config --global user.name <your name>
    git config --global user.email <your email address>
 
> Birden çok Git depoları için yürütülmekte olan, her birine yaparsanız aynı ad ve e-posta adresi kullanın. Powerbı panolar üzerinde birden çok depoları Git etkinlikleri izlemenize oluştururken aynı ad ve e-posta adresini kullanarak daha sonra uygun kanıtlar.

![20](./media/team-lead-tasks/team-leads-20-git-config-name.png)


## <a name="3-create-team-data-and-analytics-resources-optional"></a>3. Takım verileri ve çözümlemeler kaynakları (isteğe bağlı) oluşturun

Tüm ekibinizle verileri ve çözümlemeler kaynakları paylaşma performans ve maliyet avantajları vardır: Takım üyeleri paylaşılan kaynakları projelerini çalıştırmak, bütçeleri kaydedebilir ve daha verimli bir şekilde işbirliği. Bu bölümde, Azure dosya depolama oluşturma hakkında yönergeler sunuyoruz. Sonraki bölümde, yerel makinenize bağlama Azure dosya depolama alanına nasıl yönerge sağlar. Azure Hdınsight Spark kümeleri gibi Azure veri bilimi sanal makineler, diğer kaynakları paylaşma hakkında ek bilgi için bkz [platformları ve araçları](platforms-and-tools.md). Bu konuda ihtiyaçlarınız için uygun olan kaynaklara seçme konusunda rehberlik veri bilimi açısından sağlar ve ürün sayfaları ve yayımlanmış olan diğer ilgili ve kullanışlı öğreticileri bağlar.

>[AZURE.NOTE] Veri gönderme çapraz önlemek için yavaş ve pahalı olabilir, veri merkezleri, kaynak grubu, depolama hesabı ve Azure VM (örn., DSVM) aynı Azure veri merkezinde olduğundan emin olun. 

Ekibiniz için Azure dosya deposu oluşturmak için aşağıdaki komut dosyasını çalıştırın. Azure file storage ekibiniz için tüm ekibiniz için yararlı olan veri varlıkları depolamak için kullanılabilir. Komut dosyaları, komut istemine, Azure hesabınızı ve aboneliğinizi bilgi için bu nedenle bu kimlik bilgilerini girmek için hazır olması. 

### <a name="create-azure-file-storage-with-powershell-from-windows"></a>Windows PowerShell ile Azure dosya depolama oluşturma

Bu komut dosyasını Powershell'den komut satırı çalıştır:

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/CreateFileShare.ps1" -outfile "CreateFileShare.ps1"
    .\CreateFileShare.ps1

![21](./media/team-lead-tasks/team-leads-21-create-fileshare-win.png)   

İstendiğinde Microsoft Azure hesabınızda oturum açın:

![22](./media/team-lead-tasks/team-leads-22-file-create-s1.png)

Kullanmak istediğiniz Azure aboneliğini seçin:

![23](./media/team-lead-tasks/team-leads-23-file-create-s2.png)

Kullanın veya seçilen aboneliğinizi altında yeni bir tane oluşturmak için hangi depolama hesabı seçin:

![24](./media/team-lead-tasks/team-leads-24-file-create-s3.png)

Oluşturmak için Azure dosya depolama adını girin. Düşük yalnızca büyük-küçük harf karakterler, sayılar ve - kabul edilir:

![25](./media/team-lead-tasks/team-leads-25-file-create-s4.png)

Bağlama ve oluşturulduktan sonra bu depolama paylaşımı kolaylaştırmak için Azure dosya depolama bilgilerini bir metin dosyasına kaydedin ve konumuna yolunu not edin. Özellikle, sonraki bölümde, Azure sanal makineleriniz için Azure dosya depolama alanınızı bağlamak için bu dosyayı gerekir. 

Bu metin dosyasına takım ProjectTemplate deponuza iade etmek için iyi bir uygulamadır. Dizinde yerleştirmek için önerdiğimiz **Docs\DataDictionaries**. Bu nedenle, bu veri varlığına ekibinizin tüm projelerde tarafından erişilebilir. 

![26](./media/team-lead-tasks/team-leads-26-file-create-s5.png)


### <a name="create-azure-file-storage-with-a-linux-script"></a>Azure dosya depolama ile Linux komut dosyası oluşturma

Bu komut Linux Kabuğu'ndan çalıştırın:

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/CreateFileShare.sh"
    bash CreateFileShare.sh

Bu ekrandaki yönergeleri izleyerek Microsoft Azure hesabınızda oturum açın:

![27](./media/team-lead-tasks/team-leads-27-file-create-linux-s1.png)

Kullanmak istediğiniz Azure aboneliğini seçin:

![28](./media/team-lead-tasks/team-leads-28-file-create-linux-s2.png)

Kullanın veya seçilen aboneliğinizi altında yeni bir tane oluşturmak için hangi depolama hesabı seçin:

![29](./media/team-lead-tasks/team-leads-29-file-create-linux-s3.png)

Oluşturmak için Azure dosya depolama, yalnızca küçük harfler, sayılar adını girin ve - kabul edilir:

![30](./media/team-lead-tasks/team-leads-30-file-create-linux-s4.png)

Oluşturulduktan sonra bu depolama erişimi kolaylaştırmak için Azure dosya depolama bilgilerini bir metin dosyasına kaydedin ve konumuna yolunu not edin. Özellikle, sonraki bölümde, Azure sanal makineleriniz için Azure dosya depolama alanınızı bağlamak için bu dosyayı gerekir.

Bu metin dosyasına takım ProjectTemplate deponuza iade etmek için iyi bir uygulamadır. Dizinde yerleştirmek için önerdiğimiz **Docs\DataDictionaries**. Bu nedenle, bu veri varlığına ekibinizin tüm projelerde tarafından erişilebilir. 

![31](./media/team-lead-tasks/team-leads-31-file-create-linux-s5.png)


## <a name="4-mount-azure-file-storage-optional"></a>4. Bağlama Azure dosya depolama (isteğe bağlı)

Azure dosya depolama başarıyla oluşturulduktan sonra aşağıdaki PowerShell ya da Linux komut dosyalarından birini kullanarak yerel makinenize bağlanabilir.

### <a name="mount-azure-file-storage-with-powershell-from-windows"></a>Windows PowerShell ile bağlama Azure dosya depolama

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/AttachFileShare.ps1" -outfile "AttachFileShare.ps1"
    .\AttachFileShare.ps1
    
Oturum açmamış olan, ilk, oturum istenir. 

Tıklatın **Enter** veya **y** dosya ve ardından giriş Azure dosya depolama bilgi varsa sorulduğunda devam etmek için ***tamamlamak yolu ve adı** oluşturduğunuz dosyasının önceki adımı. Bir Azure dosya depolama bağlamak için bilgileri doğrudan dosyanız varsa ve sonraki adıma geçmeye hazır okunur.

![32](./media/team-lead-tasks/team-leads-32-attach-s1.png)

> [AZURE.NOTE] Azure dosya depolama bilgilerini içeren bir dosya yoksa, bu bölümün sonunda klavye bilgilerinden giriş için adımları sağlanır.

Ardından, sanal makineye eklenecek sürücü adını girmeniz istenir. Varolan sürücü adlarının bir listesini ekranda yazdırılır. Zaten listede var olmayan bir sürücü adı sağlamalıdır.

![33](./media/team-lead-tasks/team-leads-33-attach-s2.png)

Yeni bir F sürücü makinenize başarılı bir şekilde bağlı olduğunu onaylayın.

![34](./media/team-lead-tasks/team-leads-34-attach-s3.png)

**Azure dosya depolama bilgilerini el ile girmek nasıl:** bir metin dosyasını Azure dosya depolama bilgilerinizi yoksa, gerekli olan abonelik, depolama hesabı ve Azure türü için aşağıdaki ekrandaki yönergeleri izleyin dosya depolama bilgileri:

![35](./media/team-lead-tasks/team-leads-35-attach-s4.png)

Depolama hesabı Azure file storage oluşturulduğu, Azure aboneliği adı yazın, seçin ve Azure dosya depolama adı yazın:

![36](./media/team-lead-tasks/team-leads-36-attach-s5.png)

### <a name="mount-azure-file-storage-with-a-linux-script"></a>Bağlama Azure file storage ile Linux komut dosyası

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/AttachFileShare.sh"
    bash AttachFileShare.sh

![37](./media/team-lead-tasks/team-leads-37-attach-s1-linux.png)

Oturum açmamış olan, ilk, oturum istenir. 

Tıklatın **Enter** veya **y** dosya ve ardından giriş Azure dosya depolama bilgi varsa sorulduğunda devam etmek için ***tamamlamak yolu ve adı** oluşturduğunuz dosyasının önceki adımı. Bir Azure dosya depolama bağlamak için bilgileri doğrudan dosyanız varsa ve sonraki adıma geçmeye hazır okunur.

![38](./media/team-lead-tasks/team-leads-38-attach-s2-linux.png)

Ardından, sanal makineye eklenecek sürücü adını girmeniz istenir. Varolan sürücü adlarının bir listesini ekranda yazdırılır. Zaten listede var olmayan bir sürücü adı sağlamalıdır.

![39](./media/team-lead-tasks/team-leads-39-attach-s3-linux.png)

Yeni bir F sürücü makinenize başarılı bir şekilde bağlı olduğunu onaylayın.

![40](./media/team-lead-tasks/team-leads-40-attach-s4-linux.png)

**Azure dosya depolama bilgilerini el ile girmek nasıl:** bir metin dosyasını Azure dosya depolama bilgilerinizi yoksa, gerekli olan abonelik, depolama hesabı ve Azure türü için aşağıdaki ekrandaki yönergeleri izleyin dosya depolama bilgileri:

- Giriş **n**.
- Azure file storage önceki adımda oluşturulduğu abonelik adı dizinini seçin:

    ![41](./media/team-lead-tasks/team-leads-41-attach-s5-linux.png)

- Depolama hesabı türü ve abonelik altında Azure dosya depolama adı seçin:

    ![42](./media/team-lead-tasks/team-leads-42-attach-s6-linux.png)

- Tüm mevcut olanlardan farklı olmalıdır makinenize eklenecek sürücü adını girin:

    ![43](./media/team-lead-tasks/team-leads-43-attach-s7-linux.png)


## <a name="5-set-up-security-control-policy"></a>5. Güvenlik denetimi İlkesi ayarlama 

Grup VSTS sunucunuzun giriş sayfasından tıklatın **dişli simgesi** kullanıcı adınızı yanındaki sağ üst köşesinde, ardından **güvenlik** sekmesi. Ekibinize burada çeşitli izinlerle üye ekleyebilirsiniz.

![44](./media/team-lead-tasks/team-leads-44-add-team-members.png)

## <a name="next-steps"></a>Sonraki adımlar

Burada, rolleri ve görevleri takım veri bilimi işlem tarafından tanımlanan daha ayrıntılı açıklamaları bağlantıları verilmiştir:

- [Bir veri bilimi ekibi için Grup yöneticisi görevleri](group-manager-tasks.md)
- [Bir veri bilimi ekibi için takım sağlama görevleri](team-lead-tasks.md)
- [Proje veri bilimi ekibi için sağlama görevleri](project-lead-tasks.md)
- [Proje veri bilimi ekibi için tek tek Katkıda Bulunanlar](project-ic-tasks.md)