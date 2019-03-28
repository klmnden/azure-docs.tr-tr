---
title: Azure Application Gateway, HTTP üst bilgilerini yeniden | Microsoft Docs
description: Bu makalede Azure Application Gateway, HTTP üst bilgilerini yeniden yazma özelliği genel bir bakış sağlar.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 12/20/2018
ms.author: absha
ms.openlocfilehash: 67603e326583400e8fc250ea6120297e7a94d101
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58520930"
---
# <a name="rewrite-http-headers-with-application-gateway-public-preview"></a>Uygulama ağ geçidi (genel Önizleme) ile yeniden yazma HTTP üstbilgileri

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

HTTP üstbilgileri, istemci ve sunucu istek veya yanıt ek bilgilerle geçmesine izin verin. Güvenlikle ilgili üstbilgi ekleme gibi birkaç önemli senaryoları görevi bu HTTP üstbilgileri yardımcı yeniden yazma alanları HSTS gibi / X XSS koruma veya yanıt üstbilgi alanlarını kaldırma, hangi arka uç sunucu adı gibi hassas bilgileri açığa.

Application Gateway, gelen HTTP isteklerini, hem de giden HTTP yanıt üst bilgilerini yeniden yazabilme becerisine artık desteklemektedir. Ekleyin, kaldırın veya istek/yanıt paketleri istemci ve arka uç havuzları arasında taşırken HTTP istek ve yanıt üstbilgileri güncelleştirmeniz mümkün olacaktır. Hem standart hem de standart üstbilgi alanlarını yazabilirsiniz.

> [!NOTE]
> 
> HTTP üst bilgisi yeniden yazma desteği yalnızca kullanılabilir [yeni SKU [Standard_V2\]](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant)

Uygulama ağ geçidi üstbilgi yeniden yazma desteği sunar:

- **Genel üstbilgi yeniden yazma**: Tüm isteklerin ve yanıtların siteye ilişkin belirli üstbilgileri yazabilirsiniz.
- **Üst bilgi yol tabanlı yeniden yazma**: Bu tür bir yeniden yazma yalnızca istekleri ve yalnızca bir belirli site alanı, örneğin bir alışveriş sepeti alanı /cart/ tarafından belirtilen ilgili yanıtlar için üstbilgi yeniden etkinleştirir\*.

Bu değişiklik, şunları yapmanız gerekir:

1. Http üstbilgileri yeniden yazmak için gereken yeni nesneleri oluşturun: 
   - **RequestHeaderConfiguration**: Bu nesne yeniden yazmak için istediğinize isteği üst bilgi alanları ve özgün üstbilgileri için yazılması gereken yeni değeri belirtmek için kullanılır.
   - **ResponseHeaderConfiguration**: Bu nesne yeniden yazmak için istediğinize yanıt üstbilgi alanlarını ve özgün üstbilgileri için yazılması gereken yeni değeri belirtmek için kullanılır.
   - **ActionSet**: Bu nesne, yukarıda belirtilen istek ve yanıt üstbilgileri yapılandırmalarını içerir. 
   - **RewriteRule**: Bu nesnenin tüm içeren *actionSets* yukarıda belirtilen. 
   - **RewriteRuleSet**-tüm bu nesneyi içeren *rewriteRules* ve bir istek yönlendirme kuralı - temel veya yol tabanlı eklenmesi gerekir.
2. Daha sonra yeniden yazma kuralı kümesiyle yönlendirme kuralı ekleme gerekecektir. Oluşturulduktan sonra bu yeniden yazma yapılandırma kaynağı dinleyicisi aracılığıyla yönlendirme kuralı eklenir. Temel bir yönlendirme kuralını kullanırken, üstbilgi yeniden yapılandırma kaynağı dinleyici ile ilişkili ve genel üstbilgi yeniden yazma. Yola dayalı kural kullanıldığında, URL yolu haritada üstbilgi yeniden yapılandırma tanımlanır. Bu nedenle, yalnızca bir sitenin belirli bir yol alanı için geçerlidir.

Birden çok http üst bilgisi yeniden yazma kuralı kümesi oluşturabilir ve her bir yeniden yazma kuralı kümesi birden çok dinleyici uygulanabilir. Ancak, yalnızca bir http yeniden yazma kuralı için belirli bir dinleyici ayarlayın uygulayabilirsiniz.

Üst bilgilerde değeri yazabilirsiniz:

- Metin değeri. 

  *Örnek:* 

  ```azurepowershell-interactive
  $responseHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "Strict-Transport-Security" -  HeaderValue "max-age=31536000")
  ```

- Başka bir üst bilgisinden değeri. 

  *Örnek 1:* 

  ```azurepowershell-interactive
  $requestHeaderConfiguration= New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "X-New-RequestHeader" -HeaderValue {http_req_oldHeader}
  ```

  > [!Note] 
  > İstek üstbilgisi belirtmek için sözdizimi kullanmanız gerekir: {http_req_headerName}

  *Örnek 2*:

  ```azurepowershell-interactive
  $responseHeaderConfiguration= New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "X-New-ResponseHeader" -HeaderValue {http_resp_oldHeader}
  ```

  > [!Note] 
  > Yanıt üst bilgisi belirtmek için sözdizimi kullanmanız gerekir: {http_resp_headerName}

- Desteklenen sunucu değişkenleri değer.

  *Örnek:* 

  ```azurepowershell-interactive
  $requestHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "Ciphers-Used" -HeaderValue "{var_ciphers_used}"
  ```

  > [!Note] 
  > Bir sunucu değişkeni belirtmek için sözdizimi kullanmanız gerekir: {var_serverVariable}

- Yukarıdaki bir birleşim.

## <a name="server-variables"></a>Sunucu değişkenleri

Sunucu değişkenleri, bir web sunucusunda yararlı bilgileri depolar. Bu değişkenler, istemcinin IP adresini veya web tarayıcı türü gibi bağlantı sunucusu, istemcinin geçerli istek ile bağlantı hakkında bilgi sağlar. Yeni sayfa yüklendiğinde veya form gönderildikten gibi dinamik olarak değişir.  Bu değişkenler kullanıcılar'ı kullanarak istek üst bilgileri ve bunun yanı sıra yanıt üst bilgilerini ayarlayabilirsiniz. 

Bu özellik aşağıdaki sunucu değişkenleri yazmaksızın üstbilgileri destekler:

| Desteklenen sunucu değişkenleri | Açıklama                                                  |
| -------------------------- | :----------------------------------------------------------- |
| ciphers_supported          | İstemci tarafından desteklenen şifreleme listesini döndürür          |
| ciphers_used               | Yerleşik bir SSL bağlantısı için kullanılan şifrelemeleri dizesi döndürür |
| client_ip                  | Uygulama ağ geçidi isteği aldığınız istemci IP adresi. Varsa uygulama ağ geçidi ve kaynak istemcisi önce bir ters proxy ardından *client_ip* ters proxy IP adresi döndürür. tjsi değişkeni müşteriler burada yalnızca IP adresi bağlantı noktası bilgileri olmadan başlık içeren Application Gateway tarafından X-iletilen-için üstbilginin yeniden planladığınız senaryolarda özellikle yararlıdır. |
| client_port                | İstemci bağlantı noktası                                                  |
| client_tcp_rtt             | TCP Bağlantısı İstemcisi hakkında bilgiler; TCP_INFO olarak yuva seçeneği destekleyen sistemleri üzerinde kullanılabilir |
| client_user                | HTTP kimlik doğrulaması kullanılırken kullanıcı adı kimlik doğrulaması için sağlanan |
| konak                       | Bu öncelik sırasına: İstek satırı veya ana bilgisayar adı "Ana" istek üstbilgisi alanından konak adı veya sunucu adı ile eşleşen bir istek |
| cookie_*adı*              | *adı* tanımlama bilgisi |
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

## <a name="limitations"></a>Sınırlamalar

- HTTP üstbilgileri yeniden yazmak için bu özelliği şu anda yalnızca Azure PowerShell, Azure API ve Azure SDK'sı kullanılabilir. Portal ve Azure CLI aracılığıyla destek yakında kullanıma sunulacak.

- HTTP üst bilgisi yeniden yazma desteği yalnızca yeni SKU üzerinde desteklenir [Standard_V2](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant). Bu özellik eski SKU üzerinde desteklenmez.

- Bağlantı, yükseltme ve konak üstbilgileri yeniden yazma henüz desteklenmiyor.

- Http üstbilgileri koşullu olarak yeniden yazma özelliği yakında kullanıma sunulacak.

- Üst bilgi adları içerebilir herhangi bir alfasayısal karakter ve belirli simgeleri sınıfında tanımlandığı gibi [RFC 7230](https://tools.ietf.org/html/rfc7230#page-27). Ancak, "çizgi" şu anda desteklemiyoruz (\_) üst bilgi adı özel karakter. 

## <a name="need-help"></a>Yardıma mı ihtiyacınız var?

Şu adresten bizimle [ AGHeaderRewriteHelp@microsoft.com ](mailto:AGHeaderRewriteHelp@microsoft.com) bu yeteneğe sahip herhangi bir Yardım gerektiği durumlarda.

## <a name="next-steps"></a>Sonraki adımlar

HTTP üstbilgileri yeniden yazma özelliği hakkında daha fazla edindikten sonra Git [bir otomatik ölçeklendirme ve HTTP üst bilgilerini yeniden yazar bölgesel olarak yedekli bir uygulama ağ geçidi oluşturma](tutorial-http-header-rewrite-powershell.md) veya [yeniden HTTP üstbilgileri mevcut otomatik ölçeklendirme ve Bölgesel olarak yedekli bir uygulama ağ geçidi](add-http-header-rewrite-rule-powershell.md)
