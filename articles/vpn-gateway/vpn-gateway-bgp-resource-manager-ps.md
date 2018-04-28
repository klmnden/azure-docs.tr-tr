---
title: 'Azure VPN ağ geçitlerinde BGP yapılandırın: Resource Manager: PowerShell | Microsoft Docs'
description: Bu makalede Azure Resource Manager ve PowerShell kullanarak Azure VPN Gateway'ler ile BGP nasıl yapılandıracağınız anlatılmaktadır.
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
ms.openlocfilehash: fc9337188fd439082c4aa34f0cbebe3eb2da5d99
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="how-to-configure-bgp-on-azure-vpn-gateways-using-powershell"></a>PowerShell kullanarak Azure VPN Gateways üzerinde BGP'yi yapılandırma
Bu makalede, bir şirket içi siteden siteye (S2S) VPN bağlantısı ve Resource Manager dağıtım modeli ve PowerShell kullanarak VNet-VNet bağlantısı BGP'yi etkinleştirmek için adım adım anlatılmaktadır.

## <a name="about-bgp"></a>BGP hakkında
BGP iki veya daha fazla ağ arasında yönlendirme ve ulaşılabilirlik bilgilerini takas etmek üzere İnternet’te yaygın olarak kullanılan standart yönlendirme protokolüdür. BGP, Azure VPN ağ geçitleri ve BGP eşleri veya kullanılabilirliği ve ulaşılabilirliği ön eklerin ilgili ağ geçitlerinden veya yönlendiricilerden söz konusu Git için her iki ağ geçidini konusunda bilgi "yolları" değiştirmek amacıyla Komşuları olarak çağrılır, şirket içi VPN cihazlarınızı etkinleştirir. BGP ayrıca bir BGP ağ geçidinin öğrendiği yolları bir BGP eşliğinden diğer tüm BGP eşliklerine yayarak birden fazla ağ arasında geçiş yönlendirmesi sağlayabilir.

Bkz: [Azure VPN Gateways ile BGP'ye genel bakış](vpn-gateway-bgp-overview.md) daha fazla tartışma avantajları BGP ve BGP kullanma konuları ve teknik gereksinimleri anlamak için.

## <a name="getting-started-with-bgp-on-azure-vpn-gateways"></a>Azure VPN ağ geçitlerinde BGP ile çalışmaya başlama

Bu makalede aşağıdaki görevleri gerçekleştirmek için adımlarda size yol gösterir:

* [Bölüm 1 - etkinleştir BGP, Azure VPN ağ geçidi](#enablebgp)
* [Bölüm 2 - BGP şirketler arası bağlantı Kur](#crossprembgp)
* [Bölüm 3 - BGP VNet-VNet bağlantı Kur](#v2vbgp)

Her bir parçasını yönergeleri BGP ağ bağlantınızı etkinleştirmek için temel yapı bloğu oluşturur. Tüm üç bölümden tamamlarsanız, aşağıdaki çizimde gösterildiği gibi topoloji oluşturun:

![BGP topolojisi](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

Birlikte ihtiyaçlarınıza uygun bir daha karmaşık, çoklu atlamalı geçiş ağ oluşturmak için bölümleri birleştirebilirsiniz.

## <a name ="enablebgp"></a>Bölüm 1 - Azure VPN ağ geçidinde BGP yapılandırma
Yapılandırma adımları, aşağıdaki çizimde gösterildiği gibi Azure VPN ağ geçidi BGP parametreleri ayarlayın:

![BGP ağ geçidi](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a>Başlamadan önce
* Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Azure Resource Manager PowerShell cmdlet'lerini yükleyin. PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview). 

### <a name="step-1---create-and-configure-vnet1"></a>1. adım - oluşturun ve VNet1 yapılandırın
#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirme
Bu alıştırmada, değişkenlerimizi bildirerek başlayın. Aşağıdaki örnekte bu alıştırmada değerleri kullanılan değişkenler bildirilmektedir. Üretim için yapılandırma sırasında bu değerleri kendi değerlerinizle değiştirdiğinizden emin olun. Bu tür yapılandırmaları tanımaya başlamak için adımları gözden geçiriyorsanız bu değişkenleri kullanabilirsiniz. Değişkenleri değiştirin, daha sonra kopyalayın ve PowerShell konsolunuza yapıştırın.

```powershell
$Sub1 = "Replace_With_Your_Subcription_Name"
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

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. Aboneliğinize bağlanmak ve yeni bir kaynak grubu oluştur
Resource Manager cmdlet'lerini kullanmak için PowerShell moduna geçtiğinizden emin olun. Daha fazla bilgi için [Windows PowerShell’i Resource Manager ile kullanma](../powershell-azure-resource-manager.md) konusuna bakın.

PowerShell konsolunuzu açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

```powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a>3. TestVNet1 oluşturma
Aşağıdaki örnek TestVNet1 ve üç alt ağ, biri GatewaySubnet, biri FrontEnd ve diğeri Backend'dir adlı bir sanal ağ oluşturur. Kendi değerlerinizi yerleştirirken ağ geçidi alt ağınızı özellikle GatewaySubnet olarak adlandırmanız önem taşır. Başka bir ad kullanırsanız ağ geçidi oluşturma işleminiz başarısız olur.

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1 $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-bgp-parameters"></a>2. adım - VPN ağ geçidi BGP parametrelerle TestVNet1 için oluşturma
#### <a name="1-create-the-ip-and-subnet-configurations"></a>1. IP ve alt ağ yapılandırmaları oluşturma
Sanal ağınız için oluşturacağınız ağ geçidine ayrılacak genel IP adresi isteyin. Ayrıca gerekli bir alt ağ ve IP yapılandırmaları tanımlamak.

```powershell
$gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1
```

#### <a name="2-create-the-vpn-gateway-with-the-as-number"></a>2. AS numarası ile VPN ağ geçidi oluşturma
TestVNet1 için sanal ağ geçidini oluşturun. BGP bir rota tabanlı VPN ağ geçidi ve ayrıca ek parametre, - TestVNet1 için ASN (AS numarası) ayarlamak için Asn gerektirir. ASN parametre ayarlamazsanız ASN 65515 atanır. Bir ağ geçidini oluşturmak biraz zaman alabilir (tamamlanması 30 dakika ya da daha fazla sürer).

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN
```

#### <a name="3-obtain-the-azure-bgp-peer-ip-address"></a>3. Azure BGP eşdeğer IP adresi al
Ağ geçidi oluşturulduktan sonra Azure VPN Gateway BGP eşdeğer IP adresi edinmeniz gerekir. Bu adres için şirket içi VPN cihazlarınızı BGP eşi olarak Azure VPN ağ geçidi yapılandırmak için gereklidir.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet1gw.BgpSettingsText
```

Son komut karşılık gelen BGP yapılandırmaları Azure VPN ağ geçidinde gösterir; Örneğin:

```powershell
$vnet1gw.BgpSettingsText
{
    "Asn": 65010,
    "BgpPeeringAddress": "10.12.255.30",
    "PeerWeight": 0
}
```

Ağ geçidi oluşturulduktan sonra şirket içi veya BGP ile VNet-VNet bağlantısı kurmak için bu ağ geçidi'ni kullanabilirsiniz. Aşağıdaki bölümlerde alıştırma tamamlamak için adım adım.

## <a name ="crossprembbgp"></a>Bölüm 2 - BGP şirketler arası bağlantı Kur

Şirketler arası bağlantı kurmak için şirket içi VPN aygıtınızın temsil etmek için bir yerel ağ geçidi ve VPN ağ geçidi ile yerel ağ geçidi bağlanmak için bir bağlantı oluşturmanız gerekir. Bu adım adım makaleleri olsa da, bu makalede BGP yapılandırma parametrelerini belirtmek için gereken ek özellikleri içerir.

![Şirket içi BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

Devam etmeden önce tamamladığınızdan emin olun [Kısım 1](#enablebgp) Bu alıştırmada.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>1. adım - oluşturma ve yerel ağ geçidi yapılandırma

#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirme

Bu alıştırmada, aşağıdaki çizimde gösterilen yapılandırması oluşturmak devam eder. Değerleri, yapılandırma için kullanmak istediğiniz değerlerle değiştirdiğinizden emin olun.

```powershell
$RG5 = "TestBGPRG5"
$Location5 = "East US 2"
$LNGName5 = "Site5"
$LNGPrefix50 = "10.52.255.254/32"
$LNGIP5 = "Your_VPN_Device_IP"
$LNGASN5 = 65050
$BGPPeerIP5 = "10.52.255.254"
```

Yerel ağ ağ geçidi parametreleri ile ilgili dikkat edilecek noktalar birkaç:

* Yerel ağ geçidi, VPN ağ geçidi olarak aynı veya farklı konumunu ve kaynak grubu olabilir. Bu örnek, bunları farklı kaynak gruplarında farklı konumlarda gösterir.
* Yerel ağ geçidi için bildirmek için gereken en küçük ön eki BGP eşdeğer IP adresinizin, VPN cihazınızdaki konak adresidir. Bu durumda, bir /32 olan "10.52.255.254/32" öneki.
* Bir anımsatıcı farklı BGP Asn'ler şirket içi ağlarınız ve Azure VNet arasında kullanmanız gerekir. Aynı farklıysa, şirket içi VPN aygıtınızın ASN diğer BGP komşuları ile eş için zaten kullanıyorsa, VNet ASN değiştirmeniz gerekir.

Devam etmeden önce 1. Abonelik’e hala bağlı olduğunuzdan emin olun.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. Yerel ağ geçidi için Site5 oluşturma

Yerel ağ geçidi oluşturmadan önce onu, oluşturulmamışsa kaynak grubu oluşturmak emin olun. Yerel ağ geçidi için iki ek parametreler dikkat edin: Asn ve BgpPeerAddress.

```powershell
New-AzureRmResourceGroup -Name $RG5 -Location $Location5

New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
```

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>2. adım - sanal ağ geçidi ve yerel ağ geçidi Bağlan

#### <a name="1-get-the-two-gateways"></a>1. İki ağ geçidi Al

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5
```

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. Testvnet1-Site5 bağlantısını oluşturma

Bu adımda, bağlantı için Site5 TestVNet1 oluşturun. Belirtmeniz gerekir "-EnableBGP $True" Bu bağlantı için BGP'yi etkinleştirmek için. Daha önce bahsedildiği gibi aynı Azure VPN ağ geçidi için BGP ve BGP olmayan bağlantıları olması mümkündür. BGP bağlantı özelliği etkin değilse, BGP parametreleri her iki ağ geçidini zaten yapılandırılmış olsa bile Azure BGP Bu bağlantı için izin vermez.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

Aşağıdaki örnekte bu alıştırmada, şirket içi VPN cihazınızın BGP yapılandırma bölüme girin parametreleri listelenir:

```

- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP    : 10.12.255.30
- Static route         : Add a route for 10.12.255.30/32, with nexthop being the VPN tunnel interface on your device
- eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed
```

Bağlantı birkaç dakika sonra kurulur ve BGP eşliği oturumu kez bağlantı kuran IPSec başlatır.

## <a name ="v2vbgp"></a>Bölüm 3 - BGP VNet-VNet bağlantı Kur

Bu bölüm, aşağıdaki çizimde gösterildiği gibi BGP ile VNet-VNet bağlantı ekler:

![BGP VNet-VNet için](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

Aşağıdaki yönergelerde yer alan önceki adımları devam edin. Tamamlamanız gereken [bölümü ı](#enablebgp) oluşturup TestVNet1 ve VPN ağ geçidi BGP ile yapılandırmak için. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>1. adım - TestVNet2 ve VPN ağ geçidi oluşturma

Yeni sanal ağ TestVNet2, IP adres alanının herhangi bir VNet aralıkları ile çakışmaması emin olmak önemlidir.

Bu örnekte, sanal ağlar aynı aboneliğe ait. VNet-VNet bağlantıları farklı abonelikler arasında ayarlayabilirsiniz. Daha fazla bilgi için bkz: [VNet-VNet bağlantı yapılandırma](vpn-gateway-vnet-vnet-rm-ps.md). Eklediğiniz emin olun "-EnableBgp $True" BGP'yi etkinleştirmek için bir bağlantı oluşturulurken.

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
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2
```

#### <a name="3-create-the-vpn-gateway-for-testvnet2-with-bgp-parameters"></a>3. VPN ağ geçidi BGP parametrelerle TestVNet2 için oluşturma

Ağınız için oluşturacak ve gerekli bir alt ağ ve IP yapılandırmaları tanımlamak ağ geçidine ayrılacak genel IP adresi isteyin.

```powershell
$gwpip2    = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2     = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gwipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2
```

VPN ağ geçidi ile AS numarası oluşturun. Varsayılan ASN, Azure VPN ağ geçitlerinde geçersiz kılmanız gerekir. Bağlı sanal ağlar için Asn'ler BGP ve transit yönlendirme etkinleştirmek için farklı olmalıdır.

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard -Asn $VNet2ASN
```

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>2. adım - TestVNet1 ve TestVNet2 ağ geçitleri Bağlan

Bu örnekte, her iki ağ geçitleri aynı abonelikte ' dir. Bu adım aynı PowerShell oturumunda tamamlayabilirsiniz.

#### <a name="1-get-both-gateways"></a>1. Her iki ağ geçitleri Al

1 Abonelikle oturum açıp bağlandığınızdan emin olun.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
```

#### <a name="2-create-both-connections"></a>2. Her iki bağlantı oluşturma

Bu adımda, TestVNet2 TestVNet1 arasında bağlantı ve bağlantı TestVNet2 testvnet1-oluşturun.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> BGP'yi etkinleştirmek için her iki bağlantı emin olun.
> 
> 

Bu adımları tamamladıktan sonra birkaç dakika sonra bağlantı kurulur. VNet-VNet bağlantı tamamlandığında BGP eşdeğer oturumu çalışıyor.

Bu alıştırmada, tüm üç bölümden tamamladıysa, aşağıdaki ağ topolojisi kurduğunuz:

![BGP VNet-VNet için](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## <a name="next-steps"></a>Sonraki adımlar

Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Adımlar için bkz. [Sanal Makine Oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
