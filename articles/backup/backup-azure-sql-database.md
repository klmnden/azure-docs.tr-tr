---
title: SQL Server veritabanını azure'a yedekleme | Microsoft Docs
description: Bu öğretici, Azure'da SQL Server Yedekleme açıklar. Makalede, SQL Server kurtarma de açıklanmaktadır.
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
ms.date: 7/6/2018
ms.author: markgal;anuragm
ms.custom: ''
ms.openlocfilehash: 32f45b66c4b1d22da3ffc4310a8a47c17319301f
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38302832"
---
# <a name="back-up-sql-server-database-in-azure"></a>Azure'da SQL Server veritabanını yedekleyin

SQL Server veritabanları, düşük kurtarma noktası hedefi (RPO) ve uzun süreli saklama gerektiren kritik iş yükleri değildir. Azure Backup, karmaşık bir yedekleme sunucusu değildir, hiçbir yönetim aracısı veya yedek depolama yönetmek için anlamına gelir sıfır altyapı gerektiren bir SQL Server yedekleme çözümü sağlar. Azure yedekleme, tüm SQL sunucuları veya hatta farklı iş yükleri arasında yedeklemeleriniz için merkezi yönetim sağlar.

 Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * SQL Server'ı Azure'a yedekleme önkoşulları
> * Oluşturma ve bir kurtarma Hizmetleri kasası kullanma
> * SQL Server Veritabanı Yedeklemeleri Yapılandırma
> * Kurtarma noktaları için bir yedekleme (veya bekletme) ilkesi ayarlayın
> * Veritabanını geri yükleme

Bu makalede yordamları başlatmadan önce Azure'da çalışan bir SQL Server olmalıdır. [SQL marketplace sanal makineleri kolayca bir SQL Server'ı oluşturmak için kullanabileceğiniz](../sql-database/sql-database-get-started-portal.md).

## <a name="public-preview-limitations"></a>Genel Önizleme sınırlamaları

Bilinen sınırlamalar genel Önizleme aşamasında olan aşağıdaki öğelerdir.

- SQL sanal makinesi, Azure genel IP adresleri erişmek için Internet bağlantısı gerektirir. Daha fazla ayrıntı için konudaki [ağ bağlantısı kurma](backup-azure-sql-database.md#establish-network-connectivity).
- En fazla 2000 SQL veritabanı bir kurtarma Hizmetleri kasasında koruyabilir. Ek SQL veritabanı ayrı bir kurtarma Hizmetleri Kasası'nda depolanması gerekir.
- [Dağıtılmış kullanılabilirlik grupları yedekleme sınırlamalara sahiptir](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/distributed-availability-groups?view=sql-server-2017).
- SQL yük devretme kümesi örneği (FCI) desteklenmez.
- SQL Server veritabanlarını korumak için Azure yedeklemeyi yapılandırmak için Azure portalını kullanın. Azure PowerShell, CLI ve REST API'leri için destek şu anda kullanılamıyor.

## <a name="supported-azure-geos"></a>Desteklenen Azure coğrafi

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

Aşağıdaki işletim sistemlerinde desteklenir. SQL marketplace Azure sanal makineleri ve Market dışı sanal makineler (SQL Server el ile yüklü olduğu) desteklenir.

### <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

Linux şu anda desteklenmiyor.

### <a name="supported-versionseditions-of-sql-server"></a>SQL Server'ın desteklenen sürümleri

- SQL 2012 Enterprise, Standard, Web, Developer, Express
- SQL 2014 Enterprise, Standard, Web, Developer, Express
- SQL 2016 Enterprise, Standard, Web, Developer, Express
- SQL 2017 Enterprise, Standard, Web, Developer, Express


## <a name="prerequisites-for-using-azure-backup-to-protect-sql-server"></a>SQL Server'ı korumak için Azure Backup'ı kullanma önkoşulları 

SQL Server veritabanınızı yedekleyebilmeniz için önce aşağıdaki koşulları gözden geçirin. :

- Tanımlamak veya [bir kurtarma Hizmetleri kasası oluşturma](backup-azure-sql-database.md#create-a-recovery-services-vault) aynı bölge veya yerel ayar, sanal makineyi barındıran SQL Server.
- [Sanal makinenin izinlerini denetleyin](backup-azure-sql-database.md#set-permissions-for-non-marketplace-sql-vms) SQL veritabanlarını yedeklemek için gerekli.
- [SQL sanal makinenin ağ bağlantısı var.](backup-azure-sql-database.md#establish-network-connectivity).

> [!NOTE]
> SQL Server veritabanlarını yedeklemek için bir seferde yalnızca bir yedekleme çözümü olabilir. Lütfen herhangi bir SQL yedekleme bu özelliği kullanmadan önce devre dışı bırakın, başka bir yedekleme müdahale ve başarısız. Azure Backup için Iaas VM birlikte SQL yedekleme herhangi bir çakışma olmadan etkinleştirebilirsiniz 
>

Ortamınızda bu koşulların, bölümüne devam [kasanızı bir SQL veritabanını korumak için yapılandırma](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database). Önkoşullar mevcut değilse bu bölümü okunurken devam edin.


## <a name="establish-network-connectivity"></a>Ağ bağlantısı kurma

Tüm işlemler için Azure genel IP adreslerine bağlantısı SQL sanal makinesi gerekir. Genel IP adreslerine bağlantısı olmadan SQL sanal makine işlemleri (örneğin, veritabanı keşfi, yedekleme, zamanlanmış yedeklemeler, geri yükleme kurtarma noktaları vb.) başarısız. Yedekleme trafiği için bir yol sağlamak için aşağıdaki seçeneklerden birini kullanın:

- Beyaz liste Azure veri merkezi IP aralıkları - beyaz liste ile Azure veri merkezi IP aralıkları, kullanın [İndirme Merkezi sayfasında IP aralıkları ve yönergeleri hakkında ayrıntılı bilgi için](https://www.microsoft.com/download/details.aspx?id=41653). 
- Bir HTTP proxy sunucusu için trafiği yönlendirme dağıtma - bir SQL veritabanı'nda bir sanal makine yedekliyorsanız, VM'deki yedekleme uzantısına Azure Backup ve Azure Depolama'ya veri yönetimi komutları göndermek için HTTPS API'lerini kullanır. Backup uzantısı, Azure Active Directory (AAD) kimlik doğrulaması için de kullanır. Genel internet erişimi için yapılandırılan tek bileşen olduğundan HTTP proxy üzerinden bu üç hizmeti yedekleme uzantısını trafiği yönlendirme.

Bir denge Seçenekler şunlardır: yönetilebilirlik, ayrıntılı bir denetim ve maliyet.

> [!NOTE]
>Hizmet etiketleri Azure Backup'ın, genel kullanılabilirlik tarafından kullanılabilir olması gerekir.
>

| Seçenek | Avantajları | Dezavantajları |
| ------ | ---------- | ------------- |
| Beyaz liste IP aralıkları | Ek maliyet olmadan. <br/> Erişim bir NSG içinde açmak için kullanmak **kümesi AzureNetworkSecurityRule** cmdlet'i. | Etkilenen yönetmek için karmaşık IP aralıklarını zamanla değişir. <br/>Azure, yalnızca depolama tam erişim sağlar.|
| Bir HTTP Ara sunucusunu kullanacak   | Depolama üzerinde ayrıntılı denetim proxy'sinde URL'leri izin verilir. <br/>Vm'leri tek noktası internet erişimi. <br/> Azure IP adresi değişiklikleri tabi değildir. | Ara yazılımla VM çalıştırmaya yönelik ek maliyet. |

## <a name="set-permissions-for-non-marketplace-sql-vms"></a>Market dışı SQL VM'ler için izinleri ayarlama

Bir sanal makineyi yedeklemek için Azure Backup gerektirir **AzureBackupWindowsWorkload** uzantısı yüklü olmalıdır. Azure Market sanal makineler kullanıyorsanız atlayın [Bul SQL server veritabanları](backup-azure-sql-database.md#discover-sql-server-databases). SQL veritabanlarınızı barındıran sanal makine Azure Marketi'nden oluşturulmamışsa, uzantıyı yüklemek ve uygun izinleri ayarlamak için aşağıdaki bölümü tamamlayın. Ek olarak **AzureBackupWindowsWorkload** uzantısı, Azure Backup, SQL veritabanlarını korumak için SQL sysadmin ayrıcalıkları gerektirir. Sanal makine veritabanlarında bulunurken bir hesap NT Service\AzureWLBackupPluginSvc Azure Backup oluşturur. Azure Backup'ın SQL veritabanlarını bulmak NT Service\AzureWLBackupPluginSvc hesabın olmalıdır, SQL ve SQL sysadmin izinleri. Aşağıdaki yordam, bu izinleri ver açıklanmaktadır.

İzinleri yapılandırmak için:

1. İçinde [Azure portalında](https://portal.azure.com), SQL veritabanlarını korumak için kullanmakta olduğunuz kurtarma Hizmetleri kasasını açın.
2. Kasa Panosu menüsünden tıklayın **+ yedekleme** açmak için **yedekleme hedefi** menüsü.

   ![Tıklama + Backup, yedekleme hedefi menüsünde açmak için](./media/backup-azure-sql-database/open-backup-menu.png)

3. Üzerinde **yedekleme hedefi** menü, **iş yükünüz çalıştığı?** bırak menüsünde **Azure** varsayılan olarak.

4. Üzerinde **neleri yedeklemek istiyorsunuz** menüsünden açılan menüsünü genişletin ve seçin **Azure VM'de SQL Server**.

    ![Tıklama + Backup, yedekleme hedefi menüsünde açmak için](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    **Yedekleme hedefi** menü görüntüler iki yeni adımlar: **VM'lerin içinde VT Bul** ve **yedeklemeyi Yapılandır**. **VM'lerin içinde VT Bul** Azure sanal makineler için bir arama başlatır.

    ![Yeni yedekleme hedefi adımları gösterir](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

5. Tıklayın **bulmayı Başlat** Abonelikteki korumasız sanal makinelerin aranacak. Abonelikteki korumasız sanal makinelerin sayısına bağlı olarak, tüm sanal makinelere gitmek için biraz zaman alabilir.

    ![Yedekleme beklemede](./media/backup-azure-sql-database/discovering-sql-databases.png)
 
    Korumasız bir sanal makine bulununca listesinde görünür. Korumasız sanal makinelerin, sanal makine adı ve kaynak grubu tarafından listelenir. Birden fazla aynı ada sahip amacıyla sanal makineler için mümkündür. Ancak, sanal makineler aynı ada sahip farklı kaynak grubuna ait. Beklenen bir sanal makine listede görünmüyorsa, sanal makine için bir kasa zaten korumalı olup olmadığını bakın.

6. Sanal makinelerin yedekleme ve istediğiniz SQL veritabanının içeren VM listesinden **Bul DBs**. 

    Bulma işlemi yükler **AzureBackupWindowsWorkload** sanal makinede uzantı. Uzantı, SQL veritabanlarını yedeklemek için sanal makineyle iletişim kurmak Azure Backup hizmeti sağlar. Bir kez uzantısı yükler Azure Backup Windows sanal hizmet hesabını oluşturur **NT Service\AzureWLBackupPluginSvc**, sanal makine üzerinde. Sanal hizmet hesabı SQL sysadmin izni gerektirir. Hata görürseniz sanal hizmet hesabı yükleme işlemi sırasında **UserErrorSQLNoSysadminMembership**, bölümüne bakın [düzeltme SQL sysadmin izinleri](backup-azure-sql-database.md#fixing-sql-sysadmin-permissions).

    Bildirimler alanı, veritabanı bulma ilerleme durumunu gösterir. Sanal makinede kaç veritabanı yere bağlı olarak, işin tamamlanması biraz sürebilir. Seçili veritabanlarındaki bulunan bir başarı iletisi görünür.

    ![başarılı dağıtım bildirim iletisi](./media/backup-azure-sql-database/notifications-db-discovered.png)

Veritabanı kurtarma Hizmetleri kasası ile ilişkilendirdikten sonra sonraki adım olarak [yedeklemeyi yapılandırma](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database).

### <a name="fixing-sql-sysadmin-permissions"></a>SQL sysadmin izinleri düzeltme

Yükleme sırasında bir hata görürseniz **UserErrorSQLNoSysadminMembership**, SQL Server Management Studio (SSMS) oturum açmak için SQL sysadmin izinleri olan bir hesap kullanın. Özel izinler gerekmedikçe, Windows kimlik doğrulama çalışması gerekir.

1. SQL Server'da açın **güvenlik/oturum açma bilgileri** klasör.

    ![Hesapları görmek için SQL Server ve güvenlik ve oturum açma klasörlerini açın](./media/backup-azure-sql-database/security-login-list.png)

2. Oturum açma bilgileri klasörü sağ tıklatın ve seçin **yeni oturum açma**, oturum açma - yeni iletişim kutusu, tıklatıp **arama**

    ![Oturum açma - yeni bir arama iletişim kutusunu açın.](./media/backup-azure-sql-database/new-login-search.png)

3. Windows sanal hizmet hesabı bu yana **NT Service\AzureWLBackupPluginSvc** zaten gösterildiği gibi SQL bulma aşamasından ve sanal makine kaydı sırasında oluşturulan, hesap adı girin  **Seçilecek nesne adını girin** iletişim. Tıklayın **Adları Denetle** adı çözümlenemedi. 

    ![Bilinmeyen hizmet adını çözümlemek için adları denetle düğmesine tıklayın](./media/backup-azure-sql-database/check-name.png)

4. Tıklayın **Tamam** kullanıcı veya Grup Seç iletişim kutusunu kapatmak için.

5. İçinde **sunucu rolleri** iletişim kutusunda emin **sysadmin** rolü seçilir. Ardından **Tamam** kapatmak için **oturum açma - yeni**.

    ![Sysadmin sunucu rolünün seçili olduğundan emin olun](./media/backup-azure-sql-database/sysadmin-server-role.png)

    Gerekli izinleri olması gerekir.

6. İzinleri hatasını düzelttik rağmen yine de veritabanı kurtarma Hizmetleri kasası ile ilişkilendirmeniz gerekir. Azure portalında **korumalı sunucuların** listesinde hata sunucuya sağ tıklayın ve seçin **yeniden Bul DBs**.

    ![Sunucunun uygun izinlere sahip olun](./media/backup-azure-sql-database/check-erroneous-server.png)

    Bildirimler alanı, veritabanı bulma ilerleme durumunu gösterir. Sanal makinede kaç veritabanı yere bağlı olarak, işin tamamlanması biraz sürebilir. Seçili veritabanları bulundu, başarılı iletisi bildirim alanında görüntülenir.

    ![başarılı dağıtım bildirim iletisi](./media/backup-azure-sql-database/notifications-db-discovered.png)

Veritabanı kurtarma Hizmetleri kasası ile ilişkilendirdikten sonra sonraki adım olarak [yedeklemeyi yapılandırma](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database).

[!INCLUDE [Section explaining how to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>SQL Server veritabanlarını Bul

Azure Backup, yedekleme gereksinimlerinize göre korumak için bir SQL Server örneğindeki tüm veritabanlarına bulabilir. SQL veritabanlarını barındıran sanal makineyi tanımlamak için aşağıdaki yordamı kullanın. Sanal makine tanımladıktan sonra Azure Backup, SQL server veritabanlarını bulmak için basit bir uzantı yükler.

1. Oturum açmak için aboneliğinizde [Azure portalında](https://portal.azure.com/).
2. Sol taraftaki menüde **tüm hizmetleri**.

    ![Ana menüde tüm hizmetleri seçeneğini belirleyin](./media/backup-azure-sql-database/click-all-services.png) <br/>

3. Tüm hizmetleri iletişim kutusuna *kurtarma Hizmetleri*. Yazmaya başladığınızda, giriş kaynakların listesini filtrelenir. Gördüğünüzde, seçmek **kurtarma Hizmetleri kasaları**.

    ![Kurtarma Hizmetleri tüm hizmetleri iletişim kutusunda yazın](./media/backup-azure-sql-database/all-services.png) <br/>

    Abonelikte kurtarma Hizmetleri kasalarının listesi görünür. 

4. Kurtarma Hizmetleri kasası listesinden SQL veritabanlarını korumak için kullanmak istediğiniz kasayı seçin.

5. Kasa Panosu menüsünden tıklayın **+ yedekleme** açmak için **yedekleme hedefi** menüsü.

   ![Tıklama + Backup, yedekleme hedefi menüsünde açmak için](./media/backup-azure-sql-database/open-backup-menu.png)

6. Üzerinde **yedekleme hedefi** menü, **iş yükünüz çalıştığı?** bırak menüsünde **Azure** varsayılan olarak.

7. Üzerinde **neleri yedeklemek istiyorsunuz** menüsünden açılan menüsünü genişletin ve seçin **Azure VM'de SQL Server**.

    ![Tıklama + Backup, yedekleme hedefi menüsünde açmak için](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    Seçildiğinde, **yedekleme hedefi** menü görüntüler iki adımı: yedeklemeyi Yapılandır ve VM'lerin içinde VT keşfedin. 

    ![Yeni yedekleme hedefi adımları gösterir](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

8. Tıklayın **bulmayı Başlat** Abonelikteki korumasız sanal makinelerin aranacak. Abonelikteki korumasız sanal makinelerin sayısına bağlı olarak, tüm sanal makinelere gitmek için biraz zaman alabilir.

    ![Yedekleme beklemede](./media/backup-azure-sql-database/discovering-sql-databases.png)
 
    Korumasız bir sanal makine bulununca listesinde görünür. Birden çok sanal makineye aynı ada sahip olabilir. Ancak, aynı ada sahip birden çok sanal makine farklı kaynak grubuna ait. Korumasız sanal makineleri, sanal makine adı ve kaynak grubu tarafından listelenir. Beklenen bir sanal makine listede yoksa, sanal makinenin bir kasaya zaten korumalı olup olmadığını bakın.

9. Sanal makineler listesinden korumak ve SQL veritabanlarının bulunduğu sanal makinenin onay kutusunu işaretleyin **Bul DBs**.

    Azure yedekleme, sanal makinede tüm SQL veritabanları bulur. Aşağıdaki bölümde veritabanı keşif aşamasında neler olduğu hakkında daha fazla bilgi için bkz [SQL veritabanları keşfederken arka uç işlemlerine](backup-azure-sql-database.md#backend-operations-when-discovering-sql-databases). SQL veritabanları bulunuyor sonra hazırsınız [yedekleme işini yapılandırmak](backup-azure-sql-database.md#configure-your-vault-to-protect-a-sql-database).

### <a name="backend-operations-when-discovering-sql-databases"></a>SQL veritabanları keşfederken arka uç işlemleri

Kullanırken **Bul DBs** aracı, Azure Backup şu işlemleri arka planda çalıştırır:

- sanal makine iş yükü yedekleme için kurtarma Hizmetleri kasası ile kaydeder. Kayıtlı sanal makinedeki tüm veritabanları, bu kurtarma Hizmetleri kasasına yalnızca yedeklenebilir. 

- yükler **AzureBackupWindowsWorkload** sanal makinede uzantı. Bir SQL veritabanını yedekleme aracısız bir çözümdür, diğer bir deyişle, sanal makinede yüklü uzantısıyla aracı SQL veritabanı'nda yüklü.

- hizmet hesabı oluşturur **NT Service\AzureWLBackupPluginSvc**, sanal makine üzerinde. Tüm yedekleme ve geri yükleme işlemleri, hizmet hesabı kullanın. **NT Service\AzureWLBackupPluginSvc** SQL sysadmin izinleri olması gerekir. Yüklü SqlIaaSExtension ile tüm SQL Marketplace sanal makinelere gelen ve otomatik olarak gerekli izinleri almak için SQLIaaSExtension AzureBackupWindowsWorkload kullanır. Sanal makinenizde yüklü SqlIaaSExtension yok, DB bulma işlemi başarısız olur ve hata iletisini almak **UserErrorSQLNoSysAdminMembership**. Yedekleme sysadmin izinlerini eklemek için yönergeleri izleyin. [Market dışı SQL VM'ler için Azure Backup izinlerinin ayarlanması](backup-azure-sql-database.md#set-permissions-for-non--marketplace-sql-vms).

    ![vm ve veritabanı seçin](./media/backup-azure-sql-database/registration-errors.png)

## <a name="configure-backup-for-sql-server-database"></a>SQL Server veritabanı için yedeklemeyi yapılandırma

Azure Backup, SQL Server veritabanlarınızı koruyun ve yedekleme işlerini yönetmek için Yönetim Hizmetleri sağlar. Kurtarma Hizmetleri kasanız, yönetim ve izleme özellikleri bağlıdır. 

> [!NOTE]
> SQL Server veritabanlarını yedeklemek için bir seferde yalnızca bir yedekleme çözümü olabilir. Lütfen herhangi bir SQL yedekleme bu özelliği kullanmadan önce devre dışı bırakın, başka bir yedekleme müdahale ve başarısız. Azure Backup için Iaas VM birlikte SQL yedekleme herhangi bir çakışma olmadan etkinleştirebilirsiniz 
>

SQL veritabanınız için koruma yapılandırılamadı:

1. Açık kurtarma Hizmetleri kasası ile SQL sanal makinesi kayıtlı.

2. Kasa Panosu menüsünden tıklayın **+ yedekleme** açmak için **yedekleme hedefi** menüsü.

    ![Tıklama + Backup, yedekleme hedefi menüsünde açmak için](./media/backup-azure-sql-database/open-backup-menu.png)

3. Üzerinde **yedekleme hedefi** menü, **iş yükünüz çalıştığı?** bırak menüsünde **Azure** varsayılan olarak.

4. Üzerinde **neleri yedeklemek istiyorsunuz** menüsünden açılan menüsünü genişletin ve seçin **Azure VM'de SQL Server**.

    ![Tıklama + Backup, yedekleme hedefi menüsünde açmak için](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    Seçildiğinde, **yedekleme hedefi** menü görüntüler iki adımı: yedeklemeyi Yapılandır ve VM'lerin içinde VT keşfedin. Bu makalede sırayla aracılığıyla incelediğinize korumasız sanal makineleri zaten keşfettiniz ve sahip bir sanal makineyi bu kasaya kayıtlı. SQL veritabanları için koruma yapılandırılamadı artık hazırsınız.

5. Yedekleme hedefi menüsünde tıklatın **yedeklemeyi Yapılandır**.

    ![Yeni yedekleme hedefi adımları gösterir](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

    Azure Backup hizmeti, tek başına veritabanlarının yanı sıra, SQL AlwaysOn Kullanılabilirlik grupları ile tüm SQL örneğini gösterir. Tek başına veritabanları SQL örneği'nde görüntülemek için veritabanlarını görüntülemek için örnek adının yanındaki köşeli çift Ayraca tıklayın. Aşağıdaki örnekler tek başına bir örneğini ve Always On kullanılabilirlik grubu gösterilmektedir.

    > [!NOTE]
    > SQL Always On kullanılabilirlik grubu olması durumunda, biz SQL yedekleme tercihi oluşuyor. Ancak bir SQL platform sınırlaması nedeniyle tam ve farklı yedeklemelerini birincil düğümden olması gerekmez. Günlük yedekleme, yedekleme tercihinize oluşabilir. Bu sınırlama nedeniyle, birincil düğüm her zaman kullanılabilirlik grupları için kaydedilmelidir.
    >

    ![SQL örneğinde veritabanı listesi](./media/backup-azure-sql-database/discovered-databases.png)

    Veritabanlarının listesini görüntülemek için AlwaysOn Kullanılabilirlik grupları yanındaki köşeli çift Ayraca tıklayın.

    ![AlwaysOn Kullanılabilirlik grubundaki veritabanları listesi](./media/backup-azure-sql-database/discovered-database-availability-group.png)

6. Veritabanları listesinden korumak ve istediğiniz Tümünü Seç **Tamam**.

    ![onları korumak için birden çok veritabanını seçin](./media/backup-azure-sql-database/select-multiple-database-protection.png)

    Tek seferde en çok 50 veritabanı seçebilirsiniz. 50'den fazla veritabanlarını korumak istiyorsanız, birden çok geçiş yapın. İlk 50 veritabanlarını koruduktan sonra sonraki kümesini veritabanlarını korumak için bu adımı yineleyin.
    > [!Note] 
    > Yedekleme yüklerini en iyi duruma getirmek için Azure Backup büyük yedekleme işleri birden çok toplu iş ayırır. Bir yedekleme işi veritabanlarında sayısı 50'dir.
    >
    >

7. Bir yedekleme ilkesi seçin veya **yedekleme** menüsünde **yedekleme İlkesi**menüsünü açmak için.

    ![Yedekleme İlkesi seçeneğini belirleyin](./media/backup-azure-sql-database/select-backup-policy.png)

8. Gelen **yedekleme ilkesi seçmek** açılan menüsünde, bir yedekleme İlkesi'ni seçin ve tıklayın **Tamam**. Yedekleme ilkenizi oluşturma hakkında daha fazla bilgi için bölümüne bakın. [yedekleme ilkesi tanımlama](backup-azure-sql-database.md#define-a-backup-policy).

    ![aşağı açılan menüden bir yedekleme İlkesi](./media/backup-azure-sql-database/select-backup-policy-steptwo.png)

    Yedekleme İlkesi menüsünde gelen **yedekleme ilkesi seçmek** açılan menüsünü seçebilirsiniz: 
    - Varsayılan HourlyLogBackup İlkesi 
    - SQL için daha önce oluşturulmuş mevcut bir yedekleme İlkesi
    - için [yeni bir ilke tanımlama](backup-azure-sql-database.md#define-a-backup-policy) , kurtarma noktası hedefi (RPO) ve bekletme aralığına bağlı. 

    > [!Note]
    > Azure Backup, uzun süreli saklama uyumluluk ihtiyaçlarını karşıladığından yandan arka uç depolama tüketimini en iyi duruma getirmek için dedenizin bırak son yedekleme düzenini temel destekler.
    >

9. İçinde bir yedekleme İlkesi seçtikten sonra **yedekleme menüsündeki** tıklayın **yedeklemeyi etkinleştir**.

    ![Seçilen yedekleme İlkesi'ni etkinleştir](./media/backup-azure-sql-database/enable-backup-button.png)

    Portal bildirimleri alanında yapılandırmanın ilerlemesini izleyebilirsiniz.

    ![Görünüm bildirim alanı](./media/backup-azure-sql-database/notifications-area.png)


### <a name="define-a-backup-policy"></a>Yedekleme ilkesi tanımlama

Bir yedekleme İlkesi yedeklemelerin ne zaman alınacağının ve yedeklemelerin ne kadar saklanır bir matrisini tanımlar. Üç tür SQL veritabanları için yedekleme zamanlamak için Azure Backup kullanabilirsiniz:

* Tam yedekleme - tam bir veritabanı yedeği tüm veritabanını yedekler. Tam yedekleme, tüm verileri belirli veritabanı veya dosya grubu veya dosyaları ve verileri kurtarmak için yeterli günlük içerir. En fazla günde bir tam yedekleme tetikleyin. Tam günlük veya haftalık bir aralıkta yedek almak seçebilirsiniz. 
* Değişiklik yedeği - değişiklik yedeği en son, önceki tam veri yedeği temel alır. Değişiklik yedeği tam yedeklemeden itibaren değişmiş verileri yakalar. En fazla günde bir fark yedekleme tetikleyebilirsiniz. Aynı gün tam yedekleme ve bir değişiklik yedeği yapılandıramazsınız.
* İşlem günlüğü yedeklemesi - bir günlük yedeği ikinci kadar belirli bir zaman içinde nokta geri yükleme sağlar. En fazla 15 dakikada bir işlem günlüğü yedeklemeleri yapılandırabilirsiniz.

İlke kurtarma Hizmetleri kasası düzeyinde oluşturulur. Birden çok kasaları varsa kasalar ve aynı yedekleme ilkesine kullanabilirsiniz ancak her kasa için yedekleme ilkesini uygulama. Tam yedekleme, günlük bir yedekleme ilkesi oluştururken varsayılan bağlıdır. Tam yedekleme haftalık olarak gerçekleşmesini geçerseniz, ancak yalnızca bir değişiklik yedeği ekleyebilirsiniz. Aşağıdaki yordam bir Azure sanal makineler'de SQL server için bir yedekleme ilkesi oluşturma işlemini açıklar.

Bir yedekleme ilkesi oluşturmak için

1. Yedekleme İlkesi menüsünde, gelen **yedekleme ilkesi seçmek** açılan menüsünde, select **Yeni Oluştur**.

   ![Yeni bir yedekleme ilkesi oluşturma](./media/backup-azure-sql-database/create-new-backup-policy.png)

    Tüm yeni SQL server yedekleme ilkesi için gerekli alanları sağlamak için yedekleme İlkesi menüsü geçer.

   ![Yeni bir yedekleme İlkesi alanlar](./media/backup-azure-sql-database/blank-new-policy.png)

2. İçinde **ilke adı**, bir ad sağlayın. 

3. Tam yedekleme zorunludur. Tam yedekleme için varsayılan değerleri kabul edin veya tıklayın **tam yedekleme** ilkeyi düzenlemek için.

    ![Yeni bir yedekleme İlkesi alanlar](./media/backup-azure-sql-database/full-backup-policy.png)

    İçinde tam bir yedekleme İlkesi, günlük veya haftalık sıklığını seçin. Günlük seçerseniz, yedekleme işi başladığında saat ve saat diliminizi seçin. Günlük tam yedekleme seçerseniz, değişiklik yedekleri oluşturulamıyor.

   ![Günlük aralığını ayarlama](./media/backup-azure-sql-database/daily-interval.png)

    Haftalık seçerseniz, yedekleme işi başladığında gün haftanın günü, saat ve saat dilimi seçin.

   ![Haftalık aralığını ayarlama](./media/backup-azure-sql-database/weekly-interval.png)

4. Varsayılan olarak, tüm bekletme aralığı seçenekleri (günlük, haftalık, aylık ve yıllık) seçilir. Değil isterseniz ve kullanmak için belirli aralıklarda herhangi elde tutma aralığı sınırının seçimini kaldırın. Tam yedekleme İlkesi menüde **Tamam** ayarları kabul etmek için.

   ![Bekletme aralığı aralığı ayarı](./media/backup-azure-sql-database/retention-range-interval.png)

    Kurtarma noktalarının bekletme, kendi saklama aralığına göre etiketlenir. Örneğin, bir günlük seçerseniz, tam yedekleme tek bir tam yedekleme her gün tetiklenir. Haftalık bekletme bağlı olarak, belirli günün yedeğini etiketli korunur ve haftalık bekletme aralığı tabanlı. Aylık ve yıllık bekletme aralığı benzer şekilde davranır.

5. Fark yedekleme ilkesi eklemek için tıklatın **fark yedekleme** alt menüsünü açın. 

   ![fark İlkesi'ni açın](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

    Fark yedekleme İlkesi menüsünde seçin **etkinleştirme** sıklığı ve bekletme denetimleri açın. En fazla günde bir fark yedekleme tetikleyebilirsiniz.
    > [!Important] 
    > En fazla değişiklik yedekleri, 180 gün boyunca korunabilir. Daha uzun bekletme süresi gerekiyorsa, tam yedeklemeler kullanmanız gerekir; yedekleri kullanamazsınız.
    >

   ![fark ilkesini Düzenle](./media/backup-azure-sql-database/enable-differential-backup-policy.png)

    Tıklayın **Tamam** ilkeyi kaydedin ve yedekleme İlkesi ana menüye dönmek için.

6. Bir işlem günlüğü yedekleme ilkesi eklemek için tıklatın **günlük yedeği** alt menüsünü açın. Günlük yedekleme menüde **etkinleştirme**, sıklığı ve bekletme denetimleri ayarlayın. Günlük yedeklemeleri 15 dakikada bir sıklıkta meydana gelebilir ve 35 güne kadar saklanabilir. Tıklayın **Tamam** ilkeyi kaydedin ve yedekleme İlkesi ana menüye dönmek için.

   ![günlük yedekleme ilkesini Düzenle](./media/backup-azure-sql-database/log-backup-policy-editor.png)

7. SQL yedekleme sıkıştırması etkinleştirmek isteyip istemediğinizi seçin. Sıkıştırma, varsayılan olarak devre dışıdır.

    Arka uçta Azure Backup SQL yerel yedekleme sıkıştırmasını kullanır.

8. Yedekleme ilkesi için tüm düzenlemeler yapıldığında tıklayın **Tamam**. 

   ![Yeni ilkeyi kabul et](./media/backup-azure-sql-database/backup-policy-click-ok.png)

## <a name="restore-a-sql-database"></a>Bir SQL veritabanını geri yükleme

Azure Backup, işlem günlüğü yedeklemeleri kullanarak tek veritabanlarına belirli bir tarih veya saat, belirli bir kadar ikinci olarak, geri yüklemek için işlevsellik sağlar. Geri yükleme süreleri sağlarsanız, Azure Backup otomatik olarak uygun belirler, tam, değişiklik yedeği ve verilerinizi geri yüklemek için gerekli günlük yedekleme zincirini temel.

Alternatif olarak, belirli bir zaman yerine belirli bir kurtarma noktasını geri yüklemek için belirli bir tam veya değişiklik yedeği seçebilirsiniz.
 > [!Note]
 > "Ana" veritabanı geri yükleme tetiklemeden önce lütfen SQL Server tek kullanıcı modunda başlatma seçeneği ile Başlat "-m AzureWorkloadBackup". İstemci adı olmayan bağımsız değişken - m, yalnızca bu istemci izin verilecek bağlantıyı açmak için. (Master, model, msdb) tüm sistem veritabanları için geri yüklemeyi tetikleme önce lütfen SQL Aracısı hizmetini durdurun. Bu veritabanları, herhangi bir bağlantı çalmak için çalışabilir tüm uygulamaları kapatın.
>

Bir veritabanını geri yüklemek için

1. Açık kurtarma Hizmetleri kasası ile SQL sanal makinesi kayıtlı.

2. Kasa panosunda seçin **kullanım** yedekleme yedekleme öğeleri menüsünü açmak için öğeleri.

    ![Tıklama + Backup, yedekleme hedefi menüsünde açmak için](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. İçinde **yedekleme öğeleri** menüsünde Yedekleme Yönetimi türü seçin **Azure VM'deki SQL**. 

    ![Tıklama + Backup, yedekleme hedefi menüsünde açmak için](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    Yedekleme öğeleri listesi, SQL veritabanlarının listesini göstermek için ayarlar. 

4. SQL veritabanları listesinden geri yüklemek istediğiniz veritabanını seçin.

    ![SQL Azure VM'de listeden seçin.](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Veritabanına seçtiğinizde, alt menüsü açılır. Bu menü, veritabanı dahil etmek için yedekleme ayrıntıları sağlar:

    * eski ve en son geri yükleme noktaları,
    * Son 24 saat (veritabanları tam ve toplu günlüğe kaydedilen kurtarma modelinde, işlem günlüğü yedeklemeleri için yapılandırdıysanız için) günlük yedekleme durumu

5. Seçili veritabanı menüden **geri DB** geri yükleme menüsünü açmak için.

    ![geri yükleme Veritabanı Seç](./media/backup-azure-sql-database/restore-db-button.png)

    **Geri** menüsü açılır ve bunu mu **geri yükleme Yapılandırması** menüsü. **Geri yükleme Yapılandırması** menüsü geri yapılandırma ilk adımı olur. Bu menüde, verileri geri yüklemek istediğiniz yeri seçin. Seçenekler şunlardır:
    * Alternatif konum - hala özgün kaynak veritabanı korurken veritabanını alternatif bir konuma geri yüklemek istiyorsanız bu seçeneği kullanın.
    * DB - geri yükler verileri özgün kaynak aynı SQL Server örneğine üzerine yazın. Bu parametrenin etkisi, özgün veritabanının üzerine yaz ' dir.

    > [!Important]
    > Seçilen veritabanı bir Always On kullanılabilirlik grubuna dahilse, SQL veritabanı üzerine yazılmasına izin vermez. Bu durumda, yalnızca **alternatif bir konuma** seçeneği etkinleştirilir.
    >

    ![Geri Yükleme Yapılandırması menüsünü açmak için Yapılandır'a tıklayın](./media/backup-azure-sql-database/restore-restore-configuration-menu.png)

### <a name="restore-to-an-alternate-location"></a>Alternatif bir konuma geri yükleme

Bu yordamı verileri alternatif bir konuma geri aracılığıyla size yol gösterir. Veritabanı geri yüklenirken üzerine yazmak istiyorsanız, bölümüne atla [geri yükleme ve veritabanının üzerine yaz](backup-azure-sql-database.md#restore-and-overwrite-the-database). Bu yordam, Kurtarma Hizmetleri kasası açık ve geri yükleme yapılandırması menüde başladığınız varsayılır. Değilseniz, bir bölümle Başlat [bir SQL veritabanını geri](backup-azure-sql-database.md#restore-a-sql-database).

> [!NOTE]
> Aynı Azure bölgesindeki bir SQL Server veritabanını geri yükleyebilirsiniz ve hedef sunucusunun kurtarma Hizmetleri kasasına kayıtlı olması gerekir. 
>

**Sunucu** açılan menü yalnızca kurtarma Hizmetleri kasasına kayıtlı SQL sunucuları gösterir. İstediğiniz sunucu kullanımda değilse **sunucu** listesinde, bölümüne bakın [Bul SQL server veritabanları](backup-azure-sql-database.md#discover-sql-server-databases) sunucusunu bulmak için. Veritabanı bulma işlemi sırasında herhangi bir yeni sunucu kurtarma Hizmetleri kasasına kayıtlı.

1. İçinde **geri yükleme Yapılandırması** menüsü:

    * seçin **alternatif bir konuma**,
    * için **sunucu**, veritabanını geri yüklemek istediğiniz SQL server'ı seçin.
    * içinde **örneği** açılan menüsünde, bir SQL örneği seçin
    * içinde **geri DB adı** iletişim kutusunda, hedef veritabanı adını belirtin.
    * varsa **seçili SQL örneğinde aynı ada sahip bir veritabanı zaten varsa üzerine yaz**.
    * tıklayın **Tamam** hedefini yapılandırabileceği tamamlayın ve geri yükleme noktası seçme taşıma.

    ![Geri Yükleme Yapılandırması menüsünde alternatif bir konuma, örnek ve adı seçin](./media/backup-azure-sql-database/restore-configuration-menu.png)

2. İçinde **seçin, geri yükleme noktası** menüsünde ya da bir günlükleri (zaman içinde nokta) tercih edebilir veya tam ve değişiklik geri yükleme noktası. Belirli bir zaman içinde nokta günlüğüne geri yüklemek istiyorsanız, bu adım ile devam edin. Bir tam veya fark geri yükleme noktası geri yüklemek istiyorsanız, 3. adımına atlayabilirsiniz.

    ![geri yükleme noktası menüsünü seçin](./media/backup-azure-sql-database/recovery-point-menu.png)

    Noktaya geri yükleme noktası yalnızca tam sahip veritabanları için günlük yedeklemeler için kullanılabilir & toplu günlüğe kurtarma modeli. Bir özel-belirli bir noktaya için geri yüklemek için:

    1. Seçin **günlükleri (zaman içinde nokta)** geri yükleme seçeneği olarak.

        ![geri yükleme noktası türünü seçin](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. Altında **geri tarih/saat**, takvimi açmak için Takvim simgesine tıklayın. Tarihleri kalın kurtarma noktalarını içeren ve geçerli tarih vurgulanır. Kurtarma noktaları takvimde bir tarihi seçin. Kurtarma noktası ile tarihleri seçemezsiniz.

        ![Takvimi Aç](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Bir tarih seçin sonra zaman çizelgesi grafiği sürekli bir aralıktaki kullanılabilir kurtarma noktalarını gösterir.

    3. Zaman Çizelgesi grafiği ya da zaman iletişim kullanarak tıklayın ve kurtarma noktası için belirli bir zaman belirtmek **Tamam** geri yükleme noktası adımı tamamlamak için.
    
       ![Takvimi Aç](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        **Seçin, geri yükleme noktası** menü kapanır ve **Gelişmiş Yapılandırma** menüsü açılır.

       ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. Gelen **Gelişmiş Yapılandırma** menüsü:

        * Veritabanını çalışmaz geri yüklemeden sonra açık tutmak **kurtarma olmadan geri yükle** menüsünde **etkin**.
        * Hedef sunucuda geri yükleme konumunu değiştirmek isterseniz, yeni bir yol sağlayan **hedef** sütun.
        * tıklayın **Tamam** ayarları onaylamak ve kapatmak için **Gelişmiş Yapılandırma**.

    5. Üzerinde **geri** menüsünde tıklayın **geri** geri yükleme işi başlatılamadı. Bildirimler alanı ilerleme durumunu izleyebilirsiniz. Ayrıca, veritabanı geri yükleme işleri ilerleme durumunu izleyebilirsiniz.

       ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/restore-job-notification.png)

3. İçinde **seçin, geri yükleme noktası** menüsünde, bir kurtarma noktası seçin. Ya da bir günlükleri (zaman içinde nokta) seçebilir veya tam ve değişiklik. Zaman içinde nokta günlük geri yüklemek istiyorsanız, 2. adıma geri dönün. Bu adım, belirli tam ya da fark geri yükleme noktası geri yükler. Bu seçenek belirtilmişse, son 30 gün boyunca tam ve fark tüm kurtarma noktalarını görebilirsiniz. 30 günden eski kurtarma noktalarını görmek istiyorsanız, tıklayın **filtre** açmak için **filtre geri yükleme noktaları** menüsü. Değişiklik kurtarma noktasını seçerseniz, Azure Backup önce uygun bir tam kurtarma noktasını geri yükler ve ardından seçilen değişiklik kurtarma noktası uygular.

    ![geri yükleme noktası menüsünü seçin](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. İçinde **seçin, geri yükleme noktası** menüsü, select **tam ve değişiklik**.

       ![geri yükleme noktası menüsünü seçin](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        Kullanılabilir kurtarma noktaları listesi görüntülenir.

    2. Kurtarma noktaları listesinden bir kurtarma noktası seçin ve **Tamam** geri yükleme noktası yordamı tamamlamak için. 

        ![tam kurtarma noktası seçin](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        **Geri yükleme noktası** menü kapanır ve **Gelişmiş Yapılandırma** menüsü açılır.

        ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. Gelen **Gelişmiş Yapılandırma** menüsü:

        * Veritabanını çalışmaz geri yüklemeden sonra açık tutmak **kurtarma olmadan geri yükle** menüsünde **etkin**. **Kurtarma olmadan geri yükle** varsayılan olarak devre dışıdır.
        * Hedef sunucuda geri yükleme konumunu değiştirmek isterseniz, yeni bir yol sağlayan **hedef** sütun.
        * Tıklayın **Tamam** ayarları onaylamak ve kapatmak için **Gelişmiş Yapılandırma**.

    4. Üzerinde **geri** menüsünde tıklayın **geri** geri yükleme işi başlatılamadı. Bildirimler alanı ilerleme durumunu izleyebilirsiniz. Ayrıca, veritabanı geri yükleme işleri ilerleme durumunu izleyebilirsiniz.

       ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/restore-job-notification.png)

### <a name="restore-and-overwrite-the-database"></a>Geri yükleme ve veritabanının üzerine yaz

Bu yordam, verilerin geri yüklenmesi ve bir veritabanının üzerine aracılığıyla size yol gösterir. Alternatif bir konuma geri yüklemek istiyorsanız, bölümüne atla [alternatif bir konuma geri](backup-azure-sql-database.md#restore-to-an-alternate-location). Bu yordam, Kurtarma Hizmetleri kasası açık ve altındadır varsayar **geri yükleme Yapılandırması** menüsünü (aşağıdaki resme bakın). Değilseniz, bir bölümle Başlat [bir SQL veritabanını geri](backup-azure-sql-database.md#restore-a-sql-database).

![Geri Yükleme Yapılandırması menüsünü açmak için Yapılandır'a tıklayın](./media/backup-azure-sql-database/restore-overwrite-db.png)

**Sunucu** açılan menü yalnızca kurtarma Hizmetleri kasasına kayıtlı SQL sunucuları gösterir. İstediğiniz sunucu kullanımda değilse **sunucu** listesinde, bölümüne bakın [Bul SQL server veritabanları](backup-azure-sql-database.md#discover-sql-server-databases) sunucusunu bulmak için. Veritabanı bulma işlemi sırasında herhangi bir yeni sunucu kurtarma Hizmetleri kasasına kayıtlı.

1. İçinde **geri yükleme Yapılandırması** menüsünde **üzerine DB** tıklatıp **Tamam** hedefini yapılandırabileceği tamamlanması. 

   ![DB üzerine tıklayın](./media/backup-azure-sql-database/restore-configuration-overwrite-db.png)

    **Sunucu**, **örneği**, ve **geri DB adı** iletişim kutuları gerekli değildir.

2. İçinde **seçin, geri yükleme noktası** menüsünde ya da bir günlükleri (zaman içinde nokta) tercih edebilir veya tam ve değişiklik geri yükleme noktası. Zaman içinde nokta günlük geri yüklemek istiyorsanız, bu adım ile devam edin. Tam ve değişiklik geri yükleme noktası geri yüklemek istiyorsanız, 3. adımına atlayabilirsiniz.

    ![geri yükleme noktası menüsünü seçin](./media/backup-azure-sql-database/recovery-point-menu.png)

    Noktaya geri yükleme noktası yalnızca tam sahip veritabanları için günlük yedeklemeler için kullanılabilir & toplu günlüğe kurtarma modeli. İkinci bir nokta belirli bir noktaya geri yükleme seçmek için:

    1. Seçin **günlükleri (zaman içinde nokta)** geri yükleme seçeneği olarak.

        ![geri yükleme noktası türünü seçin](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. Altında **geri tarih/saat**, takvimi açmak için Takvim simgesine tıklayın. Tarihleri kalın kurtarma noktalarını içeren ve geçerli tarih vurgulanır. Kurtarma noktaları takvimde bir tarihi seçin. Kurtarma noktası ile tarihleri seçemezsiniz.

        ![Takvimi Aç](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Bir tarih seçin sonra zaman çizelgesi grafiği kullanılabilir kurtarma noktalarını gösterir.

    3. Zaman Çizelgesi grafiği ya da zaman iletişim kullanarak tıklayın ve kurtarma noktası için belirli bir zaman belirtmek **Tamam** geri yükleme noktası adımı tamamlamak için.
    
       ![Takvimi Aç](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        **Seçin, geri yükleme noktası** menü kapanır ve **Gelişmiş Yapılandırma** menüsü açılır.

       ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. Gelen **Gelişmiş Yapılandırma** menüsü:

        * Veritabanını çalışmaz geri yüklemeden sonra açık tutmak **kurtarma olmadan geri yükle** menüsünde **etkin**.
        * Hedef sunucuda geri yükleme konumunu değiştirmek isterseniz, yeni bir yol sağlayan **hedef** sütun.
        * tıklayın **Tamam** ayarları onaylamak ve kapatmak için **Gelişmiş Yapılandırma**.

    5. Üzerinde **geri** menüsünde tıklayın **geri** geri yükleme işi başlatılamadı. Bildirimler alanı ilerleme durumunu izleyebilirsiniz. Ayrıca, veritabanı geri yükleme işleri ilerleme durumunu izleyebilirsiniz.

       ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/restore-job-notification.png)

3. İçinde **seçin, geri yükleme noktası** menüsünde, bir kurtarma noktası seçin. Ya da bir günlükleri (zaman içinde nokta) seçebilir veya tam ve değişiklik. Zaman içinde nokta günlük geri yüklemek istiyorsanız, 2. adıma geri dönün. Bu adım, belirli tam ya da fark geri yükleme noktası geri yükler. Bu seçenek belirtilmişse, son 30 gün boyunca tam ve fark tüm kurtarma noktalarını görebilirsiniz. 30 günden eski kurtarma noktalarını görmek istiyorsanız, tıklayın **filtre** açmak için **filtre geri yükleme noktaları** menüsü. Değişiklik kurtarma noktasını seçerseniz, Azure Backup önce uygun bir tam kurtarma noktasını geri yükler ve ardından seçilen değişiklik kurtarma noktası uygular.

    ![geri yükleme noktası menüsünü seçin](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. İçinde **seçin, geri yükleme noktası** menüsü, select **tam ve değişiklik**.

       ![geri yükleme noktası menüsünü seçin](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        Kullanılabilir kurtarma noktaları listesi görüntülenir.

    2. Kurtarma noktaları listesinden bir kurtarma noktası seçin ve **Tamam** geri yükleme noktası yordamı tamamlamak için. 

        ![tam kurtarma noktası seçin](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        **Geri yükleme noktası** menü kapanır ve **Gelişmiş Yapılandırma** menüsü açılır.

        ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. Gelen **Gelişmiş Yapılandırma** menüsü:

        * Veritabanını çalışmaz geri yüklemeden sonra açık tutmak **kurtarma olmadan geri yükle** menüsünde **etkin**. **Kurtarma olmadan geri yükle** varsayılan olarak devre dışıdır.
        * Hedef sunucuda geri yükleme konumunu değiştirmek isterseniz, yeni bir yol sağlayan **hedef** sütun.
        * Tıklayın **Tamam** ayarları onaylamak ve kapatmak için **Gelişmiş Yapılandırma**.

    4. Üzerinde **geri** menüsünde tıklayın **geri** geri yükleme işi başlatılamadı. Bildirimler alanı ilerleme durumunu izleyebilirsiniz. Ayrıca, veritabanı geri yükleme işleri ilerleme durumunu izleyebilirsiniz.

       ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/restore-job-notification.png)


## <a name="manage-azure-backup-operations-for-sql-on-azure-vms"></a>Azure vm'lerde SQL Azure yedekleme işlemlerini yönetme

Bu bölümde, Azure sanal makinelerinde çeşitli Azure yedekleme yönetim işlemlerini kullanılabilir SQL hakkında bilgi sağlar. Aşağıdaki üst düzey işlemleri mevcuttur:

* İşleri İzleme
* Yedekleme uyarıları
* Bir SQL veritabanı'nda korumayı Durdur
* SQL veritabanı korumasını sürdürme
* Bir geçici yedekleme işi tetikleme
* SQL sunucusunun kaydını kaldırma

### <a name="monitor-jobs"></a>İşleri İzleme
Kurumsal sınıf bir çözümü olan azure Backup, Gelişmiş yedekleme uyarıları ve hatalar için bildirim (Yedekleme uyarıları bölümüne bakın) sağlar. Hala belirli işleri izlemek istiyorsanız, ihtiyacınıza göre aşağıdaki seçeneklerden herhangi birini kullanabilirsiniz:

#### <a name="use-azure-portal-for-all-adhoc-operations"></a>Tüm geçici işlemleri için Azure portalını kullanma
Azure yedekleme gösterir tüm el ile tetiklenen veya geçici, yedekleme işleri portalında işler. Portal Ekle kullanılabilir işleri: tüm yedekleme işlemleri yapılandırmak, el ile tetiklenen yedekleme işlemleri, geri yükleme işlemleri, kayıt ve veritabanı işlemleri bulmak ve yedekleme işlemleri durdurun. 
![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/jobs-list.png)

> [!NOTE]
> Tam da dahil olmak üzere tüm zamanlanan yedekleme işlerinin değişiklik ve günlük yedekleme portalında gösterilmez ve aşağıda açıklandığı gibi SQL Server Management Studio kullanılarak izlenebilir.
>

#### <a name="use-sql-server-management-studio-for-backup-jobs"></a>Yedekleme işleri için SQL Server Management Studio kullanma
Azure Backup, SQL yerel API'lerin tüm yedekleme işlemleri için kullanır. Yerel API ile tüm iş bilgileri getirebilir [SQL yedek kümesi tablo](https://docs.microsoft.com/sql/relational-databases/system-tables/backupset-transact-sql?view=sql-server-2017) msdb veritabanında.

Aşağıdaki örnekte adlı bir veritabanı için tüm yedekleme işleri getirilecek bir sorgudur **DB1**. Daha gelişmiş izleme sorgu özelleştirin.
```
select CAST (
Case type
                when 'D' 
                                 then 'Full'
                when  'I'
                               then 'Differential' 
                ELSE 'Log'
                END         
                AS varchar ) AS 'BackupType',
database_name, 
server_name,
machine_name,
backup_start_date,
backup_finish_date,
DATEDIFF(SECOND, backup_start_date, backup_finish_date) AS TimeTakenByBackupInSeconds,
backup_size AS BackupSizeInBytes
  from msdb.dbo.backupset where user_name = 'NT SERVICE\AzureWLBackupPluginSvc' AND database_name =  <DB1>  
 
```

### <a name="backup-alerts"></a>Yedekleme uyarıları

Bazen yedekleme işlerini izleme günlüğü yedeklemeleri ile her 15 dakikada gerçekleşen can sıkıcı olabilir. Azure yedekleme, herhangi bir yedekleme hata tarafından tetiklenen e-posta uyarıları sağlayarak potansiyel olarak tedious bu durum için planlanan. Uyarılar veritabanı düzeyinde hata koduna göre birleştirilir. Örneğin, bir veritabanı her hata için bir uyarı almak yerine, birden fazla yedekleme hatasına varsa, ilk başarısızlık e-posta alırsınız. Ardından Azure portalında bu veritabanı için sonraki hataları izlemek için de kaydolabilirsiniz. 

Yedekleme uyarıları izlemek için:

1. Azure aboneliğinizde oturum [Azure portalında](https://portal.azure.com).

2. Açık kurtarma Hizmetleri kasası ile SQL sanal makinesi kayıtlı.

3. Kurtarma Hizmetleri kasası menüsünde seçin **uyarıları ve olayları**. 

   ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

4. İçinde **uyarıları ve olayları** menüsünde **yedekleme uyarıları** uyarıların listesini görüntülemek için.

   ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

### <a name="stop-protection-on-a-sql-server-database"></a>Bir SQL Server veritabanının korumasını durdurun

Bir SQL Server veritabanını korumayı durdurursanız, Azure Backup kurtarma noktalarını tutmak isteyip istemediğinizi sorar. SQL veritabanı korumayı durdurmanın iki yolu vardır:

* Gelecek tarihli tüm yedekleme işlerini durdurma ve tüm kurtarma noktalarını silme
* Gelecek tarihli tüm yedekleme işlerini durdurma ama kurtarma noktalarını bırakma 

SQL için kurtarma noktalarını SQL korunan örnek ücreti yanı sıra, kullanılan depolama fiyatlandırma taşıyan olarak kurtarma noktalarını bırakmanın bir maliyeti taşır. SQL Azure Backup fiyatlandırması hakkında daha fazla bilgi için bkz. [Azure Backup fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/backup/). Veritabanı korumasını durdurmak için:

1. Açık kurtarma Hizmetleri kasası ile SQL sanal makinesi kayıtlı.

2. Kasa panosunda seçin **kullanım** yedekleme yedekleme öğeleri menüsünü açmak için öğeleri.

    ![Tıklama + Backup, yedekleme hedefi menüsünde açmak için](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. İçinde **yedekleme öğeleri** menüsünde Yedekleme Yönetimi türü seçin **Azure VM'deki SQL**. 

    ![Tıklama + Backup, yedekleme hedefi menüsünde açmak için](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    Yedekleme öğeleri listesi, SQL veritabanlarının listesini göstermek için ayarlar. 

4. SQL veritabanları listesinden korumayı durdurmak için istediğiniz veritabanını seçin.

    ![SQL Azure VM'de listeden seçin.](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Veritabanına seçtiğinizde, alt menüsü açılır. 

5. Seçili veritabanı menüden **yedeklemeyi Durdur** veritabanını korumayı durdurmak için.

    ![geri yükleme Veritabanı Seç](./media/backup-azure-sql-database/stop-db-button.png)

    **Yedeklemeyi Durdur** menüsü açılır.

6. İçinde **yedeklemeyi Durdur** menüsünde, yedekleme verilerini koru seçin ya da yedekleme verilerini sil. İsteğe bağlı olarak, koruma ve bir açıklama durdurma nedeni sağlayabilirsiniz.

    ![geri yükleme Veritabanı Seç](./media/backup-azure-sql-database/stop-backup-button.png)

7. Tıklayın **yedeklemeyi Durdur** veritabanı korumasını durdurmak için. 

### <a name="resume-protection-for-a-sql-database"></a>SQL veritabanı korumasını sürdürme

Varsa **yedekleme verilerini koru** seçeneği durdurma koruma SQL veritabanı için korumayı sürdürmek, mümkün olduğunda seçilmiştir. Koruma, yedekleme verileri korunur değil, devam edilemiyor. 

1. SQL veritabanı için korumayı sürdürmek için yedekleme öğesi açın ve **sürdürme yedekleme**.

    ![Veritabanı korumasını sürdürme](./media/backup-azure-sql-database/resume-backup-button.png)

   Yedekleme İlkesi menüsü açılır.

2. Gelen **yedekleme İlkesi** menüsünden bir ilkeyi seçin ve tıklayın **Kaydet**.

### <a name="trigger-an-adhoc-backup"></a>Geçici bir yedeklemeyi tetikleyin

İstediğiniz zaman geçici bir yedeklemeyi tetikleme yapılmasını sağlayabilirsiniz. Geçici yedekleme dört türleri vardır. Her tür hakkında ayrıntılı bilgi görmek için makaleyi [türleri, SQL yedeklerini](https://docs.microsoft.com/sql/relational-databases/backup-restore/backup-overview-sql-server?view=sql-server-2017#types-of-backups).

* Tam yedekleme
* Yalnızca tam yedek Kopyala
* Fark yedekleme
* Günlük yedekleme

### <a name="unregister-a-sql-server"></a>SQL sunucusunun kaydını kaldırma

Bir SQL server korumasını kaldırdıktan sonra ancak kasayı silmeden önce kaydını kaldırmak için

1. Açık kurtarma Hizmetleri kasası ile SQL sanal makinesi kayıtlı.

2. İçinde **Yönet** bölümünde, kasa menüsünü tıklatın **Yedekleme Altyapısı**.  

   ![Veritabanı korumasını sürdürme](./media/backup-azure-sql-database/backup-infrastructure-button.png)

3. İçinde **yönetim sunucuları** bölümünde **korumalı sunucuların**.

   ![Veritabanı korumasını sürdürme](./media/backup-azure-sql-database/protected-servers.png)

    Korumalı sunucular menüsü açılır. 

4. İçinde **korumalı sunucuların** menüsünde kaydını kaldırmak istediğiniz sunucuyu seçin. Kasayı silmek istiyorsanız, tüm sunucuların kaydını silmeniz gerekir.

5. Korumalı sunucular menüsünde, korumalı sunucuya sağ tıklayın ve seçin **Sil**. 

   ![Veritabanı korumasını sürdürme](./media/backup-azure-sql-database/delete-protected-server.png)

## <a name="sql-database-backup-faq"></a>SQL veritabanını yedekleme hakkında SSS

Aşağıdaki bölümde, SQL veritabanı yedeklemesi hakkında ek bilgi sağlar.

### <a name="can-i-throttle-the-speed-of-the-sql-backup-policy-so-it-minimizes-impact-on-the-sql-server"></a>SQL yedekleme İlkesi hızını, SQL server üzerindeki etkiyi en aza indirir şekilde kısıtlayabilir miyim

Evet, yedekleme İlkesi yürütür oranı'sini kısıtlayabilir. Bu ayarı değiştirmek için:

1. SQL Server'da içinde `C:\Program Files\Azure Workload Backup\bin` açık klasör **TaskThrottlerSettings.json**.

2. İçinde **TaskThrottlerSettings.json** dosya, değişiklik **DefaultBackupTasksThreshold** daha düşük bir değere, örneğin, 5.

3. Değişikliğinizi kaydedin ve dosyayı kapatın.

4. SQL Server, Görev Yöneticisi'ni açın ve yeniden **Azure Backup iş yükü Koordinatör hizmeti**.

### <a name="can-i-run-a-full-backup-from-a-secondary-replica"></a>Bir ikincil çoğaltma tam yedekleme çalıştırabilir miyim

Hayır, bu özellik desteklenmiyor.

### <a name="do-successful-backup-jobs-create-alerts"></a>Başarılı yedekleme işleri uyarılar oluşturulur

Hayır. Başarılı yedekleme işleri uyarılar oluşturmaz. Uyarılar, başarısız olan yedekleme işleri için gönderilir.

### <a name="are-scheduled-backup-job-details-shown-in-the-jobs-menu"></a>Zamanlanmış yedekleme işinin ayrıntılarını işler menüde gösterilir

Hayır. İşleri menüsünden geçici işi ayrıntılarını gösterir, ancak zamanlanan yedekleme işlerinin göstermez. Zamanlanmış tüm yedekleme işleri başarısız olursa, başarısız işi uyarıları tüm ayrıntıları bulabilirsiniz. İzleyici tüm zamanlanan ve geçici yedekleme işleri istiyorsanız [SQL Server Management Studio'yu](backup-azure-sql-database.md#use-sql-server-management-studio-for-backup-jobs).

### <a name="if-i-select-a-sql-server-will-future-databases-automatically-be-added"></a>Gelecekteki veritabanları, otomatik olarak bir SQL sunucusu eklenir

Hayır. Sunucu düzeyinde onay kutusunu seçerseniz, bir SQL server için koruma yapılandırılıyor, tüm veritabanları ekler. Ancak, koruma yapılandırdıktan sonra SQL Server'a veritabanları ekleme, bunları korumak için yeni veritabanlarını elle eklemeniz gerekir. Veritabanlarını yapılandırılmış korumayı otomatik olarak dahil edilmez.

### <a name="if-i-change-the-recovery-model-how-do-i-restart-protection"></a>Kurtarma modeli değiştirirsem nasıl koruma yeniden

Kurtarma modeli değiştirirseniz, bir tam yedekleme tetikleyin ve günlük yedeklemeler beklendiği gibi başlar.

## <a name="next-steps"></a>Sonraki adımlar

Azure Backup hakkında daha fazla bilgi edinmek için, şifrelenmiş sanal makinelerin yedeklenmesine yönelik PowerShell örneğine bakın.

> [!div class="nextstepaction"]
> [Şifrelenmiş sanal makineyi yedekleme](./scripts/backup-powershell-sample-backup-encrypted-vm.md)
