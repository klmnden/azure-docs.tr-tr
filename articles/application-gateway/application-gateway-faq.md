---
title: Azure Application Gateway için sık sorulan sorular
description: Bu sayfa, Azure Application Gateway hakkında sık sorulan soruların yanıtlarını sağlar
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 9/6/2018
ms.author: victorh
ms.openlocfilehash: e180f0912bdb9145f3d36492fb9bcdcb551037f0
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "44022925"
---
# <a name="frequently-asked-questions-for-application-gateway"></a>Application Gateway için sık sorulan sorular

## <a name="general"></a>Genel

**SORU. Application Gateway nedir?**

Azure Application Gateway bir uygulama teslim denetleyicisi (ADC) hizmet olarak, çeşitli 7. Katman Yük Dengeleme Özellikleri uygulamalarınız için tekliftir. Azure tarafından tam olarak yönetilen, yüksek oranda kullanılabilir ve ölçeklenebilir hizmet sunar.

**SORU. Hangi özellikleri, uygulama ağ geçidi destekliyor mu?**

Uygulama ağ geçidi SSL yük boşaltma ve uçtan uca SSL, Web uygulaması güvenlik duvarı, tanımlama bilgilerine dayalı oturum benzeşimi, url yolu tabanlı yönlendirme, çoklu site barındırmayı ve diğerleri destekler. Desteklenen özelliklerin tam listesi için ziyaret [Application Gateway'e giriş](application-gateway-introduction.md)

**SORU. Azure Load Balancer ile uygulama ağ geçidi arasındaki fark nedir?**

Uygulama ağ geçidi ile web trafiğini yalnızca (HTTP/HTTPS/WebSocket) çalıştığı anlamına gelir. bir katman 7 yük dengeleyicidir. Bu, Yük Dengeleme trafiği için SSL sonlandırma, tanımlama bilgilerine dayalı oturum benzeşimi ve hepsini bir kez deneme gibi özellikleri destekler. Yük Dengeleyici, katman 4 (TCP/UDP) trafiği dengeleyen yük.

**SORU. Hangi protokollerin, Application Gateway destekliyor mu?**

Application Gateway, HTTP, HTTPS, HTTP/2 ve WebSocket destekler.

**SORU. Application Gateway, HTTP/2 nasıl destekler?**

HTTP/2 protokolü desteği, yalnızca uygulama ağ geçidi dinleyicilere bağlanan istemciler için kullanılabilir. HTTP/1.1 arka uç sunucu havuzlarına iletişimdir. 

Varsayılan olarak, HTTP/2 desteği devre dışıdır. Aşağıdaki Azure PowerShell kod parçacığı örneği nasıl olanak sağlayabileceğiniz gösterir:

```
$gw = Get-AzureRmApplicationGateway -Name test -ResourceGroupName hm
$gw.EnableHttp2 = $true
Set-AzureRmApplicationGateway -ApplicationGateway $gw
```

**SORU. Hangi kaynakların arka uç havuzunun bir parçası bugün destekleniyor mu?**

Arka uç havuzları, ağ, sanal makine ölçek kümeleri, genel IP'ler birleştirilebilir, iç IP'ler, tam etki alanı adlarını (FQDN) ve çok kiracılı arka-Azure Web Apps gibi biter. Uygulama ağ geçidi arka uç havuzu üyeleri bir kullanılabilirlik kümesine bağlı değil. IP bağlantısı sahip oldukları sürece arka uç havuzu üyelerinin kümeleri, veri merkezleri arasında veya Azure dışında olabilir.

**SORU. Hangi bölgeler kullanılabilir hizmet?**

Uygulama ağ geçidi, tüm genel Azure bölgelerinde kullanılabilir. Ayrıca kullanılabilir, [Azure Çin](https://www.azure.cn/) ve [Azure kamu](https://azure.microsoft.com/overview/clouds/government/)

**SORU. Bu adanmış bir dağıtım için Aboneliğimi veya müşteriler arasında paylaşılır?**

Uygulama ağ geçidi, sanal ağınızda ayrılmış bir dağıtımıdır.

**SORU. İş HTTP -> HTTPS yeniden yönlendirmesi desteklenir?**

Yeniden yönlendirme desteklenir. Ziyaret [Application Gateway yeniden yönlendirmeye genel bakış](application-gateway-redirect-overview.md) daha fazla bilgi için.

**SORU. Hangi sırayla dinleyicileri işlenir?**

Dinleyiciler, gösterilen sırada işlenir. Temel dinleyici gelen bir istekle eşleşiyorsa, bu nedenle ilk önce işler.  Çok siteli dinleyicileri, trafiğin doğru arka uca yönlendirilmesini sağlamak için temel bir dinleyici önce yapılandırılmalıdır.

**SORU. Uygulama ağ geçidinin IP ve DNS nerede bulabilirim?**

Genel bir IP adresi bir uç nokta kullanırken, bu bilgileri genel IP adresi kaynağı veya genel bakış sayfasında uygulama ağ geçidi için Portalı'nda bulunabilir. İç IP adresleri için bu genel bakış sayfasında bulunabilir.

**SORU. Uygulama ağ geçidi yaşam süresi boyunca, DNS ve IP değişiyor mu?**

Ağ geçidi durduruldu ve müşteri tarafından başlatılan VIP değiştirebilirsiniz. Uygulama ağ geçidiyle ilişkili DNS, ağ geçidi yaşam döngüsü değiştirmez. Bu nedenle, bir CNAME diğer adlarını kullanma ve uygulama ağ geçidinin DNS adresine işaret önerilir.

**SORU. Application Gateway, statik IP destekliyor mu?**

Hayır, Application Gateway statik genel IP adreslerini desteklemez, ancak statik iç IP desteklemiyor.

**SORU. Application Gateway, ağ geçidinde birden çok genel IP destekliyor mu?**

Yalnızca bir genel IP adresi, bir uygulama ağ geçidinde desteklenmiyor.

**SORU. Ne kadar büyük alt Application Gateway için uygulamalıyım?**

Özel ön uç IP yapılandırması yapılandırılmışsa, uygulama ağ geçidi örneği başına bir özel IP adresi yanı sıra, başka bir özel IP adresini kullanır. Ayrıca, Azure ilk dört ayırır ve son her alt ağda iç kullanım için IP adresi.
Örneğin, üç örnek ve ardından bir/29 hiçbir özel ön uç IP uygulama ağ geçidi ayarlanırsa boyutu veya daha fazla alt ağ gereklidir. Bu durumda, uygulama ağ geçidi üç IP adresini kullanır. Üç örnekleri ve ardından/28 özel ön uç IP yapılandırması için bir IP adresi varsa dört IP adresleri gerekli olduğu gibi alt ağı veya büyük boyut gereklidir.

**SORU. Application Gateway, x-iletilen-için üstbilgiler destekliyor mu?**

Evet, Application Gateway arka ucuna iletilen istek x-iletilen-için x iletilen proto ve x iletilen bağlantı üstbilgileri ekler. X-iletilen-için üst bilgi biçimi IP: BağlantıNoktası, virgülle ayrılmış bir listesidir. Geçerli değerler x iletilen proto için http veya https ' dir. X iletilen bağlantı, uygulama ağ geçidinde istek sınırına bağlantı noktasını belirtir.

Uygulama ağ geçidi ile gelen isteği özgün ana bilgisayar üst bilgisini içeren X özgün konak üst bilgisi de ekler. Bu başlığı trafiğin arka uca yönlendirilmesini önce gelen barındırma üst bilgisi nerede değişiklik Azure Web sitesi tümleştirmesi, bu gibi senaryolarda yararlı olur.

**SORU. Ne kadar bir uygulama ağ geçidini dağıtmak için sürer? My Application Gateway, güncelleştirilen olduğunda hala çalışıyor mu?**

Yeni uygulama ağ geçidi dağıtımları sağlamak 20 dakikaya kadar sürebilir. Örnek boyutu/sayısı değişiklikler kesintiye uğratan değildir ve ağ geçidi bu süre boyunca etkin kalır.

## <a name="configuration"></a>Yapılandırma

**SORU. Application Gateway, her zaman bir sanal ağda dağıtılır?**

Evet, uygulama ağ geçidi her zaman bir sanal ağ alt ağında dağıtılır. Bu alt ağ, yalnızca uygulama ağ geçitleri içerebilir.

**SORU. Uygulama ağ geçidi örnekleri kendi sanal ağın dışında iletişim kurabilir?**

Uygulama ağ geçidi IP bağlantısı var olduğu sürece, içinde bir sanal ağ dışında örneklerine konuşabilirsiniz. İç IP'ler arka uç havuzu üyesi olarak kullanmayı planladığınız sonra gerektiren [VNET eşlemesi](../virtual-network/virtual-network-peering-overview.md) veya [VPN ağ geçidi](../vpn-gateway/vpn-gateway-about-vpngateways.md).

**SORU. Uygulama ağ geçidi alt ağında bir şey dağıtabilir miyim?**

Hayır, ancak alt ağdaki diğer uygulama ağ geçitleri dağıtabilir.

**SORU. Uygulama ağ geçidi alt ağı üzerinde ağ güvenlik grupları desteklenir?**

Ağ güvenlik grupları, uygulama ağ geçidi alt ağı aşağıdaki kısıtlamalarla aşağıdakilerde desteklenmektedir:

* Özel durumlar için gelen trafiği 65503 65534 bağlantı noktaları üzerinde yerleştirilmesi gereken. Bu bağlantı noktası aralığı, Azure altyapı iletişimi için gereklidir. Bunlar Azure sertifikaları tarafından korunur (kilitlenir). Uygun sertifikaları olmadan, bu ağ geçitlerinin müşterileri dahil dış varlıklar, bu uç noktalarında herhangi bir değişiklik başlatmak mümkün olmayacaktır.

* Giden internet bağlantısı engellenemez.

* AzureLoadBalancer etiketini gelen trafiğe izin verilmesi gerekir.

**SORU. Kullanıcı tanımlı yollara uygulama ağ geçidi alt ağı üzerinde destekleniyor mu?**

Uçtan uca istek/yanıt iletişim değiştirmeyin sürece, kullanıcı tanımlı yollar (Udr) uygulama ağ geçidi alt ağı üzerinde desteklenir.

Örneğin, uygulama ağ geçidi alt ağındaki UDR paket incelemesi için bir güvenlik duvarı Gereci işaret edecek şekilde ayarlayabilirsiniz ancak paket, istenen hedef posta İnceleme ulaşabildiğimizden emin olmanız gerekir. Bunun yapılmaması, yanlış sistem durumu araştırma ya da trafiği yönlendirme davranışını neden olabilir. Bu öğrenilen rotalar veya sanal ağ, ExpressRoute veya VPN ağ geçitleri tarafından yayılan varsayılan 0.0.0.0/0 yolları içerir.

**SORU. Application Gateway'de sınırları nelerdir? Bu limitler artırabilirim?**

Ziyaret [Application Gateway limitlerinin](../azure-subscription-service-limits.md#application-gateway-limits) sınırları görüntülemek için.

**SORU. Application Gateway iç ve dış trafiği için aynı anda kullanabilir miyim?**

Evet, uygulama ağ geçidini bir iç IP ve uygulama ağ geçidi başına bir dış IP destekler.

**SORU. VNet eşlemesi destekleniyor mu?**

Evet, VNet eşlemesi desteklenir ve diğer sanal ağlardaki trafiği Yük Dengelemesi için yararlıdır.

**SORU. Şirket içi sunucular için ExpressRoute veya VPN tünelinde tarafından bağlıyken iletişim kurabilir?**

Evet, trafiğe izin sürece.

**SORU. Birçok uygulama farklı bağlantı noktaları üzerinde hizmet veren bir arka uç havuzuna sahip olabilir miyim?**

Mikro hizmet mimarisi desteklenir. Farklı bağlantı noktalarını araştırma için yapılandırılmış birden çok http ayarları gerekir.

**SORU. Özel araştırmalar joker karakterler/regex yanıt verileri destekler mi?**

Özel araştırmalar joker karakter veya normal ifade yanıt verileri desteklemez. 

**SORU. Kuralları nasıl işlenir?**

Kurallar, bunların sırayla işlenir. Temel kural önce değerlendirilen kural çok siteli bağlantı trafiğini BC gibi bu trafiğin olasılığını azaltmak için uygun arka uca yönlendirilir temel kuralları önce çok siteli kuralları yapılandırıldığını önerilir.

**SORU. Hangi özel araştırmalar için ana bilgisayar adı alanı geldiğiniz?**

Ana bilgisayar alanı için araştırma göndermek için adını belirtir. Geçerli çok siteli, yalnızca uygulama ağ geçidinde yapılandırılan, aksi takdirde '127.0.0.1' kullanın. Bu değer VM ana bilgisayar adından farklıdır ve biçimi \<Protokolü\>://\<konak\>:\<bağlantı noktası\>\<yolu\>.

**SORU. Alabilirim beyaz liste birkaç kaynak IP'leri Application Gateway erişimi?**

Bu senaryo yapılabilir Nsg'leri kullanarak uygulama ağ geçidi alt ağı üzerinde. Aşağıdaki kısıtlamalar alt listelenen öncelik sırasına koymanız gerekir:

* IP/IP aralığı kaynağından gelen trafiğe izin veren.

* Tüm kaynaklardan gelen istekleri için 65503 65534 numaralı bağlantı noktalarına izin [arka uç sistem durumu iletişimi](application-gateway-diagnostics.md). Bu bağlantı noktası aralığı, Azure altyapı iletişimi için gereklidir. Bunlar Azure sertifikaları tarafından korunur (kilitlenir). Uygun sertifikaları olmadan, bu ağ geçitlerinin müşterileri dahil dış varlıklar, bu uç noktalarında herhangi bir değişiklik başlatmak mümkün olmayacaktır.

* Gelen Azure Load Balancer araştırmaları (AzureLoadBalancer etiketi) ve sanal noktalarında gelen ağ trafiğini (VirtualNetwork etiketi) izin [NSG](../virtual-network/security-overview.md).

* Diğer tüm gelen trafiği bir reddetme kuralı tüm engelleyin.

* Tüm hedefler için İnternet'e giden trafiğe izin verin.

**SORU. Aynı bağlantı noktasını hem genel hem de özel karşılıklı dinleyicileri için kullanılabilir mi?**

Hayır, bu desteklenmez.

## <a name="performance"></a>Performans

**SORU. Application Gateway, yüksek kullanılabilirlik ve ölçeklenebilirlik nasıl destekler?**

Dağıtılan bir veya daha fazla örneğe sahip olduğunuzda, uygulama ağ geçidi yüksek kullanılabilirlik senaryolarını destekler. Azure, bu örnekler, tüm örnekleri aynı anda başarısız olmayın emin olmak için güncelleştirme ve hata etki alanları arasında dağıtır. Application Gateway, yük paylaşmak için aynı ağ geçidinin birden çok örnek ekleyerek ölçeklenebilirliği desteklemektedir.

**SORU. Nasıl uygulama ağ geçidi ile veri merkezleri arasında alt klasörlere elde ediyorum DR senaryosu edebilirim?**

Müşteriler, Traffic Manager, farklı veri merkezlerindeki birden fazla uygulama ağ geçidi üzerinden trafiği dağıtmak için kullanabilirsiniz.

**SORU. Otomatik ölçeklendirme destekleniyor mu?**

Hayır, ancak uygulama ağ geçidine sahip bir Eşiğe ulaşıldığında, sizi uyarmak için kullanılabilir bir aktarım hızı ölçümü. Örnekleri ekleme veya değiştirme boyutu ağ geçidini yeniden değil ve varolan trafiği etkilemez.

**SORU. El ile ölçeklendirmenin yukarı/aşağı neden kapalı kalma süresi mu?**

Kapalı kalma süresi, örnekleri, yükseltme etki alanları ve hata etki alanları arasında dağıtılır.

**SORU. Uygulama ağ geçidi desteği bağlantı boşaltma mu?**

Evet. Bağlantı boşaltma kesintiye uğratmadan arka uç havuzundaki üyelerini değiştirmek için yapılandırabilirsiniz. Bu, bu bağlantı kapalı ya da yapılandırılabilir bir zaman aşımı süresi kadar önceki hedeflerine gönderilecek devam etmek var olan bağlantılara izin verir. Bu bağlantı yalnızca bekler tamamlamak geçerli uçuşan bağlantı boşaltma unutmayın. Uygulama ağ geçidi uygulaması oturum durumunu uyumlu değildir.

**SORU. Uygulama ağ geçidi boyutları nelerdir?**

Application Gateway şu anda üç büyüklükte sunulmaktadır: **Kısa**, **Orta** ve **Uzun**. Küçük örnek boyutları, geliştirme ve test senaryolarına yöneliktir.

Bir abonelik için en fazla 50 uygulama ağ geçidi oluşturabilirsiniz ve her uygulama ağ geçidi en fazla 10 örnek içerebilir. Her uygulama ağ geçidi 20 http dinleyicisinden oluşabilir. Application Gateway limitlerinin tam listesi için bkz. [Application Gateway hizmet limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#application-gateway-limits).

Aşağıdaki tabloda, SSL boşaltmasının etkin olduğu her bir Application Gateway örneği için ortalama performans aktarım hızı gösterilmiştir:

| Ortalama arka uç sayfa yanıtı boyutu | Küçük | Orta | Büyük |
| --- | --- | --- | --- |
| 6 KB |7,5 Mbps |13 Mbps |50 Mbps |
| 100 KB |35 Mbps |100 Mbps |200 Mbps |

> [!NOTE]
> Bu değerler bir uygulama ağ geçidi verimliliği için yaklaşık değerlerdir. Gerçek verimlilik; ortalama sayfa boyutu, arka uç örneklerinin konumu ve bir sayfaya hizmet etmek için işleme süresi gibi çeşitli ortam ayrıntılarına bağlıdır. Tam performans rakamlarına ulaşmak için kendi testlerinizi çalıştırmanız gerekir. Bu değerler yalnızca kapasite planlama konusunda yardımcı olmak için verilmiştir.

**SORU. Örnek boyutu ortamından kesintiye uğratmadan büyük değiştirebilirim?**

Evet, Azure örnekleri tüm örnekleri aynı anda başarısız olmayın emin olmak için güncelleştirme ve hata etki alanları arasında dağıtır. Application Gateway yük paylaşmak için aynı ağ geçidini birden çok örneğini ekleyerek ölçeği genişletmeyi destekler.

## <a name="ssl-configuration"></a>SSL Yapılandırması

**SORU. Hangi sertifikaların Application Gateway üzerinde destekleniyor mu?**

Kendinden imzalı sertifikaları, CA sertifikaları ve joker kart sertifikaları desteklenir. EV sertifikaları desteklenmez.

**SORU. Uygulama ağ geçidi tarafından desteklenen geçerli şifre paketleri nelerdir?**

Uygulama ağ geçidi tarafından desteklenen geçerli şifre paketleri şunlardır: Ziyaret edin: [yapılandırma SSL İlkesi sürümleri ve uygulama ağ geçidinde şifre paketleri](application-gateway-configure-ssl-policy-powershell.md) SSL seçeneklerini özelleştirme hakkında bilgi edinmek için.

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

**SORU. Application Gateway, aynı zamanda arka uca trafik yeniden şifrelenmesini destekliyor mu?**

Evet, uygulama ağ geçidi SSL yük boşaltmasını ve uçtan uca SSL, arka uca trafiğini yeniden şifreler destekler.

**SORU. SSL protokolü sürümlerini denetlemek için SSL İlkesi yapılandırabilirim?**

Evet, Application Gateway, TLS1.2 TLS1.0 ve TLS1.1 reddetmeyi yapılandırabilirsiniz. SSL 2.0 ve 3.0 zaten varsayılan olarak devre dışıdır ve yapılandırılabilir değildir.

**SORU. Şifre paketleri ve ilke sırasını yapılandırabilir miyim?**

Evet, [şifre paketleri yapılandırmasını](application-gateway-ssl-policy-overview.md) desteklenir. Özel bir ilke tanımlandığında, aşağıdaki şifre paketleri en az biri etkinleştirilmelidir. Application gateway için arka uç Yönetim için SHA256 kullanılmaktadır.

* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
* TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_256_CBC_SHA256
* TLS_RSA_WITH_AES_128_CBC_SHA256

**SORU. Kaç tane SSL sertifikası desteklenir mi?**

En fazla 20 SSL sertifikaları desteklenir.

**SORU. Arka uç tekrar şifrelenmesi için kaç tane kimlik doğrulama sertifikası desteklenir mi?**

En fazla 10 kimlik doğrulama sertifikaları varsayılan olarak 5 ile desteklenir.

**SORU. Application Gateway, Azure anahtar kasası ile yerel olarak tümleşik çalışıyor mu?**

Hayır, Azure anahtar kasası ile tümleşikleştirilmemiştir.

## <a name="web-application-firewall-waf-configuration"></a>Web uygulaması Güvenlik Duvarı (WAF) yapılandırma

**SORU. WAF SKU'su standart SKU ile kullanılabilen tüm özellikleri sunar?**

Evet, WAF, standart SKU tüm özelliklerini destekler.

**SORU. Application Gateway CRS sürümü nedir destekler?**

Application Gateway CRS destekler [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) ve CRS [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).

**SORU. WAF nasıl izleyebilirim?**

WAF tanılama günlüğüne kaydetme ile izlenen, tanılama günlüğüne kaydetme hakkında daha fazla bilgi bulabilirsiniz [günlüğe kaydetme tanılama ve uygulama ağ geçidi için ölçümleri](application-gateway-diagnostics.md)

**SORU. Algılama modu akışa mu?**

Hayır, algılama modunda yalnızca bir WAF kuralını tetikleyen trafiği günlüğe kaydeder.

**SORU. WAF kurallarını nasıl özelleştiririm?**

Evet, WAF kurallarını bunları ziyareti özelleştirme hakkında daha fazla bilgi için özelleştirilebilir [özelleştirme WAF kural gruplarının ve kuralların](application-gateway-customize-waf-rules-portal.md)

**SORU. İşleme hangi kuralların şu anda kullanılabilir mi?**

WAF, şu anda CRS destekler [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) ve [3.0](application-gateway-crs-rulegroups-rules.md#owasp30), çoğu açık Web uygulaması güvenlik Project (OWASP) tarafından tanımlanan en iyi 10 güvenlik açıklarına karşı temel güvenlik sağlayan burada bulunamadı[ OWASP ilk 10 Güvenlik](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013)

* SQL ekleme koruması

* Siteler arası komut dosyası koruması

* Komut ekleme, HTTP isteği kaçakçılığı, HTTP yanıtı bölme ve uzak dosya ekleme saldırıcı gibi Yaygın Web Saldırıları Koruması

* HTTP protokolü ihlallerine karşı koruma

* Eksik konak kullanıcısı-aracısı ve kabul üst bilgileri gibi HTTP protokolü anormalliklerine karşı koruma

* Robotlar, gezginler ve tarayıcıları önleme

 * Yaygın yanlış uygulama yapılandırmalarını (diğer bir deyişle, Apache, IIS, vb.) algılama

**SORU. WAF, DDoS önleme de destekliyor mu?**

Evet. DDos koruması uygulama ağ geçidi dağıtıldığı sanal ağ üzerinde etkinleştirebilirsiniz. Bu, VIP ayrıca Azure DDos koruması hizmetini kullanarak korumalı bir uygulama ağ geçidi sağlar.

## <a name="diagnostics-and-logging"></a>Tanılama ve günlüğe kaydetme

**SORU. Ne tür günlükleri ile uygulama ağ geçidi kullanılabilir mi?**

Application Gateway için kullanılabilen üç günlükleri vardır. Bu günlükler ve diğer tanılama özellikleri hakkında daha fazla bilgi için ziyaret [arka uç sistem durumu, tanılama günlükleri ve ölçümler için Application Gateway](application-gateway-diagnostics.md).

- **ApplicationGatewayAccessLog** -erişim günlüğü uygulama ağ geçidi ön uç için gönderilen her isteği içeriyor. Arayanın IP, istenen URL yanıt gecikme süresi verileri içeren, giriş ve çıkış dönüş kodu, bayt. Erişim günlüğü, her 300 saniyede toplanır. Bu günlük, uygulama ağ geçidi örneği başına tek bir kayıt içerir.
- **ApplicationGatewayPerformanceLog** -performans günlük hizmet, toplam istek dahil olmak üzere örnek başına aktarım hızı bayt üzerinde performans bilgileri yakalar, toplam istek sunulan, başarısız istek sayısı, sağlıklı ve sağlıksız arka uç örnek sayısı.
- **ApplicationGatewayFirewallLog** -Güvenlik Duvarı günlük algılama veya önleme modu web uygulaması güvenlik duvarı ile yapılandırılmış bir uygulama ağ geçidi üzerinden oturum istekleri içerir.

**SORU. My arka uç havuzu üyelerine sağlıklı olup olmadığını nasıl anlarım?**

PowerShell cmdlet'ini kullanabilirsiniz `Get-AzureRmApplicationGatewayBackendHealth` veya sistem durumu Portalı aracılığıyla ziyaret ederek [uygulama ağ geçidi tanılama](application-gateway-diagnostics.md)

**SORU. Tanılama günlükleri ile ilgili bekletme ilkesi nedir?**

Müşteriler depolama hesabına akışı tanılama günlükleri ve müşterilerin kendi tercihine göre bekletme ilkesi ayarlayabilir. Bir olay hub'ı veya Log Analytics için tanılama günlüklerini da gönderilebilir. Ziyaret [uygulama ağ geçidi tanılama](application-gateway-diagnostics.md) daha fazla ayrıntı için.

**SORU. Application Gateway için denetim günlüklerini nasıl alabilirim?**

Denetim günlükleri, Application Gateway için kullanılabilir. Portalında **etkinlik günlüğü** menü dikey penceresindeki Denetim günlüğüne erişmek için uygulama ağ geçidi. 

**SORU. Application Gateway uyarılarla ayarlayabilirim?**

Evet, Application Gateway uyarıları destekleyen, uyarılar, ölçümler devre dışı yapılandırılır. Application Gateway şu anda sahiptir yapılandırılabilir "işleme" ölçüsü uyarı. Uyarılar hakkında daha fazla bilgi edinmek için [uyarı bildirimleri alma](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

**SORU. Arka uç sistem durumu bilinmeyen durum, bu durum çözebilmek amacıyla sorunun döndürür?**

Arka uç erişimi bir NSG ya da özel DNS tarafından engelleniyor en yaygın nedenidir. Ziyaret [arka uç sistem durumu, tanılama günlükleri ve ölçümler için Application Gateway](application-gateway-diagnostics.md) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki Adımlar

Application Gateway ziyaret hakkında daha fazla bilgi edinmek için [Azure Application Gateway nedir?](overview.md)
