---
title: Azure Application Gateway, HTTP üst bilgilerini yeniden yazma
description: Bu makale, Azure PowerShell kullanarak Azure Application Gateway, HTTP üst bilgilerini yeniden yazma konusunda bilgi sağlar.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 04/12/2019
ms.author: absha
ms.openlocfilehash: 47fe6a5247622e3ad3b3720955068580e0329913
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64947198"
---
# <a name="rewrite-http-request-and-response-headers-with-azure-application-gateway---azure-powershell"></a>HTTP istek ve yanıt üst bilgilerini Azure Application Gateway - Azure PowerShell ile yeniden yazma

Bu makalede Azure PowerShell kullanarak yapılandırmak nasıl bir [Application Gateway v2 SKU](<https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant>) yeniden yazma isteklerini ve yanıtlarını HTTP üstbilgileri için örneği.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

- Yerel olarak bu makaledeki adımları tamamlamak için Azure PowerShell'i çalıştırmanız gerekir. Sonraki bir sürümünün yüklü veya modülü sürüm 1.0.0 Az olması gerekir. Çalıştırma `Import-Module Az` ardından `Get-Module Az` yüklü olan sürümü belirlenemedi. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps). PowerShell sürümünü doğruladıktan sonra, Azure ile bağlantı oluşturmak için `Login-AzAccount` komutunu çalıştırın.
- Application Gateway v2 SKU örneği olması gerekir. Üst bilgileri yeniden yazma v1 SKU'da desteklenmiyor. V2 SKU yoksa, oluşturun bir [Application Gateway v2 SKU](https://docs.microsoft.com/azure/application-gateway/tutorial-autoscale-ps) başlamadan önce örneği.

## <a name="create-required-objects"></a>Gerekli nesnelerden oluşturma

HTTP üst bilgisi yeniden yapılandırmak için bu adımları tamamlamak gerekir.

1. HTTP üst bilgisi yeniden yazmak için gerekli olan nesneleri oluşturun:

   - **RequestHeaderConfiguration**: Yeniden yazmak için istediğinize isteği üst bilgi alanları ve üst bilgileri için yeni değeri belirtmek için kullanılır.

   - **ResponseHeaderConfiguration**: Yeniden yazmak için istediğinize yanıt üstbilgi alanları ve üst bilgileri için yeni değeri belirtmek için kullanılır.

   - **ActionSet**: Daha önce belirtilen istek ve yanıt üstbilgileri yapılandırmalarını içerir.

   - **Koşul**: İsteğe bağlı yapılandırma. HTTP (S) istekleri ve yanıtları içeriğini yeniden yazma koşulları değerlendirin. HTTP (S) istek veya yanıtı yeniden yazma koşulu ile eşleşirse yeniden yazma eylemi meydana gelir.

     Birden fazla koşulu bir eylem ile ilişkilendirirseniz, yalnızca tüm koşulları sağlandığında eylem gerçekleşir. Diğer bir deyişle, bir mantıksal AND işlemi işlemdir.

   - **RewriteRule**: Birden çok yeniden yazma eylemi içeriyor / koşul birleşimleri yeniden yazın.

   - **RuleSequence**: Bir sırayı yeniden yazma kuralları belirlemeye yardımcı olur bir isteğe bağlı yapılandırma yürütün. Bir yeniden yazma kümesinde birden çok yeniden yazma kuralları varsa bu yapılandırma yararlıdır. Bir alt kural sırası değerine sahip bir yeniden yazma kuralı ilk çalıştırır. Yürütme sırası, iki yeniden yazma kuralları için aynı kural dizisi değeri atarsanız, belirleyici değildir.

     RuleSequence açıkça belirtmezseniz, varsayılan değer 100 olarak ayarlanır.

   - **RewriteRuleSet**: İstek yönlendirme kuralı ile ilişkilendirilecek birden çok yeniden yazma kuralları içerir.

2. RewriteRuleSet yönlendirme kural ekleyin. Yeniden yapılandırma, kaynak dinleyicisi aracılığıyla yönlendirme kuralı eklenir. Temel yönlendirme kuralı kullandığınızda, üstbilgi yeniden yapılandırma kaynağı dinleyici ile ilişkili ise genel üstbilgi yeniden yazma. Yola dayalı kural kullandığınızda, URL yolu haritada üstbilgi yeniden yapılandırma tanımlanır. Bu durumda, yalnızca bir sitenin belirli yolu alanını geçerlidir.

Birden çok HTTP üst bilgisi yeniden yazma kümeleri oluşturabilir ve birden çok dinleyici ayarlamak her yeniden uygulayın. Ancak bir yeniden yazma yalnızca belirli bir dinleyici kümesine uygulayabilirsiniz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

```azurepowershell
Connect-AzAccount
Select-AzSubscription -Subscription "<sub name>"
```

## <a name="specify-the-http-header-rewrite-rule-configuration"></a>HTTP üst bilgisi yeniden yazma kuralı yapılandırmasını belirtin

Bu örnekte biz bir yeniden yönlendirme URL'si HTTP yanıtında location üst bilgisini azurewebsites.net başvuru location üst bilgisini içeren her yazarak değiştireceksiniz. Bunu yapmak için azurewebsites.net location üst bilgisini yanıt içerip içermediğini değerlendirilecek olan koşul ekleyeceğiz. Desen kullanacağız `(https?):\/\/.*azurewebsites\.net(.*)$`. Ve kullanacağız `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` üstbilgi değeri. Bu değer değiştirecek *azurewebsites.net* ile *contoso.com* konum üst bilgisi içinde.

```azurepowershell
$responseHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "Location" -HeaderValue "{http_resp_Location_1}://contoso.com{http_resp_Location_2}"
$actionSet = New-AzApplicationGatewayRewriteRuleActionSet -RequestHeaderConfiguration $requestHeaderConfiguration -ResponseHeaderConfiguration $responseHeaderConfiguration
$condition = New-AzApplicationGatewayRewriteRuleCondition -Variable "http_resp_Location" -Pattern "(https?):\/\/.*azurewebsites\.net(.*)$" -IgnoreCase
$rewriteRule = New-AzApplicationGatewayRewriteRule -Name LocationHeader -ActionSet $actionSet
$rewriteRuleSet = New-AzApplicationGatewayRewriteRuleSet -Name LocationHeaderRewrite -RewriteRule $rewriteRule
```

## <a name="retrieve-the-configuration-of-your-application-gateway"></a>Application gateway'iniz yapılandırmasını alma

```azurepowershell
$appgw = Get-AzApplicationGateway -Name "AutoscalingAppGw" -ResourceGroupName "<rg name>"
```

## <a name="retrieve-the-configuration-of-your-request-routing-rule"></a>İstek yönlendirme kuralınızı yapılandırmasını alma

```azurepowershell
$reqRoutingRule = Get-AzApplicationGatewayRequestRoutingRule -Name rule1 -ApplicationGateway $appgw
```

## <a name="update-the-application-gateway-with-the-configuration-for-rewriting-http-headers"></a>Uygulama ağ geçidi HTTP üst bilgilerini yeniden yazma için yapılandırma ile güncelleştir

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

Bazı ortak kullanım durumları ayarlama hakkında daha fazla bilgi için bkz. [ortak üstbilgisi yeniden senaryoları](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers).