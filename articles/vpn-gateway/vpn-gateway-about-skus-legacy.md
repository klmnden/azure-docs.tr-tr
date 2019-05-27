---
title: Eski Azure sanal ağ VPN ağ geçidi SKU'ları | Microsoft Docs
description: Eski sanal ağ geçidi SKU'ları ile çalışma konusunda; Temel, standart ve yüksek performanslı.
services: vpn-gateway
documentationcenter: na
author: WenJason
manager: digimobile
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 03/20/2018
ms.date: 03/04/2019
ms.author: v-jay
ms.openlocfilehash: 5a9e3f63a484069bf8cd39f8a545d7c37f05c63c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66159249"
---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a>Sanal ağ geçidi SKU'ları (eski SKU'lar) ile çalışma

Bu makale, eski (eski) sanal ağ geçidi SKU'ları hakkında bilgi içerir. Eski SKU'ları önceden oluşturulmuş VPN ağ geçitleri için her iki dağıtım modeline hala çalışır. Klasik VPN ağ geçitleri eski SKU'ları, hem var olan bir ağ geçidi ve yeni ağ geçitleri için kullanmaya devam edin. Yeni Resource Manager VPN ağ geçidi oluştururken, yeni ağ geçidi SKU'ları kullanın. Yeni SKU'lar hakkında daha fazla bilgi için bkz: [VPN Gateway hakkında](vpn-gateway-about-vpngateways.md).

## <a name="gwsku"></a>Ağ Geçidi SKU'ları

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

## <a name="agg"></a>SKU'ya göre tahmini toplam verimlilik

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <a name="config"></a>SKU ve VPN türü tarafından desteklenen yapılandırmalar

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <a name="resize"></a>Bir ağ geçidini yeniden boyutlandırın

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Ağ geçidi için aynı SKU ailesi içinde bir ağ geçidi SKU'sunu yeniden boyutlandırabilirsiniz. Örneğin, standart SKU varsa, yüksek performanslı SKU için yeniden boyutlandırabilirsiniz. Ancak, VPN ağ geçidiniz eski SKU'lar ve yeni SKU aileleri arasında yeniden boyutlandıramazsınız. Örneğin, bir VpnGw2 SKU ya da bir temel SKU VpnGw1 için standart bir SKU gidilemiyor.

Klasik dağıtım modeli için bir ağ geçidi yeniden boyutlandırmak için aşağıdaki komutu kullanın:

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

PowerShell kullanarak Resource Manager dağıtım modeli için bir ağ geçidi yeniden boyutlandırmak için aşağıdaki komutu kullanın:

```powershell
$gw = Get-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```
Azure portalında bir ağ geçidi yeniden boyutlandırabilirsiniz.

## <a name="change"></a>Yeni ağ geçidi SKU'ları değiştirme

[!INCLUDE [Change to the new SKUs](../../includes/vpn-gateway-gwsku-change-legacy-sku-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Yeni ağ geçidi SKU'ları hakkında daha fazla bilgi için bkz: [ağ geçidi SKU'ları](vpn-gateway-about-vpngateways.md#gwsku).

Yapılandırma ayarları hakkında daha fazla bilgi için bkz. [VPN Gateway yapılandırma ayarları hakkında](vpn-gateway-about-vpn-gateway-settings.md).

<!--Update_Description: wording update-->