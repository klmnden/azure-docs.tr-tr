---
title: SQL Server veritabanlarını azure'a yedekleyin | Microsoft Docs
description: Bu öğreticide, SQL Server'ı Azure'a yedekleme açıklanmaktadır. Makalede, SQL Server kurtarma de açıklanmaktadır.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: raynew
ms.openlocfilehash: 50085336c59f2284f357e32b875eae08ff90d30f
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2018
ms.locfileid: "53790184"
---
# <a name="back-up-sql-server-databases-to-azure"></a>SQL Server veritabanlarını Azure'a yedekleme

SQL Server veritabanları, düşük kurtarma noktası hedefi (RPO) ve uzun süreli saklama gerektiren kritik iş yükleri değildir. Azure yedekleme sıfır altyapı gerektiren bir SQL Server yedekleme çözümü sağlar: karmaşık bir yedekleme sunucusu değildir, yönetim Aracısı değildir ve hiçbir yedekleme depolama yönetmek için. Azure yedekleme, SQL Server veya hatta farklı iş yüklerini çalıştıran tüm sunucular arasında yedeklemeleriniz için merkezi yönetim sağlar.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Bir SQL Server örneğini ayarlama Azure'a yedeklemek için Önkoşullar.
> * Nasıl oluşturabilir ve bir kurtarma Hizmetleri kasası kullanın.
> * SQL Server Veritabanı Yedeklemeleri Yapılandırma
> * Kurtarma noktaları için bir yedekleme (veya bekletme) ilkesi kurulur.
> * Veritabanını geri yükleme.

Bu makalede yordamlarına başlamadan önce Azure'da çalışan bir SQL Server örneği olmalıdır. [Kolayca bir SQL Server örneği oluşturmak için SQL Marketplace sanal makineleri'nin](../sql-database/sql-database-get-started-portal.md).

## <a name="public-preview-limitations"></a>Genel Önizleme sınırlamaları

Aşağıdaki öğeler bilinen sınırlamalar genel Önizleme aşamasında:

- SQL sanal makinesi (VM), Azure genel IP adresleri erişmek için Internet bağlantısı gerektirir. Ayrıntılar için bkz [ağ bağlantısı kurma](backup-azure-sql-database.md#establish-network-connectivity).
- Bir kurtarma Hizmetleri kasası en fazla 2.000 SQL veritabanlarında koruyun. Ek SQL veritabanı ayrı bir kurtarma Hizmetleri Kasası'nda depolanması gerekir.
- [Dağıtılmış kullanılabilirlik grupları yedekleri](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/distributed-availability-groups?view=sql-server-2017) sınırlamaları vardır.
- SQL Server her zaman üzerinde yük devretme kümesi örnekleri (Fcı'lerde) için yedekleme desteklenmez.
- SQL Server veritabanlarını korumak için Azure yedeklemeyi yapılandırmak için Azure portalını kullanın. Azure PowerShell, Azure CLI ve REST API'ler şu anda desteklenmemektedir.
- FCI yansıtma veritabanı, veritabanı anlık görüntüleri ve veritabanları için yedekleme/geri yükleme işlemleri desteklenmez.
- Veritabanı ile çok sayıda dosya korunamaz. Desteklenen dosyalar sayısı, yalnızca dosya sayısına bağlıdır ancak aynı zamanda dosya yolu uzunluğu üzerinde bağlıdır çünkü çok belirleyici bir sayı değil. Böyle durumlarda, ancak daha az yaygın. Bu durumu çözmek için bir çözüm oluşturuyor.

Lütfen [SSS bölümüne](https://docs.microsoft.com/azure/backup/backup-azure-sql-database#faq) destek/değil hakkında daha fazla ayrıntı için desteklenen senaryolar.

## <a name="support-for-azure-geos"></a>Azure coğrafi desteği

Azure yedekleme, aşağıdaki coğrafi bölgeler için desteklenir:

- Avustralya Güneydoğu (ASE)
- Brezilya Güney (BRS)
- Kanada Orta (CNC)
- Kanada Doğu (CE)
- Orta ABD (CUS)
- Doğu Asya (EA)
- Doğu Avustralya (AE)
- Doğu ABD (EUS)
- Doğu ABD 2 (EUS2)
- Hindistan Orta (INC)
- Hindistan Güney (INS)
- Japonya Doğu (JPE)
- Japonya Batı (JPW)
- Kore Orta (KRC)
- Kore Güney (KRS)
- Orta Kuzey ABD (NCUS)
- Kuzey Avrupa (NE)
- Orta Güney ABD (SCUS)
- Güneydoğu Asya (SEA)
- UK Güney (UKS)
- UK Batı (UKW)
- Batı Orta ABD (WCUS)
- Batı Avrupa (WE)
- Batı ABD (WUS)
- Batı ABD 2 (WUS 2)

## <a name="support-for-operating-systems-and-sql-server-versions"></a>İşletim sistemleri ve SQL Server sürümleri desteği

Bu bölümde, Azure Backup işletim sistemleri ve SQL Server sürümleri desteği açıklanmaktadır. SQL Marketplace Azure sanal makineler ve Market dışı sanal makineler (SQL Server elle yüklendiği) desteklenir.

### <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

Linux şu anda desteklenmemektedir.

### <a name="supported-sql-server-versions-and-editions"></a>Desteklenen SQL Server sürümleri ve sürümler

- SQL Server 2012 Enterprise, Standard, Web, Developer, Express
- SQL Server 2014 Enterprise, Standard, Web, Developer, Express
- SQL Server 2016 Enterprise, Standard, Web, Developer, Express
- SQL Server 2017 Enterprise, Standard, Web, Developer, Express

## <a name="prerequisites"></a>Önkoşullar

SQL Server veritabanınızı yedekleyin önce aşağıdaki koşulları denetleyin:

- Tanımlamak veya [bir kurtarma Hizmetleri kasası oluşturma](backup-azure-sql-database.md#create-a-recovery-services-vault) aynı bölge veya yerel ayar olarak, SQL Server örneğini barındıran sanal makine.
- [Sanal makinenin izinlerini denetleyin](backup-azure-sql-database.md#set-permissions-for-non-marketplace-sql-vms) SQL veritabanlarını yedeklemek için gerekli.
- Doğrulayın [SQL sanal makinenin ağ bağlantısı var](backup-azure-sql-database.md#establish-network-connectivity).
- SQL veritabanları olarak başına adlı olup olmadığını denetleyin [adlandırma kuralları](backup-azure-sql-database.md#sql-database-naming-guidelines-for-azure-backup) Azure Backup'ın başarılı olarak yedek alabilir.

> [!NOTE]
> SQL Server veritabanlarını yedeklemek için bir anda yalnızca bir yedekleme çözümü olabilir. Bu özelliği kullanmadan önce diğer tüm SQL yedeklerini devre dışı; Aksi takdirde, yedeklemeleri müdahale başarısız ve. Azure Backup Iaas VM için birlikte SQL yedekleme herhangi bir çakışma olmadan etkinleştirebilirsiniz.
>

Bu koşullar, ortamınızda varsa, devam [SQL Server veritabanları için yedekleme yapılandırmaya](backup-azure-sql-database.md#configure-backup-for-sql-server-databases). Önkoşullar mevcut değilse, okumaya devam edin.


## <a name="establish-network-connectivity"></a>Ağ bağlantısı kurma

Tüm işlemler için Azure genel IP adreslerine bağlantısı SQL sanal makinesi gerekir. SQL sanal makine işlemleri (veritabanı bulma gibi yedeklemeleri yapılandırma, yedeklemeler zamanlamak, Kurtarma noktalarını geri ve benzeri) genel IP adreslerine bağlantısı olmadan başarısız. Yedekleme trafiği için bir yol sağlamak için aşağıdaki seçeneklerden birini kullanın:

- Beyaz liste Azure veri merkezi IP aralıkları: Azure veri merkezi IP aralıklarını güvenilir listeye kullanın [IP aralıkları ve yönergeleri hakkında ayrıntılı bilgi için İndirme Merkezi sayfasında](https://www.microsoft.com/download/details.aspx?id=41653).
- Trafiği yönlendirmek için bir HTTP proxy sunucusu dağıtın: Bir SQL veritabanı'nda VM yedeklediğinizde, VM'deki yedekleme uzantısına Azure Backup ve Azure Depolama'ya veri yönetimi komutları göndermek için HTTPS API'lerini kullanır. Backup uzantısı, Azure Active Directory (Azure AD) kimlik doğrulaması için de kullanır. HTTP proxy üzerinden bu üç hizmeti yedekleme uzantısını trafiği yönlendirme. Uzantı genel internet erişimi için yapılandırılan tek bileşen güçlendirin.

Bir denge seçenekleri, yönetilebilirlik, ayrıntılı bir denetim ve Maliyet ' dir.

> [!NOTE]
> Hizmet etiketleri Azure Backup'ın, genel kullanılabilirlik tarafından kullanılabilir olması gerekir.
>

| Seçenek | Yararları | Dezavantajları |
| ------ | ---------- | ------------- |
| Beyaz liste IP aralıkları | Ek maliyet olmadan. <br/> Bir NSG içinde açmak erişimi için kullandığınız **kümesi AzureNetworkSecurityRule** cmdlet'i. | Etkilenen IP aralıklarını zamanla değişir olduğundan yönetmek için karmaşık. <br/>Azure, yalnızca Azure Storage'nın tam erişim sağlar.|
| Bir HTTP Ara sunucusunu kullanacak   | Depolama üzerinde ayrıntılı denetim proxy'sinde URL'leri izin verilir. <br/>Vm'leri tek noktası internet erişimi. <br/> Azure IP adresi değişiklikleri tabi değildir. | Bir VM ile Ara yazılım çalıştırmak için ek ücretler. |

## <a name="set-permissions-for-non-marketplace-sql-vms"></a>İçin olmayan Market SQL VM'lerin izinleri ayarlama

Bir sanal makineyi yedeklemek için Azure Backup gerektirir **AzureBackupWindowsWorkload** uzantısı yüklenecek. Azure Market sanal makineler kullanırsanız, devam [Bul SQL Server veritabanlarını](backup-azure-sql-database.md#discover-sql-server-databases). Azure Market'ten SQL veritabanlarınızı barındıran sanal makine oluşturulmadı, uzantıyı yüklemek ve uygun izinleri ayarlamak için aşağıdaki yordamı tamamlayın. Ek olarak **AzureBackupWindowsWorkload** uzantısı, Azure Backup, SQL veritabanlarını korumak için SQL sysadmin ayrıcalıkları gerektirir. Eşitlemesini bulmak sanal makinede veritabanları, Azure Backup, hesap oluşturur **NT SERVICE\AzureWLBackupPluginSvc**. Bu hesap, yedekleme ve geri yükleme için kullanılır ve SQL sysadmin iznine sahip olması gerekir. Ayrıca, Azure Backup özelliğinden yararlanır **NT AUTHORITY\SYSTEM** SQL ortak bir oturum açma olacak şekilde bu hesabınızın olması gerekir böylece DB bulma/sorgulama için hesap.

İzinleri yapılandırmak için:

1. İçinde [Azure portalında](https://portal.azure.com), SQL veritabanlarını korumak için kullandığınız bir kurtarma Hizmetleri kasasını açın.

2. Üzerinde **kurtarma Hizmetleri kasası** panoyu seçin **yedekleme**. **Yedekleme hedefi** menüsü açılır.

   ![Yedekleme yedekleme hedefi menüsünü açmak için seçin](./media/backup-azure-sql-database/open-backup-menu.png)

3. Üzerinde **yedekleme hedefi** menüsünde ayarlamak **iş yükünüz çalıştığı** varsayılan: **Azure**.

4. Genişletin **neleri yedeklemek istiyorsunuz** aşağı açılan liste kutusunu seçip alt **Azure VM'de SQL Server**.

    ![SQL Server Azure VM için yedeklemeyi seçin.](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    **Yedekleme hedefi** menü iki adımı görüntüler: **VM'lerin içinde VT Bul** ve **yedeklemeyi yapılandırma**. **VM'lerin içinde VT Bul** adım, Azure sanal makineler için bir arama başlatın.

    ![İki yedekleme hedefi adımlarını gözden geçirin](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

5. Altında **VM'lerin içinde VT Bul**seçin **bulmayı Başlat** Abonelikteki korumasız sanal makinelerin aranacak. Bu, tüm sanal makinelerin aramak için biraz zaman alabilir. Arama süresini abonelik korumasız sanal makinelerin sayısına bağlı olarak değişir.

    ![Yedekleme Beklemede sırasında VM'lerin içinde VT arayın](./media/backup-azure-sql-database/discovering-sql-databases.png)

6. Sanal makineler listesinde, yedeklenecek veritabanını ve ardından SQL sahip sanal Makineyi seçin **Bul DBs**.

    Bulma işlemi yükler **AzureBackupWindowsWorkload** sanal makinede uzantı. Uzantı, SQL veritabanlarını yedeklemek için sanal makineyle iletişim kurmak Azure Backup hizmeti sağlar. Uzantıyı yükledikten sonra Azure Backup, Windows sanal hizmet hesabı oluşturur. **NT Service\AzureWLBackupPluginSvc** sanal makinede. Sanal hizmet hesabı SQL sysadmin izni gerektirir. Hatasını alırsanız sanal hizmet hesabı yükleme işlemi sırasında `UserErrorSQLNoSysadminMembership`, bkz: [düzeltme SQL sysadmin izinleri](backup-azure-sql-database.md#fix-sql-sysadmin-permissions).

    **Bildirimleri** alanı veritabanı bulma ilerlemesini gösterir. Bu işin tamamlanması biraz sürebilir. İş saati kaç veritabanları sanal makine üzerinde bağlıdır. Seçili veritabanlarındaki bulunduğunda bir başarı iletisi görünür.

    ![Dağıtım başarılı iletisi](./media/backup-azure-sql-database/notifications-db-discovered.png)

Veritabanı kurtarma Hizmetleri kasası ile ilişkilendirdikten sonra sonraki adım olarak [yedekleme işini yapılandırmak](backup-azure-sql-database.md#configure-backup-for-sql-server-databases).

### <a name="fix-sql-sysadmin-permissions"></a>SQL sysadmin izinleri Düzelt

Hatasını alırsanız yükleme işlemi sırasında `UserErrorSQLNoSysadminMembership`, SQL Server Management Studio (SSMS) oturum açmak için SQL Server sysadmin izinleri olan bir hesap kullanın. Özel izinler gerekmedikçe, Windows kimlik doğrulama çalışması gerekir.

1. SQL Server'da Güvenlik/oturum açma bilgileri klasörü açın.

    ![Hesapları görmek için güvenlik/oturum açma bilgileri klasörünü açın](./media/backup-azure-sql-database/security-login-list.png)

2. Oturum açma bilgileri klasörüne sağ tıklayıp **yeni oturum açma**. İçinde **oturum açma - yeni** iletişim kutusunda **arama**.

    ![Oturum Aç - yeni iletişim kutusu, Ara'yı seçin](./media/backup-azure-sql-database/new-login-search.png)

3. Windows sanal hizmet hesabı **NT SERVICE\AzureWLBackupPluginSvc** SQL bulma aşamasından ve sanal makine kaydı sırasında oluşturuldu. Gösterildiği gibi hesap adını girin **Seçilecek nesne adını girin** kutusu. Seçin **Adları Denetle** adı çözümlenemedi.

    ![Bilinmeyen hizmet adını çözümlemek için adları denetle seçin](./media/backup-azure-sql-database/check-name.png)

4. Seçin **Tamam** iletişim kutusunu kapatın.

5. İçinde **sunucu rolleri** kutusunda, emin **sysadmin** rolü seçilir. Seçin **Tamam** iletişim kutusunu kapatın.

    ![Sysadmin sunucu rolünün seçili olduğundan emin olun](./media/backup-azure-sql-database/sysadmin-server-role.png)

    Gerekli izinleri olması gerekir.

6. İzinleri hatasını düzelttik olsa da, veritabanı kurtarma Hizmetleri kasası ile ilişkilendirmek gerekir. Azure portalında, **korumalı sunucuların** listesinde, bir hata durumu ve select sunucusuna sağ tıklayın **yeniden Bul DBs**.

    ![Sunucunun uygun izinlere sahip olun](./media/backup-azure-sql-database/check-erroneous-server.png)

    **Bildirimleri** alanı veritabanı bulma ilerlemesini gösterir. Bu işin tamamlanması biraz sürebilir. İş saati kaç veritabanları sanal makine üzerinde bağlıdır. Seçili veritabanlarındaki bulunduğunda bir başarı iletisi görünür.

    ![Dağıtım başarılı iletisi](./media/backup-azure-sql-database/notifications-db-discovered.png)

Veritabanı kurtarma Hizmetleri kasası ile ilişkilendirdikten sonra sonraki adım olarak [yedekleme işini yapılandırmak](backup-azure-sql-database.md#configure-backup-for-sql-server-databases).

## <a name="sql-database-naming-guidelines-for-azure-backup"></a>SQL veritabanı, Azure Backup için adlandırma kuralları
Iaas VM'de SQL Server için Azure Backup kullanarak kesintisiz yedeklemeleri emin olmak için aşağıdaki veritabanlarını adlandırma sırasında kaçının:

  * Baştaki/sondaki boşluk
  * Sondaki '!'

Azure tablo desteklenmeyen karakterler için diğer ad kullanımı sahibiz ancak bu de önleme öneririz. Daha fazla bilgi için bkz. Bu [makale](https://docs.microsoft.com/rest/api/storageservices/Understanding-the-Table-Service-Data-Model?redirectedfrom=MSDN).

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>SQL Server veritabanlarını Bul

Azure yedekleme, SQL Server örneğindeki tüm veritabanlarını bulur. Veritabanlarını yedekleme gereksinimlerinize göre koruyabilirsiniz. SQL veritabanlarını barındıran sanal makineyi tanımlamak için aşağıdaki yordamı kullanın. Sanal makine tanımladıktan sonra Azure Backup, SQL Server veritabanlarını bulmak için basit bir uzantı yükler.

1. Oturum açmak için aboneliğinizde [Azure portalında](https://portal.azure.com/).

2. Sol menüden **tüm hizmetleri**.

    ![Tüm hizmetleri seçin](./media/backup-azure-sql-database/click-all-services.png) <br/>

3. İçinde **tüm hizmetleri** iletişim kutusuna **kurtarma Hizmetleri**. Giriş, siz yazarken kaynakların listesini filtreler. Seçin **kurtarma Hizmetleri kasaları** listesinde.

  ![Girin ve kurtarma Hizmetleri kasaları seçin](./media/backup-azure-sql-database/all-services.png) <br/>

    Abonelikte kurtarma Hizmetleri kasalarının listesi görünür.

4. Kurtarma Hizmetleri kasaları listesinde SQL veritabanlarını korumak için kullanılacak kasayı seçin.

5. Üzerinde **kurtarma Hizmetleri kasası** panoyu seçin **yedekleme**. **Yedekleme hedefi** menüsü açılır.

   ![Yedekleme yedekleme hedefi menüsünü açmak için seçin](./media/backup-azure-sql-database/open-backup-menu.png)

6. Üzerinde **yedekleme hedefi** menüsünde ayarlamak **iş yükünüz çalıştığı** varsayılan: **Azure**.

7. Genişletin **neleri yedeklemek istiyorsunuz** aşağı açılan liste kutusunu seçip alt **Azure VM'de SQL Server**.

    ![SQL Server Azure VM için yedeklemeyi seçin.](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    **Yedekleme hedefi** menü iki adımı görüntüler: **VM'lerin içinde VT Bul** ve **yedeklemeyi yapılandırma**.

    ![İki yedekleme hedefi adımlarını gözden geçirin](./media/backup-azure-sql-database/backup-goal-menu-step-one.png)

8. Altında **VM'lerin içinde VT Bul**seçin **bulmayı Başlat** Abonelikteki korumasız sanal makinelerin aranacak. Bu, tüm sanal makinelerin aramak için biraz zaman alabilir. Arama süresini abonelik korumasız sanal makinelerin sayısına bağlı olarak değişir.

    ![Yedekleme Beklemede sırasında VM'lerin içinde VT arayın](./media/backup-azure-sql-database/discovering-sql-databases.png)

    Korumasız bir sanal makine bulunduktan sonra listede görünür. Birden çok sanal makineye aynı ada sahip olabilir. Ancak, sanal makineler aynı ada sahip farklı kaynak grubuna ait. Korumasız sanal makinelerin, sanal makine adı ve kaynak grubu tarafından listelenir. Beklenen bir sanal makine listede yoksa, bu sanal makinenin bir kasaya zaten korunuyor, bkz.

9. Sanal makineler listesinde, yedeklenecek veritabanını ve ardından SQL sahip sanal Makineyi seçin **Bul DBs**.

    Azure yedekleme, sanal makinede tüm SQL veritabanları bulur. Veritabanı bulma işlemi sırasında neler olduğu hakkında daha fazla bilgi için bkz: [arka plan işlemleri](backup-azure-sql-database.md#background-operations). SQL veritabanlarını bulunduktan sonra hazır [yedekleme işini yapılandırmak](backup-azure-sql-database.md#configure-backup-for-sql-server-databases).

### <a name="background-operations"></a>Arka plan işlemleri

Kullanırken **Bul DBs** aracı, Azure Backup şu işlemleri arka planda çalıştırır:

- Sanal makine iş yükü yedekleme için kurtarma Hizmetleri kasasına kaydedin. Bu kurtarma Hizmetleri kasasına kayıtlı sanal makine üzerindeki tüm veritabanlarını yedeklenebilir.

- Yükleme **AzureBackupWindowsWorkload** sanal makinede uzantı. Yedekleme bir SQL veritabanı bir aracısız çözümüdür. Sanal makinede uzantı yüklü ve SQL veritabanı'nda hiçbir aracının yüklü.

- Hizmet hesabı **NT Service\AzureWLBackupPluginSvc** sanal makinede. Tüm yedekleme ve geri yükleme işlemleri, hizmet hesabı kullanın. **NT Service\AzureWLBackupPluginSvc** SQL sysadmin izinleri olması gerekir. Tüm SQL Marketplace sanal makineler ile gelir **SqlIaaSExtension** yüklü. **AzureBackupWindowsWorkload** uzantısı kullanan **SQLIaaSExtension** otomatik olarak gerekli izinleri almak için. Sanal makineniz yoksa **SqlIaaSExtension** yüklü DB bulma işlemi hata iletisiyle başarısız `UserErrorSQLNoSysAdminMembership`. Yedekleme sysadmin izinlerini eklemek için yönergeleri izleyin. [Azure yedekleme izinleri için olmayan Market SQL VM'lerin](backup-azure-sql-database.md#set-permissions-for-non-marketplace-sql-vms).

    ![VM ve veritabanı seçin](./media/backup-azure-sql-database/registration-errors.png)

## <a name="configure-backup-for-sql-server-databases"></a>SQL Server veritabanları için yedeklemeyi yapılandırma

Azure Backup, SQL Server veritabanlarınızı koruyun ve yedekleme işlerini yönetmek için Yönetim Hizmetleri sağlar. Kurtarma Hizmetleri kasanız, yönetim ve izleme işlevleri bağlıdır.

> [!NOTE]
> SQL Server veritabanlarını yedeklemek için bir anda yalnızca bir yedekleme çözümü olabilir. Bu özelliği kullanmadan önce diğer tüm SQL yedeklerini devre dışı; Aksi takdirde, yedeklemeleri müdahale başarısız ve. Azure Backup Iaas VM için birlikte SQL yedekleme herhangi bir çakışma olmadan etkinleştirebilirsiniz.
>

Bir SQL veritabanı için korumayı yapılandırmak için:

1. SQL sanal makine ile kayıtlı kurtarma Hizmetleri kasasını açın.

2. Üzerinde **kurtarma Hizmetleri kasası** panoyu seçin **yedekleme**. **Yedekleme hedefi** menüsü açılır.

   ![Yedekleme yedekleme hedefi menüsünü açmak için seçin](./media/backup-azure-sql-database/open-backup-menu.png)

3. Üzerinde **yedekleme hedefi** menüsünde ayarlamak **iş yükünüz çalıştığı** varsayılan: **Azure**.

4. Genişletin **neleri yedeklemek istiyorsunuz** aşağı açılan liste kutusunu seçip alt **Azure VM'de SQL Server**.

    ![SQL Server Azure VM için yedeklemeyi seçin.](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

    **Yedekleme hedefi** menü iki adımı görüntüler: **VM'lerin içinde VT Bul** ve **yedeklemeyi yapılandırma**.

    Sırayla bu makaledeki adımları tamamladıysanız, korumasız sanal makineleri keşfettiniz ve sahip bir sanal makineyi bu kasaya kayıtlı. Şimdi, SQL veritabanları için koruma yapılandırılamadı hazırsınız demektir.

5. Üzerinde **yedekleme hedefi** menüsünde **yedeklemeyi Yapılandır**.

    ![Seçin yedeklemeyi yapılandırma](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

    Azure Backup hizmeti tek başına veritabanları ve SQL Server her zaman kullanılabilirlik grupları ile tüm SQL Server örneğini gösterir. Tek başına veritabanları SQL Server örneğinde görüntülemek için örnek adının solundaki köşeli çift ayracı seçin. Benzer şekilde, veritabanlarının listesini görüntülemek için her zaman üzerinde kullanılabilirlik grubu solundaki köşeli çift ayracı seçin. Aşağıdaki resimde, bir tek başına bir örneğini ve her zaman üzerinde kullanılabilirlik grubu örneğidir.

      ![Tek başına veritabanları ile tüm SQL Server örneklerini görüntüleme](./media/backup-azure-sql-database/list-of-sql-databases.png)

6. Veritabanları listesinde istediğiniz korumak ve tüm veritabanlarını seçin **Tamam**.

    ![Veritabanını koruma](./media/backup-azure-sql-database/select-database-to-protect.png)

    Aynı anda en çok 50 veritabanı seçebilirsiniz. 50'den fazla veritabanlarını korumak için çeşitli geçiş yapın. İlk 50 veritabanlarını koruduktan sonra sonraki kümesini veritabanlarını korumak için bu adımı yineleyin.

    > [!Note]
    > Yedekleme yüklerini en iyi duruma getirmek için Azure Backup büyük yedekleme işleri birden çok toplu iş ayırır. Bir yedekleme işi veritabanlarında sayısı 50'dir.
    >

      Alternatif olarak, etkinleştirebilirsiniz [otomatik korumayı](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm) tüm örneği veya her zaman üzerinde kullanılabilirlik grubu seçerek **ON** seçeneği karşılık gelen açılır penceresinde **AUTOPROTECT**  sütun. [Otomatik korumayı](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm) özellik yalnızca tek bir seferde tüm var olan veritabanları korumasını sağlar, ancak gelecekte bu örneği veya kullanılabilirlik grubuna eklenecek yeni veritabanlarını da otomatik olarak korur.  

      ![Always On kullanılabilirlik grubunda otomatik korumayı etkinleştir](./media/backup-azure-sql-database/enable-auto-protection.png)

7. Bir yedekleme ilkesi seçin veya **yedekleme** menüsünde **yedekleme İlkesi**. **Yedekleme İlkesi** menüsü açılır.

    ![Yedekleme ilkesini seçin](./media/backup-azure-sql-database/select-backup-policy.png)

8. İçinde **yedekleme ilkesi seçmek** aşağı açılan listesinde, bir yedekleme ilkesi seçip **Tamam**. Bir yedekleme ilkesi oluşturma hakkında daha fazla bilgi için bkz: [yedekleme ilkesi tanımlama](backup-azure-sql-database.md#define-a-backup-policy).

   > [!NOTE]
   > Önizleme süresince yedekleme ilkeleri düzenleyemezsiniz. Listede bulunandan başka bir ilke istiyorsanız, bu ilkeyi oluşturmanız gerekir. Bölümünde, yeni bir yedekleme ilkesi oluşturma hakkında daha fazla bilgi için bkz. [yedekleme ilkesi tanımlama](backup-azure-sql-database.md#define-a-backup-policy).

    ![Listeden bir yedekleme ilkesi seçin](./media/backup-azure-sql-database/select-backup-policy-steptwo.png)

    Üzerinde **yedekleme İlkesi** menü, **yedekleme ilkesi seçmek** aşağı açılan liste kutusunda, şunları yapabilirsiniz:
    - Varsayılan ilkeyi seçin: **HourlyLogBackup**.
    - SQL için daha önce oluşturulmuş mevcut bir yedekleme İlkesi'ni seçin.
    - [Yeni bir ilke tanımlama](backup-azure-sql-database.md#define-a-backup-policy) RPO ve bekletme aralığını esas alarak.

    > [!Note]
    > Azure Backup dedenizin bırak son yedekleme şemasını temel alır, uzun süreli saklama destekler. Toplantı uyumluluk gereksinimlerini arka uç depolama tüketimi düzenini iyileştirir.
    >

9. Üzerinde bir yedekleme İlkesi seçtikten sonra **yedekleme menüsündeki**seçin **yedeklemeyi etkinleştir**.

    ![Seçilen yedekleme İlkesi'ni etkinleştir](./media/backup-azure-sql-database/enable-backup-button.png)

    Yapılandırma ilerlemeyi **bildirimleri** portalının alan.

    ![Bildirim alanı](./media/backup-azure-sql-database/notifications-area.png)


## <a name="auto-protect-sql-server-in-azure-vm"></a>Azure VM'de SQL Server otomatik olarak koruma  

Otomatik koruma, mevcut tüm veritabanlarını ve gelecekte bir tek başına SQL Server örneği veya SQL Server her zaman kullanılabilirlik grubuna eklersiniz veritabanları otomatik olarak korumanıza olanak sağlar. Kapatma **ON** otomatik koruma ve yedekleme İlkesi'ni seçerek yeni korunan veritabanları için geçerli, var olan korumalı veritabanlarını önceki ilke kullanmaya devam eder.

![Always On kullanılabilirlik grubunda otomatik korumayı etkinleştir](./media/backup-azure-sql-database/enable-auto-protection.png)

Tek bir seferde seçiliyor veritabanları sayısına bir sınır yoktur kullanarak otomatik olarak korumak özelliği. Yapılandırma Yedekleme tüm veritabanları için birlikte tetiklenir ve izlenebilir **yedekleme işleri**.

Herhangi bir nedenden dolayı örneği otomatik koruması devre dışı bırakmanız gerekirse, örnek adı altında tıklayın **yedeklemeyi Yapılandır** olan sağ taraftaki Bilgi paneli **Autoprotect devre dışı** üzerinde Sayfanın Üstü. Tıklayın **devre dışı Autoprotect** örneğine otomatik korumayı devre dışı bırakmak için.

![Bu örneği otomatik korumasını devre dışı](./media/backup-azure-sql-database/disable-auto-protection.png)

Bu örnekteki tüm veritabanlarının korunmaya devam eder. Ancak, bu eylem, gelecekte eklenecek veritabanları üzerinde otomatik korumayı devre dışı bırakır.

### <a name="define-a-backup-policy"></a>Yedekleme ilkesi tanımlama

Bir yedekleme İlkesi, bir matris yedekleme zaman alınır ve ne kadar süreyle tutulur tanımlar. Üç tür SQL veritabanları için yedekleme zamanlamak için Azure Backup kullanın:

* Tam yedekleme: Tam veritabanı yedeği tüm veritabanını yedekler. Tam yedekleme, tüm verileri belirli bir veritabanında veya dosya grubu veya dosya ve verileri kurtarmak için yeterli günlükleri bir dizi içeriyor. En fazla günde bir tam yedekleme tetikleyin. Tam günlük veya haftalık bir aralıkta yedek almak seçebilirsiniz.
* Fark yedekleme: Değişiklik yedeği en son, önceki tam veri yedeği temel alır. Değişiklik yedeği, yalnızca tam yedeklemeden bu yana değişmiş olan verileri yakalar. En fazla günde bir fark yedekleme tetikleyebilirsiniz. Aynı gün tam yedekleme ve bir değişiklik yedeği yapılandıramazsınız.
* İşlem günlüğü yedeklemesi: Bir günlük yedeği, belirli bir saniye kadar zaman içinde nokta geri yükleme sağlar. En fazla 15 dakikada bir işlem günlüğü yedeklemeleri yapılandırabilirsiniz.

İlkenin kurtarma Hizmetleri kasası düzeyi oluşturdunuz. Birden çok kasa ve aynı yedekleme ilkesine kullanabilirsiniz, ancak her kasa için yedekleme ilkesini uygulama. Bir yedekleme ilkesi oluşturduğunuzda, günlük tam yedekleme varsayılandır. Tam yedekleme haftalık olarak gerçekleşecek şekilde yapılandırırsanız, ancak yalnızca bir değişiklik yedeği ekleyebilirsiniz. Aşağıdaki yordam bir Azure sanal makineler'de SQL Server örneği için bir yedekleme ilkesi oluşturma işlemini açıklar.

> [!NOTE]
> Önizleme aşamasında olan bir yedekleme İlkesi düzenleyemezsiniz. Bunun yerine, istenen ayrıntıları ile yeni bir ilke oluşturmanız gerekir.  

Bir yedekleme ilkesi oluşturmak için:

1. SQL veritabanı koruyan kurtarma Hizmetleri kasasında tıklayın **yedekleme ilkeleri**ve ardından **Ekle**.

   ![Oluştur Yeni yedekleme İlkesi iletişim kutusunu açın](./media/backup-azure-sql-database/new-policy-workflow.png)

   **Ekle** menü görünür.

2. İçinde **Ekle** menüsünü tıklatın **Azure VM'de SQL Server**.

   ![Yeni bir yedekleme ilkesi için bir ilke türü seçin](./media/backup-azure-sql-database/policy-type-details.png)

   Azure VM'de SQL Server'ı seçerek ilke türünü tanımlar ve yedekleme İlkesi menüsü açılır. **Yedekleme İlkesi** menüsünden Yeni bir SQL Server Yedekleme ilkesi için gerekli olan alanları gösterir.

3. İçinde **ilke adı**, yeni ilke için bir ad girin.

4. Tam yedekleme zorunludur; devre dışı bırakamazlar **tam yedekleme** seçeneği. Tıklayın **tam yedekleme** görüntüleme ve ilkeyi düzenleyin. Yedekleme İlkesi değişmez bile ilke ayrıntıları görüntülemeniz gerekir.

    ![Yeni bir yedekleme İlkesi alanlar](./media/backup-azure-sql-database/full-backup-policy.png)

    İçinde **tam yedekleme İlkesi** menüsünde için **yedekleme sıklığı**, seçin **günlük** veya **haftalık**. İçin **günlük**yedekleme işi başladığında saat dilimini ve saat seçin. Günlük tam yedekleme için fark yedeklemelerinin oluşturulamıyor.

   ![Günlük aralığını ayarlama](./media/backup-azure-sql-database/daily-interval.png)

    İçin **haftalık**, yedekleme işi başladığında günü, haftanın günü, saat ve saat dilimi seçin.

   ![Haftalık aralığını ayarlama](./media/backup-azure-sql-database/weekly-interval.png)

5. Varsayılan olarak, tüm **bekletme aralığı** seçeneklerinin belirlendiğini: günlük, haftalık, aylık ve yıllık. Bir istenmeyen bekletme aralığı sınırları seçimini kaldırın. Kullanılacak aralıkları ayarlayın. İçinde **tam yedekleme İlkesi** menüsünde **Tamam** ayarları kabul etmek için.

   ![Bekletme aralığı aralığı ayarları](./media/backup-azure-sql-database/retention-range-interval.png)

    Kurtarma noktalarının bekletme, bekletme aralığına göre etiketlenir. Örneğin, bir günlük tam yedekleme öğesini seçerseniz, yalnızca bir tam yedekleme her gün tetiklenir. Yedekleme haftalık bir bekletme aralığı ve haftalık bekletme ayarınızı bağlı olarak belirli bir günde etiketlenmiş ve korunur. Aylık ve yıllık bekletme aralıkları benzer şekilde davranır.

6. Fark yedekleme ilkesi eklemek için seçin **fark yedekleme**. **Fark yedekleme İlkesi** menüsü açılır.

   ![Fark yedekleme İlkesi menüsü açın](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

    Üzerinde **fark yedekleme İlkesi** menüsünde **etkinleştirmek** sıklığı ve bekletme denetimleri açın. En fazla günde bir fark yedekleme tetikleyebilirsiniz.

    > [!Important]
    > Değişiklik yedekleri, en fazla 180 gün için saklanabilir. Daha uzun bekletme süresi gerekiyorsa, tam yedeklemeler kullanmanız gerekir.
    >

   ![Fark yedekleme ilkesini Düzenle](./media/backup-azure-sql-database/enable-differential-backup-policy.png)

    Seçin **Tamam** ilkeyi kaydedin ve ana menüye dön **yedekleme İlkesi** menüsü.

7. İşlem günlüğü yedekleme ilkesi eklemek için seçin **günlük yedekleme**. **Günlük yedeği** menüsü açılır.

    İçinde **günlük yedeği** menüsünde **etkinleştirme**ve ardından sıklığı ve bekletme denetimleri ayarlayın. Günlük yedeklemeleri 15 dakikada bir sıklıkta meydana gelebilir ve 35 güne kadar saklanabilir. Seçin **Tamam** ilkeyi kaydedin ve ana menüye dön **yedekleme İlkesi** menüsü.

   ![Günlük yedekleme ilkesini Düzenle](./media/backup-azure-sql-database/log-backup-policy-editor.png)

8. Üzerinde **yedekleme İlkesi** menüsünde etkinleştirilip etkinleştirilmeyeceğini seçin **SQL yedekleme sıkıştırması**. Sıkıştırma, varsayılan olarak devre dışıdır.

    Arka uçta Azure Backup SQL yerel yedekleme sıkıştırmasını kullanır.

9. Yedekleme İlkesi düzenlemeleri tamamladığınızda seçin **Tamam**.

   ![Yeni yedekleme ilkesini kabul edin](./media/backup-azure-sql-database/backup-policy-click-ok.png)

## <a name="restore-a-sql-database"></a>Bir SQL veritabanını geri yükleme
Azure Backup, tek veritabanlarına belirli bir tarih veya saat (saniye için) işlem günlüğü yedeklemeleri kullanarak geri yüklemek için işlevsellik sağlar. Azure yedekleme, otomatik olarak uygun tam fark ve, geri yükleme zamanına göre verilerinizi geri yüklemek için gereken günlük yedekleme zincirini belirler.

Belirli bir zaman yerine belirli bir kurtarma noktasını geri yüklemek için belirli bir tam veya değişiklik yedeklemesinden de seçebilirsiniz.

### <a name="pre-requisite-before-triggering-a-restore"></a>Bir geri yüklemeyi tetikleme önce önkoşul

1. Veritabanı, aynı Azure bölgesindeki bir SQL Server'ın bir örneğine geri yükleyebilirsiniz. Hedef sunucuda kaynak olarak aynı kurtarma Hizmetleri kasasına kayıtlı olması gerekir.  
2. TDE şifrelenmiş veritabanı başka bir SQL Server'a geri yüklemek için lütfen önce sertifikanın hedef sunucuya belgelenen aşağıdaki adımları geri [burada](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017).
3. "Ana" veritabanı geri yükleme tetiklemeden önce SQL Server örneği tek kullanıcı modunda başlatma seçeneği ile başlatın `-m AzureWorkloadBackup`. Bağımsız değişkeni `-m` seçeneği, istemci adıdır. Yalnızca bu istemci bağlantıyı açık izin verilmez. Geri yüklemeyi tetikleyecek önce (modeli, master, msdb), tüm sistem veritabanları için SQL Aracı hizmeti durdurun. Bu veritabanlarının herhangi bir bağlantı çalabilir deneyebilir tüm uygulamaları kapatın.

### <a name="steps-to-restore-a-database"></a>Bir veritabanını geri yüklemek için adımları:

1. SQL sanal makine ile kayıtlı kurtarma Hizmetleri kasasını açın.

2. Üzerinde **kurtarma Hizmetleri kasası** Pano altındaki **kullanım**seçin **yedekleme öğeleri** açmak için **yedekleme öğeleri** menüsü.

    ![Yedekleme öğeleri menüsünü açın](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. Üzerinde **yedekleme öğeleri** menüsü altında **Yedekleme Yönetimi türü**seçin **Azure VM'deki SQL**.

    ![Azure VM'de SQL seçin](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    **Yedekleme öğeleri** menüsü SQL veritabanlarının listesini gösterir.

4. SQL veritabanları listesinde veritabanını geri yüklemek için seçin.

    ![Geri yüklenecek veritabanını seçin](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Veritabanına seçtiğinizde, alt menüsü açılır. Menü veritabanı için yedekleme ayrıntıları sağlar. dahil olmak üzere:

    * Eski ve en son geri yükleme noktaları.
    * Son 24 saat (işlem günlüğü yedeklemeleri için yapılandırılmışsa tam ve toplu günlük kurtarma modelinde veritabanları için) günlük yedekleme durumu.

5. Seçili veritabanı için menüden **geri DB**. **Geri** menüsü açılır.

    ![Geri yükleme Veritabanı Seç](./media/backup-azure-sql-database/restore-db-button.png)

    Zaman **geri** menüsü açılır, **geri yükleme Yapılandırması** ayrıca menüsü açılır. **Geri yükleme Yapılandırması** menüsü geri yüklemeyi yapılandırmak için ilk adımı olur. Verileri geri yükleneceği yeri seçmek için bu menüyü kullanın. Seçenekler şunlardır:
    - **Alternatif konuma**: Veritabanını alternatif bir konuma geri yükleyin ve özgün kaynak veritabanını korur.
    - **Üzerine yaz**: Verilerin, özgün kaynak aynı SQL Server örneğine geri yükleyin. Bu seçenek, özgün veritabanının üzerine yazmak için etkisidir.

    > [!Important]
    > Seçilen veritabanı bir her zaman üzerinde kullanılabilirlik grubuna dahilse, SQL Server veritabanı üzerine yazılmasına izin vermez. Bu durumda, yalnızca **alternatif bir konuma** seçeneği etkinleştirilir.
    >

    ![Yapılandırma menü geri yükleme](./media/backup-azure-sql-database/restore-restore-configuration-menu.png)

### <a name="restore-to-an-alternate-location"></a>Alternatif bir konuma geri yükleme

Bu yordamı, verileri alternatif bir konuma geri yükleme yoluyla açıklanmaktadır. Veritabanını geri yükleme sırasında üzerine yazmak için devam [geri yükleme ve veritabanının üzerine yaz](backup-azure-sql-database.md#restore-and-overwrite-the-database). Bu aşamada, Kurtarma Hizmetleri kasanıza açıktır ve **geri yükleme Yapılandırması** menü görünür. Bu aşamada değilseniz başlayın [bir SQL veritabanını geri yükleme](backup-azure-sql-database.md#restore-a-sql-database).

> [!NOTE]
> Veritabanı, aynı Azure bölgesindeki bir SQL Server'ın bir örneğine geri yükleyebilirsiniz. Hedef sunucuyu kurtarma Hizmetleri kasasına kayıtlı olması gerekir.
>

Üzerinde **geri yükleme Yapılandırması** menüsünde **sunucu** aşağı açılan liste kutusunu yalnızca kurtarma Hizmetleri kasasına kayıtlı SQL Server örnekleri gösterilir. İstediğiniz sunucu listede yoksa bkz [Bul SQL Server veritabanlarını](backup-azure-sql-database.md#discover-sql-server-databases) sunucusunu bulmak için. Bulma işlemi sırasında kurtarma Hizmetleri Kasası'na yeni sunucular kaydedilir.

1. İçinde **geri yükleme Yapılandırması** menüsü:

    * Altında **nereye Geri Yüklensin**seçin **alternatif bir konuma**.
    * Açık **sunucu** aşağı açılan liste kutusunda ve veritabanını geri yüklemek için SQL Server örneği seçin.
    * Açık **örneği** aşağı açılan liste kutusunda ve SQL Server örneği seçin.
    * İçinde **geri DB adı** kutusunda, hedef veritabanı adı girin.
    * Uygun seçeneğini **seçili SQL örneğinde aynı ada sahip bir veritabanı zaten varsa üzerine yaz**.
    * Seçin **Tamam** hedef yapılandırmasını tamamlayın ve geri yükleme noktası seçmek devam etmek için.

    ![Geri Yükleme Yapılandırması menüsünü değerleri sağlayın](./media/backup-azure-sql-database/restore-configuration-menu.png)

2. Üzerinde **seçin, geri yükleme noktası** menüsünde seçin **günlükleri (zaman içinde nokta)** veya **tam ve değişiklik** geri yükleme noktası olarak. Belirli bir zaman içinde nokta günlüğüne geri yüklemek için bu adım ile devam edin. Tam ve fark geri yükleme noktası geri yüklemek için 3. adımına devam edin.

    ![geri yükleme noktası menüsünü seçin](./media/backup-azure-sql-database/recovery-point-menu.png)

    Belirli bir noktaya geri yükleme, yalnızca tam ve toplu günlük kurtarma modeli veritabanları için günlük yedeklemeler için kullanılabilir. Belirli bir noktaya geri yükleme için:

    1. Seçin **günlükleri (zaman içinde nokta)** geri yükleme türü.

        ![Geri yükleme noktası türünü seçin](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. Altında **geri tarih/saat**, açmak için mini takvim seçin **Takvim**. Üzerinde **Takvim**Kalın tarihleri kurtarma noktalarına sahip ve geçerli tarih vurgulanır. Kurtarma noktaları olan bir tarih seçin. Kurtarma noktaları olmadan tarih seçilemez.

        ![Takvimi Aç](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Bir tarih seçtikten sonra zaman çizelgesi grafiği sürekli bir aralıktaki kullanılabilir kurtarma noktalarını görüntüler.

    3. Zaman Çizelgesi grafiği kullanın veya **zaman** iletişim kutusunda, kurtarma noktası için belirli bir zaman belirtmek için. Seçin **Tamam** geri yükleme noktası adımı tamamlamak için.

       ![Takvimi Aç](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        **Seçin, geri yükleme noktası** menü kapanır ve **Gelişmiş Yapılandırma** menüsü açılır.

       ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. Üzerinde **Gelişmiş Yapılandırma** menüsü:

        * Veritabanını geri yüklemeden sonra çalışmaz durumda tutmak için ayarlanmış **kurtarma olmadan geri yükle** için **etkin**.
        * Hedef sunucuda geri yükleme konumunu değiştirmek için yeni bir yol girin **hedef** sütun.
        * Seçin **Tamam** ayarları onaylamak için. Kapat **Gelişmiş Yapılandırma** menüsü.

    5. Üzerinde **geri** menüsünde **geri** geri yükleme işi başlatılamadı. Geri Yükleme ilerlemesini **bildirimleri** seçin veya alan **geri yükleme işleri** veritabanı menüsünde.

       ![Geri yükleme işi ilerleme durumu](./media/backup-azure-sql-database/restore-job-notification.png)

3. Üzerinde **seçin, geri yükleme noktası** menüsünde seçin **günlükleri (zaman içinde nokta)** veya **tam ve değişiklik** geri yükleme noktası olarak. Zaman içinde nokta günlük geri yüklemek için 2. adıma geri dönün. Bu adım, belirli tam ya da fark geri yükleme noktası geri yükler. Son 30 güne ait tüm tam ve farklı bir kurtarma noktası görebilirsiniz. 30 günden eski kurtarma noktalarını görmek için seçin **filtre** açmak için **filtre geri yükleme noktaları** menüsü. Azure Backup, bir değişiklik kurtarma noktası için önce uygun bir tam kurtarma noktasını geri yükler ve ardından seçilen değişiklik kurtarma noktası uygular.

    ![geri yükleme noktası menüsünü seçin](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. İçinde **seçin, geri yükleme noktası** menüsü, select **tam ve değişiklik**.

       ![Tam ve fark seçin](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        Menü kullanılabilir kurtarma noktalarının listesini gösterir.

    2. Bir kurtarma noktası listeden seçip **Tamam** geri yükleme noktası yordamı tamamlamak için.

        ![Tam kurtarma noktası seçin](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        **Geri yükleme noktası** menü kapanır ve **Gelişmiş Yapılandırma** menüsü açılır.

        ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. Üzerinde **Gelişmiş Yapılandırma** menüsü:

        * Veritabanını geri yüklemeden sonra çalışmaz durumda tutmak için ayarlanmış **kurtarma olmadan geri yükle** için **etkin**. **Kurtarma olmadan geri yükle** varsayılan olarak devre dışıdır.
        * Hedef sunucuda geri yükleme konumunu değiştirmek için yeni bir yol girin **hedef** sütun.
        * Seçin **Tamam** ayarları onaylamak için. Kapat **Gelişmiş Yapılandırma** menüsü.

    4. Üzerinde **geri** menüsünde **geri** geri yükleme işi başlatılamadı. Geri Yükleme ilerlemesini **bildirimleri** seçin veya alan **geri yükleme işleri** veritabanı menüsünde.

       ![Geri yükleme işi ilerleme durumu](./media/backup-azure-sql-database/restore-job-notification.png)

### <a name="restore-and-overwrite-the-database"></a>Geri yükleme ve veritabanının üzerine yaz

Bu yordamı verilerin geri yüklenmesi ve bir veritabanının üzerine size kılavuzluk eder. Alternatif bir konuma geri yüklemek için devam [alternatif bir konuma geri](backup-azure-sql-database.md#restore-to-an-alternate-location). Bu aşamada, Kurtarma Hizmetleri kasanıza açıktır ve **geri yükleme Yapılandırması** menüsü görünür (aşağıda görebilirsiniz) olur. Bu aşamada değilseniz başlayın [bir SQL veritabanını geri yükleme](backup-azure-sql-database.md#restore-a-sql-database).

![Yapılandırma menü geri yükleme](./media/backup-azure-sql-database/restore-overwrite-db.png)

Üzerinde **geri yükleme Yapılandırması** menüsünde **sunucu** aşağı açılan liste kutusunu yalnızca kurtarma Hizmetleri kasasına kayıtlı SQL Server örnekleri gösterilir. İstediğiniz sunucu listede yoksa bkz [Bul SQL Server veritabanlarını](backup-azure-sql-database.md#discover-sql-server-databases) sunucusunu bulmak için. Bulma işlemi sırasında kurtarma Hizmetleri Kasası'na yeni sunucular kaydedilir.

1. İçinde **geri yükleme Yapılandırması** menüsünde **üzerine DB**ve ardından **Tamam** hedef yapılandırmasını tamamlamak için.

   ![DB üzerine yazmayı seçin](./media/backup-azure-sql-database/restore-configuration-overwrite-db.png)

    **Sunucu**, **örneği**, ve **geri DB adı** ayarları gerekli değil.

2. Üzerinde **seçin, geri yükleme noktası** menüsünde seçin **günlükleri (zaman içinde nokta)** veya **tam ve değişiklik** geri yükleme noktası olarak. Belirli bir zaman içinde nokta günlüğüne geri yüklemek için bu adım ile devam edin. Tam ve fark geri yükleme noktası geri yüklemek için 3. adımına devam edin.

    ![geri yükleme noktası menüsünü seçin](./media/backup-azure-sql-database/recovery-point-menu.png)

    Belirli bir noktaya geri yükleme, yalnızca tam ve toplu günlük kurtarma modeli veritabanları için günlük yedeklemeler için kullanılabilir. Belirli bir saniye olarak geri yüklemek için:

    1. Seçin **günlükleri (zaman içinde nokta)** geri yükleme noktası olarak.

        ![Geri yükleme noktası türünü seçin](./media/backup-azure-sql-database/recovery-point-logs.png)

    2. Altında **geri tarih/saat**, açmak için mini takvim seçin **Takvim**. Üzerinde **Takvim**Kalın tarihleri kurtarma noktalarına sahip ve geçerli tarih vurgulanır. Kurtarma noktaları olan bir tarih seçin. Kurtarma noktaları olmadan tarih seçilemez.

        ![Takvimi Aç](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

        Bir tarih seçtikten sonra zaman çizelgesi grafiği kullanılabilir kurtarma noktalarını görüntüler.

    3. Zaman Çizelgesi grafiği kullanın veya **zaman** iletişim kutusunda, kurtarma noktası için belirli bir zaman belirtmek için. Seçin **Tamam** geri yükleme noktası adımı tamamlamak için.

       ![Takvimi Aç](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

        **Seçin, geri yükleme noktası** menü kapanır ve **Gelişmiş Yapılandırma** menüsü açılır.

       ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    4. Üzerinde **Gelişmiş Yapılandırma** menüsü:

        * Veritabanını geri yüklemeden sonra çalışmaz durumda tutmak için ayarlanmış **kurtarma olmadan geri yükle** için **etkin**.
        * Hedef sunucuda geri yükleme konumunu değiştirmek için yeni bir yol girin **hedef** sütun.
        * Seçin **Tamam** ayarları onaylamak için. Kapat **Gelişmiş Yapılandırma** menüsü.

    5. Üzerinde **geri** menüsünde **geri** geri yükleme işi başlatılamadı. Geri Yükleme ilerlemesini **bildirimleri** seçin veya alan **geri yükleme işleri** veritabanı menüsünde.

       ![Geri yükleme işi ilerleme durumu](./media/backup-azure-sql-database/restore-job-notification.png)

3. Üzerinde **seçin, geri yükleme noktası** menüsünde seçin **günlükleri (zaman içinde nokta)** veya **tam ve değişiklik** geri yükleme noktası olarak. Zaman içinde nokta günlük geri yüklemek için 2. adıma geri dönün. Bu adım, belirli tam ya da fark geri yükleme noktası geri yükler. Son 30 güne ait tüm tam ve farklı bir kurtarma noktası görebilirsiniz. 30 günden eski kurtarma noktalarını görmek için seçin **filtre** açmak için **filtre geri yükleme noktaları** menüsü. Azure Backup, bir değişiklik kurtarma noktası için önce uygun bir tam kurtarma noktasını geri yükler ve ardından seçilen değişiklik kurtarma noktası uygular.

    ![geri yükleme noktası menüsünü seçin](./media/backup-azure-sql-database/recovery-point-menu.png)

    1. İçinde **seçin, geri yükleme noktası** menüsü, select **tam ve değişiklik**.

       ![Tam ve fark seçin](./media/backup-azure-sql-database/recovery-point-logs-fd.png)

        Menü kullanılabilir kurtarma noktalarının listesini gösterir.

    2. Bir kurtarma noktası listeden seçip **Tamam** geri yükleme noktası yordamı tamamlamak için.

        ![Tam kurtarma noktası seçin](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

        **Geri yükleme noktası** menü kapanır ve **Gelişmiş Yapılandırma** menüsü açılır.

        ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

    3. Üzerinde **Gelişmiş Yapılandırma** menüsü:

        * Veritabanını geri yüklemeden sonra çalışmaz durumda tutmak için ayarlanmış **kurtarma olmadan geri yükle** için **etkin**. **Kurtarma olmadan geri yükle** varsayılan olarak devre dışıdır.
        * Hedef sunucuda geri yükleme konumunu değiştirmek için yeni bir yol girin **hedef** sütun.
        * Seçin **Tamam** ayarları onaylamak için. Kapat **Gelişmiş Yapılandırma** menüsü.

    4. Üzerinde **geri** menüsünde **geri** geri yükleme işi başlatılamadı. Geri Yükleme ilerlemesini **bildirimleri** alan seçerek veya **geri yükleme işleri** veritabanı menüsünde.

       ![Geri yükleme işi ilerleme durumu](./media/backup-azure-sql-database/restore-job-notification.png)

## <a name="manage-azure-backup-operations-for-sql-on-azure-vms"></a>Azure vm'lerde SQL Azure yedekleme işlemlerini yönetme

Bu bölümde, Azure sanal makinelerinde SQL için kullanılabilen çeşitli Azure yedekleme yönetim işlemleri hakkında bilgi sağlar. Aşağıdaki üst düzey işlemleri mevcuttur:

* İşleri izleme
* Yedekleme uyarıları
* Bir SQL veritabanı'nda korumayı Durdur
* SQL veritabanı korumasını sürdürme
* Geçici yedekleme işi tetikleme
* SQL Server çalıştıran bir sunucunun kaydını Kaldır

### <a name="monitor-backup-jobs"></a>Yedekleme işleri İzle
Azure yedekleme Gelişmiş yedekleme uyarıları ve hataları için bildirimler sağlayan kurumsal sınıf çözümüdür. (Bkz [yedekleme uyarıları görüntüleyip](backup-azure-sql-database.md#view-backup-alerts).) Belirli işleri izlemek için gereksinimlerinize göre aşağıdaki seçeneklerden birini kullanın.

#### <a name="use-the-azure-portal-for-adhoc-operations"></a>Geçici işlemleri için Azure portalını kullanma
Azure yedekleme gösterir tüm el ile tetiklenen veya geçici, işleri de **yedekleme işleri** portalı. Kullanılabilen işleri **yedekleme işleri** portalı içerir:
- Tüm yedekleme işlemleri yapılandırın.
- El ile yedekleme işlemleri tetiklenir.
- Geri yükleme işlemleri.
- Kayıt ve veritabanı işlemleri keşfedin.
- Yedekleme işlemleri durdurun.

![Yedekleme işleri portalı](./media/backup-azure-sql-database/jobs-list.png)

> [!NOTE]
> Tam, değişiklik ve günlük yedeklemesi dahil olmak üzere tüm zamanlanmış yedekleme işleri gösterilen olmayan **yedekleme işleri** portalı. Sonraki bölümde açıklandığı gibi zamanlanmış yedekleme işleri izlemek için SQL Server Management Studio'yu kullanın.
>

#### <a name="use-sql-server-management-studio-for-backup-jobs"></a>Yedekleme işleri için SQL Server Management Studio kullanma
Azure Backup, SQL yerel API'lerin tüm yedekleme işlemleri için kullanır. Tüm iş bilgileri getirmek için yerel API kullanan [SQL yedek kümesi tablo](https://docs.microsoft.com/sql/relational-databases/system-tables/backupset-transact-sql?view=sql-server-2017) msdb veritabanında.

Aşağıdaki örnekte adlı bir veritabanı için tüm yedekleme işleri getiren bir sorgudur **DB1**. Gelişmiş izleme sorgu özelleştirin.

```
select CAST (
Case type
                when 'D' 
                                 then 'Full'
                when  'I'
                               then 'Differential' 
                ELSE 'Log'
                END         
                AS varchar ) AS 'BackupType',
database_name, 
server_name,
machine_name,
backup_start_date,
backup_finish_date,
DATEDIFF(SECOND, backup_start_date, backup_finish_date) AS TimeTakenByBackupInSeconds,
backup_size AS BackupSizeInBytes
  from msdb.dbo.backupset where user_name = 'NT SERVICE\AzureWLBackupPluginSvc' AND database_name =  <DB1>  

```

### <a name="view-backup-alerts"></a>Yedekleme Uyarıları görüntüle

Günlük yedeklemelerin yedekleme işlerini izleme eşitleyin, bazen can sıkıcı olabilir. Azure Backup, bu durumda Yardım sağlar. Tüm yedekleme hataları için e-posta uyarıları tetiklenir. Uyarılar veritabanı düzeyinde hata koduna göre birleştirilir. Yalnızca ilk yedekleme hatası için bir veritabanı için bir e-posta uyarı gönderilir. Bir veritabanı için tüm hataları izlemek için Azure portalında oturum açın.

Yedekleme uyarıları izlemek için:

1. Azure aboneliğinizde oturum açın [Azure portalında](https://portal.azure.com).

2. SQL sanal makine ile kayıtlı kurtarma Hizmetleri kasasını açın.

3. Üzerinde **kurtarma Hizmetleri kasası** panoyu seçin **uyarıları ve olayları**.

   ![Uyarıları ve olayları seçin](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

4. Üzerinde **uyarıları ve olayları** menüsünde **yedekleme uyarıları** uyarıların listesini görüntülemek için.

   ![Yedekleme uyarıları seçin](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

### <a name="stop-protection-for-a-sql-server-database"></a>SQL Server veritabanı için korumayı Durdur

SQL Server veritabanı için korumayı durdurduğunuzda, Azure Backup kurtarma noktalarını tutmak isteyip istemediğinizi ister. SQL veritabanı için korumayı durdurmanın iki yolu vardır:

* Gelecek tarihli tüm yedekleme işlerini durdurma ve tüm kurtarma noktalarını silin.
* Gelecek tarihli tüm yedekleme işlerini durdurma ama kurtarma noktalarını bırakın.

Yedeklemeyi Durdur seçeneğini belirlerseniz verileri tut ile kurtarma noktaları yedekleme İlkesi uyarınca silinecektir. SQL korunan örnek ücreti yanı sıra, tüm kurtarma noktalarını temizlenir kadar tüketilen depolama fiyatlandırması uygulanır. SQL Azure Backup fiyatlandırması hakkında daha fazla bilgi için bkz. [Azure Backup fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/backup/).

Bir veritabanı için korumayı durdurmak için:

1. SQL sanal makine ile kayıtlı kurtarma Hizmetleri kasasını açın.

2. Üzerinde **kurtarma Hizmetleri kasası** Pano altındaki **kullanım**seçin **yedekleme öğeleri** açmak için **yedekleme öğeleri** menüsü.

    ![Yedekleme öğeleri menüsünü açın](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. Üzerinde **yedekleme öğeleri** menüsü altında **Yedekleme Yönetimi türü**seçin **Azure VM'deki SQL**.

    ![Azure VM'de SQL seçin](./media/backup-azure-sql-database/sql-restore-backup-items.png)

    **Yedekleme öğeleri** menüsü SQL veritabanlarının listesini gösterir.

4. SQL veritabanları listesinde korumasını durdurmak için veritabanını seçin.

    ![Korumayı durdurmak için veritabanını seçin](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

    Veritabanına seçtiğinizde, alt menüsü açılır.

5. Seçili veritabanı için menüden **yedeklemeyi Durdur**.

    ![Yedeklemeyi Durdur seçin](./media/backup-azure-sql-database/stop-db-button.png)

    **Yedeklemeyi Durdur** menüsü açılır.

6. Üzerinde **yedeklemeyi Durdur** menüsünde tercih **yedekleme verilerini koru** veya **yedekleme verilerini Sil**. Bir seçenek olarak, korumayı ve bir açıklama durdurmak için bir neden belirtin.

    ![Yedekleme menüsündeki Durdur](./media/backup-azure-sql-database/stop-backup-button.png)

7. Seçin **yedeklemeyi Durdur** veritabanı korumasını durdurmak için.

  Lütfen unutmayın **yedeklemeyi Durdur** seçeneği, bir veritabanı için çalışmaz bir [otomatik korumayı](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm) örneği. Devre dışı bırakmak için bu veritabanını korumayı durdurmanın tek yolu olduğundan [otomatik korumayı](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm) şimdilik örneğinde seçip **yedeklemeyi Durdur** altındaki **yedekleme öğeleri**bu veritabanı için.<br>
  Otomatik korumayı devre dışı bıraktıktan sonra şunları yapabilirsiniz **yedeklemeyi Durdur** veritabanı altında **yedekleme öğeleri**. Örneği yeniden için otomatik korumayı artık etkinleştirilebilir.


### <a name="resume-protection-for-a-sql-database"></a>SQL veritabanı korumasını sürdürme

Varsa **yedekleme verilerini koru** seçeneği seçildiğinde SQL veritabanı için korumayı durdurduğunuzda, koruma devam edebilir. Koruma, yedekleme verileri korunur değildi, sürdüremezsiniz.

1. SQL veritabanı için korumayı sürdürmek için yedekleme öğesi açın ve seçin **yedeklemeyi Sürdür**.

    ![Veritabanı korumayı sürdürmek için yedeklemeyi Sürdür eylemi seçin](./media/backup-azure-sql-database/resume-backup-button.png)

   **Yedekleme İlkesi** menüsü açılır.

2. Üzerinde **yedekleme İlkesi** menüsünde, bir ilkeyi seçin ve ardından **Kaydet**.

### <a name="trigger-an-adhoc-backup"></a>Geçici bir yedeklemeyi tetikleyin

Gerektiği gibi geçici yedeklemeler tetikler. Geçici yedekleme dört türleri şunlardır:

* Tam yedekleme
* Yalnızca kopya tam yedekleme
* Değişiklik yedeği
* Günlük yedekleme

Her tür hakkında daha fazla bilgi için bkz: [türleri, SQL yedeklerini](https://docs.microsoft.com/sql/relational-databases/backup-restore/backup-overview-sql-server?view=sql-server-2017#types-of-backups).

### <a name="unregister-a-sql-server-instance"></a>SQL Server örneği kaydı

Koruma kaldırdıktan sonra ancak kasayı silmeden önce bir SQL Server örneği kaydı için:

1. SQL sanal makine ile kayıtlı kurtarma Hizmetleri kasasını açın.

2. Üzerinde **kurtarma Hizmetleri kasası** Pano altındaki **Yönet**seçin **Yedekleme Altyapısı**.  

   ![Yedekleme Altyapısı'nı seçin](./media/backup-azure-sql-database/backup-infrastructure-button.png)

3. Altında **yönetim sunucuları**seçin **korumalı sunucuların**.

   ![Korumalı sunucuları seçin](./media/backup-azure-sql-database/protected-servers.png)

    **Korumalı sunucuların** menüsü açılır.

4. Üzerinde **korumalı sunucuların** menüsünde kaydını kaldırmak için sunucuyu seçin. Kasayı silmek için tüm sunucuların kaydını silmeniz gerekir.

5. Üzerinde **korumalı sunucuların** menüsünde, korumalı sunucuya sağ tıklayın ve seçin **Sil**.

   ![Sil'i seçin](./media/backup-azure-sql-database/delete-protected-server.png)

## <a name="faq"></a>SSS

Aşağıdaki bölümde, SQL veritabanı yedeklemesi hakkında ek bilgi sağlar.

### <a name="can-i-throttle-the-speed-of-the-sql-server-backup-policy"></a>Ben, SQL Server Yedekleme İlkesi hızına kısıtlayabilir miyim?

Evet. Bir SQL Server örneği üzerindeki etkiyi en aza indirmek için yedekleme ilkesini yürütür oranı kısıtlayabilirsiniz.
Bu ayarı değiştirmek için:
1. SQL Server örneğinde, *C:\Program Files\Azure iş yükü Backup\bin klasör*, oluşturma **ExtensionSettingsOverrides.json** dosya.
2. İçinde **ExtensionSettingsOverrides.json** dosya, değişiklik **DefaultBackupTasksThreshold** ayarını daha düşük bir değere (örneğin, 5) <br>
  ` {"DefaultBackupTasksThreshold": 5}`

3. Yaptığınız değişiklikleri kaydedin. Dosyayı kapatın.
4. SQL Server örneğinde açın **Görev Yöneticisi'ni**. Yeniden **AzureWLBackupCoordinatorSvc** hizmeti.

### <a name="can-i-run-a-full-backup-from-a-secondary-replica"></a>Bir ikincil çoğaltma tam yedekleme çalıştırabilir miyim?
Hayır. Bu özellik desteklenmez.

### <a name="do-successful-backup-jobs-create-alerts"></a>Başarılı yedekleme işleri uyarılar oluşturulur?

Hayır. Başarılı yedekleme işleri uyarıları oluşturma. Uyarılar, başarısız olan yedekleme işleri için gönderilir.

### <a name="can-i-see-details-for-scheduled-backup-jobs-in-the-jobs-menu"></a>İşleri menüsünde zamanlanan yedekleme işlerinin ayrıntılarını görebilir miyim?

Hayır. **Yedekleme işleri** geçici iş ayrıntıları, ancak değil zamanlanan yedekleme işlerinin menüsü gösterir. Zamanlanmış tüm yedekleme işleri başarısız olursa, başarısız işi uyarıları ayrıntıları kullanılabilir. Tüm izlemek için zamanlanan ve geçici yedekleme işlerini kullanma [SQL Server Management Studio](backup-azure-sql-database.md#use-sql-server-management-studio-for-backup-jobs).

### <a name="when-i-select-a-sql-server-instance-are-future-databases-automatically-added"></a>Bir SQL Server örneği seçtiğinizde otomatik olarak eklenen gelecekteki veritabanları misiniz?

Hayır. Sunucu düzeyi seçeneği seçerseniz bir SQL Server örneği için korumayı yapılandırdığınızda, tüm veritabanları eklenir. Bir SQL Server örneğine veritabanları eklerseniz, koruma yapılandırdıktan sonra onları korumak için yeni veritabanlarını elle eklemeniz gerekir. Veritabanlarını yapılandırılmış korumayı otomatik olarak dahil edilmez.

### <a name="if-i-change-the-recovery-model-how-do-i-restart-protection"></a>Kurtarma modeli değiştirirsem nasıl koruma yeniden?

Tam yedekleme tetikleyin. Günlük yedeklemeler beklendiği gibi başlayın.

### <a name="can-i-protect-sql-always-on-availability-groups-where-the-primary-replica-is-on-premises"></a>SQL Always On kullanılabilirlik birincil çoğaltmaya şirket içi olduğu grupları koruyabilirim?

Hayır. Azure Backup, Azure'da çalışan SQL sunucuları korur. Bir kullanılabilirlik grubu (ağ), Azure ve şirket içi makineler arasında yayılır, yalnızca birincil çoğaltma Azure'da çalışıyorsa AG korunabilir. Ayrıca, Azure Backup yalnızca kurtarma Hizmetleri kasasıyla aynı Azure bölgesinde çalışan düğümleri korur.

### <a name="can-i-protect-sql-always-on-availability-groups-which-are-spread-across-azure-regions"></a>SQL Always On kullanılabilirlik hangi Azure bölgeleri arasında yayılır grupları koruyabilirim?

Azure Backup kurtarma Hizmetleri kasası, algılayın ve kurtarma Hizmetleri kasasıyla aynı bölgede olan tüm düğümleri koruyun. Birden fazla Azure bölgesini kapsayan bir SQL her zaman üzerinde kullanılabilirlik grubu varsa, birincil düğüm olan bölgeden yedeklemeyi yapılandırmak gerekir. Azure yedekleme, algılamak ve yedekleme tercihi göre kullanılabilirlik grubundaki tüm veritabanlarını korumak mümkün olacaktır. Yedekleme tercihi karşılanmazsa, yedeklemeler başarısız olur ve hata uyarısı alırsınız.

### <a name="while-i-want-to-protect-most-of-the-databases-in-an-instance-i-would-like-to-exclude-a-few-is-it-possible-to-still-use-the-auto-protection-feature"></a>Çoğu bir örneğindeki veritabanlarını, korumak istediğiniz, ancak birkaç dışlanacak istiyorum. Otomatik koruma özelliğini kullanmaya devam mümkündür?

Hayır, [otomatik korumayı](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm) tüm örneğine uygular. Seçime bağlı olarak, otomatik koruma kullanarak bir örnek veritabanlarını koruyamaz.

### <a name="can-i-have-different-policies-for-different-databases-in-an-auto-protected-instance"></a>Bir otomatik korumalı örnekte farklı veritabanları için farklı ilkeler olabilir mi?

Korumalı bazı veritabanları bir örneğine zaten varsa bunlar da açıldıktan sonra ilgili ilkelerini ile korunacak devam eder **ON** [otomatik korumayı](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm) seçeneği. Ancak gelecekte eklersiniz olanları yanı sıra tüm korumasız veritabanlarını altında tanımladığınız yalnızca tek bir ilke olacaktır **yedeklemeyi Yapılandır** veritabanlarını seçtikten sonra. Aslında, korunan diğer veritabanlarının, ilke için bir veritabanı örneği otomatik korumalı altında bile değiştiremezsiniz.
Bunu yapmak istiyorsanız, tek şimdilik örneği otomatik korumasını devre dışı ve ardından bu veritabanı için ilkeyi değiştirmek için yoludur. Bu örnek için otomatik korumayı yeniden etkinleştirebilirsiniz.

### <a name="if-i-delete-a-database-from-an-auto-protected-instance-will-the-backups-for-that-database-also-stop"></a>Bir veritabanı otomatik korumalı örneği silerseniz, bu veritabanı için yedeklemeler de durdurur mu?

Hayır, bir veritabanı otomatik korumalı örneği kesilirse, veritabanı yedekleri hala denenir. Bu, silinen veritabanını altında sağlıksız görünmesini başlar gelir **yedekleme öğeleri** ve hala korumalı olarak kabul edilir.

Devre dışı bırakmak için bu veritabanını korumayı durdurmanın tek yolu olduğundan [otomatik korumayı](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm) şimdilik örneğinde seçip **yedeklemeyi Durdur** altındaki **yedekleme öğeleri**bu veritabanı için. Bu örnek için otomatik korumayı yeniden etkinleştirebilirsiniz.

###  <a name="why-cant-i-see-the-newly-added-database-to-an-auto-protected-instance-under-the-protected-items"></a>Otomatik korumalı örneğine olan korumalı öğelerin altında yeni eklenen veritabanınızı neden göremiyorum?

Yeni eklenen bir veritabanına göremeyebilirsiniz bir [otomatik korumalı](backup-azure-sql-database.md#auto-protect-sql-server-in-azure-vm) örnek, korumalı anında. Bulma, genellikle her 8 saatte bir çalışır olmasıdır. Ancak, kullanıcı kullanarak el ile keşif çalıştırabilirsiniz **veritabanlarını kurtarmak** seçeneği bulmak ve yeni korumak için veritabanları hemen gösterildiği gibi aşağıdaki görüntüde:

  ![Yeni eklenen veritabanınızı görüntüleyin](./media/backup-azure-sql-database/view-newly-added-database.png)


## <a name="next-steps"></a>Sonraki adımlar

Azure Backup hakkında daha fazla bilgi için şifrelenmiş sanal makineleri yedeklemek için Azure PowerShell örnek bakın.

> [!div class="nextstepaction"]
> [Şifrelenmiş bir VM'yi yedekleme](./scripts/backup-powershell-sample-backup-encrypted-vm.md)
