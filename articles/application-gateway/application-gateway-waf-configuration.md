---
title: Web uygulaması güvenlik duvarı istek boyutu sınırları ve Azure Application Gateway'de - Azure portalı hariç tutma listeleri
description: Bu makale, web uygulaması güvenlik duvarı istek boyutu sınırları hakkında bilgi sağlar ve Azure portal ile Application Gateway yapılandırmasında dışlama listeler.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 5/15/2019
ms.author: victorh
ms.topic: conceptual
ms.openlocfilehash: 5ddcdeca41e2f21fa27db25f7e0721c7ef87e491
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65620281"
---
# <a name="web-application-firewall-request-size-limits-and-exclusion-lists"></a>Web uygulaması güvenlik duvarı istek boyutu sınırları ve hariç tutma listeleri

Azure Application Gateway web uygulaması Güvenlik Duvarı (WAF), web uygulamaları için koruma sağlar. Bu makalede WAF istek boyutu sınırları ve yapılandırma dışlama listeler.

## <a name="waf-request-size-limits"></a>WAF istek boyutu sınırları

![İstek boyutu sınırları](media/application-gateway-waf-configuration/waf-requestsizelimit.png)

Web uygulaması güvenlik duvarı, istek boyutu sınırları içinde alt ve üst sınırlarını yapılandırmanıza olanak sağlar. Aşağıdaki iki boyutu sınırlarını yapılandırma kullanılabilir:

- En fazla istek gövdesi boyutu alanı kilobayt ve denetimler genel istek boyutu sınırı dışında herhangi bir dosya yükler belirtilir. Bu alanın 128 KB'lık maksimum değerine 1 KB'lık minimumdan değişebilir. 128 KB istek gövdesi boyutu için varsayılan değerdir.
- Dosya karşıya yükleme sınırı alanı MB olarak belirtilir ve izin verilen en büyük dosya yükleme boyutunu yönetir. Orta SKU en fazla 100 MB'ın olsa Bu alan 1 MB en düşük değerini ve en fazla 500 MB büyük SKU örnekleri için sahip olabilir. Dosya karşıya yükleme sınırı için varsayılan değer 100 MB'dir.

Ayrıca, WAF istek gövdesi İnceleme açıp kapatmak için yapılandırılabilir bir düğme sunar. İstek gövdesi İnceleme varsayılan olarak etkinleştirilir. İstek gövdesi İnceleme kapalıysa, WAF HTTP ileti gövdesi içeriğini değerlendirmez. Böyle durumlarda, üst bilgiler, tanımlama bilgileri ve URI WAF kurallarını zorunlu tutmak WAF devam eder. İstek gövdesi İnceleme kapalıysa, en büyük istek gövdesi boyutu alan geçerli değildir ve ayarlanamaz. İletiler için istek gövdesi İnceleme kapatmak için WAF gönderilecek 128 KB'tan büyük sağlar, ancak ileti gövdesi güvenlik açıklarına karşı inceledi değil.

## <a name="waf-exclusion-lists"></a>WAF hariç tutma listeleri

![waf exclusion.png](media/application-gateway-waf-configuration/waf-exclusion.png)

WAF hariç tutma listeleri, belirli bir WAF değerlendirme özniteliklerini atlamak izin verin. Yaygın olarak karşılaşılan örneklerden, Active Directory kimlik doğrulaması veya parola alanı için kullanılan belirteçleri eklenen ' dir. Bir hatalı pozitif sonuç WAF kurallarından tetikleyebilir özel karakterler içermesini gibi öznitelikleri fazladır. Bir öznitelik WAF dışlama listesine eklendikten sonra tüm yapılandırılmış ve etkin bir WAF kural tarafından kabul değil. Hariç tutma listeleri, genel kapsam içindedir.

Hariç tutma listeleri aşağıdaki öznitelikleri eklenebilir:

* İstek üst bilgileri
* İstek tanımlama bilgileri
* İstek özniteliği adı (argumenty)

   * Çok bölümlü form verilerinin
   * XML
   * JSON
   * URL sorgu değişkenleri

Belirtin tam istek üst bilgisi, gövdesi, tanımlama bilgisi veya sorgu dizesi özniteliği eşleşme.  Veya kısmi eşleşmeler isteğe bağlı olarak belirtebilirsiniz. Dışlama hiçbir zaman değeri üzerinde bir üstbilgi alanı her zaman açıktır. Hariç tutma kuralları kapsamı geneldir ve tüm sayfaları ve tüm kurallar için.

Desteklenen eşleşme ölçütlerini işleçler şunlardır:

- **Eşittir**:  Bu işleç, tam bir eşleşme için kullanılır. Adlı bir üst bilgi seçmek için örnek olarak **bearerToken**, eşittir işleci olarak seçiciyi kullanın **bearerToken**.
- **İle başlayan**: Bu işleç, belirtilen Seçici değerle başlayan tüm alanları eşleşir.
- **İle biten**:  Bu işleç, belirtilen seçici değeri ile biten tüm isteği alanları eşleşir.
- **İçeren**: Bu işleç belirtilen seçici değeri içeren tüm isteği alanları eşleşir.
- **Herhangi bir eşittir**: Bu işleç, tüm istek alanları eşleşir. * Seçici değeri olması.

Her durumda eşleştirme büyük küçük harfe duyarlı değildir ve normal ifade Seçici izin verilmiyor.

### <a name="examples"></a>Örnekler

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Aşağıdaki örnekler dışlamaları kullanımını göstermektedir.

### <a name="example-1"></a>Örnek 1

Bu örnekte, user-agent üstbilgisi dışlamak istediğiniz. User-agent isteği üstbilgisi, uygulama türü, işletim sistemi, yazılım satıcısı veya istekte bulunan yazılım kullanıcı aracısı yazılım sürümünü belirlemek ağ protokolü eşleri izin veren özellik dizeyi içerir. Daha fazla bilgi için [User-Agent](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent).

Herhangi bir sayıda bu üst bilgiyi değerlendirirken devre dışı bırakmak için neden olabilir. WAF görür ve kötü amaçlı olduğu varsayılır bir dize olabilir. Örneğin, Klasik SQL saldırı "x = x" bir dize. Bazı durumlarda, bu yasal trafik olabilir. Bu nedenle bu başlığı WAF değerlendirmesinden gelen hariç gerekebilir.

Aşağıdaki Azure PowerShell cmdlet'i, user-agent üstbilgisi değerlendirmesinden gelen dışlar:

```azurepowershell
$exclusion1 = New-AzApplicationGatewayFirewallExclusionConfig `
   -MatchVariable "RequestHeaderNames" `
   -SelectorMatchOperator "Equals" `
   -Selector "User-Agent"
```

### <a name="example-2"></a>Örnek 2

Bu örnek değeri hariç *kullanıcı* istekte URL yoluyla geçirilen parametre. Örneğin, bir kullanıcı alanı, onu engellediğinde, WAF kötü amaçlı içerik görüntüleyen bir dize içermesi ortamınızdaki ortak varsayalım.  WAF alanındaki herhangi bir şey değerlendirmez. böylece, bu durumda kullanıcı parametresi hariç tutabilirsiniz.

Aşağıdaki Azure PowerShell cmdlet'i kullanıcı parametresi değerlendirmesinden gelen dışlar:

```azurepowershell
$exclusion2 = New-AzApplicationGatewayFirewallExclusionConfig `
   -MatchVariable "RequestArgNames" `
   -SelectorMatchOperator "Equals" `
   -Selector "user"
```
Dolayısıyla URL **http://www.contoso.com/?user=fdafdasfda** geçirilen WAF için dize değerlendirmek olmaz **fdafdasfda**.

## <a name="next-steps"></a>Sonraki adımlar

WAF ayarlarınızı yapılandırdıktan sonra WAF günlükleri görüntüleme hakkında bilgi edinebilirsiniz. Daha fazla bilgi için [Application Gateway tanılama](application-gateway-diagnostics.md#diagnostic-logging).
