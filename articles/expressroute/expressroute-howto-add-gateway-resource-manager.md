---
title: "ExpressRoute için bir sanal ağa bir sanal ağ geçidi eklemek: PowerShell: Azure | Microsoft Docs"
description: "Bu makalede önceden oluşturulmuş bir Resource Manager Vnet'i ExpressRoute için sanal ağ geçidine eklerken size yol gösterilir."
documentationcenter: na
services: expressroute
author: charwen
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 63e0bd60-abad-4963-8e27-3aa973e0d968
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/17/2017
ms.author: charwen
ms.openlocfilehash: 3aeddd03e0be548933775164ae790ba208fc13ae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell"></a>PowerShell kullanarak ExpressRoute için sanal ağ geçidi yapılandırma
> [!div class="op_single_selector"]
> * [Resource Manager - Azure portalı](expressroute-howto-add-gateway-portal-resource-manager.md)
> * [Resource Manager - PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klasik - PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video - Azure portalı](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Bu makalede, eklemek, yeniden boyutlandırma ve önceden var olan bir VNet için bir sanal ağ (VNet) ağ geçidini kaldırmak için adım adım anlatılmaktadır. Bu yapılandırma için özellikle bir expressroute bağlantı yapılandırmasında kullanılacak Resource Manager dağıtım modeli kullanılarak oluşturulan sanal ağlar için adımlardır. Sanal ağ geçitleri ve ExpressRoute ağ geçidi yapılandırma ayarları hakkında daha fazla bilgi için bkz: [ExpressRoute için sanal ağ geçitleri hakkında](expressroute-about-virtual-network-gateways.md). 


## <a name="before-beginning"></a>Başlamadan önce
En son Azure PowerShell cmdlet'lerini yüklü olduğunu doğrulayın. En son cmdlet'leri yüklemediyseniz yapılandırma adımlarına başlamadan önce bunu yapmanız gerekir. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).

[!INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
VNet ağ geçidini oluşturduktan sonra bir expressroute bağlantı hattı ağınıza bağlayabilirsiniz. Bkz: [bir expressroute bağlantı hattı için bir sanal ağ bağlantı](expressroute-howto-linkvnet-arm.md).

