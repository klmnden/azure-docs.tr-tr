---
title: "Azure yedekleme: Yedekleme ve Azure Backup ve PowerShell kullanarak Azure vm'lerinde SQL veritabanlarını geri yükleme"
description: Yedekleme ve Azure Backup ve PowerShell kullanarak Azure vm'lerinde SQL veritabanlarını geri yükleyin.
services: backup
author: pvrk
manager: vijayts
keywords: Azure yedekleme; SQL;
ms.service: backup
ms.topic: conceptual
ms.date: 03/15/2019
ms.author: pullabhk
ms.assetid: 57854626-91f9-4677-b6a2-5d12b6a866e1
ms.openlocfilehash: 6a2e065466ab4426a6472b64fae19d264ff8dd81
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734234"
---
# <a name="back-up-and-restore-sql-databases-in-azure--vms-with-powershell"></a>Yedekleme ve PowerShell ile Azure sanal makinelerinde SQL veritabanlarını geri yükleme

Bu makalede, yedekleme ve Kurtarma kullanarak bir Azure VM içinde bir SQL DB için Azure PowerShell kullanmayı açıklar [Azure Backup](backup-overview.md) kurtarma Hizmetleri kasası.

Bu öğreticide, aşağıdaki işlemlerin nasıl yapılacağı açıklanmaktadır:

> [!div class="checklist"]
> * PowerShell'i ayarlayın ve Azure kurtarma Hizmetleri sağlayıcısını kaydedin.
> * Kurtarma Hizmetleri kasası oluşturun.
> * SQL DB ile bir Azure VM için yedeklemeyi yapılandırın.
> * Bir yedekleme işi çalıştırın.
> * Yedeklenen SQL DB geri yükleyin.
> * Yedeklemeyi izleme ve geri yükleme işleri.

## <a name="before-you-start"></a>Başlamadan önce

* [Daha fazla bilgi edinin](backup-azure-recovery-services-vault-overview.md) kurtarma Hizmetleri kasası hakkında.
* Özellik özellikleri için hakkında okuyun [Azure vm'lerde SQL veritabanlarını yedekleme](backup-azure-sql-database.md#before-you-start).
* Kurtarma Hizmetleri için PowerShell nesne hiyerarşisi gözden geçirin.

### <a name="recovery-services-object-hierarchy"></a>Kurtarma Hizmetleri nesne hiyerarşisi

Nesne hiyerarşisi aşağıdaki diyagramda özetlenir.

![Kurtarma Hizmetleri nesne hiyerarşisi](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

Gözden geçirme **Az.RecoveryServices** [cmdlet başvurusu](/powershell/module/az.recoveryservices) Azure Kitaplığı Başvurusu.

### <a name="set-up-and-install"></a>Ayarlama ve yükleme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell'i aşağıdaki gibi ayarlayın:

1. [Az PowerShell'in en son sürümünü indirin](/powershell/azure/install-az-ps). Gereken en düşük sürümü 1.5.0 ' dir.

2. Bu komut Azure Backup PowerShell cmdlet'leriyle bulun:

    ```powershell
    Get-Command *azrecoveryservices*
    ```

3. Diğer adları ve cmdlet'leri, Azure yedekleme ve kurtarma Hizmetleri kasası için gözden geçirin. Ne görebileceğinize ilişkin bir örnek aşağıda verilmiştir. Cmdlet öğelerinin tam bir listesi değil.

    ![Kurtarma Hizmetleri cmdlet'lerinin listesi](./media/backup-azure-afs-automation/list-of-recoveryservices-ps-az.png)

4. Azure hesabınızda oturum açın **Connect AzAccount**.
5. Web görünen sayfasında hesabı kimlik bilgilerinizle giriş istenir.

    * Alternatif olarak, bir parametre olarak bir hesap kimlik bilgilerinizi içerebilir **Connect AzAccount** cmdlet'iyle **-Credential**.
    * Bir kiracı için çalışan bir CSP iş ortağı olduğunuzdan, müşteri, Kiracı kimliği veya Kiracı birincil etki alanı adlarını kullanarak Kiracı olarak belirtin. Bir örnek **Connect AzAccount-Kiracı** fabrikam.com.

6. Hesabınız birden fazla abonelik olabilir çünkü hesabı ile kullanmak istediğiniz aboneliği ilişkilendirin.

    ```powershell
    Select-AzSubscription -SubscriptionName $SubscriptionName
    ```

7. Azure Backup'ı ilk kez kullanıyorsanız, kullanın **Register-AzResourceProvider** Azure kurtarma Hizmetleri sağlayıcısını aboneliğinize kaydetmeniz için cmdlet'i.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

8. Sağlayıcılar başarıyla kaydedildiğini doğrulayın:

    ```powershell
    Get-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

9. Komut çıktısında, doğrulayın **RegistrationState** değişikliklerini **kayıtlı**. Bu çalışmazsa **Register-AzResourceProvider** cmdlet'ini yeniden.

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Bir kurtarma Hizmetleri kasası oluşturmak için aşağıdaki adımları izleyin.

Kurtarma Hizmetleri kasası bir Resource Manager kaynağı olduğundan, bir kaynak grubu içinde yerleştirmeniz gerekir. Mevcut bir kaynak grubunu kullanabilir veya bir kaynak grubu ile oluşturabileceğiniz **yeni AzResourceGroup** cmdlet'i. Bir kaynak grubu oluşturduğunuzda, kaynak grubunun konumunu ve adını belirtin.

1. Bir kasayı bir kaynak grubuna yerleştirilir. Mevcut bir kaynak grubu, yeni bir yoksa [yeni AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-1.4.0). Bu örnekte, Batı ABD bölgesinde yeni bir kaynak grubu oluştururuz.

    ```powershell
    New-AzResourceGroup -Name "test-rg" -Location "West US"
    ```

2. Kullanım [yeni AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/New-AzRecoveryServicesVault?view=azps-1.4.0) kasası oluşturmak için cmdlet'i. Kasayla aynı konumda kaynak grubu için kullanılan belirtin.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```

3. Kasa depolamak için kullanılacak yedeklilik türünü belirtin.

    * Kullanabileceğiniz [yerel olarak yedekli depolama](../storage/common/storage-redundancy-lrs.md) veya [coğrafi olarak yedekli depolama](../storage/common/storage-redundancy-grs.md).
    * Aşağıdaki örnek kümeleri **- BackupStorageRedundancy** seçeneğini[kümesi AzRecoveryServicesBackupProperty](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupproperty) için cmd **testvault** kümesine  **GeoRedundant**.

    ```powershell
    $vault1 = Get-AzRecoveryServicesVault -Name "testvault"
    Set-AzRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

### <a name="view-the-vaults-in-a-subscription"></a>Bir abonelikte kasalarını görüntüle

Tüm kasaları abonelikte görüntülemek için kullanın [Get-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesvault?view=azps-1.4.0).

```powershell
Get-AzRecoveryServicesVault
```

Çıktı aşağıdakine benzer. İlişkili kaynak grubunu ve konumu sağlanır.

```powershell
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```

### <a name="set-the-vault-context"></a>Kasa bağlamını ayarlayın

Kasasının bir değişkende Store ve kasa bağlamını ayarlayın.

* Kasasının bir değişkende depolanması uygundur bunu çoğu Azure Backup cmdlet'i, girdi olarak kurtarma Hizmetleri kasasını gerektirir.
* Kasa bağlamı, kasada korunan veri türüdür. İle ayarlayın [kümesi AzRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesvaultcontext?view=azps-1.4.0). Bağlamı ayarlandıktan sonra sonraki tüm cmdlet'ler için geçerlidir.

Aşağıdaki örnek için kasa bağlamını ayarlar **testvault**.

```powershell
Get-AzRecoveryServicesVault -Name "testvault" | Set-AzRecoveryServicesVaultContext
```

### <a name="fetch-the-vault-id"></a>Getirilemedi vault kimliği

Azure PowerShell yönergelerine uygun olarak ayarlamak kasa bağlamını kullanım dışı bırakılıyor planlıyoruz. Bunun yerine, saklamak veya kasa kimliği getirin ve ilgili komutları gibi geçirin:

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "Contoso-docs-rg" -Name "testvault" | select -ExpandProperty ID
```

## <a name="configure-a-backup-policy"></a>Bir yedekleme ilkesi yapılandırma

Bir yedekleme İlkesi yedeklemeler için zamanlamayı belirtir ve ne kadar süreyle yedekleme kurtarma noktaları tutulması gereken:

* Bir yedekleme ilkesi en az bir bekletme ilkesi ile ilişkilidir. Bir bekletme ilkesi, silinmeden önce ne kadar bir kurtarma noktası tutulur tanımlar.
* Varsayılan yedekleme İlkesi bekletme kullanarak Görünüm [Get-AzRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupretentionpolicyobject?view=azps-1.4.0).
* Varsayılan yedekleme İlkesi zamanlamayı kullanarak Görünüm [Get-AzRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupschedulepolicyobject?view=azps-1.4.0).
* Kullandığınız [yeni AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupprotectionpolicy?view=azps-1.4.0) cmdlet'i yeni bir yedekleme ilkesi oluşturmak için. Zamanlama ve Bekletme İlkesi nesneleri girdiğiniz.

Aşağıdaki örnek zamanlama ilkesini ve bekletme ilkesi değişkenlerinde depolar. Ardından bu değişkenleri parametreler için yeni bir ilke kullanır (**NewSQLPolicy**). **NewSQLPolicy** "Tam" bir günlük yedeğini alır, 180 gün boyunca tutar ve her 2 saatte bir günlük yedeği alır

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "MSSQL"
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "MSSQL"
$NewSQLPolicy = New-AzRecoveryServicesBackupProtectionPolicy -Name "NewSQLPolicy" -WorkloadType "MSSQL" -RetentionPolicy $retPol -SchedulePolicy $schPol
```

Çıktı aşağıdakine benzer.

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                Frequency                                IsDifferentialBackup IsLogBackupEnabled
                                                                                                                                Enabled
----                 ------------       -------------------- ----------                ---------                                -------------------- ------------------
NewSQLPolicy         MSSQL              AzureWorkload        3/15/2019 9:00:00 PM      Daily                                    False                True
```

## <a name="enable-backup"></a>Yedeklemeyi etkinleştir

### <a name="registering-the-sql-vm"></a>SQL VM kaydediliyor

Azure VM yedeklemeleri ve Azure dosya paylaşımları için yedekleme hizmeti için bu Azure Resource Manager kaynaklarına bağlanabilir ve ilgili ayrıntıları getirilemedi. SQL Azure VM ile bir uygulama olduğundan, Backup hizmeti, uygulamaya erişmek ve gerekli bilgileri getirmek için izniniz gerekiyor. Bunu yapabilmek için yapmanız *'register'* bir kurtarma Hizmetleri kasası ile SQL uygulamayı içeren bir Azure VM. SQL VM bir kasaya kaydettiğinizde, SQL DB'leri yalnızca bu kasaya koruyabilirsiniz. Kullanım [Register-AzRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/az.recoveryservices/Register-AzRecoveryServicesBackupContainer?view=azps-1.5.0) PS cmdlet'i, VM kaydedilemiyor.

````powershell
 $myVM = Get-AzVM -ResourceGroupName <VMRG Name> -Name <VMName>
Register-AzRecoveryServicesBackupContainer -ResourceId $myVM.ID -BackupManagementType AzureWorkload -WorkloadType MSSQL -VaultId $targetVault.ID -Force
````

Komut bu kaynağın 'yedekleme kapsayıcısı' döndürür ve durumu 'kaydedilecek'

> [!NOTE]
> Force parametresini verilmezse, kullanıcı ile bir metin ', bu kapsayıcı için korumayı devre dışı bırakmak istiyor musunuz' onaylaması istenir. Lütfen bu metin yoksay ve onaylamak için "Y" söyleyin. Bu bilinen bir sorundur ve metin ve force parametresini gereksinimini kaldırmak için çalışıyoruz

### <a name="fetching-sql-dbs"></a>SQL DB'leri getiriliyor

Kayıt tamamlandıktan sonra Backup hizmeti VM'de tüm kullanılabilir SQL bileşenleri listelemek mümkün olacaktır. Tüm SQL bileşenleri görüntülemek için henüz bu kasayı kullanmak için yedeklenecek [Get-AzRecoveryServicesBackupProtectableItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupProtectableItem?view=azps-1.5.0) PS cmdlet'i

````powershell
Get-AzRecoveryServicesBackupProtectableItem -WorkloadType MSSQL -VaultId $targetVault.ID
````

Çıktı öğesi türü ve ServerName ile bu kasaya kayıtlı tüm SQL VM'ler arasında tüm korumasız SQL bileşenleri gösterir. Daha fazla geçirerek belirli bir SQL VM filtreleyebilirsiniz '-kapsayıcı ' parametresi veya Kullan 'Name' ve 'ServerName' ItemType birlikte birleşimi bayrak benzersiz bir SQL öğe ulaşması için.

````powershell
$SQLDB = Get-AzRecoveryServicesBackupProtectableItem -workloadType MSSQL -ItemType SQLDataBase -VaultId $targetVault.ID -Name "<Item Name>" -ServerName "<Server Name>"
````

### <a name="configuring-backup"></a>Yedeklemeyi yapılandırma

Gerekli SQL DB ve ilke ile sahip olduğumuz göre hangi BT yedeklenmesi gereken, kullanabiliriz [etkinleştir AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/Enable-AzRecoveryServicesBackupProtection?view=azps-1.5.0) bu SQL DB için yedeklemeyi yapılandırmak için cmdlet.

````powershell
Enable-AzRecoveryServicesBackupProtection -ProtectableItem $SQLDB -Policy $NewSQLPolicy
````

Yapılandırma Yedekleme tamamlandığında ve aşağıdaki çıktıyı döndürür kadar komutu bekler.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
master           ConfigureBackup      Completed            3/18/2019 6:00:21 PM      3/18/2019 6:01:35 PM      654e8aa2-4096-402b-b5a9-e5e71a496c4e
```

### <a name="fetching-new-sql-dbs"></a>Yeni SQL DB'leri getiriliyor

Makine kaydedildiğinde, Backup hizmeti ardından kullanılabilir DBs ayrıntılarını getirir. Yeni (yeni eklenen olanlar dahil) tüm korumasız veritabanlarını almak için ' sorgusu gerçekleştirmek için backup hizmeti el ile tetiklemek gerektiren kullanıcı SQL veritabanı/SQL örnekleri için kayıtlı makine daha sonra eklerse, yeniden. Kullanım [başlatma AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/Initialize-AzRecoveryServicesBackupProtectableItem?view=azps-1.5.0) PS cmdlet'i yeni bir sorgu gerçekleştirmek için SQL VM üzerinde. Komut işlemi tamamlanana kadar bekler. Daha sonra [Get-AzRecoveryServicesBackupProtectableItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupProtectableItem?view=azps-1.5.0) PS cmdlet'i son korumasız SQL bileşenleri listesini almak için

````powershell
$SQLContainer = Get-AzRecoveryServicesBackupContainer -ContainerType AzureVMAppContainer -FriendlyName <VM name> -VaultId $targetvault.ID
Initialize-AzRecoveryServicesBackupProtectableItem -Container $SQLContainer -WorkloadType MSSQL -VaultId $targetvault.ID
Get-AzRecoveryServicesBackupProtectableItem -workloadType MSSQL -ItemType SQLDataBase -VaultId $targetVault.ID
````

İlgili korunabilir öğeleri getirilen sonra belirtildiği gibi yedeklemeleri etkinleştirmek [bölümde yukarıdaki](#configuring-backup).
Bir el ile yeni bir DBs algılamak istememektedir, bunlar için autoprotection açıklandığı gibi seçebilirsiniz [aşağıda](#enable-autoprotection).

## <a name="enable-autoprotection"></a>AutoProtection etkinleştir

Bir kullanıcı, gelecekte eklediğiniz tüm veritabanları otomatik olarak belirli bir ilkeyle korunan gibi yedekleme yapılandırabilirsiniz. Autoprotection etkinleştirmek için [etkinleştir AzRecoveryServicesBackupAutoProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/Enable-AzRecoveryServicesBackupAutoProtection?view=azps-1.5.0) PS cmdlet'i.

Yönerge tüm gelecek Db'ler, işlemi sırasında bir Sqlınstance yapılır yedekleme düzeyi olduğundan.

```powershell
$SQLInstance = Get-AzRecoveryServicesBackupProtectableItem -workloadType MSSQL -ItemType SQLInstance -VaultId $targetVault.ID -Name "<Protectable Item name>" -ServerName "<Server Name>"
Enable-AzRecoveryServicesBackupAutoProtection -InputItem $SQLInstance -BackupManagementType AzureWorkload -WorkloadType MSSQL -Policy $targetPolicy -VaultId $targetvault.ID
```

Autoprotection amaç verildikten sonra yeni getirilecek makine araştırılması DBs zamanlanmış arka plan görevi olarak her 8 saatte bir gerçekleşir eklendi.

## <a name="restore-sql-dbs"></a>SQL veritabanlarını geri yükleme

Azure yedekleme gibi Azure Vm'lerinde çalışan SQL Server veritabanlarını geri yükleyebilirsiniz:

1. Belirli bir tarih veya saat (saniye için) işlem günlüğü yedeklemeleri kullanarak geri yükleyin. Azure yedekleme, otomatik olarak uygun tam değişiklik yedeği belirler ve geri yüklemek için gerekli günlük yedekleme zincirini seçili zamana dayalı.
2. Belirli bir kurtarma noktasına geri yüklemek için belirli bir tam veya değişiklik yedeklemesinden geri yükleyin.

Belirtilen önkoşulları denetleyin [burada](restore-sql-database-azure-vm.md#prerequisites) SQL veritabanlarını geri yüklemeden önce.

İlk ilgili SQL DB kullanarak desteklenen fetch [Get-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupItem?view=azps-1.5.0) PS cmdlet'i.

````powershell
$bkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureWorkload -WorkloadType MSSQL -Name "<backup item name>" -VaultId $targetVault.ID
````

### <a name="fetch-the-relevant-restore-time"></a>Getirme ilgili geri yükleme süresi

Yukarıda özetlendiği gibi kullanıcı yedeklenen SQL DB tam/değişiklik kopyasını geri yükleyebilirsiniz **veya** bir log-belirli bir noktaya için.

#### <a name="fetch-distinct-recovery-points"></a>Farklı bir kurtarma noktası getirilemedi

Kullanım [Get-AzRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupRecoveryPoint?view=azps-1.5.0) farklı (tam/değişiklik) kurtarma noktaları için yedeklenen SQL DB getirilemedi.

````powershell
$startDate = (Get-Date).AddDays(-7).ToUniversalTime()
$endDate = (Get-Date).ToUniversalTime()
Get-AzRecoveryServicesBackupRecoveryPoint -Item $bkpItem -VaultId $targetVault.ID -StartDate $startdate -EndDate $endDate
````

Aşağıdaki örneğe benzer bir çıkış

````powershell
RecoveryPointId    RecoveryPointType  RecoveryPointTime      ItemName                             BackupManagemen
                                                                                                  tType
---------------    -----------------  -----------------      --------                             ---------------
6660368097802      Full               3/18/2019 8:09:35 PM   MSSQLSERVER;model             AzureWorkload
````

İlgili kurtarma noktasını getirilecek 'Recoverypointıd' filtresi veya bir dizi filtresi kullanın.

````powershell
$FullRP = Get-AzRecoveryServicesBackupRecoveryPoint -Item $bkpItem -VaultId $targetVault.ID -RecoveryPointId "6660368097802"
````

#### <a name="fetch-point-in-time-recovery-point"></a>Belirli bir noktaya kurtarma noktası getirilemedi

Kullanıcının istediği bir belirli-belirli bir noktaya için DB geri kullanırsanız [Get-AzRecoveryServicesBackupRecoveryLogChain](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupRecoveryLogChain?view=azps-1.5.0) PS cmdlet'i. Cmdlet bu SQL yedekleme öğesi için başlangıç ve bitiş zamanlarını kesintisiz ve sürekli günlük zinciri temsil eden tarihler listesini döndürür. İstenen-belirli bir noktaya bu aralığı içinde olmalıdır.

```powershell
Get-AzRecoveryServicesBackupRecoveryLogChain -Item $bkpItem -Item -VaultId $targetVault.ID
```

Çıktı aşağıdaki örneğe benzer olacaktır.

````powershell
ItemName                       StartTime                      EndTime
--------                       ---------                      -------
SQLDataBase;MSSQLSERVER;azu... 3/18/2019 8:09:35 PM           3/19/2019 12:08:32 PM
````

Yukarıdaki çıktı, o kullanıcı her-belirli bir noktaya görüntülenen başlangıç ve bitiş zamanı arasında geri yükleyebilirsiniz anlamına gelir. Saatler UTC biçimindedir. Tüm-belirli bir noktaya yukarıda gösterilen aralığında PS içinde oluşturun.

> [!NOTE]
> Bir günlük olarak geri yükleme işlemi için seçilen noktaya, kullanıcı belirtmemeniz gerekir başlangıç noktası başka bir deyişle, kendisinden DB geri tam yedekleme. Azure Backup hizmeti halleder tüm kurtarma planı başka bir deyişle, hangi tam yedekleme seçmek için hangi günlük yedeklemeler vb. uygulamak için.

### <a name="determine-recovery-configuration"></a>Kurtarma yapılandırması

SQL DB geri yükleme durumunda, aşağıdaki geri yükleme senaryolarında desteklenir.

1. Yedeklenen SQL DB ile verileri başka bir kurtarma noktasından - OriginalWorkloadRestore geçersiz kılma
2. Aynı SQL örneğinde - AlternateWorkloadRestore yeni bir veritabanı olarak SQL veritabanı geri yükleme
3. Başka bir SQL VM - AlternateWorkloadRestore içinde başka bir SQL örneğinde yeni bir veritabanı olarak SQL veritabanı geri yükleme

Sonra ilgili kurtarma noktasını alma (ayrı veya günlük zaman içinde nokta), kullanın [Get-AzRecoveryServicesBackupWorkloadRecoveryConfig](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupWorkloadRecoveryConfig?view=azps-1.5.0) PS cmdlet'i istenen kurtarma planına göre kurtarma Yapılandırma nesnesini almak için.

#### <a name="original-workload-restore"></a>Özgün iş yükü geri yükleme

Yedeklenen DB'yi kurtarma noktasından verilerle geçersiz kılmak için yalnızca doğru bayrağı ve ilgili kurtarma noktasını aşağıdaki example(s) gösterildiği gibi belirtin.

##### <a name="original-restore-with-distinct-recovery-point"></a>Farklı bir kurtarma noktasıyla özgün geri yükleme

````powershell
$OverwriteWithFullConfig = Get-AzRecoveryServicesBackupWorkloadRecoveryConfig -RecoveryPoint $FullRP -OriginalWorkloadRestore -VaultId $targetVault.ID
````

##### <a name="original-restore-with-log-point-in-time"></a>Özgün log-belirli bir noktaya geri yükleme

```powershell
$OverwriteWithLogConfig = Get-AzRecoveryServicesBackupWorkloadRecoveryConfig -PointInTime $PointInTime -Item $bkpItem  -OriginalWorkloadRestore -VaultId $targetVault.ID
```

#### <a name="alternate-workload-restore"></a>Diğer iş yükü geri yükleme

> [!IMPORTANT]
> Yedeklenen SQL DB yalnızca başka bir Sqlınstance için yeni bir veritabanı olarak bir Azure VM'de 'Bu kasaya kayıtlı' geri yüklenebilir.

Hedef Sqlınstance başka bir Azure VM içinde yer alıyorsa, yukarıda belirtildiği gibi olduğundan emin olmak [bu kasaya](#registering-the-sql-vm) ve ilgili Sqlınstance bir korunabilir öğe görünür.

````powershell
$TargetInstance = Get-AzRecoveryServicesBackupProtectableItem -WorkloadType MSSQL -ItemType SQLInstance -Name "<SQLInstance Name>" -ServerName "<SQL VM name>" -VaultId $targetVault.ID
````

Ardından ilgili kurtarma noktası, hedef SQL örneği doğru bayrağıyla aşağıda gösterilen geçirmeniz yeterlidir.

##### <a name="alternate-restore-with-distinct-recovery-point"></a>Farklı bir kurtarma noktasıyla alternatif geri yükleme

````powershell
$AnotherInstanceWithFullConfig = Get-AzRecoveryServicesBackupWorkloadRecoveryConfig -RecoveryPoint $FullRP -TargetItem $TargetInstance -AlternateWorkloadRestore -VaultId $targetVault.ID
````

##### <a name="alternate-restore-with-log-point-in-time"></a>Diğer log-belirli bir noktaya geri yükleme

```powershell
$AnotherInstanceWithLogConfig = Get-AzRecoveryServicesBackupWorkloadRecoveryConfig -PointInTime $PointInTime -Item $bkpItem -AlternateWorkloadRestore -VaultId $targetVault.ID
```

Son kurtarma noktası yapılandırma nesnesi elde [Get-AzRecoveryServicesBackupWorkloadRecoveryConfig](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupWorkloadRecoveryConfig?view=azps-1.5.0) PS cmdlet'i geri yükleme için tüm bilgileri ve aşağıda gösterildiği gibi.

````powershell
TargetServer         : <SQL server name>
TargetInstance       : <Target Instance name>
RestoredDBName       : <Target Instance name>/azurebackup1_restored_3_19_2019_1850
OverwriteWLIfpresent : No
NoRecoveryMode       : Disabled
targetPhysicalPath   : {azurebackup1, azurebackup1_log}
ContainerId          : /Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/vmappcontainer;compute;computeRG;SQLVMName
SourceResourceId     : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/computeRG/VMAppContainer/SQLVMName
RestoreRequestType   : Alternate WL Restore
RecoveryPoint        : Microsoft.Azure.Commands.RecoveryServices.Backup.Cmdlets.Models.AzureWorkloadRecoveryPoint
PointInTime          : 1/1/0001 12:00:00 AM
````

Geri yüklenen veritabanı adı, OverwriteWLIfpresent NoRecoveryMode ve targetPhysicalPath alanları düzenleyebilirsiniz. Aşağıda gösterildiği gibi hedef dosya yolları için daha fazla bilgi edinin.

````powershell
$AnotherInstanceWithFullConfig.targetPhysicalPath

MappingType SourceLogicalName SourcePath                  TargetPath
----------- ----------------- ----------                  ----------
Data        azurebackup1      F:\Data\azurebackup1.mdf    F:\Data\azurebackup1_1553001753.mdf
Log         azurebackup1_log  F:\Log\azurebackup1_log.ldf F:\Log\azurebackup1_log_1553001753.ldf
````

Aşağıda gösterildiği gibi dize değerleri olarak ilgili PS özelliklerini ayarlayın.

````powershell
$AnotherInstanceWithFullConfig.OverwriteWLIfpresent = "Yes"
$AnotherInstanceWithFullConfig | fl

TargetServer         : <SQL server name>
TargetInstance       : <Target Instance name>
RestoredDBName       : <Target Instance name>/azurebackup1_restored_3_19_2019_1850
OverwriteWLIfpresent : Yes
NoRecoveryMode       : Disabled
targetPhysicalPath   : {azurebackup1, azurebackup1_log}
ContainerId          : /Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/vmappcontainer;compute;computeRG;SQLVMName
SourceResourceId     : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/computeRG/VMAppContainer/SQLVMName
RestoreRequestType   : Alternate WL Restore
RecoveryPoint        : Microsoft.Azure.Commands.RecoveryServices.Backup.Cmdlets.Models.AzureWorkloadRecoveryPoint
PointInTime          : 1/1/0001 12:00:00 AM
````

> [!IMPORTANT]
> Yapılandırma nesnesi üzerinde geri yükleme işlemi dayanacak beri tüm gerekli ve uygun değerleri nihai kurtarma yapılandırma nesnesi olduğundan emin olun.

### <a name="restore-with-relevant-configuration"></a>Geri yükleme ile ilgili yapılandırma

İlgili kurtarma yapılandırma nesnesi alındı ve doğrulandı sonra kullanın [geri yükleme-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/Restore-AzRecoveryServicesBackupItem?view=azps-1.5.0) PS cmdlet'i, geri yükleme işlemini başlatın.

````powershell
Restore-AzRecoveryServicesBackupItem -WLRecoveryConfig $AnotherInstanceWithLogConfig -VaultId $targetVault.ID
````

Geri yükleme işlemi, izlenecek bir işin döndürür.

````powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
MSSQLSERVER/m... Restore              InProgress           3/17/2019 10:02:45 AM                                3274xg2b-e4fg-5952-89b4-8cb566gc1748
````

## <a name="manage-sql-backups"></a>SQL yedeklemelerini yönetme

### <a name="on-demand-backup"></a>İsteğe bağlı yedekleme

Bir DB için yedekleme etkinleştirildikten sonra kullanıcı için DB kullanarak bir talep üzerine yedekleme de tetikleyebilirsiniz [yedekleme AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/Backup-AzRecoveryServicesBackupItem?view=azps-1.5.0) PS cmdlet'i. Aşağıdaki örnek bir SQL DB üzerinde tam yedekleme ile sıkıştırma etkin tetikler ve tam yedekleme için 60 gün tutulmalıdır.

````powershell
$bkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureWorkload -WorkloadType MSSQL -Name "<backup item name>" -VaultId $targetVault.ID
$endDate = (Get-Date).AddDays(60).ToUniversalTime()
Backup-AzRecoveryServicesBackupItem -Item $bkpItem -BackupType Full -EnableCompression -VaultId $targetVault.ID -ExpiryDateTimeUTC $endDate
````

İzlenecek bir işin geçici yedekleme komut döndürür.

````powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
MSSQLSERVER/m... Backup               InProgress           3/18/2019 8:41:27 PM                                2516bb1a-d3ef-4841-97a3-9ba455fb0637
````

Çıkış kaybolursa veya ilgili iş Kimliğini almak istiyorsanız [işlerin listesini alma](#track-azure-backup-jobs) Azure Backup'tan hizmet ve ardından ve ayrıntılarını izleyin.

### <a name="change-policy-for-backup-items"></a>Yedekleme öğeleri ilkesini değiştirme

Kullanıcı var olan bir ilkeyi değiştirmek veya yedekleme öğesi İlkesi İlke1 ' Policy2 için değiştirin. İlkeleri bir yedekleme öğesi için geçiş yapmak için yalnızca uygun ilke fetch öğeyi geri ve kullanabileceğiniz [etkinleştir AzRecoveryServices](https://docs.microsoft.com/powershell/module/az.recoveryservices/Enable-AzRecoveryServicesBackupProtection?view=azps-1.5.0) parametre olarak yedekleme öğesi ile komutu.

````powershell
$TargetPol1 = Get-AzRecoveryServicesBackupProtectionPolicy -Name <PolicyName>
$anotherBkpItem = Get-AzRecoveryServicesBackupItem -WorkloadType MSSQL -BackupManagementType AzureWorkload -Name "<BackupItemName>"
Enable-AzRecoveryServicesBackupProtection -Item $anotherBkpItem -Policy $TargetPol1
````

Yapılandırma Yedekleme tamamlandığında ve aşağıdaki çıktıyı döndürür kadar komutu bekler.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
master           ConfigureBackup      Completed            3/18/2019 8:00:21 PM      3/18/2019 8:02:16 PM      654e8aa2-4096-402b-b5a9-e5e71a496c4e
```

### <a name="re-register-sql-vms"></a>SQL Vm'leri yeniden kaydedin

> [!WARNING]
> Bu okuduğunuzdan emin olun [belge](backup-sql-server-azure-troubleshoot.md#re-registration-failures) Ver'i çalışmadan önce neden olur ve hatanın belirtileri anlamak için

SQL VM Ver'i tetiklemek için ilgili yedekleme kapsayıcısı getirmek ve kayıt cmdlet'e geçirin.

````powershell
$SQLContainer = Get-AzRecoveryServicesBackupContainer -ContainerType AzureVMAppContainer -FriendlyName <VM name> -VaultId $targetvault.ID
Register-AzRecoveryServicesBackupContainer -Container $SQLContainer -BackupManagementType AzureWorkload -WorkloadType MSSQL -VaultId $targetVault.ID
````

### <a name="stop-protection"></a>Korumayı Durdur

#### <a name="retain-data"></a>Verileri Tut

Kullanıcı korumasını durdurmak isterse kullanabilecekleri [devre dışı bırak AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/Disable-AzRecoveryServicesBackupProtection?view=azps-1.5.0) PS cmdlet'i. Bu zamanlanmış yedeklemeleri durdurur ancak kadar yukarı yedeklenen verileri artık sonsuza kadar korunur.

````powershell
$bkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureWorkload -WorkloadType MSSQL -Name "<backup item name>" -VaultId $targetVault.ID
Disable-AzRecoveryServicesBackupProtection -Item $bkpItem -VaultId $targetVault.ID
````

#### <a name="delete-backup-data"></a>Yedekleme verilerini silme

Yedekleme verileri kasaya tamamen kaldırmak için yalnızca Ekle '-RemoveRecoveryPoints bayrağı/anahtara ['koruma komutunu devre dışı bırak'](#retain-data).

````powershell
Disable-AzRecoveryServicesBackupProtection -Item $bkpItem -VaultId $targetVault.ID -RemoveRecoveryPoints
````

#### <a name="disable-auto-protection"></a>Otomatik korumayı devre dışı bırakın

Autoprotection bir Sqlınstance üzerinde yapılandırılan kullanıcı kullanarak devre dışı bırakabilirsiniz [devre dışı bırak AzRecoveryServicesBackupAutoProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/Disable-AzRecoveryServicesBackupAutoProtection?view=azps-1.5.0) PS cmdlet'i.

````powershell
$SQLInstance = Get-AzRecoveryServicesBackupProtectableItem -workloadType MSSQL -ItemType SQLInstance -VaultId $targetVault.ID -Name "<Protectable Item name>" -ServerName "<Server Name>"
Disable-AzRecoveryServicesBackupAutoProtection -InputItem $SQLInstance -BackupManagementType AzureWorkload -WorkloadType MSSQL -VaultId $targetvault.ID
````

#### <a name="unregister-sql-vm"></a>SQL VM kaydını sil

SQL server'ın tüm veritabanlarını [artık korumalı ve Hayır yedekleme verileri var olan](#delete-backup-data), kullanıcı bu kasadan SQL VM kaydını silin. Ancak bundan sonra kullanıcı, başka bir kasa için veritabanlarını koruyabilir. Kullanım [Unregister-AzRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/az.recoveryservices/Unregister-AzRecoveryServicesBackupContainer?view=azps-1.5.0) PS cmdlet'ini SQL VM kaydını silin.

````powershell
$SQLContainer = Get-AzRecoveryServicesBackupContainer -ContainerType AzureVMAppContainer -FriendlyName <VM name> -VaultId $targetvault.ID
 Unregister-AzRecoveryServicesBackupContainer -Container $SQLContainer -VaultId $targetvault.ID
````

### <a name="track-azure-backup-jobs"></a>Azure yedekleme işlerini izleme

Azure Backup yalnızca SQL yedekleme tetikleyen kullanıcı iş izleyen dikkat edin önemlidir. Zamanlanmış yedeklemeler (günlük yedeklerini dahil), portal/powershell görünmez. Ancak, zamanlanan işler başarısız, bir [yedekleme Uyarısı](backup-azure-monitoring-built-in-monitor.md#backup-alerts-in-recovery-services-vault) oluşturulur ve Portalı'nda gösterilmez. [Azure İzleyici](backup-azure-monitoring-use-azuremonitor.md) zamanlanan tüm işlerin ve diğer ilgili bilgileri izlemek için.

Kullanıcılar döndürülür JobId ile tetiklenen geçici/kullanıcı işlemlerini izleyebilirsiniz [çıkış](#on-demand-backup) , yedekleme gibi zaman uyumsuz işler. Kullanım [Get-AzRecoveryServicesBackupJobDetail](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupJobDetail) PS cmdlet'i iş ve ayrıntılarını izlemek için.

````powershell
 Get-AzRecoveryServicesBackupJobDetails -JobId 2516bb1a-d3ef-4841-97a3-9ba455fb0637 -VaultId $targetVault.ID
````

Azure Backup hizmetinden adhoc işlerini ve bunların durumlarını listesini almak için kullanın [Get-AzRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupJob?view=azps-1.5.0) PS cmdlet'i. Aşağıdaki örnek, tüm devam eden SQL işleri döndürür.

```powershell
Get-AzRecoveryServicesBackupJob -Status InProgress -BackupManagementType AzureWorkload
```

Devam eden işi iptal etmek için kullanın [Stop-AzRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/az.recoveryservices/Stop-AzRecoveryServicesBackupJob?view=azps-1.5.0) PS cmdlet'i.

## <a name="managing-sql-always-on-availability-groups"></a>SQL her zaman üzerinde kullanılabilirlik grupları yönetme

SQL Always On kullanılabilirlik grupları için mutlaka [tüm düğümleri kaydetme](#registering-the-sql-vm) kullanılabilirlik grubu (ağ). Tüm düğümler için kayıt tamamlandıktan sonra bir SQL kullanılabilirlik grubu nesnesi mantıksal olarak korunabilir öğeleri altında oluşturulur. SQL AG altındaki veritabanlarını 'Temel' listelenir. Düğümleri tek başına örneklerden gösterilir ve bunları altındaki Varsayılan SQL veritabanlarını da SQL veritabanları listelenir.

Örneğin, bir SQL AG olan iki düğüm varsayalım: SQL AG DB 'sql server 0' ve 'sql sunucu 1', 1. Varsa bu iki düğüm kaydedildikten sonra kullanıcı [korunabilir öğeleri listeler](#fetching-sql-dbs), aşağıdaki bileşenleri listeler

1. Bir SQL AG nesnesi - korunabilir öğe türü olarak SQLAvailabilityGroup
2. SQL veritabanı olarak SQL AG DB - korunabilir öğe türü
3. korunabilir öğe türü Sqlınstance SQL-server-0-
4. SQL-server-1 - korunabilir öğe türü Sqlınstance
5. Sql-server-0 - altındaki tüm varsayılan SQL veritabanı (master, model, msdb) korunabilir öğe türü SQL veritabanı
6. Hiçbir ' % s'varsayılan SQL veritabanı (master, model, msdb) sql server-1 altında-korunabilir öğe türü SQL veritabanı

SQL-server-0, sql server 1 de "AzureVMAppContainer" ne zaman listelenir [yedekleme kapsayıcıları listelenen](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupContainer?view=azps-1.5.0).

Yalnızca ilgili SQL veritabanına getirme [yedeklemeyi etkinleştirme](#configuring-backup) ve [geçici yedekleme](#on-demand-backup) ve [PS cmdlet'leri geri](#restore-sql-dbs) aynıdır.
