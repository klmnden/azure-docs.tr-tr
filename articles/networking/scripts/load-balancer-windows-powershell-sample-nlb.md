---
title: Azure PowerShell Betiği örnek - yük trafiği dengelemek için sanal makineleri yüksek oranda kullanılabilirlik için | Microsoft Docs
description: Azure PowerShell Betiği örnek - yük trafiği dengelemek için sanal makineleri yüksek oranda kullanılabilirlik için
services: load-balancer
documentationcenter: load-balancer
author: georgewallace
manager: timlt
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: load-balancer
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: 61d65cbdcf7867cc7a2a225648dc8b1ab7137bcb
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
ms.locfileid: "31595982"
---
# <a name="load-balance-traffic-to-vms-for-high-availability"></a>Yük trafiği dengelemek için sanal makineleri yüksek oranda kullanılabilirlik için

Bu komut dosyası örneği yüksek oranda kullanılabilir yapılandırılmış birkaç Windows sanal makineleri çalıştırmak ve dengeli yapılandırma yüklemek için gereken her şeyi oluşturur. Betiği çalıştırdıktan sonra bir Azure Kullanılabilirlik Kümesine eklenmiş ve bir Azure Load Balancer üzerinden erişilebilen üç sanal makineniz olur.

Gerekirse, [Azure PowerShell kılavuzunda](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) bulunan yönergeleri kullanarak Azure PowerShell’i yükleyin ve ardından Azure ile bağlantı oluşturmak için `Connect-AzureRmAccount` komutunu çalıştırın.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.ps1 "Quick Create VM")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir kaynak grubu, sanal makine, kullanılabilirlik kümesi, yük dengeleyici ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Bir alt ağ yapılandırması oluşturur. Bu yapılandırma, sanal ağ oluşturma işlemiyle birlikte kullanılır. |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Bir Azure sanal ağı ve alt ağ oluşturur. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress)  | Statik bir IP adresi ve ilişkili bir DNS adı ile bir genel IP adresi oluşturur. |
| [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer)  | Bir Azure yük dengeleyici oluşturur. |
| [AzureRmLoadBalancerProbeConfig yeni](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | Bir yük dengeleyici araştırması oluşturur. Yük Dengeleyici araştırmasını yük dengeleyici kümesindeki her bir VM izlemek için kullanılır. Herhangi bir VM erişilemez hale gelirse trafik VM’ye yönlendirilmez. |
| [AzureRmLoadBalancerRuleConfig yeni](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | Bir yük dengeleyici kuralı oluşturur. Bu örnekte 80 numaralı bağlantı noktası için bir kural oluşturulur. HTTP trafiği yük dengeleyicide ulaşan gibi 80 numaralı bağlantı noktasına yönlendirilir yük dengeleyici kümesindeki sanal makineleri biri. |
| [AzureRmLoadBalancerInboundNatRuleConfig yeni](/powershell/module/azurerm.network/new-azurermloadbalancerinboundnatruleconfig) | Yük Dengeleyici ağ adresi çevirisi (NAT) kuralı oluşturur.  NAT kuralları, bir VM üzerinde bir bağlantı noktasına bir bağlantı noktası yük dengeleyicinin eşleyin. Bu örnekte, SSH trafiği yük dengeleyici kümesindeki her bir VM için NAT kuralı oluşturulur.  |
| [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | İnternet ile sanal makine arasında güvenlik sınırı olan bir ağ güvenlik grubu (NSG) oluşturur. |
| [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Gelen trafiğe izin veren bir NSG kuralı oluşturur. Bu örnekte 22 numaralı bağlantı noktası SSH trafiğine açılır. |
| [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Sanal makine kartı oluşturur ve sanal ağa, alt ağa ve NSG’ye bağlar. |
| [AzureRmAvailabilitySet yeni](/powershell/module/azurerm.compute/new-azurermavailabilityset) | Bir kullanılabilirlik kümesi oluşturur. Kullanılabilirlik kümeleri, hata oluşması durumunda tüm kümenin etkilenmemesi için sanal makineleri fiziksel kaynaklara yayarak uygulama çalışma süresi sağlar. |
| [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Sanal makine yapılandırması oluşturur. Bu yapılandırma; sanal makine adı, işletim sistemi ve yönetici kimlik bilgileri gibi bilgileri içerir. Yapılandırma, sanal makine oluşturulurken kullanılır. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm)  | Sanal makine oluşturur ve ağ kartına, sanal ağa, alt ağa ve NSG’ye bağlar. Bu komut ayrıca kullanılacak sanal makine görüntüsünü ve yönetici kimlik bilgilerini belirtir.  |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/azure/overview).

Ek ağ PowerShell komut dosyası örnekleri bulunabilir [Azure ağ genel görünümü belgelerine](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
