---
title: Azure Application Gateway, HTTP üst bilgilerini yeniden yazma
description: Bu makalede Azure Application Gateway, HTTP üst bilgilerini yeniden yazma konusunda Azure PowerShell kullanarak bilgi sağlanır
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 04/12/2019
ms.author: absha
ms.openlocfilehash: 405bc9aed4605e9728e112595f33c879bf55ec7f
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60005630"
---
# <a name="rewrite-http-request-and-response-headers-with-azure-application-gateway---azure-powershell"></a>HTTP istek ve yanıt üst bilgilerini Azure Application Gateway - Azure PowerShell ile yeniden yazma

Bu makalede yapılandırmak için Azure PowerShell kullanmayı gösterir bir [Application Gateway v2 SKU](<https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant>) HTTP üstbilgileri istek ve yanıtların yeniden yazma için.

> [!IMPORTANT]
> Otomatik ölçeklendirme yapan ve alanlar arası yedekli uygulama ağ geçidi SKU'su şu anda genel önizleme aşamasındadır. Bu önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

- Bu öğretici için Azure PowerShell’i yerel olarak çalıştırmanız gerekir. Sonraki bir sürümünün yüklü veya modülü sürüm 1.0.0 Az olmalıdır. Çalıştırma `Import-Module Az` ardından`Get-Module Az` sürümü bulmak için. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps). PowerShell sürümünü doğruladıktan sonra, Azure ile bağlantı oluşturmak için `Login-AzAccount` komutunu çalıştırın.
- Üst bilgiyi yeniden beri özellik SKU v1 SKU için desteklenmiyor bir uygulama ağ geçidi v2 olması gerekir. V2 SKU yoksa, oluşturun bir [Application Gateway v2 SKU](https://docs.microsoft.com/azure/application-gateway/tutorial-autoscale-ps) başlamadan önce.

## <a name="what-is-required-to-rewrite-a-header"></a>Üstbilgi yeniden yazmak için gerekli nedir

HTTP üst bilgisi yeniden yapılandırmak için ihtiyacınız:

1. Http üstbilgileri yeniden yazmak için gereken yeni nesneleri oluşturun:

   - **RequestHeaderConfiguration**: Bu nesne yeniden yazmak için istediğinize isteği üst bilgi alanları ve özgün üstbilgileri için yazılması gereken yeni değeri belirtmek için kullanılır.

   - **ResponseHeaderConfiguration**: Bu nesne yeniden yazmak için istediğinize yanıt üstbilgi alanlarını ve özgün üstbilgileri için yazılması gereken yeni değeri belirtmek için kullanılır.

   - **ActionSet**: Bu nesne, yukarıda belirtilen istek ve yanıt üstbilgileri yapılandırmalarını içerir.

   - **Koşul**: Bu isteğe bağlı bir yapılandırmadır. bir yeniden yazma koşul eklenirse, HTTP (S) istekleri ve yanıtları içeriğini değerlendirir. HTTP (S) istek veya yanıtı yeniden yazma koşulu ile eşleşen olmadığını yeniden yazma koşulu ile ilişkili yeniden yazma eylemi yürütme kararı bağlı olacaktır. 

     Birden fazla koşullar ile ilişkili bir eylem, ardından eylem yalnızca tüm koşulları, yani karşılandığında yürütülecek, mantıksal bir AND işlemi gerçekleştirilir.

   - **RewriteRule**: birden çok yeniden yazma eylemi - yeniden yazma koşul birleşimlerini içerir.

   - **RuleSequence**: Bu isteğe bağlı bir yapılandırmadır. Bu durum, hangi farklı yeniden yazma kuralları yürütülme sırası belirlenmesine yardımcı olur. Bu, bir yeniden yazma kümesinde birden çok yeniden yazma kuralları gerektiğinde yararlıdır. Daha düşük kuralı dizisi değeri ile yeniden yazma kuralı önce yürütülen. İki yeniden yazma kuralları için aynı kural dizisi sağlarsanız, yürütme sırası belirleyici olacaktır.

     RuleSequence açıkça belirtmezseniz, varsayılan değer 100 olarak ayarlanır.

   - **RewriteRuleSet**: Bu nesne bir istek yönlendirme kuralı ile ilişkilendirilecek birden çok yeniden yazma kuralları içerir.

2. Yönlendirme kuralı ile rewriteRuleSet eklemek için gerekli olacaktır. Yeniden yapılandırma yönlendirme kuralı aracılığıyla kaynak dinleyicisine bağlı olmasıdır. Temel bir yönlendirme kuralını kullanırken, üstbilgi yeniden yapılandırma kaynağı dinleyici ile ilişkili ve genel üstbilgi yeniden yazma. Yola dayalı kural kullanıldığında, URL yolu haritada üstbilgi yeniden yapılandırma tanımlanır. Bu nedenle, yalnızca bir sitenin belirli bir yol alanı için geçerlidir.

Birden çok http üst bilgisi yeniden yazma kümeleri oluşturabilir ve her bir yeniden yazma kümesi birden çok dinleyici uygulanabilir. Ancak, bir yeniden yazma yalnızca belirli bir dinleyici kümesine uygulayabilirsiniz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

```azurepowershell
Connect-AzAccount
Select-AzSubscription -Subscription "<sub name>"
```

## <a name="specify-your-http-header-rewrite-rule-configuration"></a>**Http üst bilgisini yeniden yazma kuralı yapılandırmasını belirtin**

Bu örnekte, "azurewebsites.net" başvuru location üst bilgisini içeren her http yanıtında location üst bilgisini yazarak biz yeniden yönlendirme URL'sini değiştirir. Bunu yapmak için konum üstbilgisi yanıt deseni kullanılarak azurewebsites.net içerip içermediğini değerlendirilecek olan koşul ekleyeceğiz `(https?):\/\/.*azurewebsites\.net(.*)$`. Kullanacağız `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` üstbilgi değeri. Bu değiştirecek *azurewebsites.net* ile *contoso.com* konum üst bilgisi içinde.

```azurepowershell
$responseHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "Location" -HeaderValue "{http_resp_Location_1}://contoso.com{http_resp_Location_2}"
$actionSet = New-AzApplicationGatewayRewriteRuleActionSet -RequestHeaderConfiguration $requestHeaderConfiguration -ResponseHeaderConfiguration $responseHeaderConfiguration
$condition = New-AzApplicationGatewayRewriteRuleCondition -Variable "http_resp_Location" -Pattern "(https?):\/\/.*azurewebsites\.net(.*)$" -IgnoreCase
$rewriteRule = New-AzApplicationGatewayRewriteRule -Name LocationHeader -ActionSet $actionSet
$rewriteRuleSet = New-AzApplicationGatewayRewriteRuleSet -Name LocationHeaderRewrite -RewriteRule $rewriteRule
```

## <a name="retrieve-configuration-of-your-existing-application-gateway"></a>Mevcut uygulama ağ geçidi yapılandırmasını alma

```azurepowershell
$appgw = Get-AzApplicationGateway -Name "AutoscalingAppGw" -ResourceGroupName "<rg name>"
```

## <a name="retrieve-configuration-of-your-existing-request-routing-rule"></a>Varolan istek yönlendirme kuralı yapılandırmasını alma

```azurepowershell
$reqRoutingRule = Get-AzApplicationGatewayRequestRoutingRule -Name rule1 -ApplicationGateway $appgw
```

## <a name="update-the-application-gateway-with-the-configuration-for-rewriting-http-headers"></a>Uygulama ağ geçidi http üst bilgilerini yeniden yazma için yapılandırma ile güncelleştir

```azurepowershell
Add-AzApplicationGatewayRewriteRuleSet -ApplicationGateway $appgw -Name LocationHeaderRewrite -RewriteRule $rewriteRuleSet.RewriteRules
Set-AzApplicationGatewayRequestRoutingRule -ApplicationGateway $appgw -Name rule1 -RuleType $reqRoutingRule.RuleType -BackendHttpSettingsId $reqRoutingRule.BackendHttpSettings.Id -HttpListenerId $reqRoutingRule.HttpListener.Id -BackendAddressPoolId $reqRoutingRule.BackendAddressPool.Id -RewriteRuleSetId $rewriteRuleSet.Id
Set-AzApplicationGateway -ApplicationGateway $appgw
```

## <a name="delete-a-rewrite-rule"></a>Bir yeniden yazma kuralını Sil

```azurepowershell
$appgw = Get-AzApplicationGateway -Name "AutoscalingAppGw" -ResourceGroupName "<rg name>"
Remove-AzApplicationGatewayRewriteRuleSet -Name "LocationHeaderRewrite" -ApplicationGateway $appgw
$requestroutingrule= Get-AzApplicationGatewayRequestRoutingRule -Name "rule1" -ApplicationGateway $appgw
$requestroutingrule.RewriteRuleSet= $null
set-AzApplicationGateway -ApplicationGateway $appgw
```

## <a name="next-steps"></a>Sonraki adımlar

Çalışmaları kullanın bazı yaygın gerçekleştirmek için gerekli yapılandırma hakkında daha fazla bilgi edinmek için [ortak üstbilgisi yeniden senaryoları](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers).