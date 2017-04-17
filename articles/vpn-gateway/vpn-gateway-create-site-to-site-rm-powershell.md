---
title: "Şirket içi ağınızı bir Azure sanal ağına bağlama: Siteden Siteye VPN: PowerShell | Microsoft Docs"
description: "Şirket içi ağınız ile bir Azure sanal ağı arasında genel İnternet üzerinden bir IPSec bağlantısı oluşturma adımları. Bu adımlar PowerShell kullanarak Siteden Siteye şirket içi ve dışı karışık VPN Gateway bağlantısı oluşturmanıza yardımcı olur."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fcc2fda5-4493-4c15-9436-84d35adbda8e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 4605ac9fe40f369d562dfcbf2abe7403f307d2a9
ms.lasthandoff: 04/12/2017


---
# <a name="create-a-vnet-with-a-site-to-site-vpn-connection-using-powershell"></a>PowerShell kullanarak Siteden Siteye VPN bağlantısı ile sanal ağ oluşturma

Siteden Siteye (S2S) VPN ağ geçidi bağlantısı, IPSec/IKE (IKEv1 veya IKEv2) VPN tüneli üzerinden kurulan bir bağlantıdır. Bu bağlantı türü için, şirket içinde ortak IP adresi atanmış olan ve NAT'nin arkasında bulunmayan bir VPN cihazı gerekir. Siteden Siteye bağlantılar, şirket içi ve dışı karışık yapılandırmalar ve karma yapılandırmalar için kullanılabilir.

![Siteden Siteye şirket içi ve dışı karışık VPN Gateway bağlantısı diyagramı](./media/vpn-gateway-create-site-to-site-rm-powershell/site-to-site-connection-diagram.png)

Bu makalede, Azure Resource Manager dağıtım modelini kullanarak şirket içi ağınıza yönelik bir Siteden Siteye VPN ağ geçidi bağlantısı oluşturma ve sanal ağ oluşturma ile ilgili izleyeceğiniz yol gösterilmektedir. Siteden Siteye bağlantılar, şirket içi ve dışı karışık yapılandırmalar ve karma yapılandırmalar için kullanılabilir. Ayrıca aşağıdaki listeden farklı bir seçenek belirleyerek, farklı dağıtım araçlarıyla bu yapılandırmayı oluşturabilirsiniz:

> [!div class="op_single_selector"]
> * [Resource Manager - Azure portalı](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Klasik - Azure portalı](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Klasik - klasik portal](vpn-gateway-site-to-site-create.md)
>
>

#### <a name="additional-configurations"></a>Ek yapılandırmalar
Sanal ağları birbirine bağlamak istiyor ancak şirket içi bir konuma bağlantı oluşturmuyorsanız, bkz. [VNet’ten VNet’e bağlantıyı yapılandırma](vpn-gateway-vnet-vnet-rm-ps.md). Zaten bağlantısı bulunan bir sanal ağa Siteden Siteye bağlantı eklemek istiyorsanız bkz. [VPN ağ geçidi bağlantısı bulunan bir sanal ağa S2S bağlantısı ekleme](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).


## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Yapılandırmaya başlamadan önce aşağıdaki öğelerin bulunduğunu doğrulayın:

* Uyumlu bir VPN cihazı ve bu cihazı yapılandırabilecek biri. Bkz. [VPN Cihazları Hakkında](vpn-gateway-about-vpn-devices.md). VPN cihazınızı yapılandırma konusuyla veya şirket içi ağ yapılandırmanızdaki IP adresi aralıklarıyla ilgili fazla bilginiz yoksa size bu ayrıntıları sağlayabilecek biriyle çalışmanız gerekir.
* VPN cihazınız için dışarıya yönelik genel bir IP adresi. Bu IP adresi bir NAT’nin arkasında olamaz.
* Azure aboneliği. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial) için kaydolabilirsiniz.
* Azure Resource Manager PowerShell cmdlet'lerinin son sürümü. PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs).

## <a name="Login"></a>1. Aboneliğinize bağlanma
Resource Manager cmdlet’lerini kullanmak için PowerShell moduna geçtiğinizden emin olun. Daha fazla bilgi için [Windows PowerShell’i Resource Manager ile kullanma](../powershell-azure-resource-manager.md) konusuna bakın.

PowerShell konsolunuzu açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

```powershell
Login-AzureRmAccount
```

Hesapla ilişkili abonelikleri kontrol edin.

```powershell
Get-AzureRmSubscription
```

Kullanmak istediğiniz aboneliği belirtin.

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

## <a name="VNet"></a>2. Sanal ağ ve ağ geçidi alt ağı oluşturma
Örneklerde /28 boyutunda bir ağ geçidi alt ağı kullanılmaktadır. /29 kadar küçük bir ağ geçidi alt ağı oluşturmak mümkün olsa da en az /28 veya /27’yi seçerek daha fazla adres içeren büyük bir alt ağ oluşturmanızı öneririz. Bu, gelecekte isteyebileceğiniz ek yapılandırmaları da içerecek yeteri kadar adres sağlayacaktır.

/29 veya daha büyük bir ağ geçidi alt ağına sahip bir sanal ağınız varsa, [Yerel ağ geçidinizi ekleme](#localnet) bölümüne atlayabilirsiniz.

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="to-create-a-virtual-network-and-a-gateway-subnet"></a>Sanal ağ ve ağ geçidi alt ağı oluşturmak için
Bir sanal ağ ve ağ geçidi alt ağı oluşturmak için aşağıdaki örneği kullanın:

İlk olarak bir kaynak grubu oluşturun:

```powershell
New-AzureRmResourceGroup -Name testrg -Location 'West US'
```

Ardından, sanal ağınızı oluşturun. Belirlediğiniz adres alanlarının, şirket içi ağınızdaki adres alanlarından herhangi biriyle çakışmadığını doğrulayın.

Aşağıdaki örnekte *testvnet* adlı bir sanal ağın yanı sıra biri *GatewaySubnet* adında, diğeri ise *Subnet1* adında olmak üzere iki alt ağ oluşturulmaktadır. Adı özellikle *GatewaySubnet* olan bir alt ağ oluşturmak önemlidir. Başka bir ad kullanırsanız bağlantı yapılandırmanız başarısız olur.

Değişkenleri ayarlayın.

```powershell
$subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/28
$subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix '10.0.1.0/28'
```

VNet'i oluşturun.

```powershell
New-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg `
-Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet1, $subnet2
```

### <a name="gatewaysubnet"></a>Önceden oluşturduğunuz bir sanal ağa, bir ağ geçidi alt ağı eklemek için
Bu adım, yalnızca önceden oluşturduğunuz bir sanal ağa, bir ağ geçidi alt ağı eklemeye ihtiyaç duyduğunuz durumlarda gereklidir.

Aşağıdaki örneği kullanarak ağ geçidi alt ağınızı oluşturabilirsiniz. Ağ geçidi alt ağına 'GatewaySubnet' adını verdiğinizden emin olun. Başka bir ad kullanırsanız alt ağ oluşturulur ancak Azure bunu bir ağ geçidi alt ağı olarak değerlendirmez.

Değişkenleri ayarlayın.

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName testrg -Name testvnet
```

Ağ geçidi alt ağını oluşturun.\

```powershell
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/28 -VirtualNetwork $vnet
```

Yapılandırmayı ayarlayın.

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

## 3. <a name="localnet"></a>Yerel ağ geçidinizi ekleme
Sanal bir ağda, yerel ağ geçidi genellikle şirket içi konumunuz anlamına gelir. Bu siteye Azure'ın başvururken kullanabileceği bir ad verin. Ayrıca yerel ağ geçidine ait adres alanı ön ekini belirtin.

Azure, belirttiğiniz IP adresi ön ekini kullanarak hangi trafiğin şirket içi konumunuza gönderileceğini belirler. Bu nedenle yerel ağ geçidinizle ilişkili olmasını istediğiniz tüm adres ön eklerini belirtmeniz gerekir. Şirket içi ağınızda bir değişiklik olursa bu ön ekleri kolayca güncelleştirebilirsiniz.

PowerShell örneklerini kullanırken şunlara dikkat edin:

* *GatewayIPAddress* şirket içi VPN cihazınızın IP adresidir. VPN cihazınız bir NAT’nin arkasında olamaz.
* *AddressPrefix* şirket içi adres alanınızdır.

Tek bir adres ön ekine sahip bir yerel ağ geçidi eklemek için:

```powershell
New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```

Birden çok adres ön ekine sahip bir yerel ağ geçidi eklemek için:

```powershell
New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')
```

### <a name="to-modify-ip-address-prefixes-for-your-local-network-gateway"></a>Yerel ağ geçidinizin IP adresi ön eklerini değiştirmek için
Bazen yerel ağ geçidi ön ekleriniz değişir. IP adresi ön eklerinizi değiştirmek için uygulayacağınız adımlar bir VPN ağ geçidi bağlantısı oluşturup oluşturmadığınıza göre değişir. Bu makalenin [Yerel bir ağ geçidinin IP adresi ön eklerini değiştirme](#modify) bölümüne bakın.

## <a name="PublicIP"></a>4. VPN ağ geçidi için genel bir IP adresi isteme
Bir sonraki adımda, Azure VNet VPN ağ geçidinize genel bir IP adresinin ayrılmasını isteyeceksiniz. Bu, VPN cihazınıza atanan IP adresinin aynısı değildir. Bu IP adresi, Azure VPN ağ geçidinin kendisine atanır. Kullanmak istediğiniz IP adresini belirtemezsiniz. IP adresi, ağ geçidinize dinamik olarak ayrılır. Şirket içi VPN cihazınızı ağ geçidine bağlanmak üzere yapılandırırken bu IP adresini kullanırsınız.

Resource Manager dağıtım modeline ait Azure VPN ağ geçidi, genel IP adreslerini şu anda yalnızca Dinamik Ayırma yöntemini kullanarak desteklemektedir. Ancak bu, IP adresinin değiştiği anlamına gelmez. Azure VPN ağ geçidi IP adresi, yalnızca ağ geçidi silinip yeniden oluşturulduğunda değişir. Ağ geçidi genel IP adresi, Azure VPN ağ geçidiniz üzerinde gerçekleştirilen yeniden boyutlandırma, sıfırlama veya diğer iç bakım/yükseltme işlemleri sırasında değişmez.

Aşağıdaki PowerShell örneğini kullanın:

```powershell
$gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg -Location 'West US' -AllocationMethod Dynamic
```

## <a name="GatewayIPConfig"></a>5. Ağ geçidi IP adresleme yapılandırmasını oluşturma
Ağ geçidi yapılandırması, kullanılacak alt ağı ve genel IP adresini tanımlar. Ağ geçidi yapılandırmanızı oluşturmak için aşağıdaki örneği kullanın:

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
```

## <a name="CreateGateway"></a>6. Sanal ağ geçidini oluşturma
Bu adımda sanal ağ geçidini oluşturursunuz. Ağ geçidi oluşturma işlemi uzun sürebilir. Bu süre genellikle 45 dakika veya daha fazladır.

Aşağıdaki değerleri kullanın:

* Siteden Siteye bir yapılandırma için *-GatewayType* değeri *Vpn* olmalıdır. Ağ geçidi türü her zaman, uygulamakta olduğunuz yapılandırmaya özeldir. Örneğin, başka ağ geçidi yapılandırmalarında -GatewayType değerinin ExpressRoute olması gerekebilir.
* *-VpnType*, *RouteBased* (bazı belgelerde Dinamik Ağ Geçidi olarak adlandırılır) veya *PolicyBased* (bazı belgelerde Statik Ağ Geçidi olarak adlandırılır) olabilir. VPN ağ geçidi türleri hakkında daha fazla bilgi için bkz. [VPN Gateway Hakkında](vpn-gateway-about-vpngateways.md).
* *-GatewaySku* değeri *Basic*, *Standard* veya *HighPerformance* olabilir.

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased -GatewaySku Standard
```

## <a name="ConfigureVPNDevice"></a>7. VPN cihazınızı yapılandırma
Bu noktada, şirket içi VPN cihazınızı yapılandırmak için sanal ağ geçidinin genel IP adresi gereklidir. Belirli yapılandırma bilgilerini edinmek için cihazınızın üreticisiyle iş birliği yapın. Daha fazla bilgi edinmek için [VPN Cihazları](vpn-gateway-about-vpn-devices.md) başlıklı makaleye bakabilirsiniz.

Sanal ağ geçidinizin genel IP adresini bulmak için aşağıdaki örneği kullanın:

```powershell
Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg
```

## <a name="CreateConnection"></a>8. VPN bağlantısını oluşturma
Bir sonraki adımda, sanal ağ geçidiniz ile VPN cihazınız arasındaki Siteden Siteye VPN bağlantısını oluşturacaksınız. Değerlerin kendinizinkilerle değiştirildiğinden emin olun. Paylaşılan anahtar, VPN cihazınızın yapılandırması için kullandığınız değerin aynısı olmalıdır. Siteden Siteye bağlantı için `-ConnectionType` değerinin *IPsec* olduğunu unutmayın.

Değişkenleri ayarlayın.

```powershell
$gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
$local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
```

Bağlantıyı oluşturun.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
-ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
```

Kısa bir süre içerisinde bağlantı kurulur.

## <a name="toverify"></a>VPN bağlantısını doğrulamak için
VPN bağlantınızı doğrulamanın birkaç farklı yolu vardır.

[!INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="modify"></a>Yerel bir ağ geçidinin IP adresi ön eklerini değiştirmek için
Yerel ağ geçidiniz için ön ekleri değiştirmeniz gerekirse aşağıdaki yönergeleri uygulayın. İki ayrı yönerge grubu sunulmuştur. Hangi yönergeleri seçeceğiniz, ağ geçidi bağlantınızı önceden oluşturup oluşturmadığınıza bağlıdır.

[!INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="modifygwipaddress"></a>Yerel bir ağ geçidi için ağ geçidi IP adresini değiştirme
[!INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
*  Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).
* BGP hakkında bilgi edinmek için [BGP’ye Genel Bakış](vpn-gateway-bgp-overview.md) ve [BGP’yi yapılandırma](vpn-gateway-bgp-resource-manager-ps.md) makalelerine bakın.

