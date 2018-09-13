---
title: Azure SQL veri ambarı için PowerShell cmdlet'leri
description: Üst PowerShell cmdlet'leri, Azure SQL veri duraklatma ve sürdürme bir veritabanı dahil olmak üzere ambarı için bulun.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: f38c9e3bed93a77cd9b35c6d23983ee5785a34a7
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44714485"
---
# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a>PowerShell cmdlet'leri ve REST API'leri için SQL veri ambarı
Birçok SQL veri ambarı yönetim görevleri, Azure PowerShell cmdlet'lerini veya REST API'leri kullanılarak yönetilebilir.  SQL veri ambarı ortak görevleri otomatikleştirmek için PowerShell komutlarını kullanmayı bazı örnekler aşağıdadır.  Bazı iyi diğer örnekler için bkz [REST ölçeklenebilirliğiyle yönetme][Manage scalability with REST].

> [!NOTE]
> SQL veri ambarı ile Azure PowerShell'i kullanmak için Azure PowerShell 1.0.3 sürümü olması gerekir. veya daha büyük.  Çalıştırarak sürümünüzü kontrol edebilirsiniz **Get-Module - ListAvailable-Name Azure**.  En son sürümü yüklenebilir [Microsoft Web Platformu yükleyicisi][Microsoft Web Platform Installer].  En son sürümü yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma][How to install and configure Azure PowerShell].
> 
> 

## <a name="get-started-with-azure-powershell-cmdlets"></a>Azure PowerShell cmdlet'lerini kullanmaya başlama
1. Windows PowerShell'i açın.
2. PowerShell komut isteminde, Azure Resource Manager'a oturum açıp aboneliğinizi seçmek için şu komutları çalıştırın.
   
    ```PowerShell
    Connect-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a>Duraklatma SQL veri ambarı örneği
"" Server01."adlı bir sunucuda barındırılan Database02" adlı bir veritabanı duraklatılamadı  "ResourceGroup1." adlı bir Azure kaynak grubunda sunucusudur

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
Bir değişim Bu örnek alınan nesneyi kanallar [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase].  Sonuç olarak, veritabanı duraklatıldı. Son komut sonuçları gösterilmektedir.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a>SQL veri ambarı örneği başlatın
"" Server01."adlı bir sunucuda barındırılan Database02" adlı bir veritabanı işlemi sürdürme Sunucu "ResourceGroup1." adlı bir kaynak grubunda yer alır

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Bir değişim Bu örnek "" ResourceGroup1."adlı bir kaynak grubunda yer alan Server01" adlı bir sunucudan "Database02" adlı bir veritabanı alır. Alınan nesnenin kanallar [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase].

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

> [!NOTE]
> Sunucunuz foo.database.windows.net, ise "foo" PowerShell cmdlet'leri - ServerName kullanın unutmayın.
> 
> 

## <a name="other-supported-powershell-cmdlets"></a>Desteklenen diğer PowerShell cmdlet'leri
Bu PowerShell cmdlet'lerini, Azure SQL veri ambarı ile desteklenir.

* [Get-AzureRmSqlDatabase][Get-AzureRmSqlDatabase]
* [Get-AzureRmSqlDeletedDatabaseBackup][Get-AzureRmSqlDeletedDatabaseBackup]
* [Get-AzureRmSqlDatabaseRestorePoints][Get-AzureRmSqlDatabaseRestorePoints]
* [Yeni-AzureRmSqlDatabase][New-AzureRmSqlDatabase]
* [Remove-AzureRmSqlDatabase][Remove-AzureRmSqlDatabase]
* [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase]
* [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase]
* [Select-AzureRmSubscription][Select-AzureRmSubscription]
* [Set-AzureRmSqlDatabase][Set-AzureRmSqlDatabase]
* [Askıya Al-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase]

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
[Get-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabase
[Get-AzureRmSqlDeletedDatabaseBackup]: https://docs.microsoft.com/powershell/module/azurerm.sql/get-AzureRmSqlDeletedDatabaseBackup
[Get-AzureRmSqlDatabaseRestorePoints]: https://docs.microsoft.com/powershell/module/azurerm.sql/get-AzureRmSqlDatabaseRestorePoints
[New-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/New-AzureRmSqlDatabase
[Remove-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Remove-AzureRmSqlDatabase
[Restore-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Restore-AzureRmSqlDatabase
[Resume-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Resume-AzureRmSqlDatabase
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points to Select-AzureSubscription -->
[Select-AzureRmSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Set-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Set-AzureRmSqlDatabase
[Suspend-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Suspend-AzureRmSqlDatabase

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
