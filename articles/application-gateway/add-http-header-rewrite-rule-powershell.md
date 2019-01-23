---
title: Var olan Azure Application Gateway, HTTP üst bilgilerini yeniden yazma
description: Bu makalede bilgiler var olan Azure Application Gateway, HTTP üst bilgilerini yeniden yazma konusunda Azure PowerShell kullanarak sağlar
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 12/20/2018
ms.author: absha
ms.openlocfilehash: cb3af5dc8368dc7e598bd0b05653b8ae921a5097
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54452318"
---
# <a name="rewrite-http-headers-in-an-existing-application-gateway"></a>HTTP üstbilgileri mevcut bir Application Gateway'de yeniden yazma

Azure PowerShell yapılandırmak için kullanabileceğiniz [HTTP istek ve yanıt üst bilgileri yeniden yazma kuralları](rewrite-http-headers.md) mevcut [otomatik ölçeklendirme ve bölgesel olarak yedekli application gateway SKU'su](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant)

> [!IMPORTANT] 
> Otomatik ölçeklendirme yapan ve alanlar arası yedekli uygulama ağ geçidi SKU'su şu anda genel önizleme aşamasındadır. Bu önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Mevcut uygulama ağ geçidi yapılandırmasını alma
> * Http üst bilgisini yeniden yazma kuralı yapılandırmasını belirtin
> * Uygulama ağ geçidi http üst bilgilerini yeniden yazma için yukarıdaki yapılandırma ile güncelleştir

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici için Azure PowerShell’i yerel olarak çalıştırmanız gerekir. Sonraki bir sürümünün yüklü veya modülü sürüm 1.0.0 Az olmalıdır. Çalıştırma `Import-Module Az` ardından`Get-Module Az` sürümü bulmak için. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps). PowerShell sürümünü doğruladıktan sonra, Azure ile bağlantı oluşturmak için `Login-AzAccount` komutunu çalıştırın.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

```azurepowershell
Connect-AzAccount
Select-AzSubscription -Subscription "<sub name>"
```

## <a name="specify-your-http-header-rewrite-rule-configuration"></a>**Http üst bilgisini yeniden yazma kuralı yapılandırmasını belirtin**

Http üstbilgileri yeniden yazma için gereken yeni nesneler yapılandırın:

- **RequestHeaderConfiguration**: Bu nesne yeniden yazmak için istediğinize isteği üst bilgi alanları ve özgün üstbilgileri için yazılması gereken yeni değeri belirtmek için kullanılır.
- **ResponseHeaderConfiguration**: Bu nesne yeniden yazmak için istediğinize yanıt üstbilgi alanlarını ve özgün üstbilgileri için yazılması gereken yeni değeri belirtmek için kullanılır.
- **ActionSet**: Bu nesne, yukarıda belirtilen istek ve yanıt üstbilgileri yapılandırmalarını içerir. 
- **RewriteRule**: Bu nesnenin tüm içeren *actionSets* yukarıda belirtilen. 
- **RewriteRuleSet**-tüm bu nesneyi içeren *rewriteRules* ve bir istek yönlendirme kuralı - temel veya yol tabanlı eklenmesi gerekir.

```azurepowershell
$requestHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "X-isThroughProxy" -HeaderValue "True"
$responseHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "Strict-Transport-Security" -HeaderValue "max-age=31536000"
$actionSet = New-AzApplicationGatewayRewriteRuleActionSet -RequestHeaderConfiguration $requestHeaderConfiguration -ResponseHeaderConfiguration $responseHeaderConfiguration    
$rewriteRule = New-AzApplicationGatewayRewriteRule -Name rewriteRule1 -ActionSet $actionSet    
$rewriteRuleSet = New-AzApplicationGatewayRewriteRuleSet -Name rewriteRuleSet1 -RewriteRule $rewriteRule
```

## <a name="retrieve-configuration-of-your-existing-application-gateway"></a>Mevcut uygulama ağ geçidi yapılandırmasını alma

```azurepowershell
$appgw = Get-AzApplicationGateway -Name "AutoscalingAppGw" -ResourceGroupName "<rg name>"
```

## <a name="retrieve-configuration-of-your-existing-request-routing-rule"></a>Varolan istek yönlendirme kuralı yapılandırmasını alma

```azurepowershell
$reqRoutingRule = Get-AzApplicationGatewayRequestRoutingRule -Name Rule1 -ApplicationGateway $appgw
```

## <a name="update-the-application-gateway-with-the-configuration-for-rewriting-http-headers"></a>Uygulama ağ geçidi http üst bilgilerini yeniden yazma için yapılandırma ile güncelleştir

```azurepowershell
Add-AzApplicationGatewayRewriteRuleSet -ApplicationGateway $appgw -Name rewriteRuleSet1 -RewriteRule $rewriteRuleSet.RewriteRules
Set-AzApplicationGatewayRequestRoutingRule -ApplicationGateway $appgw -Name rule1 -RuleType $reqRoutingRule.RuleType -BackendHttpSettingsId $reqRoutingRule.BackendHttpSettings.Id -HttpListenerId $reqRoutingRule.HttpListener.Id -BackendAddressPoolId $reqRoutingRule.BackendAddressPool.Id -RewriteRuleSetId $rewriteRuleSet.Id
Set-AzApplicationGateway -ApplicationGateway $appgw
```

## <a name="delete-a-rewrite-rule"></a>Bir yeniden yazma kuralını Sil

```azurepowershell
$appgw = Get-AzApplicationGateway -Name "AutoscalingAppGw" -ResourceGroupName "<rg name>"
Remove-AzApplicationGatewayRewriteRuleSet -Name "rewriteRuleSet1" -ApplicationGateway $appgw 
$requestroutingrule= Get-AzApplicationGatewayRequestRoutingRule -Name "rule1" -ApplicationGateway $appgw
$requestroutingrule.RewriteRuleSet= $null
set-AzApplicationGateway -ApplicationGateway $appgw 
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [URL yolu tabanlı yönlendirme kuralları ile bir uygulama ağ geçidi oluşturma](./tutorial-url-route-powershell.md)
