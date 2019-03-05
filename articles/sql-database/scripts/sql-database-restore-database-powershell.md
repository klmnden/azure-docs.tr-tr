---
title: PowerShell örneği-Azure SQL veritabanını geri yükleme-yedekleme | Microsoft Docs
description: Azure PowerShell örnek betiği, tek bir Azure SQL veritabanı, coğrafi olarak yedekli yedeklemelerden geri yüklemek için
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: mashamsft
ms.author: mathoma
ms.reviewer: carlrab
manager: craigg
ms.date: 03/04/2019
ms.openlocfilehash: 8974aeb327218e6108efc6e8b37008501fb42565
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57336481"
---
# <a name="use-powershell-to-restore-an-azure-sql-single-database-from-backups"></a>Tek bir Azure SQL veritabanı yedeklemeleri geri yüklemek için PowerShell kullanma

Bu PowerShell betiği örneği, coğrafi olarak yedekli bir yedeklemeden Azure SQL veritabanını geri yükler, silinmiş bir Azure SQL veritabanını en son yedeklemeye geri yükler ve Azure SQL veritabanını belirli bir zaman noktasına geri yükler.  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz, bu öğretici Azure PowerShell modülü 5.7.0 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="sample-script"></a>Örnek betik

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/restore-database/restore-database.ps1?highlight=17-18 "Create SQL Database")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. | 
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Tek veritabanı veya elastik havuz barındıran bir SQL veritabanı sunucusu oluşturur. |
| [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) | Bir veritabanı bir tek başına veya havuza alınmış bir veritabanı olarak SQL veritabanı sunucusu oluşturur. |
[Get-AzSqlDatabaseGeoBackup](/powershell/module/az.sql/get-azsqldatabasegeobackup) | Bir tek başına veya havuza alınmış veritabanının coğrafi olarak yedekli bir yedeklemesini alır. |
| [Geri yükleme-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase) | Bir SQL tek başına veya havuza alınmış veritabanını geri yükler. |
|[Remove-AzSqlDatabase](/powershell/module/az.sql/remove-azsqldatabase) | Bir Azure SQL tek başına veya havuza alınmış veritabanını kaldırır. |
| [Get-AzSqlDeletedDatabaseBackup](/powershell/module/az.sql/get-azsqldeleteddatabasebackup) | Silinen bir tek başına veya havuza alınmış veritabanını geri yükleyebilirsiniz alır. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek SQL Veritabanı PowerShell betiği örnekleri [Azure SQL Veritabanı PowerShell betikleri](../sql-database-powershell-samples.md) içinde bulunabilir.
