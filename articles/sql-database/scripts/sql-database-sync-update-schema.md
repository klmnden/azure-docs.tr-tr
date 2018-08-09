---
title: PowerShell örneği - SQL Data Sync eşitleme şemasını güncelleştirme | Microsoft Docs
description: SQL Data Sync için eşitleme şemasını güncelleştirmek için Azure PowerShell örnek betiği
services: sql-database
documentationcenter: sql-database
author: allenwux
manager: craigg
editor: ''
tags: ''
ms.assetid: ''
ms.service: sql-database
ms.custom: load & move data, mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 01/10/2018
ms.author: xiwu
ms.reviewer: douglasl
ms.openlocfilehash: c121204e054618eef1435e64c28d959c0fe50ea9
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39617913"
---
# <a name="use-powershell-to-update-the-sync-schema-in-an-existing-sync-group"></a>Mevcut bir eşitleme grubunda eşitleme şemasını güncelleştirmek için PowerShell kullanma

Bu PowerShell örneği, mevcut bir SQL Data Sync eşitleme grubundaki eşitleme şemasını güncelleştirir. Birden çok tabloyu eşitlerken bu betik, eşitleme şemasını verimli bir şekilde güncelleştirmenize yardımcı olur.

Bu örnek, GitHub’da [UpdateSyncSchema.ps1](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-data-sync/UpdateSyncSchema.ps1) olarak kullanılabilir olan **UpdateSyncSchema** betiğinin kullanımını göstermektedir.

SQL Data Sync hizmetine genel bakış için bkz. [Azure SQL Data Sync ile birden fazla bulut ve şirket içi veritabanı arasında veri eşitleme](../sql-database-sync-data.md).
## <a name="prerequisites"></a>Ön koşullar

Bu örnek için Azure PowerShell modülünün 4.2 veya daha sonraki bir sürümü gerekir. Yüklü sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).
 
Azure ile bağlantı oluşturmak için `Connect-AzureRmAccount` komutunu çalıştırın.

## <a name="examples"></a>Örnekler

### <a name="example-1---add-all-tables-to-the-sync-schema"></a>Örnek 1 - Eşitleme şemasına tüm tabloları ekleme

Aşağıdaki örnek, veritabanı şemasını yeniler ve eşitleme şemasının hub veritabanındaki tüm geçerli tabloları ekler.

```powershell
UpdateSyncSchema.ps1 -SubscriptionId <subscription_id> -ResourceGroupName <resource_group_name> -ServerName <server_name> -DatabaseName <database_name> -SyncGroupName <sync_group_name> -RefreshDatabaseSchema $true -AddAllTables $true
```

### <a name="example-2---add-and-remove-tables-and-columns"></a>Örnek 2 - Tabloları ve sütunları ekleme ve kaldırma

Aşağıdaki örnek, eşitleme şemasına `[dbo].[Table1]` ve `[dbo].[Table2].[Column1]` ekler ve `[dbo].[Table3]` öğesini kaldırır.

```powershell
UpdateSyncSchema.ps1 -SubscriptionId <subscription_id> -ResourceGroupName <resource_group_name> -ServerName <server_name> -DatabaseName <database_name> -SyncGroupName <sync_group_name> -TablesAndColumnsToAdd "[dbo].[Table1],[dbo].[Table2].[Column1]" -TablesAndColumnsToRemove "[dbo].[Table3]"
```

## <a name="script-parameters"></a>Betik parametreleri

**UpdateSyncSchema** betiği aşağıdaki parametrelere sahiptir:

| Parametre | Notlar |
|---|---|
| $SubscriptionId | Eşitleme grubunun oluşturulduğu abonelik. |
| $ResourceGroupName | Eşitleme grubunun oluşturulduğu kaynak grubu.|
| $ServerName | Hub veritabanının sunucu adı.|
| $DatabaseName | Hub veritabanı adı. |
| $SyncGroupName | Eşitleme grubu adı. |
| $MemberName | Hub veritabanı yerine, eşitleme üyesinden veritabanı şemasını yüklemek istiyorsanız üye adını belirtin. Hub’dan veritabanı şemasını yüklemek istiyorsanız bu parametreyi boş bırakın. |
| $TimeoutInSeconds | Betik, veritabanı şemasını yenilediğinde gerçekleşen zaman aşımı. Varsayılan değer 900 saniyedir. |
| $RefreshDatabaseSchema | Betiğin, veritabanı şemasını yenilemesi gerekip gerekmediğini belirtin. Önceki yapılandırmadan veritabanı şemanız değiştiyse (örneğin, yeni bir tablo veya yeni bir sütun eklediyseniz), yeniden yapılandırmadan önce şemayı yenilemeniz gerekir. Varsayılan değer false’tur. |
| $AddAllTables | Bu değer true olursa, tüm geçerli tablolar ve sütunlar eşitleme şemasına eklenir. $TablesAndColumnsToAdd ve $TablesAndColumnsToRemove değerleri yoksayılır. |
| $TablesAndColumnsToAdd | Eşitleme şemasına eklenecek tabloları veya sütunları belirtin. Her bir tablo veya sütun adının şema adıyla tam olarak ayrılmış olması gerekir. Örneğin: `[dbo].[Table1]`, `[dbo].[Table2].[Column1]`. Birden çok tablo veya sütun adı belirtilebilir ve virgül (,) ile ayrılabilir. |
| $TablesAndColumnsToRemove | Eşitleme şemasından kaldırılacak tabloları veya sütunları belirtin. Her bir tablo veya sütun adının şema adıyla tam olarak ayrılmış olması gerekir. Örneğin: `[dbo].[Table1]`, `[dbo].[Table2].[Column1]`. Birden çok tablo veya sütun adı belirtilebilir ve virgül (,) ile ayrılabilir. |
|||

## <a name="script-explanation"></a>Betik açıklaması

**UpdateSyncSchema** betiği aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzureRmSqlSyncGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncgroup) | Bir eşitleme grubu hakkındaki bilgileri döndürür. |
| [Update-AzureRmSqlSyncGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/update-azurermsqlsyncgroup) | Bir eşitleme grubunu güncelleştirir. |
| [Get-AzureRmSqlSyncMember](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncmember) | Bir eşitleme üyesi hakkındaki bilgileri döndürür. |
| [Get-AzureRmSqlSyncSchema](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncschema) | Bir eşitleme şeması hakkındaki bilgileri döndürür. |
| [Update-AzureRmSqlSyncSchema](https://docs.microsoft.com/powershell/module/azurerm.sql/update-azurermsqlsyncschema) | Bir eşitleme şemasını güncelleştirir. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek SQL Veritabanı PowerShell betiği örnekleri [Azure SQL Veritabanı PowerShell betikleri](../sql-database-powershell-samples.md) içinde bulunabilir.

SQL Data Sync hakkında daha fazla bilgi için bkz.:

-   [Azure SQL Data Sync ile birden fazla bulut ve şirket içi veritabanı arasında veri eşitleme](../sql-database-sync-data.md)
-   [Azure SQL Data Sync’i ayarlama](../sql-database-get-started-sql-data-sync.md)
-   [Azure SQL Data Sync için en iyi yöntemler](../sql-database-best-practices-data-sync.md)
-   [Azure SQL Data Sync’i Log Analytics ile izleme](../sql-database-sync-monitor-oms.md)
-   [Azure SQL Data Sync ile ilgili sorun giderme](../sql-database-troubleshoot-data-sync.md)

-   SQL Data Sync’in nasıl yapılandırılacağını gösteren tam PowerShell örnekleri:
    -   [PowerShell kullanarak birden çok Azure SQL veritabanı arasında eşitleme](sql-database-sync-data-between-sql-databases.md)
    -   [PowerShell kullanarak bir Azure SQL Veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme](sql-database-sync-data-between-azure-onprem.md)

SQL Veritabanı hakkında daha fazla bilgi için bkz.:

-   [SQL Veritabanı'na Genel Bakış](../sql-database-technical-overview.md)
-   [Veritabanı Yaşam Döngüsü Yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
