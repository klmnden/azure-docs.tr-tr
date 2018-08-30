---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 04/05/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 66ff1e2e02728e05cb0aeedce90de1882a8804ce
ms.sourcegitcommit: baed5a8884cb998138787a6ecfff46de07b8473d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "30921317"
---
Sanal Ağdan Sanal Ağa SSS bölümü VPN Gateway bağlantıları için geçerlidir. Sanal Ağ Eşleme konusunu arıyorsanız bkz. [Sanal Ağ Eşleme](../articles/virtual-network/virtual-network-peering-overview.md)

### <a name="does-azure-charge-for-traffic-between-vnets"></a>Azure sanal ağlar arasındaki trafik için ücretli midir?

VPN ağ geçidi bağlantısı kullanılırken, aynı bölge içindeki Sanal Ağdan Sanal Ağa trafik her iki yönde de ücretsizdir. Çapraz bölge Sanal Ağdan Sanal Ağa çıkış trafiği ise kaynak bölgelerine bağlı giden Sanal Ağlar arası veri aktarım hızına bağlı olarak ücretlendirilir. Ayrıntılar için [VPN Gateway fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/vpn-gateway/) bakın. Sanal ağlarınızı bağlamak için VPN Gateway yerine Sanal Ağ Eşlemesi kullanıyorsanız bkz. [Sanal Ağ fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/virtual-network/).

### <a name="does-vnet-to-vnet-traffic-travel-across-the-internet"></a>Sanal Ağdan Sanal Ağa trafik İnternet üzerinden yolculuk ediyor mu?

Hayır. Sanal Ağdan Sanal Ağa trafiği akışı, İnternet üzerinden değil Microsoft Azure omurgası üzerinden sağlanır.

### <a name="can-i-establish-a-vnet-to-vnet-connection-across-aad-tenants"></a>ADD Kiracıları arasında Sanal Ağdan Sanal Ağa bağlantı kurabilir miyim?

Evet, Azure VPN ağ geçitleri kullanılarak kurulan Sanal Ağdan Sanal Ağa bağlantılar ADD Kiracılarında çalışır.

### <a name="is-vnet-to-vnet-traffic-secure"></a>Sanal Ağdan Sanal Ağa trafiği güvenli mi?

Evet, IPsec/IKE şifrelemesiyle korunur.

### <a name="do-i-need-a-vpn-device-to-connect-vnets-together"></a>Sanal ağları birbirine bağlamak için bir VPN cihazı gerekiyor mu?

Hayır. İşletmeler arası bağlantı gerekmediği sürece birden fazla Azure sanal ağını birleştirmek için VPN cihazına gerek yoktur.

### <a name="do-my-vnets-need-to-be-in-the-same-region"></a>Sanal ağlarımın aynı bölgede olması gerekiyor mu?

Hayır. Sanal ağlar aynı ya da farklı Azure bölgelerinde (konumlarında) bulunabilir.

### <a name="if-the-vnets-are-not-in-the-same-subscription-do-the-subscriptions-need-to-be-associated-with-the-same-ad-tenant"></a>Sanal ağlar aynı abonelikte değilse, aboneliklerin aynı AD kiracısıyla ilişkilendirilmesi gerekir mi?

Hayır.

### <a name="can-i-use-vnet-to-vnet-to-connect-virtual-networks-in-separate-azure-instances"></a>Farklı Azure örneklerinde sanal ağları bağlamak için Sanal Ağdan Sanal Ağa bağlantı kullanabilir miyim? 

Hayır. Sanal Ağdan Sanal Ağa, aynı Azure örneğindeki sanal ağları bağlamayı destekler. Örneğin, genel bir Azure örneği ve Çin / Almanya/ ABD Azure örnekleri arasında bağlantı oluşturamazsınız. Bu senaryolar için Siteden Siteye VPN bağlantısı kullanmak isteyebilirsiniz.

### <a name="can-i-use-vnet-to-vnet-along-with-multi-site-connections"></a>Sanal Ağdan Sanal Ağa bağlantıyı çoklu site bağlantılarıyla birlikte kullanabilir miyim?

Evet. Sanal ağ bağlantısı, çoklu site VPN’leri ile eşzamanlı olarak kullanılabilir.

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a>Bir sanal ağ kaç şirket içi siteye ve sanal ağa bağlanabilir?

[Ağ geçidi gereksinimleri](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#requirements) tablosuna bakın.

### <a name="can-i-use-vnet-to-vnet-to-connect-vms-or-cloud-services-outside-of-a-vnet"></a>Bir sanal ağın dışındaki sanal makineleri veya bulut hizmetlerini bağlamak için Sanal Ağdan Sanal Ağa bağlantı kullanabilir miyim?

Hayır. Sanal Ağdan Sanal Ağa, sanal ağları bağlamayı destekler. Bir sanal ağ içinde olmayan sanal makineleri veya bulut hizmetlerini bağlamayı desteklemez.

### <a name="can-a-cloud-service-or-a-load-balancing-endpoint-span-vnets"></a>Bir bulut hizmeti ya da bir yük dengeleme uç noktası sanal ağlara yayılabilir mi?

Hayır. Bir bulut hizmeti ya da yük dengeleme uç noktası, birbirlerine bağlı olsa da sanal ağlara yayılamaz.

### <a name="can-i-used-a-policybased-vpn-type-for-vnet-to-vnet-or-multi-site-connections"></a>Sanal Ağdan Sanal Ağa veya Çoklu Site bağlantıları için PolicyBased VPN türü kullanabilir miyim?

Hayır. Sanal Ağdan Sanal Ağa ve Çoklu Site bağlantıları, Azure VPN ağ geçitlerinin, önceki adı Dinamik Yönlendirme olan RouteBased VPN türleri ile bağlantılarını VPN geçidi ile bağlamalarını gerektirir.

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-to-another-vnet-with-a-policybased-vpn-type"></a>PolicyBased VPN türüne sahip sanal ağı RouteBased VPN türüne sahip başka bir sanal ağa bağlayabilir miyim?

Hayır, her iki sanal ağın da rota tabanlı (önceki adıyla Dinamik Yönlendirme) VPN kullanıyor olması GEREKİR.

### <a name="do-vpn-tunnels-share-bandwidth"></a>VPN tünelleri bant genişliğini paylaşır mı?

Evet. Sanal ağ üzerindeki tüm VPN tünelleri, Azure VPN ağ geçidi ve aynı ağ geçidi çalışma süresi SLA’sı üzerinde aynı kullanılabilir bant genişliğini paylaşır.

### <a name="are-redundant-tunnels-supported"></a>Yedekli tüneller destekleniyor mu?

Bir sanal ağ çifti arasındaki yedekli tüneller, sanal ağ geçitlerinden biri etkin-etkin olarak yapılandırıldığında desteklenir.

### <a name="can-i-have-overlapping-address-spaces-for-vnet-to-vnet-configurations"></a>Sanal Ağdan Sanal Ağa yapılandırmalar için çakışan adres alanları kullanabilir miyim?

Hayır. Çakışan IP adresi aralıklarını kullanamazsınız.

### <a name="can-there-be-overlapping-address-spaces-among-connected-virtual-networks-and-on-premises-local-sites"></a>Bağlı sanal ağlar ve şirket içi yerel siteleri arasında çakışan adres alanları olabilir mi?

Hayır. Çakışan IP adresi aralıklarını kullanamazsınız.



