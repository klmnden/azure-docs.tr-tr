---
title: "Azure SQL Data warehouse'da (PowerShell) işlem güç yönetimi | Microsoft Docs"
description: "İşlem gücüne yönetmek için PowerShell görevleri. Dwu ayarlayarak işlem kaynaklarını ölçeklendirme. Veya, duraklatma ve sürdürme işlem kaynaklarını maliyet tasarrufu sağlamak."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 8354a3c1-4e04-4809-933f-db414a8c74dc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 6a185d96447c2e1b0b463439dd062081e783da5f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-powershell"></a>Azure SQL Data warehouse'da (PowerShell) işlem güç yönetimi
> [!div class="op_single_selector"]
> * [Genel Bakış](sql-data-warehouse-manage-compute-overview.md)
> * [Portal](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>

## <a name="before-you-begin"></a>Başlamadan önce
### <a name="install-the-latest-version-of-azure-powershell"></a>Azure PowerShell'in en son sürümünü yükleyin
> [!NOTE]
> SQL Data Warehouse ile Azure PowerShell kullanmak için Azure PowerShell 1.0.3 sürümü gerekir veya daha büyük.  Komutu çalıştırın geçerli sürümünüzü doğrulamak için **Get-Module - listavailable birlikte-Name Azure**. En son sürümü yükleyebilirsiniz [Microsoft Web Platformu yükleyicisi][Microsoft Web Platform Installer].  Daha fazla bilgi için bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl][How to install and configure Azure PowerShell].
>
> 

### <a name="get-started-with-azure-powershell-cmdlets"></a>Azure PowerShell cmdlet'leri kullanmaya başlama
Başlamak için:

1. Azure PowerShell'i açın.
2. PowerShell komut isteminde, Azure Resource Manager için oturum açın ve aboneliğinizi seçmek için şu komutları çalıştırın.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Ölçek işlem gücü
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Dwu değiştirmek için kullanın [Set-AzureRmSqlDatabase] [ Set-AzureRmSqlDatabase] PowerShell cmdlet'i. Aşağıdaki örnek, MyServer sunucusunda barındırılan MySQLDW veritabanı için DW1000 için hizmet düzeyi hedefi ayarlar.

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Duraklatma işlem
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Bir veritabanı duraklatmak için kullanmak [Suspend-AzureRmSqlDatabase] [ Suspend-AzureRmSqlDatabase] cmdlet'i. Aşağıdaki örnek Server01 adlı bir sunucuda barındırılan Database02 adlı bir veritabanı duraklatır. Sunucu ResourceGroup1 adlı bir Azure kaynak grubunda yer alıyor.

> [!NOTE]
> Sunucunuz foo.database.windows.net, ise "foo" PowerShell cmdlet'leri - ServerName kullanmak unutmayın.
>
> 

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
```
Bir değişim bu sonraki örnek veritabanı $database nesnesine alır. Sonra nesneyi kanallar [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase]. Sonuçlar nesne resultDatabase içinde depolanır. Son komutun sonuçlarını gösterir.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Resume işlem
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Bir veritabanı başlatmak için kullanmak [Resume-AzureRmSqlDatabase] [ Resume-AzureRmSqlDatabase] cmdlet'i. Aşağıdaki örnek Server01 adlı bir sunucuda barındırılan Database02 adlı bir veritabanı başlatır. Sunucu ResourceGroup1 adlı bir Azure kaynak grubunda yer alıyor.

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" -DatabaseName "Database02"
```

Bir değişim bu sonraki örnek veritabanı $database nesnesine alır. Sonra nesneyi kanallar [Resume-AzureRmSqlDatabase] [ Resume-AzureRmSqlDatabase] ve sonuçları $resultDatabase içinde depolar. Son komutun sonuçlarını gösterir.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

<a name="check-database-state-bk"></a>

## <a name="check-database-state"></a>Veritabanı durumunu kontrol edin

Yukarıdaki örneklerde gösterildiği gibi kullanabilirsiniz [Get-AzureRmSqlDatabase] [ Get-AzureRmSqlDatabase] cmdlet böylece durumunu denetleme, bir veritabanı hakkında bilgi almak için aynı zamanda bir bağımsız değişken olarak kullanılacak. 

```powershell
Get-AzureRmSqlDatabase [-ResourceGroupName] <String> [-ServerName] <String> [[-DatabaseName] <String>]
 [-InformationAction <ActionPreference>] [-InformationVariable <String>] [-Confirm] [-WhatIf]
 [<CommonParameters>]
```

Hangi bir şey sonucunda ister 

```powershell   
ResourceGroupName             : nytrg
ServerName                    : nytsvr
DatabaseName                  : nytdb
Location                      : West US
DatabaseId                    : 86461aae-8e3d-4ded-9389-ac9d4bc69bbb
Edition                       : DataWarehouse
CollationName                 : SQL_Latin1General_CP1CI_AS
CatalogCollation              :
MaxSizeBytes                  : 32212254720
Status                        : Online
CreationDate                  : 10/26/2016 4:33:14 PM
CurrentServiceObjectiveId     : 620323bf-2879-4807-b30d-c2e6d7b3b3aa
CurrentServiceObjectiveName   : System2
RequestedServiceObjectiveId   : 620323bf-2879-4807-b30d-c2e6d7b3b3aa
RequestedServiceObjectiveName :
ElasticPoolName               :
EarliestRestoreDate           : 1/1/0001 12:00:00 AM
```

Burada sonra kontrol edebilirsiniz görmek için *durum* veritabanı. Bu durumda, bu veritabanının çevrimiçi olduğunu görebilirsiniz. 

Bu komutu çalıştırdığınızda, durum değeri ya da çevrimiçi, duraklatma, devam ettirmek, ölçeklendirme ve duraklatıldı almanız gerekir.

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Sonraki adımlar
Diğer yönetim görevleri için bkz: [yönetimine genel bakış][Management overview].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Management overview]: ./sql-data-warehouse-overview-manage.md
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Resume-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
[Suspend-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Get-AzureRmSqlDatabase]: /powershell/servicemanagement/azure.sqldatabase/v1.6.1/get-azuresqldatabase

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[Azure portal]: http://portal.azure.com/
