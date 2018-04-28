---
title: Azure ağı | Microsoft Docs
description: Azure ağ hizmetlerini ve özellikleri hakkında bilgi edinin.
services: networking
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/19/2017
ms.author: jdial
ms.openlocfilehash: 47ee22df081b71e7bafa40210a9c4cac0a844825
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="azure-networking"></a>Azure ağı

Azure birlikte veya ayrı olarak kullanılan ağ yeteneklerini çeşitli sağlar. Bunlar hakkında daha fazla bilgi için aşağıdaki temel işlevler birini tıklatın:
- [Azure kaynakları arasında bağlantı](#connectivity): bağlanmak Azure kaynaklarına güvenli, özel bir sanal ağ içinde ve bulutta bir araya.
- [Internet bağlantısı](#internet-connectivity): Azure kaynaklarını gelen ve giden Internet üzerinden iletişim kurar.
- [Şirket içi bağlantı](#on-premises-connectivity): Azure ayrılmış bir bağlantı veya Internet üzerinden sanal özel ağ (VPN) üzerinden Azure kaynakları için bir şirket içi ağ bağlanın.
- [Yük Dengeleme ve trafik yönü](#load-balancing): trafiği yük dengelemek için aynı konumu ve farklı konumlarda sunucularına doğrudan trafiği sunucular.
- [Güvenlik](#security): ağ alt ağları veya tek tek sanal makineler (VM) arasındaki ağ trafiğini filtre.
- [Yönlendirme](#routing): veya tam denetim Azure ve şirket kaynaklarınız arasında yönlendirme varsayılan yönlendirme kullanma.
- [Yönetilebilirlik](#manageability): izleme ve Azure ağ kaynaklarınızı yönetin.
- [Dağıtım ve yapılandırma araçları](#tools): bir web tabanlı portal kullanmak ya da platformlar arası komut satırı araçları dağıtmak ve yapılandırmak için ağ kaynakları.

## <a name="Connectivity"></a>Azure kaynakları arasında bağlantı

Sanal makineler, bulut Hizmetleri, sanal makine ölçek kümeleri ve Azure App Service ortamları gibi Azure kaynakları özel olarak birbirleri ile bir Azure sanal ağı (VNet) iletişim kurabilir. Bir VNet ayrılmış Azure bulutunun mantıksal ayırma olduğundan, [abonelik](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fnetworking%2ftoc.json). Her Azure aboneliği içindeki birden çok sanal ağlar ve Azure uygulayabilirsiniz [bölge](https://azure.microsoft.com/regions). Her sanal ağ, diğer sanal ağlardan yalıtılır. Her sanal ağ için şunları yapabilirsiniz:

- Genel ve özel (RFC 1918) adresleri kullanarak özel bir gizli IP adresi alanı belirtin. Azure atar kaynakları Vnet'e özel bir IP adresi atadığınız adres alanından bağlı.
- Sanal ağ bir veya daha fazla alt ağlara ayırabilir ve her alt ağ için sanal ağ adres alanının bir bölümü ayırın.
- Azure tarafından sağlanan ad çözümlemesi kullanın veya bir sanal ağa bağlı kaynaklar tarafından kullanmak için kendi DNS sunucusu belirtin.

Azure sanal ağ hizmeti hakkında daha fazla bilgi için okuma [sanal ağa genel bakış](../virtual-network/virtual-networks-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi. Sanal ağlar birbirlerine Vnet'lerde birbirleri ile iletişim kurmak için herhangi bir Vnet'e bağlı kaynakları etkinleştirme bağlayabilirsiniz. Sanal ağları birbirine bağlama veya her ikisini aşağıdaki seçenekleri kullanabilirsiniz:

- **Eşliği:** farklı Azure birbirleri ile iletişim kurmak için sanal ağlar aynı Azure bölgesinde içinde bağlı kaynaklar sağlar. Sanal ağlar arasında gecikme süresi ve bant genişliği aynıdır kaynaklar aynı Vnet'e bağlıymış gibi. Eşleme hakkında daha fazla bilgi için okuma [sanal ağ eşleme genel bakış](../virtual-network/virtual-network-peering-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
- **VPN ağ geçidi:** farklı Azure birbirleri ile iletişim kurmak için sanal ağlar farklı Azure bölgeleri içindeki bağlı kaynaklar sağlar. Sanal ağlar arasındaki trafik bir Azure VPN ağ geçidi üzerinden akar. Sanal ağlar arasında bant genişliği ağ geçidi bant genişliğiyle sınırlı olur. Bir VPN ağ geçidi ile sanal ağlara bağlanma hakkında daha fazla bilgi için okuma [bölgeler arasında VNet-VNet bağlantı yapılandırma](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.

## <a name="internet-connectivity"></a>Internet bağlantısı

Bir sanal ağa bağlı tüm Azure kaynaklarına giden Internet bağlantısı varsayılan olarak sahiptir. Özel IP adresi kaynağının çevrilmiş kaynak ağ adresine (SNAT) genel bir IP adresi Azure altyapısı tarafından ' dir. Giden Internet bağlantısı hakkında daha fazla bilgi için okuma [azure'da giden bağlantılar anlama](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.

Azure kaynaklarına Internet'ten gelen bağlantı kurmak veya SNAT olmadan Internet'e giden iletişim kurmak için bir kaynak genel bir IP adresi atanması gerekir. Genel IP adresleri hakkında daha fazla bilgi için okuma [ortak IP adresleri](../virtual-network/virtual-network-public-ip-address.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.

## <a name="on-premises-connectivity"></a>Şirket içi bağlantısı

Kaynaklar, sanal ağınızda güvenli bir şekilde bir VPN bağlantısı veya doğrudan özel bir bağlantı erişebilirsiniz. Şirket içi ağınızı Azure sanal ağınıza arasındaki ağ trafiğini göndermek için bir sanal ağ geçidi oluşturmanız gerekir. Ağ geçidi için VPN veya ExpressRoute istediğiniz bağlantı türünü oluşturmak için ayarları yapılandırın.

Şirket içi ağınıza aşağıdaki seçeneklerden herhangi bir bileşimini kullanarak bir sanal ağa bağlanabilir:

**Noktadan siteye (VPN üzerinden SSTP)**

Aşağıdaki resimde, birden çok bilgisayar ve bir sanal ağ arasında site bağlantıları için ayrı noktası gösterilmektedir:

![Noktadan siteye](./media/networking-overview/point-to-site.png)

Bu bağlantı tek bir bilgisayar ve bir sanal ağ arasında kurulur. Azure’ı kullanmaya yeni başladıysanız bu bağlantı türü mükemmeldir. Mevcut ağınız üzerinde çok az bir değişiklik gerektirdiğinden veya hiç değişiklik gerektirmediğinden geliştiriciler için de mükemmeldir. Ayrıca bir konferans gibi uzak bir konumdan bağlanırken kullanışlı veya giriş. Noktadan siteye bağlantılar genellikle aynı sanal ağ geçidi ile siteden siteye bağlantı ile bağlı değildir. Bağlantı, bilgisayar ve sanal ağ arasında Internet üzerinden şifrelenmiş iletişim sağlamak için SSTP protokolünü kullanır. Bir noktadan siteye VPN için gecikme süresini tahmin edilemez, olduğu Internet trafiği erişir.

**Siteden siteye (IPSec/IKE VPN tüneli)**

![Siteden siteye](./media/networking-overview/site-to-site.png)

Bu bağlantı, şirket içi VPN aygıtınızın ve bir Azure VPN ağ geçidi arasında kurulur. Bu bağlantı türü VNet erişmek üzere yetkilendirmek herhangi bir şirket içi kaynak sağlar. Şirket içi Cihazınızı ve Azure VPN ağ geçidi arasında Internet üzerinden şifrelenmiş iletişimi sağlayan bir IPSec/IKE VPN bağlantısıdır. Aynı VPN ağ geçidi için birden çok şirket içi sitelere bağlanabilir. Şirket içi VPN cihazı her sitede bir NAT'ın arkasında yer almayan bir dışarıya yönelik genel IP adresi olmalıdır Siteden siteye bağlantı için gecikme süresini tahmin edilemez, olduğu Internet trafiği erişir.

**ExpressRoute (adanmış özel bağlantı)**

![ExpressRoute](./media/networking-overview/expressroute.png)

Bu tür bir bağlantı ağınız ve Azure arasında bir ExpressRoute iş ortağı üzerinden kurulur. Bu bağlantı özeldir. Trafik Internet'e erişmez. ExpressRoute bağlantısı için gecikme süresini tahmin edilebilir, olduğu trafik Internet'e çapraz geçiş değil. ExpressRoute ile bir siteden siteye bağlantı birleştirilebilir.

Önceki tüm bağlantı seçenekleri hakkında daha fazla bilgi için okuma [bağlantı topoloji diyagramları](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.

## <a name="load-balancing"></a>Yük Dengeleme ve trafik yönü

Microsoft Azure, birden çok ağ trafiğini nasıl dağıtıldığını yönetmek için hizmetleri ve Yük Dengeleme sağlar. Aşağıdaki özellikleri birini ayrı ayrı veya birlikte kullanabilirsiniz:

**DNS Yük Dengeleme**

Azure Traffic Manager hizmeti, Genel DNS Yük Dengeleme sağlar. Trafik Yöneticisi, istemciler aşağıdaki yönlendirme yöntemleri birini temel alarak sağlıklı bir uç nokta IP adresiyle yanıt verir:
- **Coğrafi:** istemcileri yönlendirilmiş dayalı belirli Uç noktalara (Azure, dış veya iç içe) DNS sorgularını hangi coğrafi konum kaynaklanan. Bu yöntem, bir istemcinin coğrafi bölge bilerek ve bunları temel alan yönlendirme önemli olduğu senaryolara olanak sağlar. Örnek veri egemenliği gerektirir, içerik & kullanıcı deneyiminin yerelleştirme uymaktan ve farklı bölgelerdeki trafiğini ölçme verilebilir.
- **Performans:** istemciye döndürülen IP adresi "" istemciye en yakın olur. 'En yakın' bitiş noktası tarafından coğrafi uzaklığı ölçülen mutlaka yakın değil. Bunun yerine, bu yöntem, ağ gecikmesi ölçerek en yakın endpoint belirler. Trafik Yöneticisi IP adresi aralıkları ve her Azure veri merkezi gidiş dönüş süresi izlemek için bir Internet gecikme tablosu tutar.
- **Öncelik:** trafiği birincil (en yüksek öncelik) uç noktasına yönlendirilir. Birincil uç noktası kullanılabilir durumda değilse, Traffic Manager trafik ikinci uç noktasına yönlendirir. Birincil ve ikincil uç kullanılabilir değilse, üçüncü ve benzeri için trafiği gider. Uç nokta kullanılabilirliğini yapılandırılan durumu (etkin veya devre dışı) ve devam eden uç nokta izleme dayanır.
- **Ağırlıklı hepsini:** her istek için trafik Yöneticisi kullanılabilir uç nokta rastgele seçer. Bir uç noktasını olasılığını tüm kullanılabilir uç noktaları için atanan ağırlığı temel alır. Hatta trafik dağılımı tüm uç noktaları sonuçlarında arasında aynı ağırlıkta kullanma. Daha yüksek veya düşük ağırlıkları üzerinde belirli uç noktalarını kullanarak DNS yanıtları fazla veya az sık döndürülecek Bu uç neden olur.

Aşağıdaki resimde bir Web uygulaması uç noktasına yönelik bir web uygulaması için bir istek gösterir. Uç noktaları, sanal makineleri gibi diğer Azure hizmetlerine ve bulut hizmetlerini de olabilir.

![Traffic Manager](./media/networking-overview/traffic-manager.png)

İstemci, doğrudan bu uç noktasına bağlanır. Bir uç nokta sağlıksız olduğunu ve ardından istemcileri farklı, sağlıklı uç noktasına yönlendirir azure trafik Yöneticisi algılar. Trafik Yöneticisi hakkında daha fazla bilgi için okuma [Azure trafik Yöneticisi'ne genel bakış](../traffic-manager/traffic-manager-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.

**Uygulama yük dengeleme**

Azure uygulama ağ geçidi hizmeti uygulama teslim denetleyici (ADC) bir hizmet sunar. Uygulama ağ geçidi çeşitli katman 7 (HTTP/HTTPS) Yük Dengeleme özellikleri, web uygulamalarınızı güvenlik açıkları ve açıkları korumak için bir web uygulaması güvenlik duvarı da dahil olmak üzere, uygulamalarınızın sunar. Uygulama ağ geçidi CPU-yoğun SSL sonlandırma uygulama ağ geçidi için boşaltarak web grubu verimliliği en iyi duruma getirme sağlar. 

Diğer katman 7 Yönlendirme yetenekleri hepsini dağıtımını gelen trafiği, tanımlama bilgisi tabanlı oturum benzeşimi, URL yolu tabanlı Yönlendirme ve tek bir uygulama ağ geçidi arkasında birden çok Web sitesini barındırmak yeteneğini içerir. Uygulama ağ geçidi, bir Internet'e dönük ağ geçidi, bir yalnızca iç ağ geçidi veya her ikisinin birleşimini yapılandırılabilir. Tam Azure uygulama ağ geçidi yönetilen, ölçeklenebilir ve yüksek oranda kullanılabilir. Daha iyi yönetilebilirlik için zengin tanılama ve günlüğe kaydetme özellikleri sağlar. Uygulama ağ geçidi hakkında daha fazla bilgi için okuma [uygulama ağ geçidi'ne genel bakış](../application-gateway/application-gateway-introduction.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.

Aşağıdaki resimde, yol tabanlı uygulama ağ geçidi ile yönlendirme URL gösterir:

![Application Gateway](./media/networking-overview/application-gateway.png)

**Ağ yük dengeleme**

Azure yük dengeleyici için tüm UDP ve TCP protokollerini yüksek performanslı, düşük gecikme süreli katman 4 Yük Dengeleme sağlar. Gelen ve giden bağlantılara yönetir. Genel ve iç yük dengeli uç noktalarını yapılandırabilirsiniz. Hizmet kullanılabilirliği yönetmek için TCP ve HTTP sistem durumu yoklama seçeneklerini kullanarak arka uç havuzu hedeflere gelen bağlantıları eşlemek için kurallar tanımlayabilirsiniz. Yük Dengeleyici hakkında daha fazla bilgi için okuma [yük dengeleyici genel bakış](../load-balancer/load-balancer-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.

Aşağıdaki resimde, hem dış ve iç yük dengeleyici kullanan bir Internet'e yönelik çok katmanlı uygulama gösterilmiştir:

![Yük dengeleyici](./media/networking-overview/load-balancer.png)

## <a name="security"></a>Güvenlik

Şu seçenekler kullanılarak Azure kaynaklarını gelen ve giden trafiği filtreleyebilirsiniz:

- **Ağ:** Azure ağ güvenlik grupları (Nsg'ler) Azure kaynakları için gelen ve giden trafiği filtrelemek için uygulayabilirsiniz. Her NSG bir veya daha fazla gelen ve giden kurallarını içerir. Her kural kaynak IP adresleri, hedef IP adresleri, bağlantı noktası ve trafiği ile filtrelenmiş Protokolü belirtir. Nsg'ler, tek tek alt ağları ve tek tek sanal makineleri için uygulanabilir. Nsg'ler hakkında daha fazla bilgi için okuma [ağ güvenlik gruplarını genel bakış](../virtual-network/virtual-networks-nsg.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
- **Uygulama:** web uygulaması güvenlik duvarı ile bir uygulama ağ geçidi'ni kullanarak, web uygulamalarınızı güvenlik açıkları ve güvenlik açıklarına koruyabilirsiniz. SQL ekleme saldırıları, siteler arası komut dosyası ve hatalı biçimlendirilmiş üstbilgiler ortak örnek verilebilir. Uygulama ağ geçidi Bu trafik çıkış filtreleri ve web sunucularına ulaşması durdurur. Etkin istediğiniz hangi kurallarını yapılandırmak kullanabilirsiniz. Belirli ilkeleri devre dışı bırakılmasına izin vermek için SSL anlaşma ilkeleri yapılandırma olanağı sağlanır. Web uygulaması Güvenlik Duvarı hakkında daha fazla bilgi için okuma [Web uygulaması güvenlik duvarı](../application-gateway/application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.

Azure olmayan sağlayın veya şirket içi kullanan ağ uygulamalarını kullanmak istediğiniz ağ özelliğini gerekiyorsa, Vm'lerde ürünleri uygulamak ve bunları Vnet'iniz bağlayın. [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances) birkaç farklı VM şu anda kullanabilir ağ uygulamaları ile önceden yapılandırılmış içerir. Bu önceden yapılandırılmış sanal makineleri, genellikle ağ sanal Gereçleri (NVA) da adlandırılır. NVAs, güvenlik duvarı ve WAN iyileştirmesi gibi uygulamalar ile kullanılabilir.

## <a name="routing"></a>Yönlendirme

Azure varsayılan birbirleri ile iletişim kurmak için herhangi bir VNet içindeki herhangi bir alt ağa bağlı kaynaklar etkinleştirmek yönlendirme tabloları oluşturur. Ya da aşağıdaki türde Azure oluşturduğu varsayılan yolların geçersiz kılmak için yollar uygulayabilirsiniz:
- **Kullanıcı tanımlı:** özel yol tablolarını burada trafik yönlendirilir için her alt ağ için bu denetim yollar oluşturabilirsiniz. Kullanıcı tanımlı yollar hakkında daha fazla bilgi için, [Kullanıcı tanımlı yollar](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesini okuyun.
- **Sınır Ağ Geçidi Protokolü (BGP):** bir Azure VPN ağ geçidi veya ExpressRoute bağlantısı kullanarak şirket içi ağınıza ağınızı bağlanırsanız, sanal ağlar için BGP yollarını yayabilir. BGP iki veya daha fazla ağ arasında yönlendirme ve ulaşılabilirlik bilgilerini takas etmek üzere İnternet’te yaygın olarak kullanılan standart yönlendirme protokolüdür. Azure sanal ağlar bağlamında kullanıldığında, Azure VPN ağ geçitleriyle BGP etkinleştirir ve, şirket içi VPN cihazlarınızı BGP eşleri veya kullanılabilirliği ve ulaşılabilirliği ön eklerin ağ geçitleri veya yönlendiricilerden geçmeye için her iki ağ geçidini bildiren "yolları" değiştirmek amacıyla Komşuları olarak çağrılır. BGP ayrıca bir BGP ağ geçidinin bir BGP eşliğinden diğer tüm BGP eşleri öğrendiği yolları yayarak birden fazla ağ arasında geçiş yönlendirme etkinleştirebilirsiniz. BGP hakkında daha fazla bilgi için bkz: [BGP Azure VPN ağ geçidi'ne genel bakış ile](../vpn-gateway/vpn-gateway-bgp-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.

## <a name="manageability"></a>Yönetilebilirlik

Azure ağı yönetmek ve izlemek için aşağıdaki araçları sağlar:
- **Etkinlik günlükleri:** tüm Azure kaynaklarınız gerçekleştirilen işlemleri durumunu işlemleri hakkında bilgi sağlayın ve işlemi kimin başlattığını etkinlik günlükleri. Etkinlik günlükleri hakkında daha fazla bilgi için okuma [etkinlik günlükleri genel bakış](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
- **Tanılama günlüklerini:** Periyodik ve spontaneous olayları ağ kaynakları tarafından oluşturulan ve Azure depolama hesaplarında oturum, Azure olay Hub'ına gönderilen veya Azure günlük analizi için gönderilir. Tanılama günlükleri kaynak sistem durumu ilişkin bilgi sağlar. Tanılama günlükleri, yük dengeleyici (Internet'e yönelik), ağ güvenlik grupları, yol ve uygulama ağ geçidi için sağlanır. Tanılama günlükleri hakkında daha fazla bilgi için okuma [tanılama günlükleri'ne genel bakış](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
- **Ölçümler:** ölçümleri performans ölçümleri ve kaynakları bir süre boyunca toplanan sayaçları verilmiştir. Ölçümleri eşiklere dayanarak uyarıları tetiklemek için kullanılabilir. Şu anda ölçümleri uygulama ağ geçidinde bulunmaktadır. Ölçümler hakkında daha fazla bilgi için okuma [ölçümleri genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
- **Sorun giderme:** sorun giderme bilgileri doğrudan Azure Portalı'nda erişilebilir. ExpressRoute, VPN ağ geçidi, uygulama ağ geçidi, ağ güvenlik günlükleri, yollar, DNS, yük dengeleyici ve trafik Yöneticisi ile ortak sorunları tanılama bilgileri yardımcı olur.
- **Rol tabanlı erişim denetimi (RBAC):** kimin oluşturabilir ve rol tabanlı erişim denetimi (RBAC) ile ağ kaynaklarını yönetme. Okuyarak RBAC hakkında daha fazla bilgi [RBAC ile çalışmaya başlama](../role-based-access-control/overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi. 
- **Paket yakalama:** Azure Ağ İzleyicisi hizmeti uzantı VM dahilinde aracılığıyla bir paket yakalama bir VM üzerinde çalışan olanağı sağlar. Bu özellik, Linux ve Windows VM'ler için kullanılabilir. Paket yakalama hakkında daha fazla bilgi için okuma [paket yakalama genel bakış](../network-watcher/network-watcher-packet-capture-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
- **IP akışları doğrulayın:** Ağ İzleyicisi paketleri izin verilen veya reddedilen olup olmadığını belirlemek için bir Azure VM ve uzak bir kaynak arasındaki IP akışları doğrulayın olanak tanır. Bu yetenek Yöneticiler bağlantı sorunları çabucak tanılamanızı olanağı sağlar. IP akışları doğrulama hakkında daha fazla bilgi için okuma [IP akış doğrulayın genel bakış](../network-watcher/network-watcher-ip-flow-verify-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
- **VPN bağlantı sorunlarını giderme:** VPN sorun gidericisini özelliği Ağ İzleyicisi, bir bağlantı veya ağ geçidi sorgulamak ve kaynakların durumunu doğrulamak olanağı sağlar. VPN bağlantılarında sorun giderme hakkında daha fazla bilgi için okuma [sorun giderme genel bakış VPN bağlantısı](../network-watcher/network-watcher-troubleshoot-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
- **Ağ topolojisi görüntüleyin:** Ağ İzleyicisi sahip bir VNet ağ kaynaklarına grafik gösterimi görüntüleyin. Ağ topolojisi görüntüleme hakkında daha fazla bilgi için okuma [topolojisine genel bakış](../network-watcher/network-watcher-topology-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.

## <a name="tools"></a>Dağıtım ve yapılandırma araçları

Dağıtma ve Azure ağ kaynakları aşağıdaki araçlardan birini yapılandırın:

- **Azure portal:** bir tarayıcıda çalışan bir grafik kullanıcı arabirimi. [Azure portalı](http://portal.azure.com) açın.
- **Azure PowerShell:** Azure Windows bilgisayarları yönetmek için komut satırı araçları. Okuyarak Azure PowerShell hakkında daha fazla bilgi [Azure PowerShell genel bakış](/powershell/azure/overview?view=azurermps-3.8.0?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
- **Azure komut satırı arabirimi (CLI):** Azure Linux, macOS ya da Windows bilgisayarları yönetmek için komut satırı araçları. Okuyarak Azure CLI hakkında daha fazla bilgi [Azure CLI genel bakış](/cli/azure/get-started-with-azure-cli?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
- **Azure Resource Manager şablonları:** altyapısı ve Azure çözümünü yapılandırmasını tanımlayan bir dosyada (JSON biçiminde). Bir şablon kullanarak çözümünü yaşam döngüsü boyunca defalarca dağıtabilir ve kaynaklarınızın tutarlı bir durumda dağıtıldığından emin olabilirsiniz. Şablonları yazma hakkında daha fazla bilgi için okuma [en iyi uygulamalar şablonları oluşturmak için](../azure-resource-manager/resource-manager-template-best-practices.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi. Şablonlar, Azure portal, CLI veya PowerShell ile dağıtılabilir. Şablonları ile hemen kullanmaya başlamak için çok sayıda önceden yapılandırılmış şablonlarında birini dağıtmak [Azure hızlı başlangıç şablonlarını](https://azure.microsoft.com/resources/templates/?term=network) kitaplığı. 

## <a name="pricing"></a>Fiyatlandırma

Başkalarının özgürlüğüne sahip olmanıza rağmen Azure Ağ Hizmetleri bazıları bir ücret sahiptir. Görünüm [sanal ağ](https://azure.microsoft.com/pricing/details/virtual-network), [VPN ağ geçidi](https://azure.microsoft.com/pricing/details/vpn-gateway), [uygulama ağ geçidi](https://azure.microsoft.com/pricing/details/application-gateway/), [yük dengeleyici](https://azure.microsoft.com/pricing/details/load-balancer), [Ağ İzleyicisi](https://azure.microsoft.com/pricing/details/network-watcher), [DNS](https://azure.microsoft.com/pricing/details/dns), [trafik Yöneticisi](https://azure.microsoft.com/pricing/details/traffic-manager) ve [ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute) sayfalar daha fazla bilgi için fiyatlandırma.

## <a name="next-steps"></a>Sonraki adımlar

- İlk sanal ağınızı oluşturun ve birkaç VM'ler, içindeki adımları tamamlayarak bağlanmak [ilk sanal ağınızı oluşturmak](../virtual-network/quick-create-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
- Bilgisayarınızı içindeki adımları tamamlayarak bir sanal ağa bağlamak [noktadan siteye bağlantı yapılandırma](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
- Yük Dengelemesi ortak sunucuları Internet trafiğini içindeki adımları tamamlayarak [bir Internet'e yönelik Yük Dengeleyici oluşturma](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
