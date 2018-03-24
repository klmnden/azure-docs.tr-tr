---
title: include dosyası
description: include dosyası
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 03/22/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 17edbef03f1e2882bd85f5a58e2a32a1541b50c8
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
Bu görev için adımlar aşağıdaki yapılandırma başvuru listesinde değerlere dayalı bir sanal ağ kullanın. Ayrıca ek ayarlar ve adları bu listede özetlenmiştir. Bu listedeki değerlerin temelinde değişkenleri eklediğimiz ancak Biz bu listeyi adımları, doğrudan hiçbirinde kullanmayın. Değerleri kendinizinkilerle değiştirerek bir başvuru olarak kullanılacak listesini kopyalayabilirsiniz.

**Yapılandırma başvuru listesi**

* Virtual Network Name = "TestVNet"
* Sanal ağ adres alanı 192.168.0.0/16 =
* Resource Group = "TestRG"
* Subnet1 Name = "Ön uç" 
* Subnet1 adres alanı "192.168.1.0/24" =
* Ağ geçidi alt ağ adı: "GatewaySubnet gerekir her zaman adını bir ağ geçidi alt ağı" *GatewaySubnet*.
* Ağ geçidi alt ağ adres alanının "192.168.200.0/26" =
* Bölge "Doğu ABD" =
* Ağ geçidi adı "GW" =
* Ağ geçidi IP adı "GWIP" =
* Ağ geçidi IP Yapılandırması adı "gwipconf" =
* Tür = "ExpressRoute" Bu tür bir ExpressRoute yapılandırma için gereklidir.
* Ağ geçidi genel IP adı "gwpip" =

## <a name="add-a-gateway"></a>Bir ağ geçidi Ekle
1. Azure aboneliğinize bağlanma.

  ```powershell 
  Login-AzureRmAccount
  Get-AzureRmSubscription 
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
2. Bu alıştırma için değişkenleri bildirin. Kullanmak istediğiniz ayarları yansıtacak şekilde örneği düzenlemek emin olun.

  ```powershell 
  $RG = "TestRG"
  $Location = "East US"
  $GWName = "GW"
  $GWIPName = "GWIP"
  $GWIPconfName = "gwipconf"
  $VNetName = "TestVNet"
  ```
3. Sanal ağ nesnesini bir değişken olarak depolar.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
  ```
4. Bir ağ geçidi alt ağı, sanal ağınıza ekleyin. Ağ geçidi alt ağı "GatewaySubnet" şeklinde adlandırılmalıdır. / 27 bir ağ geçidi alt ağı oluşturmanız gerekir veya daha büyük (/ 26, / 25 vb..).

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26
  ```
5. Yapılandırmayı ayarlayın.

  ```powershell
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
6. Ağ geçidi alt ağı bir değişken olarak depolar.

  ```powershell
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
  ```
7. Genel bir IP adresi isteyin. IP adresi ağ geçidi oluşturmadan önce isteniyor. Kullanmak istediğiniz IP adresini belirtemezsiniz; dinamik olarak ayrılır. Sonraki yapılandırma bölümünde bu IP adresini kullanacaksınız. AllocationMethod dinamik olması gerekir.

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name $GWIPName  -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
  ```
8. Ağ geçidi yapılandırmasını oluşturun. Ağ geçidi yapılandırması, kullanılacak alt ağı ve genel IP adresini tanımlar. Bu adımda, ağ geçidi oluşturduğunuzda, kullanılacak yapılandırma belirtiyorsanız. Bu adım ağ geçidi nesnesi oluşturmaz. Aşağıdaki örneği kullanarak kendi ağ geçidi yapılandırmanızı oluşturun.

  ```powershell
  $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
  ```
9. Ağ geçidi oluşturun. Bu adımda, **- GatewayType** özellikle önemlidir. Değer kullanmalıdır **ExpressRoute**. Bu cmdlet'ler çalıştırdıktan sonra ağ geçidi 45 dakika veya oluşturmak için daha fazla sürebilir.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard
  ```

## <a name="verify-the-gateway-was-created"></a>Ağ geçidinin oluşturulduğunu doğrulayın
Ağ geçidinin oluşturulduğunu doğrulamak için aşağıdaki komutları kullanın:

```powershell
Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG
```

## <a name="resize-a-gateway"></a>Bir ağ geçidi yeniden boyutlandırma
Bir dizi vardır [ağ geçidi SKU'ları](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Ağ geçidi SKU'su herhangi bir zamanda değiştirmek için aşağıdaki komutu kullanabilirsiniz.

> [!IMPORTANT]
> Bu komut için UltraPerformance ağ geçidi çalışmıyor. UltraPerformance ağ geçidi için ağ geçidiniz değiştirmek için önce varolan ExpressRoute ağ geçidi kaldırın ve yeni UltraPerformance ağ geçidi oluşturmak. Ağ geçidiniz UltraPerformance geçidinden düşürmek için ilk UltraPerformance ağ geçidi kaldırın ve ardından yeni bir ağ geçidi oluşturun.
> 
> 

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="remove-a-gateway"></a>Bir ağ geçidi kaldırma
Bir ağ geçidini kaldırmak için aşağıdaki komutu kullanın:

```powershell
Remove-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
```