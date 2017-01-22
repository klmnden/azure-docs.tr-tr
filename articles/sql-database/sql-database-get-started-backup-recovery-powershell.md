---
title: "Veri koruma ve kurtarma için Azure SQL veritabanlarında Azure PowerShell aracılığıyla yedekleme ve geri yükleme işlevlerini kullanmaya başlama | Microsoft Belgeleri"
description: "Bu öğretici PowerShell kullanarak otomatik yedeklerden belirli bir noktaya geri yükleme gerçekleştirme, otomatik yedekleri Azure Kurtarma Hizmetleri kasasında saklama ve Azure Kurtarma Hizmetleri kasasından geri yükleme gerçekleştirme konusunda bilgi vermektedir"
keywords: "sql veritabanı öğreticisi"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: tutorial
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: hero-article
ms.date: 12/19/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: 68a4ed7aad946dda644a0f085c48fd33f453e018
ms.openlocfilehash: 15d5cb803332133c8015a8ba23ca5751b8abc29a


---


# <a name="get-started-with-backup-and-restore-for-data-protection-and-recovery-using-powershell"></a>PowerShell Kullanarak Veri Koruma ve Kurtarma için Yedekleme ve Geri Yükleme İşlevlerini Kullanmaya Başlama

Bu kullanmaya başlama öğreticisinde, Azure PowerShell'i kullanarak şu işlemleri gerçekleştirmeyi öğreneceksiniz:

- Bir veritabanının var olan yedeklerini görüntüleme
- Bir veritabanını daha önceki bir noktaya geri yükleme
- Bir veritabanı yedekleme dosyasını Azure Kurtarma Hizmetleri kasasında uzun süreli saklamak üzere yapılandırma
- Azure Kurtarma Hizmetleri kasasındaki bir veritabanını geri yükleme

**Tahmini süre**: Bu öğreticinin tamamlanması yaklaşık 30 dakika alacaktır (önkoşulları karşıladığınız varsayılarak).


## <a name="prerequisites"></a>Ön koşullar

* Bir Azure hesabınız olmalıdır. [Ücretsiz bir Azure hesabı açabilir](/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abonelik avantajlarını etkinleştirebilirsiniz](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

* Abonelik sahibi veya katkıda bulunan rolü üyesi olan bir hesap kullanarak Azure'a bağlanmış olmanız gerekir. Rol tabanlı erişim denetimi (RBAC) ile ilgili daha fazla bilgi için bkz. [Azure portalında erişim yönetimini kullanmaya başlama](../active-directory/role-based-access-control-what-is.md).

* En son Azure PowerShell sürümünün yüklü ve çalışıyor olması gerekir. Ayrıntılı bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs).

* [Azure portalı ve SQL Server Management Studio aracılığıyla Azure SQL Veritabanı sunucularını, veritabanlarını ve güvenlik duvarı kurallarını kullanmaya başlama](sql-database-get-started.md) öğreticisini veya [PowerShell sürümünü](sql-database-get-started-powershell.md) tamamladınız. Tamamlamadıysanız, bu öğretici önkoşulunu tamamlayın veya devam etmeden önce [PowerShell sürümünün](sql-database-get-started-powershell.md) sonundaki PowerShell betiğini çalıştırın.

> [!TIP]
> Aynı görevleri, [Azure portalı](sql-database-get-started-backup-recovery.md) aracılığıyla kullanmaya başlama öğreticilerinde de gerçekleştirebilirsiniz.

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="view-the-oldest-restore-point-from-the-service-generated-backups-of-a-database"></a>Bir veritabanının hizmet tarafından oluşturulan yedeklerinden en eski geri yükleme noktasını görüntüleme

Öğreticinin bu bölümünde veritabanınızın [hizmet tarafından oluşturulan otomatik yedeklerinde](sql-database-automated-backups.md) en eski geri yükleme noktası hakkında bilgileri görüntüleyeceksiniz. 

Herhangi bir veritabanını en eski geri yükleme noktası ve son kullanılabilir yedek (geçerli saatten 6 dakika öncesi) arasında dilediğiniz bir noktaya geri yükleyebilirsiniz. 

Aşağıdaki kod parçacığında, geri yüklemek istediğiniz veritabanının en eski geri yükleme noktasını bulmak için [Get-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/get-azurermsqldatabase) cmdlet'i kullanılmaktadır. Saat UTC biçiminde döndürülür ancak aşağıdaki kod parçacıkları yerel saatle nasıl çalışacağınızı göstermektedir. Canlı bir veritabanının en son kullanılabilir geri yükleme noktası genelde altı dakika öncesidir. Bu nedenle en son geri yükleme noktası geçerli saatten altı dakika öncesi olacaktır. 

```
# Get available restore points

$resourceGroupName = "{resource-group-name}"
$serverName = "{server-name}"
$databaseName = "{database-name}"

$databaseToRestore = Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName

$earliestRestorePoint = $databaseToRestore.EarliestRestoreDate.ToLocalTime()
$latestRestorePoint = (Get-Date).AddMinutes(-6).ToLocalTime()

Write-Host "'$databaseName' on '$serverName' can be restored to any point-in-time between '$earliestRestorePoint' thru '$latestRestorePoint'"
```



## <a name="restore-a-database-to-a-previous-point-in-time"></a>Bir veritabanını daha önceki bir noktaya geri yükleme

Öğreticinin bu bölümünde veri tabanını belirli bir noktadan yeni bir veritabanına geri yükleyeceksiniz. 

>[!NOTE]
>Belirli bir noktaya geri yüklediğiniz hedef sunucuyu değiştiremezsiniz. Farklı bir sunucuya geri yüklemek için [Coğrafi Geri Yükleme](sql-database-disaster-recovery.md#recover-using-geo-restore) özelliğini kullanın. Ayrıca, [elastik veritabanı havuzuna](sql-database-elastic-jobs-overview.md) veya farklı bir fiyatlandırma katmanına istediğiniz zaman geri yükleme yapabilirsiniz. 

Aşağıdaki kod parçacığında, önceki kod parçacığında ayarladığımız $databaseToRestore öğesini geri yüklemek için [Restore-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/restore-azurermsqldatabase) cmdlet'i kullanılmaktadır. **-PointInTime** parametresi için UTC biçiminde şuna benzer bir zaman değeri gerekir: *12/09/2016 20:00:00*. Kod parçacığı yerel saate dönüştürme işlemini sizin yerinize yapar.

```
# Restore a database to a previous point in time

#$resourceGroupName = {resource-group-name}
#$serverName = {server-name}
$newRestoredDatabaseName = "{new-database-name}"
$localTimeToRestoreTo = "{12/9/2016 12:00:00 PM}" # change to a valid restore point for your database
$restorePointInTime = (Get-Date $localTimeToRestoreTo).ToUniversalTime()
$newDatabaseEdition = "Basic"
$newDatabaseServiceLevel = "Basic"

Write-Host "Restoring database '$databaseName' to its state at:"(Get-Date $restorePointInTime).ToLocalTime()", to a new database named '$newRestoredDatabaseName'."

$restoredDb = Restore-AzureRmSqlDatabase -FromPointInTimeBackup -PointInTime $restorePointInTime -ResourceGroupName $resourceGroupName `
 -ServerName $serverName -TargetDatabaseName $newRestoredDatabaseName -Edition $newDatabaseEdition -ServiceObjectiveName $newDatabaseServiceLevel `
 -ResourceId $databaseToRestore.ResourceID

$restoredDb
```

> [!NOTE]
> Buradan [var olan veritabanına kopyalamak için geri yüklenen veritabanından veri ayıklama veya var olan veritabanını silerek geri yüklenen veritabanının adını var olan veritabanının adıyla değiştirme](sql-database-recovery-using-backups.md#point-in-time-restore) gibi görevleri gerçekleştirmek için [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) kullanarak geri yüklenen veritabanına bağlanabilirsiniz.

## <a name="configure-long-term-retention-of-automated-backups-in-an-azure-recovery-services-vault"></a>Otomatik yedekleri Azure Kurtarma Hizmetleri kasasında uzun süreli saklamak üzere yapılandırma 

Öğreticinin bu bölümünde [Azure Kurtarma Hizmetleri kasasını yapılandırarak otomatik yedekleri](sql-database-long-term-retention.md) hizmet katmanınızın saklama süresinden daha uzun bir süre (en fazla 10 yıl) saklamayı öğreneceksiniz. 


> [!TIP]
> Uzun süreli saklama yedeklerini silmek için bkz. [Uzun süreli saklama yedeklerini silme](sql-database-long-term-retention-delete.md).


### <a name="create-a-recovery-services-vault"></a>Kurtarma hizmetleri kasası oluşturma

> [!IMPORTANT]
> Kasanın Azure SQL mantıksal sunucusuyla aynı bölgede olması ve mantıksal sunucuyla aynı kaynak grubunu kullanması gerekir.

Aşağıdaki kod parçacığında, yeni bir kurtarma hizmetleri kasası oluşturmak ve kasadaki yedekler için yedeklilik düzeyini ayarlamak için [New-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices/v2.3.0/new-azurermrecoveryservicesvault) ve [Set-AzureRmRecoveryServicesBackupProperties](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices/v2.3.0/set-azurermrecoveryservicesbackupproperties) cmdlet'leri kullanılmaktadır. 

```
# Create a recovery services vault

#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$serverLocation = (Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroupName).Location
$recoveryServiceVaultName = "{new-vault-name}"

$vault = New-AzureRmRecoveryServicesVault -Name $recoveryServiceVaultName -ResourceGroupName $ResourceGroupName -Location $serverLocation 
Set-AzureRmRecoveryServicesBackupProperties -BackupStorageRedundancy LocallyRedundant -Vault $vault
```

### <a name="set-your-server-to-use-the-recovery-vault-you-just-created-for-its-long-term-retention-backups"></a>Sunucunuzu uzun süreli saklanacak yedekleri için yeni oluşturduğunuz kurtarma kasasını kullanacak şekilde ayarlama

Aşağıdaki kod parçacığında, önceki adımda oluşturduğunuz kurtarma hizmetleri kasasını belirli bir Azure SQL sunucusuyla ilişkilendirmek için [Set-AzureRmSqlServerBackupLongTermRetentionVault](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/set-azurermsqlserverbackuplongtermretentionvault) cmdlet'i kullanılmaktadır.

```
# Set your server to use the vault to for long-term backup retention 

Set-AzureRmSqlServerBackupLongTermRetentionVault -ResourceGroupName $resourceGroupName -ServerName $serverName -ResourceId $vault.Id
```

### <a name="create-a-retention-policy"></a>Saklama ilkesi tanımlama

Saklama ilkesi, bir veritabanı yedeğinin saklanacağı süreyi belirlediğiniz yerdir. 

Aşağıdaki kod parçacığında, yeni ilke oluşturmak için şablon olarak kullandığımız varsayılan saklama ilkesini almak için [Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupretentionpolicyobject) cmdlet'ini kullanılmaktadır. Şablonumuzu, yedeği 2 yıl boyunca saklayacak şekilde ayarlayıp [New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/new-azurermrecoveryservicesbackupprotectionpolicy) cmdlet'ini çalıştırarak yeni ilkemizi oluşturabiliriz. 

Bazı cmdlet'ler için çalıştırmadan önce kasa bağlamını ayarlamanız gerekir ([Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices/v2.3.0/set-azurermrecoveryservicesvaultcontext)). Bu nedenle bu cmdlet'i ilgili birkaç kod parçacığında görebilirsiniz. İlke, kasanın bir parçası olduğundan bağlamı ayarladık. Her kasa için birden fazla saklama ilkesi oluşturabilir ve ardından belirli veritabanlarına istediğiniz ilkeyi uygulayabilirsiniz. 


```
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

Aşağıdaki kod parçacığında yeni ilkemizi belirli bir veritabanına uygulamak için [Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/set-azurermsqldatabasebackuplongtermretentionpolicy) cmdlet'i kullanılmaktadır.

```
# Enable long-term retention for a specific SQL database
$policyState = "enabled"
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -State $policyState -ResourceId $policy.Id
```

> [!IMPORTANT]
> Yapılandırma yapıldıktan sonra yedekler sonraki yedi gün içinde kasada görünür. Yedek, kasada göründükten sonra bu öğreticiye devam edin.

## <a name="view-backup-info-and-backups-in-long-term-retention"></a>Uzun süreli saklama kapsamındaki yedekleme bilgilerini ve yedekleri görüntüleme

Öğreticinin bu bölümünde [uzun süreli saklama](sql-database-long-term-retention.md) kapsamındaki veritabanı yedekleriniz hakkındaki bilgileri görüntüleyeceksiniz. 

Aşağıdaki kod parçacıklarında, kasadan bilgi sorgulaması yapmak için [Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupcontainer), [Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupitem) ve [Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackuprecoverypoint) cmdlet'leri kullanılmaktadır.

```
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


## <a name="restore-a-database-from-a-backup-in-long-term-backup-retention"></a>Bir veritabanını uzun süreli yedek saklama kapsamındaki bir yedekten geri yükleme

Öğreticinin bu bölümünde veritabanını Azure Kurtarma Hizmetleri kasasındaki bir yedekten yeni bir veritabanına geri yükleyeceksiniz.

Bu öğreticinin önceki bölümlerindeki belirli nokta kod parçacıkları gibi uzun süreli yedek saklama da [Restore-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/restore-azurermsqldatabase) cmdlet'ini kullanır ancak bu örnekte **-FromLongTermRetentionBackup** parametresini kullanmaktadır (önceki örneklerde kullanılan **-FromPointInTimeBackup** parametresinden farklı olarak).

```
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
> Buradan [var olan veritabanına kopyalamak için geri yüklenen veritabanından veri ayıklama veya var olan veritabanını silerek geri yüklenen veritabanının adını var olan veritabanının adıyla değiştirme](sql-database-recovery-using-backups.md#point-in-time-restore) gibi görevleri gerçekleştirmek için SQL Server Management Studio kullanarak geri yüklenen veritabanına bağlanabilirsiniz.

## <a name="complete-azure-powershell-script-to-restore-from-a-previous-point-in-time-and-to-configure-and-restore-from-long-term-backup-retention-using-powershell"></a>PowerShell kullanarak belirli bir noktadan geri yükleme ile uzun süreli yedek saklamayı yapılandırma ve geri yükleme için tam Azure PowerShell betiği

> [!NOTE]
> Bu betiğin sonunda en son yedekten geri yükleme girişiminde bulunduğunuzda kasada yedek yoksa *Null dizide dizine alınamaz* özel durumuyla karşılaşırsınız.

```
# Sign in to Azure and set the subscription to work with
########################################################

$subscriptionId = "{subscription-id}"

Add-AzureRmAccount
Set-AzureRmContext -SubscriptionId $subscriptionId


# User variables
################

$myResourceGroupName = "{resource-group-name}"
$myServerName = "{server-name}"
$myDatabaseToRestoreFromPointInTime = "{database-name}"
$myLocalTimeToRestoreTo = "{12/08/2016 16:00:00}" # change to a valid restore point for your database
$myNewDatabaseRestoredFromPointInTime = "{new-database-name}"

$myRecoveryServiceVaultName = "{vault-name}"
$myRetentionPolicyDurationType = "{duration-period}" # set to Years, Months, or Weeks
$myRetentionPolicyDuration = {2}
$myRetentionPolicyName = "{retention-policy-name}"
$myDatabaseToRestoreFromLTR = "{database-name}"
$myNewDatabaseRestoredFromLTR = "{new-database-name}"


# Get available restore points for a specific database
######################################################

$resourceGroupName = $myResourceGroupName
$serverName = $myServerName
$databaseName = $myDatabaseToRestoreFromPointInTime


$databaseToRestore = Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName

$earliestRestorePoint = $databaseToRestore.EarliestRestoreDate.ToLocalTime()
$latestRestorePoint = (Get-Date).AddMinutes(-6).ToLocalTime()

Write-Host "'$databaseName' on '$serverName' can be restored to any point-in-time between '$earliestRestorePoint' thru '$latestRestorePoint'"


# Restore a database to a previous point in time
################################################

$newRestoredDatabaseName = $myNewDatabaseRestoredFromPointInTime
$localTimeToRestoreTo = $myLocalTimeToRestoreTo
$restorePointInTime = (Get-Date $localTimeToRestoreTo).ToUniversalTime()
$newDatabaseEdition = "Basic"
$newDatabaseServiceLevel = "Basic"

Write-Host "Restoring database '$databaseName' to its state at:"(Get-Date $restorePointInTime).ToLocalTime()", to a new database named '$newRestoredDatabaseName'."

$restoredDb = Restore-AzureRmSqlDatabase -FromPointInTimeBackup -PointInTime $restorePointInTime -ResourceGroupName $resourceGroupName `
 -ServerName $serverName -TargetDatabaseName $newRestoredDatabaseName -Edition $newDatabaseEdition -ServiceObjectiveName $newDatabaseServiceLevel `
 -ResourceId $databaseToRestore.ResourceID

$restoredDb



# CONFIGURE LONG-TERM RETENTION

# Create a recovery services vault
##################################

$resourceGroupName = $myResourceGroupName
$serverName = $myServerName
$serverLocation = (Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroupName).Location
$recoveryServiceVaultName = $myRecoveryServiceVaultName

$vault = New-AzureRmRecoveryServicesVault -Name $recoveryServiceVaultName -ResourceGroupName $resourceGroupName -Location $serverLocation 
Set-AzureRmRecoveryServicesBackupProperties -BackupStorageRedundancy LocallyRedundant -Vault $vault
$vault

Set-AzureRmSqlServerBackupLongTermRetentionVault -ResourceGroupName $resourceGroupName -ServerName $serverName -ResourceId $vault.Id

# Retrieve the default retention policy for the AzureSQLDatabase workload type
##############################################################################

$retentionPolicy = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType AzureSQLDatabase

# Set the retention values
$retentionPolicy.RetentionDurationType = $myRetentionPolicyDurationType
$retentionPolicy.RetentionCount = $myRetentionPolicyDuration

$retentionPolicyName = $myRetentionPolicyName

# Set the vault context to the vault you are creating the policy for
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# Create the new policy
$policy = New-AzureRmRecoveryServicesBackupProtectionPolicy -name $retentionPolicyName -WorkloadType AzureSQLDatabase -retentionPolicy $retentionPolicy
$policy

$policyState = "enabled"
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -State $policyState -ResourceId $policy.Id


$databaseNeedingRestore = $myDatabaseToRestoreFromLTR

# Set the vault context to the vault we want to restore from
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# Get the container associated with your vault
$container = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureSQL -FriendlyName $vault.Name

# Get the long-term retention metadata associated with a specific database
$item = Get-AzureRmRecoveryServicesBackupItem -Container $container -WorkloadType AzureSQLDatabase -Name $databaseNeedingRestore


# Get all available backups for the previously indicated database
# Optionally, set the -StartDate and -EndDate parameters to return backups within a specific time period
$availableBackups = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $item
$availableBackups
###

# This command restores the most recent backup: $availableBackups[0]
$resourceGroupName = $myResourceGroupName
$serverName = $myServerName
$restoredDatabaseName = $myNewDatabaseRestoredFromLTR
$edition = "Basic"
$performanceLevel = "Basic"

$restoredDbFromLtr = Restore-AzureRmSqlDatabase -FromLongTermRetentionBackup -ResourceId $availableBackups[0].Id -ResourceGroupName $resourceGroupName `
 -ServerName $serverName -TargetDatabaseName $restoredDatabaseName -Edition $edition -ServiceObjectiveName $performanceLevel

$restoredDbFromLtr
```

## <a name="next-steps"></a>Sonraki adımlar

- Hizmet tarafından oluşturulan otomatik yedekler hakkında bilgi edinmek için bkz. [otomatik yedekler](sql-database-automated-backups.md)
- Uzun süreli yedek saklama hakkında bilgi edinmek için bkz. [uzun süreli yedek saklama](sql-database-long-term-retention.md)
- Yedekleri geri yükleme hakkında bilgi edinmek için bkz. [yedekten geri yükleme](sql-database-recovery-using-backups.md)


<!--HONumber=Dec16_HO4-->


