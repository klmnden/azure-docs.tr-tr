---
title: SQL Server veritabanlarını azure'a yedekleyin | Microsoft Docs
description: Bu öğreticide, SQL Server'ı Azure'a yedekleme açıklanmaktadır.
services: backup
author: sachdevaswati
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 03/19/2019
ms.author: sachdevaswati
ms.openlocfilehash: 5e4bd3647b557b260e65e3fb1ce297892f5d7d78
ms.sourcegitcommit: 48a41b4b0bb89a8579fc35aa805cea22e2b9922c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59578833"
---
# <a name="back-up-sql-server-databases-in-azure-vms"></a>Azure VM’lerinde SQL Server veritabanlarını yedekleme

SQL Server veritabanları, düşük kurtarma noktası hedefi (RPO) ve uzun süreli saklama gerektiren kritik iş yükleri değildir. Azure Vm'leri üzerinde çalışan SQL Server veritabanlarını yedekleyebilirsiniz [Azure Backup](backup-overview.md).

Bu makalede bir Azure Backup kurtarma Hizmetleri kasası için bir Azure sanal makinesinde çalışan SQL Server veritabanını yedeklemek nasıl gösterir. Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Oluşturun ve kasa yapılandırın.
> * Veritabanlarını bulmak ve yedeklemeler ayarlayabilir.
> * Veritabanları için otomatik korumayı ayarlayın.


## <a name="prerequisites"></a>Önkoşullar

SQL Server veritabanınızı yedekleyin önce aşağıdaki koşulları denetleyin:

1. Tanımlamak veya [oluşturma](backup-sql-server-database-azure-vms.md#create-a-recovery-services-vault) aynı bölge veya VM SQL Server örneği barındıran yerel bir kurtarma Hizmetleri kasası.
2. [VM izinlerini denetleyin](backup-azure-sql-database.md#fix-sql-sysadmin-permissions) SQL veritabanlarını yedeklemek için gerekli.
3. VM sahip olduğunu doğrulayın [ağ bağlantısı](backup-sql-server-database-azure-vms.md#establish-network-connectivity).
4. SQL Server veritabanları ile uyumlu olarak adlandırılan onay [adlandırma kuralları](#verify-database-naming-guidelines-for-azure-backup) Azure Backup için.
5. Veritabanı için etkin diğer yedekleme çözümleri yoksa doğrulayın. Bu senaryoyu ayarlamak, önce diğer tüm SQL Server yedeklemeleri devre dışı bırakın. Herhangi bir çakışma olmadan VM'de çalışan SQL Server veritabanı için Azure Backup ile birlikte bir Azure VM için Azure yedeklemeyi etkinleştirebilirsiniz.


### <a name="establish-network-connectivity"></a>Ağ bağlantısı kurma

Tüm işlemler için SQL Server sanal makinesi sanal makine Azure genel IP adreslerine bağlantısı gerekir. VM işlemleri (veritabanı bulma, yedeklemeleri yapılandırma, yedeklemeler zamanlamak, geri yükleme kurtarma noktaları vb.) genel IP adreslerine bağlantısı olmadan başarısız. Bu seçeneklerden biri ile bağlantı kurar:

- **Azure veri merkezi IP aralıklarına izin verin**: İzin [IP aralıklarını](https://www.microsoft.com/download/details.aspx?id=41653) indirmesindeki. Ağ güvenlik grubu (NSG) erişmek için **kümesi AzureNetworkSecurityRule** cmdlet'i.
- **Trafiği yönlendirmek için bir HTTP Ara sunucusunu dağıtmak**: Bir Azure sanal makinesinde bir SQL Server veritabanını yedeklediğinizde, VM'deki yedekleme uzantısına Azure Backup ve Azure Depolama'ya veri yönetimi komutları göndermek için HTTPS API'lerini kullanır. Backup uzantısı, Azure Active Directory (Azure AD) kimlik doğrulaması için de kullanır. HTTP proxy üzerinden bu üç hizmeti yedekleme uzantısını trafiği yönlendirme. Uzantı genel internet erişimi için yapılandırılan tek bir bileşenidir.

Her seçenek avantajları ve dezavantajları vardır

**Seçenek** | **Avantajları** | **Dezavantajları**
--- | --- | ---
IP aralıklarına izin verin | Ek maliyet olmadan. | IP adresi aralıklarını zamanla değişir olduğundan yönetmek için karmaşık. <br/><br/> Azure, yalnızca Azure Storage'nın tam erişim sağlar.
Bir HTTP Ara sunucusunu kullanacak   | Depolama üzerinde ayrıntılı denetim proxy'sinde URL'leri izin verilir. <br/><br/> Vm'leri tek noktası internet erişimi. <br/><br/> Azure IP adresi değişiklikleri tabi değildir. | Bir VM ile Ara yazılım çalıştırmak için ek ücretler.

### <a name="set-vm-permissions"></a>VM izinleri ayarlama

Azure yedekleme, SQL Server veritabanı için yedeklemeyi yapılandırdığınız sırada birkaç şey yapar:

- Ekler **AzureBackupWindowsWorkload** uzantısı.
- Sanal makine üzerindeki veritabanlarını bulmak için Azure Backup, hesabı oluşturan **NT SERVICE\AzureWLBackupPluginSvc**. Bu hesap, yedekleme ve geri yükleme için kullanılır ve SQL sysadmin izinleri gerektirir.
- Azure Backup yararlanır **NT AUTHORITY\SYSTEM** SQL ortak bir oturum açma olacak şekilde bu hesabınızın olması gerekir böylece veritabanı bulma/sorgulama için hesap.

SQL Server VM Azure Market'te oluşturmadıysanız, bir hata alabilirsiniz **UserErrorSQLNoSysadminMembership**. Bu meydana gelirse [bu yönergeleri izleyin](backup-azure-sql-database.md#fix-sql-sysadmin-permissions).

### <a name="verify-database-naming-guidelines-for-azure-backup"></a>Azure Backup için veritabanı adlandırma yönergeleri doğrulayın

Veritabanı adları için aşağıdakileri kaçının:

  * Baştaki/sondaki boşluk
  * Sondaki '!'
  * Kapatma köşeli ayraç ']'
  * 'F:\' ile başlayan veritabanlarının adları

Azure tablo desteklenmeyen karakterler için diğer ad kullanımı sahibiz ancak bunları önleme öneririz. [Daha fazla bilgi edinin](https://docs.microsoft.com/rest/api/storageservices/Understanding-the-Table-Service-Data-Model?redirectedfrom=MSDN).


[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>SQL Server veritabanlarını Bul

Sanal makinede çalışan veritabanları keşfedin.

1. İçinde [Azure portalında](https://portal.azure.com), kullandığınız veritabanını yedeklemek için kurtarma Hizmetleri kasasını açın.

2. Üzerinde **kurtarma Hizmetleri kasası** panoyu seçin **yedekleme**.

   ![Yedekleme yedekleme hedefi menüsünü açmak için seçin](./media/backup-azure-sql-database/open-backup-menu.png)

3. İçinde **yedekleme hedefi**ayarlayın **iş yükünüz çalıştığı** için **Azure** (varsayılan).

4. İçinde **neleri yedeklemek istiyorsunuz**seçin **Azure VM'de SQL Server**.

    ![SQL Server Azure VM için yedeklemeyi seçin.](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

5. İçinde **yedekleme hedefi** > **VM'lerin içinde VT Bul**seçin **bulmayı Başlat** aboneliğinde korunmayan VM'ler için aranacak. Bu Abonelikteki korumasız sanal makinelerin sayısına bağlı olarak biraz zaman alabilir.

   - Korunmayan VM'ler sonra adı ve kaynak grubu tarafından listelenen bulma, listede görünmesi gerekir.
   - Bir VM beklediğiniz gibi listede yoksa, zaten bir kasada yedekleneceğini olup olmadığını denetleyin.
   - Birden çok VM aynı ada sahip olabilir, ancak farklı kaynak gruplarına ait.

     ![Yedekleme Beklemede sırasında VM'lerin içinde VT arayın](./media/backup-azure-sql-database/discovering-sql-databases.png)

6. VM listesinde SQL Server veritabanı çalıştıran VM'yi seçin > **Bul DBs**.

7. İzleme veritabanı bulgusunda **bildirimleri** alan. Uygulamanın, sanal makinede kaç veritabanı yere bağlı olarak işin tamamlanması biraz sürebilir. Seçili veritabanlarındaki bulunduğunda bir başarı iletisi görünür.

    ![Dağıtım başarılı iletisi](./media/backup-azure-sql-database/notifications-db-discovered.png)

8. Azure yedekleme, VM üzerindeki tüm SQL Server veritabanlarını bulur. Bulma işlemi sırasında arka planda aşağıdakiler gerçekleşir:

    - Azure yedekleme, iş yükü yedekleme kasası ile VM kaydedin. Tüm kayıtlı VM veritabanlarında, bu kasaya yalnızca yedeklenebilir.
    - Azure Backup'ı yükler **AzureBackupWindowsWorkload** VM uzantısı. SQL veritabanı'nda yüklü aracı yok.
    - Azure Backup, hizmet hesabı oluşturur **NT Service\AzureWLBackupPluginSvc** VM üzerinde.
      - Tüm yedekleme ve geri yükleme işlemleri, hizmet hesabı kullanın.
      - **NT Service\AzureWLBackupPluginSvc** SQL sysadmin izinleri olması gerekir. Azure Market'te oluşturulan tüm SQL Server Vm'lerinin gelir **SqlIaaSExtension** yüklü. **AzureBackupWindowsWorkload** uzantısı kullanan **SQLIaaSExtension** otomatik olarak gerekli izinleri almak için.
    - Marketten VM oluşturmamışsınızdır sonra VM yok **SqlIaaSExtension** yüklü ve bulma işlemi hata iletisiyle başarısız **UserErrorSQLNoSysAdminMembership**. İzleyin [yönergeleri](backup-azure-sql-database.md#fix-sql-sysadmin-permissions) bu sorunu gidermek için.

        ![VM ve veritabanı seçin](./media/backup-azure-sql-database/registration-errors.png)

## <a name="configure-backup"></a>Yedeklemeyi yapılandırma  

Yedekleme aşağıdaki gibi yapılandırın:

1. İçinde **yedekleme hedefi** seçin **yedeklemeyi Yapılandır**.

   ![Seçin yedeklemeyi yapılandırma](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

2. Tıklayın **Yapılandırma Yedekleme**, **yedeklenecek öğeleri seçin** dikey penceresi görünür. Bu, tüm kayıtlı kullanılabilirlik grupları ve tek başına SQL sunucuları listeler. Örneği veya Always on AG korumasız veritabanlarını görmek için satır solundaki Köşeli Çift Ayraca genişletin.  

    ![Tek başına veritabanları ile tüm SQL Server örneklerini görüntüleme](./media/backup-azure-sql-database/list-of-sql-databases.png)

3. Korumak istediğiniz tüm veritabanlarını seçin > **Tamam**.

   ![Veritabanını koruma](./media/backup-azure-sql-database/select-database-to-protect.png)

   Yedekleme yüklerini en iyi duruma getirmek için Azure Backup bir en fazla veritabanı sayısı 50'ye bir yedekleme işi ayarlar.

     * 50'den fazla veritabanlarını korumak için birden çok yedekleme yapılandırın.
     * Alternatif olarak, etkinleştirebilirsiniz [otomatik korumayı](#enable-auto-protection) tüm örneği veya her zaman üzerinde kullanılabilirlik grubu seçerek **ON** seçeneği karşılık gelen açılır penceresinde **AUTOPROTECT**  sütun. [Otomatik korumayı](#enable-auto-protection) özellik yalnızca tek bir seferde tüm var olan veritabanları korumasını sağlar, ancak gelecekte bu örneği veya kullanılabilirlik grubuna eklenecek yeni veritabanlarını da otomatik olarak korur.  

4. Tıklayın **Tamam** açmak için **yedekleme İlkesi** dikey penceresi.

    ![Always On kullanılabilirlik grubunda otomatik korumayı etkinleştir](./media/backup-azure-sql-database/enable-auto-protection.png)

5. İçinde **yedekleme ilkesi seçmek**, bir ilkeyi seçin ve ardından tıklayın **Tamam**.

   - Varsayılan ilkeyi seçin: HourlyLogBackup.
   - SQL için daha önce oluşturulmuş mevcut bir yedekleme İlkesi'ni seçin.
   - RPO ve bekletme aralığı alan yeni bir ilke tanımlayın.

     ![Yedekleme ilkesini seçin](./media/backup-azure-sql-database/select-backup-policy.png)

6. Üzerinde **yedekleme** menüsünde **yedeklemeyi etkinleştir**.

    ![Seçilen yedekleme İlkesi'ni etkinleştir](./media/backup-azure-sql-database/enable-backup-button.png)

7. Yapılandırma ilerlemeyi **bildirimleri** portalının alan.

    ![Bildirim alanı](./media/backup-azure-sql-database/notifications-area.png)

### <a name="create-a-backup-policy"></a>Bir yedekleme ilkesi oluşturma

Bir yedekleme İlkesi yedeklemeleri ne zaman alınır ve ne kadar süreyle tutulur tanımlar.

- Kasa düzeyinde bir ilke oluşturulur.
- Birden çok kasa ve aynı yedekleme ilkesine kullanabilirsiniz, ancak her kasa için yedekleme ilkesini uygulama.
- Bir yedekleme ilkesi oluşturduğunuzda, bir günlük tam yedekleme varsayılandır.
- Tam yedekleme haftalık olarak gerçekleşecek şekilde yapılandırırsanız, ancak yalnızca bir değişiklik yedeği ekleyebilirsiniz.
- [Hakkında bilgi edinin](backup-architecture.md#sql-server-backup-types) yedekleme ilkeleri farklı türde.

Bir yedekleme ilkesi oluşturmak için:

1. Kasaya tıklayın **yedekleme ilkeleri** > **Ekle**.
2. İçinde **Ekle** menüsünü tıklatın **Azure VM'de SQL Server**. Bu ilke türü tanımlar.

   ![Yeni bir yedekleme ilkesi için bir ilke türü seçin](./media/backup-azure-sql-database/policy-type-details.png)

3. İçinde **ilke adı**, yeni ilke için bir ad girin.
4. İçinde **tam yedekleme İlkesi**seçin bir **yedekleme sıklığı**, seçin **günlük** veya **haftalık**.

   - İçin **günlük**yedekleme işi başladığında saat dilimini ve saat seçin.
   - Tam yedekleme çalıştırmanız gerekir, devre dışı bırakamazlar **tam yedekleme** seçeneği.
   - Tıklayın **tam yedekleme** ilkesini görüntülemek için.
   - Günlük tam yedekleme için fark yedeklemelerinin oluşturulamıyor.
   - İçin **haftalık**, yedekleme işi başladığında günü, haftanın günü, saat ve saat dilimi seçin.

     ![Yeni bir yedekleme İlkesi alanlar](./media/backup-azure-sql-database/full-backup-policy.png)  

5. İçin **bekletme aralığı**, tüm seçenekleri varsayılan olarak seçilidir. Ve belirli aralıklarda kullanmak istemediğiniz herhangi bir istenmeyen bekletme aralığı sınırları temizleyin.

    - Yedekleme (tam/değişiklik/günlüğü) herhangi bir türü için en düşük bekletme süresi 7 gündür.
    - Kurtarma noktalarının bekletme, bekletme aralığına göre etiketlenir. Örneğin, bir günlük tam yedekleme öğesini seçerseniz, yalnızca bir tam yedekleme her gün tetiklenir.
    - Yedekleme haftalık bir bekletme aralığı ve haftalık bekletme ayarınızı bağlı olarak belirli bir günde etiketlenmiş ve korunur.
    - Aylık ve yıllık bekletme aralıkları benzer şekilde davranır.

   ![Bekletme aralığı aralığı ayarları](./media/backup-azure-sql-database/retention-range-interval.png)

6. İçinde **tam yedekleme İlkesi** menüsünde **Tamam** ayarları kabul etmek için.
7. Fark yedekleme ilkesi eklemek için seçin **fark yedekleme**.

   ![Bekletme aralığı aralığı ayarları](./media/backup-azure-sql-database/retention-range-interval.png)
   ![fark yedekleme İlkesi menüsü açın](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

8. İçinde **fark yedekleme İlkesi**seçin **etkinleştirme** sıklığı ve bekletme denetimleri açın.

    - En fazla günde bir fark yedekleme tetikleyebilirsiniz.
    - Değişiklik yedekleri, en fazla 180 gün için saklanabilir. Daha uzun bekletme süresi gerekiyorsa, tam yedeklemeler kullanmanız gerekir.

9. Seçin **Tamam** ilkeyi kaydedin ve ana menüye dön **yedekleme İlkesi** menüsü.

10. İşlem günlüğü yedekleme ilkesi eklemek için seçin **günlük yedekleme**.
11. İçinde **günlük yedeği**seçin **etkinleştirme**ve ardından sıklığı ve bekletme denetimleri ayarlayın. Günlük yedeklemeleri 15 dakikada bir sıklıkta meydana gelebilir ve 35 güne kadar saklanabilir.
12. Seçin **Tamam** ilkeyi kaydedin ve ana menüye dön **yedekleme İlkesi** menüsü.

    ![Günlük yedekleme ilkesini Düzenle](./media/backup-azure-sql-database/log-backup-policy-editor.png)

13. Üzerinde **yedekleme İlkesi** menüsünde etkinleştirilip etkinleştirilmeyeceğini seçin **SQL yedekleme sıkıştırması**.
    - Sıkıştırma, varsayılan olarak devre dışıdır.
    - Arka uçta Azure Backup SQL yerel yedekleme sıkıştırmasını kullanır.

14. Yedekleme İlkesi düzenlemeleri tamamladığınızda seçin **Tamam**.


### <a name="modify-policy"></a>İlkeyi değiştirin
Yedekleme sıklığı veya bekletme aralığını değiştirmek için ilkeyi değiştirin.

> [!NOTE]
> Yenilerini yanı sıra tüm eski kurtarma noktalarının Bekletme dönemi içindeki herhangi bir değişiklik retrospectively uygulanır.

Kasa panosunda Git **Yönet** > **yedekleme ilkeleri** ve düzenlemek istediğiniz ilkeyi seçin.

  ![Yedekleme ilkesini yönetme](./media/backup-azure-sql-database/modify-backup-policy.png)


## <a name="enable-auto-protection"></a>Otomatik korumayı etkinleştir  

Otomatik olarak mevcut tüm veritabanlarını ve gelecekte bir tek başına SQL Server örneği veya bir SQL Server Always on kullanılabilirlik grubu için eklenen veritabanlarını yedeklemek otomatik korumayı etkinleştirin.

- Tek bir seferde otomatik koruma için seçebileceğiniz veritabanı sayısı sınırlı değildir.
- Seçmeli olarak içerik koruyamıyor veya veritabanlarını korumadan örneği otomatik korumasını etkinleştirme zaman hariç.
- Örneğiniz korumalı bazı veritabanları içeriyorsa, otomatik koruma üzerinde açana altında ilgili ilkelerini korunması olmaya devam eder. Ancak, korumalı olmayan tüm veritabanlarını ve gelecekte eklenin veritabanları olacaktır altında otomatik korumayı etkinleştirme sırasında tanımladığınız yalnızca tek bir ilke **yedeklemeyi Yapılandır**. Ancak, daha sonra otomatik korumalı bir veritabanıyla ilişkili ilkeyi değiştirebilirsiniz.  

Otomatik korumayı etkinleştirmek için adımlar aşağıdaki gibidir:

  1. İçinde **yedeklenecek öğeleri**, otomatik korumayı etkinleştirmek istediğiniz örneği seçin.
  2. Altındaki açılan menüyü seçin **Autoprotect**ve kümesine **üzerinde**. Daha sonra, **Tamam**'a tıklayın.

      ![Always On kullanılabilirlik grubunda otomatik korumayı etkinleştir](./media/backup-azure-sql-database/enable-auto-protection.png)

  3. Yedekleme için tüm veritabanlarını birlikte yapılandırılan ve izlenebilir **yedekleme işleri**.

Otomatik korumayı devre dışı bırakmanız gerekirse, örnek adı altında tıklayın **yedeklemeyi Yapılandır**seçip **otomatik koruma devre dışı bırak** örneği. Tüm veritabanlarını yedeklemek devam eder, ancak gelecekte veritabanlarının otomatik olarak korunması gerekmez.

![Bu örneği otomatik korumasını devre dışı](./media/backup-azure-sql-database/disable-auto-protection.png)

 
## <a name="next-steps"></a>Sonraki adımlar

- [Hakkında bilgi edinin](restore-sql-database-azure-vm.md) yedeklenen SQL Server veritabanlarını geri yükleme.
- [Hakkında bilgi edinin](manage-monitor-sql-database-backup.md) yedeklenen SQL Server veritabanlarını yönetme.
