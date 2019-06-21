---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 12/03/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 72ddd0b6cd6c3e12417d3698c403f89312b531f4
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188192"
---
VNet-VNet SSS VPN gateway bağlantıları için geçerlidir. VNet eşlemesi hakkında daha fazla bilgi için bkz: [sanal ağ eşlemesi](../articles/virtual-network/virtual-network-peering-overview.md).

### <a name="does-azure-charge-for-traffic-between-vnets"></a>Azure sanal ağlar arasındaki trafik için ücretli midir?

VNet-VNet trafiği aynı bölgede bir VPN gateway bağlantısı kullandığınızda, her iki yönde de ücretsizdir. Bölgeler arası sanal ağa çıkış trafiği ise kaynak bölgelerine bağlı giden sanal ağlar arası veri aktarımı fiyatları ile ücretlendirilir. Daha fazla bilgi için [VPN Gateway fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/vpn-gateway/). Sanal ağlarınız VNet eşlemesi bir VPN ağ geçidi yerine kullanarak bağlanıyorsanız bkz [sanal ağ fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-network/).

### <a name="does-vnet-to-vnet-traffic-travel-across-the-internet"></a>VNet-VNet trafiği internet üzerinden yolculuk ediyor mu?

Hayır. VNet-VNet trafiği internet değil Microsoft Azure omurgası üzerinden dolaşır.

### <a name="can-i-establish-a-vnet-to-vnet-connection-across-azure-active-directory-aad-tenants"></a>Azure Active Directory (AAD) kiracıları arasında VNet-VNet bağlantı kurabilmesi için?

Evet, Azure VPN ağ geçidi kullanan VNet-VNet bağlantıları AAD kiracılar genelinde çalışır.

### <a name="is-vnet-to-vnet-traffic-secure"></a>Sanal Ağdan Sanal Ağa trafiği güvenli mi?

Evet, bunu yapmak için IPSec/IKE şifrelemesiyle korunur.

### <a name="do-i-need-a-vpn-device-to-connect-vnets-together"></a>Sanal ağları birbirine bağlamak için bir VPN cihazı gerekiyor mu?

Hayır. İşletmeler arası bağlantı gerekmediği sürece birden fazla Azure sanal ağını birleştirmek için VPN cihazına gerek yoktur.

### <a name="do-my-vnets-need-to-be-in-the-same-region"></a>Sanal ağlarımın aynı bölgede olması gerekiyor mu?

Hayır. Sanal ağlar aynı ya da farklı Azure bölgelerinde (konumlarında) bulunabilir.

### <a name="if-the-vnets-arent-in-the-same-subscription-do-the-subscriptions-need-to-be-associated-with-the-same-active-directory-tenant"></a>Sanal ağlar aynı abonelikte değilse, aboneliklerin aynı Active Directory kiracısıyla ilişkilendirilmiş olması gerekiyor mu?

Hayır.

### <a name="can-i-use-vnet-to-vnet-to-connect-virtual-networks-in-separate-azure-instances"></a>Farklı Azure örneklerinde sanal ağları bağlamak için Sanal Ağdan Sanal Ağa bağlantı kullanabilir miyim? 

Hayır. Sanal Ağdan Sanal Ağa, aynı Azure örneğindeki sanal ağları bağlamayı destekler. Örneğin, genel Azure ve Çince/Almanya/ABD kamu Azure arasında bir bağlantı oluşturulamıyor örnekleri. Bu senaryolar için siteden siteye VPN bağlantısı kullanmayı düşünün.

### <a name="can-i-use-vnet-to-vnet-along-with-multi-site-connections"></a>Sanal Ağdan Sanal Ağa bağlantıyı çoklu site bağlantılarıyla birlikte kullanabilir miyim?

Evet. Sanal ağ bağlantısı, çoklu site VPN’leri ile eşzamanlı olarak kullanılabilir.

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a>Bir sanal ağ kaç şirket içi siteye ve sanal ağa bağlanabilir?

Bkz: [ağ geçidi gereksinimleri](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#requirements) tablo.

### <a name="can-i-use-vnet-to-vnet-to-connect-vms-or-cloud-services-outside-of-a-vnet"></a>Bir sanal ağın dışındaki sanal makineleri veya bulut hizmetlerini bağlamak için Sanal Ağdan Sanal Ağa bağlantı kullanabilir miyim?

Hayır. Sanal Ağdan Sanal Ağa, sanal ağları bağlamayı destekler. Bu değil bağlanan sanal makineler veya sanal ağda olmayan bulut hizmetlerini.

### <a name="can-a-cloud-service-or-a-load-balancing-endpoint-span-vnets"></a>Bir bulut hizmeti ya da bir Yük Dengeleme uç noktası sanal ağlara yayılabilir mi?

Hayır. Bile birbirlerine bağlı bir bulut hizmeti ya da bir Yük Dengeleme uç noktası sanal ağlara yayılamaz.

### <a name="can-i-use-a-policybased-vpn-type-for-vnet-to-vnet-or-multi-site-connections"></a>VNet-VNet veya çoklu Site bağlantıları için PolicyBased VPN türü kullanabilir miyim?

Hayır. VNet-VNet ve çok siteli bağlantılar gerektiren Azure VPN ağ geçitleri ile (önceki adı dinamik yönlendirme) RouteBased VPN türleri.

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-to-another-vnet-with-a-policybased-vpn-type"></a>PolicyBased VPN türüne sahip sanal ağı RouteBased VPN türüne sahip başka bir sanal ağa bağlayabilir miyim?

Hayır, her iki sanal ağın rota tabanlı (önceki adıyla dinamik yönlendirme) VPN'ler kullanmanız gerekir.

### <a name="do-vpn-tunnels-share-bandwidth"></a>VPN tünelleri bant genişliğini paylaşır mı?

Evet. Sanal ağ üzerindeki tüm VPN tünelleri, Azure VPN ağ geçidi ve aynı ağ geçidi çalışma süresi SLA’sı üzerinde aynı kullanılabilir bant genişliğini paylaşır.

### <a name="are-redundant-tunnels-supported"></a>Yedekli tüneller destekleniyor mu?

Bir sanal ağ çifti arasındaki yedekli tüneller, sanal ağ geçitlerinden biri etkin-etkin olarak yapılandırıldığında desteklenir.

### <a name="can-i-have-overlapping-address-spaces-for-vnet-to-vnet-configurations"></a>Sanal Ağdan Sanal Ağa yapılandırmalar için çakışan adres alanları kullanabilir miyim?

Hayır. Çakışan IP adresi aralıklarını kullanamazsınız.

### <a name="can-there-be-overlapping-address-spaces-among-connected-virtual-networks-and-on-premises-local-sites"></a>Bağlı sanal ağlar ve şirket içi yerel siteleri arasında çakışan adres alanları olabilir mi?

Hayır. Çakışan IP adresi aralıklarını kullanamazsınız.



