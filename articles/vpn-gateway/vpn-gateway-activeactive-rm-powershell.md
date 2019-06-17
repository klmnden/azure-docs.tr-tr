---
title: 'VPN ağ geçitleri için etkin-etkin S2S VPN bağlantıları yapılandırın: Azure Resource Manager: PowerShell | Microsoft Docs'
description: Bu makalede Azure Resource Manager ve PowerShell kullanarak Azure VPN Gateways ile aktif / aktif bağlantıları nasıl yapılandıracağınız açıklanmaktadır.
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: article
ms.date: 07/24/2018
ms.author: yushwang, cherylmc
ms.openlocfilehash: 7ba4fb32ddfb8b3eb88d2dbfce265b070d521414
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66119470"
---
# <a name="configure-active-active-s2s-vpn-connections-with-azure-vpn-gateways"></a>Azure VPN Gateways ile aktif / aktif S2S VPN bağlantıları yapılandırma

Bu makalede, etkin-etkin şirket içi ve Resource Manager dağıtım modeli ve PowerShell kullanarak VNet-VNet bağlantıları oluşturmak için adımları gösterilmektedir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="about-highly-available-cross-premises-connections"></a>Yüksek oranda kullanılabilir şirket içi bağlantılar hakkında
Şirketler arası ve VNet-VNet bağlantısı için yüksek kullanılabilirlik elde etmek için birden çok VPN ağ geçidi dağıtın ve ağlarınız ve Azure arasında birden çok paralel bağlantılar kurmak gerekir. Bkz: [yüksek oranda kullanılabilir şirket içi ve VNet-VNet bağlantı](vpn-gateway-highlyavailable.md) bağlantı seçenekleri ve topoloji genel bakış.

Bu makalede bir aktif-aktif şirketler arası VPN bağlantısı ve iki sanal ağ arasında etkin-etkin bağlantı ayarlamak için yönergeler sağlar.

* [1. bölüm - oluşturma ve etkin-etkin modda, Azure VPN ağ geçidi yapılandırma](#aagateway)
* [2. Kısım - etkin-etkin şirket içi bağlantı oluşturma](#aacrossprem)
* [3. Kısım - etkin-etkin VNet-VNet bağlantıları oluşturma](#aav2v)

Bir VPN ağ geçidi zaten varsa, şunları yapabilirsiniz:
* [Var olan bir VPN ağ geçidi etkin bekleme aktif-aktif veya tam tersi güncelleştir](#aaupdate)

Bunlar birlikte ihtiyaçlarınıza uygun bir daha karmaşık ve yüksek oranda kullanılabilir ağ topolojisi inşa birleştirebilirsiniz.

> [!IMPORTANT]
> Aktif / Aktif modu, yalnızca aşağıdaki SKU'ları kullanır: 
>   * VpnGw1, VpnGw2, VpnGw3
>   * Yüksek performanslı (eski eski SKU'lar için)

## <a name ="aagateway"></a>1. bölüm - oluşturma ve etkin-etkin VPN gateways yapılandırma
Aşağıdaki adımlar, Azure VPN ağ geçidi etkin-etkin modda yapılandıracaksınız. Etkin-etkin ve etkin bekleme ağ geçitleri arasındaki temel farklar:

* İki ağ geçidi IP yapılandırması ile iki genel IP adresi oluşturmanız gerekir
* EnableActiveActiveFeature bayrağı ayarlamanız gerekir
* Ağ geçidi SKU VpnGw1, VpnGw2, VpnGw3 veya HighPerformance (eski SKU) olmalıdır.

Diğer özellikleri etkin-etkin olmayan ağ geçitleri ile aynıdır. 

### <a name="before-you-begin"></a>Başlamadan önce
* Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Azure Resource Manager PowerShell cmdlet’lerini yüklemelisiniz. Bkz: [genel bakış, Azure PowerShell](/powershell/azure/overview) PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi.

### <a name="step-1---create-and-configure-vnet1"></a>1\. adım - oluşturma ve VNet1'i yapılandırma
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

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. Aboneliğinize bağlanın ve yeni bir kaynak grubu oluşturun
Resource Manager cmdlet’lerini kullanmak için PowerShell moduna geçtiğinizden emin olun. Daha fazla bilgi için [Windows PowerShell’i Resource Manager ile kullanma](../powershell-azure-resource-manager.md) konusuna bakın.

PowerShell konsolunuzu açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName $Sub1
New-AzResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a>3. TestVNet1 oluşturma
Aşağıdaki örnekte TestVNet1 adlı bir sanal ağ ve üç alt ağ oluşturulmaktadır. Biri GatewaySubnet, biri FrontEnd, diğeri BackEnd’dir. Kendi değerlerinizi yerleştirirken ağ geçidi alt ağınızı özellikle GatewaySubnet olarak adlandırmanız önem taşır. Başka bir ad kullanırsanız ağ geçidi oluşturma işleminiz başarısız olur.

```powershell
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-active-active-mode"></a>2\. adım - aktif / aktif modu ile TestVNet1 için VPN ağ geçidi oluşturun
#### <a name="1-create-the-public-ip-addresses-and-gateway-ip-configurations"></a>1. Genel IP adresleri ve ağ geçidi IP yapılandırması oluştur
Ağınız için oluşturacağınız ağ geçidine ayrılacak iki genel IP adresi isteyin. Ayrıca, alt ağ ve gerekli IP yapılandırmaları tanımlarsınız.

```powershell
$gw1pip1 = New-AzPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$gw1pip2 = New-AzPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
$gw1ipconf2 = New-AzVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2
```

#### <a name="2-create-the-vpn-gateway-with-active-active-configuration"></a>2. Etkin-etkin yapılandırma ile VPN ağ geçidi oluşturma
TestVNet1 için sanal ağ geçidini oluşturun. İki GatewayIpConfig girişi ve EnableActiveActiveFeature bayrağı ayarlı unutmayın. Bir ağ geçidini oluşturmak biraz zaman alabilir (tamamlanması 45 dakika ya da daha fazla sürer).

```powershell
New-AzVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet1ASN -EnableActiveActiveFeature -Debug
```

#### <a name="3-obtain-the-gateway-public-ip-addresses-and-the-bgp-peer-ip-address"></a>3. Ağ geçidi genel IP adresleri ve BGP eş IP adresini alma
Ağ geçidi oluşturulduktan sonra Azure VPN Gateway'deki BGP eşdeğer IP adresini almak gerekir. Bu adres, Azure VPN Gateway BGP eşi için şirket içi VPN cihazlarınızı yapılandırmak için gereklidir.

```powershell
$gw1pip1 = Get-AzPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
$gw1pip2 = Get-AzPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
```

VPN ağ geçidiniz ve ilgili BGP eş IP adreslerini her ağ geçidi örneğinin için ayrılan iki genel IP adresini göstermek için aşağıdaki cmdlet'leri kullanın:

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

Ağ geçidi örnekleri için sırasını genel IP adresleri ve karşılık gelen BGP eşlemesi aynı adresleridir. Bu örnekte, 40.112.190.5, genel ıp'li VM ağ geçidi, BGP eşlemesi adresini 10.12.255.4 kullanır ve ağ geçidi ile 138.91.156.129 10.12.255.5 kullanır. Etkin-etkin ağ geçidine bağlanma şirket içi VPN cihazlarınızı ayarladığınızda bu bilgiler gereklidir. Ağ geçidi, tüm adresleri ile diyagramda gösterilmiştir:

![etkin-etkin ağ geçidi](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

Ağ geçidi oluşturulduktan sonra etkin-etkin şirket içi veya VNet-VNet bağlantısı kurmak için bu ağ geçidi'ni kullanabilirsiniz. Aşağıdaki bölümlerde alıştırma tamamlanması adımlarını inceleyelim.

## <a name ="aacrossprem"></a>Bölüm 2 - bir aktif-aktif şirketler arası bağlantı oluşturma
Şirketler arası bağlantı kurmak için şirket içi VPN Cihazınızı temsil etmek için bir yerel ağ geçidi ve Azure VPN ağ geçidi ile yerel ağ geçidine bağlanmak için bir bağlantı oluşturmanız gerekir. Bu örnekte, Azure VPN ağ geçidi etkin-etkin modda değil. Sonuç olarak, olsa bile yalnızca bir VPN cihazı (yerel ağ geçidi) ve bir bağlantı kaynağı şirket içinde hem Azure VPN ağ geçidi örnekleri S2S VPN tüneli ile şirket içi cihaz oluşturacaktır.

Devam etmeden önce lütfen tamamladığınızdan emin olun [bölüm 1](#aagateway) Bu alıştırmada.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>1\. adım - oluşturma ve yerel ağ geçidi yapılandırma
#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirme
Bu alıştırmada, derleme yapılandırması Aşağıdaki diyagramda gösterilen devam eder. Değerleri, yapılandırma için kullanmak istediğiniz değerlerle değiştirdiğinizden emin olun.

```powershell
$RG5 = "TestAARG5"
$Location5 = "West US"
$LNGName51 = "Site5_1"
$LNGPrefix51 = "10.52.255.253/32"
$LNGIP51 = "131.107.72.22"
$LNGASN5 = 65050
$BGPPeerIP51 = "10.52.255.253"
```

Yerel ağ geçidi parametreleri ile ilgili dikkat edilecek noktalar birkaç:

* Yerel ağ geçidi, VPN ağ geçidi olarak aynı veya farklı bir konum ve kaynak grubu içinde olabilir. Bu örnek, bunları farklı kaynak gruplarında ancak aynı Azure konumunda gösterir.
* Yukarıda da gösterildiği gibi yalnızca bir şirket içi VPN cihazı ise etkin-etkin bağlantı ile veya BGP protokol olmadan çalışabilir. Bu örnek şirketler arası bağlantı için BGP kullanır.
* BGP etkinse, yerel ağ geçidi için bildirmek için gereken ön ek VPN cihazınızın BGP eşdeğer IP adresinizin konak adresidir. Bu durumda, bir özelliğini/32 olduğu "10.52.255.253/32" öneki.
* Farklı BGP Asn'ler şirket içi ağlarınız ve Azure sanal ağı arasında bir anımsatıcı kullanmanız gerekir. Aynı olmaları durumunda, şirket içi VPN cihazınız ile diğer BGP komşu eşlenecek ASN'yi zaten kullanıyorsa, VNet ASN'nizi değiştirmeniz gerekir.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. Site5 için yerel ağ geçidi oluşturma
Devam etmeden önce lütfen 1. Abonelik’e hala bağlı olduğunuzdan emin olun. Henüz oluşturulmaz, kaynak grubu oluşturun.

```powershell
New-AzResourceGroup -Name $RG5 -Location $Location5
New-AzLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51
```

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>2\. adım - sanal ağ geçidi ve yerel ağ geçidine bağlanma
#### <a name="1-get-the-two-gateways"></a>1. İki ağ geçidi Al

```powershell
$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw1 = Get-AzLocalNetworkGateway  -Name $LNGName51 -ResourceGroupName $RG5
```

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. Testvnet1-Site5 bağlantısını oluşturma
Bu adımda, bağlantı testvnet1-Site5_1 "EnableBGP $True olarak ayarlayın" ile oluşturursunuz.

```powershell
New-AzVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-on-premises-vpn-device"></a>3. Şirket içi VPN cihazınız için VPN ve BGP parametreleri
Aşağıdaki örnekte bu alıştırma için şirket içi VPN cihazınızın BGP yapılandırması bölüme girer parametreleri listeler:

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

Birkaç dakika sonra bağlantı kurulmalıdır ve BGP eşdeğer oturumu, IPSec bağlantısı kurulduktan sonra başlayacaktır. Bu örnek, aşağıdaki diyagramda bunun sonucunda, yalnızca bir şirket içi VPN cihazı şimdiye yapılandırdı:

![etkin-etkin-crossprem](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### <a name="step-3---connect-two-on-premises-vpn-devices-to-the-active-active-vpn-gateway"></a>3\. adım - etkin-etkin VPN ağ geçidi için iki şirket içi VPN cihazı bağlama
Aynı şirket içi ağ iki VPN cihazlarınız varsa, ikinci bir VPN cihazı Azure VPN gateway'e bağlanırken tarafından çift yedeklilik elde edebilirsiniz.

#### <a name="1-create-the-second-local-network-gateway-for-site5"></a>1. Site5 için ikinci bir yerel ağ geçidi oluşturma
Ağ geçidi IP adresini, adres ön eki ve ikinci yerel ağ geçidi için BGP eşliği adresi önceki yerel ağ geçidi için aynı şirket içi ağ ile çakışmaması gerekir.

```powershell
$LNGName52 = "Site5_2"
$LNGPrefix52 = "10.52.255.254/32"
$LNGIP52 = "131.107.72.23"
$BGPPeerIP52 = "10.52.255.254"
```

```powershell
New-AzLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
```

#### <a name="2-connect-the-vnet-gateway-and-the-second-local-network-gateway"></a>2. VNet ağ geçidinin ve ikinci yerel ağ geçidine bağlanma
Testvnet1-Site5_2 "EnableBGP $True olarak ayarlayın" ile bağlantı oluşturursunuz

```powershell
$lng5gw2 = Get-AzLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5
```

```powershell
New-AzVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-second-on-premises-vpn-device"></a>3. İkinci şirket içi VPN cihazınız için VPN ve BGP parametreleri
Benzer şekilde, aşağıdaki listelerde parametreleri, ikinci bir VPN cihazı girer:

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

Bağlantı (tüneller) belirlenir sonra çift yedekli VPN cihazları ve şirket içi ağınız ile Azure bağlanma tüneller sahip olursunuz:

![çift yedeklilik crossprem](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)

## <a name ="aav2v"></a>3. Kısım - bir etkin-etkin VNet-VNet bağlantısı
Bu bölümde, BGP özellikli bir etkin-etkin VNet-VNet bağlantısı oluşturur. 

Aşağıdaki yönergeler, yukarıda listelenen geçmiş adımların devamı niteliğindedir. Tamamlamanız gereken [bölüm 1](#aagateway) oluşturma ve TestVNet1 ve VPN ağ geçidi, BGP ile yapılandırın. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>1\. adım - TestVNet2 ve VPN ağ geçidi oluşturma
IP adres alanı yeni sanal ağ TestVNet2, tüm sanal ağ Aralıklarınızın çakışmadığını emin olmak önemlidir.

Bu örnekte, sanal ağlar aynı aboneliğe ait. Farklı abonelikler arasında VNet-VNet bağlantılarında ayarlayabilirsiniz; Lütfen [bir VNet-VNet bağlantısını yapılandırma](vpn-gateway-vnet-vnet-rm-ps.md) fazla daha fazla bilgi edinmek için. Eklediğinizden emin olun "-Enablebgp'yi $True" BGP'yi etkinleştirmek için bir bağlantı oluşturulurken.

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
New-AzResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2
```

#### <a name="3-create-the-active-active-vpn-gateway-for-testvnet2"></a>3. TestVNet2 için etkin-etkin VPN ağ geçidi oluşturma
Ağınız için oluşturacağınız ağ geçidine ayrılacak iki genel IP adresi isteyin. Ayrıca, alt ağ ve gerekli IP yapılandırmaları tanımlarsınız.

```powershell
$gw2pip1 = New-AzPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
$gw2pip2 = New-AzPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2 = Get-AzVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2 = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gw2ipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
$gw2ipconf2 = New-AzVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2
```

VPN ağ geçidi, AS numarası ve "EnableActiveActiveFeature" bayrağı ile oluşturun. Azure VPN ağ geçitlerinizi üzerinde ' % s'varsayılan ASN kılmalı unutmayın. Bağlı sanal ağlar için Asn'ler BGP ve geçiş yönlendirmesi'ni etkinleştirmek için farklı olmalıdır.

```powershell
New-AzVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet2ASN -EnableActiveActiveFeature
```

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>2\. adım - TestVNet1 ve TestVNet2 ağ geçitlerini bağlama
Bu örnekte, iki ağ geçidi için aynı abonelikte ' dir. Bu adım aynı PowerShell oturumunda tamamlayabilirsiniz.

#### <a name="1-get-both-gateways"></a>1. Her iki ağ geçitlerini Al
1 Abonelikle oturum açıp bağlandığınızdan emin olun.

```powershell
$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet2gw = Get-AzVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
```

#### <a name="2-create-both-connections"></a>2. Her iki bağlantı oluşturma
Bu adımda, TestVNet2 TestVNet1 arasında bağlantı ve bağlantı TestVNet2 TestVNet1'e oluşturacaksınız.

```powershell
New-AzVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> Her iki bağlantı için BGP'yi etkinleştirmek emin olun.
> 
> 

Çift yedeklilik ile VNet-VNet bağlantısı tamamlandıktan sonra olması bağlantı birkaç dakika ile BGP, bu adımları tamamladıktan sonra eşleme oturum oluşturan olacak:

![active-active-v2v](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <a name ="aaupdate"></a>Mevcut bir VPN ağ geçidini güncelleştir

Bu bölümde aktif / aktif modu veya tam tersi var olan bir Azure VPN ağ geçidi etkin bekleme değiştirmenize yardımcı olur.

### <a name="change-an-active-standby-gateway-to-an-active-active-gateway"></a>Bir etkin-etkin ağ geçidi için bir etkin bekleme ağ geçidi değiştirme

Aşağıdaki örnek, bir etkin-etkin ağ geçidine bir etkin bekleme ağ geçidi dönüştürür. Etkin-etkin bir etkin bekleme ağ geçidi değiştirdiğinizde, başka bir genel IP adresi oluşturabilir, ardından ikinci bir ağ geçidi IP yapılandırması ekleyin.

#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirme

İçin kendi yapılandırma gerektirir, sonra bu değişkenleri bildirin ayarları ile örnekleri için kullanılan aşağıdaki parametreleri değiştirin.

```powershell
$GWName = "TestVNetAA1GW"
$VNetName = "TestVNetAA1"
$RG = "TestVPNActiveActive01"
$GWIPName2 = "gwpip2"
$GWIPconf2 = "gw1ipconf2"
```

Değişkenleri bildirme sonra kopyalayın ve PowerShell konsolunuza Bu örneği yapıştırın.

```powershell
$vnet = Get-AzVirtualNetwork -Name $VNetName -ResourceGroupName $RG
$subnet = Get-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gw = Get-AzVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
$location = $gw.Location
```

#### <a name="2-create-the-public-ip-address-then-add-the-second-gateway-ip-configuration"></a>2. Genel IP adresini oluşturun ve ardından ikinci bir ağ geçidi IP Yapılandırması Ekle

```powershell
$gwpip2 = New-AzPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
Add-AzVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2
```

#### <a name="3-enable-active-active-mode-and-update-the-gateway"></a>3. Aktif / Aktif modunu etkinleştirmek ve ağ geçidi güncelleştirme

Bu adımda, aktif / aktif modunu etkinleştirmek ve ağ geçidi güncelleştirin. Örnekte, VPN ağ geçidi şu anda eski standart SKU kullanıyor. Ancak, etkin-etkin, standart SKU desteklemez. Eski SKU (Bu durumda HighPerformance) desteklenen bir yeniden boyutlandırmak için yalnızca kullanmak istediğiniz desteklenen eski SKU belirtirsiniz.

* Bu adımı kullanarak yeni SKU'lara birine eski SKU değiştirilemiyor. Yalnızca eski SKU için desteklenen başka bir eski SKU yeniden boyutlandırabilirsiniz. (VpnGw1 etkin-etkin için desteklenen olsa bile) gibi SKU standart VpnGw1 için standart bir eski SKU'tır ve geçerli SKU VpnGw1 olduğundan değiştiremezsiniz. Yeniden boyutlandırma ve taşıma SKU'ları hakkında daha fazla bilgi için bkz: [ağ geçidi SKU'ları](vpn-gateway-about-vpngateways.md#gwsku).

* Geçerli bir SKU, örneğin VpnGw1 VpnGw3 için yeniden boyutlandırmak istiyorsanız SKU'ları aynı SKU ailesi içinde olduğundan bu adımı kullanarak bunu yapabilirsiniz. Bunu yapmak için değeri kullanırsınız: ```-GatewaySku VpnGw3```

Ağ geçidini yeniden boyutlandırın gerekmiyorsa, bu, ortamınızda kullanırken, - GatewaySku belirtmeniz gerekmez. Bu adımda, ağ geçidi nesnesinin gerçek güncelleştirmesini tetiklemek için PowerShell'de ayarlamanız gerekir, dikkat edin. Ağ geçidi yeniden boyutlandırma değil olsa bile, bu güncelleştirme 30-45 dakika sürebilir.

```powershell
Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature -GatewaySku HighPerformance
```

### <a name="change-an-active-active-gateway-to-an-active-standby-gateway"></a>Bir etkin-etkin ağ geçidi etkin bekleme gateway'e değiştirme
#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirme

İçin kendi yapılandırma gerektirir, sonra bu değişkenleri bildirin ayarları ile örnekleri için kullanılan aşağıdaki parametreleri değiştirin.

```powershell
$GWName = "TestVNetAA1GW"
$RG = "TestVPNActiveActive01"
```

Değişkenleri bildirme sonra kaldırmak istediğiniz IP yapılandırmasının adını alın.

```powershell
$gw = Get-AzVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
$ipconfname = $gw.IpConfigurations[1].Name
```

#### <a name="2-remove-the-gateway-ip-configuration-and-disable-the-active-active-mode"></a>2. Ağ geçidi IP Yapılandırması'nı kaldırın ve aktif / aktif modu devre dışı bırak

Ağ geçidi IP Yapılandırması'nı kaldırın ve aktif / aktif modu devre dışı bırakmak için bu örneği kullanın. Ağ geçidi nesnesinin gerçek güncelleştirmesini tetiklemek için PowerShell'de ayarlamalısınız dikkat edin.

```powershell
Remove-AzVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature
```

Bu güncelleştirme 30 45 dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Adımlar için bkz. [Sanal Makine Oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
