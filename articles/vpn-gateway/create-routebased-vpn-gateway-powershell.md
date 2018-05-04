---
title: 'Rota tabanlı Azure VPN ağ geçidi oluşturmak: PowerShell | Microsoft Docs'
description: Hızlı rota tabanlı VPN PowerShell kullanarak ağ geçidi oluşturma
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/04/2018
ms.author: cherylmc
ms.openlocfilehash: efa07a68cda60ea2d8256a8d068639305f7f4c86
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="create-a-route-based-vpn-gateway-using-powershell"></a>PowerShell kullanarak bir rota tabanlı VPN ağ geçidi oluşturma

Bu makalede PowerShell kullanarak bir rota tabanlı Azure VPN ağ geçidi hızlı bir şekilde oluşturmanıza yardımcı olur. Bir VPN ağ geçidi, şirket içi ağınıza bir VPN bağlantısı oluşturulurken kullanılır. Sanal ağlara bağlanmak için bir VPN ağ geçidi'ni de kullanabilirsiniz.

Bu makaledeki adımları bir sanal ağ, bir alt ağ, bir ağ geçidi alt ağı ve rota tabanlı VPN ağ geçidi (sanal ağ geçidi) oluşturur. Ağ geçidi oluşturma tamamlandıktan sonra ardından bağlantı oluşturabilirsiniz. Bu adımları bir Azure aboneliği gerektirir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, Azure PowerShell modülü 5.3.0 veya sonraki bir sürümü gerektirir. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) ile yeni bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

```azurepowershell-interactive
New-AzureRmResourceGroup -Name TestRG1 -Location EastUS
```

## <a name="vnet"></a>Bir sanal ağ oluşturma

[New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) ile sanal ağ oluşturun. Aşağıdaki örnek adlı bir sanal ağ oluşturur **VNet1** içinde **EastUS** konumu:

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName TestRG1 `
  -Location EastUS `
  -Name VNet1 `
  -AddressPrefix 10.1.0.0/16
```

Kullanarak bir alt ağ yapılandırması oluşturma [yeni AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) cmdlet'i.

```azurepowershell-interactive
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Frontend `
  -AddressPrefix 10.1.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

Sanal ağ kullanan bir alt ağ yapılandırması ayarlayın [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork) cmdlet'i.


```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="gwsubnet"></a>Bir ağ geçidi alt ağı Ekle

Ağ geçidi alt ağı sanal ağ geçidi hizmetlerini kullanmak ayrılmış IP adresleri içerir. Aşağıdaki örnekler, bir ağ geçidi alt ağı eklemek için kullanın:

Vnet'inizi için bir değişken ayarlayın.

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG1 -Name VNet1
```

Ağ geçidi alt ağı kullanarak oluşturduğunuz [Ekle AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/Add-AzureRmVirtualNetworkSubnetConfig) cmdlet'i.

```azurepowershell-interactive
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.1.255.0/27 -VirtualNetwork $vnet
```

Sanal ağ kullanan bir alt ağ yapılandırması ayarlayın [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork) cmdlet'i.

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="PublicIP"></a>İstek bir ortak IP adresi

Bir VPN ağ geçidi dinamik olarak ayrılan bir ortak IP adresi olmalıdır. Bir VPN ağ geçidi bağlantı oluşturduğunuzda, bu, belirttiğiniz IP adresidir. Bir ortak IP adresi istemek için aşağıdaki örneği kullanın:

```azurepowershell-interactive
$gwpip= New-AzureRmPublicIpAddress -Name VNet1GWIP -ResourceGroupName TestRG1 -Location 'East US' -AllocationMethod Dynamic
```

## <a name="GatewayIPConfig"></a>Ağ geçidi IP adresi yapılandırması oluştur

Ağ geçidi yapılandırması, kullanılacak alt ağı ve genel IP adresini tanımlar. Ağ geçidi yapılandırmanızı oluşturmak için aşağıdaki örneği kullanın:

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork -Name VNet1 -ResourceGroupName TestRG1
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
```
## <a name="CreateGateway"></a>VPN ağ geçidi oluşturma

Bir VPN ağ geçidi 45 dakika veya oluşturmak için daha fazla sürebilir. Ağ geçidi tamamlandıktan sonra sanal ağınız ve başka bir VNet arasında bir bağlantı oluşturabilirsiniz. Veya, sanal ağ ve bir şirket içi konum arasında bir bağlantı oluşturun. Kullanarak bir VPN ağ geçidi oluşturma [yeni AzureRmVirtualNetworkGateway](/powershell/module/azurerm.network/New-AzureRmVirtualNetworkGateway) cmdlet'i.

```azurepowershell-interactive
New-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1 `
-Location 'East US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased -GatewaySku VpnGw1
```

## <a name="viewgw"></a>VPN ağ geçidi görüntüleyin

VPN ağ geçidi kullanarak görüntüleyebileceği [Get-AzureRmVirtualNetworkGateway](/powershell/module/azurerm.network/Get-AzureRmVirtualNetworkGateway) cmdlet'i.

```azurepowershell-interactive
Get-AzureRmVirtualNetworkGateway -Name Vnet1GW -ResourceGroup TestRG1
```

Çıktı bu örneğe benzer görünür:

```
Name                   : VNet1GW
ResourceGroupName      : TestRG1
Location               : eastus
Id                     : /subscriptions/<subscription ID>/resourceGroups/TestRG1/provide
                         rs/Microsoft.Network/virtualNetworkGateways/VNet1GW
Etag                   : W/"0952d-9da8-4d7d-a8ed-28c8ca0413"
ResourceGuid           : dc6ce1de-2c4494-9d0b-20b03ac595
ProvisioningState      : Succeeded
Tags                   :
IpConfigurations       : [
                           {
                             "PrivateIpAllocationMethod": "Dynamic",
                             "Subnet": {
                               "Id": "/subscriptions/<subscription ID>/resourceGroups/Te
                         stRG1/providers/Microsoft.Network/virtualNetworks/VNet1/subnets/GatewaySubnet"
                             },
                             "PublicIpAddress": {
                               "Id": "/subscriptions/<subscription ID>/resourceGroups/Te
                         stRG1/providers/Microsoft.Network/publicIPAddresses/VNet1GWIP"
                             },
                             "Name": "default",
                             "Etag": "W/\"0952d-9da8-4d7d-a8ed-28c8ca0413\"",
                             "Id": "/subscriptions/<subscription ID>/resourceGroups/Test
                         RG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW/ipConfigurations/de
                         fault"
                           }
                         ]
GatewayType            : Vpn
VpnType                : RouteBased
EnableBgp              : False
ActiveActive           : False
GatewayDefaultSite     : null
Sku                    : {
                           "Capacity": 2,
                           "Name": "VpnGw1",
                           "Tier": "VpnGw1"
                         }
VpnClientConfiguration : null
BgpSettings            : {
     
```

## <a name="viewgwpip"></a>Genel IP adresi görüntüleyin

VPN ağ geçidiniz için genel IP adresi görüntülemek için kullanın [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/Get-AzureRmPublicIpAddress) cmdlet'i.

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -Name VNet1GWIP -ResourceGroupName TestRG1
```

Örnek yanıt olarak, genel IP adresi IPADDRESS değerdir.

```
Name                     : VNet1GWIP
ResourceGroupName        : TestRG1
Location                 : eastus
Id                       : /subscriptions/<subscription ID>/resourceGroups/TestRG1/provi
                           ders/Microsoft.Network/publicIPAddresses/VNet1GWIP
Etag                     : W/"5001666a-bc2a-484b-bcf5-ad488dabd8ca"
ResourceGuid             : 3c7c481e-9828-4dae-abdc-f95b383
ProvisioningState        : Succeeded
Tags                     :
PublicIpAllocationMethod : Dynamic
IpAddress                : 13.90.153.3
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                             "Id": "/subscriptions/<subscription ID>/resourceGroups/Test
                           RG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW/ipConfigurations/
                           default"
                           }
DnsSettings              : null
Zones                    : {}
Sku                      : {
                             "Name": "Basic"
                           }
IpTags                   : {}
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturduğunuz kaynaklara artık ihtiyacınız olmadığında, kaynak grubunu silmek için [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanın. Böylece, kaynak grubu ve içerdiği tüm kaynaklar silinir.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name TestRG1
```

## <a name="next-steps"></a>Sonraki adımlar

Ağ geçidi oluşturmayı tamamlandıktan sonra sanal ağınız ve başka bir VNet arasında bir bağlantı oluşturabilirsiniz. Veya, sanal ağ ve bir şirket içi konum arasında bir bağlantı oluşturun.

> [!div class="nextstepaction"]
> [Siteden siteye bağlantı oluşturma](vpn-gateway-create-site-to-site-rm-powershell.md)<br><br>
> [Bir noktadan siteye bağlantı oluşturma](vpn-gateway-howto-point-to-site-rm-ps.md)<br><br>
> [Başka bir VNet bağlantı oluşturun.](vpn-gateway-vnet-vnet-rm-ps.md)
