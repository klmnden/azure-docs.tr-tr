---
title: Azure PowerShell komut dosyası örneği - filtre VM ağ trafiğini | Microsoft Docs
description: Azure PowerShell Betiği örnek - gelen ve giden VM ağ trafiğini filtre.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: jdial
ms.openlocfilehash: 13089e0194b62a1f2f7a7c592d3ff21af8495dd1
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a>Gelen ve giden VM ağ trafiği filtreleme

Bu komut dosyası örneği ön uç ve arka uç alt ağları ile bir sanal ağ oluşturur. Ön uç alt ağa gelen ağ trafiğini HTTP ve HTTPS'yi sınırlı açıkken arka uç alt ağından internet giden trafiğe izin verilmez. Betiği çalıştırdıktan sonra iki NIC içeren bir sanal makine sahip. Her NIC'nin farklı bir alt ağa bağlanır.

Azure'dan komut dosyasını çalıştırabilir [bulut Kabuk](https://shell.azure.com/powershell), veya bir yerel PowerShell yüklemesinden. PowerShell'i yerel olarak kullanırsanız, bu komut dosyası AzureRM PowerShell modülü sürümü 5.4.1 gerektirir veya sonraki bir sürümü. Yüklü olan sürümü bulmak için Çalıştır `Get-Module -ListAvailable AzureRM`. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurepowershell-interactive[main](../../../powershell_scripts/virtual-network/filter-network-traffic/filter-network-traffic.ps1  "Filter VM network traffic")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Betik açıklaması

Bu komut, bir kaynak grubu, sanal ağ ve ağ güvenlik grupları oluşturmak için aşağıdaki komutları kullanır. Aşağıdaki tabloda bağlantıları komutu özgü belgelere her komut:

| Komut | Notlar |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Bir alt ağ yapılandırma nesnesi oluşturur |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Bir Azure sanal ağı ve ön uç alt ağı oluşturur. |
| [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Ağ güvenlik grubuna atanmış güvenlik kuralları oluşturur. |
| [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) |İzin veren veya özel alt ağları için belirli bağlantı noktalarını engellemek NSG kuralları oluşturur. |
| [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) | Nsg'ler alt ağlara ilişkilendirir. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | VM internet'ten erişmek için genel bir IP adresi oluşturur. |
| [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Sanal ağ arabirimi oluşturur ve bunları sanal ağın ön uç ve arka uç alt ağlara ekler. |
| [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Bir VM yapılandırması oluşturur. Bu yapılandırma VM adı, işletim sistemi ve yönetici kimlik bilgileri gibi bilgileri içerir. Yapılandırma VM oluşturma sırasında kullanılır. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Bir sanal makine oluşturun. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Bir kaynak grubu ve içerdiği tüm kaynaklar kaldırır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal ağ PowerShell komut dosyası örnekleri bulunabilir [sanal ağ PowerShell örnekleri](../powershell-samples.md).