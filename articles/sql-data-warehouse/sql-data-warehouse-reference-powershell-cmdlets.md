---
title: "Azure SQL Data Warehouse için PowerShell cmdlet'leri"
description: "Üst PowerShell cmdlet'leri için Azure SQL veri duraklatma ve sürdürme bir veritabanı da dahil olmak üzere ambarı bulun."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 6f0d5772-f05f-4cc8-9749-4adb153dfd50
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: reference
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: ce3e11587c2e0cb92923868a4f26d7f59c7ef4ca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a>PowerShell cmdlet'leri ve SQL Data Warehouse için REST API'leri
Birçok SQL veri ambarı yönetim görevleri, Azure PowerShell cmdlet'lerini veya REST API'leri kullanılarak yönetilebilir.  Aşağıda, SQL veri ambarı ortak görevleri otomatikleştirmek için PowerShell komutlarını kullanmak nasıl bazı örnekler verilmiştir.  Makale iyi bazı diğer örnekler için bkz [ölçeklenebilirlik REST ile yönetme][Manage scalability with REST].

> [!NOTE]
> SQL Data Warehouse ile Azure PowerShell kullanmak için Azure PowerShell 1.0.3 sürümü gerekir veya daha büyük.  Çalıştırarak sürümünüzü kontrol edebilirsiniz **Get-Module - listavailable birlikte-Name Azure**.  En son sürümünü yüklenebilir [Microsoft Web Platformu yükleyicisi][Microsoft Web Platform Installer].  En son sürümü yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma][How to install and configure Azure PowerShell].
> 
> 

## <a name="get-started-with-azure-powershell-cmdlets"></a>Azure PowerShell cmdlet'leri kullanmaya başlama
1. Windows PowerShell'i açın.
2. PowerShell komut isteminde, Azure Resource Manager için oturum açın ve aboneliğinizi seçmek için şu komutları çalıştırın.
   
    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a>Duraklatma SQL veri ambarı örneği
"" Server01."adlı bir sunucuda barındırılan Database02" adlı bir veritabanı duraklatma  Sunucu "ResourceGroup1." adlı bir Azure kaynak grubunda yer alıyor

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
Bir değişim Bu örnek alınan nesneyi kanallar [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase].  Sonuç olarak, veritabanı duraklatıldı. Son komutun sonuçlarını gösterir.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a>SQL veri ambarı örneği Başlat
"" Server01."adlı bir sunucuda barındırılan Database02" adlı bir veritabanı işlemi sürdürme Sunucu "ResourceGroup1." adlı bir kaynak grubunda yer alan

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Bir değişim Bu örnekte "" ResourceGroup1."adlı bir kaynak grubunda yer alan Server01" adlı bir sunucudan "Database02" adlı bir veritabanı alır. Alınan nesnesine kanallar [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase].

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

> [!NOTE]
> Sunucunuz foo.database.windows.net, ise "foo" PowerShell cmdlet'leri - ServerName kullanmak unutmayın.
> 
> 

## <a name="other-supported-powershell-cmdlets"></a>Diğer desteklenen PowerShell cmdlet'leri
Bu PowerShell cmdlet'leri, Azure SQL Data Warehouse ile desteklenir.

* [Get-AzureRmSqlDatabase][Get-AzureRmSqlDatabase]
* [Get-AzureRmSqlDeletedDatabaseBackup][Get-AzureRmSqlDeletedDatabaseBackup]
* [Get-AzureRmSqlDatabaseRestorePoints][Get-AzureRmSqlDatabaseRestorePoints]
* [AzureRmSqlDatabase yeni][New-AzureRmSqlDatabase]
* [Remove-AzureRmSqlDatabase][Remove-AzureRmSqlDatabase]
* [Geri yükleme-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase]
* [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase]
* [Select-AzureRmSubscription][Select-AzureRmSubscription]
* [Set-AzureRmSqlDatabase][Set-AzureRmSqlDatabase]
* [Askıya alma AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase]

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla PowerShell örnekler için bkz:

* [PowerShell kullanarak SQL Data Warehouse oluşturma][Create a SQL Data Warehouse using PowerShell]
* [Veritabanı geri yükleme][Database restore]

PowerShell ile otomatik olarak diğer görevler için bkz: [Azure SQL veritabanı cmdlet'leri][Azure SQL Database Cmdlets]. Tüm Azure SQL veritabanı cmdlet'leri Azure SQL Data Warehouse için desteklendiğini unutmayın.  Hangi BİLGİSAYARLARLA otomatik görevlerin listesi için bkz: [işlemleri Azure SQL veritabanları için][Operations for Azure SQL Databases].

<!--Image references-->

<!--Article references-->
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Create a SQL Data Warehouse using PowerShell]: ./sql-data-warehouse-get-started-provision-powershell.md
[Database restore]: ./sql-data-warehouse-restore-database-powershell.md
[Manage scalability with REST]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[Azure SQL Database Cmdlets]: https://msdn.microsoft.com/library/mt574084.aspx
[Operations for Azure SQL Databases]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[Get-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt603648.aspx
[Get-AzureRmSqlDeletedDatabaseBackup]: https://msdn.microsoft.com/library/mt693387.aspx
[Get-AzureRmSqlDatabaseRestorePoints]: https://msdn.microsoft.com/library/mt603642.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Remove-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619368.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx
[Resume-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points to Select-AzureSubscription -->
[Select-AzureRmSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Suspend-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
