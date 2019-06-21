---
title: Azure PowerShell kullanarak Web uygulaması güvenlik duvarı v2 özel kuralları yapılandırma
description: Azure PowerShell kullanarak WAF v2 özel kuralları yapılandırma hakkında bilgi edinin
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 6/18/2019
ms.author: victorh
ms.openlocfilehash: f4d2fd7342e0efe95a1bc69e0dba77692053cf14
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67164743"
---
# <a name="configure-web-application-firewall-v2--with-a-custom-rule-using-azure-powershell"></a>Azure PowerShell kullanarak özel bir kural ile Web uygulaması güvenlik duvarı v2 yapılandırın

<!--- If you make any changes to the PowerShell in this article, also make the change in the corresponding Sample file: azure-docs-powershell-samples/application-gateway/waf-rules/waf-custom-rules.ps1 --->

Özel kurallar, Web uygulaması Güvenlik Duvarı (WAF) v2 geçirir her istek için değerlendirilen kendi kurallar oluşturmanıza olanak sağlar. Bu kurallar, yönetilen kural kümelerini kuralları geri kalanı daha yüksek bir önceliğe basılı tutun. Özel kurallar bir eylem (izin vermek veya engellemek için), bir eşleşme koşulu ve tam özelleştirilmesine imkan tanımak üzere bir işleç var.

Bu makalede, özel bir kural kullanan bir Application Gateway WAF v2 oluşturur. User-Agent istek üst bilgisi içeriyorsa, özel kural bloklarını trafiği *evilbot*.

Özel kural ile ilgili daha fazla örnek görmek için bkz: [özel bir web uygulaması güvenlik duvarı kurallarını oluşturma ve kullanma](create-custom-waf-rules.md)

Bu makalede Azure PowerShell kopyalama, yapıştırma, çalıştırın ve bir sürekli komut dosyasında çalıştırmak istediğiniz olup [Azure uygulama ağ geçidi PowerShell örnekleri](powershell-samples.md).

## <a name="prerequisites"></a>Önkoşullar

### <a name="azure-powershell-module"></a>Azure PowerShell modülü

Azure PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu betik Azure PowerShell modülü sürüm 2.1.0 gerektirir. veya üzeri.

1. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps).
2. Azure ile bağlantı oluşturmak için çalıştırın `Connect-AzAccount`.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="example-script"></a>Örnek betik

### <a name="set-up-variables"></a>Değişkenleri ayarlayın

```azurepowershell
$rgname = "CustomRulesTest"

$location = "East US"

$appgwName = "WAFCustomRules"
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

```azurepowershell
$resourceGroup = New-AzResourceGroup -Name $rgname -Location $location
```

### <a name="create-a-vnet"></a>Sanal ağ oluşturma

```azurepowershell
$sub1 = New-AzVirtualNetworkSubnetConfig -Name "appgwSubnet" -AddressPrefix "10.0.0.0/24"

$sub2 = New-AzVirtualNetworkSubnetConfig -Name "backendSubnet" -AddressPrefix "10.0.1.0/24"

$vnet = New-AzvirtualNetwork -Name "Vnet1" -ResourceGroupName $rgname -Location $location `
  -AddressPrefix "10.0.0.0/16" -Subnet @($sub1, $sub2)
```

### <a name="create-a-static-public-vip"></a>Statik bir genel VIP oluşturma

```azurepowershell
$publicip = New-AzPublicIpAddress -ResourceGroupName $rgname -name "AppGwIP" `
  -location $location -AllocationMethod Static -Sku Standard
```

### <a name="create-pool-and-frontend-port"></a>Havuz ve ön uç bağlantı noktası oluşturma

```azurepowershell
$gwSubnet = Get-AzVirtualNetworkSubnetConfig -Name "appgwSubnet" -VirtualNetwork $vnet

$gipconfig = New-AzApplicationGatewayIPConfiguration -Name "AppGwIpConfig" -Subnet $gwSubnet

$fipconfig01 = New-AzApplicationGatewayFrontendIPConfig -Name "fipconfig" -PublicIPAddress $publicip

$pool = New-AzApplicationGatewayBackendAddressPool -Name "pool1" `
  -BackendIPAddresses testbackend1.westus.cloudapp.azure.com, testbackend2.westus.cloudapp.azure.com

$fp01 = New-AzApplicationGatewayFrontendPort -Name "port1" -Port 80
```

### <a name="create-a-listener-http-setting-rule-and-autoscale"></a>Bir dinleyicisi, http ayarı, kural ve otomatik ölçeklendirme oluşturma

```azurepowershell
$listener01 = New-AzApplicationGatewayHttpListener -Name "listener1" -Protocol Http `
  -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01

$poolSetting01 = New-AzApplicationGatewayBackendHttpSettings -Name "setting1" -Port 80 `
  -Protocol Http -CookieBasedAffinity Disabled

$rule01 = New-AzApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType basic `
  -BackendHttpSettings $poolSetting01 -HttpListener $listener01 -BackendAddressPool $pool

$autoscaleConfig = New-AzApplicationGatewayAutoscaleConfiguration -MinCapacity 3

$sku = New-AzApplicationGatewaySku -Name WAF_v2 -Tier WAF_v2
```

### <a name="create-the-custom-rule-and-apply-it-to-waf-policy"></a>Özel bir kural oluşturun ve WAF ilkesini uygula

```azurepowershell
$variable = New-AzApplicationGatewayFirewallMatchVariable -VariableName RequestHeaders -Selector User-Agent

$condition = New-AzApplicationGatewayFirewallCondition -MatchVariable $variable -Operator Contains -MatchValue "evilbot" -Transform Lowercase -NegationCondition $False  

$rule = New-AzApplicationGatewayFirewallCustomRule -Name blockEvilBot -Priority 2 -RuleType MatchRule -MatchCondition $condition -Action Block

$wafPolicy = New-AzApplicationGatewayFirewallPolicy -Name wafPolicy -ResourceGroup $rgname -Location $location -CustomRule $rule

$wafConfig = New-AzApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

### <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

```azurepowershell
$appgw = New-AzApplicationGateway -Name $appgwName -ResourceGroupName $rgname `
   -Location $location -BackendAddressPools $pool `
   -BackendHttpSettingsCollection  $poolSetting01 `
   -GatewayIpConfigurations $gipconfig -FrontendIpConfigurations $fipconfig01 `
   -FrontendPorts $fp01 -HttpListeners $listener01 `
   -RequestRoutingRules $rule01 -Sku $sku -AutoscaleConfiguration $autoscaleConfig `
   -WebApplicationFirewallConfig $wafConfig `
   -FirewallPolicy $wafPolicy
```

## <a name="next-steps"></a>Sonraki adımlar

[Web uygulaması Güvenlik Duvarı hakkında daha fazla bilgi edinin](waf-overview.md)