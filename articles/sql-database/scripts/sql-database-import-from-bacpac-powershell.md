---
title: Azure SQL veritabanında BACPAC dosyasını içeri aktarmak için PowerShell örneği | Microsoft Docs
description: BACPAC dosyasını bir SQL veritabanına içeri aktarmak için Azure PowerShell örnek betiği
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: load & move data
ms.devlang: PowerShell
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
manager: craigg
ms.date: 05/24/2019
ms.openlocfilehash: be8fb1f7219ae017d520744778c0b83feb748379
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244219"
---
# <a name="use-powershell-to-import-a-bacpac-file-into-an-azure-sql-database"></a>PowerShell kullanarak BACPAC dosyasını bir Azure SQL veritabanına içeri aktarma

Bu PowerShell betiği örneği, BACPAC dosyasındaki bir veritabanını Azure SQL veritabanına içeri aktarır.  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz Bu öğretici AZ PowerShell 1.4.0 gerektirir veya üzeri. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="sample-script"></a>Örnek betik

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/import-from-bacpac/import-from-bacpac.ps1?highlight=20-21 "Create SQL Database")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanın.

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Tek veritabanları ve elastik havuzlar barındıran bir SQL veritabanı sunucusu oluşturur. |
| [Yeni AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) | SQL veritabanı sunucusunda girilen IP adresi aralığındaki tüm tek veritabanları ve havuza alınmış veritabanını erişime izin vermek için bir SQL veritabanı sunucusu güvenlik duvarı kuralı oluşturur. |
| [New-AzSqlDatabaseImport](/powershell/module/az.sql/new-azsqldatabaseimport) | Bir BACPAC dosyasını içeri aktarır ve sunucuda yeni bir veritabanı oluşturur. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek SQL Veritabanı PowerShell betiği örnekleri [Azure SQL Veritabanı PowerShell betikleri](../sql-database-powershell-samples.md) içinde bulunabilir.
