---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/15/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: f6fd4039614dbd7c1a2b2c6ba8403502a6420fe3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66159343"
---
Daha güçlü bir yükseltileceği SKU ağ geçidi yeniden boyutlandırmak istediğiniz geçerli için SKU'ları (VpnGw1, VpnGw2 ve VPNGW3) kullanabileceğiniz `Resize-AzVirtualNetworkGateway` PowerShell cmdlet'i. Ağ geçidi SKU boyutu bu cmdlet'ini kullanarak da düşürebilir. Temel ağ geçidi SKU'su kullanmanız durumunda [bu yönergeleri kullanmanız](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md#resize) ağ geçidi yeniden boyutlandırmak için.

Aşağıdaki PowerShell örneği, bir ağ geçidi SKU'sunu VpnGw2 için yeniden boyutlandırılmaya gösterir.

```azurepowershell-interactive
$gw = Get-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

Giderek Azure portalında bir ağ geçidi de yeniden boyutlandırabilirsiniz **yapılandırma** sayfasında sanal ağ geçidiniz için ve açılan listeden farklı bir SKU seçme.