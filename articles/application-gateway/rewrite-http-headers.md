---
title: Azure Application Gateway ile HTTP üst bilgilerini yeniden | Microsoft Docs
description: Bu makalede Azure Application Gateway, HTTP üst bilgilerini yeniden yazma genel bir bakış sağlar.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 04/29/2019
ms.author: absha
ms.openlocfilehash: 9160d300270bf1ab5043bee632d27bcc4b7bf332
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66476042"
---
# <a name="rewrite-http-headers-with-application-gateway"></a>Uygulama ağ geçidi ile yeniden yazma HTTP üstbilgileri

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

HTTP üstbilgileri istemci ve sunucu bir istek veya yanıt ek bilgilerle geçmesine izin verin. Bu üst bilgilerini yazarak, güvenlikle ilgili üstbilgi alan HSTS gibi / X XSS koruma ekleme, hassas bilgileri açığa yanıt üstbilgi alanlarını kaldırma ve bağlantı noktası bilgilerini kaldırma gibi önemli görevler, gerçekleştirebilirsiniz X-iletilen-için üstbilgiler.

Application Gateway, istek ve yanıt paketleri istemci ile arka uç havuzları arasında gidip gelirken HTTP istek ve yanıt üst bilgileri eklemenize ya da mevcut bilgileri kaldırmanıza veya güncelleştirmenize olanak tanır. Ayrıca, ancak bazı koşulların yerine getirilmesi durumunda belirtilen üst bilgilerin yeniden yazılmasını sağlamak için koşullar eklemenize de olanak tanır.

Application Gateway de destekler. birkaç [sunucu değişkenleri](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers#server-variables) yardımcı olan isteklerini ve yanıtlarını hakkında ek bilgi depolar. Bu güçlü yeniden yazma kuralları oluşturmak için kolaylaştırır.

> [!NOTE]
>
> HTTP üst bilgisi yeniden yazma desteği yalnızca kullanılabilir [Standard_V2 ve WAF_v2 SKU](application-gateway-autoscaling-zone-redundant.md).

![Üst bilgileri yeniden yazma](media/rewrite-http-headers/rewrite-headers.png)

## <a name="supported-headers"></a>Desteklenen üstbilgileri

İstekleri ve yanıtları, konak, bağlantı ve yükseltme üstbilgileri hariç tüm üstbilgileri yazabilirsiniz. Uygulama ağ geçidi, özel üst bilgiler oluşturmak ve bunları isteklerin ve yanıtların üzerinden yönlendirilmesini eklemek için de kullanabilirsiniz.

## <a name="rewrite-conditions"></a>Koşullar yeniden yazma

Daha fazla koşullar veya HTTP (S) istekleri ve yanıtları içeriğini değerlendirmek ve yalnızca bir üst bilgi yeniden gerçekleştirmek için yeniden yazma Koşulları'nı kullanabilirsiniz. Application gateway, HTTP (S) isteklerin ve yanıtların içerik değerlendirmek için bu türde değişkenlere kullanır:

- İstek HTTP üstbilgileri.
- HTTP üstbilgileri yanıt.
- Uygulama Ağ Geçidi sunucu değişkenleri.

Bir koşul belirli bir değeri belirtilen değişkeni eşleşip eşleşmediğini veya belirtilen değişkeni belirli bir desenle eşleşip eşleşmediğini belirtilen değişkeni mevcut olup olmadığını değerlendirmek için kullanabilirsiniz. Kullandığınız [Perl uyumlu normal ifadeler (PCRE) kitaplığı](https://www.pcre.org/) koşullarında eşleşen normal ifade deseni ayarlamak için. Normal ifade söz dizimi hakkında bilgi edinmek için [Perl normal ifadeler ana sayfasında](https://perldoc.perl.org/perlre.html).

## <a name="rewrite-actions"></a>Yeniden yazma eylemleri

Yeniden yazma eylemleri yeniden istediğiniz istek ve yanıt üst bilgileri ve üst bilgileri için yeni değeri belirtmek için kullanın. Ya da yeni bir başlık oluşturun, varolan bir üstbilgi değerini değiştirin veya var olan bir üst bilgiyi Sil. Bu türde değerler için yeni bir üst bilgi veya varolan bir üstbilgi değerini ayarlayabilirsiniz:

- Metin.
- İstek üstbilgisi. İstek üstbilgisi belirtmek için söz dizimi kullanmanız gerekir {http_req_*headerName*}.
- Yanıt üst bilgisi. Yanıt üst bilgisi belirtmek için söz dizimi kullanmanız gerekir {http_resp_*headerName*}.
- Sunucu değişkeni. Bir sunucu değişkeni belirtmek için söz dizimi kullanmanız gerekir {var_*serverVariable*}.
- Metin, bir istek üst bilgisi, yanıt üst bilgisi ve bir sunucu değişkeni birleşimi.

## <a name="server-variables"></a>Sunucu değişkenleri

Uygulama ağ geçidi sunucusu hakkında yararlı bilgiler, istemci ile geçerli isteğin bağlantıyla bağlantıda depolamak için sunucu değişkenleri kullanır. Depolanan bilgilere örnek olarak, istemcinin IP adresini ve web tarayıcı türü içerir. Sunucu değişkenleri dinamik olarak, örneğin, yeni sayfa yüklendiğinde veya bir form gönderildiğinde değiştirin. Bu değişkenler, yeniden yazma koşulları değerlendirin ve üst bilgileri yeniden kullanabilirsiniz.

Application gateway, bu sunucu değişkenleri destekler:

| Değişken adı | Açıklama                                                  |
| -------------------------- | :----------------------------------------------------------- |
| add_x_forwarded_for_proxy  | X-iletilen-için istemci istek üstbilgi alanıyla `client_ip` değişkeni (açıklamada daha sonra bu tabloya bakın) eklenmiş şekilde IP1 biçimi, IP2, IP3 ve benzeri. X-iletilen-için alan istemci istek üst bilgisinde yoksa `add_x_forwarded_for_proxy` değişken eşittir `$client_ip` değişkeni. Bu değişken, yalnızca IP adresi bağlantı noktası bilgileri olmadan başlık içeren Application Gateway tarafından X-iletilen-için üstbilginin yeniden istediğinizde özellikle yararlıdır. |
| ciphers_supported          | İstemci tarafından desteklenen şifre listesi.          |
| ciphers_used               | Şifrelemeleri kurulan bir SSL bağlantısı için kullanılan dize. |
| client_ip                  | Uygulama ağ geçidi isteği aldığınız istemci IP adresi. Uygulama ağ geçidi ve kaynak istemcinin önce ters bir proxy varsa *client_ip* ters proxy IP adresini döndürür. |
| client_port                | İstemci bağlantı noktası.                                                  |
| client_tcp_rtt             | TCP bağlantısı istemci hakkında bilgiler. TCP_INFO olarak yuva seçeneği destekleyen sistemleri üzerinde kullanılabilir. |
| client_user                | HTTP kimlik doğrulaması kullanılırken kullanıcı adı kimlik doğrulaması için sağlanan. |
| host                       | Bu öncelik sırasına: İstek satırından ana bilgisayar adı, konak isteği üstbilgi alanından ana bilgisayar adı veya sunucu adı ile eşleşen bir istek. |
| cookie_*adı*              | *Adı* tanımlama bilgisi.                                            |
| http_method                | URL isteği yapmak için kullanılan yöntem. Örneğin, GET veya POST. |
| http_status                | Oturum durumu. Örneğin, 200, 400 veya 403.                       |
| http_version               | İsteğin protokol. Genellikle HTTP/1.0, HTTP/1.1 veya HTTP/2.0. |
| QUERY_STRING               | Aşağıdaki listedeki değişken/değer çiftleri "?" İstenen URL. |
| received_bytes             | İstek (istek satırı, başlık ve istek gövdesi dahil) uzunluğu. |
| request_query              | İstek satırda bağımsız değişkenler.                                |
| request_scheme             | İstek düzeni: http veya https.                            |
| request_uri                | (Bağımsız değişkenler ile) tam özgün istek URI'si.                   |
| sent_bytes                 | Bir istemciye gönderilen bayt sayısı.                             |
| server_port                | Bir isteği kabul sunucunun bağlantı noktası.                 |
| ssl_connection_protocol    | Yerleşik bir SSL bağlantısı protokol.        |
| ssl_enabled                | "On" ise, bağlantının SSL modunda çalışır. Aksi takdirde, boş bir dize. |

## <a name="rewrite-configuration"></a>Yeniden yapılandırma

HTTP üst bilgisi yeniden yapılandırmak için bu adımları tamamlamak gerekir.

1. HTTP üst bilgisi yeniden yazmak için gerekli olan nesneleri oluşturun:

   - **Eylem yeniden**: İstek ve yeniden istediğiniz isteği üst bilgi alanları ve üst bilgileri için yeni değeri belirtmek için kullanılır. Bir ilişkilendirme veya daha fazla bir yeniden yazma eylemi koşullarla yeniden yazma.

   - **Koşulu yeniden**: İsteğe bağlı yapılandırma. HTTP (S) istekleri ve yanıtları içeriğini yeniden yazma koşulları değerlendirin. HTTP (S) istek veya yanıtı yeniden yazma koşulu ile eşleşirse yeniden yazma eylemi meydana gelir.

     Birden fazla koşulu bir eylem ile ilişkilendirirseniz, yalnızca tüm koşulları sağlandığında eylem gerçekleşir. Diğer bir deyişle, bir mantıksal AND işlemi işlemdir.

   - **Kuralı yeniden**: Birden çok yeniden yazma eylemi içeriyor / koşul birleşimleri yeniden yazın.

   - **Kural dizisi**: İçinde yeniden yazma kuralları yürütme sırası belirlemeye yardımcı olur. Bir yeniden yazma kümesinde birden çok yeniden yazma kuralları varsa bu yapılandırma yararlıdır. Bir alt kural sırası değerine sahip bir yeniden yazma kuralı ilk çalıştırır. Yürütme sırası, iki yeniden yazma kuralları için aynı kural sırası atarsanız, belirleyici değildir.

   - **Kümesi yeniden**: İstek yönlendirme kuralı ile ilişkilendirilecek birden çok yeniden yazma kuralları içerir.

2. Yeniden yazma kümesi ekleme (*rewriteRuleSet*) yönlendirme kuralı. Yeniden yapılandırma, kaynak dinleyicisi aracılığıyla yönlendirme kuralı eklenir. Temel yönlendirme kuralı kullandığınızda, üstbilgi yeniden yapılandırma kaynağı dinleyici ile ilişkili ise genel üstbilgi yeniden yazma. Yola dayalı kural kullandığınızda, URL yolu haritada üstbilgi yeniden yapılandırma tanımlanır. Bu durumda, yalnızca bir sitenin belirli yolu alanını geçerlidir.

Birden çok HTTP üst bilgisi yeniden yazma kümeleri oluşturabilir ve birden çok dinleyici ayarlamak her yeniden uygulayın. Ancak bir yeniden yazma yalnızca belirli bir dinleyici kümesine uygulayabilirsiniz.

## <a name="common-scenarios"></a>Genel senaryolar

Üstbilgi yeniden kullanmak için bazı yaygın senaryolar aşağıda verilmiştir.

### <a name="remove-port-information-from-the-x-forwarded-for-header"></a>X-iletilen-için üst bilgisinden bağlantı noktası bilgilerini Kaldır

İstekleri arka uca iletir önce uygulama ağ geçidi tüm isteklerine X-iletilen-için üst bilgi ekler. Bu üst IP bağlantı noktaları, virgülle ayrılmış bir listesidir. Hangi arka uç sunucularının IP adreslerini içerecek şekilde üstbilgileri yalnızca gerektiği senaryolar olabilir. Üstbilgi yeniden yazma, X-iletilen-için üst bilgisinden bağlantı noktası bilgilerini kaldırmak için kullanabilirsiniz. Bunu yapmanın bir yolu add_x_forwarded_for_proxy sunucu değişkenine ayarlamaktır üst bilgi:

![Bağlantı noktası Kaldır](media/rewrite-http-headers/remove-port.png)

### <a name="modify-a-redirection-url"></a>Bir yeniden yönlendirme URL'sini değiştirme

Arka uç uygulaması, yeniden yönlendirme yanıtı gönderdiğinde, arka uç uygulama tarafından belirtilenden farklı bir URL için istemciyi yeniden yönlendirmek isteyebilirsiniz. Örneğin, bir app service, uygulama ağ geçidi barındırılan ve göreli yolu bir yeniden yönlendirme yapmak istemcinin ihtiyaç duyduğu Bunu yapmak isteyebilirsiniz. (Örneğin, bir arasında tekrar yönlendirme contoso.azurewebsites.net/path1 contoso.azurewebsites.net/path2.)

App Service, çok kiracılı bir hizmet olduğundan, doğru uç noktaya isteği yönlendirmek için istekte konak üst bilgisi kullanır. Uygulama Hizmetleri yüklü bir varsayılan etki alanı adı *. azurewebsites.net (örneğin, contoso.azurewebsites.net) uygulama ağ geçidinin etki alanı adını (örneğin, contoso.com) farklı. İstemciden özgün istek ana bilgisayar adı olarak uygulama ağ geçidinin etki alanı adı (contoso.com) olduğundan, uygulama ağ geçidi contoso.azurewebsites.net için ana bilgisayar adını değiştirir. App service, doğru uç noktaya istek yönlendirebilir, bu değişiklik yapar.

App service bir yeniden yönlendirme yanıtı gönderirken bir uygulama ağ geçidinden aldığı istek yanıtının location üst aynı ana bilgisayar adını kullanır. Bu nedenle istemci uygulama ağ geçidi (contoso.com/path2) yerine doğrudan contoso.azurewebsites.net/path2 isteği yapar. Uygulama ağ geçidi atlayarak arzu değil.

Ana bilgisayar adını uygulama ağ geçidinin etki alanı adı için konum üstbilgisi ayarlayarak bu sorunu çözebilir.

Ana bilgisayar adını değiştirmek için adımlar şunlardır:

1. Bir yeniden yazma kuralı location üst bilgisini yanıt azurewebsites.net içeriyorsa değerlendiren bir koşul oluşturun. Desen girin `(https?):\/\/.*azurewebsites\.net(.*)$`.
1. Uygulama ağ geçidinin hostname sahip olacak şekilde location üst bilgisini yeniden yazma için bir eylem gerçekleştirin. Girerek bunu `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` üstbilgi değeri.

![Location üst bilgisini değiştirin](media/rewrite-http-headers/app-service-redirection.png)

### <a name="implement-security-http-headers-to-prevent-vulnerabilities"></a>Uygulama güvenlik açıklarını önlemek için HTTP güvenlik üst bilgileri

Güvenlik açıklarına gerekli üst bilgileri uygulama yanıtta uygulayarak düzeltebilirsiniz. Bu güvenlik üst bilgileri, X XSS koruma, katı aktarım güvenliği ve içerik güvenliği ilkesi ekleyin. Tüm yanıtları için bu üst bilgilerini ayarlayacak şekilde uygulama ağ geçidi'ni kullanabilirsiniz.

![Güvenlik üst bilgisi](media/rewrite-http-headers/security-header.png)

### <a name="delete-unwanted-headers"></a>İstenmeyen üstbilgileri Sil

Bir HTTP yanıtı gelen hassas bilgilerin açığa üstbilgileri kaldırmak isteyebilirsiniz. Örneğin, arka uç sunucu adı, işletim sistemi ve kitaplık ayrıntılarını gibi bilgileri kaldırmak isteyebilirsiniz. Bu üst bilgileri kaldırmak için uygulama ağ geçidi'ni kullanabilirsiniz:

![Üst bilgisi siliniyor](media/rewrite-http-headers/remove-headers.png)

### <a name="check-for-the-presence-of-a-header"></a>Bir üst bilgisi varlığını denetle

Bir HTTP istek veya yanıt üst bir üst bilgi veya sunucu değişkeni varlığını değerlendirebilirsiniz. Bu değerlendirme, yalnızca belirli bir üst bilgisi mevcut olduğunda üstbilgi yeniden gerçekleştirmek istediğinizde yararlıdır.

![Bir üst bilgisi varlığını denetleme](media/rewrite-http-headers/check-presence.png)

## <a name="limitations"></a>Sınırlamalar

- Ardından bu üstbilgi değerini yeniden yazma, yanıt aynı ada sahip birden fazla üst bilgileri varsa, diğer üstbilgilerini yanıta bırakma neden olur. Yanıt olarak birden fazla Set-Cookie üst bilgisini olabileceği için bu genellikle ile Set-Cookie üst bilgisini oluşabilir. Bu tür senaryolardan bir app service ile bir uygulama ağ geçidi kullanıyorsanız ve tanımlama bilgilerine dayalı oturum benzeşimi, uygulama ağ geçidinde yapılandırmış olmanız gerekir. Bu örnekte yanıt 2 Set-Cookie üst bilgileri içerir: başka bir deyişle, app service tarafından kullanılan hizmet örneğiyle `Set-Cookie: ARRAffinity=ba127f1caf6ac822b2347cc18bba0364d699ca1ad44d20e0ec01ea80cda2a735;Path=/;HttpOnly;Domain=sitename.azurewebsites.net` ve başka bir uygulama ağ geçidi benzeşimi, yani `Set-Cookie: ApplicationGatewayAffinity=c1a2bd51lfd396387f96bl9cc3d2c516; Path=/`. Bu senaryoda Set-Cookie üst birini yeniden yazma, diğer bir Set-Cookie üst bilgisini yanıttan kaldırılmasında neden olabilir.

- Bağlantı, yükseltme ve konak üstbilgileri yeniden yazma şu anda desteklenmemektedir.

- Üst bilgi adları içerebilir herhangi bir alfasayısal karakter ve belirli simgeleri sınıfında tanımlandığı gibi [RFC 7230](https://tools.ietf.org/html/rfc7230#page-27). Alt çizgi şu anda desteklemiyoruz (\_) üst bilgi adları özel karakterler.

## <a name="next-steps"></a>Sonraki adımlar

HTTP üstbilgileri yeniden öğrenmek için bkz:

- [Azure portalını kullanarak HTTP üst bilgilerini yeniden yazma](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers-portal)
- [Azure PowerShell kullanarak yeniden yazma HTTP üstbilgileri](add-http-header-rewrite-rule-powershell.md)
