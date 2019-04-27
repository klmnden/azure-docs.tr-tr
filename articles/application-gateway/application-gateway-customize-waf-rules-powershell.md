---
title: Azure Application Gateway'de - PowerShell Web uygulaması güvenlik duvarı kurallarını özelleştirme
description: Bu makalede, PowerShell ile Application Gateway içindeki web uygulaması güvenlik duvarı kurallarını özelleştirme hakkında bilgi sağlar.
services: application-gateway
author: vhorne
ms.service: application-gateway
origin.date: 02/22/2019
ms.date: 03/11/2019
ms.author: v-junlch
ms.openlocfilehash: f96395a54f66b787878faeee057f02818f956ade
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60831461"
---
# <a name="customize-web-application-firewall-rules-through-powershell"></a>PowerShell aracılığıyla Web uygulaması güvenlik duvarı kurallarını özelleştirme

Azure Application Gateway web uygulaması Güvenlik Duvarı (WAF), web uygulamaları için koruma sağlar. Bu korumalar, açık Web uygulaması güvenlik Project (OWASP) çekirdek kural kümesi (CRS tarafından) sağlanır. Bazı kurallar, hatalı pozitif sonuçları neden ve gerçek trafiği engelleyin. Bu nedenle, uygulama ağ geçidi kural gruplarının ve kuralların özelleştirme yeteneği sağlar. Belirli bir kural gruplarının ve kuralların hakkında daha fazla bilgi için bkz. [web uygulaması güvenlik duvarı CRS kural gruplarının ve kuralların listesi](application-gateway-crs-rulegroups-rules.md).

## <a name="view-rule-groups-and-rules"></a>Görünüm kural gruplarının ve kuralların

Aşağıdaki kod örnekleri, kuralları ve yapılandırılabilir bir WAF özellikli uygulama ağ geçidinde Kural gruplarını görüntüleme gösterilmektedir.

### <a name="view-rule-groups"></a>Kural grupları görüntüleyin

Aşağıdaki örnek, kural gruplarını görüntülemek gösterilmektedir:

```powershell
Get-AzApplicationGatewayAvailableWafRuleSets
```

Aşağıdaki çıktı, önceki örnekte kesilmiş bir yanıt.

```
OWASP (Ver. 3.0):

    General:
        Description:

        Rules:
            RuleId     Description
            ------     -----------
            200004     Possible Multipart Unmatched Boundary.

    REQUEST-911-METHOD-ENFORCEMENT:
        Description:

        Rules:
            RuleId     Description
            ------     -----------
            911011     Rule 911011
            911012     Rule 911012
            911100     Method is not allowed by policy
            911013     Rule 911013
            911014     Rule 911014
            911015     Rule 911015
            911016     Rule 911016
            911017     Rule 911017
            911018     Rule 911018

    REQUEST-913-SCANNER-DETECTION:
        Description:

        Rules:
            RuleId     Description
            ------     -----------
            913011     Rule 913011
            913012     Rule 913012
            913100     Found User-Agent associated with security scanner
            913110     Found request header associated with security scanner
            913120     Found request filename/argument associated with security scanner
            913013     Rule 913013
            913014     Rule 913014
            913101     Found User-Agent associated with scripting/generic HTTP client
            913102     Found User-Agent associated with web crawler/bot
            913015     Rule 913015
            913016     Rule 913016
            913017     Rule 913017
            913018     Rule 913018

            ...        ...
```

## <a name="disable-rules"></a>Kuralları devre dışı bırak

Aşağıdaki örnek, kuralları devre dışı bırakır `911011` ve `911012` bir uygulama ağ geçidi:

```powershell
$disabledrules=New-AzApplicationGatewayFirewallDisabledRuleGroupConfig -RuleGroupName REQUEST-911-METHOD-ENFORCEMENT -Rules 911011,911012
Set-AzApplicationGatewayWebApplicationFirewallConfiguration -ApplicationGateway $gw -Enabled $true -FirewallMode Detection -RuleSetVersion 3.0 -RuleSetType OWASP -DisabledRuleGroups $disabledrules
Set-AzApplicationGateway -ApplicationGateway $gw
```

## <a name="mandatory-rules"></a>Zorunlu kuralları

Aşağıdaki listede, WAF önleme modundayken (özel durumlar olarak oturum açmış algılama modunda) istekte engellemek neden koşulları içerir. Bu yapılandırılmış veya devreden çıkarılamaz:

* Gövde İnceleme açık sürece devre dışı (XML, JSON, form verileri) istek gövdesi ayrıştırılamadı. hata engellenme, istekte sonuçlanır.
* İstek gövdesi (dosya ile) veri uzunluğu yapılandırılan sınırdan daha büyük:
* İstek gövdesi (dosyaları dahil) sınırdan büyük
* WAF altyapısında bir iç hata oluştu

CRS 3.x özel:

* Gelen anomali puanı aşıldı eşiği

## <a name="next-steps"></a>Sonraki adımlar

Devre dışı kurallarınızı yapılandırdıktan sonra WAF günlükleri görüntüleme hakkında bilgi edinebilirsiniz. Daha fazla bilgi için [uygulama ağ geçidi tanılama](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png

<!-- Update_Description: code update -->