---
title: Özel kuralları ve varsayılan Ruse kümesi için ön kapı - Azure PowerShell ile bir web uygulaması Güvenlik Duvarı (WAF) ilkesi yapılandırma
description: Bir WAF yapılandırma konusunda bilgi hem özel hem de yönetilen kuralları için mevcut bir ön uç noktası İlkesi oluşur.
services: frontdoor
documentationcenter: ''
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/08/2019
ms.author: kumud;tyao
ms.openlocfilehash: 414869833b894e2688505a91fed8fafe0c912b73
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65523731"
---
# <a name="configure-a-web-application-firewall-policy-using-azure-powershell"></a>Azure PowerShell kullanarak bir web uygulaması güvenlik duvarı ilkesi yapılandırma
Azure web uygulaması Güvenlik Duvarı (WAF) ilkesi ön Kapıda bir istek ulaştığında gerekli incelemeleri tanımlar.
Bu makale, bazı özel kurallar ve Azure tarafından yönetilen varsayılan Ruse etkin kümesi ile oluşan bir WAF ilkesini yapılandırma.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar
Bir hız sınırı İlkesi oluşturmaya başlamadan önce PowerShell ortamınızı ayarlamak ve ön kapısı profili oluşturun.
### <a name="set-up-your-powershell-environment"></a>PowerShell ortamınızı hazırlama
Azure PowerShell, Azure kaynaklarınızı yönetmek için [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) modelini kullanan bir dizi cmdlet sunar. 

[Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)'i yerel makinenize yükleyebilir ve herhangi bir PowerShell oturumunda kullanabilirsiniz. Azure kimlik bilgilerinizle oturum açmak için bu sayfadaki yönergeleri izleyin ve Az PowerShell modülünü yükleyin.

#### <a name="sign-in-to-azure"></a>Oturum açın: Azure
```
Connect-AzAccount

```
Front Door modülünü yüklemeden önce geçerli PowerShellGet sürümünün yüklü olduğundan emin olun. Aşağıdaki komutu çalıştırın ve PowerShell'i yeniden açın.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 

#### <a name="install-azfrontdoor-module"></a>Az.FrontDoor modülünü yükleme 

```
Install-Module -Name Az.FrontDoor
```
### <a name="create-a-front-door-profile"></a>Bir ön kapısı profili oluşturma
Açıklanan yönergeleri izleyerek bir ön kapısı profili oluşturma [hızlı başlangıç: Bir ön kapısı profili oluşturma](quickstart-create-front-door.md)

## <a name="custom-rule-based-on-http-parameters"></a>HTTP parametrelerine göre bir özel kural

Aşağıdaki örnek, bir özel kural kullanarak iki eşleştirme koşulları ile yapılandırma işlemi gösterilmektedir [yeni AzFrontDoorWafMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoorwafmatchconditionobject). İstekleri belirli bir siteden başvuran tarafından tanımlanır ve sorgu dizesi "password" içermiyor. 

```powershell-interactive
$referer = New-AzFrontDoorWafMatchConditionObject -MatchVariable RequestHeader -OperatorProperty Equal -Selector "Referer" -MatchValue "www.mytrustedsites.com/referpage.html"
$password = New-AzFrontDoorWafMatchConditionObject -MatchVariable QueryString -OperatorProperty Contains -MatchValue "password"
$AllowFromTrustedSites = New-AzFrontDoorCustomRuleObject -Name "AllowFromTrustedSites" -RuleType MatchRule -MatchCondition $referer,$password -Action Allow -Priority 1
```

## <a name="custom-rule-based-on-http-request-method"></a>HTTP istek yöntemine dayalı özel kural
"PUT" yöntemiyle engelleyen bir kural oluşturmak [yeni AzFrontDoorCustomRuleObject](/powershell/module/az.frontdoor/new-azfrontdoorwafcustomruleobject) gibi:

```powershell-interactive
$put = New-AzFrontDoorWafMatchConditionObject -MatchVariable RequestMethod -OperatorProperty Equal -MatchValue PUT
$BlockPUT = New-AzFrontDoorCustomRuleObject -Name "BlockPUT" -RuleType MatchRule -MatchCondition $put -Action Block -Priority 2
```

## <a name="create-a-custom-rule-based-on-size-constraint"></a>Boyut sınırlaması özel bir kural oluşturun

Aşağıdaki örnek, Azure PowerShell kullanarak 100 karakterden uzun URL'siyle istekleri engelleyen bir kural oluşturur:
```powershell-interactive
$url = New-AzFrontDoorWafMatchConditionObject -MatchVariable RequestUri -OperatorProperty GreaterThanOrEqual -MatchValue 100
$URLOver100 = New-AzFrontDoorCustomRuleObject -Name "URLOver100" -RuleType MatchRule -MatchCondition $url -Action Block -Priority 3
```
## <a name="add-managed-default-rule-set"></a>Yönetilen varsayılan kural kümesi Ekle

Aşağıdaki örnek, bir yönetilen varsayılan kural Azure PowerShell kullanarak kümesi oluşturur:
```powershell-interactive
$managedRules = New-AzFrontDoorManagedRuleObject -Type DefaultRuleSet -Version "preview-0.1"
```
## <a name="configure-a-security-policy"></a>Güvenlik İlkesi yapılandırma

Ön kapısı profili kullanılarak içeren kaynak grubunun adını bulma `Get-AzResourceGroup`. Ardından, bir güvenlik ilkesi kullanarak önceki adımda oluşturulan kurallarını yapılandırın [yeni AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy) ön kapısı profilini içerir belirtilen kaynak grubunda.

```powershell-interactive
$myWAFPolicy=New-AzFrontDoorWafPolicy -Name $policyName -ResourceGroupName $resourceGroupName -Customrule $AllowFromTrustedSites,$BlockPUT,$URLOver100 -ManagedRule $managedRules -EnabledState Enabled -Mode Prevention
```

## <a name="link-policy-to-a-front-door-front-end-host"></a>Bir ön kapısı ön uç konağa bağlantı İlkesi
Var olan bir ön kapısı ön uç ana bilgisayar Güvenlik İlkesi nesnesini bağlama ve ön kapısı özelliklerini güncelleştirir. İlk olarak, ön kapısı nesnesini kullanarak almak [Get-AzFrontDoor](/powershell/module/Az.FrontDoor/Get-AzFrontDoor).
Ardından, ön uç Ayarla *WebApplicationFirewallPolicyLink* özelliğini *ResourceId* önceki kullanarak adım oluşturulan "$" $myWAFPolicy [Set-AzFrontDoor](/powershell/module/Az.FrontDoor/Set-AzFrontDoor). 

Aşağıdaki örnekte kaynak grubu adı kullanan *myResourceGroupFD1* ön kapısı oluşturduğunuz varsayımıyla, sağlanan yönergeleri kullanarak profil [hızlı başlangıç: Bir ön kapı oluşturmak](quickstart-create-front-door.md) makalesi. Ayrıca, aşağıdaki örnekte, $frontDoorName ön kapısı profilinizin adı ile değiştirin. 

```powershell-interactive
   $FrontDoorObjectExample = Get-AzFrontDoor `
     -ResourceGroupName myResourceGroupFD1 `
     -Name $frontDoorName
   $FrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $myWAFPolicy.Id
   Set-AzFrontDoor -InputObject $FrontDoorObjectExample[0]
 ```

> [!NOTE]
> Yalnızca ayarlamanız gerekir *WebApplicationFirewallPolicyLink* ön kapısı ön uç için bir güvenlik ilkesi bağlamak için bir kez özelliği. Sonraki ilke güncelleştirmeleri otomatik olarak ön uç için uygulanır.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [ön kapısı](front-door-overview.md) 
- Daha fazla bilgi edinin [WAF için ön kapısı](waf-overview.md)
