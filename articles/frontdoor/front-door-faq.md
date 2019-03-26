---
title: Azure ön kapısı hizmeti - ön kapı için sık sorulan sorular | Microsoft Docs
description: Bu sayfa, Azure ön kapısı hizmeti hakkında sık sorulan soruların yanıtlarını sağlar
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/08/2019
ms.author: sharadag
ms.openlocfilehash: 256435dfd016ebbd86dbbe49f4abbb346fb1cd19
ms.sourcegitcommit: 280d9348b53b16e068cf8615a15b958fccad366a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58407760"
---
# <a name="frequently-asked-questions-for-azure-front-door-service"></a>Azure ön kapısı hizmeti için sık sorulan sorular

Bu makalede, Azure ön kapısı hizmet özellikleri ve işlevleri hakkında sık sorulan sorular yanıtlanmaktadır. Sorunuzun yanıtını görmüyorsanız, aşağıdaki kanalları (sırayla yükselen) üzerinden bize başvurabilirsiniz:

1. Bu makalede Açıklamalar bölümü.
2. [Azure ön kapısı hizmet UserVoice](https://feedback.azure.com/forums/217313-networking?category_id=345025).
3. **Microsoft desteği:** Azure portalında yeni bir destek isteği oluşturmak için **yardımcı** sekmesinde **Yardım + Destek** düğmesini ve ardından **yeni destek isteği**.

## <a name="general"></a>Genel

### <a name="what-is-azure-front-door-service"></a>Azure Front Door Hizmeti nedir?

Azure ön kapısı bir uygulama teslim ağı (ADN) bir hizmet olarak sunan çeşitli 7. Katman Yük Dengeleme Özellikleri uygulamalarınız için hizmetidir. Dinamik site Hızlandırma (DSA) ile birlikte gerçek zamanlı bir yük devretme ile küresel Yük Dengeleme sağlar. Azure tarafından tam olarak yönetilen bir yüksek oranda kullanılabilir ve ölçeklenebilir hizmet var.

### <a name="what-features-does-azure-front-door-service-support"></a>Hangi özellikler Azure ön kapısı hizmet destekliyor mu?

Dinamik site Hızlandırma (DSA), SSL yük boşaltma ve uçtan uca SSL, Web uygulaması güvenlik duvarı, tanımlama bilgilerine dayalı oturum benzeşimi, url yolu tabanlı yönlendirme, ücretsiz sertifikaları ve birden çok etki alanı yönetimi ve diğer Azure ön kapısı hizmet destekler. Desteklenen özelliklerin tam bir listesi için bkz. [Azure ön kapısı hizmet genel bakış](front-door-overview.md).

### <a name="what-is-the-difference-between-azure-front-door-service-and-azure-application-gateway"></a>Azure ön kapısı Service ve Azure Application Gateway arasındaki fark nedir?

Hem ön kapısı hem de uygulama ağ geçidi olan katman 7 (HTTP/HTTPS) yük Dengeleyiciler, ancak birincil fark uygulama ağ geçidi bölgesel bir hizmettir ancak ön kapı küresel hizmet olmasıdır. Ön kapısı farklı ölçek kümeleri/birim/damga Birimleriniz bölgeler arasında bir denge yüklenirken Application Gateway, içinde ölçek birimimiz, VM'ler/kapsayıcılar vb. arasında bir denge sağlar.

### <a name="when-should-we-deploy-an-application-gateway-behind-front-door"></a>Ne zaman size bir uygulama ağ geçidi arkasında ön kapısı dağıtmanız gerekir mi?

Bir uygulama ağ geçidinin arkasına ön kapısı neden kullanması gereken temel senaryolar şunlardır:

- Ön kapısı, yol tabanlı yük bir uygulama ağ geçidi kullanmanız gerekir, trafiğin yükünü dengelemenizi daha da kendi sanal ağına (VNET) içinde yüklemek isteyip istemediğini ancak yalnızca genel düzeyde Dengeleme gerçekleştirebilir.
- Bir VM/kapsayıcı düzeyinde ön kapısı işe yaramazsa olduğundan, bu nedenle, bağlantı boşaltma yapamazsınız. Ancak, Application Gateway bağlantı boşaltma yapmanıza olanak tanır. 
- AFD arkasındaki bir Application Gateway ile bir 100 SSL yük boşaltma ve yalnızca HTTP isteklerini kendi sanal ağına (VNET) içinde yol % elde edebilirsiniz.
- Ön kapı ve Application Gateway oturum benzeşimi destekler. Ön kapısı aynı kümede veya arka uç belirli bir bölgede bir kullanıcı oturumundan sonraki trafiği yönlendirebilirsiniz, ancak uygulama ağ geçidi doğrudan aynı sunucuya küme içindeki trafiği affinitize.  

### <a name="can-we-deploy-azure-load-balancer-behind-front-door"></a>Biz, Azure yük dengeleyici ön kapısı arkasında dağıtabilir miyim?

Azure ön kapısı hizmet trafiği yönlendirmek için genel bir VIP veya genel kullanıma açık bir DNS adı gerekiyor. Bir Azure yük dengeleyici arkasında ön kapısı dağıtma, ortak bir kullanım örneği olur.

### <a name="what-protocols-does-azure-front-door-service-support"></a>Hangi protokollerin, Azure ön kapısı hizmet destekliyor mu?

Azure ön kapısı hizmeti, HTTP, HTTPS ve HTTP/2 destekler.

### <a name="how-does-azure-front-door-service-support-http2"></a>HTTP/2 Azure ön kapısı hizmetini nasıl destekler?

HTTP/2 protokolü desteği, Azure ön kapısı yalnızca hizmetine istemciler tarafından kullanılabilir. HTTP/1.1 arka uç havuzuna arka uçları iletişimdir. HTTP/2 desteği, varsayılan olarak etkindir.

### <a name="what-resources-are-supported-today-as-part-of-backend-pool"></a>Hangi kaynakların arka uç havuzunun bir parçası bugün destekleniyor mu?

Arka uç havuzları, depolama, Web uygulaması, Kubernetes örnekleri veya genel bağlantısı olan herhangi bir özel hostname oluşabilir. Azure ön kapısı hizmeti Arka Uçlara genel bir IP veya genel olarak çözümlenebilen bir DNS ana bilgisayar adı aracılığıyla tanımlanan gerektirir. Genel bağlantı sahip oldukları sürece arka uç havuzu üyelerinin alanları, bölgeler genelinde veya hatta Azure dışında olabilir.

### <a name="what-regions-is-the-service-available-in"></a>Hangi bölgeler kullanılabilir hizmet?

Azure ön kapısı hizmeti, küresel bir hizmettir ve herhangi belirli bir Azure bölgesine bağlı değildir. Bir ön kapısı oluşturulurken belirtmeniz gereken yalnızca temel kaynak grubu için meta verileri nerede depolanacağını belirten kaynak grubu konumu konumdur. Ön kapısı kaynağının genel bir kaynak olarak oluşturulur ve yapılandırma, genel olarak tüm Pop'lere (varlık noktası) dağıtılır. 

### <a name="what-are-the-pop-locations-for-azure-front-door-service"></a>POP konumları Azure ön kapısı hizmeti nedir?

Microsoft Azure CDN POP (varlık noktası) konumları listesiyle aynı Azure ön kapısı hizmeti vardır. Bizim POP tam listesi için genişliğinizin bakın [Microsoft Azure CDN POP konumları](https://docs.microsoft.com/azure/cdn/cdn-pop-locations).

### <a name="is-azure-front-door-service-a-dedicated-deployment-for-my-application-or-is-it-shared-across-customers"></a>Azure ön kapısı Service Uygulamam için adanmış dağıtımıdır veya müşteriler arasında paylaşılır?

Azure ön kapısı Global olarak dağıtılmış çok kiracılı bir hizmet hizmetidir. Bu nedenle, altyapı için ön kapı, müşterilerinin arasında paylaşılır. Ancak, ön kapısı oluşturarak, uygulamanız için gereken belirli yapılandırmasını tanımlamak ve 

### <a name="is-http-https-redirection-supported"></a>İş HTTP -> HTTPS yeniden yönlendirmesi desteklenir?

Ön kapısı, URL yeniden yönlendirmesi şu anda desteklemiyor.

### <a name="in-what-order-are-routing-rules-processed"></a>Yönlendirme kuralları hangi sırayla işlendiklerinden?

Yollar, ön kapı için değil sıralanır ve en iyi eşleşmeye göre belirli bir yol seçildi. Daha fazla bilgi edinin [nasıl ön kapısı eşleşen istek yönlendirme kuralı için](front-door-route-matching.md).

### <a name="how-do-i-lock-down-the-access-to-my-backend-to-only-azure-front-door-service"></a>Nasıl my arka uca ön kapısı hizmetine yalnızca Azure alt klasörlere kilitleme miyim erişim?

Yalnızca Azure ön kapısı hizmetinden gelen trafiği kabul etmek için IP başarısız uçlarınıza için yapılandırabilirsiniz. Uygulamanız yalnızca Azure ön kapısı Service arka uç IP alanı gelen gelen bağlantıları kabul edecek şekilde kısıtlayabilirsiniz. İle tümleştirmeye yönelik çalışmalarımız devam [Azure IP aralıkları ve hizmet etiketleriyle](https://www.microsoft.com/download/details.aspx?id=56519) ancak aşağıda gösterildiği gibi IP aralıklarını artık bkz:
 
- **IPv4** - `147.243.0.0/16`
- **IPv6** - `2a01:111:2050::/44`

> [!WARNING]
> Bizim arka uç IP alanı daha sonra değişebilir, bu, ile tümleştirdik, gerçekleşmeden önce ancak biz emin olur [Azure IP aralıkları ve hizmet etiketleriyle](https://www.microsoft.com/download/details.aspx?id=56519). İçin abone öneririz [Azure IP aralıkları ve hizmet etiketleriyle](https://www.microsoft.com/download/details.aspx?id=56519) tüm değişiklikler ve güncelleştirmeler için. 

### <a name="can-the-anycast-ip-change-over-the-lifetime-of-my-front-door"></a>Anycast IP my ön kapısı ömrü boyunca değiştirebilir miyim?

Ön uç anycast IP, ön kapı için genellikle değil değiştirmeniz ve ön kapısı ömrü boyunca statik kalır. Ancak, **garanti** için aynı. Genişliğinizin doğrudan bağımlılıkları IP almaz.  

### <a name="does-azure-front-door-service-support-static-or-dedicated-ips"></a>Azure ön kapısı hizmeti, statik ya da ayrılmış IP'ler destekliyor mu?

Hayır, Azure ön kapısı Service statik ya da ayrılmış bir ön uç anycast IP'ler şu anda desteklemiyor. 

### <a name="does-azure-front-door-service-support-x-forwarded-for-headers"></a>Azure ön kapısı hizmet x-iletilen-için üstbilgiler destekliyor mu?

Evet, Azure ön kapısı hizmeti X-iletilen-için X iletilen konak ve X iletilen Proto üstbilgileri destekler. Örneğin, X-iletilen-için başlık zaten mevcut sonra istemci yuvası IP için ön kapı ekler. Aksi takdirde istemci yuvası IP üstbilgisi değeri olarak ekler. X iletilen konak ve X iletilen Proto için değer geçersiz kılınır.

Daha fazla bilgi edinin [ön kapısı desteklenen HTTP üstbilgileri](front-door-http-headers-protocol.md).  

### <a name="how-long-does-it-take-to-deploy-an-azure-front-door-service-does-my-front-door-still-work-when-being-updated"></a>Ne kadar Azure ön kapısı hizmeti dağıtmak için sürer? My ön kapısı, güncelleştiriliyor, hala çalışıyor mu?

Yeni bir ön kapısı oluşturma veya var olan bir ön kapısı herhangi bir güncelleştirme, global dağıtım hakkında 3-5 dakika sürer. Bu, yaklaşık 3 ila 5 dakika içinde ön kapısı yapılandırmanızı tüm müşterilerimizin POP'ları arasında genel olarak dağıtılacak anlamına gelir.

Not - özel SSL sertifika güncelleştirmelerini genel olarak dağıtılacak yaklaşık 30 dakika alın.

## <a name="configuration"></a>Yapılandırma

### <a name="can-azure-front-door-load-balance-or-route-traffic-within-a-virtual-network"></a>Bir sanal ağ içindeki Azure ön kapısı Yük Dengeleme ya da rota trafik?

Azure ön kapısı (AFD) bir genel IP veya trafiği yönlendirmek için genel olarak çözümlenebilen DNS adı gerektirir. Bu nedenle, yanıttır hiçbir AFD doğrudan bir sanal ağ içinde yönlendirme yapamazsınız, ancak bir uygulama ağ geçidi ya da Azure Load Balancer arasında kullanarak bu senaryoyu çözmek.

### <a name="what-are-the-various-timeouts-and-limits-for-azure-front-door-service"></a>Çeşitli zaman aşımları ve Azure ön kapısı hizmet sınırları nelerdir?

Hakkında bilgi edinin belgelenmiş [zaman aşımları ve Azure ön kapısı hizmet sınırlarını](https://docs.microsoft.com/azure/azure-subscription-service-limits#azure-front-door-service-limits).

## <a name="performance"></a>Performans

### <a name="how-does-azure-front-door-service-support-high-availability-and-scalability"></a>Azure ön kapısı hizmetini, yüksek kullanılabilirlik ve ölçeklenebilirlik nasıl destekler?

Azure ön kapı hizmeti muazzam miktarlarda uygulamanızın ölçeklenebilirlik gereksinimlerinize uygun kapasite ile Global olarak dağıtılmış, çok kiracılı platformdur. Global Microsoft ağının kenarından teslim ön kapı, tüm uygulama veya hatta ayrı ayrı mikro hizmetler bölgelerde veya farklı Bulutlar arasında başarısız olanak tanır, genel Yük Dengeleme özelliği sağlar.

## <a name="ssl-configuration"></a>SSL yapılandırması

### <a name="what-tls-versions-are-supported-by-azure-front-door-service"></a>Hangi TLS sürümlerini Azure ön kapısı hizmeti tarafından destekleniyor mu?

Ön kapısı, TLS 1.0, 1.1 ve 1.2 sürümleri destekler. TLS 1.3 henüz desteklenmiyor.

### <a name="what-certificates-are-supported-on-azure-front-door-service"></a>Hangi sertifikaların Azure ön kapısı hizmette destekleniyor mu?

Güvenli bir şekilde ön kapısı ile özel bir etki alanında bulunan içerik teslim etmek üzere HTTPS protokolü etkinleştirmek için Azure ön kapısı hizmet tarafından yönetilen bir sertifika kullanın veya kendi sertifikanızı kullanmayı seçebilirsiniz.
Ön kapısı, Digicert aracılığıyla standart bir SSL sertifikası seçeneğini hükümlerine yönetilen ve önde kapı'nın Key Vault depolanır. Kendi sertifikanızı kullanmayı seçerseniz ardından desteklenen bir CA'dan bir sertifika ekleyebilir ve standart SSL, Genişletilmiş Doğrulama sertifikası veya hatta bir joker karakter sertifika olabilir. Otomatik olarak imzalanan sertifikalar desteklenmez. Bilgi [için özel bir etki alanı HTTPS etkinleştirme](https://aka.ms/FrontDoorCustomDomainHTTPS).

### <a name="what-are-the-current-cipher-suites-supported-by-azure-front-door-service"></a>Azure ön kapısı hizmeti tarafından desteklenen geçerli şifre paketleri nelerdir?

Azure ön kapısı hizmeti tarafından desteklenen geçerli şifre paketleri şunlardır:

- TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_AES_256_GCM_SHA384
- TLS_RSA_WITH_AES_128_GCM_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA256
- TLS_RSA_WITH_AES_128_CBC_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA
- TLS_RSA_WITH_AES_128_CBC_SHA

### <a name="does-azure-front-door-service-also-support-re-encryption-of-traffic-to-the-backend"></a>Azure ön kapısı hizmeti, ayrıca arka uca trafik yeniden şifrelenmesini destekliyor mu?

Evet, Azure ön kapısı hizmeti SSL yük boşaltmasını ve uçtan uca SSL, arka uca trafiğini yeniden şifreler destekler. Aslında, arka uç bağlantısı üzerinden gerçekleşir beri genel IP olduğundan, yönlendirme protokolü olarak HTTPS kullanmak üzere, ön kapısı yapılandırma tavsiye edilir.

### <a name="can-i-configure-ssl-policy-to-control-ssl-protocol-versions"></a>SSL protokolü sürümlerini denetlemek için SSL İlkesi yapılandırabilirim?

Hayır, şu anda belirli TLS sürümlerini reddetmek için ön kapı desteklemiyor ya da en düşük TLS sürümlerini ayarlayabilirsiniz. 

### <a name="can-i-configure-front-door-to-only-support-specific-cipher-suites"></a>Yalnızca belirli şifre paketleri desteklemek için ön kapı yapılandırabilirim?

Hayır, belirli şifre paketleri için ön kapı yapılandırılması desteklenmiyor. 

## <a name="diagnostics-and-logging"></a>Tanılama ve günlüğe kaydetme

### <a name="what-types-of-metrics-and-logs-are-available-with-azure-front-door-service"></a>Hangi tür kaynakların ölçümlerini ve günlüklerini Azure ön kapısı hizmetiyle kullanılabilir mi?

Günlükleri ve diğer tanılama özellikleri hakkında daha fazla bilgi için bkz: [ölçüm ve günlükleri için ön kapı izleme](front-door-diagnostics.md).

### <a name="what-is-the-retention-policy-on-the-diagnostics-logs"></a>Tanılama günlükleri ile ilgili bekletme ilkesi nedir?

Müşteriler depolama hesabına akışı tanılama günlükleri ve müşterilerin kendi tercihine göre bekletme ilkesi ayarlayabilir. Tanılama günlükleri de bir olay hub'ı veya Azure İzleyici günlüklerine gönderilebilir. Daha fazla bilgi için [Azure ön kapısı hizmet tanılama](front-door-diagnostics.md).

### <a name="how-do-i-get-audit-logs-for-azure-front-door-service"></a>Denetim günlüklerini Azure ön kapısı hizmeti için nasıl alabilirim?

Denetim günlükleri, Azure ön kapısı hizmeti için kullanılabilir. Portalında **etkinlik günlüğü** Denetim günlüğüne erişmek için ön kapısı menü dikey penceresindeki. 

### <a name="can-i-set-alerts-with-azure-front-door-service"></a>Uyarılar Azure ön kapısı hizmetiyle ayarlayabilirim?

Evet, Azure ön kapısı hizmet uyarılarını destekler. Uyarılar, ölçümler üzerinde yapılandırılır. 

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.