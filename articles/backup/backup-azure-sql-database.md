---
title: SQL Server veritabanlarını azure'a yedekleyin | Microsoft Docs
description: Bu öğreticide, SQL Server'ı Azure'a yedekleme açıklanmaktadır. Makalede, SQL Server kurtarma de açıklanmaktadır.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: tutorial
ms.date: 02/19/2018
ms.author: raynew
ms.openlocfilehash: b08d4e55deac4c9dd6a11fc9a278bdcfaa739627
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58224320"
---
# <a name="back-up-sql-server-databases-on-azure-vms"></a>Azure VM’lerindeki SQL Server veritabanlarını yedekleme 

SQL Server veritabanları, düşük kurtarma noktası hedefi (RPO) ve uzun süreli saklama gerektiren kritik iş yükleri değildir. Kullanarak Azure Vm'lerinde çalışan SQL Server veritabanlarını yedekleyebilirsiniz [Azure Backup](backup-overview.md). 

Bu öğreticide bir Azure Backup kurtarma Hizmetleri kasası için bir Azure sanal makinesinde çalıştırılan bir SQL Server veritabanını yedekleyin gösterilmektedir. Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Bir SQL Server örneğini ayarlama yedekleme önkoşulları doğrulayın.
> * Oluşturun ve kasa yapılandırın.
> * Veritabanlarını bulmak ve yedeklemeler ayarlayabilir.
> * Veritabanları için otomatik korumayı ayarlayın.

> [!NOTE]
> Bu özellik şu anda genel Önizleme aşamasındadır.

## <a name="before-you-start"></a>Başlamadan önce


- Azure'da çalışan bir SQL Server örneğine sahip olduğunuzdan emin olun. Hızla [bir SQL Server örneği oluşturma](../sql-database/sql-database-get-started-portal.md) Azure Marketi'nde.
- Genel Önizleme sınırlamaları gözden geçirin.
- Senaryo destek gözden geçirin.
- [Sık sorulan soruları gözden](faq-backup-sql-server.md) bu senaryo hakkında.

## <a name="preview-limitations"></a>Önizleme sınırlamaları

Bu genel Önizleme, bir dizi sınırlandırması vardır.

- SQL Server çalıştıran bir VM, Azure genel IP adresleri erişmek için Internet bağlantısı gerektirir. 
- Bir kasadaki 2.000 SQL Server veritabanlarını yedekleyebilirsiniz. Daha fazla varsa, başka bir kasa oluşturun. 
- Yedekleri [dağıtılmış kullanılabilirlik grupları](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/distributed-availability-groups?view=sql-server-2017) tam olarak çalışmaz.
- SQL Server her zaman üzerinde yük devretme kümesi örnekleri (Fcı'lerde) için yedekleme desteklenmez.
- SQL Server Yedekleme, portalda yapılandırın. Şu anda, Azure PowerShell, CLI veya REST API ile yedekleme yapılandıramazsınız.
- FCI yansıtma veritabanı, veritabanı anlık görüntüleri ve veritabanları için yedekleme ve geri yükleme işlemleri desteklenmez.
- Veritabanları ile çok sayıda dosya korunamaz. Desteklenen dosya sayısı belirleyici değildir. Dosya sayısına bağlıdır ve ayrıca dosyaların yolunu uzunluğuna bağlıdır. 

Gözden geçirme [sık sorulan sorular](faq-backup-sql-server.md) SQL Server veritabanlarını yedekleme hakkında.
## <a name="scenario-support"></a>Senaryo desteği

**Destek** | **Ayrıntılar**
--- | ---
**Desteklenen dağıtımlar** | Market Azure Vm'leri ve Market dışı (SQL Server el ile yüklenen) sanal makineleri desteklenir.
**Desteklenen coğrafi bölgeler** | Avustralya Güneydoğu (ASE); Brezilya Güney (BRS); Kanada Orta (CNC); Kanada Doğu (CE); Orta ABD (CUS); Doğu Asya (EA); Doğu Avustralya (AE); Doğu ABD (EUS); Doğu ABD 2 (EUS2); Hindistan Orta (Inc); Hindistan Güney (INS); Japonya Doğu (JPE); Japonya Batı (JPW); Kore Orta (KRC); Kore Güney (KRS); Kuzey Orta ABD (NCUS); Kuzey Avrupa (NE); Güney Orta ABD (SCUS); Güney Doğu Asya (SEA); UK Güney (UKS); UK Batı (UKW); Batı Orta ABD (WCUS); Batı Avrupa (WE); Batı ABD (WUS); Batı ABD 2 (WUS 2)
**Desteklenen işletim sistemleri** | Windows Server 2016, Windows Server 2012 R2, Windows Server 2012<br/><br/> Linux şu anda desteklenmemektedir.
**Desteklenen SQL Server sürümleri** | SQL Server 2017, SQL Server 2016, SQL Server 2014, SQL Server 2012.<br/><br/> Enterprise, Standard, Web, Developer, Express.

## <a name="prerequisites"></a>Önkoşullar

SQL Server veritabanınızı yedekleyin önce aşağıdaki koşulları denetleyin:

- Tanımlamak veya [bir kurtarma Hizmetleri kasası oluşturma](backup-azure-sql-database.md#create-a-recovery-services-vault) aynı bölge veya SQL Server örneğini barındıran VM yerel ayar.
- [VM izinlerini denetleyin](#fix-sql-sysadmin-permissions) SQL veritabanlarını yedeklemek için gerekli.
- VM sahip olduğunu doğrulayın [ağ bağlantısı](backup-azure-sql-database.md#establish-network-connectivity).
- SQL Server veritabanları ile uyumlu olarak adlandırılan onay [Azure Backup için adlandırma kuralları](backup-azure-sql-database.md).
- Veritabanı için etkin diğer yedekleme çözümleri yoksa doğrulayın. Bu senaryoyu ayarlamak, önce diğer tüm SQL Server yedeklemeleri devre dışı bırakın. Herhangi bir çakışma olmadan VM üzerinde çalışan SQL Server veritabanı için Azure Backup ile birlikte bir Azure VM için Azure Backup'ı etkinleştirebilirsiniz.

### <a name="establish-network-connectivity"></a>Ağ bağlantısı kurma

Tüm işlemler için SQL Server sanal makinesi sanal makine Azure genel IP adreslerine bağlantısı gerekir. VM işlemleri (veritabanı bulma, yedeklemeleri yapılandırma, yedeklemeler zamanlamak, Kurtarma noktalarını geri) genel IP adreslerine bağlantısı olmadan başarısız. Bu seçeneklerden biri ile bağlantı kurar:

- **Azure veri merkezi IP aralıklarına izin verin**: İzin [IP aralıklarını](https://www.microsoft.com/download/details.aspx?id=41653) indirmesindeki. Bir ağ güvenlik grubu (NSG) erişimi için kullandığınız `Set-AzureNetworkSecurityRule` cmdlet'i.
- **Trafiği yönlendirmek için bir HTTP Ara sunucusunu dağıtmak**: Bir Azure sanal makinesinde bir SQL Server veritabanını yedeklediğinizde, sanal makinedeki yedekleme uzantısı yönetim komutları için Azure Backup göndermek için ve Azure depolama alanına veri göndermek için HTTPS API'leri kullanır. Backup uzantısı, Azure Active Directory (Azure AD) kimlik doğrulaması için de kullanır. HTTP proxy üzerinden bu üç hizmeti yedekleme uzantısını trafiği yönlendirme. Genel internet erişimi için yapılandırılan tek bileşen uzantısıdır.

Her bir seçeneğin avantajları ve dezavantajları vardır.

**Seçenek** | **Avantajları** | **Dezavantajları**
--- | --- | ---
IP aralıklarına izin verin | Ek maliyet olmadan. | IP adresi aralıklarını zamanla değişir olduğundan yönetmek için karmaşık. <br/><br/> Tüm Azure, Azure depolama için yalnızca erişim sağlar.
Bir HTTP Ara sunucusunu kullanacak   | Depolama üzerinde ayrıntılı denetim proxy'sinde URL'leri izin verilir. <br/><br/> Vm'leri tek noktası internet erişimi. <br/><br/> Azure IP adresi değişiklikleri tabi değildir. | Bir VM ile Ara yazılım çalıştırmak için ek ücretler. 

### <a name="set-vm-permissions"></a>VM izinleri ayarlama

Azure yedekleme, SQL Server veritabanı için yedeklemeyi yapılandırdığınız sırada birkaç şey yapar:

- Azure yedekleme ekler **AzureBackupWindowsWorkload** uzantısı.
- Azure Backup, hesap oluşturur **NT SERVICE\AzureWLBackupPluginSvc** sanal makine üzerindeki veritabanlarını bulmak için. Bu hesap, yedekleme ve geri yükleme için kullanılır ve SQL sysadmin izinleri gerektirir.
- Azure Backup yararlanır **NT AUTHORITY\SYSTEM** veritabanı bulma ve sorgulama için hesap. Bu hesabın SQL Server'da ortak bir oturum açma olması gerekir.

SQL Server VM Azure Market'te oluşturmadıysanız, alabileceğiniz bir **UserErrorSQLNoSysadminMembership** hata. Bu hata ortaya çıkarsa [bu yönergeleri izleyin](#fix-sql-sysadmin-permissions).

### <a name="verify-database-naming-guidelines-for-azure-backup"></a>Azure Backup için veritabanı adlandırma yönergeleri doğrulayın

Veritabanı adları için aşağıdakileri kaçının:

* Baştaki/sondaki boşluk
* Sondaki ünlem işareti (!)
* Köşeli ayraç (])

Azure tablo desteklenmeyen karakterler için diğer ad kullanımı sahibiz ancak bunları kaçınmanızı öneririz. [Daha fazla bilgi edinin](https://docs.microsoft.com/rest/api/storageservices/Understanding-the-Table-Service-Data-Model?redirectedfrom=MSDN).

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>SQL Server veritabanlarını Bul

Sanal makinede çalışan veritabanları keşfedin. 

1. İçinde [Azure portalında](https://portal.azure.com), veritabanını yedeklemek için kullandığınız bir kurtarma Hizmetleri kasasını açın.

1. Üzerinde **kurtarma Hizmetleri kasası** panoyu seçin **yedekleme**. 

   ![Yedekleme yedekleme hedefi menüsünü açmak için seçin](./media/backup-azure-sql-database/open-backup-menu.png)

1. İçinde **yedekleme hedefi**ayarlayın **iş yükünüz çalıştığı** için **Azure** (varsayılan).

1. İçinde **neleri yedeklemek istiyorsunuz**seçin **Azure VM'de SQL Server**.

    ![SQL Server Azure VM için yedeklemeyi seçin.](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)


1. İçinde **yedekleme hedefi** > **VM'lerin içinde VT Bul**seçin **bulmayı Başlat** aboneliğinde korunmayan VM'ler için aranacak. Bu Abonelikteki korumasız sanal makinelerin sayısına bağlı olarak biraz zaman alabilir.

    ![Yedekleme Beklemede sırasında VM'lerin içinde VT arayın](./media/backup-azure-sql-database/discovering-sql-databases.png)

1. VM listesinde SQL Server veritabanını çalıştıran VM'yi seçin > **Bul DBs**.

1. İzleme veritabanı bulgusunda **bildirimleri** alan. Uygulamanın kaç tane sanal makinede veritabanlarıdır bağlı olarak, bitirilecek işi biraz sürebilir. Seçili veritabanlarındaki bulunduğunda bir başarı iletisi görünür.

    ![Dağıtım başarılı iletisi](./media/backup-azure-sql-database/notifications-db-discovered.png)

    - Korunmayan VM'ler sonra adı ve kaynak grubu tarafından listelenen bulma, listede görünmesi gerekir.
    - Bir VM beklediğiniz gibi listede yoksa, zaten bir kasada yedekleneceğini olup olmadığını denetleyin.
    - Birden çok VM aynı ada sahip olabilir, ancak farklı kaynak gruplarına ait. 

1. SQL Server veritabanını çalıştıran VM'yi seçin > **Bul DBs**. 
1. Azure yedekleme, VM üzerindeki tüm SQL Server veritabanlarını bulur. Bulma işlemi sırasında aşağıdaki etkinlik arka planda gerçekleşir:

    - Azure yedekleme, VM için iş yükü yedekleme kasası ile kaydeder. Yalnızca bu kasaya kayıtlı VM üzerindeki tüm veritabanlarını yedeklenebilir.
    - Azure Backup'ı yükler **AzureBackupWindowsWorkload** VM uzantısı. SQL veritabanı'nda yüklü aracı yok.
    - Azure Backup, hizmet hesabı oluşturur **NT Service\AzureWLBackupPluginSvc** VM üzerinde.
      - Tüm yedekleme ve geri yükleme işlemleri, hizmet hesabı kullanın.
      - **NT Service\AzureWLBackupPluginSvc** SQL sysadmin izinleri olması gerekir. Azure Market'te oluşturulan tüm SQL Server Vm'lerinin gelir **SqlIaaSExtension** yüklü. **AzureBackupWindowsWorkload** uzantısı kullanan **SQLIaaSExtension** otomatik olarak gerekli izinleri almak için.
    - Marketten VM oluşturmadıysanız, VM yok **SqlIaaSExtension** yüklü ve bulma işlemi başarısız oluyor **UserErrorSQLNoSysAdminMembership** hata iletisi. Bu sorunu gidermek için [#fix-sql-sysadmin-izinleri] yönergeleri izleyin.

        ![VM ve veritabanı seçin](./media/backup-azure-sql-database/registration-errors.png)



## <a name="configure-a-backup-policy"></a>Bir yedekleme ilkesi yapılandırma 

Yedekleme yüklerini en iyi duruma getirmek için Azure Backup bir en fazla veritabanı sayısı 50'ye bir yedekleme işi ayarlar.

- 50'den fazla veritabanlarını korumak için birden çok yedekleme yapılandırın.
- Alternatif olarak, otomatik koruma etkinleştirebilirsiniz. Otomatik koruma var olan bir Git veritabanlarında korur ve kullanılabilirlik grubunun örneğine eklenen yeni veritabanları otomatik olarak korur.


Yedekleme aşağıdaki gibi yapılandırın:

1. Kasa panosunda seçin **yedekleme**. 

   ![Yedekleme yedekleme hedefi menüsünü açmak için seçin](./media/backup-azure-sql-database/open-backup-menu.png)

1. İçinde **yedekleme hedefi**ayarlayın **iş yükünüz çalıştığı?** için **Azure**.

1. İçinde **neleri yedeklemek istiyorsunuz?** seçin **Azure VM'de SQL Server**.

    ![SQL Server Azure VM için yedeklemeyi seçin.](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    
1. İçinde **yedekleme hedefi**seçin **yedeklemeyi Yapılandır**.

    ![Seçin yedeklemeyi yapılandırma](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

    ![Tek başına veritabanları ile tüm SQL Server örneklerini görüntüleme](./media/backup-azure-sql-database/list-of-sql-databases.png)

1. Korumak istediğiniz tüm veritabanlarını seçin > **Tamam**.

    ![Veritabanını koruma](./media/backup-azure-sql-database/select-database-to-protect.png)

    - (Tek başına ve kullanılabilirlik grupları) tüm SQL Server örnekleri gösterilir.
    - Filtre uygulamak için örnek adı veya kullanılabilirlik grubu solundaki aşağı oku seçin.

1. İçinde **yedekleme** menüsünde **yedekleme İlkesi**. 

    ![Yedekleme ilkesini seçin](./media/backup-azure-sql-database/select-backup-policy.png)

1. İçinde **yedekleme ilkesi seçmek**bir ilkeyi seçin ve ardından **Tamam**.

    - Varsayılan ilkeyi seçin: **HourlyLogBackup**.
    - SQL için daha önce oluşturulmuş mevcut bir yedekleme İlkesi'ni seçin.
    - [Yeni bir ilke tanımlama](backup-azure-sql-database.md#configure-a-backup-policy) RPO ve bekletme aralığını esas alarak.
    - Önizleme süresince, var olan bir yedekleme İlkesi düzenleyemezsiniz.
    
1. İçinde **yedekleme** menüsünde **yedeklemeyi etkinleştir**.

    ![Seçilen yedekleme İlkesi'ni etkinleştir](./media/backup-azure-sql-database/enable-backup-button.png)

1. Yapılandırma ilerlemeyi **bildirimleri** portalının alan.

    ![Bildirim alanı](./media/backup-azure-sql-database/notifications-area.png)

### <a name="create-a-backup-policy"></a>Bir yedekleme ilkesi oluşturma

Bir yedekleme İlkesi yedeklemeleri ne zaman alınır ve ne kadar süreyle tutulur belirler. 

- Kasa düzeyinde bir ilke oluşturulur.
- Birden çok kasa ve aynı yedekleme ilkesine kullanabilirsiniz, ancak her kasa için yedekleme ilkesini uygulama.
- Bir yedekleme ilkesi oluşturduğunuzda, bir günlük tam yedekleme varsayılandır.
- Tam yedekleme haftalık olarak gerçekleşecek şekilde yapılandırırsanız, ancak yalnızca bir değişiklik yedeği ekleyebilirsiniz.
- [Hakkında bilgi edinin](backup-architecture.md#sql-server-backup-types) yedekleme ilkeleri farklı türde.

Bir yedekleme ilkesi oluşturmak için aşağıdaki adımları izleyin:

1. Kasada seçin **yedekleme ilkeleri** > **Ekle**.
1. İçinde **Ekle** menüsünde **Azure VM'de SQL Server**. Bu ilke türü tanımlar.

   ![Yeni bir yedekleme ilkesi için bir ilke türü seçin](./media/backup-azure-sql-database/policy-type-details.png)

1. İçinde **ilke adı**, yeni ilke için bir ad girin. 
1. İçinde **tam yedekleme İlkesi**seçin bir **yedekleme sıklığı**. Seçin **günlük** veya **haftalık**.

    - İçin **günlük**saat dilimini başlamak yedekleme ve saat seçin.
    - Tam yedekleme çalıştırmanız gerekir. Devre dışı bırakamazlar **tam yedekleme** seçeneği.
    - Seçin **tam yedekleme** ilkesini görüntülemek için. 
    - Günlük tam yedekleme için fark yedeklemelerinin oluşturulamıyor.
    - İçin **haftalık**, günü, haftanın günü, saat ve başlamak yedekleme için saat dilimini seçin.

     ![Yeni bir yedekleme İlkesi alanlar](./media/backup-azure-sql-database/full-backup-policy.png)  

1. İçinde **bekletme aralığı**, tüm seçenekleri varsayılan olarak seçilidir. Temizleyin ve kullanmak istediğiniz belirli aralıklarda istemediğiniz herhangi bir istenmeyen bekletme aralığını kısıtlar. 

    - Kurtarma noktalarının bekletme, bekletme aralığına göre etiketlenir. Örneğin, bir günlük tam yedekleme öğesini seçerseniz, yalnızca bir tam yedekleme her gün tetiklenir.
    - Yedekleme haftalık bir bekletme aralığı ve haftalık bekletme ayarınızı bağlı olarak belirli bir günde etiketlenmiş ve korunur.
    - Aylık ve yıllık bekletme aralıkları benzer şekilde davranır.

   ![Bekletme aralığı aralığı ayarları](./media/backup-azure-sql-database/retention-range-interval.png)

    

1. İçinde **tam yedekleme İlkesi** menüsünde **Tamam** ayarları kabul etmek için.
1. Fark yedekleme ilkesi eklemek için seçin **fark yedekleme**. 

   ![Bekletme aralığı aralığı ayarları](./media/backup-azure-sql-database/retention-range-interval.png)
   ![fark yedekleme İlkesi menüsü açın](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

1. İçinde **fark yedekleme İlkesi**seçin **etkinleştirme** sıklığı ve bekletme denetimleri açın. 

    - En fazla günde bir fark yedekleme tetikleyebilirsiniz.
    - Değişiklik yedekleri, en fazla 180 gün için saklanabilir. Daha uzun bekletme süresi gerekiyorsa, tam yedeklemeler kullanmanız gerekir.

1. Seçin **Tamam** ilkeyi kaydedin ve ana menüye dön **yedekleme İlkesi** menüsü.

1. İşlem günlüğü yedekleme ilkesi eklemek için seçin **günlük yedekleme**.
1. İçinde **günlük yedeği**seçin **etkinleştirme**ve ardından sıklığı ve bekletme denetimleri ayarlayın. Günlük yedeklemeler ve 35 güne kadar saklanabilir 15 dakikada bir sıklıkta oluşabilir.
1. Seçin **Tamam** ilkeyi kaydedin ve ana menüye dön **yedekleme İlkesi** menüsü.

    ![Günlük yedekleme ilkesini Düzenle](./media/backup-azure-sql-database/log-backup-policy-editor.png)

1. İçinde **yedekleme İlkesi** menüsünde etkinleştirilip etkinleştirilmeyeceğini seçin **SQL yedekleme sıkıştırması**.
    - Sıkıştırma, varsayılan olarak devre dışıdır.
    - Arka uçta Azure Backup SQL yerel yedekleme sıkıştırmasını kullanır.

1. Yedekleme İlkesi düzenlemeleri tamamladığınızda seçin **Tamam**.



## <a name="enable-auto-protection"></a>Otomatik korumayı etkinleştir  

Otomatik olarak tüm veritabanlarında ve gelecekte bir tek başına SQL Server örneğine veya SQL Server Always On kullanılabilirlik grubuna eklenen veritabanlarını yedeklemek otomatik korumayı etkinleştirin. 

- Otomatik korumayı üzerinde ve bir ilkeyi seçtiğinizde, korumalı veritabanlarında önceki ilkesini kullanacak şekilde devam eder.
- Otomatik koruma için seçtiğiniz veritabanlarını sayısına bir sınır yoktur.

Otomatik korumayı etkinleştirmek için bu adımları izleyin:

1. İçinde **yedeklenecek öğeleri**, otomatik korumayı etkinleştirmek istediğiniz örneği seçin.
1. Altındaki açılan menüyü seçin **Autoprotect**ve kümesine **üzerinde**. Sonra **Tamam**’ı seçin.

    ![Always On kullanılabilirlik grubunda otomatik korumayı etkinleştir](./media/backup-azure-sql-database/enable-auto-protection.png)

1. Yedekleme için tüm veritabanlarını birlikte yapılandırılan ve izlenebilir **yedekleme işleri**.


Otomatik korumayı devre dışı bırakmanız gerekirse, altında örnek adını seçin **yedeklemeyi Yapılandır**seçip **devre dışı Autoprotect** örneği. Tüm veritabanlarını yedeklemek devam eder. Ancak, gelecekteki veritabanlarının otomatik olarak korunması gerekmez.

![Bu örneği otomatik korumasını devre dışı](./media/backup-azure-sql-database/disable-auto-protection.png)


## <a name="fix-sql-sysadmin-permissions"></a>SQL sysadmin izinleri Düzelt

İzinler nedeniyle hatayı düzeltmeniz gerekiyorsa bir **UserErrorSQLNoSysadminMembership** hata, şu adımları izleyin:

1. Bir hesap ile SQL Server sysadmin izinleri SQL Server Management Studio (SSMS) oturum açmak için kullanın. Özel izinler gerekmedikçe, Windows kimlik doğrulama çalışması gerekir.
1. SQL Server örneğinde açın **güvenlik/oturum açma bilgileri** klasör.

    ![Hesapları görmek için güvenlik/oturum açma bilgileri klasörünü açın](./media/backup-azure-sql-database/security-login-list.png)

1. Sağ **oturumları** klasörü ve select **yeni oturum açma**. İçinde **oturum açma - yeni**seçin **arama**.

    ![Oturum Aç - yeni iletişim kutusu, Ara'yı seçin](./media/backup-azure-sql-database/new-login-search.png)

1. Windows sanal hizmet hesabı **NT SERVICE\AzureWLBackupPluginSvc** SQL bulma aşamasından ve sanal makine kaydı sırasında oluşturuldu. Gösterildiği gibi hesap adını girin **Seçilecek nesne adını girin**. Seçin **Adları Denetle** adı çözümlenemedi. **Tamam**’ı seçin.

    ![Bilinmeyen hizmet adını çözümlemek için adları denetle seçin](./media/backup-azure-sql-database/check-name.png)


1. İçinde **sunucu rolleri**, emin **sysadmin** rolü seçilir. **Tamam**’ı seçin. Gerekli izinleri olması gerekir.

    ![Sysadmin sunucu rolünün seçili olduğundan emin olun](./media/backup-azure-sql-database/sysadmin-server-role.png)

1. Veritabanı kurtarma Hizmetleri kasası ile ilişkilendirin. Azure portalında, **korumalı sunucuların** listesinde, bir hata durumunda sunucusuna sağ tıklayın > **yeniden Bul DBs**.

    ![Sunucunun uygun izinlere sahip olun](./media/backup-azure-sql-database/check-erroneous-server.png)

1. İlerleme iade **bildirimleri** alan. Seçili veritabanlarındaki bulunduğunda bir başarı iletisi görünür.

    ![Dağıtım başarılı iletisi](./media/backup-azure-sql-database/notifications-db-discovered.png)

Ayrıca etkinleştirebilirsiniz [otomatik korumayı](backup-azure-sql-database.md#enable-auto-protection) tüm örneği veya Always On kullanılabilirlik grubu. Bunu etkinleştirmek için işaretleyin **ON** seçeneği karşılık gelen açılır penceresinde **AUTOPROTECT** sütun. [Otomatik korumayı](backup-azure-sql-database.md#enable-auto-protection) özelliği, var olan tüm veritabanlarını koruma sağlar. Ayrıca otomatik olarak, gelecekte bu örneği veya kullanılabilirlik grubuna eklenecek yeni veritabanlarını korur.  

   ![Always On kullanılabilirlik grubunda otomatik korumayı etkinleştir](./media/backup-azure-sql-database/enable-auto-protection.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Hakkında bilgi edinin](restore-sql-database-azure-vm.md) yedeklenen SQL Server veritabanlarını geri yükleme.
- [Hakkında bilgi edinin](manage-monitor-sql-database-backup.md) yedeklenen SQL Server veritabanlarını yönetme.

