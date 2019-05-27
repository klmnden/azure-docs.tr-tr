---
title: 'Azure VPN ağ geçitlerinde BGP yapılandırın: Kaynak Yöneticisi: PowerShell | Microsoft Docs'
description: Bu makalede Azure Resource Manager ve PowerShell kullanarak Azure VPN Gateways ile BGP nasıl yapılandıracağınız açıklanmaktadır.
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: ''
tags: azure-resource-manager
ms.assetid: 905b11a7-1333-482c-820b-0fd0f44238e5
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: yushwang
ms.openlocfilehash: c65ea038fc39702affae93cb68b8cf644393c62e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66150221"
---
# <a name="how-to-configure-bgp-on-azure-vpn-gateways-using-powershell"></a>PowerShell kullanarak Azure VPN Gateways üzerinde BGP yapılandırma
Bu makalede şirket içi siteden siteye (S2S) VPN bağlantısı ve Resource Manager dağıtım modeli ve PowerShell kullanarak VNet-VNet bağlantısı BGP'yi etkinleştirmek için adımlarında size kılavuzluk eder.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="about-bgp"></a>BGP hakkında
BGP iki veya daha fazla ağ arasında yönlendirme ve ulaşılabilirlik bilgilerini takas etmek üzere İnternet’te yaygın olarak kullanılan standart yönlendirme protokolüdür. BGP, her iki ağ geçidini ön eklerin ilgili ağ geçitlerinden veya yönlendiricilerden geçmeye yönelik kullanılabilirliği ve ulaşılabilirliği konusunda bilgilendiren “yolları” değiştirmek amacıyla, Azure VPN Gateways’i ve BGP eşlikleri veya komşuları olarak adlandırılan şirket içi VPN cihazlarınızı etkinleştirir. BGP ayrıca bir BGP ağ geçidinin öğrendiği yolları bir BGP eşliğinden diğer tüm BGP eşliklerine yayarak birden fazla ağ arasında geçiş yönlendirmesi sağlayabilir.

Bkz: [Azure VPN Gateways ile BGP'ye genel bakış](vpn-gateway-bgp-overview.md) daha fazla tartışma avantajları hakkında BGP ve BGP kullanma konuları ve teknik gereksinimleri anlamak için.

## <a name="getting-started-with-bgp-on-azure-vpn-gateways"></a>Azure VPN ağ geçitlerinde BGP ile çalışmaya başlama

Bu makalede, aşağıdaki görevleri gerçekleştirmek için adımları gösterilmektedir:

* [Bölüm 1 - etkinleştirin, Azure VPN Gateway'deki BGP](#enablebgp)
* 2. Kısım - BGP ile şirketler arası bağlantı kurun
* [3. Kısım - BGP ile VNet-VNet bağlantı kurun](#v2vbgp)

Ağ bağlantınızı BGP etkinleştirmek için yapı taşlarından yönergeleri her bir parçasını oluşturur. Üç tüm bölümleri tamamlayın, aşağıdaki diyagramda gösterildiği gibi topoloji derleme:

![BGP topolojisi](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

Birlikte ihtiyaçlarınıza uygun bir daha karmaşık, çok atlamalı aktarım ağı oluşturmak için bölümleri birleştirebilirsiniz.

## <a name ="enablebgp"></a>1. Bölüm - Azure VPN ağ geçidinde BGP yapılandırma
Yapılandırma adımları aşağıdaki diyagramda gösterildiği gibi Azure VPN ağ geçidinin BGP parametreleri ayarlayın:

![BGP ağ geçidi](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a>Başlamadan önce
* Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Azure Resource Manager PowerShell cmdlet'lerini yükleyin. PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview). 

### <a name="step-1---create-and-configure-vnet1"></a>1. adım - oluşturma ve VNet1'i yapılandırma
#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirme
Bu alıştırmada değişkenlerimizi bildirerek başlayın. Aşağıdaki örnek bu alıştırma için değerleri kullanan değişkenler bildirilmektedir. Üretim için yapılandırma sırasında bu değerleri kendi değerlerinizle değiştirdiğinizden emin olun. Bu tür yapılandırmaları tanımaya başlamak için adımları gözden geçiriyorsanız bu değişkenleri kullanabilirsiniz. Değişkenleri değiştirin, daha sonra kopyalayın ve PowerShell konsolunuza yapıştırın.

```powershell
$Sub1 = "Replace_With_Your_Subscription_Name"
$RG1 = "TestBGPRG1"
$Location1 = "East US"
$VNetName1 = "TestVNet1"
$FESubName1 = "FrontEnd"
$BESubName1 = "Backend"
$GWSubName1 = "GatewaySubnet"
$VNetPrefix11 = "10.11.0.0/16"
$VNetPrefix12 = "10.12.0.0/16"
$FESubPrefix1 = "10.11.0.0/24"
$BESubPrefix1 = "10.12.0.0/24"
$GWSubPrefix1 = "10.12.255.0/27"
$VNet1ASN = 65010
$DNS1 = "8.8.8.8"
$GWName1 = "VNet1GW"
$GWIPName1 = "VNet1GWIP"
$GWIPconfName1 = "gwipconf1"
$Connection12 = "VNet1toVNet2"
$Connection15 = "VNet1toSite5"
```

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. Aboneliğinize bağlanın ve yeni bir kaynak grubu oluşturun
Resource Manager cmdlet'lerini kullanmak için PowerShell moduna geçtiğinizden emin olun. Daha fazla bilgi için [Windows PowerShell’i Resource Manager ile kullanma](../powershell-azure-resource-manager.md) konusuna bakın.

PowerShell konsolunuzu açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName $Sub1
New-AzResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a>3. TestVNet1 oluşturma
Aşağıdaki örnekte TestVNet1 ve üç alt ağ, biri GatewaySubnet, biri FrontEnd ve diğeri Backend'dir adlı bir sanal ağ oluşturur. Kendi değerlerinizi yerleştirirken ağ geçidi alt ağınızı özellikle GatewaySubnet olarak adlandırmanız önem taşır. Başka bir ad kullanırsanız ağ geçidi oluşturma işleminiz başarısız olur.

```powershell
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1 $besub1 = New-AzVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-bgp-parameters"></a>2. adım - BGP parametrelerle TestVNet1 için VPN ağ geçidi oluşturun
#### <a name="1-create-the-ip-and-subnet-configurations"></a>1. IP ve alt ağ yapılandırmalarını oluşturun
Sanal ağınız için oluşturacağınız ağ geçidine ayrılacak genel IP adresi isteyin. Ayrıca, gerekli bir alt ağ ve IP yapılandırmaları tanımlarsınız.

```powershell
$gwpip1 = New-AzPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gwipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1
```

#### <a name="2-create-the-vpn-gateway-with-the-as-number"></a>2. VPN ağ geçidi AS numarası ile oluşturma
TestVNet1 için sanal ağ geçidini oluşturun. BGP rota tabanlı VPN ağ geçidi ve - TestVNet1 için ASN (AS numarası) ayarlamak için Asn, ayrıca parametresi gerektirir. ASN parametresi ayarlanmadı, 65515 ASN'si atanır. Bir ağ geçidini oluşturmak biraz zaman alabilir (tamamlanması 30 dakika ya da daha fazla sürer).

```powershell
New-AzVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN
```

#### <a name="3-obtain-the-azure-bgp-peer-ip-address"></a>3. Azure BGP eş IP adresini alın
Ağ geçidi oluşturulduktan sonra Azure VPN Gateway'deki BGP eşdeğer IP adresini almak gerekir. Bu adres, Azure VPN Gateway BGP eşi için şirket içi VPN cihazlarınızı yapılandırmak için gereklidir.

```powershell
$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet1gw.BgpSettingsText
```

Son komut Azure VPN ağ geçidi üzerinde karşılık gelen BGP yapılandırmaları gösterir. Örneğin:

```powershell
$vnet1gw.BgpSettingsText
{
    "Asn": 65010,
    "BgpPeeringAddress": "10.12.255.30",
    "PeerWeight": 0
}
```

Ağ geçidi oluşturulduktan sonra şirket içi veya BGP ile VNet-VNet bağlantısı kurmak için bu ağ geçidi'ni kullanabilirsiniz. Aşağıdaki bölümlerde alıştırma tamamlanması adımlarını inceleyelim.

## <a name ="crossprembbgp"></a>2. Kısım - BGP ile şirketler arası bağlantı kurun

Şirketler arası bağlantı kurmak için şirket içi VPN Cihazınızı temsil etmek için bir yerel ağ geçidi ve yerel ağ geçidi ile VPN ağ geçidine bağlanmak için bir bağlantı oluşturmanız gerekir. Bu adımlarda yardımcı makaleleri olsa da, bu makale, BGP yapılandırma parametreleri belirtmek için gereken ek özellikleri içerir.

![BGP'yi şirketler için](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

Devam etmeden önce tamamladığınızdan emin olun [bölüm 1](#enablebgp) Bu alıştırmada.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>1. adım - oluşturma ve yerel ağ geçidi yapılandırma

#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirme

Bu alıştırmada, derleme yapılandırması Aşağıdaki diyagramda gösterilen devam eder. Değerleri, yapılandırma için kullanmak istediğiniz değerlerle değiştirdiğinizden emin olun.

```powershell
$RG5 = "TestBGPRG5"
$Location5 = "East US 2"
$LNGName5 = "Site5"
$LNGPrefix50 = "10.52.255.254/32"
$LNGIP5 = "Your_VPN_Device_IP"
$LNGASN5 = 65050
$BGPPeerIP5 = "10.52.255.254"
```

Yerel ağ geçidi parametreleri ile ilgili dikkat edilecek noktalar birkaç:

* Yerel ağ geçidi, VPN ağ geçidi olarak aynı veya farklı bir konum ve kaynak grubu içinde olabilir. Bu örnek, bunları farklı kaynak gruplarında farklı konumlarda gösterir.
* Yerel ağ geçidi için bildirmek için gereken ön ek VPN cihazınızın BGP eşdeğer IP adresinizin konak adresidir. Bu durumda, bir özelliğini/32 olduğu "10.52.255.254/32" öneki.
* Farklı BGP Asn'ler şirket içi ağlarınız ve Azure sanal ağı arasında bir anımsatıcı kullanmanız gerekir. Aynı olmaları durumunda, şirket içi VPN cihazınız ile diğer BGP komşu eşlenecek ASN'yi zaten kullanıyorsa, VNet ASN'nizi değiştirmeniz gerekir.

Devam etmeden önce 1. Abonelik’e hala bağlı olduğunuzdan emin olun.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. Site5 için yerel ağ geçidi oluşturma

Yerel ağ geçidi oluşturmadan önce oluşturulduktan değil, kaynak grubu oluşturmak emin olun. Yerel ağ geçidi için iki ek parametreler dikkat edin: ASN ve BgpPeerAddress.

```powershell
New-AzResourceGroup -Name $RG5 -Location $Location5

New-AzLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
```

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>2. adım - sanal ağ geçidi ve yerel ağ geçidine bağlanma

#### <a name="1-get-the-two-gateways"></a>1. İki ağ geçidi Al

```powershell
$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5
```

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. Testvnet1-Site5 bağlantısını oluşturma

Bu adımda, bağlantı için Site5 testvnet1-oluşturursunuz. Belirtmeniz gerekir "-Enablebgp'yi $True" Bu bağlantı için BGP'yi etkinleştirmek için. Daha önce bahsedildiği gibi aynı Azure VPN Gateway için BGP ve BGP olmayan bağlantıları olması mümkündür. BGP bağlantı özelliği etkin değilse, her iki ağ geçitlerinde BGP parametreleri zaten yapılandırılmış olsa bile Azure BGP Bu bağlantı için etkin değildir.

```powershell
New-AzVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

Aşağıdaki örnek bu alıştırma için şirket içi VPN cihazınızın BGP yapılandırması bölümüne girdiğiniz parametreleri listeler:

```

- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP    : 10.12.255.30
- Static route         : Add a route for 10.12.255.30/32, with nexthop being the VPN tunnel interface on your device
- eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed
```

Birkaç dakika sonra bağlantı kurulur ve BGP eşdeğer oturumu IPSec bağlantı kurulduktan sonra başlar.

## <a name ="v2vbgp"></a>3. Kısım - BGP ile VNet-VNet bağlantı kurun

Bu bölümde, aşağıdaki diyagramda gösterildiği gibi bir VNet-VNet bağlantısı BGP ile ekler:

![VNet-VNet için BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

Aşağıdaki yönergeler, önceki adımlardan devam edin. Tamamlamanız gereken [bölüm ı](#enablebgp) oluşturma ve TestVNet1 ve VPN ağ geçidi, BGP ile yapılandırın. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>1. adım - TestVNet2 ve VPN ağ geçidi oluşturma

IP adres alanı yeni sanal ağ TestVNet2, tüm sanal ağ Aralıklarınızın çakışmadığını emin olmak önemlidir.

Bu örnekte, sanal ağlar aynı aboneliğe ait. Farklı abonelikler arasında VNet-VNet bağlantılarında ayarlayabilirsiniz. Daha fazla bilgi için [bir VNet-VNet bağlantısını yapılandırma](vpn-gateway-vnet-vnet-rm-ps.md). Eklediğinizden emin olun "-Enablebgp'yi $True" BGP'yi etkinleştirmek için bir bağlantı oluşturulurken.

#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirme

Değerleri, yapılandırma için kullanmak istediğiniz değerlerle değiştirdiğinizden emin olun.

```powershell
$RG2 = "TestBGPRG2"
$Location2 = "West US"
$VNetName2 = "TestVNet2"
$FESubName2 = "FrontEnd"
$BESubName2 = "Backend"
$GWSubName2 = "GatewaySubnet"
$VNetPrefix21 = "10.21.0.0/16"
$VNetPrefix22 = "10.22.0.0/16"
$FESubPrefix2 = "10.21.0.0/24"
$BESubPrefix2 = "10.22.0.0/24"
$GWSubPrefix2 = "10.22.255.0/27"
$VNet2ASN = 65020
$DNS2 = "8.8.8.8"
$GWName2 = "VNet2GW"
$GWIPName2 = "VNet2GWIP"
$GWIPconfName2 = "gwipconf2"
$Connection21 = "VNet2toVNet1"
$Connection12 = "VNet1toVNet2"
```

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. Yeni kaynak grubunda TestVNet2 oluşturma

```powershell
New-AzResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2
```

#### <a name="3-create-the-vpn-gateway-for-testvnet2-with-bgp-parameters"></a>3. VPN ağ geçidi, BGP parametrelerle TestVNet2 için oluşturma

Sanal ağınıza ait oluşturacak ve gerekli alt ağ ve IP yapılandırması tanımlayın ağ geçidine ayrılacak genel IP adresi isteyin.

```powershell
$gwpip2    = New-AzPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2     = Get-AzVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2   = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gwipconf2 = New-AzVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2
```

VPN ağ geçidi AS numarası ile oluşturun. ASN varsayılan Azure VPN ağ geçitlerinizi üzerinde geçersiz kılmanız gerekir. Bağlı sanal ağlar için Asn'ler BGP ve geçiş yönlendirmesi'ni etkinleştirmek için farklı olmalıdır.

```powershell
New-AzVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard -Asn $VNet2ASN
```

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>2. adım - TestVNet1 ve TestVNet2 ağ geçitlerini bağlama

Bu örnekte, iki ağ geçidi için aynı abonelikte ' dir. Bu adım aynı PowerShell oturumunda tamamlayabilirsiniz.

#### <a name="1-get-both-gateways"></a>1. Her iki ağ geçitlerini Al

1 Abonelikle oturum açıp bağlandığınızdan emin olun.

```powershell
$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet2gw = Get-AzVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
```

#### <a name="2-create-both-connections"></a>2. Her iki bağlantı oluşturma

Bu adımda, TestVNet2 TestVNet1 arasında bağlantı ve bağlantı TestVNet2 TestVNet1'e oluşturun.

```powershell
New-AzVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> Her iki bağlantı için BGP'yi etkinleştirmek emin olun.
> 
> 

Bu adımları tamamladıktan sonra birkaç dakika sonra bağlantı kurulur. VNet-VNet bağlantısı işlemi tamamlandıktan sonra BGP eşdeğer oturumu çalışıyor.

Bu alıştırmada, tüm üç bölümü tamamladıysanız, aşağıdaki ağ topolojisini sağladıktan:

![VNet-VNet için BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## <a name="next-steps"></a>Sonraki adımlar

Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Adımlar için bkz. [Sanal Makine Oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
