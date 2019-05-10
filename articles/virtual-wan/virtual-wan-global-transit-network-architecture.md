---
title: Azure sanal WAN genel geçiş ağ mimarisi | Microsoft Docs
description: Sanal WAN'için genel geçiş ağ mimarisi hakkında bilgi edinin
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: article
ms.date: 05/06/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to understand global transit network architecture as it relates to Virtual WAN.
ms.openlocfilehash: 8cda617ca60a17fceaaa818480ff9bbaef46c3fd
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65414060"
---
# <a name="global-transit-network-architecture-and-virtual-wan"></a>Ağ mimarisine genel geçiş ve sanal WAN

Genel geçiş ağ mimarisi birleştirmek, bağlama ve bulut odaklı modern kurumsal BT kaplama denetlemek için kuruluş tarafından benimseniyor. Modern bulut odaklı kuruluşta ağ trafiği için HQ backhauled olması gerekmez. Ağ mimarisine genel aktarım tanıdık ağ kavramları ve hem Bulut ve bulut tabanlı mimariler için benzersiz olan yeni kavramları temel alır.

![architecture](./media/virtual-wan-global-transit-network-architecture/architecture2.png)

**Şekil 1: Sanal WAN ile genel aktarım ağı**

Modern kuruluşlar şirket içi ve bulut arasında hyper dağıtılmış uygulamalar, veriler ve kullanıcılar arasında yaygın bağlantı gerektirir. Azure sanal WAN genel geçiş ağ mimarisi, Global olarak dağıtılmış sanal ağlar, siteler, uygulamalar ve kullanıcılar kümesi arasında bulunabilen, herhangi bir ağdan herhangi bağlantısı sağlayarak sağlar. Azure sanal WAN olan Microsoft tarafından yönetilen bir hizmettir. Bu hizmet oluşan tüm ağ bileşenlerine barındırılan ve Microsoft tarafından yönetilir. Sanal WAN hakkında daha fazla bilgi için bkz: [sanal WAN genel bakış](virtual-wan-about.md) makalesi.

Azure sanal WAN mimaride, Azure bölgeleri Dallarınızı bağlanmak seçebileceğiniz hubs hizmet eder. Dalları bağlandıktan sonra dalı VNet oluşturmak için Azure omurgası yararlanabilir ve isteğe bağlı olarak, dal için dal bağlantısı.

Tek bir sanal WAN hub uçlar (dallar, sanal ağlar, kullanıcılar) en çok sayıda bölgede oluşturarak ve ardından hub'ına diğer bölgelerde olan uçlar bağlayarak sanal WAN kurabilirsiniz. Alternatif olarak, uçlar coğrafi olarak dağıtılmış, ayrıca örneği bölgesel hub'lar ve hub bağlantısı. Hub'ları aynı sanal WAN tüm parçası olan, ancak bunlar farklı bölgesel ilkeleri ile ilişkili olabilir.

## <a name="hub"></a>Merkez ve uç geçiş

Genel geçiş ağ mimarisi burada bulutta barındırılan Ağ 'hub' farklı türde 'uçlar' arasında dağıtılmış uç noktalar arasında karşılıklı bağlantı sağlayan bir Klasik merkez ve uç bağlantı modeli temel alır.
  
Bu modelde, bir bileşen olabilir:

* Sanal ağ (Vnet)
* Fiziksel dal site
* Uzak kullanıcı
* İnternet

![Merkez ve uç genel aktarım diyagramı](./media/virtual-wan-global-transit-network-architecture/architecture.png)

**Şekil 2: Merkez ve uç**

Şekil 2 kullanıcılar coğrafi olarak dağıtılmış, fiziksel sitelerdeki ve sanal ağlar bulutta barındırılan ağ hub'ı aracılığıyla burada bağlandığına genel ağ mantıksal görünümünü gösterir. Bu mimari, ağ uç noktalar arasında mantıksal bir atlamalı geçiş bağlantısını etkinleştirir. Uçlar, çeşitli Azure Ağ Hizmetleri tarafından ExpressRoute veya siteden siteye VPN gibi fiziksel dalları, VNet eşlemesi Vnet'ler ve uzak kullanıcılar için noktadan siteye VPN için hub'ına bağlanır.

## <a name="crossregion"></a>Bölgeler arası bağlantı

Bulut kaplama alanı, kuruluş için fiziksel ayak izini genellikle izler. Çoğu kuruluş, bunların fiziksel site ve kullanıcılara en yakın bölgeden buluta erişin. Genel ağ mimarisinin anahtar Sorumlular birini bölgeler arası bağlantı uç noktaları arasındaki ağ varlıklarını etkinleştirmektir. Bulut kaplama alanı, birden çok bölgede yayılabilir. Başka bir deyişle, başka bir dal ya da farklı bir bölgedeki bir sanal ağa trafiği bir bölgede buluta bağlı bir daldan ulaşabilirsiniz.

## <a name="any"></a>Herhangi bir ağdan herhangi bir bağlantı

Genel geçiş ağ mimarisi sağlar *herhangi bir ağdan herhangi bir bağlantı* merkezi ağa hub'ı aracılığıyla. Bu mimari ortadan kaldırır veya tam mesh veya oluşturmanız ve bakımını yapmanız için daha karmaşık kısmi kafes bağlantı modelleri gereksinimini azaltır. Ayrıca, hub ve bağlı bileşen kafes ağları karşılaştırması yönlendirme denetiminde yapılandırmak ve korumak daha kolay olur.

Herhangi bir ağdan herhangi bir bağlantı, kuruluş Global olarak dağıtılmış kullanıcılar, dallar, veri merkezleri, sanal ağlar ve geçiş hub'ı aracılığıyla birbirine bağlamak için uygulamalar ile genel bir mimarisi bağlamında izin verir. Geçiş hub'ı genel aktarım sistemi olarak görev yapar.

![trafik yolları](./media/virtual-wan-global-transit-network-architecture/trafficpath.png)

**Şekil 3: Sanal WAN trafiğini yolları**

Azure sanal WAN, aşağıdaki genel aktarım bağlantısı yolları destekler. Şekil 3'e parantez içinde harf eşleyin.

* Dal-VNet (a)  
* Dal dal (b)
* Uzak kullanıcı-VNet (c)
* Uzak kullanıcı-için-dal (d)
* VNet-VNet kullanarak VNet eşlemesi (e)
* ExpressRoute küresel erişim 

### <a name="branchvnet"></a>Dal-VNet

Dal ağdan Azure sanal WAN tarafından desteklenen birincil yoludur. Bu yolu, Azure sanal ağlarında dağıtılan Azure IAAS kurumsal iş yükleri için dalları bağlanmanızı sağlar. Dallar, ExpressRoute veya siteden siteye VPN aracılığıyla sanal WAN bağlanabilir. Sanal WAN hub'larına VNet bağlantıları aracılığıyla bağlı sanal ağlar için trafiği transits.

### <a name="branchbranch"></a>Dal dal

Dallar, ExpressRoute bağlantı hatları ve/veya siteden siteye VPN bağlantıları kullanarak bir Azure sanal WAN hub'ına bağlanabilir. Dallar dala en yakın bölgede sanal WAN hub'ına bağlanabilir.

Bu seçenek, kuruluşlar dalları bağlanmak için Azure omurgası yararlanın sağlar. Ancak, bu özellik kullanılabilir olsa bile özel WAN kullanarak karşılaştırması dalları Azure sanal WAN bağlanma avantajları tartmanız gerekir.

### <a name="usertovnet"></a>Uzak kullanıcı VNet

Noktadan siteye bağlantıları bir uzak kullanıcı istemciden sanal WAN'ı kullanarak azure'a doğrudan ve güvenli uzaktan erişim etkinleştirebilirsiniz. Kurumsal uzak kullanıcılar Kurumsal VPN kullanarak buluta hairpin artık gerekmez.

### <a name="usertobranch"></a>Uzak kullanıcı dal

Uzak kullanıcı dal yolu, bulut üzerinden'i yapılandırmamız Azure erişimi şirket içi iş yüklerini ve uygulamaları bir noktadan siteye bağlantısı kullanarak uzak kullanıcıların olanak tanır. Bu yol, uzak kullanıcıların, hem de dağıtılan Azure hem de şirket içi iş yükleri için esnekliği sunar. Kuruluşlar, Azure sanal WAN merkezi bulut tabanlı güvenli uzaktan erişim hizmeti etkinleştirebilirsiniz.

### <a name="vnetvnet"></a>VNet eşlemesi kullanarak VNet-VNet geçiş

Vnet'leri birbirine birden çok sanal ağlarda uygulanan çok katmanlı uygulamaları desteklemek için bağlamak için VNet eşlemesi'ni kullanın. Azure sanal WAN üzerinden bir VNet-VNet geçiş senaryosu, şu anda desteklenmemektedir, ancak Azure yol haritası kapsamındadır. VNet eşlemesi ile sanal ağları bağlama birbiriyle bağlanması gereken sanal ağlar için önerilen çözümüdür. VNet eşlemesi hakkında daha fazla bilgi için bkz. [VNet eşlemeye genel bakış](../virtual-network/virtual-network-peering-overview.md).

### <a name="globalreach"></a>ExpressRoute küresel erişim

ExpressRoute için Microsoft Cloud, şirket içi ağlara bağlanmak için özel ve esnek bir yoludur. ExpressRoute Global erişim ExpressRoute için bir eklenti özelliği olan. Global erişim ile birlikte özel bir ağ, şirket içi ağlar arasında yapmak için ExpressRoute bağlantı hattına bağlayabilirsiniz. Azure sanal ExpressRoute kullanarak WAN için bağlı olan dalların ExpressRoute Global birbirleri ile iletişim kurmak erişim gerektirir.

Bu modelde, ExpressRoute kullanarak sanal WAN hub'a bağlı her dal ağdan dal yolu kullanarak sanal ağlara bağlanabilirsiniz. Dal dal trafik, ExpressRoute Global erişim daha iyi bir yolu Azure WAN sağladığından hub geçiş olmaz.

## <a name="security"></a>Güvenlik ve ilke denetimi

Sanal ağ hub'ı, bağlantılar ve potansiyel olarak tüm trafik görür. Bu konak merkezi ağ işlevleri ve böyle bir bulut yönlendirme, Ağ İlkesi ve güvenlik ve İnternet'e erişim denetimi gibi bir konuma olabilir.

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN kullanarak bir bağlantı oluşturun.

* [Sanal WAN kullanarak siteden siteye bağlantıları](virtual-wan-site-to-site-portal.md)
* [Sanal WAN kullanarak noktadan siteye bağlantıları](virtual-wan-point-to-site-portal.md)
* [Sanal WAN kullanan ExpressRoute bağlantıları](virtual-wan-expressroute-portal.md)
