---
title: 'Rota temelli bir Azure VPN gateway oluşturun: PowerShell | Microsoft Docs'
description: Bir rota tabanlı VPN Gateway'de PowerShell kullanarak hızlıca oluşturun
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 02/11/2019
ms.author: cherylmc
ms.openlocfilehash: 3ad391aae72bec392ae75b333fc3bfc1ff3f95f1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66727330"
---
# <a name="create-a-route-based-vpn-gateway-using-powershell"></a>PowerShell kullanarak rota temelli VPN ağ geçidi oluşturma

Bu makalede, PowerShell kullanarak rota temelli Azure VPN ağ geçidi hızlı bir şekilde oluşturmanıza yardımcı olur. Bir VPN ağ geçidi, şirket içi ağınıza bir VPN bağlantısı oluşturulurken kullanılır. Vnet'leri bağlamak için bir VPN ağ geçidi'ni de kullanabilirsiniz.

Bu makaledeki adımlarda, sanal ağ, bir alt ağ, ağ geçidi alt ağı ve rota tabanlı VPN ağ geçidi (sanal ağ geçidi) oluşturur. Ağ geçidi oluşturma tamamlandıktan sonra ardından bağlantıları oluşturabilirsiniz. Bu adımlar, bir Azure aboneliği gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bir Azure kaynak grubu oluşturun [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

```azurepowershell-interactive
New-AzResourceGroup -Name TestRG1 -Location EastUS
```

## <a name="vnet"></a>Sanal ağ oluşturma

İle sanal ağ oluşturma [yeni AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork). Aşağıdaki örnekte adlı bir sanal ağ oluşturur **VNet1** içinde **EastUS** konumu:

```azurepowershell-interactive
$virtualNetwork = New-AzVirtualNetwork `
  -ResourceGroupName TestRG1 `
  -Location EastUS `
  -Name VNet1 `
  -AddressPrefix 10.1.0.0/16
```

Kullanarak bir alt ağ yapılandırması oluşturun [yeni AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) cmdlet'i.

```azurepowershell-interactive
$subnetConfig = Add-AzVirtualNetworkSubnetConfig `
  -Name Frontend `
  -AddressPrefix 10.1.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

Kullanarak sanal ağ için alt ağ yapılandırması ayarlayın [kümesi AzVirtualNetwork](/powershell/module/az.network/Set-azVirtualNetwork) cmdlet'i.


```azurepowershell-interactive
$virtualNetwork | Set-AzVirtualNetwork
```

## <a name="gwsubnet"></a>Bir ağ geçidi alt ağı ekleme

Ağ geçidi alt ağı sanal ağ geçidi hizmetlerinin kullandığı ayrılmış IP adreslerini içerir. Bir ağ geçidi alt ağı eklemek için aşağıdaki örnekleri kullanın:

Vnet'inizi için bir değişken ayarlayın.

```azurepowershell-interactive
$vnet = Get-AzVirtualNetwork -ResourceGroupName TestRG1 -Name VNet1
```

Ağ geçidi alt ağı kullanarak oluşturduğunuz [Ekle AzVirtualNetworkSubnetConfig](/powershell/module/az.network/Add-azVirtualNetworkSubnetConfig) cmdlet'i.

```azurepowershell-interactive
Add-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.1.255.0/27 -VirtualNetwork $vnet
```

Kullanarak sanal ağ için alt ağ yapılandırması ayarlayın [kümesi AzVirtualNetwork](/powershell/module/az.network/Set-azVirtualNetwork) cmdlet'i.

```azurepowershell-interactive
$virtualNetwork | Set-AzVirtualNetwork
```

## <a name="PublicIP"></a>Genel bir IP adresi isteme

Bir VPN ağ geçidi, dinamik olarak ayrılan bir genel IP adresi olmalıdır. Bir VPN ağ geçidine bir bağlantı oluşturduğunuzda, belirttiğiniz IP adresi budur. Genel bir IP adresi istemek için aşağıdaki örneği kullanın:

```azurepowershell-interactive
$gwpip= New-AzPublicIpAddress -Name VNet1GWIP -ResourceGroupName TestRG1 -Location 'East US' -AllocationMethod Dynamic
```

## <a name="GatewayIPConfig"></a>Ağ geçidi IP adresi yapılandırması oluşturma

Ağ geçidi yapılandırması, kullanılacak alt ağı ve genel IP adresini tanımlar. Ağ geçidi yapılandırmanızı oluşturmak için aşağıdaki örneği kullanın:

```azurepowershell-interactive
$vnet = Get-AzVirtualNetwork -Name VNet1 -ResourceGroupName TestRG1
$subnet = Get-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gwipconfig = New-AzVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
```
## <a name="CreateGateway"></a>VPN ağ geçidi oluşturma

VPN ağ geçidinin oluşturulması 45 dakika veya daha uzun sürebilir. Ağ geçidi tamamlandıktan sonra sanal ağınız ile başka bir sanal ağ arasında bir bağlantı oluşturabilirsiniz. Veya sanal ağınız ile şirket içi konumunuz arasında bir bağlantı oluşturun. Kullanarak bir VPN ağ geçidi oluşturma [yeni AzVirtualNetworkGateway](/powershell/module/az.network/New-azVirtualNetworkGateway) cmdlet'i.

```azurepowershell-interactive
New-AzVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1 `
-Location 'East US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased -GatewaySku VpnGw1
```

## <a name="viewgw"></a>VPN ağ geçidi görüntüleyin

VPN ağ geçidi kullanarak görüntüleyebileceğiniz [Get-AzVirtualNetworkGateway](/powershell/module/az.network/Get-azVirtualNetworkGateway) cmdlet'i.

```azurepowershell-interactive
Get-AzVirtualNetworkGateway -Name Vnet1GW -ResourceGroup TestRG1
```

Çıktı şu örneğe benzer görünecektir:

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

## <a name="viewgwpip"></a>Genel IP adresini görüntüleyin

VPN ağ geçidiniz için genel IP adresini görüntülemek için kullanın [Get-AzPublicIpAddress](/powershell/module/az.network/Get-azPublicIpAddress) cmdlet'i.

```azurepowershell-interactive
Get-AzPublicIpAddress -Name VNet1GWIP -ResourceGroupName TestRG1
```

Örnek yanıt olarak, genel IP adresini IPADDRESS değerdir.

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

Oluşturduğunuz kaynaklara artık ihtiyacınız olduğunda kullanın [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) kaynak grubunu silmek için komutu. Böylece, kaynak grubu ve içerdiği tüm kaynaklar silinir.

```azurepowershell-interactive
Remove-AzResourceGroup -Name TestRG1
```

## <a name="next-steps"></a>Sonraki adımlar

Ağ geçidi oluşturma tamamlandıktan sonra başka bir sanal ağ ve sanal ağınız arasında bir bağlantı oluşturabilirsiniz. Veya sanal ağınız ile şirket içi konumunuz arasında bir bağlantı oluşturun.

> [!div class="nextstepaction"]
> [Siteden siteye bağlantı oluşturma](vpn-gateway-create-site-to-site-rm-powershell.md)<br><br>
> [Noktadan siteye bağlantı oluşturma](vpn-gateway-howto-point-to-site-rm-ps.md)<br><br>
> [Başka bir sanal ağa bağlantı oluşturma](vpn-gateway-vnet-vnet-rm-ps.md)
