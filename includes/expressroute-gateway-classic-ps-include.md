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
ms.openlocfilehash: 0bf55d2353d3524e65602c7e67b7adbf80432043
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
VNet ve bir ağ geçidi alt ağı önce aşağıdaki görevlere çalışmaya başlamadan önce oluşturmanız gerekir.

> [!NOTE]
> Bu örnekler S2S/ExpressRoute için geçerli olmayan yapılandırmalar bir arada.
> Coexist yapılandırmasında ağ geçitleri ile çalışma hakkında daha fazla bilgi için bkz: [arada var olabilen bağlantılar yapılandırmak](../articles/expressroute/expressroute-howto-coexist-classic.md#gw)

## <a name="add-a-gateway"></a>Bir ağ geçidi Ekle

Bir ağ geçidi oluşturmak için aşağıdaki komutu kullanın. Kendi ilgili tüm değerleri değiştirdiğinizden emin olun.

```powershell
New-AzureVNetGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType DynamicRouting -GatewaySKU  Standard
```

## <a name="verify-the-gateway-was-created"></a>Ağ geçidinin oluşturulduğunu doğrulayın

Ağ geçidinin oluşturulduğunu doğrulamak için aşağıdaki komutu kullanın. Bu komut ayrıca diğer işlemleri için gereken ağ geçidi kimliği alır.

```powershell
Get-AzureVNetGateway
```

## <a name="resize-a-gateway"></a>Bir ağ geçidi yeniden boyutlandırma

Bir dizi vardır [ağ geçidi SKU'ları](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Ağ geçidi SKU'su herhangi bir zamanda değiştirmek için aşağıdaki komutu kullanabilirsiniz.

> [!IMPORTANT]
> Bu komut için UltraPerformance ağ geçidi çalışmıyor. UltraPerformance ağ geçidi için ağ geçidiniz değiştirmek için önce varolan ExpressRoute ağ geçidi kaldırın ve yeni UltraPerformance ağ geçidi oluşturmak. Ağ geçidiniz UltraPerformance geçidinden düşürmek için ilk UltraPerformance ağ geçidi kaldırın ve ardından yeni bir ağ geçidi oluşturun. 
>
>

```powershell
Resize-AzureVNetGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

## <a name="remove-a-gateway"></a>Bir ağ geçidi kaldırma

Bir ağ geçidini kaldırmak için aşağıdaki komutu kullanın

```powershell
Remove-AzureVnetGateway -GatewayId <Gateway ID>
```