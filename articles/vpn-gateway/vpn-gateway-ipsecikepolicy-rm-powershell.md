---
title: 'S2S VPN veya VNet-VNet bağlantıları için IPSec/IKE ilkesi yapılandırın: Azure Resource Manager: PowerShell | Microsoft Docs'
description: S2S veya VNet-VNet bağlantıları için IPSec/IKE İlkesi, Azure Resource Manager ve PowerShell kullanarak Azure VPN Gateways ile yapılandırın.
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
ms.openlocfilehash: d04d62d66b4ba22437e6d854566f8bbf5536a6fc
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66121102"
---
# <a name="configure-ipsecike-policy-for-s2s-vpn-or-vnet-to-vnet-connections"></a>S2S VPN veya VNet-VNet bağlantıları için IPSec/IKE ilkesi yapılandırma

Bu makalede Resource Manager dağıtım modeli ve PowerShell kullanarak siteden siteye VPN veya VNet-VNet bağlantıları için IPSec/IKE ilke yapılandırma adımlarında size kılavuzluk eder.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="about"></a>IPSec ve IKE ilke parametreleri hakkında Azure VPN ağ geçitleri
IPSec ve IKE protokolü standart çeşitli birleşimler çok çeşitli şifreleme algoritmalarını destekler. Başvurmak [şifreleme gereksinimleri ve Azure VPN ağ geçitleri hakkında](vpn-gateway-about-compliance-crypto.md) nasıl bu sağlamaya yardımcı olabileceğini görmek için şirketler arası ve VNet-VNet bağlantısı, uyumluluk veya güvenlik gereksinimleri karşılar.

Bu makale, oluşturup bir IPSec/IKE ilkesi yapılandırma ve yeni veya var olan bağlantı uygulamak için yönergeler sağlar:

* [Bölüm 1 - oluşturun ve IPSec/IKE İlkesi ayarlamak için iş akışı](#workflow)
* [Bölüm 2 - desteklenen şifreleme algoritmaları ve anahtar güçleriyle](#params)
* [3. Kısım - yeni bir S2S VPN bağlantısı IPSec/IKE ilke oluşturun](#crossprem)
* [Bölüm 4 - yeni bir VNet-VNet bağlantı IPSec/IKE İlkesi ile oluşturma](#vnet2vnet)
* [Bölüm 5 - yönetme (oluşturma, ekleme, kaldırma) için bir bağlantı IPSec/IKE İlkesi](#managepolicy)

> [!IMPORTANT]
> 1. IPSec/IKE ilkesi yalnızca aşağıdaki ağ geçidi SKU'ları üzerinde çalıştığını unutmayın:
>    * ***VpnGw1, VpnGw2, VpnGw3*** (rota tabanlı)
>    * ***Standart*** ve ***HighPerformance*** (rota tabanlı)
> 2. Belirli bir bağlantı için yalnızca ***bir*** ilke birleşimi belirtebilirsiniz.
> 3. Hem IKE (ana mod) hem de IPSec (hızlı mod) için tüm algoritmaları ve parametreleri belirtmeniz gerekir. Kısmi ilke belirtimine izin verilmez.
> 4. İlke, şirket içi VPN cihazlarınızda desteklenen emin olmak için VPN cihaz satıcısı belirtimlerine başvurun. S2S veya VNet-VNet bağlantılarında ilkelerine uyumlu olup olmadığını kurulamıyor.

## <a name ="workflow"></a>Bölüm 1 - oluşturun ve IPSec/IKE İlkesi ayarlamak için iş akışı
Bu bölümde, iş akışı oluşturma ve S2S VPN veya VNet-VNet bağlantı IPSec/IKE ilkesini güncelleştirme özetlenmektedir:
1. Sanal ağ ve VPN ağ geçidi oluşturma
2. Bir yerel ağ geçidi arası şirket içi bağlantı veya başka bir sanal ağ ve VNet-VNet bağlantısı için ağ geçidi oluşturma
3. Seçili algoritmaları ve parametrelerle IPSec/IKE ilkesi oluşturma
4. IPSec/IKE ilke (IPSec veya VNet2VNet) bir bağlantı oluşturun
5. Ekleme/güncelleştirme/kaldırma için mevcut bir bağlantıyı bir IPSec/IKE İlkesi

Bu makaledeki yönergeleri ayarlamak ve diyagramda gösterildiği gibi IPSec/IKE ilkelerini yapılandırmanıza yardımcı olur:

![IPSec IKE İlkesi](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)

## <a name ="params"></a>Bölüm 2 - desteklenen şifreleme algoritmaları ve anahtar güçleriyle

Aşağıdaki tabloda, şifreleme algoritmaları ve anahtar güçleri müşteriler tarafından listelenmektedir:

| **IPsec/IKEv2**  | **Seçenekler**    |
| ---  | --- 
| IKEv2 Şifrelemesi | AES256, AES192, AES128, DES3, DES  
| IKEv2 Bütünlüğü  | SHA384, SHA256, SHA1, MD5  |
| DH Grubu         | DHGroup24, ECP384, ECP256, DHGroup14, DHGroup2048, DHGroup2, DHGroup1, hiçbiri |
| IPsec Şifrelemesi | GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, None    |
| IPsec Bütünlüğü  | GCMASE256, GCMAES192, GCMAES128, SHA256, SHA1, MD5 |
| PFS Grubu        | PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, Hiçbiri 
| QM SA Yaşam Süresi   | (**İsteğe bağlı**: varsayılan değerleri kullanılan belirtilmediğinde)<br>Saniye (tamsayı; **en az 300**/varsayılan 27000 saniye)<br>Kilobayt (tamsayı; **en az 1024**/varsayılan 102400000 kilobayt)   |
| Trafik Seçicisi | UsePolicyBasedTrafficSelectors ** ($True/$False; **İsteğe bağlı**, varsayılan $False belirtilmediğinde)    |
|  |  |

> [!IMPORTANT]
> 1. **Şirket içi VPN cihazınızın yapılandırması ile aynı veya aşağıdaki algoritmaları ve Azure IPSec/IKE ilkesinde belirttiğiniz parametreleri içerir:**
>    * IKE şifreleme algoritması (ana mod / 1. Aşama)
>    * IKE bütünlük algoritması (ana mod / 1. Aşama)
>    * DH grubu (ana mod / Aşama 1)
>    * IPSec şifreleme algoritması (hızlı mod / 2. Aşama)
>    * IPSec bütünlük algoritması (hızlı mod / 2. Aşama)
>    * PFS grubu (hızlı mod / Aşama 2)
>    * Seçici (UsePolicyBasedTrafficSelectors kullanılıyorsa) trafiği
>    * SA yaşam süreleri yalnızca yerel belirtimlerdir ve bunların eşleşmesi gerekmez.
>
> 2. **GCMAES IPSec şifreleme algoritması için kullanılırsa, IPSec bütünlüğü için aynı GCMAES algoritmasını ve anahtar uzunluğu seçmeniz gerekir; Örneğin, her ikisi için GCMAES128 kullanma**
> 3. Yukarıdaki tabloda:
>    * Ikev2 ana modu veya 1. Aşama karşılık gelir.
>    * IPSec hızlı mod veya Aşama 2'ye karşılık gelir.
>    * DH grubu, Diffie-Hellmen ana mod veya 1. Aşama kullanılan grubun belirtir
>    * PFS grubu, Diffie-Hellmen hızlı mod veya 2. Aşama kullanılan grubun belirtilen
> 4. Azure VPN ağ geçitlerinde IKEv2 Ana Modu SA yaşam süresi 28.800 saniye olarak sabitlenmiştir
> 5. "UsePolicyBasedTrafficSelectors" bir bağlantıda $True olarak ayarlanması, Azure VPN ağ geçidi, şirket içi VPN Güvenlik Duvarı ilke tabanlı bağlanmak için yapılandırın. PolicyBasedTrafficSelectors etkinleştirirseniz, VPN cihazınız yerine (yerel ağ geçidi) ön ekleri için/Azure sanal ağ ön ekleri, ağ, şirket içi tüm birleşimlerle tanımlanmış eşleşen trafik seçicileri sahip olduğundan emin olmak gerekir herhangi bir ağdan herhangi. Örneğin, şirket içi ağınızın ön ekleri 10.1.0.0/16 ve 10.2.0.0/16; sanal ağınızın ön ekleriyse 192.168.0.0/16 ve 172.16.0.0/16 şeklindeyse, aşağıdaki trafik seçicileri belirtmeniz gerekir:
>    * 10.1.0.0/16 <====> 192.168.0.0/16
>    * 10.1.0.0/16 <====> 172.16.0.0/16
>    * 10.2.0.0/16 <====> 192.168.0.0/16
>    * 10.2.0.0/16 <====> 172.16.0.0/16

İlke tabanlı trafik seçicileri hakkında daha fazla bilgi için bkz. [birden çok şirket içi ilke tabanlı VPN cihazını bağlama](vpn-gateway-connect-multiple-policybased-rm-ps.md).

Aşağıdaki tabloda özel ilke tarafından desteklenen karşılık gelen Diffie-Hellman grupları listelenmiştir:

| **Diffie-Hellman Grubu**  | **DHGroup**              | **PFSGroup** | **Anahtar uzunluğu** |
| --- | --- | --- | --- |
| 1                         | DHGroup1                 | PFS1         | 768 bit MODP   |
| 2                         | DHGroup2                 | PFS2         | 1024 bit MODP  |
| 14                        | DHGroup14<br>DHGroup2048 | PFS2048      | 2048 bit MODP  |
| 19                        | ECP256                   | ECP256       | 256 bit ECP    |
| 20                        | ECP384                   | ECP284       | 384 bit ECP    |
| 24                        | DHGroup24                | PFS24        | 2048 bit MODP  |

Diğer ayrıntılar için [RFC3526](https://tools.ietf.org/html/rfc3526)ve [RFC5114](https://tools.ietf.org/html/rfc5114)’e bakın.

## <a name ="crossprem"></a>3. Kısım - yeni bir S2S VPN bağlantısı IPSec/IKE ilke oluşturun

Bu bölümde bir IPSec/IKE İlkesi ile bir S2S VPN bağlantısı oluşturma adımlarında size kılavuzluk eder. Aşağıdaki adımlar, diyagramda da görüldüğü gibi bağlantıyı oluşturun:

![s2s-policy](./media/vpn-gateway-ipsecikepolicy-rm-powershell/s2spolicy.png)

Bkz: [S2S VPN bağlantısı oluşturma](vpn-gateway-create-site-to-site-rm-powershell.md) daha ayrıntılı bir S2S VPN bağlantısı oluşturmak için adım adım yönergeler için.

### <a name="before"></a>Başlamadan önce

* Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Azure Resource Manager PowerShell cmdlet'lerini yükleyin. Bkz: [genel bakış, Azure PowerShell](/powershell/azure/overview) PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi.

### <a name="createvnet1"></a>1. adım - sanal ağ VPN ağ geçidi ve yerel ağ geçidi oluşturma

#### <a name="1-declare-your-variables"></a>1. Değişkenlerinizi bildirme

Bu alıştırmada değişkenlerimizi bildirerek başlayın. Üretim için yapılandırma sırasında bu değerleri kendi değerlerinizle değiştirdiğinizden emin olun.

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

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. Aboneliğinize bağlanın ve yeni bir kaynak grubu oluşturun

Resource Manager cmdlet’lerini kullanmak için PowerShell moduna geçtiğinizden emin olun. Daha fazla bilgi için [Windows PowerShell’i Resource Manager ile kullanma](../powershell-azure-resource-manager.md) konusuna bakın.

PowerShell konsolunuzu açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName $Sub1
New-AzResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a>3. Sanal ağ VPN ağ geçidi ve yerel ağ geçidi oluşturma

Aşağıdaki örnek, sanal ağı, üç alt ağ ile testvnet1'i ve VPN ağ geçidi oluşturur. Kendi değerlerinizi yerleştirirken ağ geçidi alt ağınızı özellikle GatewaySubnet olarak adlandırmanız önem taşır. Başka bir ad kullanırsanız ağ geçidi oluşturma işleminiz başarısız olur.

```powershell
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

$gw1pip1    = New-AzPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1      = Get-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1    = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1

New-AzVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1

New-AzLocalNetworkGateway -Name $LNGName6 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP6 -AddressPrefix $LNGPrefix61,$LNGPrefix62
```

### <a name="s2sconnection"></a>2. adım - bir IPSec/IKE İlkesi ile bir S2S VPN bağlantısı oluşturma

#### <a name="1-create-an-ipsecike-policy"></a>1. Bir IPSec/IKE ilkesi oluşturma

Aşağıdaki örnek betik, aşağıdaki algoritmaları ve parametrelerle bir IPSec/IKE ilkesi oluşturur:

* Ikev2: AES256, SHA384 DHGroup24
* IPSec: AES256, SHA256, PFS hiçbiri, SA yaşam süresi 14400 saniye ve değeri 102400000 KB'dir

```powershell
$ipsecpolicy6 = New-AzIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup None -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000
```

IPSec için GCMAES kullanırsanız, hem IPSec şifrelemesi hem de bütünlüğü için aynı GCMAES algoritmasını ve anahtar uzunluğu kullanmalısınız. Örneğin yukarıdaki karşılık gelen parametre olacak "-IpsecEncryption GCMAES256 - IpsecIntegrity GCMAES256" GCMAES256 kullanırken.

#### <a name="2-create-the-s2s-vpn-connection-with-the-ipsecike-policy"></a>2. S2S VPN bağlantısı IPSec/IKE ilke oluşturun

Bir S2S VPN bağlantısı oluşturma ve daha önce oluşturduğunuz IPsec/IKE ilke uygulayın.

```powershell
$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng6 = Get-AzLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

New-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
```

İsteğe bağlı olarak Ekle "-UsePolicyBasedTrafficSelectors $True" Azure VPN ağ geçidi, şirket içi, ilke tabanlı VPN cihazına bağlanmak, yukarıda açıklanan şekilde etkinleştirmek için Oluştur bağlantısını cmdlet'e.

> [!IMPORTANT]
> Bir IPSec/IKE İlkesi, bir bağlantıda belirlendikten sonra Azure VPN ağ geçidi yalnızca göndermek veya belirtilen şifreleme algoritmaları ve anahtar güçleriyle o belirli bağlantı IPSec/IKE teklifi kabul edin. Bağlantı için şirket içi VPN cihazınızın kullandığı veya tam ilke birleşimi, aksi takdirde S2S VPN tüneli değil kuracak kabul emin olun.


## <a name ="vnet2vnet"></a>Bölüm 4 - yeni bir VNet-VNet bağlantı IPSec/IKE İlkesi ile oluşturma

Bir IPSec/IKE İlkesi ile VNet-VNet bağlantısı oluşturmaya yönelik adımlar, S2S VPN bağlantısının benzerdir. Aşağıdaki örnek komut, diyagramda da görüldüğü gibi bağlantıyı oluşturun:

![v2v-policy](./media/vpn-gateway-ipsecikepolicy-rm-powershell/v2vpolicy.png)

Bkz: [bir VNet-VNet bağlantısı oluşturma](vpn-gateway-vnet-vnet-rm-ps.md) daha ayrıntılı bir VNet-VNet bağlantısı oluşturma adımları için. Tamamlamanız gereken [3. Kısım](#crossprem) oluşturma ve TestVNet1 ve VPN ağ geçidi yapılandırma.

### <a name="createvnet2"></a>1. adım: ikinci sanal ağ ve VPN ağ geçidi oluşturma

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

#### <a name="2-create-the-second-virtual-network-and-vpn-gateway-in-the-new-resource-group"></a>2. Yeni kaynak grubunu ikinci sanal ağ ve VPN ağ geçidi oluşturma

```powershell
New-AzResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

$gw2pip1    = New-AzPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
$vnet2      = Get-AzVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2    = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gw2ipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1

New-AzVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance
```

### <a name="step-2---create-a-vnet-tovnet-connection-with-the-ipsecike-policy"></a>2\. adım - bir VNet toVNet bağlantı IPSec/IKE İlkesi ile oluşturma

S2S VPN bağlantısı için benzer bir IPSec/IKE ilkesi oluşturma, ardından yeni bir bağlantı ilkesi uygulamak.

#### <a name="1-create-an-ipsecike-policy"></a>1. Bir IPSec/IKE ilkesi oluşturma

Aşağıdaki örnek betik, şu algoritmalar ve parametreler ile farklı bir IPsec/IKE ilkesi oluşturur:
* Ikev2: Aes128, SHA1, DHGroup14
* IPSec: GCMAES128, GCMAES128, PFS14, SA yaşam süresi 14400 saniye & değeri 102400000 KB'dir

```powershell
$ipsecpolicy2 = New-AzIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption GCMAES128 -IpsecIntegrity GCMAES128 -PfsGroup PFS14 -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000
```

#### <a name="2-create-vnet-to-vnet-connections-with-the-ipsecike-policy"></a>2. VNet-VNet bağlantıları ile IPSec/IKE ilkesi oluşturma

Bir VNet-VNet bağlantısı oluşturabilir ve oluşturduğunuz IPsec/IKE ilkesini uygulayın. Bu örnekte, iki ağ geçidi için aynı abonelikte ' dir. Bu nedenle oluşturmak ve her iki bağlantı aynı PowerShell oturumunda aynı bir IPSec/IKE İlkesi ile yapılandırmak mümkündür.

```powershell
$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$vnet2gw = Get-AzVirtualNetworkGateway -Name $GWName2  -ResourceGroupName $RG2

New-AzVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'

New-AzVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'
```

> [!IMPORTANT]
> Bir IPSec/IKE İlkesi, bir bağlantıda belirlendikten sonra Azure VPN ağ geçidi yalnızca göndermek veya belirtilen şifreleme algoritmaları ve anahtar güçleriyle o belirli bağlantı IPSec/IKE teklifi kabul edin. Her iki bağlantı için IPSec ilkeleri Aksi takdirde VNet-VNet bağlantısı değil kuracak aynı olduğundan emin olun.

Bu adımları tamamladıktan sonra birkaç dakika sonra bağlantı kurulur ve başlangıçta gösterildiği gibi aşağıdaki ağ topolojisini gerekir:

![IPSec IKE İlkesi](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)


## <a name ="managepolicy"></a>Bölüm 5 - bir bağlantı için güncelleştirme IPSec/IKE İlkesi

Son bölümde var olan bir S2S veya VNet-VNet bağlantı IPSec/IKE ilkesini yönetmek nasıl gösterir. Aşağıdaki alıştırmada bir bağlantı üzerinde aşağıdaki işlemleri anlatılmaktadır:

1. Bir bağlantı IPSec/IKE İlkesi Göster
2. Ekleme veya bağlantı IPSec/IKE ilkesini güncelleştirme
3. Bağlantı IPSec/IKE İlkesi Kaldır

Aynı adımlar S2S hem de VNet-VNet bağlantıları için geçerlidir.

> [!IMPORTANT]
> IPSec/IKE İlkesi desteklenir *standart* ve *HighPerformance* rota tabanlı VPN ağ geçitleri yalnızca. Temel ağ geçidi SKU'su ya da ilke tabanlı VPN ağ geçidi çalışmaz.

#### <a name="1-show-the-ipsecike-policy-of-a-connection"></a>1. Bir bağlantı IPSec/IKE İlkesi Göster

Aşağıdaki örnek, bir bağlantı üzerinde yapılandırılmış IPSec/IKE İlkesi almak gösterilmektedir. Komut dosyalarını da yukarıdaki alıştırmaları devam edin.

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

Son komut, varsa bağlantısında yapılandırılmış geçerli bir IPSec/IKE İlkesi listeler. Bağlantı için örnek çıktı aşağıda verilmiştir:

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

Yoksa hiçbir IPSec/IKE İlkesi yapılandırılmış, komut (PS > $connection6.policy) boş bir dönüş alır. IPSec/IKE bağlantısında yapılandırılmamış, ancak hiçbir özel IPSec/IKE İlkesi olduğu anlamına gelmez. Gerçek bağlantı Azure VPN ağ geçidi ile şirket içi VPN cihazınız arasında anlaşılan varsayılan ilkeyi kullanır.

#### <a name="2-add-or-update-an-ipsecike-policy-for-a-connection"></a>2. Ekleme veya bir bağlantı için bir IPSec/IKE İlkesi güncelleştirme

Yeni bir ilke ekleyin veya mevcut bir ilkenin bir bağlantıda güncelleştirmek için adımlar aynıdır: daha sonra yeni ilkeyi uygulamak için bağlantı yeni bir ilke oluşturun.

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$newpolicy6   = New-AzIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup None -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000

Set-AzVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6
```

Bir şirket içi ilke tabanlı VPN cihazına bağlanırken "UsePolicyBasedTrafficSelectors" etkinleştirmek için Ekle "-UsePolicyBaseTrafficSelectors" cmdlet'e parametre veya seçenek devre dışı bırakmak için $False olarak ayarlayın:

```powershell
Set-AzVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6 -UsePolicyBasedTrafficSelectors $True
```

İlke güncelleştirildiğini yeniden denetlemek için bağlantı elde edebilirsiniz.

```powershell
$connection6  = Get-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

Aşağıdaki örnekte gösterildiği gibi son satırında, bir çıktı görmeniz gerekir:

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

#### <a name="3-remove-an-ipsecike-policy-from-a-connection"></a>3. Bir bağlantıdan bir IPSec/IKE ilkesini Kaldır

Bir bağlantıdan özel bir ilkeyi kaldırdığınızda, Azure VPN ağ geçidi döner [varsayılan IPSec/IKE teklifleri listesine](vpn-gateway-about-vpn-devices.md) ve yeniden şirket içi VPN cihazınız ile yeniden anlaşmaya varılır.

```powershell
$RG1           = "TestPolicyRG1"
$Connection16  = "VNet1toSite6"
$connection6   = Get-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$currentpolicy = $connection6.IpsecPolicies[0]
$connection6.IpsecPolicies.Remove($currentpolicy)

Set-AzVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6
```

Bağlantı kaldırıldı ilkeyi denetlemek için aynı betiği kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [birden çok şirket içi ilke tabanlı VPN cihazını bağlama](vpn-gateway-connect-multiple-policybased-rm-ps.md) ilke tabanlı trafik seçicileri ile ilgili daha fazla ayrıntı için.

Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Adımlar için bkz. [Sanal Makine Oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
