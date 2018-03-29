---
title: ExpressRoute sanal ağ geçitleri hakkında | Microsoft Docs
description: ExpressRoute için sanal ağ geçitleri hakkında bilgi edinin.
services: expressroute
documentationcenter: na
author: cherylmc
manager: carmonm
editor: ''
tags: azure-resource-manager, azure-service-management
ms.assetid: 7e0d9658-bc00-45b0-848f-f7a6da648635
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2018
ms.author: cherylmc
ms.openlocfilehash: ae971e7743d6dd3269c0a4f976bd2a5316300f58
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="about-virtual-network-gateways-for-expressroute"></a>ExpressRoute için sanal ağ geçitleri hakkında
Bir sanal ağ geçidi Azure sanal ağlar arasında ağ trafiğini göndermek için kullanılır ve şirket içi konumlara. ExpressRoute bağlantısı yapılandırdığınızda oluşturmak ve sanal ağ geçidi ve sanal ağ geçidi bağlantısı yapılandırmanız gerekir.

Bir sanal ağ geçidi kaynağı oluştururken birkaç ayar belirtirsiniz. Gerekli ayarları birini ağ geçidini ExpressRoute veya siteden siteye VPN trafiği için kullanılıp kullanılmayacağını belirtir. Resource Manager dağıtım modelinde ayardır '-GatewayType'.

Ağ trafiği üzerinde özel bir bağlantı gönderilir, ağ geçidi türü 'ExpressRoute' kullanın. Bu seçenek ExpressRoute ağ geçidi olarak da adlandırılır. Ağ trafiği genel Internet üzerinden şifrelenmiş olarak gönderildiğinde 'Vpn' ağ geçidi türünü kullanın. Bu seçenek VPN ağ geçidi olarak adlandırılır. Siteden Siteye, Noktadan Siteye ve Sanal Ağdan Sanal Ağa bağlantıların tümü VPN ağ geçidi kullanır.

Bir sanal ağın her ağ geçidi türü için yalnızca bir sanal ağ geçidi olabilir. Örneğin, GatewayType Vpn kullanan bir sanal ağ geçidiniz ve GatewayType ExpressRoute kullanan bir sanal ağ geçidiniz olabilir. Bu makalede ExpressRoute sanal ağ geçidinde odaklanır.

## <a name="gwsku"></a>Ağ Geçidi SKU'ları
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Ağ geçidi uygulamanızı daha güçlü bir ağ geçidi SKU'su yükseltmek istiyorsanız, çoğu durumda, 'Yeniden boyutlandırma-AzureRmVirtualNetworkGateway' PowerShell cmdlet'ini kullanabilirsiniz. Bu, standart ve HighPerformance SKU'ları yükseltmeleri için çalışır. Ancak, UltraPerformance SKU'ya yükseltmek için ağ geçidi yeniden oluşturmanız gerekir.

### <a name="aggthroughput"></a>Ağ geçidi SKU tarafından tahmini performanslarını
Aşağıdaki tabloda ağ geçidi türleri ve tahmini performanslarını gösterir. Bu tablo hem Resource Manager, hem de klasik dağıtım modellerine uygulanır.

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> Uygulama performansı uçtan uca gecikme süresi ve uygulama açılır trafik akışlarının sayısı gibi birden çok etkene bağlıdır. Tabloda yer alan numaralarını uygulama ideal bir ortamda teorik olarak elde edebilirsiniz üst sınırı temsil eder. 
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

Bkz: [ExpressRoute için bir sanal ağ geçidi oluşturmak](expressroute-howto-add-gateway-resource-manager.md) ExpressRoute ağ geçidi oluşturma hakkında daha fazla bilgi için.
