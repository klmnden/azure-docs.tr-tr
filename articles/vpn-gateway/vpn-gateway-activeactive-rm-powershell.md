---
title: 'VPN ağ geçitleri için etkin-etkin S2S VPN bağlantılarını yapılandırma: Azure Resource Manager: PowerShell | Microsoft Docs'
description: Bu makalede Azure Resource Manager ve PowerShell kullanarak Azure VPN Gateway'ler ile etkin-etkin bağlantıları nasıl yapılandıracağınız anlatılmaktadır.
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: ''
tags: azure-resource-manager
ms.assetid: 238cd9b3-f1ce-4341-b18e-7390935604fa
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/24/2018
ms.author: yushwang
ms.openlocfilehash: c09abe97d34b7220d76481a403165f1b7e07fcaa
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="configure-active-active-s2s-vpn-connections-with-azure-vpn-gateways"></a>Azure VPN ağ geçitleri ile etkin-etkin S2S VPN bağlantılarını yapılandırma

Bu makalede etkin-etkin şirket içi ve Resource Manager dağıtım modeli ve PowerShell kullanarak VNet-VNet bağlantıları oluşturmak için adımlarda size yol gösterir.

## <a name="about-highly-available-cross-premises-connections"></a>Yüksek oranda kullanılabilir şirket içi bağlantılar hakkında
Şirket içi ve VNet-VNet bağlantısı için yüksek kullanılabilirlik elde etmek için birden çok VPN ağ geçidi dağıtmak ve birden çok paralel ağlarınız ve Azure arasında bağlantı kurabilir. Bkz: [yüksek oranda kullanılabilir şirketler arası ve VNet-VNet bağlantısı](vpn-gateway-highlyavailable.md) bağlantı seçenekleri ve topolojisini genel bakış.

Bu makalede bir aktif-aktif şirketler arası VPN bağlantısı ve iki sanal ağlar arasında etkin-etkin bağlantı kurmak için yönergeler sağlar.

* [Bölüm 1 - oluşturun ve Azure VPN ağ geçidinizi etkin-etkin modunda yapılandırın](#aagateway)
* [Bölüm 2 - etkin-etkin şirketler arası bağlantı](#aacrossprem)
* [Bölüm 3 - etkin-etkin VNet-VNet bağlantıları kurmak](#aav2v)

Bir VPN ağ geçidi zaten varsa, şunları yapabilirsiniz:
* [Varolan bir VPN ağ geçidi'nden etkin bekleme etkin-etkin veya tersi güncelleştir](#aaupdate)

Bunlar birlikte ihtiyaçlarınıza uygun bir yüksek oranda kullanılabilir, daha karmaşık ağ topolojisini oluşturmak için birleştirebilirsiniz.

> [!IMPORTANT]
> Etkin-etkin modu yalnızca aşağıdaki SKU kullanır: 
  * VpnGw1, VpnGw2, VpnGw3
  * HighPerformance (için eski eski SKU)
> 
> 

## <a name ="aagateway"></a>Bölüm 1 - oluşturma ve etkin-etkin VPN ağ geçitlerini yapılandırma
Aşağıdaki adımlarda Azure VPN ağ geçidinizi etkin-etkin modlarında yapılandırır. Etkin-etkin ve etkin bekleme ağ geçitleri arasındaki temel farklılıklar:

* İki ortak IP adresi ile iki ağ geçidi IP yapılandırması oluşturmanız gerekir
* EnableActiveActiveFeature bayrağı ayarlı
* Ağ geçidi SKU'su VpnGw1, VpnGw2, VpnGw3 veya HighPerformance (eski SKU) olmalıdır.

Diğer özellikler etkin-etkin olmayan ağ geçitleri ile aynıdır. 

### <a name="before-you-begin"></a>Başlamadan önce
* Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Azure Resource Manager PowerShell cmdlet’lerini yüklemelisiniz. Bkz: [Azure PowerShell genel bakış](/powershell/azure/overview) PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi.

### <a name="step-1---create-and-configure-vnet1"></a>1. adım - oluşturun ve VNet1 yapılandırın
#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirme
Bu alıştırmaya değişkenlerimizi bildirerek başlayacağız. Aşağıdaki örnekte bu alıştırmada kullanılan değişkenler bildirilmektedir. Üretim için yapılandırma sırasında bu değerleri kendi değerlerinizle değiştirdiğinizden emin olun. Bu tür yapılandırmaları tanımaya başlamak için adımları gözden geçiriyorsanız bu değişkenleri kullanabilirsiniz. Değişkenleri değiştirin, daha sonra kopyalayın ve PowerShell konsolunuza yapıştırın.

```powershell
$Sub1 = "Ross"
$RG1 = "TestAARG1"
$Location1 = "West US"
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
$GW1IPName1 = "VNet1GWIP1"
$GW1IPName2 = "VNet1GWIP2"
$GW1IPconf1 = "gw1ipconf1"
$GW1IPconf2 = "gw1ipconf2"
$Connection12 = "VNet1toVNet2"
$Connection151 = "VNet1toSite5_1"
$Connection152 = "VNet1toSite5_2"
```

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. Aboneliğinize bağlanmak ve yeni bir kaynak grubu oluştur
Resource Manager cmdlet’lerini kullanmak için PowerShell moduna geçtiğinizden emin olun. Daha fazla bilgi için [Windows PowerShell’i Resource Manager ile kullanma](../powershell-azure-resource-manager.md) konusuna bakın.

PowerShell konsolunuzu açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

```powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a>3. TestVNet1 oluşturma
Aşağıdaki örnekte TestVNet1 adlı bir sanal ağ ve üç alt ağ oluşturulmaktadır. Biri GatewaySubnet, biri FrontEnd, diğeri BackEnd’dir. Kendi değerlerinizi yerleştirirken ağ geçidi alt ağınızı özellikle GatewaySubnet olarak adlandırmanız önem taşır. Başka bir ad kullanırsanız ağ geçidi oluşturma işleminiz başarısız olur.

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-active-active-mode"></a>2. adım - etkin-etkin moduyla TestVNet1 için VPN ağ geçidi oluşturma
#### <a name="1-create-the-public-ip-addresses-and-gateway-ip-configurations"></a>1. Ortak IP adresleri ve ağ geçidi IP yapılandırmaları oluşturma
Ağınız için oluşturacağınız ağ geçidine ayrılacak iki ortak IP adresi isteyin. Ayrıca alt ağ ve gerekli IP yapılandırmaları tanımlamak.

```powershell
$gw1pip1 = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$gw1pip2 = New-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
$gw1ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2
```

#### <a name="2-create-the-vpn-gateway-with-active-active-configuration"></a>2. Aktif-Aktif yapılandırma ile VPN ağ geçidi oluşturma
TestVNet1 için sanal ağ geçidini oluşturun. İki GatewayIpConfig giriş ve EnableActiveActiveFeature bayrağı ayarlı unutmayın. Bir ağ geçidini oluşturmak biraz zaman alabilir (tamamlanması 45 dakika ya da daha fazla sürer).

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet1ASN -EnableActiveActiveFeature -Debug
```

#### <a name="3-obtain-the-gateway-public-ip-addresses-and-the-bgp-peer-ip-address"></a>3. Ağ geçidi genel IP adresleri ve BGP eş IP adresini alın
Ağ geçidi oluşturulduktan sonra Azure VPN Gateway BGP eşdeğer IP adresi edinmeniz gerekir. Bu adres için şirket içi VPN cihazlarınızı BGP eşi olarak Azure VPN ağ geçidi yapılandırmak için gereklidir.

```powershell
$gw1pip1 = Get-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
$gw1pip2 = Get-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
```

VPN ağ geçidinizi ve ilgili BGP eş IP adreslerini her ağ geçidi örneği için ayrılan iki ortak IP adresleri göstermek için aşağıdaki cmdlet'leri kullanın:

```powershell
PS D:\> $gw1pip1.IpAddress
40.112.190.5

PS D:\> $gw1pip2.IpAddress
138.91.156.129

PS D:\> $vnet1gw.BgpSettingsText
{
  "Asn": 65010,
  "BgpPeeringAddress": "10.12.255.4,10.12.255.5",
  "PeerWeight": 0
}
```

Ağ geçidi örnekleri için sırasını genel IP adresleri ve karşılık gelen BGP eşliği adresleri aynıdır. Bu örnekte, 40.112.190.5, genel IP ile VM ağ geçidi BGP eşliği adresini 10.12.255.4 kullanır ve ağ geçidi 138.91.156.129 ile 10.12.255.5 kullanır. Etkin-etkin ağ geçidine bağlanma üzerinde şirket içi VPN cihazlarınızı ayarladığınızda, bu bilgiler gereklidir. Ağ geçidi, diyagramda tüm adresleri gösterilir:

![etkin-etkin ağ geçidi](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

Ağ geçidi oluşturulduktan sonra etkin-etkin şirket içi veya VNet-VNet bağlantısı kurmak için bu ağ geçidi'ni kullanabilirsiniz. Aşağıdaki bölümlerde alıştırma tamamlamak için adım adım.

## <a name ="aacrossprem"></a>Bölüm 2 - bir aktif-aktif şirketler arası bağlantı kuramadı
Şirketler arası bağlantı kurmak için şirket içi VPN aygıtınızın temsil etmek için bir yerel ağ geçidi ve Azure VPN ağ geçidi ile yerel ağ geçidi bağlanmak için bir bağlantı oluşturmanız gerekir. Bu örnekte, Azure VPN ağ geçidi etkin-etkin modunda değil. Sonuç olarak, olsa bile yalnızca bir VPN cihazı (yerel ağ geçidi) ve bir bağlantı kaynak şirket, hem Azure VPN ağ geçidi örnekleri ile şirket içi cihaz S2S VPN tünelleri oluşturacaktır.

Devam etmeden önce lütfen tamamladığınızdan emin olun [Kısım 1](#aagateway) Bu alıştırmada.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>1. adım - oluşturma ve yerel ağ geçidi yapılandırma
#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirme
Bu alıştırmada diyagramda gösterildiği yapılandırması oluşturmak devam eder. Değerleri, yapılandırma için kullanmak istediğiniz değerlerle değiştirdiğinizden emin olun.

```powershell
$RG5 = "TestAARG5"
$Location5 = "West US"
$LNGName51 = "Site5_1"
$LNGPrefix51 = "10.52.255.253/32"
$LNGIP51 = "131.107.72.22"
$LNGASN5 = 65050
$BGPPeerIP51 = "10.52.255.253"
```

Yerel ağ ağ geçidi parametreleri ile ilgili dikkat edilecek noktalar birkaç:

* Yerel ağ geçidi, VPN ağ geçidi olarak aynı veya farklı konumunu ve kaynak grubu olabilir. Bu örnek, bunları farklı kaynak grupları, ancak aynı Azure konumunda gösterir.
* Yukarıda gösterildiği gibi yalnızca bir şirket içi VPN cihazı ise etkin-etkin bağlantısı ile veya BGP protokolüne olmadan çalışabilir. Bu örnek şirketler arası bağlantı için BGP kullanır.
* BGP etkinleştirilirse, yerel ağ geçidi için bildirmeniz gerekir önek BGP eşdeğer IP adresinizin, VPN cihazınızdaki konak adresidir. Bu durumda, bir /32 olan "10.52.255.253/32" öneki.
* Bir anımsatıcı farklı BGP Asn'ler şirket içi ağlarınız ve Azure VNet arasında kullanmanız gerekir. Aynı farklıysa, şirket içi VPN aygıtınızın ASN diğer BGP komşuları ile eş için zaten kullanıyorsa, VNet ASN değiştirmeniz gerekir.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. Yerel ağ geçidi için Site5 oluşturma
Devam etmeden önce lütfen 1. Abonelik’e hala bağlı olduğunuzdan emin olun. Henüz oluşturulmaz, kaynak grubu oluşturun.

```powershell
New-AzureRmResourceGroup -Name $RG5 -Location $Location5
New-AzureRmLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51
```

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>2. adım - sanal ağ geçidi ve yerel ağ geçidi Bağlan
#### <a name="1-get-the-two-gateways"></a>1. İki ağ geçidi Al

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw1 = Get-AzureRmLocalNetworkGateway  -Name $LNGName51 -ResourceGroupName $RG5
```

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. Testvnet1-Site5 bağlantısını oluşturma
Bu adımda, bağlantı TestVNet1 için Site5_1 "EnableBGP $True olarak Ayarla" ile oluşturun.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-on-premises-vpn-device"></a>3. Şirket içi VPN cihazınız için VPN ve BGP parametreleri
Aşağıdaki örnekte bu alıştırmada, şirket içi VPN aygıtınızın üzerinde BGP yapılandırma bölüme girer parametreleri listelenir:

```
- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.253
- Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP 1  : 10.12.255.4 for tunnel to 40.112.190.5
- Azure VNet BGP IP 2  : 10.12.255.5 for tunnel to 138.91.156.129
- Static routes        : Destination 10.12.255.4/32, nexthop the VPN tunnel interface to 40.112.190.5
                         Destination 10.12.255.5/32, nexthop the VPN tunnel interface to 138.91.156.129
- eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed
```

Birkaç dakika sonra bağlantı kurulmalıdır ve BGP eşliği oturumu IPSec bağlantısı kurulduktan sonra başlar. Bu örnek, o ana kadarki aşağıda gösterildiği diyagram bunun sonucunda, yalnızca bir şirket içi VPN cihazı yapılandırmış:

![etkin etkin crossprem](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### <a name="step-3---connect-two-on-premises-vpn-devices-to-the-active-active-vpn-gateway"></a>3. adım - iki şirket içi VPN aygıtları etkin-etkin VPN ağ geçidine bağlanmak
İki VPN aygıtları aynı şirket içi ağ varsa, ikinci VPN cihazı Azure VPN ağ geçidi bağlanarak çift artıklık elde edebilirsiniz.

#### <a name="1-create-the-second-local-network-gateway-for-site5"></a>1. İkinci yerel ağ geçidi için Site5 oluşturma
Ağ geçidi IP adresi, adres öneki ve ikinci yerel ağ geçidi için BGP eşliği adresi aynı şirket içi ağ önceki yerel ağ geçidi ile çakışmaması gerekir.

```powershell
$LNGName52 = "Site5_2"
$LNGPrefix52 = "10.52.255.254/32"
$LNGIP52 = "131.107.72.23"
$BGPPeerIP52 = "10.52.255.254"
```

```powershell
New-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
```

#### <a name="2-connect-the-vnet-gateway-and-the-second-local-network-gateway"></a>2. Sanal ağ geçidi ve ikinci yerel ağ geçidi Bağlan
"EnableBGP $True olarak Ayarla" ile Site5_2 için TestVNet1 bağlantısı oluşturma

```powershell
$lng5gw2 = Get-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5
```

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-second-on-premises-vpn-device"></a>3. İkinci şirket içi VPN cihazınız için VPN ve BGP parametreleri
Benzer şekilde, listelerde parametreleri, ikinci bir VPN cihaz girer:

```
- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP 1  : 10.12.255.4 for tunnel to 40.112.190.5
- Azure VNet BGP IP 2  : 10.12.255.5 for tunnel to 138.91.156.129
- Static routes        : Destination 10.12.255.4/32, nexthop the VPN tunnel interface to 40.112.190.5
                         Destination 10.12.255.5/32, nexthop the VPN tunnel interface to 138.91.156.129
- eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed
```

Bir kez (tüneller) bağlantı kuran sonra çift yedekli VPN cihazları ve şirket içi ağınız ve Azure bağlanma tüneller gerekir:

![çift artıklık crossprem](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)

## <a name ="aav2v"></a>Bölüm 3 - bir aktif-aktif VNet-VNet bağlantı kuramadı
Bu bölüm BGP özellikli bir aktif-aktif VNet-VNet bağlantısı oluşturur. 

Aşağıdaki yönergeler, yukarıda listelenen geçmiş adımların devamı niteliğindedir. Tamamlamanız gereken [Kısım 1](#aagateway) oluşturup TestVNet1 ve VPN ağ geçidi BGP ile yapılandırmak için. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>1. adım - TestVNet2 ve VPN ağ geçidi oluşturma
Yeni sanal ağ TestVNet2, IP adres alanının herhangi bir VNet aralıkları ile çakışmaması emin olmak önemlidir.

Bu örnekte, sanal ağlar aynı aboneliğe ait. VNet-VNet bağlantıları farklı abonelikler arasında ayarlayabilirsiniz; Lütfen [VNet-VNet bağlantı yapılandırma](vpn-gateway-vnet-vnet-rm-ps.md) daha fazla ayrıntı öğrenin. Eklediğiniz emin olun "-EnableBgp $True" BGP'yi etkinleştirmek için bir bağlantı oluşturulurken.

#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirme
Değerleri, yapılandırma için kullanmak istediğiniz değerlerle değiştirdiğinizden emin olun.

```powershell
$RG2 = "TestAARG2"
$Location2 = "East US"
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
$GW2IPName1 = "VNet2GWIP1"
$GW2IPconf1 = "gw2ipconf1"
$GW2IPName2 = "VNet2GWIP2"
$GW2IPconf2 = "gw2ipconf2"
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

#### <a name="3-create-the-active-active-vpn-gateway-for-testvnet2"></a>3. Etkin-etkin VPN ağ geçidi için TestVNet2 oluşturma
Ağınız için oluşturacağınız ağ geçidine ayrılacak iki ortak IP adresi isteyin. Ayrıca alt ağ ve gerekli IP yapılandırmaları tanımlamak.

```powershell
$gw2pip1 = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
$gw2pip2 = New-AzureRmPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2 = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
$gw2ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2
```

VPN ağ geçidi AS numarası ve "EnableActiveActiveFeature" bayrağı ile oluşturun. Azure VPN gateway'lerinde ASN varsayılan kılmalıdır unutmayın. Bağlı sanal ağlar için Asn'ler BGP ve transit yönlendirme etkinleştirmek için farklı olmalıdır.

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet2ASN -EnableActiveActiveFeature
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
Bu adımda TestVNet1 bağlantısı TestVNet2 için ve bağlantı TestVNet2 testvnet1-oluşturacağınız.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> BGP'yi etkinleştirmek için her iki bağlantı emin olun.
> 
> 

VNet-VNet bağlantısı ile çift artıklık tamamlandıktan sonra olması bağlantı birkaç dakika ve BGP bu adımları tamamladıktan sonra eşleme oturumu yukarı olacaktır:

![active-active-v2v](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <a name ="aaupdate"></a>Varolan bir VPN ağ geçidini güncelleştirin

Bu bölümde, mevcut bir Azure VPN ağ geçidi etkin bekleme modundan Etkin-Etkin moduna veya tersi değiştirmek yardımcı olur.

### <a name="change-an-active-standby-gateway-to-an-active-active-gateway"></a>Etkin-etkin ağ geçidi etkin bekleme geçidine değiştirme

Aşağıdaki örnek, bir etkin-etkin ağ geçidine bir etkin bekleme ağ geçidi dönüştürür. Etkin-etkin bir etkin bekleme gateway değiştirdiğinizde, başka bir ortak IP adresi oluşturun, sonra ikinci bir ağ geçidi IP yapılandırması ekleyin.

#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirme

İçin kendi yapılandırma gerektirir, sonra bu değişkenleri bildirin ayarlarla örnekler için kullanılan aşağıdaki parametreleri değiştirin.

```powershell
$GWName = "TestVNetAA1GW"
$VNetName = "TestVNetAA1"
$RG = "TestVPNActiveActive01"
$GWIPName2 = "gwpip2"
$GWIPconf2 = "gw1ipconf2"
```

Değişkenleri bildirme sonra kopyalayın ve bu örnek için PowerShell konsolunuza yapıştırın.

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
$location = $gw.Location
```

#### <a name="2-create-the-public-ip-address-then-add-the-second-gateway-ip-configuration"></a>2. Genel IP adresi oluşturun, sonra ikinci ağ geçidi IP Yapılandırması Ekle

```powershell
$gwpip2 = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
Add-AzureRmVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2
```

#### <a name="3-enable-active-active-mode-and-update-the-gateway"></a>3. Etkin-Etkin modunu etkinleştirmek ve ağ geçidini güncelleştirin

Bu adımda, etkin-etkin modunu etkinleştirmek ve ağ geçidini güncelleştirin. Örnekte, VPN ağ geçidi şu anda eski bir standart SKU kullanıyor. Ancak, etkin-etkin standart SKU desteklemez. (Bu durumda, HighPerformance) desteklenen bir eski SKU yeniden boyutlandırmak için basitçe, kullanmak istediğiniz desteklenen eski SKU belirtin.

* Bu adımı kullanarak yeni SKU'ları birine eski bir SKU değiştirilemiyor. Yalnızca desteklenen başka bir eski SKU eski SKU'ya yeniden boyutlandırabilirsiniz. (VpnGw1 etkin-etkin için desteklenen olsa bile) Örneğin, SKU standart VpnGw1 için standart bir eski SKU ve VpnGw1 geçerli bir SKU olduğundan değiştirilemiyor. Yeniden boyutlandırma ve geçirme SKU'ları hakkında daha fazla bilgi için bkz: [ağ geçidi SKU'ları](vpn-gateway-about-vpngateways.md#gwsku).

* Örneğin VpnGw1 VpnGw3 için geçerli bir SKU yeniden boyutlandırmak isterseniz SKU'ları aynı SKU ailesi olduğundan bu adımı kullanarak bunu yapabilirsiniz. Bunu yapmak için değeri kullanırsınız: ```-GatewaySku VpnGw3```

Ağ geçidi yeniden boyutlandırmak gerekmiyorsa, bu, ortamınızda kullanırken, - GatewaySku belirtmeniz gerekmez. Bu adımda, ağ geçidi nesnesi gerçek güncelleştirmesini tetiklemek için PowerShell'de ayarlamanız gerekir, dikkat edin. Ağ geçidiniz yeniden boyutlandırma değil olsa bile bu güncelleştirme 30-45 dakika sürebilir.

```powershell
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature -GatewaySku HighPerformance
```

### <a name="change-an-active-active-gateway-to-an-active-standby-gateway"></a>Etkin-etkin ağ geçidi etkin bekleme ağ geçidi değiştirme
#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirme

İçin kendi yapılandırma gerektirir, sonra bu değişkenleri bildirin ayarlarla örnekler için kullanılan aşağıdaki parametreleri değiştirin.

```powershell
$GWName = "TestVNetAA1GW"
$RG = "TestVPNActiveActive01"
```

Değişkenleri bildirme sonra kaldırmak istediğiniz IP yapılandırmasının adını alın.

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
$ipconfname = $gw.IpConfigurations[1].Name
```

#### <a name="2-remove-the-gateway-ip-configuration-and-disable-the-active-active-mode"></a>2. Ağ geçidi IP yapılandırması kaldırın ve etkin-etkin modu devre dışı bırakma

Bu örnek, ağ geçidi IP yapılandırması kaldırmak ve etkin-etkin modu devre dışı bırakmak için kullanın. Ağ geçidi nesnesi gerçek güncelleştirmesini tetiklemek için PowerShell'de ayarlamalısınız dikkat edin.

```powershell
Remove-AzureRmVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature
```

Bu güncelleştirme, en fazla 30 45 dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Adımlar için bkz. [Sanal Makine Oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
