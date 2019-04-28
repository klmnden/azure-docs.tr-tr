---
title: include dosyası
description: include dosyası
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 02/21/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 922ac7eb6cb9676af65700a6a2fe7fbae35a0dc5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60366410"
---
Bu görev için adımları aşağıdaki yapılandırma başvuru listesinde değerlere göre sanal ağ kullanın. Ayrıca ek ayarların ve adların bu listede özetlenmiştir. Bu listedeki değerlere göre değişkenler ekleyeceğiz ancak Biz bu liste adımları, doğrudan hiçbirini kullanmayın. Bir başvuru olarak kullanmak için listedeki değerleri kendi değerlerinizle değiştirerek kopyalayabilirsiniz.

* Sanal ağ adı "TestVNet" =
* Sanal ağ adres alanı 192.168.0.0/16 =
* Kaynak grubu "TestRG" =
* Subnet1 Name = "FrontEnd" 
* Subnet1 adres alanı = "192.168.1.0/24"
* Ağ geçidi alt ağ adı: Gereken her zaman adını bir ağ geçidi alt ağı "GatewaySubnet" *GatewaySubnet*.
* Ağ geçidi alt ağ adres alanı = "192.168.200.0/26"
* Bölge "Doğu ABD" =
* Ağ geçidi adı "GW" =
* Ağ geçidi IP adı "GWIP" =
* Ağ geçidi IP Yapılandırması adı "gwipconf" =
* Tür = "ExpressRoute" Bu tür bir ExpressRoute yapılandırma için gereklidir.
* Ağ geçidi genel IP adı "gwpip" =

## <a name="add-a-gateway"></a>Ağ geçidi ekleme
1. Azure aboneliğinize bağlayın.

   [!INCLUDE [Sign in](expressroute-cloud-shell-connect.md)]
2. Bu alıştırma için değişkenlerinizi bildirin. Kullanmak istediğiniz ayarları yansıtacak şekilde örneği düzenlemek emin olun.

   ```azurepowershell-interactive 
   $RG = "TestRG"
   $Location = "East US"
   $GWName = "GW"
   $GWIPName = "GWIP"
   $GWIPconfName = "gwipconf"
   $VNetName = "TestVNet"
   ```
3. Sanal ağ nesnesini bir değişken olarak Store.

   ```azurepowershell-interactive
   $vnet = Get-AzVirtualNetwork -Name $VNetName -ResourceGroupName $RG
   ```
4. Bir ağ geçidi alt ağı, sanal ağınıza ekleyin. Ağ geçidi alt ağı "GatewaySubnet" olarak adlandırılmalıdır. / 27 bir ağ geçidi alt ağı oluşturmanız gerekir ya da daha büyük (/ 26, / 25 vb..).

   ```azurepowershell-interactive
   Add-AzVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26
   ```
5. Yapılandırmayı ayarlayın.

   ```azurepowershell-interactive
   $vnet = Set-AzVirtualNetwork -VirtualNetwork $vnet
   ```
6. Ağ geçidi alt ağı, bir değişken olarak Store.

   ```azurepowershell-interactive
   $subnet = Get-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
   ```
7. Genel bir IP adresi isteyin. IP adresi, ağ geçidini oluşturmadan önce istenir. Kullanmak istediğiniz IP adresi belirtilemez; dinamik olarak ayrılır. Sonraki yapılandırma bölümünde bu IP adresini kullanacaksınız. AllocationMethod dinamik olması gerekir.

   ```azurepowershell-interactive
   $pip = New-AzPublicIpAddress -Name $GWIPName  -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
   ```
8. Ağ geçidi yapılandırmasını oluşturun. Ağ geçidi yapılandırması, kullanılacak alt ağı ve genel IP adresini tanımlar. Bu adımda, ağ geçidi oluşturduğunuzda, kullanılacak yapılandırma belirtiyorsunuz. Bu adım ağ geçidi nesnesinin oluşturmaz. Aşağıdaki örneği kullanarak kendi ağ geçidi yapılandırmanızı oluşturun.

   ```azurepowershell-interactive
   $ipconf = New-AzVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
   ```
9. Ağ geçidi oluşturun. Bu adımda, **- GatewayType** özellikle önemlidir. Değer kullanmalısınız **ExpressRoute**. Bu cmdlet'leri çalıştırdıktan sonra ağ geçidinin 45 dakika veya oluşturmak için daha fazla sürebilir.

   ```azurepowershell-interactive
   New-AzVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard
   ```

## <a name="verify-the-gateway-was-created"></a>Ağ geçidinin oluşturulduğunu doğrulayın.
Ağ geçidinin oluşturulduğunu doğrulamak için aşağıdaki komutları kullanın:

```azurepowershell-interactive
Get-AzVirtualNetworkGateway -ResourceGroupName $RG
```

## <a name="resize-a-gateway"></a>Bir ağ geçidini yeniden boyutlandırın
Bir dizi vardır [ağ geçidi SKU'ları](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Herhangi bir zamanda ağ geçidi SKU'suna değiştirmek için aşağıdaki komutu kullanabilirsiniz.

> [!IMPORTANT]
> Bu komut, UltraPerformance ağ geçidi çalışmaz. Ağ geçidinize bir UltraPerformance ağ geçidi değiştirmek için önce var olan ExpressRoute ağ geçidini kaldırın ve ardından yeni bir UltraPerformance ağ geçidi oluşturun. Bir UltraPerformance ağ geçidi geçidinizden düşürmek için ilk UltraPerformance ağ geçidi kaldırın ve ardından yeni bir ağ geçidi oluşturun.
> 
> 

```azurepowershell-interactive
$gw = Get-AzVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="remove-a-gateway"></a>Ağ geçitlerini kaldırma
Bir ağ geçidini kaldırmak için aşağıdaki komutu kullanın:

```azurepowershell-interactive
Remove-AzVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
```
