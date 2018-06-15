---
title: Eski Azure sanal ağ VPN ağ geçidi SKU'ları | Microsoft Docs
description: Eski sanal ağ geçidi SKU'ları ile çalışmaya nasıl; Temel, standart ve HighPerformance.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2018
ms.author: cherylmc
ms.openlocfilehash: 4feecb9c1e91e1bc6c66a610c092e7bf894886e5
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
ms.locfileid: "30190237"
---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a>Sanal ağ geçidi SKU'ları (eski SKU) ile çalışma

Bu makale, eski (eski) sanal ağ geçidi SKU'ları hakkında bilgi içerir. Eski SKU'ları önceden oluşturulmuş VPN ağ geçitleri için her iki dağıtım modeli hala çalışın. Klasik VPN ağ geçitleri eski SKU'ları, varolan ağ geçitleri hem yeni ağ geçitlerini kullanmaya devam edin. Yeni kaynak yöneticisi VPN ağ geçidi oluştururken, yeni ağ geçidi SKU'ları kullanın. Yeni SKU'ları hakkında daha fazla bilgi için bkz: [VPN Gateway hakkında](vpn-gateway-about-vpngateways.md).

## <a name="gwsku"></a>Ağ Geçidi SKU'ları

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

## <a name="agg"></a>SKU göre tahmini toplam verimlilik

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <a name="config"></a>SKU ve VPN türü tarafından desteklenen yapılandırmalar

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <a name="resize"></a>Bir ağ geçidi yeniden boyutlandırma

Ağ geçidi SKU'su aynı SKU ailesi içinde ağ geçidi uygulamanızı yeniden boyutlandırabilirsiniz. Örneğin, bir standart SKU varsa, HighPerformance SKU'ya yeniden boyutlandırabilirsiniz. Ancak, VPN ağ geçidinizi eski SKU'ları ve yeni SKU ailesi arasında yeniden boyutlandırılamaz. Örneğin, bir VpnGw2 SKU ya da bir temel SKU VpnGw1 için bir standart SKU dönemezsiniz.

Klasik dağıtım modeli için bir ağ geçidi yeniden boyutlandırmak için aşağıdaki komutu kullanın:

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

PowerShell kullanarak Resource Manager dağıtım modeli için bir ağ geçidi yeniden boyutlandırmak için aşağıdaki komutu kullanın:

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```
Azure portalında bir ağ geçidi yeniden boyutlandırabilirsiniz.

## <a name="change"></a>Yeni ağ geçidi SKU'ları değiştirme

[!INCLUDE [Change to the new SKUs](../../includes/vpn-gateway-gwsku-change-legacy-sku-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Yeni ağ geçidi SKU'ları hakkında daha fazla bilgi için bkz: [ağ geçidi SKU'ları](vpn-gateway-about-vpngateways.md#gwsku).

Yapılandırma ayarları hakkında daha fazla bilgi için bkz: [VPN Gateway hakkında yapılandırma ayarlarını](vpn-gateway-about-vpn-gateway-settings.md).