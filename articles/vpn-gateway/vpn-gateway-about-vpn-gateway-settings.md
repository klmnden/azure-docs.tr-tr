---
title: "Şirket içi Azure bağlantıları için VPN ağ geçidi ayarları | Microsoft Docs"
description: "Azure sanal ağ geçitleri için VPN ağ geçidi ayarları hakkında bilgi edinin."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: ae665bc5-0089-45d0-a0d5-bc0ab4e79899
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/05/2018
ms.author: cherylmc
ms.openlocfilehash: e4f02e2b001b6821e732cead660aa0b758f1133e
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="about-vpn-gateway-configuration-settings"></a>VPN ağ geçidi yapılandırma ayarları hakkında

Bir VPN ağ geçidi, sanal ağınızı ve şirket içi konumunuz arasındaki şifrelenmiş trafik ortak bir bağlantı üzerinden gönderir sanal ağ geçidi türüdür. Azure omurga üzerinden sanal ağlar arasında trafiği göndermek için bir VPN ağ geçidi'ni de kullanabilirsiniz.

Bir VPN gateway bağlantısı her biri yapılandırılabilir ayarları içeren yapılandırmasına göre birden fazla kaynağı kullanır. Bu makalede bölümlerde kaynakları ve Resource Manager dağıtım modelinde oluşturulmuş bir sanal ağ için bir VPN ağ geçidi ile ilgili ayarları açıklanmaktadır. Her bağlantı çözümünüz için açıklamaları ve topoloji diyagramları bulabilirsiniz [VPN Gateway hakkında](vpn-gateway-about-vpngateways.md) makalesi.

>[!NOTE]
> Bu makalede değerleri - GatewayType 'Vpn' kullanan sanal ağ geçitleri için geçerlidir. VPN ağ geçidi olarak adlandırılır nedeni budur. -GatewayType için 'ExpressRoute' uygulamak değerleri için bkz: [ExpressRoute için sanal ağ geçitleri](../expressroute/expressroute-about-virtual-network-gateways.md). ExpressRoute ağ geçidi değerlerini VPN ağ geçitleri için kullandığınız aynı değerleri değildir.
>
>

## <a name="gwtype"></a>Ağ geçidi türleri

Her sanal ağ, yalnızca bir sanal ağ geçidi her tür olabilir. Bir sanal ağ geçidi oluştururken, ağ geçidi türü yapılandırmanız için doğru olduğundan emin olmanız gerekir.

-GatewayType kullanılabilir değerler:

* VPN
* ExpressRoute

Bir VPN ağ geçidi gerektirir `-GatewayType` *Vpn*.

Örnek:

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased
```

## <a name="gwsku"></a>Ağ Geçidi SKU'ları

[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configure-the-gateway-sku"></a>Ağ geçidi SKU'su yapılandırın

#### <a name="azure-portal"></a>Azure portalına

Bir Resource Manager sanal ağ geçidi oluşturmak için Azure portalını kullanıyorsanız, açılan listeyi kullanarak ağ geçidi SKU'su seçebilirsiniz. İle sunulan seçenekler, seçtiğiniz VPN türü ve ağ geçidi türü için karşılık gelir.

#### <a name="powershell"></a>PowerShell

Aşağıdaki PowerShell örnek belirtir `-GatewaySku` VpnGw1 olarak.

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewaySku VpnGw1 `
-GatewayType Vpn -VpnType RouteBased
```

#### <a name="resize"></a>Değişiklik (boyutlandırma) bir ağ geçidi SKU'su

Daha güçlü bir SKU, ağ geçidi SKU'su yükseltmek istiyorsanız, kullanabileceğiniz `Resize-AzureRmVirtualNetworkGateway` PowerShell cmdlet'i. Ayrıca ağ geçidi Bu cmdlet'i kullanarak SKU boyutunu düşürmek.

Aşağıdaki PowerShell örnek bir ağ geçidi için VpnGw2 yeniden boyutlandırılan SKU gösterir.

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

## <a name="connectiontype"></a>Bağlantı türleri

Resource Manager dağıtım modelinde, her yapılandırma belirli sanal ağ geçidi bağlantı türü gerektirir. `-ConnectionType` için kullanılabilir Resource Manager PowerShell değerleri şunlardır:

* IPsec
* Vnet2Vnet
* ExpressRoute
* VPNClient

Aşağıdaki PowerShell örnekte, bağlantı türü gerektiren bir S2S bağlantı oluşturuyoruz *IPSec*.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
-ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
```

## <a name="vpntype"></a>VPN türleri

VPN ağ geçidi yapılandırması için sanal ağ geçidi oluşturduğunuzda, bir VPN türü belirtmeniz gerekir. Seçtiğiniz VPN türü oluşturmak istediğiniz bağlantı topolojisine bağlıdır. Örneğin, P2S bağlantısı RouteBased VPN türü gerektirir. Bir VPN türü ayrıca kullandığınız donanımda bağlı olabilir. S2S yapılandırmaları bir VPN cihazı gerektirir. Bazı VPN cihazlarının yalnızca belirli bir VPN türü destekler.

Seçtiğiniz VPN türü oluşturmak istediğiniz tüm bağlantı çözümünün gereksinimlerini karşılaması gerekir. Örneğin, bir S2S VPN gateway bağlantısı ve P2S VPN ağ geçidi bağlantısı aynı sanal ağ oluşturmak istiyorsanız, VPN türü kullanırsınız *RouteBased* çünkü P2S RouteBased VPN türü gerektirir. VPN Cihazınızı RouteBased VPN bağlantısı desteklenen doğrulamak gerekir. 

Bir sanal ağ geçidi oluşturulduktan sonra VPN türünü değiştiremezsiniz. Sanal ağ geçidini silin ve yeni bir tane oluşturmanız gerekir. İki VPN türü vardır:

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

Aşağıdaki PowerShell örnek belirtir `-VpnType` olarak *RouteBased*. Bir ağ geçidi oluştururken, -VpnType öğesinin yapılandırmanız için doğru olduğundan emin olmanız gerekir.

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig `
-GatewayType Vpn -VpnType RouteBased
```

## <a name="requirements"></a>Ağ geçidi gereksinimleri

[!INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)]

## <a name="gwsub"></a>Ağ geçidi alt ağı

Bir VPN ağ geçidi oluşturmadan önce bir ağ geçidi alt ağı oluşturmanız gerekir. Ağ geçidi alt ağı sanal ağ geçidi sanal makineleri ve hizmetleri kullanan IP adreslerini içerir. Sanal ağ geçidinizi oluşturduğunuzda, ağ geçidi VM ağ geçidi alt ağına dağıtılan ve gerekli VPN ağ geçidi ayarlarıyla yapılandırılır. Hiçbir zaman başka bir şey (örneğin, ek VM'ler) ağ geçidi alt ağına dağıtmalısınız. Ağ geçidi alt ağı 'GatewaySubnet' adlı gerekir düzgün çalışması için. Ağ geçidi alt ağı 'GatewaySubnet' adlandırma, bu sanal ağ geçidi sanal makineleri ve Hizmetleri dağıtmak için alt olduğunu biliyor Azure olanak sağlar.

Ağ geçidi alt ağı oluştururken, alt ağın içerdiği IP adresi sayısını belirtirsiniz. Ağ geçidi alt ağdaki IP adresleri ağ geçidi sanal makineleri ve ağ geçidi Hizmetleri ayrılır. Bazı yapılandırmalar için diğerlerinden daha fazla IP adresi gerekir. Oluşturma ve oluşturmak istediğiniz ağ geçidi alt ağı bu gereksinimleri karşıladığını doğrulamak istediğiniz yapılandırma yönergelerini bakın. Ayrıca, ağ geçidi alt ağınızı gelecekteki olası ek yapılandırmalar karşılamak için yeterli IP adreslerini içerdiğinden emin olmak isteyebilirsiniz. Bir ağ geçidi alt ağı/29 kadar küçük oluşturabilirsiniz, ancak 28 ya da daha büyük bir ağ geçidi alt ağı oluşturmanızı öneririz (/ 28, / 27, /26 vs.). İşlevselliği gelecekte eklerseniz, bu şekilde, ağ geçidiniz, kesmeden sonra silip için daha fazla IP adresine izin vermek için ağ geçidi alt ağı gerekmez.

Aşağıdaki Resource Manager PowerShell örnek GatewaySubnet adlı bir ağ geçidi alt ağı gösterir. Şu anda mevcut çoğu yapılandırma için yeterli IP adresine izin veren bir/27 CIDR gösteriminde belirtir görebilirsiniz.

```powershell
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="lng"></a>Yerel ağ geçitleri

Bir VPN ağ geçidi yapılandırması oluştururken, yerel ağ geçidi genellikle şirket içi konumunuzu temsil eder. Klasik dağıtım modelinde, yerel ağ geçidi için Yerel Site olara ifade edilir. 

Yerel ağ geçidi, şirket içi VPN cihazının genel IP adresi olmak üzere bir ad verin ve şirket içi konumunda yer alan adres öneklerini belirtirsiniz. Azure ağ trafiği için hedef adres öneklerine bakar, yerel ağ geçidiniz için belirttiğiniz yapılandırma bakar ve paketleri buna uygun şekilde yönlendirir. Ayrıca, VPN ağ geçidi bağlantısı kullanma VNet-VNet yapılandırmaları için yerel ağ geçitleri de belirtin.

Aşağıdaki PowerShell örnek yeni bir yerel ağ geçidi oluşturur:

```powershell
New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```

Bazen yerel ağ geçidi ayarlarını değiştirmeniz gerekir. Örneğin, eklediğinizde veya adres aralığını değiştirmek veya VPN cihazının IP adresi değişip değişmediğini. Bkz: [PowerShell kullanarak yerel ağ geçidi ayarlarını değiştirmek](vpn-gateway-modify-local-network-gateway.md).

## <a name="resources"></a>REST API ve PowerShell cmdlet'leri

Ek teknik kaynaklar ve REST API'leri, PowerShell cmdlet'lerini veya Azure CLI için VPN ağ geçidi yapılandırmaları kullanırken belirli sözdizimi gereksinimleri için aşağıdaki sayfalarına bakın:

| **Klasik** | **Resource Manager** |
| --- | --- |
| [PowerShell](/powershell/module/azure#networking) |[PowerShell](/powershell/module/azurerm.network#vpn) |
| [REST API](https://msdn.microsoft.com/library/jj154113) |[REST API](/rest/api/network/virtualnetworkgateways) |
| Desteklenmiyor | [Azure CLI](/cli/azure/network/vnet-gateway)|

## <a name="next-steps"></a>Sonraki adımlar

Bir bağlantı yapılandırmaları hakkında daha fazla bilgi için bkz: [VPN Gateway hakkında](vpn-gateway-about-vpngateways.md).
