<properties
   pageTitle="Azure Resource Manager ve PowerShell kullanarak Siteden Siteye VPN bağlantısı olan bir sanal ağ oluşturma | Microsoft Azure"
   description="Bu makalede, Resource Manager modelini kullanarak bir sanal ağ oluşturmak ve bir S2S VPN ağ geçidi bağlantısı kullanarak söz konusu ağı yerel şirket içi ağınıza bağlamak adım adım açıklanmaktadır."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/13/2016"
   ms.author="cherylmc"/>

# PowerShell ve Azure Resource Manager kullanarak Siteden Siteye VPN bağlantısı olan bir sanal ağ oluşturma

> [AZURE.SELECTOR]
- [Azure Portalı](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Klasik Azure Portalı](vpn-gateway-site-to-site-create.md)
- [PowerShell - Resource Manager](vpn-gateway-create-site-to-site-rm-powershell.md)

Bu makalede, Azure Resource Manager dağıtım modelini kullanarak sanal bir ağ ve şirket içi ağınıza bir Siteden Siteye VPN bağlantısı oluşturmak adım adım açıklanmaktadır. Siteden Siteye bağlantılar, şirket içi ve dışı karışık yapılandırmalar ve karma yapılandırmalar için kullanılabilir. 


**Azure dağıtım modelleri hakkında**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## Bağlantı diyagramı 

![Siteden Siteye diyagram](./media/vpn-gateway-create-site-to-site-rm-powershell/site2site.png "site-to-site")

**Siteden Siteye bağlantılar için dağıtım modelleri ve araçları**

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)]

Sanal ağları birbirine bağlamak istiyor ancak şirket içi bir konuma bağlantı oluşturmuyorsanız, bkz. [Configure a VNet-to-VNet connection](vpn-gateway-vnet-vnet-rm-ps.md) (Sanal ağdan sanal ağa bağlantıyı yapılandırma). Farklı türde bir bağlantı yapılandırması istiyorsanız [VPN Gateway bağlantı topolojileri](vpn-gateway-topology.md) makalesine bakın.


## Başlamadan önce

Yapılandırmaya başlamadan önce aşağıdaki öğelerin bulunduğunu doğrulayın.

- Uyumlu bir VPN cihazı ve bu cihazı yapılandırabilecek biri. Bkz. [VPN Cihazları Hakkında](vpn-gateway-about-vpn-devices.md). VPN cihazınızı yapılandırmak konusunda veya şirket içi ağ yapılandırmanızdaki IP adresi aralıklarıyla ilgili bilginiz yoksa, bu detayları sağlayabilecek biriyle koordine olmanız gerekir.

- VPN cihazınız için dışarıya yönelik genel bir IP adresi. Bu IP adresi bir NAT’nin arkasında olamaz.
    
- Azure aboneliği. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
    
- Azure Resource Manager PowerShell cmdlet’lerinin en yeni sürümünü yüklemeniz gerekir. PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](../powershell-install-configure.md).


## 1. Aboneliğinize bağlanma 

Resource Manager cmdlet’lerini kullanmak için PowerShell moduna geçtiğinizden emin olun. Daha fazla bilgi için [Windows PowerShell’i Resource Manager ile kullanma](../powershell-azure-resource-manager.md) konusuna bakın.

PowerShell konsolunuzu açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

    Login-AzureRmAccount

Hesapla ilişkili abonelikleri kontrol edin.

    Get-AzureRmSubscription 

Kullanmak istediğiniz aboneliği belirtin.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## 2. Sanal ağ ve ağ geçidi alt ağı oluşturma

Aşağıdaki örneklerde /28’lik bir ağ geçidi alt ağı gösterilmektedir. /29 kadar küçük bir ağ geçidi alt ağı oluşturmak mümkün olsa da bu boyut önerilmez. Ek özellik gereksinimlerini karşılamak üzere en az /27 veya daha büyük (/26, /25 vb.) bir ağ geçidi alt ağı öneriyoruz. 

/29 veya daha büyük bir ağ geçidi alt ağına sahip bir sanal ağınız varsa, [Yerel ağ geçidinizi ekleme](#localnet) bölümüne atlayabilirsiniz.


[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]  

### Sanal ağ ve ağ geçidi alt ağı oluşturmak için

Aşağıdaki örneği kullanarak bir sanal ağ ve bir ağ geçidi alt ağı oluşturun. Mevcut değerlerin yerine kendi değerlerinizi koyun. 

İlk olarak bir kaynak grubu oluşturun:
    
    New-AzureRmResourceGroup -Name testrg -Location 'West US'

Ardından, sanal ağınızı oluşturun. Belirlediğiniz adres alanlarının, şirket içi ağınızdaki adres alanlarından herhangi biriyle çakışmadığını doğrulayın.

Aşağıdaki örnekte *testvnet* adlı bir sanal ağ ve iki alt ağ oluşturulmaktadır. Alt ağların birinin adı *GatewaySubnet*, diğerinin adı *Subnet1* şeklindedir. Adı özellikle *GatewaySubnet* olan bir alt ağ oluşturmak önemlidir. Başka bir ad kullanırsanız bağlantı yapılandırmanız başarısız olur. 

    $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/28
    $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix '10.0.1.0/28'
    New-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet1, $subnet2

### <a name="gatewaysubnet"></a>Önceden oluşturduğunuz bir sanal ağa, bir ağ geçidi alt ağı eklemek için

Bu adım, yalnızca önceden oluşturduğunuz bir sanal ağa, bir ağ geçidi alt ağı eklemeye ihtiyaç duyduğunuz durumlarda gereklidir.

Aşağıdaki örneği kullanarak ağ geçidi alt ağınızı oluşturabilirsiniz. Ağ geçidi alt ağına 'GatewaySubnet' adını verdiğinizden emin olun. Başka bir ad kullanırsanız, alt ağ oluşturulur ancak Azure bunu bir ağ geçidi alt ağı olarak değerlendirmez.

    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName testrg -Name testvnet
    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/28 -VirtualNetwork $vnet

Şimdi, yapılandırmasını ayarlayın. 

    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

## 3. <a name="localnet"></a>Yerel ağ geçidinizi ekleme

Sanal bir ağda, yerel ağ geçidi genellikle şirket içi konumunuz anlamına gelir. Bu siteye Azure’un başvururken kullanabileceği bir ad verin. Ayrıca, yerel ağ geçidine ait adres alanı ön ekini belirtin. 

Azure, belirttiğiniz IP adresi ön ekini hangi trafiğin şirket içi konumunuza gönderileceğini belirlemek için kullanır. Bu nedenle, yerel ağ geçidinizle ilişkili olmasını istediğiniz tüm adres ön eklerini belirtmeniz gerekir. Şirket içi ağınızda bir değişiklik olursa bu ön ekleri kolayca güncelleştirebilirsiniz. 

PowerShell örneklerini kullanırken şunlara dikkat edin:
    
- *GatewayIPAddress* şirket içi VPN cihazınızın IP adresidir. VPN cihazınız bir NAT’nin arkasında olamaz. 
- *AddressPrefix* şirket içi adres alanınızdır.

Tek bir adres ön ekine sahip bir yerel ağ geçidi eklemek için:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

Birden çok adres ön ekine sahip bir yerel ağ geçidi eklemek için:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')

### Yerel ağ geçidinizin IP adresi ön eklerini değiştirmek için

Bazen yerel ağ geçidi ön ekleriniz değişir. IP adresi ön eklerinizi değiştirmek için uygulayacağınız adımlar bir VPN ağ geçidi bağlantısı oluşturup oluşturmadığınıza bağlıdır. Bu makalenin [Yerel bir ağ geçidinin IP adresi ön eklerini değiştirme](#modify) bölümüne bakın.


## 4. VPN ağ geçidi için genel bir IP adresi isteme

Sırada, Azure Sanal Ağ VPN ağ geçidinize genel bir IP adresinin ayrılmasını istemek var. Bu, VPN cihazınıza atanan IP adresinin aynısı değildir. Bu IP adresi, Azure VPN ağ geçidinin kendisine atanır. Kullanmak istediğiniz IP adresini siz belirleyemezsiniz, IP adresi ağ geçidinize dinamik olarak ayrılır.  Şirket içi VPN cihazınızı ağ geçidine bağlanmak üzere yapılandırırken bu IP adresi kullanılır.

Aşağıdaki PowerShell örneğini kullanın. Bu adres için Ayırma Yöntemi, Dinamik olmalıdır. 

    $gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg -Location 'West US' -AllocationMethod Dynamic

>[AZURE.NOTE] Resource Manager dağıtım modeline ait Azure VPN ağ geçidi, genel IP adreslerini şu anda yalnızca Dinamik Ayırma yöntemini kullanarak desteklemektedir. Ancak bu, IP adresinin değişeceği anlamına gelmez. Azure VPN ağ geçidi IP adresi, yalnızca ağ geçidi silinip yeniden oluşturulduğunda değişir. Ağ geçidi genel IP adresi, Azure VPN ağ geçidiniz üzerinde gerçekleştirilen yeniden boyutlandırma, sıfırlama veya diğer iç bakım/yükseltme işlemleri sırasında değişmez.

## 5. Ağ geçidi IP adresleme yapılandırmasını oluşturma

Ağ geçidi yapılandırması, kullanılacak alt ağı ve genel IP adresini tanımlar. Aşağıdaki örneği kullanarak kendi ağ geçidi yapılandırmanızı oluşturun. 

    $vnet = Get-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 

## 6. Sanal ağ geçidini oluşturma

Bu adımda sanal ağ geçidini oluşturacaksınız. Ağ geçidi oluşturmanın oldukça uzun sürebileceğini unutmayın. Genellikle 20 dakika veya daha fazla zaman alır. 

Aşağıdaki değerleri kullanın:

- Siteden Siteye bir yapılandırma için *-GatewayType* değeri *Vpn* olmalıdır. Ağ geçidi türü her zaman, uygulamakta olduğunuz yapılandırmaya özeldir. Örneğin, başka ağ geçidi yapılandırmalarında -GatewayType değerinin ExpressRoute olması gerekebilir. 

- *-VpnType*, *RouteBased* (bazı belgelerde Dinamik Ağ Geçidi olarak adlandırılır) veya *PolicyBased* (bazı belgelerde Statik Ağ Geçidi olarak adlandırılır) olabilir. VPN ağ geçidi türleri hakkında daha fazla bilgi için bkz. [VPN Ağ Geçitleri Hakkında](vpn-gateway-about-vpngateways.md#vpntype).
- *-GatewaySku* değeri *Basic*, *Standard* veya *HighPerformance* olabilir.   

        New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard

## 7. VPN cihazınızı yapılandırma

Bu noktaya geldiğinizde, şirket içi VPN cihazınızın yapılandırılması için size sanal ağ geçidinin genel IP adresi gerekir. Belirli yapılandırma bilgilerini edinmek için cihazınızın üreticisiyle iş birliği yapın. Ayrıca, daha fazla bilgi için [VPN Cihazları](vpn-gateway-about-vpn-devices.md) makalesine bakın.

Sanal ağ geçidinizin genel IP adresini bulmak için aşağıdaki örneği kullanın:

    Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg

## 8. VPN bağlantısını oluşturma

Şimdi, sanal ağ geçidiniz ile VPN cihazınız arasındaki Siteden Siteye VPN bağlantısını oluşturacaksınız. Değerlerin kendinizinkilerle değiştirildiğinden emin olun. Paylaşılan anahtar, VPN cihazınızın yapılandırması için kullandığınız değerin aynısı olmalıdır. Siteden Siteye bağlantı için `-ConnectionType` değerinin*IPsec* olduğunu unutmayın. 

    $gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

Kısa bir süre içerisinde bağlantı kurulur. 

## 9. VPN bağlantısı doğrulama

VPN bağlantınızı doğrulamanın birkaç farklı yolu vardır. Aşağıda, Azure portalını kullanarak ve PowerShell kullanarak nasıl temel doğrulama yapılacağından bahsedilmektedir.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="modify"></a>Yerel bir ağ geçidinin IP adresi ön eklerini değiştirmek için

Yerel ağ geçidinizin ön eklerini değiştirmeniz gerekirse aşağıdaki yönergeleri uygulayın.  İki ayrı yönerge grubu sunulmuştur. Hangi yönerge grubunu seçeceğiniz, VPN ağ geçidi bağlantınızı önceden oluşturup oluşturmadığınıza bağlıdır. 

[AZURE.INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]


## Sonraki adımlar

- Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Adımlar için bkz. [Sanal Makine Oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md).

- BGP hakkında bilgi edinmek için [BGP’ye Genel Bakış](vpn-gateway-bgp-overview.md) ve [BGP’yi yapılandırma](vpn-gateway-bgp-resource-manager-ps.md) makalelerine bakın.




<!----HONumber=Jun16_HO2-->


