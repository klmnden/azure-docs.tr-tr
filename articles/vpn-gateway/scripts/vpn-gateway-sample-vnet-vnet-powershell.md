---
title: Azure PowerShell betiği örneği - sanal ağlar arası VPN yapılandırma | Microsoft Docs
description: Siteden siteye VPN’yi yapılandırın.
services: vpn-gateway
documentationcenter: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.devlang: powershell
ms.topic: sample
ms.date: 04/30/2018
ms.author: alzam
ms.openlocfilehash: 0d49e65ccfd13637a2e7313a5ec3a48b2ce900ca
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66113641"
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-powershell"></a>PowerShell kullanarak sanal ağlar arası VPN ağ geçidi bağlantısı yapılandırma

Bu betik, sanal ağlar arası bağlantı türünü kullanarak iki sanal ağı bağlar.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

```azurepowershell-interactive
# Declare variables for VNET 1
  $RG1 = "TestRG1"
  $VNetName1  = "VNet1"
  $FESubName1 = "FrontEnd"
  $GWSubName1 = "GatewaySubnet"
  $VNetPrefix11 = "10.1.0.0/16"
  $FESubPrefix1 = "10.1.0.0/24"
  $GWSubPrefix1 = "10.1.255.0/27"
  $Location1 = "EastUS"
  $GWName1 = "VNet1GW"
  $GWIPName1 = "VNet1GWIP"
  $GWIPconfName1 = "gwipconfig1"
  $Connection12 = "VNet1toVNet2"

# Declare variables for VNET 2  
  $RG2 = "TestRG2"
  $VNetName2  = "VNet2"
  $FESubName2 = "FrontEnd"
  $GWSubName2 = "GatewaySubnet"
  $VNetPrefix21 = "10.2.0.0/16"
  $FESubPrefix2 = "10.2.0.0/24"
  $GWSubPrefix2 = "10.2.255.0/27"
  $Location2 = "EastUS"
  $GWName2 = "VNet2GW"
  $GWIPName2 = "VNet2GWIP"
  $GWIPconfName2 = "gwipconfig2"
  $Connection21 = "VNet2toVNet1"

# Create first resource group
New-AzResourceGroup -Name $RG1 -Location $Location1

# Create a virtual network 1
$virtualNetwork1 = New-AzVirtualNetwork `
  -ResourceGroupName $RG1 `
  -Location $Location1 `
  -Name $VNetName1 `
  -AddressPrefix $VNetPrefix11

# Create a subnet configuration
Add-AzVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1 -VirtualNetwork $virtualNetwork1

# Set the subnet configuration for virtual network 1
$virtualNetwork1 | Set-AzVirtualNetwork

# Add a gateway subnet
Add-AzVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1 -VirtualNetwork $virtualNetwork1

# Set the subnet configuration for the virtual network
$virtualNetwork1 | Set-AzVirtualNetwork

# Request a public IP address
$gwpip1= New-AzPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 `
 -AllocationMethod Dynamic

# Create the gateway IP address configuration
$vnet1 = Get-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzVirtualNetworkSubnetConfig -Name $GWSubName1 -VirtualNetwork $vnet1
$gwipconfig1 = New-AzVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -SubnetId $subnet1.Id -PublicIpAddressId $gwpip1.Id

# Create the VPN gateway (takes 20-40 minutes)
New-AzVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 `
 -Location $Location1 -IpConfigurations $gwipconfig1 -GatewayType Vpn `
 -VpnType RouteBased -GatewaySku VpnGw1 

# Create the second resource group
New-AzResourceGroup -Name $RG2 -Location $Location2

# Create a virtual network 2
$virtualNetwork2 = New-AzVirtualNetwork `
  -ResourceGroupName $RG2 `
  -Location $Location2 `
  -Name $VNetName2 `
  -AddressPrefix $VNetPrefix21

# Create a subnet configuration
Add-AzVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2 -VirtualNetwork $virtualNetwork2

# Set the subnet configuration for virtual network 2
$virtualNetwork2 | Set-AzVirtualNetwork

# Add a gateway subnet
Add-AzVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2 -VirtualNetwork $virtualNetwork2

# Set the subnet configuration for the virtual network
$virtualNetwork2 | Set-AzVirtualNetwork

# Request a public IP address
$gwpip2 = New-AzPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 `
 -AllocationMethod Dynamic

# Create the gateway IP address configuration
$vnet2 = Get-AzVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2 = Get-AzVirtualNetworkSubnetConfig -Name $GWSubName2 -VirtualNetwork $vnet2
$gwipconfig2 = New-AzVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -SubnetId $subnet2.Id -PublicIpAddressId $gwpip2.Id

# Create the VPN gateway (takes 20-40 minutes)
New-AzVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 `
 -Location $Location2 -IpConfigurations $gwipconfig2 -GatewayType Vpn `
 -VpnType RouteBased -GatewaySku VpnGw1

# Create the connections
$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet2gw = Get-AzVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
New-AzVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 `
-VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 `
-ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
New-AzVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 `
-VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 `
-ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
 ```
 
## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturduğunuz kaynaklara artık ihtiyacınız olduğunda kullanın [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) kaynak grubunu silmek için komutu. Böylece, kaynak grupları ve içerdiği tüm kaynaklar silinir.

```azurepowershell-interactive
Remove-AzResourceGroup -Name TestRG1
Remove-AzResourceGroup -Name TestRG2
```
 
 
## <a name="script-explanation"></a>Betik açıklaması

Bu betik, dağıtımı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [AzVirtualNetworkSubnetConfig ekleyin](/powershell/module/az.network/add-azvirtualnetworksubnetconfig) | Bir alt ağ yapılandırması ekler. Bu yapılandırma, sanal ağ oluşturma işlemiyle birlikte kullanılır. |
| [Get-AzVirtualNetwork](/powershell/module/az.network/get-azvirtualnetwork) | Sanal ağ ayrıntılarını alır. |
| [Get-AzVirtualNetworkGateway](/powershell/module/az.network/get-azvirtualnetworkgateway) | Sanal ağ geçidi ayrıntılarını alır. |
| [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/get-azvirtualnetworksubnetconfig) | Sanal ağ alt ağ yapılandırma ayrıntılarını alır. |
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Yeni AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | Bir alt ağ yapılandırması oluşturur. Bu yapılandırma, sanal ağ oluşturma işlemiyle birlikte kullanılır. |
| [Yeni AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Sanal ağ oluşturur. |
| [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | Genel bir IP adresi oluşturur. |
| [Yeni AzVirtualNetworkGatewayIpConfig](/powershell/module/az.network/new-azvirtualnetworkgatewayipconfig) | Yeni bir ağ geçidi ip yapılandırması oluşturur. |
| [Yeni AzVirtualNetworkGateway](/powershell/module/az.network/new-azvirtualnetworkgateway) | VPN Gateway oluşturur. |
| [Yeni AzVirtualNetworkGatewayConnection](/powershell/module/az.network/new-azvirtualnetworkgatewayconnection) | Sanal ağlar arasında bağlantı oluşturur. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. |
| [Set-AzVirtualNetwork](/powershell/module/az.network/set-azvirtualnetwork) | Sanal ağ için alt ağ yapılandırmasını ayarlar. |
| [Set-AzVirtualNetworkGateway](/powershell/module/az.network/set-azvirtualnetworkgateway) | VPN Gateway için yapılandırmayı ayarlar. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).
