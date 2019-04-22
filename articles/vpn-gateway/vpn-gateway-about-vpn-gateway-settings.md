---
title: Şirket içi Azure bağlantıları için VPN ağ geçidi ayarları | Microsoft Docs
description: Azure sanal ağ geçidi için VPN Gateway ayarları hakkında bilgi edinin.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: cherylmc
ms.openlocfilehash: 4030c196d6a4de721b640f5da0b692f4d8157d12
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59049872"
---
# <a name="about-vpn-gateway-configuration-settings"></a>VPN Gateway yapılandırma ayarları hakkında

Bir VPN ağ geçidi, ortak bir bağlantı üzerinden sanal ağınız ile şirket içi konumunuz arasında şifrelenmiş trafik gönderen sanal ağ geçidi türüdür. Azure omurgası üzerinden sanal ağlar arasında trafik göndermek için bir VPN ağ geçidi'ni de kullanabilirsiniz.

Bir VPN ağ geçidi bağlantısı, her biri yapılandırılabilir ayarlar içeren yapılandırmasına birden çok kaynak kullanır. Bu makaledeki bölümler, kaynakları ve Resource Manager dağıtım modelinde oluşturulan sanal ağ için bir VPN ağ geçidi ile ilgili ayarları ele alınmıştır. Her bağlantı çözüm için açıklamalar ve topoloji diyagramlarını bulabilirsiniz [VPN Gateway hakkında](vpn-gateway-about-vpngateways.md) makalesi.

Bu makalede değerleri, VPN ağ geçitleri (- GatewayType Vpn kullanan sanal ağ geçitleri) uygulayın. Bu makalede, tüm ağ geçidi türleri veya bölgesel olarak yedekli ağ geçitleri kapsamaz.

* -GatewayType için 'ExpressRoute' geçerli değerler için bkz: [ExpressRoute için sanal ağ geçitleri](../expressroute/expressroute-about-virtual-network-gateways.md).

* Bölgesel olarak yedekli ağ geçitleri için bkz: [bölgesel olarak yedekli ağ geçitleri hakkında](about-zone-redundant-vnet-gateways.md).

* Sanal WAN için bkz. [hakkında sanal WAN](../virtual-wan/virtual-wan-about.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="gwtype"></a>Ağ geçidi türleri

Her sanal ağın yalnızca bir sanal ağ geçidi her türden olabilir. Bir sanal ağ geçidi oluştururken, ağ geçidi türünü yapılandırmanız için doğru olduğundan emin olmanız gerekir.

-GatewayType için kullanılabilen değerler şunlardır:

* VPN
* ExpressRoute

Bir VPN ağ geçidi gerektirir `-GatewayType` *Vpn*.

Örnek:

```azurepowershell-interactive
New-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased
```

## <a name="gwsku"></a>Ağ Geçidi SKU'ları

[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configure-a-gateway-sku"></a>Ağ geçidi SKU'sunu yapılandırın

#### <a name="azure-portal"></a>Azure portal

Resource Manager sanal ağ geçidi oluşturmak için Azure portalını kullanıyorsanız, açılan listeyi kullanarak ağ geçidi SKU'sunu seçebilirsiniz. İle sunulan seçenekler, seçtiğiniz VPN türü ve ağ geçidi türü için karşılık gelir.

#### <a name="powershell"></a>PowerShell

Aşağıdaki PowerShell örneği belirtir `-GatewaySku` VpnGw1 olarak. Bir ağ geçidi oluşturmak için PowerShell kullanırken ilk IP yapılandırmasını oluşturun ve ardından buna başvurmak için bir değişken kullanın gerekir. Bu örnekte, yapılandırma değişkeni $gwipconfig ' dir.

```azurepowershell-interactive
New-AzVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1 `
-Location 'US East' -IpConfigurations $gwipconfig -GatewaySku VpnGw1 `
-GatewayType Vpn -VpnType RouteBased
```

#### <a name="azure-cli"></a>Azure CLI

```azurecli
az network vnet-gateway create --name VNet1GW --public-ip-address VNet1GWPIP --resource-group TestRG1 --vnet VNet1 --gateway-type Vpn --vpn-type RouteBased --sku VpnGw1 --no-wait
```

###  <a name="resizechange"></a>Yeniden boyutlandırma veya SKU değiştirme

Bir VPN ağ geçidi varsa ve farklı bir ağ geçidi SKU'sunu kullanmak istediğiniz ya da, ağ geçidi SKU'sunu yeniden boyutlandırmak için veya başka bir SKU'ya değiştirmek için seçenekleriniz şunlardır. Başka bir ağ geçidi SKU'su değiştirdiğinizde, mevcut ağ geçidini tamamen silin ve yeni bir tane oluşturun. Bir ağ geçidi oluşturma 45 dakika kadar sürebilir. Buna karşılık, bir ağ geçidi SKU'su, yeniden boyutlandırdığınızda yok kadar kapalı kalma süresi silin ve ağ geçidini yeniden olmadığı için. Seçeneği değiştirmek yerine, ağ geçidi SKU'sunu yeniden boyutlandırma varsa, bunu istersiniz. Ancak, kural yok yeniden boyutlandırma ile ilgili:

1. VpnGw1, VpnGw2 ve VpnGw3 SKU'ları arasında yeniden boyutlandırma gerçekleştirebilirsiniz.
2. Eski ağ geçidi SKU'larıyla çalışırken Temel, Standart ve Yüksek Performanslı SKU'lar arasında yeniden boyutlandırma yapabilirsiniz.
3. Temel/Standart/Yüksek Performanslı SKU'ları yeni VpnGw1/VpnGw2/VpnGw3 SKU'larıyla aynı olacak şekilde **yeniden boyutlandıramazsınız**. Bunun yerine, gerekir [değiştirme](#change) yeni SKU'lara.

#### <a name="resizegwsku"></a>Bir ağ geçidi yeniden boyutlandırmak için

[!INCLUDE [Resize a SKU](../../includes/vpn-gateway-gwsku-resize-include.md)]

####  <a name="change"></a>(Eski) eski bir SKU'dan yeni bir SKU'ya değiştirmek için

[!INCLUDE [Change a SKU](../../includes/vpn-gateway-gwsku-change-legacy-sku-include.md)]

## <a name="connectiontype"></a>Bağlantı türleri

Resource Manager dağıtım modelinde, her yapılandırma bir özel sanal ağ geçidi bağlantı türü gerektirir. `-ConnectionType` için kullanılabilir Resource Manager PowerShell değerleri şunlardır:

* IPsec
* Vnet2Vnet
* ExpressRoute
* VPNClient

Aşağıdaki PowerShell örneği, bağlantı türü gerektiren bir S2S bağlantısı oluşturacağız *IPSec*.

```azurepowershell-interactive
New-AzVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
-ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
```

## <a name="vpntype"></a>VPN türleri

VPN ağ geçidi yapılandırması için sanal ağ geçidi oluşturduğunuzda, bir VPN türünü belirtmeniz gerekir. Oluşturmak istediğiniz bağlantı topolojisine seçtiğiniz VPN türüne bağlıdır. Örneğin, P2S bağlantısı RouteBased VPN türü gerektirir. Bir VPN türü, ayrıca kullandığınız donanımda bağlı olabilir. S2S yapılandırmaları bir VPN cihazı gerektirir. Bazı VPN cihazlarının yalnızca belirli bir VPN türünü destekler.

Seçtiğiniz VPN türüne oluşturmak istediğiniz tüm bağlantı çözümünün gereksinimlerini karşılaması gerekir. Örneğin, bir S2S VPN gateway bağlantısı ve aynı sanal ağ için P2S VPN ağ geçidi bağlantısı oluşturmak istiyorsanız, VPN türü kullanırsınız *RouteBased* çünkü P2S RouteBased VPN türü gerektirir. VPN Cihazınızı RouteBased VPN bağlantısı desteklenen doğrulamak gerekir. 

Bir sanal ağ geçidi oluşturulduktan sonra VPN türünü değiştiremezsiniz. Sanal ağ geçidini silin ve yeni bir tane oluşturmanız gerekir. İki VPN türü vardır:

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

Aşağıdaki PowerShell örneği belirtir `-VpnType` olarak *RouteBased*. Bir ağ geçidi oluştururken, -VpnType öğesinin yapılandırmanız için doğru olduğundan emin olmanız gerekir.

```azurepowershell-interactive
New-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig `
-GatewayType Vpn -VpnType RouteBased
```

## <a name="requirements"></a>Ağ geçidi gereksinimleri

[!INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)]

## <a name="gwsub"></a>Ağ geçidi alt ağı

Bir VPN ağ geçidi oluşturmadan önce bir ağ geçidi alt ağı oluşturmanız gerekir. Ağ geçidi alt ağı sanal ağ geçidi Vm'lerini ve hizmetlerini kullanan IP adreslerini içerir. Sanal ağ geçidinizi oluştururken, ağ geçidi Vm'leri ağ geçidi alt ağına dağıtılır ve gerekli VPN ağ geçidi ayarlarla yapılandırılır. Hiçbir şey (örneğin, ek VM'ler) ağ geçidi alt ağına dağıtmayın. Ağ geçidi alt ağı 'GatewaySubnet' olarak adlandırılmalıdır düzgün çalışması için. Ağ geçidi alt ağı "GatewaySubnet" adlandırma, bu alt ağ sanal ağ geçidi Vm'leri ve Hizmetleri dağıtmak için hazır olduğunu biliyor Azure olanak tanır.

>[!NOTE]
>[!INCLUDE [vpn-gateway-gwudr-warning.md](../../includes/vpn-gateway-gwudr-warning.md)]
>

Ağ geçidi alt ağı oluştururken, alt ağın içerdiği IP adresi sayısını belirtirsiniz. Ağ geçidi alt ağı IP adresleri, ağ geçidi Vm'leri ve ağ geçidi hizmetlerine ayrılır. Bazı yapılandırmalar için diğerlerinden daha fazla IP adresi gerekir. Oluşturun ve oluşturmak istediğiniz ağ geçidi alt ağı bu gereksinimleri karşıladığını doğrulamak için istediğiniz yapılandırmayı yönergelerine bakın. Ayrıca, gelecekteki olası ek yapılandırmaları barındırmak için yeterli IP adresi, ağ geçidi alt ağı içerdiğinden emin olmak isteyebilirsiniz. Bir ağ geçidi alt ağı/29 kadar küçük oluşturmanız mümkün olsa da/28'lik veya daha büyük bir ağ geçidi alt ağı oluşturmanızı öneririz (/ 28, en az/27, / 26 vb..). İşlevselliğini gelecekte eklerseniz bu şekilde, ağ geçidiniz, ayırma sonra silin ve daha fazla IP adresi için izin vermek için ağ geçidi alt ağı oluşturmanız gerekmez.

Resource Manager PowerShell aşağıdaki örnek GatewaySubnet adlı bir ağ geçidi alt ağı gösterir. Şu anda mevcut çoğu yapılandırma için yeterli IP adresi izin veren bir/27 CIDR gösterimini belirtir görebilirsiniz.

```azurepowershell-interactive
Add-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="lng"></a>Yerel ağ geçitleri

 Bir yerel ağ geçidi bir sanal ağ geçidi farklıdır. Bir VPN ağ geçidi yapılandırması oluştururken, yerel ağ geçidi genellikle şirket içi konumunuzu temsil eder. Klasik dağıtım modelinde, yerel ağ geçidi için Yerel Site olara ifade edilir.

Yerel ağ geçidi şirket içi VPN cihazının genel IP adresini bir ad verip şirket içi konumunda yer alan adres öneklerini belirtirsiniz. Azure ağ trafiği için hedef adres öneklerine bakar, yerel ağ geçidiniz için belirttiğiniz yapılandırma bakar ve paketleri buna göre yönlendirir. VPN ağ geçidi bağlantısı VNet-VNet yapılandırmaları için yerel ağ geçitleri de belirtirsiniz.

Aşağıdaki PowerShell örneği, yeni bir yerel ağ geçidi oluşturur:

```azurepowershell-interactive
New-AzLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```

Bazen yerel ağ geçidi ayarlarını değiştirmeniz gerekir. Örneğin, eklediğinizde veya adres aralığını değiştirmek veya VPN cihazının IP adresi değişirse. Bkz: [PowerShell kullanarak yerel ağ geçidi ayarlarını değiştirme](vpn-gateway-modify-local-network-gateway.md).

## <a name="resources"></a>REST API'ler, PowerShell cmdlet'leri ve CLI

Ek teknik kaynaklar ve REST API'ler, PowerShell cmdlet'leri veya Azure CLI için VPN ağ geçidi yapılandırmaları kullanırken belirli bir söz dizimi gereksinimler için şu sayfalara bakın:

| **Klasik** | **Resource Manager** |
| --- | --- |
| [PowerShell](/powershell/module/az.network/#networking) |[PowerShell](/powershell/module/az.network#vpn) |
| [REST API](https://msdn.microsoft.com/library/jj154113) |[REST API](/rest/api/network/virtualnetworkgateways) |
| Desteklenmiyor | [Azure CLI](/cli/azure/network/vnet-gateway)|

## <a name="next-steps"></a>Sonraki adımlar

Kullanılabilir bağlantı yapılandırmaları hakkında daha fazla bilgi için bkz. [VPN Gateway hakkında](vpn-gateway-about-vpngateways.md).
