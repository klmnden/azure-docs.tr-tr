---
title: "Azure PowerShell Betiği örnek - yük trafiği dengelemek için sanal makineleri yüksek oranda kullanılabilirlik için | Microsoft Docs"
description: "Azure PowerShell Betiği örnek - yük trafiği dengelemek için sanal makineleri yüksek oranda kullanılabilirlik için"
services: load-balancer
documentationcenter: load-balancer
author: georgewallace
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: c77def8906b151f2cc6e4bbc4188be8ecbeac732
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="load-balance-traffic-to-vms-for-high-availability"></a>Yük trafiği dengelemek için sanal makineleri yüksek oranda kullanılabilirlik için

Bu komut dosyası örneği yüksek oranda kullanılabilir yapılandırılmış birkaç Windows sanal makineleri çalıştırmak ve dengeli yapılandırma yüklemek için gereken her şeyi oluşturur. Betiği çalıştırdıktan sonra üç sanal makineler, birleştirilmiş bir Azure kullanılabilirlik kümesi için ve bir Azure yük dengeleyici üzerinden erişilebilir olacaktır.

Gerekirse, bulunan yönergeleri kullanarak Azure PowerShell'i yükleme [Azure PowerShell Kılavuzu](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)ve ardından çalıştırın `Login-AzureRmAccount` Azure ile bir bağlantı oluşturmak için.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.ps1 "Quick Create VM")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, bir kaynak grubu, sanal makine, kullanılabilirlik kümesi, yük dengeleyici ve tüm ilişkili kaynakları oluşturmak için aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [Yeni-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [AzureRmVirtualNetworkSubnetConfig yeni](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Bir alt ağ yapılandırması oluşturur. Bu yapılandırma sanal ağ oluşturma işlemine kullanılır. |
| [Yeni-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Bir Azure sanal ağ ve alt ağ oluşturur. |
| [AzureRmPublicIpAddress yeni](/powershell/module/azurerm.network/new-azurermpublicipaddress)  | Bir ortak IP adresi statik bir IP adresi ve ilişkili bir DNS adı ile oluşturur. |
| [AzureRmLoadBalancer yeni](/powershell/module/azurerm.network/new-azurermloadbalancer)  | Bir Azure yük dengeleyici oluşturur. |
| [AzureRmLoadBalancerProbeConfig yeni](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | Bir yük dengeleyici araştırması oluşturur. Yük Dengeleyici araştırmasını yük dengeleyici kümesindeki her bir VM izlemek için kullanılır. Tüm VM erişilemez hale gelirse VM trafik yönlendirilmez. |
| [AzureRmLoadBalancerRuleConfig yeni](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | Bir yük dengeleyici kuralı oluşturur. Bu örnekte, bağlantı noktası 80 için bir kural oluşturulur. HTTP trafiği yük dengeleyicide ulaşan gibi 80 numaralı bağlantı noktasına yönlendirilir yük dengeleyici kümesindeki sanal makineleri biri. |
| [AzureRmLoadBalancerInboundNatRuleConfig yeni](/powershell/module/azurerm.network/new-azurermloadbalancerinboundnatruleconfig) | Yük Dengeleyici ağ adresi çevirisi (NAT) kuralı oluşturur.  NAT kuralları, bir VM üzerinde bir bağlantı noktasına bir bağlantı noktası yük dengeleyicinin eşleyin. Bu örnekte, SSH trafiği yük dengeleyici kümesindeki her bir VM için NAT kuralı oluşturulur.  |
| [AzureRmNetworkSecurityGroup yeni](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Internet ve sanal makine arasında bir güvenlik sınırı olan bir ağ güvenlik grubu (NSG) oluşturur. |
| [AzureRmNetworkSecurityRuleConfig yeni](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Gelen trafiğe izin veren bir NSG kuralı oluşturur. Bu örnekte, bağlantı noktası 22 SSH trafiği için açıldı. |
| [AzureRmNetworkInterface yeni](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Bir sanal ağ kartı oluşturur ve sanal ağ, alt ağ ve NSG ekler. |
| [AzureRmAvailabilitySet yeni](/powershell/module/azurerm.compute/new-azurermavailabilityset) | Bir kullanılabilirlik kümesi oluşturur. Kullanılabilirlik kümeleri uygulama çalışma süresi hatası oluşursa, kümesinin tamamını değil parametreden şekilde sanal makineler arasında fiziksel kaynakları yayarak emin olun. |
| [AzureRmVMConfig yeni](/powershell/module/azurerm.compute/new-azurermvmconfig) | Bir VM yapılandırması oluşturur. Bu yapılandırma VM adı, işletim sistemi ve yönetici kimlik bilgileri gibi bilgileri içerir. Yapılandırma VM oluşturma sırasında kullanılır. |
| [Yeni-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm)  | Sanal makine oluşturur ve ağ kartı, sanal ağ, alt ağ ve NSG bağlanır. Bu komut ayrıca kullanılan ve yönetici kimlik bilgileri olması için sanal makine görüntüsü belirtir.  |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](https://docs.microsoft.com/powershell/azure/overview).

Ek ağ PowerShell komut dosyası örnekleri bulunabilir [Azure ağ genel görünümü belgelerine](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
