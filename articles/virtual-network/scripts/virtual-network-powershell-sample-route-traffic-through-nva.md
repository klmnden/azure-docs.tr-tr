---
title: Azure PowerShell komut dosyası örneği - rota trafiği ağ sanal gereç aracılığıyla | Microsoft Docs
description: Azure PowerShell komut dosyası örneği - güvenlik duvarı ağ sanal gereç yoluyla trafiği yönlendirme.
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
ms.openlocfilehash: 766d800a7875536b729f564a3d9948e5598de6aa
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>Bir ağ sanal gereç yoluyla trafiği yönlendirme

Bu komut dosyası örneği ön uç ve arka uç alt ağları ile bir sanal ağ oluşturur. Ayrıca IP iletme iki alt ağlar arasında trafiği yönlendirmek için etkin olan bir VM oluşturur. Komut dosyasını çalıştırdıktan sonra VM için bir güvenlik duvarı uygulaması gibi ağ yazılım dağıtabilirsiniz.

Azure'dan komut dosyasını çalıştırabilir [bulut Kabuk](https://shell.azure.com/powershell), veya bir yerel PowerShell yüklemesinden. PowerShell'i yerel olarak kullanırsanız, bu komut dosyası AzureRM PowerShell modülü sürümü 5.4.1 gerektirir veya sonraki bir sürümü. Yüklü olan sürümü bulmak için Çalıştır `Get-Module -ListAvailable AzureRM`. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik


[!code-azurepowershell-interactive[main](../../../powershell_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.ps1 "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```
## <a name="script-explanation"></a>Betik açıklaması

Bu komut, bir kaynak grubu, sanal ağ ve ağ güvenlik grupları oluşturmak için aşağıdaki komutları kullanır. Aşağıdaki tabloda bağlantıları komutu özgü belgelere her komut:

| Komut | Notlar |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Bir Azure sanal ağı ve ön uç alt ağı oluşturur. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Arka uç ve DMZ alt ağlar oluşturur. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | VM internet'ten erişmek için genel bir IP adresi oluşturur. |
| [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Bir sanal ağ arabirimi ve onu için IP iletimini etkinleştirme oluşturur. |
| [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Bir ağ güvenlik grubu (NSG) oluşturur. |
| [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | HTTP ve HTTPS bağlantı noktalarını VM'ye gelen izin NSG kuralları oluşturur. |
| [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)| Nsg'ler ve alt ağlara yol tablolarını ilişkilendirir. |
| [New-AzureRmRouteTable](/powershell/module/azurerm.network/new-azurermroutetable)| Tüm yollar için yol tablosu oluşturur. |
| [New-AzureRMRouteConfig](/powershell/module/azurerm.network/new-azurermrouteconfig)| Alt ağlar ve VM ile internet arasında trafiği yönlendirmek için yollar oluşturur. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Bir sanal makine oluşturur ve NIC ekler. Bu komut ayrıca kullanmak için sanal makine görüntüsü ve yönetici kimlik bilgilerini belirtir. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup)  | Bir kaynak grubu ve içerdiği tüm kaynakları siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/azure/overview).

Ek sanal ağ PowerShell komut dosyası örnekleri bulunabilir [sanal ağ PowerShell örnekleri](../powershell-samples.md).