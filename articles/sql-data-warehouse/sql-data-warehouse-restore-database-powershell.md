---
title: Bir Azure SQL veri ambarı'nı (PowerShell) geri yükleme | Microsoft Docs
description: Bir Azure SQL veri ambarını geri yüklemek için PowerShell görevleri
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: fde06a9c7c0f0b225840c3b5a49d00fbfbcf33d9
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43306275"
---
# <a name="restore-an-azure-sql-data-warehouse-powershell"></a>Bir Azure SQL veri ambarı'nı (PowerShell) geri yükleme
> [!div class="op_single_selector"]
> * [Genel bakış][Overview]
> * [Portal][Portal]
> * [PowerShell][PowerShell]
> * [REST][REST]
> 
> 

Bu makalede, PowerShell kullanarak bir Azure SQL veri ambarı geri yükleme öğreneceksiniz.

## <a name="before-you-begin"></a>Başlamadan önce
**DTU kapasitenizi doğrulayın.** Her SQL veri ambarı varsayılan DTU kotası olan bir SQL server tarafından (örn. myserver.database.windows.net) barındırılır.  SQL veri ambarı geri yüklemeden önce olduğunu doğrulayın. SQL server'ınızı geri yüklenen veritabanı için yeterli kalan DTU kotası vardır. Gerekli DTU'yu hesaplama veya daha fazla DTU istemek için öğrenmek için bkz: [DTU kota değişiklik isteği][Request a DTU quota change].

### <a name="install-powershell"></a>PowerShell yükleme
SQL veri ambarı ile Azure PowerShell'i kullanmak için Azure PowerShell 1.0 veya üzeri bir sürümü yüklemeniz gerekir.  Çalıştırarak sürümünüzü kontrol edebilirsiniz **Get-Module - ListAvailable-Name AzureRM**.  En son sürümü yüklenebilir [Microsoft Web Platformu yükleyicisi][Microsoft Web Platform Installer].  En son sürümü yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma][How to install and configure Azure PowerShell].

## <a name="restore-an-active-or-paused-database"></a>Etkin ya da duraklatılmış bir veritabanını geri yükleme
Anlık görüntü kullanılmakta olan bir veritabanını geri yüklemek için [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] PowerShell cmdlet'i.

1. Windows PowerShell'i açın.
2. Azure hesabınıza bağlanın ve hesabınızla ilişkili tüm abonelikleri listeleyin.
3. Geri yüklenecek veritabanını içeren aboneliği seçin.
4. Veritabanı için geri yükleme noktalarını listeleyin.
5. RestorePointCreationDate kullanarak istenen bir geri yükleme noktası seçin.
6. Veritabanını geri yüklemek için istenen geri yükleme noktası.
7. Geri yüklenen veritabanının çevrimiçi olduğunu doğrulayın.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Connect-AzureRmAccount
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
> Geri yükleme tamamlandıktan sonra takip ederek, kurtarılan veritabanı yapılandırabilirsiniz [kurtarma işleminden sonra veritabanını yapılandırma][Configure your database after recovery].
> 
> 

## <a name="restore-a-deleted-database"></a>Silinen veritabanını geri yükleme
Silinen bir veritabanını geri yüklemek için kullanın [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] cmdlet'i.

1. Windows PowerShell'i açın.
2. Azure hesabınıza bağlanın ve hesabınızla ilişkili tüm abonelikleri listeleyin.
3. Silinen veritabanını geri içeren aboneliği seçin.
4. Belirli silinen veritabanını alır.
5. Silinen veritabanını geri yükleyin.
6. Geri yüklenen veritabanının çevrimiçi olduğunu doğrulayın.

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Connect-AzureRmAccount
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
> Geri yükleme tamamlandıktan sonra takip ederek, kurtarılan veritabanı yapılandırabilirsiniz [kurtarma işleminden sonra veritabanını yapılandırma][Configure your database after recovery].
> 
> 

## <a name="restore-from-an-azure-geographical-region"></a>Azure bir coğrafi bölgesinden geri yükleme
Bir veritabanını kurtarmak için kullanmak [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] cmdlet'i.

> [!NOTE]
> İşlem performans katmanı için en iyi duruma getirilmiş için coğrafi geri yükleme bir gerçekleştirebileceğiniz! Bunu yapmak için bir en iyi duruma getirilmiş işlem ServiceObjectiveName için isteğe bağlı bir parametre olarak belirtin. 
>
> 

1. Windows PowerShell'i açın.
2. Azure hesabınıza bağlanın ve hesabınızla ilişkili tüm abonelikleri listeleyin.
3. Geri yüklenecek veritabanını içeren aboneliği seçin.
4. Kurtarmak istediğiniz veritabanını alır.
5. Veritabanı için kurtarma isteği oluşturun.
6. Coğrafi geri veritabanının durumunu doğrulayın.

```Powershell
Connect-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID -ServiceObjectiveName "<YourTargetServiceLevel>"

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
[Restore-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
