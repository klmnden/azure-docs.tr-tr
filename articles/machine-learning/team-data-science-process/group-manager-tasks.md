---
title: "Takım veri bilimi işlem grup yöneticisi görevleri - Azure | Microsoft Docs"
description: "Grup Yöneticisi veri bilimi takım projesi üzerinde görevlerde ana hattı."
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: bradsev;
ms.openlocfilehash: 58cea8b0288469a76dd8c4eb967caa8e87cd3dd7
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="group-manager-tasks"></a>Grup yöneticisi görevleri

Grup yöneticisi beklenen görevleri tamamlamak için hist bu konuda anahatları / kendi veri bilimi kuruluş. Amaç üzerinde standartlaştıran işbirliğine dayalı Grup ortamı belirtmektir [takım veri bilimi işlemi](overview.md) (TDSP). Bu işlem üzerinde Standartlaştırma personel rolleri ve veri bilimi ekibi tarafından işlenen ilişkilendirilen görevlerinin bir özetini görmek [takım veri bilimi işlemi rolleri ve görevleri](roles-tasks.md).

**Grup Yöneticisi** bir kuruluştaki tüm veri bilimi biriminin yöneticisidir. Bir veri bilimi birimi, her biri ayrı iş verticals birden çok veri bilimi proje üzerinde çalıştığı birden çok ekibin olabilir. Grup Yöneticisi görevlerini bir yedek için temsilci, ancak rol ile ilişkili görevleri aynıdır. Aşağıdaki çizimde gösterildiği gibi altı ana görevleri şunlardır:

![0](./media/group-manager-tasks/tdsp-group-manager.png)


>[AZURE.NOTE] VSTS aşağıdaki yönergeleri kullanarak TDSP grubu ortamı kurmak için gerekli adımları ana hatlarını vermektedir. Biz, biz Microsoft'taki TDSP nasıl uygulamak olduğundan bu görevlerin VSTS ile nasıl gerçekleştirileceğini belirtin. Grubunuz için başka bir kod barındırma platformu kullanılıyorsa, grup yöneticisi tarafından genellikle tamamlanması gereken görevler değiştirmeyin. Ancak bu görevleri tamamlamak için yol farklı olacak.

1. Ayarlanan **Visual Studio Team Services (VSTS) sunucu** grubu için.
2. Oluşturma bir **Grup takım projesi** Visual Studio Team Services sunucusunda (VSTS kullanıcılar için)
3. Oluşturma **GroupProjectTemplate** deposu
4. Oluşturma **GroupUtilities** deposu
5. Çekirdek **GroupProjectTemplate** ve **GroupUtilities** depoları TDSP depoları içerikten VSTS sunucusuyla için.
6. Ayarlanan **güvenlik denetimleri** ekip üyelerinin GroupProjectTemplate ve GroupUtilities depoları için erişim.

Yukarıdaki adımların her biri ayrıntılı olarak açıklanmıştır. Ancak önce biz kısaltmalar ile hakkında bilgi edinin ve depoları ile çalışmak için ön koşullar tartışın.

### <a name="abbreviations-for-repositories-and-directories"></a>Kısaltmalar depoları ve dizinler

Bu öğretici depoları ve dizinleri kısaltılmış adlarını kullanır. Bu tanımları depoları ve dizinler arasında işlemleri izlemek kolaylaştırır. Aşağıdaki bölümlerde bu gösterim kullanılır:

- **G1**: Proje şablonu deposu geliştirilen ve Microsoft TDSP ekibi tarafından yönetilebilir.
- **G2**: yardımcı programlar depo geliştirilen ve Microsoft TDSP ekibi tarafından yönetilebilir.
- **R1**: GroupProjectTemplate Git deposunu sizin ayarladığınız VSTS Grup sunucunuz üzerinde.
- **R2**: GroupUtilities Git deposunu sizin ayarladığınız VSTS Grup sunucunuz üzerinde.
- **LG1** ve **LG2**: G1 ve G2 için sırasıyla kopyalama makinenizde yerel dizinleri.
- **LR1** ve **LR2**: R1 ve R2 için sırasıyla kopyalama makinenizde yerel dizinleri.

### <a name="pre-requisites-for-cloning-repositories-and-checking-code-in-and-out"></a>Depoları kopyalama ve kod giriş ve çıkış denetimi için ön koşullar
 
- Makinenizde Git yüklenmesi gerekir. Bir veri bilimi sanal makine (DSVM) kullanıyorsanız, Git önceden yüklenmiş ve hazırsınız. Aksi takdirde bkz [platformları ve araçlarına ek](platforms-and-tools.md#appendix).  
- Kullanıyorsanız bir **Windows DSVM**, olmasına gerek [Git kimlik bilgisi Yöneticisi (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) makinenize yüklü. README.md dosyasında doğru aşağı kaydırın **yükleyip** 'ye tıklayın *son yükleyici*. Bu adım en son yükleyici sayfasına gidersiniz. .Exe yükleyici buradan indirin ve çalıştırın. 
- Kullanıyorsanız **Linux DSVM**, bir SSH ortak anahtarı, DSVM üzerinde oluşturun ve Grup VSTS sunucunuzu ekleyin. SSH hakkında daha fazla bilgi için bkz: **oluşturma SSH ortak anahtarı** bölümüne [platformları ve araçlarına ek](platforms-and-tools.md#appendix). 


## <a name="1-create-account-on-vsts-server"></a>1. VSTS sunucuda hesabı oluşturma

VSTS sunucunun aşağıdaki depoları barındırır:

- **Ortak depoları grup**: birden çok veri bilimi proje için bir grup içindeki birden çok ekibin tarafından benimsenen genel amaçlı depoları. Örneğin, *GroupProjectTemplate* ve *GroupUtilities* depoları.
- **Ekip depoları**: bir grup içindeki belirli takımlar için depoları. Bu depoları bir ekibin gereksinimini özeldir ve bu ekibi tarafından yürütülen birden çok proje tarafından kabul edilen, ancak genel yeterli veri bilimi grup içindeki birden çok ekibin kullanışlı olması için olabilir. 
- **Proje depoları**: depoları belirli projeler için kullanılabilir. Bu tür depoları birden çok proje ekibi tarafından gerçekleştirilen ve veri bilimi grubunda birden çok ekibin kullanışlı olması için genel olmayabilir.


### <a name="setting-up-the-vsts-server-sign-into-your-microsoft-account"></a>Microsoft hesabınızda VSTS sunucu oturum ayarlama
    
Git [Visual Studio online](https://www.visualstudio.com/), tıklatın **oturum** sağ üst köşesinde ve Microsoft hesabınızda oturum. 
    
![1](./media/group-manager-tasks/login.PNG)

Bir Microsoft hesabınız yoksa tıklatın **şimdi kaydolun** bir Microsoft hesabı oluşturun ve sonra bu hesabı kullanarak oturum açın. 

Kuruluşunuz bir Visual Studio/MSDN aboneliğiniz varsa, yeşil tıklatın **iş veya Okul hesabınızla oturum açın** kutusu ve bu abonelikle ilişkili kimlik bilgileriyle oturum açın. 
        
![2](./media/group-manager-tasks/signin.PNG)


        
Oturum açtıktan sonra tıklatın **yeni hesap oluştur** aşağıdaki resimde gösterildiği gibi sağ üst köşesindeki:
        
![3](./media/group-manager-tasks/create-account-1.PNG)
        
Oluşturmak istediğiniz VSTS sunucu bilgileri doldurun **hesabınızı oluşturmak** aşağıdaki değerlerle Sihirbazı: 

- **Sunucu URL'si**: Değiştir *mysamplegroup* kendi değerlerinizle *sunucu adı*. Sunucunuzun URL'sini olacağını: *https://\<servername\>. visualstudio.com*. 
- **Kod kullanarak yönetme:** seçin  **_Git_**.
- **Proje adı:** Enter *GroupCommon*. 
- **İş kullanarak düzenlemek:** Seç *Çevik*.
- **Projelerinizde barındırmak:** bir coğrafi konum seçin. Bu örnekte, biz seçin *Orta Güney ABD*. 
        
![4](./media/group-manager-tasks/fill-in-account-information.png)

>[AZURE.NOTE] ' Ya tıkladıktan sonra aşağıdaki açılır pencere görüyorsanız, **yeni hesabı oluşturma**, tıklamanız gerekir sonra **değiştirmek ayrıntıları** listelenen tüm alanları görüntülemek için.

![5](./media/group-manager-tasks/create-account-2.png)


Tıklatın **devam**. 

## <a name="2-groupcommon-team-project"></a>2. GroupCommon takım projesi

**GroupCommon** sayfa (*https://\<servername\>.visualstudio.com/GroupCommon*) VSTS sunucunuz oluşturulduktan sonra açılır.
                            
![6](./media/group-manager-tasks/server-created-2.PNG)

## <a name="3-create-the-grouputilities-r2-repository"></a>3. GroupUtilities (R2) deposu oluşturma

Oluşturmak için **GroupUtilities** (R2) deposu VSTS sunucusu altında:

- Açmak için **yeni bir havuz oluşturma** Sihirbazı'nı tıklatın **yeni bir havuz** üzerinde **sürüm denetimi** takım projenizin sekmesi. 

![7](./media/group-manager-tasks/create-grouputilities-repo-1.png) 

- Seçin *Git* olarak **türü**ve girin *GroupUtilities* olarak **adı**ve ardından **oluşturma**. 

![8](./media/group-manager-tasks/create-grouputilities-repo-2.png)
                
Artık iki Git depoları görmelisiniz **GroupProjectTemplate** ve **GroupUtilities** sol sütununda **sürüm denetimi** sayfa: 

![9](./media/group-manager-tasks/two-repo-under-groupCommon.PNG)


## <a name="4-create-the-groupprojecttemplate-r1-repository"></a>4. GroupProjectTemplate (R1) deposu oluşturma

Depoları VSTS grubu sunucusu için Kurulum iki görevden oluşur:

- Varsayılan yeniden adlandırma **GroupCommon** deposu***GroupProjectTemplate***.
- Oluşturma **GroupUtilities** takım projesi altında VSTS sunucuda deposu **GroupCommon**. 

Yönergeler için ilk görev adlandırma kuralları ilgili açıklamalar veya bizim depoları ve dizinleri sonra bu bölümde yer alır. Aşağıdaki bölümde 4. adım için ikinci görev için yönergeler bulunur.

### <a name="rename-the-default-groupcommon-repository"></a>Varsayılan GroupCommon depo yeniden adlandırma

Varsayılan yeniden adlandırmak için **GroupCommon** deposu olarak *GroupProjectTemplate* (olarak adlandırılan **R1** bu öğreticideki):
    
- Tıklatın **işbirliği kodu** üzerinde **GroupCommon** takım projesi sayfası. Bu takım projesi varsayılan Git deposu sayfasına götürür **GroupCommon**. Şu anda bu Git deposu boştur. 

![10](./media/group-manager-tasks/rename-groupcommon-repo-3.png)
        
- Tıklatın **GroupCommon** (aşağıdaki şekilde kırmızı kutu ile vurgulanan) sol üst köşede Git deposu sayfasında üzerinde **GroupCommon** seçip **depolarıyönetme**(aşağıdaki şekilde yeşil bir kutusuyla vurgulanan). Bu yordamı getirir **denetim MASASI**. 
- Seçin **sürüm denetimi** takım projenizin sekmesi. 

![11](./media/group-manager-tasks/rename-groupcommon-repo-4.png)

- Tıklatın **...**  sağındaki **GroupCommon** sol panelde ve seçim deposu **yeniden adlandır deposu**. 

![12](./media/group-manager-tasks/rename-groupcommon-repo-5.png)
        
- İçinde **GroupCommon depo yeniden adlandırma** açılarak girin Sihirbazı'nı *GroupProjectTemplate* içinde **depo adını** kutusuna ve ardından **yeniden adlandır** . 

![13](./media/group-manager-tasks/rename-groupcommon-repo-6.png)



## <a name="5-seed-the-r1--r2-repositories-on-the-vsts-server"></a>5. VSTS sunucuda R1 ve R2 depoları çekirdek

Yordamın bu aşamasında, çekirdek *GroupProjectTemplate* (R1) ve *GroupUtilities* önceki bölümde ayarlayın (R2) depoları. Bu depoları ile sağlanmış ***ProjectTemplate*** (**G1**) ve ***yardımcı programları*** (**G2**) tarafından yönetilen depoları Microsoft Team veri bilimi işlemi için. Bu dengeli tamamlandığında:

- R1 deponuz G1 mu dizinler ve belge şablonları aynı kümesini olacaktır
- R2 deponuz veri bilimi yardımcı programları Microsoft tarafından geliştirilen kümesini içeren zordur.

Dengeli dağıtım yordam dizinleri, yerel DSVM üzerinde Ara hazırlama siteler olarak kullanır. Bu bölümde ardından adımlar şunlardır:

- G1 & G2 - klonlanmış LG1 & LG2 -> için
- R1 ve R2 - klonlanmış LR1 & LR2 -> için
- LG1 & LG2 - kopyalanır dosyaları LR1 & LR2 ->
- LR1 & LR2 (isteğe bağlı) özelleştirme
- LR1 & LR2 - içeriği eklemek R1 ve R2 ->


### <a name="clone-g1--g2-repositories-to-your-local-dsvm"></a>Yerel DSVM için kopya G1 & G2 depoları

Bu adımda, takım veri bilimi işlem (TDSP) ProjectTemplate depo (G1) ve yardımcı programları (G2) da TDSP github depolarının klasörlere LG1 ve LG2 olarak, yerel DSVM içinde kopyalama:

- Depolarının, tüm kopyalarını barındırmak için kök dizini olarak hizmet verecek bir dizin oluşturun. 
    -  Windows DSVM içinde bir dizin oluşturun *C:\GitRepos\TDSPCommon*. 
    -  Linux DSVM içinde bir dizin oluşturun *GitRepos\TDSPCommon* giriş dizininizdeki. 

- Aşağıdaki komutları çalıştırarak *GitRepos\TDSPCommon* dizini.

    `git clone https://github.com/Azure/Azure-TDSP-ProjectTemplate`<br>
    `git clone https://github.com/Azure/Azure-TDSP-Utilities`
        
![14](./media/group-manager-tasks/two-folder-cloned-from-TDSP-windows.PNG)

- Bizim kısaltılmış deposu adlarını kullanarak, bu komut dosyalarını elde ettikleri şudur: 
    - İçine klonlanmış G1 - LG1 ->
    - İçine klonlanmış G2 - LG2 ->
- Kopyalama tamamlandıktan sonra iki dizini görüyor olmalısınız _ProjectTemplate_ ve _yardımcı programları_altında **GitRepos\TDSPCommon** dizini. 

### <a name="clone-r1--r2-repositories-to-your-local-dsvm"></a>Yerel DSVM için kopya R1 ve R2 depoları

Bu adımda, GroupProjectTemplate depo (R1) ve (sırasıyla LR1 ve LR2, adlandırılır) yerel dizinlerde GroupUtilities havuzda (R2) kopyalama **GitRepos\GroupCommon** , DSVM üzerinde.

- R1 ve R2 depoları URL'lerini almak için şu adrese gidin, **GroupCommon** VSTS giriş sayfasında. Bu genellikle URL'sine sahip *https://\<VSTS sunucu adınız\>.visualstudio.com/GroupCommon*. 
- Tıklatın **kod**. 
- Seçin **GroupProjectTemplate** ve **GroupUtilities** depoları. Kopyalayın ve her (HTTPS için Windows; URL'leri kaydedin Linux için SSH) gelen **kopya URL** öğesi, ardından, aşağıdaki komut dosyaları kullanmak için:  

![15](./media/group-manager-tasks/find_https_ssh_2.PNG)

- Dönüştürme **GitRepos\GroupCommon** , Windows veya Linux DSVM ve aşağıdaki adımlardan birini R1 ve R2 bu dizine kopyalama komutları çalıştırma bir dizin.
        
Windows ve Linux komutlar şunlardır:

    # Windows DSVM

    git clone <the HTTPS URL of the GroupProjectTemplate repository>
    git clone <the HTTPS URL of the GroupUtilities repository>

![16](./media/group-manager-tasks/clone-two-empty-group-reo-windows-2.PNG)

    # Linux DSVM

    git clone <the SSH URL of the GroupProjectTemplate repository>
    git clone <the SSH URL of the GroupUtilities repository>

![17](./media/group-manager-tasks/clone-two-empty-group-reo-linux-2.PNG)

>[AZURE.NOTE] LR1 ve LR2 boş uyarı iletileri almak bekler.    

- Bizim kısaltılmış deposu adlarını kullanarak, bu komut dosyalarını elde ettikleri şudur: 
    - İçine klonlanmış R1 - LR1 ->
    - İçine klonlanmış R2 - LR2 ->   


### <a name="seed-your-groupprojecttemplate-lr1-and-grouputilities-lr2"></a>GroupProjectTemplate (LR1) ve GroupUtilities (LR2) çekirdek

Ardından, yerel makinenizde içeriği ProjectTemplate ve yardımcı programlar dizinlerin (dışında meta verileri .git dizinlerde) altında GitRepos\TDSPCommon GroupProjectTemplate ve GroupUtilities dizinlerinizi altında kopyalamayı **GitRepos\ GroupCommon**. Bu adımı tamamlamak için iki görevler şunlardır:

- İçinde GitRepos\TDSPCommon\ProjectTemplate dosyaları kopyalayın (**LG1**) GitRepos\GroupCommon\GroupProjectTemplate için (**LR1**) 
- İçinde GitRepos\TDSPCommon\Utilities dosyaları kopyalayın (**LG2** GitRepos\GroupCommon\Utilities için (**LR2**). 

Bu iki görevleri elde etmek için aşağıdaki betikler PowerShell konsolunda (Windows) veya kabuk komut konsolunda (Linux) çalıştırın. Tam yollar LG1, LR1, LG2 ve LR2 giriş istenir. Girdiğiniz yol doğrulanır. Var olmayan bir dizin girişi, yeniden giriş istenir. 

    # Windows DSVM      
    
    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_win.ps1" -outfile "tdsp_local_copy_win.ps1"
    .\tdsp_local_copy_win.ps1 1

![18](./media/group-manager-tasks/copy-two-folder-to-group-folder-windows-2.PNG)

Şimdi (dışında .git dizindeki dosyaları) LG1 ve LG1 dizinlerdeki dosyaları LR1 ve LR2, sırasıyla kopyalandığını görebilirsiniz.

![19](./media/group-manager-tasks/copy-two-folder-to-group-folder-windows.PNG)

    # Linux DSVM

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_linux.sh"
    bash tdsp_local_copy_linux.sh 1

![20](./media/group-manager-tasks/copy-two-folder-to-group-folder-linux-2.PNG)
        
Şimdi (dışında .git dizindeki dosyaları) iki klasörlerdeki dosyaların GroupProjectTemplate ve GroupUtilities için sırasıyla kopyalandığından emin bakın.
    
![21](./media/group-manager-tasks/copy-two-folder-to-group-folder-linux.PNG)

- Bizim kısaltılmış deposu adlarını kullanarak, bu komut dosyalarını elde ettikleri şudur:
    - LG1 - LR1 içine kopyalanan dosya ->
    - LG2 - LR2 içine kopyalanan dosya ->

### <a name="option-to-customize-the-contents-of-lr1--lr2"></a>LR1 & LR2 içeriğini özelleştirmek için seçeneği
    
LR1 ve LR2 grubunuz belirli ihtiyaçlarını karşılamak üzere içeriğini özelleştirmek istiyorsanız, burada uygun yordamı aşaması budur. Şablon belgeleri değiştirebilir, dizin yapısını değiştirmek ve grubunuzun geliştirmiştir veya tüm grubunuz için yararlı olan mevcut yardımcı programları ekleyebilirsiniz. 

### <a name="add-the-contents-in-lr1--lr2-to-r1--r2-on-group-server"></a>İçeriği LR1 & LR2 R1 ve R2 grup sunucuya ekleme

Şimdi, LR1 ve LR2 R1 ve R2 depoları içeriği eklemeniz gerekir. Windows PowerShell veya Linux çalıştırabilirsiniz bash komutlar git şunlardır. 

GitRepos\GroupCommon\GroupProjectTemplate dizininden aşağıdaki komutları çalıştırın:

    git status
    git add .
    git commit -m"push from DSVM"
    git push

-M seçeneği git işleme için bir ileti ayarlamanıza olanak tanır.

![22](./media/group-manager-tasks/push-to-group-server-2.PNG)

Grubunuzun VSTS Server'daki GroupProjectTemplate deposunda dosyaları hemen eşitlendiğini görebilirsiniz.

![23](./media/group-manager-tasks/push-to-group-server-showed-up-2.PNG)

Son olarak, Dönüştür **GitRepos\GroupCommon\GroupUtilities** directory ve çalışma kümesine git bash komutlar:

    git status
    git add .
    git commit -m"push from DSVM"
    git push

>[AZURE.NOTE] Bir Git deposuna yürüttükten ilk kez kullanıyorsanız, genel parametreleri yapılandırmanız gereken *user.name* ve *user.email* çalıştırmadan önce `git commit` komutu. Aşağıdaki iki komutu çalıştırın:
        
    git config --global user.name <your name>
    git config --global user.email <your email address>
 
>Birden çok Git depoları için yürütülmekte olan, her birine yaparsanız aynı ad ve e-posta adresi kullanın. Powerbı panolar üzerinde birden çok depoları Git etkinlikleri izlemenize oluştururken aynı ad ve e-posta adresini kullanarak daha sonra uygun kanıtlar.


- Bizim kısaltılmış deposu adlarını kullanarak, bu komut dosyalarını elde ettikleri şudur:
    - LR1 - içeriği eklemek R1 ->
    - LR2 - içeriği eklemek R2 ->

## <a name="6-add-group-members-to-the-group-server"></a>6. Grup üyelerinin grup sunucuya ekleyin

Grup VSTS sunucunuzun giriş sayfasından tıklatın **dişli simgesi** kullanıcı adınızı yanındaki sağ üst köşesinde, ardından **güvenlik** sekmesi. Grubunuzun burada çeşitli izinlerle üyeler ekleyebilirsiniz.

![24](./media/group-manager-tasks/add_member_to_group.PNG) 


## <a name="next-steps"></a>Sonraki adımlar

Burada, rolleri ve görevleri takım veri bilimi işlem tarafından tanımlanan daha ayrıntılı açıklamaları bağlantıları verilmiştir:

- [Bir veri bilimi ekibi için Grup yöneticisi görevleri](group-manager-tasks.md)
- [Bir veri bilimi ekibi için takım sağlama görevleri](team-lead-tasks.md)
- [Proje veri bilimi ekibi için sağlama görevleri](project-lead-tasks.md)
- [Proje veri bilimi ekibi için tek tek Katkıda Bulunanlar](project-ic-tasks.md)