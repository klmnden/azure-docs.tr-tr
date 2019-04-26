---
title: Ağ güvenlik kavramları ve azure'da gereksinimleri | Microsoft Docs
description: Bu makalede, Azure'un bu alanların her birinde sunduğu üzerinde ağ güvenlik kavramları ve gereksinimler ve bilgiler hakkında temel açıklamalar sağlar.
services: security
documentationcenter: na
author: TomShinder
manager: barbkess
editor: TomSh
ms.assetid: bedf411a-0781-47b9-9742-d524cf3dbfc1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/29/2018
ms.author: terrylan
ms.openlocfilehash: 2aabe3d1fa8a6034c2dab38c8d6fa6da4b00ac1b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60444274"
---
# <a name="azure-network-security-overview"></a>Azure ağ güvenliğine genel bakış

Ağ güvenliği, ağ trafiğini denetimleri uygulayarak kaynakları yetkisiz erişim veya saldırının koruma işlemi olarak tanımlanabilir. Yalnızca güvenli trafiğe izin verildiğinden emin emin olmaktır. Azure, uygulama ve hizmet bağlantı gereksinimlerini desteklemek için sağlam bir ağ altyapısı içerir. Azure'da, şirket içi arasında bulunan kaynaklar arasında ağ bağlantısı mümkündür ve Azure kaynakları, barındırılan ve gelen ve giden İnternet'e ve Azure.

Bu makale Azure ağ güvenlik alanında sunan seçeneklerden bazıları kapsar. Hakkında bilgi edinebilirsiniz:

* Azure ağı
* Ağ erişim denetimi
* Azure Güvenlik Duvarı
* Güvenli uzaktan erişim ve şirket içi bağlantı
* Kullanılabilirlik
* Ad çözümlemesi
* Çevre ağı (DMZ) mimarisi
* Azure DDoS koruması
* Azure ön kapısı
* Traffic manager
* İzleme ve tehdit algılama

## <a name="azure-networking"></a>Azure ağı

Azure sanal makineler bir Azure sanal ağına bağlanması gerekir. Bir sanal ağ üzerinde fiziksel Azure ağ dokusu oluşturulmuş mantıksal bir yapıdır. Her sanal ağ, diğer tüm sanal ağlardan yalıtılır. Bu, ağ trafiğini dağıtımlarınız için diğer Azure müşterilerinden erişilebilir olmadığından emin olun yardımcı olur.

Daha fazla bilgi edinin:

* [Sanal ağa genel bakış](../virtual-network/virtual-networks-overview.md)

## <a name="network-access-control"></a>Ağ erişim denetimi

Ağ erişim denetimi belirli cihazlara veya bir sanal ağ içindeki alt ağlara gelen ve giden bağlantı sınırlama, işlemidir. Sanal makinelerinize erişimi ve Hizmetleri onaylanan kullanıcılar ve cihazlara ağ erişim denetimi amacı sınırlandırmaktır. Erişim denetimleri veya sanal makine veya hizmet bağlantıları reddetmek için kararları temel alır.

Azure, çeşitli ağ erişim denetimi gibi destekler:

* Ağ katmanı denetimi
* Denetim yönlendirmek ve zorlamalı tünel
* Sanal ağ güvenlik Gereçleri

### <a name="network-layer-control"></a>Ağ katmanı denetimi

Herhangi bir güvenli dağıtıma bazı ölçü ağ erişim denetimi gerektirir. Ağ erişim denetimi gerekli sistemleri için sanal makine iletişimi sınırlandırmak için hedefidir. Diğer iletişim girişimleri engellenir.

> [!NOTE]
> Depolama güvenlik duvarlarını kapsamdaki [Azure depolama güvenliğine genel bakış](security-storage-overview.md) makale

#### <a name="network-security-rules-nsgs"></a>Ağ güvenlik kuralları (Nsg'ler)

Temel ağ düzeyinde erişim denetimi (IP adresini ve TCP veya UDP protokollerini temel alınarak) gerekiyorsa, ağ güvenlik grupları (Nsg'ler) kullanabilirsiniz. Bir NSG, basic, durum bilgisi olan, paket filtreleme güvenlik duvarı olduğundan ve dayalı olarak erişimi denetlemenize olanak sağlayan bir [5-tuple](https://www.techopedia.com/definition/28190/5-tuple). Nsg'ler, yönetimi basitleştirmek ve yapılandırması hataları olasılığını azaltmak için işlevselliğini içerir:

* **Genişletilmiş Güvenlik kuralları** NSG kuralı tanımını basitleştirin ve aynı sonuca ulaşmak için birden çok basit kurallar oluşturmak zorunda yerine karmaşık kurallar oluşturmanıza imkan tanır.
* **Hizmet etiketleri** Microsoft IP adreslerinden oluşan bir grubu temsil eden bir etiket oluşturulur. Bunlar dinamik olarak ekleme etikette tanımlayan koşulları karşılayan IP aralıklarını içerecek şekilde güncelleştirin. Örneğin, Doğu bölgesindeki tüm Azure depolama için uygulanan bir kural oluşturmak istiyorsanız Storage.EastUS kullanabilirsiniz
* **Uygulama güvenlik grupları** uygulama gruplarına kaynakları dağıtma ve bu uygulama grupları kullanan kurallar oluşturarak bu kaynaklara erişimi denetlemek sağlar. Örneğin, 'Webservers' uygulama grubuna dağıtılan webservers varsa 'Webservers' uygulama grubundaki tüm sistemler için Internet'ten 443 trafiğe izin veren bir NSG uygulanan bir kural oluşturabilirsiniz.

Nsg'ler, uygulama katmanı İnceleme sağlamaz veya kimliği doğrulanmış erişim denetimleri.

Daha fazla bilgi edinin:

* [Ağ güvenlik grupları](../virtual-network/security-overview.md)

#### <a name="asc-just-in-time-vm-access"></a>ASC'de yalnızca zamanında VM erişimi

[Azure Güvenlik Merkezi](../security-center/security-center-intro.md) vm'lerdeki Nsg'ler yönetebilir ve uygun rol tabanlı erişim denetimi ile bir kullanıcı kadar sanal makine erişimine izin kilitleme [RBAC](../role-based-access-control/overview.md) erişim isteğinde izinleri bulunur. Kullanıcı başarıyla olduğunda yetkili ASC belirtilen süre için seçilen bağlantı noktası erişim izni vermek için Nsg'ler değişiklikler yapar. Süre dolduğunda Nsg'ler, önceki durumu güvenli geri yüklenir.

Daha fazla bilgi edinin:

* [Azure Güvenlik Merkezi tam zamanında erişim](../security-center/security-center-just-in-time.md)

#### <a name="service-endpoints"></a>Hizmet uç noktaları

Hizmet uç noktaları trafiğiniz denetime uygulamak için başka bir yoludur. Desteklenen hizmetler ile iletişimi yalnızca sanal ağlarınıza doğrudan bağlantı üzerinden sınırlayabilirsiniz. Belirtilen bir Azure hizmetine ağınızdan gelen trafik, Microsoft Azure omurga ağında kalır.  

Daha fazla bilgi edinin:

* [Hizmet uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md#securing-azure-services-to-virtual-networks)

### <a name="route-control-and-forced-tunneling"></a>Denetim yönlendirmek ve zorlamalı tünel

Sanal ağlarınızda yönlendirme davranışını denetlemek için kritik önem taşır. Yönlendirme hatalı yapılandırıldıysa, uygulamalar ve hizmetler, sanal makinede barındırılan ait ve potansiyel saldırganlar tarafından işletilen sistemleri dahil olmak üzere, yetkisiz cihazlara bağlanabilir.

Azure ağı, sanal ağlarınızda ağ trafiğini yönlendirme davranışını özelleştirmek için özelliğini destekler. Bu, sanal ağınızda bulunan tablo girişleri yönlendirme varsayılan alter sağlar. Yönlendirme davranışını denetimin belirli bir cihaz veya cihaz grubunuz, giden tüm trafiği girer veya sanal ağınız belirli bir konuma bırakır emin olmanıza yardımcı olur.

Örneğin, sanal ağınızda sanal ağ güvenlik Gereci olabilir. Sanal ağınızdan gelen ve giden tüm trafiği, güvenlik sanal gereçten aşması emin olmanız gerekir. Bunu yapılandırarak yapabilirsiniz [kullanıcı tanımlı yollar](../virtual-network/virtual-networks-udr-overview.md) (Udr) azure'da.

[Zorlamalı tünel](https://www.petri.com/azure-forced-tunneling) hizmetlerinizi internet'teki cihazlar için bir bağlantı başlatmak için izin verilmiyor emin olmak için kullanabileceğiniz bir mekanizmadır. Bu gelen bağlantıları kabul etmesini ve onlara yanıt farklı olduğunu unutmayın. Ön uç web sunucusuna internet konaklarından isteklerine yanıt gerekir ve bu nedenle internet kaynaklı trafik bu web sunucularına gelen ve web sunucularını yanıt vermesine izin izin verilir.

İzin vermek istemediğiniz bir giden talep başlatmak için bir ön uç web sunucusudur. Bu bağlantılar kötü amaçlı yazılım indirmek için kullanılabildiğinden, bu tür istekleri bir güvenlik riski temsil edebilir. İnternet'e Giden istekleri başlatmak için bu ön uç sunucularına isteseniz bile, bunları, şirket içi web proxy'leri Git zorlamak isteyebilirsiniz. Bu, filtreleme ve günlüğe kaydetme URL'sinin yararlanmasına olanak sağlar.

Bunun yerine, bunu önlemek için zorlamalı tünel kullanmak istersiniz. Zorlamalı tünel etkinleştirdiğinizde, şirket içi ağ geçidi üzerinden İnternet'e yönelik tüm bağlantılar zorlanır. Zorlamalı tünel Udr'ler avantajlarından yararlanarak yapılandırabilirsiniz.

Daha fazla bilgi edinin:

* [Kullanıcı tanımlı yollar ve IP iletimi nedir](../virtual-network/virtual-networks-udr-overview.md)

### <a name="virtual-network-security-appliances"></a>Sanal ağ güvenlik Gereçleri

Nsg'ler, Udr ve zorlamalı tünel ağ ve Aktarım katmanı güvenlik düzeyini sunarken [OSI modeli](https://en.wikipedia.org/wiki/OSI_model), ağı yüksek düzeyde güvenlik seçeneğini etkinleştirmek isteyebilirsiniz.

Örneğin, güvenlik gereksinimlerinizi şunlar olabilir:

* Kimlik doğrulama ve yetkilendirme, uygulamanıza erişmesine izin vermeden önce
* Yetkisiz giriş algılama ve yanıt yetkisiz erişim
* Üst düzey protokoller için uygulama katmanı İnceleme
* URL filtreleme
* Ağ düzeyinde virüsten koruma ve kötü amaçlı yazılımdan koruma
* Bot virüsten koruma
* Uygulama erişimi denetimi
* Ek bir DDoS koruma (yukarıda, Azure fabric tarafından kendisine sağlanan DDoS koruması)

Bir Azure iş ortağı çözümü kullanarak gelişmiş ağ güvenliği özelliklere erişebilirsiniz. Güvenlik çözümleri ziyaret ederek en güncel Azure iş ortağı ağı bulabilirsiniz [Azure Marketi](https://azure.microsoft.com/marketplace/)ve "güvenlik" ve "ağ güvenliği" için arama

## <a name="azure-firewall"></a>Azure Güvenlik Duvarı

Azure Güvenlik Duvarı, Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı bir güvenlik hizmetidir. Yerleşik yüksek kullanılabilirlik oranı ve kısıtlamasız bulut ölçeklenebilirliğiyle hizmet olarak tam durum bilgisi olan bir güvenlik duvarıdır. Bazı özellikler şunlardır:

* Yüksek kullanılabilirlik
* Bulut ölçeklenebilirliği
* Uygulama FQDN filtreleme kuralları
* Ağ trafiği filtreleme kuralları

Daha fazla bilgi edinin:

* [Azure güvenlik duvarına genel bakış](../firewall/overview.md)

## <a name="secure-remote-access-and-cross-premises-connectivity"></a>Güvenli uzaktan erişim ve şirket içi bağlantı

Kurulum, yapılandırma ve yönetim uzaktan yapılacak Azure kaynaklarını gereksinimleriniz. Ayrıca, dağıtmak isteyebilirsiniz [karma BT](https://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) bileşenleri şirket içi çözümler ve Azure genel bulut. Bu senaryolar, güvenli uzaktan erişim gerektirir.

Azure ağ güvenli uzaktan erişim aşağıdaki senaryoları destekler:

* Ayrı iş istasyonları bir sanal ağa bağlanma
* Bir sanal ağa bir VPN ile şirket içi ağınıza bağlama
* Şirket içi ağınızı ayrılmış bir WAN bağlantısı ile sanal bir ağa bağlayın
* Sanal ağları birbirine bağlama

### <a name="connect-individual-workstations-to-a-virtual-network"></a>Ayrı iş istasyonları bir sanal ağa bağlanma

Bireysel geliştiriciler veya operasyon personeli sanal makinelerini ve hizmetlerini azure'da yönetmenize olanak tanıyan isteyebilirsiniz. Örneğin, bir sanal ağ üzerindeki bir sanal makine erişmeniz varsayalım. Ancak, güvenlik ilkeniz ayrı sanal makinelere RDP veya SSH ile uzaktan erişim izin vermiyor. Bu durumda, kullanabileceğiniz bir [noktadan siteye VPN](../vpn-gateway/point-to-site-about.md) bağlantı.

Noktadan siteye VPN bağlantısı, kullanıcı ve sanal ağ arasında özel ve güvenli bir bağlantı oluşturmanıza olanak sağlar. VPN bağlantı kurulduğunda kullanıcı için RDP veya SSH VPN bağlantısı üzerinden sanal ağdaki tüm sanal makine. (Bu kullanıcı kimlik doğrulaması yapabilir ve yetkili varsayar.) Noktadan siteye VPN destekler:

* Yuva Tünel Protokolü (SSTP), bir özel SSL tabanlı VPN Protokolü güvenliğini sağlayın. Bir SSL VPN çözümü, güvenlik duvarları, SSL kullanan TCP bağlantı noktası 443 de çoğu güvenlik duvarı açık olduğundan duvarlarından geçebildiği. SSTP yalnızca Windows cihazlarda desteklenir. Azure, SSTP (Windows 7 ve üzeri) yüklü Windows'ın tüm sürümlerini destekler.

* IKEv2 VPN, standart tabanlı bir IPsec VPN çözümüdür. IKEv2 VPN, Mac cihazlardan (OSX sürüm 10.11 ve üzeri) bağlantı kurmak için kullanılabilir.

* [OpenVPN](https://azure.microsoft.com/updates/openvpn-support-for-azure-vpn-gateways/)

Daha fazla bilgi edinin:

* [PowerShell kullanarak bir sanal ağa noktadan siteye bağlantı yapılandırma](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="connect-your-on-premises-network-to-a-virtual-network-with-a-vpn"></a>Bir sanal ağa bir VPN ile şirket içi ağınıza bağlama

Tüm kurumsal ağa veya bölümleri, bir sanal ağa bağlanmak isteyebilirsiniz. Bu karma BT'de yaygın senaryoları, burada kuruluşlar [kendi şirket içi veri merkezinizi Azure'a genişletir](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84). Çoğu durumda, kuruluşların bir hizmet olarak Azure ve şirket bölümleri bölümlerini barındırır. Örneğin, bunlar bir çözümü, Azure'da ön uç web sunucuları içerir ve arka uç şirket içi veritabanları, bunu yapabilir. Bu tür bir "şirket içi" bağlantıları da yer alan Azure yönetim olun kaynaklara daha güvenli ve Active Directory etki alanı denetleyicileri Azure'a genişletme gibi senaryoları etkinleştirmek.

Tek yönlü gerçekleştirmek için bu kullanmaktır bir [siteden siteye VPN](https://www.techopedia.com/definition/30747/site-to-site-vpn). Noktadan siteye VPN siteden siteye VPN arasındaki fark, ikincisi tek bir cihazı bir sanal ağa bağlandığını ' dir. Siteden siteye VPN tamamını bir ağa (örneğin, şirket içi ağınıza) bir sanal ağa bağlanır. Bir sanal ağ için siteden siteye VPN'ler, yüksek oranda güvenli IPSec tünel modu VPN protokolü kullanın.

Daha fazla bilgi edinin:

* [Azure portalını kullanarak siteden siteye VPN bağlantısı bir Resource Manager sanal ağı oluşturma](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [VPN Gateway hakkında](../vpn-gateway/vpn-gateway-about-vpngateways.md)

### <a name="connect-your-on-premises-network-to-a-virtual-network-with-a-dedicated-wan-link"></a>Şirket içi ağınızı ayrılmış bir WAN bağlantısı ile sanal bir ağa bağlayın

Noktadan siteye ve siteden siteye VPN bağlantıları, şirket içi bağlantılar etkinleştirmek için geçerlidir. Ancak, bazı kuruluşlar onlara aşağıdaki dezavantajları göz önünde bulundurun:

* VPN bağlantıları internet üzerinden veri taşıyın. Bu, olası güvenlik sorunlarına genel bir ağ üzerinden veri taşıma ile ilgili bu bağlantılar sunar. Ayrıca internet bağlantıları için kullanılabilirlik ve güvenilirlik garanti edilemez.
* Sanal ağlara VPN bağlantıları, bazı uygulamalar ve max yaklaşık 200 MB/sn hızında kullanıma geldiklerinde amaçları için bant genişliği olmayabilir.

Yüksek düzeyde güvenlik ve kullanılabilirlik, şirketler arası bağlantılar için genellikle gereken kuruluşlar adanmış WAN bağlantıları uzak sitelere bağlanmak için kullanın. Azure, şirket içi ağınızı bir sanal ağa bağlanmak için kullanabileceğiniz özel bir WAN bağlantısı kullanma olanağı sağlar. Azure ExpressRoute, Express route doğrudan ve Express route küresel erişim etkinleştirme bu.

Daha fazla bilgi edinin:

* [Expressroute'a teknik genel bakış](../expressroute/expressroute-introduction.md)
* [ExpressRoute doğrudan](../expressroute/expressroute-erdirect-about.md)
* [Express route küresel erişim](..//expressroute/expressroute-global-reach.md)

### <a name="connect-virtual-networks-to-each-other"></a>Sanal ağları birbirine bağlama

Çok sayıda sanal ağ dağıtımlarınız için kullanmak mümkündür. Neden bunu yapabilirsiniz çeşitli nedenleri vardır. Yönetimini basitleştirmek isteyebilirsiniz veya daha fazla güvenlik isteyebilirsiniz. Farklı sanal ağlardaki kaynaklar yerleştirme motivasyon bakılmaksızın kaynakların birbiriyle bağlanmak için bu ağların her istediğinizde zamanlar olabilir.

Bir seçenektir "geri döngü" başka bir sanal ağda hizmetlerine bağlanmak için bir sanal ağ hizmetleri için internet üzerinden. Bağlantı, bir sanal ağ'da başlar, Internet üzerinden geçer ve hedef sanal ağa geri gelir. Bu seçenek, herhangi bir internet tabanlı iletişimde devralınan güvenlik sorunları bağlantı kullanıma sunar.

Bağlanan iki sanal ağ arasında siteden siteye VPN oluşturmak için daha iyi bir seçenek olabilir. Bu yöntem aynı kullanır [IPSec tünel modu](https://technet.microsoft.com/library/cc786385.aspx) protokolü olarak yukarıda belirtilen şirket içi siteden siteye VPN bağlantısı.

Bu yaklaşımın avantajı, internet üzerinden bağlamak yerine Azure ağ yapısında üzerinden VPN bağlantısı kurulduktan olmasıdır. Bu güvenlik internet üzerinden bağlanan bir siteden siteye VPN ile karşılaştırıldığında, ek bir koruma katmanı sağlar.

Daha fazla bilgi edinin:

* [Azure Resource Manager ve PowerShell kullanarak VNet-VNet bağlantı yapılandırma](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

Sanal ağlarınıza bağlanmak için başka bir yolu [VNET eşlemesi](../virtual-network/virtual-network-peering-overview.md). Bu özellik, aralarında iletişim, Internet üzerinden geçmeden olmadan Microsoft omurga altyapısı olur. böylece iki Azure ağları bağlamanıza olanak sağlar. VNET eşlemesi aynı bölgedeki iki sanal ağı veya iki sanal ağ, Azure bölgeleri arasında bağlanabilirsiniz. Nsg'ler, farklı alt ağlara veya sistemler arasında bağlantı sınırlamak için kullanılabilir.

## <a name="availability"></a>Kullanılabilirlik

Kullanılabilirlik, herhangi bir güvenlik programının temel bir bileşenidir. Kullanıcılar ve sistemlerden ağ üzerinden erişmek gereksinim duydukları şeyleri erişemiyorsanız, hizmet olarak kabul edilir tehlikeye. Azure aşağıdaki yüksek kullanılabilirlik mekanizmaları ağ teknolojilerini içerir:

* HTTP tabanlı Yük Dengeleme
* Ağ düzeyinde Yük Dengeleme
* Genel Yük Dengeleme

Yük Dengeleme bağlantıları birden çok cihaz arasında eşit olarak dağıtmak için tasarlanmış bir mekanizmadır. Yük Dengeleme hedefleri şunlardır:

* Kullanılabilirliğini artırmak için. Birden çok cihazda Bakiye bağlantıları yüklediğinizde, bir veya daha fazla aygıtlar hizmet ödün vermeden kullanılamaz hale gelebilir. Kalan çevrimiçi cihaz üzerinde çalışan hizmetleri hizmetinden gelen içerik sunmak devam edebilirsiniz.
* Performansı artırmak için. Birden çok cihazda Bakiye bağlantıları yüklediğinizde, tüm işlemleri işlemek tek bir cihaz yok. Bunun yerine, içeriği sunması için işlem ve bellek taleplerini yayılır birden çok cihazda.

### <a name="http-based-load-balancing"></a>HTTP tabanlı Yük Dengeleme

Bu web hizmetleri önündeki bir HTTP tabanlı yük dengeleyici web tabanlı Hizmetleri hangi sıklıkta çalıştırılacağını kuruluşların bağlamasına. Bu durum, yeterli düzeyde performans ve yüksek kullanılabilirlik sağlamaya yardımcı olur. Geleneksel, ağ tabanlı yük Dengeleyiciler, ağ ve Aktarım katmanı protokolleri kullanır. HTTP tabanlı yük Dengeleyiciler, diğer taraftan, HTTP protokolü özelliklerine dayanan kararlar alın.

Azure Application Gateway, HTTP tabanlı Yük Dengeleme, web tabanlı hizmetler sunar. Application Gateway destekler:

* Tanımlama bilgilerine dayalı oturum benzeşimi. Bu özellik, yük dengeleyicinin arkasına sunucularından birine kurulan istemci ve sunucu arasında değişmeden kalmasını sağlar. Bu işlem kararlılığını sağlar.
* SSL yük boşaltma. Bir istemci yük dengeleyiciyle bağlandığında, bu oturuma HTTPS (SSL) protokolü kullanılarak şifrelenir. Ancak, performansı artırmak için yük dengeleyici ve yük dengeleyicinin arkasındaki web sunucusu arasında bağlantı kurmak için HTTP (şifrelenmemiş) protokolünü kullanabilirsiniz. Yük dengeleyicinin arkasındaki web sunucuları işlemci ek yükü şifrelemeyle ilgili deneyimi yoktur çünkü bu "SSL boşaltma" adlandırılır. Web sunucuları bu nedenle istekleri daha hızlı bir şekilde hizmet verebilirsiniz.
* URL tabanlı içerik yönlendirme. Bu özellik, hedef URL temel alınarak ileriye doğru bağlantıları burada hakkında kararlar yapmak yük dengeleyici için mümkün kılar. Bu, Yük Dengeleme kararları IP adreslerine göre olun çözümlerine kıyasla çok daha fazla esneklik sağlar.

Daha fazla bilgi edinin:

* [Application Gateway'e genel bakış](../application-gateway/application-gateway-introduction.md)

### <a name="network-level-load-balancing"></a>Ağ düzeyinde Yük Dengeleme

HTTP tabanlı Yük Dengeleme aksine ağ düzeyinde Yük Dengeleme, IP adresi ve bağlantı noktası (TCP veya UDP) rakamlara dayanan kararlarını verir.
Azure'daki Azure Load Balancer'ı kullanarak ağ düzeyinde Yük Dengeleme faydaları elde edebilirsiniz. Load Balancer'ın önemli özelliklerinden bazıları şunlardır:

* Ağ düzeyinde Yük Dengeleme, IP adresi ve bağlantı noktası numaralarını temel.
* Herhangi bir uygulama katmanı protokolü için destek.
* Azure sanal makinelerine yükünü dengeleyen ve rol örnekleri bulut Hizmetleri.
* Internet'e (Dış Yük Dengeleme) ve internet olmayan için kullanılabilir (iç Yük Dengeleme) uygulamaları ve sanal makineler karşılıklı.
* Yük dengeleyicinin arkasındaki hizmetlerinden herhangi birinin kullanılamaz duruma olmadığını belirlemek için kullanılan, uç nokta izleme.

Daha fazla bilgi edinin:

* [Birden çok sanal makine veya hizmet arasında Internet'e yönelik Yük Dengeleyici](../load-balancer/load-balancer-internet-overview.md)
* [İç yük dengeleyiciye genel bakış](../load-balancer/load-balancer-internal-overview.md)

### <a name="global-load-balancing"></a>Genel Yük Dengeleme

Bazı kuruluşlar, olası en yüksek düzeyde kullanılabilirlik istersiniz. Bu hedefe ulaşmak için bir Global olarak dağıtılmış veri merkezlerinde uygulamalarını barındırmak için yoludur. Dünyanın dört bir yanında bulunan veri merkezlerinde barındırılan bir uygulama, olası tüm coğrafi bir bölgenin kullanılamaz duruma gelir ve yine de uygulamanın sahip ve çalışır durumdadır.

Bu yük dengeleyici strateji performans avantajları da sağlayabilir. Hizmet isteği yapan cihaz en yakın veri merkezine için istekleri yönlendirebilir.

Azure'da, Azure Traffic Manager kullanarak genel yük dengelemenin avantajlarını elde edebilirsiniz.

Daha fazla bilgi edinin:

* [Traffic Manager nedir?](../traffic-manager/traffic-manager-overview.md)

## <a name="name-resolution"></a>Ad çözümlemesi

Ad çözümlemesi, tüm hizmetleri Azure'da barındırmak için önemli bir işlevdir. Güvenlik açısından bakıldığında, ad çözümlemesi işlevini güvenliğinin aşılması, sitelerden istekleri bir saldırganın sitesine yönlendirme saldırgan neden olabilir. Güvenli ad çözümlemesi, tüm barındırılan bulut Hizmetleri için bir gereksinimdir.

Ad çözümlemesi ilgilenmeniz gereken iki çeşit vardır:

* İç ad çözümlemesi. Bu, sanal ağlarınızı, şirket içi ağlarınızı ya da her ikisi de Hizmetleri tarafından kullanılır. İç ad çözümlemesi için kullanılan adları internet üzerinden erişilebilir değildir. En iyi güvenlik için iç ad çözümlemesi düzeninizi dış kullanıcılar için erişilebilir değil önemlidir.
* Dış ad çözümlemesi. Bu, şirket içi ağlarınız ve sanal ağları dışındaki kişi ve cihazlarla tarafından kullanılır. Bu, internet'e görünür olduğundan ve bulut tabanlı hizmetlerinize bağlantı yönlendirmek için kullanılan adlardır.

İç ad çözümlemesi için iki seçeneğiniz vardır:

* Bir sanal ağın DNS sunucusu. Yeni bir sanal ağ oluşturduğunuzda, bir DNS sunucusu sizin için oluşturulur. Bu DNS sunucusu, bu sanal ağ üzerinde bulunan makinelerin adlarını çözebilirsiniz. Bu DNS sunucusu yapılandırılabilir değildir, Azure fabric Yöneticisi tarafından yönetilen ve bu nedenle, ad çözümlemesi çözümünüzün güvenliğini sağlamanıza yardımcı olabilir.
* Kendi DNS sunucunuzu getirin. Kendi seçtiğiniz sanal ağınızda bir DNS sunucusuna geçirmeden seçeneğiniz vardır. Bu DNS sunucusu, DNS sunucusu veya Azure Marketi'nden edinebileceğiniz bir Azure ortağı tarafından sağlanan özel bir DNS sunucusu çözüm bir Active Directory tümleşik olabilir.

Daha fazla bilgi edinin:

* [Sanal ağa genel bakış](../virtual-network/virtual-networks-overview.md)
* [Bir sanal ağın kullandığı DNS sunucularını yönetme](../virtual-network/manage-virtual-network.md#change-dns-servers)

Dış ad çözümlemesi için iki seçeneğiniz vardır:

* Kendi dış DNS sunucusu şirket içi barındırın.
* Dış kendi DNS sunucunuzu bir hizmet sağlayıcısı ile barındırın.

Birçok büyük kuruluşlar, kendi DNS sunucuları şirket içi barındırın. Bunu yapmak için küresel bir erişim ağına ve ağ uzmanlığı olduğundan, bunu yapabilirsiniz.

Çoğu durumda, DNS ad çözümleme hizmetleri bir hizmet sağlayıcı ile barındırmanız iyidir. Bu hizmet sağlayıcıları ağ uzmanlık ve ad çözümleme hizmetleriniz için yüksek kullanılabilirlik sağlamak için küresel bir erişim ağına sahiptir. Kullanılabilirlik, ad çözümleme hizmetleri başarısız olursa, hiç kimsenin Hizmetleri, Internet'e erişmek mümkün olacağından, DNS hizmetleri için gereklidir.

Azure, bir yüksek oranda kullanılabilir ve yüksek performanslı dış DNS çözüm formundaki, Azure DNS sağlar. Bu dış ad çözümlemesi çözüm dünya çapındaki Azure DNS altyapısı yararlanır. Etki alanınızı azure'da aynı kimlik bilgileri, API'ler, Araçlar ve diğer Azure hizmetlerinde faturalama barındırmanıza olanak tanır. Azure bir parçası olarak, platformda yerleşik olarak güçlü güvenlik denetimleri de devralır.

Daha fazla bilgi edinin:

* [Azure DNS genel bakış](../dns/dns-overview.md)
* [Azure DNS özel bölgelerini](../dns/private-dns-overview.md) Azure kaynakları için özel DNS adları yerine özel bir DNS çözümü eklemek zorunda kalmadan otomatik olarak atanan adlarını yapılandırmanıza olanak tanır.

## <a name="perimeter-network-architecture"></a>Çevre ağ mimarisi

Çoğu büyük kuruluş ağlarına segmentlere ayırmak için çevre ağları kullanın ve internet ile hizmet arasında tampon bölgesi oluşturma. Ağ çevre kısmı bir düşük güvenlikli bölgenin kabul edilir ve hiçbir yüksek değerli varlıklar söz konusu ağ segmente yerleştirilir. Çevre ağ kesimindeki bir ağ arabirimine sahip ağ güvenlik cihazlar genellikle görürsünüz. Sanal makineler ve hizmetler, internet'ten gelen bağlantıları kabul edecek olan bir ağa bağlı başka bir ağ arabirimi.

Bir dizi farklı yolla Çevre ağları tasarlayabilirsiniz. Bir çevre ağına ve ardından bir kullanmaya karar verirseniz kullanmak için ne tür bir çevre ağ dağıtma kararı, ağ güvenlik gereksinimlerine bağlıdır.

Daha fazla bilgi edinin:

* [Microsoft bulut Hizmetleri ve ağ güvenliği](../best-practices-network-security.md)

## <a name="azure-ddos-protection"></a>Azure DDoS koruması

Dağıtılmış hizmet engelleme (DDoS) saldırıları, uygulamalarını buluta taşıyan müşterilerin karşılaştığı en büyük kullanılabilirlik ve güvenlik sorunlarından biridir. Bir DDoS saldırısının yasal kullanıcılara uygulamayı kullanılamaz hale getirme, uygulamanın kaynaklarını tüketebilir dener. DDoS saldırıları internet üzerinden genel olarak erişilebilen herhangi bir uç noktasını hedefleyebilir.
Microsoft olarak bilinen DDoS koruması sağlar **temel** Azure platformunun bir parçası olarak. Bu, ücretsiz olarak sunulur ve her zaman ortak ağ düzeyinde saldırı izleme ve gerçek zamanlı azaltma üzerinde içerir. DDoS koruması ile dahil korumaları yanı sıra **temel** etkinleştirebilirsiniz **standart** seçeneği. DDoS koruması standart özellikler şunlardır:

* **Yerel platform tümleştirme:** Yerel olarak Azure'a entegre. Azure portalı üzerinden yapılandırma içerir. Standart DDoS koruması, kaynaklarınızı ve kaynak yapılandırması farkındadır.
* **Kullanıma hazır koruma:** DDoS koruması standart etkin olarak Basitleştirilmiş yapılandırma, bir sanal ağ üzerindeki tüm kaynaklar hemen korur. Hiçbir müdahale veya kullanıcı tanımı gereklidir. Bunu algılandığında DDoS koruması standart anında ve otomatik olarak saldırı azaltır.
* **Her zaman açık trafik izleme:** Uygulama trafiği düzenlerinin, DDoS saldırılarının göstergelerini bakarak haftada 7 gün, günde 24 saat izlenir. Koruma ilkeleri aşıldığında azaltma gerçekleştirilir.
* **Saldırı azaltma raporları** saldırı azaltma raporları toplanan ağ akışı veri kaynaklarınıza hedeflenen saldırıları hakkında ayrıntılı bilgi sağlamak için kullanın.
* **Saldırı azaltma akış günlükleri** saldırı azaltma akış günlüklerini gözden trafiğin bırakılmasına izin iletilen trafiği ve diğer saldırı verileri neredeyse gerçek zamanlı etkin bir DDoS saldırı sırasında.
* **Uyarlamalı ayarlama:** Akıllı trafik profil oluşturma uygulamanızdaki trafiği zamanla öğrenir seçer ve hizmetiniz için en uygun profili güncelleştirir. Trafiği zamanla olan değişimini profilinin ayarlar. Katman 7 koruması için Katman 3: Tam yığın DDoS koruması, web uygulaması güvenlik duvarı ile kullanıldığında sağlar.
* **Kapsamlı azaltma Ölçek:** En büyük bilinen DDoS saldırılarına karşı korumaya yönelik genel kapasiteyle 60'tan fazla farklı saldırı türleri azaltılabilir.
* **Saldırı ölçümleri:** Her saldırılara karşı özetlenmiş ölçümler, Azure İzleyici erişilebilir.
* **Saldırı Uyarı:** Uyarılar, başlatma ve durdurma bir saldırı sırasında yapılandırılabilir ve saldırı'nın süresi boyunca yerleşik saldırı ölçümleri kullanarak. Uyarıları operasyonel yazılımınızı Microsoft Azure İzleyicisi günlükleri, Splunk, Azure depolama, e-posta ve Azure portalı gibi tümleştirin.
* **Maliyet garantisi:**  Veri aktarımı ve uygulama ölçeklendirme hizmet iadeleri belgelenmiş bir DDoS saldırıları için.
* **DDoS hızlı yanıt veren** DDoS koruması standart müşterileri artık etkin bir saldırı sırasında hızlı yanıt takım erişime sahiptir. DRR saldırı araştırma, bir saldırı ve saldırı sonrası analiz sırasında özel azaltmaları yardımcı olabilir.


Daha fazla bilgi edinin:

* [DDOS koruma genel bakış](../virtual-network/ddos-protection-overview.md)

## <a name="azure-front-door"></a>Azure ön kapısı

Azure ön kapısı hizmeti tanımlayın, yönetmek ve genel yönlendirme, web trafiğini izlemenize olanak sağlar. Bu, en iyi performans ve yüksek kullanılabilirlik için trafiğiniz ait yönlendirme iyileştirir. Azure Front Door, HTTP/HTTPS iş yükünüzü istemci IP adreslerine, ülke koduna ve http parametrelerine dayalı istismardan korumaya yönelik erişim denetimi için özel web uygulaması güvenlik duvarı (WAF) kuralları yazmanıza olanak tanır. Ayrıca, ön kapısı ayrıca hız sınırı Savaşı kötü amaçlı bot trafiği kuralları oluşturmanıza olanak sağlar, SSL yük boşaltma ve başına HTTP/HTTPS isteği içeren uygulama katmanı işleme.

Ön kapısı platform kendisi, Azure DDoS koruması temel tarafından korunur. Korumayı artırmak için, sanal ağlarınızda Azure DDoS Koruması Standart etkinleştirilebilir ve kaynakları ağ katmanı (TCP/UDP) saldırılarına karşı otomatik ayar ve risk azaltma yoluyla koruma altına alır. Ön kapısı 7. Katman ters Ara sunucu, yalnızca web trafiğinin yedeklemek için geçişine izin son sunucuları ve diğer trafik türleri varsayılan olarak engelleyin.

Daha fazla bilgi edinin:

* Azure ön tam kümesini hakkında daha fazla bilgi için özellikleri gözden geçirebileceğiniz kapı [Azure ön kapısı genel bakış](../frontdoor/front-door-overview.md)

## <a name="azure-traffic-manager"></a>Azure Traffic manager

Azure Traffic Manager, trafiği farklı Azure bölgelerindeki hizmetlere en uygun şekilde dağıtırken yüksek kullanılabilirlik ve yanıtlama hızı sağlayan DNS tabanlı bir trafik yük dengeleyicidir. Traffic Manager, trafik yönlendirme yöntemine ve uç noktaların sistem durumuna bağlı olarak istemci isteklerini en uygun hizmet uç noktasına yönlendirmek için DNS hizmetini kullanır. Uç nokta, Azure içinde veya dışında barındırılan İnternet'e yönelik bir hizmettir. Traffic manager uç noktaları izler ve kullanım dışı olan uç nokta trafiği yönlendirmek değil.

Daha fazla bilgi edinin:

* [Azure Traffic Manager'a genel bakış](../traffic-manager/traffic-manager-overview.md)

## <a name="monitoring-and-threat-detection"></a>İzleme ve tehdit algılama

Azure ile erken algılama, izleme, bu anahtar alanında yardımcı olmak için özellikleri sağlar ve ağ trafiğini toplamaya ve gözden geçirme.

### <a name="azure-network-watcher"></a>Azure Ağ İzleyicisi

Azure Ağ İzleyicisi gidermenize, yardımcı olabilir ve tüm yeni bir güvenlik sorunlarını tanımlaması yardımcı olan araçlar sağlar.

[Güvenlik grubu görünümü](../network-watcher/network-watcher-security-group-view-overview.md) sanal makinelerin güvenlik denetim ve uyumluluk ile yardımcı olur. Programlı denetimler, her sanal makineleriniz için geçerli kuralları için kuruluşunuz tarafından tanımlanan temel ilkeleri karşılaştırma gerçekleştirmek için bu özelliği kullanın. Bu, tüm yapılandırma değişikliklerini belirlemenize yardımcı olabilir.

[Paket yakalaması](../network-watcher/network-watcher-packet-capture-overview.md) sanal makineye gelen ve giden ağ trafiği yakalamanızı sağlar. Ağ istatistikleri verileri toplamak ve ağ tehditlerine araştırmada olmada çok yararlı olabilir, uygulama sorunlarını giderin. Azure işlevleri ile birlikte bu özellik, belirli Azure uyarılara yanıt olarak ağ yakalamaları başlatmak için de kullanabilirsiniz.

Ağ İzleyicisi ve bazı işlevlerini test etme, Labs'de başlatma hakkında daha fazla bilgi için bkz. [izlemeye genel bakış Azure Ağ İzleyicisi](../network-watcher/network-watcher-monitoring-overview.md).

> [!NOTE]
> Kullanılabilirlik ve bu hizmetin durumunu ilgili en güncel bildirimler için denetleme [Azure güncelleştirmeleri sayfası](https://azure.microsoft.com/updates/?product=network-watcher).

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi

Azure Güvenlik Merkezi, tehditleri önleyin, algılayın ve yardımcı olur ve artırılması, görünürlük ve denetime, Azure kaynaklarınızın güvenliğini sağlar. Azure aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar, aksi takdirde gözden kaçan geçebilir tehditleri ve çok sayıda güvenlik çözümleri ile çalışır algılamaya yardımcı olur.

Güvenlik Merkezi, en iyi duruma getirme ve ağ güvenliği tarafından izlemenize yardımcı olur:

* Ağ güvenlik önerileri sağlama.
* Ağ güvenliği yapılandırmanızı durumunu izleme.
* Uç nokta ve ağ düzeyinde hem de tehditleri tabanlı ağ için uyarı.

Daha fazla bilgi edinin:

* [Azure Güvenlik Merkezi'ne Giriş](../security-center/security-center-intro.md)

### <a name="virtual-network-tap"></a>Sanal Ağ TAP

Azure sanal ağ TAP (Terminal erişim noktası) bir ağ paketi Toplayıcı veya Analiz aracı, sanal makine ağ trafiğini sürekli akışı sağlar. Analiz aracı ve Toplayıcı, bir ağ sanal Gereci iş ortağı tarafından sağlanır. Aynı sanal ağda birden çok ağ arabirimi aynı veya farklı Aboneliklerdeki trafiğe DOKUNUN kaynak kullanabilirsiniz.

Daha fazla bilgi edinin:

* [Sanal ağ TAP](../virtual-network/virtual-network-tap-overview.md)

### <a name="logging"></a>Günlüğe kaydetme

Ağ düzeyinde günlüğe kaydetme, herhangi bir ağ güvenlik senaryo için temel bir işlevdir. Azure'da ağ düzeyinde bilgi günlük kaydı almak Nsg'ler için elde edilen bilgileri günlüğe kaydedebilirsiniz. NSG günlüğüyle bilgileri alın:

* [Etkinlik günlükleri](../azure-monitor/platform/activity-logs-overview.md). Bu günlükler, Azure aboneliklerinize gönderilen tüm işlemleri görüntülemek için kullanın. Bu günlükler varsayılan olarak etkindir ve Azure portalında kullanılabilir. Daha önce denetim veya işlem günlükleri bilinirdi.
* Olay günlükleri. Bu günlükler, hangi NSG kuralları uygulanan hakkında bilgi sağlar.
* Sayaç günlükleri. Bu günlükler her bir NSG kuralı, trafiğine izin vermek veya reddetmek için uygulanan kaç kez size sağlar.

Ayrıca [Microsoft Power BI](https://powerbi.microsoft.com/what-is-power-bi/), görüntülemek ve bu günlükleri analiz etmek için güçlü veri görselleştirme aracı.
Daha fazla bilgi edinin:

* [Ağ güvenlik grupları (Nsg'ler) için Azure izleme günlükleri](../virtual-network/virtual-network-nsg-manage-log.md)
