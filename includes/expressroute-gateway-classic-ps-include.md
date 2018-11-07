---
title: include dosyası
description: include dosyası
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 11/05/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: fab00e5281bb91bce10228b3bc2e9cfd503d5d5b
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219826"
---
Bir sanal ağ ve ağ geçidi alt ağı ilk olarak, aşağıdaki görevler üzerinde çalışmaya başlamadan önce oluşturmanız gerekir.

> [!NOTE]
> Bu örnekler S2S/ExpressRoute için geçerli değildir yapılandırmaları bir arada.
> Coexist yapılandırmasında ağ geçitleri ile çalışma hakkında daha fazla bilgi için bkz. [arada var olabilen bağlantılar yapılandırma](../articles/expressroute/expressroute-howto-coexist-classic.md#gw)

## <a name="add-a-gateway"></a>Ağ geçidi ekleme

Bir ağ geçidi oluşturmak için aşağıdaki komutu kullanın. Tüm değerleri kendi değiştirdiğinizden emin olun.

```powershell
New-AzureVNetGateway -VNetName "MyAzureVNET" -GatewayType DynamicRouting -GatewaySKU  Standard
```

## <a name="verify-the-gateway-was-created"></a>Ağ geçidinin oluşturulduğunu doğrulayın.

Ağ geçidinin oluşturulduğunu doğrulamak için aşağıdaki komutu kullanın. Bu komut ayrıca diğer işlemler için gereken ağ geçidi kimliği alır.

```powershell
Get-AzureVNetGateway
```

## <a name="resize-a-gateway"></a>Bir ağ geçidini yeniden boyutlandırın

Bir dizi vardır [ağ geçidi SKU'ları](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Herhangi bir zamanda ağ geçidi SKU'suna değiştirmek için aşağıdaki komutu kullanabilirsiniz.

> [!IMPORTANT]
> Bu komut, UltraPerformance ağ geçidi çalışmaz. Ağ geçidinize bir UltraPerformance ağ geçidi değiştirmek için önce var olan ExpressRoute ağ geçidini kaldırın ve ardından yeni bir UltraPerformance ağ geçidi oluşturun. Bir UltraPerformance ağ geçidi geçidinizden düşürmek için ilk UltraPerformance ağ geçidi kaldırın ve ardından yeni bir ağ geçidi oluşturun. 
>
>

```powershell
Resize-AzureVNetGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

## <a name="remove-a-gateway"></a>Ağ geçitlerini kaldırma

Bir ağ geçidini kaldırmak için aşağıdaki komutu kullanın.

```powershell
Remove-AzureVnetGateway -GatewayId <Gateway ID>
```
