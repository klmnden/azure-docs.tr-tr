---
title: Azure Güvenlik Duvarı nedir?
description: Azure Güvenlik Duvarı özelliklerini öğrenin.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: overview
ms.custom: mvc
ms.date: 7/16/2018
ms.author: victorh
ms.openlocfilehash: 3657b619dc57b994158c711c46d4db6924aa2930
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39089830"
---
# <a name="what-is-azure-firewall"></a>Azure Güvenlik Duvarı nedir?

Azure Güvenlik Duvarı, Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı bir güvenlik hizmetidir. Yerleşik yüksek kullanılabilirlik oranı ve kısıtlamasız bulut ölçeklenebilirliğiyle hizmet olarak tam durum bilgisi olan bir güvenlik duvarıdır. 

[!INCLUDE [firewall-preview-notice](../../includes/firewall-preview-notice.md)]

![Güvenlik duvarına genel bakış](media/overview/firewall-overview.png)



Aboneliklerle sanal ağlarda uygulama ve ağ bağlantısı ilkelerini merkezi olarak oluşturabilir, zorlayabilir ve günlüğe alabilirsiniz. Azure Güvenlik Duvarı, dış güvenlik duvarlarının sanal ağınızdan kaynaklanan trafiği tanımlamasına olanak tanımak amacıyla sanal ağ kaynaklarınız için statik genel bir IP adresi kullanır.  Hizmet, günlük ve analiz için Azure İzleyici ile tamamen tümleşik çalışır.

## <a name="features"></a>Özellikler

Azure Güvenlik Duvarı genel önizlemesi şu özellikleri sunar:

### <a name="built-in-high-availability"></a>Yerleşik yüksek kullanılabilirlik
Yüksek kullanılabilirlik yerleşiktir; bu nedenle, ek bir yük dengeleyici gerekmez ve yapılandırmanız gereken hiçbir şey yoktur.

### <a name="unrestricted-cloud-scalability"></a>Kısıtlamasız bulut ölçeklenebilirliği 
Azure Güvenlik Duvarı, değişen ağ trafiği akışlarıyla başa çıkmak için gerek duyduğunuz kadar ölçeklenebilir, bu sayede trafiğinizin en yoğun olduğu durum için bütçe yapmak zorunda kalmazsınız.

### <a name="fqdn-filtering"></a>FQDN filtreleme 
Giden HTTP/S trafiğini, joker karakter de içerebilen tam etki alanı adlarının (FQDN) belirtilen bir listesiyle sınırlayabilirsiniz. Bu özelliğe SSL sonlandırması gerekmez.

### <a name="network-traffic-filtering-rules"></a>Ağ trafiği filtreleme kuralları

Ağ filtreleme kurallarını kaynak ve hedef IP adresine, bağlantı noktasına ve protokole göre merkezi olarak oluşturabilir, *izin verebilir* veya *reddedebilirsiniz*. Azure Güvenlik Duvarı tamamen durum bilgisine sahiptir; bu nedenle, farklı türden bağlantıların geçerli paketlerini tanıyabilir. Kurallar, birden çok abonelik ve sanal ağda zorlanır ve günlüğe kaydedilir.

### <a name="outbound-snat-support"></a>Giden SNAT desteği

Tüm giden sanal ağ trafiği IP adresleri Azure Güvenlik Duvarı genel IP’sine çevrilir (Kaynak Ağ Adresi Çevirisi). Sanal ağınızdan kaynaklanan uzak İnternet hedeflerine yönelik trafiği tanımlayabilir ve buna izin verebilirsiniz.

### <a name="azure-monitor-logging"></a>Azure İzleyici günlükleri

Günlükleri depolama hesabında arşivleyebilmeniz, olayların Event Hub'a akışını yapabilmeniz ve bunları Log Analytics'e gönderebilmeniz için, tüm olaylar Azure İzleyici ile tümleştirilmiştir.

## <a name="known-issues"></a>Bilinen sorunlar

Azure Güvenlik Duvarı genel önizlemesindeki bilinen sorunla şunlardır:


|Sorun  |Açıklama  |Risk azaltma  |
|---------|---------|---------|
|NSG’lerle birlikte çalışabilirlik     |Güvenlik duvarı alt ağına bir ağ güvenlik grubu (NSG) uygulanırsa, NGS giden İnternet erişimine izin verecek şekilde yapılandırılmış olsa bile giden İnternet bağlantısı engellenebilir. Giden İnternet bağlantıları bir VirtualNetwork'ten geliyor olarak işaretlenir ve hedef de İnternettir. NSG'nin varsayılan olarak VirtualNetwork'ten VirtualNetwork'e *izni* vardır ama hedef İnternet olduğunda bu geçerli değildir.|Sorunu hafifletmek için, güvenlik duvarı alt ağına uygulanan NSG'ye aşağıdaki gelen kuralını ekleyin:<br><br>Kaynak: VirtualNetwork Kaynak bağlantı noktaları: Herhangi Biri <br><br>Hedef: Tüm Hedef Bağlantı Noktaları: Herhangi Biri <br><br>Protokol: Tüm Erişim: İzin Ver|
|Azure Güvenlik Merkezi (ASC) Tam Zamanında (JIT) özelliği çakışması|Sanal makineye JIT kullanılarak erişilirse ve bu sanal makine varsayılan ağ geçidi olarak Azure Güvenlik Duvarı'na işaret eden kullanıcı tanımlı bir yola sahip bir alt ağ içindeyse, ASC JIT çalışmaz. Bu, asimetrik yönlendirmenin bir sonucudur; paket, sanal makine genel IP'si yoluyla gelir (JIT erişimi açmıştır) ama dönüş yolu güvenlik duvarı üzerindendir ve güvenlik duvarında hiçbir oturum oluşturulmadığından güvenlik duvarı paketi bırakır.|Bu soruna geçici bir çözüm olarak, JIT sanal makinelerini güvenlik duvarına kullanıcı tanımlı bir yolu olmayan ayrı bir alt ağa yerleştirin.|
|Global eşlemeli merkez ve uç çalışmıyor|Merkezle güvenlik duvarının bir Azure bölgesinde dağıtıldığı ve uçların başka Azure bölgesinde yer aldığı merkez ve uç modelinde, merkeze Global Sanal Ağ eşlemesi üzerinden bağlanma desteklenmez.|Daha fazla bilgi için bkz. [Sanal ağ eşlemeyi oluşturma, değiştirme veya silme](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-manage-peering#requirements-and-constraints)|
TCP/UDP dışı protokollere (örneğin ICMP) yönelik ağ filtreleme kuralları İnternet'e bağlı trafik için çalışmaz|TCP/UDP dışı protokollere yönelik ağ filtreleme kuralları genel IP adresinize SNAT ile çalışmaz. TCP/UDP dışı protokoller, uç alt ağlarla sanal ağlar arasında desteklenir.|Azure Güvenlik Duvarı, [bugün IP protokolleri için SNAT desteği olmayan](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-standard-overview#limitations) Standart Load Balancer kullanır. Gelecek sürümlerden birinde bu senaryoyu destekleme seçeneklerini gözden geçiriyoruz.



## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure portalını kullanarak Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma](tutorial-firewall-deploy-portal.md)
- [Şablon kullanarak Azure Güvenlik Duvarı’nı dağıtma](deploy-template.md)
- [Azure Güvenlik Duvarı test ortamı oluşturma](scripts/sample-create-firewall-test.md)

