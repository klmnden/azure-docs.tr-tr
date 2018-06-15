---
title: 'S2S VPN veya VNet-VNet bağlantıları için IPSec/IKE ilkesi yapılandırın: Azure Resource Manager: PowerShell | Microsoft Docs'
description: Azure Resource Manager ve PowerShell kullanarak Azure VPN Gateway'ler ile IPSec/IKE İlkesi S2S veya VNet-VNet bağlantıları için yapılandırın.
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
ms.date: 02/14/2018
ms.author: yushwang
ms.openlocfilehash: fa1aed76f63e500a6c2849fb9b62a918e85c9fb0
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
ms.locfileid: "31601160"
---
# <a name="configure-ipsecike-policy-for-s2s-vpn-or-vnet-to-vnet-connections"></a>S2S VPN veya VNet-VNet bağlantıları için IPSec/IKE ilkesi yapılandırma

Bu makalede, Resource Manager dağıtım modeli ve PowerShell kullanarak siteden siteye VPN veya VNet-VNet bağlantıları için IPSec/IKE İlkesi yapılandırmak için adım adım anlatılmaktadır.

## <a name="about"></a>IPSec ve IKE İlkesi parametreleri hakkında Azure VPN ağ geçitleri için
IPSec ve IKE protokolü standart çeşitli bileşimlerde çok çeşitli şifreleme algoritmalarını destekler. Başvurmak [şifreleme gereksinimleri ve Azure VPN ağ geçitleri hakkında](vpn-gateway-about-compliance-crypto.md) nasıl bu sağlamaya yardımcı olabileceğini görmek için şirket içi ve VNet-VNet bağlantısı uyumluluk ve güvenlik gereksinimlerinizi karşılayan.

Bu makalede oluşturmak ve bir IPSec/IKE ilkesi yapılandırma ve yeni veya var olan bağlantı uygulamak için yönergeler sağlar:

* [Bölüm 1 - oluşturmak ve IPSec/IKE İlkesi ayarlamak için iş akışı](#workflow)
* [Desteklenen bölüm 2 - şifreleme algoritmaları ve anahtar gücü](#params)
* [Bölüm 3 - IPSec/IKE İlkesi ile yeni bir S2S VPN bağlantısı oluşturma](#crossprem)
* [4 Kısım - yeni bir VNet-VNet bağlantı IPSec/IKE ilkesiyle oluşturun.](#vnet2vnet)
* [Bölüm 5 - yönetme (oluşturma, ekleme, kaldırma) bir bağlantı için IPSec/IKE İlkesi](#managepolicy)

> [!IMPORTANT]
> 1. IPSec/IKE ilkesi yalnızca aşağıdaki ağ geçidi SKU'ları üzerinde çalıştığını unutmayın:
>    * ***VpnGw1, VpnGw2, VpnGw3*** (rota tabanlı)
>    * ***Standart*** ve ***HighPerformance*** (rota tabanlı)
> 2. Belirli bir bağlantı için yalnızca ***bir*** ilke birleşimi belirtebilirsiniz.
> 3. IKE (ana mod) ve IPSec (hızlı mod) için tüm algoritmalar ve parametreleri belirtmeniz gerekir. Kısmi ilke belirtimine izin verilmez.
> 4. İlke şirket içi VPN cihazlarınızı üzerinde desteklenen emin olmak için VPN cihaz Satıcı belirtimleri başvurun. İlkeleri uyumlu olup olmadığını S2S veya VNet-VNet bağlantı kurulamıyor.

## <a name ="workflow"></a>Bölüm 1 - oluşturmak ve IPSec/IKE İlkesi ayarlamak için iş akışı
Bu bölümde oluşturmak ve bir S2S VPN veya VNet-VNet bağlantısı IPSec/IKE İlkesi güncelleştirmek için iş akışı özetlenmektedir:
1. Sanal ağ ve VPN ağ geçidi oluşturma
2. Bir şirket içi bağlantı veya başka bir sanal ağ çapraz yerel ağ geçidi için ve VNet-VNet bağlantısı için ağ geçidi oluşturun
3. Seçilen algoritmalar ve parametrelerine sahip bir IPSec/IKE ilkesi oluşturun
4. Bir bağlantı (IPSec veya VNet2VNet) ile IPSec/IKE ilkesi oluşturma
5. Ekleme/güncelleştirme/kaldırma mevcut bir bağlantı için bir IPSec/IKE İlkesi

Bu makaledeki yönergeleri ayarlayın ve aşağıdaki çizimde gösterildiği gibi IPSec/IKE ilkelerini yapılandırmanıza yardımcı olur:

![IPSec IKE İlkesi](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)

## <a name ="params"></a>Desteklenen bölüm 2 - şifreleme algoritmaları ve anahtar gücü

Aşağıdaki tabloda, desteklenen şifreleme algoritmaları ve anahtar gücü yapılandırılabilir ve müşterilerin listelenmektedir:

| **IPsec/IKEv2**  | **Seçenekler**    |
| ---  | --- 
| IKEv2 Şifrelemesi | AES256, AES192, AES128, DES3, DES  
| IKEv2 Bütünlüğü  | SHA384, SHA256, SHA1, MD5  |
| DH Grubu         | DHGroup24, ECP384, ECP256, DHGroup14, DHGroup2048, DHGroup2, DHGroup1, yok |
| IPsec Şifrelemesi | GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, None    |
| IPsec Bütünlüğü  | GCMASE256, GCMAES192, GCMAES128, SHA256, SHA1, MD5 |
| PFS Grubu        | PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, Hiçbiri 
| QM SA Yaşam Süresi   | (**İsteğe bağlı**: varsayılan değerleri kullanılan belirtilmediğinde)<br>Saniye (tamsayı; **en az 300**/varsayılan 27000 saniye)<br>Kilobayt (tamsayı; **en az 1024**/varsayılan 102400000 kilobayt)   |
| Trafik Seçicisi | UsePolicyBasedTrafficSelectors ** ($True/$False; **İsteğe bağlı**, varsayılan $False belirtilmediğinde)    |
|  |  |

> [!IMPORTANT]
> 1. **Şirket içi VPN cihazınızın yapılandırması eşleşmiyor veya aşağıdaki algoritmaları ve Azure IPSec/IKE ilkesindeki belirttiğiniz parametreleri içerir:**
>    * IKE şifreleme algoritması (ana mod / Aşama 1)
>    * IKE bütünlük algoritması (ana mod / Aşama 1)
>    * DH grubu (ana mod / Aşama 1)
>    * IPSec şifreleme algoritması (hızlı mod / Aşama 2)
>    * IPSec bütünlük algoritması (hızlı mod / Aşama 2)
>    * PFS grubu (hızlı mod / Aşama 2)
>    * Seçici (UsePolicyBasedTrafficSelectors kullanılıyorsa) trafiği
>    * SA yaşam süreleri yalnızca yerel belirtimlerdir ve bunların eşleşmesi gerekmez.
>
> 2. **GCMAES IPSec şifreleme algoritması için kullanılırsa, IPSec bütünlüğü için aynı GCMAES algoritması ve anahtar uzunluğu seçmeniz gerekir; Örneğin, GCMAES128 her ikisi için kullanma**
> 3. Yukarıdaki tabloda:
>    * Ikev2, ana mod veya Aşama 1'e karşılık gelen
>    * Hızlı mod veya Aşama 2 IPSec karşılık gelen
>    * DH grubu ana mod veya Aşama 1 kullanılan Diffie-Hellmen grubu belirtir
>    * Hızlı mod veya Aşama 2 kullanılan Diffie-Hellmen grubu PFS grubu belirtilen
> 4. Azure VPN ağ geçitlerinde IKEv2 Ana Modu SA yaşam süresi 28.800 saniye olarak sabitlenmiştir
> 5. "UsePolicyBasedTrafficSelectors" bir bağlantıda $True olarak ayarlanması, Azure VPN ağ geçidi şirket içi VPN Güvenlik Duvarı ilke tabanlı bağlanmak için yapılandırın. PolicyBasedTrafficSelectors etkinleştirirseniz, VPN cihazınız yerine (yerel ağ geçidi) önekleri denetleyicisinden Azure sanal ağı önekleri ağ şirket içi tüm bileşimleri tanımlanan eşleşen trafik Seçici sahip olduğundan emin olun gerekir any herhangi. Örneğin, şirket içi ağınızın ön ekleri 10.1.0.0/16 ve 10.2.0.0/16; sanal ağınızın ön ekleriyse 192.168.0.0/16 ve 172.16.0.0/16 şeklindeyse, aşağıdaki trafik seçicileri belirtmeniz gerekir:
>    * 10.1.0.0/16 <====> 192.168.0.0/16
>    * 10.1.0.0/16 <====> 172.16.0.0/16
>    * 10.2.0.0/16 <====> 192.168.0.0/16
>    * 10.2.0.0/16 <====> 172.16.0.0/16

İlke tabanlı trafik Seçici ile ilgili daha fazla bilgi için bkz: [birden çok şirket içi ilke tabanlı VPN aygıtlarını bağlamak](vpn-gateway-connect-multiple-policybased-rm-ps.md).

Aşağıdaki tabloda özel ilke tarafından desteklenen karşılık gelen Diffie-Hellman grupları listeler:

| **Diffie-Hellman Grubu**  | **DHGroup**              | **PFSGroup** | **Anahtar uzunluğu** |
| --- | --- | --- | --- |
| 1                         | DHGroup1                 | PFS1         | 768 bit MODP   |
| 2                         | DHGroup2                 | PFS2         | 1024 bit MODP  |
| 14                        | DHGroup14<br>DHGroup2048 | PFS2048      | 2048 bit MODP  |
| 19                        | ECP256                   | ECP256       | 256 bit ECP    |
| 20                        | ECP384                   | ECP284       | 384 bit ECP    |
| 24                        | DHGroup24                | PFS24        | 2048 bit MODP  |

Diğer ayrıntılar için [RFC3526](https://tools.ietf.org/html/rfc3526)ve [RFC5114](https://tools.ietf.org/html/rfc5114)’e bakın.

## <a name ="crossprem"></a>Bölüm 3 - IPSec/IKE İlkesi ile yeni bir S2S VPN bağlantısı oluşturma

Bu bölümde, bir IPSec/IKE ilkesiyle S2S VPN bağlantısı oluşturmak adım adım anlatılmaktadır. Aşağıdaki adımları bağlantı diyagramda gösterildiği gibi oluşturun:

![s2s İlkesi](./media/vpn-gateway-ipsecikepolicy-rm-powershell/s2spolicy.png)

Bkz: [S2S VPN bağlantısı oluşturmak](vpn-gateway-create-site-to-site-rm-powershell.md) daha ayrıntılı bir S2S VPN bağlantısı oluşturmak için adım adım yönergeler için.

### <a name="before"></a>Başlamadan önce

* Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Azure Resource Manager PowerShell cmdlet'lerini yükleyin. Bkz: [Azure PowerShell genel bakış](/powershell/azure/overview) PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi.

### <a name="createvnet1"></a>1. adım - sanal ağ, VPN ağ geçidi ve yerel ağ geçidi oluşturma

#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirme

Bu alıştırmada, değişkenlerimizi bildirerek başlayın. Üretim için yapılandırma sırasında bu değerleri kendi değerlerinizle değiştirdiğinizden emin olun.

```powershell
$Sub1          = "<YourSubscriptionName>"
$RG1           = "TestPolicyRG1"
$Location1     = "East US 2"
$VNetName1     = "TestVNet1"
$FESubName1    = "FrontEnd"
$BESubName1    = "Backend"
$GWSubName1    = "GatewaySubnet"
$VNetPrefix11  = "10.11.0.0/16"
$VNetPrefix12  = "10.12.0.0/16"
$FESubPrefix1  = "10.11.0.0/24"
$BESubPrefix1  = "10.12.0.0/24"
$GWSubPrefix1  = "10.12.255.0/27"
$DNS1          = "8.8.8.8"
$GWName1       = "VNet1GW"
$GW1IPName1    = "VNet1GWIP1"
$GW1IPconf1    = "gw1ipconf1"
$Connection16  = "VNet1toSite6"

$LNGName6      = "Site6"
$LNGPrefix61   = "10.61.0.0/16"
$LNGPrefix62   = "10.62.0.0/16"
$LNGIP6        = "131.107.72.22"
```

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. Aboneliğinize bağlanmak ve yeni bir kaynak grubu oluştur

Resource Manager cmdlet’lerini kullanmak için PowerShell moduna geçtiğinizden emin olun. Daha fazla bilgi için [Windows PowerShell’i Resource Manager ile kullanma](../powershell-azure-resource-manager.md) konusuna bakın.

PowerShell konsolunuzu açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

```powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a>3. Sanal ağ, VPN ağ geçidi ve yerel ağ geçidi oluşturma

Aşağıdaki örnek, sanal ağ, üç alt ağ ile TestVNet1 ve VPN ağ geçidi oluşturur. Kendi değerlerinizi yerleştirirken ağ geçidi alt ağınızı özellikle GatewaySubnet olarak adlandırmanız önem taşır. Başka bir ad kullanırsanız ağ geçidi oluşturma işleminiz başarısız olur.

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

$gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1

New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1

New-AzureRmLocalNetworkGateway -Name $LNGName6 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP6 -AddressPrefix $LNGPrefix61,$LNGPrefix62
```

### <a name="s2sconnection"></a>2. adım - bir IPSec/IKE İlkesi ile bir S2S VPN bağlantısı oluşturun

#### <a name="1-create-an-ipsecike-policy"></a>1. Bir IPSec/IKE ilkesi oluşturun

Aşağıdaki örnek komut dosyası için aşağıdaki algoritmaları ve parametrelerine sahip bir IPSec/IKE ilkesi oluşturur:

* Ikev2: AES256, SHA384 DHGroup24
* IPSec: AES256, SHA256, PFS hiçbiri, SA ömrü 14400 saniye ve 102400000KB

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup None -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000
```

IPSec için GCMAES kullanırsanız, IPSec şifreleme ve bütünlük için aynı GCMAES algoritması ve anahtar uzunluğu kullanmalısınız. Örneğin yukarıdaki ilgili parametreleri olacaktır "-IpsecEncryption GCMAES256 - IpsecIntegrity GCMAES256" GCMAES256 kullanırken.

#### <a name="2-create-the-s2s-vpn-connection-with-the-ipsecike-policy"></a>2. IPSec/IKE ilkesiyle S2S VPN bağlantısı oluşturun

S2S VPN bağlantısı oluşturun ve daha önce oluşturduğunuz IPsec/IKE İlkesi uygulayın.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng6 = Get-AzureRmLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
```

İsteğe bağlı olarak ekleyebileceğiniz "-UsePolicyBasedTrafficSelectors $True" yukarıda açıklandığı gibi şirket içi, ilke tabanlı VPN cihazları bağlanmak Azure VPN ağ geçidi etkinleştirmek için create bağlantı cmdlet'e.

> [!IMPORTANT]
> Bir IPSec/IKE İlkesi bir bağlantıda belirlendikten sonra Azure VPN ağ geçidi yalnızca göndermek veya belirtilen şifreleme algoritmaları ve anahtar gücü belirli bu bağlantıda ile IPSec/IKE teklifi kabul edin. Şirket içi VPN aygıtınızın bağlantı kullanır veya aksi halde S2S VPN tüneli değil kuracak tam İlkesi birleşimi kabul emin olun.


## <a name ="vnet2vnet"></a>4 Kısım - yeni bir VNet-VNet bağlantı IPSec/IKE ilkesiyle oluşturun.

Bir IPSec/IKE İlkesi ile VNet-VNet bağlantı oluşturma adımları S2S VPN bağlantısı için benzer. Aşağıdaki örnek betikler bağlantı diyagramda gösterildiği gibi oluşturun:

![v2v İlkesi](./media/vpn-gateway-ipsecikepolicy-rm-powershell/v2vpolicy.png)

Bkz: [bir VNet-VNet bağlantısı oluşturma](vpn-gateway-vnet-vnet-rm-ps.md) daha ayrıntılı bir VNet-VNet bağlantı oluşturma adımları için. Tamamlamanız gereken [bölümü 3](#crossprem) oluşturup TestVNet1 ve VPN ağ geçidi yapılandırmak için.

### <a name="createvnet2"></a>1. adım - ikinci sanal ağ ve VPN ağ geçidi oluşturma

#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirme

Değerleri, yapılandırma için kullanmak istediğiniz değerlerle değiştirdiğinizden emin olun.

```powershell
$RG2          = "TestPolicyRG2"
$Location2    = "East US 2"
$VNetName2    = "TestVNet2"
$FESubName2   = "FrontEnd"
$BESubName2   = "Backend"
$GWSubName2   = "GatewaySubnet"
$VNetPrefix21 = "10.21.0.0/16"
$VNetPrefix22 = "10.22.0.0/16"
$FESubPrefix2 = "10.21.0.0/24"
$BESubPrefix2 = "10.22.0.0/24"
$GWSubPrefix2 = "10.22.255.0/27"
$DNS2         = "8.8.8.8"
$GWName2      = "VNet2GW"
$GW2IPName1   = "VNet2GWIP1"
$GW2IPconf1   = "gw2ipconf1"
$Connection21 = "VNet2toVNet1"
$Connection12 = "VNet1toVNet2"
```

#### <a name="2-create-the-second-virtual-network-and-vpn-gateway-in-the-new-resource-group"></a>2. İkinci sanal ağ ve VPN ağ geçidi yeni kaynak grubu oluştur

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

$gw2pip1    = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
$vnet2      = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1

New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance
```

### <a name="step-2---create-a-vnet-tovnet-connection-with-the-ipsecike-policy"></a>2. adım - VNet toVNet bağlantı IPSec/IKE ilkesiyle oluşturun

S2S VPN bağlantısı için benzer bir IPSec/IKE ilkesi oluşturun sonra yeni bir bağlantı ilkesi uygulanır.

#### <a name="1-create-an-ipsecike-policy"></a>1. Bir IPSec/IKE ilkesi oluşturun

Aşağıdaki örnek komut dosyası için aşağıdaki algoritmaları ve parametrelerine sahip başka bir IPSec/IKE ilke oluşturur:
* Ikev2: AES128, SHA1, DHGroup14
* IPSec: GCMAES128, GCMAES128, PFS14, SA ömrü 14400 saniye & 102400000KB

```powershell
$ipsecpolicy2 = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption GCMAES128 -IpsecIntegrity GCMAES128 -PfsGroup PFS14 -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000
```

#### <a name="2-create-vnet-to-vnet-connections-with-the-ipsecike-policy"></a>2. VNet-VNet bağlantıları ile IPSec/IKE ilkesi oluşturma

VNet-VNet bağlantı oluşturun ve oluşturduğunuz IPsec/IKE İlkesi uygulayın. Bu örnekte, her iki ağ geçitleri aynı abonelikte ' dir. Bu nedenle oluşturmak ve her iki bağlantı aynı IPSec/IKE İlkesi ile aynı PowerShell oturumunda yapılandırmak da mümkündür.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2  -ResourceGroupName $RG2

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'
```

> [!IMPORTANT]
> Bir IPSec/IKE İlkesi bir bağlantıda belirlendikten sonra Azure VPN ağ geçidi yalnızca göndermek veya belirtilen şifreleme algoritmaları ve anahtar gücü belirli bu bağlantıda ile IPSec/IKE teklifi kabul edin. Her iki bağlantıları için IPSec ilkeleri aksi VNet-VNet bağlantısı olmayan kuracak aynı olduğundan emin olun.

Bu adımları tamamladıktan sonra birkaç dakika içerisinde bağlantı kurulur ve ' den itibaren gösterildiği gibi aşağıdaki ağ topolojisi gerekir:

![IPSec IKE İlkesi](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)


## <a name ="managepolicy"></a>Bölüm 5 - bir bağlantı için güncelleştirme IPSec/IKE İlkesi

Son Kısım varolan S2S veya VNet-VNet bağlantı IPSec/IKE ilkesini yönetmek nasıl gösterir. Aşağıdaki alıştırmada bir bağlantı üzerinde aşağıdaki işlemleri size yol göstermektedir:

1. Bir bağlantı IPSec/IKE İlkesi Göster
2. Ekleme veya bağlantı IPSec/IKE ilkesini güncelleştirin
3. Öğesinden bir bağlantı IPSec/IKE İlkesi Kaldır

Aynı adımlar S2S ve VNet-VNet bağlantıları için geçerlidir.

> [!IMPORTANT]
> IPSec/IKE İlkesi desteklenir *standart* ve *HighPerformance* rota tabanlı VPN ağ geçitleri yalnızca. Temel ağ geçidi SKU veya ilke tabanlı VPN ağ geçidi çalışmaz.

#### <a name="1-show-the-ipsecike-policy-of-a-connection"></a>1. Bir bağlantı IPSec/IKE İlkesi Göster

Aşağıdaki örnek, bir bağlantıda yapılandırılmış IPSec/IKE ilkeyi almak üzere gösterilmiştir. Komut dosyalarını da yukarıdaki alıştırmaları devam edin.

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

Varsa son komut bağlantısında yapılandırılmış geçerli IPSec/IKE İlkesi listeler. Bağlantı için örnek bir çıktı verilmiştir:

```powershell
SALifeTimeSeconds   : 14400
SADataSizeKilobytes : 102400000
IpsecEncryption     : AES256
IpsecIntegrity      : SHA256
IkeEncryption       : AES256
IkeIntegrity        : SHA384
DhGroup             : DHGroup24
PfsGroup            : PFS24
```

Varsa hiçbir IPSec/IKE İlkesi yapılandırılmış, komut (PS > $connection6.policy) boş bir dönüş alır. IPSec/IKE bağlantısında yapılandırılmamış, ancak hiçbir özel IPSec/IKE ilke olduğu anlamına gelmez. Gerçek bağlantı şirket içi VPN aygıtınızın ve Azure VPN ağ geçidi arasında anlaşılan varsayılan ilkesi kullanır.

#### <a name="2-add-or-update-an-ipsecike-policy-for-a-connection"></a>2. Eklemek veya bir bağlantı için bir IPSec/IKE ilkesini güncelleştirin

Yeni bir ilke ekleme veya var olan bir ilkenin bir bağlantısı güncelleştirmek için aynı adımlardır: yeni bir ilke oluşturun ardından bağlantı için yeni ilke uygulama.

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$newpolicy6   = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup None -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6
```

Şirket içi ilke tabanlı VPN cihazına bağlanırken "UsePolicyBasedTrafficSelectors" etkinleştirmek için add "-UsePolicyBaseTrafficSelectors" cmdlet'ine parametresini ya da devre dışı bırakma seçeneği $False olarak ayarlayın:

```powershell
Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6 -UsePolicyBasedTrafficSelectors $True
```

İlke güncelleştirildiğini yeniden denetlemek için bağlantıyı elde edebilirsiniz.

```powershell
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

Aşağıdaki örnekte gösterildiği gibi son satırından çıktı görmeniz gerekir:

```powershell
SALifeTimeSeconds   : 14400
SADataSizeKilobytes : 102400000
IpsecEncryption     : AES256
IpsecIntegrity      : SHA256
IkeEncryption       : AES128
IkeIntegrity        : SHA1
DhGroup             : DHGroup14
PfsGroup            : None
```

#### <a name="3-remove-an-ipsecike-policy-from-a-connection"></a>3. Bir IPSec/IKE İlkesi bir bağlantısından Kaldır

Öğesinden bir bağlantı özel ilke kaldırdıktan sonra Azure VPN ağ geçidi geri döner [IPSec/IKE teklifleri varsayılan listesini](vpn-gateway-about-vpn-devices.md) ve yeniden şirket içi VPN Cihazınızı yeniden anlaşmaya varılır.

```powershell
$RG1           = "TestPolicyRG1"
$Connection16  = "VNet1toSite6"
$connection6   = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$currentpolicy = $connection6.IpsecPolicies[0]
$connection6.IpsecPolicies.Remove($currentpolicy)

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6
```

İlke bağlantısından kaldırdığını olmadığını denetlemek için aynı komut dosyasını kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [birden çok şirket içi ilke tabanlı VPN aygıtlarını bağlamak](vpn-gateway-connect-multiple-policybased-rm-ps.md) ilke tabanlı trafik Seçici ile ilgili daha fazla ayrıntı için.

Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Adımlar için bkz. [Sanal Makine Oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
