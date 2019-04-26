---
title: 'ExpressRoute için sanal ağ geçidi bir sanal ağa ekleyin: PowerShell: Azure | Microsoft Docs'
description: Bu makalede önceden oluşturulmuş bir Resource Manager Vnet'i ExpressRoute için VNet ağ geçidinin eklemenize yardımcı olur.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: article
ms.date: 02/21/2019
ms.author: charwen
ms.custom: seodec18
ms.openlocfilehash: f126f4fe6f7c0a66ce6f1031945442927f48113b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60366391"
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell"></a>PowerShell kullanarak ExpressRoute için sanal ağ geçidi yapılandırma
> [!div class="op_single_selector"]
> * [Resource Manager - Azure portalı](expressroute-howto-add-gateway-portal-resource-manager.md)
> * [Resource Manager - PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klasik - PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video - Azure portalı](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Bu makalede, ekleme, yeniden boyutlandırma ve önceden var olan bir VNet için bir sanal ağ (VNet) ağ geçidini kaldırmak yardımcı olur. Bu yapılandırma için adımlar, bir ExpressRoute yapılandırması için Resource Manager dağıtım modeli kullanılarak oluşturulan sanal ağlar için geçerlidir. Daha fazla bilgi için [ExpressRoute için sanal ağ geçitleri hakkında](expressroute-about-virtual-network-gateways.md).

## <a name="before-beginning"></a>Başlamadan önce

### <a name="working-with-powershell"></a>PowerShell ile çalışma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [working with cloud shell](../../includes/expressroute-cloud-shell-powershell-about.md)]

### <a name="configuration-reference-list"></a>Yapılandırma başvuru listesi

[!INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
VNet ağ geçidinin oluşturduktan sonra ağınız bir ExpressRoute bağlantı hattına bağlayabilirsiniz. Bkz: [bir sanal ağı ExpressRoute devresine bağlama](expressroute-howto-linkvnet-arm.md).