---
title: "Azure PowerShell Betiği örnek - yük dengelemesi Azure PowerShell ile birden çok Web siteleri | Microsoft Docs"
description: "Azure PowerShell Betiği örnek - yükünü dengelemek için aynı sanal makine birden çok Web sitesi"
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
ms.openlocfilehash: d73cdc98ff279c3ee1b93443abe4b6c7c97786a2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="load-balance-multiple-websites"></a>Yük Dengelemesi birden çok Web sitesi

Bu komut dosyası örneği iki sanal bir kullanılabilirlik kümesi üyesi olan makinelerle (VM) bir sanal ağ oluşturur. Bir yük dengeleyici iki VM için iki ayrı IP adresleri için trafiğini yönlendirir. Komut dosyasını çalıştırdıktan sonra web sunucusu yazılımı VM'ler ve ana bilgisayar, her biri kendi IP adresiyle birden çok web sitelerini dağıtabilirsiniz.

Gerekirse, bulunan yönergeleri kullanarak Azure PowerShell'i yükleme [Azure PowerShell Kılavuzu](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)ve ardından çalıştırın `Login-AzureRmAccount` Azure ile bir bağlantı oluşturmak için.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.ps1  "Load balance multiple web sites")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, bir kaynak grubu, sanal ağ, yük dengeleyici ve tüm ilişkili kaynakları oluşturmak için aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [Yeni-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [AzureRmAvailabilitySet yeni](/powershell/module/azurerm.compute/new-azurermavailabilityset) | Yüksek kullanılabilirlik sağlamak üzere bir Azure kullanılabilirlik kümesi oluşturur. |
| [AzureRmVirtualNetworkSubnetConfig yeni](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Bir alt ağ yapılandırması oluşturur. Bu yapılandırma sanal ağ oluşturma işlemine kullanılır. |
| [Yeni-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Sanal ağ oluşturur. |
| [AzureRmPublicIpAddress yeni](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Bir ortak IP adresi oluşturur. |
| [AzureRmLoadBalancerFrontendIpConfig yeni](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) | Bir yük dengeleyici için bir ön uç IP yapılandırması oluşturur. |
| [AzureRmLoadBalancerBackendAddressPoolConfig yeni](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) | Bir yük dengeleyici için arka uç adres havuzu yapılandırması oluşturur. |
| [AzureRmLoadBalancerProbeConfig yeni](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | Bir NLB araştırması oluşturur. Bir NLB araştırması NLB kümesindeki her bir VM izlemek için kullanılır. Tüm VM erişilemez hale gelirse VM trafik yönlendirilmez. |
| [AzureRmLoadBalancerRuleConfig yeni](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | Bir NLB kuralı oluşturur. Bu örnekte, bağlantı noktası 80 için bir kural oluşturulur. NLB HTTP trafiği ulaşan gibi 80 numaralı bağlantı noktasına yönlendirilir NLB kümesindeki sanal makineleri biri. |
| [AzureRmLoadBalancer yeni](/powershell/module/azurerm.network/new-azurermloadbalancer) | Bir yük dengeleyici oluşturur. |
| [AzureRmNetworkInterfaceIpConfig yeni](/powershell/module/azurerm.network/new-azurermnetworkinterfaceipconfig) | Bir sanal ağ arabirimi için gelişmiş özelliklerini tanımlar. |
| [AzureRmNetworkInterface yeni](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Bir ağ arabirimi oluşturur. |
| [AzureRmVMConfig yeni](/powershell/module/azurerm.compute/new-azurermvmconfig) | Bir VM yapılandırması oluşturur. Bu yapılandırma VM adı, işletim sistemi ve yönetici kimlik bilgileri gibi bilgileri içerir. Yapılandırma VM oluşturma sırasında kullanılır. |
| [Yeni-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Bir sanal makine oluşturun. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Bir kaynak grubu ve içerdiği tüm kaynaklar kaldırır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](https://docs.microsoft.com/powershell/azure/overview).

Ek ağ PowerShell komut dosyası örnekleri bulunabilir [Azure ağ genel görünümü belgelerine](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
