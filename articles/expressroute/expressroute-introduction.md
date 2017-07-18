---
title: "ExpressRoute’a Genel Bakış: Şirket içi ağınızı bir özel bağlantı üzerinden Azure’a genişletme | Microsoft Docs"
description: "Bu ExpressRoute’a Teknik Genel Bakış bölümünde, şirket içi ağınızı bir özel bağlantı üzerinden Azure’a genişletmek üzere ExpressRoute bağlantısının nasıl çalıştığı açıklanmaktadır."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
ms.assetid: fd95dcd5-df1d-41d6-85dd-e91d0091af05
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: cherylmc
ms.translationtype: Human Translation
ms.sourcegitcommit: bb794ba3b78881c967f0bb8687b1f70e5dd69c71
ms.openlocfilehash: d1e513933ea647a1afe9a4eb214bb9216d3bb20a
ms.contentlocale: tr-tr
ms.lasthandoff: 07/06/2017


---
# <a name="expressroute-overview"></a>ExpressRoute'a genel bakış
Microsoft Azure ExpressRoute, bağlantı sağlayıcı tarafından kolaylaştırılan özel bağlantı üzerinden şirket içi ağlarınızı Microsoft bulutuna genişletmenizi sağlar. ExpressRoute'u kullanarak Microsoft Azure, Office 365 ve Dynamics 365 gibi Microsoft bulut hizmetleriyle bağlantı kurabilirsiniz.

Ortak yerleşim tesisinde bağlantı sağlayıcısı üzerinden herhangi bir ağdan herhangi bir ağa (IP VP), noktadan noktaya Ethernet ağı veya sanal çapraz bağlantısından bağlantı olabilir.  ExpressRoute bağlantıları ortak İnternet üzerinden geçmemektedir. Bu, ExpressRoute bağlantılarına İnternet üzerindeki sıradan bağlantılara göre daha fazla güvenilirlik, yüksek hız, düşük gecikme ve normal bağlantılardan daha yüksek güvenlik sağlar. ExpressRoute kullanarak ağınızı Microsoft’a bağlama hakkında bilgi için bkz. [ExpressRoute bağlantı modelleri](expressroute-connectivity-models.md).

![](./media/expressroute-introduction/expressroute-connection-overview.png)

## <a name="key-benefits"></a>Önemli avantajlar

* Bağlantı sağlayıcı üzerinden şirket içi ağınız ve Microsoft Cloud arasındaki Katman 3 bağlantısı. Herhangi bir ağdan herhangi bir (IPVPN) ağa, noktadan noktaya Ethernet bağlantısı veya Ethernet değişimi aracığıyla sanal çapraz bağlantısı üzerinden bağlantı olabilir.
* Coğrafi bölgedeki tüm bölgeler arasında Microsoft bulut hizmetlerine erişim.
* ExpressRoute premium eklentisine sahip tüm bölgelere arasında Microsoft hizmetlerine genel bağlantı.
* Endüstri standardı protokolleri (BGP) üzerinden ağınız ve Microsoft arasında dinamik yönlendirme.
* Yüksek güvenilirlik için her eşleme konumunda yerleşik yedeklilik.
* Bağlantı çalışma süresi [SLA](https://azure.microsoft.com/support/legal/sla/).
* Skype Kurumsal için QoS.

Daha fazla bilgi için bkz. [ExpressRoute SSS](expressroute-faqs.md).

## <a name="features"></a>Özellikler

### <a name="layer-3-connectivity"></a>Katman 3 bağlantısı
Microsoft, şirket içi ağınız ile Azure ve Microsoft ortak adreslerinde bulunan örnekleriniz arasındaki yolları değiştirmek için endüstri standardı dinamik yönlendirme protokolünü (BGP) kullanır.  Farklı trafik profilleri için ağınızda çoklu BGP oturumları kuruyoruz. Daha fazla bilgi [ExpressRoute bağlantı hattı ve yönlendirme etki alanları](expressroute-circuit-peerings.md) makalesinde bulunabilir. 

### <a name="redundancy"></a>Yedeklilik
Her ExpressRoute bağlantı hattı, bağlantı sağlayıcısından veya ağınızın kenarından Microsoft Kurumsal kenar yönlendiricilerine (MSEEs) yapılan iki bağlantıdan oluşur. Microsoft, her MSEE için bir adet olmak üzere bağlantı sağlayıcısından veya sizin tarafınızdan ikili BGP bağlantısı gerektirir. Kendi tarafınızdaki yedekli cihazlara veya Ethernet bağlantı hattına dağıtmamayı seçebilirsiniz. Ancak, bağlantı sağlayıcılar bağlantılarınızın yedekli olarak Microsoft’a devredildiğinden emin olmak için yedekli cihazlar kullanır. Yedekli Layer 3 bağlantı yapılandırması [SLA](https://azure.microsoft.com/support/legal/sla/)’mızın geçerli olması için bir gereksinimdir.

### <a name="connectivity-to-microsoft-cloud-services"></a>Microsoft bulut hizmetlerine bağlantı
[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

ExpressRoute bağlantıları aşağıdaki hizmetlere erişim sağlar:

* Microsoft Azure hizmetleri
* Microsoft Office 365 hizmetleri
* Microsoft Dynamics 365

ExpressRoute üzerinde desteklenen hizmetlerin detaylı listesi için [ExpressRoute SSS](expressroute-faqs.md) sayfasını ziyaret edebilirsiniz.

### <a name="connectivity-to-all-regions-within-a-geopolitical-region"></a>Coğrafi konumdaki tüm bölgelere bağlantı
[Eşleme konumlarımızın](expressroute-locations.md) biriyle Microsoft’a bağlanabilir ve coğrafi konum içindeki tüm bölgelere erişebilirsiniz.  

Örneğin, Microsoft’a Amsterdam’da ExpressRoute aracılığıyla bağlandıysanız Kuzey Avrupa ve Batı Avrupa’da barındırılan tüm Microsoft bulut hizmetlerine erişiminiz olur. Jeopolitik bölgeler, ilişkili Microsoft bulut bölgeleri ve karşılık gelen ExpressRoute eşleme konumlarına genel bakış için [ExpressRoute iş ortakları ve eşleme konumları](expressroute-locations.md) makalesine bakın.

### <a name="global-connectivity-with-expressroute-premium-add-on"></a>ExpressRoute premium eklentisine ile genel bağlantı
Coğrafi sınırlar arasındaki bağlantıyı genişletmek için ExpressRoute premium eklenti özelliğini etkinleştirebilirsiniz. Örneğin, Microsoft’a Amsterdam’da ExpressRoute aracılığıyla bağlanırsanız dünyadaki tüm bölgelerde (ulusal bulutlar dışında) barındırılan tüm Microsoft bulut hizmetlerine erişiminiz olur. Güney Amerika ve Avustralya’da dağıtılan hizmetlere Kuzey ve Batı Avrupa bölgeleriyle aynı şekilde erişebilirsiniz.

### <a name="rich-connectivity-partner-ecosystem"></a>Zengin bağlantı iş ortağı ekosistemi
ExpressRoute sürekli büyüyen bağlantı sağlayıcıları ve SI ortakları ekosistemine sahiptir. En yeni bilgiler için [ExpressRoute sağlayıcıları ve konumları](expressroute-locations.md) makalesine başvurabilirsiniz.

### <a name="connectivity-to-national-clouds"></a>Ulusal bulutlara bağlantı
Microsoft, özel coğrafi bölgeler ve müşteri kesimine yönelik yalıtılmış bulut ortamlarını çalıştırır. Ulusal bulutlar ve sağlayıcılar listesi için [ExpressRoute sağlayıcıları ve konumları](expressroute-locations.md) sayfasına başvurun.

### <a name="bandwidth-options"></a>Bant genişliği seçenekleri
ExpressRoute bağlantı hattını çeşitli sayıda bant genişlikleriyle satın alabilirsiniz. Desteklenen bant genişlikleri aşağıda listelenmiştir. Sağladıkları desteklenen bant genişlikleri listesini belirlemek için bağlantı sağlayıcınıza danıştığınızdan emin olun.

* 50 Mbps
* 100 Mbps
* 200 Mbps
* 500 Mbps
* 1 Gbps
* 2 Gbps
* 5 Gbps
* 10 Gbps

### <a name="dynamic-scaling-of-bandwidth"></a>Bant genişliğini dinamik ölçeklendirme
Bağlantınızı kesmeden ExpressRoute bağlantı hattı bant genişliğini (en iyi çaba ilkesine göre) artırabilirsiniz. 

### <a name="flexible-billing-models"></a>Esnek faturalama modelleri
Size en uygun faturalama modelini seçin. Aşağıda listelenen faturalama modelleri listesinden seçin. Daha fazla bilgi için bkz. [ExpressRoute SSS](expressroute-faqs.md).

* **Sınırsız veri**. ExpressRoute bağlantı hattı aylık ücrete dayalı olarak ücretlendirilir ve tüm gelen, giden veri aktarımı ücretsiz olarak yer alır. 
* **Tarifeli veri**. ExpressRoute bağlantı hattı aylık ücrete dayalı olarak ücretlendirilir. Tüm gelen veri aktarımı ücretsizdir. Giden veri aktarımı, her GB veri aktarımı için ücretlendirilir. Veri aktarımı bölgelere göre farklılık gösterir.
* **ExpressRoute premium eklentisi**. ExpressRoute premium ExpressRoute bağlantı hattı üzerinde bir eklentidir. ExpressRoute premium eklentisi aşağıdaki yetenekleri sağlar: 
  * Azure ortak ve Azure özel eşleme için 4,000 yoldan 10,000 yola artırılmış yol sınırları.
  * Hizmetler için genel bağlantı. Herhangi bir bölgede oluşturulan ExpressRoute bağlantı hattı dünyada bulunan tüm diğer bölgelerdeki kaynaklara erişime sahip olur. Örneğin, Batı Avrupa’da oluşturulan bir sana ağa Silikon Vadisi’nde sağlanan bir ExpressRoute bağlantı hattı üzerinden erişilebilir.
  * Bağlantı hattı bağlantı genişliğine bağlı olarak 10’dan daha yüksek bir sınıra kadar artırılmış ExpressRoute bağlantı hattı başına VNet bağlantı sayısı.

## <a name="faq"></a>SSS

ExpressRoute hakkında sık sorulan sorular için bkz. [ExpressRoute SSS](expressroute-faqs.md).

## <a name="next-steps"></a>Sonraki adımlar

* [ExpressRoute bağlantı modelleri](expressroute-connectivity-models.md) hakkında bilgi edinin.
* ExpressRoute bağlantıları ve yönlendirme etki alanları hakkında bilgi edinin. Bkz. [ExpressRoute bağlantı hatları ve etki alanlarını yönlendirme](expressroute-circuit-peerings.md).
* Bir hizmet sağlayıcı bulun. Bkz. [ExpressRoute ortakları ve eşleme konumları](expressroute-locations.md).
* Tüm önkoşulların sağlandığından emin olun. Bkz. [ExpressRoute önkoşulları](expressroute-prerequisites.md).
* [Yönlendirme](expressroute-routing.md), [NAT](expressroute-nat.md) ve [QoS](expressroute-qos.md) için gereksinimlere bakın.
* ExpressRoute bağlantınızı yapılandırın.
  * [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-portal-resource-manager.md)
  * [ExpressRoute bağlantı hattı için eşleme yapılandırma](expressroute-howto-routing-portal-resource-manager.md)
  * [Bir sanal ağı ExpressRoute bağlantı hattına bağlama](expressroute-howto-linkvnet-portal-resource-manager.md)

