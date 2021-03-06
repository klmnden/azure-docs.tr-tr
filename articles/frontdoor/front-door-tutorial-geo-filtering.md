---
title: Öğretici - coğrafi filtrelemeyi yapılandırma web uygulaması güvenlik duvarı ilkesi ön kapısı Azure hizmeti için
description: Bu öğreticide basit bir coğrafi filtreleme ilkesi oluşturmayı ve bu ilkeyi mevcut Front Door ön uç konağınız ile ilişkilendirmeyi öğreneceksiniz
services: frontdoor
documentationcenter: ''
author: KumudD
manager: twooley
editor: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: kumud
ms.reviewer: tyao
ms.openlocfilehash: 97a3d63f94ca7e60f57177703a98b60e3f6c42ec
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67849198"
---
# <a name="how-to-set-up-a-geo-filtering-waf-policy-for-your-front-door"></a>Ön kapısı bir coğrafi filtreleme WAF İlkesi ayarlama
Bu öğreticide, örnek bir coğrafi filtreleme ilkesi oluşturmak ve bu ilkeyi mevcut bir Front Door ön uç konağı ile ilişkilendirmek için Azure PowerShell kullanma gösterilmektedir. Bu örnek coğrafi filtreleme İlkesi, tüm diğer ülkeler/bölgeler dışında Amerika Birleşik Devletleri gelen istekleri engeller.

Azure aboneliğiniz yoksa şimdi [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar
Coğrafi filtreleme İlkesi ayarlama başlamadan önce PowerShell ortamınızı ayarlayın ve bir ön kapısı profili oluşturun.
### <a name="set-up-your-powershell-environment"></a>PowerShell ortamınızı hazırlama
Azure PowerShell, Azure kaynaklarınızı yönetmek için [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) modelini kullanan bir dizi cmdlet sunar. 

[Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)'i yerel makinenize yükleyebilir ve herhangi bir PowerShell oturumunda kullanabilirsiniz. Azure kimlik bilgilerinizle oturum açmak için bu sayfadaki yönergeleri izleyin ve Az PowerShell modülünü yükleyin.

#### <a name="connect-to-azure-with-an-interactive-dialog-for-sign-in"></a>Oturum açma için etkileşimli bir iletişim kutusu ile Azure'a bağlanma
```
Connect-AzAccount
Install-Module -Name Az
```
PowerShellGet yüklü geçerli sürümü olduğundan emin olun. Aşağıdaki komutu çalıştırın ve PowerShell'i yeniden açın.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 
#### <a name="install-azfrontdoor-module"></a>Az.FrontDoor modülünü yükleme 

```
Install-Module -Name Az.FrontDoor
```

### <a name="create-a-front-door-profile"></a>Bir ön kapısı profili oluşturma
Açıklanan yönergeleri izleyerek bir ön kapısı profili oluşturma [hızlı başlangıç: Ön kapısı profil oluşturma](quickstart-create-front-door.md).

## <a name="define-geo-filtering-match-condition"></a>Coğrafi filtreleme tanımla eşleşmesi koşulu

Değil "ABD" kullanarak gelen istekleri seçen bir örnek eşleşme koşulu oluşturma [yeni AzFrontDoorWafMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoorwafmatchconditionobject) eşleşme koşulu oluştururken parametreleri. İki harf ülke kodları ülke eşleme için sağlanan [burada](front-door-geo-filtering.md).

```azurepowershell-interactive
$nonUSGeoMatchCondition = New-AzFrontDoorWafMatchConditionObject `
-MatchVariable RemoteAddr `
-OperatorProperty GeoMatch `
-NegateCondition $true `
-MatchValue "US"
```
 
## <a name="add-geo-filtering-match-condition-to-a-rule-with-action-and-priority"></a>Eylem ve Öncelik ile bir kurala coğrafi filtreleme eşleşme koşulu ekleme

Customrules nesne oluşturma `nonUSBlockRule` eşleşme koşulu, bir eylem ve kullanarak bir önceliğe göre [yeni AzFrontDoorWafCustomRuleObject](/powershell/module/az.frontdoor/new-azfrontdoorwafcustomruleobject).  Bir CustomRule'un birden fazla MatchCondition'ı olabilir.  Bu örnekte Eylem Engelle değerine, Öncelik ise en yüksek öncelik olan 1 değerine ayarlanmıştır.

```
$nonUSBlockRule = New-AzFrontDoorWafCustomRuleObject `
-Name "geoFilterRule" `
-RuleType MatchRule `
-MatchCondition $nonUSGeoMatchCondition `
-Action Block `
-Priority 1
```

## <a name="add-rules-to-a-policy"></a>Bir ilke kuralları ekleme
Ön kapısı profili kullanılarak içeren kaynak grubunun adını bulma `Get-AzResourceGroup`. Ardından, oluşturun bir `geoPolicy` İlkesi nesnesini içeren `nonUSBlockRule` kullanarak [yeni AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy) ön kapısı profilini içerir belirtilen kaynak grubunda. Coğrafi ilkesi için benzersiz bir ad sağlamanız gerekir. 

Aşağıdaki örnekte kaynak grubu adı kullanan *myResourceGroupFD1* ön kapısı oluşturduğunuz varsayımıyla, sağlanan yönergeleri kullanarak profil [hızlı başlangıç: Bir ön kapı oluşturmak](quickstart-create-front-door.md) makalesi. İçinde aşağıdaki örnekte, ilke adını değiştirin *geoPolicyAllowUSOnly* ile benzersiz bir ilke adı.

```
$geoPolicy = New-AzFrontDoorWafPolicy `
-Name "geoPolicyAllowUSOnly" `
-resourceGroupName myResourceGroupFD1 `
-Customrule $nonUSBlockRule  `
-Mode Prevention `
-EnabledState Enabled
```

## <a name="link-waf-policy-to-a-front-door-frontend-host"></a>Bir ön kapısı frontend ana bağlantı WAF İlkesi
Var olan ön kapısı ön uç konağa WAF İlkesi nesnesini bağlama ve ön kapısı özelliklerini güncelleştirir. 

Bunu yapmak için ilk ön kapısı nesnesini kullanarak alınması [Get-AzFrontDoor](/powershell/module/az.frontdoor/get-azfrontdoor). 

```
$geoFrontDoorObjectExample = Get-AzFrontDoor -ResourceGroupName myResourceGroupFD1
$geoFrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $geoPolicy.Id
```

Ardından, ön uç WebApplicationFirewallPolicyLink özelliği ResourceId için ayarlanmış `geoPolicy`kullanarak [kümesi AzFrontDoor](/powershell/module/az.frontdoor/set-azfrontdoor).

```
Set-AzFrontDoor -InputObject $geoFrontDoorObjectExample[0]
```

> [!NOTE] 
> Yalnızca bir WAF ilke ön kapısı frontend ana bilgisayara bağlamak için bir kez WebApplicationFirewallPolicyLink özelliği ayarlayın gerekir. Sonraki ilke güncelleştirmeleri otomatik olarak ön uç konağa uygulanır.

## <a name="next-steps"></a>Sonraki adımlar
- Hakkında bilgi edinin [Azure web uygulaması güvenlik duvarı](waf-overview.md).
- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
