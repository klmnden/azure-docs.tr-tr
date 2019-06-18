---
title: Azure uygulama ağ geçidi yapılandırmasına genel bakış
description: Bu makalede Azure Application Gateway bileşenlerini yapılandırma
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 6/1/2019
ms.author: absha
ms.openlocfilehash: c5cc39c2f2a7f2a79b8d6bc2bd95506ee5532a84
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67073974"
---
# <a name="application-gateway-configuration-overview"></a>Uygulama ağ geçidi yapılandırmasına genel bakış

Azure Application Gateway, farklı senaryolar için çeşitli şekillerde yapılandırabilirsiniz, çeşitli bileşenlerden oluşur. Bu makalede nasıl her bileşenin yapılandırılacağını gösterir.

![Uygulama ağ geçidi bileşenleri akış çizelgesi](./media/configuration-overview/configuration-overview1.png)

Bu görüntüde üç dinleyicisi olan bir uygulama gösterilmiştir. Çok siteli dinleyicileri için ilk iki olan `http://acme.com/*` ve `http://fabrikam.com/*`sırasıyla. Her ikisi de 80 numaralı bağlantı noktasında dinler. Üçüncü uçtan uca Güvenli Yuva Katmanı (SSL) sonlandırma içeren temel bir dinleyici olduğundan.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

### <a name="azure-virtual-network-and-dedicated-subnet"></a>Azure sanal ağı ve adanmış alt ağı

Bir uygulama ağ geçidi, sanal ağınızda ayrılmış bir dağıtımıdır. Sanal ağ içinde ayrılmış bir alt ağ için uygulama ağ geçidi gereklidir. Bir alt ağdan belirli bir uygulama ağ geçidi dağıtımı birden çok örneği olabilir. Alt ağdaki diğer uygulama ağ geçitleri de dağıtabilirsiniz. Ancak herhangi bir uygulama ağ geçidi alt ağının kaynakta dağıtamazsınız.

> [!NOTE]
> Aynı alt ağda Standard_v2 ve standart Azure Application Gateway karıştırılamaz.

#### <a name="size-of-the-subnet"></a>Alt ağ boyutu

Özel ön uç IP olarak yapılandırılmışsa, uygulama ağ geçidi örneği başına 1 özel IP adresi yanı sıra, başka bir özel IP adresini kullanır.

Azure Ayrıca, iç kullanım için her alt ağ içindeki 5 IP adresi ayırır: ilk 4 ve son IP adresleri. Örneğin, 15 uygulama ağ geçidi örnekleri ile özel bir ön uç IP yok göz önünde bulundurun. Bu alt ağ için en az 20 IP adresi ihtiyacınız: iç kullanım ile uygulama ağ geçidi örnekleri için 15 için 5. Bu nedenle, / 27 gerekir alt ağda boyuta ya da daha büyük.

Özel ön uç IP için 27 uygulama ağ geçidi örnekleri ve bir IP adresi olan bir alt ağ göz önünde bulundurun. Bu durumda, 33 IP adresleri gerekir: 27 özel ön uç için 1 ile 5 iç kullanım için uygulama ağ geçidi örnekleri için. Bu nedenle, bir /26 gerekir alt ağda boyuta ya da daha büyük.

Bir alt ağ boyutu en az/28'i kullanmanızı öneririz. Bu boyut, 11 kullanılabilir bir IP adresi sunar. Uygulama yükünüz 10'dan fazla IP adresi gerektiriyorsa, / 27 veya /26 göz önünde bulundurun alt ağ boyutu.

#### <a name="network-security-groups-on-the-application-gateway-subnet"></a>Uygulama ağ geçidi alt ağı üzerinde ağ güvenlik grupları

Ağ güvenlik grupları (Nsg'ler), Application Gateway üzerinde desteklenir. Ancak, bazı kısıtlamalar vardır:

- V2 SKU 65503 65534 Application Gateway v1 SKU ve bağlantı noktaları 65200 65535 gelen trafiği için özel durumlar eklemeniz gerekir. Bu bağlantı noktası aralığı, Azure altyapı iletişimi için gereklidir. Bu bağlantı noktaları (Azure sertifikaları tarafından kilitli) korunur. Dış varlıklar, bu ağ geçitlerinin müşterileri de dahil olmak üzere yerinde uygun sertifikaları olmadan bu uç noktalarında değişiklikleri başlatamazsınız.

- Giden internet bağlantısı engellenemez. Giden kuralları NSG varsayılan internet bağlantısı sağlar. Olmasını öneririz:

  - Giden varsayılan kuralları kaldırın.
  - Giden internet bağlantısı Reddet diğer giden kuralları oluşturmayın.

- Gelen trafiği **AzureLoadBalancer** etiket izin verilmelidir.

##### <a name="allow-application-gateway-access-to-a-few-source-ips"></a>Application Gateway birkaç kaynak IP'leri erişmesine izin vermek

Bu senaryo için uygulama ağ geçidi alt ağda Nsg kullanın. Bu öncelik sırasına göre bir alt ağda bulunan aşağıdaki kısıtlamalar yerleştirin:

1. IP/IP aralığını bir kaynaktan gelen trafiğe izin veren.
2. Tüm kaynaklardan gelen istekleri için 65503 65534 numaralı bağlantı noktalarına izin [arka uç sistem durumu iletişimi](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics). Bu bağlantı noktası aralığı, Azure altyapı iletişimi için gereklidir. Bu bağlantı noktaları (Azure sertifikaları tarafından kilitli) korunur. Yerinde uygun sertifikaları olmadan, dış varlıklar, bu uç noktalarında değişiklikleri başlatamazsınız.
3. Gelen Azure Load Balancer araştırmaları izin ver (*AzureLoadBalancer* etiketi) ve sanal ağ trafiği gelen (*VirtualNetwork* etiketi) üzerinde [ağ güvenlik grubu](https://docs.microsoft.com/azure/virtual-network/security-overview).
4. Tümünü Reddet kural kullanarak tüm gelen trafiği engeller.
5. Tüm hedefler için İnternet'e giden trafiğe izin verin.

#### <a name="user-defined-routes-supported-on-the-application-gateway-subnet"></a>Uygulama ağ geçidi alt ağı üzerinde desteklenen kullanıcı tanımlı yollar

Uçtan uca istek/yanıt iletişim alter yoksa sürece v1 SKU için uygulama ağ geçidi alt ağı üzerinde kullanıcı tanımlı yollar (Udr) desteklenir. Örneğin, uygulama ağ geçidi alt ağındaki UDR paket incelemesi için bir güvenlik duvarı Gereci işaret edecek şekilde ayarlayabilirsiniz. Ancak, paket İnceleme sonra hedeflenen hedefine ulaşabildiğimizden emin olmanız gerekir. Bunun yapılmaması, hatalı bir durum araştırması veya trafik yönlendirme davranışı neden olabilir. Bu öğrenilen rotalar veya Azure ExpressRoute veya VPN ağ geçitlerini sanal ağ tarafından yayılan varsayılan 0.0.0.0/0 yolları içerir.

V2 SKU için Udr, uygulama ağ geçidi alt ağı üzerinde desteklenmez. Daha fazla bilgi için [Azure Application Gateway v2 SKU](application-gateway-autoscaling-zone-redundant.md#differences-with-v1-sku).

> [!NOTE]
> Udr'ler v2 SKU için desteklenmez.  Udr'ler gerekiyorsa v1 SKU dağıtmaya devam.

> [!NOTE]
> Udr'ler kullanarak uygulama ağ geçidi alt ağı üzerinde neden olan sistem durumu [arka uç sistem durumu görünümü](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics#back-end-health) "Bilinmiyor" olarak görüntülenecek Nesil uygulama ağ geçidi günlükleri ve ölçümleri başarısız olmasına neden olur. Arka uç sistem durumu, günlükleri ve ölçümleri görüntüleyebilmek, Udr uygulama ağ geçidi alt ağda kullanmayın öneririz.

## <a name="front-end-ip"></a>Ön uç IP

Uygulama ağ geçidinin genel IP adresi, özel bir IP adresi veya her ikisini de yapılandırabilirsiniz. İstemcileri internet üzerinden bir internet'e yönelik sanal IP (VIP) üzerinden erişmesi gereken bir arka uç barındırdığınızda bir genel IP gereklidir. 

Bir genel IP, internet'e açık olmayan bir iç uç nokta için gerekli değildir. Olarak bilinen bir *iç yük dengeleyici* (ILB) uç noktası. Bir uygulama ağ geçidi ILB internet'e açık olmayan iç satır iş kolu uygulamaları için kullanışlıdır. İnternet'e açık değildir ancak hepsini bir kez deneme gerektiren çok katmanlı bir uygulama güvenlik sınırı içinde katmanda yük dağıtım, oturum sürekliliği veya SSL sonlandırma ve ayrıca hizmetleri daha kullanışlıdır.

Genel IP adresi yalnızca 1 veya 1 özel IP adresi desteklenir. Uygulama ağ geçidi oluşturduğunuzda, ön uç IP'sini seçin.

- Bir genel IP için yeni bir ortak IP adresi oluşturabilir veya application gateway ile aynı konumda var olan bir genel IP kullanın. Yeni bir genel IP, seçtiğiniz IP adresi türü (statik veya dinamik) oluşturursanız, daha sonra değiştirilemez. Daha fazla bilgi için [dinamik genel IP adresi ve statik](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#static-versus-dynamic-public-ip-address).

- Özel bir IP için uygulama ağ geçidinin oluşturulduğu alt ağdan özel bir IP adresi belirtebilirsiniz. Belirtmezseniz, rastgele bir IP adresi alt ağından otomatik olarak seçilir. Daha fazla bilgi için [bir iç yük dengeleyiciyle bir uygulama ağ geçidi oluşturma](https://docs.microsoft.com/azure/application-gateway/application-gateway-ilb-arm).

Ön uç IP adresi için ilişkili bir *dinleyici*, ön uç IP'sini gelen istekleri için denetler.

## <a name="listeners"></a>Dinleyiciler

Dinleyici bağlantı noktası, protokol, ana bilgisayar ve IP adresi kullanarak gelen bağlantı istekleri için denetleyen bir mantıksal bir varlıktır. Dinleyici yapılandırdığınızda, bu ağ geçidi gelen istekteki karşılık gelen değerlerle eşleşen değerler girmeniz gerekir.

Azure portalını kullanarak bir uygulama ağ geçidi oluşturduğunuzda, dinleyici için protokol ve bağlantı noktası'nı seçerek Ayrıca varsayılan dinleyici oluşturursunuz. Dinleyici HTTP2 desteği etkinleştirmeyi seçebilirsiniz. Uygulama ağ geçidi oluşturduktan sonra bu varsayılan dinleyici ayarlarını düzenleyebilir (*appGatewayHttpListener*/*appGatewayHttpsListener*) veya yeni dinleyici oluşturun.

### <a name="listener-type"></a>Dinleyici türü

Yeni bir dinleyici oluşturduğunuzda, arasından seçim [ *temel* ve *çok siteli*](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#types-of-listeners).

- Tek bir sitede bir uygulama ağ geçidi arkasında koyduysanız temel seçin. Bilgi [bir uygulama ağ geçidi ile temel bir dinleyici oluşturmak nasıl](https://docs.microsoft.com/azure/application-gateway/quick-create-portal).

- Aynı uygulama ağ geçidi örneğinde birden çok alt etki alanlarını aynı üst etki alanının veya birden fazla web uygulaması yapılandırıyorsanız, çoklu site dinleyicisi seçin. Bir çoklu site dinleyicisi için bir ana bilgisayar adı girmeniz gerekir. Application Gateway aynı genel IP adresi ve bağlantı noktası üzerinde birden fazla Web sitesi barındırmak için HTTP 1.1 barındırma bilgilerini kullanır olmasıdır.

#### <a name="order-of-processing-listeners"></a>Dinleyicileri işleme sırası

V1 SKU için dinleyicileri listelendikleri sırayla işlenir. Temel dinleyici gelen bir istekle eşleşiyorsa, dinleyici o isteği önce işler. Bu nedenle, trafiğin doğru arka uca yönlendirildiğinden emin olmak için temel dinleyicileri önce çok siteli dinleyicileri yapılandırın.

V2 SKU için çok siteli dinleyicileri temel dinleyicileri önce işlenir.

### <a name="front-end-ip"></a>Ön uç IP

Bu dinleyici ile ilişkilendirmek için planladığınız ön uç IP adresi seçin. Dinleyici için bu IP gelen istekleri dinler.

### <a name="front-end-port"></a>Ön uç bağlantı noktası

Ön uç bağlantı noktasını seçin. Var olan bir bağlantı noktası seçin veya yeni bir tane oluşturun. Herhangi bir değer seçin [bağlantı noktası aralığına izin](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#ports). Yalnızca bilinen bağlantı noktaları 80 ve 443, gibi ancak uygun bir izin verilen özel bir bağlantı noktası kullanabilirsiniz. Bir bağlantı noktası, genel kullanıma yönelik dinleyicileri veya özel bakan dinleyicileri için kullanılabilir.

### <a name="protocol"></a>Protocol

HTTP veya HTTPS seçin:

- HTTP seçerseniz, istemci ve uygulama ağ geçidi arasındaki trafiği şifrelenmez.

- İsterseniz, HTTPS seçin [SSL sonlandırma](https://docs.microsoft.com/azure/application-gateway/overview#secure-sockets-layer-ssltls-termination) veya [uçtan uca SSL şifrelemesini](https://docs.microsoft.com/azure/application-gateway/ssl-overview). İstemci ve uygulama ağ geçidi arasındaki trafiği şifrelenir. Ve uygulama ağ geçidinde SSL bağlantısını sonlandırır. Uçtan uca SSL şifrelemesini istiyorsanız, HTTPS seçin ve yapılandırmanız gerekir **arka uç HTTP** ayarı. Bu, arka uç uygulama ağ geçidi'ndeki dolaşımı sırasında trafiği yeniden şifrelenir sağlar.

SSL sonlandırma ve uçtan uca SSL şifrelemesini yapılandırmak için bir simetrik anahtar türetmek uygulama ağ geçidi'ni etkinleştirmek için dinleyici için bir sertifika eklemeniz gerekir. Bu, SSL protokolü belirtimi tarafından belirlenir. Simetrik anahtar şifreleme ve şifre çözme için ağ geçidi gönderilen trafiği için kullanılır. Ağ geçidi sertifikası kişisel bilgi değişimi (PFX) biçiminde olmalıdır. Bu biçim şifrelemek ve trafiği şifresini çözmek için ağ geçidini kullanan özel anahtarı dışa aktarmanızı sağlar.

#### <a name="supported-certificates"></a>Desteklenen sertifika

Bkz: [SSL sonlandırma için desteklenen sertifika](https://docs.microsoft.com/azure/application-gateway/ssl-overview#certificates-supported-for-ssl-termination).

### <a name="additional-protocol-support"></a>Ek protokol desteği

#### <a name="http2-support"></a>HTTP2 desteği

HTTP/2 protokolü desteği, yalnızca uygulama ağ geçidi dinleyicileri bağlanan istemciler için kullanılabilir. HTTP/1.1 arka uç sunucu havuzlarına iletişimdir. Varsayılan olarak, HTTP/2 desteği devre dışıdır. Aşağıdaki Azure PowerShell kod parçacığı, bunu etkinleştirmek gösterilmektedir:

```azurepowershell
$gw = Get-AzApplicationGateway -Name test -ResourceGroupName hm

$gw.EnableHttp2 = $true

Set-AzApplicationGateway -ApplicationGateway $gw
```

#### <a name="websocket-support"></a>WebSocket desteği

WebSocket desteği, varsayılan olarak etkindir. Etkinleştirmek veya devre dışı bırakmak için kullanıcı tarafından yapılandırılabilir bir ayar yoktur. WebSockets hem HTTP hem de HTTPS dinleyicileri ile kullanabilirsiniz.

### <a name="custom-error-pages"></a>Özel hata sayfaları

Özel hata genel düzeyde veya dinleyici düzeyinde tanımlayabilirsiniz. Ancak genel düzeyde özel hata sayfaları Azure portalından oluşturulması şu anda desteklenmiyor. Bir 403 web uygulaması güvenlik duvarı hatası için bir özel hata sayfası veya 502 Bakım Sayfası dinleyici düzeyinde yapılandırabilirsiniz. Belirtilen hata durum kodu Genel olarak erişilebilir blob URL'sini de belirtmeniz gerekir. Daha fazla bilgi için bkz. [Application Gateway özel hata sayfası oluşturma](https://docs.microsoft.com/azure/application-gateway/custom-error).

![Uygulama ağ geçidi hata kodları](https://docs.microsoft.com/azure/application-gateway/media/custom-error/ag-error-codes.png)

Genel özel hata sayfasını yapılandırmak için bkz: [Azure PowerShell Yapılandırması](https://docs.microsoft.com/azure/application-gateway/custom-error#azure-powershell-configuration).

### <a name="ssl-policy"></a>SSL ilkesi

SSL sertifika yönetimini merkezden gerçekleştirin ve şifre çözme şifreleme bir arka uç sunucu grubu için ek yükü azaltır. Merkezi SSL işleme, ayrıca, güvenlik gereksinimlerinize uygun merkezi SSL İlkesi belirtmenize olanak sağlar. Seçebileceğiniz *varsayılan*, *önceden tanımlanmış*, veya *özel* SSL ilkesi.

SSL İlkesi denetimi SSL protokolü sürümlerini için yapılandırdığınız. Uygulama ağ geçidi TLS1.0 TLS1.1 ve TLS1.2 Reddet yapılandırabilirsiniz. Varsayılan olarak, SSL 2.0 ve 3.0 devre dışı bırakılır ve yapılandırılabilir değildir. Daha fazla bilgi için [Application Gateway SSL ilkesine genel bakış](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-policy-overview).

Dinleyici oluşturduktan sonra bir istek yönlendirme kuralı ile ilişkilendirin. Bu kural, arka uca Dinleyicide almış isteklerinin nasıl yönlendirileceğini belirler.

## <a name="request-routing-rules"></a>İstek yönlendirme kuralları

Azure portalını kullanarak bir uygulama ağ geçidi oluşturduğunuzda, varsayılan kuralı oluşturma (*bağlanma1*). Bu kural varsayılan dinleyiciyi bağlar (*appGatewayHttpListener*) varsayılan arka uç havuzu (*appGatewayBackendPool*) ve varsayılan arka uç HTTP ayarları ( *appGatewayBackendHttpSettings*). Ağ geçidi oluşturduktan sonra varsayılan kuralın ayarlarını düzenleyin veya yeni kurallar oluşturun.

### <a name="rule-type"></a>Kural türü

Bir kuralı oluşturduğunuzda, arasından seçim [ *temel* ve *yol tabanlı*](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#request-routing-rules).

- İlişkili dinleyici üzerindeki tüm istekleri iletmek istiyorsanız, temel seçin (örneğin, *blog<i></i>.contoso.com/\*)* tek bir arka uç havuzuna.
- Yola dayalı belirli URL yolları gelen istekleri belirli arka uç havuzları yönlendirmek istiyorsanız seçin. Yol deseni yalnızca için sorgu parametrelerini URL'nin yol uygulanır.

#### <a name="order-of-processing-rules"></a>Kuralları işleme sırası

V1 SKU için gelen istekleri desen eşleştirme yolları yola dayalı kural URL yol haritasını listelenen sırayla işlenir. Bir istek yolu eşlem içindeki iki veya daha fazla yollarda desenle eşleşiyorsa, listelenen yolun ilk eşleştirilir. Ve isteği arka uca, bu yol ile ilişkili iletilir.

URL yol haritası yolu sırayla daha yüksek önceliğe v2 SKU için tam bir eşleşme var. Bir istek, iki veya daha çok yol deseninde eşleşirse, isteğin istek tam olarak eşleşen yol ile ilişkili arka uç iletilir. Gelen istek yolunda herhangi bir Yol Haritası'nda tam olarak eşleşmezse, isteği desen eşleştirme yola dayalı kural için yol haritası sipariş listesinde işlenir.

### <a name="associated-listener"></a>İlişkili dinleyicisi

Kural için bir dinleyici ilişkilendirmek için *istek yönlendirme kuralı* ilişkili dinleyici isteği yönlendirmek için arka uç havuzu belirlemek için değerlendirilir.

### <a name="associated-back-end-pool"></a>İlişkilendirilmiş arka uç havuzu

Dinleyici alan isteklere hizmet arka uç hedefleri içeren bir arka uç havuzu kuralı ilişkilendirin.

 - Temel bir kural için yalnızca bir arka uç havuzu izin verilir. İlişkili dinleyici üzerindeki tüm istekleri bu arka uç havuzuna iletilir.

 - Yola dayalı kural için her URL yolu için karşılık gelen birden fazla arka uç havuzu ekleyin. Girilen URL yolu eşleşen istekleri karşılık gelen arka uç havuzuna iletilir. Ayrıca, varsayılan bir arka uç havuzu ekleyin. Kural herhangi bir URL yolu eşleşmeyen istekleri için bu havuzu iletilir.

### <a name="associated-back-end-http-setting"></a>İlişkilendirilmiş arka uç HTTP ayarı

Her kural için bir arka uç HTTP ayarı ekleyin. İstekler, bağlantı noktası, protokol ve bu ayarda belirttiğiniz diğer bilgileri kullanarak arka uç hedefleri için uygulama ağ geçidinden yönlendirilir.

Temel bir kural için yalnızca bir arka uç HTTP ayarı izin verilir. İlişkili dinleyici üzerindeki tüm istekleri bu HTTP ayarıyla ilgili arka uç hedefe iletilir.

Her kural için bir arka uç HTTP ayarı ekleyin. İstekler, bağlantı noktası, protokol ve bu ayarda belirttiğiniz diğer bilgileri kullanarak arka uç hedefleri için uygulama ağ geçidinden yönlendirilir.

Temel bir kural için yalnızca bir arka uç HTTP ayarı izin verilir. İlişkili dinleyici üzerindeki tüm istekleri bu HTTP ayarıyla ilgili arka uç hedefe iletilir.

Yola dayalı kural için karşılık gelen birden fazla arka uç HTTP ayarları, her URL yoluna ekleyin. Bu ayar URL yolunda eşleşen istekleri, her URL yolu için karşılık gelen HTTP ayarları kullanarak karşılık gelen arka uç hedefe iletilir. Ayrıca, varsayılan bir HTTP ayarı ekleyin. Bu kural herhangi bir URL yolu eşleşmeyen istekleri, varsayılan HTTP ayarı kullanarak varsayılan arka uç havuzuna iletilir.

### <a name="redirection-setting"></a>Yeniden yönlendirme ayarı

Yeniden yönlendirme için temel bir kuralı yapılandırdıysanız, ilişkili dinleyici üzerindeki tüm istekleri hedef yönlendirilir. Bu *genel* yeniden yönlendirme. Yeniden yönlendirme için bir yol tabanlı kural yapılandırılmışsa, yalnızca belirli bir site alanında isteklerini yönlendirilir. Tarafından belirtilen bir alışveriş sepeti alanı örneğidir */cart/\** . Bu *yol tabanlı* yeniden yönlendirme.

Yeniden yönlendirme hakkında daha fazla bilgi için bkz: [Application Gateway yeniden yönlendirmeye genel bakış](https://docs.microsoft.com/azure/application-gateway/redirect-overview).

#### <a name="redirection-type"></a>Yönlendirme türü

Gerekli bir yeniden yönlendirme türünü seçin: *PERMANENT(301)* , *Temporary(307)* , *Found(302)* , veya *other(303) bkz*.

#### <a name="redirection-target"></a>Yeniden yönlendirme hedefi

Başka bir dinleyici veya dış bir siteye yeniden yönlendirme hedefi olarak seçin.

##### <a name="listener"></a>Dinleyici

Dinleyicisi, ağ geçidi üzerinde başka bir dinleyici giden trafiği yönlendirmek için yeniden yönlendirme hedefi olarak seçin. HTTP-HTTPS yeniden yönlendirmesi sağlamak istiyorsanız bu gerekli bir ayardır. Bu, gelen HTTPS isteklerinin denetler hedef dinleyici gelen HTTP isteklerinin denetler kaynak dinleyici gelen trafiği yönlendirir. Yeniden yönlendirme hedef iletilen istek özgün istek yolu ve sorgu dizesi eklemek seçebilirsiniz.

![Uygulama ağ geçidi bileşenleri iletişim kutusu](./media/configuration-overview/configure-redirection.png)

HTTP-HTTPS yeniden yönlendirmesi hakkında daha fazla bilgi için bkz:
- [Azure portalını kullanarak HTTP-HTTPS yeniden yönlendirmesi](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-portal)
- [PowerShell kullanarak, HTTP ve HTTPS yeniden yönlendirmesi](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-powershell)
- [Azure CLI kullanarak HTTP-HTTPS yeniden yönlendirmesi](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-cli)

##### <a name="external-site"></a>Dış site

Dış bir siteye bu kuralla ilişkili dinleyici üzerindeki trafik yönlendirmek istediğinizde dış site seçin. Yeniden yönlendirme hedef iletilen istek özgün istek sorgu dizesi dahil seçebilirsiniz. Yolun, özgün isteğinde dış bir siteye iletemez.

Yeniden yönlendirme hakkında daha fazla bilgi için bkz:
- [PowerShell kullanarak dış bir siteye yeniden yönlendirme trafiği](https://docs.microsoft.com/azure/application-gateway/redirect-external-site-powershell)
- [CLI kullanarak dış bir siteye yeniden yönlendirme trafiği](https://docs.microsoft.com/azure/application-gateway/redirect-external-site-cli)

#### <a name="rewrite-the-http-header-setting"></a>HTTP üstbilgisi ayarı yeniden yazma

Bu ayar ekler, kaldırır veya istek sırasında HTTP istek ve yanıt üstbilgileri güncelleştirir ve yanıt paketi istemci ve arka uç havuzları arasında taşıyabilirsiniz. Yalnızca bu özelliği PowerShell üzerinden yapılandırabilirsiniz. Azure portalı ve CLI desteği henüz kullanılamamaktadır. Daha fazla bilgi için bkz.

 - [HTTP üstbilgileri genel bakış yeniden yazma](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers)
 - [HTTP üst bilgisi yeniden yapılandırma](https://docs.microsoft.com/azure/application-gateway/add-http-header-rewrite-rule-powershell#specify-the-http-header-rewrite-rule-configuration)

## <a name="http-settings"></a>HTTP ayarları

Uygulama ağ geçidi, burada belirttiğiniz yapılandırmayı kullanarak arka uç sunucularına trafiği yönlendirir. Bir HTTP ayarı oluşturduktan sonra bir veya daha fazla istek yönlendirme kuralı ile ilişkilendirmeniz gerekir.

### <a name="cookie-based-affinity"></a>Tanımlama bilgisi tabanlı benzeşim

Bu özellik, bir kullanıcı oturumunu aynı sunucu üzerinde tutmak istediğinizde kullanışlıdır. Uygulama ağ geçidi doğrudan sonraki trafiği bir kullanıcı oturumundan aynı sunucuya işlenmek üzere ağ geçidi ile yönetilen tanımlama bilgilerini sağlar. Oturum durumu için bir kullanıcı oturumu sunucu üzerinde yerel olarak kaydedildiğinde, bu önemlidir. Uygulamanın, tanımlama bilgisi temelli benzeşimi yapamıyorsa, bu özellik kullanamazsınız. Kullanmak için istemcileri tanımlama bilgilerini desteklediği emin olun.

### <a name="connection-draining"></a>Bağlantı boşaltma

Bağlantı boşaltma düzgün bir şekilde arka uç havuzu üyeleri planlanan hizmet güncelleştirmeleri sırasında kaldırmanıza yardımcı olur. Kural oluşturma sırasında bir arka uç havuzunun tüm üyeleri için bu ayarı uygulayabilirsiniz. Arka uç havuzuna serbest kayıt tüm örneklerini yeni istekler almadığınız sağlar. Bu arada, mevcut isteklerine yapılandırılmış bir süre sınırı içinde tamamlamak için izin verilir. Bağlantı boşaltma arka uç havuzundan bir API çağrısı tarafından açıkça kaldırılan arka uç örnekleri için geçerlidir. Olarak bildirilen bir arka uç örneklerinin aynı zamanda geçerli *sağlıksız* sistem tarafından.

### <a name="protocol"></a>Protocol

Application Gateway, HTTP ve HTTPS yönlendirme istekleri arka uç sunucuları için destekler. Arka uç sunucularına trafiği, HTTP seçerseniz şifrelenmez. Şifrelenmemiş iletişimin kabul edilebilir değilse, HTTPS seçin.

Bu ayar ile HTTPS dinleyicisinin destekler birleştirilmiş [uçtan uca SSL](https://docs.microsoft.com/azure/application-gateway/ssl-overview). Bu hassas verileri arka uca şifrelenmiş güvenli bir şekilde aktarmanıza olanak sağlar. Her arka uç sunucusunda uçtan uca SSL etkin olan arka uç havuzundaki bir sertifikayla güvenli iletişime izin verecek şekilde yapılandırılması gerekir.

### <a name="port"></a>Port

Bu ayar, burada trafiği arka uç sunucuları uygulama ağ geçidinden dinleme bağlantı noktasını belirtir. 1 ile 65535 arasında değişen bağlantı noktaları yapılandırabilirsiniz.

### <a name="request-timeout"></a>İstek zaman aşımı

Bu ayar application gateway arka uç havuzundan "bağlantı zaman aşımına uğradı" hata iletisi döndürmeden önce yanıt almayı bekleyeceği saniye sayısıdır.

### <a name="override-back-end-path"></a>Arka uç yolu geçersiz kıl

Bu ayar isteği arka uca iletildiğinde kullanmak için bir isteğe bağlı özel iletme yolu yapılandırmanızı sağlar. Özel yolunda eşleşen gelen yolunun herhangi bir bölümü **arka uç yolu geçersiz kılma** iletilen yoluna kopyalanır. Aşağıdaki tabloda, bu özelliği nasıl çalıştığını gösterir:

- Ne zaman HTTP ayarı için bir temel istek yönlendirme kuralı eklenir:

  | Özgün istek  | Arka uç yolu geçersiz kıl | Arka uca iletilen istek |
  | ----------------- | --------------------- | ---------------------------- |
  | /Home/            | /override/            | / / home/geçersiz kıl              |
  | / home/secondhome / | /override/            | / override/home/secondhome /   |

- Ne zaman HTTP ayarı bir yol tabanlı istek yönlendirme kuralı için eklenir:

  | Özgün istek           | Yol kuralı       | Arka uç yolu geçersiz kıl | Arka uca iletilen istek |
  | -------------------------- | --------------- | --------------------- | ---------------------------- |
  | /pathrule/home /            | /pathrule*      | /override/            | / / home/geçersiz kıl              |
  | / pathrule/home/secondhome / | /pathrule*      | /override/            | / override/home/secondhome /   |
  | /Home/                     | /pathrule*      | /override/            | / / home/geçersiz kıl              |
  | / home/secondhome /          | /pathrule*      | /override/            | / override/home/secondhome /   |
  | /pathrule/home /            | / pathrule/giriş * | /override/            | /override/                   |
  | / pathrule/home/secondhome / | / pathrule/giriş * | /override/            | / / secondhome/geçersiz kıl        |

### <a name="use-for-app-service"></a>App service için kullanın

Azure App Service arka ucu için iki gerekli ayarları seçen bir UI kısayol budur. Bağlayabileceğinizi **arka uç adres ana bilgisayar adından çekme**, ve yeni bir özel araştırma oluşturur. (Daha fazla bilgi için [arka uç adres çekme ana bilgisayar adından](#pick) bu makalenin ayarlama.) Yeni bir araştırma oluşturulur ve araştırma başlığı arka uç üyenin adresinden çekilir.

### <a name="use-custom-probe"></a>Özel araştırma kullan

Bu ayar ilişkilendirir bir [özel araştırma](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#custom-health-probe) HTTP ayarı. Bir HTTP ayarı ile yalnızca bir özel araştırma ilişkilendirebilirsiniz. Özel bir araştırma açıkça ilişkilendirme durumunda [varsayılan araştırma](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#default-health-probe-settings) arka uç durumunu izlemek için kullanılır. Sistem durumu izleme, arka ucunun üzerine daha fazla denetim için özel bir araştırma oluşturmanızı öneririz.

> [!NOTE]
> Özel araştırma, karşılık gelen HTTP ayarı açıkça bir dinleyici ile ilişkili olmadığı sürece arka uç havuzu durumunu izlemek değildir.

### <a id="pick"/></a>Arka uç adresi konak adı seçin

Bu özellik dinamik olarak ayarlar *konak* ana bilgisayar adı arka uç havuzunun isteği başlığı. Bir IP adresi veya FQDN kullanır.

Bu özellik, arka uç etki alanı adını uygulama ağ geçidinin DNS adından farklı olduğunda ve belirli ana bilgisayar üst bilgisini veya sunucu adı belirtme (SNI) uzantısına doğru uç noktaya çözümlemek için arka uç dayanan yardımcı olur.

Bir örnek çok kiracılı hizmet arka uçtaki bir durumdur. Tek bir IP adresi ile paylaşılan bir alanı kullanan çok kiracılı bir hizmet bir uygulama hizmetidir. Bu nedenle, bir app service özel etki alanı ayarlarında yapılandırılan ana bilgisayar adları aracılığıyla yalnızca erişilebilir.

Varsayılan olarak, özel etki alanı adıdır *example.azurewebsites.<i> </i>net*. App service açıkça kayıtlı bir ana bilgisayar adı veya uygulama ağ geçidinin FQDN aracılığıyla uygulama ağ geçidi'ni kullanarak uygulama hizmetinize erişmek için app service'nın ana bilgisayar adı için özgün istek ana bilgisayar adı geçersiz. Bunu yapmak için etkinleştirme **arka uç adresi konak adı çekme** ayarı.

Mevcut özel DNS adını app Service'e eşlenmiş bir özel etki alanı için bu ayarı etkinleştirmeniz gerekmez.

> [!NOTE]
> Ayrılmış dağıtım olduğu özelliği bu ayar, PowerApps için App Service ortamı için gerekli değildir.

### <a name="host-name-override"></a>Konak adı geçersiz kılma

Bu özellik değiştirir *konak* belirttiğiniz ana bilgisayar adı ile application Gateway gelen istekteki üstbilgi.

Örneğin, varsa *www.contoso<i></i>.com* belirtilen **ana bilgisayar adı** ayarlama, özgün istek *https:/<i></i>/appgw.eastus.cloudapp.net/path1* değiştirilir *https:/<i></i>/www.contoso.com/path1* isteği arka uç sunucusuna iletilmesi zaman.

## <a name="back-end-pool"></a>Arka uç havuzu

Arka uç üyeleri dört tür için bir arka uç havuzu işaret edebilir: belirli bir sanal makine, bir sanal makine ölçek kümesi, bir IP adresini/FQDN'yi veya bir app service. Her arka uç havuzu için aynı türde birden fazla üye işaret edebilir. Aynı arka uç havuzundaki farklı türlerin üyelerini işaret desteklenmez.

Arka uç havuzunu oluşturduktan sonra bir veya daha fazla istek yönlendirme kuralı ile ilişkilendirmeniz gerekir. Application gateway'iniz sistem durumu araştırmaları her arka uç havuzu için de yapılandırmanız gerekir. Bir istek yönlendirme kuralı koşulu karşılandığında, uygulama ağ geçidi trafiği (sistem durumu araştırmalarının tarafından belirlendiği şekilde) karşılık gelen arka uç havuzunda iyi durumda sunucularına iletir.

## <a name="health-probes"></a>Sistem durumu araştırmaları

Varsayılan olarak, arka uçtaki tüm kaynakların bir uygulama ağ geçidi izler. Ancak daha fazla denetime sistem durumu izleme almak her arka uç HTTP ayarı için özel bir araştırma oluşturma öneririz. Özel bir araştırma yapılandırmanız öğrenmek için bkz. [özel durum yoklama ayarları](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#custom-health-probe-settings).

> [!NOTE]
> Özel durum araştırması oluşturduktan sonra bir arka uç HTTP ayarıyla ilişkilendirmek gerekir. Karşılık gelen HTTP ayarı açıkça bir dinleyici ile ilişkili olmadığı sürece, özel bir araştırma arka uç havuzunun izlemez.

## <a name="next-steps"></a>Sonraki adımlar

Application Gateway bileşenleri hakkında artık bildiğinize göre şunları yapabilirsiniz:

- [Azure portalını kullanarak bir uygulama ağ geçidi oluşturma](quick-create-portal.md)
- [PowerShell kullanarak bir uygulama ağ geçidi oluşturma](quick-create-powershell.md)
- [Azure CLI kullanarak bir uygulama ağ geçidi oluşturma](quick-create-cli.md)
