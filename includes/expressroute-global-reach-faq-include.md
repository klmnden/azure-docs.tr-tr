---
title: include dosyası
description: include dosyası
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 03/19/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 317a480c13c5c6e00653fd61878a379df3f65ac4
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "65753931"
---
### <a name="what-is-expressroute-global-reach"></a>ExpressRoute Global erişim nedir?

ExpressRoute Global erişim global Microsoft ağı ExpressRoute hizmeti aracılığıyla şirket içi ağlarınızı bağlanan bir Azure hizmetidir. Bir özel veri merkezine bağlı Silikon ExpressRoute ve ExpressRoute ile ExpressRoute Global erişim, Dallas, bağlı Teksas başka bir özel veri merkezinde California varsa, örneğin, özel veri merkezlerinizdeki birlikte bağlanabilirsiniz iki ExpressRoute bağlantıları ile geçici veri merkezi trafik Microsoft omurga gerçekleşmedikçe.

### <a name="how-do-i-enable-or-disable-expressroute-global-reach"></a>Nasıl etkinleştirmek veya devre dışı ExpressRoute Global erişim?

Birlikte, ExpressRoute devreleri bağlanarak ExpressRoute Global erişim sağlar. Özelliği, devre kesme tarafından devre dışı. Bkz: [yapılandırma](../articles/expressroute/expressroute-howto-set-global-reach.md).

### <a name="do-i-need-expressroute-premium-for-expressroute-global-reach"></a>ExpressRoute Premium ExpressRoute Global erişim gerekiyor mu?

Birbirine bağlamak, ExpressRoute Premium, ExpressRoute devreleri aynı jeopolitik bölgede ise gerekmez. Farklı coğrafi bölgelerde iki ExpressRoute devreniz varsa bunlar arasında bağlantıyı etkinleştirmek için her iki bağlantı hatları için ExpressRoute Premium gerekir. 

### <a name="how-will-i-be-charged-for-expressroute-global-reach"></a>Nasıl ExpressRoute genel ulaşmak için ücret öder miyim?

ExpressRoute, Microsoft bulut Hizmetleri için şirket içi ağınızdan bağlantısı sağlar. ExpressRoute Global erişim global Microsoft ağının yararlanarak, var olan ExpressRoute bağlantı hatları ile kendi şirket içi ağlar arasında bağlantı sağlar. ExpressRoute Global erişim mevcut bir ExpressRoute hizmetinden ayrı olarak faturalandırılır. Her ExpressRoute bağlantı hattı üzerinde bu özelliği etkinleştirmek için bir ek ücreti yoktur. ExpressRoute Global erişim tarafından etkin, şirket içi ağlar arasındaki trafik, bir giriş fiyatı üzerinden hedef ve kaynak bir çıkış fiyatı için faturalandırılırsınız. Ücretler devreler bulunduğu olduğu bölgeye göre belirlenir.

### <a name="where-is-expressroute-global-reach-supported"></a>Burada ExpressRoute Global erişim desteklenir?

ExpressRoute Global erişim içerisinde desteklendiği [ülkeler/bölgeler veya basamak](../articles/expressroute/expressroute-global-reach.md). Bu ülkeler/bölgeler veya basamak eşleme adreslerde ExpressRoute bağlantı hatları yeniden oluşturulması gerekir.

### <a name="i-have-more-than-two-on-premises-networks-each-connected-to-an-expressroute-circuit-can-i-enable-expressroute-global-reach-to-connect-all-of-my-on-premises-networks-together"></a>İkiden fazla şirket içi ağlar sahibim, her bir ExpressRoute bağlantı hattına bağlı. ExpressRoute tüm şirket içi ağlarımı birbirine bağlamak Global erişim etkinleştirebilir miyim?

Desteklenen ülkeler/bölgeler devreler olduğu sürece Evet, kullanabilirsiniz. Aynı anda iki ExpressRoute bağlantı hatları bağlanmanız gerekmez. Örgü tam olarak bir ağ oluşturmak için tüm devre çiftinin listeleme ve yapılandırmayı yinelemeniz gerekir. 

### <a name="can-i-enable-expressroute-global-reach-between-two-expressroute-circuits-at-the-same-peering-location"></a>ExpressRoute genel erişim eşlemesi aynı konumda iki ExpressRoute bağlantı hatları arasında etkinleştirebilirim?

Hayır. İki bağlantı hatlarının eşleme farklı konumlardan olması gerekir. Bir metro desteklenen ülke/bölge içinde birden fazla ExpressRoute eşleme konumu varsa, o metro eşleme farklı konumlarda oluşturulma ExpressRoute bağlantı hatları birbirine bağlayabilirsiniz. 

### <a name="if-expressroute-global-reach-is-enabled-between-circuit-x-and-circuit-y-and-between-circuit-y-and-circuit-z-will-my-on-premises-networks-connected-to-circuit-x-and-circuit-z-talk-to-each-other-via-microsofts-network"></a>Bağlantı hattı X ve Y Bağlantı hattı arasında ve bağlantı hattı Y ve Z devre ExpressRoute Global erişim etkinse, şirket içi ağlarımı X işlem hattına bağlı ve bağlantı hattı Z konuşmak birbirine Microsoft'un ağ üzerinden olacak mı?

Hayır. Her iki şirket içi ağlarınız arasında bağlantıyı etkinleştirmek için karşılık gelen ExpressRoute bağlantı hatları açıkça bağlanmanız gerekir. Yukarıdaki örnekte, bağlantı hattı X ve bağlantı hattı Z bağlamanız gerekir. 

### <a name="what-is-the-network-throughput-i-can-expect-between-my-on-premises-networks-after-i-enable-expressroute-global-reach"></a>ExpressRoute Global erişim etkinleştirdiğinizde my şirket içi ağlar arasında bekleyebilirim ağ aktarım hızı nedir?

Ağ aktarım hızını ExpressRoute Global erişim tarafından etkin şirket içi ağlarınız arasında küçük iki ExpressRoute devrelerinin göre ücret alınır. Şirket içinden Azure'a hem de şirket içi ve şirket içi trafiği aynı bağlantı hattına paylaşabilir ve aynı bant genişliği sınırına tabi olan. 

### <a name="with-expressroute-global-reach-what-are-the-limits-on-the-number-of-routes-i-can-advertise-and-the-number-of-routes-i-will-receive"></a>ExpressRoute Global erişim ile tanıtabilir miyim yolların sayısını ve yolları alma sınırları nelerdir?

Sayısı, Microsoft Azure özel eşleme üzerinde tanıtabilirsiniz standart devredeki 4000 veya 10000 Premium devresi üzerinde kalır. Azure özel eşleme hakkında Microsoft'tan alacağınız yolların sayısını Azure sanal ağlarınıza yollarını toplamı olacak ve yolları diğer şirket içi ağlarınızı ExpressRoute Global erişim bağlı. Lütfen şirket içi yönlendiricinizi uygun en fazla önek sınırı ayarladığınızdan emin olun. 

### <a name="what-is-the-sla-for-expressroute-global-reach"></a>ExpressRoute genel ulaşmak için SLA'sı nedir?

ExpressRoute Global erişim sağlar; aynı [kullanılabilirlik SLA'sı](https://azure.microsoft.com/support/legal/sla/expressroute/v1_3/) normal ExpressRoute hizmet olarak.
