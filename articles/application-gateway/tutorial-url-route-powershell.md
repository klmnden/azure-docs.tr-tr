---
title: URL'ye göre web trafiğini yönlendirme - Azure PowerShell
description: Azure PowerShell kullanarak sunucuların belirli ölçeklenebilir havuzlarının URL'sine göre web trafiğini yönlendirmeyi öğrenin.
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: tutorial
ms.workload: infrastructure-services
ms.date: 10/25/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: c636ab9956b369702c8319d67a83e33070113857
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66729498"
---
# <a name="route-web-traffic-based-on-the-url-using-azure-powershell"></a>Azure PowerShell kullanarak URL'ye göre web trafiğini yönlendirme

Uygulamanıza erişmek için kullanılan URL’ye bağlı olarak belirli ölçeklenebilir sunucu havuzlarına yönlendirilecek web trafiğini yapılandırmak için Azure PowerShell’i kullanabilirsiniz. Bu öğreticide [Sanal Makine Ölçek Kümeleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) kullanan üç arka uç havuzuna sahip bir [Azure Application Gateway](application-gateway-introduction.md) oluşturursunuz. Arka uç havuzlarının her biri ortak veriler, görüntüler ve video gibi belirli bir amaca hizmet eder.  Trafiği farklı havuzlara ayırmak, müşterilerinizin ihtiyaç duydukları bilgileri ihtiyaç duydukları zaman edinmesini sağlar.

Trafik yönlendirmeyi etkinleştirmek için web trafiğinin havuzlardaki uygun sunuculara ulaşmasını sağlayacak belirli bağlantı noktalarını dinleyen dinleyicilere atanmış [yönlendirme kuralları](application-gateway-url-route-overview.md) oluşturursunuz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Ağı ayarlama
> * Dinleyicileri, URL yol haritasını ve kuralları oluşturma
> * Ölçeklenebilir arka uç havuzları oluşturma

![URL yönlendirme örneği](./media/tutorial-url-route-powershell/scenario.png)

Tercih ederseniz, [Azure CLI](tutorial-url-route-cli.md) veya [Azure portal](create-url-route-portal.md) kullanarak bu öğreticiyi tamamlayabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz Bu öğretici Azure PowerShell modülü sürüm 1.0.0 gerektirir veya üzeri. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzAccount` komutunu da çalıştırmanız gerekir.

Kaynak oluşturmak için gereken süre nedeniyle bu öğreticiyi tamamlamak 90 dakikaya kadar sürebilir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Uygulamanıza yönelik tüm kaynakları içeren bir kaynak grubu oluşturmanız gerekir. 

Kullanarak bir Azure kaynak grubu oluşturmanız [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup).  

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroupAG -Location eastus
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma

Mevcut bir sanal ağı kullanmanıza veya yeni bir tane oluşturmanıza bakılmaksızın, yalnızca uygulama ağ geçitleri için kullanılan bir alt ağ içerdiğinden emin olmanız gerekir. Bu öğreticide, uygulama ağ geçidi için bir alt ağ ve ölçek kümeleri için bir alt ağ oluşturacaksınız. Uygulama ağ geçidindeki kaynaklara erişimi etkinleştirmek için bir genel IP adresi oluşturun.

Alt ağ yapılandırmalarını oluşturma *myAGSubnet* ve *myBackendSubnet* kullanarak [yeni AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig). Adlı sanal ağ oluşturma *myVNet* kullanarak [yeni AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) alt ağ yapılandırmalarını ile. Ve son olarak, adlı ortak IP adresi oluşturma *myAGPublicIPAddress* kullanarak [yeni AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress). Bu kaynaklar, uygulama ağ geçidi ve ilişkili kaynakları ile ağ bağlantısı sağlamak için kullanılır.

```azurepowershell-interactive
$backendSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.1.0/24

$agSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.2.0/24

$vnet = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $backendSubnetConfig, $agSubnetConfig
$pip = New-AzPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Bu bölümde, uygulama ağ geçidini destekleyen kaynakları ve son olarak uygulama ağ geçidini oluşturacaksınız. Şu kaynakları oluşturacaksınız:

- *IP yapılandırmaları ve ön uç bağlantı noktası* - Daha önce oluşturduğunuz alt ağı uygulama ağ geçidi ile ilişkilendirir ve ona erişmek için kullanılacak bir bağlantı noktası atar.
- *Varsayılan havuz* - Tüm uygulama ağ geçitlerinde en az bir sunucu arka uç havuzu olmalıdır.
- *Varsayılan dinleyici ve kural* - Varsayılan dinleyici, atanan bağlantı noktası üzerindeki trafiği dinler, varsayılan kural ise trafiği varsayılan havuza gönderir.

### <a name="create-the-ip-configurations-and-frontend-port"></a>IP yapılandırmaları ve ön uç bağlantı noktası oluşturma

İlişkilendirme *myAGSubnet* uygulama ağ geçidi kullanarak daha önce oluşturduğunuz [yeni AzApplicationGatewayIPConfiguration](/powershell/module/az.network/new-azapplicationgatewayipconfiguration). Ata *myAGPublicIPAddress* kullanarak uygulama ağ geçidi [yeni AzApplicationGatewayFrontendIPConfig](/powershell/module/az.network/new-azapplicationgatewayfrontendipconfig).

```azurepowershell-interactive
$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet

$subnet=$vnet.Subnets[0]

$pip = Get-AzPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Name myAGPublicIPAddress

$gipconfig = New-AzApplicationGatewayIPConfiguration `
  -Name myAGIPConfig `
  -Subnet $subnet

$fipconfig = New-AzApplicationGatewayFrontendIPConfig `
  -Name myAGFrontendIPConfig `
  -PublicIPAddress $pip

$frontendport = New-AzApplicationGatewayFrontendPort `
  -Name myFrontendPort `
  -Port 80
```

### <a name="create-the-default-pool-and-settings"></a>Varsayılan havuz ve ayarlar oluşturma

Adlı varsayılan arka uç havuzu oluşturun *appGatewayBackendPool* kullanarak uygulama ağ geçidi için [yeni AzApplicationGatewayBackendAddressPool](/powershell/module/az.network/new-azapplicationgatewaybackendaddresspool). Kullanarak arka uç havuzu için ayarları yapılandırma [yeni AzApplicationGatewayBackendHttpSettings](/powershell/module/az.network/new-azapplicationgatewaybackendhttpsetting).

```azurepowershell-interactive
$defaultPool = New-AzApplicationGatewayBackendAddressPool `
  -Name appGatewayBackendPool

$poolSettings = New-AzApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-default-listener-and-rule"></a>Varsayılan dinleyici ve kural oluşturma

Uygulama ağ geçidinin trafiği arka uç havuzuna uygun şekilde yönlendirmesini sağlamak için bir dinleyici gereklidir. Bu öğreticide, iki dinleyici oluşturacaksınız. Oluşturduğunuz ilk temel dinleyici, kök URL'deki trafiği dinler. Oluşturduğunuz ikinci dinleyici ise belirli URL'lerdeki trafiği dinler.

Adlı varsayılan dinleyiciyi oluşturun *myDefaultListener* kullanarak [yeni AzApplicationGatewayHttpListener](/powershell/module/az.network/new-azapplicationgatewayhttplistener) daha önce oluşturduğunuz ön uç bağlantı noktasını ve ön uç yapılandırması. 

Dinleyicinin gelen trafik için kullanacağı arka uç havuzunu bilmesi için bir kural gerekir. Adlı bir temel kuralı oluşturun *bağlanma1* kullanarak [yeni AzApplicationGatewayRequestRoutingRule](/powershell/module/az.network/new-azapplicationgatewayrequestroutingrule).

```azurepowershell-interactive
$defaultlistener = New-AzApplicationGatewayHttpListener `
  -Name myDefaultListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport

$frontendRule = New-AzApplicationGatewayRequestRoutingRule `
  -Name rule1 `
  -RuleType Basic `
  -HttpListener $defaultlistener `
  -BackendAddressPool $defaultPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

Gerekli destekleyici kaynakları oluşturduğunuz, adlı uygulama ağ geçidi için parametreleri belirtin *myAppGateway* kullanarak [yeni AzApplicationGatewaySku](/powershell/module/az.network/new-azapplicationgatewaysku)ve ardından kullanarakoluşturun[AzApplicationGateway yeni](/powershell/module/az.network/new-azapplicationgateway).

```azurepowershell-interactive
$sku = New-AzApplicationGatewaySku `
  -Name Standard_Medium `
  -Tier Standard `
  -Capacity 2

$appgw = New-AzApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -BackendAddressPools $defaultPool `
  -BackendHttpSettingsCollection $poolSettings `
  -FrontendIpConfigurations $fipconfig `
  -GatewayIpConfigurations $gipconfig `
  -FrontendPorts $frontendport `
  -HttpListeners $defaultlistener `
  -RequestRoutingRules $frontendRule `
  -Sku $sku
```

Uygulama ağ geçidinin oluşturulması 30 dakikaya kadar sürebilir. Sonraki bölüme geçmeden önce dağıtımın başarıyla tamamlanmasını bekleyin. Öğreticinin bu noktasında, 80 numaralı bağlantı noktasındaki trafiği dinleyen ve bu trafiği varsayılan sunucu havuzuna gönderen bir uygulama ağ geçidiniz vardır.

### <a name="add-image-and-video-backend-pools-and-port"></a>Görüntü ve video arka uç havuzlarını ve bağlantı noktasını ekleme

Adlı arka uç havuzları ekleme *imagesBackendPool* ve *videoBackendPool* application gateway'iniz için[Ekle AzApplicationGatewayBackendAddressPool](/powershell/module/az.network/add-azapplicationgatewaybackendaddresspool). Ön uç bağlantı noktasını kullanarak havuzları ekleme [Ekle AzApplicationGatewayFrontendPort](/powershell/module/az.network/add-azapplicationgatewayfrontendport). Uygulama ağ geçidi kullanarak değişiklikleri gönderme [kümesi AzApplicationGateway](/powershell/module/az.network/set-azapplicationgateway).

```azurepowershell-interactive
$appgw = Get-AzApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway

Add-AzApplicationGatewayBackendAddressPool `
  -ApplicationGateway $appgw `
  -Name imagesBackendPool

Add-AzApplicationGatewayBackendAddressPool `
  -ApplicationGateway $appgw `
  -Name videoBackendPool

Add-AzApplicationGatewayFrontendPort `
  -ApplicationGateway $appgw `
  -Name bport `
  -Port 8080

Set-AzApplicationGateway -ApplicationGateway $appgw
```

Uygulama ağ geçidinin güncelleştirilmesi 20 dakikaya kadar sürebilir.

### <a name="add-backend-listener"></a>Arka uç dinleyicisi ekleme

Adlı arka uç dinleyici Ekle *backendListener* kullanarak trafiği yönlendirmek gerekli [Ekle AzApplicationGatewayHttpListener](/powershell/module/az.network/add-azapplicationgatewayhttplistener).

```azurepowershell-interactive
$appgw = Get-AzApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway

$backendPort = Get-AzApplicationGatewayFrontendPort `
  -ApplicationGateway $appgw `
  -Name bport

$fipconfig = Get-AzApplicationGatewayFrontendIPConfig `
  -ApplicationGateway $appgw

Add-AzApplicationGatewayHttpListener `
  -ApplicationGateway $appgw `
  -Name backendListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $backendPort

Set-AzApplicationGateway -ApplicationGateway $appgw
```

### <a name="add-url-path-map"></a>URL yol eşlemesini ekleme

URL yol eşlemeleri, uygulamanıza gönderilen URL’lerin belirli arka uç havuzlarına yönlendirilmesini sağlar. URL oluşturmak adlı yolu eşlemeleri *imagePathRule* ve *videoPathRule* kullanarak [yeni AzApplicationGatewayPathRuleConfig](/powershell/module/az.network/new-azapplicationgatewaypathruleconfig) ve [ Ekleme AzApplicationGatewayUrlPathMapConfig](/powershell/module/az.network/add-azapplicationgatewayurlpathmapconfig).

```azurepowershell-interactive
$appgw = Get-AzApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway

$poolSettings = Get-AzApplicationGatewayBackendHttpSettings `
  -ApplicationGateway $appgw `
  -Name myPoolSettings

$imagePool = Get-AzApplicationGatewayBackendAddressPool `
  -ApplicationGateway $appgw `
  -Name imagesBackendPool

$videoPool = Get-AzApplicationGatewayBackendAddressPool `
  -ApplicationGateway $appgw `
  -Name videoBackendPool

$defaultPool = Get-AzApplicationGatewayBackendAddressPool `
  -ApplicationGateway $appgw `
  -Name appGatewayBackendPool

$imagePathRule = New-AzApplicationGatewayPathRuleConfig `
  -Name imagePathRule `
  -Paths "/images/*" `
  -BackendAddressPool $imagePool `
  -BackendHttpSettings $poolSettings

$videoPathRule = New-AzApplicationGatewayPathRuleConfig `
  -Name videoPathRule `
    -Paths "/video/*" `
    -BackendAddressPool $videoPool `
    -BackendHttpSettings $poolSettings

Add-AzApplicationGatewayUrlPathMapConfig `
  -ApplicationGateway $appgw `
  -Name urlpathmap `
  -PathRules $imagePathRule, $videoPathRule `
  -DefaultBackendAddressPool $defaultPool `
  -DefaultBackendHttpSettings $poolSettings

Set-AzApplicationGateway -ApplicationGateway $appgw
```

### <a name="add-routing-rule"></a>Yönlendirme kuralı ekleme

Yönlendirme kuralı URL eşlemesini oluşturduğunuz dinleyici ile ilişkilendirir. Adlı kural ekleme *bağlanma2* kullanarak [Ekle AzApplicationGatewayRequestRoutingRule](/powershell/module/az.network/add-azapplicationgatewayrequestroutingrule).

```azurepowershell-interactive
$appgw = Get-AzApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway

$backendlistener = Get-AzApplicationGatewayHttpListener `
  -ApplicationGateway $appgw `
  -Name backendListener

$urlPathMap = Get-AzApplicationGatewayUrlPathMapConfig `
  -ApplicationGateway $appgw `
  -Name urlpathmap

Add-AzApplicationGatewayRequestRoutingRule `
  -ApplicationGateway $appgw `
  -Name rule2 `
  -RuleType PathBasedRouting `
  -HttpListener $backendlistener `
  -UrlPathMap $urlPathMap

Set-AzApplicationGateway -ApplicationGateway $appgw
```

## <a name="create-virtual-machine-scale-sets"></a>Sanal makine ölçek kümelerini oluşturma

Bu örnekte, oluşturduğunuz üç arka uç havuzunu destekleyen üç sanal makine ölçek kümesi oluşturacaksınız. Oluşturduğunuz ölçek kümeleri *myvmss1*, *myvmss2* ve *myvmss3* olarak adlandırılır. IP ayarlarını yapılandırırken ölçek kümesini arka uç havuzuna atayın.

```azurepowershell-interactive
$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet

$appgw = Get-AzApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway

$backendPool = Get-AzApplicationGatewayBackendAddressPool `
  -Name appGatewayBackendPool `
  -ApplicationGateway $appgw

$imagesPool = Get-AzApplicationGatewayBackendAddressPool `
  -Name imagesBackendPool `
  -ApplicationGateway $appgw

$videoPool = Get-AzApplicationGatewayBackendAddressPool `
  -Name videoBackendPool `
  -ApplicationGateway $appgw

for ($i=1; $i -le 3; $i++)
{
  if ($i -eq 1)
  {
     $poolId = $backendPool.Id
  }
  if ($i -eq 2) 
  {
    $poolId = $imagesPool.Id
  }
  if ($i -eq 3)
  {
    $poolId = $videoPool.Id
  }

  $ipConfig = New-AzVmssIpConfig `
    -Name myVmssIPConfig$i `
    -SubnetId $vnet.Subnets[1].Id `
    -ApplicationGatewayBackendAddressPoolsId $poolId

  $vmssConfig = New-AzVmssConfig `
    -Location eastus `
    -SkuCapacity 2 `
    -SkuName Standard_DS2 `
    -UpgradePolicyMode Automatic

  Set-AzVmssStorageProfile $vmssConfig `
    -ImageReferencePublisher MicrosoftWindowsServer `
    -ImageReferenceOffer WindowsServer `
    -ImageReferenceSku 2016-Datacenter `
    -ImageReferenceVersion latest
    -OsDiskCreateOption FromImage

  Set-AzVmssOsProfile $vmssConfig `
    -AdminUsername azureuser `
    -AdminPassword "Azure123456!" `
    -ComputerNamePrefix myvmss$i

  Add-AzVmssNetworkInterfaceConfiguration `
    -VirtualMachineScaleSet $vmssConfig `
    -Name myVmssNetConfig$i `
    -Primary $true `
    -IPConfiguration $ipConfig

  New-AzVmss `
    -ResourceGroupName myResourceGroupAG `
    -Name myvmss$i `
    -VirtualMachineScaleSet $vmssConfig
}
```

### <a name="install-iis"></a>IIS yükleme

Her ölçek kümesi, üzerine IIS yüklediğiniz ve uygulama ağ geçidinin çalışıp çalışmadığını test etmek üzere örnek bir sayfa çalıştıran iki sanal makine örneği içerir.

```azurepowershell-interactive
$publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/appgatewayurl.ps1"); 
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }

for ($i=1; $i -le 3; $i++)
{
  $vmss = Get-AzVmss -ResourceGroupName myResourceGroupAG -VMScaleSetName myvmss$i
  Add-AzVmssExtension -VirtualMachineScaleSet $vmss `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $publicSettings

  Update-AzVmss `
    -ResourceGroupName myResourceGroupAG `
    -Name myvmss$i `
    -VirtualMachineScaleSet $vmss
}
```

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

Kullanım [Get-AzPublicIPAddress](/powershell/module/az.network/get-azpublicipaddress) uygulama ağ geçidinin genel IP adresini almak için. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın. Gibi `http://52.168.55.24`, `http://52.168.55.24:8080/images/test.htm`, veya `http://52.168.55.24:8080/video/test.htm`.

```azurepowershell-interactive
Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![Temel URL’yi uygulama ağ geçidinde test etme](./media/tutorial-url-route-powershell/application-gateway-iistest.png)

URL’yi http://&lt;ip-address&gt;:8080/images/test.html olarak değiştirin. Burada &lt;ip-address&gt; değeri olarak kendi IP adresinizi kullanın. Aşağıdaki örneğe benzer bir sonuç olacaktır:

![Görüntü URL’sini uygulama ağ geçidinde test etme](./media/tutorial-url-route-powershell/application-gateway-iistest-images.png)

URL’yi http://&lt;ip-address&gt;:8080/video/test.html olarak değiştirin. Burada &lt;ip-address&gt; değeri olarak kendi IP adresinizi kullanın. Aşağıdaki örneğe benzer bir sonuç olacaktır:

![Video URL’sini uygulama ağ geçidinde test etme](./media/tutorial-url-route-powershell/application-gateway-iistest-video.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, uygulama ağ geçidi ve tüm ilgili kaynakları kullanarak kaldırma [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup).

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroupAG
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Ağı ayarlama
> * Dinleyicileri, URL yol haritasını ve kuralları oluşturma
> * Ölçeklenebilir arka uç havuzları oluşturma

> [!div class="nextstepaction"]
> [URL’yi temel alarak web trafiğini yeniden yönlendirme](./tutorial-url-redirect-powershell.md)
