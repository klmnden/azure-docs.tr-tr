---
title: Azure Güvenlik Duvarı nedir?
description: Azure Güvenlik Duvarı özelliklerini öğrenin.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: overview
ms.custom: mvc
ms.date: 9/26/2018
ms.author: victorh
ms.openlocfilehash: 868c20e6f0244794299678214902adf3e6e95f14
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50241421"
---
# <a name="what-is-azure-firewall"></a>Azure Güvenlik Duvarı nedir?

Azure Güvenlik Duvarı, Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı bir güvenlik hizmetidir. Yerleşik yüksek kullanılabilirlik oranı ve kısıtlamasız bulut ölçeklenebilirliğiyle hizmet olarak tam durum bilgisi olan bir güvenlik duvarıdır. 

![Güvenlik duvarına genel bakış](media/overview/firewall-overview.png)

Aboneliklerle sanal ağlarda uygulama ve ağ bağlantısı ilkelerini merkezi olarak oluşturabilir, zorlayabilir ve günlüğe alabilirsiniz. Azure Güvenlik Duvarı, dış güvenlik duvarlarının sanal ağınızdan kaynaklanan trafiği tanımlamasına olanak tanımak amacıyla sanal ağ kaynaklarınız için statik genel bir IP adresi kullanır.  Hizmet, günlük ve analiz için Azure İzleyici ile tamamen tümleşik çalışır.

## <a name="features"></a>Özellikler

Azure Güvenlik Duvarı şu özellikleri sunar:

### <a name="built-in-high-availability"></a>Yerleşik yüksek kullanılabilirlik
Yüksek kullanılabilirlik yerleşiktir; bu nedenle, ek bir yük dengeleyici gerekmez ve yapılandırmanız gereken hiçbir şey yoktur.

### <a name="unrestricted-cloud-scalability"></a>Kısıtlamasız bulut ölçeklenebilirliği 
Azure Güvenlik Duvarı, değişen ağ trafiği akışlarıyla başa çıkmak için gerek duyduğunuz kadar ölçeklenebilir, bu sayede trafiğinizin en yoğun olduğu durum için bütçe yapmak zorunda kalmazsınız.

### <a name="application-fqdn-filtering-rules"></a>Uygulama FQDN filtreleme kuralları

Giden HTTP/S trafiğini, joker karakter de içerebilen tam etki alanı adlarının (FQDN) belirtilen bir listesiyle sınırlayabilirsiniz. Bu özelliğe SSL sonlandırması gerekmez.

### <a name="network-traffic-filtering-rules"></a>Ağ trafiği filtreleme kuralları

Ağ filtreleme kurallarını kaynak ve hedef IP adresine, bağlantı noktasına ve protokole göre merkezi olarak oluşturabilir, *izin verebilir* veya *reddedebilirsiniz*. Azure Güvenlik Duvarı tamamen durum bilgisine sahiptir; bu nedenle, farklı türden bağlantıların geçerli paketlerini tanıyabilir. Kurallar, birden çok abonelik ve sanal ağda zorlanır ve günlüğe kaydedilir.

### <a name="fqdn-tags"></a>FQDN etiketleri

FQDN etiketleri, tanınan Azure hizmeti ağ trafiğine güvenlik duvarınızda izin vermenizi kolaylaştırır. Örneğin Windows Update ağ trafiğine güvenlik duvarınızda izin vermek istediğiniz varsayalım. Bir uygulama kuralı oluşturup Windows Update etiketini eklersiniz. Artık Windows Update’in ağ trafiği, güvenlik duvarınızdan geçebilir.

### <a name="outbound-snat-support"></a>Giden SNAT desteği

Tüm giden sanal ağ trafiği IP adresleri Azure Güvenlik Duvarı genel IP’sine çevrilir (Kaynak Ağ Adresi Çevirisi). Sanal ağınızdan kaynaklanan uzak İnternet hedeflerine yönelik trafiği tanımlayabilir ve buna izin verebilirsiniz.

### <a name="inbound-dnat-support"></a>Gelen DNAT desteği

Güvenlik duvarınızın genel IP adresine gelen trafik çevrilir (Hedef Ağ Adresi Çevirisi) ve sanal ağınızdaki özel IP adreslerine filtrelenir. 

### <a name="azure-monitor-logging"></a>Azure İzleyici günlükleri

Günlükleri depolama hesabında arşivleyebilmeniz, olayların Event Hub'a akışını yapabilmeniz ve bunları Log Analytics'e gönderebilmeniz için, tüm olaylar Azure İzleyici ile tümleştirilmiştir.

## <a name="known-issues"></a>Bilinen sorunlar

Azure Güvenlik Duvarındaki bilinen sorunlar şunlardır:


|Sorun  |Açıklama  |Risk azaltma  |
|---------|---------|---------|
|Azure Güvenlik Merkezi (ASC) Tam Zamanında (JIT) özelliği çakışması|Sanal makineye JIT kullanılarak erişilirse ve bu sanal makine varsayılan ağ geçidi olarak Azure Güvenlik Duvarı'na işaret eden kullanıcı tanımlı bir yola sahip bir alt ağ içindeyse, ASC JIT çalışmaz. Bu, asimetrik yönlendirmenin bir sonucudur; paket, sanal makine genel IP'si yoluyla gelir (JIT erişimi açmıştır) ama dönüş yolu güvenlik duvarı üzerindendir ve güvenlik duvarında hiçbir oturum oluşturulmadığından güvenlik duvarı paketi bırakır.|Bu soruna geçici bir çözüm olarak, JIT sanal makinelerini güvenlik duvarına kullanıcı tanımlı bir yolu olmayan ayrı bir alt ağa yerleştirin.|
|Global eşlemeli merkez ve uç desteklenmiyor|Merkezle güvenlik duvarının bir Azure bölgesinde dağıtıldığı ve uçların başka Azure bölgesinde yer aldığı merkez ve uç modeli desteklenmez. Merkeze Global Sanal Ağ eşlemesi üzerinden bağlanma desteklenmez.|Bu tasarım gereğidir. Daha fazla bilgi için bkz. [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../azure-subscription-service-limits.md#azure-firewall-limits)|
TCP/UDP dışı protokollere (örneğin ICMP) yönelik ağ filtreleme kuralları İnternet'e bağlı trafik için çalışmaz|TCP/UDP dışı protokollere yönelik ağ filtreleme kuralları genel IP adresinize SNAT ile çalışmaz. TCP/UDP dışı protokoller, uç alt ağlarla sanal ağlar arasında desteklenir.|Azure Güvenlik Duvarı, [bugün IP protokolleri için SNAT desteği olmayan](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview#limitations) Standart Load Balancer kullanır. Gelecek sürümlerden birinde bu senaryoyu destekleme seçeneklerini gözden geçiriyoruz.|
|Hedef NAT’ı (DNAT) 80 ve 22 numaralı bağlantı noktaları için çalışmaz.|NAT kural koleksiyonundaki Hedef Bağlantı Noktası alanı 80 veya 22 bağlantı noktalarını içeremez.|Yakın gelecekte bu sorunu düzeltmek için çalışıyoruz. Bu sırada NAT kurallarında hedef bağlantı noktası olarak başka bir bağlantı noktası kullanın. Bağlantı noktası 80 veya 22 hala çevrilmiş bağlantı noktası olarak kullanılabilir. (Örneğin, genel ip:81 ile özel ip:80’i eşleyebilirsiniz).|
|ICMP için eksik PowerShell ve CLI desteği|Azure PowerShell ve CLI ağ kurallarında geçerli bir protokol olarak ICMP'yi desteklemez.|Ancak ICMP'yi portal ve REST API aracılığıyla bir protokol olarak kullanmak mümkündür. ICMP'yi yakında PowerShell ve CLI'ye eklemek üzere çalışıyoruz.|
|FQDN etiketleri bir protokol: bağlantı noktası ayarlamayı gerektirir|FQDN etiketleri olan uygulama kuralları bağlantı noktası:protokol tanımı gerektirir.|Bağlantı noktası:protokol değeri olarak **https** kullanabilirsiniz. Bu alanı FQDN etiketleri kullanıldığında isteğe bağlı yapmak için çalışıyoruz.|
|Bir güvenlik duvarını başka bir gruba veya aboneliğe taşımak desteklenmez.|Bir güvenlik duvarını başka bir gruba veya aboneliğe taşımak desteklenmez.|Bu işlevselliğin desteklenmesi yol haritamızdadır. Bir güvenlik duvarını başka bir kaynak grubuna veya aboneliğe taşımak için geçerli örneği silmeniz ve yeni kaynak grubunda veya abonelikte yeniden oluşturmanız gerekir.|

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure portalını kullanarak Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma](tutorial-firewall-deploy-portal.md)
- [Şablon kullanarak Azure Güvenlik Duvarı’nı dağıtma](deploy-template.md)
- [Azure Güvenlik Duvarı test ortamı oluşturma](scripts/sample-create-firewall-test.md)

