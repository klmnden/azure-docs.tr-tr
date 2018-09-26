---
title: Uzun süreli yedek saklama - Azure SQL veritabanı yapılandırma | Microsoft Docs
description: Otomatik yedekleri Azure kurtarma Hizmetleri kasasında saklama ve Azure kurtarma Hizmetleri kasasından geri yükleme hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 05/08/2018
ms.openlocfilehash: a9a3d696f1c503969b89795f8c6d86a77bd353e8
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47160733"
---
# <a name="configure-and-restore-from-azure-sql-database-long-term-backup-retention-using-azure-recovery-services-vault"></a>Yapılandırma ve Azure kurtarma Hizmetleri kasası kullanarak Azure SQL veritabanı uzun süreli yedek saklamadan geri yükleme

Azure kurtarma Hizmetleri kasası, Azure SQL veritabanı yedeklemelerini depolamak ve ardından Azure portal veya PowerShell kullanarak kasada korunan Yedekleme kullanarak bir veritabanını kurtarmak için yapılandırabilirsiniz.

> [!NOTE]
> Ekim 2016'daki uzun süreli yedek saklama Önizleme'nin ilk sürümünden bir parçası olarak, yedekleri Azure kurtarma Hizmetleri Kasası'nda depolanır. Bu güncelleştirme bu bağımlılığı kaldırır, ancak geriye dönük uyumluluk için 31 Mayıs 2018'e kadar özgün API desteklenir. Azure Hizmetleri Recovery kasasındaki yedekleri ile etkileşim kurmak gerekiyorsa bkz [Azure kurtarma Hizmetleri Kasası'nı kullanarak uzun süreli yedek saklama](sql-database-long-term-backup-retention-configure-vault.md). 


## <a name="azure-portal"></a>Azure portal

Aşağıdaki bölümlerde, Azure portalında Azure kurtarma Hizmetleri kasasını yapılandırarak, yedekleme kasası ve kasasından geri yükleme görüntülemek nasıl kullanılacağını gösterir.

### <a name="configure-the-vault-register-the-server-and-select-databases"></a>Kasa yapılandırma sunucusunu kaydetmek ve veritabanlarını seçin

Bir Azure kurtarma Hizmetleri kasasına yapılandırmak [otomatik yedekleri tutma](sql-database-long-term-retention.md) hizmet katmanınızın saklama süresinden daha uzun bir süre. 

1. Açık **SQL Server** sunucunuzun sayfası.

   ![SQL server sayfası](./media/sql-database-get-started-portal/sql-server-blade.png)

2. **Uzun süreli yedek saklama**'ya tıklayın.

   ![uzun süreli yedek saklama bağlantısı](./media/sql-database-get-started-backup-recovery/long-term-backup-retention-link.png)

3. Üzerinde **uzun süreli yedek saklama** sayfasında sunucunuz için gözden geçirin ve (, zaten yapmadıysanız veya bu özellik artık Önizleme aşamasında olduğu sürece) önizleme koşullarını kabul edin.

   ![önizleme koşullarını kabul edin](./media/sql-database-get-started-backup-recovery/accept-the-preview-terms.png)

4. Uzun süreli yedek saklamayı yapılandırmak için kılavuz o veritabanını seçin ve ardından **yapılandırma** araç.

   ![uzun süreli yedek saklama için veritabanı seçme](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

5. Üzerinde **yapılandırma** sayfasında **gerekli ayarları Yapılandır** altında **kurtarma Hizmetleri kasası**.

   ![kasayı yapılandırma bağlantısı](./media/sql-database-get-started-backup-recovery/configure-vault-link.png)

6. Üzerinde **kurtarma Hizmetleri kasası** sayfasında, varsa mevcut bir kasayı seçin. Aboneliğinize ait kurtarma hizmetleri kasası yoksa akıştan çıkın ve kurtarma hizmetleri kasası oluşturun.

   ![Kasa oluşturma bağlantısı](./media/sql-database-get-started-backup-recovery/create-new-vault-link.png)

7. Üzerinde **kurtarma Hizmetleri kasaları** sayfasında **Ekle**.

   ![Kasa ekleme bağlantısı](./media/sql-database-get-started-backup-recovery/add-new-vault-link.png)
   
8. Üzerinde **kurtarma Hizmetleri kasası** sayfasında, Kurtarma Hizmetleri kasası için geçerli bir ad sağlayın.

   ![yeni kasa adı](./media/sql-database-get-started-backup-recovery/new-vault-name.png)

9. Aboneliğinizi ve kaynak grubunu seçtikten sonra kasa için bir konum belirleyin. İşiniz bittiğinde **Oluştur**'a tıklayın.

   ![Kasa oluştur](./media/sql-database-get-started-backup-recovery/create-new-vault.png)

   > [!IMPORTANT]
   > Kasanın Azure SQL mantıksal sunucusuyla aynı bölgede olması ve mantıksal sunucuyla aynı kaynak grubunu kullanması gerekir.
   >

10. Yeni kasayı oluşturduktan sonra geri dönmek için gerekli adımları gerçekleştirerek **kurtarma Hizmetleri kasası** sayfası.

11. Üzerinde **kurtarma Hizmetleri kasası** sayfası, kasaya tıklayın ve ardından **seçin**.

   ![var olan kasayı seçme](./media/sql-database-get-started-backup-recovery/select-existing-vault.png)

12. Üzerinde **yapılandırma** sayfasında, yeni bir bekletme ilkesi için geçerli bir ad belirtin, varsayılan saklama ilkesini gerekli şekilde değiştirin ve ardından **Tamam**.

   ![saklama ilkesi tanımlama](./media/sql-database-get-started-backup-recovery/define-retention-policy.png)
   
   >[!NOTE]
   >Saklama ilkesi adları boşluk da dahil olmak üzere, bazı karakterler izin vermez.

13. Üzerinde **uzun süreli yedek saklama** sayfasında Veritabanınızı **Kaydet** ve ardından **Tamam** uzun süreli yedek saklama ilkesini seçilen tüm veritabanlarına uygulanacak.

   ![saklama ilkesi tanımlama](./media/sql-database-get-started-backup-recovery/save-retention-policy.png)

14. Yapılandırdığınız Azure Kurtarma Hizmetleri kasasında bu yeni ilkeyi kullanan uzun süreli yedek saklamayı etkinleştirmek için **Kaydet**'e tıklayın.

   ![saklama ilkesi tanımlama](./media/sql-database-get-started-backup-recovery/enable-long-term-retention.png)

> [!IMPORTANT]
> Yapılandırma yapıldıktan sonra yedekler sonraki yedi gün içinde kasada görünür. Bu öğreticiye devam etmek için yedeklerin kasada görünmesini bekleyin.
>

### <a name="view-backups-in-long-term-retention-using-azure-portal"></a>Azure portalını kullanarak uzun süreli saklama kapsamındaki yedekleri görüntüleme

Veritabanı Yedekleriniz hakkındaki bilgileri görüntüleyin [uzun süreli yedek saklama](sql-database-long-term-retention.md). 

1. Azure portalında Azure kurtarma Hizmetleri kasanız için veritabanı Yedekleriniz Aç (Git **tüm kaynakları** ve aboneliğinizin kaynak listesinden seçin) veritabanı Yedekleriniz tarafından kullanılan depolama miktarını görüntülemek için Kasa.

   ![kurtarma hizmetleri kasasını ve yedekleri görüntüleme](./media/sql-database-get-started-backup-recovery/view-recovery-services-vault-with-data.png)

2. Açık **SQL veritabanı** veritabanınıza sayfası.

   ![Yeni örnek veritabanı sayfası](./media/sql-database-get-started-portal/new-sample-db-blade.png)

3. Araç çubuğunda **Geri yükle**'ye tıklayın.

   ![geri yükleme araç çubuğu](./media/sql-database-get-started-backup-recovery/restore-toolbar.png)

4. Geri yükleme sayfasında tıklayın **uzun vadeli**.

5. Azure kasası yedeklemelerinin altında **Bir yedekleme seçin**'e tıklayarak uzun süreli saklama kapsamındaki kullanılabilir veritabanı yedeklerini görüntüleyebilirsiniz.

   ![kasadaki yedekler](./media/sql-database-get-started-backup-recovery/view-backups-in-vault.png)

### <a name="restore-a-database-from-a-backup-in-long-term-backup-retention-using-the-azure-portal"></a>Bir veritabanını Azure portalını kullanarak uzun süreli yedek saklama bir yedekten geri yükleme

Azure kurtarma Hizmetleri kasasındaki bir yedekten yeni bir veritabanı için veritabanı geri.

1. Üzerinde **Azure kasası yedeklemelerinin** sayfasında, yedekleme geri yükleyin ve ardından **seçin**.

   ![kasadaki bir yedeği seçme](./media/sql-database-get-started-backup-recovery/select-backup-in-vault.png)

2. **Veritabanı adı** metin kutusunda geri yüklenen veritabanı için bir ad girin.

   ![yeni veritabanı adı](./media/sql-database-get-started-backup-recovery/new-database-name.png)

3. Veritabanınızı kasadaki yedekten yeni veritabanına geri yüklemek için **Tamam**'a tıklayın.

4. Geri yükleme işinin durumunu görüntülemek için araç çubuğundaki bildirim simgesine tıklayın.

   ![kasadan geri yükleme işi ilerleme durumu](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. Geri yükleme işi tamamlandığında açın **SQL veritabanları** yeni geri yüklenen veritabanını görüntülemek için sayfa.

   ![kasadan geri yüklenen veritabanı](./media/sql-database-get-started-backup-recovery/restored-database-from-vault.png)

> [!NOTE]
> Buradan [var olan veritabanına kopyalamak için geri yüklenen veritabanından veri ayıklama veya var olan veritabanını silerek geri yüklenen veritabanının adını var olan veritabanının adıyla değiştirme](sql-database-recovery-using-backups.md#point-in-time-restore) gibi görevleri gerçekleştirmek için SQL Server Management Studio kullanarak geri yüklenen veritabanına bağlanabilirsiniz.
>

## <a name="powershell"></a>PowerShell

Aşağıdaki bölümlerde, PowerShell'in Azure kurtarma Hizmetleri kasasını yapılandırarak, yedekleme kasası ve kasasından geri yükleme görüntülemek için nasıl kullanılacağı gösterilmektedir.

### <a name="create-a-recovery-services-vault"></a>Kurtarma hizmetleri kasası oluşturma

Kullanım [New-AzureRmRecoveryServicesVault](/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault) bir kurtarma Hizmetleri kasası oluşturmak için.

> [!IMPORTANT]
> Kasanın Azure SQL mantıksal sunucusuyla aynı bölgede olması ve mantıksal sunucuyla aynı kaynak grubunu kullanması gerekir.

```PowerShell
# Create a recovery services vault

#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$serverLocation = (Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroupName).Location
$recoveryServiceVaultName = "{new-vault-name}"

$vault = New-AzureRmRecoveryServicesVault -Name $recoveryServiceVaultName -ResourceGroupName $ResourceGroupName -Location $serverLocation 
Set-AzureRmRecoveryServicesBackupProperties -BackupStorageRedundancy LocallyRedundant -Vault $vault
```

### <a name="set-your-server-to-use-the-recovery-vault-for-its-long-term-retention-backups"></a>Sunucunuzu, uzun süreli saklama yedeklerini için kurtarma kasasını kullanacak şekilde ayarlama

Kullanım [Set-AzureRmSqlServerBackupLongTermRetentionVault](/powershell/module/azurerm.sql/set-azurermsqlserverbackuplongtermretentionvault) daha önce oluşturulan kurtarma Hizmetleri kasası belirli bir Azure SQL sunucusuyla ilişkilendirmek için cmdlet'i.

```PowerShell
# Set your server to use the vault to for long-term backup retention 

Set-AzureRmSqlServerBackupLongTermRetentionVault -ResourceGroupName $resourceGroupName -ServerName $serverName -ResourceId $vault.Id
```

### <a name="create-a-retention-policy"></a>Saklama ilkesi tanımlama

Saklama ilkesi, bir veritabanı yedeğinin saklanacağı süreyi belirlediğiniz yerdir. Kullanma [Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupretentionpolicyobject) ilkeleri oluşturmak için şablon olarak kullanmak için varsayılan saklama ilkesini almak için cmdlet. Bu şablonda 2 yıl boyunca saklama dönemi ayarlanmadı. Ardından, çalıştırma [New-AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy) son ilkesi oluşturmak için. 

> [!NOTE]
> Bazı cmdlet'ler için çalıştırmadan önce kasa bağlamını ayarlamanız gerekir ([Set-AzureRmRecoveryServicesVaultContext](/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)) Bu cmdlet'i ilgili birkaç kod görürsünüz. İlke, kasanın bir parçası olduğundan bağlamı ayarlayın. Her kasa için birden fazla saklama ilkesi oluşturabilir ve ardından belirli veritabanlarına istediğiniz ilkeyi uygulayabilirsiniz. 


```PowerShell
# Retrieve the default retention policy for the AzureSQLDatabase workload type
$retentionPolicy = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType AzureSQLDatabase

# Set the retention value to two years (you can set to any time between 1 week and 10 years)
$retentionPolicy.RetentionDurationType = "Years"
$retentionPolicy.RetentionCount = 2
$retentionPolicyName = "my2YearRetentionPolicy"

# Set the vault context to the vault you are creating the policy for
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# Create the new policy
$policy = New-AzureRmRecoveryServicesBackupProtectionPolicy -name $retentionPolicyName -WorkloadType AzureSQLDatabase -retentionPolicy $retentionPolicy
$policy
```

### <a name="configure-a-database-to-use-the-previously-defined-retention-policy"></a>Bir veritabanını önceden tanımlanan saklama ilkesini kullanacak şekilde yapılandırma

Kullanım [Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy](/powershell/module/azurerm.sql/set-azurermsqldatabasebackuplongtermretentionpolicy) belirli bir veritabanına yeni ilkeyi uygulamak için cmdlet'i.

```PowerShell
# Enable long-term retention for a specific SQL database
$policyState = "enabled"
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -State $policyState -ResourceId $policy.Id
```

### <a name="view-backup-info-and-backups-in-long-term-retention"></a>Uzun süreli saklama kapsamındaki yedekleme bilgilerini ve yedekleri görüntüleme

Veritabanı Yedekleriniz hakkındaki bilgileri görüntüleyin [uzun süreli yedek saklama](sql-database-long-term-retention.md). 

Yedekleme bilgilerini görüntülemek için aşağıdaki cmdlet'leri kullanın:

- [Get-AzureRmRecoveryServicesBackupContainer](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)
- [Get-AzureRmRecoveryServicesBackupItem](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)
- [Get-AzureRmRecoveryServicesBackupRecoveryPoint](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)

```PowerShell
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$databaseNeedingRestore = $databaseName

# Set the vault context to the vault we want to restore from
#$vault = Get-AzureRmRecoveryServicesVault -ResourceGroupName $resourceGroupName
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# the following commands find the container associated with the server 'myserver' under resource group 'myresourcegroup'
$container = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureSQL -FriendlyName $vault.Name

# Get the long-term retention metadata associated with a specific database
$item = Get-AzureRmRecoveryServicesBackupItem -Container $container -WorkloadType AzureSQLDatabase -Name $databaseNeedingRestore

# Get all available backups for the previously indicated database
# Optionally, set the -StartDate and -EndDate parameters to return backups within a specific time period
$availableBackups = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $item
$availableBackups
```

### <a name="restore-a-database-from-a-backup-in-long-term-backup-retention"></a>Bir veritabanını uzun süreli yedek saklama kapsamındaki bir yedekten geri yükleme

Uzun süreli yedek saklama kullanan [Restore-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) cmdlet'i.

```PowerShell
# Restore the most recent backup: $availableBackups[0]
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$restoredDatabaseName = "{new-database-name}"
$edition = "Basic"
$performanceLevel = "Basic"

$restoredDb = Restore-AzureRmSqlDatabase -FromLongTermRetentionBackup -ResourceId $availableBackups[0].Id -ResourceGroupName $resourceGroupName `
 -ServerName $serverName -TargetDatabaseName $restoredDatabaseName -Edition $edition -ServiceObjectiveName $performanceLevel
$restoredDb
```


> [!NOTE]
> Buradan, görevleri gerçekleştirmek için SQL Server Management Studio kullanarak geri yüklenen veritabanına bağlanabilirsiniz, geri yüklenen veritabanından var olan veritabanına kopyalamak için veya var olan silmek için veri ayıklama gibi veritabanı ve geri yüklenen yeniden adlandır veritabanı için varolan bir veritabanı adı. Bkz: [noktaya geri yükleme noktası](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="how-to-cleanup-backups-in-recovery-services-vault"></a>Kurtarma Hizmetleri kasasındaki yedekleri temizleme nasıl

1 Temmuz 2018'den itibaren LTR V1 API kullanım dışıdır ve SQL veritabanı tarafından yönetilen LTR depolama kapsayıcıları için kurtarma hizmeti kasası, tüm mevcut yedeklemeler geçirilmiş olması gerekir. Artık özgün yedeklemeleri için ücretlendirilir emin olmak için kasaları geçişten sonra kaldırılmış. Ancak, kilit kasanızdaki yerleştirdiyseniz yedeklemeleri var. devam edecektir. Gereksiz ücretlerden kaçınmak için eski yedeklemeleri aşağıdaki betiği kullanarak kurtarma Hizmetleri kasasından el ile kaldırabilirsiniz. 

```PowerShell
<#
.EXAMPLE
    .\Drop-LtrV1Backup.ps1 -SubscriptionId “{vault_sub_id}” -ResourceGroup “{vault_resource_group}” -VaultName “{vault_name}” 
#>
[CmdletBinding()]
Param (
    [Parameter(Mandatory = $true, HelpMessage="The vault subscription ID")]
    $SubscriptionId,

    [Parameter(Mandatory = $true, HelpMessage="The vault resource group name")]
    $ResourceGroup,

    [Parameter(Mandatory = $true, HelpMessage="The vault name")]
    $VaultName
)

Login-AzureRmAccount

Select-AzureRmSubscription -SubscriptionId $SubscriptionId

$vaults = Get-AzureRmRecoveryServicesVault
$vault = $vaults | where { $_.Name -eq $VaultName }

Set-AzureRmRecoveryServicesVaultContext -Vault $vault

$containers = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureSQL

ForEach ($container in $containers)
{
   $canDeleteContainer = $true  
   $ItemCount = 0
   Write-Host "Working on container" $container.Name
   $items = Get-AzureRmRecoveryServicesBackupItem -container $container -WorkloadType AzureSQLDatabase
   ForEach ($item in $items)
   {
          write-host "Deleting item" $item.name
          Disable-AzureRmRecoveryServicesBackupProtection -RemoveRecoveryPoints -item $item -Force
   }

   Write-Host "Deleting container" $container.Name
   Unregister-AzureRmRecoveryServicesBackupContainer -Container $container
}
```

## <a name="next-steps"></a>Sonraki adımlar

- Hizmet tarafından oluşturulan otomatik yedekler hakkında bilgi edinmek için bkz. [otomatik yedekler](sql-database-automated-backups.md)
- Uzun süreli yedek saklama hakkında bilgi edinmek için bkz. [uzun süreli yedek saklama](sql-database-long-term-retention.md)
- Yedekleri geri yükleme hakkında bilgi edinmek için bkz. [yedekten geri yükleme](sql-database-recovery-using-backups.md)
