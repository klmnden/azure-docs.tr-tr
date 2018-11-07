---
title: Azure Application Gateway'de - PowerShell Web uygulaması güvenlik duvarı kurallarını özelleştirme | Microsoft Docs
description: Bu makalede, PowerShell ile Application Gateway içindeki web uygulaması güvenlik duvarı kurallarını özelleştirme hakkında bilgi sağlar.
documentationcenter: na
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: ''
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: victorh
ms.openlocfilehash: dfcd82a17a399f213f5c4e32326a8995d26e8458
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51218278"
---
# <a name="customize-web-application-firewall-rules-through-powershell"></a>PowerShell aracılığıyla Web uygulaması güvenlik duvarı kurallarını özelleştirme

> [!div class="op_single_selector"]
> * [Azure portal](application-gateway-customize-waf-rules-portal.md)
> * [PowerShell](application-gateway-customize-waf-rules-powershell.md)
> * [Azure CLI](application-gateway-customize-waf-rules-cli.md)

Azure Application Gateway web uygulaması Güvenlik Duvarı (WAF), web uygulamaları için koruma sağlar. Bu korumalar, açık Web uygulaması güvenlik Project (OWASP) çekirdek kural kümesi (CRS tarafından) sağlanır. Bazı kurallar, hatalı pozitif sonuçları neden ve gerçek trafiği engelleyin. Bu nedenle, uygulama ağ geçidi kural gruplarının ve kuralların özelleştirme yeteneği sağlar. Belirli bir kural gruplarının ve kuralların hakkında daha fazla bilgi için bkz. [web uygulaması güvenlik duvarı CRS kural gruplarının ve kuralların listesi](application-gateway-crs-rulegroups-rules.md).

## <a name="view-rule-groups-and-rules"></a>Görünüm kural gruplarının ve kuralların

Aşağıdaki kod örnekleri, kuralları ve yapılandırılabilir bir WAF özellikli uygulama ağ geçidinde Kural gruplarını görüntüleme gösterilmektedir.

### <a name="view-rule-groups"></a>Kural grupları görüntüleyin

Aşağıdaki örnek, kural gruplarını görüntülemek gösterilmektedir:

```powershell
Get-AzureRmApplicationGatewayAvailableWafRuleSets
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
$disabledrules=New-AzureRmApplicationGatewayFirewallDisabledRuleGroupConfig -RuleGroupName REQUEST-911-METHOD-ENFORCEMENT -Rules 911011,911012
Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -ApplicationGateway $gw -Enabled $true -FirewallMode Detection -RuleSetVersion 3.0 -RuleSetType OWASP -DisabledRuleGroups $disabledrules
Set-AzureRmApplicationGateway -ApplicationGateway $gw
```

## <a name="next-steps"></a>Sonraki adımlar

Devre dışı kurallarınızı yapılandırdıktan sonra WAF günlükleri görüntüleme hakkında bilgi edinebilirsiniz. Daha fazla bilgi için [uygulama ağ geçidi tanılama](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
