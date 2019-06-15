---
title: ExpressRoute sanal ağ geçitleri - Azure hakkında | Microsoft Docs
description: ExpressRoute için sanal ağ geçitleri hakkında öğrenin. Bu makalede, ağ geçidi SKU'ları ve türler hakkında bilgi içerir.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mialdrid
ms.custom: seodec18
ms.openlocfilehash: 18615cf737eedcd188fd59d2aa98482210b9333a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65991583"
---
# <a name="expressroute-virtual-network-gateway-and-fastpath"></a>ExpressRoute sanal ağ geçidi ve FastPath
Azure sanal ağınız ve ExpressRoute aracılığıyla şirket içi ağınıza bağlanmak için bir sanal ağ geçidi oluşturmalısınız. Bir sanal ağ geçidi iki amaca hizmet eder: exchange IP yolları ağları arasında ağ trafiği yönlendirme. Bu makalede, ağ geçidi türleri, ağ geçidi SKU'ları ve SKU'ya göre tahmini performans açıklanmaktadır. Bu makalede ayrıca ExpressRoute açıklar [FastPath](#fastpath), performansı artırmak için sanal ağ geçidi atlamak için şirket içi ağınızdan ağ trafiğini etkinleştiren bir özellik.

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

## <a name="fastpath"></a>FastPath
ExpressRoute sanal ağ geçidi, ağ yollarını gönderip ve ağ trafiğini yönlendirmek için tasarlanmıştır. FastPath sanal ağınız ile şirket içi ağınız arasında veri yolu performansını artırmak için tasarlanmıştır. Etkin olduğunda, FastPath ağ trafiğini doğrudan sanal ağdaki sanal makinelerin ağ geçidi atlayarak gönderir. 

FastPath edinilebilir [ExpressRoute doğrudan](expressroute-erdirect-about.md) yalnızca. Bu özellik yalnızca, diğer bir deyişle, etkinleştirebilirsiniz, [uygulamanızı sanal ağınıza bağlama](expressroute-howto-linkvnet-arm.md) bir ExpressRoute doğrudan bağlantı noktasında oluşturulan bir ExpressRoute devresi için. FastPath yine de şirket içi ağ ile sanal ağ arasındaki yolları için oluşturulacak sanal ağ geçidi gerektirir. Sanal ağ geçidi, Ultra yüksek performans veya ErGw3AZ olması gerekir.

FastPath aşağıdaki özellikleri desteklemez:
* Ağ geçidi alt ağı üzerinde UDR: şirket içi ağınızdan ağ trafiğini sanal ağ geçidi için gönderilecek devam edecek, sanal ağınızın ağ geçidi alt ağı için UDR geçerli değilse.
* VNet eşlemesi: varsa diğer sanal ağlar ile şirket içi ağınızdan ağ trafiğini diğer sanal ağlara (yani sözde "Uç" sanal ağlar) sanal ağa gönderilecek devam edecek expressroute'a bağlı bir eşlenmiş ağ geçidi. Tüm sanal ağları ExpressRoute işlem hattına doğrudan bağlanmak için çözüm olabilir.

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

Bkz: [ExpressRoute için sanal ağ bağlantısı](expressroute-howto-linkvnet-arm.md) FastPath etkinleştirme hakkında daha fazla bilgi için. 
