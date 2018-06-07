---
title: Azure SQL Server veritabanına yedekleme | Microsoft Docs
description: Bu öğretici için Azure SQL Server Yedekleme açıklanmaktadır. Makale ayrıca SQL Server kurtarma açıklar.
services: backup
documentationcenter: ''
author: markgalioto
manager: carmonm
editor: ''
keywords: ''
ms.assetid: ''
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 6/1/2018
ms.author: markgal;anuragm
ms.custom: ''
ms.openlocfilehash: f48cbdb41f8ad7a3bad4546fa5cb77cf66780bed
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34808511"
---
# <a name="back-up-sql-server-database-in-azure"></a>Azure SQL Server veritabanını yedekleyin

SQL Server, önemli iş yüklerine düşük kurtarma noktası hedefi (RPO) ve uzun vadeli bekletme gerektiren veritabanlarıdır. Azure Backup karmaşık yedekleme sunucusu yok, hiçbir yönetim aracısı veya yedekleme depolama yönetmek için anlamına gelir sıfır altyapısı gerektiren bir SQL Serveryedeği çözüm sağlar. Azure yedekleme, tüm SQL Server'lar veya hatta farklı iş yükleri arasında yedeklemeleriniz için merkezi yönetim sağlar.

 Bu makalede, öğreneceksiniz:

> [!div class="checklist"]
> * SQL Server'ı Azure'a yedeklemek için önkoşulları
> * Oluşturma ve bir kurtarma Hizmetleri kasası kullanma
> * SQL Server Veritabanı Yedeklemeleri Yapılandırma
> * Kurtarma noktaları için bir yedekleme (veya bekletme) ilkesi ayarlama
> * Veritabanını geri yükleme

Bu makaledeki yordamları başlatmadan önce Azure'da çalışan bir SQL Server olmalıdır. [SQL Market sanal makineler bir SQL Server hızlı bir şekilde oluşturmak için kullanabileceğiniz](../sql-database/sql-database-get-started-portal.md).

## <a name="public-preview-limitations"></a>Genel Önizleme sınırlamaları

Aşağıdaki öğeler, genel Önizleme için bilinen sınırlamalardır.

- SQL sanal makinesi Azure genel IP adreslerine erişmek için Internet bağlantısı gerektirir. Daha fazla ayrıntı için bölümüne bakın [ağ bağlantısı kurmak](backup-azure-sql-database.md#establish-network-connectivity).
- Bir kurtarma Hizmetleri kasasına en fazla 2000 SQL veritabanlarını koruyabilir. Ek SQL veritabanlarını ayrı bir kurtarma Hizmetleri kasasına depolanması gerekir.
- [Dağıtılmış kullanılabilirlik grupları yedekleme sınırlamalara sahiptir](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/distributed-availability-groups?view=sql-server-2017).
- SQL yük devretme kümesi örneği (FCI) desteklenmiyor.
- SQL Server veritabanlarını korumak için Azure Yedekleme'yi yapılandırmak için Azure Portalı'nı kullanın. Azure PowerShell'i, CLI ve REST API desteği şu anda kullanılamıyor.

## <a name="supported-azure-geos"></a>Desteklenen Azure GEOs

- Avustralya Güneydoğu (ASE) 
- Brezilya Güney (BRS)
- Kanada Orta (CNC)
- Kanada Doğu (CE)
- Orta ABD (CUS)
- Doğu Asya (EA)
- Doğu Avustralya (AE) 
- Doğu ABD (EUS)
- Doğu ABD 2 (EUS2)
- Japonya Doğu (JPE)
- Japonya Batı (JPW)
- Hindistan Orta (INC) 
- Hindistan Güney (INS)
- Kore Orta (KRC)
- Kore Güney (KRS)
- Orta Kuzey ABD (NCUS) 
- Kuzey Avrupa (NE) 
- Orta Güney ABD (SCUS) 
- Güneydoğu Asya (SEA)
- UK Güney (UKS) 
- UK Batı (UKW) 
- Batı Avrupa (WE) 
- Batı ABD (WUS)
- Batı Orta ABD (WCUS)
- Batı ABD 2 (WUS 2) 

## <a name="supported-operating-systems-and-versions-of-sql-server"></a>Desteklenen işletim sistemleri ve SQL server sürümleri

Aşağıdaki desteklenen işletim sistemleri ve SQL Server sürümleri SQL Market'te Azure sanal makineleri uygulamak ve (SQL Server elle yüklendiği) Market olmayan sanal makineleri.

### <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

Linux şu anda desteklenmiyor.

### <a name="supported-versionseditions-of-sql-server"></a>SQL Server'ın desteklenen sürümleri

- SQL 2012 Enterprise, Standard, Web, geliştirici, Express
- SQL 2014 Enterprise, Standard, Web, geliştirici, Express
- SQL 2016 Enterprise, Standard, Web, geliştirici, Express
- SQL 2017 Enterprise, Standard, Web, geliştirici, Express


## <a name="prerequisites-for-using-azure-backup-to-protect-sql-server"></a>SQL Server korumak için Azure Backup'ı kullanma önkoşulları 

SQL Server veritabanınızı yedeklemeniz önce aşağıdaki koşulların denetleyin. :

- Tanımlamak veya [bir kurtarma Hizmetleri kasası oluşturma](backup-azure-sql-database.md#create-a-recovery-services-vault) aynı bölge veya yerel ayar, sanal makineyi barındıran SQL Server.
- [Sanal makinenin izinlerini denetleyin](backup-azure-sql-database.md#set-permissions-for-non-marketplace-sql-vms) SQL veritabanlarını yedeklemek için gerekli.
- [SQL sanal makinesi sahip ağ bağlantısı](backup-azure-sql-database.md#establish-network-connectivity).

Bu koşullar, ortamınızda yoksa, bölümüne devam [bir SQL veritabanını korumak için kasanızı yapılandırma](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database). Önkoşullar yoksa bu bölümü okumadan devam edin.


## <a name="establish-network-connectivity"></a>Ağ bağlantısı kurma

Tüm işlemler için SQL sanal makinesi Azure genel IP adreslerine bağlantısı gerekir. Genel IP adreslerine bağlantısı olmadan SQL sanal makine işlemlerini (örneğin, veritabanı bulma, yapılandırma yedekleme, zamanlanmış yedeklemeler, geri yükleme kurtarma noktaları vb.) başarısız. Yedekleme trafiği için açık bir yol sağlamak için aşağıdaki seçeneklerden birini kullanın:

- Beyaz liste Azure veri merkezi IP aralıkları - beyaz liste Azure veri merkezi IP aralıkları, kullanın [merkezi IP aralıkları ve yönergeleri hakkında ayrıntılar için indirme](https://www.microsoft.com/download/details.aspx?id=41653). 
- Yönlendirme trafiği için bir HTTP proxy sunucusu dağıtma - VM bir SQL veritabanında yedekliyorsanız, yedekleme uzantısını VM yönetimi komutları Azure Backup ve Azure Storage veri göndermek için HTTPS API'lerini kullanır. Backup uzantısı, ayrıca kimlik doğrulaması için Azure Active Directory (AAD) kullanır. Genel internet erişimi için yapılandırılmış tek bileşen olduğundan HTTP proxy üzerinden üç bu hizmetler için backup uzantısı trafiği yönlendirmek.

Seçenekler arasında bileşim şunlardır: yönetilebilirlik, ayrıntılı bir denetim ve maliyet.

> [!NOTE]
>Azure Backup hizmeti etiketleri genel kullanılabilirlik tarafından kullanılabilir olması gerekir.
>

| Seçenek | Avantajları | Olumsuz yönleri |
| ------ | ---------- | ------------- |
| Beyaz liste IP aralıkları | Hiçbir ek maliyet. <br/> Bir NSG'yi erişim açmak için kullanmak **kümesi AzureNetworkSecurityRule** cmdlet'i. | Etkilenen yönetmek için karmaşık IP aralıkları zamanla değiştirin. <br/>Azure, yalnızca depolama tam erişim sağlar.|
| Bir HTTP proxy sunucu kullan   | Depolama üzerinde ayrıntılı denetim proxy'de URL'leri izin verilir. <br/>Sanal makineleri tek noktası internet erişimi. <br/> Azure IP adresi değişiklikleri tabi değildir. | Bir VM ile Ara yazılım çalıştırmak için ek ücrete. |

## <a name="set-permissions-for-non-marketplace-sql-vms"></a>Market dışı SQL VM'ler için izinleri ayarlama

Bir sanal makineyi yedeklemek için Azure Backup gerektirir **AzureBackupWindowsWorkload** uzantısı yüklü olmalıdır. İçin Azure Market sanal makineler kullanıyorsanız,'ın İleri atlayabilirsiniz [Bul SQL server veritabanları](backup-azure-sql-database.md#discover-sql-server-databases). SQL veritabanlarını barındıran sanal makine Azure Marketi'nden oluşturulmamışsa uzantısını yükleyin ve uygun izinleri ayarlamak için aşağıdaki bölümü tamamlayın. Ek olarak **AzureBackupWindowsWorkload** uzantısı, Azure Backup SQL veritabanlarını korumak için SQL sysadmin ayrıcalıkları gerektirir. Sanal makinede veritabanları bulma sırasında Azure Backup bir hesabı, NT Service\AzureWLBackupPluginSvc oluşturur. Azure Backup'ın SQL veritabanları bulmak NT Service\AzureWLBackupPluginSvc hesap SQL oturum açma ve SQL sysadmin izinlerine sahip olmalıdır. Aşağıdaki yordam, bu izinleri sağlamayı açıklar.

İzinleri yapılandırmak için:

1. İçinde [Azure portal](https://portal.azure.com), SQL veritabanlarını korumak için kullanmakta olduğunuz kurtarma Hizmetleri kasası açın.
2. Kasa Panosu menüsünden **+ Backup** açmak için **yedekleme hedefi** menüsü.

   ![Tıklatın + yedekleme yedekleme hedefi menüsünü açın](./media/backup-azure-sql-database/open-backup-menu.png)

3. Üzerinde **yedekleme hedefi** menüsü, **, iş yükünü çalıştırdığı?** menüsünde bırakın **Azure** varsayılan olarak.

4. Üzerinde **neleri yedeklemek istiyorsunuz** menüsünden açılan menüsünü genişletin ve seçin **Azure VM'deki SQL Server**.

    ![Tıklatın + yedekleme yedekleme hedefi menüsünü açın](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    **Yedekleme hedefi** menüsünü görüntüler iki yeni adımlar: **Bul DBs VM'ler içinde** ve **Yedekleme'yi yapılandırma**. **Sanal makineleri DBs Bul** Azure sanal makineler için bir arama başlatır.

    ![Yeni yedekleme hedefi adımları gösterir](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

5. Tıklatın **bulmayı Başlat** Abonelikteki korumasız sanal makinelerin aramak için. Abonelikteki korumasız sanal makinelerin sayısına bağlı olarak, tüm sanal makineler gitmek için biraz zaman alabilir.

    ![Yedekleme beklemede](./media/backup-azure-sql-database/discovering-sql-databases.png)
 
    Korumasız bir sanal makine bulunduktan sonra listede görüntülenir. Korumasız sanal makinelerin, sanal makine adını ve kaynak grubu tarafından listelenir. Birden çok aynı ada sahip için sanal makineler için mümkündür. Ancak, aynı ada sahip sanal makineler farklı kaynak gruplarına aittir. Beklenen bir sanal makine listede görünmüyorsa, sanal makine zaten bir kasaya korunup korunmadığını.

6. Sanal makinelerin yedekleme ve tıklatın SQL veritabanını içeren VM listesinden **Bul DBs**. 

    Bulma işlemi yükler **AzureBackupWindowsWorkload** sanal makinede uzantı. Uzantı, SQL veritabanlarını yedeklemek için sanal makineyle iletişim kurmak Azure Backup hizmeti sağlar. Bir kez uzantısı yükler Azure yedekleme bir Windows sanal hizmet hesabı oluşturur **NT Service\AzureWLBackupPluginSvc**, sanal makinede. Sanal hizmet hesabının SQL sysadmin izinlerine gerek duyuyor. Hata görürseniz sanal hizmet hesabı yükleme işlemi sırasında **UserErrorSQLNoSysadminMembership**, başlıklı bölüme bakın [düzelttikten SQL sysadmin izinleri](backup-azure-sql-database.md#fixing-sql-sysadmin-permissions).

    Bildirimler alanı veritabanı bulma ilerlemesini gösterir. Kaç tane sanal makineyi veritabanlarıdır bağlı olarak, bu işin tamamlanması biraz sürebilir. Seçili veritabanları bulunan, bir başarı iletisi görüntülenir.

    ![başarılı dağıtım bildirim iletisi](./media/backup-azure-sql-database/notifications-db-discovered.png)

Veritabanı kurtarma Hizmetleri kasası ile ilişkilendirdikten sonra sıradaki adım olacak [yedeklemeyi yapılandırma](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database).

### <a name="fixing-sql-sysadmin-permissions"></a>SQL sysadmin izinleri çözme

Hata görürseniz yükleme işlemi sırasında **UserErrorSQLNoSysadminMembership**, SQL sysadmin iznine sahip bir hesapla oturum SQL Server Management Studio (SSMS) içine. Özel izinler gerektirmedikçe hesap tanımak için Windows kimlik doğrulaması kullanmanız mümkün olması gerekir.

1. SQL Server'da açmak **güvenlik/oturumları** klasör.

    ![Hesapları görmek için SQL Server ve güvenlik ve oturum açma klasörleri açın](./media/backup-azure-sql-database/security-login-list.png)

2. Oturum açmalar klasörüne sağ tıklayın ve seçin **yeni oturum açma**, oturum aç - yeni iletişim tıklatıp **arama**

    ![Oturum açma - yeni arama iletişim kutusu açın](./media/backup-azure-sql-database/new-login-search.png)

3. Windows sanal hizmet hesabı bu yana **NT Service\AzureWLBackupPluginSvc** zaten gösterildiği gibi sanal makine kaydı ve SQL bulma aşaması sırasında oluşturulan, hesap adını girin  **Seçilecek nesne adını girin** iletişim. Tıklatın **Adları Denetle** adı çözümlenemedi. 

    ![Bilinmeyen hizmet adı çözümlemek için Adları Denetle düğmesini tıklatın.](./media/backup-azure-sql-database/check-name.png)

4. Tıklatın **Tamam** kullanıcı veya Grup Seç iletişim kutusunu kapatmak için.

5. İçinde **sunucu rolleri** iletişim kutusunda, emin olun **sysadmin** rolü seçilir. Ardından **Tamam** kapatmak için **yeni oturum açma -**.

    ![Sysadmin sunucu rolünün seçili olduğundan emin olun](./media/backup-azure-sql-database/sysadmin-server-role.png)

    Gerekli izinleri artık mevcut olması gerekir.

6. İzin hatası sabit olsa hala veritabanı kurtarma Hizmetleri kasası ile ilişkilendirmeniz gerekir. Azure portalında **korunan sunucuları** listesinde, sunucunun hata sağ tıklatın ve seçin **yeniden Bul DBs**.

    ![Sunucunun uygun izinlere sahip doğrulayın](./media/backup-azure-sql-database/check-erroneous-server.png)

    Bildirimler alanı veritabanı bulma ilerlemesini gösterir. Kaç tane sanal makineyi veritabanlarıdır bağlı olarak, bu işin tamamlanması biraz sürebilir. Seçili veritabanları bulunduğunda bildirim alanında bir başarı iletisi görüntülenir.

    ![başarılı dağıtım bildirim iletisi](./media/backup-azure-sql-database/notifications-db-discovered.png)

Veritabanı kurtarma Hizmetleri kasası ile ilişkilendirdikten sonra sıradaki adım olacak [yedeklemeyi yapılandırma](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database).

[!INCLUDE [Section explaining how to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>SQL Server veritabanlarını Bul

Azure yedekleme, yedekleme gereksinimlerinize korumak için bir SQL Server örneğindeki tüm veritabanlarına bulabilir. SQL veritabanlarını barındıran sanal makine tanımlamak için aşağıdaki yordamı kullanın. Sanal makine tanımladığınızda, Azure yedekleme SQL server veritabanlarını keşfetmek için hafif bir uzantı yükler.

1. Oturum açtığınızda, aboneliğinizde [Azure portal](https://portal.azure.com/).
2. Sol menüde seçin **tüm hizmetleri**.

    ![Ana menüde tüm hizmetleri seçeneği seçin](./media/backup-azure-sql-database/click-all-services.png) <br/>

3. Tüm hizmetler iletişim kutusuna *kurtarma Hizmetleri*. Yazmaya başladığınızda, girişinizi kaynakların listesini filtreler. Gördüğünüzde, seçeneğini **kurtarma Hizmetleri kasaları**.

    ![Kurtarma Hizmetleri tüm hizmetleri iletişim kutusunda yazın](./media/backup-azure-sql-database/all-services.png) <br/>

    Abonelikteki kurtarma Hizmetleri kasalarının listesi görüntülenir. 

4. Kurtarma Hizmetleri kasası listesinden SQL veritabanlarını korumak için kullanmak istediğiniz kasayı seçin.

5. Kasa Panosu menüsünden **+ Backup** açmak için **yedekleme hedefi** menüsü.

   ![Tıklatın + yedekleme yedekleme hedefi menüsünü açın](./media/backup-azure-sql-database/open-backup-menu.png)

6. Üzerinde **yedekleme hedefi** menüsü, **, iş yükünü çalıştırdığı?** menüsünde bırakın **Azure** varsayılan olarak.

7. Üzerinde **neleri yedeklemek istiyorsunuz** menüsünden açılan menüsünü genişletin ve seçin **Azure VM'deki SQL Server**.

    ![Tıklatın + yedekleme yedekleme hedefi menüsünü açın](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    Bir kez seçili **yedekleme hedefi** menüsünü görüntüler iki adımı: Bul DBs VM'ler ve yedekleme yapılandırın. 

    ![Yeni yedekleme hedefi adımları gösterir](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

8. Tıklatın **bulmayı Başlat** Abonelikteki korumasız sanal makinelerin aramak için. Abonelikteki korumasız sanal makinelerin sayısına bağlı olarak, tüm sanal makineler gitmek için biraz zaman alabilir.

    ![Yedekleme beklemede](./media/backup-azure-sql-database/discovering-sql-databases.png)
 
    Korumasız bir sanal makine bulunduktan sonra listede görüntülenir. Birden çok sanal makine aynı ada sahip olabilir. Ancak, aynı ada sahip birden çok sanal makine farklı kaynak gruplarına aittir. Korumasız sanal makineleri, sanal makine adını ve kaynak grubu tarafından listelenir. Beklenen bir sanal makine listede yoksa, bu sanal makine zaten bir kasaya korunup korunmadığını.

9. Sanal makineler listesinden korumak ve tıklatın SQL veritabanlarının bulunduğu sanal makinenin onay kutusunu işaretleyin **Bul DBs**.

    Sanal makinedeki tüm SQL veritabanları Azure yedekleme bulur. Aşağıdaki bölümde veritabanı bulma aşamasında neler olduğu hakkında daha fazla bilgi için bkz [SQL veritabanları keşfederken arka uç işlemleri](backup-azure-sql-database.md#backend-operations-when-discovering-sql-databases). SQL veritabanları öğrendiğinizde etmeye hazır olduğunuz [yedekleme işi yapılandırma](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database).

### <a name="backend-operations-when-discovering-sql-databases"></a>SQL veritabanları keşfederken arka uç işlemleri

Kullandığınızda **Bul DBs** aracı, Azure Backup aşağıdaki işlemleri arka planda çalıştırır:

- sanal makine iş yükü yedekleme için kurtarma Hizmetleri kasası ile kaydeder. Kayıtlı sanal makinedeki tüm veritabanları, bu kurtarma Hizmetleri Kasası'na yalnızca yedeklenebilir. 

- yükler **AzureBackupWindowsWorkload** sanal makinede uzantı. Bir SQL veritabanı yedeklemesi aracısız bir çözümdür, diğer bir deyişle, sanal makinede yüklü uzantısıyla aracı SQL veritabanı üzerinde yüklü.

- hizmet hesabı oluşturur **NT Service\AzureWLBackupPluginSvc**, sanal makinede. Tüm yedekleme ve geri yükleme işlemleri hizmet hesabı kullanın. **NT Server\AzureWLBackupPluginSvc** SQL sysadmin izinleri olması gerekir. Tüm SQL Market sanal makineleri yüklü SqlIaaSExtension ile gelir ve otomatik olarak gerekli izinleri almak için SQLIaaSExtension AzureBackupWindowsWorkload kullanır. Sanal makinenize yüklü SqlIaaSExtension sahip değil, DB bulma işlemi başarısız olursa ve hata iletisini almak **UserErrorSQLNoSysAdminMembership**. Yedekleme için sysadmin iznine eklemek için yönergeleri izleyin [Market dışı SQL VM'ler için Azure yedekleme izinleri ayarlama](backup-azure-sql-database.md#set-permissions-for-non--marketplace-sql-vms).

    ![vm ve veritabanını seçin](./media/backup-azure-sql-database/registration-errors.png)

## <a name="configure-backup-for-sql-server-database"></a>SQL Server veritabanı için yedekleme yapılandırın

Azure yedekleme, SQL Server veritabanlarını koruma ve yedekleme işlerini yönetmek için Yönetim Hizmetleri sağlar. Yönetim ve izleme olanakları, Kurtarma Hizmetleri kasası bağlıdır. SQL veritabanı için korumayı yapılandırmak için:

1. Açık kurtarma Hizmetleri kasası ile SQL sanal makinesi kayıtlı.

2. Kasa Panosu menüsünden **+ Backup** açmak için **yedekleme hedefi** menüsü.

    ![Tıklatın + yedekleme yedekleme hedefi menüsünü açın](./media/backup-azure-sql-database/open-backup-menu.png)

3. Üzerinde **yedekleme hedefi** menüsü, **, iş yükünü çalıştırdığı?** menüsünde bırakın **Azure** varsayılan olarak.

4. Üzerinde **neleri yedeklemek istiyorsunuz** menüsünden açılan menüsünü genişletin ve seçin **Azure VM'deki SQL Server**.

    ![Tıklatın + yedekleme yedekleme hedefi menüsünü açın](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    Bir kez seçili **yedekleme hedefi** menüsünü görüntüler iki adımı: Bul DBs VM'ler ve yedekleme yapılandırın. Bu makalede sırayla aracılığıyla gitti, korumasız sanal makineleri zaten keşfettiniz ve sahip bir sanal makineyi bu kasaya kayıtlı. Artık SQL veritabanları için korumayı yapılandırmak hazırsınız.

5. Yedekleme hedefi menüye tıklayın **Yedekleme'yi yapılandırma**.

    ![Yeni yedekleme hedefi adımları gösterir](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

    Azure Backup hizmeti, bağımsız veritabanları gibi SQL AlwaysOn Kullanılabilirlik grupları ile tüm SQL örneğini gösterir. Tek başına veritabanı SQL örneğinde görüntülemek için veritabanlarını görmek için örnek adının yanındaki köşeli çift Ayraca tıklayın. Aşağıdaki görüntüleri tek başına bir örneğini ve Always On kullanılabilirlik grubu örnekleri göster.

    > [!NOTE]
    > Bu sınırlama SQL platform olduğu gibi tam ve değişim yedeklemeleri birincil düğümden gerçekleşir. Günlük yedekleme, yedekleme tercihinize gerçekleşebilir. Bu sınırlama nedeniyle, birincil düğüm kayıtlı olması gerekir.
    >

    ![Veritabanı SQL örneğinde listesi](./media/backup-azure-sql-database/discovered-databases.png)

    Veritabanlarının listesini görüntülemek için AlwaysOn Kullanılabilirlik grupları yanındaki köşeli çift Ayraca tıklayın.

    ![AlwaysOn Kullanılabilirlik grubu veritabanlarında listesi](./media/backup-azure-sql-database/discovered-database-availability-group.png)

6. Veritabanlarının korumak ve tıklatın istediğiniz tüm listesinden **Tamam**.

    ![onları korumak için birden çok veritabanını seçin](./media/backup-azure-sql-database/select-multiple-database-protection.png)

    Bir kerede en çok 50 veritabanları seçebilirsiniz. 50'den fazla veritabanlarını korumak istiyorsanız, birden çok geçiş yapın. İlk 50 veritabanlarını korumaya aldıktan sonra sonraki veritabanları kümesini korumak için bu adımı yineleyin.
    > [!Note] 
    > Yedekleme yüklerini en iyi duruma getirmek için Azure yedekleme büyük yedekleme işleri birden çok yığınlara keser. Bir yedekleme işi veritabanlarında sayısı 50'dir.
    >
    >

7. Oluşturmak veya bir yedekleme ilkesi seçin **yedekleme** menüsünde, select **yedekleme İlkesi**, menüsünü açın.

    ![Yedekleme İlkesi seçeneğini belirleyin](./media/backup-azure-sql-database/select-backup-policy.png)

8. Gelen **yedekleme ilkesi seçin** açılır menüsünde, bir yedekleme ilkesi seçin ve tıklatın **Tamam**. Yedekleme ilkenizi oluşturma hakkında daha fazla bilgi için, bkz [bir yedekleme ilkesi tanımlama](backup-azure-sql-database.md#define-a-backup-policy).

    ![aşağı açılan menüden bir yedekleme İlkesi](./media/backup-azure-sql-database/select-backup-policy-steptwo.png)

    Yedekleme İlkesi menüde gelen **yedekleme ilkesi seçin** açılır menü seçebilirsiniz: 
    - Varsayılan HourlyLogBackup İlkesi 
    - SQL için daha önce oluşturduğunuz bir mevcut bir yedekleme İlkesi
    - için [yeni bir ilke tanımlamak](backup-azure-sql-database.md#define-a-backup-policy) , kurtarma noktası hedefi (RPO) ve bekletme aralığına bağlı olarak. 

    > [!Note]
    > Azure yedekleme uzun vadeli bekletme arka uç depolama alanı tüketimi uyumluluk ihtiyaçlarını çalışırken en iyi duruma getirmek için üst öğe son yedekleme düzenini temel destekler.
    >

9. İçinde bir yedekleme İlkesi seçtikten sonra **yedekleme menü** tıklatın **yedeklemeyi etkinleştir**.

    ![Seçilen yedekleme ilkesini etkinleştir](./media/backup-azure-sql-database/enable-backup-button.png)

    Portal bildirimleri alanında yapılandırma ilerleme durumunu izleyebilirsiniz.

    ![Görünüm bildirim alanı](./media/backup-azure-sql-database/notifications-area.png)


### <a name="define-a-backup-policy"></a>Yedekleme ilkesi tanımlama

Bir yedekleme İlkesi yedeklemelerin ne zaman alınır ve yedeklemelerin ne kadar süreyle saklanacağını bir matrisini tanımlar. SQL veritabanları için yedekleme üç tür zamanlamak için Azure Yedekleme'yi kullanabilirsiniz:

* Tam yedekleme - tam bir veritabanı yedeği tüm veritabanını yedekler. Tam yedekleme belirli veritabanı veya dosya grupları veya dosyaları tüm veri ve bu verileri kurtarmak için yeterli günlük içerir. En fazla günde bir tam yedekleme tetikleyebilir. Bir günlük veya haftalık aralıkta bir tam yedekleme gerçekleştirin seçebilirsiniz. 
* Değişiklik yedeklemesi - değişiklik yedeklemesi en son, önceki tam veri yedeklemeyi temel alır. Yalnızca tam yedeklemeden sonra değişen verileri değişiklik yedekleme yakalar. En fazla günde bir değişiklik yedeklemesi tetikleyebilir. Tam yedekleme ve değişiklik yedeklemesi aynı gün içinde yapılandıramazsınız.
* İşlem günlüğü yedeklemesi - bir günlük yedeği ikinci kadar belirli bir zaman içinde nokta geri yükleme sağlar. En fazla 15 dakikada bir işlem günlüğü yedeklemeleri yapılandırabilirsiniz.

İlke kurtarma Hizmetleri kasası düzeyinde oluşturulur. Birden çok kasa varsa ve aynı yedekleme ilkesine kasalarını kullanabilirsiniz, ancak her kasaya yedekleme İlkesi uygulamanız gerekir. Günlük, bir yedekleme ilkesi oluştururken, tam yedekleme varsayılandır. Haftalık gerçekleşmesi için tam yedeklemeler geçiş yaparsanız, ancak yalnızca bir değişiklik yedeklemesi ekleyebilirsiniz. Aşağıdaki yordam, bir Azure sanal makinesi bir SQL server için bir yedekleme ilkesi oluşturmak açıklanmaktadır.

Bir yedekleme ilkesi oluşturmak için

1. Yedekleme İlkesi menüsünde gelen **yedekleme ilkesi seçin** açılır menüsünde, select **Yeni Oluştur**.

   ![Yeni yedekleme İlkesi Oluştur](./media/backup-azure-sql-database/create-new-backup-policy.png)

    Tüm yeni SQL server yedekleme ilkesi için gerekli alanları sağlamak için yedekleme İlkesi menü geçer.

   ![Yeni bir yedekleme İlkesi alanlar](./media/backup-azure-sql-database/blank-new-policy.png)

2. İçinde **ilke adı**, bir ad sağlayın. 

3. Tam yedekleme zorunludur. Tam yedekleme için varsayılan değerleri kabul edin veya tıklatın **tam yedekleme** ilkesini düzenlemek için.

    ![Yeni bir yedekleme İlkesi alanlar](./media/backup-azure-sql-database/full-backup-policy.png)

    Tam yedekleme İlkesi içinde günlük veya haftalık sıklığını seçin. Günlük seçerseniz, yedekleme işi başladığında saat ve saat dilimi, seçin. Günlük tam yedeklemeler seçerseniz, yedekleri oluşturulamıyor.

   ![günlük aralığı ayarı](./media/backup-azure-sql-database/daily-interval.png)

    Haftalık seçerseniz, yedekleme işi başladığında gün haftanın günü, saat ve saat dilimi seçin.

   ![Haftalık aralığını ayarlama](./media/backup-azure-sql-database/weekly-interval.png)

4. Varsayılan olarak, tüm bekletme aralığı Seçenekler (günlük, haftalık, aylık ve yıllık) seçilidir. Değil isterseniz ve kullanılacak aralıkları ayarlama herhangi tutma aralığı sınırının seçimini kaldırın. Tam yedekleme İlkesi menüsünü tıklatın **Tamam** ayarları kabul etmek için.

   ![Bekletme aralığı aralığı ayarı](./media/backup-azure-sql-database/retention-range-interval.png)

    Kurtarma noktası bekletme, kendi saklama aralığına göre etiketlenir. Örneğin, bir günlük seçerseniz, tam yedekleme tek bir tam yedekleme her gün tetiklenir. Haftalık bekletme bağlı olarak, belirli günün yedek etiketli korunur ve haftalık bekletme aralığı tabanlı. Aylık ve yıllık bekletme aralığı benzer şekilde davranır.

5. Fark bir yedekleme ilkesi eklemek için tıklatın **fark yedekleme** kendi menüsünü açın. 

   ![fark İlkesi'ni açın](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

    Değişiklik yedeklemesi İlkesi menüsünü **etkinleştirmek** sıklığı ve bekletme denetimleri açın. Size, en fazla günde bir değişiklik yedeklemesi tetikleyebilir.
    > [!Important] 
    > En fazla yedekleri 180 gün boyunca korunabilir. Daha uzun bekletme gerekiyorsa, tam yedeklemeler kullanmanız gerekir; yedekleri kullanamazsınız.
    >

   ![fark ilkesini Düzenle](./media/backup-azure-sql-database/enable-differential-backup-policy.png)

    Tıklatın **Tamam** ilkeyi kaydedin ve ana yedekleme İlkesi menüye dönmek için.

6. Bir işlem günlüğü yedeklemesi ilke eklemek için tıklatın **günlük yedeği** kendi menüsünü açın. Günlük yedekleme menüde seçin **etkinleştirmek**, sıklığı ve bekletme denetimleri ayarlayın. Günlük yedeklerini olarak 15 dakikada bir sıklıkta ortaya çıkabilir ve 35 gün boyunca korunabilir. Tıklatın **Tamam** ilkeyi kaydedin ve ana yedekleme İlkesi menüye dönmek için.

   ![günlük yedekleme ilkesini Düzenle](./media/backup-azure-sql-database/log-backup-policy-editor.png)

7. SQL yedekleme sıkıştırma etkinleştirilip etkinleştirilmeyeceğini seçin. Sıkıştırma, varsayılan olarak devre dışıdır.

    Arka uçta SQL yerel yedekleme sıkıştırma Azure Backup'i kullanır.

8. Yedekleme İlkesi tüm düzenlemeleri yapıldığında tıklatın **Tamam**. 

   ![fark bekletme aralığı](./media/backup-azure-sql-database/differential-backup-policy.png)

## <a name="restore-a-sql-database"></a>Bir SQL veritabanını geri yükle

Azure yedekleme, işlem günlüğü yedeklemeleri kullanarak tek tek veritabanları için belirli bir tarih veya saat, belirli bir kadar ikinci olarak, geri yüklemek için işlevsellik sağlar. Geri yükleme süreleri sağlarsanız, Azure Backup otomatik olarak uygun belirler tam, fark ve verilerinizi geri yüklemek için gerekli günlük yedeklerini zincirine bağlı.

Alternatif olarak, belirli bir zamanda yerine belirli bir kurtarma noktasını geri yüklemek için belirli bir tam veya fark yedek seçebilirsiniz.

Bir veritabanını geri yüklemek için

1. Açık kurtarma Hizmetleri kasası ile SQL sanal makinesi kayıtlı.

2. Kasa panosunda seçin **kullanım** yedekleme yedekleme öğeleri menüsünü açmak için öğeleri.

    ![Tıklatın + yedekleme yedekleme hedefi menüsünü açın](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. İçinde **yedekleme öğeleri** menüsünde yedekleme yönetimi türünü seçin **Azure VM'deki SQL**. 

    ![Tıklatın + yedekleme yedekleme hedefi menüsünü açın](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    Yedekleme öğeleri listesi, SQL veritabanlarının listesini gösterecek şekilde ayarlanır. 

4. SQL veritabanları listesinden geri yüklemek istediğiniz veritabanını seçin.

    ![SQL Azure VM'de listeden seçin.](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Veritabanını seçtiğinizde, kendi menüsü açılır. Bu menü veritabanı dahil etmek için yedekleme Ayrıntılar verilmiştir:

    * eski ve en son geri yükleme noktaları,
    * günlük yedekleme durumu son 24 saat (için veritabanları tam ve toplu günlüğe yazılan kurtarma modelinde, işlem günlüğü yedeklemeleri için yapılandırdıysanız)

5. Seçili veritabanı menüye tıklayın **geri DB** geri yükleme menüsünü açın.

    ![Veritabanı geri yükleme seçin](./media/backup-azure-sql-database/restore-db-button.png)

    **Geri** menüsü açılır ve bunu mu **geri yükleme Yapılandırması** menüsü. **Geri yükleme Yapılandırması** menü olduğundan geri yükleme yapılandırma ilk adımı. Bu menüde verileri geri yüklemek istediğiniz yeri seçin. Seçenekler şunlardır:
    * Alternatif konumu - hala özgün kaynak veritabanı korurken veritabanını alternatif bir konuma geri yüklemek istiyorsanız bu seçeneği kullanın.
    * DB - geri yüklemeler özgün kaynak aynı SQL Server örneğine verileri üzerine yazın. Bunun etkisi, özgün veritabanının üzerine yaz ' dir.

    > [!Important]
    > Seçili veritabanında bir Always On kullanılabilirlik grubuna aitse, SQL veritabanı üzerine yazılmasına izin vermiyor. Bu durumda, yalnızca **alternatif bir konum** seçeneği etkinleşir.
    >

    ![geri yükleme yapılandırma menüsünü açmak için Yapılandır'ı tıklatın](./media/backup-azure-sql-database/restore-restore-configuration-menu.png)

### <a name="restore-to-an-alternate-location"></a>Alternatif bir konuma geri yükleme

Bu yordam, verileri alternatif bir konuma geri aracılığıyla anlatılmaktadır. Veritabanı geri yüklenirken üzerine yazmak istiyorsanız bölüme atlamak [geri yüklemek ve veritabanının üzerine yaz](backup-azure-sql-database.md#restore-and-overwrite-the-database). Bu yordam, açık kurtarma Hizmetleri kasanız varsa ve geri yükleme yapılandırması menüsünde olan varsayar. Yoksa, bir bölümle Başlat [SQL veritabanını geri](backup-azure-sql-database.md#restore-a-sql-database).

**Server** aşağı açılır menüden kurtarma Hizmetleri kasasına kayıtlı SQL sunucuları yalnızca gösterir. İstediğiniz sunucu değilse **sunucu** listesinde, bölümüne bakın [Bul SQL server veritabanları](backup-azure-sql-database.md#discover-sql-server-databases) sunucu bulunamıyor. Keşif veritabanı işlemi sırasında herhangi bir yeni sunucu kurtarma Hizmetleri Kasası'na kaydedilir.

1. İçinde **geri yükleme Yapılandırması** menüsü:

    * seçin **alternatif bir konum**,
    * için **Server**, veritabanını geri yüklemek istediğiniz SQL server'ı seçin.
    * içinde **örneği** açılır menüsünde, bir SQL örneği seçin
    * içinde **geri DB adı** iletişim kutusunda, hedef veritabanı adını belirtin.
    * (varsa) seçin **aynı ada sahip veritabanı seçilen SQL örneğinde zaten varsa üzerine**.
    * tıklatın **Tamam** hedef yapılandırma tamamlamak ve geri yükleme noktası seçme taşıyın.

    ![geri yükleme yapılandırmasını menüde alternatif bir konum, örneği ve adı seçin](./media/backup-azure-sql-database/restore-configuration-menu.png)

2. İçinde **seçin geri yükleme noktası** menüsünde ya da bir günlükleri (zaman içinde nokta) seçebilir veya tüm & fark geri yükleme noktası. Belirli bir zaman içinde nokta günlük geri yüklemek istiyorsanız, bu adımıyla devam edin. Şimdi bir tam veya fark geri yükleme noktası geri yüklemek istiyorsanız, 3. adıma atlayın.

    ![geri yükleme noktası menüsünü seçin](./media/backup-azure-sql-database/recovery-point-menu.png)

    Geri yükleme noktası yalnızca tam olan veritabanları için günlük yedeklemeler için kullanılabilir & kurtarma modeli toplu günlüğe. Bir özel noktası zaman için geri yüklemek için:

    1. Seçin **günlükleri (zaman içinde nokta)** geri yükleme seçeneği olarak.

        ![geri yükleme noktası türünü seçin](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. Altında **geri tarih**, takvimi açmak için takvim simgesini tıklatın. Tarihleri kalın yazı tipiyle kurtarma noktaları içeren ve geçerli tarih vurgulanır. Kurtarma noktaları takvimde bir tarihi seçin. Kurtarma noktası tarihlerle seçemezsiniz.

        ![Takvim açın](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Bir tarih seçtikten sonra zaman çizelgesi grafiği kesintisiz bir aralıkta kullanılabilir kurtarma noktalarını görüntüler.

    3. Zaman Çizelgesi grafik veya zaman iletişim kullanarak tıklatın ve kurtarma noktası için belirli bir süre belirtin **Tamam** geri yükleme noktası adımı tamamlamak için.
    
       ![Takvim açın](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        **Seçin geri yükleme noktası** menü kapanır ve **Gelişmiş Yapılandırma** menüsü açılır.

       ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. Gelen **Gelişmiş Yapılandırma** menüsü:

        * Veritabanını çalışmaz geri yüklendikten sonra açık tutmak için **kurtarma olmadan geri yükle** menüsünde, select **etkin**.
        * Hedef sunucuda geri yükleme konumunu değiştirmek istiyorsanız, yeni bir yol sağlamak **hedef** sütun.
        * tıklatın **Tamam** ayarları onaylayın ve kapatmak için **Gelişmiş Yapılandırma**.

    5. Üzerinde **geri** menüsünde tıklatın **geri** geri yükleme işi başlatmak için. Bildirimler alanı, ilerleme durumunu izleyebilirsiniz. Veritabanı geri yükleme işi devam eden da izleyebilirsiniz.

       ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/restore-job-notification.png)

3. İçinde **seçin geri yükleme noktası** menüsünde bir kurtarma noktası seçin. Ya da bir günlükleri (zaman içinde nokta) seçebilir ya da tam ve fark. Zaman içinde nokta günlük geri yüklemek istiyorsanız, 2. adıma geri dönün. Bu adım, belirli tam ya da fark geri yükleme noktası geri yükler. Bu seçenek ile son 30 gün boyunca tüm tam ve fark kurtarma noktalarını görebilirsiniz. 30 günden eski kurtarma noktalarını görmek istiyorsanız, tıklayın **filtre** açmak için **filtre geri yükleme noktaları** menüsü. Bir fark kurtarma noktası seçerseniz, Azure Backup ilk uygun tam kurtarma noktası geri yükler ve sonra seçilen fark kurtarma noktası uygular.

    ![geri yükleme noktası menüsünü seçin](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. İçinde **seçin geri yükleme noktası** menüsünde seçin **tam & fark**.

       ![geri yükleme noktası menüsünü seçin](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        Kullanılabilir kurtarma noktaları listesi görüntülenir.

    2. Kurtarma noktaları listesinden bir kurtarma noktası seçin ve **Tamam** geri yükleme noktası yordamı tamamlamak için. 

        ![tam kurtarma noktası seçin](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        **Geri yükleme noktası** menü kapanır ve **Gelişmiş Yapılandırma** menüsü açılır.

        ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. Gelen **Gelişmiş Yapılandırma** menüsü:

        * Veritabanını çalışmaz geri yüklendikten sonra açık tutmak için **kurtarma olmadan geri yükle** menüsünde, select **etkin**. **Kurtarma olmadan geri yükle** varsayılan olarak devre dışıdır.
        * Hedef sunucuda geri yükleme konumunu değiştirmek istiyorsanız, yeni bir yol sağlamak **hedef** sütun.
        * Tıklatın **Tamam** ayarları onaylayın ve kapatmak için **Gelişmiş Yapılandırma**.

    4. Üzerinde **geri** menüsünde tıklatın **geri** geri yükleme işi başlatmak için. Bildirimler alanı, ilerleme durumunu izleyebilirsiniz. Veritabanı geri yükleme işi devam eden da izleyebilirsiniz.

       ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/restore-job-notification.png)

### <a name="restore-and-overwrite-the-database"></a>Geri yükleme ve veritabanının üzerine yaz

Bu yordam, verileri geri yüklemek ve bir veritabanının üzerine aracılığıyla anlatılmaktadır. Alternatif bir konuma geri yüklemek istiyorsanız, bölüme atlamak [alternatif bir konuma geri](backup-azure-sql-database.md#restore-to-an-alternate-location). Bu yordam, açık kurtarma Hizmetleri kasanız varsa ve altındadır varsayar **geri yükleme Yapılandırması** menü (aşağıdaki görüntüde bakın). Yoksa, bir bölümle Başlat [SQL veritabanını geri](backup-azure-sql-database.md#restore-a-sql-database).

![geri yükleme yapılandırma menüsünü açmak için Yapılandır'ı tıklatın](./media/backup-azure-sql-database/restore-overwrite-db.png)

**Server** aşağı açılır menüden kurtarma Hizmetleri kasasına kayıtlı SQL sunucuları yalnızca gösterir. İstediğiniz sunucu değilse **sunucu** listesinde, bölümüne bakın [Bul SQL server veritabanları](backup-azure-sql-database.md#discover-sql-server-databases) sunucu bulunamıyor. Keşif veritabanı işlemi sırasında herhangi bir yeni sunucu kurtarma Hizmetleri Kasası'na kaydedilir.

1. İçinde **geri yükleme Yapılandırması** menüsünde, select **üzerine DB** tıklatıp **Tamam** hedef yapılandırma tamamlamak için. 

   ![DB üzerine tıklayın](./media/backup-azure-sql-database/restore-configuration-overwrite-db.png)

    **Server**, **örneği**, ve **geri DB adı** iletişim kutuları gerekli değildir.

2. İçinde **seçin geri yükleme noktası** menüsünde ya da bir günlükleri (zaman içinde nokta) seçebilir veya tüm & fark geri yükleme noktası. Zaman içinde nokta günlük geri yüklemek istiyorsanız, bu adımıyla devam edin. Şimdi bir fark & tam geri yükleme noktası geri yüklemek istiyorsanız, 3. adıma atlayın.

    ![geri yükleme noktası menüsünü seçin](./media/backup-azure-sql-database/recovery-point-menu.png)

    Geri yükleme noktası yalnızca tam olan veritabanları için günlük yedeklemeler için kullanılabilir & kurtarma modeli toplu günlüğe. İkinci bir noktaya geri yükleme belirli bir seçmek için:

    1. Seçin **günlükleri (zaman içinde nokta)** geri yükleme seçeneği olarak.

        ![geri yükleme noktası türünü seçin](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. Altında **geri tarih**, takvimi açmak için takvim simgesini tıklatın. Tarihleri kalın yazı tipiyle kurtarma noktaları içeren ve geçerli tarih vurgulanır. Kurtarma noktaları takvimde bir tarihi seçin. Kurtarma noktası tarihlerle seçemezsiniz.

        ![Takvim açın](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Bir tarih seçtikten sonra kullanılabilir kurtarma noktalarını zaman çizelgesi grafiği görüntüler.

    3. Zaman Çizelgesi grafik veya zaman iletişim kullanarak tıklatın ve kurtarma noktası için belirli bir süre belirtin **Tamam** geri yükleme noktası adımı tamamlamak için.
    
       ![Takvim açın](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        **Seçin geri yükleme noktası** menü kapanır ve **Gelişmiş Yapılandırma** menüsü açılır.

       ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. Gelen **Gelişmiş Yapılandırma** menüsü:

        * Veritabanını çalışmaz geri yüklendikten sonra açık tutmak için **kurtarma olmadan geri yükle** menüsünde, select **etkin**.
        * Hedef sunucuda geri yükleme konumunu değiştirmek istiyorsanız, yeni bir yol sağlamak **hedef** sütun.
        * tıklatın **Tamam** ayarları onaylayın ve kapatmak için **Gelişmiş Yapılandırma**.

    5. Üzerinde **geri** menüsünde tıklatın **geri** geri yükleme işi başlatmak için. Bildirimler alanı, ilerleme durumunu izleyebilirsiniz. Veritabanı geri yükleme işi devam eden da izleyebilirsiniz.

       ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/restore-job-notification.png)

3. İçinde **seçin geri yükleme noktası** menüsünde bir kurtarma noktası seçin. Ya da bir günlükleri (zaman içinde nokta) seçebilir ya da tam ve fark. Zaman içinde nokta günlük geri yüklemek istiyorsanız, 2. adıma geri dönün. Bu adım, belirli tam ya da fark geri yükleme noktası geri yükler. Bu seçenek ile son 30 gün boyunca tüm tam ve fark kurtarma noktalarını görebilirsiniz. 30 günden eski kurtarma noktalarını görmek istiyorsanız, tıklayın **filtre** açmak için **filtre geri yükleme noktaları** menüsü. Bir fark kurtarma noktası seçerseniz, Azure Backup ilk uygun tam kurtarma noktası geri yükler ve sonra seçilen fark kurtarma noktası uygular.

    ![geri yükleme noktası menüsünü seçin](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. İçinde **seçin geri yükleme noktası** menüsünde seçin **tam & fark**.

       ![geri yükleme noktası menüsünü seçin](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        Kullanılabilir kurtarma noktaları listesi görüntülenir.

    2. Kurtarma noktaları listesinden bir kurtarma noktası seçin ve **Tamam** geri yükleme noktası yordamı tamamlamak için. 

        ![tam kurtarma noktası seçin](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        **Geri yükleme noktası** menü kapanır ve **Gelişmiş Yapılandırma** menüsü açılır.

        ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. Gelen **Gelişmiş Yapılandırma** menüsü:

        * Veritabanını çalışmaz geri yüklendikten sonra açık tutmak için **kurtarma olmadan geri yükle** menüsünde, select **etkin**. **Kurtarma olmadan geri yükle** varsayılan olarak devre dışıdır.
        * Hedef sunucuda geri yükleme konumunu değiştirmek istiyorsanız, yeni bir yol sağlamak **hedef** sütun.
        * Tıklatın **Tamam** ayarları onaylayın ve kapatmak için **Gelişmiş Yapılandırma**.

    4. Üzerinde **geri** menüsünde tıklatın **geri** geri yükleme işi başlatmak için. Bildirimler alanı, ilerleme durumunu izleyebilirsiniz. Veritabanı geri yükleme işi devam eden da izleyebilirsiniz.

       ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/restore-job-notification.png)


## <a name="manage-azure-backup-operations-for-sql-on-azure-vms"></a>Azure vm'lerinde SQL Azure yedekleme işlemleri yönetmek

Bu bölümde, Azure sanal makinelerde çeşitli Azure Backup yönelik yönetim işlemlerini kullanılabilir SQL hakkında bilgi sağlar. Aşağıdaki üst düzey işlemleri mevcuttur:

* İşleri İzleme
* Yedekleme uyarıları
* Bir SQL veritabanı korumasını durdurun
* Bir SQL veritabanı için korumayı Sürdür
* Bir geçici yedekleme işi tetikleyeceğinden
* Bir SQL server kaydı

### <a name="monitor-jobs"></a>İşleri İzleme

Azure yedekleme, tüm yedekleme işlemleri için SQL native API'lerini kullanır. Yerel API'lerini kullanarak, tüm iş bilgilerinden getirebilirsiniz [SQL yedekleme kümesini tablo](https://docs.microsoft.com/sql/relational-databases/system-tables/backupset-transact-sql?view=sql-server-2017) msdb veritabanında. Ayrıca, Azure Backup tüm el ile Tetiklenmiş gösterir veya geçici, yedekleme işleri Portalı'nda işler. Portal Ekle kullanılabilir işler: tüm yedekleme işlemleri, geri yükleme işlemleri, kayıt yapılandırmak ve veritabanı işlemleri bulmak ve yedekleme işlemleri durdurun. Tüm zamanlanmış işler de OMS günlük analizi ile izlenebilir. Günlük analizi kullanarak işleri dağınıklığı kaldırır ve izleme ya da belirli işler filtreleme için ayrıntılı esneklik sağlar.

![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/jobs-list.png)

### <a name="backup-alerts"></a>Yedekleme uyarıları

15 dakikada bir gerçekleşen günlüğü yedeklemeleri ile bazen yedekleme işlerini izleme can sıkıcı olabilir. Azure yedekleme, herhangi bir yedekleme hata tarafından tetiklenen e-posta uyarıları sağlayarak bu büyük olasılıkla sıkıcı durumda planlanmış. Uyarıları veritabanı düzeyinde hata kodu tarafından birleştirilir. Örneğin, bir veritabanında her hatası için bir uyarı almak yerine, birden fazla yedekleme hatasına varsa, ilk başarısızlık için e-posta alırsınız. Bu veritabanı için sonraki hataları izlemek için Azure portalına sonra imzalayabilirsiniz. 

Yedekleme uyarıları izlemek için:

1. Azure aboneliğinizde içine oturum [Azure portal](https://portal.azure.com).

2. Açık kurtarma Hizmetleri kasası ile SQL sanal makinesi kayıtlı.

3. Kurtarma Hizmetleri kasası menüsünü **uyarı ve olayları**. 

   ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

4. İçinde **uyarı ve olayları** menüsünde, select **yedekleme uyarıları** uyarıların listesini görüntülemek için.

   ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

### <a name="stop-protection-on-a-sql-server-database"></a>Bir SQL Server veritabanı korumasını durdurun

SQL Server veritabanını korumayı durdurursanız, Azure yedekleme kurtarma noktalarını korumak isteyip istemediğinizi sorar. SQL veritabanı korumayı durdurmak için iki yolu vardır:

* Sonraki tüm yedekleme işleri durdurun ve tüm kurtarma noktalarını silin,
* Sonraki tüm yedekleme işleri durdurmak, ancak kurtarma noktaları bırakın 

SQL için kurtarma noktalarını ücret yanı sıra, kullanılan depolama fiyatlandırma SQL korumalı örneği taşımak gibi kurtarma noktaları bırakarak bir maliyeti vardır. SQL Azure yedekleme fiyatlandırma hakkında daha fazla bilgi için bkz: [Azure yedekleme fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/backup/). Veritabanı için korumayı durdurmak için:

1. Açık kurtarma Hizmetleri kasası ile SQL sanal makinesi kayıtlı.

2. Kasa panosunda seçin **kullanım** yedekleme yedekleme öğeleri menüsünü açmak için öğeleri.

    ![Tıklatın + yedekleme yedekleme hedefi menüsünü açın](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. İçinde **yedekleme öğeleri** menüsünde yedekleme yönetimi türünü seçin **Azure VM'deki SQL**. 

    ![Tıklatın + yedekleme yedekleme hedefi menüsünü açın](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    Yedekleme öğeleri listesi, SQL veritabanlarının listesini gösterecek şekilde ayarlanır. 

4. SQL veritabanları listesinden korumayı durdurmak için istediğiniz veritabanını seçin.

    ![SQL Azure VM'de listeden seçin.](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Veritabanını seçtiğinizde, kendi menüsü açılır. 

5. Seçili veritabanı menüye tıklayın **Dur yedekleme** veritabanını korumayı durdurmak için.

    ![Veritabanı geri yükleme seçin](./media/backup-azure-sql-database/stop-db-button.png)

    **Durdurmak yedekleme** menüsü açılır.

6. İçinde **durdurmak yedekleme** menüsünde yedekleme verileri Koru seçin veya yedekleme verilerini silin. İsteğe bağlı olarak, koruma ve bir açıklama durdurma nedeni sağlayabilir.

    ![Veritabanı geri yükleme seçin](./media/backup-azure-sql-database/stop-backup-button.png)

7. Tıklatın **Dur yedekleme** veritabanını korumayı durdurmak için. 

### <a name="resume-protection-for-a-sql-database"></a>Bir SQL veritabanı için korumayı Sürdür

Varsa **yedekleme verileri koru** seçeneği seçildiğinde SQL veritabanı korumasını durdurma, korumayı sürdürmek mümkün olduğunda. Yedekleme verilerini korunur değil, koruma devam edemiyor. 

1. SQL veritabanı için korumayı sürdürmek için yedek öğesini açın ve **sürdürme yedekleme**.

    ![Veritabanı korumayı sürdürmek](./media/backup-azure-sql-database/resume-backup-button.png)

   Yedekleme İlkesi menüsü açılır.

2. Gelen **yedekleme İlkesi** menüsünde bir ilke seçin ve tıklatın **kaydetmek**.

### <a name="trigger-an-adhoc-backup"></a>Geçici bir yedeklemeyi tetikleyin

İstediğiniz zaman geçici bir yedeklemeyi tetikleyin. Geçici yedeklemeler dört tür vardır. Her tür ayrıntıları görmek için makaleyi [türleri, SQL yedeklemeleri](https://docs.microsoft.com/sql/relational-databases/backup-restore/backup-overview-sql-server?view=sql-server-2017#types-of-backups).

* Tam yedekleme
* Kopya yalnızca tam yedekleme
* Değişiklik yedeklemesi
* Günlük yedekleme

### <a name="unregister-a-sql-server"></a>Bir SQL Server kaydı

Bir SQL server korumasını kaldırdıktan sonra ancak kasa silmeden önce kaydını kaldırmak için

1. Açık kurtarma Hizmetleri kasası ile SQL sanal makinesi kayıtlı.

2. İçinde **Yönet** bölüm kasası, menüsünü **Yedekleme Altyapısı**.  

   ![Veritabanı korumayı sürdürmek](./media/backup-azure-sql-database/backup-infrastructure-button.png)

3. İçinde **yönetim sunucuları** 'yi tıklatın **korunan sunucuları**.

   ![Veritabanı korumayı sürdürmek](./media/backup-azure-sql-database/protected-servers.png)

    Korunan sunucuları menüsü açılır. 

4. İçinde **korunan sunucuları** menüsünde kaydını kaldırmak istediğiniz sunucuyu seçin. Kasayı silmek istiyorsanız, tüm sunucuların kaydını silin gerekir.

5. Korunan sunucuları menüde korumalı sunucuya sağ tıklayın ve seçin **silmek**. 

   ![Veritabanı korumayı sürdürmek](./media/backup-azure-sql-database/delete-protected-server.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure Backup hakkında daha fazla bilgi edinmek için, şifrelenmiş sanal makinelerin yedeklenmesine yönelik PowerShell örneğine bakın.

> [!div class="nextstepaction"]
> [Şifrelenmiş sanal makineyi yedekleme](./scripts/backup-powershell-sample-backup-encrypted-vm.md)
