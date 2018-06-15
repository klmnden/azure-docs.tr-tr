---
title: Ağ güvenlik kavramları ve Azure gereksinimleri | Microsoft Docs
description: Bu makale, hangi Azure bu alanların her birinde sunar üzerinde ağ güvenlik kavramları ve gereksinimler ve bilgiler hakkında temel açıklamalar sağlar.
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
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: fbd589aedb955ee4bd61dc0ec754d8713a98179a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34365636"
---
# <a name="azure-network-security-overview"></a>Azure ağ güvenliğine genel bakış
Azure uygulama ve hizmet bağlantı gereksinimlerini desteklemek için sağlam bir ağ altyapısı içerir. Azure'da, şirket içi arasında bulunan kaynaklar arasındaki ağ bağlantısı mümkündür ve Azure barındırılan kaynakları ve kitaplığa ve Internet ve Azure.

Bu makalede ne Azure ağ güvenliği alanında sunar açıklamak için hedefidir. Çekirdek Ağ güvenlik kavramları ve gereksinimleri hakkındaki temel açıklamalarının yanı sıra hakkında bilgi alabilirsiniz:

* Azure ağı
* Ağ erişim denetimi
* Güvenli uzaktan erişim ve şirket içi bağlantı
* Kullanılabilirlik
* Ad çözümlemesi
* Çevre ağı (DMZ) mimarisi
* İzleme ve tehdit algılama

## <a name="azure-networking"></a>Azure ağı
Sanal makinelerin ağ bağlantısı gerekir. Bu gereksinimi desteklemek için Azure sanal ağa bağlanması için sanal makineleri Azure gerektirir. Bir sanal ağ üzerinde fiziksel Azure ağ doku yerleşik mantıksal bir yapıdır. Her mantıksal sanal diğer tüm sanal ağlardan yalıtılmış bir ağdır. Bu, ağ trafiğini dağıtımlarınızı diğer Azure müşterilerine erişilebilir değil sağlamaya yardımcı olur.

Daha fazla bilgi edinin:

* [Sanal ağ genel bakış](../virtual-network/virtual-networks-overview.md)


## <a name="network-access-control"></a>Ağ erişim denetimi
Ağ erişim denetimi belirli aygıtları veya bir sanal ağ içindeki alt ağlara gelen ve giden bağlantı sınırlaması, işlemidir. Ağ erişim denetimi amacı, sanal makinelere erişimin ve Hizmetleri onaylanan kullanıcılar ve aygıtlar için çalıştırmanız gerekir. Erişim denetimleri veya sanal makine veya hizmet bağlantılarını reddetmek için kararları dayanır.

Azure gibi ağ erişim denetimi, çeşitli türlerini destekler:

* Ağ katmanı denetimi
* Denetim yönlendirmek ve zorlanan tünel
* Sanal ağ güvenlik uygulamaları

### <a name="network-layer-control"></a>Ağ katmanı denetimi
Tüm güvenli dağıtım bazı ölçü ağ erişim denetimi gerektirir. Ağ erişim denetimi amacı gerekli sistemleriyle sanal makine iletişim kısıtlamaktır. Diğer iletişim girişimleri engellenir.

Temel ağ düzeyinde erişim denetimi (IP adresini ve TCP veya UDP protokollerini temel alınarak) gerekiyorsa, ağ güvenlik grupları (Nsg'ler) kullanabilirsiniz. Bir NSG'yi bir basic, durum bilgisi olan, paket güvenlik duvarı filtreleme, ve temelinde erişimi denetlemenize olanak sağlayan bir [5-tanımlama grubu](https://www.techopedia.com/definition/28190/5-tuple). Nsg'ler erişim denetimleri kimlik doğrulamalı veya uygulama katmanı denetleme sağlamaz.

Daha fazla bilgi edinin:

* [Ağ güvenlik grupları](../virtual-network/security-overview.md)

### <a name="route-control-and-forced-tunneling"></a>Denetim yönlendirmek ve zorlanan tünel
Sanal ağlarınızdaki yönlendirme davranışını denetleme olanağı önemlidir. Yönlendirme hatalı yapılandırıldıysa, uygulamaları ve Hizmetleri, sanal makinede barındırılan ait ve potansiyel saldırganlar tarafından işletilen sistemleri yetkisiz aygıtlar bağlanabilir.

Azure ağı, sanal ağlarda ağ trafiğini yönlendirme davranışını özelleştirme yeteneği destekler. Bu, sanal ağınızda tablo girişleri yönlendirme varsayılan alter olanak sağlar. Yönlendirme davranışını denetimi, belirli bir aygıt veya cihaz grubunu gelen tüm trafiği girdiğinde veya sanal ağınız belirli bir konuma bırakır emin olun yardımcı olur.

Örneğin, sanal ağınızda bir sanal ağ güvenlik Gereci olabilir. Sanal ağınızı gelen ve giden tüm trafiği, sanal güvenlik gereç yoluyla gider emin olmak istersiniz. Bunu yapılandırarak yapmak [kullanıcı tanımlı yollar](../virtual-network/virtual-networks-udr-overview.md) (Udr'ler) azure'da.

[Zorlanan tünel](https://www.petri.com/azure-forced-tunneling) hizmetlerinizi Internet'teki cihazlar için bir bağlantı başlatmak için izin verilmiyor emin olmak için kullanabileceğiniz bir mekanizmadır. Bu gelen bağlantıları kabul ettiğini ve bunlara yanıt farklı olduğunu unutmayın. İnternet ana bilgisayarlarını isteklerini yanıtlamak ön uç web sunucuları gerekir ve bu nedenle Internet kaynaklanan trafiği web sunucuları yanıt izin verildiğini ve bu web sunucularına gelen izin verilir.

İzin vermek istemediğiniz bir giden talep başlatmak için bir ön uç web sunucusudur. Bu tür istekleri, çünkü bu bağlantılar kötü amaçlı yazılım yüklemek için kullanılabilir bir güvenlik riski temsil edebilir. Giden istekler internet başlatmak için bu ön uç sunucular istediğiniz olsa bile, bunları, şirket içi web proxy'si Git zorlamak isteyebilirsiniz. Bu, filtreleme ve günlüğe kaydetme URL yararlanmak sağlar.

Bunun yerine, bu durumu önlemek için zorlamalı tünel kullanmak istersiniz. Zorlamalı tünel etkinleştirdiğinizde, Internet'e tüm bağlantılar, şirket içi ağ geçidi üzerinden zorlanır. Zorlamalı tünel Udr'ler yararlanarak yapılandırabilirsiniz.

Daha fazla bilgi edinin:

* [Kullanıcı tanımlı yollar ve IP iletimi nedir](../virtual-network/virtual-networks-udr-overview.md)

### <a name="virtual-network-security-appliances"></a>Sanal ağ güvenlik uygulamaları
Nsg'ler, Udr'ler ve zorlamalı tünel ağ ve Aktarım katmanı güvenlik düzeyini sunarken [OSI modeli](https://en.wikipedia.org/wiki/OSI_model), ağ yüksek düzeyde güvenlik etkinleştirmek isteyebilirsiniz.

Örneğin, güvenlik gereksinimlerinizi şunlar olabilir:

* Kimlik doğrulama ve yetkilendirme, uygulamanıza erişime izin vermeden önce
* İzinsiz giriş algılama ve yetkisiz erişim yanıtı
* Üst düzey protokoller için uygulama katmanı denetleme
* URL filtreleme
* Ağ düzeyinde virüsten koruma ve kötü amaçlı yazılımdan koruma
* Koruma bot koruma
* Uygulama erişim denetimi
* Ek DDoS Koruması (yukarıda Azure fabric tarafından kendisine sağlanan DDoS koruması)

Bir Azure iş ortağı çözümü kullanarak bu Gelişmiş ağ güvenliği özelliklere erişebilir. En güncel Azure iş ortağı ağı güvenlik çözümleri ziyaret ederek bulabileceğiniz [Azure Marketi](https://azure.microsoft.com/marketplace/)ve "güvenlik" ve "ağ güvenliği" için arama

## <a name="secure-remote-access-and-cross-premises-connectivity"></a>Güvenli uzaktan erişim ve şirket içi bağlantı
Kurulum, yapılandırma ve yönetimi için Azure kaynaklarını uzaktan yapılması gerekiyor. Ayrıca, dağıtmak isteyebilirsiniz [karma BT](http://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) içi bileşenleri çözümleri ve Azure genel bulutunda. Bu senaryolar güvenli uzaktan erişim gerektirir.

Azure ağı aşağıdaki güvenli uzaktan erişim senaryoları destekler:

* Ayrı iş istasyonları sanal bir ağa bağlan
* Şirket içi ağınıza bir VPN ile sanal bir ağa bağlan
* Şirket içi ağınızdaki ayrılmış bir WAN bağlantısı ile sanal bir ağa bağlayın
* Sanal ağları birbirine bağlayan

### <a name="connect-individual-workstations-to-a-virtual-network"></a>Ayrı iş istasyonları sanal bir ağa bağlan
Her bir geliştirici veya işlemleri kişisel sanal makineler ve Azure hizmetleri yönetmek etkinleştirmek isteyebilirsiniz. Örneğin, bir sanal ağ üzerindeki bir sanal makineye erişmeniz varsayalım. Ancak, güvenlik ilkeniz ayrı sanal makinelere RDP veya SSH uzaktan erişim izin vermiyor. Bu durumda, bir noktadan siteye VPN bağlantısı kullanabilirsiniz.

Noktadan siteye VPN bağlantısı kullanır [SSTP VPN](https://technet.microsoft.com/library/cc731352.aspx) protokolü, kullanıcı ve sanal ağ arasında özel ve güvenli bir bağlantı ayarlamanıza olanak verir. VPN bağlantısı kurulduktan sonra kullanıcı açabilir RDP veya SSH VPN bağlantısı üzerinden sanal ağda herhangi bir sanal makine içine. (Bu kullanıcı kimlik doğrulaması yapabilir ve yetkili varsayar.)

Daha fazla bilgi edinin:

* [PowerShell kullanarak bir sanal ağa noktadan siteye bağlantı yapılandırma](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="connect-your-on-premises-network-to-a-virtual-network-with-a-vpn"></a>Şirket içi ağınıza bir VPN ile sanal bir ağa bağlan
Tüm şirket ağı veya bölümleri, bir sanal ağa bağlamak isteyebilirsiniz. Bu karma BT ortak senaryolar, burada kuruluşlar [Azure'da kendi şirket içi veri merkezine genişletmek](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84). Çoğu durumda, kuruluşların Azure ve bölümleri şirket içi hizmetinde bölümlerini barındırır. Örneğin, bir çözüm ön uç web sunucuları Azure'da içerir ve şirket içi uç veritabanları bunu. Bu tür "şirket içi" bağlantılar da bulunan Azure Yönetimi olun kaynakları daha güvenli ve Azure Active Directory etki alanı denetleyicileri genişletme gibi senaryolar etkinleştirin.

Tek yönlü gerçekleştirmek için bu kullanmaktır bir [siteden siteye VPN](https://www.techopedia.com/definition/30747/site-to-site-vpn). Siteden siteye VPN ve noktadan siteye VPN arasındaki fark, ikincisi tek bir sanal ağa bağlandığını ' dir. Siteden siteye VPN tüm bir ağa (örneğin, şirket içi ağınıza) bir sanal ağa bağlanır. Bir sanal ağ için siteden siteye VPN yüksek güvenlikli IPSec tünel modu VPN protokolü kullanın.

Daha fazla bilgi edinin:

* [Azure portalını kullanarak siteden siteye VPN bağlantısı olan bir Resource Manager Vnet'i oluşturma](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [VPN ağ geçidi için planlama ve tasarım](../vpn-gateway/vpn-gateway-plan-design.md)

### <a name="connect-your-on-premises-network-to-a-virtual-network-with-a-dedicated-wan-link"></a>Şirket içi ağınızdaki ayrılmış bir WAN bağlantısı ile sanal bir ağa bağlayın
Noktadan siteye ve siteden siteye VPN bağlantıları, şirket içi bağlantılar etkinleştirmek için geçerlidir. Ancak, bazı kuruluşlar onlara aşağıdaki dezavantajları göz önünde bulundurun:

* VPN bağlantıları Internet üzerinden verileri taşıyın. Bu, ortak bir ağ üzerinden veri taşıma ile ilgili olası güvenlik sorunlarını için bu bağlantıları kullanıma sunar. Ayrıca, güvenilirlik ve internet bağlantıları için kullanılabilirlik garanti edilemez.
* VPN bağlantıları sanal ağlar için bazı uygulamalar ve olarak yaklaşık 200 MB/sn hızında max çıkışı amaçları için bant genişliği olmayabilir.

Yüksek düzeyde güvenlik ve kullanılabilirlik genellikle kendi şirket içi bağlantılar için gereken kuruluşlar uzaktan sitelere bağlanmak için ayrılmış WAN bağlantıları kullanın. Azure şirket içi ağınıza bir sanal ağa bağlamak için kullanabileceğiniz ayrılmış bir WAN bağlantısı kullanma yeteneği sağlar. Azure ExpressRoute bu sağlar.

Daha fazla bilgi edinin:

* [ExpressRoute teknik genel bakış](../expressroute/expressroute-introduction.md)

### <a name="connect-virtual-networks-to-each-other"></a>Sanal ağları birbirine bağlayan
Dağıtımlar için çok sayıda sanal ağlar kullanmak da mümkündür. Neden bunu yapabilirsiniz çeşitli nedenleri vardır. Güvenliği artırmak isteyebilirsiniz veya yönetimini basitleştirmek isteyebilirsiniz. Farklı sanal ağlardaki kaynakları yerleştirme motivasyon bakılmaksızın birbiriyle bağlanmak için ağların her biri kaynakları istediğinizde zamanlar olabilir.

Bir seçenektir "geri döngü yapmasıyla" başka bir sanal ağda hizmetlerine bağlanmak için bir sanal ağda Hizmetleri için Internet üzerinden. Bağlantı bir sanal ağ üzerindeki başlatır, Internet üzerinden geçer ve hedef sanal ağa geri gelir. Bu seçenek tüm internet tabanlı iletişimde devralınan güvenlik sorunları bağlantısı kullanıma sunar.

İki sanal ağ arasında bağlayan siteden siteye VPN oluşturmak için daha iyi bir seçenek olabilir. Bu yöntem aynı kullanır [IPSec tünel modu](https://technet.microsoft.com/library/cc786385.aspx) Protokolü yukarıda belirtilen şirket içi siteden siteye VPN bağlantısı olarak.

Bu yaklaşımın avantajı, VPN bağlantısı internet üzerinden bağlanma yerine Azure ağ yapısı üzerinden kurulur ' dir. Bu fazladan bir Internet üzerinden bağlanma siteden siteye VPN ile karşılaştırıldığında güvenlik katmanı sağlar.

Daha fazla bilgi edinin:

* [Azure Resource Manager ve PowerShell kullanarak VNet-VNet bağlantı yapılandırma](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="availability"></a>Kullanılabilirlik
Kullanılabilirlik herhangi bir güvenlik programı anahtar bir bileşenidir. Bunlar ağ üzerinden erişmesi gereken kullanıcılar ve sistemlerden erişemiyorsanız hizmet sayılabileceğini tehlikeye. Azure aşağıdaki yüksek kullanılabilirlik mekanizmaları ağ teknolojileri sahiptir:

* HTTP tabanlı Yük Dengeleme
* Ağ düzeyinde Yük Dengeleme
* Genel Yük Dengeleme

Yük Dengeleme, birden çok aygıt arasındaki bağlantıları eşit olarak dağıtmak için tasarlanmış bir mekanizmadır. Yük Dengelemesi hedefleri şunlardır:

* Kullanılabilirliğini artırmak için. Birden çok cihazda Bakiye bağlantıları yüklediğinizde, bir veya daha fazla aygıtları hizmet ödün vermeden kullanılamaz hale gelebilir. Kalan çevrimiçi cihazlarda çalışan hizmetleri hizmetinden içeriğe hizmet edecek şekilde devam edebilirsiniz.
* Performansı artırmak için. Birden çok cihazda Bakiye bağlantıları yüklediğinizde, tek bir cihazı tüm işleme işlemeye sahip değil. Bunun yerine, içeriği sunması için işleme ve bellek taleplerini yayılır birçok cihaz arasında.

### <a name="http-based-load-balancing"></a>HTTP tabanlı Yük Dengeleme
Bu web hizmetleri önünde bir HTTP tabanlı yük dengeleyici web tabanlı hizmetler genellikle çalıştırdığı kuruluşlar işlemleriniz. Bu, yeterli düzeyde performans ve yüksek kullanılabilirlik sağlamaya yardımcı olur. Geleneksel, ağ tabanlı yük dengeleyici, ağ ve Aktarım katmanı protokolleri kullanır. HTTP tabanlı yük dengeleyici, diğer yandan, HTTP protokolünü özellikler temelinde kararlar.

Azure uygulama ağ geçidi, web tabanlı hizmetler için HTTP tabanlı yük dengelemesini sağlar. Uygulama ağ geçidi destekler:

* Tanımlama bilgisi tabanlı oturum benzeşimi. Bu özellik bu yük dengeleyici arkasında sunuculardan birine kurulan bağlantılar istemci ve sunucu arasında değişmeden kalmasını emin olur. Bu işlemler kararlılığını sağlar.
* SSL boşaltma. İstemci olan yük dengeleyici bağlandığında, bu oturum HTTPS (SSL) protokolü kullanılarak şifrelenir. Ancak, performansı artırmak için yük dengeleyici ve yük dengeleyicinin arkasındaki web sunucusu arasında bağlantı kurmak için HTTP (şifrelenmemiş) protokolünü kullanabilirsiniz. Yük dengeleyicinin arkasındaki web sunucuları şifrelemeyle ilgili işlemci yükü deneyimi yoktur çünkü bu için "SSL boşaltma" adlandırılır. Web sunucuları bu nedenle daha hızlı istekleri.
* URL tabanlı içerik yönlendirme. Bu özellik, hedef URL temel alınarak iletme bağlantıları burada hakkında kararlar yapmak yük dengeleyici için mümkün kılar. Bu, Yük Dengeleme kararları IP adreslerine dayalı olun çözümleri çok daha fazla esneklik sağlar.

Daha fazla bilgi edinin:

* [Uygulama ağ geçidi'ne genel bakış](../application-gateway/application-gateway-introduction.md)

### <a name="network-level-load-balancing"></a>Ağ düzeyinde Yük Dengeleme
HTTP tabanlı Yük Dengeleme aksine ağ düzeyinde Yük Dengelemesi, IP adresi ve bağlantı noktası (TCP veya UDP) sayılara göre kararlarını verir.
Azure'daki Azure yük dengeleyici kullanarak ağ düzeyinde Yük Dengeleme avantajları elde. Yük dengeleyicinin anahtar bazı özellikleri içerir:

* Ağ düzeyinde Yük Dengeleme IP adresi ve bağlantı noktası numaralarını temel.
* Tüm uygulama katmanı protokolü desteği.
* Azure sanal makinelerine yükünü dengeleyen ve rol örnekleri bulut Hizmetleri.
* İnternete dönük (Dış Yük Dengeleme) ve Internet olmayan için kullanılabilir (iç Yük Dengeleme) uygulamalar ve sanal makineleri karşılıklı.
* Yük dengeleyicinin arkasındaki hizmetlerden herhangi biri kullanılamaz duruma olmadığını belirlemek için kullanılan, uç noktası izleme.

Daha fazla bilgi edinin:

* [Birden çok sanal makineler veya hizmetler arasında Internet'e yönelik Yük Dengeleyici](../load-balancer/load-balancer-internet-overview.md)
* [İç yük dengeleyiciye genel bakış](../load-balancer/load-balancer-internal-overview.md)

### <a name="global-load-balancing"></a>Genel Yük Dengeleme
Bazı kuruluşlar, olası en yüksek düzeyde kullanılabilirlik istiyorsunuz. Bu hedefe ulaşmak için bir genel olarak dağıtılmış veri merkezlerinde uygulamalarını barındırmasını yoludur. Bir uygulama, dünyanın her yerinde bulunan veri merkezlerinde barındırıldığında kullanılamaz hale ve uygulama hala sahip tüm bir coğrafi bölge için olası ve çalışır durumdadır.

Bu yük dengeleyici stratejisi performans avantajları da sağlar. İsteği yapan aygıt en yakın veri merkezine hizmet isteklerini yönlendirebilirsiniz.

Azure'da, Azure trafik Yöneticisi'ni kullanarak genel Yükü Dengeleme faydaları elde edebilirsiniz.

Daha fazla bilgi edinin:

* [Traffic Manager nedir?](../traffic-manager/traffic-manager-overview.md)


## <a name="name-resolution"></a>Ad çözümlemesi
Ad çözümlemesi, tüm hizmetleri Azure'da barındırmak için kritik bir işlevdir. Güvenlik açısından bakıldığında, ad çözümlemesi işlevi bir saldırganın bir saldırganın site sitelerinizi isteklerini yeniden yönlendirmek için bırakılmasına neden olabilir. Güvenli ad çözümlemesi tüm barındırılan bulut Hizmetleri için gerekli değildir.

Ad çözümlemesi ele almanız gereken iki tür vardır:

* Dahili ad çözümlemesi. Bu, sanal ağlar, şirket içi ağlarınız veya her ikisi de Hizmetleri tarafından kullanılır. İç ad çözümlemesi için kullanılan adları internet üzerinden erişilebilir değildir. En iyi güvenlik için iç ad çözümlemesi düzeninizi dış kullanıcılar için erişilebilir değil önemlidir.
* Dış ad çözümlemesi. Bu, insanların ve cihazların şirket içi ağlarınız ve sanal ağlar dışında tarafından kullanılır. Bunlar internet'e görünür olan ve doğrudan bulut tabanlı hizmetler için bağlantı için kullanılan adlardır.

Dahili ad çözümlemesi için iki seçeneğiniz vardır:

* Bir sanal ağ DNS sunucusu. Yeni bir sanal ağ oluşturduğunuzda, bir DNS sunucusu sizin için oluşturulur. Bu DNS sunucusu, bu sanal ağ üzerinde bulunan makineler adlarını çözümleyebilir. Bu DNS sunucusu yapılandırılabilir değildir, Azure doku Yöneticisi tarafından yönetilen ve bu nedenle, ad çözümlemesi çözümünüzün güvenliğini sağlamanıza yardımcı olabilir.
* DNS sunucunuzun duruma getirin. Bir DNS sunucusu kendi sanal ağınızda seçme koyma seçeneğiniz vardır. Bu DNS sunucusu, DNS sunucusu veya Azure Marketi'nden edinebileceğiniz bir Azure ortak tarafından sağlanan adanmış bir DNS sunucusu çözümü bir Active Directory tümleşik olabilir.

Daha fazla bilgi edinin:

* [Sanal ağ genel bakış](../virtual-network/virtual-networks-overview.md)
* [Bir sanal ağ tarafından kullanılan DNS sunucularını yönetin](../virtual-network/manage-virtual-network.md#change-dns-servers)

Dış ad çözümlemesi için iki seçeneğiniz vardır:

* Kendi dış DNS sunucusu şirket içi ana bilgisayar.
* Kendi dış DNS sunucusu hizmeti sağlayıcısı ile ana bilgisayar.

Birçok büyük kuruluşların kendi DNS sunucuları şirket içi barındırır. Bunu yapmak için genel durum ve ağ uzmanlığa sahip oldukları için bunu.

Çoğu durumda, bir hizmet sağlayıcısı ile DNS ad çözümleme hizmetleri barındırmak daha iyi olur. Bu hizmet sağlayıcıları, ad çözümleme hizmetleri çok yüksek kullanılabilirliğini sağlamak için genel durum ve ağ uzmanlığa sahip. Ad Çözümleme Hizmetleri başarısız olursa, hiç kimse Hizmetleri, Internet'e ulaşabilmesi olacağı için kullanılabilirlik DNS hizmetleri için gereklidir.

Azure, yüksek oranda kullanılabilir bir sahip ve kullanıcı dış DNS çözüm Azure DNS biçiminde sağlar. Bu dış ad çözümlemesi çözüm dünya çapında Azure DNS altyapısı yararlanır. Aynı kimlik bilgileri, API ' larını araçları kullanarak ve diğer Azure hizmetlerinizi faturalama Azure etki alanınızda barındırmanıza olanak sağlar. Azure bir parçası olarak platformu ile oluşturulmuş güçlü güvenlik denetimleri de devralır.

Daha fazla bilgi edinin:

* [Azure DNS'ye genel bakış](../dns/dns-overview.md)

## <a name="perimeter-network-architecture"></a>Çevre ağ mimarisi
Birçok büyük kuruluşların Çevre ağları ağlarını segmentlere ayırmak için kullanır ve Internet ve kendi hizmetleri arasında bir arabellek bölge oluşturun. Ağ çevre kısmı düşük güvenlikli bölgenin olarak kabul edilir ve hiçbir yüksek değerli varlıklar, ağ kesimine yerleştirilir. Çevre ağ kesimindeki bir ağ arabirimine sahip ağ güvenlik aygıtları genellikle görürsünüz. Başka bir ağ arabirimi sanal makineleri ve Internet'ten gelen bağlantıları kabul Hizmetleri olan bir ağa bağlı.

Çevre ağı çeşitli farklı şekillerde tasarlayabilirsiniz. Bir çevre ağına ve daha sonra ne tür bir çevre ağ biri kullanmaya karar verirseniz kullanılacak dağıtma kararı, ağ güvenlik gereksinimlerine bağlıdır.

Daha fazla bilgi edinin:

* [Microsoft bulut Hizmetleri ve ağ güvenliği](../best-practices-network-security.md)


## <a name="monitoring-and-threat-detection"></a>İzleme ve tehdit algılama

Ağ trafiği toplama ve gözden geçirme ve Azure erken algılama, izleme, bu anahtar alanında yardımcı olmak için özellikleri sağlar.

### <a name="azure-network-watcher"></a>Azure Ağ İzleyicisi
Azure Ağ İzleyicisi, gidermenize yardımcı olabilir ve güvenlik sorunları tanımlaması yardımcı olan araçlar yepyeni bir dizi sağlar.

[Güvenlik grubu görünümü ](../network-watcher/network-watcher-security-group-view-overview.md) sanal makineleri denetim ve güvenlik uyumluluğunu ile yardımcı olur. Her Vm'leriniz için etkili kuralları, kuruluşunuz tarafından tanımlanan temel ilkeleri karşılaştırma programlı denetimleri gerçekleştirmek için bu özelliği kullanın. Bu, tüm yapılandırma değişikliklerini belirlemenize yardımcı olabilir.

[Paket yakalama](../network-watcher/network-watcher-packet-capture-overview.md) ve sanal makineden ağ trafiğini yakalamanıza olanak sağlar. Ağ istatistikleri toplamak ve ağ yetkisiz araştırma içinde çok yararlı olabilir uygulama sorunlarını giderebilirsiniz. Azure işlevleri ile birlikte bu özellik, ağ yakalamaları belirli Azure uyarılara yanıt olarak başlatmak için de kullanabilirsiniz.

Ağ İzleyicisi'ni ve bazı işlevlerini, Labs'de sınama başlatmak nasıl daha fazla bilgi için bkz: [izlemeye genel bakış Azure Ağ İzleyicisi](../network-watcher/network-watcher-monitoring-overview.md).

>[!NOTE]
En güncel bildirimlerinin kullanılabilirlik ve bu hizmetin durumunu denetleme [Azure güncelleştirmeleri sayfasında](https://azure.microsoft.com/updates/?product=network-watcher).

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi
Azure Güvenlik Merkezi, engellemenize, algılamanıza ve tehditlerine karşı yanıt yardımcı olur ve artan, görünürlük ve denetim üzerinden, Azure kaynaklarınızın güvenliğini sağlar. Azure aboneliklerinize arasında tümleşik güvenlik izleme ve ilke yönetimi sağlar, kaçabilecek tehditleri ve çok sayıda güvenlik çözümleri çalışır algılamaya yardımcı olur.

Güvenlik Merkezi en iyi duruma getirme ve ağ güvenliği tarafından izlemenize yardımcı olur:

* Ağ güvenlik önerileri sağlar.
* Ağ güvenliği yapılandırması durumunu izleme.
* Uç nokta ve ağ düzeyinde hem de tehditleri tabanlı ağ için uyarı.

Daha fazla bilgi edinin:

* [Azure Güvenlik Merkezi'ne Giriş](../security-center/security-center-intro.md)


### <a name="logging"></a>Günlüğe kaydetme
Ağ düzeyinde günlüğe kaydetme, bir ağ güvenlik senaryonuz için anahtar bir işlevdir. Azure üzerinde ağ bilgilerini günlüğe kaydetme düzeyi almak Nsg'ler için elde edilen bilgilerle oturum açabilir. NSG günlüğüyle bilgileri alın:

* [Etkinlik günlükleri](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md). Bu günlükler, Azure aboneliklerinize gönderilen tüm işlemleri görüntülemek için kullanın. Bu günlükler varsayılan olarak etkindir ve Azure portalı içinde kullanılabilir. Daha önce denetim veya işlem günlükleri bilinirdi.
* Olay günlüğe kaydedilir. Bu günlükler hangi NSG kuralları uygulanan hakkında bilgi sağlar.
* Sayaç günlükleri. Bu günlükler, her bir NSG kural Reddet izin verme veya trafiği uygulandığı kaç kez bilmeniz olanak tanır.

Aynı zamanda [Microsoft Power BI](https://powerbi.microsoft.com/what-is-power-bi/), görüntülemek ve bu günlükler çözümlemek için güçlü veri görselleştirme aracı.

Daha fazla bilgi edinin:

* [Ağ güvenlik grupları (Nsg'ler) için günlük analizi](../virtual-network/virtual-network-nsg-manage-log.md)
