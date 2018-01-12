---
title: "PowerShell örnek - güncelleştirme SQL veri eşitleme eşitleme şema | Microsoft Docs"
description: "SQL veri eşitleme için eşitleme şemasını güncelleştirmek için azure PowerShell örnek betiği"
services: sql-database
documentationcenter: sql-database
author: jognanay
manager: craigg
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data, mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 01/10/2018
ms.author: jognanay
ms.reviewer: douglasl
ms.openlocfilehash: 66bf084f585b86979e6521321daf466c571de10c
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="use-powershell-to-update-the-sync-schema-in-an-existing-sync-group"></a>Varolan bir eşitleme grubu eşitleme şemasında güncelleştirmek için PowerShell kullanın

Bu PowerShell örnek varolan bir eşitleme grubu eşitleme şemasında güncelleştirir. Birden çok tablo eşitlerken, bu komut dosyası, eşitleme şemasını verimli bir şekilde güncelleştirmek için yardımcı olur.

Bu örnek kullanımını gösteren **UpdateSyncSchema** github'da kullanılabilir komut dosyası [UpdateSyncSchema.ps1](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-data-sync/UpdateSyncSchema.ps1).

SQL veri eşitleme genel bakış için bkz: [verileri Eşitle birden çok Bulut ve şirket içi veritabanları arasında Azure SQL veri eşitleme (Önizleme) ile](../sql-database-sync-data.md).
## <a name="prerequisites"></a>Önkoşullar

Bu örnek, Azure PowerShell modülü 4.2 veya sonraki bir sürümü gerektiriyor. Çalıştırma `Get-Module -ListAvailable AzureRM` yüklü olan sürümü bulunamıyor. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).
 
Çalıştırma `Login-AzureRmAccount` Azure ile bir bağlantı oluşturmak için.

## <a name="examples"></a>Örnekler

### <a name="example-1---add-all-tables-to-the-sync-schema"></a>Örnek 1 - eşitleme şemaya tüm tablolar ekleyin

Aşağıdaki örnek veritabanı şeması yeniler ve tüm geçerli tabloları eşitleme şemasına hub veritabanına ekler.

```powershell
UpdateSyncSchema.ps1 -SubscriptionId <subscription_id> -ResourceGroupName <resource_group_name> -ServerName <server_name> -DatabaseName <database_name> -SyncGroupName <sync_group_name> -RefreshDatabaseSchema $true -AddAllTables $true
```

### <a name="example-2---add-and-remove-tables-and-columns"></a>Örnek 2 - ekleme ve tabloları ve sütunları kaldırma

Aşağıdaki örnek, `[dbo].[Table1]` ve `[dbo].[Table2].[Column1]` kaldırır ve Eşitleme şeması için `[dbo].[Table3]`.

```powershell
UpdateSyncSchema.ps1 -SubscriptionId <subscription_id> -ResourceGroupName <resource_group_name> -ServerName <server_name> -DatabaseName <database_name> -SyncGroupName <sync_group_name> -TablesAndColumnsToAdd "[dbo].[Table1],[dbo].[Table2].[Column1]" -TablesAndColumnsToRemove "[dbo].[Table3]"
```

## <a name="script-parameters"></a>Komut dosyası parametreleri

**UpdateSyncSchema** betiği aşağıdaki parametreleri vardır:

| Parametre | Notlar |
|---|---|
| $SubscriptionId | Eşitleme grubunu oluşturulduğu abonelik. |
| $ResourceGroupName | Eşitleme grubunu oluşturulduğu kaynak grubu.|
| $ServerName | Hub veritabanı sunucu adı.|
| $DatabaseName | Hub veritabanı adı. |
| $SyncGroupName | Eşitleme grubu adı. |
| $MemberName | Veritabanı şeması eşitleme üye hub veritabanından yüklemek istiyorsanız, üye adı belirtin. Veritabanı şeması hub'dan yüklemek istiyorsanız, bu parametre boş bırakın. |
| $TimeoutInSeconds | Veritabanı şeması betik yeniler zaman aşımı oluştu. Varsayılan değer 900 saniyedir. |
| $RefreshDatabaseSchema | Komut dosyası veritabanı şeması yenilemek gerekip gerekmediğini belirtin. Yeni bir tablo eklediyseniz, veritabanı şemasını önceki yapılandırmasından - Örneğin, değişirse veya mı başlatacağı sütun), onu yeniden önce şema yenilemeniz gerekir. Varsayılan false olur. |
| $AddAllTables | Bu değeri true ise, tüm geçerli tablolar ve sütunlar eşitleme şemaya eklenir. $TablesAndColumnsToAdd ve $TablesAndColumnsToRemove değerlerini göz ardı edilir. |
| $TablesAndColumnsToAdd | Tablolar ve sütunlar eşitleme şemaya eklenecek belirtin. Her bir tablo veya sütun adı ile şema adı tam olarak ayrılmış gerekir. Örneğin: `[dbo].[Table1]`, `[dbo].[Table2].[Column1]`. Birden çok tablo veya sütun adları belirtilen ve virgülle (,) ayırarak. |
| $TablesAndColumnsToRemove | Tablolar ve sütunlar eşitleme şemadan kaldırılacak belirtin. Her bir tablo veya sütun adı şema adı ile tam olarak ayrılmış gerekir. Örneğin: `[dbo].[Table1]`, `[dbo].[Table2].[Column1]`. Birden çok tablo veya sütun adları belirtilen ve virgülle (,) ayırarak. |
|||

## <a name="script-explanation"></a>Komut dosyası açıklaması

**UpdateSyncSchema** komut dosyası aşağıdaki komutları kullanır. Komut özgü belgelere Tablo bağlantıları her komut.

| Komut | Notlar |
|---|---|
| [Get-AzureRmSqlSyncGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncgroup) | Eşitleme grubu hakkında bilgi döndürür. |
| [Güncelleştirme AzureRmSqlSyncGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/update-azurermsqlsyncgroup) | Eşitleme grubunu güncelleştirir. |
| [Get-AzureRmSqlSyncMember](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncmember) | Eşitleme üyesi hakkında bilgi döndürür. |
| [Get-AzureRmSqlSyncSchema](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncschema) | Eşitleme şeması hakkında bilgi döndürür. |
| [Güncelleştirme AzureRmSqlSyncSchema](https://docs.microsoft.com/powershell/module/azurerm.sql/update-azurermsqlsyncschema) | Bir eşitleme şema güncelleştirir. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Ek SQL veritabanı PowerShell Betiği örnekleri bulunabilir [Azure SQL veritabanı PowerShell komut dosyalarını](../sql-database-powershell-samples.md).

SQL veri eşitleme hakkında daha fazla bilgi için bkz:

-   [Eşitleme verilerle birden çok Bulut ve şirket içi veritabanları arasında Azure SQL veri eşitleme](../sql-database-sync-data.md)
-   [Azure SQL veri eşitleme ayarı](../sql-database-get-started-sql-data-sync.md)
-   [Azure SQL veri eşitleme için en iyi yöntemler](../sql-database-best-practices-data-sync.md)
-   [OMS günlük analizi ile İzleyici Azure SQL veri eşitleme](../sql-database-sync-monitor-oms.md)
-   [Azure SQL veri eşitleme ile ilgili sorunları giderme](../sql-database-troubleshoot-data-sync.md)

-   SQL veri eşitleme yapılandırmayı gösterir PowerShell örnekleri tamamlayın:
    -   [Birden çok Azure SQL veritabanları arasında eşitlemek için PowerShell kullanma](sql-database-sync-data-between-sql-databases.md)
    -   [Bir Azure SQL Database ve SQL Server içi veritabanı arasında eşitlemek için PowerShell kullanma](sql-database-sync-data-between-azure-onprem.md)

-   [SQL veri eşitleme REST API belgelerini indirebilirsiniz](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

SQL veritabanı hakkında daha fazla bilgi için bkz:

-   [SQL veritabanı genel bakış](../sql-database-technical-overview.md)
-   [Veritabanı yaşam döngüsü yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
