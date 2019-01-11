---
title: Öğretici - Azure Front Door Hizmeti için bir etki alanında coğrafi filtreleme yapılandırma | Microsoft Docs
description: Bu öğreticide basit bir coğrafi filtreleme ilkesi oluşturmayı ve bu ilkeyi mevcut Front Door ön uç konağınız ile ilişkilendirmeyi öğreneceksiniz
services: frontdoor
documentationcenter: ''
author: sharad4u
editor: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/20/2018
ms.author: sharadag
ms.openlocfilehash: 68da9a0255cde6cbad5c675901c80193888bf255
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2019
ms.locfileid: "54214887"
---
# <a name="how-to-set-up-a-geo-filtering-policy-for-your-front-door"></a>Front Door hizmetiniz için bir coğrafi filtreleme ilkesi hazırlama
Bu öğreticide, örnek bir coğrafi filtreleme ilkesi oluşturmak ve bu ilkeyi mevcut bir Front Door ön uç konağı ile ilişkilendirmek için Azure PowerShell kullanma gösterilmektedir. Bu örnek coğrafi filtreleme ilkesi, Birleşik Devletler dışındaki diğer tüm ülkelerden gelen istekleri engeller.

## <a name="1-set-up-your-powershell-environment"></a>1. PowerShell ortamınızı hazırlama
Azure PowerShell, Azure kaynaklarınızı yönetmek için [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) modelini kullanan bir dizi cmdlet sunar. 

[Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)'i yerel makinenize yükleyebilir ve herhangi bir PowerShell oturumunda kullanabilirsiniz. Azure kimlik bilgilerinizle oturum açmak için sayfadaki yönergeleri izleyin ve AzureRM'yi yükleyin.
```
# Connect to Azure with an interactive dialog for sign-in
Connect-AzureRmAccount
Install-Module -Name AzureRM
```
> [!NOTE]
>  [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) desteği yakında çıkacaktır.

Front Door modülünü yüklemeden önce geçerli PowerShellGet sürümünün yüklü olduğundan emin olun. Aşağıdaki komutu çalıştırın ve PowerShell'i yeniden açın.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 

AzureRM.FrontDoor modülünü yükleyin. 

```
Install-Module -Name AzureRM.FrontDoor -AllowPrerelease
```

## <a name="2-define-geo-filtering-match-conditions"></a>2. Coğrafi filtreleme eşleşme koşullarını tanımlama
Önce "ABD"'den gelmeyen istekleri seçen örnek bir eşleşme koşulu oluşturun. Bir eşleşme koşulu oluştururken parametreler konusunda PowerShell [kılavuzuna](https://docs.microsoft.com/azure/frontdoor/new-azurermfrontdoormatchconditionobject) başvurun. Ülke eşleme için iki harflik ülke kodu [burada](front-door-geo-filtering.md) sağlanmıştır.

```
$nonUSGeoMatchCondition = New-AzureRmFrontDoorMatchConditionObject -MatchVariable RemoteAddr -OperatorProperty GeoMatch -NegateCondition $true -MatchValue "US"
```
 
## <a name="3-add-geo-filtering-match-condition-to-a-rule-with-action-and-priority"></a>3. Eylem ve Öncelik ile bir kurala coğrafi filtreleme eşleşme koşulu ekleme

Ardından, eşleşme koşuluna, bir Eylem'e ve bir Öncelik'e göre bir CustomRule nesnesi `nonUSBlockRule` oluşturun.  Bir CustomRule'un birden fazla MatchCondition'ı olabilir.  Bu örnekte Eylem Engelle değerine, Öncelik ise en yüksek öncelik olan 1 değerine ayarlanmıştır.

```
$nonUSBlockRule = New-AzureRmFrontDoorCustomRuleObject -Name "geoFilterRule" -RuleType MatchRule -MatchCondition $nonUSGeoMatchCondition -Action Block -Priority 1
```

Bir CustomRuleObject oluştururken parametreler konusunda PowerShell [kılavuzuna](https://docs.microsoft.com/azure/frontdoor/new-azurermfrontdoorcustomruleobject) başvurun.

## <a name="4-add-rules-to-a-policy"></a>4. Bir İlkeye Kurallar ekleme
Bu adımda, belirtilen kaynak grubunda önceki adımdan `nonUSBlockRule` kuralını içeren bir `geoPolicy` ilke nesnesi oluşturulmaktadır. Değişken adı $resourceGroup olan ResourceGroupName değerini bulmak için `Get-AzureRmResourceGroup` kullanın.

```
$geoPolicy = New-AzureRmFrontDoorFireWallPolicy -Name "geoPolicyAllowUSOnly" -resourceGroupName $resourceGroup -Customrule $nonUSBlockRule  -Mode Prevention -EnabledState Enabled
```

Bir ilke oluştururken parametreler konusunda PowerShell [kılavuzuna](https://docs.microsoft.com/azure/frontdoor/new-azurermfrontdoorfirewallpolicy) başvurun.

## <a name="5-link-policy-to-a-front-door-frontend-host"></a>5. İlkeyi bir Front Door ön uç kaynağına bağlama
Son adımlar, koruma ilkesi nesnesini mevcut bir Front Door ön uç konağına bağlamak ve Front Door özelliklerini güncelleştirmektir. Önce [Get-AzureRmFrontDoor](https://docs.microsoft.com/azure/frontdoor/get-azurermfrontdoor) kullanarak Front Door nesnenizi alın, ardından bunun WebApplicationFirewallPolicyLink özelliğini `geoPolicy` ilkesinin resourceId değerine ayarlayın.

```
$geoFrontDoorObjectExample = Get-AzureRmFrontDoor -ResourceGroupName $resourceGroup
$geoFrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $geoPolicy.Id
```

Front Door nesnenizi güncelleştirmek için aşağıdaki [komutu](https://docs.microsoft.com/azure/frontdoor/set-azurermfrontdoor) kullanın.

```
Set-AzureRmFrontDoor -InputObject $geoFrontDoorObjectExample[0]
```

> [!NOTE] 
> WebApplicationFirewallPolicyLink özelliğini, Front Door ön uç konağına bir koruma ilkesini bağlamak için yalnızca bir kez ayarlarsınız. Sonraki ilke güncelleştirmeleri ön uç konağa otomatik olarak uygulanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door ile uygulama katmanı güvenliği](front-door-application-security.md) hakkında bilgi edinin.
- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
