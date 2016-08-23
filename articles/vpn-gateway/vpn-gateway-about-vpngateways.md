<properties 
   pageTitle="VPN Gateway hakkında| Microsoft Azure"
   description="Azure Virtual Network için VPN Gateway hakkında bilgi edinin."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/20/2016"
   ms.author="cherylmc" />

# VPN Gateway hakkında

VPN Gateway sanal ağlar ve şirket içi konumlara arasında ağ trafiği göndermek için kullanılan bir ayar koleksiyonudur. Bu makaledeki bölümler VPN Gateway ile ilgili ayarları ele almaktadır. VPN Gateway Siteden Siteye, Noktadan Siteye ve ExpressRoute bağlantıları için kullanılır. VPN Gateway ayrıca Azure’da (VNet-VNet) birden çok sanal ağ arasında trafik göndermek için de kullanılır. 

VPN Gateway bağlantı oluşturmak üzere bir sanal ağa eklenebilir. Her sanal ağda yalnızca bir VPN Gateway olabilir ve her bağlantı için özel yapılandırma adımları vardır. Bağlantı diyagramları için bkz. [VPN Gateway bağlantı topolojileri](vpn-gateway-topology.md). 

## <a name="gwsku"></a>Ağ geçidi SKU'ları

VPN ağ geçidi oluşturduğunuzda, kullanmak istediğiniz ağ geçidi SKU’sunu belirtmeniz gerekir. Gateway SKU’ları hem ExpressRoute hem de Vpn gateway türleri için geçerlidir. Ağ geçidi SKU'ları arasında fiyatlandırma farklılık gösterir. Fiyatlandırma hakkında daha fazla bilgi için, bkz. [VPN Gateway fiyatlandırması](https://azure.microsoft.com/pricing/details/vpn-gateway/). ExpressRoute hakkında daha fazla bilgi için bkz. [ExpressRoute’a Teknik Genel Bakış](../expressroute/expressroute-introduction.md).

3 VPN Gateway SKU'su vardır:

- Temel
- Standart
- HighPerformance

Aşağıdaki örnek `-GatewaySku` öğesini *Standart* olarak belirtir.

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg -Location 'West US' -IpConfigurations $gwipconfig -GatewaySku Standard -GatewayType Vpn -VpnType RouteBased

###  <a name="aggthroughput"></a>SKU ve ağ geçidi türüne göre tahmini toplam verimlilik.


Aşağıdaki tabloda ağ geçidi türleri ve tahmini toplam verimlilik gösterilmiştir. Bu tablo hem Resource Manager hem de klasik dağıtım modellerine uygulanır.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 

## <a name="gwtype"></a>Ağ geçidi türleri

Ağ geçidi türü, ağ geçidinin nasıl bağlandığını belirtir ve Resource Manager dağıtım modeli için gereken yapılandırma ayarıdır. Ağ geçidi türünü, VPN’iniz için rota türünü belirten VPN türü ile karıştırmayın. `-GatewayType` için kullanılabilir değerler şunlardır: 

- VPN
- ExpressRoute


Resource Manager dağıtım modeli için bu örnek -GatewayType öğesini *Vpn* olarak belirtir. Bir ağ geçidi oluştururken, ağ geçidi öğesinin yapılandırmanız için doğru olduğundan emin olmanız gerekir. 

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

## <a name="connectiontype"></a>Bağlantı türleri

Her bağlantı belirli bir bağlantı türü gerektirir. `-ConnectionType` için kullanılabilir Resource Manager PowerShell değerleri şunlardır:

- IPsec
- Vnet2Vnet
- ExpressRoute
- VPNClient

Aşağıdaki örnekte, "IPsec" bağlantı türü gerektiren bir Siteden Siteye bağlantı oluşturuyoruz.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

## <a name="vpntype"></a>VPN türleri

Her yapılandırma çalışmak için belirli bir VPN türü gerektirir. Aynı VNet’e Siteden Siteye bağlantı ve Noktadan Siteye bağlantı oluşturma gibi iki yapılandırmayı birleştiriyorsanız, her iki bağlantı gereksinimini de karşılayan bir VPN türü kullanmalısınız. 

Noktadan Siteye ve Siteden Siteye birlikte bağlantı bulunması durumunda, Azure Resource Manager dağıtım modeliyle çalışırken rota tabanlı VPN türü ya da klasik dağıtım modeliyle çalışıyorsanız dinamik ağ geçidi kullanmalısınız.

Yapılandırmanızı oluşturduğunuzda, bağlantınız için gerekli olan VPN türünü seçersiniz. 

İki VPN türü vardır:

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

Resource Manager dağıtım modeli için bu örnek `-VpnType` öğesini *RouteBased* olarak belirtir. Bir ağ geçidi oluştururken, -VpnType öğesinin yapılandırmanız için doğru olduğundan emin olmanız gerekir. 

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

##  <a name="requirements"></a>Ağ geçidi gereksinimleri

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)] 


## <a name="gwsub"></a>Ağ geçidi alt ağı

Bir VPN ağ geçidi yapılandırmak için, önce VNet’inizi için bir ağ geçidi alt ağı oluşturmanız gerekir. Ağ geçidi alt ağı düzgün çalışması için *GatewaySubnet* şeklinde adlandırılmalıdır. Bu ad, Azure’un bu alt ağın ağ geçidi için kullanılması gerektiğini bilmesini sağlar. <BR>Klasik portalı kullanıyorsanız, ağ geçidi alt ağı portal arabiriminde otomatik olarak *Gateway* adını alır. Bu yalnızca ağ geçidi alt ağını klasik portalda görüntülemeye özgüdür. Bu durumda, alt Azure’da aslında *GatewaySubnet* olarak oluşturulur ve Azure portalda ve PowerShell’de bu şekilde görüntülenebilir.

Ağ geçidi alt ağı minimum boyutu tümüyle oluşturmak istediğiniz yapılandırmaya bağlıdır. Bazı yapılandırmalar için /29 kadar küçük ağ geçidi alt ağı yapılandırmaları oluşturmak mümkün olmakla birlikte, /28 ya da daha büyük (/28, /27, /26, vb.) ağ geçidi alt ağı oluşturmanızı öneriyoruz. 

Daha büyük bir ağ geçidi boyutu oluşturmak ağ geçidi boyutu sınırlamalarıyla uğraşmanızı önler. Örneğin, /29 ağ geçidi alt ağı boyutuna sahip bir ağ geçidi oluşturduysanız ve Siteden Siteye/ExpressRoute bağlantılarını birlikte yapılandırmak istiyorsanız, ağ geçidini silmeniz, ağ geçidi alt ağını silmeniz, /28 ya da daha büyük bir ağ geçidi alt ağı oluşturmanız ve sonra ağ geçidinizi yeniden oluşturmanız gerekirdi. 

Başlangıçta daha büyük bir ağ geçidi alt ağı oluşturarak, ağ ortamınıza yeni yapılandırma özellikleri eklerken gelecekte zamandan tasarruf edebilirsiniz. 

Aşağıdaki örnek GatewaySubnet adlı bir ağ geçidi alt ağını gösterir. CIDR gösteriminin, bu sırada mevcut çoğu yapılandırma için yeterli IP adresine izin veren /27 değerini belirttiğini görebilirsiniz.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27

>[AZURE.IMPORTANT] Bu, bağlantıların başarısız olmasına neden olabileceğinden, GatewaySubnet’in bir Ağ Güvenlik Grubu’na (NSG) sahip olmadığından emin olun.



## <a name="lng"></a>Yerel ağ geçidi geçitleri

Yerel ağ geçidi genellikle şirket içi konumunuz anlamına gelir. Klasik dağıtım modelinde, yerel ağ geçidi için Yerel Site olara ifade edilir. Yerel ağ geçidine bir ad, şirket içi VPN cihazının genel IP adresini verir ve şirket içi konumunda yer alan adres öneklerini belirtirsiniz. Azure ağ trafiği için hedef adres öneklerine bakar ve sırasıyla yerel ağ geçidiniz için belirttiğiniz yapılandırmaya ve rota paketlerine danışır. Bu adres öneklerini gerektiği gibi değiştirebilirsiniz.


### Adres öneklerini değiştirme - Resource Manager

Adres öneklerini değiştirirken, VPN ağ geçidinizi önceden oluşturup oluşturmamanıza göre, yordam farklılık gösterir. [Yerel ağ geçidini adres öneklerini değiştirme](vpn-gateway-create-site-to-site-rm-powershell.md#modify) makale bölümüne bakın.

Aşağıdaki örnekte, MyOnPremiseWest adlı bir yerel ağ geçidi belirtildiğini ve iki IP adresi öneki içereceğini görebilirsiniz.

    New-AzureRmLocalNetworkGateway -Name MyOnPremisesWest -ResourceGroupName testrg -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24') 

### Adres öneklerini değiştirme - klasik dağıtım

Klasik dağıtım modeli kullanılırken yerel sitelerinizi değiştirmeniz gerekiyorsa, klasik portaldaki Yerel Ağlar yapılandırma sayfasını kullanabilir veya NETCFG.XML Ağ Yapılandırma dosyasını doğrudan değiştirebilirsiniz.



## Sonraki adımlar

Yapılandırmanızı planlama ve tasarlamaya geçmeden önce daha fazla bilgi için, bkz. [VPN Gateway SSS](vpn-gateway-vpn-faq.md).





 



<!--HONumber=Aug16_HO1-->


