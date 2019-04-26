---
title: ExpressRoute sanal ağ geçitleri - Azure hakkında | Microsoft Docs
description: ExpressRoute için sanal ağ geçitleri hakkında öğrenin. Bu makalede, ağ geçidi SKU'ları ve türler hakkında bilgi içerir.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: mialdrid
ms.custom: seodec18
ms.openlocfilehash: d9c607114d6c6c56c25303a88dcc11f4ab804eb4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60367991"
---
# <a name="about-virtual-network-gateways-for-expressroute"></a>ExpressRoute için sanal ağ geçitleri hakkında
Bir sanal ağ geçidi Azure sanal ağları arasında ağ trafiği göndermek için kullanılır ve şirket içi konumlar. Bir sanal ağ geçidi trafiği ExpressRoute veya VPN trafiği için kullanabilirsiniz. Bu makalede, ExpressRoute sanal ağ geçitleri üzerinde odaklanır ve tahmini performans SKU ve ağ geçidi türleri, SKU'ları hakkında bilgiler içerir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="gateway-types"></a>Ağ geçidi türleri

Bir sanal ağ geçidi oluştururken birkaç ayar yapılandırırsınız gerekir. Gerekli ayarlardan biri '-GatewayType', ağ geçidini ExpressRoute için kullanılıp kullanılmayacağını belirtir ya da VPN trafiği. İki ağ geçidi türleri şunlardır:

* **VPN** - genel Internet şifrelenmiş trafik göndermek için 'Vpn' ağ geçidi türünü kullanın. Bu VPN ağ geçidi olarak da adlandırılır. Siteden Siteye, Noktadan Siteye ve Sanal Ağdan Sanal Ağa bağlantıların tümü VPN ağ geçidi kullanır.

* **ExpressRoute** - özel bir bağlantı üzerinde ağ trafiği göndermek için 'ExpressRoute' ağ geçidi türünü kullanın. Bu, bir ExpressRoute ağ geçidi olarak da adlandırılır ve ExpressRoute yapılandırma sırasında kullanılan ağ geçidi türüdür.

Bir sanal ağın her ağ geçidi türü için yalnızca bir sanal ağ geçidi olabilir. Örneğin, GatewayType Vpn kullanan bir sanal ağ geçidiniz ve GatewayType ExpressRoute kullanan bir sanal ağ geçidiniz olabilir.

## <a name="gwsku"></a>Ağ Geçidi SKU'ları
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Çoğu durumda, daha güçlü bir ağ geçidi SKU'sunu ağ geçidinize yükseltmek istiyorsanız, 'Yeniden boyutlandırma AzVirtualNetworkGateway' PowerShell cmdlet'ini kullanabilirsiniz. Bu, standart ve yüksek performanslı SKU'lar yükseltmeleri için çalışır. Ancak, UltraPerformance SKU'su için yükseltmek için ağ geçidini yeniden oluşturmanız gerekecektir. Bir ağ geçidi yeniden kapalı kalma süresi artmasına neden olur.

### <a name="aggthroughput"></a>Ağ geçidi SKU'suna göre tahmini performans
Aşağıdaki tabloda, ağ geçidi türleri ve tahmini performanslarını gösterir. Bu tablo hem Resource Manager, hem de klasik dağıtım modellerine uygulanır.

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> Uygulama performansını uçtan uca gecikme süresi ve trafik akışları uygulama açılır sayısı gibi birden çok etkene bağlıdır. Tablo sayılar uygulama teorik olarak ideal bir ortam elde edebilirsiniz üst sınırını ifade eder.
>
>

### <a name="zrgw"></a>Bölgesel olarak yedekli ağ geçidi SKU'ları

Azure kullanılabilirlik alanları, ExpressRoute ağ geçitleri de dağıtabilirsiniz. Bu fiziksel ve mantıksal olarak bunları farklı kullanılabilirlik alanları, şirket içi ağ bağlantınızı bölge düzeyinde hatalardan Azure'a koruma halinde ayırır.

![Bölgesel olarak yedekli ExpressRoute ağ geçidi](./media/expressroute-about-virtual-network-gateways/zone-redundant.png)

Bölgesel olarak yedekli ağ geçitleri, ExpressRoute ağ geçidi için belirli yeni ağ geçidi SKU'ları kullanın.

* ErGw1AZ
* ErGw2AZ
* ErGw3AZ

Yeni ağ geçidi SKU'ları, en iyi sonucu gereksinimlerinize uyacak şekilde diğer dağıtım seçenekleri de destekler. Yeni ağ geçidi SKU'ları kullanarak bir sanal ağ geçidi oluştururken, aynı zamanda belirli bir bölgenin ağ geçidi dağıtmak için seçeneğiniz vardır. Bu, bölgesel bir ağ geçidi olarak adlandırılır. Bölgesel bir ağ geçidi dağıttığınızda, ağ geçidinin tüm örnekleri aynı kullanılabilirlik alanında dağıtılır.

## <a name="resources"></a>REST API ve PowerShell cmdlet'leri
Ek teknik kaynaklar ve sanal ağ geçidi yapılandırması için REST API'ler ve PowerShell cmdlet'lerini kullanırken, belirli bir söz dizimi gereksinimler için şu sayfalara bakın:

| **Klasik** | **Resource Manager** |
| --- | --- |
| [PowerShell](https://docs.microsoft.com/powershell/module/servicemanagement/azure/?view=azuresmps-4.0.0#azure) |[PowerShell](https://docs.microsoft.com/powershell/module/az.network#networking) |
| [REST API](https://msdn.microsoft.com/library/jj154113.aspx) |[REST API](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a>Sonraki adımlar
Bkz: [Expressroute'a genel bakış](expressroute-introduction.md) kullanılabilir bağlantı yapılandırmaları hakkında daha fazla bilgi.

Bkz: [ExpressRoute için sanal ağ geçidi oluşturma](expressroute-howto-add-gateway-resource-manager.md) ExpressRoute ağ geçitleri oluşturma hakkında daha fazla bilgi için.

Bkz: [bölgesel olarak yedekli sanal ağ geçidi oluşturma](../../articles/vpn-gateway/create-zone-redundant-vnet-gateway.md) bölgesel olarak yedekli ağ geçitlerini yapılandırma hakkında daha fazla bilgi.
