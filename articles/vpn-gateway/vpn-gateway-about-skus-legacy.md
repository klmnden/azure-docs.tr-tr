---
title: "Eski Azure sanal ağ geçidi SKU'ları | Microsoft Docs"
description: "Eski sanal ağ geçidi SKU'ları ağ."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/31/2017
ms.author: cherylmc
ms.openlocfilehash: d5127c7fa512bad49817fa4c8edf3a16ca2f7d60
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a>Sanal ağ geçidi SKU'ları (eski SKU) ile çalışma

Bu makale, eski (eski) sanal ağ geçidi SKU'ları hakkında bilgi içerir. Eski SKU'ları önceden oluşturulmuş VPN ağ geçitleri için her iki dağıtım modeli hala çalışın. Klasik VPN ağ geçitleri eski SKU'ları, varolan ağ geçitleri hem yeni ağ geçitlerini kullanmaya devam edin. Yeni kaynak yöneticisi VPN ağ geçidi oluştururken, yeni ağ geçidi SKU'ları kullanın. Yeni SKU'ları hakkında daha fazla bilgi için bkz: [VPN Gateway hakkında](vpn-gateway-about-vpngateways.md).

## <a name="gwsku"></a>Ağ Geçidi SKU'ları

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

## <a name="agg"></a>SKU göre tahmini toplam verimlilik

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <a name="config"></a>SKU ve VPN türü tarafından desteklenen yapılandırmalar

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <a name="resize"></a>Bir ağ geçidi (ağ geçidi SKU'su değiştirme) yeniden boyutlandırma

Ağ geçidi SKU'su aynı SKU ailesi içinde yeniden boyutlandırabilirsiniz. Örneğin, bir standart SKU varsa, HighPerformance SKU'ya yeniden boyutlandırabilirsiniz. VPN ağ geçitleri eski SKU'ları ve yeni SKU ailesi arasında yeniden boyutlandırılamaz. Örneğin, bir standart SKU VpnGw2 SKU'ya gidemezsiniz.

>[!IMPORTANT]
>Bir ağ geçidi yeniden boyutlandırdığınızda boyutlandırılır ise, ağ geçidi için kapalı kalma süresi 20-30 dakika sahip olur.
>
>

Klasik dağıtım modeli için bir ağ geçidi SKU'su yeniden boyutlandırmak için aşağıdaki komutu kullanın:

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

Resource Manager dağıtım modeli için bir ağ geçidi SKU'su yeniden boyutlandırmak için aşağıdaki komutu kullanın:

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="migrate"></a>Yeni ağ geçidi SKU'ları geçirme

Resource Manager dağıtım modeliyle çalışıyorsanız, yeni ağ geçidi SKU'ları geçirebilirsiniz. Klasik dağıtım modeliyle çalışıyorsanız, yeni SKU'ları geçiremezsiniz ve bunun yerine eski SKU'ları kullanmaya devam etmeniz gerekir.

[!INCLUDE [Migrate SKU](../../includes/vpn-gateway-migrate-legacy-sku-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Yeni ağ geçidi SKU'ları hakkında daha fazla bilgi için bkz: [ağ geçidi SKU'ları](vpn-gateway-about-vpngateways.md#gwsku).

Yapılandırma ayarları hakkında daha fazla bilgi için bkz: [VPN Gateway hakkında yapılandırma ayarlarını](vpn-gateway-about-vpn-gateway-settings.md).
