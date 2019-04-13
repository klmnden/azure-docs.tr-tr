---
title: Azure Application Gateway, HTTP üst bilgilerini yeniden | Microsoft Docs
description: Bu makalede Azure Application Gateway, HTTP üst bilgilerini yeniden yazma özelliği genel bir bakış sağlar.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 04/11/2019
ms.author: absha
ms.openlocfilehash: efb7b46919066beb1382d70b676a2115ea0fb8ac
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59544161"
---
# <a name="rewrite-http-headers-with-application-gateway-public-preview"></a>Uygulama ağ geçidi (genel Önizleme) ile yeniden yazma HTTP üstbilgileri

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

HTTP üstbilgileri, istemci ve sunucu istek veya yanıt ek bilgilerle geçmesine izin verin. Bu HTTP üstbilgileri yeniden yazma HSTS gibi güvenlikle ilgili üstbilgi alanlarını ekleme gibi birkaç önemli senaryoları gerçekleştirmenize yardımcı olur / X-XSS-koruma yanıt üst bilgisi kaldırma, hassas bilgiler bağlantı noktası bilgilerini şeridi oluşturma gösterebilir alanları X-iletilen-için üst bilgiler, vb. Uygulama ağ geçidi eklemek, kaldırmak veya güncelleştirme isteği sırasında HTTP istek ve yanıt üstbilgileri yeteneğini destekler ve yanıt paketleri istemci ve arka uç havuzları arasında taşıyın. Ayrıca, yalnızca belirli koşullar karşılandığında belirtilen üst bilgiler yazılır emin olmak için koşullar ekleme olanağı sağlar.
> [!NOTE]
>
> HTTP üst bilgisi yeniden yazma desteği yalnızca kullanılabilir [yeni SKU [Standard_V2\]](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant)

## <a name="headers-supported-for-rewrite"></a>Desteklenen üstbilgileri yeniden yazma

Özellik, tüm üstbilgileri istek ve yanıt konak, bağlantı ve yükseltme üstbilgileri normalleştirilmesi yeniden olanak tanır. Uygulama ağ geçidi, özel üst bilgiler oluşturmak ve bunları istek ve yanıtların üzerinden yönlendirilmesini eklemek için de kullanabilirsiniz. 

## <a name="rewrite-conditions"></a>Koşullar yeniden yazma

HTTP (S) istekleri ve yanıtları içeriğini değerlendirmek ve üst bilgi gerçekleştirmek koşulları yalnızca bir veya daha fazla koşullar karşılandığında yeniden yeniden kullanıyor. 3 şu türde değişkenlere uygulama ağ geçidi tarafından HTTP (S) istekleri ve yanıtları içeriğini değerlendirmek için kullanılır:

- İstek HTTP üstbilgileri
- HTTP yanıt üstbilgileri
- Uygulama Ağ Geçidi sunucu değişkenleri

Bir koşul belirli bir değeri belirtilen değişkeni bir tam olarak eşleşip eşleşmediğini veya belirtilen değişkeni tam olarak belirli bir desenle eşleşip eşleşmediğini belirtilen değişkeni mevcut olup olmadığını değerlendirmek için kullanılabilir. [Perl uyumlu normal ifadeler (PCRE) kitaplığı](https://www.pcre.org/) koşullarında eşleşen normal ifade deseni uygulamak için kullanılır. Normal ifade söz dizimi hakkında bilgi edinmek için [Perl normal ifadeler adam sayfa](http://perldoc.perl.org/perlre.html).

## <a name="rewrite-actions"></a>Yeniden yazma eylemleri

Yeniden yazma eylemleri yeniden yazmak için istediğinize istek ve yanıt üst bilgileri ve özgün üstbilgileri için yazılması gereken yeni değeri belirtmek için kullanılır. Yeni bir başlığı oluşturmak, varolan bir üstbilgi değerini değiştirin veya var olan bir üst bilgiyi Sil. Aşağıdaki türde değerler için yeni bir üst bilgi veya varolan bir üstbilgi değerini ayarlayabilirsiniz:

- Metin 
- İstek üstbilgisi: İstek üstbilgisi belirtmek için sözdizimi kullanmanız gerekir {http_req_*headerName*}
- Yanıt üst bilgisi: Yanıt üst bilgisi belirtmek için sözdizimi kullanmanız gerekir {http_resp_*headerName*}
- Sunucu değişkeni: Bir sunucu değişkeni belirtmek için sözdizimi kullanmanız gerekir {var_*serverVariable*}
- Metin, istek üst bilgisi, yanıt üst bilgisi ve bir sunucu değişkeni birleşimi.

## <a name="server-variables"></a>Sunucu değişkenleri

Uygulama ağ geçidi sunucusu, istemcinin geçerli istek ile bağlantı hakkında yararlı bilgiler istemcinin IP adresini veya web tarayıcı türü gibi bağlantı depolamak istediğiniz sunucu değişkenleri kullanır. Yeni sayfa yüklendiğinde veya form gönderildikten gibi bu değişkenler dinamik olarak değiştirin. Bu sunucu değişkenleri, yeniden yazma koşulları değerlendirin ve üst bilgileri yeniden kullanabilirsiniz. 

Application gateway, aşağıdaki sunucu değişkenleri destekler:

| Desteklenen sunucu değişkenleri | Açıklama                                                  |
| -------------------------- | :----------------------------------------------------------- |
| add_x_forwarded_for_proxy  | "X-iletilen-için" istemci istek üstbilgi alanıyla içeren `client_ip` (Bu tabloda açıklanmıştır) biçiminde (IP1, IP2, IP3,...) eklenmiş değişkeni. "X-iletilen-için" alanı, istemci istek üst bilgisinde mevcut değilse `add_x_forwarded_for_proxy` değişken eşittir `$client_ip` değişkeni. Bu değişken, müşterilerin nerede üstbilgisi yalnızca IP adresi bağlantı noktası bilgileri olmadan içerir, uygulama ağ geçidi tarafından X-iletilen-için üstbilginin yeniden planladığınız senaryolarda özellikle yararlıdır. |
| ciphers_supported          | İstemci tarafından desteklenen şifreleme listesini döndürür          |
| ciphers_used               | Yerleşik bir SSL bağlantısı için kullanılan şifrelemeleri dizesi döndürür |
| client_ip                  | Uygulama ağ geçidi isteği aldığınız istemci IP adresi. Varsa uygulama ağ geçidi ve kaynak istemcisi önce bir ters proxy ardından *client_ip* ters proxy IP adresini döndürür. |
| client_port                | İstemci bağlantı noktası                                                  |
| client_tcp_rtt             | TCP Bağlantısı İstemcisi hakkında bilgiler; TCP_INFO olarak yuva seçeneği destekleyen sistemleri üzerinde kullanılabilir |
| client_user                | HTTP kimlik doğrulaması kullanılırken kullanıcı adı kimlik doğrulaması için sağlanan |
| konak                       | Bu öncelik sırasına: İstek satırı veya ana bilgisayar adı "Ana" istek üstbilgisi alanından konak adı veya sunucu adı ile eşleşen bir istek |
| cookie_*adı*              | *adı* tanımlama bilgisi                                            |
| http_method                | URL isteği yapmak için kullanılan yöntem. Örneğin GET, POST vs. |
| http_status                | oturum durumu, örn: 200, 400, 403 vs.                       |
| http_version               | İstek protokolü, genellikle "HTTP/1.0", "HTTP/1.1" veya "HTTP/2.0" |
| QUERY_STRING               | değişken değeri listesi çiftlerini izleyen "?" İstenen URL. |
| received_bytes             | İstek uzunluğu (istek satırı, başlık ve istek gövdesi dahil) |
| request_query              | İstek satırı bağımsız değişkenleri                                |
| request_scheme             | İstek düzeni, "http" veya "https"                            |
| request_uri                | tam özgün istekle URI'si (bağımsız değişkenler)                   |
| sent_bytes                 | bir istemciye gönderilen bayt sayısı                             |
| server_port                | bir isteği kabul sunucusunun bağlantı noktası                 |
| ssl_connection_protocol    | Yerleşik bir SSL bağlantısı Protokolü döndürür        |
| ssl_enabled                | SSL modu veya boş bir dize değilse bağlantı "on" ise çalışır |

## <a name="rewrite-configuration"></a>Yeniden yapılandırma

HTTP üst bilgisi yeniden yapılandırmak için ihtiyacınız:

1. Http üstbilgileri yeniden yazmak için gereken yeni nesneleri oluşturun:

   - **Eylem yeniden**: istek ve yeniden yazmak için istediğinize isteği üst bilgi alanları ve özgün üstbilgileri için yazılması gereken yeni değeri belirtmek için kullanılır. Bir veya daha fazla yeniden yazma koşulu bir yeniden yazma eylemi ile ilişkilendirilecek seçebilirsiniz.

   - **Koşulu yeniden**: Bu isteğe bağlı bir yapılandırmadır. bir yeniden yazma koşul eklenirse, HTTP (S) istekleri ve yanıtları içeriğini değerlendirir. HTTP (S) istek veya yanıtı yeniden yazma koşulu ile eşleşen olmadığını yeniden yazma koşulu ile ilişkili yeniden yazma eylemi yürütme kararı bağlı olacaktır. 

     Birden fazla koşullar ile ilişkili bir eylem, ardından eylem yalnızca tüm koşulları, yani karşılandığında yürütülecek, mantıksal bir AND işlemi gerçekleştirilir.

   - **Kuralı yeniden**: yeniden yazma kuralı içeren birden çok yeniden yazma eylemi - koşul birleşimleri yeniden yazın.

   - **Kural dizisi**:, farklı bir yeniden yazma kuralları sırayla yürütülen belirlemeye yardımcı olur. Bu, bir yeniden yazma kümesinde birden çok yeniden yazma kuralları gerektiğinde yararlıdır. Daha düşük kuralı dizisi değeri ile yeniden yazma kuralı önce yürütülen. İki yeniden yazma kuralları için aynı kural dizisi sağlarsanız, yürütme sırası belirleyici olacaktır.

   - **Kümesi yeniden**: İstek yönlendirme kuralı ile ilişkilendirilecek birden çok yeniden yazma kuralları içerir.

2. Yeniden yazma kümesi eklemek için gerekli olacaktır (*rewriteRuleSet*) yönlendirme kuralı ile. Yeniden yapılandırma yönlendirme kuralı aracılığıyla kaynak dinleyicisine bağlı olmasıdır. Temel bir yönlendirme kuralını kullanırken, üstbilgi yeniden yapılandırma kaynağı dinleyici ile ilişkili ve genel üstbilgi yeniden yazma. Yola dayalı kural kullanıldığında, URL yolu haritada üstbilgi yeniden yapılandırma tanımlanır. Bu nedenle, yalnızca bir sitenin belirli bir yol alanı için geçerlidir.

Birden çok http üst bilgisi yeniden yazma kümeleri oluşturabilir ve her bir yeniden yazma kümesi birden çok dinleyici uygulanabilir. Ancak, bir yeniden yazma yalnızca belirli bir dinleyici kümesine uygulayabilirsiniz.

## <a name="common-scenarios"></a>Genel senaryolar

Bazı üstbilgi yeniden yazma gerektiren yaygın senaryolar aşağıda belirtilmiştir.

### <a name="remove-port-information-from-the-x-forwarded-for-header"></a>X-iletilen-için üst bilgisinden bağlantı noktası bilgilerini Kaldır

Uygulama ağ geçidi istekleri arka uca iletir önce tüm istekler için X-iletilen-için üst bilgi ekler. Bu üst bilgi biçimi IP: BağlantıNoktası, virgülle ayrılmış bir listesidir. Ancak, burada arka uç sunucularına yalnızca IP adresleri içerecek şekilde üst bilgisi gerektiren senaryolar da olabilir. Bu senaryolara işlemi gerçekleştirmek için üstbilgi yeniden yazma X-iletilen-için üst bilgisinden bağlantı noktası bilgilerini kaldırmak için kullanılabilir. Bunu yapmanın bir yolu, üst bilgi add_x_forwarded_for_proxy sunucu değişkenine ayarlamaktır. 

![Bağlantı noktası Kaldır](media/rewrite-http-headers/remove-port.png)

### <a name="modify-the-redirection-url"></a>Yeniden yönlendirme URL'sini değiştirme

Bir arka uç uygulaması, yeniden yönlendirme yanıtı gönderdiğinde, arka uç uygulama tarafından belirtilenden farklı bir URL için istemciyi yeniden yönlendirmek isteyebilirsiniz. Bu tür senaryolardan bir app service, uygulama ağ geçidi barındırılan ve kendi göreli yol (arasında tekrar yönlendirme contoso.azurewebsites.net/path1 contoso.azurewebsites.net/path2) için bir yeniden yönlendirme yapmak istemcinin ihtiyaç duyduğu andır. 

App service, çok kiracılı bir hizmet olduğundan, doğru uç noktasına yönlendirme için istekte konak üst bilgisi kullanır. Uygulama Hizmetleri yüklü bir varsayılan etki alanı adı *. azurewebsites.net (örneğin, contoso.azurewebsites.net) (örneğin, contoso.com) uygulama ağ geçidinin etki alanı adından farklıdır. Özgün istekteki istemci uygulama ağ geçidinin alan adı contoso.com konak adı olduğundan, uygulama ağ geçidi böylece app service doğru uç noktaya yönlendirebilirsiniz contoso.azurewebsites.net için ana bilgisayar adını değiştirir. App service bir yeniden yönlendirme yanıtı gönderirken bir uygulama ağ geçidinden aldığı istek yanıtının location üst aynı ana bilgisayar adını kullanır. Bu nedenle, istemci uygulama ağ geçidi (contoso.com/path2) yerine doğrudan contoso.azurewebsites.net/path2, isteği yapar. Uygulama ağ geçidi atlayarak arzu değil. 

Ana bilgisayar adını uygulama ağ geçidinin etki alanı adı için konum üstbilgisi ayarlayarak bu sorun çözülebilir. Bunu yapmak için bir yeniden yazma kuralı ile yanıt location üst bilgisini girerek azurewebsites.net içeriyorsa değerlendiren bir koşul oluşturabilirsiniz `(https?):\/\/.*azurewebsites\.net(.*)$` deseni olarak ve uygulama ağ geçidinin olmasını location üst bilgisini yeniden yazma için bir eylem gerçekleştirir ana bilgisayar adı girerek `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` üstbilgi değeri.

![Location üst bilgisini değiştirin](media/rewrite-http-headers/app-service-redirection.png)

### <a name="implement-security-http-headers-to-prevent-vulnerabilities"></a>Uygulama güvenlik açıklarını önlemek için HTTP güvenlik üst bilgileri

Uygulama yanıt olarak gerekli üst bilgileri uygulayarak güvenlik açıklarına düzeltilebilir. Bu güvenlik üst bilgileri bazıları X XSS koruma, katı aktarım güvenliği, içerik-güvenlik-ilke, vs. Tüm yanıtları için bu üst bilgilerini ayarlayacak şekilde uygulama ağ geçidi'ni kullanabilirsiniz.

![Güvenlik üst bilgisi](media/rewrite-http-headers/security-header.png)

## <a name="limitations"></a>Sınırlamalar

- Bağlantı, yükseltme ve konak üstbilgileri yeniden yazma henüz desteklenmiyor.

- Üst bilgi adları içerebilir herhangi bir alfasayısal karakter ve belirli simgeleri sınıfında tanımlandığı gibi [RFC 7230](https://tools.ietf.org/html/rfc7230#page-27). Ancak, "çizgi" şu anda desteklemiyoruz (\_) üst bilgi adı özel karakter. 

## <a name="need-help"></a>Yardıma mı ihtiyacınız var?

Şu adresten bizimle [ AGHeaderRewriteHelp@microsoft.com ](mailto:AGHeaderRewriteHelp@microsoft.com) bu yeteneğe sahip herhangi bir Yardım gerektiği durumlarda.

## <a name="next-steps"></a>Sonraki adımlar

HTTP üstbilgileri yeniden öğrenmek için bkz:

-  [Azure portalını kullanarak HTTP üst bilgilerini yeniden yazma](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers-portal)
-  [Azure PowerShell kullanarak yeniden yazma HTTP üstbilgileri](add-http-header-rewrite-rule-powershell.md)