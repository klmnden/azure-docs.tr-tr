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
ms.date: 01/25/2019
ms.openlocfilehash: 5bf7d8717fd6b10fab454c6f11d459c6a620b42c
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55458746"
---
# <a name="use-powershell-to-restore-an-azure-sql-single-database-from-backups"></a>Tek bir Azure SQL veritabanı yedeklemeleri geri yüklemek için PowerShell kullanma

Bu PowerShell betiği örneği, coğrafi olarak yedekli bir yedeklemeden Azure SQL veritabanını geri yükler, silinmiş bir Azure SQL veritabanını en son yedeklemeye geri yükler ve Azure SQL veritabanını belirli bir zaman noktasına geri yükler.  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz, bu öğretici Azure PowerShell modülü 5.7.0 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="sample-script"></a>Örnek betik

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/restore-database/restore-database.ps1?highlight=17-18 "Create SQL Database")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. | [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Tek veritabanı veya elastik havuz barındıran bir SQL veritabanı sunucusu oluşturur. |
| [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) | Bir veritabanı bir tek başına veya havuza alınmış bir veritabanı olarak SQL veritabanı sunucusu oluşturur. |
[Get-AzureRmSqlDatabaseGeoBackup](/powershell/module/azurerm.sql/get-azurermsqldatabasegeobackup) | Bir tek başına veya havuza alınmış veritabanının coğrafi olarak yedekli bir yedeklemesini alır. |
| [Restore-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) | Bir SQL tek başına veya havuza alınmış veritabanını geri yükler. |
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase) | Bir Azure SQL tek başına veya havuza alınmış veritabanını kaldırır. |
| [Get-AzureRmSqlDeletedDatabaseBackup](/powershell/module/azurerm.sql/get-azurermsqldeleteddatabasebackup) | Silinen bir tek başına veya havuza alınmış veritabanını geri yükleyebilirsiniz alır. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek SQL Veritabanı PowerShell betiği örnekleri [Azure SQL Veritabanı PowerShell betikleri](../sql-database-powershell-samples.md) içinde bulunabilir.
