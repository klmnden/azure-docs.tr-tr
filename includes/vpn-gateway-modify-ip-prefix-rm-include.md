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
ms.openlocfilehash: 8803aada61ae58f1e221767aeb382f7d74c63eb4
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
ms.locfileid: "30326556"
---
### <a name="noconnection"></a>Yerel ağ geçidinin IP adresi ön eklerini değiştirmek için - ağ geçidi bağlantısı yok

Başka adres ön ekleri eklemek için:

```azurepowershell-interactive
$local = Get-AzureRmLocalNetworkGateway -Name Site1 -ResourceGroupName TestRG1 `
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
-AddressPrefix @('10.101.0.0/24','10.101.1.0/24','10.101.2.0/24')
```

Adres ön eklerini kaldırmak için:<br>
Artık gereği olmayan önekleri bırakın. Bu örnekte, biz artık 10.101.2.0/24 öneki yerel ağ geçidi güncelleştiriyoruz şekilde (önceki örnekten), bu önek dışında.

```azurepowershell-interactive
$local = Get-AzureRmLocalNetworkGateway -Name Site1 -ResourceGroupName TestRG1 `
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
-AddressPrefix @('10.101.0.0/24','10.101.1.0/24')
```

### <a name="withconnection"></a>Yerel ağ geçidinin IP adresi ön eklerini değiştirmek için - ağ geçidi bağlantısı var

Ağ geçidi bağlantınız varsa ve yerel ağ geçidinizde bulunan IP adresi ön eklerini eklemek veya kaldırmak istiyorsanız aşağıdaki adımları sırasıyla uygulamanız gerekir. Bunun sonucunda, VPN bağlantınızda kesinti oluşur. IP adresi öneklerini değiştirirken, VPN ağ geçidini silmeniz gerekmez. Yalnızca bağlantıyı kaldırmanız gerekir.


1. Bağlantıyı kaldırın.

  ```azurepowershell-interactive
  Remove-AzureRmVirtualNetworkGatewayConnection -Name VNet1toSite1 -ResourceGroupName TestRG1
  ```
2. Yerel ağ geçidiniz için adres ön eklerini değiştirin.
   
  LocalNetworkGateway için değişkeni ayarlayın.

  ```azurepowershell-interactive
  $local = Get-AzureRmLocalNetworkGateway -Name Site1 -ResourceGroupName TestRG1
  ```
   
  Ön ekleri değiştirin.
   
  ```azurepowershell-interactive
  Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
  -AddressPrefix @('10.101.0.0/24','10.101.1.0/24')
  ```
3. Bağlantıyı oluşturun. Bu örnekte bir IPsec bağlantı türü yapılandırıyoruz. Bağlantınızı yeniden oluşturduktan sonra, yapılandırmanız için belirtilen bağlantı türünü kullanın. Ek bağlantı türleri için [PowerShell cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) sayfasına bakın.
   
  VirtualNetworkGateway için değişkeni ayarlayın.

  ```azurepowershell-interactive
  $gateway1 = Get-AzureRmVirtualNetworkGateway -Name VNet1GW  -ResourceGroupName TestRG1
  ```
   
  Bağlantıyı oluşturun. Bu örnek 2. adımda ayarladığınız $local değişkenini kullanır.

  ```azurepowershell-interactive
  New-AzureRmVirtualNetworkGatewayConnection -Name VNet1toSite1 `
  -ResourceGroupName TestRG1 -Location 'East US' `
  -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
  -ConnectionType IPsec `
  -RoutingWeight 10 -SharedKey 'abc123'
  ```