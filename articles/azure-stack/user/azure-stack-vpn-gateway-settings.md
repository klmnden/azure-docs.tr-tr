---
title: Azure Stack için VPN gateway ayarları | Microsoft Docs
description: VPN ağ geçitleri Azure Stack ile kullanmak için ayarlar hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: fa8d3adc-8f5a-4b4f-8227-4381cf952c56
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: sethm
ms.lastreviewed: 12/27/2018
ms.openlocfilehash: 1ff5aeddbf05011f7c7d105e6c48552bca81580c
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58483292"
---
# <a name="vpn-gateway-configuration-settings-for-azure-stack"></a>Azure Stack için VPN gateway yapılandırma ayarları

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bir VPN ağ geçidi, sanal ağınızda Azure Stack ve uzak bir VPN ağ geçidi arasında şifrelenmiş trafik gönderen sanal ağ geçidi türüdür. Uzak VPN ağ geçidi, Azure, veri merkezinizde bir cihaz veya bir cihazda başka bir site olabilir. İki uç nokta ağ bağlantısı varsa, iki ağ arasında güvenli bir siteden siteye (S2S) VPN bağlantısı kurabilirsiniz.

Bir VPN ağ geçidi bağlantısı, her biri yapılandırılabilir ayarlar içeren yapılandırmasına birden çok kaynak kullanır. Bu makalede Resource Manager dağıtım modelinde oluşturulan sanal ağ için bir VPN ağ geçidi ile ilgili ayarlar ve kaynaklar açıklanır. Her bağlantı çözüm için açıklamalar ve topoloji diyagramlarını bulabilirsiniz [Azure Stack için VPN Gateway hakkında](azure-stack-vpn-gateway-about-vpn-gateways.md).

## <a name="vpn-gateway-settings"></a>VPN gateway ayarları

### <a name="gateway-types"></a>Ağ geçidi türleri

Her Azure Stack sanal ağ türü olması gereken tek bir sanal ağ geçidi, destekliyor **Vpn**.  Bu destek ek türlerini destekler Azure'dan farklıdır.  

Bir sanal ağ geçidi oluşturduğunuzda ağ geçidi türünü yapılandırmanız için doğru olduğundan emin olmanız gerekir. Bir VPN ağ geçidi gerektirir `-GatewayType Vpn`bayrak; örneğin:

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn
-VpnType RouteBased
```

### <a name="gateway-skus"></a>Ağ geçidi SKU'ları

Bir sanal ağ geçidi oluşturduğunuzda, kullanmak istediğiniz SKU ağ geçidi belirtmeniz gerekir. İş yükü, aktarım hızı, özellik ve SLA türlerine bağlı olarak gereksinimlerinize uyan SKU'ları seçin.

Azure Stack, VPN ağ geçidi SKU'ları aşağıdaki tabloda gösterilen sunar.

|   | VPN gateway performansı |VPN ağ geçidi en fazla IPSec tüneli |
|-------|-------|-------|
|**Temel SKU**  | 100 Mbps  | 20    |
|**Standart SKU**           | 100 Mbps  | 20    |
|**Yüksek performanslı SKU** | 200 Mbps    | 10    |

### <a name="resizing-gateway-skus"></a>Ağ geçidi SKU'ları yeniden boyutlandırma

Azure Stack, bir yeniden boyutlandırma SKU'lar arasında desteklenen eski SKU'ları desteklemez.

Benzer şekilde, Azure Stack (VpnGw1, VpnGw2 ve VpnGw3) Azure tarafından desteklenen daha yeni bir SKU için desteklenen bir eski SKU (temel, standart ve yüksek performanslı) gelen bir yeniden boyutlandırma desteklemiyor

### <a name="configure-the-gateway-sku"></a>Ağ geçidi SKU'sunu yapılandırın

#### <a name="azure-stack-portal"></a>Azure Stack portalı

Resource Manager sanal ağ geçidi oluşturmak için Azure Stack portalını kullanıyorsanız, açılan listeyi kullanarak ağ geçidi SKU'sunu seçebilirsiniz. Seçenekler, seçtiğiniz VPN türü ve ağ geçidi türü için karşılık gelir.

#### <a name="powershell"></a>PowerShell

Aşağıdaki PowerShell örneği belirtir **- GatewaySku** olarak `VpnGw1`:

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig -GatewaySku VpnGw1
-GatewayType Vpn -VpnType RouteBased
```

### <a name="connection-types"></a>Bağlantı türleri

Resource Manager dağıtım modelinde, her yapılandırma bir özel sanal ağ geçidi bağlantı türü gerektirir. Kullanılabilir Resource Manager PowerShell değerleri **- ConnectionType** şunlardır:

* IPsec

   Aşağıdaki PowerShell örneği, IPSec bağlantı türü gerektiren bir S2S bağlantısı oluşturulur:

   ```powershell
   New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg
   -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local
   -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
   ```

### <a name="vpn-types"></a>VPN türleri

VPN ağ geçidi yapılandırması için sanal ağ geçidi oluşturduğunuzda, bir VPN türünü belirtmeniz gerekir. Oluşturmak istediğiniz bağlantı topolojisine seçtiğiniz VPN türüne bağlıdır. Bir VPN türü, ayrıca kullandığınız donanımda bağlı olabilir. S2S yapılandırmaları bir VPN cihazı gerektirir. Bazı VPN cihazlarının yalnızca belirli bir VPN türünü destekler.

> [!IMPORTANT]  
> Şu anda, Azure Stack, yalnızca rota tabanlı VPN türünü destekler. Cihazınız yalnızca ilke tabanlı VPN'ler destekliyorsa, Azure Stack bu cihazlara bağlantılarından sonra desteklenmez.  
>
> Özel IPSec/IKE İlkesi yapılandırmalarını desteklenmediği için Ayrıca, Azure Stack ilke tabanlı trafik seçicileri için rota tabanlı ağ geçitleri şu anda kullanma desteği olmamasıdır.

* **PolicyBased**: İlke tabanlı VPN'ler şifreler ve şirket içi ağınız ve Azure Stack Vnet'iniz arasında adres öneklerinin birleşimleriyle yapılandırılmış IPSec ilkeleri temelindeki IPSec tüneller üzerinden paketleri doğrudan. İlke veya trafik Seçici, genellikle VPN cihazı yapılandırmasında bir erişim listesi olduğu.

  >[!NOTE]
  >**PolicyBased** azure'da ve Azure stack'teki desteklenir.

* **RouteBased**: RouteBased VPN IP iletme veya yönlendirme tablosuna paketleri kendi ilgili arabirimlerine yapılandırılan yollar kullanın. Bundan sonra tünel arabirimleri, paketleri tünellerin içinde veya dışında şifreler veya şifrelerini çözer. İlke veya trafik Seçici için **RouteBased** VPN'ler herhangi bir ağdan herhangi olarak yapılandırılır (veya joker karakterler kullanın.) Varsayılan olarak, bunlarda değişiklik yapılamaz. Değeri bir **RouteBased** VPN türü **RouteBased**.

Aşağıdaki PowerShell örneği belirtir **- VpnType** olarak **RouteBased**. Bir ağ geçidi oluşturduğunuzda, emin olmanız gerekir **- VpnType** yapılandırmanız için doğru olduğundan.

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig
-GatewayType Vpn -VpnType RouteBased
```

### <a name="gateway-requirements"></a>Ağ geçidi gereksinimleri

Aşağıdaki tabloda, VPN ağ geçitleri için gereksinimler listelenmektedir.

| |PolicyBased temel VPN Gateway | Temel RouteBased VPN ağ geçidi | Standart RouteBased VPN ağ geçidi | RouteBased ve yüksek performanslı VPN Gateway|
|--|--|--|--|--|
| **Siteden siteye bağlantı (S2S bağlantısı)** | Desteklenmiyor | RouteBased VPN yapılandırması | RouteBased VPN yapılandırması | RouteBased VPN yapılandırması |
| **Kimlik doğrulama yöntemi**  | Desteklenmiyor | S2S bağlantısı için önceden paylaşılan anahtar  | S2S bağlantısı için önceden paylaşılan anahtar  | S2S bağlantısı için önceden paylaşılan anahtar  |   
| **S2S bağlantılarının maksimum sayısı**  | Desteklenmiyor | 20 | 20| 10|
|**Etkin yönlendirme desteği (BGP)** | Desteklenmiyor | Desteklenmiyor | Desteklenen | Desteklenen |

### <a name="gateway-subnet"></a>Ağ geçidi alt ağı

Bir VPN ağ geçidi oluşturmadan önce bir ağ geçidi alt ağı oluşturmanız gerekir. Ağ geçidi alt ağı sanal ağ geçidi Vm'lerini ve hizmetlerini kullanan IP adresleri bulunur. Sanal ağ geçidinizi oluştururken, ağ geçidi Vm'leri ağ geçidi alt ağına dağıtılır ve gerekli VPN ağ geçidi ayarlarla yapılandırılır. Başka bir şey (örneğin, ek VM'ler) ağ geçidi alt ağına dağıtmayın.

>[!IMPORTANT]
>Ağ geçidi alt ağı düzgün çalışması için **GatewaySubnet** şeklinde adlandırılmalıdır. Azure Stack, sanal ağ geçidi Vm'leri ve Hizmetleri dağıtmak için alt ağı belirlerken bu adı kullanır.

Ağ geçidi alt ağı oluştururken, alt ağın içerdiği IP adresi sayısını belirtirsiniz. Ağ geçidi alt ağı IP adresleri, ağ geçidi Vm'leri ve ağ geçidi hizmetlerine ayrılır. Bazı yapılandırmalar için diğerlerinden daha fazla IP adresi gerekir. Oluşturun ve oluşturmak istediğiniz ağ geçidi alt ağı bu gereksinimleri karşıladığını doğrulamak için istediğiniz yapılandırmayı yönergelerine bakın.

Ayrıca, ağ geçidi alt ağınızı gelecekteki ek yapılandırmalar işlemek için yeterli IP adresi olduğundan emin olmanız gerekir. / 29 kadar küçük bir ağ geçidi alt ağı oluşturabilirsiniz, ancak bir ağ geçidi alt ağı/28'lik veya daha büyük (/ 28, / 27, / 26 vb..) oluşturduğunuz öneririz İşlevselliğini gelecekte eklerseniz bu şekilde, geçidinizi yıkılıp sonra silin ve daha fazla IP adresi için izin vermek için ağ geçidi alt ağı oluşturmanız gerekmez.

Aşağıdaki Resource Manager PowerShell örnek adlı bir ağ geçidi alt ağı gösterir **GatewaySubnet**. Şu anda mevcut çoğu yapılandırma için yeterli IP adresi izin veren bir/27 CIDR gösterimini belirtir görebilirsiniz.

```powershell
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

> [!IMPORTANT]
> Ağ geçidi alt ağlarıyla çalışırken, ağ güvenlik grubunu (NSG) ağ geçidi alt ağıyla ilişkilendirmekten kaçının. Bu alt ağ için ağ güvenlik grubu ilişkilendirilmesi, VPN gateway'inizin biklendiği gibi çalışmayı durdurmasına neden olabilir. Ağ güvenlik grupları hakkında daha fazla bilgi için bkz. [bir ağ güvenlik grubu nedir?](../../virtual-network/virtual-networks-nsg.md).

### <a name="local-network-gateways"></a>Yerel ağ geçidi geçitleri

Bir VPN ağ geçidi yapılandırması Azure'da oluştururken, yerel ağ geçidi genellikle şirket içi konumunuzu temsil eder. Azure Stack'te Azure Stack dışında yer alan herhangi bir uzak VPN cihazı temsil eder. Bu, veri merkezinizi (veya uzak bir veri merkezinde) bir VPN cihazı ya da Azure VPN ağ geçidi olabilir.

Yerel ağ geçidi VPN cihazının genel IP adresini bir ad verip şirket içi konum olan adres öneklerini belirtirsiniz. Azure ağ trafiği için hedef adres öneklerine bakar, yerel ağ geçidiniz için belirttiğiniz yapılandırma bakar ve paketleri buna göre yönlendirir.

Sonraki PowerShell örneği, yeni bir yerel ağ geçidi oluşturur:

```powershell
New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```

Bazen yerel ağ geçidi ayarlarını değiştirmeniz gerekir; Örneğin, eklediğinizde veya adres aralığını değiştirmek veya VPN cihazının IP adresi değişirse. Bkz: [PowerShell kullanarak yerel ağ geçidi ayarlarını değiştirme](../../vpn-gateway/vpn-gateway-modify-local-network-gateway.md).

## <a name="ipsecike-parameters"></a>IPSec/IKE parametreleri

Azure Stack'te bir VPN bağlantısı ayarladığınızda, her iki uçta da bağlantı yapılandırmanız gerekir. Azure Stack ve bir anahtar veya bir VPN ağ geçidi olarak görev yapan yönlendirici gibi bir donanım cihazı arasında bir VPN bağlantısı yapılandırıyorsanız, bu cihaz için ek ayarlar isteyebilir.

Birden çok teklife destekleyen hem Başlatıcı hem de bir Yanıtlayıcı olarak Azure, Azure Stack, yalnızca bir teklif destekler.

### <a name="ike-phase-1-main-mode-parameters"></a>IKE Aşama 1 (Ana Mod) parametreleri

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
|Şifreleme ve karma algoritmaları (şifreleme)     | GCMAES256|
|Şifreleme ve karma algoritmaları (kimlik doğrulaması) | GCMAES256|
|SA Yaşam Süresi (Zaman)  | 27.000 saniye  |
|SA yaşam süresi (KB) | 33,553,408     |
|Kusursuz İletme Gizliliği (PFS) |Hiçbiri<sup>bkz. Not 1</sup> |
|Kullanılmayan Eş Algılama | Desteklenen|  

* *Not 1:*  Sürüm 1807 önce Azure Stack değeri PFS2048, Perfect Forward Secrecy (PFS için) kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* [ExpressRoute kullanarak bağlanma](../azure-stack-connect-expressroute.md)
