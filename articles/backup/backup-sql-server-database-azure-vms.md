---
title: Azure vm'lerde SQL Server veritabanlarını yedekleme | Microsoft Docs
description: Azure vm'lerde SQL Server veritabanlarını yedeklemek hakkında bilgi edinin
services: backup
author: sachdevaswati
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 06/18/2019
ms.author: sachdevaswati
ms.openlocfilehash: 28577bfc755d80cd479a40b9e2b653af6ddec319
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204445"
---
# <a name="back-up-sql-server-databases-in-azure-vms"></a>Azure VM’lerinde SQL Server veritabanlarını yedekleme

SQL Server veritabanları, düşük kurtarma noktası hedefi (RPO) ve uzun süreli saklama gerektiren kritik iş yükleri değildir. Azure sanal makinelerinde (VM'ler) kullanarak çalışan SQL Server veritabanlarını yedekleyebilirsiniz [Azure Backup](backup-overview.md).

Bu makalede, bir Azure Backup kurtarma Hizmetleri kasası için bir Azure sanal makinesinde çalışan SQL Server veritabanını yedeklemek gösterilmektedir.

Bu makalede, öğreneceksiniz nasıl yapılır:

> [!div class="checklist"]
> * Oluşturun ve kasa yapılandırın.
> * Veritabanlarını bulmak ve yedeklemeler ayarlayabilir.
> * Veritabanları için otomatik korumayı ayarlayın.


## <a name="prerequisites"></a>Önkoşullar

Bir SQL Server veritabanını yedekleme önce aşağıdaki ölçütleri kontrol edin:

1. Kimliğinizi belirlemek veya oluşturma bir [kurtarma Hizmetleri kasası](backup-sql-server-database-azure-vms.md#create-a-recovery-services-vault) aynı bölge veya VM SQL Server örneği barındıran yerel ayar.
2. VM sahip olduğunu doğrulayın [ağ bağlantısı](backup-sql-server-database-azure-vms.md#establish-network-connectivity).
3. SQL Server veritabanlarını uyguladığınızdan emin olun [adlandırma kuralları için Azure Backup veritabanı](#database-naming-guidelines-for-azure-backup).
4. Özellikle SQL 2008 ve 2008 R2 için [kayıt defteri anahtarı ekleme](#add-registry-key-to-enable-registration) sunucu kaydı etkinleştirmek için. Bu adım olması olmayacak gerekli özelliği genel kullanıma sunulduğunda.
5. Veritabanı için etkin diğer yedekleme çözümleri almadığınızı denetleyin. Veritabanını yedekleme önce diğer tüm SQL Server yedeklemeleri devre dışı bırakın.

> [!NOTE]
> Azure Backup, bir Azure VM ve çakışma olmadan VM'de çalışan SQL Server veritabanı için de etkinleştirebilirsiniz.


### <a name="establish-network-connectivity"></a>Ağ bağlantısı kurma

Tüm işlemler için bir SQL Server VM, Azure genel IP adresleri bağlantısı gerektirir. Azure genel IP adreslerine bağlantısı olmadan (veritabanı keşfi, yedeklemeleri yapılandırma, yedeklemeler zamanlamak, Kurtarma noktalarını geri ve benzeri) VM işlem başarısız.

Aşağıdaki seçeneklerden birini kullanarak bağlantı kurar:

- **Azure veri merkezi IP aralıklarına izin verin**. Bu seçenek sayesinde [IP aralıklarını](https://www.microsoft.com/download/details.aspx?id=41653) indirmesindeki. Bir ağ güvenlik grubu (NSG) erişmek için Set-AzureNetworkSecurityRule cmdlet'ini kullanın. Güvenli yalnızca bölgeye özel IP'ler alıcıların listesi, aynı zamanda kimlik doğrulamasını etkinleştirmek için Azure Active Directory (Azure AD) hizmet etiketi güvenli alıcı listesi güncelleştirmeniz gerekir.

- **NSG etiketleri kullanarak erişim izni**. Bağlantı kısıtlamak için Nsg kullanırsanız, bu seçeneği AzureBackup etiketini kullanarak Azure Backup için giden erişim veren NSG kuralı ekler. Bu etiket yanı sıra, ayrıca karşılık gelen gerekir [kuralları](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) Azure AD için ve kimlik doğrulaması ve veri aktarımı için bağlantısına izin vermek için Azure depolama. AzureBackup etiketi yalnızca PowerShell üzerinde şu anda kullanılabilir. AzureBackup etiketini kullanarak bir kural oluşturmak için:

    - Azure hesabı kimlik bilgilerini ekleyin ve Ulusal bulutlarda güncelleştirin<br/>
    `Add-AzureRmAccount`

    - NSG aboneliği seçin<br/>
    `Select-AzureRmSubscription "<Subscription Id>"`

     - NSG seçin<br/>
    `$nsg = Get-AzureRmNetworkSecurityGroup -Name "<NSG name>" -ResourceGroupName "<NSG resource group name>"`

    - Eklemek için Azure Backup hizmeti etiketi giden kuralı izin ver<br/>
    `Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name "AzureBackupAllowOutbound" -Access Allow -Protocol * -Direction Outbound -Priority <priority> -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix "AzureBackup" -DestinationPortRange 443 -Description "Allow outbound traffic to Azure Backup service"`

  - NSG Kaydet<br/>
    `Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg`
- **Azure güvenlik duvarı etiketleri kullanarak erişim izni**. Azure güvenlik duvarı kullanıyorsanız, AzureBackup kullanarak bir uygulama kuralı oluşturma [FQDN etiketi](https://docs.microsoft.com/azure/firewall/fqdn-tags). Bu, Azure Backup için giden erişim sağlar.
- **Trafiği yönlendirmek için bir HTTP Ara sunucusunu dağıtmak**. Bir Azure sanal makinesinde bir SQL Server veritabanını yedeklediğinizde, VM'deki yedekleme uzantısına Azure Backup ve Azure Depolama'ya veri yönetimi komutları göndermek için HTTPS API'lerini kullanır. Backup uzantısı, Azure AD kimlik doğrulaması için de kullanır. HTTP proxy üzerinden bu üç hizmeti yedekleme uzantısını trafiği yönlendirme. Genel internet erişimi için yapılandırılan tek bileşen uzantılarıdır.

Bağlantı seçenekleri, aşağıdaki avantajları ve dezavantajları şunlardır:

**Seçeneği** | **Avantajları** | **Dezavantajları**
--- | --- | ---
IP aralıklarına izin verin | Ek maliyet olmadan | IP adresi aralıklarını zamanla değişir olduğundan yönetmek için karmaşık <br/><br/> Azure, yalnızca Azure Storage'nın tam erişim sağlar
NSG hizmet etiketleri kullanma | Aralık değişiklikleri otomatik olarak birleştirilmiş olarak yönetilmesi daha kolay <br/><br/> Ek maliyet olmadan <br/><br/> | Nsg'ler ile yalnızca kullanılabilir <br/><br/> Tüm hizmet erişim sağlar
Azure güvenlik duvarı FQDN etiketleri kullanma | Gerekli FQDN'leri otomatik olarak yönetildiği yönetilmesi daha kolay | Azure güvenlik duvarı ile yalnızca kullanılabilir
Bir HTTP Ara sunucusunu kullanacak | Depolama üzerinde ayrıntılı denetim proxy'sinde URL'leri izin verilir <br/><br/> Vm'leri tek noktası internet erişimi <br/><br/> Azure IP adresi değişiklikleri tabidir | Bir VM ile Ara yazılım çalıştırmak için ek ücretler

### <a name="database-naming-guidelines-for-azure-backup"></a>Azure Backup için adlandırma kuralları veritabanı

Aşağıdaki öğeleri veritabanı adları kullanmaktan kaçının:

  * Baştaki ve sondaki
  * Sondaki ünlem işareti (!)
  * Köşeli ayraç (])
  * Noktalı virgül ';'
  * Eğik çizgi '/'

Diğer ad kullanımı için desteklenmeyen karakterler kullanılabilir, ancak bunları önleme öneririz. Daha fazla bilgi için bkz. [Tablo Hizmeti Veri Modelini anlama](https://docs.microsoft.com/rest/api/storageservices/Understanding-the-Table-Service-Data-Model?redirectedfrom=MSDN).

### <a name="add-registry-key-to-enable-registration"></a>Kaydı etkinleştirmek için kayıt defteri anahtarını ekleyin

1. Açık Regedit
2. Kayıt defteri dizin yolunu oluşturun: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WorkloadBackup\TestHook (sırayla altında Microsoft oluşturulması gereken WorkloadBackup altında 'Key' TestHook oluşturmanız gerekecektir).
3. Kayıt defteri dizin yolu altında yeni bir 'dize değeri' dize adıyla oluşturun **AzureBackupEnableWin2K8R2SP1** ve değer: **TRUE**

    ![RegEdit kaydı etkinleştirmek için](media/backup-azure-sql-database/reg-edit-sqleos-bkp.png)

Alternatif olarak, aşağıdaki komutla .reg dosyasını çalıştırarak bu adımı otomatikleştirebilirsiniz:

```csharp
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WorkloadBackup\TestHook]
"AzureBackupEnableWin2K8R2SP1"="True"
```

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>SQL Server veritabanlarını Bul

Bir VM'de çalışan veritabanlarını bulmak nasıl:

1. İçinde [Azure portalında](https://portal.azure.com), kullandığınız veritabanını yedeklemek için kurtarma Hizmetleri kasasını açın.

2. İçinde **kurtarma Hizmetleri kasası** panoyu seçin **yedekleme**.

   ![Yedekleme yedekleme hedefi menüsünü açmak için seçin](./media/backup-azure-sql-database/open-backup-menu.png)

3. İçinde **yedekleme hedefi**ayarlayın **iş yükünüz çalıştığı?** için **Azure**.

4. İçinde **neleri yedeklemek istiyorsunuz**seçin **Azure VM'de SQL Server**.

    ![SQL Server Azure VM için yedeklemeyi seçin.](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

5. İçinde **yedekleme hedefi** > **VM'lerin içinde VT Bul**seçin **bulmayı Başlat** aboneliğinde korunmayan VM'ler için aranacak. Bu arama aboneliğinde korunmayan VM'ler sayısına bağlı olarak biraz zaman alabilir.

   - Korunmayan VM'ler sonra adı ve kaynak grubu tarafından listelenen bulma, listede görünmesi gerekir.
   - Bir VM beklediğiniz gibi listede yoksa, zaten bir kasada yedekleneceğini olmadığını bakın.
   - Birden çok VM aynı ada sahip olabilir, ancak farklı kaynak gruplarına ait.

     ![Yedekleme Beklemede sırasında VM'lerin içinde VT arayın](./media/backup-azure-sql-database/discovering-sql-databases.png)

6. VM listesinde SQL Server veritabanı çalıştıran VM'yi seçin > **Bul DBs**.

7. İzleme veritabanı bulgusunda **bildirimleri**. Bu eylem için gereken süre VM veritabanları sayısına bağlıdır. Seçili veritabanlarındaki bulunduğunda bir başarı iletisi görünür.

    ![Dağıtım başarılı iletisi](./media/backup-azure-sql-database/notifications-db-discovered.png)

8. Azure yedekleme, VM üzerindeki tüm SQL Server veritabanlarını bulur. Bulma işlemi sırasında aşağıdaki öğeleri arka planda gerçekleşir:

    - Azure yedekleme, VM için iş yükü yedekleme kasası ile kaydeder. Yalnızca bu kasaya kayıtlı VM üzerindeki tüm veritabanlarını yedeklenebilir.
    - Azure yedekleme, VM üzerinde AzureBackupWindowsWorkload uzantıyı yükler. Aracı bir SQL veritabanı'nda yüklü.
    - Azure Backup, hizmet hesabını NT Service\AzureWLBackupPluginSvc VM oluşturur.
      - Tüm yedekleme ve geri yükleme işlemleri, hizmet hesabı kullanın.
      - NT Service\AzureWLBackupPluginSvc SQL sysadmin izinleri gerektirir. Market'te oluşturulan tüm SQL Server Vm'lerinin yüklü SqlIaaSExtension ile gelir. AzureBackupWindowsWorkload uzantı SQLIaaSExtension otomatik olarak gerekli izinleri almak için kullanır.
    - Marketten VM oluşturmadıysanız veya SQL 2008 ve 2008 R2 üzerinde kullanıyorsanız, VM SqlIaaSExtension yüklü olmayabilir ve bulma işlemi UserErrorSQLNoSysAdminMembership hata iletisiyle başarısız olur. Bu sorunu düzeltmek için yönergeleri uygulayın. [kümesi VM izinleri](backup-azure-sql-database.md#set-vm-permissions).

        ![VM ve veritabanı seçin](./media/backup-azure-sql-database/registration-errors.png)

## <a name="configure-backup"></a>Yedeklemeyi yapılandırma  

1. İçinde **yedekleme hedefi** > **2. adım: Yedeklemeyi yapılandırma**seçin **yedeklemeyi Yapılandır**.

   ![Seçin yedeklemeyi yapılandırma](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

2. İçinde **yedeklenecek öğeleri seçin**, tüm kayıtlı kullanılabilirlik grupları ve tek başına SQL Server örneklerinin görürsünüz. Bu örnek ya da Always On kullanılabilirlik grubu korumasız tüm veritabanlarının listesini genişletmek için bir satır sol oku seçin.  

    ![Tek başına veritabanları ile tüm SQL Server örneklerini görüntüleme](./media/backup-azure-sql-database/list-of-sql-databases.png)

3. Koruyun ve ardından istediğiniz veritabanlarını seçin **Tamam**.

   ![Veritabanını koruma](./media/backup-azure-sql-database/select-database-to-protect.png)

   Yedekleme yüklerini en iyi duruma getirmek için Azure Backup bir en fazla veritabanı sayısı 50'ye bir yedekleme işi ayarlar.

     * 50'den fazla veritabanlarını korumak için birden çok yedekleme yapılandırın.
     * Etkinleştirmek için [ ](#enable-auto-protection) tüm örneği veya Always On kullanılabilirlik grubu. İçinde **AUTOPROTECT** aşağı açılan listesinden **ON**ve ardından **Tamam**.

    > [!NOTE]
    > [Otomatik korumayı](#enable-auto-protection) özellik, yalnızca tüm veritabanlarında aynı anda koruma sağlar, ancak bu örneği veya kullanılabilirlik grubuna eklenen tüm yeni veritabanları otomatik olarak korur.  

4. Seçin **Tamam** açmak için **yedekleme İlkesi**.

    ![Always On kullanılabilirlik grubu için otomatik korumayı etkinleştir](./media/backup-azure-sql-database/enable-auto-protection.png)

5. İçinde **yedekleme İlkesi**, bir ilkeyi seçin ve ardından **Tamam**.

   - Varsayılan ilke HourlyLogBackup seçin.
   - SQL için daha önce oluşturulmuş mevcut bir yedekleme İlkesi'ni seçin.
   - RPO ve bekletme aralığı alan yeni bir ilke tanımlayın.

     ![Yedekleme ilkesini seçin](./media/backup-azure-sql-database/select-backup-policy.png)

6. İçinde **yedekleme**seçin **yedeklemeyi etkinleştir**.

    ![Seçilen yedekleme İlkesi'ni etkinleştir](./media/backup-azure-sql-database/enable-backup-button.png)

7. Yapılandırma ilerlemeyi **bildirimleri** portalının alan.

    ![Bildirim alanı](./media/backup-azure-sql-database/notifications-area.png)

### <a name="create-a-backup-policy"></a>Bir yedekleme ilkesi oluşturma

Bir yedekleme İlkesi yedeklemeleri ne zaman alınır ve ne kadar süreyle tutulur tanımlar.

- Kasa düzeyinde bir ilke oluşturulur.
- Birden çok kasa ve aynı yedekleme ilkesine kullanabilirsiniz, ancak her kasa için yedekleme ilkesini uygulama.
- Bir yedekleme ilkesi oluşturduğunuzda, bir günlük tam yedekleme varsayılandır.
- Tam yedekleme haftalık olarak gerçekleşecek şekilde yapılandırırsanız, ancak yalnızca bir değişiklik yedeği ekleyebilirsiniz.
- Hakkında bilgi edinin [yedekleme ilkeleri farklı türde](backup-architecture.md#sql-server-backup-types).

Bir yedekleme ilkesi oluşturmak için:

1. Kasada seçin **yedekleme ilkeleri** > **Ekle**.
2. İçinde **Ekle**seçin **Azure VM'de SQL Server** ilke türü tanımlamak için.

   ![Yeni bir yedekleme ilkesi için bir ilke türü seçin](./media/backup-azure-sql-database/policy-type-details.png)

3. İçinde **ilke adı**, yeni ilke için bir ad girin.
4. İçinde **tam yedekleme İlkesi**seçin bir **yedekleme sıklığı**. Seçin ya da **günlük** veya **haftalık**.

   - İçin **günlük**yedekleme işi başladığında saat dilimini ve saat seçin.
   - İçin **haftalık**, yedekleme işi başladığında günü, haftanın günü, saat ve saat dilimi seçin.
   - Devre dışı bırakamazlar, çünkü bir tam yedekleme çalıştırma **tam yedekleme** seçeneği.
   - Seçin **tam yedekleme** ilkesini görüntülemek için.
   - Günlük tam yedekleme için fark yedeklemelerinin oluşturulamıyor.

     ![Yeni bir yedekleme İlkesi alanlar](./media/backup-azure-sql-database/full-backup-policy.png)  

5. İçinde **bekletme ARALIĞI**, tüm seçenekleri varsayılan olarak seçilidir. Clear yoksa istediğiniz ve ardından kullanılacak aralıkları ayarlayın, herhangi bir bekletme aralığı sınırlar.

    - Yedekleme (tam, değişiklik ve günlük) herhangi bir türü için en düşük bekletme süresi yedi gündür.
    - Kurtarma noktalarının bekletme, bekletme aralığına göre etiketlenir. Örneğin, bir günlük tam yedekleme öğesini seçerseniz, yalnızca bir tam yedekleme her gün tetiklenir.
    - Yedekleme haftalık bir bekletme aralığı ve haftalık bir tutma ayarı bağlı olarak belirli bir günde etiketlenmiş ve korunur.
    - Aylık ve yıllık bekletme aralıkları benzer şekilde davranır.

       ![Bekletme aralığı aralığı ayarları](./media/backup-azure-sql-database/retention-range-interval.png)

6. İçinde **tam yedekleme İlkesi** menüsünde **Tamam** ayarları kabul etmek için.
7. Fark yedekleme ilkesi eklemek için seçin **fark yedekleme**.

   ![Bekletme aralığı aralığı ayarları](./media/backup-azure-sql-database/retention-range-interval.png)
   ![fark yedekleme İlkesi menüsü açın](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

8. İçinde **fark yedekleme İlkesi**seçin **etkinleştirme** sıklığı ve bekletme denetimleri açın.

    - Gün başına yalnızca bir değişiklik yedeği tetikleyebilirsiniz.
    - Değişiklik yedekleri, en fazla 180 gün için saklanabilir. Daha uzun bekletme süresi için tam yedeklemelerini kullanın.

9. Seçin **Tamam** ilkeyi kaydedin ve ana menüye dön **yedekleme İlkesi** menüsü.

10. İşlem günlüğü yedekleme ilkesi eklemek için seçin **günlük yedekleme**.
11. İçinde **günlük yedeği**seçin **etkinleştirme**ve ardından sıklığı ve bekletme denetimleri ayarlayın. Günlük yedeklemeler ve 35 güne kadar saklanabilir 15 dakikada bir sıklıkta oluşabilir.
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

Otomatik olarak bir tek başına SQL Server örneğine veya bir Always On kullanılabilirlik grubu için tüm mevcut ve gelecekteki veritabanlarını yedeklemek otomatik korumayı etkinleştirebilirsiniz.

- Veritabanları için otomatik korumayı anda seçmek, sayısına bir sınır yoktur.
- Seçmeli olarak içerik koruyamıyor veya veritabanlarını korumadan örneği otomatik korumasını etkinleştirme zaman hariç.
- Örneğiniz korumalı bazı veritabanları içeriyorsa, üzerinde otomatik koruma açana altında ilgili ilkelerini korumalı kalırlar. Daha sonra eklenen tüm korumasız veritabanlarını otomatik-koruma altında listelenen etkinleştirme zaman tanımladığınız yalnızca tek bir ilke olacaktır **yedeklemeyi Yapılandır**. Ancak, daha sonra otomatik korumalı bir veritabanıyla ilişkili ilkeyi değiştirebilirsiniz.  

Otomatik korumayı etkinleştirmek için:

  1. İçinde **yedeklenecek öğeleri**, otomatik korumayı etkinleştirmek istediğiniz örneği seçin.
  2. Aşağı açılan listesinde seçin **AUTOPROTECT**, seçin **ON**ve ardından **Tamam**.

      ![Kullanılabilirlik grubunda otomatik korumayı etkinleştir](./media/backup-azure-sql-database/enable-auto-protection.png)

  3. Yedekleme için tüm veritabanlarını birlikte yapılandırılan ve izlenebilir **yedekleme işleri**.

Otomatik korumayı devre dışı bırakmanız gerekirse, altında örnek adını seçin **yedeklemeyi Yapılandır**ve ardından **devre dışı Autoprotect** örneği. Tüm veritabanlarının yedeklenmesine devam eder, ancak gelecekte veritabanlarının otomatik olarak korunması gerekmez.

![Bu örneği otomatik korumasını devre dışı](./media/backup-azure-sql-database/disable-auto-protection.png)

 
## <a name="next-steps"></a>Sonraki adımlar

Şunları nasıl yapacağınızı öğrenin:

- [Yedeklenen SQL Server veritabanlarını geri yükleyin](restore-sql-database-azure-vm.md)
- [Yedeklenen SQL Server veritabanlarını yönet](manage-monitor-sql-database-backup.md)
