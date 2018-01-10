---
title: "Azure uygulama ağ geçidi - PowerShell Web uygulaması güvenlik duvarı kurallarında özelleştirme | Microsoft Docs"
description: "Bu makalede PowerShell ile uygulama ağ geçidi web uygulaması güvenlik duvarı kurallarında özelleştirme hakkında bilgi sağlar."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: davidmu
ms.openlocfilehash: 4595864a7bc624375ba2ff6ace09ebae5b0f843a
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2018
---
# <a name="customize-web-application-firewall-rules-through-powershell"></a>Web uygulaması güvenlik duvarı kuralları PowerShell aracılığıyla özelleştirme

> [!div class="op_single_selector"]
> * [Azure portalı](application-gateway-customize-waf-rules-portal.md)
> * [PowerShell](application-gateway-customize-waf-rules-powershell.md)
> * [Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md)

Azure uygulama ağ geçidi web uygulaması Güvenlik Duvarı (WAF) web uygulamaları için koruma sağlar. Bu korumalar açık Web uygulaması güvenlik proje (OWASP) çekirdek kuralı ayarlayın (CR tarafından) sağlanır. Bazı kurallar hatalı pozitif sonuç neden ve gerçek trafiği engelle. Bu nedenle, uygulama ağ geçidi Kural gruplarını ve kurallarını özelleştirme yeteneği sağlar. Özel kural gruplarını ve kurallarını hakkında daha fazla bilgi için bkz: [web uygulaması güvenlik duvarı CRS Kural gruplarını ve kurallarını listesi](application-gateway-crs-rulegroups-rules.md).

## <a name="view-rule-groups-and-rules"></a>Görünüm kural gruplar ve kurallar

Aşağıdaki kod örnekleri, kuralları ve WAF etkin uygulama ağ geçidi üzerinde yapılandırılabilir Kural gruplarını görüntülemek nasıl gösterir.

### <a name="view-rule-groups"></a>Kural grupları görüntüle

Aşağıdaki örnek, kural gruplarını görüntülemek gösterilmektedir:

```powershell
Get-AzureRmApplicationGatewayAvailableWafRuleSets
```

Önceki örnekte kesilmiş yanıttan aşağıdaki çıktısı şöyledir:

```
OWASP (Ver. 3.0):

    REQUEST-910-IP-REPUTATION:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            910011     Rule 910011
            910012     Rule 910012
            ...        ...

    REQUEST-911-METHOD-ENFORCEMENT:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            911011     Rule 911011
            ...        ...

OWASP (Ver. 2.2.9):

    crs_20_protocol_violations:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            960911     Invalid HTTP Request Line
            981227     Apache Error: Invalid URI in Request.
            960000     Attempted multipart/form-data bypass
            ...        ...
```

## <a name="disable-rules"></a>Kuralları devre dışı

Aşağıdaki örnek kuralları devre dışı bırakır `910018` ve `910017` bir uygulama ağ geçidi üzerinde:

```powershell
$disabledrules=New-AzureRmApplicationGatewayFirewallDisabledRuleGroupConfig -RuleGroupName REQUEST-910-IP-REPUTATION -Rules 910018,910017
Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -ApplicationGateway $gw -Enabled $true -FirewallMode Detection -RuleSetVersion 3.0 -RuleSetType OWASP -DisabledRuleGroups $disabledrules
```

## <a name="next-steps"></a>Sonraki adımlar

Devre dışı kurallarınızı yapılandırdıktan sonra WAF günlükleri görüntülemek nasıl öğrenebilirsiniz. Daha fazla bilgi için bkz: [uygulama ağ geçidi tanılama](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
