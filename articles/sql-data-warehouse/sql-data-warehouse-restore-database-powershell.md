---
title: "Bir Azure SQL veri ambarı (PowerShell) geri | Microsoft Docs"
description: "Azure SQL Data Warehouse geri yüklemek için PowerShell görevler."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: ac62f154-c8b0-4c33-9c42-f480808aa1d2
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 02/27/2018
ms.author: barbkess
ms.openlocfilehash: 533907ccbae5db3b68ede2ee1b226e663a04cb64
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="restore-an-azure-sql-data-warehouse-powershell"></a>Bir Azure SQL veri ambarı (PowerShell) geri yükleme
> [!div class="op_single_selector"]
> * [Genel bakış][Overview]
> * [Portal][Portal]
> * [PowerShell][PowerShell]
> * [REST][REST]
> 
> 

Bu makalede PowerShell kullanarak Azure SQL Data Warehouse geri yükleme öğreneceksiniz.

## <a name="before-you-begin"></a>Başlamadan önce
**DTU kapasitenizi doğrulayın.** Her SQL veri ambarı varsayılan DTU kota olan bir SQL server tarafından (örneğin myserver.database.windows.net) barındırılıyor.  SQL Data Warehouse geri yükleyebilmeniz için önce doğrulayın yeterli kalan DTU kota geri yüklenen veritabanı için SQL server'ınızdaki sahiptir. Gerekli DTU hesaplamak için ya da daha fazla DTU istemek için öğrenmek için bkz: [DTU kota değişiklik isteği][Request a DTU quota change].

### <a name="install-powershell"></a>PowerShell yükleme
SQL Data Warehouse ile Azure PowerShell kullanmak için Azure PowerShell 1.0 veya büyük bir sürümü yüklemeniz gerekir.  Çalıştırarak sürümünüzü kontrol edebilirsiniz **Get-Module - listavailable birlikte-adı AzureRM**.  En son sürümünü yüklenebilir [Microsoft Web Platformu yükleyicisi][Microsoft Web Platform Installer].  En son sürümü yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma][How to install and configure Azure PowerShell].

## <a name="restore-an-active-or-paused-database"></a>Etkin ya da duraklatılmış bir veritabanını geri yükle
Anlık görüntü kullanılmakta olan bir veritabanını geri yüklemek için [geri yükleme-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] PowerShell cmdlet'i.

1. Windows PowerShell'i açın.
2. Azure hesabınıza bağlanın ve hesabınızla ilişkili tüm abonelikleri listeler.
3. Geri yüklenecek veritabanını içeren aboneliği seçin.
4. Veritabanını geri yükleme noktaları listesi.
5. RestorePointCreationDate kullanarak istenen geri yükleme noktası seçin.
6. Veritabanını geri yüklemek için istenen geri yükleme noktası.
7. Geri yüklenen veritabanının çevrimiçi olduğunu doğrulayın.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# List the last 10 database restore points
((Get-AzureRMSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName)).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

> [!NOTE]
> Geri yükleme tamamlandıktan sonra izleyerek, kurtarılan veritabanının yapılandırabilirsiniz [kurtarma işleminden sonra veritabanını yapılandırma][Configure your database after recovery].
> 
> 

## <a name="restore-a-deleted-database"></a>Silinen veritabanını geri yükleme
Silinen bir veritabanını geri yüklemek için kullanmak [geri yükleme-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] cmdlet'i.

1. Windows PowerShell'i açın.
2. Azure hesabınıza bağlanın ve hesabınızla ilişkili tüm abonelikleri listeler.
3. Geri yüklenecek silinmiş veritabanını içeren aboneliği seçin.
4. Belirli silinen bu veritabanını alın.
5. Silinen bir veritabanını geri yükleyin.
6. Geri yüklenen veritabanının çevrimiçi olduğunu doğrulayın.

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

> [!NOTE]
> Geri yükleme tamamlandıktan sonra izleyerek, kurtarılan veritabanının yapılandırabilirsiniz [kurtarma işleminden sonra veritabanını yapılandırma][Configure your database after recovery].
> 
> 

## <a name="restore-from-an-azure-geographical-region"></a>Bir Azure coğrafi bölgesinden geri yükleme
Bir veritabanını kurtarmak için kullanmak [geri yükleme-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] cmdlet'i.

1. Windows PowerShell'i açın.
2. Azure hesabınıza bağlanın ve hesabınızla ilişkili tüm abonelikleri listeler.
3. Geri yüklenecek veritabanını içeren aboneliği seçin.
4. Kurtarmak istediğiniz veritabanı alın.
5. Veritabanı için kurtarma isteği oluşturun.
6. Coğrafi geri veritabanının durumunu doğrulayın.

```Powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

> [!NOTE]
> Geri yükleme tamamlandıktan sonra veritabanını yapılandırmak için bkz: [kurtarma işleminden sonra veritabanını yapılandırma][Configure your database after recovery].
> 
> 

Kaynak veritabanı TDE etkinse kurtarılmış veritabanını TDE etkin olacaktır.

## <a name="next-steps"></a>Sonraki adımlar
Azure SQL veritabanı sürümlerini iş sürekliliği özellikleri hakkında bilgi edinmek için lütfen okuyun [Azure SQL Database iş sürekliliğine genel bakış][Azure SQL Database business continuity overview].

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery

<!--MSDN references-->
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
