---
title: Bir web uygulaması güvenlik duvarı oranı sınırı kuralı yapılandırmak için ön kapı - Azure PowerShell
description: Mevcut bir ön uç noktası için oranı sınırı kural yapılandırmayı öğrenin.
services: frontdoor
documentationcenter: ''
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/16/2019
ms.author: kumud;tyao
ms.openlocfilehash: 99b0cab3fd277f90a675f0e6087d572853053a08
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66387343"
---
# <a name="configure-a-web-application-firewall-rate-limit-rule-using-azure-powershell"></a>Azure PowerShell kullanarak web uygulaması güvenlik duvarı oranı sınırı kuralı yapılandırma
Azure web uygulaması Güvenlik Duvarı (WAF) oranı sınırı kuralı için Azure ön kapısı, bir dakikalık süre bir tek istemci IP izin istekleri sayısını denetler.
Bu makale, tek bir istemciden içeren bir web uygulaması için izin verilen istek sayısı denetleyen bir WAF oranı sınırı kural yapılandırma */promo* Azure PowerShell kullanarak URL.

> [!IMPORTANT]
> WAF oranı sınırı kural özelliği için Azure ön kapısı, şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar
Bir hız sınırı İlkesi oluşturmaya başlamadan önce PowerShell ortamınızı ayarlamak ve ön kapısı profili oluşturun.
### <a name="set-up-your-powershell-environment"></a>PowerShell ortamınızı hazırlama
Azure PowerShell, Azure kaynaklarınızı yönetmek için [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) modelini kullanan bir dizi cmdlet sunar. 

[Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)'i yerel makinenize yükleyebilir ve herhangi bir PowerShell oturumunda kullanabilirsiniz. Azure kimlik bilgilerinizle oturum açmak için bu sayfadaki yönergeleri izleyin ve Az PowerShell modülünü yükleyin.

#### <a name="connect-to-azure-with-an-interactive-dialog-for-sign-in"></a>Oturum açma için etkileşimli bir iletişim kutusu ile Azure'a bağlanma
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

## <a name="define-url-match-conditions"></a>URL eşleştirme koşulları tanımlayın
(URL /promo içerir) bir URL eşleşme koşulu tanımla kullanarak [yeni AzFrontDoorWafMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoorwafmatchconditionobject).
Aşağıdaki örnek eşleşir */promo* değeri olarak *RequestUri* değişkeni:

```powershell-interactive
   $promoMatchCondition = New-AzFrontDoorWafMatchConditionObject `
     -MatchVariable RequestUri `
     -OperatorProperty Contains `
     -MatchValue "/promo"
```
## <a name="create-a-custom-rate-limit-rule"></a>Bir özel oranı sınırı kuralı oluşturma
Hızı sınırı kullanılarak ayarlanan [yeni AzFrontDoorWafCustomRuleObject](/powershell/module/az.frontdoor/new-azfrontdoorwafcustomruleobject). Aşağıdaki örnekte, sınırı 1000'e ayarlanır. Herhangi bir istemciden istekleri 1000 aşan bir dakika promosyon sayfasına, sonraki bir dakika başlatana kadar engellenir.

```powershell-interactive
   $promoRateLimitRule = New-AzFrontDoorWafCustomRuleObject `
     -Name "rateLimitRule" `
     -RuleType RateLimitRule `
     -MatchCondition $promoMatchCondition `
     -RateLimitThreshold 1000 `
     -Action Block -Priority 1
```


## <a name="configure-a-security-policy"></a>Güvenlik İlkesi yapılandırma

Ön kapısı profili kullanılarak içeren kaynak grubunun adını bulma `Get-AzureRmResourceGroup`. Ardından, özel oranı sınırı kuralı kullanarak bir güvenlik ilkesi yapılandırma [yeni AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy) ön kapısı profilini içerir belirtilen kaynak grubunda.

Aşağıdaki örnekte kaynak grubu adı kullanan *myResourceGroupFD1* ön kapısı oluşturduğunuz varsayımıyla, sağlanan yönergeleri kullanarak profil [hızlı başlangıç: Bir ön kapı oluşturmak](quickstart-create-front-door.md) makalesi.

 kullanarak [yeni AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy).

```powershell-interactive
   $ratePolicy = New-AzFrontDoorWafPolicy `
     -Name "RateLimitPolicyExamplePS" `
     -resourceGroupName myResourceGroupFD1 `
     -Customrule $promoRateLimitRule `
     -Mode Prevention `
     -EnabledState Enabled
```
## <a name="link-policy-to-a-front-door-front-end-host"></a>Bir ön kapısı ön uç konağa bağlantı İlkesi
Var olan bir ön kapısı ön uç ana bilgisayar Güvenlik İlkesi nesnesini bağlama ve ön kapısı özelliklerini güncelleştirir. Ön kapısı nesnesini kullanarak ilk alınması [Get-AzFrontDoor](/powershell/module/Az.FrontDoor/Get-AzFrontDoor) komutu.
Ardından, ön uç Ayarla *WebApplicationFirewallPolicyLink* özelliğini *ResourceId* "önceki kullanarak adım oluşturulan $ratePolicy", [kümesi AzFrontDoor](/powershell/module/Az.FrontDoor/Set-AzFrontDoor) komutu. 

Aşağıdaki örnekte kaynak grubu adı kullanan *myResourceGroupFD1* ön kapısı oluşturduğunuz varsayımıyla, sağlanan yönergeleri kullanarak profil [hızlı başlangıç: Bir ön kapı oluşturmak](quickstart-create-front-door.md) makalesi. Ayrıca, aşağıdaki örnekte, $frontDoorName ön kapısı profilinizin adı ile değiştirin. 

```powershell-interactive
   $FrontDoorObjectExample = Get-AzFrontDoor `
     -ResourceGroupName myResourceGroupFD1 `
     -Name $frontDoorName
   $FrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $ratePolicy.Id
   Set-AzFrontDoor -InputObject $FrontDoorObjectExample[0]
 ```

> [!NOTE]
> Yalnızca ayarlamanız gerekir *WebApplicationFirewallPolicyLink* ön kapısı ön uç için bir güvenlik ilkesi bağlamak için bir kez özelliği. Sonraki ilke güncelleştirmeleri otomatik olarak ön uç için uygulanır.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [ön kapısı](front-door-overview.md) 


