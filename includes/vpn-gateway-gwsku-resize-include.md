---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 081352a23e6a0d8f9e2daa77eca1f8ac85172ff6
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2019
ms.locfileid: "56418237"
---
Daha güçlü bir yükseltileceği SKU ağ geçidi yeniden boyutlandırmak istediğiniz geçerli için SKU'ları (VpnGw1, VpnGw2 ve VPNGW3) kullanabileceğiniz `Resize-AzVirtualNetworkGateway` PowerShell cmdlet'i. Ağ geçidi SKU boyutu bu cmdlet'ini kullanarak da düşürebilir. Temel ağ geçidi SKU'su kullanmanız durumunda [bu yönergeleri kullanmanız](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md#resize) ağ geçidi yeniden boyutlandırmak için.

Aşağıdaki PowerShell örneği, bir ağ geçidi SKU'sunu VpnGw2 için yeniden boyutlandırılmaya gösterir.

```powershell
$gw = Get-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

Giderek Azure portalında bir ağ geçidi de yeniden boyutlandırabilirsiniz **yapılandırma** sayfasında sanal ağ geçidiniz için ve açılan listeden farklı bir SKU seçme.