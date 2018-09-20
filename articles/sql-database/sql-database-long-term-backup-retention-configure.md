---
title: Azure SQL veritabanı uzun süreli yedek saklamayı yönetme | Microsoft Docs
description: Otomatik yedekleme SQL Azure depolamada depolamak ve ardından geri yükleme hakkında bilgi edinin
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 850467dff0a16cb2ac7cda44537406f0267711b4
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46366533"
---
# <a name="manage-azure-sql-database-long-term-backup-retention"></a>Azure SQL veritabanı uzun vadeli yedekleme bekletmeyi yönetme

Azure SQL veritabanı ile yapılandırabileceğiniz bir [uzun süreli yedek saklama](sql-database-long-term-retention.md) ilke otomatik olarak Azure blob depolama alanındaki yedeklemeler için 10 yıla kadar korumak için (LTR). Ardından, Azure portal veya PowerShell kullanarak bu yedekleri kullanarak bir veritabanını kurtarabilirsiniz.

## <a name="use-the-azure-portal-to-configure-long-term-retention-policies-and-restore-backups"></a>Uzun vadeli bekletme ilkelerini yapılandırma ve yedeklemeleri geri yüklemek için Azure portalını kullanın

Aşağıdaki bölümlerde, uzun süreli saklama yapılandırma, uzun vadeli bekletme yedeklemeleri görüntülemek ve uzun süreli saklama yedeklemesini geri yükleme için Azure portalını kullanmayı gösterir.

### <a name="configure-long-term-retention-policies"></a>Uzun vadeli bekletme ilkelerini yapılandırma

SQL veritabanı'na yapılandırabileceğiniz [otomatik yedekleri tutma](sql-database-long-term-retention.md) hizmet katmanınızın saklama süresinden daha uzun bir süre. 

1. Azure portalında SQL server'ınızı seçin ve ardından **yedekleri Yönet**. Üzerinde **ilkelerini yapılandırma** sekmesinde, onay kutusunu veya uzun süreli yedek saklama ilkeleri değiştirmek istediğiniz veritabanını seçin.

   ![yedeklemeleri bağlantısını yönetme](./media/sql-database-long-term-retention/ltr-configure-ltr.png)

2. İçinde **ilkelerini yapılandırma** bölmesinde, select if haftalık, aylık veya yıllık yedeklemeler korumak ve her saklama süresini belirtin. 

   ![ilkeleri yapılandırma](./media/sql-database-long-term-retention/ltr-configure-policies.png)

3. Tamamlandığında, tıklayın **Uygula**.

### <a name="view-backups-and-restore-from-a-backup-using-azure-portal"></a>Yedeklemeleri görüntülemek ve Azure portalını kullanarak bir yedekten geri yükleyin

LTR İlkesi ve bu yedeklerden geri yükleme ile belirli bir veritabanı saklanır yedeklemeleri görüntüleyin. 

1. Azure portalında SQL server'ınızı seçin ve ardından **yedekleri Yönet**. Üzerinde **kullanılabilir yedekler** sekmesinde, kullanılabilir yedekler görmek istediğiniz veritabanını seçin.

   ![veritabanı seçin](./media/sql-database-long-term-retention/ltr-available-backups-select-database.png)

3. İçinde **kullanılabilir yedekler** bölmesinde kullanılabilir yedekler gözden geçirin. 

   ![yedekleri görüntüleme](./media/sql-database-long-term-retention/ltr-available-backups.png)

4. Yedeklemeyi geri yüklemek istediğiniz seçin ve ardından yeni veritabanı adını belirtin.

   ![geri yükleme](./media/sql-database-long-term-retention/ltr-restore.png)

5. Tıklayın **Tamam** veritabanınızı Azure SQL depolama alanındaki bir yedekten yeni veritabanına geri yüklemek için.

6. Geri yükleme işinin durumunu görüntülemek için araç çubuğundaki bildirim simgesine tıklayın.

   ![geri yükleme işi ilerleme durumu](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. Geri yükleme işi tamamlandığında açın **SQL veritabanları** yeni geri yüklenen veritabanını görüntülemek için sayfa.

> [!NOTE]
> Buradan [var olan veritabanına kopyalamak için geri yüklenen veritabanından veri ayıklama veya var olan veritabanını silerek geri yüklenen veritabanının adını var olan veritabanının adıyla değiştirme](sql-database-recovery-using-backups.md#point-in-time-restore) gibi görevleri gerçekleştirmek için SQL Server Management Studio kullanarak geri yüklenen veritabanına bağlanabilirsiniz.
>

## <a name="use-powershell-to-configure-long-term-retention-policies-and-restore-backups"></a>Uzun vadeli bekletme ilkelerini yapılandırma ve yedeklemeleri geri yüklemek için PowerShell'i kullanma

Aşağıdaki bölümlerde, PowerShell'in uzun süreli yedek saklama yapılandırma, Azure SQL depolama ve Azure SQL depolama alanındaki bir yedekten geri yükleme yedekleri görüntülemek için nasıl kullanılacağı gösterilmektedir.

> [!IMPORTANT]
> LTR V2 API'si aşağıdaki PowerShell sürümlerinde desteklenir:
- [Azurerm.SQL'e 4.5.0](https://www.powershellgallery.com/packages/AzureRM.Sql/4.5.0) ya da daha yeni
- [AzureRM 6.1.0](https://www.powershellgallery.com/packages/AzureRM/6.1.0) ya da daha yeni
> 

### <a name="create-an-ltr-policy"></a>LTR ilkesi oluşturma

```powershell
# Get the SQL server 
# $subId = “{subscription-id}”
# $serverName = “{server-name}”
# $resourceGroup = “{resource-group-name}” 
# $dbName = ”{database-name}”

Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId $subId

# get the server
$server = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroup

# create LTR policy with WeeklyRetention = 12 weeks. MonthlyRetention and YearlyRetention = 0 by default.
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W 

# create LTR policy with WeeklyRetention = 12 weeks, YearlyRetetion = 5 years and WeekOfYear = 16 (week of April 15). MonthlyRetention = 0 by default.
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W -YearlyRetention P5Y -WeekOfYear 16
```

### <a name="view-ltr-policies"></a>LTR ilkeleri görüntüle
Bu örnekte bir sunucu içinde LTR ilkeleri listesinde gösterilmiştir

```powershell
# Get all LTR policies within a server
$ltrPolicies = Get-AzureRmSqlDatabase -ResourceGroupName Default-SQL-WestCentralUS -ServerName trgrie-ltr-server | Get-AzureRmSqlDatabaseLongTermRetentionPolicy -Current 

# Get the LTR policy of a specific database 
$ltrPolicies = Get-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName  -ResourceGroupName $resourceGroup -Current
```
### <a name="clear-an-ltr-policy"></a>LTR İlkesi Temizle
Bu örnek, bir veritabanı LTR İlkesi temizleme gösterir.

```powershell
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -RemovePolicy
```

### <a name="view-ltr-backups"></a>LTR yedekleri görüntüleme

Bu örnek, bir sunucu içinde LTR yedekleme listesinde gösterilmiştir. 

```powershell
# Get the list of all LTR backups in a specific Azure region 
# The backups are grouped by the logical database id.
# Within each group they are ordered by the timestamp, the earliest
# backup first.  
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -Location $server.Location 

# Get the list of LTR backups from the Azure region under 
# the named server. 
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -Location $server.Location -ServerName $serverName

# Get the LTR backups for a specific database from the Azure region under the named server 
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -Location $server.Location -ServerName $serverName -DatabaseName $dbName

# List LTR backups only from live databases (you have option to choose All/Live/Deleted)
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -Location $server.Location -DatabaseState Live

# Only list the latest LTR backup for each database 
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -Location $server.Location -ServerName $serverName -OnlyLatestPerDatabase
```

### <a name="delete-ltr-backups"></a>LTR yedeklerini silme

Bu örnek bir LTR silme yedeklemeleri listesinden yedekleme gösterir.

```powershell
# remove the earliest backup 
$ltrBackup = $ltrBackups[0]
Remove-AzureRmSqlDatabaseLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId
```

### <a name="restore-from-ltr-backups"></a>LTR yedeklerden geri yükleme
Bu örnek, bir LTR yedekten geri yükleme gösterilmektedir. Not: Bu arabirim değiştiremezsiniz, ancak kaynak kimliği parametresi artık LTR yedekleme kaynak kimliğini gerektirir. 

```powershell
# Restore LTR backup as an S3 database
Restore-AzureRmSqlDatabase -FromLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId -ServerName $serverName -ResourceGroupName $resourceGroup -TargetDatabaseName $dbName -ServiceObjectiveName S3
```

> [!NOTE]
> Buradan, görevleri gerçekleştirmek için SQL Server Management Studio kullanarak geri yüklenen veritabanına bağlanabilirsiniz, geri yüklenen veritabanından var olan veritabanına kopyalamak için veya var olan silmek için veri ayıklama gibi veritabanı ve geri yüklenen yeniden adlandır veritabanı için varolan bir veritabanı adı. Bkz: [noktaya geri yükleme noktası](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="next-steps"></a>Sonraki adımlar

- Hizmet tarafından oluşturulan otomatik yedekler hakkında bilgi edinmek için bkz. [otomatik yedekler](sql-database-automated-backups.md)
- Uzun süreli yedek saklama hakkında bilgi edinmek için bkz. [uzun süreli yedek saklama](sql-database-long-term-retention.md)
