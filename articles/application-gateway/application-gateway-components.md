---
title: Uygulama ağ geçidi bileşenleri
description: Bu makalede bir uygulama ağ geçidinde çeşitli bileşenleri hakkında bilgi sağlar.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/20/2019
ms.author: absha
ms.openlocfilehash: f5dfa34760bcef23bf54d65b35e3ad8f48cc2ee5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60831845"
---
# <a name="application-gateway-components"></a>Uygulama ağ geçidi bileşenleri

 Bir uygulama ağ geçidi tek istemcilerin iletişim noktası işlevi görür. Bu, Azure Vm'leri, sanal makine ölçek kümeleri, Azure App Service ve şirket içi/dış sunucuları içeren birden fazla arka uç havuzu, gelen uygulama trafiğini dağıtır. Trafiği dağıtmak için çeşitli bileşenleri bu makalede açıklanan uygulama ağ geçidi kullanır.

![Bir uygulama ağ geçidinde kullanılan bileşenler](./media/application-gateway-components/application-gateway-components.png)

## <a name="frontend-ip-addresses"></a>Ön uç IP adresleri

Bir ön uç IP adresi, bir uygulama ağ geçidi ile ilişkili IP adresidir. Uygulama ağ geçidi genel IP adresi, özel bir IP adresi veya her ikisini de yapılandırabilirsiniz. Bir uygulama ağ geçidi, bir genel veya özel bir IP adresi destekler. Sanal ağ ve genel IP adresi, application gateway'iniz ile aynı konumda olmalıdır. Oluşturulduktan sonra bir ön uç IP adresini dinleyici ile ilişkilidir.

### <a name="static-versus-dynamic-public-ip-address"></a>Statik ve dinamik genel IP adresi

V1 SKU yalnızca statik iç IP adreslerini destekler; ancak hem statik iç ve statik genel IP adresleri, Azure Application Gateway v2 SKU destekler. Bir uygulama ağ geçidi durduruldu ve başlatıldığında sanal IP (VIP) adresi değiştirebilirsiniz.

Bir uygulama ağ geçidi ile ilişkili DNS adı, ağ geçidi yaşam döngüsü değiştirmez. Sonuç olarak, bir CNAME diğer adlarını kullanma ve uygulama ağ geçidinin DNS adresine işaret gerekir.

## <a name="listeners"></a>Dinleyiciler

Dinleyici için gelen bağlantı istekleri denetleyen bir mantıksal bir varlıktır. Protokol, bağlantı noktası, konak ve istekle ilişkili IP adresi aynı öğeleri dinleyici yapılandırmasıyla ilişkili eşleşiyorsa bir dinleyici isteği kabul eder.

Bir uygulama ağ geçidi kullanmadan önce en az bir dinleyici eklemeniz gerekir. Bir uygulama ağ geçidine eklenmiş birden çok dinleyici olabilir ve aynı protokolü için kullanılabilir.

İstemcilerden gelen istekleri bir dinleyici algıladıktan sonra uygulama ağ geçidi bu istekleri arka uç havuzu üyelerine yönlendirir. Uygulama ağ geçidi için gelen bir istek aldı dinleyici tanımlanan istek yönlendirme kurallarını kullanır.

Dinleyiciler, aşağıdaki bağlantı noktaları ve protokolleri destekler.

### <a name="ports"></a>Bağlantı Noktaları

Burada istemci isteği için bir dinleyici dinleyen bir bağlantı noktasıdır. 1-v2 SKU için v1 SKU ve 1 için 65199 65502 arasında bağlantı noktaları yapılandırabilirsiniz.

### <a name="protocols"></a>Protokoller

Uygulama ağ geçidi dört protokollerini destekler: HTTP, HTTPS, HTTP/2 ve WebSocket:

- HTTP ve HTTPS arasında protokolleri dinleyici yapılandırmasına belirtin.
- Destek [WebSockets ve HTTP/2 protokolleri](https://docs.microsoft.com/azure/application-gateway/overview#websocket-and-http2-traffic) sayfalarını yerel olarak sağlanır ve [WebSocket desteği](https://docs.microsoft.com/azure/application-gateway/application-gateway-websocket) varsayılan olarak etkindir. WebSocket desteğini isteğe bağlı olarak etkinleştirmek veya devre dışı bırakmak için kullanıcı tarafından yapılandırılabilen bir ayar yoktur. WebSockets, hem HTTP hem de HTTPS dinleyicileri ile kullanın.
- HTTP/2 protokolü desteği, yalnızca uygulama ağ geçidi dinleyicileri bağlanan istemciler için kullanılabilir. HTTP/1.1 arka uç sunucu havuzlarına iletişimdir. Varsayılan olarak, HTTP/2 desteği devre dışıdır. Etkinleştirmek seçebilirsiniz.

Bir HTTPS dinleyicisi için SSL sonlandırma kullanır. Web sunucularınız tarafından yükü burdened olmayan biçimde bir HTTPS dinleyicisi application gateway'iniz için şifreleme ve şifre çözme iş üzerinizden alır. Ardından, uygulamalarınızın iş mantığına odaklanırsınız ücretsizdir.

### <a name="custom-error-pages"></a>Özel hata sayfaları

Uygulama ağ geçidi, varsayılan hata sayfalarını görüntüleme yerine özel hata sayfaları oluşturmanızı sağlar. Özel hata sayfası sayesinde kendi logonuzu ve sayfa düzeninizi kullanabilirsiniz. Bir isteği arka uç ulaştığında uygulama ağ geçidi özel hata sayfası görüntüler.

Daha fazla bilgi için [uygulama ağ geçidiniz için özel hata sayfaları](https://docs.microsoft.com/azure/application-gateway/custom-error).

### <a name="types-of-listeners"></a>Dinleyici türü

Dinleyicileri iki tür vardır:

- **Temel**. Bu tür bir dinleyicisi, tek bir DNS eşlemesi uygulama ağ geçidinin IP adresine sahip olduğu bir tek etki alanı siteye dinler. Tek bir sitede bir uygulama ağ geçidi arkasında barındırdığınızda, bu dinleyici yapılandırması gereklidir.

- **Çok siteli**. Aynı uygulama ağ geçidi örneğinde birden fazla web uygulaması yapılandırdığınızda bu dinleyici yapılandırmasına gerek yoktur. Bir uygulama ağ geçidine en fazla 100 Web sitesi ekleyerek dağıtımlarınız için daha verimli bir topoloji yapılandırmanıza olanak sağlar. Her web sitesi, kendi arka uç havuzuna yönlendirilebilir. Örneğin, üç alt etki alanlarını, abc.contoso.com, xyz.contoso.com ve pqr.contoso.com, uygulama ağ geçidinin IP adresine işaret edecek. Üç çok siteli dinleyicileri oluşturma ve ilgili bağlantı noktası ve protokol ayarları için her bir dinleyici yapılandırma.

    Daha fazla bilgi için [birden çok site barındırma](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview).

Dinleyici oluşturduktan sonra bir istek yönlendirme kuralı ile ilişkilendirin. Bu kural, dinleyici alınan isteği arka uca nasıl yönlendirileceğini belirler.

Uygulama ağ geçidi dinleyicileri gösterilen sırada işler. Temel dinleyici gelen bir istekle eşleşiyorsa, önce işlenir. Trafiği doğru arka ucuna yönlendirmek için bir çoklu site dinleyicisi önce temel dinleyiciyi yapılandırın.

## <a name="request-routing-rules"></a>İstek yönlendirme kuralları

İstek yönlendirme kuralı bir uygulama ağ geçidi temel bir bileşenidir çünkü Dinleyicide trafik yönlendirme hakkında belirler. Kural dinleyiciyi arka uç sunucu havuzu ve arka uç HTTP ayarları bağlar.

İstek bir dinleyici kabul ettiğinde, istek yönlendirme kuralı isteği arka uca iletir veya başka bir yerde yeniden yönlendirir. İstek arka ucuna iletilirse, istek yönlendirme kuralı hangi arka uç sunucu havuzuna ilet tanımlar. Ayrıca, istek yönlendirme kuralı da isteğinde üstbilgileri yazılması olup olmadığını belirler. Bir kural için bir dinleyici eklenebilir.

İstek yönlendirme kuralları iki tür vardır:

- **Temel**. İlgili dinleyiciyi (örneğin, blog.contoso.com/*) tüm istekleri ilişkili arka uç havuzuna ilişkili HTTP ayarı kullanılarak iletilir.

- **Yol tabanlı**. Bu yönlendirme kuralı istekleri için istek URL'SİNDE dayalı belirli bir arka uç havuzu, ilişkili dinleyici üzerindeki yol sağlar. Bir istek URL'SİNDE yolu bir yol tabanlı kural içindeki yol deseni eşleşiyorsa, kural bu isteği yönlendirir. Yol deseni uygulandığı yalnızca URL yolu için değil, kendi sorgu parametreleri. Dinleyici isteğinde bir URL yolu yol tabanlı kurallardan herhangi birinin eşleşmezse, HTTP ayarları ve varsayılan arka uç havuzu için isteği yönlendirir.

Daha fazla bilgi için [URL tabanlı yönlendirme](https://docs.microsoft.com/azure/application-gateway/url-route-overview).

### <a name="redirection-support"></a>Yeniden yönlendirme desteği

İstek yönlendirme kuralı uygulama ağ geçidinde trafiği yeniden yönlendirme sağlar. İçin yönlendirebilir ve herhangi bir bağlantı noktasından kuralları kullanarak tanımladığınız bir genel yönlendirme mekanizma budur.

(Bu otomatik HTTP-HTTPS yeniden yönlendirmesi etkinleştirmek yardımcı olabilir) başka bir dinleyici veya dış bir siteye yeniden yönlendirme hedefi seçebilirsiniz. Ayrıca, geçici veya kalıcı yeniden yönlendirme olması ya da yeniden yönlendirilen URI yolu ve sorgu dizesini URL'ye de seçebilirsiniz.

Daha fazla bilgi için [, uygulama ağ geçidinde trafiği yeniden yönlendirme](https://docs.microsoft.com/azure/application-gateway/redirect-overview).

### <a name="rewrite-http-headers"></a>HTTP üst bilgilerini yeniden üretme

İstek yönlendirme kurallarını kullanarak ekleyin, kaldırın veya güncelleştirme isteği HTTP (S) ve yanıt üstbilgileri, istek ve yanıt paketleri olarak arka uç ve istemci arasında havuzları uygulama ağ geçidi aracılığıyla taşıyın.

Üstbilgileri statik değerlerine veya diğer üst bilgileri ve sunucu değişkenleri ayarlayabilirsiniz. Bu, istemci IP adresleri hakkında daha fazla güvenlik ekleme arka uç, hassas bilgileri kaldırılıyor, ayıklama gibi durumlarda önemli ile kullanın ve benzeri yardımcı olur.

Daha fazla bilgi için [application gateway'iniz yeniden HTTP üstbilgilerine](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers).

## <a name="http-settings"></a>HTTP ayarları

Bir uygulama ağ geçidi bağlantı noktası, protokol ve bu bileşeni ayrıntılı diğer ayarları kullanarak (HTTP ayarları içeren istek yönlendirme kuralında belirtilir) arka uç sunucularına trafiği yönlendirir.

HTTP ayarlarında kullanılan protokolü ve bağlantı noktası uygulama ağ geçidi ve arka uç sunucuları arasındaki trafiğin (uçtan uca SSL sağlama) şifrelenmiş olup olmadığını belirlemek veya şifrelenmemiş.

Bu bileşen için de kullanılır:

- Kullanıcı oturumunu aynı sunucu üzerinde kullanarak tutulması olup olmadığını [tanımlama bilgilerine dayalı oturum benzeşimi](https://docs.microsoft.com/azure/application-gateway/overview#session-affinity).

- Düzgün bir şekilde kullanarak arka uç havuzu üyeleri kaldırma [bağlantı boşaltma](https://docs.microsoft.com/azure/application-gateway/overview#connection-draining).

- Arka uç sistem durumu izleme, istek zaman aşımı aralığı ayarlamak, ana bilgisayar adı ve istek yolunda geçersiz kılmak ve App Service arka ucu ayarlarını belirtmek için tek tıklamayla kolayca sağlamak için özel bir araştırma ilişkilendirin.

## <a name="backend-pools"></a>Arka uç havuzları

Arka uç havuzu hizmet isteği arka uç sunucuları için isteği yönlendirir. Arka uç havuzları içerebilir:

- NIC’ler
- Sanal makine ölçek kümeleri
- Genel IP adresleri
- İç IP adresleri
- FQDN
- Çok kiracılı arka uçlar (örneğin, App Service)

Uygulama ağ geçidi arka uç havuzu üyeleri, bir kullanılabilirlik kümesine bağlı değildir. Bir uygulama ağ geçidi örnekleri içinde sanal ağın dışında iletişim kurabilir. IP bağlantısı var olduğu sürece sonuç olarak, arka uç havuzu üyelerinin kümelerinde, veri merkezleri ya da Azure dışından arasında olabilir.

Arka uç havuzu üyesi olarak iç IP'ler kullanırsanız kullanmalısınız [sanal ağ eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) veya [VPN ağ geçidi](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Sanal Ağ eşlemesi desteklenen ve için Yük Dengeleme diğer sanal ağlardaki trafiğidir.

Trafiğe izin gerekiyorsa Azure ExpressRoute veya VPN tünelinde tarafından bağlı olduğunuzda bir uygulama ağ geçidi aynı zamanda şirket içi sunucular için iletişim kurabilir.

Farklı türde istekler için farklı arka uç havuzları oluşturabilirsiniz. Örneğin, genel istekleri için bir arka uç havuzu ve mikro hizmetler, uygulamanızın isteklere için başka bir arka uç havuzu oluşturun.

## <a name="health-probes"></a>Sistem durumu araştırmaları

Varsayılan olarak, bir uygulama ağ geçidi, arka uç havuzundaki tüm kaynakların izler ve iyi durumda olmayan yorumlar otomatik olarak kaldırır. Bunu daha sonra iyi durumda olmayan örnekler izler ve kullanılabilir hale gelir ve sistem durumu araştırmaları için yanıt sağlam bir arka uç havuzuna eklenir.

Varsayılan sistem durumu izleme yoklaması kullanmanın yanı sıra durum yoklaması, uygulamanızın gereksinimlerine uyacak şekilde özelleştirebilirsiniz. Özel araştırmalar sistem durumu izleme daha ayrıntılı denetim sağlar. Özel araştırmalar kullanırken, araştırma aralığı, URL ve test etmek için yolu ve arka uç havuzu örnek sağlıksız olarak işaretlenmeden önce kabul etmek için kaç başarısız yanıtları yapılandırabilirsiniz. Her arka uç havuzu durumunu izlemek için özel araştırmalar yapılandırmanızı öneririz.

Daha fazla bilgi için [uygulama ağ geçidinizin durumunu izlemek](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview).

## <a name="next-steps"></a>Sonraki adımlar

Bir uygulama ağ geçidi oluşturun:

* [Azure portalında](quick-create-portal.md)
* [Azure PowerShell kullanarak](quick-create-powershell.md)
* [Azure CLI kullanarak](quick-create-cli.md)
