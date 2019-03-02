---
title: Azure uygulama ağ geçidi bileşenleri
description: Bu makalede, çeşitli bileşenler Application Gateway hakkında bilgi sağlar.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/20/2019
ms.author: absha
ms.openlocfilehash: a7b4dd14cd6270838fde33b0b62ec5adeceb3434
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2019
ms.locfileid: "57247027"
---
# <a name="application-gateway-components"></a>Uygulama ağ geçidi bileşenleri

 Bir uygulama ağ geçidi tek istemcilerin iletişim noktası işlevi görür. Gelen uygulama trafiğini Azure Vm'leri, sanal makine ölçek kümeleri, uygulama hizmetleri veya şirket içi/dış sunucuları gibi birden fazla arka uç havuzu arasında dağıtır. Bunu yapmak için bu makalede açıklanan çeşitli bileşenleri kullanır.

![Uygulama ağ geçidi bileşenleri](.\media\application-gateway-components\application-gateway-components.png)

## <a name="frontend-ip-address"></a>Ön uç IP adresi

Bir ön uç IP adresi, application gateway ile ilişkili IP adresidir. Uygulama ağ geçidi genel IP adresi veya özel bir IP adresi ya da hem olmasını ya da yapılandırabilirsiniz. Yalnızca bir genel IP adresi, bir uygulama ağ geçidinde desteklenmiyor. Sanal ağ ve genel IP adresi, application gateway'iniz ile aynı konumda olmalıdır.

### <a name="static-vs-dynamic-public-ip-address"></a>Statik ve dinamik genel IP adresi

V1 SKU statik iç IP adreslerini destekler, ancak statik genel IP adreslerini desteklemez. Uygulama ağ geçidi durduruldu ve başlatıldığında VIP değiştirebilirsiniz. Uygulama ağ geçidiyle ilişkili DNS adı, ağ geçidi yaşam döngüsü değiştirmez. Bu nedenle, bir CNAME diğer adlarını kullanma ve uygulama ağ geçidinin DNS adresine işaret önerilir.

Application Gateway v2 SKU statik genel IP adresleri gibi statik iç IP adreslerini destekler. Yalnızca bir genel IP adresi veya bir özel IP adresi desteklenir.

Bir ön uç IP adresi için ilişkili bir *dinleyici* oluşturulduktan sonra. Ön uç IP adresi üzerindeki gelen istekler için bir dinleyici denetler.

## <a name="listeners"></a>Dinleyiciler

Application gateway'iniz kullanmaya başlamadan önce bir veya daha fazla dinleyicileri eklemeniz gerekir. Dinleyici için gelen bağlantı istekleri denetler ve protokol, bağlantı noktası, konak ve IP adresini dinleyici yapılandırması ile eşleşirse isteklerini kabul mantıksal bir varlıktır. Bir uygulama ağ geçidine eklenmiş birden çok dinleyici olabilir ve aynı protokolü için kullanılabilir. Dinleyici istemcilerden gelen istekleri algıladıktan sonra uygulama ağ geçidi bu istekleri arka uç havuzundaki arka uç sunucuları için gelen istek aldığınız dinleyici için tanımladığınız istek yönlendirme kurallarını kullanarak yönlendirir.

Dinleyiciler, aşağıdaki bağlantı noktaları ve protokolleri destekler:

### <a name="ports"></a>Bağlantı Noktaları

Bu istemci isteği için dinleyici dinlediği bağlantı noktasıdır. 1-65502 için V1 SKU ve 1 için 65199 V2 SKU için arasında bağlantı noktaları yapılandırabilirsiniz.

### <a name="protocols"></a>Protokoller

Uygulama ağ geçidi, aşağıdaki dört protokollerini destekler: HTTP, HTTPS, HTTP/2 ve WebSocket

Dinleyici yapılandırdığınızda, HTTP ve HTTPS protokolü arasında seçim gerekir. Uygulama ağ geçidi WebSockets ve HTTP/2 protokolleri için yerel destek sağlar. WebSockets hem HTTP hem de HTTPS dinleyicileri ile kullanabilirsiniz.

HTTP/2 protokolü desteği, yalnızca uygulama ağ geçidi dinleyicileri bağlanan istemciler için kullanılabilir. HTTP/1.1 arka uç sunucu havuzlarına iletişimdir. Varsayılan olarak, HTTP/2 desteği devre dışıdır. Aşağıdaki Azure PowerShell kod parçacığı örneği nasıl olanak sağlayabileceğiniz gösterir:

```azurepowershell
$gw = Get-AzApplicationGateway -Name test -ResourceGroupName hm

$gw.EnableHttp2 = $true

Set-AzApplicationGateway -ApplicationGateway $gw
```

Websocket desteği, varsayılan olarak etkindir. WebSocket desteğini isteğe bağlı olarak etkinleştirmek veya devre dışı bırakmak için kullanıcı tarafından yapılandırılabilen bir ayar yoktur.

SSL sonlandırma için bir HTTPS dinleyicisi kullanabilirsiniz. Bir HTTPS dinleyicisi yük boşaltmalar şifreleme ve şifre çözme böylece web sunucularınız tarafından bu yükü burdened olmayan application gateway'iniz için çalışır. Ardından, uygulamalarınızın, iş mantığına odaklanırsınız ücretsizdir.

En az bir SSL sunucu sertifikası için bir HTTPS dinleyicisi dağıtmanız gerekir. Daha fazla bilgi için [SSL sonlandırma yapılandırma](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal)

### <a name="custom-error-pages"></a>Özel hata sayfaları

Uygulama ağ geçidi, varsayılan hata sayfalarını görüntüleme yerine özel hata sayfaları oluşturmanıza olanak sağlar. Özel hata sayfası sayesinde kendi logonuzu ve sayfa düzeninizi kullanabilirsiniz. Bir isteği arka uç ulaştığında uygulama ağ geçidi özel hata sayfası görüntüleyebilirsiniz. Daha fazla bilgi için [uygulama ağ geçidiniz için özel hata sayfaları](https://docs.microsoft.com/azure/application-gateway/custom-error).

### <a name="types-of-listeners"></a>Dinleyici türü

Dinleyicileri iki tür vardır:

- **Temel**: Bu tür bir dinleyicisi, tek bir DNS eşlemesi uygulama ağ geçidinin IP adresine sahip olduğu bir tek etki alanı siteye dinler. Tek bir sitede bir uygulama ağ geçidi arkasında barındırırken bu dinleyici yapılandırmasına gerek yoktur.
- **Çok siteli**: Aynı uygulama ağ geçidi örneğinde birden fazla web uygulaması yapılandırdığınızda bu dinleyici yapılandırmasına gerek yoktur. Bu, bir uygulama ağ geçidine en fazla 100 Web sitesi ekleyerek dağıtımlarınız için daha verimli bir topoloji yapılandırmanıza olanak sağlar. Her web sitesi, kendi arka uç havuzuna yönlendirilebilir. Örneğin:  Üç alt etki alanları için — abc.contoso.com xyz.contoso.com ve uygulama ağ geçidi IP adresini işaret eden pqr.contoso.com. Türünde üç dinleyicileri oluşturma *çok siteli* ilgili bağlantı noktası için her bir dinleyici yapılandırma ve ayarlama protokol.

Dinleyici oluşturduktan sonra nasıl arka uca yönlendirilmesini Dinleyicide alınan isteği olduğunu belirleyen bir istek yönlendirme kuralı ilişkilendirin.

Dinleyiciler, gösterilen sırada işlenir. Temel dinleyici gelen bir istekle eşleşiyorsa, bu nedenle, bunu önce işler. Çok siteli dinleyicileri, trafiğin doğru arka uca yönlendirilmesini sağlamak için temel bir dinleyici önce yapılandırılmalıdır.

## <a name="request-routing-rule"></a>Yönlendirme kuralı iste

Bu uygulama ağ geçidinin en önemli bir bileşendir ve bu kuralla ilişkili dinleyici üzerindeki trafik nasıl yönlendirileceğini belirler. Kural dinleyiciyi arka uç sunucu havuzu ve arka uç HTTP ayarları bağlar. Bu, hangi arka uç sunucu havuzu için bir Dinleyicide trafik olduğunda trafiğin yönlendirileceği tanımlar. Tek bir dinleyici için yalnızca bir kural eklenebilir.

İstek yönlendirme kuralları iki türde olabilir:

- **Temel:** İlişkili dinleyici üzerindeki tüm istekler (örneğin: blog.contoso.com/*) ilişkilendirilmiş arka uç havuzuna ilişkili HTTP ayarı kullanılarak iletilir.
- **Yol tabanlı:** Bu kural türü, istek URL'SİNDE dayalı belirli bir arka uç havuzu için ilişkili dinleyici üzerindeki istek yönlendirme sağlar. Bir istek URL'SİNDE yolu bir yol tabanlı kural içindeki yol deseni eşleşiyorsa, bu kuralda veya ek kullanarak istek yönlendirilir. Yol deseni yalnızca için sorgu parametrelerini URL'nin yol uygulanır.

Bir isteğin bir dinleyici üzerindeki URL yolu, yol tabanlı kurallardan herhangi birinin eşleşmiyor sonra istek yönlendirilir *varsayılan* arka uç havuzu ve *varsayılan* HTTP ayarları.

Kuralları yapılandırılmışsa sırayla işlenir. Temel kural bağlantı noktası çok siteli kuralı, değerlendirilen göre trafiği BC gibi bu trafiğin olasılığını azaltmak için uygun arka uca yönlendirilir temel kuralları önce çok siteli kuralları yapılandırıldığını önerilir.

### <a name="redirection-support"></a>Yeniden yönlendirme desteği

İstek yönlendirme kuralı uygulama ağ geçidinde trafiği yeniden yönlendirme sağlar. Bu, kuralları kullanarak tanımladığınız herhangi bir bağlantı noktasından ve bağlantı noktasına yeniden yönlendirebilmeniz için genel bir yeniden yönlendirme mekanizmasıdır. Bu, HTTPS yeniden yönlendirmesi için otomatik HTTP şifrelenmiş bir yolu, kullanıcılarınızın bir uygulama arasındaki tüm iletişimi sağlamak etkinleştirmek yardımcı olabilir. Hedef dinleyici veya dış siteye yeniden yönlendirme, istediğiniz belirtebilirsiniz. Yapılandırma öğesi, URI yolu ve sorgu dizesini URL'ye yeniden yönlendirilen ekleyerek etkinleştirmek için seçeneklerini de destekler. Ayrıca, yeniden yönlendirme (HTTP durum kodu 302) geçici veya kalıcı bir yeniden yönlendirme (HTTP durum kodu 301) olup olmadığını da seçebilirsiniz. Temel kural kullanırken, yeniden yönlendirme yapılandırması kaynak dinleyici ile ilişkili ve genel bir yeniden yönlendirme. Yola dayalı kural kullanıldığında, yeniden yönlendirme yapılandırması URL yolu haritada tanımlanır. Bu nedenle yalnızca bir sitenin belirli bir yol alanı için geçerlidir. Daha fazla bilgi için [, uygulama ağ geçidinde trafiği yeniden yönlendirme](https://docs.microsoft.com/azure/application-gateway/redirect-overview).

### <a name="http-headers"></a>HTTP üstbilgileri

Uygulama ağ geçidi arka ucuna iletilen istek x-iletilen-için x iletilen proto ve x iletilen bağlantı üstbilgileri ekler. X-iletilen-için üst bilgi biçimi IP: BağlantıNoktası, virgülle ayrılmış bir listesidir. Geçerli değerler x iletilen proto için HTTP veya HTTPS ' dir. X iletilen bağlantı, uygulama ağ geçidinde istek sınırına bağlantı noktasını belirtir. Uygulama ağ geçidi ile gelen isteği özgün ana bilgisayar üst bilgisini içeren X özgün konak üst bilgisi de ekler. Bu başlığı trafiğin arka uca yönlendirilmesini önce gelen barındırma üst bilgisi nerede değişiklik Azure Web sitesi tümleştirmesi, bu gibi senaryolarda yararlı olur.

#### <a name="rewrite-http-headers"></a>HTTP üst bilgilerini yeniden üretme

İstek ve yanıt üst bilgilerini ekleme, kaldırma veya HTTP (S) güncelleştirme isteği yönlendirme kurallarını isteği kullanarak while ve istemci ve arka uç havuzları, uygulama ağ geçidi üzerinden arasında yanıt paketleri taşıyın. Üst bilgiler yalnızca statik değerlerle aynı zamanda diğer üst bilgiler ve önemli sunucu değişkenleri ayarlanamaz. Bu IP adresi vb. ek güvenlik önlemleri ekleme hakkında arka uç, hassas bilgileri kaldırılıyor, istemcilerin ayıklama gibi birkaç önemli kullanım örneklerini gerçekleştirmenize yardımcı olur. Daha fazla bilgi için [application gateway'iniz yeniden HTTP üstbilgilerine](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers).

## <a name="http-settings"></a>HTTP ayarları

Application gateway, bağlantı noktası, protokol ve bu kaynağında belirtilen diğer ayarları kullanarak arka uç sunucularına trafiği yönlendirir.

Arka uç havuzları, aşağıdaki bağlantı noktaları ve protokolleri destekler:

### <a name="ports"></a>Bağlantı Noktaları

Bu arka uç sunucuları uygulama ağ geçidinden gelen trafiği için dinleme bağlantı noktasıdır. 1 ile 65535 arasında değişen bağlantı noktaları yapılandırabilirsiniz.

### <a name="protocols"></a>Protokoller

Uygulama ağ geçidi yönlendirme istekleri arka uç sunucuları için hem HTTP hem de HTTPS protokollerini destekler.

HTTP protokolü seçilirse, ardından arka uç sunucularına şifrelenmemiş akışlar trafiği.

Arka uç sunucularıyla şifrelenmemiş iletişim kabul edilebilir bir seçenek olduğu Böyle durumlarda, HTTPS protokolünü seçmeniz gerekir. Bu dinleyicisi HTTPS protokolünü seçme ile birleştirilmiş ayarı etkinleştirmenize olanak sağlar [uçtan uca SSL](https://docs.microsoft.com/azure/application-gateway/ssl-overview). Hassas verileri arka uca şifrelenmiş güvenli bir şekilde aktarmanıza olanak sağlar. Arka uç sunucusunda uçtan uca SSL’nin etkin olduğu her arka uç sunucusunun, güvenli iletişime izin veren bir sertifikayla yapılandırılması gerekir.

Çeşitli HTTP ayarı özelliklerini kullanabilirsiniz.

### <a name="cookie-based-session-affinity"></a>Tanımlama bilgilerine dayalı oturum benzeşimi

Bu özellik, bir kullanıcı oturumunu aynı sunucu üzerinde tutmak istediğinizde kullanışlıdır. Ağ geçidi ile yönetilen tanımlama bilgilerini kullanan uygulama ağ geçidi, sonraki trafiği işleme amacıyla bir kullanıcı oturumundan aynı sunucuya yönlendirebilir. Bu, oturum durumunun bir kullanıcı oturumuna ait sunucuya yerel olarak kaydedildiği durumlarda önemlidir.

Uygulamanın, tanımlama bilgisi temelli benzeşimi yapamıyorsa, ardından, bu özelliğin kullanılabilmesi mümkün olmayacaktır

### <a name="connection-draining"></a>Bağlantı boşaltma

Bağlantı boşaltma, planlı hizmet güncelleştirmeleri sırasında arka uç havuzu üyelerinin normal bir şekilde kapatılmasına yardımcı olur. Bu ayar, kural oluşturma sırasında tüm arka uç havuzu üyelerine uygulanabilir. Etkinleştirildikten sonra uygulama ağ geçidi arka uç havuzuna serbest kayıt tüm örneklerini yeni istekler yapılandırılmış bir süre sınırı içinde tamamlamak mevcut isteklerini verirken almalarını sağlar. Bu durum hem bir API çağrısı ile açıkça arka uç havuzundan kaldırılan arka uç örnekleri hem de durum yoklamaları ile iyi durumda olmadığı bildirilen arka uç örnekleri için geçerlidir.

### <a name="override-backend-path"></a>Arka uç yolunu geçersiz kıl

Bu özellik, böylece istekleri belirli bir yol için başka bir yolu yönlendirilebilir URL yolu geçersiz kılmanıza olanak tanır. Örneğin, istekleri bu metin kutusunda varsayılan sonra girin contoso.com/images için '/' yönlendirmek istiyorsanız ve daha sonra bu HTTP ayarıyla contoso.com/images ile ilişkilendirilen kural ekleme.

### <a name="pick-host-name-from-a-backend-address"></a>Arka uç adresi konak adı seçin

Bu özellik ayarlar *konak* ana bilgisayar adını bir IP adresini veya tam etki alanı adı - FQDN kullanarak arka uç havuzu için istekteki üstbilgi. Bu, arka uç etki alanı adını Azure App Service arka uç olarak kullanıldığı bir senaryo gibi uygulamanın DNS adından farklı olduğu senaryolarda yararlıdır. Azure App Service paylaşılan bir boşluk ile tek bir IP adresi kullanarak çok kiracılı bir ortam olduğundan, bir App Service özel etki alanı ayarları'nda yapılandırılmış yalnızca ana bilgisayar adları ile erişilebilir. Varsayılan olarak, olduğu *example.azurewebsites.net*. App Service içinde kayıtlı değil bir ana bilgisayar adı veya uygulama ağ geçidinin FQDN ile uygulama ağ geçidi kullanarak uygulama hizmetinize erişmek istiyorsanız, ana bilgisayar özgün istek App Service'nın ana bilgisayar adı için geçersiz kılmak sahip.

### <a name="host-override"></a>**Ana bilgisayar geçersiz kılma**

Bu özellik değiştirir *konak* burada belirttiğiniz ana bilgisayar adı için uygulama ağ geçidinde gelen istekteki üstbilgi.

Bir HTTP ayarı oluşturduktan sonra biriyle ilişkilendirmek gereken veya daha fazla yönlendirme kuralları isteyin.

## <a name="backend-pool"></a>Arka uç havuzu

Arka uç havuzu hizmet isteği arka uç sunucuları için isteği yönlendirmek için kullanılır. Arka uç havuzları ağ oluşan, sanal makine ölçek kümeleri, genel IP adresleri, iç IP adresleri, FQDN ve çok kiracılı arka uçlar, Azure App Service ister. Uygulama ağ geçidi arka uç havuzu üyeleri bir kullanılabilirlik kümesine bağlı değil. IP bağlantısı sahip oldukları sürece arka uç havuzu üyelerinin kümeleri, veri merkezleri arasında veya Azure dışında olabilir. Farklı türde istekler için farklı arka uç havuzları oluşturabilirsiniz. Örneğin, genel istekleri için bir arka uç havuzu ve mikro hizmetler, uygulamanızın isteklere diğer arka uç havuzu oluşturun.

Arka uç havuzu oluşturduktan sonra bir veya daha fazla istek yönlendirme kurallarıyla eşlemeniz gerekir. Ayrıca, her arka uç havuzu için sistem durumu araştırmaları, application gateway üzerinde yapılandırma gerekir. Bir istek yönlendirme kuralı koşulu karşılandığında, uygulama ağ geçidi trafiği (sistem durumu araştırmalarının tarafından belirlendiği şekilde) karşılık gelen arka uç havuzundaki sağlıklı sunucularına iletir.

## <a name="health-probes"></a>Sistem durumu araştırmaları

Varsayılan olarak Application gateway, arka uç havuzundaki tüm kaynakların izler ve herhangi bir kaynak havuzundan iyi durumda olmadığı kabul otomatik olarak kaldırır. Application gateway, iyi durumda olmayan örnekler izlemeye devam eder ve kullanılabilir hale gelir ve sistem durumu araştırmaları için yanıt sağlam bir arka uç havuzuna eklenir. Uygulama ağ geçidi arka uç HTTP Ayarları'nda tanımlanan aynı bağlantı noktası ile sistem durumu araştırmalarının gönderir.

Varsayılan sistem durumu izleme yoklaması kullanmanın yanı sıra durum yoklaması, uygulamanızın gereksinimlerine uyacak şekilde özelleştirebilirsiniz. Özel araştırmalar sistem durumu izleme daha ayrıntılı denetime sahip olmanızı sağlar. Özel araştırmalar kullanırken, araştırma aralığı, URL ve test etmek için yolu ve arka uç havuzu örnek sağlıksız olarak işaretlenmeden önce kabul etmek için kaç başarısız yanıtları yapılandırabilirsiniz. Her arka uç havuzu durumunu izlemek için özel araştırmalar yapılandırmanız önerilir. Daha fazla bilgi için [uygulama ağ geçidinizin durumunu izlemek](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview).

## <a name="next-steps"></a>Sonraki adımlar

Application Gateway bileşenleri hakkında daha fazla edindikten sonra şunları yapabilirsiniz:
* [Azure portalını kullanarak bir uygulama ağ geçidi oluşturma](quick-create-portal.md)
* [PowerShell kullanarak Application Gateway oluşturma](quick-create-powershell.md)
* [Azure CLI kullanarak bir uygulama ağ geçidi oluşturma](quick-create-cli.md).