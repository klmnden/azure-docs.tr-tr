---
title: VPN ağ geçidi ayarlarını Azure yığınının | Microsoft Docs
description: VPN ağ geçitleri Azure yığın ile kullanmak için ayarları hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: fa8d3adc-8f5a-4b4f-8227-4381cf952c56
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/18/2018
ms.author: brenduns
ms.openlocfilehash: d23f5b91e08c169975ac5d0bb8d9f048828c2910
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="vpn-gateway-configuration-settings-for-azure-stack"></a>Azure yığını için VPN ağ geçidi yapılandırma ayarları

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bir VPN ağ geçidi, sanal ağınızda Azure yığını ve uzak bir VPN ağ geçidi arasında şifrelenmiş trafik gönderir sanal ağ geçidi türüdür. Uzak VPN ağ geçidi, Azure, bir aygıt, veri merkezinizdeki veya başka bir sitedeki bir cihaz olabilir.  İki uç noktaları ağ bağlantısı varsa, iki ağ arasında güvenli bir siteden siteye (S2S) VPN bağlantı kurabilirsiniz.

Bir VPN gateway bağlantısı her biri yapılandırılabilir ayarları içeren yapılandırmasına göre birden fazla kaynağı kullanır. Bu makalede bölümlerde kaynakları ve Resource Manager dağıtım modelinde oluşturulmuş bir sanal ağ için bir VPN ağ geçidi ile ilgili ayarları açıklanmaktadır. Her bağlantı çözümünüz için açıklamaları ve topoloji diyagramları bulabilirsiniz [Azure yığınının VPN Gateway hakkında](azure-stack-vpn-gateway-about-vpn-gateways.md).

## <a name="vpn-gateway-settings"></a>VPN ağ geçidi ayarları

### <a name="gateway-types"></a>Ağ geçidi türleri
Her Azure yığın sanal ağ türü olmalıdır tek sanal ağ geçidi destekleyen **Vpn**.  Bu destek ek türlerini destekler Azure'dan farklıdır.  

Bir sanal ağ geçidi oluştururken, ağ geçidi türü yapılandırmanız için doğru olduğundan emin olmanız gerekir. Bir VPN ağ geçidi gerektirir `-GatewayType Vpn`.

Örnek:
```PowerShell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn
-VpnType RouteBased
```

### <a name="gateway-skus"></a>Ağ geçidi SKU'ları
Bir sanal ağ geçidi oluşturduğunuzda, kullanmak istediğiniz ağ geçidi SKU’sunu belirtmeniz gerekir. İş yükü, aktarım hızı, özellik ve SLA türlerine bağlı olarak gereksinimlerinize uyan SKU'ları seçin.

Aşağıdaki VPN ağ geçidi SKU'ları Azure yığın sunar:

|   | VPN Gateway işlemesi |VPN Gateway maks. IPSec tünelleri |
|-------|-------|-------|
|**Temel SKU**  | 100 Mbps  | 10    |
|**Standart SKU**           | 100 Mbps  | 10    |
|**Yüksek performanslı SKU** | 200 Mbps    | 5 |

### <a name="resizing-gateway-skus"></a>Ağ geçidi SKU'ları yeniden boyutlandırma
Azure yığın SKU desteklenen eski SKU'ları arasında bir yeniden boyutlandırma desteklemez.

Benzer şekilde, Azure yığın (VpnGw1, VpnGw2 ve VpnGw3) Azure tarafından desteklenen yeni SKU'ları için desteklenen bir eski SKU (temel, standart ve HighPerformance) gelen bir yeniden boyutlandırma desteklemez.

### <a name="configure-the-gateway-sku"></a>Ağ geçidi SKU'su yapılandırın
#### <a name="azure-stack-portal"></a>Azure yığın portalı
Bir Resource Manager sanal ağ geçidi oluşturmak için Azure yığın Portalı'nı kullanırsanız, açılan listeyi kullanarak ağ geçidi SKU'su seçebilirsiniz. İle sunulan seçenekler, seçtiğiniz VPN türü ve ağ geçidi türü için karşılık gelir.

#### <a name="powershell"></a>PowerShell
Aşağıdaki PowerShell örnek VpnGw1 - GatewaySku belirtir.

```PowerShell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig -GatewaySku VpnGw1
-GatewayType Vpn -VpnType RouteBased
```
### <a name="connection-types"></a>Bağlantı türleri
Resource Manager dağıtım modelinde, her yapılandırma belirli sanal ağ geçidi bağlantı türü gerektirir. -ConnectionType için kullanılabilir Resource Manager PowerShell değerleri şunlardır:

- IPsec

Aşağıdaki PowerShell örneğinde S2S bağlantısı IPSec bağlantı türü gerektiren oluşturulur.  

```PowerShell
New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg
-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local
-ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
```

### <a name="vpn-types"></a>VPN türleri
VPN ağ geçidi yapılandırması için sanal ağ geçidi oluşturduğunuzda, bir VPN türü belirtmeniz gerekir. Seçtiğiniz VPN türü oluşturmak istediğiniz bağlantı topolojisine bağlıdır.  Bir VPN türü ayrıca kullandığınız donanımda bağlı olabilir. S2S yapılandırmaları bir VPN cihazı gerektirir. Bazı VPN cihazlarının yalnızca belirli bir VPN türü destekler.

> [!IMPORTANT]  
> Şu anda Azure yığını yalnızca rota tabanlı VPN türü destekler. Cihazınız yalnızca ilke tabanlı VPN'leri destekliyorsa, Azure yığın gelen bağlantıları bu cihazlar için desteklenmez.  Ayrıca, Azure yığın özel IPSec/IKE İlkesi yapılandırmalarını olmadığından ilke tabanlı trafik Seçici rota tabanlı ağ geçitleri için şu anda kullanmayı desteklemez henüz desteklenmiyor.

- **PolicyBased**: *(Azure, ancak Azure yığını tarafından desteklenir)* ilke temelli VPN'ler şifreler ve yönlendirirler adres öneklerinin birleşimleriyle yapılandırılmış IPSec ilkeleri temelindeki IPSec tüneller üzerinden paketleri Şirket içi ağınız ve Azure yığın VNet arasında. İlke (veya trafik seçici) çoğunlukla VPN cihazı yapılandırmasında bir erişim listesi olarak tanımlanır.

- **RouteBased**: RouteBased VPN IP iletme veya yönlendirme tablosunu paketleri kendi ilgili arabirimlerine yönlendirmek "yolları" kullanın. Bundan sonra tünel arabirimleri, paketleri tünellerin içinde veya dışında şifreler veya şifrelerini çözer. İlke (veya trafik Seçici) RouteBased VPN için yapılandırılmış olan herhangi herhangi olarak (veya joker karakterler) tarafından varsayılan ve değiştirilemez. RouteBased RouteBased VPN türüyle ilgili değer.

Aşağıdaki PowerShell örnek RouteBased - VpnType belirtir. Bir ağ geçidi oluştururken, -VpnType öğesinin yapılandırmanız için doğru olduğundan emin olmanız gerekir.

```PowerShell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig
-GatewayType Vpn -VpnType RouteBased
```
### <a name="gateway-requirements"></a>Ağ geçidi gereksinimleri
Aşağıdaki tabloda, VPN ağ geçitleri için gereksinimleri listelenmiştir.

| |PolicyBased temel VPN ağ geçidi | RouteBased temel VPN ağ geçidi | RouteBased standart VPN ağ geçidi | RouteBased yüksek performanslı VPN Gateway|
|--|--|--|--|--|
| **Siteden siteye bağlantı (S2S bağlantısı)** | Desteklenmiyor | RouteBased VPN yapılandırması | RouteBased VPN yapılandırması | RouteBased VPN yapılandırması |
| **Kimlik doğrulama yöntemi**  | Desteklenmiyor | S2S bağlantısı için önceden paylaşılan anahtar  | S2S bağlantısı için önceden paylaşılan anahtar  | S2S bağlantısı için önceden paylaşılan anahtar  |   
| **S2S bağlantısı sayısı**  | Desteklenmiyor | 10 | 10| 5|
|**Etkin yönlendirme desteği (BGP)** | Desteklenmiyor | Desteklenmiyor | Desteklenen | Desteklenen |

### <a name="gateway-subnet"></a>Ağ geçidi alt ağı
Bir VPN ağ geçidi oluşturmadan önce bir ağ geçidi alt ağı oluşturmanız gerekir. Ağ geçidi alt ağı sanal ağ geçidi sanal makineleri ve hizmetleri kullanan IP adreslerini içerir. Sanal ağ geçidinizi oluşturduğunuzda, ağ geçidi VM ağ geçidi alt ağına dağıtılan ve gerekli VPN ağ geçidi ayarlarıyla yapılandırılır. Başka bir şey (örneğin, ek VM'ler) ağ geçidi alt ağına dağıtmayın. Ağ geçidi alt ağı 'GatewaySubnet' adlı gerekir düzgün çalışması için. Ağ geçidi alt ağını adlandırmayla 'GatewaySubnet' Azure sanal ağ geçidi sanal makineleri ve Hizmetleri dağıtmak için alt ağı tanımlamak için yığın sağlar.

Ağ geçidi alt ağı oluştururken, alt ağın içerdiği IP adresi sayısını belirtirsiniz. Ağ geçidi alt ağdaki IP adresleri ağ geçidi sanal makineleri ve ağ geçidi Hizmetleri ayrılır. Bazı yapılandırmalar için diğerlerinden daha fazla IP adresi gerekir. Oluşturma ve oluşturmak istediğiniz ağ geçidi alt ağı bu gereksinimleri karşıladığını doğrulamak istediğiniz yapılandırma yönergelerini bakın. Ayrıca, ağ geçidi alt ağınızı gelecekteki olası ek yapılandırmalar karşılamak için yeterli IP adreslerini içerdiğinden emin olmak isteyebilirsiniz. Bir ağ geçidi alt ağı/29 kadar küçük oluşturabilirsiniz, ancak 28 ya da daha büyük bir ağ geçidi alt ağı oluşturmanızı öneririz (/ 28, / 27, /26 vs.). İşlevselliği gelecekte eklerseniz, bu şekilde, ağ geçidiniz, kesmeden sonra silip için daha fazla IP adresine izin vermek için ağ geçidi alt ağı gerekmez.

Aşağıdaki Resource Manager PowerShell örnek GatewaySubnet adlı bir ağ geçidi alt ağı gösterir. Şu anda mevcut çoğu yapılandırma için yeterli IP adresine izin veren bir/27 CIDR gösteriminde belirtir görebilirsiniz.

```PowerShell
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

> [!IMPORTANT]
> Ağ geçidi alt ağlarıyla çalışırken, ağ güvenlik grubunu (NSG) ağ geçidi alt ağıyla ilişkilendirmekten kaçının. Ağ güvenlik grubunun bu alt ağ ile ilişkilendirilmesi, VPN Gateway’inizin beklendiği gibi çalışmayı durdurmasına neden olabilir. Ağ güvenlik grupları hakkında daha fazla bilgi için bkz: [bir ağ güvenlik grubu nedir?](/azure/virtual-network/virtual-networks-nsg).

### <a name="local-network-gateways"></a>Yerel ağ geçidi geçitleri
Bir VPN ağ geçidi yapılandırması Azure'da oluştururken, yerel ağ geçidi genellikle şirket içi konumunuzu temsil eder. Azure yığınında Azure yığın dışında bulunur uzak bir VPN cihazı temsil eder.  Bu, veri merkezi, uzak bir veri merkezinde veya Azure VPN ağ geçidi bir VPN cihazı olabilir.

Yerel ağ geçidi, VPN cihazının genel IP adresi olmak üzere bir ad verin ve şirket içi konumunda olan adres öneklerini belirtirsiniz. Azure ağ trafiği için hedef adres öneklerine bakar, yerel ağ geçidiniz için belirttiğiniz yapılandırma bakar ve paketleri buna uygun şekilde yönlendirir.

Aşağıdaki PowerShell örnek yeni bir yerel ağ geçidi oluşturur:

```PowerShell
New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```
Bazen yerel ağ geçidi ayarlarını değiştirmeniz gerekir. Örneğin, eklediğinizde veya adres aralığını değiştirmek veya VPN cihazının IP adresi değişip değişmediğini. Bkz: [PowerShell kullanarak yerel ağ geçidi ayarlarını değiştirmek](/azure/vpn-gateway/vpn-gateway-modify-local-network-gateway).

## <a name="ipsecike-parameters"></a>IPSec/IKE parametreleri
Bir VPN bağlantısı Azure yığınında ayarladığınızda, bağlantının her iki uçta yapılandırmanız gerekir.  Azure yığını ve bir anahtar veya bir VPN ağ geçidi olarak işlev gören, yönlendirici gibi bir donanım aygıtı arasında bir VPN bağlantısı yapılandırıyorsanız, bu cihaz için ek ayarlar isteyebilir.

Birden çok teklifleri hem Başlatıcı hem de bir Yanıtlayıcı olarak destekleyen, Azure farklı olarak, yalnızca bir teklifi Azure yığını destekler.

###  <a name="ike-phase-1-main-mode-parameters"></a>IKE Aşama 1 (Ana Mod) parametreleri
| Özellik              | Değer|
|-|-|
| IKE Sürümü           | IKEv2 |
|Diffie-Hellman Grubu   | Grup 2 (1024 bit) |
| Kimlik Doğrulama Yöntemi | Önceden Paylaşılan Anahtar |
|Şifreleme ve Karma Algoritmaları | AES256, SHA256 |
|SA Yaşam Süresi (Zaman)     | 28.800 saniye|

### <a name="ike-phase-2-quick-mode-parameters"></a>IKE Aşama 2 (Hızlı Mod) parametreleri
| Özellik| Değer|
|-|-|
|IKE Sürümü |IKEv2 |
|Şifreleme ve karma algoritmalar (şifreleme)     | GCMAES256|
|Şifreleme ve karma algoritmalar (kimlik doğrulaması) | GCMAES256|
|SA Yaşam Süresi (Zaman)  | 27.000 saniye |
|SA Yaşam Süresi (Bayt) | 819,200       |
|Kusursuz İletme Gizliliği (PFS) |PFS2048 |
|Kullanılmayan Eş Algılama | Desteklenen|  
