---
title: Azure uygulama ağ geçidi bileşenleri
description: Bu makalede, çeşitli bileşenler Application Gateway hakkında bilgi sağlar.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/20/2019
ms.author: absha
ms.openlocfilehash: 14f400a1b85e213496ac98996d6c2378cf559026
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2019
ms.locfileid: "56675817"
---
# <a name="application-gateway-components"></a>Uygulama ağ geçidi bileşenleri

 Bir uygulama ağ geçidi tek istemcilerin iletişim noktası işlevi görür. Gelen uygulama trafiğini Azure Vm'leri, VMSS, uygulama hizmetleri veya şirket içi/dış sunucuları gibi birden fazla arka uç havuzu arasında dağıtır. Bunu yapmak için aşağıda açıklanan çeşitli bileşenleri kullanır:

![Uygulama ağ geçidi bileşenleri](.\media\application-gateway-components\application-gateway-components.png)

## <a name="frontend-ip"></a>Ön uç IP

Fronted IP, uygulama ağ geçidiyle ilişkili (Internet Protokolü) IP adresidir. Uygulama ağ geçidi genel IP veya bir özel IP veya her ikisini ya da sahip yapılandırabilirsiniz. Yalnızca bir genel IP adresi, bir uygulama ağ geçidinde desteklenmiyor. Sanal ağ ve genel IP adresi, Application Gateway ile aynı konumda olması gerekir.

### <a name="static-vs-dynamic-public-ip"></a>Statik ve dinamik genel IP

V1 SKU statik iç IP destekler, ancak statik genel IP'ler desteklemez. Uygulama ağ geçidi durduruldu ve başlatıldığında VIP değiştirebilirsiniz. Uygulama ağ geçidiyle ilişkili DNS adı, ağ geçidi yaşam döngüsü değiştirmez. Bu nedenle, bir CNAME diğer adlarını kullanma ve uygulama ağ geçidinin DNS adresine işaret önerilir.

Application Gateway v2 SKU iç Ips'ler statik genel IP'ler ve de destekler. Yalnızca bir genel IP adresi, bir uygulama ağ geçidinde desteklenmiyor.

Bir ön uç IP oluşturduktan sonra ilişkili olduğu *dinleyicileri* ön uç IP gelen istekler için denetleyin.

## <a name="listeners"></a>Dinleyiciler

Application Gateway'iniz kullanmaya başlamadan önce bir veya daha fazla dinleyicileri eklemeniz gerekir. Dinleyici kullanarak uygulama Ağ Geçidi Protokolü ve yapılandırdığınız bağlantı noktasını kullanarak gelen bağlantı istekleri için denetleyen bir işlemdir. Dinleyici istemcilerden gelen istekleri algıladıktan sonra uygulama ağ geçidi bu istekleri arka uç havuzundaki arka uç sunucuları için gelen istek aldığınız dinleyici için tanımladığınız istek yönlendirme kurallarını kullanarak yönlendirir.

Dinleyiciler, aşağıdaki bağlantı noktaları ve protokolleri destekler:

### <a name="ports"></a>Bağlantı Noktaları

Bu, istemci isteği dinleyicisinin dinleme yaptığı bağlantı noktasıdır. Şu aralıkta bağlantı noktası yapılandırması için izin verilir: 1-65535

### <a name="protocols"></a>Protokoller

Uygulama ağ geçidi, aşağıdaki 4 protokollerini destekler: HTTP, HTTPS ve HTTP/2, Websocket

Dinleyici yapılandırması sırasında arasında HTTP ve HTTPS protokolü seçmeniz gerekir. Uygulama ağ geçidi WebSockets ve HTTP/2 protokolleri için yerel destek sağlar. WebSockets hem HTTP hem de HTTPS dinleyicileri ile kullanabilirsiniz. 

HTTP/2 protokolü desteği, yalnızca uygulama ağ geçidi dinleyicileri bağlanan istemciler için kullanılabilir. HTTP/1.1 arka uç sunucu havuzlarına iletişimdir. Varsayılan olarak, HTTP/2 desteği devre dışıdır. Aşağıdaki Azure PowerShell kod parçacığı örneği nasıl olanak sağlayabileceğiniz gösterir:

```azurepowershell
$gw = Get-AzureRmApplicationGateway -Name test -ResourceGroupName hm

$gw.EnableHttp2 = $true

Set-AzureRmApplicationGateway -ApplicationGateway $gw
```

Websocket desteği, varsayılan olarak etkindir. WebSocket desteğini isteğe bağlı olarak etkinleştirmek veya devre dışı bırakmak için kullanıcı tarafından yapılandırılabilen bir ayar yoktur.

SSL sonlandırma, bir şifreleme ve şifre çözme için uygulama ağ geçidinizin çalışması, web sunucularının maliyetli şifreleme ve şifre çözme ek yükünden kurtulmasını ve uygulamalarınız üzerinde odaklanabilirsiniz boşaltmak için bir HTTPS dinleyicisi kullanabilirsiniz, iş mantığı. En az bir SSL sunucu sertifikası için bir HTTPS dinleyicisi dağıtmanız gerekir. Daha fazla bilgi için [SSL sonlandırma yapılandırma](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal)

### <a name="custom-error-pages"></a>Özel hata sayfaları

Application Gateway, varsayılan hata sayfalarını göstermek yerine özel hata sayfaları oluşturmanızı sağlar. Özel hata sayfası sayesinde kendi logonuzu ve sayfa düzeninizi kullanabilirsiniz. Bir isteği arka uç ulaştığında uygulama ağ geçidi özel hata sayfası görüntüleyebilirsiniz. Daha fazla bilgi için [uygulama ağ geçidiniz için özel hata sayfaları](https://docs.microsoft.com/azure/application-gateway/custom-error)

### <a name="types-of-listeners"></a>Dinleyici türü

Dinleyicileri iki tür vardır:

- **Temel**: Bu tür bir dinleyicisi, tek bir DNS eşlemesi uygulama ağ geçidinin IP adresine sahip olduğu bir tek etki alanı siteye dinler. Tek bir sitede bir uygulama ağ geçidi arkasında barındırırken bu dinleyici yapılandırma gereklidir
- **Çok siteli**: Aynı uygulama ağ geçidi örneğinde birden fazla web uygulaması yapılandırıyorsanız, bu dinleyici yapılandırması gereklidir. Bu, bir uygulama ağ geçidine en fazla 100 Web sitesi ekleyerek dağıtımlarınız için daha verimli bir topoloji yapılandırmanıza olanak sağlar. Her web sitesi, kendi arka uç havuzuna yönlendirilebilir. Örneğin:  Üç alt etki alanları için — abc.alpha.com xyz.alpha.com ve uygulama ağ geçidinin IP adresine işaret eden pqr.alpha.com. ' % S'tür 'çok siteli' 3 dinleyicileri oluşturma ve her dinleyici için ilgili bir bağlantı noktası ve protokol ayarları yapılandırın. 

Dinleyici oluşturduktan sonra nasıl arka uca yönlendirilmesini Dinleyicide alınan isteği olduğunu belirleyen bir istek yönlendirme kuralı ile ilişkilendirin.

Dinleyiciler, gösterilen sırada işlenir. Temel dinleyici gelen bir istekle eşleşiyorsa, bu nedenle ilk önce işler. Çok siteli dinleyicileri, trafiğin doğru arka uca yönlendirilmesini sağlamak için temel bir dinleyici önce yapılandırılmalıdır.

## <a name="request-routing-rule"></a>Yönlendirme kuralı iste

Bu uygulama ağ geçidinin en önemli bir bileşendir ve bu kuralla ilişkili dinleyici üzerindeki trafik nasıl yönlendirilmesini belirler. Kural dinleyiciyi arka uç sunucu havuzu ve arka uç HTTP ayarları bağlar ve için bir Dinleyicide trafik olduğunda trafiğin yönlendirileceği hangi arka uç sunucu havuzuna yönlendirileceğini belirler. Tek bir dinleyici için yalnızca bir kural eklenebilir.

İstek yönlendirme kuralları iki türde olabilir:

- **Temel:** Tüm isteklerin ilişkili dinleyici üzerindeki (örn: blog.contoso.com/*) ilişkilendirilmiş arka uç havuzuna ilişkili HTTP ayarı kullanılarak iletilir.
- **Yol tabanlı:** Bu kural türü istekleri ilgili dinleyiciyi istek URL'SİNDE dayalı belirli bir arka uç havuzuna yönlendirmek özelliği sağlar. Bir istek URL'SİNDE yolu bir yol tabanlı kural içindeki yol deseni eşleşiyorsa, bu kuralda veya ek kullanarak istek yönlendirilir. Yol deseni yalnızca için sorgu parametrelerini URL'nin yol uygulanır. 

Bir isteğin bir dinleyici üzerindeki URL yolu yol tabanlı kural eşleşmiyor sonra istek yönlendirilir *varsayılan* arka uç havuzu ve *varsayılan* HTTP ayarları.

Kurallar, bunların sırayla işlenir. Temel kural bağlantı noktası çok siteli kuralı, değerlendirilen göre trafiği BC gibi bu trafiğin olasılığını azaltmak için uygun arka uca yönlendirilir temel kuralları önce çok siteli kuralları yapılandırıldığını önerilir.

### <a name="redirection-support"></a>Yeniden yönlendirme desteği

İstek yönlendirme kuralı uygulama ağ geçidinde trafiği yeniden yönlendirme sağlar. Bu, kuralları kullanarak tanımladığınız herhangi bir bağlantı noktasından ve bağlantı noktasına yeniden yönlendirebilmeniz için genel bir yeniden yönlendirme mekanizmasıdır. Bu, HTTPS yeniden yönlendirmesi için otomatik HTTP şifrelenmiş bir yolu, kullanıcılarınızın uygulama arasındaki tüm iletişimi sağlamak etkinleştirmek yardımcı olabilir. Hedef dinleyici veya dış siteye yeniden yönlendirme istenildiği gibi belirtebilirsiniz. Yapılandırma öğesi, URI yolu ve sorgu dizesini URL'ye yeniden yönlendirilen ekleyerek etkinleştirmek için seçeneklerini de destekler. Ayrıca, yeniden yönlendirme (HTTP durum kodu 302) geçici veya kalıcı bir yeniden yönlendirme (HTTP durum kodu 301) olup olmadığını da seçebilirsiniz. Temel kural kullanırken, yeniden yönlendirme yapılandırması kaynak dinleyici ile ilişkili ve genel bir yeniden yönlendirme. Yola dayalı kural kullanıldığında, yeniden yönlendirme yapılandırması URL yolu haritada tanımlanır. Bu nedenle yalnızca bir sitenin belirli bir yol alanı için geçerlidir. Daha fazla bilgi için [, uygulama ağ geçidinde trafiği yeniden yönlendirme](https://docs.microsoft.com/azure/application-gateway/redirect-overview).

### <a name="http-headers"></a>HTTP üstbilgileri

Uygulama ağ geçidi arka ucuna iletilen istek x-iletilen-için x iletilen proto ve x iletilen bağlantı üstbilgileri ekler. X-iletilen-için üst bilgi biçimi IP: BağlantıNoktası, virgülle ayrılmış bir listesidir. Geçerli değerler x iletilen proto için http veya https ' dir. X iletilen bağlantı, uygulama ağ geçidinde istek sınırına bağlantı noktasını belirtir. Uygulama ağ geçidi ile gelen isteği özgün ana bilgisayar üst bilgisini içeren X özgün konak üst bilgisi de ekler. Bu başlığı trafiğin arka uca yönlendirilmesini önce gelen barındırma üst bilgisi nerede değişiklik Azure Web sitesi tümleştirmesi, bu gibi senaryolarda yararlı olur.

#### <a name="rewrite-http-headers"></a>HTTP üst bilgilerini yeniden üretme

Ekleyin, kaldırın veya istek ve yanıt paketleri havuzları, uygulama ağ geçidi aracılığıyla arka uç ve istemci arasında taşırken HTTP (S) istek ve yanıt üstbilgileri güncelleştirme isteği yönlendirme kurallarını kullanma. Üst bilgiler yalnızca statik değerlerle aynı zamanda diğer üst bilgiler ve önemli sunucu değişkenleri ayarlanamaz. Bu IP adresi vb. ek güvenlik önlemleri ekleme hakkında arka uç, hassas bilgileri kaldırılıyor, istemcilerin ayıklama gibi birkaç önemli kullanım örneklerini gerçekleştirmenize yardımcı olur. Daha fazla bilgi için[Application Gateway'iniz yeniden HTTP üstbilgileri](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers)

## <a name="http-settings"></a>HTTP Ayarları

Application Gateway, bağlantı noktası, protokol ve bu kaynağın belirtilen diğer ayarları kullanarak arka uç sunucularına trafiği yönlendirir. 

Arka uç havuzları, aşağıdaki bağlantı noktaları ve protokolleri destekler:

### <a name="ports"></a>Bağlantı Noktaları

Bu, arka uç sunucuları uygulama ağ geçidinden gelen trafiği dinleme bağlantı noktasıdır. Şu aralıkta bağlantı noktası yapılandırması için izin verilir: 1-65535

### <a name="protocols"></a>Protokoller

Uygulama ağ geçidi yönlendirme istekleri arka uç sunucuları için hem HTTP hem de HTTPS protokollerini destekler.

HTTP protokolü seçilirse, ardından arka uç sunucularına şifrelenmemiş akışlar trafiği. 

Arka uç sunucularıyla şifrelenmemiş iletişim kabul edilebilir bir seçenek olduğu Böyle durumlarda, HTTPS protokolü seçmeniz gerekir. HTTPS protokolünü dinleyicisi seçme ile birleştirilmiş bu ayarı etkinleştirmek sağlayacak [uçtan uca SSL](https://docs.microsoft.com/azure/application-gateway/ssl-overview). Hassas verileri arka uca şifrelenmiş güvenli bir şekilde aktarmanıza olanak sağlayacak. Arka uç sunucusunda uçtan uca SSL’nin etkin olduğu her arka uç sunucusunun, güvenli iletişime izin veren bir sertifikayla yapılandırılması gerekir.

HTTP ayarı kullanarak çeşitli özellikleri yararlanabilirsiniz:

### <a name="cookie-based-session-affinity"></a>Tanımlama bilgilerine dayalı oturum benzeşimi

Bu özellik, bir kullanıcı oturumunu aynı sunucu üzerinde tutmak istediğinizde kullanışlıdır. Ağ geçidi ile yönetilen tanımlama bilgilerini kullanan Application Gateway, sonraki trafiği işleme amacıyla bir kullanıcı oturumundan aynı sunucuya yönlendirebilir. Bu, oturum durumunun bir kullanıcı oturumuna ait sunucuya yerel olarak kaydedildiği durumlarda önemlidir.

Uygulamanın, tanımlama bilgisi temelli benzeşimi yapamıyorsa, ardından, bu özelliğin kullanılabilmesi mümkün olmayacaktır

### <a name="connection-draining"></a>Bağlantı boşaltma

Bağlantı boşaltma, planlı hizmet güncelleştirmeleri sırasında arka uç havuzu üyelerinin normal bir şekilde kapatılmasına yardımcı olur. Bu ayar, kural oluşturma sırasında tüm arka uç havuzu üyelerine uygulanabilir. Application Gateway etkinleştirildikten sonra bir arka uç havuzunun tüm kayıt kaldırma örneklerinin yeni istek almamasını ve var olan isteklerin yapılandırılan süre sınırı içinde tamamlanmasını sağlar. Bu durum hem bir API çağrısı ile açıkça arka uç havuzundan kaldırılan arka uç örnekleri hem de durum yoklamaları ile iyi durumda olmadığı bildirilen arka uç örnekleri için geçerlidir.

### <a name="override-backend-path"></a>Arka uç yolunu geçersiz kıl

Bu özellik, böylece istekleri belirli bir yol için başka bir yolu yönlendirilebilir URL yolu geçersiz kılmanıza olanak tanır. Örneğin, istekleri bu metin kutusunda varsayılan sonra girin contoso.com/images için '/' yönlendirmek istiyorsanız ve daha sonra bu HTTP ayarıyla contoso.com/images ile ilişkilendirilen kural ekleme.

### <a name="pick-host-name-from-a-backend-address"></a>Arka uç adresi konak adı seçin

Bu özellik ayarlar *konak* arka uç havuzu (IP veya FQDN) ana bilgisayar adını isteği başlığı. Bu, arka uç etki alanı adını Azure App Service arka uç olarak kullanıldığı bir senaryo gibi uygulamanın DNS adından farklı olduğu senaryolarda yararlıdır. Yalnızca özel etki alanı ayarları'nda yapılandırılmış konak adları ile Azure App Service paylaşılan bir boşluk ile tek bir IP adresi kullanarak çok kiracılı bir ortam olduğundan, bir App Service erişilebilir olmasıdır. Varsayılan olarak "example.azurewebsites.net" olduğunu ve App Service içinde kayıtlı değil bir ana bilgisayar adı veya uygulama ağ geçidinin FQDN ile uygulama ağ geçidi kullanarak uygulama hizmetinize erişmek istiyorsanız, ana bilgisayar uygulamasına özgün istekteki geçersiz kılmak sahip Hizmetin ana bilgisayar adı.

### <a name="host-override"></a>**Ana bilgisayar geçersiz kılma**

Bu özellik değiştirir *konak* burada belirttiğiniz ana bilgisayar adı için uygulama ağ geçidinde gelen istekteki üstbilgi. 

Bir HTTP ayarı oluşturduktan sonra biriyle ilişkilendirmek gereken veya daha fazla yönlendirme kuralları isteyin.

## <a name="backend-pool"></a>Arka uç havuzu

Arka uç havuzu, isteği sunan arka uç sunucuları istekleri yönlendirmek için kullanılır. Arka uç havuzları, ağ, sanal makine ölçek kümeleri, genel IP'ler birleştirilebilir, iç IP'ler, tam etki alanı adlarını (FQDN) ve çok kiracılı arka-Azure App Service gibi biter. Uygulama ağ geçidi arka uç havuzu üyeleri bir kullanılabilirlik kümesine bağlı değil. IP bağlantısı sahip oldukları sürece arka uç havuzu üyelerinin kümeleri, veri merkezleri arasında veya Azure dışında olabilir. Farklı türde istekler için farklı arka uç havuzları oluşturabilirsiniz. Örneğin, genel istekleri için bir arka uç havuzu ve mikro hizmetler, uygulamanızın isteklere diğer arka uç havuzu oluşturun.

Arka uç havuzu oluşturduktan sonra bir veya daha fazla istek yönlendirme kurallarıyla ilişkilendirmeniz gerekir. Ayrıca, her arka uç havuzu için sistem durumu araştırmaları, Application Gateway üzerinde yapılandırma gerekir. Bir istek yönlendirme kuralı koşulu karşılandığında, uygulama ağ geçidi trafiği sağlıklı sunucularına (sistem durumu araştırmalarının tarafından belirlendiği şekilde) karşılık gelen arka uç havuzundaki iletir.

## <a name="health-probes"></a>Sistem durumu araştırmaları

Varsayılan olarak Azure Application Gateway, arka uç havuzunda tüm kaynakların izler ve herhangi bir kaynak havuzundan iyi durumda olmadığı kabul otomatik olarak kaldırır. Application Gateway, iyi durumda olmayan örnekler izlemeye devam eder ve kullanılabilir hale gelir ve sistem durumu araştırmaları yanıt sonra bunları sağlıklı arka uç havuzuna ekler. Uygulama ağ geçidi arka uç HTTP Ayarları'nda tanımlanan aynı bağlantı noktası ile sistem durumu araştırmalarının gönderir.

Varsayılan sistem durumu izleme yoklaması kullanmanın yanı sıra durum yoklaması, uygulamanızın gereksinimlerine uyacak şekilde özelleştirebilirsiniz. Özel araştırmalar sistem durumu izleme daha ayrıntılı denetime sahip olmanızı sağlar. Özel araştırmalar kullanırken, araştırma aralığı, URL ve test etmek için yolu ve arka uç havuzu örnek sağlıksız olarak işaretleme önce kabul etmek için kaç başarısız yanıtları yapılandırabilirsiniz. Her arka uç havuzu durumunu izlemek için özel araştırmalar yapılandırmanız önerilir. Daha fazla bilgi için [uygulama ağ geçidinizin durumunu izleme](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview)

## <a name="next-steps"></a>Sonraki adımlar

Application Gateway bileşenleri hakkında bilgi edindikten sonra [bir uygulama ağ geçidi, Azure portalında oluşturabilirsiniz](quick-create-portal.md) veya [PowerShell kullanarak Application Gateway oluşturma](quick-create-powershell.md) veya [oluşturma bir Azure CLI kullanarak uygulama ağ geçidi](quick-create-cli.md).