---
title: Azure PowerShell kullanarak bir VM için bağlantı noktalarını açmak | Microsoft Docs
description: Bir bağlantı noktasını açmak bir uç noktası, Windows Azure PowerShell ve Azure resource manager dağıtım modunu kullanarak VM oluşturma hakkında bilgi edinin
services: virtual-machines-windows
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
ms.assetid: cf45f7d8-451a-48ab-8419-730366d54f1e
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/13/2017
ms.author: iainfou
ms.openlocfilehash: b8eb36e4abad12cb46493846a024da00f9146d40
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="how-to-open-ports-and-endpoints-to-a-vm-in-azure-using-powershell"></a>Bağlantı noktaları ve uç noktaları bir VM için Azure PowerShell kullanarak nasıl açılacağı
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Hızlı komutlar
Bir ağ güvenlik grubu oluşturup ACL kuralları ihtiyacınız [Azure PowerShell'in yüklü en son sürümünü](/powershell/azureps-cmdlets-docs). Ayrıca [Azure Portalı'nı kullanarak bu adımları uygulamadan](nsg-quickstart-portal.md).

Azure hesabınızda oturum açın:

```powershell
Connect-AzureRmAccount
```

Aşağıdaki örneklerde, parametre adları kendi değerlerinizle değiştirin. Örnek parametre adları dahil *myResourceGroup*, *myNetworkSecurityGroup*, ve *myVnet*.

Bir kural oluştururken [yeni AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig). Aşağıdaki örnek, adında bir kural oluşturur *myNetworkSecurityGroupRule* izin vermek için *tcp* bağlantı noktasında trafik *80*:

```powershell
$httprule = New-AzureRmNetworkSecurityRuleConfig `
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

Ardından, ağ güvenlik grubu ile oluşturma [yeni AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) ve yeni oluşturduğunuz gibi HTTP kural atayın. Aşağıdaki örnek adlı bir ağ güvenlik grubu oluşturur *myNetworkSecurityGroup*:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName "myResourceGroup" `
    -Location "EastUS" `
    -Name "myNetworkSecurityGroup" `
    -SecurityRules $httprule
```

Şimdi şimdi bir alt ağ için ağ güvenlik grubu atayın. Aşağıdaki örnek adlı varolan bir sanal ağ atar *myVnet* değişkenine *$vnet* ile [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork):

```powershell
$vnet = Get-AzureRmVirtualNetwork `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

Ağ güvenlik grubu, alt ağ ile ilişkilendirmek [kümesi AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig). Aşağıdaki örnek adlı alt ağın ilişkilendirir *mySubnet* ağ güvenlik grubu ile:

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

Son olarak, sanal ağınızla güncelleştirme [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) sırada, değişikliklerin etkili olması:

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a>Ağ güvenlik grupları hakkında daha fazla bilgi
Burada hızlı komutlar VM'nize akan trafikle başlamak ve çalıştırmak için olanak sağlar. Ağ güvenlik grupları birçok harika özellikler ve kaynaklarınıza erişimi denetlemek için ayrıntı düzeyi sağlar. Daha fazla bilgi edinebilirsiniz [bir ağ güvenlik grubu ve ACL oluşturma kuralları burada](tutorial-virtual-network.md#secure-network-traffic).

Yüksek oranda kullanılabilir web uygulamaları için Azure yük dengeleyici arkasında Vm'leriniz yerleştirmeniz gerekir. Yük Dengeleyici trafik filtreleme sağlar bir ağ güvenlik grubu olan VM'ler için trafiği dağıtır. Daha fazla bilgi için bkz: [nasıl yükleneceğini Linux sanal makineleri yüksek oranda kullanılabilir bir uygulama oluşturmak için Azure Bakiye](tutorial-load-balancer.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekte, HTTP trafiğine izin veren basit bir kural oluşturuldu. Aşağıdaki makalelerde ayrıntılı ortamları oluşturma hakkında bilgi bulabilirsiniz:

* [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md)
* [Ağ Güvenlik Grubu (NSG) Nedir?](../../virtual-network/virtual-networks-nsg.md)
* [Yük Dengeleyiciler için Azure Resource Manager'a genel bakış](../../load-balancer/load-balancer-arm.md)

