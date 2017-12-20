---
title: "PowerShell kullanarak ExpressRoute için bir VNet ağ geçidini yapılandırma: Klasik: Azure | Microsoft Docs"
description: "Klasik dağıtım için bir sanal ağ geçidi yapılandırmak için bir ExpressRoute yapılandırma PowerShell kullanarak VNet model."
documentationcenter: na
services: expressroute
author: charwen
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 85ee0bc1-55be-4760-bfb4-34d9f2c96f30
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: charwen
ms.openlocfilehash: 195a38fa45f1c514a93980e777fb0d8238aa3f3f
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell-classic"></a>Bir sanal ağ geçidi (Klasik) PowerShell kullanarak ExpressRoute için yapılandırma
> [!div class="op_single_selector"]
> * [Resource Manager - PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klasik - PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video - Azure portalı](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Bu makalede eklemek, yeniden boyutlandırma ve önceden var olan bir VNet için bir sanal ağ (VNet) ağ geçidini kaldırmak için adımlarda size yol gösterecek. Adımlardır kullanılarak oluşturulan özellikle sanal ağlar için bu yapılandırma için **Klasik dağıtım modeli** ve, bir expressroute bağlantı yapılandırmasında kullanılabilir. 

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Azure dağıtım modelleri hakkında**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-beginning"></a>Başlamadan önce
Bu yapılandırma için gerekli Azure PowerShell cmdlet'lerini yüklü olduğunu doğrulayın (1.0.2 veya üzeri). Cmdlet'leri yüklemediyseniz yapılandırma adımlarına başlamadan önce bunu yapmanız gerekir. Azure PowerShell'i yükleme hakkında daha fazla bilgi için bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).

[!INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
VNet ağ geçidini oluşturduktan sonra bir expressroute bağlantı hattı ağınıza bağlayabilirsiniz. Bkz: [bir expressroute bağlantı hattı için bir sanal ağ bağlantı](expressroute-howto-linkvnet-classic.md).

