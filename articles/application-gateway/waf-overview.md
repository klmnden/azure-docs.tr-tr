---
title: Azure Application Gateway için web uygulaması Güvenlik Duvarı (WAF) giriş
description: Bu makalede, Application Gateway için web uygulaması Güvenlik Duvarı (WAF) genel bir bakış sağlanmaktadır
services: application-gateway
author: amsriva
ms.service: application-gateway
ms.date: 10/6/2017
ms.author: amsriva
ms.openlocfilehash: a16f8d988c900d015810bfe72b04ff5e9eb0682a
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48815677"
---
# <a name="web-application-firewall-waf"></a>Web uygulaması güvenlik duvarı (WAF)

Web uygulaması güvenlik duvarı (WAF), web uygulamalarınızda açıklardan yararlanmaya ve güvenlik açıklarına karşı merkezi koruma sağlayan bir Application Gateway özelliğidir. 

Web uygulamaları, bilinen yaygın güvenlik açıklarından yararlanan kötü amaçlı saldırıların giderek daha fazla hedefi olmaktadır. Bu açıklardan yararlanma örnekleri arasında SQL ekleme saldırıları, siteler arası komut dosyası saldırıları yaygındır. Uygulama kodunda bu tür saldırıların önlenmesi zor olabilir ve uygulama topolojisinin birden fazla katmanında ayrıntılı bakım, düzeltme eki uygulama ve izleme işlemleri gerektirebilir. Merkezi bir web uygulaması güvenlik duvarı, güvenlik yönetimini çok daha kolay hale getirir ve yetkisiz erişim ya da izinsiz giriş tehditlerine karşı uygulama yöneticilerine daha iyi güvence verir. Bir WAF çözümü, bilinen bir güvenlik açığına merkezi bir konumda düzeltme eki uygulayarak güvenlik tehdidine karşı, web uygulamalarının her birinin güvenliğini sağlamaya göre daha hızlı tepki verebilir. Var olan uygulama ağ geçitleri, web uygulaması güvenlik duvarı bulunan bir uygulama ağ geçidine kolaylıkla dönüştürülebilir.

WAF kurallarını temel [OWASP çekirdek kural kümeleri](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.0 veya 2.2.9'daki. Yeni güvenlik açıklarına karşı koruma, gereken ek bir yapılandırma olmadan içerecek şekilde otomatik olarak güncelleştirir.

![imageURLroute](./media/waf-overview/WAF1.png)

Application Gateway bir uygulama teslim denetleyicisi (ADC) çalışır ve SSL sonlandırma, tanımlama bilgilerine dayalı oturum benzeşimi, hepsini bir kez deneme yük dağıtımı, içerik tabanlı yönlendirme, birden çok Web siteleri ve güvenlik geliştirmeleri barındırma olanağı sunar. Application Gateway tarafından güvenlik geliştirmeleri SSL ilke yönetimi, uçtan uca SSL desteğidir. Uygulama güvenliği, ADC teklifi ile doğrudan tümleştirilen WAF (web uygulaması güvenlik duvarı) ile artık daha güçlüdür. Bunun yapılması, merkezi bir konumu web uygulamalarınızı yönetmek ve yaygın web güvenlik açıklarına karşı korumak üzere yapılandırmayı kolaylaştırır.

## <a name="benefits"></a>Avantajlar

Application Gateway ve web uygulaması güvenlik duvarının sunduğu temel avantajlar şunlardır:

### <a name="protection"></a>Koruma

* Arka uç kodunda herhangi bir değişiklik yapmadan web uygulamanızı web güvenlik açıklarından ve saldırılarından koruma.

* Bir uygulama ağ geçidinin arkasında birden fazla web uygulamasını aynı anda koruma. Application Gateway, tek bir ağ geçidinin arkasında tümü WAF ile web saldırılarına karşı korunabilecek 20’ye kadar web sitesini barındırmayı destekler.

### <a name="monitoring"></a>İzleme

* Gerçek zamanlı bir WAF günlüğü kullanarak web uygulamanızı saldırılara karşı izleyin. Bu günlük, WAF uyarılarını ve günlüklerini takip edip eğilimleri daha kolay izlemek için [Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview.md) ile tümleştirilmiştir.

* WAF yakında Azure Güvenlik Merkezi ile tümleştirilecektir. Azure Güvenlik Merkezi, tüm Azure kaynaklarınızın güvenlik durumuna ilişkin genel bir görünüm sağlar.

### <a name="customization"></a>Özelleştirme

* WAF kurallarını ve kural gruplarını, uygulama gereksinimlerinize göre ve hatalı pozitif sonuçları ortadan kaldıracak şekilde özelleştirme becerisi.

## <a name="features"></a>Özellikler

Web uygulaması güvenlik duvarı, CRS 3.0 ile varsayılan olarak önceden yapılandırılmış halde gelir veya 2.2.9’u kullanmayı seçebilirsiniz. CRS 3.0, 2.2.9 sürümüne göre daha az hatalı pozitif sonuç verir. [Kuralları gereksinimlerinize göre özelleştirme olanağı](application-gateway-customize-waf-rules-portal.md) sağlanır. Web uygulaması güvenlik duvarının koruma sağladığı bazı yaygın web güvenlik açıkları şunlardır:

* SQL ekleme koruması
* Siteler arası komut dosyası koruması
* Komut ekleme, HTTP isteği kaçakçılığı, HTTP yanıtı bölme ve uzak dosya ekleme saldırıcı gibi Yaygın Web Saldırıları Koruması
* HTTP protokolü ihlallerine karşı koruma
* Eksik konak kullanıcısı-aracısı ve kabul üst bilgileri gibi HTTP protokolü anormalliklerine karşı koruma
* Robotlar, gezginler ve tarayıcıları önleme
* Yaygın yanlış uygulama yapılandırmalarını (diğer bir deyişle, Apache, IIS, vb.) algılama

Kurallar ve korumalarını içeren daha ayrıntılı bir liste için aşağıdaki [Çekirdek kural kümeleri](#core-rule-sets) bölümüne bakın.

### <a name="core-rule-sets"></a>Çekirdek kural kümeleri

Application Gateway CRS 3.0 ve CRS 2.2.9 şeklinde iki kural kümesini destekler. Bu çekirdek kural kümeleri, web uygulamalarınızı kötü amaçlı etkinliğe karşı koruyan kurallar içeren koleksiyonlardır.

#### <a name="owasp30"></a>OWASP_3.0

Sağlanan 3.0 çekirdek kural kümesi, aşağıdaki tabloda gösterilen 13 kural grubunu içerir. Bu kural gruplarının her biri devre dışı bırakılabilen birden çok kural içerir.

|RuleGroup|Açıklama|
|---|---|
|**[REQUEST-911-METHOD-ENFORCEMENT](application-gateway-crs-rulegroups-rules.md#crs911)**|Yöntemleri kilitlemeye yönelik kurallar içerir (PUT, PATCH< ..)|
|**[REQUEST-913-SCANNER-DETECTION](application-gateway-crs-rulegroups-rules.md#crs913)**| Bağlantı noktası ve ortam tarayıcılara karşı korumaya yönelik kurallar içerir.|
|**[REQUEST-920-PROTOCOL-ENFORCEMENT](application-gateway-crs-rulegroups-rules.md#crs920)**|Protokol ve kodlama sorunlarına karşı korumaya yönelik kurallar içerir.|
|**[REQUEST-921-PROTOCOL-ATTACK](application-gateway-crs-rulegroups-rules.md#crs921)**|Üst bilgi ekleme, istek kaçakçılığı ve yanıt bölmeye karşı korumaya yönelik kurallar içerir|
|**[REQUEST-930-APPLICATION-ATTACK-LFI](application-gateway-crs-rulegroups-rules.md#crs930)**|Dosya ve yol saldırılarına karşı korumaya yönelik kurallar içerir.|
|**[REQUEST-931-APPLICATION-ATTACK-RFI](application-gateway-crs-rulegroups-rules.md#crs931)**|Uzaktan Dosya Eklemeye (RFI) karşı korumaya yönelik kurallar içerir|
|**[REQUEST-932-APPLICATION-ATTACK-RCE](application-gateway-crs-rulegroups-rules.md#crs932)**|Uzaktan Kod Yürütmeye karşı korumaya yönelik kurallar içerir.|
|**[REQUEST-933-APPLICATION-ATTACK-PHP](application-gateway-crs-rulegroups-rules.md#crs933)**|PHP ekleme saldırılarına karşı korumaya yönelik kurallar içerir.|
|**[REQUEST-941-APPLICATION-ATTACK-XSS](application-gateway-crs-rulegroups-rules.md#crs941)**|Siteler arası betik oluşturmaya karşı korumaya yönelik kurallar içerir.|
|**[REQUEST-942-APPLICATION-ATTACK-SQLI](application-gateway-crs-rulegroups-rules.md#crs942)**|SQL ekleme saldırılarına karşı korumaya yönelik kurallar içerir.|
|**[REQUEST-943-APPLICATION-ATTACK-SESSION-FIXATION](application-gateway-crs-rulegroups-rules.md#crs943)**|Oturum Sabitleme Saldırılarına karşı korumaya yönelik kurallar içerir.|

#### <a name="owasp229"></a>OWASP_2.2.9

Sağlanan 2.2.9 çekirdek kural kümesi, aşağıdaki tabloda gösterilen 10 kural grubunu içerir. Bu kural gruplarının her biri devre dışı bırakılabilen birden çok kural içerir.

|RuleGroup|Açıklama|
|---|---|
|**[crs_20_protocol_violations](application-gateway-crs-rulegroups-rules.md#crs20)**|Protokol ihlallerine (geçersiz karakterler, istek gövdesi ile GET vb.) karşı korumaya yönelik kurallar içerir.|
|**[crs_21_protocol_anomalies](application-gateway-crs-rulegroups-rules.md#crs21)**|Hatalı üst bilgilere karşı korumaya yönelik kurallar içerir.|
|**[crs_23_request_limits](application-gateway-crs-rulegroups-rules.md#crs23)**|Sınırları aşan bağımsız değişken veya dosyalara karşı korumaya yönelik kurallar içerir.|
|**[crs_30_http_policy](application-gateway-crs-rulegroups-rules.md#crs30)**|Kısıtlı yöntemler, üst bilgiler ve dosya türlerine karşı korumaya yönelik kurallar içerir. |
|**[crs_35_bad_robots](application-gateway-crs-rulegroups-rules.md#crs35)**|Web gezginleri ve tarayıcılara karşı korumaya yönelik kurallar içerir.|
|**[crs_40_generic_attacks](application-gateway-crs-rulegroups-rules.md#crs40)**|Genel saldırılara (oturum sabitleme, uzak dosya ekleme, PHP ekleme vb.) karşı korumaya yönelik kurallar içerir|
|**[crs_41_sql_injection_attacks](application-gateway-crs-rulegroups-rules.md#crs41sql)**|SQL ekleme saldırılarına karşı korumaya yönelik kurallar içerir|
|**[crs_41_xss_attacks](application-gateway-crs-rulegroups-rules.md#crs41xss)**|Siteler arası betik oluşturmaya karşı korumaya yönelik kurallar içerir.|
|**[crs_42_tight_security](application-gateway-crs-rulegroups-rules.md#crs42)**|Yol çapraz geçişi saldırılarına karşı korumaya yönelik bir kural içerir|
|**[crs_45_trojans](application-gateway-crs-rulegroups-rules.md#crs45)**|Arka kapı Truva atlarına karşı korumaya yönelik kurallar içerir.|

### <a name="waf-modes"></a>WAF Modları

Application Gateway WAF, aşağıdaki iki modda çalışacak şekilde yapılandırılabilir:

* **Algılama modu** – Application Gateway WAF algılama modunda çalışacak şekilde yapılandırıldığında, izler ve tüm tehdit uyarılarını bir günlük dosyasına kaydeder. **Tanılama** bölümünden yararlanarak Application Gateway günlük tanılamaları açılmalıdır. Ayrıca, WAF günlüğünün seçili ve açık olduğundan emin olmanız gerekir. Algılama modunda çalışırken, web uygulaması güvenlik duvarı gelen istekleri engellemez.
* **Önleme modu** – Application Gateway önleme modunda çalışacak şekilde yapılandırıldığında izinsiz girişleri ve kuralları tarafından algılanan saldırıları etkin bir şekilde engeller. Saldırgan bir 403 yetkisiz erişim özel durumu alır ve bağlantı sonlandırılır. Önleme modu bu tür saldırıları WAF günlüklerine kaydetmeye devam eder.

### <a name="application-gateway-waf-reports"></a>WAF İzleme

Uygulama ağ geçidinizin durumunu izlemek önemlidir. Web uygulaması güvenlik duvarınız ile koruduğu uygulamaların durumu Azure İzleyici, Azure Güvenlik Merkezi (yakında) ve Log Analytics ile günlüğe kaydetme ve tümleştirme işlemleriyle izlenir.

![tanılama](./media/waf-overview/diagnostics.png)

#### <a name="azure-monitor"></a>Azure İzleyici

Her uygulama ağ geçidi günlüğü [Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview.md) ile tümleştirilir.  Bunun yapılması, WAF uyarıları ve günlükleri de dahil olmak üzere tanılama bilgilerini izlemenize olanak tanır.  Bu özellik, portaldaki **Tanılama** sekmesinin Application Gateway kaynağında veya doğrudan Azure İzleyici hizmetinde sağlanır. Uygulama ağ geçidi için tanılama günlüklerini etkinleştirme hakkında daha fazla bilgi için [Application Gateway tanılama](application-gateway-diagnostics.md)

#### <a name="azure-security-center"></a>Azure Güvenlik Merkezi

[Azure Güvenlik Merkezi](../security-center/security-center-intro.md), Azure kaynaklarınızın güvenliğine yönelik artırılmış görünürlük ve denetim yoluyla tehditleri engellemenize, algılamanıza ve yanıtlamanıza yardımcı olur. Application Gateway artık [Azure Güvenlik Merkezi ile tümleşiktir](application-gateway-integration-security-center.md). Azure Güvenlik Merkezi, korumasız web uygulamalarını algılamak için ortamınızı tarar. Artık bu savunmasız kaynakları korumak için Application Gateway WAF'ye önerilerde bulunabilir. Doğrudan Azure Güvenlik Merkezi'nden Application Gateway WAF oluşturabilirsiniz.  Bu WAF örnekleri, Azure Güvenlik Merkezi ile tümleşik olup raporlama için Azure Güvenlik Merkezi'ne uyarılar ve durum bilgileri gönderir.

![Şekil 1](./media/waf-overview/figure1.png)

#### <a name="logging"></a>Günlüğe kaydetme

Application Gateway WAF, algıladığı her tehdit için ayrıntılı raporlar sağlar. Günlük kaydı Azure Tanılama günlükleri ile tümleştirilir ve uyarılar json biçiminde kaydedilir. Bu günlükler [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) ile tümleştirilebilir.

![imageURLroute](./media/waf-overview/waf2.png)

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

Web uygulaması güvenlik duvarı yeni bir WAF SKU altında bulunur. Bu SKU yalnızca Azure Resource Manager sağlama modelinde mevcuttur ve klasik dağıtım modelinde bulunmaz. Ayrıca WAF SKU sadece orta ve büyük uygulama ağ geçidi örnek boyutlarında gelir. Uygulama ağ geçidine ilişkin tüm sınırlar WAF SKU için de geçerlidir. Fiyatlandırma, saatlik ağ geçidi örneği ücretine ve veri işleme ücretine bağlıdır. WAF SKU’su için saatlik ağ geçidi fiyatlandırması, Standart SKU ücretlerinden farklıdır ve [Application Gateway fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/application-gateway/) bölümünde bulunabilir. Veri işleme ücretleri aynı kalır. Kural başına veya kural grubu başına ücret yoktur. Aynı web uygulaması güvenlik duvarının arkasındaki birden fazla web uygulamasını koruyabilirsiniz ve birden fazla uygulamayı desteklemek için ek ücret alınmaz. 

## <a name="next-steps"></a>Sonraki adımlar

WAF hakkında daha fazla edindikten sonra bkz: [Application Gateway üzerinde web uygulaması güvenlik duvarı yapılandırma](tutorial-restrict-web-traffic-powershell.md).

