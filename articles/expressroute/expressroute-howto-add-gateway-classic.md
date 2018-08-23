---
title: 'PowerShell kullanarak ExpressRoute için sanal ağ geçidi yapılandırma: Klasik: Azure | Microsoft Docs'
description: Klasik dağıtım için bir VNet ağ geçidi bir ExpressRoute yapılandırma için PowerShell kullanarak VNet model.
documentationcenter: na
services: expressroute
author: charwen
manager: carmonm
editor: ''
tags: azure-service-management
ms.assetid: 85ee0bc1-55be-4760-bfb4-34d9f2c96f30
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: charwen
ms.openlocfilehash: ec7e4a326bc0d5e9393db28c83c9d859a93dc8a3
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42059919"
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell-classic"></a>PowerShell (Klasik) kullanarak ExpressRoute için sanal ağ geçidi yapılandırma
> [!div class="op_single_selector"]
> * [Resource Manager - PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klasik - PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video - Azure portalı](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Bu makalede, ekleme, yeniden boyutlandırma ve önceden var olan bir VNet için bir sanal ağ (VNet) ağ geçidini kaldırmak için adımlarında size yol gösterir. Bu yapılandırmanın adımları kullanılarak oluşturulan özel sanal ağlar için olan **Klasik dağıtım modeli** ve ExpressRoute yapılandırması kullanılır. 

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Azure dağıtım modelleri hakkında**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-beginning"></a>Başlamadan önce
Bu yapılandırma için gerekli Azure PowerShell cmdlet'lerinin yüklü olduğunu doğrulayın (1.0.2 veya üzeri). Cmdlet'ler yüklemediyseniz yapılandırma adımlarına başlamadan önce yapmanız gerekir. Azure PowerShell'i yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview).

[!INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
VNet ağ geçidinin oluşturduktan sonra ağınız bir ExpressRoute bağlantı hattına bağlayabilirsiniz. Bkz: [bir sanal ağı ExpressRoute devresine bağlama](expressroute-howto-linkvnet-classic.md).

