---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/28/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4ee182202cf1ecbbb0845541269f7241de26c170
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
ms.locfileid: "30326560"
---
### <a name="gwipnoconnection"></a> 'GatewayIpAddress' yerel ağ geçidini değiştirmek için - ağ geçidi bağlantısı yok

Bağlanmak istediğiniz VPN cihazının genel IP adresi değiştiyse, yerel ağ geçidini bu değişikliği yansıtacak şekilde değiştirmeniz gerekir. Ağ geçidi bağlantısı olmayan bir yerel ağ geçidini değiştirmek için örneği kullanın.

Bu değeri değiştirirken aynı zamanda adres ön eklerini de değiştirebilirsiniz. Geçerli ayarların üzerine yazmak için yerel ağ geçidinizin mevcut adını kullandığınızdan emin olun. Farklı bir ad kullanırsanız mevcut olanın üzerine yazmak yerine yeni bir yerel ağ geçidi oluşturursunuz.

```azurepowershell-interactive
New-AzureRmLocalNetworkGateway -Name Site1 `
-Location "East US" -AddressPrefix @('10.101.0.0/24','10.101.1.0/24') `
-GatewayIpAddress "5.4.3.2" -ResourceGroupName TestRG1
```

### <a name="gwipwithconnection"></a> 'GatewayIpAddress' yerel ağ geçidini değiştirmek için - ağ geçidi bağlantısı var

Bağlanmak istediğiniz VPN cihazının genel IP adresi değiştiyse, yerel ağ geçidini bu değişikliği yansıtacak şekilde değiştirmeniz gerekir. Zaten bir ağ geçidi bağlantısı varsa öncelikle bu bağlantıyı kaldırmanız gerekir. Bağlantı kaldırıldıktan sonra ağ geçidi IP adresini değiştirebilir ve yeni bir bağlantı oluşturabilirsiniz. Aynı zamanda adres ön eklerini de değiştirebilirsiniz. Bunun sonucunda, VPN bağlantınızda kesinti oluşur. Ağ geçidi IP adresini değiştirirken, VPN ağ geçidini silmeniz gerekmez. Yalnızca bağlantıyı kaldırmanız gerekir.
 

1. Bağlantıyı kaldırın. 'Get-AzureRmVirtualNetworkGatewayConnection' cmdlet’ini kullanarak bağlantınızın adını bulabilirsiniz.

  ```azurepowershell-interactive
  Remove-AzureRmVirtualNetworkGatewayConnection -Name VNet1toSite1 `
  -ResourceGroupName TestRG1
  ```
2. 'GatewayIpAddress' değerini değiştirin. Aynı zamanda adres ön eklerini de değiştirebilirsiniz. Geçerli ayarların üzerine yazmak için yerel ağ geçidinizin mevcut adını kullandığınızdan emin olun. Bunu kullanmazsanız, mevcut olanın üzerine yazmak yerine yeni bir yerel ağ geçidi oluşturursunuz.

  ```azurepowershell-interactive
  New-AzureRmLocalNetworkGateway -Name Site1 `
  -Location "East US" -AddressPrefix @('10.101.0.0/24','10.101.1.0/24') `
  -GatewayIpAddress "104.40.81.124" -ResourceGroupName TestRG1
  ```
3. Bağlantıyı oluşturun. Bu örnekte bir IPsec bağlantı türü yapılandırıyoruz. Bağlantınızı yeniden oluşturduktan sonra, yapılandırmanız için belirtilen bağlantı türünü kullanın. Ek bağlantı türleri için [PowerShell cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) sayfasına bakın.  VirtualNetworkGateway adını almak için 'Get-AzureRmVirtualNetworkGateway' cmdlet'ini çalıştırabilirsiniz.
   
    Değişkenleri ayarlayın.

  ```azurepowershell-interactive
  $local = Get-AzureRMLocalNetworkGateway -Name Site1 -ResourceGroupName TestRG1 `
  $vnetgw = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1
  ```
   
    Bağlantıyı oluşturun.

  ```azurepowershell-interactive 
  New-AzureRmVirtualNetworkGatewayConnection -Name VNet1Site1 -ResourceGroupName TestRG1 `
  -Location "East US" `
  -VirtualNetworkGateway1 $vnetgw `
  -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```