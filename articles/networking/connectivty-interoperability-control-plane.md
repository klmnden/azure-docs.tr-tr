---
title: 'ExpressRoute, siteden siteye VPN ve VNet eşlemesi - birlikte çalışabilirliği denetim düzlemi çözümleme: Azure arka uç bağlantısı özellikleri birlikte çalışabilirlik | Microsoft Docs'
description: Bu sayfa ExpressRoute, siteden siteye VPN ve VNet eşlemesi özellikleri birlikte çalışabilirliği analiz etmek için oluşturulan test kurulumunu denetim düzlemi analizini sağlar.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,vpn-gateway,virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: ee887da18b5666e61bc25365791b2e7dffb925e0
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49947358"
---
# <a name="interoperability-of-expressroute-site-to-site-vpn-and-vnet-peering---control-plane-analysis"></a>ExpressRoute, siteden siteye VPN ve VNet-eşlemesi - denetim düzlemi analizi birlikte çalışabilirliği

Bu makalede, testi kurulumun denetim düzlemi analizi Bahsedelim. Test Kurulumu gözden geçirmek istiyorsanız, bkz. [Test Kurulum][Setup]. Test Kurulumu yapılandırma ayrıntıları gözden geçirmek için bkz: [Test Kurulum Yapılandırması][Configuration].

Denetim düzlemi analizi, aslında bir topoloji ağ arasında alınıp verilen yollar inceler. Denetim düzlemi analizi Yardım nasıl farklı ağ topolojisini görüntüleme.

##<a name="hub-and-spoke-vnet-perspective"></a>Merkez ve uç sanal ağ perspektifi

Aşağıdaki diyagram, Hub sanal ağ ve (mavi renkle vurgulandığı) VNet uç perspektif ağdan gösterir. Diyagram ayrıca farklı ağ ve farklı ağ arasında alınıp verilen yollar Otonom sistem numarası (ASN) gösterir. 

[![1]][1]

ASN, sanal ağın ExpressRoute ağ geçidi ASN, Microsoft Kurumsal kenar yönlendiricilerine (msees yapılan) farklı olduğuna dikkat edin. Özel bir ASN (65515) ExpressRoute ağ geçidi kullanır ve Msee genel genel ASN'yi (12076) kullanın. ExpressRoute eşdüzey hizmet sağlama, MSEE eş olduğundan yapılandırdığınızda, eş ASN 12076 kullanın. Azure tarafında MSEE eBGP eşliğini ExpressRoute ağ geçidi ile oluşturur. Her ExpressRoute eşleme MSEE kuran çift eBGP eşliğini denetim düzlemi düzeyinde saydamdır. Bu nedenle, bir ExpressRoute yol tablosu görüntülendiğinde, alt ağın ön ekleri için alt ağın ExpressRoute GW ASN bakın. Bir örnek ExpressRoute rota tablosu ekran görüntüsü aşağıda gösterilmiştir: 

[![5]][5]

Azure içinde ASN'yi yalnızca bir eşleme açısından önemlidir. Varsayılan olarak 65515 ASN'si hem ExpressRoute ağ geçidi hem de VPN ağ geçidi bulunur.

##<a name="on-premises-location-1-and-remote-vnet-perspective-via-expressroute-1"></a>1. ExpressRoute aracılığıyla şirket içi konum-1 ve uzak sanal ağ perspektifi

Şirket içi konum-1 ve uzak sanal ağ, hem ExpressRoute 1 aracılığıyla merkez sanal ağa bağlı ve aynı açısından topoloji, bu nedenle paylaştıkları gösterildiği diyagram aşağıda.

[![2]][2]

##<a name="on-premises-location-1-and-branch-vnet-perspective-via-site-to-site-vpn"></a>Siteden siteye VPN aracılığıyla şirket içi konum-1 ve dal VNet perspektifi

Şirket içi konum-1 ve dal VNet hem de bağlı olduğunuz siteden siteye VPN bağlantıları aracılığıyla Hub sanal ağın VPN GW ve bu nedenle topoloji, aynı açısından gösterildiği şekilde paylaşırlar diyagram aşağıda.

[![3]][3]

##<a name="on-premises-location-2-perspective"></a>Şirket içi konum-2 perspektifi

Şirket içi konum-2 2. ExpressRoute özel eşlemesi aracılığıyla merkez sanal ağa bağlanır. 

[![4]][4]

## <a name="further-reading"></a>Daha fazla bilgi

### <a name="using-expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>ExpressRoute ve siteden siteye VPN bağlantısı dağıtımınızla kullanma

####  <a name="site-to-site-vpn-over-expressroute"></a>ExpressRoute üzerinden siteden siteye VPN

Özel olarak gizliliği, yürütmeyi, kimlik doğrulaması ve bütünlük ile şirket içi ağınız ve Azure Vnet'ler arasında veri alışverişi ExpressRoute Microsoft eşlemesi üzerinde siteden siteye VPN yapılandırılabilir. ExpressRoute Microsoft eşlemesi üzerinde siteden siteye IPSec VPN tüneli modunda yapılandırma hakkında daha fazla bilgi için bkz. [ExpressRoute Microsoft eşlemesi üzerinde siteden siteye VPN][S2S-Over-ExR]. 

Aktarım hızı, Microsoft eşlemesi üzerinden S2S VPN yapılandırma ana sınırlamasıdır. IPSec tüneli üzerinden aktarım hızı ile VPN ağ geçidi kapasitesi sınırlı. ExpressRoute üretilen kıyasla daha az VPN GW aktarım hızıdır. Böyle senaryolarda güvenli yüksek trafik ve diğer tüm trafiği için özel eşdüzey hizmet sağlama için IPSec tüneli kullanarak ExpressRoute bant genişliği kullanımını en iyi duruma getirme yardımcı olacaktır.

#### <a name="site-to-site-vpn-as-a-secure-failover-path-for-expressroute"></a>ExpressRoute için bir güvenli bir yük devretme yolu olarak siteden siteye VPN
ExpressRoute, yüksek kullanılabilirlik sağlamak için yedekli devre çiftinin sunulur. Farklı Azure bölgelerinde coğrafi olarak yedekli ExpressRoute bağlantı yapılandırabilirsiniz. ExpressRoute bağlantınızı için bir yük devretme yolu isterseniz de bizim test ayarında gibi belirli bir Azure bölgesi içinde siteden siteye VPN kullanarak bunu yapabilirsiniz. ExpressRoute ve S2S VPN üzerinden aynı ön eklerin tanıtılıp, Azure ExpressRoute üzerinden S2S VPN tercih eder. ExpressRoute ve S2S VPN arasında asimetrik yönlendirme önlemek için ağ yapılandırması de S2S VPN bağlantısı ExpressRoute belgelemeyi reciprocate şirket içi.

ExpressRoute ve siteden siteye VPN bir arada var olabilen bağlantılar yapılandırma hakkında daha fazla bilgi için bkz. [ExpressRoute ve siteden siteye birlikte kullanımı][ExR-S2S-CoEx].

### <a name="extending-backend-connectivity-to-spoke-vnets-and-branch-locations"></a>Arka uç bağlantısı için uç sanal ağları ve şube konumları genişletme

#### <a name="spoke-vnet-connectivity-using-vnet-peering"></a>Uç sanal ağ bağlantısı kullanarak VNet eşlemesi

Merkez ve uç Vnet mimarisi yaygın olarak kullanılır. Hub'ı, merkezi bir şirket içi ağınıza, uç sanal ağları arasında bağlantı noktası gören azure'daki bir sanal ağın (VNet) olan. Uçlar hub'la eş ve iş yüklerini yalıtmak için kullanılabilir sanal ağlar ' dir. Hub'ı bir ExpressRoute veya VPN bağlantısı aracılığıyla şirket içi veri merkeziniz arasındaki trafik akışı. Mimarisi hakkında daha fazla ayrıntı için bkz. [merkez ve uç mimarisi][Hub-n-Spoke]

VNet eşlemesi bir bölge içinde uç sanal ağları hub VNet ağ geçitleriniz (hem VPN ve ExpressRoute ağ geçitleri), uzak ağlarla iletişim kuracak kullanmasını sağlar.

#### <a name="branch-vnet-connectivity-using-site-to-site-vpn"></a>Siteden siteye VPN kullanarak sanal ağa bağlantı dal

İstediğiniz dalı (farklı bölgelerde) sanal ağlar ve şirket içi ağlarda hub sanal ağ birbirleriyle iletişim, yerel Azure VPN kullanarak siteden siteye VPN bağlantısı çözümüdür. Alternatif bir seçenek, hub'ı yönlendirme için bir NVA kullanmaktır.

VPN ağ geçitlerini yapılandırmak için bkz: [VPN ağ geçidi yapılandırma][VPN]. Yüksek oranda kullanılabilir bir NVA dağıtmak için bkz: [yüksek oranda kullanılabilir bir NVA dağıtın][Deploy-NVA].

## <a name="next-steps"></a>Sonraki adımlar

Testi kurulumun veri düzlemi analizi ve izleme özellikleri görünümleri Azure ağ için bkz: [veri düzlemi analiz][Data-Analysis].

Kaç ExpressRoute bağlantı hatları için bir ExpressRoute ağ geçidiyle bağlanabilir veya kaç ExpressRoute ağ geçidi bir ExpressRoute bağlantı hattına bağlayabilirsiniz öğrenmek ya da diğer ExpressRoute ölçek sınırları öğrenmek için bkz. [ExpressRoute SSS][ExR-FAQ]


<!--Image References-->
[1]: ./media/backend-interoperability/HubView.png "hub ve bağlı bileşen VNet perspektifi topolojisi"
[2]: ./media/backend-interoperability/Loc1ExRView.png "konum-1 ve topoloji 1 ExpressRoute aracılığıyla uzak sanal ağ perspektifi"
[3]: ./media/backend-interoperability/Loc1VPNView.png "konum-1 ve dal VNet perspektif aracılığıyla S2S VPN topolojinin"
[4]: ./media/backend-interoperability/Loc2View.png "topolojinin konum 2 perspektifi"
[5]: ./media/backend-interoperability/ExR1-RouteTable.png "ExpressRoute 1 RouteTable"

<!--Link References-->
[Setup]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-preface
[Configuration]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-config
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




