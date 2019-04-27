---
title: Azure ağı | Microsoft Docs
description: Azure ağ hizmetleri ve özellikleri hakkında bilgi edinin.
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
ms.openlocfilehash: 02db9f2b8cb2ec71d23ad077b90eeacb905d2a16
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60565863"
---
# <a name="azure-networking"></a>Azure ağı

Azure, çeşitli birlikte veya ayrı ayrı kullanılabilir ağ özellikleri sağlar. Bunlar hakkında daha fazla bilgi edinmek için aşağıdaki temel özellikleri birine tıklayın:
- [Azure kaynakları arasında bağlantı](#connectivity): Azure kaynaklarını bulutta güvenli ve özel sanal ağ içinde birbirine bağlayın.
- [Internet bağlantısı](#internet-connectivity): Azure kaynaklarını gelen ve giden Internet üzerinden iletişim kurar.
- [Şirket içi bağlantı](#on-premises-connectivity): Bir şirket içi ağ, Internet üzerinden sanal özel ağ (VPN) veya Azure için adanmış bir bağlantı aracılığıyla Azure kaynaklarına bağlayın.
- [Yük Dengeleme ve trafik yönü](#load-balancing): Trafiğinde Yük Dengeleme sunucuları aynı konum ve farklı konumlardaki sunucularına trafiği.
- [Güvenlik](#security): Ağ alt ağları veya tek tek sanal makineleri (VM) arasındaki ağ trafiğini filtreleyin.
- [Yönlendirme](#routing): Varsayılan yönlendirme kullanın veya tamamen Azure ve şirket içi kaynaklarınız arasında yönlendirme denetlemenizi sağlar.
- [Yönetilebilirlik](#manageability): İzleme ve ağ, Azure kaynaklarınızı yönetme.
- [Dağıtım ve yapılandırma araçları](#tools): Ağ kaynaklarını yapılandırmak ve dağıtmak, web tabanlı bir portal veya platformlar arası komut satırı araçlarını kullanın.

## <a name="connectivity"></a>Azure kaynakları arasında bağlantı

Sanal makineler, bulut Hizmetleri, sanal makine ölçek kümeleri ve Azure App Service ortamları gibi Azure kaynaklarının birbiriyle birbirleri ile bir Azure sanal ağı (VNet) iletişim kurabilir. Bir mantıksal yalıtım adanmış Azure bulutunun sanal ağ olduğundan, [abonelik](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fnetworking%2ftoc.json). Her Azure aboneliğinde birden fazla sanal ağ ve Azure uygulayabilirsiniz [bölge](https://azure.microsoft.com/regions). Her VNet diğer Vnet'lere yalıtılır. Her sanal ağ için şunları yapabilirsiniz:

- Genel ve özel (RFC 1918) adresleri kullanarak özel bir gizli IP adresi alanı belirtin. Azure atar kaynaklara, atadığınız adres alanından özel bir IP adresi sanal ağa bağlı.
- Sanal ağ bir veya daha fazla alt ağa bölün ve her alt ağa VNet adres alanının bir bölümü ayırın.
- Azure tarafından sağlanan ad çözümlemesini kullanabilir veya bir sanal ağa bağlı kaynaklar tarafından kullanılmak üzere kendi DNS sunucunuzu belirtin.

Azure sanal ağ hizmeti hakkında daha fazla bilgi edinmek için [sanal ağa genel bakış](../virtual-network/virtual-networks-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi. Sanal ağlar birbiriyle sanal ağlarda birbirleri ile iletişim kurmak için herhangi bir sanal ağa bağlı kaynaklara etkinleştirme bağlanabilirsiniz. Vnet'leri birbirine bağlamak için veya her ikisini aşağıdaki seçeneklerden birini kullanabilirsiniz:

- **Eşleme:** Farklı Azure birbirleri ile iletişim kurmak için sanal ağlar aynı Azure bölgesindeki bağlı kaynakları sağlar. Sanal ağlar arasında gecikme süresi ve bant genişliği olan aynı kaynakları aynı sanal ağa bağlıymış gibi. Eşlemesi hakkında daha fazla bilgi edinmek için [sanal ağ eşleme genel bakış](../virtual-network/virtual-network-peering-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
- **VPN ağ geçidi:** Farklı Azure birbirleri ile iletişim kurmak için sanal ağ içinde farklı Azure bölgelerine bağlı kaynakları sağlar. Sanal ağlar arasındaki trafik bir Azure VPN ağ geçidi üzerinden akar. Sanal ağlar arasında bant genişliği, ağ geçidinin bant genişliğini sınırlıdır. Bir VPN ağ geçidi ile sanal ağları bağlama hakkında daha fazla bilgi edinmek için [bölgeler arasında bir VNet-VNet bağlantısını yapılandırma](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.

## <a name="internet-connectivity"></a>Internet bağlantısı

Bir sanal ağa bağlı tüm Azure kaynakları, varsayılan olarak İnternet'e giden bağlantı vardır. Özel IP adresini kaynağın çevrilmiş kaynak ağ (SNAT) bir genel IP adresini Azure altyapısı tarafından adresidir. Giden Internet bağlantısı hakkında daha fazla bilgi edinmek için [azure'da giden bağlantıları anlama](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.

Azure kaynaklarına Internet'ten gelen iletişim kurmak için ya da SNAT olmadan İnternet'e giden iletişim kurmak için bir kaynak bir genel IP adresi atanmış olmalıdır. Genel IP adresleri hakkında daha fazla bilgi edinmek için [genel IP adresleri](../virtual-network/virtual-network-public-ip-address.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.

## <a name="on-premises-connectivity"></a>Şirket içi bağlantı

Kaynakları ağınızda güvenli bir VPN bağlantısı veya doğrudan bir özel bağlantı erişebilirsiniz. Şirket içi ağınız ile Azure sanal ağınız arasında ağ trafiği göndermek için bir sanal ağ geçidi oluşturmanız gerekir. Ağ geçidi için VPN veya ExpressRoute istediğiniz bağlantı türü oluşturmak için ayarları yapılandırın.

Şirket içi ağınıza aşağıdaki seçeneklerden herhangi bir birleşimini kullanarak bir sanal ağa bağlanabilir:

**Noktadan siteye (SSTP üzerinden VPN)**

Aşağıdaki resimde, birden çok bilgisayar ve bir sanal ağ arasındaki site bağlantıları için ayrı noktası gösterilmektedir:

![Noktadan siteye](./media/networking-overview/point-to-site.png)

Bu bağlantı tek bir bilgisayar arasında bir VNet oluşturulur. Azure’ı kullanmaya yeni başladıysanız bu bağlantı türü mükemmeldir. Mevcut ağınız üzerinde çok az bir değişiklik gerektirdiğinden veya hiç değişiklik gerektirmediğinden geliştiriciler için de mükemmeldir. Ayrıca, uygun bir konferans gibi uzak bir konumdan bağlanırken veya giriş. Noktadan siteye bağlantılar genellikle aynı sanal ağ geçidi ile siteden siteye bağlantı ile bağlı. Bağlantı, bilgisayar ve sanal ağ arasında Internet üzerinden şifrelenmiş iletişim sağlamak üzere SSTP protokolünü kullanır. Noktadan siteye VPN için gecikme süresini tahmin, olduğu trafiği Internet'ten gönderilir.

**Siteden siteye (IPSec/IKE VPN tüneli)**

![Siteden siteye](./media/networking-overview/site-to-site.png)

Bu bağlantı, Azure VPN Gateway ve şirket içi VPN cihazınız arasında kurulur. Bu bağlantı türü, sanal ağa erişebilmesi için yetkilendirme herhangi bir şirket içi kaynak sağlar. Cihazınız şirket içi ve Azure VPN ağ geçidi arasında Internet üzerinden şifrelenmiş iletişimi sağlayan bir IPSec/IKE VPN bağlantısıdır. Aynı VPN ağ geçidine birden çok şirket içi siteye bağlanabilirsiniz. Şirket içi VPN cihazının her sitede NAT arkasında olmayan bir dışarıya yönelik genel IP adresi olmalıdır Siteden siteye bağlantı gecikme sürelerini öngörülemeyen, olduğu trafiği Internet'ten gönderilir.

**ExpressRoute (adanmış özel bağlantı)**

![ExpressRoute](./media/networking-overview/expressroute.png)

Bu tür bir bağlantı, ExpressRoute iş ortağı aracılığıyla ağınız ve Azure arasında oluşturulur. Bu bağlantı özeldir. Trafik Internet'i dolaşmaz. ExpressRoute bağlantısı için gecikme süresini tahmin olduğu trafiği İnternet'e geçiş değil. ExpressRoute, siteden siteye bağlantı ile birleştirilebilir.

Önceki bağlantı seçenekleri hakkında daha fazla bilgi edinmek için [bağlantı topolojisi diyagramları](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.

## <a name="load-balancing"></a>Yük Dengeleme ve trafik yönü

Microsoft Azure, birden çok ağ trafiğin dağıtımını yönetmek için hizmet ve Yük Dengelemesi sağlar. Aşağıdaki özelliklerden birini ayrı ayrı veya birlikte kullanabilirsiniz:

**DNS Yük Dengeleme**

Azure Traffic Manager hizmeti, Genel DNS Yük Dengeleme sağlar. Traffic Manager, istemciler aşağıdaki yönlendirme yöntemlerinden birine dayalı sağlıklı bir uç nokta, IP adresi ile yanıt verir:
- **Coğrafi:** İstemciler, DNS sorgularını kaynaklandığı hangi coğrafi konum tabanlı belirli Uç noktalara (Azure, dış veya iç içe geçmiş) yönlendirilir. Bu yöntem, bir istemcinin coğrafi bölgede bilmek ve bunları temel alan yönlendirme önemli olduğu senaryolar sağlar. Veri egemenliği mandates içeriği ve kullanıcı deneyimi, yerelleştirme uymak ve farklı bölgelerden trafik ölçme verilebilir.
- **Performans:** İstemciye döndürülen IP adresi "" istemciye en yakın olan. 'En yakın' uç noktası tarafından coğrafi uzaklık ölçülen mutlaka en yakın değil. Bunun yerine, bu yöntem, ağ gecikmesi ölçerek en yakın uç nokta belirler. Traffic Manager her bir Azure veri merkezi IP adres aralıkları arasında gidiş dönüş süresi izlemek için bir Internet gecikme tabloda tutar.
- **Önceliği:** Trafik, birincil (en yüksek öncelik) uç noktaya yönlendirilir. Birincil uç nokta kullanılabilir durumda değilse, Traffic Manager trafiği ikinci uç noktasına yönlendirir. Birincil ve ikincil uç kullanılabilir durumda değilse, trafiği üçüncü ve benzeri gider. Uç noktanın kullanılabilirliğini yapılandırılan durumu (etkin veya devre dışı) ve devam eden bir uç nokta izleme bağlıdır.
- **Ağırlıklı hepsini bir kez deneme:** Traffic Manager, her bir istek için kullanılabilir uç nokta rastgele seçer. Bir uç noktasını olasılığını tüm kullanılabilir uç noktaları için atanan ağırlıklara göre faturalandırılır. Genelinde tüm uç noktalar sonuçları bir bile trafik dağılımı ağırlık kullanıyor. Belirli Uç noktalara daha yüksek veya düşük Ağırlıklar kullanılıyor, bu Uç noktalara daha fazla veya daha az sık DNS yanıtlarını döndürülecek neden olur.

Aşağıdaki resimde, bir Web uygulaması uç noktasına yönelik bir web uygulaması için bir istek gösterilmiştir. Uç noktaları, sanal makineleri gibi diğer Azure Hizmetleri ve bulut Hizmetleri ayrıca olabilir.

![Traffic Manager](./media/networking-overview/traffic-manager.png)

İstemci, doğrudan bu uç noktasına bağlanır. Azure Traffic Manager uç nokta sağlıksız olduğunu ve daha sonra istemciler farklı, sağlıklı bir uç noktasına yönlendirir algılar. Traffic Manager hakkında daha fazla bilgi edinmek için [Azure Traffic Manager'a genel bakış](../traffic-manager/traffic-manager-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.

**Uygulama yük dengeleme**

Azure Application Gateway hizmeti, hizmet olarak uygulama teslim denetleyicisi (ADC) sunar. Uygulama ağ geçidi, çeşitli 7. Katman (HTTP/HTTPS) Yük Dengeleme Özellikleri web uygulamalarınızı güvenlik açıklarına ve açıklardan yararlanmaya karşı korumak için bir web uygulaması güvenlik duvarı gibi uygulamalarınız için sunar. Uygulama ağ geçidi ayrıca CPU yoğunluklu SSL sonlandırması yükünü application gateway'e boşaltarak web grubu üretkenliğini iyileştirme olanağı tanır. 

Hepsini bir kez deneme dağıtımını gelen trafiği, tanımlama bilgilerine dayalı oturum benzeşimi, URL yolu tabanlı Yönlendirme ve tek bir uygulama ağ geçidinin arkasında birden fazla Web sitesi barındırma olanağı diğer 7. Katman yönlendirme özelliklerini içerir. Uygulama ağ geçidi, bir Internet'e yönelik ağ geçidi, yalnızca dahili ağ geçidi veya her ikisinin bir birleşimi yapılandırılabilir. Application Gateway tamamen Azure yönetilen, ölçeklenebilir ve yüksek oranda kullanılabilir. Daha iyi yönetilebilirlik için zengin tanılama ve günlüğe kaydetme özellikleri sağlar. Application Gateway hakkında daha fazla bilgi edinmek için [Application Gateway'e genel bakış](../application-gateway/application-gateway-introduction.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.

Aşağıdaki resimde, URL yolu tabanlı yönlendirme ile Application Gateway gösterir:

![Application Gateway](./media/networking-overview/application-gateway.png)

**Ağ yük dengeleme**

Azure Load Balancer tüm UDP ve TCP protokollerini için yüksek performanslı, düşük gecikme süreli katman 4 Yük Dengeleme sağlar. Bu, gelen ve giden bağlantıları yönetir. Genel ve iç yük dengeli uç noktası yapılandırabilirsiniz. Hizmet kullanılabilirliği yönetmek için TCP ve HTTP sistem durumu yoklaması seçeneklerini kullanarak arka uç havuzu hedeflere gelen bağlantıları eşlemeye yönelik kurallar tanımlayabilirsiniz. Load Balancer hakkında daha fazla bilgi edinmek için [Load Balancer'a genel bakış](../load-balancer/load-balancer-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.

Aşağıdaki resimde, hem dış ve iç yük Dengeleyiciler yararlanan bir Internet'e yönelik çok katmanlı uygulaması gösterilmiştir:

![Yük dengeleyici](./media/networking-overview/load-balancer.png)

## <a name="security"></a>Güvenlik

Aşağıdaki seçenekleri kullanarak Azure kaynaklarını gelen ve giden trafiği filtreleyebilirsiniz:

- **Ağ:** Azure ağ güvenlik grupları (Nsg'ler) Azure kaynakları için gelen ve giden trafiği filtrelemek için uygulayabilirsiniz. Her NSG, bir veya daha fazla gelen ve giden kurallar içerir. Her kural, kaynak IP adresleri, hedef IP adresleri, bağlantı noktası ve trafik ile filtre Protokolü belirtir. Nsg'leri belirli alt ağlar ve tek tek sanal makineleri için uygulanabilir. Nsg'ler hakkında daha fazla bilgi edinmek için [ağ güvenlik gruplarına genel bakış](../virtual-network/security-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
- **Uygulama:** Web uygulaması güvenlik duvarı ile bir uygulama ağ geçidi'ni kullanarak web uygulamalarınızı güvenlik açıklarına ve açıklardan yararlanmaya karşı koruyabilirsiniz. SQL ekleme saldırıları, siteler arası komut dosyası ve hatalı biçimlendirilmiş üst bilgiler ortak örnek verilebilir. Uygulama ağ geçidi, bu trafik çıkış filtreler ve uygulamanızı web sunucularınızdan ulaşmasını durdurur. Etkin istediğiniz hangi kuralları yapılandırabilirsiniz. SSL anlaşma ilkelerini yapılandırma becerisi, belirli ilkeler devre dışı bırakılmasına olanak sağlanır. Web uygulaması Güvenlik Duvarı hakkında daha fazla bilgi edinmek için [Web uygulaması güvenlik duvarı](../application-gateway/application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.

Azure olmayan sağlayın veya ağ uygulamaları şirket içinde kullandığınız kullanmak istediğiniz ağ özelliğini gerekirse, ürünleri Vm'lere uygulamak ve bunları sanal ağınıza bağlayın. [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances) şu anda kullanabilir ağ uygulamaları ile önceden yapılandırılmış birçok farklı sanal makineleri içerir. Bu önceden yapılandırılmış sanal makineler, genellikle ağ sanal Gereçleri (NVA) da adlandırılır. Nva'ları, güvenlik duvarı ve WAN iyileştirme gibi uygulamalar ile kullanılabilir.

## <a name="routing"></a>Yönlendirme

Azure, varsayılan birbirleri ile iletişim kurmak için herhangi bir sanal ağ içindeki herhangi bir alt ağa bağlı kaynaklara sağlayan rota tabloları oluşturur. Ya da Azure oluşturduğu varsayılan rotaları geçersiz kılmak için rotalar aşağıdaki türde uygulayabilirsiniz:
- **Kullanıcı tanımlı:** Trafiği için her alt ağ için yönlendirildiği denetleyen rotalarla özel rota tabloları oluşturabilirsiniz. Kullanıcı tanımlı yollar hakkında daha fazla bilgi için, [Kullanıcı tanımlı yollar](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesini okuyun.
- **Sınır Ağ Geçidi Protokolü (BGP):** Sanal ağınızı bir Azure VPN Gateway veya ExpressRoute bağlantısı kullanarak şirket içi ağınıza bağlanırsa BGP yolları sanal ağlarınıza yayabilirsiniz. BGP iki veya daha fazla ağ arasında yönlendirme ve ulaşılabilirlik bilgilerini takas etmek üzere İnternet’te yaygın olarak kullanılan standart yönlendirme protokolüdür. Azure sanal ağları bağlamında kullanıldığında, BGP Azure VPN ağ geçitleri ve BGP eşlikleri veya kullanılabilirliği ve ulaşılabilirliği gitmek bu ön ekler için her iki ağ geçitlerinde konusunda bilgilendiren "yolları" değiştirmek amacıyla Komşuları, adlı şirket içi VPN cihazlarınızı etkinleştirir. ağ geçitlerinden veya yönlendiricilerden söz konusu. BGP yolları bir BGP ağ geçidinin bir BGP eşliğinden diğer tüm BGP eşleri yayma ile birden fazla ağ arasında geçiş yönlendirme de etkinleştirebilirsiniz. BGP hakkında daha fazla bilgi için bkz: [genel Azure VPN Gateways ile BGP'ye](../vpn-gateway/vpn-gateway-bgp-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.

## <a name="manageability"></a>Yönetilebilirlik

Azure ağı yönetmek ve izlemek için aşağıdaki araçları sağlar:
- **Etkinlik günlükleri:** Tüm Azure kaynakları, işlemi kimin başlattığını ve gerçekleştirilen işlemlerin durumunu işlemleri hakkında bilgi sağlayan etkinlik günlükleri vardır. Etkinlik günlükleri hakkında daha fazla bilgi edinmek için [etkinlik günlükleri genel bakış](../azure-monitor/platform/activity-logs-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
- **Tanılama günlükleri:** Düzenli ve spontaneous olaylar ağ kaynaklar tarafından oluşturulan ve Azure depolama hesaplarında, bir Azure olay Hub'ına gönderilen veya Azure İzleyici günlüklerine gönderilen günlüğe kaydedilir. Tanılama günlükleri öngörülere ve kaynak durumunu sağlar. Tanılama günlükleri (Internet'e yönelik) yük dengeleyici, ağ güvenlik grupları, yollar ve Application Gateway için sağlanır. Tanılama günlükleri hakkında daha fazla bilgi edinmek için [tanılama günlüklerine genel bakış](../azure-monitor/platform/diagnostic-logs-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
- **Ölçümleri:** Performans ölçümleri ve kaynakları bir süre içinde toplanan sayaçları ölçümleridir. Ölçümler üzerinde eşiklerine göre uyarıları tetiklemek için kullanılabilir. Şu anda ölçümler Application Gateway üzerinde kullanılabilir. Ölçümler hakkında daha fazla bilgi edinmek için [ölçümlerine genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
- **Sorun giderme:** Sorun giderme bilgilerini, doğrudan Azure Portalı'nda erişilebilir. ExpressRoute, VPN ağ geçidi, Application Gateway, ağ güvenlik günlükleri, yollar, DNS, yük dengeleyici ve Traffic Manager ile ortak sorunları tanılama bilgileri sağlar.
- **Rol tabanlı erişim denetimi (RBAC):** Oluşturabilir ve rol tabanlı erişim denetimi (RBAC) ile ağ kaynaklarını yönetme denetimi. RBAC hakkında daha fazla bilgi edinmek [RBAC ile çalışmaya başlama](../role-based-access-control/overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi. 
- **Paket yakalaması:** Azure Ağ İzleyicisi hizmeti, bir paket yakalama VM'de bir uzantı aracılığıyla bir sanal makine üzerinde çalıştırma olanağı sağlar. Bu özellik, Linux ve Windows Vm'leri için kullanılabilir. Paket yakalama hakkında daha fazla bilgi edinmek için [paket yakalamaya genel bakış](../network-watcher/network-watcher-packet-capture-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
- **IP akışlarını doğrulama:** Ağ İzleyicisi arasında Azure VM ve uzak kaynak paketlerinin izin verilen veya reddedilen olup olmadığını belirlemek için IP akışlarını doğrulama sağlar. Bu özellik, yöneticiler bağlantı sorunları çabucak tanılamanızı olanağı sağlar. IP akışlarını doğrulama hakkında daha fazla bilgi edinmek için [IP akışı doğrulama genel bakış](../network-watcher/network-watcher-ip-flow-verify-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
- **VPN bağlantı sorunlarını giderme:** Ağ İzleyicisi VPN sorun giderici yeteneğini kaynakların durumunu doğrulayın ve bir bağlantı veya ağ geçidi sorgulama olanağı sağlar. VPN bağlantı sorunlarını giderme hakkında daha fazla bilgi edinmek için [sorun gidermeye genel bakış VPN bağlantısı](../network-watcher/network-watcher-troubleshoot-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
- **Ağ topolojisini görüntüleme:** Bir Sanal Ağ İzleyicisi ile ağ kaynaklarını grafik gösterimi görüntüleyin. Ağ topolojisini görüntüleme hakkında daha fazla bilgi edinmek için [topolojisine genel bakış](../network-watcher/network-watcher-topology-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.

## <a name="tools"></a>Dağıtım ve yapılandırma araçları

Dağıtma ve Azure ağ kaynakları aşağıdaki araçlardan birini yapılandırın:

- **Azure portalı:** Bir tarayıcıda çalışan bir grafik kullanıcı arabirimi. [Azure portalı](https://portal.azure.com) açın.
- **Azure PowerShell:** Azure Windows bilgisayarlardan yönetmek için komut satırı araçları. Azure PowerShell hakkında daha fazla bilgi edinmek [Azure PowerShell'e genel bakış](/powershell/azure/overview?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
- **Azure komut satırı arabirimi (CLI):** Azure Linux, macOS veya Windows bilgisayarlardan yönetmek için komut satırı araçları. Azure CLI hakkında daha fazla bilgi edinmek [Azure CLI'yı genel bakış](/cli/azure/get-started-with-azure-cli?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
- **Azure Resource Manager şablonları:** Altyapı ve bir Azure çözümü yapılandırmasını tanımlayan bir dosyası (JSON biçiminde). Bir şablon kullanarak çözümünü yaşam döngüsü boyunca defalarca dağıtabilir ve kaynaklarınızın tutarlı bir durumda dağıtıldığından emin olabilirsiniz. Yazma şablonları hakkında daha fazla bilgi edinmek için [şablonları oluşturmaya yönelik en iyi uygulamalar](../azure-resource-manager/resource-manager-template-best-practices.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi. Şablonları Azure portalı, CLI veya PowerShell ile dağıtılabilir. Şablonlarla hemen çalışmaya başlamak için çok sayıda önceden yapılandırılmış şablonlar birini dağıtmak [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?term=network) kitaplığı. 

## <a name="pricing"></a>Fiyatlandırma

Başkalarının ücretsizdir ancak Azure Ağ Hizmetleri bir ücreti vardır. Görünüm [sanal ağ](https://azure.microsoft.com/pricing/details/virtual-network), [VPN ağ geçidi](https://azure.microsoft.com/pricing/details/vpn-gateway), [Application Gateway](https://azure.microsoft.com/pricing/details/application-gateway/), [yük dengeleyici](https://azure.microsoft.com/pricing/details/load-balancer), [Ağİzleyicisi](https://azure.microsoft.com/pricing/details/network-watcher), [DNS](https://azure.microsoft.com/pricing/details/dns), [Traffic Manager](https://azure.microsoft.com/pricing/details/traffic-manager) ve [ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute) fiyatlandırma sayfalarına daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar

- İlk Vnet'inizi oluşturma ve birkaç Vm'lere, adımları tamamlayarak [ilk sanal ağınızı oluşturma](../virtual-network/quick-create-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
- İçindeki adımları tamamlayarak bilgisayarınızı bir Vnet'e bağlayın [noktadan siteye bağlantı yapılandırma](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
- Yük Dengelemesi ortak sunucuları Internet trafiğini adımları tamamlayarak [bir Internet'e yönelik Yük Dengeleyici oluşturma](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesi.
