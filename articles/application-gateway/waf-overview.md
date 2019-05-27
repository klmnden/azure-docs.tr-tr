---
title: Azure Application Gateway web uygulaması güvenlik duvarı için giriş
description: Bu makalede, Application Gateway için web uygulaması Güvenlik Duvarı (WAF) genel bir bakış sağlanmaktadır
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 5/22/2019
ms.author: amsriva
ms.topic: conceptual
ms.openlocfilehash: 9c2759222198f5df682d9e7a5363c0d9679e0fad
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65991394"
---
# <a name="web-application-firewall-for-azure-application-gateway"></a>Azure Application Gateway Web uygulaması güvenlik duvarı

Azure Application Gateway, web uygulamalarınızda ortak açıklardan yararlanmaya ve güvenlik açıkları merkezi koruma sağlayan bir web uygulaması Güvenlik Duvarı (WAF) sunar. Web uygulamaları tarafından yaygın olarak bilinen açıklarından kötü amaçlı saldırılar giderek hedeflenir. SQL ekleme ve siteler arası betik en yaygın saldırı arasında var.

Uygulama kodunda bu tür saldırılarını önleme zordur. Bu ayrıntılı bakım, düzeltme eki uygulama ve uygulama topolojisinin birden çok katmanına izleme gerektirebilir. Merkezi bir web uygulaması güvenlik duvarı, güvenlik yönetimi çok daha kolay hale getirir. Bir WAF, ayrıca uygulama yöneticileri tehditleri ve izinsiz girişi karşı koruma daha iyi güvence verir.

Bir WAF çözümü, merkezi olarak her tek tek web uygulamasını güvenli hale getirme yerine bilinen bir güvenlik düzeltme eki uygulayarak güvenlik tehdidine daha hızlı tepki verebilir. Mevcut uygulama ağ geçitleri yangın duvar özellikli uygulama ağ geçitleri kolaylıkla dönüştürülebilir.

Application Gateway WAF dayanır [çekirdek kural kümesi (CRS)](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.0 veya 2.2.9'daki gelen açık Web uygulaması güvenlik Project (OWASP). WAF, gereken ek bir yapılandırma olmadan yeni güvenlik açıklarına karşı koruma içerecek şekilde otomatik olarak güncelleştirir.

![Application Gateway WAF diyagramı](./media/waf-overview/WAF1.png)

Uygulama ağ geçidi, bir uygulama teslim denetleyicisi (ADC) çalışır. Güvenli Yuva Katmanı (SSL) sonlandırma, tanımlama bilgilerine dayalı oturum benzeşimi, hepsini bir kez deneme yük dağıtımı, içerik tabanlı yönlendirme, birden çok Web siteleri ve güvenlik geliştirmeleri barındırma olanağı sunar.

Uygulama ağ geçidi güvenlik geliştirmeleri SSL ilke yönetimi içerir ve uçtan uca SSL desteği. Application Gateway WAF tümleştirme tarafından uygulama güvenliği güçlendirilmiş. Birlikte web uygulamalarınızı yaygın güvenlik açıklarına karşı korur. Ve bunu yönetmek için bir kolayca yapılandırma merkezi bir konum sağlar.

## <a name="benefits"></a>Avantajlar

Bu bölümde, uygulama ağ geçidi ve, WAF'yi sağlayan temel avantajlar açıklanmaktadır.

### <a name="protection"></a>Koruma

* Web güvenlik açıklarından ve saldırılarından arka uç kodunda değişiklik yapmadan web uygulamalarınızı koruyun.

* Aynı anda birden çok web uygulamaları koruyun. Uygulama ağ geçidi örneğini barındıran bir web uygulaması güvenlik duvarı tarafından korunur en fazla 100 Web siteleri.

### <a name="monitoring"></a>İzleme

* Gerçek zamanlı bir WAF günlüğü kullanarak web uygulamalarınızı saldırılara izleyin. Günlük ile tümleşiktir [Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview.md) WAF uyarılarını takip edip eğilimleri kolayca izleyin.

* Application Gateway WAF, Azure Güvenlik Merkezi ile tümleşiktir. Güvenlik Merkezi, tüm Azure kaynaklarınızın güvenlik durumuna genel bir görünüm sağlar.

### <a name="customization"></a>Özelleştirme

* WAF kurallarını ve kural gruplarını, uygulama gereksinimlerinize göre ve hatalı pozitif sonuçları ortadan kaldırmak için özelleştirebilirsiniz.

## <a name="features"></a>Özellikler

- SQL ekleme koruması.
- Siteler arası komut dosyası koruması.
- Komut ekleme, HTTP isteği kaçakçılığı, HTTP yanıtı bölme ve uzak dosya ekleme gibi diğer yaygın web saldırılarına karşı koruma.
- HTTP protokolü ihlallerine karşı koruma.
- Eksik gibi HTTP protokolü anormalliklerine karşı koruma, konak kullanıcısı-aracısı ve kabul üst bilgileri.
- Robotlar, gezginler ve tarayıcıları karşı koruma.
- Yaygın yanlış uygulama yapılandırmalarını (örneğin, Apache ve IIS gibi) algılanması.
- Yapılandırılabilir istek boyutu, alt ve üst sınırları ile sınırlar.
- Hariç tutma listeleri, belirli bir WAF değerlendirme özniteliklerini atlamak sağlar. Kimlik doğrulaması veya parola alanı için kullanılan Active Directory eklenen belirteçleri buna yaygın bir örnektir.

### <a name="core-rule-sets"></a>Çekirdek kural kümeleri

Application Gateway, CRS 3.0 ve CRS 2.2.9 şeklinde iki kural kümelerini destekler. Bu kurallar, web uygulamalarınızı kötü amaçlı etkinliği koruyun.

Application Gateway WAF CRS 3.0 ile varsayılan olarak önceden yapılandırılmış olarak gelir. Ancak, CRS 2.2.9 kullanmayı seçebilirsiniz. CRS 3.0 teklifler hatalı pozitif sonuçları CRS 2.2.9 ile karşılaştırıldığında sınırlı. Ayrıca [kuralları gereksinimlerinize uyacak şekilde özelleştirin](application-gateway-customize-waf-rules-portal.md).

WAF, aşağıdaki web güvenlik açıklarına karşı korur:

- SQL ekleme saldırıları
- Siteler arası betik saldırıları
- Komut ekleme, HTTP gibi diğer yaygın saldırılardan kaçakçılığı, HTTP yanıtı bölme ve uzak dosya ekleme isteği
- HTTP protokolü ihlallerine
- HTTP protokolü anormalliklerine gibi eksik konak kullanıcısı-aracısı ve kabul üst bilgileri
- Robotlar, gezginler ve tarayıcıları
- Yaygın yanlış uygulama yapılandırmalarını (örneğin, Apache ve IIS)

#### <a name="owasp-crs-30"></a>OWASP CRS 3.0

CRS 3.0, aşağıdaki tabloda gösterilen 13 kural grubunu içerir. Her grubu devre dışı bırakılabilen birden çok kural içerir.

|Kural grubu|Açıklama|
|---|---|
|**[REQUEST-911-METHOD-ENFORCEMENT](application-gateway-crs-rulegroups-rules.md#crs911)**|Kilitleme yöntemleri (PUT, PATCH)|
|**[REQUEST-913-SCANNER-DETECTION](application-gateway-crs-rulegroups-rules.md#crs913)**|Bağlantı noktası ve ortam tarayıcılara karşı korumaya|
|**[REQUEST-920-PROTOCOL-ENFORCEMENT](application-gateway-crs-rulegroups-rules.md#crs920)**|Protokol ve kodlama sorunlarına karşı koruma|
|**[REQUEST-921-PROTOCOL-ATTACK](application-gateway-crs-rulegroups-rules.md#crs921)**|Üst bilgi ekleme, istek kaçakçılığı ve yanıt karşı koruma|
|**[REQUEST-930-APPLICATION-ATTACK-LFI](application-gateway-crs-rulegroups-rules.md#crs930)**|Dosya ve yol saldırılarına karşı koruyun|
|**[REQUEST-931-APPLICATION-ATTACK-RFI](application-gateway-crs-rulegroups-rules.md#crs931)**|Uzak dosya ekleme (RFI) saldırılarına karşı koruyun|
|**[REQUEST-932-APPLICATION-ATTACK-RCE](application-gateway-crs-rulegroups-rules.md#crs932)**|Tekrar uzaktan kod yürütme saldırılarına|
|**[REQUEST-933-APPLICATION-ATTACK-PHP](application-gateway-crs-rulegroups-rules.md#crs933)**|PHP ekleme saldırılarına karşı koruyun|
|**[REQUEST-941-APPLICATION-ATTACK-XSS](application-gateway-crs-rulegroups-rules.md#crs941)**|Siteler arası betik saldırılara karşı koruma|
|**[REQUEST-942-APPLICATION-ATTACK-SQLI](application-gateway-crs-rulegroups-rules.md#crs942)**|SQL ekleme saldırılarına karşı koruyun|
|**[REQUEST-943-APPLICATION-ATTACK-SESSION-FIXATION](application-gateway-crs-rulegroups-rules.md#crs943)**|Oturum sabitleme saldırılarına karşı koruyun|

#### <a name="owasp-crs-229"></a>OWASP CRS 2.2.9

Aşağıdaki tabloda gösterildiği gibi CRS 2.2.9 10 kural grubunu içerir. Her grubu devre dışı bırakılabilen birden çok kural içerir.

|Kural grubu|Açıklama|
|---|---|
|**[crs_20_protocol_violations](application-gateway-crs-rulegroups-rules.md#crs20)**|(Örneğin, geçersiz karakterler ya da bir Al ile istek gövdesi) protokolü ihlallerine karşı koruma|
|**[crs_21_protocol_anomalies](application-gateway-crs-rulegroups-rules.md#crs21)**|Hatalı üst bilgilere karşı koruma|
|**[crs_23_request_limits](application-gateway-crs-rulegroups-rules.md#crs23)**|Bağımsız değişkenler veya sınırları aşan dosyaları karşı koruma|
|**[crs_30_http_policy](application-gateway-crs-rulegroups-rules.md#crs30)**|Kısıtlı yöntemler, üst bilgiler ve dosya türlerine karşı koruma|
|**[crs_35_bad_robots](application-gateway-crs-rulegroups-rules.md#crs35)**|Web gezginleri ve tarayıcılara karşı koruma|
|**[crs_40_generic_attacks](application-gateway-crs-rulegroups-rules.md#crs40)**|(Oturum sabitleme, uzak dosya ekleme ve PHP ekleme gibi) genel saldırılara karşı koruma|
|**[crs_41_sql_injection_attacks](application-gateway-crs-rulegroups-rules.md#crs41sql)**|SQL ekleme saldırılarına karşı koruyun|
|**[crs_41_xss_attacks](application-gateway-crs-rulegroups-rules.md#crs41xss)**|Siteler arası betik saldırılara karşı koruma|
|**[crs_42_tight_security](application-gateway-crs-rulegroups-rules.md#crs42)**|Yol çapraz geçişi saldırılarına karşı koruyun|
|**[crs_45_trojans](application-gateway-crs-rulegroups-rules.md#crs45)**|Arka kapı Truva atlarına karşı korumaya|

### <a name="waf-modes"></a>WAF modları

Application Gateway WAF, aşağıdaki iki modda çalışacak şekilde yapılandırılabilir:

* **Algılama modu**: İzler ve tüm tehdit uyarıları günlüğe kaydeder. Uygulama ağ geçidi için günlük tanılamayı Aç **tanılama** bölümü. Ayrıca WAF günlüğünün seçili ve açık olduğundan emin olmanız gerekir. Algılama modunda çalışırken, web uygulaması güvenlik duvarı gelen istekleri engellemez.
* **Önleme modu**: Blokları izinsiz girişleri ve kuralları algılamak saldırıları. Saldırgan "403 yetkisiz erişim" özel durumu alır ve bağlantı sonlandırılır. Önleme modu bu tür saldırıları WAF günlüklerine kaydeder.

### <a name="anomaly-scoring-mode"></a>Anomali Puanlama modu

OWASP trafiği engellemek karar verme için iki mod vardır: Geleneksel modu ve Anomali Puanlama modu.

Geleneksel modunda herhangi bir kural eşleşen trafik herhangi bir kural eşleşen bağımsız olarak kabul edilir. Bu mod, anlaşılması kolay bir işlemdir. Ancak, belirli bir istek kaç kurallarla eşleşecek hakkında bilgi eksikliği bir sınırlamadır. Bu nedenle, Anomali Puanlama modu sunulmuştur. OWASP 3 için varsayılandır. *x*.

Güvenlik Duvarı önleme modunda olduğunda Anomali Puanlama modunda hemen herhangi bir kural eşleşen trafik engellenmiş değil. Kuralları belirli bir önem derecesi vardır: *Kritik*, *hata*, *uyarı*, veya *bildirimi*. Bu önem derecesi Anomali puanı adlı istek için sayısal bir değer etkiler. Örneğin, bir *uyarı* kural eşleştirme 3 puana katkıda bulunur. Bir *kritik* kural eşleşen 5 katkıda bulunur.

|Severity  |Değer  |
|---------|---------|
|Kritik     |5|
|Hata        |4|
|Uyarı      |3|
|Bildirim       |2|

Bir Anomali puanı trafiği engellemek için 5 eşiğinin yoktur. Bunu, tek bir *kritik* kural eşleşmedir önleme modunda bile bir istek engellemek Application Gateway WAF için yeterli. Ancak bir *uyarı* kural eşleşme, yalnızca tek başına trafiği engellemek için yeterli değildir Anomali puanı 3 artırır.

> [!NOTE]
> Trafiği WAF kural eşleşen olduğunda günlüğe ileti "Engellendi." eylemi değeri içerir Ancak, gerçekte yalnızca bir Anomali puanı 5 için engellenen veya daha yüksek trafik.  

### <a name="waf-monitoring"></a>WAF izleme

Uygulama ağ geçidinizin durumunu izlemek önemlidir. WAF ile koruduğu uygulamaların durumunu izleme Azure Güvenlik Merkezi, Azure İzleyici ile entegrasyon tarafından desteklenen ve Azure İzleyici günlüğe kaydeder.

![Application Gateway WAF tanılama diyagramı](./media/waf-overview/diagnostics.png)

#### <a name="azure-monitor"></a>Azure İzleyici

Uygulama ağ geçidi günlükleri ile tümleştirilir [Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview.md). Bu, WAF uyarıları ve günlükleri de dahil olmak üzere tanılama bilgilerini izlemenize olanak sağlar. Bu özellik erişebileceğiniz **tanılama** portalında veya Azure İzleyici aracılığıyla doğrudan Application Gateway kaynağında sekmesi. Günlükleri etkinleştirme hakkında daha fazla bilgi edinmek için [Application Gateway tanılama](application-gateway-diagnostics.md).

#### <a name="azure-security-center"></a>Azure Güvenlik Merkezi

[Güvenlik Merkezi](../security-center/security-center-intro.md) önleyin, algılayın ve tehditlere yanıt verin yardımcı olur. Bu, artırılmış görünürlük ve denetim, Azure kaynaklarınızın güvenliğine sağlar. Application Gateway [Güvenlik Merkezi ile tümleşik](application-gateway-integration-security-center.md). Güvenlik Merkezi, korumasız web uygulamalarını algılamak için ortamınızı tarar. Bu, bu savunmasız kaynakları korumak için Application Gateway WAF önerilerde bulunur. Doğrudan Güvenlik Merkezi'nden güvenlik duvarları oluşturun. Bu WAF örnekleri Güvenlik Merkezi ile tümleşiktir. Bunlar uyarılar ve durum bilgileri için Güvenlik Merkezi raporlama için gönderin.

![Güvenlik merkezine genel bakış penceresi](./media/waf-overview/figure1.png)

#### <a name="logging"></a>Günlüğe kaydetme

Application Gateway WAF, algıladığı her tehdit ayrıntılı raporlama sağlar. Günlük kaydı Azure tanılama günlükleri ile tümleştirilir. Uyarılar .json biçiminde kaydedilir. Bu günlükleri ile tümleştirilebilir [Azure İzleyicisi](../azure-monitor/insights/azure-networking-analytics.md).

![Windows uygulama ağ geçidi tanılama günlükleri](./media/waf-overview/waf2.png)

```json
{
  "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupId}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{appGatewayName}",
  "operationName": "ApplicationGatewayFirewall",
  "time": "2017-03-20T15:52:09.1494499Z",
  "category": "ApplicationGatewayFirewallLog",
  "properties": {
    "instanceId": "ApplicationGatewayRole_IN_0",
    "clientIp": "104.210.252.3",
    "clientPort": "4835",
    "requestUri": "/?a=%3Cscript%3Ealert(%22Hello%22);%3C/script%3E",
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "ruleId": "941320",
    "message": "Possible XSS Attack Detected - HTML Tag Handler",
    "action": "Blocked",
    "site": "Global",
    "details": {
      "message": "Warning. Pattern match \"<(a|abbr|acronym|address|applet|area|audioscope|b|base|basefront|bdo|bgsound|big|blackface|blink|blockquote|body|bq|br|button|caption|center|cite|code|col|colgroup|comment|dd|del|dfn|dir|div|dl|dt|em|embed|fieldset|fn|font|form|frame|frameset|h1|head|h ...\" at ARGS:a.",
      "data": "Matched Data: <script> found within ARGS:a: <script>alert(\\x22hello\\x22);</script>",
      "file": "rules/REQUEST-941-APPLICATION-ATTACK-XSS.conf",
      "line": "865"
    }
  }
} 

```

## <a name="application-gateway-waf-sku-pricing"></a>Application Gateway WAF SKU fiyatlandırması

Application Gateway WAF yeni bir altında kullanılabilir bir SKU. Bu SKU yalnızca Azure Resource Manager sağlama modelinde, Klasik dağıtım modelinde kullanılabilir. Ayrıca, WAF SKU sadece orta ve büyük uygulama ağ geçidi örnek boyutlarında gelir. Uygulama ağ geçidine ilişkin tüm sınırlar WAF SKU için de geçerlidir.

Fiyatlandırma, saatlik bir ağ geçidi örneği ücretine ve bir veri işleme ücretine bağlıdır. [Application Gateway fiyatlandırması](https://azure.microsoft.com/pricing/details/application-gateway/) WAF SKU'su, standart SKU ücretlerinden farklıdır. Veri işleme ücretleri aynı olur. Kural başına veya kural grubu ücretlendirme yoktur. Aynı web uygulaması güvenlik duvarı arkasında birden fazla web uygulamasını koruyabilirsiniz. Birden fazla uygulamayı desteklemek için ücretlendirilmezsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [Application Gateway üzerinde web uygulaması güvenlik duvarı yapılandırma](tutorial-restrict-web-traffic-powershell.md).
