---
title: Azure PowerShell kullanarak bir VM için bağlantı noktalarını açmak | Microsoft Docs
description: Bağlantı noktası açma / bir uç nokta için Azure resource manager dağıtım modu ve Azure PowerShell kullanarak Windows VM'nizi oluşturma hakkında bilgi edinin
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
ms.assetid: cf45f7d8-451a-48ab-8419-730366d54f1e
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/13/2017
ms.author: cynthn
ms.openlocfilehash: 2910882424326f5a09b00d31c0e0fedb45d1e5d8
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67720113"
---
# <a name="how-to-open-ports-and-endpoints-to-a-vm-in-azure-using-powershell"></a>PowerShell kullanarak Azure'da bağlantı noktalarını ve uç noktaları bir VM'ye açma
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Hızlı komutlar
Bir ağ güvenlik grubu oluşturup ACL kuralları ihtiyacınız [Azure PowerShell'in en son sürümünü](/powershell/azureps-cmdlets-docs). Ayrıca [Azure portalı kullanarak aşağıdaki adımları gerçekleştirmek](nsg-quickstart-portal.md).

Azure hesabınızda oturum açın:

```powershell
Connect-AzAccount
```

Aşağıdaki örneklerde, parametre adları kendi değerlerinizle değiştirin. Örnek parametre adları dahil *myResourceGroup*, *Vm2*, ve *myVnet*.

Bir kural oluştururken [yeni AzNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecurityruleconfig). Aşağıdaki örnekte adlı bir kural oluşturur *myNetworkSecurityGroupRule* izin vermek için *tcp* bağlantı noktası üzerinde trafiğe *80*:

```powershell
$httprule = New-AzNetworkSecurityRuleConfig `
    -Name "myNetworkSecurityGroupRule" `
    -Description "Allow HTTP" `
    -Access "Allow" `
    -Protocol "Tcp" `
    -Direction "Inbound" `
    -Priority "100" `
    -SourceAddressPrefix "Internet" `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 80
```

Ardından, ağ güvenlik grubu oluşturma [yeni AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecuritygroup) ve yeni oluşturduğunuz gibi HTTP kuralı atayın. Aşağıdaki örnekte adlı bir ağ güvenlik grubu oluşturur *Vm2*:

```powershell
$nsg = New-AzNetworkSecurityGroup `
    -ResourceGroupName "myResourceGroup" `
    -Location "EastUS" `
    -Name "myNetworkSecurityGroup" `
    -SecurityRules $httprule
```

Şimdi github'dan ağ güvenlik grubunuzu bir alt ağa atayın. Aşağıdaki örnekte adlı varolan bir sanal ağ atar *myVnet* değişkenine *$vnet* ile [Get-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetwork):

```powershell
$vnet = Get-AzVirtualNetwork `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

Alt ağınız ile ağ güvenlik grubunuzu ilişkilendirmek [kümesi AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/set-azvirtualnetworksubnetconfig). Aşağıdaki örnekte adlı alt ağ ilişkilendirir *mySubnet* , ağ güvenlik grubu ile:

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

Son olarak, sanal ağınız ile güncelleştirme [kümesi AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/set-azvirtualnetwork) sırasıyla, değişikliklerin etkili olabilmesi için:

```powershell
Set-AzVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a>Ağ güvenlik grupları hakkında daha fazla bilgi
Hızlı komutlar Burada, sanal makinenizde akan trafikle ayarlayıp çalıştırmaya başlamasına olanak tanır. Ağ güvenlik grupları, çok sayıda harika özellik ve kaynaklarınıza erişimi denetlemek için ayrıntı düzeyi sağlar. Daha fazla bilgi edinebilirsiniz [oluşturma bir ağ güvenlik grubu ve ACL kurallarını burada](tutorial-virtual-network.md#secure-network-traffic).

Yüksek oranda kullanılabilir web uygulamaları için Vm'lerinizi bir Azure yük dengeleyici arkasında yerleştirmeniz gerekir. Yük Dengeleyici trafik filtreleme sağlar bir ağ güvenlik grubu Vm'lerle trafiği dağıtır. Daha fazla bilgi için [nasıl yükleneceğini yüksek oranda kullanılabilir bir uygulama oluşturmak için azure'da Linux sanal makineleri Bakiye](tutorial-load-balancer.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekte, HTTP trafiğine izin veren basit bir kuralı oluşturuldu. Aşağıdaki makalelerde daha ayrıntılı ortamları oluşturma hakkında bilgi bulabilirsiniz:

* [Azure Resource Manager’a genel bakış](../../azure-resource-manager/resource-group-overview.md)
* [Bir ağ güvenlik grubu nedir?](../../virtual-network/security-overview.md)
* [Yük Dengeleyiciler için Azure Resource Manager'a genel bakış](../../load-balancer/load-balancer-arm.md)

