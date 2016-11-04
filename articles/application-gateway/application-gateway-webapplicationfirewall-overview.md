---
title: Application Gateway Web Uygulaması Güvenlik Duvarı| Microsoft Docs
description: Bu sayfada Application Gateway Web Uygulaması Güvenlik Duvarı işlevselliğine genel bir bakış sunulmaktadır.
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva

ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/26/2016
ms.author: amsriva

---
# <a name="application-gateway-web-application-firewall-(preview)"></a>Application Gateway Web Uygulaması Güvenlik Duvarı (önizleme)
Web uygulamaları, bilinen yaygın güvenlik açıklarından yararlanan kötü amaçlı saldırıların giderek daha fazla hedefi olmaktadır. Bu açıklardan yararlanma örnekleri arasında SQL ekleme saldırıları, siteler arası komut dosyası saldırıları yaygındır.
Uygulama kodunda bu tür saldırıların önlenmesi zor olabilir ve uygulama topolojisinin birden fazla katmanında ayrıntılı bakım, düzeltme eki uygulama ve izleme işlemleri gerektirebilir. Merkezi bir web saldırısından korunma, güvenlik yönetimini çok daha kolay getirir ve izinsiz girişlere karşı uygulamaya yönelik daha iyi güvence sağlar. Bir WAF çözümü, web uygulamalarının her birinin güvenliğini sağlamaya göre, bilinen bir güvenlik açığına merkezi bir konumda düzeltme eki uygulayarak, güvenlik tehdidine daha hızlı tepki verebilir.

![imageURLroute](./media/application-gateway-webapplicationfirewall-overview/WAF1.png)

Application Gateway bir uygulama teslim denetleyicisi olarak çalışır ve SSL sonlandırma, tanımlama bilgilerine dayalı oturum benzeşimi, hepsini bir kez deneme yük dağıtımı, içerik tabanlı yönlendirme, birden fazla web sitesini barındırma olanağı ve güvenlik geliştirmeleri sunar. Application Gateway tarafından güvenlik geliştirmeleri SSL ilke yönetimi, uçtan uca SSL desteğidir. ADC teklifiyle doğrudan tümleştirilmiş bir WAF (web uygulaması güvenlik duvarı) sunarak hizmetimizin uygulama güvenliği özelliklerini güçlendiriyoruz. Bunun yapılması, merkezi bir konumu web uygulamalarınızı yönetmek ve yaygın web güvenlik açıklarına karşı korumak üzere yapılandırmayı kolaylaştırır.

Uygulama ağ geçidinde WAF’nin yapılandırılması size aşağıdaki avantajları sağlar:

* Arka uç kodunda herhangi bir değişiklik yapmadan web uygulamanızı web güvenlik açıklarından ve saldırılarından koruma.
* Bir uygulama ağ geçidinin arkasında birden fazla web uygulamasını aynı anda koruma. Application Gateway, tek bir ağ geçidinin arkasında tümü web saldırılarına karşı korunabilecek 20’ye kadar web sitesini barındırmayı destekler.
* Uygulama ağ geçidi WAF günlükleri tarafından oluşturulan gerçek zamanlı raporları kullanarak web uygulamanızı saldırılara karşı izleyin.
* Bazı uyumluluk denetimleri, İnternet’e yönelik tüm uç noktaların bir WAF çözümü tarafından korunmasını gerektirir. WAF etkinken uygulama ağ geçidini kullanarak, bu uyumluluk gereksinimlerini karşılayabilirsiniz.

## <a name="overview"></a>Genel Bakış
Application Gateway WAF yeni bir SKU’da (WAF SKU) sunulur ve en yaygın 10 OWASP web güvenlik açığının birçoğuna karşı temel koruma sağlamak üzere ModSecurity ve OWASP Çekirdek Kuralı Kümesi ile önceden yapılandırılmış halde teslim edilir.

* SQL ekleme koruması
* Siteler arası komut dosyası koruması
* Komut ekleme, HTTP isteği kaçakçılığı, HTTP yanıtı bölme ve uzak dosya ekleme saldırıcı gibi Yaygın Web Saldırıları Koruması
* HTTP protokolü ihlallerine karşı koruma
* Eksik konak kullanıcısı-aracısı ve kabul üst bilgileri gibi HTTP protokolü anormalliklerine karşı koruma
* HTTP taşması ve yavaş HTTP DoS önleme gibi HTTP DoS Korumaları
* Robotlar, gezginler ve tarayıcıları önleme
* Yaygın yanlış uygulama yapılandırmalarını (i.e. Apache, IIS, vb.) algılama

## <a name="waf-modes"></a>WAF Modları
Application Gateway WAF, aşağıdaki iki modda çalışacak şekilde yapılandırılabilir:

* **Algılama modu** – Application Gateway WAF algılama modunda çalışacak şekilde yapılandırıldığında tüm tehdit uyarılarını izler ve bir günlük dosyasına kaydeder. Tanılama bölümünden yararlanarak Application Gateway günlük tanılamalarının açık olduğundan emin olmanız gerekir. Ayrıca, WAF günlüğünün seçili ve açık olduğundan emin olmanız gerekir.
* **Önleme modu** – Application Gateway önleme modunda çalışacak şekilde yapılandırıldığında izinsiz girişleri ve kuralları tarafından algılanan saldırıları etkin bir şekilde engeller. Saldırgan bir 403 yetkisiz erişim özel durumu alır ve bağlantı sonlandırılır. Önleme modu bu tür saldırıları WAF günlüklerine kaydetmeye devam eder.

## <a name="application-gateway-waf-reports"></a>Application Gateway WAF raporları
Application Gateway WAF, algıladığı her tehdit için ayrıntılı raporlar sağlar. Günlük kaydı Azure Tanılama Günlükleri ile tümleştirilir ve uyarılar json biçiminde kaydedilir.

![imageURLroute](./media/application-gateway-webapplicationfirewall-overview/waf2.png)

    {
        "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<applicationGatewayName>",
        "operationName": "ApplicationGatewayFirewall",
        "time": "2016-09-20T00:40:04.9138513Z",
        "category": "ApplicationGatewayFirewallLog",
        "properties":     {
            "instanceId":"ApplicationGatewayRole_IN_0",
            "clientIp":"108.41.16.164",
            "clientPort":1815,
            "requestUri":"/wavsep/active/RXSS-Detection-Evaluation-POST/",
            "ruleId":"OWASP_973336",
            "message":"XSS Filter - Category 1: Script Tag Vector",
            "action":"Logged",
            "site":"Global",
            "message":"XSS Filter - Category 1: Script Tag Vector",
            "details":{"message":" Warning. Pattern match "(?i)(<script","file":"/owasp_crs/base_rules/modsecurity_crs_41_xss_attacks.conf","line":"14"}}
    }

## <a name="application-gateway-waf-sku-pricing"></a>Application Gateway WAF SKU fiyatlandırması
Önizleme sırasında Application Gateway WAF kullanımına yönelik ek bir ücret yoktur. Var olan Temel SKU ücretlerini ödemeye devam edersiniz. WAF SKU ücretleri GA zamanında duyurulacaktır. Application Gateway’i WAF SKU’suna dağıtmayı seçen müşteriler için WAF SKU fiyatlandırması yalnızca GA duyurusundan sonra tahakkuk etmeye başlar.

## <a name="next-steps"></a>Sonraki adımlar
WAF özellikleri hakkında daha bilgi edindikten sonra lütfen [Application Gateway üzerinde Web Uygulaması Güvenlik Duvarı’nı yapılandırma](application-gateway-web-application-firewall-portal.md) sayfasını ziyaret edin.

<!--HONumber=Oct16_HO3-->


