---
title: Azure Application Gateway hakkında sık sorulan sorular
description: Azure Application Gateway hakkında sık sorulan soruların yanıtlarını bulun.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 4/30/2019
ms.author: victorh
ms.openlocfilehash: 72eaa18d570e85274efbea80f12e9a7301eecbfa
ms.sourcegitcommit: ed66a704d8e2990df8aa160921b9b69d65c1d887
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64947077"
---
# <a name="frequently-asked-questions-about-application-gateway"></a>Application Gateway hakkında sık sorulan sorular

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="general"></a>Genel

### <a name="what-is-application-gateway"></a>Application Gateway Nedir?

Azure Application Gateway bir uygulama teslim denetleyicisi (ADC) hizmet olarak sunar. Çeşitli 7. Katman Yük Dengeleme, uygulamalarınız için özellikleri sunar. Bu, yüksek oranda kullanılabilir, ölçeklenebilir ve Azure tarafından tam olarak yönetilen hizmetidir.

### <a name="what-features-does-application-gateway-support"></a>Hangi özellikleri, uygulama ağ geçidi destekliyor mu?

Application Gateway, otomatik ölçeklendirme, SSL yük boşaltma ve uçtan uca SSL, bir web uygulaması Güvenlik Duvarı (WAF), tanımlama bilgilerine dayalı oturum benzeşimi, URL yolu tabanlı yönlendirme, çoklu site barındırma ve diğer özellikleri destekler. Desteklenen özelliklerin tam bir listesi için bkz. [Application Gateway'e giriş](application-gateway-introduction.md).

### <a name="how-do-application-gateway-and-azure-load-balancer-differ"></a>Uygulama ağ geçidi ve Azure Load Balancer farkı?

Uygulama ağ geçidi yalnızca ile web trafiğini (HTTP, HTTPS, WebSocket ve HTTP/2) çalıştığı anlamına gelir. bir katman 7 yük dengeleyicidir. Yük Dengeleme için SSL sonlandırma, tanımlama bilgilerine dayalı oturum benzeşimi ve hepsini bir kez deneme gibi özellikleri destekler trafiği. Yük Dengeleme gerçekleştirir trafikte katmanında 4 (TCP veya UDP) yük.

### <a name="what-protocols-does-application-gateway-support"></a>Hangi protokollerin, Application Gateway destekliyor mu?

Application Gateway, HTTP, HTTPS, HTTP/2 ve WebSocket destekler.

### <a name="how-does-application-gateway-support-http2"></a>Application Gateway, HTTP/2 nasıl destekler?

Bkz: [HTTP/2 desteği](https://docs.microsoft.com/azure/application-gateway/configuration-overview#http2-support).

### <a name="what-resources-are-supported-as-part-of-a-backend-pool"></a>Hangi kaynakların bir arka uç havuzunun bir parçası destekleniyor mu?

Bkz: [arka uç kaynaklarına desteklenen](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#backend-pool).

### <a name="in-what-regions-is-application-gateway-available"></a>Application Gateway, hangi bölgelerde kullanılabilir?

Uygulama ağ geçidi, tüm genel Azure bölgelerinde kullanılabilir. Ayrıca kullanılabilir, [Azure Çin 21Vianet](https://www.azure.cn/) ve [Azure kamu](https://azure.microsoft.com/overview/clouds/government/).

### <a name="is-this-deployment-dedicated-for-my-subscription-or-is-it-shared-across-customers"></a>Bu dağıtım için Aboneliğimi ayrılmış olduğu ve müşteriler arasında paylaşılır?

Uygulama ağ geçidi, sanal ağınızda ayrılmış bir dağıtımıdır.

### <a name="does-application-gateway-support-http-to-https-redirection"></a>Application Gateway HTTP-HTTPS yeniden yönlendirmesi destekliyor mu?

Yeniden yönlendirme desteklenir. Bkz: [Application Gateway yeniden yönlendirmeye genel bakış](application-gateway-redirect-overview.md).

### <a name="in-what-order-are-listeners-processed"></a>Hangi sırayla dinleyicileri işlenir?

Bkz: [sipariş işleme dinleyicisinin](https://docs.microsoft.com/azure/application-gateway/configuration-overview#order-of-processing-listeners).

### <a name="where-do-i-find-the-application-gateway-ip-and-dns"></a>Uygulama ağ geçidi IP ve DNS nerede bulabilirim?

Bir uç noktası olarak bir genel IP adresi kullanıyorsanız, genel IP adresine kaynak IP ve DNS bilgileri bulabilirsiniz. Veya uygulama ağ geçidi için genel bakış sayfasında portalında bulabilirsiniz. İç IP adreslerini kullanıyorsanız, genel bakış sayfasındaki bilgileri bulun.

### <a name="what-are-the-settings-for-keep-alive-timeout-and-tcp-idle-timeout"></a>Tutma zaman aşımı ve TCP boşta kalma zaman aşımı ayarlarını nelerdir?

 Application Gateway v1 SKU, Keep-Alive zaman aşımı 120 saniyedir. V2 SKU Keep-Alive zaman aşımı 75 saniyedir. TCP boşta kalma zaman aşımı, Application Gateway ön uç sanal IP (VIP) 4 dakikalık varsayılandır.

### <a name="does-the-ip-or-dns-name-change-over-the-lifetime-of-the-application-gateway"></a>IP veya DNS adını uygulama ağ geçidi ömrü boyunca ile değişiyor mu?

Uygulama ağ geçidi durdurup VIP değiştirebilirsiniz. Ancak, DNS adını uygulama ağ geçidiyle ilişkili ağ geçidi ömrü boyunca değiştirmez. DNS adı değişmediğinden, CNAME diğer adlarını kullanma ve uygulama ağ geçidinin DNS adresine işaret gerekir.

### <a name="does-application-gateway-support-static-ip"></a>Application Gateway, statik IP destekliyor mu?

Evet, uygulama ağ geçidi v2 SKU statik genel IP adreslerini destekler. V1 SKU statik iç IP destekler.

### <a name="does-application-gateway-support-multiple-public-ips-on-the-gateway"></a>Application Gateway, ağ geçidinde birden çok genel IP destekliyor mu?

Bir uygulama ağ geçidi, yalnızca bir genel IP adresini destekler.

### <a name="how-large-should-i-make-my-subnet-for-application-gateway"></a>Ne kadar büyük alt Application Gateway için uygulamalıyım?

Bkz: [uygulama ağ geçidi alt ağı boyutunu konuları](https://docs.microsoft.com/azure/application-gateway/configuration-overview#size-of-the-subnet).

### <a name="can-i-deploy-more-than-one-application-gateway-resource-to-a-single-subnet"></a>Tek bir alt ağa birden fazla Application Gateway kaynağında dağıtabilir miyim?

Evet. Belirli bir uygulama ağ geçidi dağıtım birden çok örneğini ek olarak, farklı bir uygulama ağ geçidi kaynağı içeren mevcut bir alt ağ için benzersiz bir uygulama ağ geçidi kaynak sağlayabilirsiniz.

Tek bir alt ağ Standard_v2 hem de standart Application Gateway birlikte destekleyemez.

### <a name="does-application-gateway-support-x-forwarded-for-headers"></a>Application Gateway, x-iletilen-için üstbilgiler destekliyor mu?

Evet. Bkz: [değişiklik isteğine](https://docs.microsoft.com/azure/application-gateway/how-application-gateway-works#modifications-to-the-request).

### <a name="how-long-does-it-take-to-deploy-an-application-gateway-will-my-application-gateway-work-while-its-being-updated"></a>Ne kadar bir uygulama ağ geçidini dağıtmak için sürer? My application gateway, güncelleştirilirken çalışacak mı?

Yeni uygulama ağ geçidi v1 SKU dağıtımlar sağlamak 20 dakikaya kadar sürebilir. Örnek boyutunu veya sayısını değişiklikler kesintiye uğratan değildir ve ağ geçidi bu süre boyunca etkin kalır.

V2 SKU kullanan dağıtımlar için sağlama 6 dakikaya kadar sürebilir.

### <a name="can-i-use-exchange-server-as-a-backend-with-application-gateway"></a>Uygulama ağ geçidi ile bir arka uç olarak Exchange Server kullanabilir miyim?

Hayır. Uygulama ağ geçidi POP3 SMTP ve IMAP gibi e-posta protokollerini desteklemez. 

## <a name="performance"></a>Performans

### <a name="how-does-application-gateway-support-high-availability-and-scalability"></a>Application Gateway, yüksek kullanılabilirlik ve ölçeklenebilirlik nasıl destekler?

İki veya daha fazla örneğini dağıttınız, uygulama ağ geçidi v1 SKU yüksek kullanılabilirlik senaryolarını destekler. Azure, bu örnekler, örnekleri aynı anda tüm başarısız olmayan emin olmak için güncelleştirme ve hata etki alanları arasında dağıtır. V1 SKU yük paylaşmak için aynı ağ geçidinin birden çok örnek ekleyerek ölçeklenebilirliği desteklemektedir.

V2 SKU yeni örnekleri hata etki alanlarına ve güncelleştirme etki alanlarına otomatik olarak sağlar. Bölge artıklığı seçerseniz, en yeni örnekleri de bölgesel hatası dayanıklılık sunmak için kullanılabilirlik alanları genelinde yayılır.

### <a name="how-do-i-achieve-a-dr-scenario-across-datacenters-by-using-application-gateway"></a>Nasıl ben bir DR senaryosuna veri merkezleri arasında uygulama ağ geçidi'ni kullanarak elde edebilirim?

Traffic Manager, farklı veri merkezlerindeki birden fazla uygulama ağ geçidi üzerinden trafiği dağıtmak için kullanın.

### <a name="does-application-gateway-support-autoscaling"></a>Application Gateway, otomatik ölçeklendirme destekliyor mu?

Evet, uygulama ağ geçidi v2 SKU otomatik ölçeklendirmeyi destekler. Daha fazla bilgi için [otomatik ölçeklendirme ve bölgesel olarak yedekli Application Gateway](application-gateway-autoscaling-zone-redundant.md).

### <a name="does-manual-scale-up-or-scale-down-cause-downtime"></a>Yoksa el ile ölçeği artırma veya ölçeği azaltma neden kapalı kalma süresi?

Hayır. Örnekleri, yükseltme etki alanları ve hata etki alanları arasında dağıtılır.

### <a name="does-application-gateway-support-connection-draining"></a>Application Gateway bağlantı boşaltma destekliyor mu?

Evet. Arka uç havuzundaki kesintiye uğratmadan üyelerini değiştirmek için boşaltma bağlantısı ayarlayabilirsiniz. Bu kurulum, var olan bağlantıları önceki hedeflerine kadar bu bağlantı göndermek devam etmenizi kapatır veya yapılandırılabilir bir zaman aşımı süresi sağlar. Tamamlanması yalnızca geçerli uçuşan bağlantıları için bağlantı boşaltma bekler. Uygulama ağ geçidini, uygulama oturum durumunu farkında değildir.

### <a name="can-i-change-instance-size-from-medium-to-large-without-disruption"></a>Örnek boyutu ortamından kesintiye uğratmadan büyük değiştirebilirim?

Evet. Azure örnekleri örneklerinin tümü aynı anda başarısız olmayan emin olmak için güncelleştirme ve hata etki alanları arasında dağıtır. Application Gateway yük paylaşmak için aynı ağ geçidini birden çok örneğini ekleyerek ölçeği genişletmeyi destekler.

## <a name="configuration"></a>Yapılandırma

### <a name="is-application-gateway-always-deployed-in-a-virtual-network"></a>Application Gateway, her zaman bir sanal ağda dağıtılır?

Evet. Uygulama ağ geçidi her zaman bir sanal ağ alt ağında dağıtılır. Bu alt ağ, yalnızca uygulama ağ geçitleri içerebilir. Daha fazla bilgi için [sanal ağ ve alt ağ gereksinimleri](https://docs.microsoft.com/azure/application-gateway/configuration-overview#azure-virtual-network-and-dedicated-subnet).

### <a name="can-application-gateway-communicate-with-instances-outside-of-its-virtual-network-or-outside-of-its-subscription"></a>Uygulama ağ geçidi örnekleri kendi sanal ağ dışındaki veya aboneliğini dışında ile iletişim kurabilir?

IP bağlantısı olduğu sürece, uygulama ağ geçidi örnekleri içinde sanal ağın dışında iletişim kurabilir. Uygulama ağ geçidi örnekleri içinde abonelik dışında ile de iletişim kurabilir. İç IP'ler arka uç havuzu üyesi olarak kullanmayı planlıyorsanız, kullanmak [sanal ağ eşlemesi](../virtual-network/virtual-network-peering-overview.md) veya [Azure VPN ağ geçidi](../vpn-gateway/vpn-gateway-about-vpngateways.md).

### <a name="can-i-deploy-anything-else-in-the-application-gateway-subnet"></a>Uygulama ağ geçidi alt ağında bir şey dağıtabilir miyim?

Hayır. Ancak, alt ağdaki diğer uygulama ağ geçitleri dağıtabilir.

### <a name="are-network-security-groups-supported-on-the-application-gateway-subnet"></a>Uygulama ağ geçidi alt ağı üzerinde ağ güvenlik grupları desteklenir?

Bkz: [ağ güvenlik grupları uygulama ağ geçidi alt](https://docs.microsoft.com/azure/application-gateway/configuration-overview#network-security-groups-on-the-application-gateway-subnet).

### <a name="does-the-application-gateway-subnet-support-user-defined-routes"></a>Uygulama ağ geçidi alt ağı, kullanıcı tanımlı yollar destekliyor mu?

Bkz: [kullanıcı tanımlı yollara uygulama ağ geçidi alt ağında desteklenen](https://docs.microsoft.com/azure/application-gateway/configuration-overview#user-defined-routes-supported-on-the-application-gateway-subnet).

### <a name="what-are-the-limits-on-application-gateway-can-i-increase-these-limits"></a>Application Gateway'de sınırları nelerdir? Bu limitler artırabilirim?

Bkz: [Application Gateway sınırlar](../azure-subscription-service-limits.md#application-gateway-limits).

### <a name="can-i-simultaneously-use-application-gateway-for-both-external-and-internal-traffic"></a>Aynı anda uygulama ağ geçidi iç ve dış trafiği için kullanabilir miyim?

Evet. Application Gateway, bir iç IP ve uygulama ağ geçidi başına bir dış IP destekler.

### <a name="does-application-gateway-support-virtual-network-peering"></a>Application Gateway, sanal ağ eşlemesi destekliyor mu?

Evet. Sanal Ağ eşlemesi, diğer sanal ağlarda trafik yükünü dengele yardımcı olur.

### <a name="can-i-talk-to-on-premises-servers-when-theyre-connected-by-expressroute-or-vpn-tunnels"></a>Şirket içi sunucular için ExpressRoute veya VPN tünelinde tarafından bağlı olduğunuzda iletişim kurabilir?

Evet, trafiğe izin sürece.

### <a name="can-one-backend-pool-serve-many-applications-on-different-ports"></a>Bir arka uç havuzu, çoğu uygulama farklı noktalarında görebilir?

Mikro hizmet mimarisi desteklenir. Farklı bağlantı noktalarını araştırma için birden çok HTTP ayarları yapılandırmanız gerekir.

### <a name="do-custom-probes-support-wildcards-or-regex-on-response-data"></a>Özel araştırmalar joker karakterler veya normal ifade üzerinde yanıt verileri destekler mi?

Hayır. 

### <a name="how-are-routing-rules-processed-in-application-gateway"></a>Yönlendirme kuralları, Application Gateway'i nasıl işlenir?

Bkz: [kuralları işleme sırası](https://docs.microsoft.com/azure/application-gateway/configuration-overview#order-of-processing-rules).

### <a name="for-custom-probes-what-does-the-host-field-signify"></a>Özel araştırmalar için ana bilgisayar adı alanı ne geldiğiniz?

Ana bilgisayar adı alanı yapılandırdığınız araştırması için çok siteli uygulama ağ geçidinde gönderilecek adını belirtir. Aksi takdirde '127.0.0.1' kullanın. Bu değer sanal makine konak adından farklıdır. Şu biçimdedir \<Protokolü\>://\<konak\>:\<bağlantı noktası\>\<yolu\>.

### <a name="can-i-whitelist-application-gateway-access-to-only-a-few-source-ips"></a>Alabilirim beyaz liste yalnızca birkaç kaynak IP'leri Application Gateway erişimi?

Evet. Bkz: [belirli kaynak IP'leri erişimi kısıtlama](https://docs.microsoft.com/azure/application-gateway/configuration-overview#whitelist-application-gateway-access-to-a-few-source-ips).

### <a name="can-i-use-the-same-port-for-both-public-facing-and-private-facing-listeners"></a>Aynı bağlantı noktasını hem genel kullanıma yönelik hem de özel bakan dinleyicileri için kullanabilir miyim?

Hayır.

## <a name="configuration---ssl"></a>Yapılandırma - SSL

### <a name="what-certificates-does-application-gateway-support"></a>Hangi sertifikaların Application Gateway destekliyor mu?

Application Gateway, otomatik olarak imzalanan sertifikaları, sertifika yetkilisi (CA) sertifikaları, Genişletilmiş Doğrulama (EV) sertifikaları ve joker karakterli sertifikalar destekler.

### <a name="what-cipher-suites-does-application-gateway-support"></a>Hangi şifre paketlerinin mu Application Gateway destekler?

Application Gateway, aşağıdaki şifre paketleri destekler. 

- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
- TLS_DHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_DHE_RSA_WITH_AES_256_CBC_SHA
- TLS_DHE_RSA_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_AES_256_GCM_SHA384
- TLS_RSA_WITH_AES_128_GCM_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA256
- TLS_RSA_WITH_AES_128_CBC_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA
- TLS_RSA_WITH_AES_128_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_3DES_EDE_CBC_SHA
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA

SSL seçeneklerini özelleştirme hakkında daha fazla bilgi için bkz: [yapılandırma SSL İlkesi sürümleri ve uygulama ağ geçidinde şifre paketleri](application-gateway-configure-ssl-policy-powershell.md).

### <a name="does-application-gateway-support-reencryption-of-traffic-to-the-backend"></a>Uygulama ağ geçidini yeniden şifreleme işlemi arka uca trafik destekliyor mu?

Evet. Application Gateway, arka uca trafik giden SSL yük boşaltmasını ve uçtan uca SSL, destekler.

### <a name="can-i-configure-ssl-policy-to-control-ssl-protocol-versions"></a>SSL İlkesi denetimi SSL protokolü sürümleri yapılandırabilirim?

Evet. Application Gateway, TLS1.2 TLS1.0 ve TLS1.1 reddetmeyi yapılandırabilirsiniz. Varsayılan olarak, SSL 2.0 ve 3.0 zaten devre dışı bırakılır ve yapılandırılabilir değildir.

### <a name="can-i-configure-cipher-suites-and-policy-order"></a>Şifre paketleri ve ilke sırasını yapılandırabilir miyim?

Evet. Application Gateway'i yapabilecekleriniz [şifre paketleri yapılandırma](application-gateway-ssl-policy-overview.md). Özel bir ilke tanımlamak için aşağıdaki şifre paketleri en az birini etkinleştirin. 

* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
* TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_256_CBC_SHA256
* TLS_RSA_WITH_AES_128_CBC_SHA256

Application Gateway için arka uç Yönetim için SHA256 kullanılmaktadır.

### <a name="how-many-ssl-certificates-does-application-gateway-support"></a>Kaç tane SSL sertifikalarını uygulama ağ geçidi destekliyor mu?

Uygulama ağ geçidi en fazla 100 SSL sertifikalarını destekler.

### <a name="how-many-authentication-certificates-for-backend-reencryption-does-application-gateway-support"></a>Arka uç yeniden şifreleme işlemi için kaç tane kimlik doğrulama sertifikalarını uygulama ağ geçidi destekliyor mu?

Application Gateway, en fazla 10 kimlik doğrulama sertifikalarını destekler. Varsayılan değer 5'tir.

### <a name="does-application-gateway-natively-integrate-with-azure-key-vault"></a>Application Gateway, yerel olarak Azure Key Vault ile tümleşik çalışıyor mu?

Evet, uygulama ağ geçidi v2 SKU Key Vault destekler. Daha fazla bilgi için [sertifikaları Key Vault ile SSL sonlandırma](key-vault-certs.md).

### <a name="how-do-i-configure-https-listeners-for-com-and-net-sites"></a>HTTPS dinleyicileri .com ve .net siteleri için nasıl yapılandırabilirim? 

Birden çok etki alanı tabanlı (ana bilgisayar tabanlı) yönlendirme için çok siteli dinleyicileri oluşturma, HTTPS protokolü olarak kullanan dinleyicileri ayarlayın ve dinleyicileri yönlendirme kuralları ile ilişkilendirin. Daha fazla bilgi için [uygulama ağ geçidi kullanarak birden fazla siteyi barındırma](https://docs.microsoft.com/azure/application-gateway/multiple-site-overview). 

## <a name="configuration---web-application-firewall-waf"></a>Yapılandırma - web uygulaması Güvenlik Duvarı (WAF)

### <a name="does-the-waf-sku-offer-all-the-features-available-in-the-standard-sku"></a>WAF SKU'su standart SKU'da kullanılabilir olan tüm özellikleri sunar?

Evet. WAF, standart SKU tüm özelliklerini destekler.

### <a name="which-crs-versions-does-application-gateway-support"></a>Application Gateway CRS hangi sürümlerinin destekliyor mu?

Application Gateway CRS destekler [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) ve CRS [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).

### <a name="how-do-i-monitor-waf"></a>WAF nasıl izleyebilirim?

Tanılama günlüğüne kaydetme ile WAF izleme. Daha fazla bilgi için [tanılama günlüğüne kaydetme ve ölçümler için Application Gateway](application-gateway-diagnostics.md).

### <a name="does-detection-mode-block-traffic"></a>Algılama modu akışa mu?

Hayır. Algılama modunda yalnızca bir WAF kural tetikleyen trafiği günlüğe kaydeder.

### <a name="can-i-customize-waf-rules"></a>WAF kurallarını özelleştirebilirim?

Evet. Daha fazla bilgi için [özelleştirme WAF kural gruplarının ve kuralların](application-gateway-customize-waf-rules-portal.md).

### <a name="what-rules-are-currently-available-for-waf"></a>İşleme hangi kuralların WAF için şu anda kullanılabilir mi?

WAF, şu anda CRS destekler [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) ve [3.0](application-gateway-crs-rulegroups-rules.md#owasp30). Bu kurallar, açık Web uygulaması güvenlik Project (OWASP) tanımlayan en önemli 10 güvenlik açıklarını çoğuna karşı temel güvenlik sağlar: 

* SQL ekleme koruması
* Siteler arası komut dosyası koruması
* Komut ekleme, HTTP isteği kaçakçılığı, HTTP yanıtı bölme ve uzak dosya ekleme saldırıcı gibi yaygın web saldırılarına karşı koruma
* HTTP protokolü ihlallerine karşı koruma
* Eksik konak kullanıcısı-aracısı ve kabul üst bilgileri gibi HTTP protokolü anormalliklerine karşı koruma
* Robotlar, gezginler ve tarayıcıları önleme
* Yaygın yanlış uygulama yapılandırmalarını (diğer bir deyişle, Apache, IIS vb.) algılama

Daha fazla bilgi için [OWASP top-10 Güvenlik](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013).

### <a name="does-waf-support-ddos-protection"></a>WAF, DDoS koruması destekliyor mu?

Evet. DDoS koruması uygulama ağ geçidi dağıtıldığı sanal ağ üzerinde etkinleştirebilirsiniz. Bu ayar, Azure DDoS koruması hizmeti Ayrıca uygulama ağ geçidi sanal IP (VIP) koruma sağlar.

## <a name="diagnostics-and-logging"></a>Tanılama ve günlüğe kaydetme

### <a name="what-types-of-logs-does-application-gateway-provide"></a>Application Gateway günlüklerini ne tür sağlar?

Application Gateway, üç günlükleri sağlar: 

* **ApplicationGatewayAccessLog**: Uygulama ağ geçidi ön uç için gönderilen her istek erişim günlüğü içerir. Veriler içeri ve dışarı arayan IP, istenen URL, yanıt gecikme süresi, dönüş kodu ve bayt içerir. Erişim günlüğü, her 300 saniyede toplanır. Uygulama ağ geçidi başına tek bir kayıt var.
* **ApplicationGatewayPerformanceLog**: Performans günlüğü, her uygulama ağ geçidi için performans bilgileri yakalar. Bayt cinsinden aktarım hızı bilgilerini içerir, toplam istek sunulan, başarısız istek sayısı ve sağlıklı ve sağlıksız arka uç örnek sayısı.
* **ApplicationGatewayFirewallLog**: WAF ile yapılandırdığınız application gateway'ler için güvenlik duvarı günlüğü algılama modunda veya önleme modu günlüğe kaydedilen istekleri içerir.

Daha fazla bilgi için [arka uç sistem durumu, tanılama günlükleri ve ölçümler için Application Gateway](application-gateway-diagnostics.md).

### <a name="how-do-i-know-if-my-backend-pool-members-are-healthy"></a>My arka uç havuzu üyelerine sağlıklı olup olmadığını nasıl anlarım?

PowerShell cmdlet'ini kullanarak sistem durumu doğrulama `Get-AzApplicationGatewayBackendHealth` veya portalı. Daha fazla bilgi için [Application Gateway tanılama](application-gateway-diagnostics.md).

### <a name="whats-the-retention-policy-for-the-diagnostic-logs"></a>Tanılama günlüklerine yönelik bekletme ilkesi nedir?

Müşterinin depolama hesabına tanılama günlüklerinin akışı. Müşteriler, kendi tercihine göre bekletme ilkesi ayarlayabilirsiniz. Tanılama günlüklerini, olay hub'ı veya Azure İzleyici günlüklerine gönderilebilir. Daha fazla bilgi için [Application Gateway tanılama](application-gateway-diagnostics.md).

### <a name="how-do-i-get-audit-logs-for-application-gateway"></a>Application Gateway için denetim günlüklerini nasıl alabilirim?

Portalındaki menü dikey penceresinde bir uygulama ağ geçidi seçin **etkinlik günlüğü** Denetim günlüğüne erişemedi. 

### <a name="can-i-set-alerts-with-application-gateway"></a>Application Gateway uyarılarla ayarlayabilirim?

Evet. Application Gateway'i ölçümler üzerinde uyarılar yapılandırılır. Daha fazla bilgi için [uygulama ağ geçidi ölçümleri](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics#metrics) ve [uyarı bildirimleri alma](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

### <a name="how-do-i-analyze-traffic-statistics-for-application-gateway"></a>Application Gateway için nasıl trafiği istatistikleri analiz?

Görüntüleyebilir ve erişim günlükleri çeşitli yollarla analiz edin. Azure İzleyici günlüklerine, Excel, Power BI ve benzeri kullanın.

Yüklenen ve popüler çalıştırılan bir Resource Manager şablonu da kullanabilirsiniz [GoAccess](https://goaccess.io/) Çözümleyicisi uygulama ağ geçidi günlüklerine erişim için oturum açın. GoAccess benzersiz ziyaretçiler, istenen dosyaları, ana bilgisayar, işletim sistemleri, tarayıcılar ve HTTP durum kodları gibi değerli HTTP trafiğini istatistikler sağlar. Github'da, daha fazla bilgi için bkz. [Resource Manager şablonu klasörde Benioku dosyası](https://aka.ms/appgwgoaccessreadme).

### <a name="what-could-cause-backend-health-to-return-an-unknown-status"></a>Durumu bilinmeyen döndürmek arka uç sistem durumu nedeni ne?

Genellikle, bir ağ güvenlik grubu (NSG), özel DNS veya kullanıcı tanımlı yönlendirme (UDR) uygulama ağ geçidi alt ağı üzerinde arka uca Erişim engellendiğinde durumu bilinmeyen bakın. Daha fazla bilgi için [arka uç sistem durumu, tanılama günlükleri ve ölçümler için Application Gateway](application-gateway-diagnostics.md).

## <a name="next-steps"></a>Sonraki adımlar

Application Gateway hakkında daha fazla bilgi için bkz: [Azure Application Gateway nedir?](overview.md).
