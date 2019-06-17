---
title: Azure Application Gateway için Web uygulaması güvenlik duvarı sorunlarını giderme
description: Bu makalede Azure Application Gateway için Web uygulaması Güvenlik Duvarı (WAF) için sorun giderme bilgileri sağlar.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 6/11/2019
ms.author: ant
ms.topic: conceptual
ms.openlocfilehash: b81ffc5769fa5521094734da5d00aab77aa7cd35
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67082449"
---
# <a name="troubleshoot-web-application-firewall-waf-for-azure-application-gateway"></a>Azure Application Gateway için Web uygulaması Güvenlik Duvarı (WAF) sorunlarını giderme

Web uygulaması Güvenlik Duvarı (WAF) ile geçmelidir istekler engelleniyorsa yapabileceğiniz birkaç şey vardır.

İlk olarak, okuma olun [WAF genel bakış](waf-overview.md) ve [WAF Yapılandırması](application-gateway-waf-configuration.md) belgeleri. Ayrıca, etkin emin [WAF izleme](application-gateway-diagnostics.md) Bu makaleler nasıl WAF, nasıl iş, WAF kural kümesi işlevleri açıklanmaktadır ve WAF günlüklerine erişmek nasıl.

## <a name="understanding-waf-logs"></a>Anlama WAF günlükleri

WAF günlükleri amacı, eşleşen veya WAF tarafından engellenen her istek göstermektir. Bu, bir kayıt defteri eşleşen ya da engellenen değerlendirilen tüm isteklerin olur. WAF (hatalı pozitif) karakteri bir istek engeller görürseniz, birkaç şey yapabilirsiniz. İlk olarak daraltmak ve belirli bir istek bulun. İsteğin belirli URI, zaman damgası veya işlem Kimliğini bulmak için günlükler arayın. İlişkili günlük girişlerini bulduğunuzda, hatalı pozitif sonuçları üzerinde işlem yapmaya başlayabilirsiniz.

Örneğin, dize içeren bir yasal trafiği sahip düşünelim *1 = 1* WAF geçirmek istediğiniz. İstek çalışırsanız, WAF içeren trafiği engeller, *1 = 1* herhangi bir parametre veya alan dize. Bu, genellikle SQL ekleme saldırısına ile ilişkilendirilmiş bir dizedir. Günlükleri Ara ve istek engellendi/eşleşen kuralları ve zaman damgası bakın.

Aşağıdaki örnekte, dört kural (TransactionID alanı kullanarak) aynı istek sırasında tetiklenen görebilirsiniz. Eşleşen kullanıcı sayısal/IP adresi kullanıldığından ilk bildiren bir uyarı olduğundan artıran anomali puanı üç istek için URL. Eşleşen bir sonraki 942130, aradığınız biri olduğu kuralıdır. Gördüğünüz *1 = 1* içinde `details.data` alan. Bir uyarı da olduğu gibi bu ek anomali puanı üç tarafından yeniden artırır. Genellikle, eyleminin her kural **eşleşen** arttıkça anomali puan ve bu noktada anomali puanı altı olacaktır. Daha fazla bilgi için [Anomali Puanlama modu](waf-overview.md#anomaly-scoring-mode).

Son iki günlük girişlerini anomali puanı yeterince yüksek olduğu için istek engellendi gösterir. Bu girişlerin diğer iki değerinden farklı bir eylem yoktur. Bunlar Göster bunlar gerçekten *engellenen* istek. Bu kurallar zorunludur ve devre dışı bırakılamaz. Bunlar kuralları, ancak daha fazla WAF iç işlevleri, çekirdek altyapının olarak zorlayıcı olabilir olmamalıdır.

```json
{ 
    "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2", 
    "operationName": "ApplicationGatewayFirewall", 
    "category": "ApplicationGatewayFirewallLog", 
    "properties": { 
        "instanceId": "appgw_3", 
        "clientIp": "167.220.2.139", 
        "clientPort": "", 
        "requestUri": "\/", 
        "ruleSetType": "OWASP_CRS", 
        "ruleSetVersion": "3.0.0", 
        "ruleId": "920350", 
        "message": "Host header is a numeric IP address", 
        "action": "Matched", 
        "site": "Global", 
        "details": { 
            "message": "Warning. Pattern match \\\"^[\\\\\\\\d.:]+$\\\" at REQUEST_HEADERS:Host. ", 
            "data": "40.90.218.160", 
            "file": "rules\/REQUEST-920-PROTOCOL-ENFORCEMENT.conf\\\"", 
            "line": "791" 
        }, 
        "hostname": "vm000003", 
        "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt" 
    } 
} 
{ 
    "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2", 
    "operationName": "ApplicationGatewayFirewall", 
    "category": "ApplicationGatewayFirewallLog", 
    "properties": { 
        "instanceId": "appgw_3", 
        "clientIp": "167.220.2.139", 
        "clientPort": "", 
        "requestUri": "\/", 
        "ruleSetType": "OWASP_CRS", 
        "ruleSetVersion": "3.0.0", 
        "ruleId": "942130", 
        "message": "SQL Injection Attack: SQL Tautology Detected.", 
        "action": "Matched", 
        "site": "Global", 
        "details": { 
            "message": "Warning. Pattern match \\\"(?i:([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)([\\\\\\\\d\\\\\\\\w]++)([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)(?:(?:=|\\u003c=\\u003e|r?like|sounds\\\\\\\\s+like|regexp)([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)\\\\\\\\2|(?:!=|\\u003c=|\\u003e=|\\u003c\\u003e|\\u003c|\\u003e|\\\\\\\\^|is\\\\\\\\s+not|not\\\\\\\\s+like|not\\\\\\\\s+regexp)([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)(?!\\\\\\\\2)([\\\\\\\\d\\\\\\\\w]+)))\\\" at ARGS:text1. ", 
            "data": "Matched Data: 1=1 found within ARGS:text1: 1=1", 
            "file": "rules\/REQUEST-942-APPLICATION-ATTACK-SQLI.conf\\\"", 
            "line": "554" 
        }, 
        "hostname": "vm000003", 
        "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt" 
    } 
} 
{ 
    "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2", 
    "operationName": "ApplicationGatewayFirewall", 
    "category": "ApplicationGatewayFirewallLog", 
    "properties": { 
        "instanceId": "appgw_3", 
        "clientIp": "167.220.2.139", 
        "clientPort": "", 
        "requestUri": "\/", 
        "ruleSetType": "", 
        "ruleSetVersion": "", 
        "ruleId": "0", 
        "message": "Mandatory rule. Cannot be disabled. Inbound Anomaly Score Exceeded (Total Score: 8)", 
        "action": "Blocked", 
        "site": "Global", 
        "details": { 
            "message": "Access denied with code 403 (phase 2). Operator GE matched 5 at TX:anomaly_score. ", 
            "data": "", 
            "file": "rules\/REQUEST-949-BLOCKING-EVALUATION.conf\\\"", 
            "line": "57" 
        }, 
        "hostname": "vm000003", 
        "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt" 
    } 
} 
{ 
    "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2", 
    "operationName": "ApplicationGatewayFirewall", 
    "category": "ApplicationGatewayFirewallLog", 
    "properties": { 
        "instanceId": "appgw_3", 
        "clientIp": "167.220.2.139", 
        "clientPort": "", 
        "requestUri": "\/", 
        "ruleSetType": "", 
        "ruleSetVersion": "", 
        "ruleId": "0", 
        "message": "Mandatory rule. Cannot be disabled. Inbound Anomaly Score Exceeded (Total Inbound Score: 8 - SQLI=5,XSS=0,RFI=0,LFI=0,RCE=0,PHPI=0,HTTP=0,SESS=0): SQL Injection Attack: SQL Tautology Detected.", 
        "action": "Blocked", 
        "site": "Global", 
        "details": { 
            "message": "Warning. Operator GE matched 5 at TX:inbound_anomaly_score. ", 
            "data": "", 
            "file": "rules\/RESPONSE-980-CORRELATION.conf\\\"", 
            "line": "73" 
        }, 
        "hostname": "vm000003", 
        "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt" 
    }
}
```

## <a name="fixing-false-positives"></a>Hatalı pozitif sonuçları düzeltme

Bu bilgiler ve 942130 olan eşleşen bir kural bilgi ile *1 = 1* dize, bu trafiğinizi paylaşacağından durdurmak için birkaç şey yapabilir:

- Bir dışlama listesi kullanın

   Bkz: [WAF Yapılandırması](application-gateway-waf-configuration.md#waf-exclusion-lists) dışlama hakkında daha fazla bilgi için listeler.
- Kuralı devre dışı bırakın.

### <a name="using-an-exclusion-list"></a>Bir dışlama listesi kullanma

Yanlış bir işleme hakkında bilinçli bir karar pozitif yapmak için uygulamanızın kullandığı teknolojileriyle tanımak önemlidir. Örneğin, teknoloji yığınınızın bir SQL server değildir ve bu kurallar için ilgili hatalı pozitif sonuçları elde edeceğinizi varsayalım. Bu kuralları devre dışı bırakma güvenlik gerekmeyen zayıflatabilir değil.

Bir dışlama listesi kullanmanın bir avantajı, bir isteğin yalnızca belirli bir bölümünü devre dışı bırakılıyor, ' dir. Ancak, bu belirli bir dışlama, genel bir ayar olduğundan WAF geçen tüm trafiğin için uygulanabilir olduğu anlamına gelir. Örneğin, bu bir soruna, yol açabilecek *1 = 1* geçerli bir istek gövdesindeki belirli bir uygulama için ancak diğerleri için değil. Gövde, üstbilgileri ve tanımlama bilgileri arasında belirli bir koşul karşılanırsa, tüm istek hariç aksine hariç tutulacak seçebileceğiniz, başka bir avantajdır.

Bazen, belirli parametreleri sezgisel olmayabilir bir şekilde WAF içine burada geçen durumlar vardır. Örneğin, Azure Active Directory kullanarak kimlik doğrulaması yapılırken iletilir bir belirteç yoktur. Bu belirteç *__RequestVerificationToken*, genellikle istek tanımlama bilgisi geçirilen. Ancak, tanımlama bilgilerini devre dışı olduğu bazı durumlarda, bu belirteç de istek özniteliği veya "değişken" olarak geçirilir. Böyle bir durumda olmasını sağlamak gereken *__RequestVerificationToken* dışlama listesine eklenen bir **isteği öznitelik adı** de.

![İstisnalar](media/waf-tshoot/exclusion-list.png)

Bu örnekte, dışlamak istediğiniz **isteği öznitelik adı** eşit *text1*. Güvenlik duvarı günlükleri öznitelik adı görebileceğiniz için bu açıktır: **veri: Eşleşen veri: 1 = 1 ARGS:text1 içinde bulunamadı: 1=1**. Öznitelik **text1**. Ayrıca bu öznitelik adı diğer birkaç bulabilirsiniz yollar bkz [öznitelik adları isteği bulma](#finding-request-attribute-names).

![WAF hariç tutma listeleri](media/waf-tshoot/waf-config.png)

### <a name="disabling-rules"></a>Kuralları devre dışı bırakma

Bir hatalı pozitif almak için başka bir giriş WAF düşünce üzerinde eşleşen kural devre dışı bırakmak için kötü amaçlı yoludur. WAF günlükleri ayrıştırılır ve kural 942130 aşağı daraltıldığı olduğundan, bunu Azure portalında devre dışı bırakabilirsiniz. Bkz: [Azure portalı üzerinden web uygulaması güvenlik duvarı kurallarını özelleştirme](application-gateway-customize-waf-rules-portal.md).

Bir kural devre dışı bırakma yararlarından biri, normalde engellenecek belirli bir koşulu içeren tüm trafiği geçerli trafiğidir biliyorsanız, bu kural tüm WAF için devre dışı olur. Ancak, yalnızca belirli bir kullanım örneği'nde geçerli trafiği durumda, genel bir ayar olduğundan bu kural tüm WAF için devre dışı bırakarak bir güvenlik açığı açın.

Azure PowerShell kullanmak istiyorsanız, bkz. [PowerShell aracılığıyla web uygulaması güvenlik duvarı kurallarını özelleştirme](application-gateway-customize-waf-rules-powershell.md). Azure CLI kullanmak istiyorsanız, bkz. [Azure CLI aracılığıyla web uygulaması güvenlik duvarı kurallarını özelleştirme](application-gateway-customize-waf-rules-cli.md).

![WAF kurallarını](media/waf-tshoot/waf-rules.png)

## <a name="finding-request-attribute-names"></a>Bulma isteği öznitelik adları

Yardım [Fiddler](https://www.telerik.com/fiddler), istekleri ayrı ayrı inceleyin ve bir web sayfasının hangi belirli alanları olarak adlandırılan belirleyin. Bu Dışlama listeleri kullanarak İnceleme belirli alanları dışlanacak yardımcı olabilir.

Bu örnekte, gördüğünüz gibi alanın burada *1 = 1* dize girilen çağrılır **text1**.

![Fiddler](media/waf-tshoot/fiddler-1.png)

Bu, hariç tutabilirsiniz bir alandır. Hariç tutma listeleri hakkında daha fazla bilgi için bkz: [Web uygulaması güvenlik duvarı istek boyutu sınırları ve hariç tutma listeleri](application-gateway-waf-configuration.md#waf-exclusion-lists). Bu durumda aşağıdaki dışlama yapılandırarak değerlendirme dışlayabilirsiniz:

![WAF dışlama](media/waf-tshoot/waf-exclusion-02.png)

Ayrıca, dışlama listesine eklemeniz gereken görmek için bilgi almak için güvenlik duvarı günlüklerini inceleyebilirsiniz. Günlük kaydını etkinleştirmek için bkz: [arka uç sistem durumu, tanılama günlükleri ve ölçümler için Application Gateway](application-gateway-diagnostics.md).

Güvenlik Duvarı günlüğünü inceleyin ve incelemek istediğiniz isteğin gerçekleştiği saati için PT1H.json dosyasına görüntüleyin.

Bu örnekte, aynı TransactionID ile dört kural vardır ve hepsi tam aynı anda ortaya çıktığını görebilirsiniz:

```json
-   {
-       "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2",
-       "operationName": "ApplicationGatewayFirewall",
-       "category": "ApplicationGatewayFirewallLog",
-       "properties": {
-           "instanceId": "appgw_3",
-           "clientIp": "167.220.2.139",
-           "clientPort": "",
-           "requestUri": "\/",
-           "ruleSetType": "OWASP_CRS",
-           "ruleSetVersion": "3.0.0",
-           "ruleId": "920350",
-           "message": "Host header is a numeric IP address",
-           "action": "Matched",
-           "site": "Global",
-           "details": {
-               "message": "Warning. Pattern match \\\"^[\\\\\\\\d.:]+$\\\" at REQUEST_HEADERS:Host. ",
-               "data": "40.90.218.160",
-               "file": "rules\/REQUEST-920-PROTOCOL-ENFORCEMENT.conf\\\"",
-               "line": "791"
-           },
-           "hostname": "vm000003",
-           "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt"
-       }
-   }
-   {
-       "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2",
-       "operationName": "ApplicationGatewayFirewall",
-       "category": "ApplicationGatewayFirewallLog",
-       "properties": {
-           "instanceId": "appgw_3",
-           "clientIp": "167.220.2.139",
-           "clientPort": "",
-           "requestUri": "\/",
-           "ruleSetType": "OWASP_CRS",
-           "ruleSetVersion": "3.0.0",
-           "ruleId": "942130",
-           "message": "SQL Injection Attack: SQL Tautology Detected.",
-           "action": "Matched",
-           "site": "Global",
-           "details": {
-               "message": "Warning. Pattern match \\\"(?i:([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)([\\\\\\\\d\\\\\\\\w]++)([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)(?:(?:=|\\u003c=\\u003e|r?like|sounds\\\\\\\\s+like|regexp)([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)\\\\\\\\2|(?:!=|\\u003c=|\\u003e=|\\u003c\\u003e|\\u003c|\\u003e|\\\\\\\\^|is\\\\\\\\s+not|not\\\\\\\\s+like|not\\\\\\\\s+regexp)([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)(?!\\\\\\\\2)([\\\\\\\\d\\\\\\\\w]+)))\\\" at ARGS:text1. ",
-               "data": "Matched Data: 1=1 found within ARGS:text1: 1=1",
-               "file": "rules\/REQUEST-942-APPLICATION-ATTACK-SQLI.conf\\\"",
-               "line": "554"
-           },
-           "hostname": "vm000003",
-           "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt"
-       }
-   }
-   {
-       "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2",
-       "operationName": "ApplicationGatewayFirewall",
-       "category": "ApplicationGatewayFirewallLog",
-       "properties": {
-           "instanceId": "appgw_3",
-           "clientIp": "167.220.2.139",
-           "clientPort": "",
-           "requestUri": "\/",
-           "ruleSetType": "",
-           "ruleSetVersion": "",
-           "ruleId": "0",
-           "message": "Mandatory rule. Cannot be disabled. Inbound Anomaly Score Exceeded (Total Score: 8)",
-           "action": "Blocked",
-           "site": "Global",
-           "details": {
-               "message": "Access denied with code 403 (phase 2). Operator GE matched 5 at TX:anomaly_score. ",
-               "data": "",
-               "file": "rules\/REQUEST-949-BLOCKING-EVALUATION.conf\\\"",
-               "line": "57"
-           },
-           "hostname": "vm000003",
-           "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt"
-       }
-   }
-   {
-       "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2",
-       "operationName": "ApplicationGatewayFirewall",
-       "category": "ApplicationGatewayFirewallLog",
-       "properties": {
-           "instanceId": "appgw_3",
-           "clientIp": "167.220.2.139",
-           "clientPort": "",
-           "requestUri": "\/",
-           "ruleSetType": "",
-           "ruleSetVersion": "",
-           "ruleId": "0",
-           "message": "Mandatory rule. Cannot be disabled. Inbound Anomaly Score Exceeded (Total Inbound Score: 8 - SQLI=5,XSS=0,RFI=0,LFI=0,RCE=0,PHPI=0,HTTP=0,SESS=0): SQL Injection Attack: SQL Tautology Detected.",
-           "action": "Blocked",
-           "site": "Global",
-           "details": {
-               "message": "Warning. Operator GE matched 5 at TX:inbound_anomaly_score. ",
-               "data": "",
-               "file": "rules\/RESPONSE-980-CORRELATION.conf\\\"",
-               "line": "73"
-           },
-           "hostname": "vm000003",
-           "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt"
-       }
-   }
```

Bilginizi nasıl CRS kural iş ayarlar ve CRS kural kümesi 3.0 Puanlama sistemi bir anomali ile çalışır (bkz [Azure Application Gateway Web uygulaması güvenlik duvarı](waf-overview.md)) iki alt ile kurallar bildiğiniz  **Eylem: Engellenen** özelliği engelleme toplam anomali puanına göre. Odaklanmak için üstteki iki kurallardır.

Kullanıcı, bu durumda yoksayılabilir Application Gateway için gitmek için sayısal bir IP adresi kullanıldığından ilk giriş günlüğe kaydedilir.

İkinci (kuralı 942130) biridir ilgi çekici bir. (1 = 1) bir desenle eşleşen ve bu alan adlı ayrıntıları gördüğünüz **text1**. Dışlanacak aynı önceki adımları izleyerek **istek öznitelik adı** , **eşittir** **1 = 1**.

## <a name="finding-request-header-names"></a>Bulma isteği üst bilgi adları

Fiddler isteği üst bilgi adları yeniden bulmak için kullanışlı bir araçtır. Aşağıdaki ekran görüntüsünde, içeren üst bilgiler bu GET isteği için gördüğünüz *Content-Type*, *User-Agent*ve benzeri.

![Fiddler](media/waf-tshoot/fiddler-2.png)

İstek ve yanıt üst bilgileri görüntülemek için başka bir yolu, Chrome, geliştirici araçları içinde aramaktır. F12 tuşuna basarak veya sağ tıklayarak -> **inceleyin** -> **Geliştirici Araçları**seçip **ağ** sekmesi. Bir web sayfası yükleme ve incelemek istediğiniz isteği'ni tıklatın.

![Chrome F12](media/waf-overview/chrome-f12.png)

## <a name="finding-request-cookie-names"></a>Bulma isteği tanımlama bilgisi adı

İstek tanımlama bilgisi içeriyorsa **tanımlama bilgilerini** Fiddler'da görüntülemek için sekmesinde seçilebilir.

## <a name="restrict-global-parameters-to-eliminate-false-positives"></a>Hatalı pozitif sonuçları ortadan kaldırmak için genel parametre kısıtlama

- İstek gövdesi denetimi devre dışı bırak

   Ayarlayarak **inceleyin istek gövdesi** kapalı olarak istek gövdeleri tüm trafiği WAF tarafından değerlendirilmez. Bu, istek gövdesi, uygulamanız için kötü amaçlı olmayan bildiğiniz durumlarda yararlı olabilir.

   Bu seçenek devre dışı bırakarak yalnızca istek gövdesi inceledi değil. Dışlama listesi işlevselliğini kullanarak tek tek olanlar hariç tutulmadığı sürece üstbilgileri ve tanımlama bilgileri incelenen, kalır.

- Dosya boyutu sınırları

   Dosya boyutu WAF'yi sınırlayarak, web sunucularınızın'olmuyor saldırı olma olasılığını sınırlama. Büyük dosyaları karşıya yüklenmesine izin vererek dolmasını arka ucunuzu riskini artırır. Uygulamanız için bir normal kullanım durumu için dosya boyutu sınırlama saldırıları önlemek için başka bir yoludur.

   > [!NOTE]
   > Uygulamanızı herhangi bir dosyayı karşıya yükleme hiçbir zaman gerekir biliyorsanız, belirli bir boyut, bir sınır ayarlayarak sınırlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [Application Gateway üzerinde web uygulaması güvenlik duvarı yapılandırma](tutorial-restrict-web-traffic-powershell.md).
