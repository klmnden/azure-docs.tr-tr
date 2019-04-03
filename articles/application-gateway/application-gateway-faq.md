---
title: Azure Application Gateway için sık sorulan sorular
description: Bu sayfa, Azure Application Gateway hakkında sık sorulan soruların yanıtlarını sağlar
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 3/20/2019
ms.author: victorh
ms.openlocfilehash: c40f372d3574f940e475a6626f998adae37a6d61
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58851161"
---
# <a name="frequently-asked-questions-for-application-gateway"></a>Application Gateway için sık sorulan sorular

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="general"></a>Genel

### <a name="what-is-application-gateway"></a>Application Gateway Nedir?

Azure Application Gateway bir uygulama teslim denetleyicisi (ADC) hizmet olarak, çeşitli 7. Katman Yük Dengeleme Özellikleri uygulamalarınız için tekliftir. Azure tarafından tam olarak yönetilen, yüksek oranda kullanılabilir ve ölçeklenebilir hizmet sunar.

### <a name="what-features-does-application-gateway-support"></a>Hangi özellikleri, uygulama ağ geçidi destekliyor mu?

Application Gateway, otomatik ölçeklendirme, SSL yük boşaltma ve uçtan uca SSL, Web uygulaması güvenlik duvarı, tanımlama bilgilerine dayalı oturum benzeşimi, url yolu tabanlı yönlendirme, çoklu site barındırma ve diğerleri destekler. Desteklenen özelliklerin tam bir listesi için bkz. [Application Gateway'e giriş](application-gateway-introduction.md).

### <a name="what-is-the-difference-between-application-gateway-and-azure-load-balancer"></a>Azure Load Balancer ile uygulama ağ geçidi arasındaki fark nedir?

Uygulama ağ geçidi ile web trafiğini yalnızca (HTTP/HTTPS/WebSocket/HTTP/2) çalıştığı anlamına gelir. bir katman 7 yük dengeleyicidir. Bu, Yük Dengeleme trafiği için SSL sonlandırma, tanımlama bilgilerine dayalı oturum benzeşimi ve hepsini bir kez deneme gibi özellikleri destekler. 4 (TCP/UDP) katmanında trafikte yük dengeleyen yük.

### <a name="what-protocols-does-application-gateway-support"></a>Hangi protokollerin, Application Gateway destekliyor mu?

Application Gateway, HTTP, HTTPS, HTTP/2 ve WebSocket destekler.

### <a name="how-does-application-gateway-support-http2"></a>Application Gateway, HTTP/2 nasıl destekler?

Bkz: [HTTP/2 desteği](https://docs.microsoft.com/azure/application-gateway/configuration-overview#http2-support) uygulama ağ geçidi HTTP/2 protokolüne nasıl desteklediğini öğrenin.

### <a name="what-resources-are-supported-today-as-part-of-backend-pool"></a>Hangi kaynakların arka uç havuzunun bir parçası bugün destekleniyor mu?

Bkz: [arka uç kaynaklarına desteklenen](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#backend-pool) hangi kaynakların uygulama ağ geçidi tarafından desteklenen öğrenin.

### <a name="what-regions-is-the-service-available-in"></a>Hangi bölgeler kullanılabilir hizmet?

Uygulama ağ geçidi, tüm genel Azure bölgelerinde kullanılabilir. Ayrıca kullanılabilir, [Azure Çin 21Vianet](https://www.azure.cn/) ve [Azure kamu](https://azure.microsoft.com/overview/clouds/government/)

### <a name="is-this-a-dedicated-deployment-for-my-subscription-or-is-it-shared-across-customers"></a>Bu adanmış bir dağıtım için Aboneliğimi veya müşteriler arasında paylaşılır?

Uygulama ağ geçidi, sanal ağınızda ayrılmış bir dağıtımıdır.

### <a name="is-http-https-redirection-supported"></a>İş HTTP -> HTTPS yeniden yönlendirmesi desteklenir?

Yeniden yönlendirme desteklenir. Bkz: [Application Gateway yeniden yönlendirmeye genel bakış](application-gateway-redirect-overview.md) daha fazla bilgi için.

### <a name="in-what-order-are-listeners-processed"></a>Hangi sırayla dinleyicileri işlenir?

Bkz: [dinleyicileri işleme sırası](https://docs.microsoft.com/azure/application-gateway/configuration-overview#order-of-processing-listeners).

### <a name="where-do-i-find-application-gateways-ip-and-dns"></a>Uygulama ağ geçidinin IP ve DNS nerede bulabilirim?

Genel bir IP adresi bir uç nokta kullanırken, bu bilgileri genel IP adresi kaynağı veya genel bakış sayfasında application Gateway portalında bulunabilir. İç IP adresleri için bu genel bakış sayfasında bulunabilir.

### <a name="what-is-keep-alive-timeout-and-tcp-idle-timeout-setting-on-application-gateway"></a>Tutma zaman aşımı ve TCP boşta kalma zaman aşımı ayarını Application Gateway nedir?

V1 SKU tutma zaman aşımı süresi 120 sec. tutma zaman aşımını v2 SKU 4 dakikalık varsayılandır Application Gateway VIP ön uç üzerinde 75 sec. TCP boşta kalma zaman aşımı şeklindedir.

### <a name="does-the-ip-or-dns-name-change-over-the-lifetime-of-the-application-gateway"></a>IP veya DNS adını uygulama ağ geçidi ömrü boyunca ile değişiyor mu?

Uygulama ağ geçidi durduruldu ve başlatıldığında VIP değiştirebilirsiniz. Uygulama ağ geçidiyle ilişkili DNS adı, ağ geçidi yaşam döngüsü değiştirmez. Bu nedenle, bir CNAME diğer adlarını kullanma ve uygulama ağ geçidinin DNS adresine işaret önerilir.

### <a name="does-application-gateway-support-static-ip"></a>Application Gateway, statik IP destekliyor mu?

Evet, uygulama ağ geçidi v2 SKU statik genel IP adreslerini desteklemez. V1 SKU statik iç IP destekler.

### <a name="does-application-gateway-support-multiple-public-ips-on-the-gateway"></a>Application Gateway, ağ geçidinde birden çok genel IP destekliyor mu?

Yalnızca bir genel IP adresi, bir uygulama ağ geçidinde desteklenmiyor.

### <a name="how-large-should-i-make-my-subnet-for-application-gateway"></a>Ne kadar büyük alt Application Gateway için uygulamalıyım?

Bkz: [uygulama ağ geçidi alt ağı boyutunu konuları](https://docs.microsoft.com/azure/application-gateway/configuration-overview#size-of-the-subnet) dağıtımınız için gereken alt ağ boyutu anlamak için.

### <a name="q-can-i-deploy-more-than-one-application-gateway-resource-to-a-single-subnet"></a>S. Tek bir alt ağa birden fazla Application Gateway kaynağında dağıtabilir miyim?

Evet, belirli bir uygulama ağ geçidi dağıtım birden çok örneğini sahip olmaya ek olarak, farklı bir uygulama ağ geçidi kaynağı içeren mevcut bir alt ağ için benzersiz bir uygulama ağ geçidi kaynak sağlayabilirsiniz.

Standard_v2 ve standart Application Gateway, aynı alt ağda karıştırma desteklenmiyor.

### <a name="does-application-gateway-support-x-forwarded-for-headers"></a>Application Gateway, x-iletilen-için üstbilgiler destekliyor mu?

Evet. Bkz: [istemek için değişiklikler](https://docs.microsoft.com/azure/application-gateway/how-application-gateway-works#modifications-to-the-request) uygulama ağ geçidi tarafından desteklenen x-iletilen-için üstbilgiler hakkında bilgi edinmek için.

### <a name="how-long-does-it-take-to-deploy-an-application-gateway-does-my-application-gateway-still-work-when-being-updated"></a>Ne kadar bir uygulama ağ geçidini dağıtmak için sürer? My Application Gateway, güncelleştirilen olduğunda hala çalışıyor mu?

Yeni uygulama ağ geçidi v1 SKU dağıtımlar sağlamak 20 dakikaya kadar sürebilir. Örnek boyutu/sayısı değişiklikler kesintiye uğratan değildir ve ağ geçidi bu süre boyunca etkin kalır.

V2 SKU dağıtımlar sağlamak yaklaşık beş için altı dakika sürebilir.

### <a name="can-exchange-server-be-used-as-backend-with-application-gateway"></a>Exchange server ile uygulama ağ geçidi arka uç olarak kullanılabilir mi?

Hayır, Application Gateway, SMTP ve IMAP POP3 gibi e-posta protokollerini desteklemez. 

## <a name="performance"></a>Performans

### <a name="how-does-application-gateway-support-high-availability-and-scalability"></a>Application Gateway, yüksek kullanılabilirlik ve ölçeklenebilirlik nasıl destekler?

Application Gateway v1 SKU dağıtılan bir veya daha fazla örneğe sahip yüksek kullanılabilirlik senaryolarını destekler. Azure, bu örnekler, tüm örnekleri aynı anda başarısız olmayın emin olmak için güncelleştirme ve hata etki alanları arasında dağıtır. V1 SKU yük paylaşmak için aynı ağ geçidinin birden çok örnek ekleyerek ölçeklenebilirliği desteklemektedir.

V2 SKU yeni örnekleri hata etki alanlarına ve güncelleştirme etki alanlarına otomatik olarak sağlar. Bölge artıklığı seçilirse, en yeni örnekleri de bölgesel hatası dayanıklılık sunmak için kullanılabilirlik alanları genelinde yayılır.

### <a name="how-do-i-achieve-dr-scenario-across-data-centers-with-application-gateway"></a>Nasıl uygulama ağ geçidi ile veri merkezleri arasında alt klasörlere elde ediyorum DR senaryosu edebilirim?

Müşteriler, Traffic Manager, farklı veri merkezlerindeki birden fazla uygulama ağ geçidi üzerinden trafiği dağıtmak için kullanabilirsiniz.

### <a name="is-autoscaling-supported"></a>Otomatik ölçeklendirme destekleniyor mu?

Evet, uygulama ağ geçidi v2 SKU otomatik ölçeklendirmeyi destekler. Daha fazla bilgi için [otomatik ölçeklendirme ve bölgesel olarak yedekli (genel Önizleme) Application Gateway](application-gateway-autoscaling-zone-redundant.md).

### <a name="does-manual-scale-updown-cause-downtime"></a>El ile ölçeklendirmenin yukarı/aşağı neden kapalı kalma süresi mu?

Kapalı kalma süresi yoktur. Örnekleri, yükseltme etki alanları ve hata etki alanları arasında dağıtılır.

### <a name="does-application-gateway-support-connection-draining"></a>Application Gateway bağlantı boşaltma destekliyor mu?

Evet. Bağlantı boşaltma kesintiye uğratmadan arka uç havuzundaki üyelerini değiştirmek için yapılandırabilirsiniz. Bu, bu bağlantı kapalı ya da yapılandırılabilir bir zaman aşımı süresi kadar önceki hedeflerine gönderilecek devam etmek var olan bağlantılara izin verir. Yalnızca bağlantı boşaltma tamamlamak geçerli uçuşan bağlantıları için bekler. Uygulama ağ geçidi uygulaması oturum durumunu uyumlu değildir.

### <a name="can-i-change-instance-size-from-medium-to-large-without-disruption"></a>Örnek boyutu ortamından kesintiye uğratmadan büyük değiştirebilirim?

Evet, Azure örnekleri tüm örnekleri aynı anda başarısız olmayın emin olmak için güncelleştirme ve hata etki alanları arasında dağıtır. Application Gateway yük paylaşmak için aynı ağ geçidini birden çok örneğini ekleyerek ölçeği genişletmeyi destekler.

## <a name="configuration"></a>Yapılandırma

### <a name="is-application-gateway-always-deployed-in-a-virtual-network"></a>Application Gateway, her zaman bir sanal ağda dağıtılır?

Evet, uygulama ağ geçidi her zaman bir sanal ağ alt ağında dağıtılır. Bu alt ağ, yalnızca uygulama ağ geçitleri içerebilir. Bkz: [sanal ağ ve alt ağ gereksinimleri](https://docs.microsoft.com/azure/application-gateway/configuration-overview#azure-virtual-network-and-dedicated-subnet) uygulama ağ geçidi alt ağı yapılacak değerlendirmeleri anlamaktır.

### <a name="can-application-gateway-communicate-with-instances-outside-of-the-virtual-network-it-is-in-or-outside-of-the-subscription-it-is-in"></a>Uygulama ağ geçidi olarak sanal ağ dışındaki veya içinde abonelik dışında örneğiyle iletişim kurabilir?

IP bağlantısı var olduğu sürece uygulama ağ geçidi olarak sanal ağ dışındaki veya içinde olduğu abonelik dışında örneğiyle iletişim kurabilir. İç IP'ler arka uç havuzu üyesi olarak kullanmayı planladığınız sonra gerektiren [VNET eşlemesi](../virtual-network/virtual-network-peering-overview.md) veya [VPN ağ geçidi](../vpn-gateway/vpn-gateway-about-vpngateways.md).

### <a name="can-i-deploy-anything-else-in-the-application-gateway-subnet"></a>Uygulama ağ geçidi alt ağında bir şey dağıtabilir miyim?

Hayır, ancak alt ağdaki diğer uygulama ağ geçitleri dağıtabilir.

### <a name="are-network-security-groups-supported-on-the-application-gateway-subnet"></a>Uygulama ağ geçidi alt ağı üzerinde ağ güvenlik grupları desteklenir?

Bkz: [uygulama ağ geçidi alt ağı üzerinde ağ güvenlik grupları kısıtlamaları](https://docs.microsoft.com/azure/application-gateway/configuration-overview#network-security-groups-on-the-application-gateway-subnet) desteklenen uygulama ağ geçidi alt ağı üzerinde ağ güvenlik grupları hakkında bilgi edinin.

### <a name="are-user-defined-routes-supported-on-the-application-gateway-subnet"></a>Kullanıcı tanımlı yollara uygulama ağ geçidi alt ağı üzerinde destekleniyor mu?

Bkz: [kullanıcı tanımlı yollar kısıtlamaları](https://docs.microsoft.com/azure/application-gateway/configuration-overview#user-defined-routes-supported-on-the-application-gateway-subnet) uygulama ağ geçidi alt ağı üzerinde desteklenen kullanıcı tanımlı yollar hakkında bilgi edinmek için.

### <a name="what-are-the-limits-on-application-gateway-can-i-increase-these-limits"></a>Application Gateway'de sınırları nelerdir? Bu limitler artırabilirim?

Bkz: [Application Gateway limitlerinin](../azure-subscription-service-limits.md#application-gateway-limits) sınırları görüntülemek için.

### <a name="can-i-use-application-gateway-for-both-external-and-internal-traffic-simultaneously"></a>Application Gateway iç ve dış trafiği için aynı anda kullanabilir miyim?

Evet, uygulama ağ geçidini bir iç IP ve uygulama ağ geçidi başına bir dış IP destekler.

### <a name="is-vnet-peering-supported"></a>VNet eşlemesi destekleniyor mu?

Evet, VNet eşlemesi desteklenir ve diğer sanal ağlardaki trafiği Yük Dengelemesi için yararlıdır.

### <a name="can-i-talk-to-on-premises-servers-when-they-are-connected-by-expressroute-or-vpn-tunnels"></a>Şirket içi sunucular için ExpressRoute veya VPN tünelinde tarafından bağlıyken iletişim kurabilir?

Evet, trafiğe izin sürece.

### <a name="can-i-have-one-backend-pool-serving-many-applications-on-different-ports"></a>Birçok uygulama farklı bağlantı noktaları üzerinde hizmet veren bir arka uç havuzuna sahip olabilir miyim?

Mikro hizmet mimarisi desteklenir. Farklı bağlantı noktalarını araştırma için yapılandırılmış birden çok http ayarları gerekir.

### <a name="do-custom-probes-support-wildcardsregex-on-response-data"></a>Özel araştırmalar joker karakterler/regex yanıt verileri destekler mi?

Özel araştırmalar joker karakter veya normal ifade yanıt verileri desteklemez.

### <a name="how-are-rules-processed"></a>Kuralları nasıl işlenir?

Bkz: [kuralları işleme sırası](https://docs.microsoft.com/azure/application-gateway/configuration-overview#order-of-processing-rules) nasıl yönlendirme kuralları aşağıdaki noktaları kavramaktır Application Gateway'de işlemleri.

### <a name="what-does-the-host-field-for-custom-probes-signify"></a>Hangi özel araştırmalar için ana bilgisayar adı alanı geldiğiniz?

Ana bilgisayar alanı için araştırma göndermek için adını belirtir. Geçerli çok siteli, yalnızca uygulama ağ geçidinde yapılandırılan, aksi takdirde '127.0.0.1' kullanın. Bu değer VM ana bilgisayar adından farklıdır ve biçimi \<Protokolü\>://\<konak\>:\<bağlantı noktası\>\<yolu\>.

### <a name="can-i-whitelist-application-gateway-access-to-a-few-source-ips"></a>Alabilirim beyaz liste birkaç kaynak IP'leri Application Gateway erişimi?

Evet. Bkz: [belirli kaynak IP'leri erişimi kısıtlama](https://docs.microsoft.com/azure/application-gateway/configuration-overview#whitelist-application-gateway-access-to-a-few-source-ips) , yalnızca izin verilenler listesinde emin olmak nasıl anlamak için uygulama ağ geçidi kaynak IP'leri erişebilirsiniz.

### <a name="can-the-same-port-be-used-for-both-public-and-private-facing-listeners"></a>Aynı bağlantı noktasını hem genel hem de özel karşılıklı dinleyicileri için kullanılabilir mi?

Hayır, bu desteklenmez.

## <a name="configuration---ssl"></a>Yapılandırma - SSL

### <a name="what-certificates-are-supported-on-application-gateway"></a>Hangi sertifikaların Application Gateway üzerinde destekleniyor mu?

Otomatik olarak imzalanan sertifikaları, CA sertifikaları, EV sertifikaları ve joker kart sertifikaları desteklenir.

### <a name="what-are-the-current-cipher-suites-supported-by-application-gateway"></a>Uygulama ağ geçidi tarafından desteklenen geçerli şifre paketleri nelerdir?

Uygulama ağ geçidi tarafından desteklenen geçerli şifre paketleri şunlardır: Bkz: [yapılandırma SSL İlkesi sürümleri ve uygulama ağ geçidinde şifre paketleri](application-gateway-configure-ssl-policy-powershell.md) SSL seçeneklerini özelleştirme hakkında bilgi edinmek için.

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

### <a name="does-application-gateway-also-support-re-encryption-of-traffic-to-the-backend"></a>Application Gateway, aynı zamanda arka uca trafik yeniden şifrelenmesini destekliyor mu?

Evet, uygulama ağ geçidi SSL yük boşaltmasını ve uçtan uca SSL, arka uca trafiğini yeniden şifreler destekler.

### <a name="can-i-configure-ssl-policy-to-control-ssl-protocol-versions"></a>SSL protokolü sürümlerini denetlemek için SSL İlkesi yapılandırabilirim?

Evet, Application Gateway, TLS1.2 TLS1.0 ve TLS1.1 reddetmeyi yapılandırabilirsiniz. SSL 2.0 ve 3.0 zaten varsayılan olarak devre dışıdır ve yapılandırılabilir değildir.

### <a name="can-i-configure-cipher-suites-and-policy-order"></a>Şifre paketleri ve ilke sırasını yapılandırabilir miyim?

Evet, [şifre paketleri yapılandırmasını](application-gateway-ssl-policy-overview.md) desteklenir. Özel bir ilke tanımlandığında, aşağıdaki şifre paketleri en az biri etkinleştirilmelidir. Application gateway için arka uç Yönetim için SHA256 kullanılmaktadır.

* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
* TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_256_CBC_SHA256
* TLS_RSA_WITH_AES_128_CBC_SHA256

### <a name="how-many-ssl-certificates-are-supported"></a>Kaç tane SSL sertifikası desteklenir mi?

En fazla 100 SSL sertifikaları desteklenir.

### <a name="how-many-authentication-certificates-for-backend-re-encryption-are-supported"></a>Arka uç tekrar şifrelenmesi için kaç tane kimlik doğrulama sertifikası desteklenir mi?

En fazla 10 kimlik doğrulama sertifikaları varsayılan olarak 5 ile desteklenir.

### <a name="does-application-gateway-integrate-with-azure-key-vault-natively"></a>Application Gateway, Azure anahtar kasası ile yerel olarak tümleşik çalışıyor mu?

Hayır, Azure anahtar kasası ile tümleşikleştirilmemiştir.

### <a name="how-to-configure-https-listeners-for-com-and-net-sites"></a>.Com ve .net siteleri için HTTPS dinleyicisi yapılandırma nasıl? 

Birden çok etki alanı tabanlı (ana bilgisayar tabanlı) yönlendirme için çok siteli dinleyicileri oluşturabilir, HTTPS dinleyicisi Yapılandırma Protokolü olarak seçin ve dinleyicileri yönlendirme kuralları ile ilişkilendirin. Daha fazla ayrıntı için [uygulama ağ geçidi ile birden fazla siteyi barındırma](https://docs.microsoft.com/azure/application-gateway/multiple-site-overview). 

## <a name="configuration---web-application-firewall-waf"></a>Yapılandırma - Web uygulaması Güvenlik Duvarı (WAF)

### <a name="does-the-waf-sku-offer-all-the-features-available-with-the-standard-sku"></a>WAF SKU'su standart SKU ile kullanılabilen tüm özellikleri sunar?

Evet, WAF, standart SKU tüm özelliklerini destekler.

### <a name="what-is-the-crs-version-application-gateway-supports"></a>Application Gateway CRS sürümü nedir destekler?

Application Gateway CRS destekler [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) ve CRS [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).

### <a name="how-do-i-monitor-waf"></a>WAF nasıl izleyebilirim?

WAF tanılama günlüğüne kaydetme ile izlenen, tanılama günlüğüne kaydetme hakkında daha fazla bilgi bulabilirsiniz [günlüğe kaydetme tanılama ve uygulama ağ geçidi için ölçümleri](application-gateway-diagnostics.md)

### <a name="does-detection-mode-block-traffic"></a>Algılama modu akışa mu?

Hayır, algılama modunda yalnızca bir WAF kuralını tetikleyen trafiği günlüğe kaydeder.

### <a name="can-i-customize-waf-rules"></a>WAF kurallarını özelleştirebilirim?

Evet, WAF kurallarını özelleştirilebilir. Daha fazla bilgi için [özelleştirme WAF kural gruplarının ve kuralların](application-gateway-customize-waf-rules-portal.md)

### <a name="what-rules-are-currently-available"></a>İşleme hangi kuralların şu anda kullanılabilir

WAF, şu anda CRS destekler [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) ve [3.0](application-gateway-crs-rulegroups-rules.md#owasp30), çoğu açık Web uygulaması güvenlik Project (OWASP) tarafından tanımlanan en iyi 10 güvenlik açıklarına karşı temel güvenlik sağlayan burada bulunamadı[ OWASP ilk 10 Güvenlik](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013)

* SQL ekleme koruması

* Siteler arası komut dosyası koruması

* Komut ekleme, HTTP isteği kaçakçılığı, HTTP yanıtı bölme ve uzak dosya ekleme saldırıcı gibi Yaygın Web Saldırıları Koruması

* HTTP protokolü ihlallerine karşı koruma

* Eksik konak kullanıcısı-aracısı ve kabul üst bilgileri gibi HTTP protokolü anormalliklerine karşı koruma

* Robotlar, gezginler ve tarayıcıları önleme

* Yaygın yanlış uygulama yapılandırmalarını (diğer bir deyişle, Apache, IIS, vb.) algılama

### <a name="does-waf-also-support-ddos-prevention"></a>WAF, DDoS önleme de destekliyor mu?

Evet. DDos koruması uygulama ağ geçidi dağıtıldığı sanal ağ üzerinde etkinleştirebilirsiniz. Bu, VIP ayrıca Azure DDos koruması hizmetini kullanarak korumalı bir uygulama ağ geçidi sağlar.

## <a name="diagnostics-and-logging"></a>Tanılama ve günlüğe kaydetme

### <a name="what-types-of-logs-are-available-with-application-gateway"></a>Ne tür günlükleri ile uygulama ağ geçidi kullanılabilir mi?

Application Gateway için kullanılabilen üç günlükleri vardır. Bu günlükler ve diğer tanılama özellikleri hakkında daha fazla bilgi için bkz. [arka uç sistem durumu, tanılama günlükleri ve ölçümler için Application Gateway](application-gateway-diagnostics.md).

* **ApplicationGatewayAccessLog** -erişim günlüğü uygulama ağ geçidi ön uç için gönderilen her isteği içeriyor. Arayanın IP, istenen URL yanıt gecikme süresi verileri içeren, giriş ve çıkış dönüş kodu, bayt. Erişim günlüğü, her 300 saniyede toplanır. Bu günlük, uygulama ağ geçidi örneği başına tek bir kayıt içerir.
* **ApplicationGatewayPerformanceLog** -performans günlük hizmet, toplam istek dahil olmak üzere örnek başına aktarım hızı bayt üzerinde performans bilgileri yakalar, toplam istek sunulan, başarısız istek sayısı, sağlıklı ve sağlıksız arka uç örnek sayısı.
* **ApplicationGatewayFirewallLog** -Güvenlik Duvarı günlük algılama veya önleme modu web uygulaması güvenlik duvarı ile yapılandırılmış bir uygulama ağ geçidi üzerinden oturum istekleri içerir.

### <a name="how-do-i-know-if-my-backend-pool-members-are-healthy"></a>My arka uç havuzu üyelerine sağlıklı olup olmadığını nasıl anlarım?

PowerShell cmdlet'ini kullanabilirsiniz `Get-AzApplicationGatewayBackendHealth` veya sistem durumu Portalı aracılığıyla ziyaret ederek [uygulama ağ geçidi tanılama](application-gateway-diagnostics.md)

### <a name="what-is-the-retention-policy-on-the-diagnostics-logs"></a>Tanılama günlükleri ile ilgili bekletme ilkesi nedir?

Müşteriler depolama hesabına akışı tanılama günlükleri ve müşterilerin kendi tercihine göre bekletme ilkesi ayarlayabilir. Tanılama günlükleri de bir olay hub'ı veya Azure İzleyici günlüklerine gönderilebilir. Bkz: [uygulama ağ geçidi tanılama](application-gateway-diagnostics.md) daha fazla ayrıntı için.

### <a name="how-do-i-get-audit-logs-for-application-gateway"></a>Application Gateway için denetim günlüklerini nasıl alabilirim?

Denetim günlükleri, Application Gateway için kullanılabilir. Portalında **etkinlik günlüğü** menü dikey penceresinde, Denetim günlüğüne erişmek için bir uygulama ağ geçidi. 

### <a name="can-i-set-alerts-with-application-gateway"></a>Application Gateway uyarılarla ayarlayabilirim?

Evet, Application Gateway uyarıları desteklemiyor. Uyarılar, ölçümler üzerinde yapılandırılır. Bkz: [uygulama ağ geçidi ölçümleri](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics#metrics) uygulama ağ geçidi ölçümleri hakkında daha fazla bilgi edinmek için. Uyarılar hakkında daha fazla bilgi edinmek için [uyarı bildirimleri alma](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

### <a name="how-do-i-analyze-traffic-statistics-for-application-gateway"></a>Application Gateway için nasıl trafiği istatistikleri analiz?

Görüntüleyebilir ve Azure İzleyici günlüklerine, Excel, Power BI vb. gibi çeşitli mekanizmalar aracılığıyla erişim günlüklerini analiz edin.

Ayrıca, yükler ve popüler çalışan Resource Manager şablonu yayımladık [GoAccess](https://goaccess.io/) Çözümleyicisi uygulama ağ geçidi günlüklerine erişim için oturum açın. GoAccess benzersiz ziyaretçiler, istenen dosyaları, konaklar, işletim sistemleri, tarayıcılar, HTTP durum kodları ve daha fazlası gibi değerli HTTP trafiğini istatistikler sağlar. Daha fazla ayrıntı için lütfen bkz [GitHub Resource Manager şablonu bir klasörde Benioku dosyası](https://aka.ms/appgwgoaccessreadme).

### <a name="backend-health-returns-unknown-status-what-could-be-causing-this-status"></a>Arka uç sistem durumu bilinmeyen durum, bu durum çözebilmek amacıyla sorunun döndürür?

Erişim için en yaygın nedenidir arka uç, özel DNS, bir NSG tarafından engellenir veya uygulama ağ geçidi alt ağı üzerinde bir UDR sahip. Bkz: [arka uç sistem durumu, tanılama günlükleri ve ölçümler için Application Gateway](application-gateway-diagnostics.md) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki Adımlar

Bkz. Application Gateway hakkında daha fazla bilgi edinmek için [Azure Application Gateway nedir?](overview.md)
