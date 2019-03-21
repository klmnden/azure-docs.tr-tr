---
title: Azure uygulama ağ geçidi bileşenleri
description: Bu makalede, çeşitli bileşenler Application Gateway hakkında bilgi sağlar.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/20/2019
ms.author: absha
ms.openlocfilehash: 44c8b331ebb258c39a003c91e0711e6dfb87cb12
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58076312"
---
# <a name="application-gateway-components"></a>Uygulama ağ geçidi bileşenleri

 Bir uygulama ağ geçidi tek istemcilerin iletişim noktası işlevi görür. Gelen uygulama trafiğini Azure Vm'leri, sanal makine ölçek kümeleri, uygulama hizmetleri veya şirket içi/dış sunucuları gibi birden fazla arka uç havuzu arasında dağıtır. Bunu yapmak için bu makalede açıklanan çeşitli bileşenleri kullanır.

![Uygulama ağ geçidi bileşenleri](./media/application-gateway-components/application-gateway-components.png)

## <a name="frontend-ip-address"></a>Ön uç IP adresi

Bir ön uç IP adresi, application gateway ile ilişkili IP adresidir. Uygulama ağ geçidi genel IP adresi veya özel bir IP adresi ya da hem olmasını ya da yapılandırabilirsiniz. Yalnızca bir genel IP adresi veya bir özel IP adresi desteklenir. Sanal ağ ve genel IP adresi, application gateway'iniz ile aynı konumda olmalıdır.

Bir ön uç IP adresi için ilişkili bir *dinleyici* oluşturulduktan sonra. 

### <a name="static-vs-dynamic-public-ip-address"></a>Statik ve dinamik genel IP adresi

V1 SKU statik iç IP adreslerini destekler, ancak statik genel IP adreslerini desteklemez. Uygulama ağ geçidi durduruldu ve başlatıldığında VIP değiştirebilirsiniz. Uygulama ağ geçidiyle ilişkili DNS adı, ağ geçidi yaşam döngüsü değiştirmez. Bu nedenle, bir CNAME diğer adlarını kullanma ve uygulama ağ geçidinin DNS adresine işaret önerilir.

Application Gateway v2 SKU statik genel IP adresleri gibi statik iç IP adreslerini destekler. 

## <a name="listeners"></a>Dinleyiciler

Application gateway'iniz kullanmaya başlamadan önce bir veya daha fazla dinleyicileri eklemeniz gerekir. Dinleyici için gelen bağlantı istekleri denetler ve protokol, bağlantı noktası, konak ve IP adresi isteği eşleşme protokol, bağlantı noktası, konak ve dinleyici yapılandırması ile ilişkili IP adresi ile ilişkilendirilen isteklerini kabul mantıksal bir varlıktır. Bir uygulama ağ geçidine eklenmiş birden çok dinleyici olabilir ve aynı protokolü için kullanılabilir. Dinleyici istemcilerden gelen istekleri algıladıktan sonra uygulama ağ geçidi bu istekleri Gelen istek aldığınız dinleyici için tanımladığınız istek yönlendirme kurallarını kullanarak arka uç havuzu üyelerine yönlendirir.

Dinleyiciler, aşağıdaki bağlantı noktaları ve protokolleri destekler:

### <a name="ports"></a>Bağlantı Noktaları

Bu istemci isteği için dinleyici dinlediği bağlantı noktasıdır. 1-65502 için V1 SKU ve 1 için 65199 V2 SKU için arasında bağlantı noktaları yapılandırabilirsiniz.

### <a name="protocols"></a>Protokoller

Uygulama ağ geçidi, aşağıdaki dört protokollerini destekler: HTTP, HTTPS, HTTP/2 ve WebSocket

HTTP ve HTTPS protokolleri dinleyici yapılandırma arasında seçim yapma açıkça belirtin. [WebSockets ve HTTP/2 protokoller için desteği](https://docs.microsoft.com/azure/application-gateway/overview#websocket-and-http2-traffic) yerel olarak sağlanır. [Websocket desteği](https://docs.microsoft.com/azure/application-gateway/application-gateway-websocket) varsayılan olarak etkindir. WebSocket desteğini isteğe bağlı olarak etkinleştirmek veya devre dışı bırakmak için kullanıcı tarafından yapılandırılabilen bir ayar yoktur. WebSockets hem HTTP hem de HTTPS dinleyicileri ile kullanabilirsiniz. HTTP/2 protokolü desteği, yalnızca uygulama ağ geçidi dinleyicileri bağlanan istemciler için kullanılabilir. HTTP/1.1 arka uç sunucu havuzlarına iletişimdir. Varsayılan olarak, HTTP/2 desteği devre dışıdır. Etkinleştirmek seçebilirsiniz.

SSL sonlandırma için bir HTTPS dinleyicisi kullanabilirsiniz. Bir HTTPS dinleyicisi yük boşaltmalar şifreleme ve şifre çözme böylece web sunucularınız tarafından bu yükü burdened olmayan application gateway'iniz için çalışır. Ardından, uygulamalarınızın, iş mantığına odaklanırsınız ücretsizdir.

### <a name="custom-error-pages"></a>Özel hata sayfaları

Uygulama ağ geçidi, varsayılan hata sayfalarını görüntüleme yerine özel hata sayfaları oluşturmanıza olanak sağlar. Özel hata sayfası sayesinde kendi logonuzu ve sayfa düzeninizi kullanabilirsiniz. Bir isteği arka uç ulaştığında uygulama ağ geçidi özel hata sayfası görüntüleyebilirsiniz. Daha fazla bilgi için [uygulama ağ geçidiniz için özel hata sayfaları](https://docs.microsoft.com/azure/application-gateway/custom-error).

### <a name="types-of-listeners"></a>Dinleyici türü

Dinleyicileri iki tür vardır:

- **Temel**: Bu tür bir dinleyicisi, tek bir DNS eşlemesi uygulama ağ geçidinin IP adresine sahip olduğu bir tek etki alanı siteye dinler. Tek bir sitede bir uygulama ağ geçidi arkasında barındırırken bu dinleyici yapılandırmasına gerek yoktur.
- **Çok siteli**: Aynı uygulama ağ geçidi örneğinde birden fazla web uygulaması yapılandırdığınızda bu dinleyici yapılandırmasına gerek yoktur. Bu, bir uygulama ağ geçidine en fazla 100 Web sitesi ekleyerek dağıtımlarınız için daha verimli bir topoloji yapılandırmanıza olanak sağlar. Her web sitesi, kendi arka uç havuzuna yönlendirilebilir. Örneğin:  Üç alt etki alanları için — abc.contoso.com xyz.contoso.com ve uygulama ağ geçidi IP adresini işaret eden pqr.contoso.com. Türünde üç dinleyicileri oluşturma *çok siteli* ilgili bağlantı noktası için her bir dinleyici yapılandırma ve ayarlama protokol. Daha fazla bilgi için [birden çok site barındırma](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview).

Dinleyici oluşturduktan sonra nasıl arka uca yönlendirilmesini Dinleyicide alınan isteği olduğunu belirleyen bir istek yönlendirme kuralı ilişkilendirin.

Dinleyiciler, gösterilen sırada işlenir. Temel dinleyici gelen bir istekle eşleşiyorsa, bu nedenle, bunu önce işler. Çok siteli dinleyicileri, trafiğin doğru arka uca yönlendirilmesini sağlamak için temel bir dinleyici önce yapılandırılmalıdır.

## <a name="request-routing-rule"></a>Yönlendirme kuralı iste

Bu uygulama ağ geçidinin en önemli bir bileşendir ve bu kuralla ilişkili dinleyici üzerindeki trafik nasıl yönlendirileceğini belirler. Kural dinleyiciyi arka uç sunucu havuzu ve arka uç HTTP ayarları bağlar. Bir isteği dinleyici tarafından kabul edildikten sonra istek yönlendirme kuralı isteği arka uca iletilen ya da başka bir yerde yeniden yönlendirilen olup olmadığını belirler. İstek arka ucuna iletilecek belirlenirse, istek yönlendirme kuralı, iletilmesi gereken hangi arka uç sunucu havuzuna tanımlar. Ayrıca, istek yönlendirme kuralı da isteğinde üstbilgileri yazılması olup olmadığını belirler. Tek bir dinleyici için yalnızca bir kural eklenebilir.

İstek yönlendirme kuralları iki türde olabilir:

- **Temel:** İlişkili dinleyici üzerindeki tüm istekler (örneğin: blog.contoso.com/*) ilişkilendirilmiş arka uç havuzuna ilişkili HTTP ayarı kullanılarak iletilir.
- **Yol tabanlı:** Bu kural türü, istek URL'SİNDE dayalı belirli bir arka uç havuzu için ilişkili dinleyici üzerindeki istek yönlendirme sağlar. Bir istek URL'SİNDE yolu bir yol tabanlı kural içindeki yol deseni eşleşiyorsa, bu kuralda veya ek kullanarak istek yönlendirilir. Yol deseni yalnızca için sorgu parametrelerini URL'nin yol uygulanır. Bir isteğin bir dinleyici üzerindeki URL yolu, yol tabanlı kurallardan herhangi birinin eşleşmiyor sonra istek yönlendirilir *varsayılan* arka uç havuzu ve *varsayılan* HTTP ayarları. Daha fazla bilgi için [URL tabanlı yönlendirme](https://docs.microsoft.com/azure/application-gateway/url-route-overview).

### <a name="redirection-support"></a>Yeniden yönlendirme desteği

İstek yönlendirme kuralı uygulama ağ geçidinde trafiği yeniden yönlendirme sağlar. Bu, kuralları kullanarak tanımladığınız herhangi bir bağlantı noktasından ve bağlantı noktasına yeniden yönlendirebilmeniz için genel bir yeniden yönlendirme mekanizmasıdır. (Bu otomatik HTTP-HTTPS yeniden yönlendirmesi etkinleştirmek yardımcı olabilir) başka bir dinleyici veya dış bir siteye yeniden yönlendirme hedefi seçin, geçici veya kalıcı olması için yeniden yönlendirme seçin veya URI yolu ve sorgu dizesini URL'ye yeniden yönlendirilen ekleme seçin. Daha fazla bilgi için [, uygulama ağ geçidinde trafiği yeniden yönlendirme](https://docs.microsoft.com/azure/application-gateway/redirect-overview).

#### <a name="rewrite-http-headers"></a>HTTP üst bilgilerini yeniden üretme

İstek ve yanıt üst bilgilerini ekleme, kaldırma veya HTTP (S) güncelleştirme isteği yönlendirme kurallarını isteği kullanarak while ve istemci ve arka uç havuzları, uygulama ağ geçidi üzerinden arasında yanıt paketleri taşıyın. Üst bilgiler yalnızca statik değerlerle aynı zamanda diğer üst bilgiler ve önemli sunucu değişkenleri ayarlanamaz. Bu IP adresi vb. ek güvenlik önlemleri ekleme hakkında arka uç, hassas bilgileri kaldırılıyor, istemcilerin ayıklama gibi birkaç önemli kullanım örneklerini gerçekleştirmenize yardımcı olur. Daha fazla bilgi için [application gateway'iniz yeniden HTTP üstbilgilerine](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers).

## <a name="http-settings"></a>HTTP ayarları

Arka uç bağlantı noktası numarası kullanarak sunucuları (HTTP ayarları bağlı istek yönlendirme kuralında belirtilir) protokolünü ve bu bileşen içinde belirtilen diğer ayarları uygulama ağ geçidi yolları trafiği. HTTP ayarlarında kullanılan protokolü ve bağlantı noktası uygulama ağ geçidi ve arka uç sunucuları arasındaki trafik, böylece uçtan uca SSL yerine getirmeye veya şifrelenmemiş şifreli olup olmadığını belirler. Bu bileşen için de kullanılır: bir kullanıcı oturumunu aynı sunucu üzerinde kullanarak tutulması olup olmadığını [tanımlama bilgilerine dayalı oturum benzeşimi](https://docs.microsoft.com/azure/application-gateway/overview#session-affinity), arka uç havuzu üyelerine kullanarak normal kaldırılmasını tamamlamak [bağlantı boşaltma](https://docs.microsoft.com/azure/application-gateway/overview#connection-draining), arka uç sistem durumu izleme, istek zaman aşımı aralığı ayarlamak, ana bilgisayar adı ve istek yolunda geçersiz kılmak için özel araştırma ilişkilendirmek ve App service arka ucu için arka uç ayar belirtmek için tek tıklamayla kolayca sağlayın. 

## <a name="backend-pool"></a>Arka uç havuzu

Arka uç havuzu hizmet isteği arka uç sunucuları için isteği yönlendirmek için kullanılır. Arka uç havuzları ağ oluşan, sanal makine ölçek kümeleri, genel IP adresleri, iç IP adresleri, FQDN ve çok kiracılı arka uçlar, Azure App Service ister. Uygulama ağ geçidi arka uç havuzu üyeleri bir kullanılabilirlik kümesine bağlı değil. IP bağlantısı var olduğu sürece uygulama ağ geçidi sanal ağ dışında örneklerle durumda, bu nedenle, arka uç havuzu üyelerinin kümeleri, veri merkezleri arasında veya Azure'a dışında olabilir iletişim kurabilir. İç IP'ler arka uç havuzu üyesi olarak kullanmayı planladığınız sonra gerektiren [VNET eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) veya [VPN ağ geçidi](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). VNet eşlemesi desteklendiği ve için diğer sanal ağlardaki trafiğin Yük Dengeleme. ExpressRoute veya VPN tünelleri tarafından bağlıyken trafiğe izin sürece uygulama ağ geçidi aynı zamanda şirket içi sunucular için iletişim kurabilir.

Farklı türde istekler için farklı arka uç havuzları oluşturabilirsiniz. Örneğin, genel istekleri için bir arka uç havuzu ve mikro hizmetler, uygulamanızın isteklere diğer arka uç havuzu oluşturun.

## <a name="health-probes"></a>Sistem durumu araştırmaları

Varsayılan olarak Application gateway, arka uç havuzundaki tüm kaynakların izler ve herhangi bir kaynak havuzundan iyi durumda olmadığı kabul otomatik olarak kaldırır. Bu, iyi durumda olmayan örnekler izlemeye devam eder ve kullanılabilir hale gelir ve sistem durumu araştırmaları için yanıt sağlam bir arka uç havuzuna eklenir. Varsayılan sistem durumu izleme yoklaması kullanmanın yanı sıra durum yoklaması, uygulamanızın gereksinimlerine uyacak şekilde özelleştirebilirsiniz. Özel araştırmalar sistem durumu izleme daha ayrıntılı denetime sahip olmanızı sağlar. Özel araştırmalar kullanırken, araştırma aralığı, URL ve test etmek için yolu ve arka uç havuzu örnek sağlıksız olarak işaretlenmeden önce kabul etmek için kaç başarısız yanıtları yapılandırabilirsiniz. Her arka uç havuzu durumunu izlemek için özel araştırmalar yapılandırmanız önerilir. Daha fazla bilgi için [uygulama ağ geçidinizin durumunu izlemek](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview).

## <a name="next-steps"></a>Sonraki adımlar

Application Gateway bileşenleri hakkında daha fazla edindikten sonra şunları yapabilirsiniz:
* [Azure portalını kullanarak bir uygulama ağ geçidi oluşturma](quick-create-portal.md)
* [Azure PowerShell kullanarak bir uygulama ağ geçidi oluşturma](quick-create-powershell.md)
* [Azure CLI kullanarak bir uygulama ağ geçidi oluşturma](quick-create-cli.md)
