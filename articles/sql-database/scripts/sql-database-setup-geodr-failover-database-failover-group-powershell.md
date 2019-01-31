---
title: PowerShell örneği coğrafi çoğaltma yük devretme grubu-tek başına Azure SQL veritabanı | Microsoft Docs
description: Etkin coğrafi çoğaltma yük devretme kümesi için azure PowerShell örnek betiği, Azure SQL veritabanı'nda tek bir veritabanı için Grup ve yük devretme.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: mashamsft
ms.author: mathoma
ms.reviewer: carlrab
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 31027e266f29ae0308ed70abfea5dbec3736f824
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55469303"
---
# <a name="use-powershell-to-configure-an-active-geo-replication-failover-group-for-a-single-database-in-azure-sql-database"></a>Azure SQL veritabanı'nda bir tek bir veritabanı için etkin coğrafi çoğaltma yük devretme grubu yapılandırmak için PowerShell kullanma

Bu PowerShell Betiği örneği, bir tek bir veritabanı için etkin coğrafi çoğaltma yük devretme grubu yapılandırır ve veritabanının ikincil bir çoğaltmasına yük devreder.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz, bu öğretici Azure PowerShell modülü 5.7.0 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="sample-scripts"></a>Örnek komut dosyaları

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/setup-geodr-and-failover/setup-geodr-and-failover-database-failover-group.ps1?highlight=19-22 "Set up failover group for single database")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $primaryresourcegroupname
Remove-AzureRmResourceGroup -ResourceGroupName $secondaryresourcegroupname
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Tek veritabanları ve elastik havuzlar barındıran bir SQL veritabanı sunucusu oluşturur. |
| [New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | Elastik havuz oluşturur. |
| [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) | Veritabanı özelliklerini güncelleştirir veya bir veritabanını elastik havuzun içine veya dışına ya da elastik havuzlar arasında taşır. |
| [New-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary)| Mevcut bir veritabanı için ikincil bir veritabanı oluşturur ve veri çoğaltmaya başlar. |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)| Bir veya daha fazla veritabanını alır. |
| [Set-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary)| Yük devretmeyi başlatmak için ikincil bir veritabanını birincil olarak değiştirir.|
| [Get-AzureRmSqlDatabaseReplicationLink](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) | Bir Azure SQL Veritabanı ve bir kaynak grubu veya SQL Server arasındaki coğrafi çoğaltma bağlantılarını alır. |
| [Remove-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) | Bir SQL Veritabanı ile belirtilen ikincil veritabanı arasında veri çoğaltmayı sonlandırır. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
| [New-AzureRMSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/new-azurermsqldatabasefailovergroup) | Belirtilen sunucular için yeni bir Azure SQL Veritabanı Yük Devretme Grubu oluşturur. |
| [Switch-AzureRMSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/switch-azurermsqldatabasefailovergroup) | Yük Devretme Grubundaki sunucuların rollerini değiştirir ve tüm ikincil veritabanlarını birincil role değiştirir. |
| [Get-AzureRMSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/get-azurermsqldatabasefailovergroup) | Belirli bir Azure SQL Veritabanı Yük Devretme Grubunu alır veya bir sunucudaki Yük Devretme Gruplarını listeler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek SQL Veritabanı PowerShell betiği örnekleri [Azure SQL Veritabanı PowerShell betikleri](../sql-database-powershell-samples.md) içinde bulunabilir.
