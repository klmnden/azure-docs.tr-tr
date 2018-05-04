---
title: Azure uygulama ağ geçidi için sık sorulan sorular
description: Bu sayfa, Azure uygulama ağ geçidi hakkında sık sorulan soruların yanıtlarını sağlar.
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 3/29/2018
ms.author: victorh
ms.openlocfilehash: 37d069b1be86d59d0b1f79c382dc494b067cb934
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2018
---
# <a name="frequently-asked-questions-for-application-gateway"></a>Uygulama ağ geçidi için sık sorulan sorular

## <a name="general"></a>Genel

**Q. Uygulama ağ geçidi nedir?**

Azure uygulama ağ geçidi uygulama teslim denetleyici (ADC) bir hizmet olarak çeşitli katman 7 Yük Dengeleme, uygulamalarınız için sunumu ' dir. Azure tarafından tam olarak yönetilen, yüksek oranda kullanılabilir ve ölçeklenebilir hizmet sunar.

**Q. Hangi özelliklerin uygulama ağ geçidi destekliyor mu?**

Uygulama ağ geçidi SSL boşaltma ve uçtan uca SSL, Web uygulaması güvenlik duvarı, tanımlama bilgisi tabanlı oturum benzeşimi, url yolu tabanlı yönlendirme, çoklu siteyi barındıran ve diğerleri destekler. Desteklenen özelliklerin tam listesi için ziyaret [uygulama ağ geçidi giriş](application-gateway-introduction.md)

**Q. Uygulama ağ geçidi ve Azure yük dengeleyici arasındaki fark nedir?**

Uygulama ağ geçidi web trafiği ile yalnızca (HTTP/HTTPS/WebSocket) çalıştığı anlamına gelir bir katman 7 yük dengeleyicidir. Bu, Yük Dengeleme trafiğini SSL sonlandırma, tanımlama bilgisi tabanlı oturum benzeşimi ve hepsini bir kez gibi özellikleri destekler. Yük Dengeleyici, 4 (TCP/UDP) katmanında bakiyelerini trafiğin yük.

**Q. Hangi protokollerin, uygulama ağ geçidi destekliyor mu?**

Uygulama ağ geçidi, HTTP, HTTPS, HTTP/2 ve WebSocket destekler.

**Q. Uygulama ağ geçidi HTTP/2 nasıl destekler?**

HTTP/2 protokolü desteği yalnızca uygulama ağ geçidi dinleyicileri bağlanan istemciler kullanılabilir. Arka uç sunucu havuzu için HTTP/1.1 iletişimidir. 

Varsayılan olarak HTTP/2 desteği devre dışıdır. Azure PowerShell kod parçacığı aşağıda nasıl etkinleştirebilirsiniz gösterir:

```
$gw = Get-AzureRmApplicationGateway -Name test -ResourceGroupName hm
$gw.EnableHttp2 = $true
Set-AzureRmApplicationGateway -ApplicationGateway $gw
```

**Q. Hangi kaynaklara arka uç havuzu bir parçası olarak bugün destekleniyor mu?**

Arka uç havuzları olan NIC'ler, sanal makine ölçek kümeleri, genel IP'ler birleştirilebilen, iç IP, tam etki alanı adları (FQDN) ve çok kiracılı arka Azure Web Apps gibi uçları. Uygulama ağ geçidi arka uç havuzu üyelerini bir kullanılabilirlik kümesine bağlı değil. IP bağlantısını sahip oldukları sürece arka uç havuzları üyeleri kümeler, veri merkezleri arasında veya Azure dışında olabilir.

**Q. Hangi bölgeleri hizmettir kullanılabilir?**

Uygulama ağ geçidi genel Azure tüm bölgelerde kullanılabilir. Ayrıca, kullanılabilir [Azure Çin](https://www.azure.cn/) ve [Azure kamu](https://azure.microsoft.com/overview/clouds/government/)

**Q. Bu Aboneliğimi için adanmış bir dağıtımı veya müşterileri arasında paylaşılan?**

Uygulama ağ geçidi, sanal ağınızda adanmış bir dağıtımıdır.

**Q. İş HTTP -> desteklenen HTTPS yeniden yönlendirmesi?**

Yeniden yönlendirme desteklenir. Ziyaret [uygulama ağ geçidi yeniden yönlendirmeye genel bakış](application-gateway-redirect-overview.md) daha fazla bilgi için.

**Q. Hangi sırayla dinleyicileri işlenir?**

Dinleyicileri bunlar gösterilen sırada işlenir. Gelen bir istek temel dinleyici eşleşiyorsa, bu nedenle ilk işler.  Çok siteli dinleyicileri trafiği doğru arka uç yönlendirilmesini sağlamak için temel bir dinleyici önce yapılandırılması gerekir.

**Q. Uygulama ağ geçidi IP ve DNS nerede bulabilirim?**

Bir ortak IP adresi bir uç nokta kullanılırken, bu bilgileri ortak IP adresi kaynağı veya genel bakış sayfasında uygulama ağ geçidi için Portalı'nda bulunabilir. İç IP adresleri için bu genel bakış sayfasında bulunabilir.

**Q. IP veya DNS uygulama ağ geçidi ömrü boyunca değişiyor mu?**

Ağ geçidi durduruldu ve müşteri tarafından başlatılan VIP değiştirebilirsiniz. Uygulama ağ geçidi ile ilişkili DNS, ağ geçidi yaşam döngüsü değiştirmez. Bu nedenle, bir CNAME diğer adı kullanın ve uygulama ağ geçidi DNS adresi noktasındaki önerilir.

**Q. Uygulama ağ geçidi, statik IP destekliyor mu?**

Hayır, uygulama ağ geçidi ortak statik IP adresleri desteklemez, ancak statik iç IP desteklemiyor.

**Q. Uygulama ağ geçidi birden çok ortak IP ağ geçidinde destekliyor mu?**

Yalnızca bir genel IP adresi, bir uygulama ağ geçidi üzerinde desteklenir.

**Q. Uygulama ağ geçidi x-iletilen-için üstbilgiler destekliyor mu?**

Evet, uygulama ağ geçidi arka ucuna iletilen isteği x-iletilen-için x iletilen proto ve x iletilen bağlantı üstbilgileri ekler. X-iletilen-için üstbilgi biçimi IP: BağlantıNoktası, virgülle ayrılmış bir listesidir. Geçerli x iletilen proto http veya https değerler. X iletilen bağlantı aktarılma istek uygulama ağ geçidi üst sınırına bağlantı noktasını belirtir.

**Q. Ne kadar bir uygulama ağ geçidi dağıtmak için sürer? My uygulama ağ geçidi, güncelleştirilen zaman hala çalışıyor mu?**

Yeni uygulama ağ geçidi dağıtımları sağlamak için 20 dakika kadar sürebilir. Örnek boyutu/sayısı değişiklikler kesintiye uğratan değildir ve ağ geçidi bu süre boyunca etkin kalır.

## <a name="configuration"></a>Yapılandırma

**Q. Uygulama ağ geçidi, sanal bir ağa her zaman dağıtılır?**

Evet, uygulama ağ geçidi her zaman bir sanal ağ alt ağında dağıtılır. Bu alt ağ yalnızca uygulama ağ geçitleri içerebilir.

**Q. Uygulama ağ geçidi örnekleri kendi sanal ağ dışından iletişim kurabilirsiniz?**

Uygulama ağ geçidi IP bağlantısı var olduğu sürece olarak sanal ağ dışında örneklerine iletişim kurabilirsiniz. İç IP arka uç havuzu üye olarak kullanmayı planladığınız sonra gerektirdiği [VNET eşlemesi](../virtual-network/virtual-network-peering-overview.md) veya [VPN ağ geçidi](../vpn-gateway/vpn-gateway-about-vpngateways.md).

**Q. Uygulama ağ geçidi alt ağ içindeki başka bir şey dağıtabilir miyim?**

Hayır, ancak alt ağdaki diğer uygulama ağ geçitleri dağıtabilir.

**Q. Ağ güvenlik grupları uygulama ağ geçidi alt ağda destekleniyor mu?**

Ağ güvenlik grupları, aşağıdaki kısıtlamalarla uygulama ağ geçidi alt ağı üzerinde desteklenir:

* Özel durumlar gelen trafiği 65503-65534 düzgün çalışması arka uç sistem durumu için bağlantı noktalarında koyun gerekir.

* Giden internet bağlantısı engellenemez.

* AzureLoadBalancer etiketi gelen trafiği için izin verilmelidir.

**Q. Uygulama ağ geçidi sınırları nelerdir? Bu sınırları artırmak?**

Ziyaret [uygulama ağ geçidi sınırları](../azure-subscription-service-limits.md#application-gateway-limits) sınırları görüntülemek için.

**Q. Uygulama ağ geçidi iç ve dış trafiği için aynı anda kullanabilir miyim?**

Evet, uygulama ağ geçidini bir iç IP ve bir dış IP başına uygulama ağ geçidi destekler.

**Q. VNet eşlemesi destekleniyor mu?**

Evet, VNet eşlemesi desteklenir ve Yük Dengeleme trafiğini diğer sanal ağlarda için yararlıdır.

**Q. Şirket içi sunucular, ExpressRoute ya da VPN tünelleri tarafından bağlıyken iletişim kurabilirsiniz?**

Evet, trafiğe izin sürece.

**Q. Birçok uygulama farklı bağlantı noktaları üzerinde hizmet veren bir arka uç havuzu olabilir mi?**

Mikro hizmet mimarisi desteklenir. Farklı bağlantı noktalarında araştırması için yapılandırılmış birden çok http ayarları gerekir.

**Q. Özel araştırmalara joker karakter/regex yanıt verileri destekler mi?**

Özel araştırmalara joker karakter veya regex yanıt verileri desteklemez. 

**Q. Kuralları nasıl işlenir?**

Yapılandırılmış olan sırada işlenir. Temel kural değerlendirilen çok siteli kural önce bağlantı noktası göre trafiği eşleşecek şekilde bu trafiğin olasılığını azaltmak için uygun arka ucuna yönlendirilir basic kuralları önce çok siteli kuralları yapılandırılır önerilir.

**Q. Hangi özel araştırmalara için ana bilgisayar adı alanı belirtmek?**

Ana bilgisayar alanı araştırma göndermek için adını belirtir. Geçerli yalnızca çok siteli uygulama ağ geçidi üzerinde yapılandırılmış, aksi takdirde '127.0.0.1' kullanın. Bu değer VM ana bilgisayar adından farklı olduğundan ve biçimde \<Protokolü\>://\<konak\>:\<bağlantı noktası\>\<yolu\>.

**Q. Yapabilirim beyaz liste IP'leri birkaç kaynağına uygulama ağ geçidi erişim?**

Bu senaryo yapılabilir uygulama ağ geçidi alt ağda Nsg'leri kullanarak. Aşağıdaki kısıtlamalar alt listelenen öncelik sırasına sokulmalıdır:

* IP/IP aralığı kaynağından gelen trafiğe izin verecek.

* İçin bağlantı noktalarını 65503 65534 tüm kaynaklardan gelen isteklere izin [arka uç sağlık iletişimi](application-gateway-diagnostics.md).

* Gelen Azure yük dengeleyici araştırmalar (AzureLoadBalancer etiketi) ve gelen sanal ağ trafiği (VirtualNetwork etiketi) izin [NSG](../virtual-network/virtual-networks-nsg.md).

* Bir reddetme ile diğer tüm gelen trafiği engelle tüm kuralı.

* Tüm hedefler için internet giden trafiğe izin verin.

## <a name="performance"></a>Performans

**Q. Uygulama ağ geçidi, yüksek kullanılabilirlik ve ölçeklenebilirlik nasıl destekler?**

Uygulama ağ geçidi dağıtılan iki veya daha çok örneği varsa, yüksek kullanılabilirlik senaryolarını destekler. Azure Bu örnekler tüm örnekleri aynı anda başarısız olmayan emin olmak için güncelleştirme ve hata etki alanları arasında dağıtır. Uygulama ağ geçidi, yükü paylaştırmak için aynı ağ geçidi birden çok örneğini ekleyerek ölçeklenebilirlik destekler.

**Q. Nasıl ı DR senaryosuna uygulama ağ geçidi ile veri merkezleri arasında elde etmek?**

Müşteriler, trafik Yöneticisi, farklı veri merkezlerinde bulunan birden çok uygulama ağ geçidi üzerinden trafik dağıtmak için kullanabilirsiniz.

**Q. Otomatik ölçeklendirme destekleniyor mu?**

Hayır, ancak uygulama ağ geçidi bir Eşiğe ulaşıldığında, sizi uyarmak için kullanılan bir işleme ölçümü. El ile örnekleri ekleme veya boyutunu değiştirme ağ geçidini yeniden değil ve varolan trafiği etkilemez.

**Q. El ile ölçek yukarı/aşağı neden kapalı kalma süresi mu?**

Kapalı kalma süresi olmadan, örnekler yükseltme etki alanları ve hata etki alanları arasında dağıtılır.

**Q. Uygulama ağ geçidi destek bağlantı boşaltma mu?**

Evet. Bağlantı kesintisi olmadan bir arka uç havuzundaki üyeleri değiştirmek için boşaltma yapılandırabilirsiniz. Bu, bu bağlantı kapalı ya da yapılandırılabilir bir zaman aşımı süresi kadar önceki hedeflerine gönderilecek devam etmek var olan bağlantılara izin verir. Bu bağlantı yalnızca bekler tamamlamak geçerli yürütülen bağlantılarını boşaltma unutmayın. Uygulama ağ geçidi uygulama oturum durumunu uyumlu değil.


**Q. Örnek boyutu ortamından kesintiye uğratmadan büyük değiştirebilirim?**

Evet, Azure örnekleri tüm örnekleri aynı anda başarısız olmayan emin olmak için güncelleştirme ve hata etki alanları arasında dağıtır. Uygulama ağ geçidi, yükü paylaştırmak için aynı ağ geçidi birden çok örneğini ekleyerek ölçeklendirmeyi destekler.

## <a name="ssl-configuration"></a>SSL yapılandırması

**Q. Hangi sertifikaların uygulama ağ geçidinde destekleniyor mu?**

Otomatik olarak imzalanan sertifikaları, CA sertifikaları ve joker sertifikaları desteklenir. EV sertifikaları desteklenmez.

**Q. Uygulama ağ geçidi tarafından desteklenen geçerli şifre paketleri nelerdir?**

Uygulama ağ geçidi tarafından desteklenen geçerli şifre paketleri şunlardır: Ziyaret edin: [yapılandırma SSL İlkesi sürümleri ve şifre paketleri uygulama ağ geçidi üzerinde](application-gateway-configure-ssl-policy-powershell.md) SSL seçeneklerini özelleştirme hakkında bilgi edinmek için.

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

**Q. Uygulama ağ geçidi, ayrıca, arka uç trafiğinin yeniden şifrelenmesini destekliyor mu?**

Evet, uygulama ağ geçidi SSL yük boşaltımı ve arka uç trafiğini yeniden şifreler uçtan uca SSL destekler.

**Q. SSL protokol sürümleri denetlemek için SSL İlkesi yapılandırabilir miyim?**

Evet, uygulama ağ geçidi TLS1.0, TLS1.1 ve TLS1.2 reddedecek şekilde yapılandırabilirsiniz. SSL 2.0 ve 3.0 zaten varsayılan olarak devre dışıdır ve yapılandırılabilir değildir.

**Q. Şifre paketleri ve ilke sırasını yapılandırabilir miyim?**

Evet, [şifre paketleri yapılandırmasını](application-gateway-ssl-policy-overview.md) desteklenir. Özel bir ilke tanımlandığında, aşağıdaki şifre paketleri en az birinin etkinleştirilmesi gerekir. Uygulama ağ geçidi için SHA256 arka uç yönetimi için kullanır.

* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
* TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_256_CBC_SHA256
* TLS_RSA_WITH_AES_128_CBC_SHA256

**Q. Kaç tane SSL sertifikalarını destekleniyor mu?**

En fazla 20 SSL sertifikaları desteklenir.

**Q. Arka uç yeniden şifreleme için kaç tane kimlik doğrulama sertifikaları destekleniyor mu?**

En fazla 10 kimlik doğrulama sertifikaları 5 varsayılan ile desteklenir.

**Q. Uygulama ağ geçidi, Azure anahtar kasası ile yerel olarak tümleşik çalışıyor mu?**

Hayır, bunu Azure anahtar kasası ile tümleşiktir değil.

## <a name="web-application-firewall-waf-configuration"></a>Web uygulaması Güvenlik Duvarı (WAF) yapılandırması

**Q. WAF SKU standart SKU ile kullanılabilen tüm özellikleri sunar?**

Evet, WAF standart SKU tüm özelliklerini destekler.

**Q. Uygulama ağ geçidi CRS sürümü nedir destekler?**

Uygulama ağ geçidi destekleyen CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) ve CRS [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).

**Q. WAF nasıl izlerim?**

WAF tanılama günlük aracılığıyla izlenen, tanılama günlüğe kaydetme hakkında daha fazla bilgi bulunabilir [tanılama günlüğe kaydetme ve uygulama ağ geçidi ölçümleri](application-gateway-diagnostics.md)

**Q. Algılama modunu akışa mu?**

Hayır, algılama modunu yalnızca WAF kuralını tetikleyen trafiği günlüğe kaydeder.

**Q. WAF kuralları nasıl özelleştirebilirim?**

Evet, WAF kuralları ziyaret özelleştirme hakkında daha fazla bilgi için özelleştirilebilir [özelleştirme WAF kural gruplar ve kurallar](application-gateway-customize-waf-rules-portal.md)

**Q. Hangi kuralları şu anda kullanılabilir?**

WAF şu anda CRS destekler [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) ve [3.0](application-gateway-crs-rulegroups-rules.md#owasp30), açık Web uygulaması güvenlik proje (OWASP) tarafından tanımlanan en iyi 10 güvenlik açıklarını çoğunu karşı temel güvenlik sağlayan burada bulundu[ OWASP ilk 10 güvenlik açıkları](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013)

* SQL ekleme koruması

* Siteler arası komut dosyası koruması

* Komut ekleme, HTTP isteği kaçakçılığı, HTTP yanıtı bölme ve uzak dosya ekleme saldırıcı gibi Yaygın Web Saldırıları Koruması

* HTTP protokolü ihlallerine karşı koruma

* Eksik konak kullanıcısı-aracısı ve kabul üst bilgileri gibi HTTP protokolü anormalliklerine karşı koruma

* Robotlar, gezginler ve tarayıcıları önleme

 * Ortak uygulama yapılandırma hataları (diğer bir deyişle, Apache, IIS, vb.) algılanması

**Q. WAF de DDoS önleme destekliyor mu?**

Hayır, WAF DDoS önleme sağlamaz.

## <a name="diagnostics-and-logging"></a>Tanılama ve günlüğe kaydetme

**Q. Ne tür günlükleri ile uygulama ağ geçidi var mı?**

Uygulama ağ geçidi için üç günlükleri vardır. Bu günlükler ve diğer tanılama yetenekleri hakkında daha fazla bilgi için ziyaret [arka uç sistem durumu, tanılama günlüklerini ve uygulama ağ geçidi ölçümleri](application-gateway-diagnostics.md).

- **ApplicationGatewayAccessLog** -erişim günlüğü, her istek için uygulama ağ geçidi ön uç gönderildi içerir. Dönüş kodu, bayt giriş ve çıkış, çağıranın IP, istenen URL yanıt gecikme verileri içerir. Erişim günlüğüne 300 saniyede toplanır. Bu günlük, uygulama ağ geçidi örneği başına bir kayıt içerir.
- **ApplicationGatewayPerformanceLog** -performans günlüğü sunulan, toplam istek dahil olmak üzere her örneği bazında üretilen iş bayt performans bilgilerini yakalar, toplam istek sayısı sunulan, başarısız istek sayısı, sağlıklı ve sağlıksız arka uç örnek sayısı.
- **ApplicationGatewayFirewallLog** -Güvenlik Duvarı günlük algılama veya önleme modu web uygulaması güvenlik duvarı ile yapılandırılmış bir uygulama ağ geçidi üzerinden oturum isteklerini içerir.

**Q. My arka uç havuzu üyeleri sağlıklı olup olmadığını nasıl anlayabilirim?**

PowerShell cmdlet'ini kullanabilirsiniz `Get-AzureRmApplicationGatewayBackendHealth` veya sistem durumu Portalı aracılığıyla ziyaret ederek doğrulamak [uygulama ağ geçidi tanılama](application-gateway-diagnostics.md)

**Q. Tanılama günlüklerini üzerinde bekletme ilkesi nedir?**

Akış müşteriler depolama hesabına tanılama günlükleri ve müşterilerin kendi tercihine göre bekletme ilkesi ayarlayabilirsiniz. Tanılama günlükleri, bir olay hub'ını veya günlük analizi gönderilebilir. Ziyaret [uygulama ağ geçidi tanılama](application-gateway-diagnostics.md) daha fazla ayrıntı için.

**Q. Uygulama ağ geçidi için denetim günlüklerini nasıl sağlarım?**

Denetim günlükleri, uygulama ağ geçidi için kullanılabilir. Portalı'nda tıklatın **etkinlik günlüğü** menü dikey penceresinde, Denetim günlüğü erişmek için uygulama ağ geçidi. 

**Q. Uygulama ağ geçidi uyarılarla ayarlayabilir miyim?**

Evet, uygulama ağ geçidi uyarıları destek, uyarılar ölçümleri yapılandırılır. Uygulama ağ geçidi şu anda yapılandırılabilir "işleme" ölçüsü yok uyarı. Uyarılar hakkında daha fazla bilgi için [uyarı bildirimleri alma](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

**Q. Arka uç sistem durumu bilinmeyen durum, bu durum neden olabilecek verir?**

En yaygın nedeni, bir NSG veya özel DNS tarafından arka uç erişim engellendi ' dir. Ziyaret [arka uç sistem durumu, tanılama günlüğe kaydetme ve uygulama ağ geçidi ölçümleri](application-gateway-diagnostics.md) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki Adımlar

Uygulama ağ geçidi ziyaret hakkında daha fazla bilgi edinmek için [Azure uygulama ağ geçidi nedir?](overview.md)
