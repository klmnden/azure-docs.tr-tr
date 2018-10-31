---
title: Azure ExpressRoute sanal ağ geçitleri hakkında | Microsoft Docs
description: ExpressRoute için sanal ağ geçitleri hakkında öğrenin.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: cherylmc
ms.openlocfilehash: bc48101decce9a92a01b8e6958bed08850a94b7e
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50241404"
---
# <a name="about-virtual-network-gateways-for-expressroute"></a>ExpressRoute için sanal ağ geçitleri hakkında
Bir sanal ağ geçidi Azure sanal ağları arasında ağ trafiği göndermek için kullanılır ve şirket içi konumlar. Kullanabileceğiniz bir sanal ağ geçidi trafiği ExpressRoute veya VPN trafiği için kullanılabilir. Bu makalede, ExpressRoute sanal ağ geçitleri üzerinde odaklanır.

## <a name="gateway-types"></a>Ağ geçidi türleri

Bir sanal ağ geçidi kaynağı oluştururken birkaç ayar belirtirsiniz. Gerekli ayarlardan biri '-GatewayType', ağ geçidini ExpressRoute için kullanılıp kullanılmayacağını belirtir ya da VPN trafiği. İki ağ geçidi türleri şunlardır: 

* **VPN** - genel Internet şifrelenmiş trafik göndermek için 'Vpn' ağ geçidi türünü kullanın. Bu VPN ağ geçidi olarak da adlandırılır. Siteden Siteye, Noktadan Siteye ve Sanal Ağdan Sanal Ağa bağlantıların tümü VPN ağ geçidi kullanır.

* **ExpressRoute** - özel bir bağlantı üzerinde ağ trafiği göndermek için 'ExpressRoute' ağ geçidi türünü kullanın. Bu, bir ExpressRoute ağ geçidi olarak da adlandırılır ve ExpressRoute yapılandırırken kullandığınız ağ geçidi türüdür.


Bir sanal ağın her ağ geçidi türü için yalnızca bir sanal ağ geçidi olabilir. Örneğin, GatewayType Vpn kullanan bir sanal ağ geçidiniz ve GatewayType ExpressRoute kullanan bir sanal ağ geçidiniz olabilir.

## <a name="gwsku"></a>Ağ Geçidi SKU'ları
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Çoğu durumda, daha güçlü bir ağ geçidi SKU'sunu ağ geçidinize yükseltmek istiyorsanız, 'Yeniden boyutlandırma-AzureRmVirtualNetworkGateway' PowerShell cmdlet'ini kullanabilirsiniz. Bu, standart ve yüksek performanslı SKU'lar yükseltmeleri için çalışır. Ancak, UltraPerformance SKU'su için yükseltmek için ağ geçidini yeniden oluşturmanız gerekecektir. Bir ağ geçidi yeniden kapalı kalma süresi artmasına neden olur.

### <a name="aggthroughput"></a>Ağ geçidi SKU'suna göre tahmini performans
Aşağıdaki tabloda, ağ geçidi türleri ve tahmini performanslarını gösterir. Bu tablo hem Resource Manager, hem de klasik dağıtım modellerine uygulanır.

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> Uygulama performansını uçtan uca gecikme süresi ve trafik akışları uygulama açılır sayısı gibi birden çok etkene bağlıdır. Tablo sayılar uygulama teorik olarak ideal bir ortam elde edebilirsiniz üst sınırını ifade eder. 
> 
>

### <a name="zrgw"></a>Bölgesel olarak yedekli ağ geçidi SKU'ları (Önizleme)

Azure kullanılabilirlik alanları, ExpressRoute ağ geçitleri de dağıtabilirsiniz. Bu fiziksel ve mantıksal olarak bunları farklı kullanılabilirlik alanları, şirket içi ağ bağlantınızı bölge düzeyinde hatalardan Azure'a koruma halinde ayırır.

![Bölgesel olarak yedekli ExpressRoute ağ geçidi](./media/expressroute-about-virtual-network-gateways/zone-redundant.png)

Bölgesel olarak yedekli ağ geçitleri, ExpressRoute ağ geçidi için belirli yeni ağ geçidi SKU'ları kullanın. Yeni SKU'lara şu anda kullanılabilir **genel Önizleme**.

* ErGw1AZ
* ErGw2AZ
* ErGw3AZ

Yeni ağ geçidi SKU'ları, en iyi sonucu gereksinimlerinize uyacak şekilde diğer dağıtım seçenekleri de destekler. Yeni ağ geçidi SKU'ları kullanarak bir sanal ağ geçidi oluştururken, aynı zamanda belirli bir bölgenin ağ geçidi dağıtmak için seçeneğiniz vardır. Bu, bölgesel bir ağ geçidi olarak adlandırılır. Bölgesel bir ağ geçidi dağıttığınızda, ağ geçidinin tüm örnekleri aynı kullanılabilirlik alanında dağıtılır. Önizlemede kaydetmek için bkz [bölgesel olarak yedekli sanal ağ geçidi oluşturma](../../articles/vpn-gateway/create-zone-redundant-vnet-gateway.md).

## <a name="resources"></a>REST API ve PowerShell cmdlet'leri
Ek teknik kaynaklar ve sanal ağ geçidi yapılandırması için REST API'ler ve PowerShell cmdlet'lerini kullanırken, belirli bir söz dizimi gereksinimler için şu sayfalara bakın:

| **Klasik** | **Resource Manager** |
| --- | --- |
| [PowerShell](https://docs.microsoft.com/powershell/module/servicemanagement/azure/?view=azuresmps-4.0.0#azure) |[PowerShell](https://docs.microsoft.com/powershell/module/azurerm.network#networking) |
| [REST API](https://msdn.microsoft.com/library/jj154113.aspx) |[REST API](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a>Sonraki adımlar
Bkz: [Expressroute'a genel bakış](expressroute-introduction.md) kullanılabilir bağlantı yapılandırmaları hakkında daha fazla bilgi.

Bkz: [ExpressRoute için sanal ağ geçidi oluşturma](expressroute-howto-add-gateway-resource-manager.md) ExpressRoute ağ geçitleri oluşturma hakkında daha fazla bilgi için.
