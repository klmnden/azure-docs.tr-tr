---
title: "Şirket içi ağınızı Azure'a bağlanma | Microsoft Docs"
description: "Azure, Office 365 ve Dynamics CRM Online gibi Microsoft bulut hizmetlerine bağlanmak için farklı yöntemler kullanılabilir karşılaştırır ve açıklanmaktadır."
services: 
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: 
ms.assetid: 294d39ad-40e0-476a-a362-57911b1172b7
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2016
ms.author: cherylmc
ms.openlocfilehash: 37d83d3b6dea1763d85f2411816ba2fee4279100
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2018
---
# <a name="connecting-your-on-premises-network-to-azure"></a>Şirket içi ağınızı Azure'a bağlanılıyor
Microsoft, çeşitli bulut hizmetleri sağlar. Tüm hizmetlere ortak Internet üzerinden bağlanabilirsiniz, ancak bazı Internet üzerinden veya Microsoft'a doğrudan, özel bir bağlantı üzerinden sanal özel ağ (VPN) tüneli kullanarak Hizmetleri bağlanabilir. Bu makale, hangi bağlantı seçeneği tükettiğiniz Microsoft bulut hizmetlerine türlerine göre gereksinimlerinizi en iyi şekilde karşılayacağını belirlemek yardımcı olur. Çoğu kuruluş, aşağıda açıklanan birden çok bağlantı türlerini kullanın.

## <a name="connecting-over-the-public-internet"></a>Ortak Internet üzerinden bağlanma
Bu bağlantı türü aşağıda gösterildiği gibi Internet üzerinden doğrudan Microsoft bulut hizmetlerine erişim sağlar.

![Internet](./media/guidance-connecting-your-on-premises-network-to-azure/internet.png "Internet")

Bu bağlantı genellikle Microsoft bulut hizmetlerine bağlanmak için kullanılan ilk türüdür. Aşağıdaki tabloda, avantajları ve dezavantajları Bu bağlantı türünü listeler.

| **Avantajları** | **Dikkat edilmesi gerekenler** |
| --- | --- |
| Tüm istemci cihazları sahip olduğu sürece herhangi bir değişiklik, şirket içi ağınıza gerektiren tüm IP adresleri ve bağlantı noktalarını Internet üzerinde sınırsız erişimi. |Trafik, genellikle HTTPS kullanılarak şifrelenir ancak genel Internet geçeceğini bu yana aktarım sırasında geçirilebilir. |
| Ortak Internet'e açık tüm Microsoft bulut hizmetlerine bağlanabilir. |Beklenmeyen gecikme bağlantı Internet geçtiğinden. |
| Varolan bir Internet bağlantısı kullanır. | |
| Herhangi bir bağlantı aygıtı yönetimi gerektirmez. | |

Var olan Internet bağlantınız kullandığından bu bağlantısı hiçbir bağlantı veya bant genişliği maliyetlerini vardır.

## <a name="connecting-with-a-point-to-site-connection"></a>Noktadan siteye bağlantı bağlanma
Bu bağlantı türü aşağıda gösterildiği gibi Internet üzerinden bir Güvenli Yuva Tünel Protokolü (SSTP) tüneli yoluyla bazı Microsoft bulut hizmetlerine erişim sağlar.

![P2S](./media/guidance-connecting-your-on-premises-network-to-azure/p2s.png "noktadan siteye bağlantı")

Bağlantı, varolan bir Internet bağlantısı yapılır, ancak Azure VPN ağ geçidi kullanılmasını gerektirir. Aşağıdaki tabloda, avantajları ve dezavantajları Bu bağlantı türünü listeler.

| **Avantajları** | **Dikkat edilmesi gerekenler** |
| --- | --- |
| Tüm istemci cihazları sahip olduğu sürece herhangi bir değişiklik, şirket içi ağınıza gerektiren tüm IP adresleri ve bağlantı noktalarını Internet üzerinde sınırsız erişimi. |Trafiği IPSec kullanarak şifrelenir ancak genel Internet geçeceğini beri yoldaki geçirilebilir. |
| Varolan bir Internet bağlantısı kullanır. |Beklenmeyen gecikme bağlantı Internet geçtiğinden. |
| 200 Mb/sn ağ geçidi başına en fazla üretilen işi. |Oluşturma ve her aygıt, şirket içi ağınızdaki her aygıt için bağlanması için gereken her ağ geçidi arasında ayrı bağlantılarının yönetim gerektirir. |
| Bir Azure sanal ağ (VNet) bağlı Azure hizmetlerine bağlanmak için Azure sanal makineler ve Azure bulut Hizmetleri gibi kullanılabilir. |Bir Azure VPN ağ geçidi'nin en düşük devam eden yönetim gerektirir. |
| Microsoft Office 365 veya Dynamics CRM Online bağlanmak için kullanılamaz. | |
| Bir sanal ağa bağlı Azure hizmetlerine bağlanmak için kullanılamaz. | |

Daha fazla bilgi edinmek [VPN ağ geçidi](../vpn-gateway/vpn-gateway-about-vpngateways.md) hizmeti, kendi [fiyatlandırma](https://azure.microsoft.com/pricing/details/vpn-gateway)ve giden veri aktarımı [fiyatlandırma](https://azure.microsoft.com/pricing/details/data-transfers).

## <a name="connecting-with-a-site-to-site-connection"></a>Siteden siteye bağlantı bağlanma
Bu bağlantı türü aşağıda gösterildiği gibi Internet üzerinden bir IPSec tüneli üzerinden bazı Microsoft bulut hizmetlerine erişim sağlar.

![S2S](./media/guidance-connecting-your-on-premises-network-to-azure/s2s.png "siteden siteye bağlantı")

Bağlantı, varolan bir Internet bağlantısı yapılır, ancak ilişkili fiyatlandırma ve fiyatlandırma giden veri aktarımı ile bir Azure VPN ağ geçidi kullanılmasını gerektirir. Aşağıdaki tabloda, avantajları ve dezavantajları Bu bağlantı türünü listeler.

| **Avantajları** | **Dikkat edilmesi gerekenler** |
| --- | --- |
| Şirket içi ağınızdaki tüm cihazları; böylece her aygıt için ayrı ayrı bağlantıları yapılandırmak için gerekli bir sanal ağa bağlı Azure hizmetleriyle iletişim kurabilir. |Trafiği IPSec kullanarak şifrelenir ancak genel Internet geçeceğini beri yoldaki geçirilebilir. |
| Varolan bir Internet bağlantısı kullanır. |Beklenmeyen gecikme bağlantı Internet geçtiğinden. |
| Sanal makineler gibi bir sanal ağa bağlı Azure hizmetlerine ve bulut hizmetlerine bağlanmak için kullanılabilir. |Yapılandırmalı ve bir doğrulanan VPN cihaz * yönetmek şirket içi. |
| 200 Mb/sn ağ geçidi başına en fazla üretilen işi. |Bir Azure VPN ağ geçidi'nin en düşük devam eden yönetim gerektirir. |
| Kullanıcı tanımlı yollar veya sınır ağ geçidi Protokolü (BGP) kullanarak oturum ve inceleme için şirket içi ağ üzerinden bulut sanal makinelerden başlatılan giden trafik zorlayabilirsiniz **. |Microsoft Office 365 veya Dynamics CRM Online bağlanmak için kullanılamaz. |
| Bir sanal ağa bağlı Azure hizmetlerine bağlanmak için kullanılamaz. | |
| Şirket içi cihazları geri bağlantılar başlatmak Hizmetleri kullanıyorsanız ve güvenlik ilkelerini gerektiren, şirket içi ağınız ve Azure arasında bir güvenlik duvarı gerekebilir. | |

* * Bir listesini görüntülemek [doğrulanan VPN cihazları](../vpn-gateway/vpn-gateway-about-vpn-devices.md#devicetable).
* ** Kullanma hakkında daha fazla bilgi [kullanıcı tanımlı yollar](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) veya [BGP](../vpn-gateway/vpn-gateway-bgp-overview.md) şirket içi cihazına Azure Vnet yönlendirme zorlamak için.

## <a name="connecting-with-a-dedicated-private-connection"></a>Adanmış özel bağlantı ile bağlanma
Bu bağlantı türü tüm Microsoft bulut hizmetlerine erişim, adanmış özel bağlantı aşağıda gösterildiği gibi Internet erişmez Microsoft'a sağlar.

![ER](./media/guidance-connecting-your-on-premises-network-to-azure/er.png "ExpressRoute bağlantısı")

Bağlantı ExpressRoute hizmeti ve bağlantı sağlayıcı bağlantı kullanılmasını gerektirir. Aşağıdaki tabloda, avantajları ve dezavantajları Bu bağlantı türünü listeler.

| **Avantajları** | **Dikkat edilmesi gerekenler** |
| --- | --- |
| Hizmet sağlayıcısı üzerinden adanmış bir bağlantı kullanıldığından trafiği yoldaki genel Internet üzerinden ele geçirilebilecek olamaz. |Şirket içi yönlendirici yönetim gerektirir. |
| 10 Gb/sn expressroute bağlantı hattı ve 2 Gb/sn her ağ geçidi için en fazla başına en fazla bant genişliği. |Bağlantı sağlayıcı için ayrılmış bir bağlantı gerektiriyor. |
| Tahmin edilebilir gecikme Internet erişmez Microsoft'a ayrılmış bir bağlantı olduğundan. |Bir veya daha fazla Azure VPN ağ geçitlerinin en az devam eden Yönetim (bağlantı hattı sanal ağlara bağlanma) gerektirebilir. |
| İsterseniz trafiğini şifreleyebilir olsa şifreli iletişim gerektirmez. |Şirket içi cihazları geri bağlantılar başlatmak bulut Hizmetleri kullanıyorsanız, şirket içi ağınız ve Azure arasında bir güvenlik duvarı gerekebilir. |
| Doğrudan tüm Microsoft bulut hizmetleriyle, birkaç özel durum * bağlanabilir. |Şirket içi IP adreslerinin bir VNet.* * için bağlanamaz Hizmetleri için Microsoft veri merkezleri girme ağ adresi çevirisi (NAT) gerektirir |
| İnceleme ve BGP kullanarak günlüğe kaydetme için şirket içi ağ üzerinden bulut sanal makinelerden başlatılan giden trafik zorlayabilirsiniz. | |

* * Görüntülemek bir [hizmetlerin listesini](../expressroute/expressroute-faqs.md#supported-services) ExpressRoute ile kullanılamaz. Office 365'e bağlanmak için Azure aboneliğinizin onaylanması gerekir.  Bkz: [Office 365 için Azure ExpressRoute](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd?ui=en-US&rs=en-US&ad=US&fromAR=1) Ayrıntılar için makale.
* ** ExpressRoute hakkında daha fazla bilgi [NAT](../expressroute/expressroute-nat.md) gereksinimleri.

Daha fazla bilgi edinmek [ExpressRoute](../expressroute/expressroute-introduction.md), onun ilişkili [fiyatlandırma](https://azure.microsoft.com/pricing/details/expressroute), ve [bağlantı sağlayıcıları](../expressroute/expressroute-locations.md#locations).

## <a name="additional-considerations"></a>Diğer konular
* Yukarıdaki seçeneklerden VNet bağlantıları, ağ geçidi bağlantıları ve diğer ölçütlere destekleyebilir çeşitli maksimum sınırlara sahiptir. Azure gözden geçirmeniz önerilir [sınırları ağ](../azure-subscription-service-limits.md#networking-limits) herhangi birini kullanmak için bağlantı türleri etkisi anlamak için.
* Bir ağ geçidi bir siteden siteye VPN bağlantısını bir ExpressRoute ağ geçidi olarak aynı sanal ağa bağlanmak istiyorsanız, önemli kısıtlamalarıyla ilk tanımak. Bkz: [yapılandırma ExpressRoute ve siteden siteye bir arada var olabilen bağlantılar](../expressroute/expressroute-howto-coexist-resource-manager.md#limits-and-limitations) daha fazla ayrıntı için makale.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki kaynaklar, bu makalede ele alınan bağlantı türlerini uygulamak açıklanmaktadır.

* [Bir noktadan siteye bağlantı uygulama](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)
* [Siteden siteye bağlantı uygulama](guidance-hybrid-network-vpn.md)
* [Adanmış özel bağlantı uygulama](guidance-hybrid-network-expressroute.md)
* [Yüksek kullanılabilirlik için bir siteden siteye bağlantı ile adanmış özel bağlantı uygulama](guidance-hybrid-network-expressroute-vpn-failover.md)
