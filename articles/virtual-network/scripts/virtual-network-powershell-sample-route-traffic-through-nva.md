---
title: Azure PowerShell betiği örneği - Ağ sanal gereci aracılığıyla trafiği yönlendirme | Microsoft Docs
description: Azure PowerShell betiği örneği - Güvenlik duvarı ağ sanal gereci aracılığıyla trafiği yönlendirme.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: jdial
ms.openlocfilehash: 332d6ecc6becc9f1219c3b90cbbf35f8fa85faa9
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="route-traffic-through-a-network-virtual-appliance-script-sample"></a>Ağ sanal gereci aracılığıyla trafiği yönlendirme betiği örneği

Bu betik örneği, ön uç ve arka uç alt ağları ile sanal ağ oluşturur. Ayrıca iki alt ağ arasında trafiği yönlendirmek için IP iletme etkinleştirilmiş şekilde bir sanal makine de oluşturur. Betiği çalıştırdıktan sonra, sanal makineye güvenlik duvarı uygulaması gibi ağ yazılımı dağıtabilirsiniz.

Azure [Cloud Shell](https://shell.azure.com/powershell)’den veya yerel bir PowerShell yüklemesinden betiği yürütebilirsiniz. PowerShell’i yerel olarak kullanıyorsanız bu betik, AzureRM PowerShell modülünün 5.4.1 veya üzeri sürümlerini gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik


[!code-azurepowershell-interactive[main](../../../powershell_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.ps1 "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, sanal makineyi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```
## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir kaynak grubu, sanal ağ ve ağ güvenliği grupları oluşturmak için aşağıdaki komutları kullanır. Aşağıdaki tabloda yer alan her komut, komuta özgü belgelere yönlendirir:

| Komut | Notlar |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Bir Azure sanal ağı ve ön uç alt ağı oluşturur. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Arka uç ve DMZ alt ağları oluşturur. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | İnternet’ten sanal makineye erişmek için genel IP adresi oluşturur. |
| [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Bir sanal ağ arabirimi oluşturur ve bunun için IP iletimini etkinleştirir. |
| [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Ağ güvenlik grubu (NSG) oluşturur. |
| [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Sanal makineye gelen HTTP ve HTTPS bağlantı noktalarına izin veren NSG kuralları oluşturur. |
| [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)| NSG’leri ve rota tablolarını alt ağlarla ilişkilendirir. |
| [New-AzureRmRouteTable](/powershell/module/azurerm.network/new-azurermroutetable)| Tüm rotalar için bir rota tablosu oluşturur. |
| [New-AzureRMRouteConfig](/powershell/module/azurerm.network/new-azurermrouteconfig)| Sanal makine aracılığıyla alt ağlar ile İnternet arasında trafiği yönlendirmek için rotalar oluşturur. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Bir sanal makine oluşturur ve ona NIC’yi ekler. Bu komut ayrıca kullanılacak sanal makine görüntüsünü ve yönetici kimlik bilgilerini belirtir. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup)  | Bir kaynak grubunu ve içerdiği tüm kaynakları siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/azure/overview).

Ek sanal ağ PowerShell betiği örnekleri, [Sanal ağ PowerShell örnekleri](../powershell-samples.md) bölümünde bulunabilir.