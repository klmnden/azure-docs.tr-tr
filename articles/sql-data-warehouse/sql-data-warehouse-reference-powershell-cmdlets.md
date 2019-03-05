---
title: Azure SQL veri ambarı için PowerShell cmdlet'leri
description: Üst PowerShell cmdlet'leri, Azure SQL veri duraklatma ve sürdürme bir veritabanı dahil olmak üzere ambarı için bulun.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: ec1f9a8a10f346c4b8e9e8dac364b2687637c204
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2019
ms.locfileid: "57317909"
---
# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a>PowerShell cmdlet'leri ve REST API'leri için SQL veri ambarı
Birçok SQL veri ambarı yönetim görevleri, Azure PowerShell cmdlet'lerini veya REST API'leri kullanılarak yönetilebilir.  SQL veri ambarı ortak görevleri otomatikleştirmek için PowerShell komutlarını kullanmayı bazı örnekler aşağıdadır.  Bazı iyi diğer örnekler için bkz [REST ölçeklenebilirliğiyle yönetme][Manage scalability with REST].

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="get-started-with-azure-powershell-cmdlets"></a>Azure PowerShell cmdlet'lerini kullanmaya başlama
1. Windows PowerShell'i açın.
2. PowerShell komut isteminde, Azure Resource Manager'a oturum açıp aboneliğinizi seçmek için şu komutları çalıştırın.
   
    ```PowerShell
    Connect-AzAccount
    Get-AzSubscription
    Select-AzSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a>Duraklatma SQL veri ambarı örneği
"" Server01."adlı bir sunucuda barındırılan Database02" adlı bir veritabanı duraklatılamadı  "ResourceGroup1." adlı bir Azure kaynak grubunda sunucusudur

```Powershell
Suspend-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
Bir değişim Bu örnek alınan nesneyi kanallar [Suspend-AzSqlDatabase][Suspend-AzSqlDatabase].  Sonuç olarak, veritabanı duraklatıldı. Son komut sonuçları gösterilmektedir.

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a>SQL veri ambarı örneği başlatın
"" Server01."adlı bir sunucuda barındırılan Database02" adlı bir veritabanı işlemi sürdürme Sunucu "ResourceGroup1." adlı bir kaynak grubunda yer alır

```Powershell
Resume-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Bir değişim Bu örnek "" ResourceGroup1."adlı bir kaynak grubunda yer alan Server01" adlı bir sunucudan "Database02" adlı bir veritabanı alır. Alınan nesnenin kanallar [sürdürme AzSqlDatabase][Resume-AzSqlDatabase].

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzSqlDatabase
```

> [!NOTE]
> Sunucunuz foo.database.windows.net, ise "foo" PowerShell cmdlet'leri - ServerName kullanın unutmayın.
> 
> 

## <a name="other-supported-powershell-cmdlets"></a>Desteklenen diğer PowerShell cmdlet'leri
Bu PowerShell cmdlet'lerini, Azure SQL veri ambarı ile desteklenir.

* [Get-AzSqlDatabase][Get-AzSqlDatabase]
* [Get-AzSqlDeletedDatabaseBackup][Get-AzSqlDeletedDatabaseBackup]
* [Get-AzSqlDatabaseRestorePoints][Get-AzSqlDatabaseRestorePoints]
* [New-AzSqlDatabase][New-AzSqlDatabase]
* [Remove-AzSqlDatabase][Remove-AzSqlDatabase]
* [Geri yükleme-AzSqlDatabase][Restore-AzSqlDatabase]
* [Resume-AzSqlDatabase][Resume-AzSqlDatabase]
* [AzSubscription seçin][Select-AzSubscription]
* [Set-AzSqlDatabase][Set-AzSqlDatabase]
* [Suspend-AzSqlDatabase][Suspend-AzSqlDatabase]

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla PowerShell örnekleri için bkz:

* [PowerShell kullanarak SQL Data Warehouse oluşturma][Create a SQL Data Warehouse using PowerShell]
* [Veritabanı geri yükleme][Database restore]

PowerShell ile otomatik olarak diğer görevleri görmek [Azure SQL veritabanı cmdlet'leri][Azure SQL Database Cmdlets]. Tüm Azure SQL veritabanı cmdlet'leri Azure SQL veri ambarı için desteklendiğini unutmayın.  REST ile otomatik olarak yapılabilir görevleri bir listesi için bkz. [işlemleri için Azure SQL veritabanı][Operations for Azure SQL Database].

<!--Image references-->

<!--Article references-->
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Create a SQL Data Warehouse using PowerShell]: ./create-data-warehouse-powershell.md
[Database restore]: ./sql-data-warehouse-restore-database-powershell.md
[Manage scalability with REST]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[Azure SQL Database Cmdlets]: https://docs.microsoft.com/powershell/module/azurerm.sql
[Operations for Azure SQL Database]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[Get-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/get-Azsqldatabase
[Get-AzSqlDeletedDatabaseBackup]: https://docs.microsoft.com/powershell/module/azurerm.sql/get-AzSqlDeletedDatabaseBackup
[Get-AzSqlDatabaseRestorePoints]: https://docs.microsoft.com/powershell/module/azurerm.sql/get-AzSqlDatabaseRestorePoints
[New-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/New-AzSqlDatabase
[Remove-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Remove-AzSqlDatabase
[Restore-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Restore-AzSqlDatabase
[Resume-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Resume-AzSqlDatabase
<!-- It appears that Select-AzSubscription isn't documented, so this points to Select-AzureSubscription -->
[Select-AzSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Set-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Set-AzSqlDatabase
[Suspend-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Suspend-AzSqlDatabase

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
