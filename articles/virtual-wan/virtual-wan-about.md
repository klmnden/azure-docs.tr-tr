---
title: Azure Sanal WAN’ye genel bakış | Microsoft Docs
description: Ölçeklenebilir sanal WAN otomatik dal için dal bağlantısı, kullanılabilir bölgeleri ve iş ortakları hakkında bilgi edinin.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: overview
ms.date: 03/04/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to understand what Virtual WAN is and if it is the right choice for my Azure network.
ms.openlocfilehash: 6a6fc9df2b102fd16bba03f26df4e24a1c946875
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57410001"
---
# <a name="what-is-azure-virtual-wan"></a>Azure Sanal WAN nedir?

Azure Sanal WAN, Azure aracılığıyla şubeden şubeye iyileştirilmiş ve otomatik bağlantı sağlayan bir ağ hizmetidir. Sanal WAN, Azure ile iletişim kurmak için dal cihazlarını bağlamanızı ve yapılandırmanızı sağlar. Bu, el ile veya bir sanal WAN iş ortağı aracılığıyla iş ortağı cihazları kullanarak yapılabilir. İş ortağı cihazları kullanarak, bağlantı ve yapılandırma yönetimini basitleştirme kullanımını kolaylaştırmak sağlar. Daha fazla bilgi için [konumlar ve iş ortakları](virtual-wan-locations-partners.md) makalesi. Ayrıca, Azure WAN yerleşik Panosu size zaman kazandırabilir yardımcı olabilir ve büyük ölçekli bağlantı görüntülemek için kolay bir yol sağlar, sorun giderme anlık Öngörüler sağlar.

![Sanal WAN diyagramı](./media/virtual-wan-about/virtualwan.png)

Bu makalede, Azure ve Azure dışı iş yüklerinizin ağ bağlantısına hızlı bir bakış sağlanır. Sanal WAN şu avantajları sunar:

* **Merkez ve uç çözümlerinde tümleşik bağlantısı:** Siteden siteye yapılandırması ve bir Azure hub'ı şirket içi siteler arasında bağlantısı otomatikleştirin.
* **Otomatik uç kurulumu ve yapılandırması:** Sanal ağlarınız ve iş yüklerini Azure hub'ı sorunsuz bir şekilde bağlanın.
* **Sezgisel sorunlarını giderme:** Azure'da uçtan uca akışını bakın ve gerekli eylemleri için bu bilgileri kullanın.

## <a name="s2s"></a>Siteden siteye bağlantılar

Sanal WAN kullanarak Siteden Siteye bağlantı oluşturmak için, [Sanal WAN iş ortağı](virtual-wan-locations-partners.md) üzerinden ilerleyebilir veya bağlantıyı el ile oluşturabilirsiniz.

### <a name="s2spartner"></a>Ortak iş akışı

Sanal WAN bir iş ortağıyla çalışmak, iş akışıdır:

1. Dal cihazı (VPN/SDWAN) denetleyicisinin, [Azure Hizmet Sorumlusu](../active-directory/develop/howto-create-service-principal-portal.md) kullanarak site merkezli bilgileri Azure’a aktarmak için kimliği doğrulanır.
2. Dal cihazı (VPN/SDWAN) denetleyicisi Azure bağlantı yapılandırmasını alır ve yerel cihazı güncelleştirir. Bu, şirket içi VPN cihazının yapılandırma indirme, düzenleme ve güncelleştirme işlemlerini otomatikleştirir.
3. Cihazda doğru Azure yapılandırması olduktan sonra, Azure WAN’a bir siteden siteye bağlantısı (iki etkin tünel) kurulur. Azure hem IKEv1’i hem de IKEv2'yi destekler. BGP isteğe bağlıdır.

Bir iş ortağı kullanmak istemiyorsanız, bağlantıyı el ile yapılandırın, bkz: [sanal WAN kullanarak siteden siteye bağlantı oluşturma](virtual-wan-site-to-site-portal.md).

## <a name="p2s"></a>Noktadan siteye bağlantıları (Önizleme)

Noktadan Siteye (P2S) bağlantı, ayrı bir istemci bilgisayardan sanal hub'ınıza güvenli bir bağlantı oluşturmanıza olanak tanır. P2S bağlantısı, istemci bilgisayardan başlatılarak oluşturulur. Bu çözüm, uzak bir konumdan (örneğin, evden veya bir konferanstan) bağlanmak isteyen uzaktan çalışan kişiler için kullanışlıdır. P2S VPN, ayrıca, bağlanması gereken yalnızca birkaç istemciniz olduğunda, S2S VPN yerine kullanabileceğiniz yararlı bir çözümüdür.

Bağlantıyı el ile oluşturmak için bkz. [Sanal WAN ile noktadan siteye bağlantı oluşturma](virtual-wan-point-to-site-portal.md).

## <a name="er"></a>ExpressRoute bağlantıları (Önizleme)

Bağlantıyı el ile oluşturmak için bkz. [Sanal WAN ile ExpressRoute bağlantısı oluşturma](virtual-wan-expressroute-portal.md).

## <a name="resources"></a>Sanal WAN kaynakları

Uçtan uca sanal WAN'yi yapılandırmak için şu kaynakları oluşturursunuz:

* **virtualWAN:** VirtualWAN kaynak sanal bir katmana Azure ağınızın temsil eder ve birden fazla kaynak oluşan bir koleksiyondur. Sanal WAN’de bulunmasını istediğiniz tüm sanal hub'larınızın bağlantılarını içerir. Sanal WAN kaynakları birbirinden yalıtılmıştır ve ortak bir hub içeremezler. Sanal WAN’daki Sanal Hub'lar birbiriyle iletişim kurmaz. 'Dallar arası trafiğe izin verme' özelliği, VPN’den ExpressRoute hizmeti etkinleştirilmiş sitelere gidenin yanı sıra VPN siteleri arasındaki trafiği de mümkün kılar. Azure Sanal WAN’daki ExpressRoute hizmetinin şu anda önizlemede olduğunu unutmayın.

* **Site:** Vpnsite bilinen sitesinde kaynak şirket içi VPN cihazınız ile ayarlarını temsil eder. Sanal WAN iş ortağıyla çalıştığınızda, bu bilgileri otomatik olarak Azure’a aktarmak için yerleşik bir çözümünüz olur.

* **Hub:** Bir sanal hub'ı Microsoft tarafından yönetilen bir sanal ağ ' dir. Hub'da, şirket içi ağınızdan (vpnsite) gelen bağlantıyı etkinleştirmek için çeşitli hizmet uç noktaları bulunur. Hub, bir bölgedeki ağınızın merkezidir. Her Azure bölgesinde tek bir hub bulunabilir. Azure portalı kullanarak bir hub oluşturduğunuzda, bir sanal hub VNet’i ve vpngateway sanal hub'ı oluşturulur.

  Hub ağ geçidi, ExpressRoute ve VPN Gateway için kullandığınız sanal ağ geçidiyle aynı değildir. Örneğin, Sanal WAN kullanırken şirket içi sitenizden doğrudan Sanal Ağınıza bir Siteden Siteye bağlantı oluşturmazsınız. Bunun yerine, hub'a Siteden Siteye bağlantısı oluşturursunuz. Trafik her zaman hub ağ geçidi üzerinden gider. Başka bir deyişle, Sanal Ağlarınızın kendi sanal ağ geçitlerine gerek kalmaz. Sanal WAN, Sanal Ağlarınızın sanal hub ve sanal hub ağ geçidi aracılığıyla ölçeklemeden kolayca yararlanmasına olanak tanır. 

* **Merkez sanal ağ bağlantısı:** Hub sanal ağa bağlantı kaynağı hub'a sorunsuz bir şekilde sanal ağınıza bağlanmak için kullanılır. Şu anda, yalnızca aynı hub bölgesi içinde yer alan sanal ağlara bağlanabilirsiniz.

* **Hub yol tablosu:**  Sanal hub rotasını oluşturabilir ve rota sanal hub yol tablosuna uygulayın. Sanal hub rota tablosuna birden fazla rota uygulayabilirsiniz.

## <a name="partner-region"></a>İş ortakları ve konumları

### <a name="partner"></a>İş ortakları

[!INCLUDE [partners](../../includes/virtual-wan-partners-include.md)]

### <a name="locations"></a>konumları

[!INCLUDE [regions](../../includes/virtual-wan-regions-include.md)]

## <a name="faq"></a>SSS

[!INCLUDE [Virtual WAN FAQ](../../includes/virtual-wan-faq-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Görünüm [sanal WAN iş ortakları ve konumları](virtual-wan-locations-partners.md) sayfası.
