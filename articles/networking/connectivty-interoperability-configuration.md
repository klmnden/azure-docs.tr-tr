---
title: 'Birlikte çalışabilirlik Azure arka uç bağlantısı özellikleri: Yapılandırma ayrıntıları | Microsoft Docs'
description: ExpressRoute, siteden siteye VPN ve sanal ağ eşlemesi ile Azure arasında birlikte çalışabilirlik analiz etmek için kullanabileceğiniz test kurulumu için yapılandırma ayrıntıları bu makalede açıklanır.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: 2ceb4aeac55bd555a41c29bd41b00c771490e5f9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60425800"
---
# <a name="interoperability-in-azure-back-end-connectivity-features-test-configuration-details"></a>Birlikte çalışabilirlik Azure arka uç bağlantısı özellikleri: Test yapılandırma ayrıntıları

Yapılandırma ayrıntıları bu makalede [kurulumunu test][Setup]. Test kurulumu nasıl Azure Ağ Hizmetleri interoperate denetim düzlemi ve veri düzlemi düzeyindeki analiz etmenize yardımcı olur.

## <a name="spoke-vnet-connectivity-by-using-vnet-peering"></a>VNet eşlemesi kullanarak uç VNet bağlantısı

Aşağıdaki şekilde bir uç sanal ağ (VNet) Azure sanal ağ eşleme ayrıntılarını gösterir. İki sanal ağ arasında eşleme ayarlama hakkında bilgi edinmek için bkz: [yönetme VNet eşlemesi][VNet-Config]. Merkez sanal ağa, select bağlı ağ geçidi kullanan VNet uç istiyorsanız **uzak ağ geçitlerini kullan**.

[![1]][1]

Aşağıdaki şekil, merkez sanal ağa sanal ağ eşleme ayrıntılarını gösterir. Hub VNet ağ geçitleriniz kullanılacak sanal ağ uç istiyorsanız belirleyin **uzak ağ geçitlerini kullan**.

[![2]][2]

## <a name="branch-vnet-connectivity-by-using-a-site-to-site-vpn"></a>Siteden siteye VPN kullanarak sanal ağa bağlantı dal

Azure VPN ağ geçidi VPN ağ geçitleri kullanarak hub dal sanal ağları arasında siteden siteye VPN bağlantısı ayarlayın. Varsayılan olarak, VPN ağ geçitleri ve Azure ExpressRoute ağ geçitleri özel Otonom sistem numarası (ASN) değerini kullanın **65515**. VPN ağ geçidi ASN değeri değiştirebilirsiniz. Sanal ağ VPN ağ geçidi dal ASN değeri değiştirilir test kurulumunda **65516** eBGP hub ve dal sanal ağlar arasında yönlendirme desteklemek için.


[![3]][3]


## <a name="on-premises-location-1-connectivity-by-using-expressroute-and-a-site-to-site-vpn"></a>ExpressRoute ve siteden siteye VPN kullanarak şirket içi konum 1 bağlantı

### <a name="expressroute-1-configuration-details"></a>ExpressRoute 1 yapılandırma ayrıntıları

Şirket içi konum 1 müşteri edge (CE) yönlendiricileri doğru Azure bölgesini 1 ExpressRoute bağlantı hattı yapılandırma aşağıdaki şekilde gösterilmiştir:

[![4]][4]

Aşağıdaki şekil 1 ExpressRoute bağlantı hattı ve merkez sanal ağı arasında bağlantı yapılandırması gösterilmektedir:

[![5]][5]

Aşağıdaki liste, ExpressRoute özel eşlemesi bağlantısını için birincil CE yönlendirici yapılandırmasını gösterir. (Cisco ASR1000 yönlendiriciler test Kurulum CE yönlendiricileri olarak kullanılır.) Azure ExpressRoute bağlantı hattı siteden siteye VPN ve ExpressRoute bağlantı hatları, paralel bir şirket içi ağı Azure'a bağlama için yapılandırıldığında, varsayılan olarak öncelik verir. Asimetrik yönlendirmeyi önlemek için şirket içi ağ de siteden siteye VPN bağlantısı ExpressRoute bağlantınızın öncelik vermelisiniz. Aşağıdaki yapılandırma, BGP kullanarak önceliği kurar **yerel tercih** özniteliği:

    interface TenGigabitEthernet0/0/0.300
     description Customer 30 private peering to Azure
     encapsulation dot1Q 30 second-dot1q 300
     ip vrf forwarding 30
     ip address 192.168.30.17 255.255.255.252
    !
    interface TenGigabitEthernet1/0/0.30
     description Customer 30 to south bound LAN switch
     encapsulation dot1Q 30
     ip vrf forwarding 30
     ip address 192.168.30.0 255.255.255.254
     ip ospf network point-to-point
    !
    router ospf 30 vrf 30
     router-id 10.2.30.253
     redistribute bgp 65021 subnets route-map BGP2OSPF
     network 192.168.30.0 0.0.0.1 area 0.0.0.0
    default-information originate always
     default-metric 10
    !
    router bgp 65021
     !
     address-family ipv4 vrf 30
      network 10.2.30.0 mask 255.255.255.128
      neighbor 192.168.30.18 remote-as 12076
      neighbor 192.168.30.18 activate
      neighbor 192.168.30.18 next-hop-self
      neighbor 192.168.30.18 soft-reconfiguration inbound
      neighbor 192.168.30.18 route-map prefer-ER-over-VPN in
      neighbor 192.168.30.18 prefix-list Cust30_to_Private out
     exit-address-family
    !
    route-map prefer-ER-over-VPN permit 10
     set local-preference 200
    !
    ip prefix-list Cust30_to_Private seq 10 permit 10.2.30.0/25
    !

### <a name="site-to-site-vpn-configuration-details"></a>Siteden siteye VPN yapılandırma ayrıntıları

Aşağıdaki liste, siteden siteye VPN bağlantısı için birincil CE yönlendirici yapılandırmasını gösterir:

    crypto ikev2 proposal Cust30-azure-proposal
     encryption aes-cbc-256 aes-cbc-128 3des
     integrity sha1
     group 2
    !
    crypto ikev2 policy Cust30-azure-policy
     match address local 66.198.12.106
     proposal Cust30-azure-proposal
    !
    crypto ikev2 keyring Cust30-azure-keyring
     peer azure
      address 52.168.162.84
      pre-shared-key local IamSecure123
      pre-shared-key remote IamSecure123
    !
    crypto ikev2 profile Cust30-azure-profile
     match identity remote address 52.168.162.84 255.255.255.255
     identity local address 66.198.12.106
     authentication local pre-share
     authentication remote pre-share
     keyring local Cust30-azure-keyring
    !
    crypto ipsec transform-set Cust30-azure-ipsec-proposal-set esp-aes 256 esp-sha-hmac
     mode tunnel
    !
    crypto ipsec profile Cust30-azure-ipsec-profile
     set transform-set Cust30-azure-ipsec-proposal-set
     set ikev2-profile Cust30-azure-profile
    !
    interface Loopback30
     ip address 66.198.12.106 255.255.255.255
    !
    interface Tunnel30
     ip vrf forwarding 30
     ip address 10.2.30.125 255.255.255.255
     tunnel source Loopback30
     tunnel mode ipsec ipv4
     tunnel destination 52.168.162.84
     tunnel protection ipsec profile Cust30-azure-ipsec-profile
    !
    router bgp 65021
     !
     address-family ipv4 vrf 30
      network 10.2.30.0 mask 255.255.255.128
      neighbor 10.10.30.254 remote-as 65515
      neighbor 10.10.30.254 ebgp-multihop 5
      neighbor 10.10.30.254 update-source Tunnel30
      neighbor 10.10.30.254 activate
      neighbor 10.10.30.254 soft-reconfiguration inbound
     exit-address-family
    !
    ip route vrf 30 10.10.30.254 255.255.255.255 Tunnel30

## <a name="on-premises-location-2-connectivity-by-using-expressroute"></a>ExpressRoute kullanarak şirket içi konum 2 bağlantı

Şirket içi konum 2 için daha yakından olmasa ikinci bir ExpressRoute devresi, şirket içi konum 2 merkez sanal ağa bağlanır. Aşağıdaki şekilde ikinci ExpressRoute yapılandırması gösterilmektedir:

[![6]][6]

Aşağıdaki şekilde ikinci ExpressRoute bağlantı hattı ve merkez sanal ağı arasında bağlantı yapılandırması gösterilmektedir:

[![7]][7]

ExpressRoute 1, farklı bir Azure bölgesinde uzak bir sanal ağa merkez sanal ağı ve şirket içi konum 1 bağlanır:

[![8]][8]

## <a name="expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>Tutarlılığın ExpressRoute ve siteden siteye VPN bağlantısı

###  <a name="site-to-site-vpn-over-expressroute"></a>ExpressRoute üzerinden siteden siteye VPN

ExpressRoute özel olarak şirket içi ağınız ve Azure Vnet'ler arasında veri alışverişi eşleme Microsoft kullanarak siteden siteye VPN yapılandırabilirsiniz. Bu yapılandırma ile gizliliği, kimlik doğrulaması ve bütünlük ile veri alışverişinde bulunabilir. Veri değişimi yürütmeyi de olur. ExpressRoute eşdüzey hizmet sağlama Microsoft kullanarak siteden siteye IPSec VPN tüneli modunda yapılandırma hakkında daha fazla bilgi için bkz. [ExpressRoute Microsoft eşlemesi üzerinde siteden siteye VPN][S2S-Over-ExR]. 

Microsoft eşlemesi kullanan bir siteden siteye VPN yapılandırma birincil sınırlama aktarım hızıdır. IPSec tüneli üzerinden aktarım hızı ile VPN ağ geçidi kapasitesi sınırlı. VPN gateway performansı ExpressRoute üretilen işten daha küçük. Bu senaryoda, ExpressRoute bant genişliği kullanımını iyileştirmek için yüksek oranda güvenli trafiği IPSec tünel kullanılarak ve diğer tüm trafiği için özel eşdüzey hizmet sağlama kullanarak yardımcı olur.

### <a name="site-to-site-vpn-as-a-secure-failover-path-for-expressroute"></a>ExpressRoute için bir güvenli bir yük devretme yolu olarak siteden siteye VPN

ExpressRoute, yüksek kullanılabilirlik sağlamak için yedekli devre çiftinin görev yapar. Farklı Azure bölgelerinde coğrafi olarak yedekli ExpressRoute bağlantı yapılandırabilirsiniz. Ayrıca, bir Azure bölgesi içinde bizim test Kurulum gösterildiği şekilde bir yük devretme yolu için ExpressRoute bağlantınızı oluşturmak için bir siteden siteye VPN kullanabilirsiniz. ExpressRoute ve siteden siteye VPN üzerinden aynı ön eklerin tanıtılıp, Azure ExpressRoute önceliklendirir. ExpressRoute ve siteden siteye VPN arasında asimetrik yönlendirme önlemek için ağ yapılandırması de siteden siteye VPN bağlantısı kullanmadan önce ExpressRoute bağlantısı kullanarak reciprocate şirket içi.

ExpressRoute ve siteden siteye VPN için bir arada var olabilen bağlantılar yapılandırma hakkında daha fazla bilgi için bkz. [ExpressRoute ve siteden siteye birlikte kullanımı][ExR-S2S-CoEx].

## <a name="extend-back-end-connectivity-to-spoke-vnets-and-branch-locations"></a>Arka uç bağlantı uç sanal ağları ve şube konumları için genişletin

### <a name="spoke-vnet-connectivity-by-using-vnet-peering"></a>VNet eşlemesi kullanarak uç VNet bağlantısı

Merkez ve uç VNet mimarisi yaygın olarak kullanılır. Hub merkezi bir şirket içi ağınıza, uç sanal ağları arasında bağlantı noktası gören azure'daki bir sanal ağ ' dir. Uçlar hub'la eş sanal ağ olan ve hangi iş yüklerini yalıtmak için kullanabilirsiniz. Hub'ı bir ExpressRoute veya VPN bağlantısı aracılığıyla şirket içi veri merkeziniz arasındaki trafik akışı. Mimarisi hakkında daha fazla bilgi için bkz. [Azure'da merkez-uç ağ topolojisi uygulama][Hub-n-Spoke].

Bir bölge içinde eşlemesi sanal ağda uç sanal ağları, uzak ağlarla iletişim kuracak hub VNet ağ geçitleriniz (hem VPN ve ExpressRoute ağ geçitleri) kullanabilirsiniz.

### <a name="branch-vnet-connectivity-by-using-site-to-site-vpn"></a>Siteden siteye VPN kullanarak sanal ağa bağlantı dal

Sanal ağlar farklı bölgelerde ve şirket içi ağlarda hub sanal ağ birbirleriyle iletişim kurmak için dal isteyebilirsiniz. Yerel Azure bu yapılandırma için siteden siteye VPN bağlantısı bir VPN kullanarak çözümüdür. Bir alternatif, hub'ı yönlendirme için bir ağ sanal Gereci (NVA) kullanmaktır.

Daha fazla bilgi için [VPN ağ geçidi nedir?] [ VPN] ve [yüksek oranda kullanılabilir bir NVA dağıtın][Deploy-NVA].

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [denetim düzlemi analiz] [ Control-Analysis] test kurulumu ve farklı sanal ağları veya VLAN'lar topolojide görünümlerini.

Hakkında bilgi edinin [veri düzlemi analiz] [ Data-Analysis] test kurulumu ve izleme görünümleriyle Özelliği Azure ağı.

Bkz: [ExpressRoute SSS] [ ExR-FAQ] için:
-   Bir ExpressRoute ağ geçidine bağlanabilir kaç ExpressRoute bağlantı hatları öğrenin.
-   Bir ExpressRoute bağlantı hattına bağlayabilirsiniz kaç ExpressRoute ağ geçitleri hakkında bilgi edinin.
-   Diğer ExpressRoute ölçek sınırları hakkında bilgi edinin.


<!--Image References-->
[1]: ./media/backend-interoperability/SpokeVNet_peering.png "uç VNet'ın sanal ağ eşlemesi"
[2]: ./media/backend-interoperability/HubVNet-peering.png "hub sanal ağın sanal ağ eşlemesi"
[3]: ./media/backend-interoperability/BranchVNet-VPNGW.png "bir dalın VNet VPN ağ geçidi yapılandırması"
[4]: ./media/backend-interoperability/ExR1.png "ExpressRoute 1 yapılandırma"
[5]: ./media/backend-interoperability/ExR1-Hub-Connection.png "bir hub'a VNet ExR ağ geçidi bağlantısı yapılandırma ExpressRoute 1"
[6]: ./media/backend-interoperability/ExR2.png "ExpressRoute 2 yapılandırma"
[7]: ./media/backend-interoperability/ExR2-Hub-Connection.png "VNet ExR ağ geçidi hub'ına ExpressRoute 2 bağlantı yapılandırma"
[8]: ./media/backend-interoperability/ExR2-Remote-Connection.png "uzak VNet ExR ağ geçidi için ExpressRoute 2 bağlantı yapılandırma"

<!--Link References-->
[Setup]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-preface
[ExpressRoute]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways
[VNet]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal
[Configuration]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-configuration
[Control-Analysis]:https://docs.microsoft.com/azure/networking/connectivty-interoperability-control-plane
[Data-Analysis]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-data-plane
[ExR-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[S2S-Over-ExR]: https://docs.microsoft.com/azure/expressroute/site-to-site-vpn-over-microsoft-peering
[ExR-S2S-CoEx]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager
[Hub-n-Spoke]: https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke
[Deploy-NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha
[VNet-Config]: https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering


