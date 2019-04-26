---
title: Team Data Science Process grup yöneticisi görevleri
description: Görev bir veri bilimi takım projesindeki bir grup yöneticisi için ana hat.
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/13/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: fb0482be1670a96befdd69a5356c9e21476d9f9f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60305416"
---
# <a name="tasks-for-a-group-manager-on-a-data-science-team-project"></a>Görevler için veri bilimi takım projesi üzerinde Grup Yöneticisi

Bu konuda anahatları grup yöneticisi beklenen görevleri tamamlamak için hist / veri bilimi organizasyonunda. Hedefi üzerinde standartlaştırır işbirliğine dayalı Grup ortamı oluşturmaktır [Team Data Science Process](overview.md) (TDSP). Bu işlemi, standart personel rolleri ve veri bilimi ekibi tarafından işlenen ilişkilendirilen görevlerinin bir özetini görmek [Team Data Science Process rolleri ve görevleri](roles-tasks.md).

**Grup Yöneticisi** kuruluştaki tüm veri bilimi biriminin yöneticisidir. Bir veri bilimi birimi, birden çok takıma, her biri ayrı iş sıralamasına koydunuz birden çok veri bilimi proje üzerinde çalışıyor olabilir. Grup Yöneticisi görevlerini bir yedek için temsilci, ancak rol ile ilişkili görevleri aynıdır. Aşağıdaki diyagramda gösterildiği gibi altı ana görevler şunlardır:

![0](./media/group-manager-tasks/tdsp-group-manager.png)


> [!NOTE] 
> Azure DevOps Hizmetleri'ni kullanarak aşağıdaki yönergelerde TDSP grubu ortamı ayarlama için gerekli olan adımları genel çizgileriyle belirtin. Biz nasıl biz Microsoft'ta TDSP uygulama olduğu için bu görevlerin Azure DevOps hizmetleriyle nasıl gerçekleştirileceğini belirtin. Grubunuz için başka bir kod barındırma platformu kullanılıyorsa, genel olarak grup yöneticisi tarafından tamamlanması gereken görevler değiştirmeyin. Ancak bu görevleri tamamlamak için yol farklı zordur.

1. Ayarlanan **Azure DevOps Hizmetleri** grubu için.
2. Oluşturma bir **grubu projesi** Azure DevOps hizmetlerine (Azure DevOps Hizmetleri kullanıcılar için)
3. Oluşturma **GroupProjectTemplate** depo
4. Oluşturma **GroupUtilities** depo
5. Çekirdek **GroupProjectTemplate** ve **GroupUtilities** TDSP depoları içerik Azure DevOps hizmetleriyle için depolar.
6. Ayarlanan **güvenlik denetimleri** için GroupProjectTemplate ve GroupUtilities depolarınıza erişmek takım üyeleri için.

Yukarıdaki adımların her birini ayrıntılı olarak açıklanmıştır. Ancak, ilk olarak, kısaltmalar ile tanıyın ve depoları ile çalışmak için ön koşullar tartışın.

### <a name="abbreviations-for-repositories-and-directories"></a>Depoları ve dizinleri kısaltmaları

Bu öğreticide, kısaltılmış depoları ve dizinler için kullanılır. Bu tanımları dizinlerini ve depoları işlemleri izlemenizi kolaylaştırır. Aşağıdaki bölümlerde bu gösterim kullanılır:

- **G1**: Proje şablonu deposuna geliştirilen ve Microsoft TDSP ekibi tarafından yönetilebilir.
- **G2**: Yardımcı programları depoyu geliştirilen ve Microsoft TDSP ekibi tarafından yönetilen.
- **R1**: Azure DevOps Grup sunucunuz üzerinde ayarladığınız Git GroupProjectTemplate havuzda.
- **R2**: Azure DevOps Grup sunucunuz üzerinde ayarladığınız Git GroupUtilities havuzda.
- **LG1** ve **LG2**: G1 ve G2 için sırasıyla kopyalama makinenizde yerel dizinler.
- **LR1** ve **LR2**: R1 ve R2 için sırasıyla kopyalama makinenizde yerel dizinler.

### <a name="pre-requisites-for-cloning-repositories-and-checking-code-in-and-out"></a>Ön koşullar depoların kopyalanması ve kodu açma ve kapatma

- Git makinenizde yüklü olması gerekir. Bir veri bilimi sanal makinesi (DSVM) kullanıyorsanız, Git önceden yüklenmiş ve hazırsınız demektir. Aksi takdirde bkz [platformları ve araçlarıyla ek](platforms-and-tools.md#appendix).
- Kullanıyorsanız bir **Windows DSVM**, ihtiyacınız [Git Credential Manager'ı (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) makinenizde yüklü. README.md dosyasında doğru aşağı kaydırın **indirme ve yükleme** tıklayın ve bölüm *en son yükleyicisi*. Bu adım, en son yükleyici sayfasına götürür. .Exe yükleyiciyi buradan indirin ve çalıştırın.
- Kullanıyorsanız **Linux DSVM'sini**, bir SSH ortak anahtarı üzerinde DSVM oluşturma ve Grup Azure DevOps hizmetlerinizi ekleyin. SSH hakkında daha fazla bilgi için bkz: **oluşturma SSH ortak anahtarı** konusundaki [platformları ve araçlarıyla ek](platforms-and-tools.md#appendix).


## <a name="1-create-account-on-azure-devops-services"></a>1. Azure DevOps Services hesabı oluşturma

Azure DevOps hizmetler aşağıdaki depolardan barındırır:

- **Ortak depoları grup**: Birden çok veri bilimi proje için bir grup içindeki birden çok takımı tarafından benimsenen genel amaçlı depolar. Örneğin, *GroupProjectTemplate* ve *GroupUtilities* depolar.
- **Takım depolarını**:  Bir gruptaki belirli takımlar için depolar. Bu depolar, belirli bir ekibin gereksinimini ve bu takım tarafından yürütülen birden fazla proje tarafından benimsenen, ancak genel yeterli bir veri bilimi grubu içinde birden çok takımlar için yararlı olabilir.
- **Proje depoları**: Depo belirli projeleri için kullanılabilir. Bu tür depolara birden çok proje ekibi tarafından gerçekleştirilen ve bir veri bilimi grubundaki birden fazla takımlar için yararlı olacak genel olmayabilir.


### <a name="setting-up-the-azure-devops-services-sign-into-your-microsoft-account"></a>Microsoft hesabınızda Azure DevOps Hizmetleri oturum ayarlama

Git [Visual Studio Team Services](https://www.visualstudio.com/), tıklayın **oturum** simgesine tıklayın veya dokunun ve Microsoft hesabınızda oturum.

![1](./media/group-manager-tasks/login.PNG)

Bir Microsoft hesabınız yoksa tıklayın **şimdi kaydolun** bir Microsoft hesabı oluşturun ve ardından bu hesabı kullanarak oturum açın.

Kuruluşunuzun Visual Studio/MSDN aboneliği varsa, yeşil tıklayın **iş veya Okul hesabınızla oturum** kutusuna ve bu abonelikle ilişkili kimlik bilgileriyle oturum açın.

![2](./media/group-manager-tasks/signin.PNG)



Oturum açtıktan sonra tıklayın **yeni hesap oluştur** aşağıdaki görüntüde gösterildiği gibi sağ üst köşedeki:

![3](./media/group-manager-tasks/create-account-1.PNG)

Oluşturmak istediğiniz Azure DevOps Hizmetleri için bilgileri doldurun **hesabınızı oluşturun** aşağıdaki değerlerle Sihirbazı:

- **Sunucu URL'si**: Değiştirin *mysamplegroup* ile kendi *sunucu adı*. Sunucunuzun URL'sini olacağı: *https://\<servername\>. visualstudio.com*.
- **Kodu şunu kullanarak Yönet:** Seçin  **_Git_**.
- **Proje adı:** Girin *GroupCommon*.
- **Şunu kullanarak çalışmayı Düzenle:** Seçin *Çevik*.
- **Projelerinizi barındırın:** Coğrafi konumu seçin. Bu örnekte, Seçtiğimiz *Orta Güney ABD*.

![4](./media/group-manager-tasks/fill-in-account-information.png)

> [!NOTE] 
> Tıkladıktan sonra aşağıdaki açılır penceresi görürseniz **yeni hesap oluşturun**, sonra da'ye tıklamanız **değiştirme ayrıntıları** listelenen tüm alanları görüntülemek için.

![5](./media/group-manager-tasks/create-account-2.png)


**Devam**’a tıklayın.

## <a name="2-groupcommon-project"></a>2. GroupCommon proje

**GroupCommon** sayfa (*https://\<servername\>.visualstudio.com/GroupCommon*), Azure DevOps hizmetlerinizi oluşturulduktan sonra açılır.

![6](./media/group-manager-tasks/server-created-2.PNG)

## <a name="3-create-the-grouputilities-r2-repository"></a>3. GroupUtilities (R2) depo oluştur

Oluşturulacak **GroupUtilities** Azure DevOps Hizmetleri altında (R2) depo:

- Açmak için **yeni depo Oluştur** Sihirbazı'nı tıklatın **yeni depo** üzerinde **sürüm denetimi** projenizin sekmesi.

  ![7](./media/group-manager-tasks/create-grouputilities-repo-1.png)

- Seçin *Git* olarak **türü**girin *GroupUtilities* olarak **adı**ve ardından **Oluştur**.

  ![8](./media/group-manager-tasks/create-grouputilities-repo-2.png)

Artık iki Git depoları görmelisiniz **GroupProjectTemplate** ve **GroupUtilities** sol sütununda **sürüm denetimi** sayfası:

![9](./media/group-manager-tasks/two-repo-under-groupCommon.PNG)


## <a name="4-create-the-groupprojecttemplate-r1-repository"></a>4. GroupProjectTemplate (R1) depo oluştur

Azure DevOps grubu sunucusu için depoları kurulumu iki görevden oluşur:

- Varsayılan Yeniden Adlandır **GroupCommon** depo***GroupProjectTemplate***.
- Oluşturma **GroupUtilities** depo hizmetlerdeki Azure DevOps projesi altındaki **GroupCommon**.

Yönergeler için ilk görev adlandırma kuralları ile ilgili açıklamalar veya bizim depoları ve dizinleri sonra bu bölümde yer alır. Aşağıdaki bölümde 4. adım için ikinci görev için yönergeler yer alır.

### <a name="rename-the-default-groupcommon-repository"></a>Varsayılan GroupCommon depoyu yeniden adlandır

Varsayılan yeniden adlandırmak için **GroupCommon** deposu olarak *GroupProjectTemplate* (olarak adlandırılan **R1** bu öğreticideki):

- Tıklayın **kod üzerinde işbirliği** üzerinde **GroupCommon** proje sayfası. Bu projenin varsayılan Git deposu sayfasına götürür **GroupCommon**. Şu anda bu Git deposu boş olur.

  ![10](./media/group-manager-tasks/rename-groupcommon-repo-3.png)

- Tıklayın **GroupCommon** (kırmızı kutu içinde aşağıdaki şekilde ile vurgulanan) sol üst köşedeki Git deposu sayfasında üzerinde **GroupCommon** seçip **depolarıYönet**(aşağıdaki şekilde yeşil bir kutuyla vurgulanmış). Bu yordamı getirir **denetim MASASI**.
- Seçin **sürüm denetimi** projenizin sekmesi.

  ![11](./media/group-manager-tasks/rename-groupcommon-repo-4.png)

- Tıklayın **...**  sağındaki **GroupCommon** depo Sol paneli ve seçim **yeniden adlandırma depo**.

  ![12](./media/group-manager-tasks/rename-groupcommon-repo-5.png)

- İçinde **GroupCommon depoyu yeniden adlandırabilir** açılarak girin Sihirbazı *GroupProjectTemplate* içinde **depo adı** kutusuna ve ardından **yeniden adlandır** .

  ![13](./media/group-manager-tasks/rename-groupcommon-repo-6.png)



## <a name="5-seed-the-r1--r2-repositories-on-the-azure-devops-services"></a>5. Azure DevOps hizmetler R1 ve R2 depoları temel

Yordamın bu aşamada, temel *GroupProjectTemplate* (R1) ve *GroupUtilities* önceki bölümde belirlediğiniz (R2) depolar. Bu depolar ile sağlanmış ***ProjectTemplate*** (**G1**) ve ***yardımcı programları*** (**G2**) tarafından yönetilen bir depo Microsoft Team Data Science Process için. Bu dengeli dağıtım tamamlandığında:

- R1 deponuzu G1 yapan dizin ve belge şablonları aynı dizi olacaktır
- Microsoft tarafından geliştirilen veri bilimi yardımcı programları kümesini içeren R2 deponuzu zordur.

Dengeli dağıtım yordam dizinleri yerel DSVM'ye Ara hazırlama siteler olarak kullanır. Bu bölümde izleyen adımlar şunlardır:

- G1 & G2 - kopyalanmış LG1 & LG2 -> için
- R1 ve R2 - kopyalanmış LR1 & LR2 -> için
- LG1 & LG2 - kopyalanmasını dosyaları LR1 & LR2 ->
- LR1 & LR2 özelleştirme (isteğe bağlı)
- LR1 & LR2 - içeriğini Ekle R1 ve R2 ->


### <a name="clone-g1--g2-repositories-to-your-local-dsvm"></a>Yerel DSVM'ye için G1 & G2 depoları kopyalayın

Bu adımda, yerel DSVM'ye LG1 ve LG2 içinde Team Data Science işlem (TDSP) ProjectTemplate depo (G1) ve hizmet programları (G2) TDSP GitHub depolarından klasörleri kopyalama:

- Tüm klonları depolar barındırmak için kök dizini olarak görev yapacak bir dizin oluşturun.
  -  Windows DSVM içinde bir dizin oluşturma *C:\GitRepos\TDSPCommon*.
  -  Linux DSVM'sini içinde bir dizin oluşturma *GitRepos\TDSPCommon* giriş dizininizde.

- Aşağıdaki komutları kümesini çalıştırmak *GitRepos\TDSPCommon* dizin.

  `git clone https://github.com/Azure/Azure-TDSP-ProjectTemplate`<br>
  `git clone https://github.com/Azure/Azure-TDSP-Utilities`

  ![14](./media/group-manager-tasks/two-folder-cloned-from-TDSP-windows.PNG)

- Bizim kısaltılmış depo adları kullanarak, bu betikleri elde ettikleri budur:
    - İçine kopyalanan G1 - LG1 ->
    - G2 - içine kopyalanan LG2 ->
- Kopyalama tamamlandıktan sonra iki dizin görmeye olmalısınız _ProjectTemplate_ ve _yardımcı programları_altında **GitRepos\TDSPCommon** dizin.

### <a name="clone-r1--r2-repositories-to-your-local-dsvm"></a>Yerel DSVM'ye için R1 ve R2 depoları kopyalayın

Bu adımda, GroupProjectTemplate depo (R1) ve (sırasıyla LR1 ve LR2, olarak bilinir) yerel dizinlerde GroupUtilities havuzda (R2) kopyalama **GitRepos\GroupCommon** DSVM üzerinde.

- R1 ve R2 depoları URL'lerini almak için Git, **GroupCommon** Azure DevOps Services giriş sayfasında. Bu URL genellikle sahip *https://\<uygulamanızın Azure DevOps Hizmetleri adı\>.visualstudio.com/GroupCommon*.
- Tıklayın **kod**.
- Seçin **GroupProjectTemplate** ve **GroupUtilities** depolar. Kopyalayabilir ve URL'lerin (HTTPS için Windows; Kaydet Linux için SSH) öğesinden **kopya URL'si** öğesinde açın, aşağıdaki komut dosyaları kullanmak için:

  ![15](./media/group-manager-tasks/find_https_ssh_2.PNG)

- Dönüştürme **GitRepos\GroupCommon** , Windows veya Linux DSVM'sini ve çalıştırma komutları R1 ve R2 bu dizine kopyalamak için aşağıdaki adımlardan birini dizin.

Windows ve Linux betikler şunlardır:

    # Windows DSVM

    git clone <the HTTPS URL of the GroupProjectTemplate repository>
    git clone <the HTTPS URL of the GroupUtilities repository>

![16](./media/group-manager-tasks/clone-two-empty-group-reo-windows-2.PNG)

    # Linux DSVM

    git clone <the SSH URL of the GroupProjectTemplate repository>
    git clone <the SSH URL of the GroupUtilities repository>

![17](./media/group-manager-tasks/clone-two-empty-group-reo-linux-2.PNG)

> [!NOTE] 
> Uyarı iletileri LR1 ve LR2 boş beklenir.

- Bizim kısaltılmış depo adları kullanarak, bu betikleri elde ettikleri budur:
    - R1 - içine kopyalanan LR1 ->
    - R2 - içine kopyalanan LR2 ->   


### <a name="seed-your-groupprojecttemplate-lr1-and-grouputilities-lr2"></a>GroupProjectTemplate (LR1) ve GroupUtilities (LR2) temel

Ardından, yerel makinenize GroupProjectTemplate ve GroupUtilities dizinlere altında altında GitRepos\TDSPCommon ProjectTemplate ve yardımcı programlar dizinleri (dışında .git dizinlerde meta veriler) içeriğini kopyalayın **GitRepos\ GroupCommon**. Bu adımda iki görevlerin şunlardır:

- İçinde GitRepos\TDSPCommon\ProjectTemplate dosyaları kopyalayın (**LG1**) GitRepos\GroupCommon\GroupProjectTemplate için (**LR1**)
- İçinde GitRepos\TDSPCommon\Utilities dosyaları kopyalayın (**LG2** GitRepos\GroupCommon\Utilities için (**LR2**).

Bu iki görevleri gerçekleştirmek için aşağıdaki komut kabuğu komut konsolunda (Linux) veya PowerShell konsolunu (Windows) çalıştırın. Tam yolları LG1, LR1 LG2 ve LR2 giriş istenir. Girdiğiniz yolları doğrulanır. Var olmayan bir dizin girişi yeniden giriş istenir.

    # Windows DSVM
    
    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_win.ps1" -outfile "tdsp_local_copy_win.ps1"
    .\tdsp_local_copy_win.ps1 1

![18](./media/group-manager-tasks/copy-two-folder-to-group-folder-windows-2.PNG)

Artık dosyaları (.git dizini dosyalarında) dışındaki dizinlerde LG1 ve LG1 LR1 ve LR2, sırasıyla kopyalandığını görebilirsiniz.

![19](./media/group-manager-tasks/copy-two-folder-to-group-folder-windows.PNG)

    # Linux DSVM

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_linux.sh"
    bash tdsp_local_copy_linux.sh 1

![20](./media/group-manager-tasks/copy-two-folder-to-group-folder-linux-2.PNG)

Şimdi iki klasör (dışında .git dizini dosyalarında) dosyalarında GroupProjectTemplate ve GroupUtilities sırasıyla kopyalandığını görürsünüz.

![21](./media/group-manager-tasks/copy-two-folder-to-group-folder-linux.PNG)

- Bizim kısaltılmış depo adları kullanarak, bu betikleri elde ettikleri budur:
    - LG1 - LR1 kopyalanmasını Dosya ->
    - LG2 - LR2 kopyalanmasını Dosya ->

### <a name="option-to-customize-the-contents-of-lr1--lr2"></a>Seçenek LR1 & LR2 içeriğini özelleştirmek için
    
İçeriğini LR1 ve LR2 grubunuzun belirli gereksinimlerini karşılayacak şekilde özelleştirmek istiyorsanız, burada uygun yordamı aşaması budur. Şablon belgeleri değiştirmek, dizin yapısını değiştirmek ve grubunuz geliştiren veya, tüm grup için yararlı olan mevcut programları ekleyin.

### <a name="add-the-contents-in-lr1--lr2-to-r1--r2-on-group-server"></a>İçeriği LR1 & LR2 R1 ve R2 için grubu sunucusuna ekleme

Artık depoları R1 ve R2 LR1 ve LR2 içeriği eklemeniz gerekir. Linux veya Windows PowerShell çalıştırabileceğiniz bash komutları git aşağıdadır.

GitRepos\GroupCommon\GroupProjectTemplate dizinden aşağıdaki komutları çalıştırın:

    git status
    git add .
    git commit -m"push from DSVM"
    git push

-M seçeneği, git işleme için bir ileti ayarlamanıza olanak tanır.

![22](./media/group-manager-tasks/push-to-group-server-2.PNG)

GroupProjectTemplate depodaki grubunuzun Azure DevOps Hizmetleri'nde dosyaları anında eşitlendiğini görebilirsiniz.

![23](./media/group-manager-tasks/push-to-group-server-showed-up-2.PNG)

Son olarak, geçin **GitRepos\GroupCommon\GroupUtilities** directory ve çalışma aynı kümesini git bash komutları:

    git status
    git add .
    git commit -m"push from DSVM"
    git push

> [!NOTE] 
> Bu bir Git deposuna işleme ilk kez kullanıyorsanız, genel parametrelerini yapılandırmanıza gerek *user.name* ve *user.email* çalıştırmadan önce `git commit` komutu. Aşağıdaki iki komutu çalıştırın:
>
>  `git config --global user.name <your name>`  
>  `git config --global user.email <your email address>`
>
> Birden çok Git deposu için yürütülmekte olan, her birine işlemesini oluştururken aynı ad ve e-posta adresi kullanın. Git etkinliklerinizi birden çok depolara göre izlemek için Power BI panoları oluşturduğunuzda aynı ad ve e-posta adresi kullanarak daha sonra uygun kanıtlar.


- Bizim kısaltılmış depo adları kullanarak, bu betikleri elde ettikleri budur:
    - LR1 - içeriğini Ekle R1 ->
    - LR2 - içeriğini Ekle R2 ->

## <a name="6-add-group-members-to-the-group-server"></a>6. Grup üyelerinin grup sunucuya ekleyin.

Grup Azure DevOps hizmetlerinizi'nın giriş sayfasından tıklayın **dişli simgesini** sağ üst köşesinde kullanıcı adınızı yanında, ardından **güvenlik** sekmesi. Grubunuz burada çeşitli izinlerine sahip üyeleri ekleyebilirsiniz.

![24](./media/group-manager-tasks/add_member_to_group.PNG)


## <a name="next-steps"></a>Sonraki adımlar

Team Data Science Process tarafından tanımlanan görevleri ve rolleri ayrıntılı açıklamaları için bağlantılar şunlardır:

- [Bir veri bilimi takım için Grup yöneticisi görevleri](group-manager-tasks.md)
- [Bir veri bilimi takım için takım sağlama görevleri](team-lead-tasks.md)
- [Proje için bir veri bilimi ekibi müşteri adayı görevleri](project-lead-tasks.md)
- [Bir veri bilimi takım için proje bağımsız katılımcıları](project-ic-tasks.md)
