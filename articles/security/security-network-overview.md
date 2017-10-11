---
title: "Ağ güvenlik kavramları & Azure gereksinimleri | Microsoft Docs"
description: " Bu makalede, Microsoft Azure ağ güvenliği alanında sunmak sahip anlamak kolaylaştırır. Hangi Azure bu alanların her birinde sunmak açmıştır Çekirdek Ağ güvenlik kavramları ve gereksinimler ve bilgiler için temel açıklamaları sunuyoruz. "
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TomSh
ms.assetid: bedf411a-0781-47b9-9742-d524cf3dbfc1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: terrylan
ms.openlocfilehash: 27243856d0c6b70c7515b6bde66b99ef6160eb36
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="azure-network-security-overview"></a>Azure ağ güvenliğine genel bakış
Microsoft Azure uygulama ve hizmet bağlantı gereksinimlerini desteklemek için sağlam bir ağ altyapısı içerir. Azure'da, şirket içi arasında bulunan kaynaklar arasındaki ağ bağlantısı mümkündür ve Azure barındırılan kaynakları ve kitaplığa ve Internet ve Azure.

Bu makalede Microsoft Azure ağ güvenliği alanında sunmak sahip anlamak kolaylaştırmak için hedefidir. Burada Çekirdek Ağ güvenlik kavramları ve gereksinimleri için temel açıklamalar sağlar. Ayrıca, bilgileri ne Azure her, bu alanları yanı sıra, ilginç alanları daha derin bir anlayış kazanmanıza yardımcı olmak üzere bağlantılarını sunmak açmıştır sunuyoruz.

Azure ağ güvenliğine genel bakış makalede aşağıdaki alanlar üzerinde odaklanır:

* Azure ağı
* Ağ erişim denetimi
* Güvenli uzaktan erişim ve şirket içi bağlantı
* Kullanılabilirlik
* Ad çözümlemesi
* DMZ mimarisi
* İzleme ve tehdit algılama


## <a name="azure-networking"></a>Azure Ağı
Sanal makinelerin ağ bağlantısı gerekir. Bu gereksinimi desteklemek için sanal makinelerin bir Azure sanal ağına bağlı Azure gerektirir. Bir Azure sanal ağ üzerinde fiziksel Azure ağ doku yerleşik mantıksal bir yapıdır. Her mantıksal Azure sanal tüm diğer Azure sanal ağlardan yalıtılmış bir ağdır. Bu, ağ trafiğini dağıtımlarınızı diğer Microsoft Azure müşterilerine erişilebilir değil güvence altına yardımcı olur.

Daha fazla bilgi edinin:

* [Sanal ağ genel bakış](../virtual-network/virtual-networks-overview.md)


## <a name="network-access-control"></a>Ağ erişim denetimi
Ağ erişim denetimi belirli aygıtları veya bir Azure sanal ağ içindeki alt ağlara gelen ve giden bağlantı sınırlaması, işlemidir. Ağ erişim denetimi amacı, sanal makinelere erişimin ve Hizmetleri onaylanan kullanıcılar ve aygıtlar için çalıştırmanız gerekir. Erişim denetimleri dayalı üzerinde izin verin veya sanal makine veya hizmet gelen ve giden kararları bağlantıları için reddedin.

Azure ağ erişim denetimi gibi çeşitli türlerini destekler:

* Ağ katmanı denetimi
* Denetim yönlendirmek ve zorlanan tünel
* Sanal ağ güvenlik uygulamaları

### <a name="network-layer-control"></a>Ağ katmanı denetimi
Tüm güvenli dağıtım bazı ölçü ağ erişim denetimi gerektirir. Ağ erişim denetimi gerekli sistemleriyle sanal makine iletişim ve diğer iletişim girişimleri engellenir kısıtlamak için hedefidir.

Temel ağ düzeyinde erişim denetimi (IP adresini ve TCP veya UDP protokollerini temel alınarak) ihtiyacınız varsa, ağ güvenlik gruplarını kullanabilirsiniz. Ağ güvenlik grubu (NSG) temel bir durum bilgisi olan paket güvenlik duvarı filtreleme olduğunu ve temelinde erişimi denetlemenize olanak sağlayan bir [5-tanımlama grubu](https://www.techopedia.com/definition/28190/5-tuple). Nsg'ler erişim denetimleri kimlik doğrulamalı veya uygulama katmanı denetleme sağlamaz.

Daha fazla bilgi edinin:

* [Ağ güvenlik grupları](../virtual-network/virtual-networks-nsg.md)

### <a name="route-control-and-forced-tunneling"></a>Rota denetimi ve zorlamalı tünel
Azure sanal ağlarınızdaki yönlendirme davranışını denetlemek için bir kritik ağ güvenliği ve erişim denetimi yetenektir. Yönlendirme hatalı yapılandırıldıysa, uygulamaları ve Hizmetleri, sanal makinede barındırılan ait ve potansiyel saldırganlar tarafından işletilen sistemleri de dahil olmak üzere yetkisiz cihazlarına bağlanabilir.

Azure ağı, Azure sanal ağlarda ağ trafiğini yönlendirme davranışını özelleştirme yeteneği destekler. Bu, Azure sanal ağınızda varsayılan yönlendirme tablosu girdileri alter olanak sağlar. Yönlendirme davranışını denetimi, belirli bir aygıt veya cihaz grubunu gelen tüm trafiği girdiğinde veya sanal ağınız belirli bir konuma bırakır emin olun yardımcı olur.

Örneğin, Azure sanal ağınızda bir sanal ağ güvenlik Gereci olabilir. Azure sanal ağınızda gelen ve giden tüm trafiği, sanal güvenlik gereç yoluyla gider emin olmak istersiniz. Bunu yapılandırarak yapmak [kullanıcı tanımlı yollar](../virtual-network/virtual-networks-udr-overview.md) azure'da.

[Zorlanan tünel](https://www.petri.com/azure-forced-tunneling) hizmetlerinizi Internet'teki cihazlar için bir bağlantı başlatmak için izin verilmiyor emin olmak için kullanabileceğiniz bir mekanizmadır. Bu gelen bağlantıları kabul ettiğini ve bunlara yanıt farklı olduğunu unutmayın. Internet ana bilgisayarlarını isteklerini yanıtlamak ön uç web sunucuları gerekir ve bu nedenle Internet kaynaklanan trafiği web sunucuları yanıt izin verildiğini ve bu web sunucularına gelen izin verilir.

İzin vermek istemediğiniz bir giden talep başlatmak için bir ön uç web sunucusudur. Kötü amaçlı yazılım yüklemek için bu bağlantıları kullanılabileceği için bu tür istekleri bir güvenlik riski oluşturabilir. Giden istekler internet başlatmak için bu ön uç sunucular bile istiyorsanız, bunları filtreleme ve günlüğe kaydetme URL yararlanabilirsiniz, şirket içi web proxy'si Git zorlamak isteyebilirsiniz.

Bunun yerine, bu durumu önlemek için zorlamalı tünel kullanmak istersiniz. Zorlamalı tünel etkinleştirdiğinizde, Internet'e tüm bağlantılar, şirket içi ağ geçidi üzerinden zorlanır. Kullanıcı tanımlı yollar yararlanarak zorlamalı tünel yapılandırabilirsiniz.

Daha fazla bilgi edinin:

* [Kullanıcı tanımlı yollar ve IP iletimi nedir](../virtual-network/virtual-networks-udr-overview.md)

### <a name="virtual-network-security-appliances"></a>Sanal ağ güvenlik uygulamaları
Ağ güvenlik grupları, kullanıcı tanımlı yollar ve zorlamalı tünel ağ ve Aktarım katmanı güvenlik düzeyini sunarken [OSI modeli](https://en.wikipedia.org/wiki/OSI_model), daha yüksek düzeyde güvenlik etkinleştirmek istediğiniz zaman zamanlar olabilir Ağ.

Örneğin, güvenlik gereksinimlerinizi şunlar olabilir:

* Kimlik doğrulama ve yetkilendirme, uygulamanıza erişime izin vermeden önce
* İzinsiz giriş algılama ve yetkisiz erişim yanıtı
* Üst düzey protokoller için uygulama katmanı denetleme
* URL filtreleme
* Ağ düzeyinde virüsten koruma ve kötü amaçlı yazılımdan koruma
* Koruma bot koruma
* Uygulama erişim denetimi
* Ek DDoS Koruması (yukarıda Azure doku koruması sağlanan DDoS)

Bir Azure iş ortağı çözümü kullanarak bu Gelişmiş ağ güvenliği özelliklere erişebilir. En güncel Azure iş ortağı ağı güvenlik çözümleri ziyaret ederek bulabileceğiniz [Azure Marketi](https://azure.microsoft.com/marketplace/) ve "güvenlik" ve "ağ güvenliği" için arama

## <a name="secure-remote-access-and-cross-premises-connectivity"></a>Güvenli uzaktan erişim ve şirket içi ve dışı bağlantı
Kurulum, yapılandırma ve yönetimi için Azure kaynaklarını uzaktan yapılması gerekiyor. Ayrıca, dağıtmak isteyebilirsiniz [karma BT](http://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) içi bileşenleri çözümleri ve Azure genel bulutunda. Bu senaryolar güvenli uzaktan erişim gerektirir.

Azure ağı aşağıdaki güvenli uzaktan erişim senaryoları destekler:

* Ayrı iş istasyonları bir Azure sanal ağa bağlan
* Bir Azure sanal ağ VPN ile şirket içi ağınıza bağlanmak
* Bir Azure sanal ağı ayrılmış bir WAN bağlantısı ile şirket içi ağınıza bağlanmak
* Azure sanal ağları birbirine bağlayan

### <a name="connect-individual-workstations-to-an-azure-virtual-network"></a>Azure sanal ağını ayrı ayrı iş istasyonları Bağlan
Sanal makineler ve Azure hizmetleri yönetmek tek tek geliştiriciler veya işlemleri personel etkinleştirmek istediğiniz zaman zamanlar olabilir. Örneğin, bir Azure sanal ağ üzerindeki bir sanal makineye erişmesi ve güvenlik ilkeniz ayrı sanal makinelere RDP veya SSH uzaktan erişim izin vermiyor. Bu durumda, bir noktadan siteye VPN bağlantısı kullanabilirsiniz.

Noktadan siteye VPN bağlantısı kullanır [SSTP VPN](https://technet.microsoft.com/library/cc731352.aspx) protokolü, kullanıcı ve Azure sanal ağ arasında özel ve güvenli bir bağlantı ayarlamanıza olanak verir. VPN bağlantısı kurulduktan sonra (kullanıcı kimlik doğrulaması yapabilir ve yetkili varsayılarak) kullanıcı RDP veya SSH Azure sanal ağ üzerindeki herhangi bir sanal makine içine VPN bağlantısı üzerinden açabilecektir.

Daha fazla bilgi edinin:

* [PowerShell kullanarak sanal bir ağa noktadan siteye bağlantı yapılandırma](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="connect-your-on-premises-network-to-an-azure-virtual-network-with-a-vpn"></a>Bir Azure sanal ağ VPN ile şirket içi ağınıza bağlanmak
Tüm şirket ağı veya bölümleri, bir Azure sanal ağınıza bağlamak isteyebilirsiniz. Bu karma BT ortak senaryolar burada şirketler [Azure'da kendi şirket içi veri merkezine genişletmek](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84). Çoğu durumda şirketler Azure bölümleri şirket içi ve Azure ve arka uç veritabanı şirket içi bir çözüm ön uç web sunucuları ne zaman içerir gibi hizmet bölümlerini barındırır. Bu tür "şirket içi" bağlantılar da bulunan Azure Yönetimi olun kaynakları daha güvenli ve Azure Active Directory etki alanı denetleyicileri genişletme gibi senaryolar etkinleştirin.

Tek yönlü gerçekleştirmek için bu kullanmaktır bir [siteden siteye VPN](https://www.techopedia.com/definition/30747/site-to-site-vpn). Siteden siteye VPN ve noktadan siteye VPN arasında bir siteden siteye VPN Azure sanal ağı için tüm bir ağa (örneğin, şirket içi ağınıza) bağlanırken bir noktadan siteye VPN tek bir cihazı bir Azure sanal ağa bağlandığını farktır. Siteden siteye VPN Azure sanal ağını yüksek güvenlikli IPSec tünel modu VPN protokolü kullanın.

Daha fazla bilgi edinin:

* [Azure Portalı'nı kullanarak siteden siteye VPN bağlantısı olan bir Resource Manager Vnet'i oluşturma](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [VPN ağ geçidi için planlama ve tasarım](../vpn-gateway/vpn-gateway-plan-design.md)

### <a name="connect-your-on-premises-network-to-an-azure-virtual-network-with-a-dedicated-wan-link"></a>Ayrılmış bir WAN bağlantısı sahip Azure sanal ağı şirket içi ağınıza bağlanmak
Noktadan siteye ve siteden siteye VPN bağlantıları, şirket içi bağlantılar etkinleştirmek için geçerlidir. Ancak, bazı kuruluşlar onlara aşağıdaki dezavantajları göz önünde bulundurun:

* VPN bağlantıları Internet üzerinden veri taşıma – bu ortak bir ağ üzerinden veri taşıma ile ilgili olası güvenlik sorunlarını için bu bağlantıları gösterir. Ayrıca, güvenilirlik ve Internet bağlantıları için kullanılabilirlik garanti edilemez.
* Bazı uygulamalar ve olarak yaklaşık 200 MB/sn hızında max çıkışı amaçlar için bant genişliği kısıtlı Azure sanal ağlara VPN bağlantıları kabul.

Yüksek düzeyde güvenlik ve kullanılabilirlik genellikle kendi şirket içi bağlantılar için gereken kuruluşlar uzaktan sitelere bağlanmak için ayrılmış WAN bağlantıları kullanın. Azure bir Azure sanal ağı şirket içi ağınıza bağlamak için kullanabileceğiniz ayrılmış bir WAN bağlantısı kullanma yeteneği sağlar. Bu Azure ExpressRoute aracılığıyla etkinleştirilir.

Daha fazla bilgi edinin:

* [ExpressRoute teknik genel bakış](../expressroute/expressroute-introduction.md)

### <a name="connect-azure-virtual-networks-to-each-other"></a>Azure sanal ağları birbirine bağlayan
Dağıtımlar için çok sayıda Azure sanal ağlar kullanmanızı mümkündür. Neden bunu yapabilirsiniz pek çok neden vardır. Nedenlerinden biri, yönetimi basitleştirmek için olabilir; başka bir güvenlik nedenleriyle olabilir. Motivasyon veya kaynakları farklı Azure sanal ağlarda yerleştirme stratejinin bağımsız olarak birbiriyle bağlanmak için ağların her biri kaynakları istediğinizde zamanlar olabilir.

Hizmetler bir Azure sanal ağdaki başka bir Azure sanal ağ hizmetleri "geri döngü yapmasıyla" bağlanmak için bir seçenek olacaktır Internet üzerinden. Bağlantı, bir Azure sanal ağ üzerinde Internet üzerinden gidin ve hedef Azure sanal ağ için geri dönün. Bu seçenek tüm Internet tabanlı iletişim devralınan güvenlik sorunları bağlantısı kullanıma sunar.

Bir Azure sanal ağı Azure sanal ağ siteden siteye VPN oluşturmak için daha iyi bir seçenek olabilir. Bu Azure sanal ağı Azure sanal ağ siteden siteye VPN aynı kullanan [IPSec tünel modu](https://technet.microsoft.com/library/cc786385.aspx) Protokolü yukarıda belirtilen şirket içi siteden siteye VPN bağlantısı olarak.

Bir Azure sanal ağı Azure sanal ağ siteden siteye VPN kullanmanın avantajı, VPN bağlantısı Internet üzerinden bağlanma yerine Azure ağ yapısı üzerinden kurulur ' dir. Bu fazladan bir Internet üzerinden bağlanma siteden siteye VPN ile karşılaştırıldığında güvenlik katmanı sağlar.

Daha fazla bilgi edinin:

* [Azure Resource Manager ve PowerShell kullanarak VNet-VNet bağlantı yapılandırma](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="availability"></a>Kullanılabilirlik
Kullanılabilirlik herhangi bir güvenlik programı anahtar bir bileşenidir. Bunlar ağ üzerinden erişmesi gereken kullanıcılar ve sistemlerden erişemiyorsanız hizmet sayılabileceğini tehlikeye. Azure aşağıdaki yüksek kullanılabilirlik mekanizmaları ağ teknolojileri sahiptir:

* HTTP tabanlı Yük Dengeleme
* Ağ düzeyinde Yük Dengeleme
* Genel Yük Dengeleme

Yük Dengeleme, birden çok aygıt arasındaki bağlantıları eşit olarak dağıtmak için tasarlanmış bir mekanizmadır. Yük Dengelemesi hedefleri şunlardır:

* Kullanılabilirlik – Bakiye bağlantıları birçok cihaz arasında yük, bir veya daha fazla aygıtları kullanılamaz hale gelebilir ve kalan çevrimiçi cihazlarda çalışan hizmetleri hizmetinden içeriğe hizmet edecek şekilde devam edebilirsiniz artırın
* Performansı artırmak – birden çok cihazda Bakiye bağlantıları yüklediğinizde, tek bir cihazı işlemci isabet yapılacak sahip değil. Bunun yerine, içeriği sunması için işleme ve bellek taleplerini yayılır birçok cihaz arasında.

### <a name="http-based-load-balancing"></a>HTTP tabanlı Yük Dengeleme
Bir HTTP tabanlı yük dengeleyici önünde yeterli düzeyde performans ve yüksek kullanılabilirlik güvence altına yardımcı olması için bu web Hizmetleri web tabanlı hizmetler genellikle çalıştırdığı kuruluşlar işlemleriniz. Geleneksel ağ tabanlı yük dengeleyici aksine Yük Dengeleme HTTP tabanlı tarafından kararlara değil ağ ve Aktarım katmanı protokolleri, HTTP protokolünü özelliklerini yük dengeleyici dayanır.

HTTP tabanlı Yük Dengeleme için web tabanlı hizmetleri sağlamak için Azure, Azure uygulama ağ geçidi sağlar. Azure uygulama ağ geçidi destekler:

* HTTP tabanlı yük dengelemesi – Yük Dengeleme kararları yapılma tabanlı karakteristiğini üzerinde özel HTTP protokolü ile
* Tanımlama bilgisi tabanlı oturum benzeşimi – bu özellik bu yük dengeleyici arkasında sunuculardan birine kurulan bağlantılar istemci ve sunucu arasında değişmeden kalmasını emin olur. Bu işlemler kararlılığını oluşturmasını sağlar.
* Bir istemci bağlantı kurulur zaman olan yük dengeleyici, yük dengeleyici ile istemci arasında oturum HTTPS kullanılarak şifrelenir SSL boşaltma – (SSL /) protokolü. Ancak, performansı artırmak için yük dengeleyici ve yük dengeleyici kullanın (şifrelenmemiş) HTTP protokolünü arkasında web sunucusu arasında bağlantınız seçeneğiniz vardır. "SSL boşaltma" web sunucuları yük dengeleyicinin arkasındaki şifrelemeyle ilgili işlemci yükü deneyimi yoktur ve bu nedenle daha hızlı bir şekilde hizmet isteklerine gerekir çünkü bu adı verilir.
* URL tabanlı içerik yönlendirme – bu özellik, hedef URL temel alınarak bağlantıları iletmek nerede hakkında kararlar almak yük dengeleyici için mümkün kılar. Bu, Yük Dengeleme kararları IP adreslerine dayalı olun çözümleri çok daha fazla esneklik sağlar.

Daha fazla bilgi edinin:

* [Uygulama ağ geçidi'ne genel bakış](../application-gateway/application-gateway-introduction.md)

### <a name="network-level-load-balancing"></a>Ağ düzeyinde Yük Dengeleme
HTTP tabanlı Yük Dengeleme aksine, ağ düzeyinde Yük Dengelemesi, IP adresi ve bağlantı noktası (TCP veya UDP) sayılara göre Yük Dengeleme kararlarını verir.
Azure'daki Azure yük dengeleyici kullanarak ağ düzeyinde Yük Dengeleme avantajları elde. Azure yük dengeleyici anahtar bazı özellikleri içerir:

* IP adresi ve bağlantı noktası numaralarını temel ağ düzeyinde Yük Dengelemesi
* Tüm uygulama katmanı protokol desteği
* Azure sanal makinelerine yükünü dengeleyen ve rol örnekleri bulut Hizmetleri
* İnternete dönük (Dış Yük Dengeleme) ve Internet olmayan için kullanılabilir (iç Yük Dengeleme) uygulamalar ve sanal makineleri karşılıklı
* Yük dengeleyicinin arkasındaki hizmetlerden herhangi biri kullanılamaz duruma olmadığını belirlemek için kullanılan, uç noktası izleme

Daha fazla bilgi edinin:

* [Birden çok Sanal Makine veya hizmet arasında İnternet’e yönelik yük dengeleyici](../load-balancer/load-balancer-internet-overview.md)
* [İç yük dengeleyiciye genel bakış](../load-balancer/load-balancer-internal-overview.md)

### <a name="global-load-balancing"></a>Genel Yük Dengeleme
Bazı kuruluşlar, yüksek düzeyde kullanılabilirlik olası isteyeceksiniz. Bu hedefe ulaşmak için bir genel olarak dağıtılmış veri merkezlerinde uygulamalarını barındırmasını yoludur. Bir uygulama, dünyanın her yerinde bulunan veri merkezlerinde barındırıldığında kullanılamaz hale ve uygulama hala sahip tüm bir coğrafi bölge için olası ve çalışır durumdadır.

Genel olarak dağıtılmış veri merkezlerinde uygulamaları barındırarak alma kullanılabilirlik avantajlara ek olarak, performans avantajı da alabilirsiniz. Bu performans avantajı istekleri için hizmet isteği yapan aygıt en yakın veri merkezi için yönlendiren bir mekanizma kullanılarak elde edilebilir.

Genel Yük Dengeleme avantajlar hem de sağlayabilir. Azure'da, Azure trafik Yöneticisi'ni kullanarak genel Yükü Dengeleme faydaları elde edebilirsiniz.

Daha fazla bilgi edinin:

* [Traffic Manager nedir?](../traffic-manager/traffic-manager-overview.md)


## <a name="name-resolution"></a>Ad çözümlemesi
Ad çözümlemesi, tüm hizmetleri Azure'da barındırmak için kritik bir işlevdir. Güvenlik açısından bakıldığında, ad çözümlemesi işlevi bir saldırganın bir saldırganın site sitelerinizi isteklerini yeniden yönlendirmek için bırakılmasına neden olabilir. Güvenli ad çözümlemesi tüm barındırılan bulut Hizmetleri için gerekli değildir.

Ad çözümlemesi ele almanız gereken iki tür vardır:

* Dahili ad çözümlemesi – dahili ad çözümlemesi, Azure sanal ağlar, şirket içi ağlarınızı ya da her ikisini de Hizmetleri tarafından kullanılır. İç ad çözümlemesi için kullanılan adları Internet üzerinden erişilebilir değildir. En iyi güvenlik için iç ad çözümlemesi düzeninizi dış kullanıcılar için erişilebilir değil önemlidir.
* Dış ad çözümlemesi – dış ad çözümlemesi, insanların ve cihazların şirket içi ve Azure sanal ağlar dışında tarafından kullanılır. Bunlar Internet'e görünür olan ve doğrudan bulut tabanlı hizmetler için bağlantı için kullanılan adlardır.

Dahili ad çözümlemesi için iki seçeneğiniz vardır:

* Yeni bir Azure sanal ağ, oluşturduğunuzda, bir Azure sanal ağ DNS sunucusu – bir DNS sunucusu sizin için oluşturulur. Bu DNS sunucusu, Azure sanal ağınızda bulunan makineler adlarını çözümleyebilir. Bu DNS sunucusu yapılandırılabilir değildir ve dolayısıyla güvenli ad çözümlemesi çözüm yapmadan Azure doku Yöneticisi tarafından yönetilir.
* DNS sunucunuzun Getir – kendi Azure sanal ağınızda seçme bir DNS sunucusu koyma seçeneğiniz vardır. Bu DNS sunucusu, DNS sunucusu veya Azure Marketi'nden edinebileceğiniz bir Azure ortak tarafından sağlanan adanmış bir DNS sunucusu çözümü bir Active Directory tümleşik olabilir.

Daha fazla bilgi edinin:

* [Sanal ağ genel bakış](../virtual-network/virtual-networks-overview.md)
* [Bir sanal ağ (VNet) tarafından kullanılan DNS sunucularını yönetin](../virtual-network/virtual-network-manage-network.md#dns-servers)

Dış DNS çözümlemesi için iki seçeneğiniz vardır:

* Kendi dış DNS sunucusu şirket içi ana bilgisayar
* Kendi dış DNS sunucusu hizmeti sağlayıcısı ile ana bilgisayar

Birçok büyük kuruluşların kendi DNS sunucuları şirket içi barındırır. Bunu yapmak için genel durum ve ağ uzmanlığa sahip oldukları için bunu.

Çoğu durumda, bir hizmet sağlayıcısı ile DNS ad çözümleme hizmetleri barındırmak daha iyi olur. Bu hizmet sağlayıcıları, ad çözümleme hizmetleri çok yüksek kullanılabilirliğini sağlamak için genel durum ve ağ uzmanlığa sahip. Ad Çözümleme Hizmetleri başarısız olursa, hiç kimse Hizmetleri, Internet'e ulaşabilmesi olacağı için kullanılabilirlik DNS hizmetleri için gereklidir.

Azure sağlar, yüksek oranda kullanılabilir ve kullanıcı dış DNS çözüm Azure DNS biçiminde. Bu dış ad çözümlemesi çözüm dünya çapında Azure DNS altyapısı yararlanır. Azure aynı kimlik bilgileri, API ' larını araçları kullanarak ve diğer Azure hizmetlerinizi faturalama etki alanınızın barındırmanıza olanak sağlar. Azure bir parçası olarak platformu ile oluşturulmuş güçlü güvenlik denetimleri de devralır.

Daha fazla bilgi edinin:

* [Azure DNS'ye genel bakış](../dns/dns-overview.md)

## <a name="dmz-architecture"></a>DMZ mimarisi
Çoğu işletmenin DMZ'ler Internet ve kendi hizmetleri arasında bir arabellek bölgesi oluşturmak için ağlarını segmentlere ayırmak için kullanın. Ağ DMZ kısmı düşük güvenlikli bölgenin olarak kabul edilir ve hiçbir yüksek değerli varlıklar, ağ kesimine yerleştirilir. Genellikle, bir ağ arabirimi DMZ kesimindeki ve sanal makineleri ve Internet'ten gelen bağlantıları kabul Hizmetleri olan bir ağa bağlı başka bir ağ arabirimine sahip ağ güvenlik aygıtları görürsünüz.

Bir dizi DMZ tasarım ve bir DMZ dağıtma kararı çeşidi vardır ve DMZ, kullanmaya karar verirseniz kullanmak için ne tür ağ güvenlik gereksinimlerinize göre temel sonra.

Daha fazla bilgi edinin:

* [Microsoft bulut Hizmetleri ve ağ güvenliği](../best-practices-network-security.md)


## <a name="monitoring-and-threat-detection"></a>İzleme ve tehdit algılama

Ağ trafiği izleme ve toplama ve gözden geçirmek için özelliği, Azure ile erken algılama bu anahtar alanında yardımcı olmak için özellikleri sağlar.

### <a name="azure-network-watcher"></a>Azure Ağ İzleyicisi
Azure Ağ İzleyicisi çok sayıda gidermeye yardımcı olarak yardımcı olan araçlar yepyeni bir dizi güvenlik sorunları tanımlaması sağlamak özellikleri içerir.

[Güvenlik grubu görünümü ](/network-watcher/network-watcher-security-group-view-overview.md) ve her Vm'leriniz için etkili kuralları, kuruluşunuz tarafından tanımlanan temelleri ilkeleri karşılaştırma programlı denetimleri gerçekleştirmek için kullanılan sanal makineleri denetim ve güvenlik uyumluluğunu ile yardımcı olur. Bu, tüm yapılandırma değişikliklerini belirlemenize yardımcı olabilir.

[Paket yakalama](/network-watcher/network-watcher-packet-capture-overview.md) ve sanal makineden ağ trafiğini yakalamanıza olanak sağlar. Ağ istatistikleri toplamak sağlayarak ve uygulama sorunlarını giderme konusunda yardımcı yanı sıra paket yakalama ağ yetkisiz araştırma içinde çok yararlı olabilir. Azure işlevleri ile birlikte bu işlevselliği, belirli Azure uyarılara yanıt olarak ağ yakalamaları başlatmak için de kullanabilirsiniz.

Azure Ağ İzleyicisi'ni ve test başlatma hakkında daha fazla bilgi için bazı işlevlerini, ortamlarındaki göz atın [Azure Ağ İzleyicisi izlemeye genel bakış](/network-watcher/network-watcher-monitoring-overview.md)

>[!NOTE]
Azure Ağ İzleyicisi hala genel önizlemede olduğundan, genel kullanılabilirlik hizmetlerini serbest olarak aynı düzeyde kullanılabilirlik ve güvenilirlik olmayabilir. Belirli özellikler desteklenmiyor olabilir, yetenekleri kısıtlı ve tüm Azure konumlarda kullanılamayabilir. En güncel bildirimlerinin kullanılabilirlik ve bu hizmetin durumunu denetleme [Azure güncelleştirmeler sayfası](https://azure.microsoft.com/updates/?product=network-watcher)

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi
Güvenlik Merkezi, engellemenize, algılamanıza ve tehditlerine karşı yanıt yardımcı olur ve artan, görünürlük ve denetim üzerinden, Azure kaynaklarınızın güvenliğini sağlar. Azure aboneliklerinize arasında tümleşik güvenlik izleme ve ilke yönetimi sağlar, kaçabilecek tehditleri ve çok sayıda güvenlik çözümleri çalışır algılamaya yardımcı olur.

Azure Güvenlik Merkezi en iyi duruma getirme ve ağ güvenliği tarafından izlemenize yardımcı olur:

* Ağ güvenlik önerileri sağlama
* Ağ güvenliği yapılandırması durumunu izleme
* Temel ağ tehditlerinden her iki uç ve ağ düzeyinde konusunda sizi uyarmayı

Daha fazla bilgi edinin:

* [Azure Güvenlik Merkezi'ne giriş](../security-center/security-center-intro.md)


### <a name="logging"></a>Günlüğe kaydetme
Ağ düzeyinde günlüğe kaydetme, bir ağ güvenlik senaryonuz için anahtar bir işlevdir. Azure üzerinde ağ bilgilerini günlüğe kaydetme düzeyi almak ağ güvenlik grupları için elde edilen bilgilerle oturum açabilir. NSG günlüğüyle bilgileri alın:

* [Etkinlik günlükleri](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) – Bu günlükler, Azure aboneliklerinize gönderilen tüm işlemleri görüntülemek için kullanılır. Bu günlükler varsayılan olarak etkindir ve Azure portalı içinde kullanılabilir. Daha önce "Denetim günlüklerini" veya "İşletimsel Logs" olarak bilinirdi.
* Olay günlükleri – Bu günlükler hangi NSG kuralları uygulanan hakkında bilgi sağlar.
* Sayaç günlükleri – Bu günlükler, her NSG kural Reddet izin verme veya trafiği uygulandığı kaç kez bilmeniz olanak tanır.

Aynı zamanda [Microsoft Power BI](https://powerbi.microsoft.com/what-is-power-bi/), görüntülemek ve bu günlükler çözümlemek için güçlü veri görselleştirme aracı.

Daha fazla bilgi edinin:

* [Ağ güvenlik grupları (Nsg'ler) için günlük analizi](../virtual-network/virtual-network-nsg-manage-log.md)
