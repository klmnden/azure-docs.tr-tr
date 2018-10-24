---
title: 'ExpressRoute, siteden siteye VPN ve VNet eşlemesi - birlikte çalışabilirliği yapılandırma ayrıntıları: Azure arka uç bağlantısı özellikleri birlikte çalışabilirlik | Microsoft Docs'
description: Bu sayfada, ExpressRoute, siteden siteye VPN ve VNet eşlemesi özellikleri birlikte çalışabilirliği analiz etmek için kullanılan test kurulum yapılandırma ayrıntılarını sağlar.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,vpn-gateway,virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: d94900b764331c6fff0e0384e6edbebc88ac938b
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49947340"
---
# <a name="interoperability-of-expressroute-site-to-site-vpn-and-vnet-peering---test-configuration-details"></a>ExpressRoute, siteden siteye VPN ve VNet eşleme - Test yapılandırma ayrıntılarını birlikte çalışabilirliği

Bu makalede, şimdi test kurulum yapılandırma ayrıntılarını geçelim. Test Kurulumu gözden geçirmek için bkz: [Test Kurulum][Setup]. 

##<a name="spoke-vnet-connectivity-using-vnet-peering"></a>Uç sanal ağ bağlantısı kullanarak VNet eşlemesi

Aşağıdaki Azure portalı ekran görüntüsü, bir uç sanal ağı, sanal ağ eşleme ayrıntılarını gösterir. İki sanal ağ arasında VNet eşlemesi yapılandırmak adım adım yönergeler için bkz. [yönetme VNet eşlemesi][VNet-Config]. Merkez sanal ağa bağlı Ata gateway'ler kullanılacak uç VNet istiyorsanız denetlemeniz gerekir *uzak ağ geçitlerini kullan*.

[![1]][1]

Aşağıdaki Azure portalı ekran görüntüsü, merkez sanal ağa sanal ağ eşleme ayrıntılarını gösterir. Uç kendi ağ geçidi kullanan VNet izin vermek için Hub VNet istiyorsanız denetlemeniz gerekir *uzak ağ geçitlerini kullan*.

[![2]][2]

##<a name="branch-vnet-connectivity-using-site-to-site-vpn"></a>Siteden siteye VPN kullanarak sanal ağa bağlantı dal

Merkez ve şube sanal ağlar arasında siteden siteye VPN bağlantısı, VPN ağ geçitlerini kullanarak yapılandırılır. Varsayılan olarak, VPN ve ExpressRoute ağ geçitleri 65515 özel ASN değeri ile yapılandırılır. VPN ağ geçidi ASN değeri değiştirmenize izin verir. Test Kurulumu aşağıdaki Azure portal ekran görüntüsünde gösterildiği gibi 65516 eBGP Hub ve dal sanal ağlar arasında yönlendirme sağlamak için sanal ağ VPN ağ geçidi dal ASN değeri değiştirilir.


[![3]][3]


##<a name="location-1-on-premises-connectivity-using-expressroute-and-site-to-site-vpn"></a>ExpressRoute ve siteden siteye VPN kullanarak konum 1 şirket içi bağlantı

###<a name="expressroute1-configuration-details"></a>ExpressRoute1 yapılandırma ayrıntıları

Konum 1 şirket içi CE yönlendiriciler doğrultusunda Azure bölgesini 1 ExpressRoute bağlantı hattı yapılandırma aşağıdaki portal ekran görüntüsü gösterilmektedir.

[![4]][4]

Aşağıdaki portal ekran görüntüsü ExpressRoute1 devre Hub Vnet'iniz arasında bağlantı yapılandırması gösterir.

[![5]][5]

Birincil CE yönlendirici (Cisco yönlendiricileri test Kurulum CE yönlendiricileri olarak kullanılan ASR1000) listesini aşağıdaki yapılandırmadır yapılandırmasıyla ilgili ExpressRoute özel eşlemesi bağlantısı. Ne zaman hem siteden siteye VPN ve ExpressRoute bağlantı hattı şirket içi ağı Azure'a bağlama için paralel yapılandırılır; Azure ExpressRoute bağlantı hattı, varsayılan olarak tercih eder. Asimetrik yönlendirmeyi önlemek için şirket içi ağ de ExpressRoute hem siteden siteye VPN ve ExpressRoute üzerinden alınan rotalar için siteden siteye VPN üzerinden tercih etmelisiniz. Bu, aşağıdaki yapılandırmayı BGP yerel tercih özniteliği kullanılarak elde edilir. 

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

###<a name="site-to-site-vpn-configuration-details"></a>Siteden siteye VPN yapılandırma ayrıntıları

Siteden siteye VPN bağlantılarıyla ilgili birincil CE yönlendirici yapılandırması listesi verilmiştir:

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

##<a name="location-2-on-premises-connectivity-using-expressroute"></a>ExpressRoute kullanarak konum 2 şirket içi bağlantı

Daha yakından olmasa konum 2 şirket içi, ikinci ExpressRoute devresi şirket konum 2 merkez sanal ağa bağlanır. İkinci ExpressRoute yapılandırması aşağıdaki portal ekran görüntüsü gösterilmektedir.

[![6]][6]

Aşağıdaki portal ekran görüntüsü ikinci ExpressRoute bağlantı hattı ve Hub sanal ağ arasında bağlantı yapılandırması gösterir.

[![7]][7]

ExpressRoute1 şirket içi Merkezi sanal ağ ve konum 1 farklı bir Azure bölgesinde uzak bir sanal ağa bağlanır.

[![8]][8]

## <a name="further-reading"></a>Daha fazla bilgi

### <a name="using-expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>ExpressRoute ve siteden siteye VPN bağlantısı dağıtımınızla kullanma

#### <a name="site-to-site-vpn-over-expressroute"></a>ExpressRoute üzerinden siteden siteye VPN

Özel olarak gizliliği, yürütmeyi, kimlik doğrulaması ve bütünlük ile şirket içi ağınız ve Azure Vnet'ler arasında veri alışverişi ExpressRoute Microsoft eşlemesi üzerinde siteden siteye VPN yapılandırılabilir. ExpressRoute Microsoft eşlemesi üzerinde siteden siteye IPSec VPN tüneli modunda yapılandırma hakkında daha fazla bilgi için bkz. [ExpressRoute Microsoft eşlemesi üzerinde siteden siteye VPN][S2S-Over-ExR]. 

Aktarım hızı, Microsoft eşlemesi üzerinden S2S VPN yapılandırma ana sınırlamasıdır. IPSec tüneli üzerinden aktarım hızı ile VPN ağ geçidi kapasitesi sınırlı. ExpressRoute üretilen kıyasla daha az VPN GW aktarım hızıdır. Böyle senaryolarda güvenli yüksek trafik ve diğer tüm trafiği için özel eşdüzey hizmet sağlama için IPSec tüneli kullanarak ExpressRoute bant genişliği kullanımını en iyi duruma getirme yardımcı olacaktır.

#### <a name="site-to-site-vpn-as-a-secure-failover-path-for-expressroute"></a>ExpressRoute için bir güvenli bir yük devretme yolu olarak siteden siteye VPN
ExpressRoute, yüksek kullanılabilirlik sağlamak için yedekli devre çiftinin sunulur. Farklı Azure bölgelerinde coğrafi olarak yedekli ExpressRoute bağlantı yapılandırabilirsiniz. ExpressRoute bağlantınızı için bir yük devretme yolu isterseniz de bizim test ayarında gibi belirli bir Azure bölgesi içinde siteden siteye VPN kullanarak bunu yapabilirsiniz. ExpressRoute ve S2S VPN üzerinden aynı ön eklerin tanıtılıp, Azure ExpressRoute üzerinden S2S VPN tercih eder. ExpressRoute ve S2S VPN arasında asimetrik yönlendirme önlemek için ağ yapılandırması de S2S VPN bağlantısı ExpressRoute belgelemeyi reciprocate şirket içi.

ExpressRoute ve siteden siteye VPN bir arada var olabilen bağlantılar yapılandırma hakkında daha fazla bilgi için bkz. [ExpressRoute ve siteden siteye birlikte kullanımı][ExR-S2S-CoEx].

### <a name="extending-backend-connectivity-to-spoke-vnets-and-branch-locations"></a>Arka uç bağlantısı için uç sanal ağları ve şube konumları genişletme

#### <a name="spoke-vnet-connectivity-using-vnet-peering"></a>Uç sanal ağ bağlantısı kullanarak VNet eşlemesi

Merkez ve uç Vnet mimarisi yaygın olarak kullanılır. Hub'ı, merkezi bir şirket içi ağınıza, uç sanal ağları arasında bağlantı noktası gören azure'daki bir sanal ağın (VNet) olan. Uçlar hub'la eş ve iş yüklerini yalıtmak için kullanılabilir sanal ağlar ' dir. Hub'ı bir ExpressRoute veya VPN bağlantısı aracılığıyla şirket içi veri merkeziniz arasındaki trafik akışı. Mimarisi hakkında daha fazla bilgi için bkz. [merkez ve uç mimarisi][Hub-n-Spoke]

VNet eşlemesi bir bölge içinde uç sanal ağları hub VNet ağ geçitleriniz (hem VPN ve ExpressRoute ağ geçitleri), uzak ağlarla iletişim kuracak kullanmasını sağlar.

#### <a name="branch-vnet-connectivity-using-site-to-site-vpn"></a>Siteden siteye VPN kullanarak sanal ağa bağlantı dal

İstediğiniz dalı (farklı bölgelerde) sanal ağlar ve şirket içi ağlarda hub sanal ağ birbirleriyle iletişim, yerel Azure VPN kullanarak siteden siteye VPN bağlantısı çözümüdür. Alternatif bir seçenek, hub'ı yönlendirme için bir NVA kullanmaktır.

VPN ağ geçitlerini yapılandırmak için bkz: [VPN ağ geçidi yapılandırma][VPN]. Yüksek oranda kullanılabilir bir NVA dağıtmak için bkz: [yüksek oranda kullanılabilir bir NVA dağıtın][Deploy-NVA].

## <a name="next-steps"></a>Sonraki adımlar

Denetim düzlemi analizi testi kurulumun ve farklı sanal ağ/VLAN topolojinin görünümlerini öğrenmek için bkz: [denetim düzlemi analiz][Control-Analysis].

Testi kurulumun veri düzlemi analizi ve izleme özellikleri görünümleri Azure ağ için bkz: [veri düzlemi analiz][Data-Analysis].

Kaç ExpressRoute bağlantı hatları için bir ExpressRoute ağ geçidiyle bağlanabilir veya kaç ExpressRoute ağ geçidi bir ExpressRoute bağlantı hattına bağlayabilirsiniz öğrenmek ya da diğer ExpressRoute ölçek sınırları öğrenmek için bkz. [ExpressRoute SSS][ExR-FAQ]


<!--Image References-->
[1]: ./media/backend-interoperability/SpokeVNet_peering.png "uç VNet'ın sanal ağ eşlemesi"
[2]: ./media/backend-interoperability/HubVNet-peering.png "hub sanal ağın sanal ağ eşlemesi"
[3]: ./media/backend-interoperability/BranchVNet-VPNGW.png "dal VNet VPN GW yapılandırması"
[4]: ./media/backend-interoperability/ExR1.png "ExpressRoute1 yapılandırma"
[5]: ./media/backend-interoperability/ExR1-Hub-Connection.png "ExpressRoute1 Hub VNet ExR GW için bağlantı yapılandırması"
[6]: ./media/backend-interoperability/ExR2.png "ExpressRoute2 yapılandırma"
[7]: ./media/backend-interoperability/ExR2-Hub-Connection.png "ExpressRoute2 Hub VNet ExR GW için bağlantı yapılandırması"
[8]: ./media/backend-interoperability/ExR2-Remote-Connection.png "ExpressRoute2 uzak VNet ExR GW için bağlantı yapılandırması"

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




