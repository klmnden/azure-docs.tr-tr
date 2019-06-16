---
title: Azure Sanal WAN’ye genel bakış | Microsoft Docs
description: Ölçeklenebilir sanal WAN otomatik dal için dal bağlantısı, kullanılabilir bölgeleri ve iş ortakları hakkında bilgi edinin.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: overview
ms.date: 06/11/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to understand what Virtual WAN is and if it is the right choice for my Azure network.
ms.openlocfilehash: 7ee6b2dd07a89de4f5347e82bde19990dbb6c995
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67077536"
---
# <a name="what-is-azure-virtual-wan"></a>Azure Sanal WAN nedir?

Azure sanal WAN en iyi duruma getirilmiş ve otomatik Dal bağlantısı için ve Azure üzerinden sağlayan bir ağ hizmetidir. Azure bölgeleri, dallarınız için bağlanmayı seçebilirsiniz hub'ları görür. Ayrıca dalları bağlanmak ve dal-VNet bağlantısı keyfini çıkarmak için Azure omurgası yararlanabilirsiniz. Bir Azure sanal WAN VPN bağlantısı otomasyonla destekleyen iş ortaklarının listesi sahibiz. Daha fazla bilgi için [sanal WAN iş ortakları ve konumları](virtual-wan-locations-partners.md) makalesi.

Azure sanal WAN siteden siteye VPN (genel kullanıma sunuldu) ExpressRoute (Önizleme), tek bir işletimsel arabirim uygulamasına noktadan siteye kullanıcı (Önizleme) VPN gibi birçok Azure bulut Bağlantı Hizmetleri getirmektedir. Sanal ağ bağlantıları kullanarak Azure Sanal Ağları için bağlantı kurulur.

![Sanal WAN diyagramı](./media/virtual-wan-about/virtualwan1.png)

Bu makalede, Azure sanal WAN ağ bağlantısı hızlı bir görünüm sağlar. Sanal WAN şu avantajları sunar:

* **Merkez ve uç çözümlerinde tümleşik bağlantısı:** Siteden siteye yapılandırması ve bir Azure hub'ı şirket içi siteler arasında bağlantısı otomatikleştirin.
* **Otomatik uç kurulumu ve yapılandırması:** Sanal ağlarınız ve iş yüklerini Azure hub'ı sorunsuz bir şekilde bağlanın.
* **Sezgisel sorunlarını giderme:** Azure'da uçtan uca akışını bakın ve ardından gerekli eylemleri için bu bilgileri kullanın.

## <a name="resources"></a>Sanal WAN kaynakları

Bir uçtan uca sanal WAN yapılandırmak için aşağıdaki kaynakları oluşturun:

* **virtualWAN:** VirtualWAN kaynak sanal bir katmana Azure ağınızın temsil eder ve birden fazla kaynak oluşan bir koleksiyondur. Sanal WAN’de bulunmasını istediğiniz tüm sanal hub'larınızın bağlantılarını içerir. Sanal WAN kaynakları birbirinden yalıtılmıştır ve ortak bir hub içeremezler. Sanal WAN’daki Sanal Hub'lar birbiriyle iletişim kurmaz. ExpressRoute (şu anda önizlemede) VPN yanı sıra VPN siteler arasında etkin trafiğe sahip siteler 'dalı için dal trafiğe izin' özelliği sağlar.

* **Hub:** Bir sanal hub'ı Microsoft tarafından yönetilen bir sanal ağ ' dir. Hub'da, şirket içi ağınızdan (vpnsite) gelen bağlantıyı etkinleştirmek için çeşitli hizmet uç noktaları bulunur. Hub, bir bölgedeki ağınızın merkezidir. Her Azure bölgesinde tek bir hub bulunabilir. Azure portalı kullanarak bir hub oluşturduğunuzda, bir sanal hub VNet’i ve vpngateway sanal hub'ı oluşturulur.

  Hub ağ geçidi, ExpressRoute ve VPN Gateway için kullandığınız sanal ağ geçidiyle aynı değildir. Örneğin, sanal WAN kullanırken, siteden siteye bağlantı, şirket içi sitenizden ağınıza doğrudan oluşturmayın. Bunun yerine, hub siteden siteye bağlantı oluşturun. Trafik her zaman hub ağ geçidi üzerinden gider. Başka bir deyişle, Sanal Ağlarınızın kendi sanal ağ geçitlerine gerek kalmaz. Sanal WAN, Sanal Ağlarınızın sanal hub ve sanal hub ağ geçidi aracılığıyla ölçeklemeden kolayca yararlanmasına olanak tanır.

* **Merkez sanal ağ bağlantısı:** Hub sanal ağa bağlantı kaynağı hub'a sorunsuz bir şekilde sanal ağınıza bağlanmak için kullanılır. Şu anda, yalnızca aynı hub bölgesi içinde yer alan sanal ağlara bağlanabilirsiniz.

* **Hub yol tablosu:**  Sanal hub rotasını oluşturabilir ve rota sanal hub yol tablosuna uygulayın. Sanal hub rota tablosuna birden fazla rota uygulayabilirsiniz.

**Sanal WAN ek kaynaklar**

  * **Site:** Bu kaynak siteden siteye bağlantılar için kullanılır. Site kaynak **vpnsite**. Şirket içi VPN cihazınız ile ayarlarını temsil eder. Sanal WAN iş ortağıyla çalıştığınızda, bu bilgileri otomatik olarak Azure’a aktarmak için yerleşik bir çözümünüz olur.

## <a name="connectivity"></a>Bağlantı

Sanal WAN üç tür bağlantı sağlar: siteden siteye, noktadan siteye (Önizleme) ve ExpressRoute (Önizleme).

### <a name="s2s"></a>Siteden siteye VPN bağlantıları

![Sanal WAN diyagramı](./media/virtual-wan-about/virtualwan.png)

Sanal WAN siteden siteye bağlantı oluşturduğunuzda, mevcut bir iş ortağı ile çalışabilirsiniz. Bir iş ortağı kullanmak istemiyorsanız, bağlantıyı el ile yapılandırabilirsiniz. Daha fazla bilgi için [sanal WAN kullanarak siteden siteye bağlantı oluşturma](virtual-wan-site-to-site-portal.md).

#### <a name="s2spartner"></a>Sanal WAN ortak iş akışı

Sanal WAN bir iş ortağıyla çalışmak, iş akışıdır:

1. Dal cihazı (VPN/SDWAN) denetleyicisinin, [Azure Hizmet Sorumlusu](../active-directory/develop/howto-create-service-principal-portal.md) kullanarak site merkezli bilgileri Azure’a aktarmak için kimliği doğrulanır.
2. Dal cihazı (VPN/SDWAN) denetleyicisi Azure bağlantı yapılandırmasını alır ve yerel cihazı güncelleştirir. Bu, şirket içi VPN cihazının yapılandırma indirme, düzenleme ve güncelleştirme işlemlerini otomatikleştirir.
3. Cihazda doğru Azure yapılandırması olduktan sonra, Azure WAN’a bir siteden siteye bağlantısı (iki etkin tünel) kurulur. Azure hem IKEv1’i hem de IKEv2'yi destekler. BGP isteğe bağlıdır.

#### <a name="partners"></a>İş ortakları için siteden siteye sanal WAN bağlantıları

Kullanılabilir iş ortakları ve konumları listesi için bkz. [sanal WAN iş ortakları ve konumları](virtual-wan-locations-partners.md) makalesi.

### <a name="p2s"></a>Noktadan siteye VPN bağlantıları (Önizleme)

Noktadan siteye (P2S) bağlantısı bir tek tek istemci bilgisayardan sanal hub'ınıza güvenli bir bağlantı oluşturmanıza olanak sağlar. P2S bağlantısı, istemci bilgisayardan başlatılarak oluşturulur. Bu çözüm, uzak bir konumdan (örneğin, evden veya bir konferanstan) bağlanmak isteyen uzaktan çalışan kişiler için kullanışlıdır. P2S VPN, ayrıca, bağlanması gereken yalnızca birkaç istemciniz olduğunda, S2S VPN yerine kullanabileceğiniz yararlı bir çözümüdür.

Bağlantı oluşturmak için bkz: [sanal WAN kullanarak noktadan siteye bağlantı oluşturma](virtual-wan-point-to-site-portal.md).

### <a name="er"></a>ExpressRoute bağlantıları (Önizleme)

ExpressRoute, şirket içi ağınızı bir özel bağlantı üzerinden Azure'a bağlanma sağlar. Bağlantı oluşturmak için bkz: [sanal WAN kullanarak bir ExpressRoute bağlantısı oluşturma](virtual-wan-expressroute-portal.md).

## <a name="locations"></a>konumları

Konum için bilgi [sanal WAN iş ortakları ve konumları](virtual-wan-locations-partners.md) makalesi.

## <a name="faq"></a>SSS

[!INCLUDE [Virtual WAN FAQ](../../includes/virtual-wan-faq-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Sanal WAN kullanarak siteden siteye bağlantı oluşturma](virtual-wan-site-to-site-portal.md)
