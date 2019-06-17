---
title: 'Birlikte çalışabilirlik Azure arka uç bağlantısı özellikleri: Test Kurulumu | Microsoft Docs'
description: Bu makalede, ExpressRoute, siteden siteye VPN ve sanal ağ eşlemesi ile Azure arasında birlikte çalışabilirlik analiz etmek için kullanabileceğiniz bir test ayarı açıklanır.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: 8be546c5dba4c6c694c8cef03a4bdd6005d68189
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60811115"
---
# <a name="interoperability-in-azure-back-end-connectivity-features-test-setup"></a>Birlikte çalışabilirlik Azure arka uç bağlantısı özellikleri: Test Kurulumu

Bu makalede, Azure ağ hizmetlerinin nasıl interoperate denetim düzlemi ve veri düzlemi düzeyindeki analiz etmek için kullanabileceğiniz bir test ayarı açıklanır. Kısaca Azure ağ bileşenlerine göz atalım:

-   **Azure ExpressRoute**: Azure sanal ağı dağıtımlarınıza şirket içi ağınızda özel IP alanları doğrudan bağlanmak Azure ExpressRoute özel eşlemesi'ı kullanın. ExpressRoute, yüksek bant genişliğini ve özel bir bağlantı elde etmenize yardımcı olabilir. ExpressRoute ekonomik iş ortaklarının çoğu, SLA'lar ile ExpressRoute bağlantı sunar. ExpressRoute hakkında daha fazla bilgi edinin ve ExpressRoute yapılandırma hakkında bilgi edinmek için bkz: [Expressroute'a giriş][ExpressRoute].
-   **Siteden siteye VPN**: Azure VPN ağ geçidi, internet üzerinden veya ExpressRoute kullanarak bir şirket içi ağı Azure'a güvenli bir şekilde bağlanmak için siteden siteye VPN kullanabilirsiniz. Azure'a bağlanmak için siteden siteye VPN yapılandırma konusunda bilgi için bkz: [VPN ağ geçidi yapılandırma][VPN].
-   **VNet eşlemesi**: Azure sanal ağındaki sanal ağlar arasında bağlantı kurmak için sanal ağ (VNet) eşlemesi'ı kullanın. VNet eşlemesi hakkında daha fazla bilgi için bkz: [VNet eşlemesi öğretici][VNet].

## <a name="test-setup"></a>Test Kurulumu

Test Kurulumu aşağıdaki şekilde gösterilmiştir:

[![1]][1]

Test Kurulumu güçlerinden merkez sanal ağa Azure bölge 1 ' dir. Merkez sanal ağa, aşağıdaki yollarla farklı ağlara bağlanır:

-   Merkez sanal ağa bir uç sanal ağı, VNet eşlemesi kullanarak bağlanır. Uç sanal ağı, merkez sanal ağa uzaktan erişim iki ağ geçidi için vardır.
-   Merkez sanal ağa dala sanal ağ, siteden siteye VPN kullanarak bağlanır. Bağlantı eBGP yolları için kullanır.
-   Merkez sanal ağa, birincil yolu olarak ExpressRoute özel eşlemesini kullanarak şirket içi konum 1 ağa bağlıdır. Yedekleme yolu olarak siteden siteye VPN bağlantısı kullanır. Bu makalenin kalanında ExpressRoute 1 Bu ExpressRoute bağlantı hattına diyoruz. Varsayılan olarak, yüksek kullanılabilirlik için yedekli bağlantı ExpressRoute devreleri sağlar. ExpressRoute 1'de, ikincil Microsoft Kurumsal kenar yönlendirici (MSEE) yüzler ikincil müşteri edge (CE) yönlendiricisinin arabirim devre dışı bırakıldı. Kırmızı bir çizgi üzerinden satır içi çift ok önceki şekilde devre dışı CE yönlendirici alt arabirimi temsil eder.
-   Merkez sanal ağa, başka bir ExpressRoute özel eşlemesini kullanarak şirket içi konum 2 ağa bağlıdır. Bu makalenin kalanında ExpressRoute 2 olarak bu ikinci ExpressRoute bağlantı hattına diyoruz.
-   ExpressRoute 1 ayrıca hem merkez sanal ağa hem de şirket içi konum 1 ağ Azure bölge 2'de uzak bir sanal ağa bağlanır.

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

Hakkında bilgi edinin [yapılandırma ayrıntılarını] [ Configuration] test topolojisi için.

Hakkında bilgi edinin [denetim düzlemi analiz] [ Control-Analysis] test kurulumu ve farklı sanal ağları veya VLAN'lar topolojide görünümlerini.

Hakkında bilgi edinin [veri düzlemi analiz] [ Data-Analysis] test kurulumu ve izleme görünümleriyle Özelliği Azure ağı.

Bkz: [ExpressRoute SSS] [ ExR-FAQ] için:
-   Bir ExpressRoute ağ geçidine bağlanabilir kaç ExpressRoute bağlantı hatları öğrenin.
-   Bir ExpressRoute bağlantı hattına bağlayabilirsiniz kaç ExpressRoute ağ geçitleri hakkında bilgi edinin.
-   Diğer ExpressRoute ölçek sınırları hakkında bilgi edinin.


<!--Image References-->
[1]: ./media/backend-interoperability/TestSetup.png "test topoloji diyagramı"

<!--Link References-->
[ExpressRoute]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways
[VNet]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal
[Configuration]: connectivty-interoperability-configuration.md
[Control-Analysis]: connectivty-interoperability-control-plane.md
[Data-Analysis]: connectivty-interoperability-data-plane.md
[ExR-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[S2S-Over-ExR]: https://docs.microsoft.com/azure/expressroute/site-to-site-vpn-over-microsoft-peering
[ExR-S2S-CoEx]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager
[Hub-n-Spoke]: https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke
[Deploy-NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha


