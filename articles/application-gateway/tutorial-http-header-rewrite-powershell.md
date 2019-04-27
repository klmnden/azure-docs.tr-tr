---
title: Azure Application Gateway, HTTP üst bilgilerini yeniden yazma
description: Bu makalede Azure Application Gateway oluşturma ve Azure PowerShell kullanarak HTTP üst bilgilerini yeniden yazma hakkında bilgi sağlar.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 12/20/2018
ms.author: absha
ms.openlocfilehash: 4747d824dcf531ed883d476a0daad182ea081c39
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60715105"
---
# <a name="tutorial-create-an-application-gateway-and-rewrite-http-headers"></a>Öğretici: Application gateway oluşturma ve HTTP üst bilgilerini yeniden yazma

Azure PowerShell yapılandırmak için kullanabileceğiniz [HTTP istek ve yanıt üst bilgileri yeniden yazma kuralları](rewrite-http-headers.md) oluşturduğunuzda, yeni [otomatik ölçeklendirme ve bölgesel olarak yedekli application gateway SKU](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant)

> [!IMPORTANT] 
> Otomatik ölçeklendirme yapan ve alanlar arası yedekli uygulama ağ geçidi SKU'su şu anda genel önizleme aşamasındadır. Bu önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Otomatik ölçeklendirme sanal ağ oluşturma
> * Ayrılmış genel IP adresi oluşturma
> * Uygulama ağ geçidi altyapısını kurma
> * Http üst bilgisini yeniden yazma kuralı yapılandırmasını belirtin
> * Otomatik ölçeklendirmeyi belirtme
> * Uygulama ağ geçidi oluşturma
> * Uygulama ağ geçidini test etme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici için Azure PowerShell’i yerel olarak çalıştırmanız gerekir. Sonraki bir sürümünün yüklü veya modülü sürüm 1.0.0 Az olmalıdır. Çalıştırma `Import-Module Az` ardından`Get-Module Az` sürümü bulmak için. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps). PowerShell sürümünü doğruladıktan sonra, Azure ile bağlantı oluşturmak için `Login-AzAccount` komutunu çalıştırın.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

```azurepowershell
Connect-AzAccount
Select-AzSubscription -Subscription "<sub name>"
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Kullanılabilir konumlardan birinde bir kaynak grubu oluşturun.

```azurepowershell
$location = "East US 2"
$rg = "<rg name>"

#Create a new Resource Group
New-AzResourceGroup -Name $rg -Location $location
```

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Otomatik ölçeklendirme uygulama ağ geçidi için ayrılmış bir alt ağ ile sanal ağ oluşturun. Şu anda her ayrılmış alt ağda yalnızca bir otomatik ölçeklendirme yapan uygulama ağ geçidi dağıtılabilir.

```azurepowershell
#Create VNet with two subnets
$sub1 = New-AzVirtualNetworkSubnetConfig -Name "AppGwSubnet" -AddressPrefix "10.0.0.0/24"
$sub2 = New-AzVirtualNetworkSubnetConfig -Name "BackendSubnet" -AddressPrefix "10.0.1.0/24"
$vnet = New-AzvirtualNetwork -Name "AutoscaleVNet" -ResourceGroupName $rg `
       -Location $location -AddressPrefix "10.0.0.0/16" -Subnet $sub1, $sub2
```

## <a name="create-a-reserved-public-ip"></a>Ayrılmış genel IP adresi oluşturma

Publicıpaddress ayırma yöntemini belirtin **statik**. Otomatik ölçeklendirme yapan uygulama ağ geçidi VIP’si yalnızca statik olabilir. Dinamik IP’ler desteklenmez. Yalnızca standart PublicIpAddress SKU’su desteklenir.

```azurepowershell
#Create static public IP
$pip = New-AzPublicIpAddress -ResourceGroupName $rg -name "AppGwVIP" `
       -location $location -AllocationMethod Static -Sku Standard
```

## <a name="retrieve-details"></a>Ayrıntıları alma

Uygulama ağ geçidi için IP yapılandırma ayrıntıları oluşturmak için yerel bir nesne, kaynak grubu, alt ağ ve IP bilgilerini alın.

```azurepowershell
$resourceGroup = Get-AzResourceGroup -Name $rg
$publicip = Get-AzPublicIpAddress -ResourceGroupName $rg -name "AppGwVIP"
$vnet = Get-AzvirtualNetwork -Name "AutoscaleVNet" -ResourceGroupName $rg
$gwSubnet = Get-AzVirtualNetworkSubnetConfig -Name "AppGwSubnet" -VirtualNetwork $vnet
```

## <a name="configure-the-infrastructure"></a>Altyapısını yapılandırma

IP yapılandırması, ön uç IP yapılandırması, arka uç havuzu, HTTP ayarları, sertifika, bağlantı noktası ve dinleyici mevcut standart uygulama ağ geçidi için aynı biçimde yapılandırın. Yeni SKU, standart SKU ile aynı nesne modelini izler.

```azurepowershell
$ipconfig = New-AzApplicationGatewayIPConfiguration -Name "IPConfig" -Subnet $gwSubnet
$fip = New-AzApplicationGatewayFrontendIPConfig -Name "FrontendIPCOnfig" -PublicIPAddress $publicip
$pool = New-AzApplicationGatewayBackendAddressPool -Name "Pool1" `
       -BackendIPAddresses testbackend1.westus.cloudapp.azure.com, testbackend2.westus.cloudapp.azure.com
$fp01 = New-AzApplicationGatewayFrontendPort -Name "HTTPPort" -Port 80

$listener01 = New-AzApplicationGatewayHttpListener -Name "HTTPListener" `
             -Protocol Http -FrontendIPConfiguration $fip -FrontendPort $fp01

$setting = New-AzApplicationGatewayBackendHttpSettings -Name "BackendHttpSetting1" `
          -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

## <a name="specify-your-http-header-rewrite-rule-configuration"></a>HTTP üst bilgisini yeniden yazma kuralı yapılandırmasını belirtin

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

## <a name="specify-the-routing-rule"></a>Yönlendirme kuralını belirtin

İstek yönlendirme kuralı oluşturun. Oluşturulduktan sonra bu yeniden yazma yapılandırma kaynağı dinleyicisi aracılığıyla yönlendirme kuralı eklenir. Temel bir yönlendirme kuralını kullanırken, üstbilgi yeniden yapılandırma kaynağı dinleyici ile ilişkili ve genel üstbilgi yeniden yazma. Yola dayalı kural kullanıldığında, URL yolu haritada üstbilgi yeniden yapılandırma tanımlanır. Bu nedenle, yalnızca bir sitenin belirli bir yol alanı için geçerlidir. Aşağıda, temel bir yönlendirme kuralı oluşturulur ve yeniden yazma kuralı kümesine eklenir.

```azurepowershell
$rule01 = New-AzApplicationGatewayRequestRoutingRule -Name "Rule1" -RuleType basic `
         -BackendHttpSettings $setting -HttpListener $listener01 -BackendAddressPool $pool -RewriteRuleSet $rewriteRuleSet
```

## <a name="specify-autoscale"></a>Otomatik ölçeklendirmeyi belirtme

Artık uygulama ağ geçidi için otomatik ölçeklendirme yapılandırması belirtebilirsiniz. İki otomatik ölçeklendirme yapılandırması türü desteklenir:

* **Sabit kapasite modu**. Bu modda, uygulama ağ geçidi otomatik ölçeklendirme yapmaz ve sabit bir Ölçek Birimi kapasitesinde çalışır.

   ```azurepowershell
   $sku = New-AzApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2 -Capacity 2
   ```

* **Otomatik ölçeklendirme modu**. Bu modda, uygulama ağ geçidi uygulamanın trafik desenine bağlı olarak otomatik ölçeklendirme yapar.

   ```azurepowershell
   $autoscaleConfig = New-AzApplicationGatewayAutoscaleConfiguration -MinCapacity 2
   $sku = New-AzApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2
   ```

## <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

Uygulama ağ geçidi oluşturma ve yedeklilik bölgeler ve otomatik ölçeklendirme yapılandırması içerir.

```azurepowershell
$appgw = New-AzApplicationGateway -Name "AutoscalingAppGw" -Zone 1,2,3 -ResourceGroupName $rg -Location $location -BackendAddressPools $pool -BackendHttpSettingsCollection $setting -GatewayIpConfigurations $ipconfig -FrontendIpConfigurations $fip -FrontendPorts $fp01 -HttpListeners $listener01 -RequestRoutingRules $rule01 -Sku $sku -AutoscaleConfiguration $autoscaleConfig -RewriteRuleSet $rewriteRuleSet
```

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

Uygulama ağ geçidinin genel IP adresini almak için get-AzPublicIPAddress kullanın. Genel IP adresini veya DNS adını kopyalayıp tarayıcınızın adres çubuğuna yapıştırın.

```azurepowershell
Get-AzPublicIPAddress -ResourceGroupName $rg -Name AppGwVIP
```



## <a name="clean-up-resources"></a>Kaynakları temizleme

İlk uygulama ağ geçidi ile oluşturulan kaynakları keşfedin. Ardından, artık ihtiyaç duyulan, kullanabileceğiniz `Remove-AzResourceGroup` komutunu kullanarak kaynak grubunu, uygulama ağ geçidini kaldırmak için ve tüm ilgili kaynakları.

`Remove-AzResourceGroup -Name $rg`

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [URL yolu tabanlı yönlendirme kuralları ile bir uygulama ağ geçidi oluşturma](./tutorial-url-route-powershell.md)
