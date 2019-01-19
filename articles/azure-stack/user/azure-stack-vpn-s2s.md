---
title: Azure Stack siteden siteye VPN bağlantıları yapılandırma | Microsoft Docs
description: Azure stack'teki siteden siteye VPN veya VNet-VNet bağlantıları için IPSec/IKE İlkesi hakkında bilgi edinin
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2019
ms.author: sethm
ms.openlocfilehash: cfd46f8178f36213ecc16db0e092e81ac2d0eff1
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2019
ms.locfileid: "54414779"
---
# <a name="configure-ipsecike-policy-for-site-to-site-vpn-or-vnet-to-vnet-connections"></a>Siteden siteye VPN veya VNet-VNet bağlantıları için IPSec/IKE ilkesi yapılandırma

Bu makale, siteden siteye (S2S) VPN için bir IPSec/IKE İlkesi yapılandırmak için gereken adımları size Azure Stack'te bağlantıları.

## <a name="ipsec-and-ike-policy-parameters-for-vpn-gateways"></a>VPN ağ geçitleri için IPSec ve IKE ilke parametreleri

Standart IPSec ve IKE protokolü çeşitli birleşimler çok çeşitli şifreleme algoritmalarını destekler. Azure Stack'te parametreleri desteklenen görmek için bkz [IPSec/IKE parametreleri](azure-stack-vpn-gateway-settings.md#ipsecike-parameters), uyumluluk veya güvenlik gereksinimlerinizi karşılayan yardımcı olabilir.

Bu makalede, oluşturun ve yeni veya mevcut bir bağlantı için geçerli bir IPSec/IKE ilkesi yapılandırma hakkında yönergeler açıklanmaktadır.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu ilkeler kullanılırken aşağıdaki önemli noktalara dikkat unutmayın:

- IPSec/IKE ilkesi yalnızca üzerinde çalışır *standart* ve *HighPerformance* (rota tabanlı) ağ geçidi SKU'ları.

- Yalnızca belirtebilirsiniz **bir** belirli bir bağlantı için ilke birleşimi.

- Hem IKE (ana mod) hem de IPSec (hızlı mod) için tüm algoritmaları ve parametreleri belirtmeniz gerekir. Kısmi ilke belirtimine izin verilmez.

- İlke, şirket içi VPN cihazlarınızda desteklenen emin olmak için VPN cihaz satıcısı belirtimlerine başvurun. İlkeleri uyumsuzsa siteden siteye bağlantı kurulamıyor.

## <a name="part-1---workflow-to-create-and-set-ipsecike-policy"></a>Bölüm 1 - oluşturun ve IPSec/IKE İlkesi ayarlamak için iş akışı

Bu bölümde bir siteden siteye VPN bağlantısı IPSec/IKE ilkesi oluşturmak için gerekli iş akışını açıklar:

1. Bir sanal ağ ve VPN ağ geçidi oluşturun.

2. Şirketler arası bağlantı için bir yerel ağ geçidi oluşturun.

3. Seçili algoritmaları ve parametrelerle IPSec/IKE ilkesi oluşturun.

4. Bir IPSec bağlantısı IPSec/IKE ilke ile oluşturun.

5. Ekleme/güncelleştirme/kaldırma için mevcut bir bağlantıyı bir IPSec/IKE ilkesi.

Bu makalede Yardımı'ndaki yönergeleri ayarlayabilir ve IPSec/IKE ilkeleri, aşağıdaki resimde gösterildiği gibi yapılandırın:

![Ayarlama ve IPSec/IKE ilkelerini yapılandırma](media/azure-stack-vpn-s2s/site-to-site.png)

## <a name="part-2---supported-cryptographic-algorithms-and-key-strengths"></a>Bölüm 2 - desteklenen şifreleme algoritmaları ve anahtar güçleriyle

Aşağıdaki tabloda, şifreleme algoritmaları ve anahtar güçleri Azure Stack müşterileri tarafından listelenmektedir:

| IPSec/Ikev2                                          | Seçenekler                                                                  |
|------------------------------------------------------|--------------------------------------------------------------------------|
| IKEv2 Şifrelemesi                                     | AES256, AES192, AES128, DES3, DES                                        |
| IKEv2 Bütünlüğü                                      | SHA384, SHA256, SHA1, MD5                                                |
| DH Grubu                                             | ECP384, ECP256, DHGroup14, DHGroup2048, DHGroup2, DHGroup1, hiçbiri         |
| IPsec Şifrelemesi                                     | GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, None |
| IPsec Bütünlüğü                                      | GCMASE256, GCMAES192, GCMAES128, SHA256, SHA1, MD5                       |
| PFS Grubu                                            | PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, Hiçbiri                         |
| QM SA Yaşam Süresi                                       | (İsteğe bağlı: varsayılan değerleri kullanılan belirtilmediğinde)<br />                         Saniye (tamsayı; en az 300/varsayılan 27000 saniye)<br />                         Kilobayt (tamsayı; en az 1024/varsayılan 102400000 kilobayt) |                                                                          |
| Trafik Seçicisi                                     | İlke tabanlı trafik seçicileri Azure Stack'te desteklenmez.         |

- Şirket içi VPN cihazı yapılandırmanızın Azure IPsec/IKE ilkesinde belirttiğiniz şu algoritmalar ve parametrelerle eşleşmesi ya da bunları içermesi gerekir:

  - IKE şifreleme algoritması (ana mod / 1. Aşama)
  - IKE bütünlük algoritması (ana mod / 1. Aşama)
  - DH grubu (ana mod / Aşama 1)
  - IPSec şifreleme algoritması (hızlı mod / 2. Aşama)
  - IPSec bütünlük algoritması (hızlı mod / 2. Aşama)
  - PFS grubu (hızlı mod / Aşama 2)
  - SA yaşam süreleri yalnızca yerel belirtimlerdir ve bunların eşleşmesi gerekmez.

- GCMAES IPSec şifreleme algoritması için kullanılırsa, anahtar uzunluğu IPSec bütünlüğü ve aynı GCMAES algoritmasını seçmeniz gerekir; Örneğin, GCMAES128 her ikisi için kullanma.

- Yukarıdaki tabloda:

  - Ikev2 ana modu veya 1. Aşama karşılık gelir.
  - IPSec hızlı mod veya Aşama 2'ye karşılık gelir.
  - DH grubu, Diffie-Hellmen ana mod veya 1. Aşama kullanılan grubun belirtir
  - PFS grubu, Diffie-Hellmen hızlı mod veya 2. Aşama kullanılan grubun belirtilen

- Ikev2 ana modu SA yaşam süresi 28.800 saniye olarak Azure Stack VPN ağ geçitleri sabitlenmiştir.

Aşağıdaki tabloda özel ilke tarafından desteklenen karşılık gelen Diffie-Hellman grupları listelenmiştir:

| Diffie-Hellman Grubu | DHGroup   | PFSGroup      | Anahtar uzunluğu    |
|----------------------|-----------|---------------|---------------|
| 1                    | DHGroup1  | PFS1          | 768 bit MODP  |
| 2                    | DHGroup2  | PFS2          | 1024 bit MODP |
| 14                   | DHGroup14 |               |               |
| DHGroup2048          | PFS2048   | 2048 bit MODP |               |
| 19                   | ECP256    | ECP256        | 256 bit ECP   |
| 20                   | ECP384    | ECP284        | 384 bit ECP   |

Daha fazla bilgi için [RFC3526](https://tools.ietf.org/html/rfc3526) ve [RFC5114](https://tools.ietf.org/html/rfc5114).

## <a name="part-3---create-a-new-site-to-site-vpn-connection-with-ipsecike-policy"></a>3. Kısım - yeni bir siteden siteye VPN bağlantısı IPSec/IKE ilke oluşturun

Bu bölümde bir IPSec/IKE İlkesi ile bir siteden siteye VPN bağlantısı oluşturma adımlarında size kılavuzluk eder. Aşağıdaki adımlar, aşağıdaki şekilde gösterildiği gibi bağlantı oluşturun:

![Site için site İlkesi](media/azure-stack-vpn-s2s/site-to-site.png)

Daha ayrıntılı bir siteden siteye VPN bağlantısı oluşturmak için adım adım yönergeler için bkz. [bir siteden siteye VPN bağlantısı oluşturma](../../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md).

### <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdaki önkoşulların karşılandığından emin olun:

- Azure aboneliği. Azure aboneliğiniz yoksa, etkinleştirebilir, [MSDN abone Avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), ya da kaydolun bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/).

- Azure Resource Manager PowerShell cmdlet'leri. Bkz: [Azure Stack için PowerShell yükleme](../azure-stack-powershell-install.md) PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi.

### <a name="step-1---create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a>1. adım - sanal ağ VPN ağ geçidi ve yerel ağ geçidi oluşturma

#### <a name="1-declare-variables"></a>1. Değişkenleri bildirin

Bu alıştırma için aşağıdaki değişkenleri bildirerek başlayın. Üretim için yapılandırırken yer tutucuları kendi değerlerinizle değiştirdiğinizden emin olun:

```powershell
$Sub1 = "\<YourSubscriptionName\>"
$RG1 = "TestPolicyRG1"
$Location1 = "East US 2"
$VNetName1 = "TestVNet1"
$FESubName1 = "FrontEnd"
$BESubName1 = "Backend"
$GWSubName1 = "GatewaySubnet"
$VNetPrefix11 = "10.11.0.0/16"
$VNetPrefix12 = "10.12.0.0/16"
$FESubPrefix1 = "10.11.0.0/24"
$BESubPrefix1 = "10.12.0.0/24"
$GWSubPrefix1 = "10.12.255.0/27"
$DNS1 = "8.8.8.8"
$GWName1 = "VNet1GW"
$GW1IPName1 = "VNet1GWIP1"
$GW1IPconf1 = "gw1ipconf1"
$Connection16 = "VNet1toSite6"
$LNGName6 = "Site6"
$LNGPrefix61 = "10.61.0.0/16"
$LNGPrefix62 = "10.62.0.0/16"
$LNGIP6 = "131.107.72.22"
```

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. Aboneliğinize bağlanın ve yeni bir kaynak grubu oluşturun

Resource Manager cmdlet’lerini kullanmak için PowerShell moduna geçtiğinizden emin olun. Daha fazla bilgi için [bir kullanıcı olarak PowerShell ile Azure stack'e bağlanma](azure-stack-powershell-configure-user.md).

PowerShell konsolunuzu açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

```powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a>3. Sanal ağ VPN ağ geçidi ve yerel ağ geçidi oluşturma

Aşağıdaki örnek, sanal ağ oluşturur. **TestVNet1**üç alt ağ ve VPN ağ geçidi. Değerlerinizi yerleştirirken, her zaman ağ geçidi alt ağınızı özellikle adlandırmanız önem **GatewaySubnet**. Başka bir ad kullanırsanız ağ geçidi oluşturma işleminiz başarısız olur.

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

$gw1pip1 = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1

$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet"
-VirtualNetwork $vnet1

$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1
-Subnet $subnet1 -PublicIpAddress $gw1pip1

New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
-Location $Location1 -IpConfigurations $gw1ipconf1 -GatewayType Vpn
-VpnType RouteBased -GatewaySku VpnGw1

New-AzureRmLocalNetworkGateway -Name $LNGName6 -ResourceGroupName $RG1
-Location $Location1 -GatewayIpAddress $LNGIP6 -AddressPrefix
$LNGPrefix61,$LNGPrefix62
```

### <a name="step-2---create-a-site-to-site-vpn-connection-with-an-ipsecike-policy"></a>2. adım - bir IPSec/IKE İlkesi ile siteden siteye VPN bağlantısı oluşturma

#### <a name="1-create-an-ipsecike-policy"></a>1. Bir IPSec/IKE ilkesi oluşturma

Bu örnek betik, aşağıdaki algoritmaları ve parametrelerle bir IPSec/IKE ilkesi oluşturur:

- Ikev2: Aes128, SHA1, DHGroup14
- IPSec: AES256, SHA256, none, SA yaşam süresi 14400 saniye ve değeri 102400000 KB'dir

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup none -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000
```

IPSec için GCMAES kullanırsanız, hem IPSec şifrelemesi hem de bütünlüğü için aynı GCMAES algoritmasını ve anahtar uzunluğu kullanmalısınız.

#### <a name="2-create-the-site-to-site-vpn-connection-with-the-ipsecike-policy"></a>2. Siteden siteye VPN bağlantısı IPSec/IKE ilke oluşturun

Bir siteden siteye VPN bağlantısı oluşturma ve önceden oluşturduğunuz IPsec/IKE İlkesi uygulayın.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$lng6 = Get-AzureRmLocalNetworkGateway -Name $LNGName6 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -IpsecPolicies $ipsecpolicy6 -SharedKey 'Azs123'
```

> [!IMPORTANT]
> Bir IPSec/IKE İlkesi, bir bağlantıda belirlendikten sonra Azure VPN ağ geçidi yalnızca göndermek veya belirtilen şifreleme algoritmaları ve anahtar güçleriyle o belirli bağlantı IPSec/IKE teklifi kabul edin. Bağlantı için şirket içi VPN cihazınızın kullandığı veya siteden siteye VPN tüneli kurulmaz tam ilke birleşimi, aksi takdirde kabul emin olun.

## <a name="part-4---update-ipsecike-policy-for-a-connection"></a>Bölüm 4 - bir bağlantı için güncelleştirme IPSec/IKE İlkesi

Önceki bölümde, var olan bir siteden siteye bağlantı IPSec/IKE ilkesini yönetmek nasıl gösterilmiştir. Aşağıdaki bölümde bir bağlantı üzerinde aşağıdaki işlemleri anlatılmaktadır:

1. Bir bağlantı IPSec/IKE İlkesi Göster
2. Ekleme veya bağlantı IPSec/IKE ilkesini güncelleştirme
3. Bağlantı IPSec/IKE İlkesi Kaldır

> [!NOTE]
> IPSec/IKE İlkesi desteklenir *standart* ve *HighPerformance* rota tabanlı VPN ağ geçitleri yalnızca. Üzerinde çalışmaz *temel* ağ geçidi SKU'sunu.

### <a name="1-show-the-ipsecike-policy-of-a-connection"></a>1. Bir bağlantı IPSec/IKE İlkesi Göster

Aşağıdaki örnek, bir bağlantı üzerinde yapılandırılmış IPSec/IKE İlkesi almak gösterilmektedir. Betikler ayrıca önceki alıştırmada devam edin:

```powershell
$RG1 = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6 = Get-AzureRmVirtualNetworkGatewayConnection -Name
$Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

Son komut, bağlantı varsa yapılandırılmış geçerli bir IPSec/IKE İlkesi listeler. Aşağıdaki örnekte, bağlantı için örnek çıktı verilmiştir:

```shell
SALifeTimeSeconds : 14400
SADataSizeKilobytes : 102400000
IpsecEncryption : AES256
IpsecIntegrity : SHA256
IkeEncryption : AES128
IkeIntegrity : SHA1
DhGroup : DHGroup14
PfsGroup : None
```

Yoksa hiçbir IPSec/IKE İlkesi yapılandırılmış, komut `$connection6.policy` boş dönüş alır. IPSec/IKE bağlantısında yapılandırılmamış gelmez; Bu, özel IPSec/IKE İlkesi yok anlamına gelir. Gerçek bağlantı Azure VPN ağ geçidi ile şirket içi VPN cihazınız arasında anlaşılan varsayılan ilkeyi kullanır.

### <a name="2-add-or-update-an-ipsecike-policy-for-a-connection"></a>2. Ekleme veya bir bağlantı için bir IPSec/IKE İlkesi güncelleştirme

Yeni bir ilke ekleyin veya mevcut bir ilkenin bir bağlantıda güncelleştirmek için adımlar aynıdır: yeni bir ilke oluşturun ve ardından yeni ilkeyi uygulamak için bağlantı.

```powershell
$RG1 = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6 = Get-AzureRmVirtualNetworkGatewayConnection -Name
$Connection16 -ResourceGroupName $RG1

$newpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup None -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000

$connection6.SharedKey = "AzS123"

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6
```

Bağlantı İlkesi güncelleştirilir, yeniden denetlemek için alabilirsiniz:

```powershell
$connection6 = Get-AzureRmVirtualNetworkGatewayConnection -Name
$Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

Aşağıdaki örnekte gösterildiği gibi son satırında, bir çıktı görmeniz gerekir:

```shell
SALifeTimeSeconds : 14400
SADataSizeKilobytes : 102400000
IpsecEncryption : AES256
IpsecIntegrity : SHA256
IkeEncryption : AES128
IkeIntegrity : SHA1
DhGroup : DHGroup14
PfsGroup : None
```

### <a name="3-remove-an-ipsecike-policy-from-a-connection"></a>3. Bir bağlantıdan bir IPSec/IKE ilkesini Kaldır

Bir bağlantıdan özel bir ilkeyi kaldırdığınızda, Azure VPN ağ geçidi döner [varsayılan IPSec/IKE teklif](azure-stack-vpn-gateway-settings.md#ipsecike-parameters)ve şirket içi VPN cihazınız ile yeniden anlaşmaya varılır.

```powershell
$RG1 = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6 = Get-AzureRmVirtualNetworkGatewayConnection -Name
$Connection16 -ResourceGroupName $RG1
$connection6.SharedKey = “AzS123”
$currentpolicy = $connection6.IpsecPolicies\[0\]
$connection6.IpsecPolicies.Remove($currentpolicy)

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6
```

Bağlantı kaldırıldı ilkeyi denetlemek için aynı betiği kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack için VPN gateway yapılandırma ayarları](azure-stack-vpn-gateway-settings.md)