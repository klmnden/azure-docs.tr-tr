---
title: 'Birden çok şirket içi ilke tabanlı VPN cihazı Azure VPN ağ geçitlerine bağlanın: Azure Resource Manager: PowerShell | Microsoft Docs'
description: Bir Azure rota tabanlı VPN ağ geçidi için Azure Resource Manager ve PowerShell kullanarak birden çok ilke tabanlı VPN cihazı yapılandırma.
services: vpn-gateway
documentationcenter: na
author: yushwang
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: yushwang
ms.openlocfilehash: 9085d5ee21b1e955b7d9416a379ee730ba26ad3e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66150097"
---
# <a name="connect-azure-vpn-gateways-to-multiple-on-premises-policy-based-vpn-devices-using-powershell"></a>PowerShell kullanarak birden fazla şirket içi ilke tabanlı VPN cihazı Azure VPN ağ geçitlerini bağlama

Bu makalede S2S VPN bağlantıları üzerinde özel IPSec/IKE ilkeleri yararlanarak birden çok şirket içi ilke tabanlı VPN cihazına bağlanmak için bir Azure rota tabanlı VPN ağ geçidini yapılandırmanıza yardımcı olur.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="about"></a>İlke tabanlı ve rota tabanlı VPN ağ geçitleri hakkında

İlke - *karşılaştırması* rota tabanlı VPN cihazı farklı IPSec trafik seçicileri bir bağlantıda nasıl ayarlanır:

* **İlke tabanlı** VPN cihazları nasıl şifrelenmiş/şifresi IPSec tüneller trafiği tanımlamak için her iki ağ ön ekleri birleşimlerini kullanın. Ayrıca, güvenlik duvarı cihazlarda paket filtreleme işlemleri genellikle oluşturulmuştur. IPSec tüneli şifreleme ve şifre çözme, filtreleme ve işleme paketi eklenir.
* **Rota tabanlı** VPN cihazları herhangi bir ağdan herhangi (joker karakterler) trafik seçicileri kullanın ve IPSec tüneli farklı tablolar doğrudan trafik yönlendirme/iletme olanak tanır. Genellikle, her bir IPSec tünel ağ arabirimiyle ya da VTI (sanal tunnel arabirimi) nerede modellenmiş yönlendirici platformlarda oluşturulmuştur.

Aşağıdaki diyagramlarda iki modeli seçin:

### <a name="policy-based-vpn-example"></a>İlke tabanlı VPN örneği
![İlke tabanlı](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedmultisite.png)

### <a name="route-based-vpn-example"></a>Rota tabanlı VPN örneği
![Rota tabanlı](./media/vpn-gateway-connect-multiple-policybased-rm-ps/routebasedmultisite.png)

### <a name="azure-support-for-policy-based-vpn"></a>İlke tabanlı VPN için Azure desteği
Şu anda Azure VPN ağ geçitleri hem modunu destekler: rota tabanlı VPN ağ geçitleri ve ilke tabanlı VPN ağ geçitleri. Farklı belirtimlerinde sonucunda, farklı platformlarda iç oluşturulur:

|                          | **PolicyBased VPN ağ geçidi** | **RouteBased VPN ağ geçidi**               |
| ---                      | ---                         | ---                                      |
| **Azure ağ geçidi SKU'su**    | Temel                       | Temel, standart, yüksek performanslı, VpnGw1, VpnGw2, VpnGw3 |
| **IKE sürümü**          | IKEv1                       | IKEv2                                    |
| **Maks. S2S bağlantıları** | **1**                       | Temel/standart: 10<br> Yüksek performanslı: 30 |
|                          |                             |                                          |

Özel IPSec/IKE İlkesi ile ön ek tabanlı trafik seçicileri seçeneği ile kullanılacak Azure rota tabanlı VPN ağ geçitleri artık yapılandırabilirsiniz "**PolicyBasedTrafficSelectors**", şirket içi ilke tabanlı VPN cihazı için bağlanmak için. Bu özellik, bir Azure sanal ağı birbirine bağlamanızı sağlar ve VPN ağ geçidine birden çok şirket ilke tabanlı VPN/güvenlik duvarı cihazına, tek bir bağlantı sınırı geçerli Azure ilke tabanlı VPN Gateway bileşenlerinden kaldırılıyor.

> [!IMPORTANT]
> 1. Bu bağlantıyı etkinleştirmek için şirket içi ilke tabanlı VPN cihazı desteklemelidir **Ikev2** Azure rota tabanlı VPN ağ geçitlerine bağlanmak için. VPN cihaz spesifikasyonlara denetleyin.
> 2. Şirket içi ağlar ile Bu mekanizma ilke tabanlı VPN cihazına bağlanma, yalnızca Azure sanal ağına bağlanabilir; **diğer şirket içi ağlara veya sanal ağlar aynı Azure VPN ağ geçidi üzerinden geçiş yapılamıyor**.
> 3. Yapılandırma seçeneği, özel IPSec/IKE bağlantı İlkesi bir parçasıdır. İlke tabanlı trafik Seçici seçeneği etkinleştirirseniz, tüm ilke (IPSec/IKE şifreleme ve bütünlüğü algoritmalar, anahtar güçleriyle ve SA yaşam süreleri) belirtmeniz gerekir.

Aşağıdaki diyagram, Azure VPN ağ geçidi üzerinden geçiş yönlendirmesi ilke tabanlı seçeneğiyle neden çalışmıyor gösterir:

![İlke tabanlı geçiş](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedtransit.png)

Diyagramda gösterildiği gibi Azure VPN ağ geçidi, şirket içi ağ ön ekleri ancak çapraz bağlantısından öneklerini her için sanal ağ arasında trafik seçicileri sahiptir. Örneğin, site 2, bölge 3 ve 4 site her VNet1'sırasıyla iletişim kurabilir, ancak birbirine Azure VPN ağ geçidi üzerinden bağlanamaz şirket içi. Bu yapılandırma altında Azure VPN ağ geçidinde kullanılamıyor cross-connect trafik seçicileri diyagramda gösterilmektedir.

## <a name="configurepolicybased"></a>İlke tabanlı trafik seçicileri üzerinde bağlantı yapılandırma

Bu makaledeki yönergeleri aynı örnekte açıklandığı gibi izleyin [S2S veya VNet-VNet bağlantıları için IPSec/IKE yapılandırma İlkesi](vpn-gateway-ipsecikepolicy-rm-powershell.md) S2S VPN bağlantısı kuracak şekilde. Bu, aşağıdaki diyagramda gösterilmiştir:

![s2s-policy](./media/vpn-gateway-connect-multiple-policybased-rm-ps/s2spolicypb.png)

Bu bağlantıyı etkinleştirmek için iş akışı:
1. Sanal ağ, VPN ağ geçidi ve yerel ağ geçidi için şirketler arası bağlantı oluşturma
2. Bir IPSec/IKE ilkesi oluşturma
3. Bir S2S veya VNet-VNet bağlantı oluşturduğunuzda ilkenin geçerli ve **ilke tabanlı trafik seçicileri etkinleştirme** bağlantı
4. Bağlantıyı zaten oluşturduysanız, uygulama veya mevcut bir bağlantı ilkesini güncelleştirme

## <a name="before-you-begin"></a>Başlamadan önce

Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial) için kaydolabilirsiniz.

[!INCLUDE [powershell](../../includes/vpn-gateway-cloud-shell-powershell-about.md)]

## <a name="enablepolicybased"></a>İlke tabanlı trafik seçicileri bağlantısı etkinleştirme

Tamamladığınızdan emin olun [yapılandırma IPSec/IKE İlkesi makale 3. Kısım](vpn-gateway-ipsecikepolicy-rm-powershell.md) bu bölümü için. Aşağıdaki örnek, aynı parametre ve adımları kullanır:

### <a name="step-1---create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a>1\. adım - sanal ağ VPN ağ geçidi ve yerel ağ geçidi oluşturma

#### <a name="1-connect-to-your-subscription-and-declare-your-variables"></a>1. Aboneliğinize bağlanın ve değişkenlerinizi bildirme

[!INCLUDE [sign in](../../includes/vpn-gateway-cloud-shell-ps-login.md)]

Değişkenlerinizi bildirin. Bu alıştırma için aşağıdaki değişkenleri kullanırız:

```azurepowershell-interactive
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

#### <a name="2-create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a>2. Sanal ağ VPN ağ geçidi ve yerel ağ geçidi oluşturma

Bir kaynak grubu oluşturun.

```azurepowershell-interactive
New-AzResourceGroup -Name $RG1 -Location $Location1
```

Sanal ağ TestVNet1 üç alt ağ ve VPN ağ geçidi oluşturmak için aşağıdaki örneği kullanın. Değerleri değiştirmek istiyorsanız, ağ geçidi alt ağınızı her zaman özel olarak 'GatewaySubnet' adını önemlidir. Başka bir ad kullanırsanız ağ geçidi oluşturma işleminiz başarısız olur.

```azurepowershell-interactive
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

$gw1pip1    = New-AzPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1      = Get-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1    = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1

New-AzVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance

New-AzLocalNetworkGateway -Name $LNGName6 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP6 -AddressPrefix $LNGPrefix61,$LNGPrefix62
```

### <a name="step-2---create-a-s2s-vpn-connection-with-an-ipsecike-policy"></a>2\. adım - bir IPSec/IKE İlkesi ile bir S2S VPN bağlantısı oluşturma

#### <a name="1-create-an-ipsecike-policy"></a>1. Bir IPSec/IKE ilkesi oluşturma

> [!IMPORTANT]
> Bağlantıda "UsePolicyBasedTrafficSelectors" seçeneğini etkinleştirmek için bir IPSec/IKE İlkesi oluşturmanız gerekir.

Aşağıdaki örnek, bu algoritmalar ve parametrelerle IPSec/IKE ilkesi oluşturur:
* Ikev2: AES256, SHA384 DHGroup24
* IPSec: AES256, SHA256, PFS hiçbiri, SA yaşam süresi 14400 saniye ve değeri 102400000 KB'dir

```azurepowershell-interactive
$ipsecpolicy6 = New-AzIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup None -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000
```

#### <a name="2-create-the-s2s-vpn-connection-with-policy-based-traffic-selectors-and-ipsecike-policy"></a>2. S2S VPN bağlantısı ilke tabanlı trafik seçicileri ve IPSec/IKE İlkesi ile oluşturma
Bir S2S VPN bağlantısı oluşturun ve önceki adımda oluşturduğunuz IPsec/IKE ilke uygulayın. Ek parametre dikkat "-UsePolicyBasedTrafficSelectors $True" ilke tabanlı trafik seçicileri bağlantı sağlar.

```azurepowershell-interactive
$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng6 = Get-AzLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

New-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -UsePolicyBasedTrafficSelectors $True -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
```

Adımları tamamladıktan sonra S2S VPN bağlantısı tanımlanan IPsec/IKE ilkesini kullanın ve ilke tabanlı trafik seçicileri bağlantıda etkinleştirin. Daha fazla bağlantı, ek şirket içi ilke tabanlı VPN cihazı için aynı Azure VPN ağ geçidini eklemek için aynı adımları yineleyebilirsiniz.

## <a name="update-policy-based-traffic-selectors-for-a-connection"></a>İlke tabanlı trafik seçicileri için bir bağlantı güncelleştirme
Son bölümde, var olan bir S2S VPN bağlantısı için ilke tabanlı trafik seçicileri seçeneği güncelleştirme gösterilir.

### <a name="1-get-the-connection"></a>1. Bağlantı Al
Bağlantı kaynağı alın.

```azurepowershell-interactive
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
```

### <a name="2-check-the-policy-based-traffic-selectors-option"></a>2. İlke tabanlı trafik seçicileri seçeneği işaretleyin
Aşağıdaki satırı, ilke tabanlı trafik seçicileri bağlantı için kullanılıp kullanılmayacağını gösterir:

```azurepowershell-interactive
$connection6.UsePolicyBasedTrafficSelectors
```

Satır döndürürse "**True**", ardından, ilke tabanlı trafik seçiciler, bağlantıda yapılandırılır; Aksi halde döndürür "**False**."

### <a name="3-enabledisable-the-policy-based-traffic-selectors-on-a-connection"></a>3. Bir bağlantısı ilke tabanlı trafik seçicileri etkinleştir/devre dışı bırak
Bağlantı kaynağı edindikten sonra etkinleştirebilir veya devre dışı bırakma seçeneği.

#### <a name="to-enable-usepolicybasedtrafficselectors"></a>UsePolicyBasedTrafficSelectors etkinleştirmek için
Aşağıdaki örnekte, ilke tabanlı trafik seçicileri seçeneğini etkinleştirir ancak IPSec/IKE ilke değişmeden kalır:

```azurepowershell-interactive
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

Set-AzVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $True
```

#### <a name="to-disable-usepolicybasedtrafficselectors"></a>UsePolicyBasedTrafficSelectors devre dışı bırakmak için
Aşağıdaki örnekte, ilke tabanlı trafik seçicileri seçeneği devre dışı bırakır, ancak değişmeden IPSec/IKE İlkesi bırakır:

```azurepowershell-interactive
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

Set-AzVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $False
```

## <a name="next-steps"></a>Sonraki adımlar
Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Adımlar için bkz. [Sanal Makine Oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Ayrıca gözden [S2S VPN veya VNet-VNet bağlantıları için IPSec/IKE yapılandırma İlkesi](vpn-gateway-ipsecikepolicy-rm-powershell.md) özel IPSec/IKE ilkeleri hakkında daha fazla ayrıntı için.
