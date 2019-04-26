---
title: 'Birlikte çalışabilirlik Azure arka uç bağlantısı özellikleri: Denetim düzlemi analizi | Microsoft Docs'
description: Bu makalede, ExpressRoute, siteden siteye VPN ve sanal ağ eşlemesi ile Azure arasında birlikte çalışabilirlik analiz etmek için kullanabileceğiniz test kurulum denetim düzlemi analizini sağlar.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: 28ce4cfd0c62586510a6f7dfdeca8b552fe9638e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60425637"
---
# <a name="interoperability-in-azure-back-end-connectivity-features-control-plane-analysis"></a>Birlikte çalışabilirlik Azure arka uç bağlantısı özellikleri: Denetim düzlemi analizi

Bu makalede, Denetim düzlemi analizi [kurulumunu test][Setup]. Ayrıca inceleyebilirsiniz [test Kurulum Yapılandırması] [ Configuration] ve [veri düzlemi analiz] [ Data-Analysis] testi kurulumun.

Denetim düzlemi analizi, aslında bir topoloji ağ arasında alınıp verilen yollar inceler. Denetim düzlemi analizi nasıl farklı ağlarda anlamanıza yardımcı olabilir topolojiyi görüntülemek.

## <a name="hub-and-spoke-vnet-perspective"></a>Merkez ve uç sanal ağ perspektifi

Aşağıdaki şekilde, hub sanal ağ (VNet) ve bağlı bileşen (mavi renkle vurgulandığı) VNet perspektifinden ağ gösterilmektedir. Şekilde, ayrıca farklı ağlarda farklı ağ arasında alınıp verilen yollar ve Otonom sistem numarası (ASN) gösterilmektedir: 

[![1]][1]

Alt ağın Azure ExpressRoute ağ geçidi ASN'si ASN, Microsoft Kurumsal kenar yönlendiricilerine (Msee) farklıdır. Özel bir ASN bir ExpressRoute ağ geçidi kullanır (değerini **65515**) ve Msee genel ASN (değerini **12076**) genel. MSEE eş olduğundan ExpressRoute eşdüzey hizmet sağlama, yapılandırma, kullandığınız **12076** eşdüzey hizmet sağlayıcı ASN'si olarak. Azure tarafında MSEE eBGP eşliğini ExpressRoute ağ geçidi ile oluşturur. Her ExpressRoute eşleme MSEE kuran çift eBGP eşliğini denetim düzlemi düzeyinde saydamdır. Bu nedenle, bir ExpressRoute yol Tablosu'nu görüntüleyin, sanal ağın ön ekleri için alt ağın ExpressRoute ağ geçidi ASN bakın. 

Bir örnek ExpressRoute yol tablosu aşağıdaki şekilde gösterilmiştir: 

[![5]][5]

Azure içinde ASN, yalnızca bir eşleme açısından önemlidir. Varsayılan olarak, ASN'yi hem ExpressRoute ağ geçidi hem de Azure VPN ağ geçidi VPN ağ geçidi olduğundan **65515**.

## <a name="on-premises-location-1-and-the-remote-vnet-perspective-via-expressroute-1"></a>Şirket içi konum 1 ve ExpressRoute 1 aracılığıyla uzak sanal ağ perspektifi

Hem şirket içi konum 1 hem de uzak sanal ağ, merkez sanal ağa ExpressRoute 1 aracılığıyla bağlanır. Aşağıdaki diyagramda gösterildiği gibi topoloji, aynı açısından paylaştıkları:

[![2]][2]

## <a name="on-premises-location-1-and-the-branch-vnet-perspective-via-a-site-to-site-vpn"></a>Şirket içi konum 1 ve dalı bir siteden siteye VPN aracılığıyla VNet perspektifi

Hem şirket içi konum 1 hem de VNet dalı bir hub sanal ağın VPN gateway siteden siteye VPN bağlantısı aracılığıyla bağlanır. Aşağıdaki diyagramda gösterildiği gibi topoloji, aynı açısından paylaştıkları:

[![3]][3]

## <a name="on-premises-location-2-perspective"></a>Şirket içi konum 2 perspektifi

Konum 2 2. ExpressRoute özel eşlemesi aracılığıyla içinde merkez sanal ağa bağlı şirket içinde: 

[![4]][4]

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

Hakkında bilgi edinin [veri düzlemi analiz] [ Data-Analysis] test kurulumu ve izleme görünümleriyle Özelliği Azure ağı.

Bkz: [ExpressRoute SSS] [ ExR-FAQ] için:
-   Bir ExpressRoute ağ geçidine bağlanabilir kaç ExpressRoute bağlantı hatları öğrenin.
-   Bir ExpressRoute bağlantı hattına bağlayabilirsiniz kaç ExpressRoute ağ geçitleri hakkında bilgi edinin.
-   Diğer ExpressRoute ölçek sınırları hakkında bilgi edinin.


<!--Image References-->
[1]: ./media/backend-interoperability/HubView.png "merkez ve uç topolojisinin sanal ağ perspektifi"
[2]: ./media/backend-interoperability/Loc1ExRView.png "konum 1 ve topoloji 1 ExpressRoute aracılığıyla uzak sanal ağ perspektifi"
[3]: ./media/backend-interoperability/Loc1VPNView.png "konum 1 ve dalı bir siteden siteye VPN aracılığıyla topolojinin VNet perspektifi"
[4]: ./media/backend-interoperability/Loc2View.png "topolojinin konum 2 perspektifi"
[5]: ./media/backend-interoperability/ExR1-RouteTable.png "ExpressRoute 1 yol tablosu"

<!--Link References-->
[Setup]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-preface
[Configuration]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-configuration
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


