---
title: "ExpressRoute sanal ağ geçitleri hakkında | Microsoft Docs"
description: "ExpressRoute için sanal ağ geçitleri hakkında bilgi edinin."
services: expressroute
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
tags: azure-resource-manager, azure-service-management
ms.assetid: 7e0d9658-bc00-45b0-848f-f7a6da648635
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: cherylmc
ms.openlocfilehash: a6363fa380d0bab05d7500141cc6019d1d3f68b8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="about-virtual-network-gateways-for-expressroute"></a>ExpressRoute için sanal ağ geçitleri hakkında
Bir sanal ağ geçidi Azure sanal ağlar arasında ağ trafiğini göndermek için kullanılır ve şirket içi konumlara. ExpressRoute bağlantısı yapılandırdığınızda oluşturmak ve sanal ağ geçidi ve sanal ağ geçidi bağlantısı yapılandırmanız gerekir.

Bir sanal ağ geçidi kaynağı oluştururken birkaç ayar belirtirsiniz. Gerekli ayarları birini ağ geçidini ExpressRoute veya siteden siteye VPN trafiği için kullanılıp kullanılmayacağını belirtir. Resource Manager dağıtım modelinde ayardır '-GatewayType'.

Ağ trafiği üzerinde özel bir bağlantı gönderilir, ağ geçidi türü 'ExpressRoute' kullanın. Bu seçenek ExpressRoute ağ geçidi olarak da adlandırılır. Ağ trafiği genel Internet üzerinden şifrelenmiş olarak gönderildiğinde 'Vpn' ağ geçidi türünü kullanın. Bu seçenek VPN ağ geçidi olarak adlandırılır. Siteden Siteye, Noktadan Siteye ve Sanal Ağdan Sanal Ağa bağlantıların tümü VPN ağ geçidi kullanır.

Bir sanal ağın her ağ geçidi türü için yalnızca bir sanal ağ geçidi olabilir. Örneğin, GatewayType Vpn kullanan bir sanal ağ geçidiniz ve GatewayType ExpressRoute kullanan bir sanal ağ geçidiniz olabilir. Bu makalede ExpressRoute sanal ağ geçidinde odaklanır.

## <a name="gwsku"></a>Ağ Geçidi SKU'ları
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Ağ geçidi uygulamanızı daha güçlü bir ağ geçidi SKU'su yükseltmek istiyorsanız, çoğu durumda, 'Yeniden boyutlandırma-AzureRmVirtualNetworkGateway' PowerShell cmdlet'ini kullanabilirsiniz. Bu, standart ve HighPerformance SKU'ları yükseltmeleri için çalışır. Ancak, UltraPerformance SKU'ya yükseltmek için ağ geçidi yeniden oluşturmanız gerekir.

### <a name="aggthroughput"></a>Ağ geçidi SKU'su göre tahmini toplam verimlilik
Aşağıdaki tabloda ağ geçidi türleri ve tahmini toplam verimlilik gösterilmiştir. Bu tablo hem Resource Manager, hem de klasik dağıtım modellerine uygulanır.

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> Uygulama işlemenizi uçtan uca gecikme süresi ve uygulama açılır trafik akışlarının sayısı gibi birden çok etkene bağlıdır. Tabloda yer alan numaralarını uygulama can theorectically elde ideal bir ortamda üst sınırı temsil eder. 
> 
>

## <a name="resources"></a>REST API ve PowerShell cmdlet'leri
Ek teknik kaynaklar ve REST API'leri ve PowerShell cmdlet'leri için sanal ağ geçidi yapılandırmaları kullanırken belirli sözdizimi gereksinimleri için aşağıdaki sayfalarına bakın:

| **Klasik** | **Resource Manager** |
| --- | --- |
| [PowerShell](https://msdn.microsoft.com/library/mt270335.aspx) |[PowerShell](https://msdn.microsoft.com/library/mt163510.aspx) |
| [REST API](https://msdn.microsoft.com/library/jj154113.aspx) |[REST API](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a>Sonraki adımlar
Bkz: [ExpressRoute genel bakış](expressroute-introduction.md) kullanılabilir bağlantı yapılandırmaları hakkında daha fazla bilgi için. 

