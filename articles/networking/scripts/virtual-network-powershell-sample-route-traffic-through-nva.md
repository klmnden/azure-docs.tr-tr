---
title: "Azure PowerShell komut dosyası örneği - rota trafiği ağ sanal gereç aracılığıyla | Microsoft Docs"
description: "Azure PowerShell komut dosyası örneği - güvenlik duvarı ağ sanal gereç yoluyla trafiği yönlendirme."
services: virtual-network
documentationcenter: virtual-network
author: georgewallace
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: 883d28dac72a66c2186d222f72b04d68e532cead
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>Bir ağ sanal gereç yoluyla trafiği yönlendirme

Bu komut dosyası örneği ön uç ve arka uç alt ağları ile bir sanal ağ oluşturur. Ayrıca IP iletme iki alt ağlar arasında trafiği yönlendirmek için etkin olan bir VM oluşturur. Komut dosyasını çalıştırdıktan sonra VM için bir güvenlik duvarı uygulaması gibi ağ yazılım dağıtabilirsiniz.

Gerekirse, bulunan yönergeleri kullanarak Azure PowerShell'i yükleme [Azure PowerShell Kılavuzu](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)ve ardından çalıştırın `Login-AzureRmAccount` Azure ile bir bağlantı oluşturmak için.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası


[!code-powershell[Ana](../../../powershell_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.ps1 "trafiği ağ sanal gereç aracılığıyla yönlendirmek")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```
## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, bir kaynak grubu, sanal ağ ve ağ güvenlik grupları oluşturmak için aşağıdaki komutları kullanır. Komut özgü belgelere Tablo bağlantıları her komut.

| Komut | Notlar |
|---|---|
| [Yeni-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [Yeni-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Bir Azure sanal ağı ve ön uç alt ağı oluşturur. |
| [AzureRmVirtualNetworkSubnetConfig yeni](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Arka uç ve DMZ alt ağlar oluşturur. |
| [AzureRmPublicIpAddress yeni](/powershell/module/azurerm.network/new-azurermpublicipaddress) | VM Internet'ten erişmek için genel bir IP adresi oluşturur. |
| [AzureRmNetworkInterface yeni](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Bir sanal ağ arabirimi ve onu için IP iletimini etkinleştirme oluşturur. |
| [AzureRmNetworkSecurityGroup yeni](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Bir ağ güvenlik grubu (NSG) oluşturur. |
| [AzureRmNetworkSecurityRuleConfig yeni](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | HTTP ve HTTPS bağlantı noktalarını VM'ye gelen izin NSG kuralları oluşturur. |
| [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)| Nsg'ler ve alt ağlara yol tablolarını ilişkilendirir. |
| [AzureRmRouteTable yeni](/powershell/module/azurerm.network/new-azurermroutetable)| Tüm yollar için yol tablosu oluşturur. |
| [AzureRMRouteConfig yeni](/powershell/module/azurerm.network/new-azurermrouteconfig)| Alt ağlar ve VM ile Internet arasında trafiği yönlendirmek için yollar oluşturur. |
| [Yeni-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Bir sanal makine oluşturur ve NIC ekler. Bu komut ayrıca kullanmak için sanal makine görüntüsü ve yönetici kimlik bilgilerini belirtir. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup)  | Bir kaynak grubu ve içerdiği tüm kaynakları siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](https://docs.microsoft.com/powershell/azure/overview).

Ek ağ PowerShell komut dosyası örnekleri bulunabilir [Azure ağ genel görünümü belgelerine](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).