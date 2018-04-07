---
title: Azure SQL veritabanı uzun vadeli yedekleme bekletme yönetme | Microsoft Docs
description: SQL Azure depolama alanına otomatik yedeklemeler depolar ve bunları geri yükleme hakkında bilgi edinin
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: article
ms.date: 04/04/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 29bfc914dd5c1f4c8b5405ff0e7202b767d032b8
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="manage-azure-sql-database-long-term-backup-retention"></a>Azure SQL veritabanı uzun vadeli yedekleme bekletme yönetme

Azure SQL veritabanı ile yapılandırabileceğiniz bir [uzun vadeli yedekleme bekletme](sql-database-long-term-retention.md) ilke otomatik olarak Azure blob depolama alanındaki yedekleri 10 yılı aşkın tutma (LTR). Ardından Azure portal veya PowerShell kullanarak bu yedeklemeler kullanarak bir veritabanını kurtarabilirsiniz.

> [!NOTE]
> Ekim 2016'deki bu özellik Önizleme ilk sürümünü bir parçası olarak, Azure kurtarma Hizmetleri kasasına yedekleme depolandı. Bu güncelleştirme bu bağımlılığı kaldırır, ancak geriye dönük uyumluluk için özgün API'si 31 May 2018 kadar desteklenir. Azure Hizmetleri kurtarma kasasında yedeklemeleri etkileşimde gerekiyorsa, bkz: [Azure kurtarma Hizmetleri kasası kullanılarak uzun vadeli yedekleme bekletme](sql-database-long-term-backup-retention-configure-vault.md). 

## <a name="use-the-azure-portal-to-configure-long-term-retention-policies-and-restore-backups"></a>Uzun vadeli bekletme ilkeleri yapılandırmak ve yedekleri geri yüklemek için Azure portalını kullanın

Aşağıdaki bölümlerde, uzun vadeli bekletme yapılandırmak, yedeklemeleri uzun vadeli bekletme içinde görüntülemek ve uzun vadeli bekletme yedeklemesini geri yükleme için Azure Portalı'nı kullanmayı gösterir.

### <a name="configure-long-term-retention-policies"></a>Uzun vadeli bekletme ilkeleri yapılandırma

SQL veritabanına yapılandırabilirsiniz [otomatik yedeklemelerini Beklet](sql-database-long-term-retention.md) hizmet katmanı için saklama süresinden daha uzun bir süre. 

1. Azure portalında, SQL server'ı ve ardından **uzun vadeli yedekleme bekletme**.

   ![uzun süreli yedek saklama bağlantısı](./media/sql-database-long-term-retention/ltr-configure-ltr.png)

2. Üzerinde **ilkeleri yapılandırma** sekmesinde, ayarlamak veya uzun vadeli yedekleme bekletme ilkeleri değiştirmek istediğiniz veritabanını seçin.

   ![Veritabanını seçin](./media/sql-database-long-term-retention/ltr-configure-select-database.png)

3. İçinde **ilkelerini yapılandırma** bölmesinde, select IF haftalık, aylık veya yıllık yedeklemelerini Beklet ve her biri için bekletme aralığını belirtin. 

   ![İlkeleri yapılandırma](./media/sql-database-long-term-retention/ltr-configure-policies.png)

4. Tamamlandığında, tıklayın **Uygula**.

### <a name="view-backups-and-restore-from-a-backup-using-azure-portal"></a>Yedeklemeleri görüntülemek ve Azure portalını kullanarak bir yedekten geri yükleyin

LTR İlkesi ve bu yedekleri geri yükleme ile belirli bir veritabanı için korunur yedeklemeleri görüntüleyin. 

1. Azure portalında, SQL server'ı ve ardından **uzun vadeli yedekleme bekletme**.

   ![uzun süreli yedek saklama bağlantısı](./media/sql-database-long-term-retention/ltr-configure-ltr.png)

2. Üzerinde **kullanılabilir yedeklemeleri** sekmesinde, kullanılabilir yedekleri görmek istediğiniz veritabanını seçin.

   ![Veritabanını seçin](./media/sql-database-long-term-retention/ltr-available-backups-select-database.png)

3. İçinde **kullanılabilir yedeklemeleri** bölmesinde, kullanılabilir yedekleri gözden geçirin. 

   ![Yedekleri görüntüle](./media/sql-database-long-term-retention/ltr-available-backups.png)

4. Geri yüklemek istediğiniz yedeği seçin ve ardından yeni bir veritabanı adı belirtin.

   ![geri yükleme](./media/sql-database-long-term-retention/ltr-restore.png)

5. Tıklatın **Tamam** yeni veritabanına veritabanınızı Azure SQL depolama yedekten geri yükleme.

6. Geri yükleme işinin durumunu görüntülemek için araç çubuğundaki bildirim simgesine tıklayın.

   ![kasadan geri yükleme işi ilerleme durumu](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. Geri yükleme işi tamamlandığında açmak **SQL veritabanları** yeni geri yüklenen veritabanı görüntülemek için sayfa.

> [!NOTE]
> Buradan [var olan veritabanına kopyalamak için geri yüklenen veritabanından veri ayıklama veya var olan veritabanını silerek geri yüklenen veritabanının adını var olan veritabanının adıyla değiştirme](sql-database-recovery-using-backups.md#point-in-time-restore) gibi görevleri gerçekleştirmek için SQL Server Management Studio kullanarak geri yüklenen veritabanına bağlanabilirsiniz.
>

## <a name="use-powershell-to-configure-long-term-retention-policies-and-restore-backups"></a>Uzun vadeli bekletme ilkeleri yapılandırmak ve yedekleri geri yüklemek için PowerShell kullanın

Aşağıdaki bölümlerde PowerShell uzun vadeli yedekleme bekletme yapılandırma, Azure SQL depolama ve Azure SQL depolama alanında bir yedekten yedeklemeleri görünümünde nasıl kullanılacağını gösterir.

### <a name="create-an-ltr-policy"></a>LTR ilkesi oluşturma

```powershell
# Get the SQL server 
# $subId = “{subscription-id}”
# $serverName = “{server-name}”
# $resourceGroup = “{resource-group-name}” 
# $dbName = ”{database-name}”

Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId $subId

# get the server
$server = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroup

# create LTR policy with WeeklyRetention = 12 weeks. MonthlyRetention and YearlyRetention = 0 by default.
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W 

# create LTR policy with WeeklyRetention = 12 weeks, YearlyRetetion = 5 years and WeekOfYear = 16 (week of April 15). MonthlyRetention = 0 by default.
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W -YearlyRetention P5Y -WeekOfYear 16
```

### <a name="view-ltr-policies"></a>Görünüm LTR ilkeleri
Bu örnek bir sunucu içindeki LTR ilkeleri listelemek nasıl gösterir

```powershell
# Get all LTR policies within a server
$ltrPolicies = Get-AzureRmSqlDatabase -ResourceGroupName Default-SQL-WestCentralUS -ServerName trgrie-ltr-server | Get-AzureRmSqlDatabaseLongTermRetentionPolicy -Current 

# Get the LTR policy of a specific database 
$ltrPolicies = Get-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName  -ResourceGroupName $resourceGroup -Current
```
### <a name="clear-an-ltr-policy"></a>LTR ilkeyi Temizle
Bu örnek bir veritabanı LTR ilkesinden temizlemek nasıl gösterir

```powershell
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -RemovePolicy
```

### <a name="view-ltr-backups"></a>LTR yedekleri görüntüle

Bu örnek, bir sunucu içindeki LTR yedeklerini Listele gösterilmektedir. 

```powershell
# Get the list of all LTR backups in a specific Azure region 
# The backups are grouped by the logical database id.
# Within each group they are ordered by the timestamp, the earliest
# backup first.  
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -LocationName $server.Location 

# Get the list of LTR backups from the Azure region under 
# the named server. 
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -LocationName $server.Location -ServerName $serverName

# Get the LTR backups for a specific database from the Azure region under the named server 
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -LocationName $server.Location -ServerName $serverName -DatabaseName $dbName

# List LTR backups only from live databases (you have option to choose All/Live/Deleted)
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -LocationName $server.Location -DatabaseState Live

# Only list the latest LTR backup for each database 
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -LocationName $server.Location -ServerName $serverName -OnlyLatestPerDatabase
```

### <a name="delete-ltr-backups"></a>LTR yedekleri Sil

Bu örnek bir LTR silmek nasıl yedekleri listesinden yedekleme gösterir.

```powershell
# remove the earliest backup 
$ltrBackup = $ltrBackups[0]
Remove-AzureRmSqlDatabaseLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId
```

### <a name="restore-from-ltr-backups"></a>LTR yedeklerden geri yükleme
Bu örnek bir LTR yedekten geri yükleme gösterir. Not: Bu arabirim değiştiremezsiniz, ancak kaynak kimliği parametresi artık LTR yedekleme kaynak kimliği gerekiyor. 

```powershell
# Restore LTR backup as an S3 database
Restore-AzureRmSqlDatabase -FromLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId -ServerName $serverName -ResourceGroupName $resourceGroup -TargetDatabaseName $dbName -ServiceObjectiveName S3
```

> [!NOTE]
> Buradan, gerekli görevleri gerçekleştirmek için SQL Server Management Studio'yu kullanarak geri yüklenen veritabanına bağlanmak, geri yüklenen veritabanından var olan veritabanına kopyalamak için veya varolan silmek için bir bit veri ayıklamak gibi veritabanı ve geri yüklenen yeniden adlandırın Varolan bir veritabanı adı veritabanı. Bkz: [geri yükleme noktası](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="next-steps"></a>Sonraki adımlar

- Hizmet tarafından oluşturulan otomatik yedekler hakkında bilgi edinmek için bkz. [otomatik yedekler](sql-database-automated-backups.md)
- Uzun süreli yedek saklama hakkında bilgi edinmek için bkz. [uzun süreli yedek saklama](sql-database-long-term-retention.md)
