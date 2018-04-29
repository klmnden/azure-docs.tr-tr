---
title: 'Birden çok şirket içi ilke tabanlı VPN cihazı Azure VPN ağ geçitleri bağlanın: Azure Resource Manager: PowerShell | Microsoft Docs'
description: Bir Azure yol tabanlı VPN ağ geçidi Azure Resource Manager ve PowerShell kullanarak birden çok ilke tabanlı VPN aygıtları için yapılandırın.
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/14/2018
ms.author: yushwang
ms.openlocfilehash: dc2dc660262cec892270f8d6e70691fdd169a5c4
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="connect-azure-vpn-gateways-to-multiple-on-premises-policy-based-vpn-devices-using-powershell"></a>PowerShell kullanarak birden çok şirket içi ilke tabanlı VPN cihazı Azure VPN ağ geçitleri Bağlan

Bu makalede özel IPSec/IKE ilkeler S2S VPN bağlantılarında yararlanarak birden çok şirket içi ilke tabanlı VPN aygıtları bağlanmak için bir Azure yol tabanlı VPN ağ geçidi yapılandırmanıza yardımcı olur.

## <a name="about-policy-based-and-route-based-vpn-gateways"></a>İlke tabanlı ve rota tabanlı VPN ağ geçitleri hakkında

İlke - *karşılaştırması* rota tabanlı VPN aygıtları farklı IPSec trafiği seçici bir bağlantıda nasıl ayarlanır:

* **İlke tabanlı** VPN aygıtları nasıl trafiği IPSec tüneller şifrelenmiş/şifresi tanımlamak için her iki ağ ön ekleri birleşimlerini kullanın. Bu genelde paket filtreleme gerçekleştiren güvenlik duvarı cihazlarda yerleşik olarak bulunur. IPSec tüneli şifreleme ve şifre çözme filtreleme ve altyapısı işleme paketi eklenir.
* **Rota tabanlı** VPN aygıtları herhangi herhangi (joker karakterler) trafiğini Seçici kullanın ve IPSec tünelleri farklı tabloları doğrudan trafik yönlendirme/iletme olanak tanır. Genellikle, burada her IPSec tüneli bir ağ arabirimi veya VTI (sanal tünel arabirimi) olarak modellenir yönlendirici platformlarda oluşturulmuştur.

Aşağıdaki diyagramlarda iki model vurgula:

### <a name="policy-based-vpn-example"></a>İlke tabanlı VPN örneği
![İlke tabanlı](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedmultisite.png)

### <a name="route-based-vpn-example"></a>Rota tabanlı VPN örneği
![Rota tabanlı](./media/vpn-gateway-connect-multiple-policybased-rm-ps/routebasedmultisite.png)

### <a name="azure-support-for-policy-based-vpn"></a>Azure ilke temelli VPN desteği
Şu anda Azure VPN ağ geçitlerinin iki modlarını destekler: rota tabanlı VPN ağ geçitleri ve ilke tabanlı VPN ağ geçitleri. Farklı teknik özelliklerine sonucunda, farklı platformlarda iç oluşturulur:

|                          | **PolicyBased VPN ağ geçidi** | **RouteBased VPN ağ geçidi**               |
| ---                      | ---                         | ---                                      |
| **Azure ağ geçidi SKU'su**    | Temel                       | Temel, standart, HighPerformance, VpnGw1, VpnGw2, VpnGw3 |
| **IKE sürümü**          | IKEv1                       | IKEv2                                    |
| **Maks. S2S bağlantılarının** | **1**                       | Basic/standart: 10<br> HighPerformance: 30 |
|                          |                             |                                          |

Özel IPSec/IKE ilkesiyle önek tabanlı trafik Seçici seçeneği ile kullanmak için Azure yol tabanlı VPN ağ geçitleri artık yapılandırabilirsiniz "**PolicyBasedTrafficSelectors**", şirket içi ilke tabanlı VPN cihazlara bağlamak için. Bu özellik bir Azure sanal ağ üzerinden bağlanmanıza olanak sağlar ve birden çok VPN ağ geçidi ilke tabanlı VPN/güvenlik duvarı aygıtları tek bağlantı sınırı geçerli Azure ilke tabanlı VPN Gateway bileşenlerinden kaldırma, şirket.

> [!IMPORTANT]
> 1. Bu bağlantıyı etkinleştirmek için şirket içi ilke tabanlı VPN aygıtları desteklemelidir **Ikev2** Azure yol tabanlı VPN ağ geçitleri bağlanmak için. VPN aygıt belirtimlerine bakın.
> 2. İlke tabanlı VPN aygıtları Bu mekanizma ile bağlanma şirket içi ağlar yalnızca Azure sanal ağa bağlanabilir; **diğer şirket içi ağlarda veya sanal ağlar aynı Azure VPN ağ geçidi üzerinden geçiş yapamazsınız**.
> 3. Yapılandırma seçeneği özel IPSec/IKE bağlantı İlkesi bir parçasıdır. İlke tabanlı trafik Seçici seçeneği etkinleştirirseniz, tam bir ilke (IPSec/IKE şifreleme ve bütünlük algoritmalar, anahtar gücü ve SA yaşam süreleri) belirtmeniz gerekir.

Aşağıdaki diyagramda gösterildiği yoluyla Azure VPN ağ geçidi transit yönlendirme ilke tabanlı seçeneğiyle neden çalışmıyor:

![İlke tabanlı geçiş](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedtransit.png)

Diyagramda gösterildiği gibi Azure VPN ağ geçidi her bir şirket içi ağ önekleri, ancak arası bağlantı önekleri sanal ağ trafiğini Seçici vardır. Örneğin, site 2, 3 site ve site 4 her VNet1'sırasıyla iletişim kurabilir ancak birbirlerine Azure VPN ağ geçidi üzerinden bağlanamıyor şirket içi. Diyagramda bu yapılandırma bölümünde Azure VPN ağ geçidi kullanılamayan cross-connect trafiğini Seçici gösterilmektedir.

## <a name="configure-policy-based-traffic-selectors-on-a-connection"></a>İlke tabanlı trafik seçici bir bağlantıda yapılandırın

Bu makalede aynı örnek bölümünde açıklanan yönergeleri [S2S veya VNet-VNet bağlantıları için IPSec/IKE yapılandırma İlkesi](vpn-gateway-ipsecikepolicy-rm-powershell.md) S2S VPN bağlantısı kurmak için. Bu, aşağıdaki çizimde gösterilmiştir:

![s2s İlkesi](./media/vpn-gateway-connect-multiple-policybased-rm-ps/s2spolicypb.png)

Bu bağlantıyı etkinleştirmek için iş akışı:
1. Sanal ağ, VPN ağ geçidi ve yerel ağ geçidi, şirket içi bağlantı oluşturma
2. Bir IPSec/IKE ilkesi oluşturun
3. S2S veya VNet-VNet bağlantısı oluştururken ilkeyi uygulamak ve **ilke tabanlı trafik Seçici etkinleştirmek** bağlantı
4. Bağlantıyı zaten oluşturduysanız, uygulama veya mevcut bir bağlantı İlkesi güncelleştir

## <a name="enable-policy-based-traffic-selectors-on-a-connection"></a>İlke tabanlı trafik seçici bir bağlantısı etkinleştir

Tamamladığınızdan emin olun [bölümü 3 IPSec/IKE yapılandırma İlkesi makalenin](vpn-gateway-ipsecikepolicy-rm-powershell.md) Bu bölüm için. Aşağıdaki örnek, aynı parametreleri ve adımları kullanır:

### <a name="step-1---create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a>1. adım - sanal ağ, VPN ağ geçidi ve yerel ağ geçidi oluşturma

#### <a name="1-declare-your-variables--connect-to-your-subscription"></a>1. Değişkenlerinizi bildirme & aboneliğinize bağlanma
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
Resource Manager cmdlet'lerini kullanmak için PowerShell moduna geçtiğinizden emin olun. Daha fazla bilgi için [Windows PowerShell’i Resource Manager ile kullanma](../powershell-azure-resource-manager.md) konusuna bakın.

PowerShell konsolunuzu açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

```powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="2-create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a>2. Sanal ağ, VPN ağ geçidi ve yerel ağ geçidi oluşturma
Aşağıdaki örnekte, sanal ağ, üç alt ağ ile TestVNet1 ve VPN ağ geçidi oluşturur. Değerleri değiştirerek, ağ geçidi alt ağınızı her zaman özellikle 'GatewaySubnet' adını önemlidir. Başka bir ad kullanırsanız ağ geçidi oluşturma işleminiz başarısız olur.

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

$gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1

New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance

New-AzureRmLocalNetworkGateway -Name $LNGName6 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP6 -AddressPrefix $LNGPrefix61,$LNGPrefix62
```

### <a name="step-2---create-a-s2s-vpn-connection-with-an-ipsecike-policy"></a>2. adım - bir IPSec/IKE İlkesi ile bir S2S VPN bağlantısı oluşturun

#### <a name="1-create-an-ipsecike-policy"></a>1. Bir IPSec/IKE ilkesi oluşturun

> [!IMPORTANT]
> Bağlantı "UsePolicyBasedTrafficSelectors" seçeneğini etkinleştirmek için bir IPSec/IKE İlkesi oluşturmanız gerekir.

Aşağıdaki örnek, bu algoritmaları ve parametreleri bir IPSec/IKE ilkesi oluşturur:
* Ikev2: AES256, SHA384 DHGroup24
* IPSec: AES256, SHA256, PFS24, SA ömrü 3600 saniye & 2048KB

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup PFS24 -SALifeTimeSeconds 3600 -SADataSizeKilobytes 2048
```

#### <a name="2-create-the-s2s-vpn-connection-with-policy-based-traffic-selectors-and-ipsecike-policy"></a>2. İlke tabanlı trafik Seçici ve IPSec/IKE İlkesi ile S2S VPN bağlantısı oluşturun
S2S VPN bağlantısı oluşturun ve önceki adımda oluşturduğunuz IPsec/IKE İlkesi uygulayın. Ek parametresinin unutmayın "-UsePolicyBasedTrafficSelectors $True" ilke tabanlı trafik Seçici bağlantı sağlar.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng6 = Get-AzureRmLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -UsePolicyBasedTrafficSelectors $True -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
```

Adımları tamamladıktan sonra S2S VPN bağlantısı tanımlanan IPsec/IKE İlkesi'ni kullanın ve ilke tabanlı trafik Seçici bağlantıda etkinleştirin. Daha fazla bağlantı şirket içinde ek ilke tabanlı VPN aygıtları için aynı Azure VPN ağ geçidi'nden eklemek için aynı adımları yineleyebilirsiniz.

## <a name="update-policy-based-traffic-selectors-for-a-connection"></a>İlke tabanlı trafik Seçici bağlantı güncelleştir
Son Kısım ilke tabanlı trafik Seçici seçeneği mevcut S2S VPN bağlantısı için bir güncelleştirme gösterilmiştir.

### <a name="1-get-the-connection"></a>1. Bağlantı Al
Bağlantı kaynağı alın.

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
```

### <a name="2-check-the-policy-based-traffic-selectors-option"></a>2. İlke tabanlı trafik Seçici seçeneği işaretleyin.
Aşağıdaki satırı, ilke tabanlı trafik Seçici bağlantı için kullanılıp kullanılmadığını gösterir:

```powershell
$connection6.UsePolicyBasedTrafficSelectors
```

Satır döndürürse "**True**", ardından ilke tabanlı trafik Seçici bağlantısı yapılandırılır; Aksi halde döndürür "**False**."

### <a name="3-update-the-policy-based-traffic-selectors-on-a-connection"></a>3. İlke tabanlı trafik seçici bir bağlantısı güncelleştir
Bağlantı kaynağı edindikten sonra etkinleştirebilir veya devre dışı bırakma seçeneği.

#### <a name="disable-usepolicybasedtrafficselectors"></a>UsePolicyBasedTrafficSelectors devre dışı bırak
Aşağıdaki örnek, ilke tabanlı trafik Seçici seçeneği devre dışı bırakır, ancak değişmeden IPSec/IKE İlkesi bırakır:

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $False
```

#### <a name="enable-usepolicybasedtrafficselectors"></a>UsePolicyBasedTrafficSelectors etkinleştir
Aşağıdaki örnek, ilke tabanlı trafik Seçici seçeneğini etkinleştirir ancak değişmeden IPSec/IKE İlkesi bırakır:

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $True
```

## <a name="next-steps"></a>Sonraki adımlar
Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Adımlar için bkz. [Sanal Makine Oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Ayrıca gözden [S2S VPN veya VNet-VNet bağlantıları için IPSec/IKE yapılandırma İlkesi](vpn-gateway-ipsecikepolicy-rm-powershell.md) özel IPSec/IKE ilkeleri hakkında daha fazla ayrıntı için.
