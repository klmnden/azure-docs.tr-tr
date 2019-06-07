---
title: Azure Güvenlik Duvarı nedir?
description: Azure Güvenlik Duvarı özelliklerini öğrenin.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: overview
ms.custom: mvc
ms.date: 6/5/2019
ms.author: victorh
Customer intent: As an administrator, I want to evaluate Azure Firewall so I can determine if I want to use it.
ms.openlocfilehash: 4b33174b20cdf42e29cdb5b4786122513d2c6080
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66753745"
---
# <a name="what-is-azure-firewall"></a>Azure Güvenlik Duvarı nedir?

Azure Güvenlik Duvarı, Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı bir güvenlik hizmetidir. Bir yerleşik yüksek kullanılabilirlik ve ölçeklenebilirlik sınırsız bulut hizmetiyle tamamen durum bilgisi olan bir güvenlik duvarı gibidir. 

![Güvenlik duvarına genel bakış](media/overview/firewall-threat.png)

Aboneliklerle sanal ağlarda uygulama ve ağ bağlantısı ilkelerini merkezi olarak oluşturabilir, zorlayabilir ve günlüğe alabilirsiniz. Azure Güvenlik Duvarı, dış güvenlik duvarlarının sanal ağınızdan kaynaklanan trafiği tanımlamasına olanak tanımak amacıyla sanal ağ kaynaklarınız için statik genel bir IP adresi kullanır.  Hizmet, günlük ve analiz için Azure İzleyici ile tamamen tümleşik çalışır.

## <a name="features"></a>Özellikler

Azure Güvenlik Duvarı şu özellikleri sunar:

### <a name="built-in-high-availability"></a>Yerleşik yüksek kullanılabilirlik

Yüksek kullanılabilirlik, yapılandırmanız gereken bir şey yoktur ve hiçbir ek yük dengeleyicisi gerekli şekilde oluşturulmuştur.

### <a name="unrestricted-cloud-scalability"></a>Kısıtlamasız bulut ölçeklenebilirliği

Azure Güvenlik Duvarı, değişen ağ trafiği akışlarıyla başa çıkmak için gerek duyduğunuz kadar ölçeklenebilir, bu sayede trafiğinizin en yoğun olduğu durum için bütçe yapmak zorunda kalmazsınız.

### <a name="application-fqdn-filtering-rules"></a>Uygulama FQDN filtreleme kuralları

Giden HTTP/S trafiğini, joker karakter de içerebilen tam etki alanı adlarının (FQDN) belirtilen bir listesiyle sınırlayabilirsiniz. Bu özellik, SSL sonlandırma gerektirmez.

### <a name="network-traffic-filtering-rules"></a>Ağ trafiği filtreleme kuralları

Ağ filtreleme kurallarını kaynak ve hedef IP adresine, bağlantı noktasına ve protokole göre merkezi olarak oluşturabilir, *izin verebilir* veya *reddedebilirsiniz*. Azure Güvenlik Duvarı tamamen durum bilgisine sahiptir; bu nedenle, farklı türden bağlantıların geçerli paketlerini tanıyabilir. Kurallar, birden çok abonelik ve sanal ağda zorlanır ve günlüğe kaydedilir.

### <a name="fqdn-tags"></a>FQDN etiketleri

FQDN etiketleri, tanınan Azure hizmeti ağ trafiğine güvenlik duvarınızda izin vermenizi kolaylaştırır. Örneğin Windows Update ağ trafiğine güvenlik duvarınızda izin vermek istediğiniz varsayalım. Bir uygulama kuralı oluşturup Windows Update etiketini eklersiniz. Artık Windows Update’in ağ trafiği, güvenlik duvarınızdan geçebilir.

### <a name="service-tags"></a>Hizmet etiketleri

Hizmet etiketi, güvenlik kuralı oluşturma sırasındaki karmaşıklığı en aza indirmeye yardımcı olmak için bir IP adresi ön eki grubunu temsil eder. Kendi hizmet etiketinizi oluşturamaz veya hangi IP adreslerinin bir etiket içinde yer belirtin. Hizmet etiketine dahil olan adres ön ekleri Microsoft tarafından yönetilir ve hizmet etiketi adresler değiştikçe otomatik olarak güncelleştirilir.

### <a name="threat-intelligence"></a>Tehdit bilgileri

Güvenlik duvarınız için tehdit bilgileri tabanlı filtrelemeyi etkinleştirerek, kötü amaçlı olduğu bilinen IP adresleri ve etki alanları ile trafik yaşanması durumunda uyarı alabilir ve trafiği reddedebilirsiniz. IP adresleri ve etki alanları, Microsoft Tehdit Bilgileri akışından alınır.

### <a name="outbound-snat-support"></a>Giden SNAT desteği

Tüm giden sanal ağ trafiği IP adresleri Azure Güvenlik Duvarı genel IP’sine çevrilir (Kaynak Ağ Adresi Çevirisi). Sanal ağınızdan kaynaklanan uzak İnternet hedeflerine yönelik trafiği tanımlayabilir ve buna izin verebilirsiniz.

### <a name="inbound-dnat-support"></a>Gelen DNAT desteği

Güvenlik duvarınızın genel IP adresine gelen trafik çevrilir (Hedef Ağ Adresi Çevirisi) ve sanal ağınızdaki özel IP adreslerine filtrelenir.

### <a name="azure-monitor-logging"></a>Azure İzleyici günlükleri

Tüm olaylar, bir depolama hesabına veya olay hub'ınıza olayları akış günlüklerini arşivleyin veya Azure İzleyici günlüklerine şirketlerde olanak tanıyan Azure İzleyici ile tümleştirilmiştir.

## <a name="known-issues"></a>Bilinen sorunlar

Azure Güvenlik Duvarındaki bilinen sorunlar şunlardır:

|Sorun  |Açıklama  |Risk azaltma  |
|---------|---------|---------|
|Azure Güvenlik Merkezi (ASC) Tam Zamanında (JIT) özelliği çakışması|Sanal makineye JIT kullanılarak erişilirse ve bu sanal makine varsayılan ağ geçidi olarak Azure Güvenlik Duvarı'na işaret eden kullanıcı tanımlı bir yola sahip bir alt ağ içindeyse, ASC JIT çalışmaz. Bu asimetrik yönlendirme bir sonucu olan – sanal makinenin genel IP bir paket halinde sunulur (JIT açık erişim), ancak Güvenlik Duvarı'nı kurulan oturum olduğundan paket bırakılır güvenlik duvarı aracılığıyla dönüş yoludur.|Bu soruna geçici bir çözüm olarak, JIT sanal makinelerini güvenlik duvarına kullanıcı tanımlı bir yolu olmayan ayrı bir alt ağa yerleştirin.|
TCP/UDP dışı protokollere (örneğin ICMP) yönelik ağ filtreleme kuralları İnternet'e bağlı trafik için çalışmaz|TCP/UDP dışı protokollere yönelik ağ filtreleme kuralları genel IP adresinize SNAT ile çalışmaz. TCP/UDP dışı protokoller, uç alt ağlarla sanal ağlar arasında desteklenir.|Azure Güvenlik Duvarı, [bugün IP protokolleri için SNAT desteği olmayan](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview#limitations) Standart Load Balancer kullanır. Gelecekteki bir sürümde bu senaryoyu desteklemek için seçenekleri araştırırken ediyoruz.|
|ICMP için eksik PowerShell ve CLI desteği|Azure PowerShell ve CLI ağ kurallarında geçerli bir protokol olarak ICMP'yi desteklemez.|ICMP portalı ve REST API aracılığıyla bir protokol olarak kullanmak yine de mümkündür. PowerShell ve CLI ICMP yakında eklemek için çalışıyoruz.|
|FQDN etiketleri bir protokol: bağlantı noktası ayarlamayı gerektirir|Uygulama kuralları FQDN etiketlerle, bağlantı noktası gerektirir: protokol tanımı.|Bağlantı noktası:protokol değeri olarak **https** kullanabilirsiniz. FQDN etiketleri kullanıldığında bu alan isteğe bağlı yapmak için çalışıyoruz.|
|Bir Güvenlik Duvarı'nı bir farklı kaynak grubuna veya aboneliğe taşıma desteklenmiyor|Bir Güvenlik Duvarı'nı bir farklı kaynak grubuna veya aboneliğe taşıma desteklenmiyor.|Bu işlevi destekleyen bizim üzerinde yol haritasıdır. Bir güvenlik duvarını başka bir kaynak grubuna veya aboneliğe taşımak için geçerli örneği silmeniz ve yeni kaynak grubunda veya abonelikte yeniden oluşturmanız gerekir.|
|Bağlantı noktası aralığında ağ ve uygulama kuralları|Yüksek bağlantı noktaları, yönetimi ve sistem durumu için ayrılmış olarak bağlantı noktaları için 64.000 sınırlı araştırmaları. |Bu sınırlama gevşetmek için çalışıyoruz.|
|Tehdit zekası uyarıları maskelenmiş|Ağ kuralları 80/443 numaralı giden filtreleme maskeleri için hedef ile uyarı yalnızca modu için yapılandırıldığında zeka uyarılar tehdit.|80/uygulama kurallarını kullanarak 443 üzerinden giden filtreleme oluşturun. Veya, tehdit zekası moduna **uyar ve reddetme**.|
|Azure güvenlik duvarı Azure DNS ad çözümlemesi için yalnızca kullanır.|Azure güvenlik duvarı yalnızca Azure DNS kullanma FQDN'leri çözümler. Özel bir DNS sunucusu desteklenmez. Diğer alt ağlardaki DNS çözümlemesi üzerinde hiçbir etkisi yoktur.|Bu sınırlama gevşetmek için çalışıyoruz.|
|Azure güvenlik duvarı SNAT/dnat'ı özel IP hedefler için çalışmıyor|Internet giriş/çıkış için Azure güvenlik duvarı SNAT/dnat'ı destek sınırlıdır. SNAT/dnat'ı için özel IP hedefleri şu anda çalışmıyor. Örneğin, uç için uç.|Bu, gelecekteki bir güncelleştirmesi için yol haritası açıktır.
## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Dağıtma ve Azure Azure portalını kullanarak güvenlik duvarı yapılandırma](tutorial-firewall-deploy-portal.md)
- [Şablon kullanarak Azure Güvenlik Duvarı’nı dağıtma](deploy-template.md)
- [Azure Güvenlik Duvarı test ortamı oluşturma](scripts/sample-create-firewall-test.md)