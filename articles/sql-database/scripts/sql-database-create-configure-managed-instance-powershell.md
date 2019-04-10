---
title: PowerShell örneği - Azure SQL veritabanı'nda yönetilen örnek oluşturma | Microsoft Docs
description: Azure PowerShell örnek betiği Azure SQL veritabanı yönetilen örnek oluşturma
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
manager: craigg
ms.date: 03/25/2019
ms.openlocfilehash: c85b967615e866635cb4dd93be5ddeb78a8c7129
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59357013"
---
# <a name="use-powershell-to-create-an-azure-sql-database-managed-instance"></a>Yönetilen örnek bir Azure SQL veritabanı oluşturmak için PowerShell kullanma

Bu PowerShell Betiği örneği, yeni bir sanal ağ içinde ayrılmış bir alt ağdaki bir Azure SQL veritabanı yönetilen örneği oluşturur. Ayrıca, bir yol tablosu ve sanal ağ için ağ güvenlik grubu yapılandırır. Betik başarıyla çalıştırıldıktan sonra yönetilen örnek gelen sanal ağ içindeki veya bir şirket içi Ortamı'ndan erişilebilir. Bkz: [bir Azure SQL veritabanı yönetilen örneğine bağlanmak için Azure VM yapılandırma](../sql-database-managed-instance-configure-vm.md) ve [noktadan siteye bağlantı, şirket içinden Azure SQL veritabanı yönetilen örneği için yapılandırma](../sql-database-managed-instance-configure-p2s.md).

> [!IMPORTANT]
> Kısıtlamalar için bkz: [desteklenen bölgeler](../sql-database-managed-instance-resource-limits.md#supported-regions) ve [desteklenen abonelik türleri](../sql-database-managed-instance-resource-limits.md#supported-subscription-types).

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz Bu öğretici AZ PowerShell 1.4.0 gerektirir veya üzeri. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="sample-script"></a>Örnek betik

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/managed-instance/create-and-configure-managed-instance.ps1 "Create managed instance")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanın.

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notes |
|---|---|
| [Yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur.
| [Yeni AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Bir sanal ağ oluşturur |
| [AzVirtualNetworkSubnetConfig ekleyin](/powershell/module/az.network/Add-AzVirtualNetworkSubnetConfig) | Bir sanal ağa bir alt ağ yapılandırması ekler |
| [Get-AzVirtualNetwork](/powershell/module/az.network/Get-AzVirtualNetwork) | Bir kaynak grubunda bir sanal ağ alır |
| [Set-AzVirtualNetwork](/powershell/module/az.network/Set-AzVirtualNetwork) | Bir sanal ağ için hedef durumunu ayarlar. |
| [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/Get-AzVirtualNetworkSubnetConfig) | Sanal ağ içinde bir alt ağ alır |
| [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/Set-AzVirtualNetworkSubnetConfig) | Sanal ağ içinde bir alt ağ yapılandırması için hedef durumunu yapılandırır |
| [Yeni AzRouteTable](/powershell/module/az.network/New-AzRouteTable) | Bir rota tablosu oluşturur |
| [Get-AzRouteTable](/powershell/module/az.network/Get-AzRouteTable) | Rota tabloları alır |
| [Set-AzRouteTable](/powershell/module/az.network/Set-AzRouteTable) | Bir yol tablosu için hedef durumunu ayarlar. |
| [Yeni AzSqlInstance](/powershell/module/az.sql/New-AzSqlInstance) | Azure SQL veritabanı yönetilen örneği oluşturur |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek SQL Veritabanı PowerShell betiği örnekleri [Azure SQL Veritabanı PowerShell betikleri](../sql-database-powershell-samples.md) içinde bulunabilir.
