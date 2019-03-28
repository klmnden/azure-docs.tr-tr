---
title: Azure uygulama ağ geçidi yapılandırmasına genel bakış
description: Bu makale, Azure Application Gateway çeşitli bileşenleri yapılandırma hakkında bilgi sağlar.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 03/20/2019
ms.author: absha
ms.openlocfilehash: ca4f9bf00d70f327ff756558e25315762a9a77a8
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58519757"
---
# <a name="application-gateway-configuration-overview"></a>Uygulama ağ geçidi yapılandırmasına genel bakış

Uygulama ağ geçidi farklı senaryolar işlemi gerçekleştirmek için farklı şekilde yapılandırılabilir çeşitli bileşenlerden oluşur. Bu makalede nasıl her yapılandırılması bileşendir aracılığıyla gösterilmektedir.

![Uygulama ağ geçidi bileşenleri](./media/configuration-overview/configuration-overview1.png)

Yukarıdaki örnek resim 3 dinleyicileri içeren bir uygulama yapılandırma gösterilmektedir. Çok siteli dinleyicileri için ilk ikisidir `http://acme.com/*` ve `http://fabrikam.com/*`sırasıyla. Her ikisi de 80 numaralı bağlantı noktasında dinliyor. Üçüncü dinleyici uçtan uca SSL sonlandırma ile temel bir dinleyici olduğundan. 

## <a name="prerequisites"></a>Önkoşullar

### <a name="azure-virtual-network-and-dedicated-subnet"></a>Azure sanal ağı ve adanmış alt ağı

Uygulama ağ geçidi, sanal ağınızda ayrılmış bir dağıtımıdır. Sanal ağ içinde ayrılmış bir alt ağ, uygulama ağ geçidiniz için gereklidir. Bu alt ağda birden fazla belirli bir uygulama ağ geçidi dağıtımı olabilir. Alt ağdaki diğer uygulama ağ geçitleri de dağıtabilirsiniz, ancak herhangi bir uygulama ağ geçidi alt ağının kaynakta dağıtamazsınız.  

> [!NOTE]   
> Standard_v2 ve standart Application Gateway, aynı alt ağda karıştırma desteklenmiyor.

#### <a name="size-of-the-subnet"></a>Alt ağ boyutu

Özel ön uç IP yapılandırması yapılandırılmışsa, uygulama ağ geçidi örneği başına bir özel IP adresi yanı sıra, başka bir özel IP adresini kullanır. Ayrıca, Azure beş IP adresleri - ayırır ilk dört ve son IP adresi - iç kullanım için her alt ağ. Bir uygulama ağ geçidi 15 örnekleri ve hiçbir özel ön uç IP olarak ayarlanırsa, örneğin, ardından en az 20 IP adresi alt - iç kullanım için beş IP adresleri ve uygulama ağ geçidinin 15 örnekleri için 15 IP adresleri gerekli olacaktır. Bu nedenle, / 27 durumda alt ağ boyutu veya üzeri gereklidir. 27 örnekleri varsa ve bir IP adresi için özel ön uç IP yapılandırmasının, 33 IP adresleri gerekli - sonra uygulama ağ geçidi 27 örnekleri için 27 IP adresleri iç kullanım için özel ön uç IP ve beş IP için bir IP adresi yöneliktir. Bu nedenle, bu bir /26 durumda alt ağ boyutu veya üzeri gereklidir.

En az/28'i kullanmak için önerilen alt ağ boyutu. Bu, 11 kullanılabilir adresleri tükeniyor sağlar. Uygulama yükünüz 10'dan fazla örnekleri gerektiriyorsa, / 27 veya /26 dikkate almanız gereken alt ağ boyutu.

#### <a name="network-security-groups-supported-on-the-application-gateway-subnet"></a>Desteklenen uygulama ağ geçidi alt ağı üzerinde ağ güvenlik grupları

Ağ güvenlik grupları (Nsg'ler), uygulama ağ geçidi alt ağı aşağıdaki kısıtlamalarla aşağıdakilerde desteklenmektedir: 

- Özel durumlar için gelen trafiği 65503 65534 noktalarına v1 SKU ve bağlantı noktaları 65200-65535 Application Gateway için v2 SKU için yerleştirilmesi gereken. Bu bağlantı noktası aralığı, Azure altyapı iletişimi için gereklidir. Bunlar Azure sertifikaları tarafından korunur (kilitlenir). Uygun sertifikaları olmadan, bu ağ geçitlerinin müşterileri dahil dış varlıklar, bu uç noktalarında herhangi bir değişiklik başlatmak mümkün değildir.

- Giden internet bağlantısı engellenemez. Giden kuralları NSG varsayılan internet bağlantısı zaten izin verin. Giden varsayılan kuralları kaldırmayın ve giden internet bağlantısı Reddet diğer giden kuralları oluşturmayın öneririz.

- AzureLoadBalancer etiketini gelen trafiğe izin verilmesi gerekir.

##### <a name="whitelist-application-gateway-access-to-a-few-source-ips"></a>Beyaz liste Application Gateway erişimi birkaç kaynak IP'leri

Bu senaryo yapılabilir Nsg'leri kullanarak uygulama ağ geçidi alt ağı üzerinde. Aşağıdaki kısıtlamalar alt listelenen öncelik sırasına koymanız gerekir:

1. IP/IP aralığı kaynağından gelen trafiğe izin veren.
2. Tüm kaynaklardan gelen istekleri için 65503 65534 numaralı bağlantı noktalarına izin [arka uç sistem durumu iletişimi](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics). Bu bağlantı noktası aralığı, Azure altyapı iletişimi için gereklidir. Bunlar Azure sertifikaları tarafından korunur (kilitlenir). Uygun sertifikaları olmadan, bu ağ geçitlerinin müşterileri dahil dış varlıklar, bu uç noktalarında herhangi bir değişiklik başlatmak mümkün olmayacaktır.
3. Gelen Azure Load Balancer araştırmaları (AzureLoadBalancer etiketi) ve sanal noktalarında gelen ağ trafiğini (VirtualNetwork etiketi) izin [NSG](https://docs.microsoft.com/azure/virtual-network/security-overview).
4. Diğer tüm gelen trafiği bir reddetme kuralı tüm engelleyin.
5. Tüm hedefler için İnternet'e giden trafiğe izin verin.

#### <a name="user-defined-routes-supported-on-the-application-gateway-subnet"></a>Uygulama ağ geçidi alt ağı üzerinde desteklenen kullanıcı tanımlı yollar

Uçtan uca istek/yanıt iletişim değiştirmeyin sürece v1 SKU olması durumunda, uygulama ağ geçidi alt ağı üzerinde kullanıcı tanımlı yollar (Udr) desteklenir. Örneğin, uygulama ağ geçidi alt ağındaki UDR paket incelemesi için bir güvenlik duvarı Gereci işaret edecek şekilde ayarlayabilirsiniz ancak paket, istenen hedef posta İnceleme ulaşabildiğimizden emin olmanız gerekir. Bunun yapılmaması, yanlış sistem durumu araştırma ya da trafiği yönlendirme davranışını neden olabilir. Bu öğrenilen rotalar veya sanal ağ, ExpressRoute veya VPN ağ geçitleri tarafından yayılan varsayılan 0.0.0.0/0 yolları içerir.

Durumunda v2 SKU, uygulama ağ geçidi alt ağı üzerinde Udr'ler desteklenmez. Daha fazla bilgi için [otomatik ölçeklendirme ve bölgesel olarak yedekli (genel Önizleme) Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant#known-issues-and-limitations).

> [!NOTE]
> Sistem durumu Udr'ler kullanarak uygulama ağ geçidi alt ağı üzerinde neden olacak [arka uç sistem durumu görünümü](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics#back-end-health) olarak gösterilecek **bilinmeyen** ve ayrıca uygulama ağ geçidi günlükleri oluşturma failue neden olur ve Ölçümleri. Udr uygulama ağ geçidi alt ağı üzerinde bir arka uç sistem durumu, günlükleri ve ölçümleri görüntülemek için kullanmamanız önerilir.

## <a name="frontend-ip"></a>Ön uç IP

Uygulama ağ geçidi genel IP adresi veya özel bir IP adresi ya da hem olmasını ya da yapılandırabilirsiniz. Genel IP aracılığıyla Internet'e yönelik VIP internet üzerinden istemcileri tarafından erişilecek gerekli olan bir arka uç barındırırken gereklidir. Genel IP olarak da bilinen bir iç yük dengeleyici (ILB) uç nokta Internet'e gösterilmeyen bir iç uç nokta için gerekli değildir. Ağ geçidini bir ILB ile yapılandırma İnternet’e sunulmamış iç iş kolu uygulamaları için kullanışlıdır. Güvenlik sınırı içinde bulunan, İnternet’e sunulmamış ancak hala hepsini bir kez deneme yük dağıtımı, oturum sürekliliği veya Güvenli Yuva Katmanı (SLL) sonlandırması gerektiren çok katmanlı uygulamalar içindeki hizmetler ve katmanlar için de kullanışlıdır.

Yalnızca bir genel IP adresi veya bir özel IP adresi desteklenir. Uygulama ağ geçidi oluşturulurken, ön uç IP seçin. 

- Bir genel IP olması durumunda, yeni bir genel IP oluşturma veya Application Gateway ile aynı konumda var olan bir genel IP kullanmayı seçebilirsiniz. Seçili IP adresi türü (statik veya dinamik), yeni bir ortak IP adresi oluşturursanız, daha sonra değiştirilemez. Daha fazla bilgi için [statik ve dinamik genel IP](https://docs.microsoft.com/azure/application-gateway/application-gateway-components) 

- Özel IP olması durumunda, uygulama ağ geçidinin oluşturulduğu alt ağdan özel bir IP adresi belirtmeyi tercih edebilirsiniz. Açıkça belirtilmediği takdirde, rastgele bir IP adresi alt ağından otomatik olarak seçilir. Daha fazla bilgi için [bir iç yük dengeleyici (ILB) uç noktası ile uygulama ağ geçidi oluşturun.](https://docs.microsoft.com/azure/application-gateway/application-gateway-ilb-arm)

Bir ön uç IP ilişkili olduğu *dinleyici* ön uç IP gelen istekler için denetler.

## <a name="listeners"></a>Dinleyiciler

Dinleyici bağlantı noktası, protokol, ana bilgisayar ve IP adresi kullanarak gelen bağlantı istekleri için denetler, mantıksal bir varlığı olan. Bu bağlantı noktası değerini girmeniz gerektiğini dinleyicisi yapılandırırken, bu nedenle, protokol, ana bilgisayar ve IP ağ geçidi gelen istekteki karşılık gelen değerleri aynı olan adres. Azure portalını kullanarak bir uygulama ağ geçidi oluşturduğunuzda, dinleyici için protokol ve bağlantı noktası'nı seçerek Ayrıca varsayılan dinleyici oluşturursunuz. Ayrıca, dinleyici HTTP2 desteği etkinleştirmek isteyip istemediğinizi seçebilirsiniz. Uygulama ağ geçidi oluşturulduktan sonra bu varsayılan dinleyici ayarını düzenleyebileceğiniz (*appGatewayHttpListener*/*appGatewayHttpsListener*) ve/veya yeni dinleyici oluşturun.

### <a name="listener-type"></a>Dinleyici türü

Arasından seçim yapabilirsiniz [temel veya çoklu site dinleyicisi](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#types-of-listeners) yeni bir dinleyici oluşturulurken. 

- Tek bir sitede bir uygulama ağ geçidi arkasında barındırıyorsanız, temel dinleyici seçin. Bilgi [temel dinleyici ile bir uygulama ağ geçidi oluşturma](https://docs.microsoft.com/azure/application-gateway/quick-create-portal).

- Ardından aynı uygulama ağ geçidi örneğinde birden çok alt etki alanlarını aynı üst etki alanının veya birden fazla web uygulaması yapılandırıyorsanız, çoklu site dinleyicisi seçin. Çoklu site dinleyicisi için ayrıca bir ana bilgisayar adı girmeniz gerekir. Application Gateway aynı genel IP adresi ve bağlantı noktası üzerinde birden fazla Web sitesi barındırmak için HTTP 1.1 barındırma bilgilerini kullanır olmasıdır.

#### <a name="order-of-processing-listeners"></a>Dinleyicileri işleme sırası

V1 SKU'ları durumunda dinleyicileri bunlar gösterilen sırada işlenir. Temel dinleyici gelen bir istekle eşleşiyorsa, bu nedenle ilk önce işler. Bu nedenle, çok siteli dinleyicileri, trafiğin doğru arka uca yönlendirilmesini sağlamak için temel bir dinleyici önce yapılandırılmalıdır.

V2 SKU'ları olması durumunda, çok siteli dinleyicileri temel dinleyicileri önce işlenir.

### <a name="frontend-ip"></a>Ön uç IP

Bu dinleyici ile ilişkilendirmek istediğiniz ön uç IP seçin. Dinleyicisinin dinleme gelen isteğe göre bu IP.

### <a name="frontend-port"></a>Ön uç bağlantı noktası

Ön uç bağlantı noktasını seçin. Var olan bağlantı noktalarından seçin veya yeni bir tane oluşturun. Herhangi bir değer seçebileceğiniz [bağlantı noktası aralığına izin](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#ports). Bu, yalnızca bilinen bağlantı noktaları 80 ve 443 gibi kullandığınız ancak herhangi özel bir bağlantı noktası kullanım için uygun izin verir. Bir bağlantı noktası ya da genel kullanıma yönelik dinleyicileri veya özel karşılıklı dinleyicileri için kullanılabilir.

### <a name="protocol"></a>Protokol

HTTP ve HTTPS protokolü arasında seçmeniz gerekebilir. 

- HTTP seçerseniz, istemci ve uygulama ağ geçidi arasındaki trafik akışını şifrelenmemiş.

- İlgilendiğiniz HTTPS belirleyin [Güvenli Yuva Katmanı (SSL) sonlandırma](https://docs.microsoft.com/azure/application-gateway/overview) veya [uçtan uca SSL şifrelemesini](https://docs.microsoft.com/azure/application-gateway/ssl-overview). HTTPS seçerseniz, istemci ve uygulama ağ geçidi arasındaki trafik şifrelenir ve uygulama ağ geçidinde SSL bağlantı sonlandırılacak.  Uçtan uca SSL şifrelemesini isterseniz, ayrıca yapılandırılırken HTTPS protokolünü seçin gerekecektir *arka uç HTTP ayarı*. Bu, arka uç uygulama ağ geçidi'ndeki dolaşımı sırasında trafiği yeniden şifrelenmiş olduğunu garanti eder.

  Güvenli Yuva Katmanı (SSL) sonlandırma ve uçtan uca SSL şifrelemesini yapılandırmak için bir simetrik anahtar SSL protokolü belirtimi uyarınca türetmek Application Gateway etkinleştirmek için dinleyici eklenecek bir sertifika gereklidir. Simetrik anahtar, ardından şifrelemek ve şifresini çözmek için ağ geçidi gönderilen trafiği için kullanılır. Ağ geçidi sertifikası kişisel bilgi değişimi (PFX) biçiminde olması gerekir. Bu dosya biçimi, uygulama ağ geçidi tarafından şifreleme ve şifre çözme trafik gerçekleştirmek için gerekli olan özel anahtarı dışarı olanak tanır. 

#### <a name="supported-certificates"></a>Desteklenen sertifika

Bkz: [SSL sonlandırma için desteklenen sertifika](https://docs.microsoft.com/azure/application-gateway/ssl-overview#certificates-supported-for-ssl-termination).

### <a name="additional-protocol-support"></a>Ek protokol desteği

#### <a name="http2-support"></a>HTTP2 desteği

HTTP/2 protokolü desteği, yalnızca uygulama ağ geçidi dinleyicileri bağlanan istemciler için kullanılabilir. HTTP/1.1 arka uç sunucu havuzlarına iletişimdir. Varsayılan olarak, HTTP/2 desteği devre dışıdır. Aşağıdaki Azure PowerShell kod parçacığı örneği nasıl olanak sağlayabileceğiniz gösterir:

```azurepowershell
$gw = Get-AzureRmApplicationGateway -Name test -ResourceGroupName hm

$gw.EnableHttp2 = $true

Set-AzureRmApplicationGateway -ApplicationGateway $gw
```

#### <a name="websocket-support"></a>Websocket desteği

Websocket desteği, varsayılan olarak etkindir. WebSocket desteğini isteğe bağlı olarak etkinleştirmek veya devre dışı bırakmak için kullanıcı tarafından yapılandırılabilen bir ayar yoktur. WebSockets hem HTTP hem de HTTPS dinleyicileri ile kullanabilirsiniz. 

### <a name="custom-error-page"></a>Özel hata sayfası

Özel hata sayfaları dinleyici düzeyi yanı sıra genel düzeyde tanımlanabilir, ancak genel düzeyi özel hata sayfaları Azure portalından oluşturulması şu anda desteklenmiyor. Bir 403 WAF hatası için bir özel hata sayfası veya 502 Bakım Sayfası dinleyici düzeyinde yapılandırabilirsiniz. Ayrıca belirli hata durum kodu için bir ortak olarak erişilebilen blob URL'sini belirtmek gerekir. Daha fazla bilgi için [oluşturma özel hata sayfası](https://docs.microsoft.com/azure/application-gateway/custom-error).

![Uygulama ağ geçidi hata kodları](https://docs.microsoft.com/azure/application-gateway/media/custom-error/ag-error-codes.png)

Genel özel hata sayfası yapılandırmak için kullanın [yapılandırması için Azure PowerShell](https://docs.microsoft.com/azure/application-gateway/custom-error#azure-powershell-configuration) 

### <a name="ssl-policy"></a>SSL İlkesi

SSL sertifika yönetimini merkezden gerçekleştirin ve arka uç sunucu grubundan şifreleme ve şifre çözme yükü azaltın. Ayrıca işleme bu merkezi SSL Kurumsal güvenlik gereksinimlerinize uygun merkezi SSL İlkesi belirtmenize olanak sağlar.  Varsayılan, önceden tanımlanmış özel SSL İlkesi arasında seçim yapabilirsiniz. 

SSL protokolü sürümlerini denetlemek için SSL ilkesi yapılandırabilirsiniz. Uygulama ağ geçidi, TLS1.2 TLS1.0 ve TLS1.1 reddetmeyi yapılandırabilirsiniz. SSL 2.0 ve 3.0 zaten varsayılan olarak devre dışıdır ve yapılandırılabilir değildir. Daha fazla bilgi için [Application Gateway SSL ilkesine genel bakış](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-policy-overview).

Dinleyici oluşturduktan sonra nasıl arka uca yönlendirilmesini Dinleyicide alınan isteği olduğunu belirleyen bir istek yönlendirme kuralı ile ilişkilendirin.

## <a name="request-routing-rule"></a>Yönlendirme kuralı iste

Azure portalını kullanarak uygulama ağ geçidi oluştururken, varsayılan kuralı oluşturma (*bağlanma1*), varsayılan dinleyici bağlar (*appGatewayHttpListener*) varsayılan arka uç havuzu (*appGatewayBackendPool*) ve varsayılan arka uç HTTP ayarları (*appGatewayBackendHttpSettings*). Uygulama ağ geçidi oluşturulduktan sonra siz bu varsayılan kural ayarı düzenleyin veya yeni kurallar oluşturun.

### <a name="rule-type"></a>Kural türü

Arasından seçim yapabilirsiniz [temel veya yol tabanlı kural](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#request-routing-rule) yeni bir kural oluştururken. 

- İlişkili dinleyici üzerindeki tüm istekleri iletmek istiyorsanız (örn: blog.contoso.com/*) tek bir arka uç havuzu için temel dinleyici seçin. 
- Belirli bir URL yoluna belirli arka uç havuzları ile yönlendirmek istiyorsanız yol tabanlı dinleyiciyi istekleri seçin. Yol deseni yalnızca için sorgu parametrelerini URL'nin yol uygulanır.


#### <a name="order-of-processing-rules"></a>Kuralları işleme sırası

V1 SKU'ları olması durumunda, gelen isteğin deseniyle eşleşen yolların bir yola dayalı kural URL yol haritasını listelendikleri sırada işlenir. Bu nedenle istek URL yolu eşlem içindeki iki veya daha fazla yollarda desenle eşleşiyorsa, ardından listelenen yolun ilk eşleştirilir ve isteği arka uca bu yol ile ilişkili iletilir.

V2 SKU'ları olması durumunda, tam bir eşleşme yolları URL yolu eşlemesinde listelendiği sıra üzerinde daha yüksek öncelikli tutar. İçin bir istek isteği arka uca yönlendirilir sonra iki veya daha fazla yollarda desenle eşleşiyorsa nedeni, istekle birlikte tam olarak eşleşen söz konusu yoldaki ilişkili. Gelen istek yolunda URL Yol Haritası'nda herhangi bir yol tam olarak eşleşmezse, ardından gelen isteğin deseniyle eşleşen yolların bir yola dayalı kural URL yol haritasını listelendikleri sırada işlenir.

### <a name="associated-listener"></a>İlişkili dinleyicisi

Kural için bir dinleyici eşlemeniz gerekir böylece *yönlendirme kuralı iste* ilişkili *dinleyici* belirlemek için değerlendirilir *arka uç havuzu* hangi yönlendirilmesini isteğidir.

### <a name="associated-backend-pool"></a>İlişkilendirilmiş arka uç havuzu

Dinleyici tarafından alınan isteklere hizmet arka uç hedefleri içeren arka uç havuzu ilişkilendirin. Bu arka uç havuzuna ilişkili dinleyici üzerindeki tüm istekleri iletilir beri temel kural olması durumunda, yalnızca bir arka uç havuzu izin verilir. Yola dayalı kural olması durumunda, her URL yoluna karşılık gelen birden fazla arka uç havuzu ekleyin. Burada girilen URL yolu eşleşen istekleri karşılık gelen arka uç havuzuna iletilir. Ayrıca, bu kuralda girilen herhangi bir URL yolu eşleşmeyen istekleri iletilir olduğundan varsayılan arka uç havuzu ekleyin.

### <a name="associated-backend-http-setting"></a>İlişkilendirilmiş arka uç HTTP ayarı

Her kural için bir arka uç HTTP ayarı ekleyin. İstek bağlantı noktası, protokol ve bu ayarında belirtilen diğer ayarları'nı kullanarak arka uç hedeflerini için uygulama ağ geçidinden yönlendirilir. İlişkili dinleyici üzerindeki tüm istekleri bu HTTP ayarıyla ilgili arka uç hedefe iletilir beri temel kural olması durumunda, yalnızca bir arka uç HTTP ayarı izin verilir. Yola dayalı kural olması durumunda, her URL yoluna karşılık gelen birden fazla arka uç HTTP ayarları ekleyin. Burada girilen URL yolu eşleşen istekler, her URL yoluna karşılık gelen HTTP ayarları kullanarak karşılık gelen arka uç hedeflere iletilir. Ayrıca, bu kuralda girilen herhangi bir URL yolu eşleşmeyen isteklerin varsayılan HTTP ayarı kullanarak varsayılan arka uç havuzuna iletilir olduğundan varsayılan HTTP ayarı ekleyin.

### <a name="redirection-setting"></a>Yeniden yönlendirme ayarı

Yeniden yönlendirme için temel bir kuralı yapılandırdıysanız, ilişkili dinleyici üzerindeki tüm istekleri böylece genel yönlendirmesi'ni etkinleştirmeyi yeniden yönlendirme hedefine yönlendirilirsiniz. Yeniden yönlendirme istekleri yalnızca belirli bir site alanında bir yol tabanlı kural için yapılandırılmışsa, örneğin bir alışveriş sepeti sepet/tarafından belirtilen alan *, böylece yol tabanlı yeniden yönlendirme etkinleştirme, yeniden yönlendirme hedefine yönlendirilirsiniz. 

Yeniden yönlendirme özelliği hakkında daha fazla bilgi için bkz: [yeniden yönlendirmeye genel bakış](https://docs.microsoft.com/azure/application-gateway/redirect-overview).

- #### <a name="redirection-type"></a>Yeniden yönlendirme türü

  Öğesinden gerekli yeniden yönlendirme türünü seçin: Other(303) PERMANENT(301), Temporary(307), Found(302) veya bakın.

- #### <a name="redirection-target"></a>Yeniden yönlendirme hedefi

  Yeniden yönlendirme hedefi, başka bir dinleyici veya dış bir siteye arasında seçim yapabilirsiniz. 

  - ##### <a name="listener"></a>Dinleyici

    Yeniden yönlendirme hedefi seçme dinleyicisi, tek bir dinleyici için ağ geçidi üzerinde başka bir dinleyici yönlendirme yardımcı olur. HTTP, HTTPS yeniden yönlendirmesi, yani, gelen HTTPS isteklerinin denetimi hedef dinleyici için gelen HTTP isteklerini denetleme kaynak dinleyiciyi yeniden yönlendirme trafiği için etkinleştirmek istediğiniz bu ayar gereklidir. Yeniden yönlendirme hedef iletilen istek dahil edilecek özgün istek yolu ve sorgu dizesi de seçebilirsiniz.![Uygulama ağ geçidi bileşenleri](./media/configuration-overview/configure-redirection.png)

    HTTPS yeniden yönlendirmesi için HTTP hakkında daha fazla bilgi için bkz: [portalını kullanarak HTTP için HTTP yeniden yönlendirmesi](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-portal), [PowerShell kullanarak HTTP için HTTP yeniden yönlendirmesi](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-powershell), [CLI'yı kullanarak HTTP için HTTP yeniden yönlendirmesi](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-cli)

  - ##### <a name="external-site"></a>Dış site

    Dış siteye yeniden yönlendirilmesi böylece kuralıyla ilişkili dinleyici üzerindeki trafik yönlendirmek istediğinizde dış site seçin. Yeniden yönlendirme hedef iletilen istek dahil edilecek özgün istek sorgu dizesini seçebilirsiniz. Özgün istek için dış site yolunda iletemez.

    Dış siteye yeniden yönlendirme hakkında daha fazla bilgi için bkz. [PowerShell kullanarak dış siteye yeniden trafik yönlendirme](https://docs.microsoft.com/azure/application-gateway/redirect-external-site-powershell) ve [https://docs.microsoft.com/azure/application-gateway/redirect-external-site-cli](https://docs.microsoft.com/azure/application-gateway/redirect-external-site-cli)

#### <a name="rewrite-http-header-setting"></a>HTTP üstbilgisi ayarı yeniden yazma

Bu özellik, eklemek, kaldırmak veya güncelleştirme isteği sırasında HTTP istek ve yanıt üstbilgileri olanak tanır ve yanıt paketleri istemci ve arka uç havuzları arasında taşıyın.    Bu özellik yalnızca PowerShell üzerinden yapılandırabilirsiniz. Portal ve CLI desteği henüz mevcut değildir. Daha fazla bilgi için [yeniden HTTP üstbilgileri](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers) genel bakış ve [yapılandırma HTTP üst bilgisi yeniden yazma](https://docs.microsoft.com/azure/application-gateway/add-http-header-rewrite-rule-powershell#specify-your-http-header-rewrite-rule-configuration).

## <a name="http-settings"></a>HTTP ayarları

Uygulama ağ geçidi, bu bileşeninde belirtilen yapılandırma kullanılarak arka uç sunucularına trafiği yönlendirir. Bir HTTP ayarı oluşturduktan sonra biriyle ilişkilendirmek gereken veya daha fazla yönlendirme kuralları isteyin.

### <a name="cookie-based-affinity"></a>Tanımlama bilgisi tabanlı benzeşim

Bu özellik, bir kullanıcı oturumunu aynı sunucu üzerinde tutmak istediğinizde kullanışlıdır. Ağ geçidi ile yönetilen tanımlama bilgilerini kullanan uygulama ağ geçidi, sonraki trafiği işleme amacıyla bir kullanıcı oturumundan aynı sunucuya yönlendirebilir. Bu, oturum durumunun bir kullanıcı oturumuna ait sunucuya yerel olarak kaydedildiği durumlarda önemlidir. Uygulamanın, tanımlama bilgisi temelli benzeşimi yapamıyorsa, ardından, bu özelliğin kullanılabilmesi mümkün olmayacaktır. Tanımlama bilgilerine dayalı oturum benzeşimi kullanmak için istemcileri tanımlama bilgilerini desteklemelidir emin olmalısınız. 

### <a name="connection-draining"></a>Bağlantı boşaltma

Bağlantı boşaltma, planlı hizmet güncelleştirmeleri sırasında arka uç havuzu üyelerinin normal bir şekilde kapatılmasına yardımcı olur. Bu ayar, kural oluşturma sırasında tüm arka uç havuzu üyelerine uygulanabilir. Etkinleştirildikten sonra uygulama ağ geçidi arka uç havuzuna serbest kayıt tüm örneklerini yeni istekler yapılandırılmış bir süre sınırı içinde tamamlamak mevcut isteklerini verirken almalarını sağlar. Bu durum hem bir API çağrısı ile açıkça arka uç havuzundan kaldırılan arka uç örnekleri hem de durum yoklamaları ile iyi durumda olmadığı bildirilen arka uç örnekleri için geçerlidir.

### <a name="protocol"></a>Protokol

Uygulama ağ geçidi yönlendirme istekleri arka uç sunucuları için hem HTTP hem de HTTPS protokollerini destekler. HTTP protokolü seçilirse, ardından arka uç sunucularına şifrelenmemiş akışlar trafiği. Arka uç sunucularıyla şifrelenmemiş iletişim kabul edilebilir bir seçenek olduğu Böyle durumlarda, HTTPS protokolünü seçmeniz gerekir. Bu dinleyicisi HTTPS protokolünü seçme ile birleştirilmiş ayarı etkinleştirmenize olanak sağlar [uçtan uca SSL](https://docs.microsoft.com/azure/application-gateway/ssl-overview). Hassas verileri arka uca şifrelenmiş güvenli bir şekilde aktarmanıza olanak sağlar. Arka uç sunucusunda uçtan uca SSL’nin etkin olduğu her arka uç sunucusunun, güvenli iletişime izin veren bir sertifikayla yapılandırılması gerekir.

### <a name="port"></a>Bağlantı noktası

Bu arka uç sunucuları uygulama ağ geçidinden gelen trafiği için dinleme bağlantı noktasıdır. 1 ile 65535 arasında değişen bağlantı noktaları yapılandırabilirsiniz.

### <a name="request-timeout"></a>İstek zaman aşımına uğradı

Application gateway arka uç havuzundan "Bağlantı zaman aşımına uğradı" bir hatayı döndürmeden önce yanıt almayı bekleyeceği saniye sayısı.

### <a name="override-backend-path"></a>Arka uç yolunu geçersiz kıl

Bu ayar isteği arka uca iletildiğinde kullanmak için bir isteğe bağlı özel iletme yolu yapılandırmanıza olanak sağlar. Belirtilen özel yol için eşleşen gelen yolunun herhangi bir bölümü bu kopyalayacak **arka uç yolu geçersiz kılma** yönlendirilmiş yol alanı. Özelliği nasıl çalıştığını anlamak için aşağıdaki tabloya bakın.

- Ne zaman HTTP ayarı için bir temel istek yönlendirme kuralı eklenir:

  | Özgün istek  | Arka uç yolunu geçersiz kıl | Arka uca iletilen istek |
  | ----------------- | --------------------- | ---------------------------- |
  | /Home/            | /override/            | / / home/geçersiz kıl              |
  | / home/secondhome / | /override/            | / override/home/secondhome /   |

- Ne zaman HTTP ayarı için bir yol tabanlı bir istek yönlendirme kuralı eklenir:

  | Özgün istek           | Yol kuralı       | Arka uç yolunu geçersiz kıl | Arka uca iletilen istek |
  | -------------------------- | --------------- | --------------------- | ---------------------------- |
  | /pathrule/home /            | /pathrule*      | /override/            | / / home/geçersiz kıl              |
  | / pathrule/home/secondhome / | /pathrule*      | /override/            | / override/home/secondhome /   |
  | /Home/                     | /pathrule*      | /override/            | / / home/geçersiz kıl              |
  | / home/secondhome /          | /pathrule*      | /override/            | / override/home/secondhome /   |
  | /pathrule/home /            | / pathrule/giriş * | /override/            | /override/                   |
  | / pathrule/home/secondhome / | / pathrule/giriş * | /override/            | / / secondhome/geçersiz kıl        |

### <a name="use-for-app-service"></a>Uygulama hizmeti için kullan

Bu App service arka ucu için iki gerekli ayarları seçen bir UI kısayoldur - etkinleştirir, arka uç adresi konak adı seçin ve yeni bir özel araştırma oluşturur. Eski neden yapılır nedeni bölümünde açıklanan **arka uç adresi konak adı çekme** ayarı. Yeni bir araştırma araştırma başlığı arka uç üyenin adresinden de burada çekilen oluşturulur.

### <a name="use-custom-probe"></a>Özel araştırma kullan

Bu ayar ilişkilendirmek için kullanılan bir [özel araştırma](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#custom-health-probe) bu HTTP ayarıyla. Bir HTTP ayarı ile yalnızca bir özel araştırma ilişkilendirebilirsiniz. Siz açıkça özel bir araştırma ardından ilişkilendirme durumunda [varsayılan araştırma](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#default-health-probe-settings) arka uç durumunu izlemek için kullanılır. Sistem durumu izleme uçlarınıza ilgili daha ayrıntılı bir denetim sağlamak için özel bir araştırma oluşturma önerilir.

> [!NOTE]   
> Özel araştırma karşılık gelen HTTP ayarı açıkça bir dinleyici ile ilişkili olmadığı sürece arka uç havuzunun izleme başlamaz.

### <a name="pick-host-name-from-backend-address"></a>Arka uç adresinden konak seçme adı

Bu özellik dinamik olarak ayarlar *konak* ana bilgisayar adını bir IP adresini veya tam etki alanı adı (FQDN) kullanarak arka uç havuzu için istekteki üstbilgi. Burada arka uç etki alanı adını uygulama ağ geçidinin DNS adından farklıdır ve belirli bir ana bilgisayar üst bilgisini veya SNI uzantısını çok kiracılı hizmet olarak durumunda olduğu gibi doğru uç noktaya çözümlemek için arka uç dayanan senaryolarda yararlıdır arka uç. App service paylaşılan bir boşluk ile tek bir IP adresi kullanarak çok kiracılı bir hizmet olduğundan, bir App service özel etki alanı ayarları'nda yapılandırılmış yalnızca ana bilgisayar adları ile erişilebilir. Varsayılan olarak, özel etki alanı adıdır *example.azurewebsites.net*. App Service'te açıkça kayıtlı ya da bir ana bilgisayar adı veya uygulama ağ geçidinin FQDN ile uygulama ağ geçidi kullanarak uygulama hizmetinize erişmek istiyorsanız, bu nedenle, özgün istek için App service'nın ana bilgisayar adı, ana bilgisayar tarafından geçersiz kılmak kullandığınız etkinleştirme **arka uç adresi konak adı çekme** ayarı.

Ardından özel bir etki alanına sahip ve mevcut özel DNS adını App Service'e eşlediğiniz, bu ayarın etkinleştirilmesi gerekmez.

> [!NOTE]   
> ASE adanmış bir dağıtım olduğundan bu ayarı App Service ortamı (ASE) için gerekli değildir. 

### <a name="host-name-override"></a>Konak adı geçersiz kılma

Bu özellik değiştirir *konak* burada belirttiğiniz ana bilgisayar adı için uygulama ağ geçidinde gelen istekteki üstbilgi. Örneğin, www\.contoso.com olarak belirtilen **ana bilgisayar adı** ayarlama, özgün istek https://appgw.eastus.cloudapp.net/path1 değiştirilecek https://www.contoso.com/path1 isteği arka uç sunucusuna iletilmesi ne zaman. 

## <a name="backend-pool"></a>Arka uç havuzu

Arka uç havuzu için dört çeşit arka uç üyeleri işaret: belirli bir sanal makine, sanal makine ölçek kümesi, bir IP adresini/FQDN'yi veya bir app service. Her arka uç havuzu için aynı türde birden fazla üye işaret edebilir. Aynı arka uç havuzundaki farklı türlerin üyelerini işaret eden desteklenmiyor. 

Arka uç havuzu oluşturduktan sonra bir veya daha fazla istek yönlendirme kurallarıyla eşlemeniz gerekir. Ayrıca, her arka uç havuzu için sistem durumu araştırmaları, application gateway üzerinde yapılandırma gerekir. Bir istek yönlendirme kuralı koşulu karşılandığında, uygulama ağ geçidi trafiği (sistem durumu araştırmalarının tarafından belirlendiği şekilde) karşılık gelen arka uç havuzundaki sağlıklı sunucularına iletir.

## <a name="health-probes"></a>Sistem durumu araştırmaları

Application gateway, arka uç tüm kaynakları varsayılan olarak izler olsa bile, sistem durumu izleme üzerinde daha ayrıntılı bir denetime sahip için her arka uç HTTP ayarı için özel bir araştırma oluşturma önemle tavsiye edilir. Özel durum araştırması yapılandırma konusunda bilgi için bkz: [özel durum yoklama ayarları](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#custom-health-probe-settings).

> [!NOTE]   
> Özel durum araştırması oluşturduktan sonra bir arka uç HTTP ayarıyla ilişkilendirmek gerekir. Özel araştırma karşılık gelen HTTP ayarı açıkça bir dinleyici ile ilişkili olmadığı sürece arka uç havuzunun izleme başlamaz.

## <a name="next-steps"></a>Sonraki adımlar

Application Gateway bileşenleri hakkında daha fazla edindikten sonra şunları yapabilirsiniz:

- [Azure portalını kullanarak bir uygulama ağ geçidi oluşturma](quick-create-portal.md)
- [PowerShell kullanarak Application Gateway oluşturma](quick-create-powershell.md)
- [Azure CLI kullanarak bir uygulama ağ geçidi oluşturma](quick-create-cli.md)
