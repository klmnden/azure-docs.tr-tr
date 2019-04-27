---
title: Şirket içi ağınızı Azure'a bağlama | Microsoft Docs
description: Açıklar ve Microsoft Azure, Office 365 ve Dynamics CRM Online gibi bulut hizmetlerine bağlanmak için farklı yöntemler kullanılabilir karşılaştırır.
services: ''
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: ''
ms.assetid: 294d39ad-40e0-476a-a362-57911b1172b7
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2016
ms.author: cherylmc
ms.openlocfilehash: 9361c0b2ee84d7459bd432f5a8ce13fc5fd85ef8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60583545"
---
# <a name="connecting-your-on-premises-network-to-azure"></a>Şirket içi ağınızı Azure'a bağlama
Microsoft, birden fazla bulut hizmetleri sağlar. Tüm hizmetler için genel Internet üzerinden bağlanabilirsiniz buna karşın bazı hizmetler Internet üzerinden veya Microsoft'a doğrudan, özel bir bağlantı üzerinden bir sanal özel ağ (VPN) tüneli kullanarak bağlanabilirsiniz. Bu makalede, hangi bağlantı seçeneği en iyi tükettiğiniz Microsoft bulut Hizmetleri türlerine göre gereksinimlerinizi karşıladığını belirlemenize yardımcı olur. Çoğu kuruluş, aşağıda açıklanan birden çok bağlantı türlerini kullanın.

## <a name="connecting-over-the-public-internet"></a>Genel Internet üzerinden bağlanma
Bu bağlantı türü aşağıda gösterildiği gibi doğrudan Internet üzerinden Microsoft bulut hizmetlerine erişim sağlar.

![Internet](./media/guidance-connecting-your-on-premises-network-to-azure/internet.png "Internet")

Bu bağlantı, genellikle Microsoft bulut hizmetlerine bağlamak için kullanılan ilk türü değil. Aşağıdaki tabloda, Artıları ve eksileri Bu bağlantı türü listeler.

| **Avantajlar** | **Dikkat edilmesi gerekenler** |
| --- | --- |
| Tüm istemci cihazları olduğu sürece şirket içi ağınıza herhangi bir değişiklik gerektiren tüm IP adresleri ve bağlantı noktalarını Internet'teki sınırsız erişim. |Trafik, genellikle HTTPS kullanılarak şifrelenir, ancak genel Internet trafiğiyle beri Aktarımdaki geçirilebilir. |
| Genel Internet'e açık olan tüm Microsoft bulut hizmetlerine bağlanabilirsiniz. |Beklenmeyen gecikme nedeniyle bağlantı, Internet'ten gönderilir. |
| Var olan Internet bağlantınız kullanır. | |
| Herhangi bir bağlantı aygıtı yönetimi gerektirmez. | |

Var olan Internet bağlantınız kullandığından bu bağlantının bağlantı ya da bant genişliği maliyetlerini sahiptir.

## <a name="connecting-with-a-point-to-site-connection"></a>Noktadan siteye bağlantı ile bağlanma
Bu bağlantı türü aşağıda gösterildiği gibi Internet üzerinden bir Güvenli Yuva Tünel Protokolü (SSTP) tüneli yoluyla bazı Microsoft bulut hizmetlerine erişim sağlar.

![P2S](./media/guidance-connecting-your-on-premises-network-to-azure/p2s.png "noktadan siteye bağlantı")

Bağlantı, var olan Internet bağlantınız üzerinden yapılır, ancak bir Azure VPN ağ geçidinin kullanılmasını gerektirir. Aşağıdaki tabloda, Artıları ve eksileri Bu bağlantı türü listeler.

| **Avantajlar** | **Dikkat edilmesi gerekenler** |
| --- | --- |
| Tüm istemci cihazları olduğu sürece şirket içi ağınıza herhangi bir değişiklik gerektiren tüm IP adresleri ve bağlantı noktalarını Internet'teki sınırsız erişim. |Trafik, IPSec kullanarak şifrelenir, ancak genel Internet trafiğiyle olduğundan Aktarımdaki geçirilebilir. |
| Var olan Internet bağlantınız kullanır. |Beklenmeyen gecikme nedeniyle bağlantı, Internet'ten gönderilir. |
| 200 Mb/sn ağ geçidi başına en fazla aktarım hızı. |Oluşturulması ve her bir cihaza bağlanmak için gereken her ağ geçidi ile şirket içi ağınızdaki her bir cihaz arasında ayrı bağlantılar Yönetimi gerektirir. |
| Bir Azure sanal ağları (VNet) bağlı Azure hizmetlerine bağlanmak için Azure sanal makineler ve Azure bulut Hizmetleri gibi kullanılabilir. |Bir Azure VPN ağ geçidinin en az devam eden yönetim gerektirir. |
| Microsoft Office 365 veya Dynamics CRM Online hizmetine bağlanmak için kullanılamaz. | |
| Bir sanal ağa bağlı Azure hizmetlerine bağlanmak için kullanılamaz. | |

Daha fazla bilgi edinin [VPN ağ geçidi](../vpn-gateway/vpn-gateway-about-vpngateways.md) hizmeti, kendi [fiyatlandırma](https://azure.microsoft.com/pricing/details/vpn-gateway)ve giden veri aktarımı [fiyatlandırma](https://azure.microsoft.com/pricing/details/data-transfers).

## <a name="connecting-with-a-site-to-site-connection"></a>Bir siteden siteye bağlantı ile bağlanma
Bu bağlantı türü aşağıda gösterildiği gibi Internet üzerinden bir IPSec tüneli üzerinden bazı Microsoft bulut hizmetlerine erişim sağlar.

![S2S](./media/guidance-connecting-your-on-premises-network-to-azure/s2s.png "siteden siteye bağlantı")

Bağlantı, var olan Internet bağlantınız üzerinden yapılır, ancak ilişkili fiyatlandırma ve giden veri aktarımı fiyatlandırması ile bir Azure VPN ağ geçidinin kullanılmasını gerektirir. Aşağıdaki tabloda, Artıları ve eksileri Bu bağlantı türü listeler.

| **Avantajlar** | **Dikkat edilmesi gerekenler** |
| --- | --- |
| Tüm cihazlar şirket içi ağınızdaki her cihaz için ayrı ayrı bağlantıları yapılandırmak gerekmez. Bu nedenle, bir sanal ağa bağlı Azure hizmetleriyle iletişim kurabilir. |Trafik, IPSec kullanarak şifrelenir, ancak genel Internet trafiğiyle olduğundan Aktarımdaki geçirilebilir. |
| Var olan Internet bağlantınız kullanır. |Beklenmeyen gecikme nedeniyle bağlantı, Internet'ten gönderilir. |
| Sanal makineler gibi bir sanal ağa bağlı Azure Hizmetleri ve bulut hizmetlerine bağlanmak için kullanılabilir. |Yapılandırma ve yönetme bir doğrulanmış VPN cihaz * şirket içi. |
| 200 Mb/sn ağ geçidi başına en fazla aktarım hızı. |Bir Azure VPN ağ geçidinin en az devam eden yönetim gerektirir. |
| Giden trafiği, inceleme ve günlüğe kaydetme kullanıcı tanımlı yollar veya sınır ağ geçidi Protokolü (BGP) kullanarak şirket içi ağ üzerinden sanal makinelerden bulut tarafından başlatılan zorlayabilirsiniz **. |Microsoft Office 365 veya Dynamics CRM Online hizmetine bağlanmak için kullanılamaz. |
| Bir sanal ağa bağlı Azure hizmetlerine bağlanmak için kullanılamaz. | |
| Şirket içi cihazlar için geri yönelik bağlantıları başlatmasını Hizmetleri kullanıyorsanız ve güvenlik ilkelerinize gerektiren şirket içi ağınız ve Azure arasında bir güvenlik duvarı gerekebilir. | |

* * Bir listesini görüntülemek [doğrulanmış VPN cihazları](../vpn-gateway/vpn-gateway-about-vpn-devices.md#devicetable).
* ** Kullanma hakkında daha fazla bilgi [kullanıcı tanımlı yollar](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) veya [BGP](../vpn-gateway/vpn-gateway-bgp-overview.md) ait Azure sanal ağlar için bir şirket içi cihaz yönlendirme zorlamak için.

## <a name="connecting-with-a-dedicated-private-connection"></a>Adanmış özel bağlantı ile bağlanma
Bu bağlantı türü tüm Microsoft bulut hizmetlerine erişim, bir adanmış özel bağlantı üzerinden aşağıda gösterildiği gibi Internet'i dolaşmaz Microsoft'a sağlar.

![ER](./media/guidance-connecting-your-on-premises-network-to-azure/er.png "ExpressRoute bağlantısı")

Bağlantı, ExpressRoute hizmetini ve bağlantı sağlayıcısından bağlantı kullanımını gerektirir. Aşağıdaki tabloda, Artıları ve eksileri Bu bağlantı türü listeler.

| **Avantajlar** | **Dikkat edilmesi gerekenler** |
| --- | --- |
| Bir hizmet sağlayıcısı aracılığıyla özel bir bağlantı kullanıldığından trafik yolda genel Internet üzerinden müdahale edilebilir olamaz. |Şirket içi yönlendiriciyi Yönetimi gerektirir. |
| ExpressRoute bağlantı hattı ve her ağ geçidi için 2 Gb/s aktarım başına 10 Gb/s bant genişliği. |Bağlantı sağlayıcı için ayrılmış bir bağlantı gerektirir. |
| Tahmin edilebilir gecikme süresi, Internet'i dolaşmaz Microsoft'a adanmış bir bağlantı olduğundan. |Bir veya daha fazla Azure VPN ağ geçitlerini en az devam eden Yönetim (bağlantı hattı için sanal ağlar bağlanılıyorsa) gerektirebilir. |
| Kullanarak trafiği şifrelemenize ancak şifreli iletişim gerektirmez. |Şirket içi cihazlar için geri yönelik bağlantıları başlatmasını bulut hizmetlerini kullanıyorsanız, şirket içi ağınız ve Azure arasında bir güvenlik duvarı gerekebilir. |
| Bazı özel durumlar * ile tüm Microsoft bulut hizmetlerine doğrudan bağlanabilirsiniz. |Şirket içi IP adreslerinin bir VNet.* * için bağlanamaz Hizmetleri için Microsoft veri merkezleri girme ağ adresi çevirisi (NAT) gerektirir. |
| Giden trafiği inceleme ve BGP kullanarak günlüğe kaydetme için şirket içi ağ üzerinden bulut sanal makinelerden başlatılan zorlayabilirsiniz. | |

* * Görüntülemek bir [hizmetlerin listesi](../expressroute/expressroute-faqs.md#supported-services) ExpressRoute ile kullanılamaz. Azure aboneliğinizi Office 365'e bağlanmak için onaylanmış olması gerekir.  Bkz: [Office 365 için Azure ExpressRoute](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd?ui=en-US&rs=en-US&ad=US&fromAR=1) makale Ayrıntılar için.
* ** ExpressRoute hakkında daha fazla bilgi [NAT](../expressroute/expressroute-nat.md) gereksinimleri.

Daha fazla bilgi edinin [ExpressRoute](../expressroute/expressroute-introduction.md), ilişkili [fiyatlandırma](https://azure.microsoft.com/pricing/details/expressroute), ve [bağlantı sağlayıcıları](../expressroute/expressroute-locations.md#locations).

## <a name="additional-considerations"></a>Diğer konular
* Yukarıdaki seçeneklerden, sanal ağ bağlantıları, ağ geçidi bağlantıları ve diğer ölçütlere destekleyebilir çeşitli maksimum sınırlara sahiptir. Azure gözden geçirmeniz önerilir [sınırları ağ](../azure-subscription-service-limits.md#networking-limits) anlamak bunların hiçbirine kullanmak için seçtiğiniz bağlantı türlerini etkiler.
* Bir siteden siteye VPN bağlantısını bir ağ geçidi bir ExpressRoute ağ geçidiyle aynı sanal ağa bağlanmak planlıyorsanız, önemli kısıtlamaları ile ilk planladığınızdan. Bkz: [yapılandırma ExpressRoute ve siteden siteye bir arada var olabilen bağlantılar](../expressroute/expressroute-howto-coexist-resource-manager.md#limits-and-limitations) makale daha fazla ayrıntı için.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki kaynaklar, bu makalede ele alınan bağlantı türleri uygulayan işlemleri açıklanmaktadır.

* [Noktadan siteye bağlantı uygulayın](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)
* [Uygulama bir siteden siteye bağlantı](guidance-hybrid-network-vpn.md)
* [Adanmış özel bağlantı uygulayın](guidance-hybrid-network-expressroute.md)
* [Yüksek kullanılabilirlik için adanmış özel bağlantı siteden siteye bağlantı ile uygulama](guidance-hybrid-network-expressroute-vpn-failover.md)
