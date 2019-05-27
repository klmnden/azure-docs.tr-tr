---
title: Azure SQL veritabanı uzun süreli yedek saklamayı yönetme | Microsoft Docs
description: Otomatik yedekleme SQL Azure depolamada depolamak ve ardından geri yükleme hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 04/17/2019
ms.openlocfilehash: 255f118d6dc6873364c2f8d4569e23c3e54ea83e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66164318"
---
# <a name="manage-azure-sql-database-long-term-backup-retention"></a>Azure SQL veritabanı uzun vadeli yedekleme bekletmeyi yönetme

Azure SQL veritabanı'nda tek veya havuza alınmış bir veritabanı ile yapılandırabileceğiniz bir [uzun süreli yedek saklama](sql-database-long-term-retention.md) ilke otomatik olarak Azure Blob Depolama alanında yedeklemeler için 10 yıla kadar korumak için (LTR). Ardından, Azure portal veya PowerShell kullanarak bu yedekleri kullanarak bir veritabanını kurtarabilirsiniz.

> [!IMPORTANT]
> [Azure SQL veritabanı yönetilen örneği](sql-database-managed-instance.md) uzun süreli yedek saklama şu anda desteklemiyor.

## <a name="use-the-azure-portal-to-configure-long-term-retention-policies-and-restore-backups"></a>Uzun vadeli bekletme ilkelerini yapılandırma ve yedeklemeleri geri yüklemek için Azure portalını kullanın

Aşağıdaki bölümlerde, uzun süreli saklama yapılandırma, uzun vadeli bekletme yedeklemeleri görüntülemek ve uzun süreli saklama yedeklemesini geri yükleme için Azure portalını kullanmayı gösterir.

### <a name="configure-long-term-retention-policies"></a>Uzun vadeli bekletme ilkelerini yapılandırma

SQL veritabanı'na yapılandırabileceğiniz [otomatik yedekleri tutma](sql-database-long-term-retention.md) hizmet katmanınızın saklama süresinden daha uzun bir süre. 

1. Azure portalında SQL server'ınızı seçin ve ardından **yedekleri Yönet**. Üzerinde **ilkelerini yapılandırma** sekmesinde *ayarlamak veya uzun süreli yedek saklama ilkeleri değiştirmek istediğiniz veritabanı için onay kutusunu işaretleyin*. Veritabanı yanındaki onay kutusunu seçili değilse, ilke için değişiklikler bu veritabanına uygulanmaz.  

   ![yedeklemeleri bağlantısını yönetme](./media/sql-database-long-term-retention/ltr-configure-ltr.png)

2. İçinde **ilkelerini yapılandırma** bölmesinde, select if haftalık, aylık veya yıllık yedeklemeler korumak ve her saklama süresini belirtin. 

   ![ilkeleri yapılandırma](./media/sql-database-long-term-retention/ltr-configure-policies.png)

3. Tamamlandığında, tıklayın **Uygula**.

> [!IMPORTANT]
> Bir uzun süreli yedek saklama ilkesini etkinleştirdiğinizde, bu ilk yedekleme görünür ve geri yüklemek kullanılabilir olmasını 7 güne kadar sürebilir. LTR yedekleme cadance ayrıntıları için bkz. [uzun süreli yedek saklama](sql-database-long-term-retention.md).

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

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

Aşağıdaki bölümlerde, PowerShell'in uzun süreli yedek saklama yapılandırma, Azure SQL depolama ve Azure SQL depolama alanındaki bir yedekten geri yükleme yedekleri görüntülemek için nasıl kullanılacağı gösterilmektedir.


### <a name="rbac-roles-to-manage-long-term-retention"></a>Uzun süreli saklama yönetmek için RBAC rolleri

LTR yedekleme yönetmek için olması gerekecektir 
- Abonelik sahibi veya
- SQL Server katkıda bulunan rolü **abonelik** kapsam veya
- SQL veritabanı katkıda bulunan rolü **abonelik** kapsamı

Daha ayrıntılı bir denetim gerekiyorsa, özel bir RBAC rolleri oluşturabilir ve bunları atama **abonelik** kapsam. 

İçin **Get-AzSqlDatabaseLongTermRetentionBackup** ve **geri yükleme-AzSqlDatabase** rolü aşağıdaki izinleri olması gerekir:

Microsoft.Sql/locations/longTermRetentionBackups/read Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionBackups/read Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/ longTermRetentionBackups/okuma
 
İçin **Remove-AzSqlDatabaseLongTermRetentionBackup** rolü aşağıdaki izinleri olması gerekir:

Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/delete


### <a name="create-an-ltr-policy"></a>LTR ilkesi oluşturma

```powershell
# Get the SQL server 
# $subId = “{subscription-id}”
# $serverName = “{server-name}”
# $resourceGroup = “{resource-group-name}” 
# $dbName = ”{database-name}”

Connect-AzAccount
Select-AzSubscription -SubscriptionId $subId

# get the server
$server = Get-AzSqlServer -ServerName $serverName -ResourceGroupName $resourceGroup

# create LTR policy with WeeklyRetention = 12 weeks. MonthlyRetention and YearlyRetention = 0 by default.
Set-AzSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W 

# create LTR policy with WeeklyRetention = 12 weeks, YearlyRetention = 5 years and WeekOfYear = 16 (week of April 15). MonthlyRetention = 0 by default.
Set-AzSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W -YearlyRetention P5Y -WeekOfYear 16
```

### <a name="view-ltr-policies"></a>LTR ilkeleri görüntüle
Bu örnekte bir sunucu içinde LTR ilkeleri listesinde gösterilmiştir

```powershell
# Get all LTR policies within a server
$ltrPolicies = Get-AzSqlDatabase -ResourceGroupName Default-SQL-WestCentralUS -ServerName trgrie-ltr-server | Get-AzSqlDatabaseLongTermRetentionPolicy -Current 

# Get the LTR policy of a specific database 
$ltrPolicies = Get-AzSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName  -ResourceGroupName $resourceGroup -Current
```
### <a name="clear-an-ltr-policy"></a>LTR İlkesi Temizle
Bu örnek, bir veritabanı LTR İlkesi temizleme gösterir.

```powershell
Set-AzSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -RemovePolicy
```

### <a name="view-ltr-backups"></a>LTR yedekleri görüntüleme

Bu örnek, bir sunucu içinde LTR yedekleme listesinde gösterilmiştir. 

```powershell
# Get the list of all LTR backups in a specific Azure region 
# The backups are grouped by the logical database id.
# Within each group they are ordered by the timestamp, the earliest
# backup first.  
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location 

# Get the list of LTR backups from the Azure region under 
# the named server. 
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location -ServerName $serverName

# Get the LTR backups for a specific database from the Azure region under the named server 
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location -ServerName $serverName -DatabaseName $dbName

# List LTR backups only from live databases (you have option to choose All/Live/Deleted)
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location -DatabaseState Live

# Only list the latest LTR backup for each database 
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location -ServerName $serverName -OnlyLatestPerDatabase
```

### <a name="delete-ltr-backups"></a>LTR yedeklerini silme

Bu örnek bir LTR silme yedeklemeleri listesinden yedekleme gösterir.

```powershell
# remove the earliest backup 
$ltrBackup = $ltrBackups[0]
Remove-AzSqlDatabaseLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId
```
> [!IMPORTANT]
> Silme LTR yedekleme çevrilemez. Her silme hakkında bildirimler Azure İzleyici'de işlem için filtreleyerek 'uzun süreli bekletme yedeklemesi siler' ayarlayabilirsiniz. Etkinlik günlüğü istek yapıldığında ve kimin hakkında bilgi içerir. Bkz: [etkinlik günlüğü uyarıları oluşturma](../azure-monitor/platform/alerts-activity-log.md) ayrıntılı yönergeler için.
>

### <a name="restore-from-ltr-backups"></a>LTR yedeklerden geri yükleme
Bu örnek, bir LTR yedekten geri yükleme gösterilmektedir. Not: Bu arabirim değiştiremezsiniz, ancak kaynak kimliği parametresi artık LTR yedekleme kaynak kimliğini gerektirir. 

```powershell
# Restore LTR backup as an S3 database
Restore-AzSqlDatabase -FromLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId -ServerName $serverName -ResourceGroupName $resourceGroup -TargetDatabaseName $dbName -ServiceObjectiveName S3
```

> [!NOTE]
> Buradan, görevleri gerçekleştirmek için SQL Server Management Studio kullanarak geri yüklenen veritabanına bağlanabilirsiniz, geri yüklenen veritabanından var olan veritabanına kopyalamak için veya var olan silmek için veri ayıklama gibi veritabanı ve geri yüklenen yeniden adlandır veritabanı için varolan bir veritabanı adı. Bkz: [noktaya geri yükleme noktası](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="next-steps"></a>Sonraki adımlar

- Hizmet tarafından oluşturulan otomatik yedekler hakkında bilgi edinmek için bkz. [otomatik yedekler](sql-database-automated-backups.md)
- Uzun süreli yedek saklama hakkında bilgi edinmek için bkz. [uzun süreli yedek saklama](sql-database-long-term-retention.md)
