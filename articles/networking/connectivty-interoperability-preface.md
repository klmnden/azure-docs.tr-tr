---
title: 'ExpressRoute, siteden siteye VPN ve VNet eşlemesi - birlikte çalışabilirliği Test Kurulum: Azure arka uç bağlantısı özellikleri birlikte çalışabilirlik | Microsoft Docs'
description: Bu sayfada, ExpressRoute, siteden siteye VPN ve VNet eşlemesi özellikleri birlikte çalışabilirliği analiz etmek için kullanılan bir test Kurulum sağlar.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,vpn-gateway,virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: e859a0a3ac35a9d9f2dab579b7609192e599f90f
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49947352"
---
# <a name="interoperability-of-expressroute-site-to-site-vpn-and-vnet-peering---test-setup"></a>ExpressRoute, siteden siteye VPN ve VNet eşleme - test Kurulum birlikte çalışabilirliği
Bu makalede, şimdi nasıl farklı özellikler birbiriyle her iki denetim düzlemi ve veri düzlemi düzeyinde arası çalışan analiz etmek için kullanabileceğiniz bir test Kurulum belirleyin. Test Kurulumu görüştükten önce kısa bir süre bu farklı Azure ağ özellikleri anlamı uygulamasına göz atalım.

ExpressRoute: ExpressRoute kullanarak özel eşleme doğrudan şirket içi ağınızdaki özel IP alanlarından Azure sanal ağı dağıtımlarınıza bağlanabilirsiniz.  ExpressRoute kullanarak daha yüksek bant genişliği ve özel bağlantı elde edebilirsiniz. SLA'sı ile ExpressRoute bağlantısı sunan ExpressRoute ekonomik iş ortaklarının çoğu, vardır. ExpressRoute ve nasıl yapılandırılacağı hakkında daha fazla bilgi için bkz: [Expressroute'a giriş][ExpressRoute]

Siteden siteye VPN: Internet üzerinden veya ExpressRoute üzerinden şirket içi ağ Azure'a güvenli bir şekilde bağlanmak için siteden siteye (S2S) VPN seçeneği kullanılabilir. S2S VPN Azure'a bağlanmak için yapılandırma hakkında bilgi edinmek için [VPN ağ geçidi yapılandırma][VPN]

VNet eşlemesi: VNet eşlemesi sanal ağlar (Vnet'ler) arasında bağlantı kurmak kullanılabilir. VNet eşlemesi hakkında daha fazla bilgi için bkz: [VNet eşlemesi öğretici][VNet].

##<a name="test-setup"></a>Test Kurulumu

Test Kurulumu Aşağıdaki diyagramda gösterilmiştir.

[![1]][1]

Azure bölgesi 1 Hub Vnet'inde test Kurulum Merkezi taşıdır. Merkez sanal ağ gibi farklı ağlara bağlanır:

1.  Vnet-Vnet eşlemesi aracılığıyla uç için. Uç sanal ağı, uzaktan erişim iki ağ geçidi için Hub Vnet'inde sahiptir.
2.  Vnet'e siteden siteye VPN aracılığıyla dal. Bağlantı eBGP yolları için kullanır.
3.  Konum 1 için ağ yedekleme yolu olarak siteden siteye VPN bağlantısı ve birincil yolu olarak ExpressRoute özel eşlemesi aracılığıyla şirket içi. Bu belgede kalanında bu ExpressRoute devresinin ExpressRoute1 şimdi başvurun. Varsayılan olarak, ExpressRoute bağlantı hatları için yüksek kullanılabilirlik yedekli bağlantı sağlar. ExpressRoute1 üzerinde ikincil karşılıklı ikincil CE yönlendiricisinin arabirim devre dışı bırakıldı. Bu işlem, yukarıdaki diyagramda satır içi çift okun üzerinde kırmızı çizgi kullanarak belirtilir.
4.  Konum 2'ye ağ başka bir ExpressRoute özel eşlemesi aracılığıyla şirket içi. Bu belgenin kalan ikinci bu ExpressRoute devresinin ExpressRoute2 şimdi başvurun.
5.  ExpressRoute1, şirket içi Merkezi sanal ağ ve konum 1 ayrıca Azure bölge 2'de uzak bir sanal ağa bağlanır.

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

Test topolojisini yapılandırma ayrıntıları için bkz. [yapılandırma ayrıntılarını][Configuration].

Denetim düzlemi analizi testi kurulumun ve farklı sanal ağ/VLAN topolojinin görünümlerini öğrenmek için bkz: [denetim düzlemi analiz][Control-Analysis].

Testi kurulumun veri düzlemi analizi ve izleme özellikleri görünümleri Azure ağ için bkz: [veri düzlemi analiz][Data-Analysis].

Kaç ExpressRoute bağlantı hatları için bir ExpressRoute ağ geçidiyle bağlanabilir veya kaç ExpressRoute ağ geçidi bir ExpressRoute bağlantı hattına bağlayabilirsiniz öğrenmek ya da diğer ExpressRoute ölçek sınırları öğrenmek için bkz. [ExpressRoute SSS][ExR-FAQ]



<!--Image References-->
[1]: ./media/backend-interoperability/TestSetup.png "Test topolojisi"

<!--Link References-->
[ExpressRoute]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways
[VNet]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal
[Configuration]: https://docs.microsoft.com/azure/connectivty-interoperability-configuration
[Control-Analysis]:https://docs.microsoft.com/azure/connectivty-interoperability-control-plane
[Data-Analysis]: https://docs.microsoft.com/azure/connectivty-interoperability-data-plane
[ExR-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[S2S-Over-ExR]: https://docs.microsoft.com/azure/expressroute/site-to-site-vpn-over-microsoft-peering
[ExR-S2S-CoEx]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager
[Hub-n-Spoke]: https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke
[Deploy-NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha




