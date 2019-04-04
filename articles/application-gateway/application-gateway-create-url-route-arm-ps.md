---
title: URL yolu tabanlı yönlendirme kurallarıyla - Azure PowerShell bir uygulama ağ geçidi oluşturma | Microsoft Docs
description: URL yolu tabanlı yönlendirme kuralları için Azure PowerShell kullanarak bir uygulama ağ geçidi ve sanal makine ölçek kümesi oluşturmayı öğrenin.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/26/2018
ms.author: victorh
ms.openlocfilehash: 291a4474adb7a70ea89db41362763f045d70ea57
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58905435"
---
# <a name="create-an-application-gateway-with-url-path-based-routing-rules-using-azure-powershell"></a>Azure PowerShell kullanarak URL yolu tabanlı yönlendirme kurallarıyla bir uygulama ağ geçidi oluşturma

Azure PowerShell yapılandırmak için kullanabileceğiniz [URL yolu tabanlı yönlendirme kuralları](application-gateway-url-route-overview.md) oluşturduğunuzda bir [uygulama ağ geçidi](application-gateway-introduction.md). Bu öğreticide oluşturduğunuz arka uç havuzları kullanan bir [sanal makine ölçek kümesi](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). Daha sonra uygun sunucularla havuzlarındaki web trafiğini ulaştığında emin yönlendirme kuralları oluşturursunuz.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Ağı ayarlama
> * URL eşlemesi ile bir uygulama ağ geçidi oluşturma
> * Arka uç havuzları ile sanal makine ölçek kümeleri oluşturma

![URL yönlendirme örneği](./media/application-gateway-create-url-route-arm-ps/scenario.png)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz Bu öğretici Azure PowerShell modülünü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Kullanarak bir Azure kaynak grubu oluşturmanız [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup).  

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroupAG -Location eastus
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma

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

Adlı varsayılan arka uç havuzu oluşturun *appGatewayBackendPool* kullanarak uygulama ağ geçidi için [yeni AzApplicationGatewayBackendAddressPool](/powershell/module/az.network/new-azapplicationgatewaybackendaddresspool). Kullanarak arka uç havuzu için ayarları yapılandırma [yeni AzApplicationGatewayBackendHttpSettings](/powershell/module/az.network/new-azapplicationgatewaybackendhttpsettings).

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

Adlı varsayılan dinleyiciyi oluşturun *myDefaultListener* kullanarak [yeni AzApplicationGatewayHttpListener](/powershell/module/az.network/new-azapplicationgatewayhttplistener) daha önce oluşturduğunuz ön uç bağlantı noktasını ve ön uç yapılandırması. Dinleyicinin gelen trafik için kullanacağı arka uç havuzunu bilmesi için bir kural gerekir. Adlı bir temel kuralı oluşturun *bağlanma1* kullanarak [yeni AzApplicationGatewayRequestRoutingRule](/powershell/module/az.network/new-azapplicationgatewayrequestroutingrule).

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

### <a name="add-image-and-video-backend-pools-and-port"></a>Görüntü ve video arka uç havuzlarını ve bağlantı noktasını ekleme

Adlı arka uç havuzları ekleyebilirsiniz *imagesBackendPool* ve *videoBackendPool* kullanarak uygulama ağ geçidi için [Ekle AzApplicationGatewayBackendAddressPool](/powershell/module/az.network/add-azapplicationgatewaybackendaddresspool). Ön uç bağlantı noktasını kullanarak havuzların eklediğiniz [Ekle AzApplicationGatewayFrontendPort](/powershell/module/az.network/add-azapplicationgatewayfrontendport). Ardından uygulama ağ geçidi kullanarak değişiklikleri gönderdiğiniz [kümesi AzApplicationGateway](/powershell/module/az.network/set-azapplicationgateway).

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

URL yol eşlemeleri belirli URL'lerin belirli arka uç havuzlarına yönlendirilmesini sağlar. URL yolu eşlemeleri adlı oluşturabilirsiniz *imagePathRule* ve *videoPathRule* kullanarak [yeni AzApplicationGatewayPathRuleConfig](/powershell/module/az.network/new-azapplicationgatewaypathruleconfig) ve [ Ekleme AzApplicationGatewayUrlPathMapConfig](/powershell/module/az.network/add-azapplicationgatewayurlpathmapconfig).

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

Yönlendirme kuralı URL eşlemesini oluşturduğunuz dinleyici ile ilişkilendirir. Adlı bir kural ekleyebileceğiniz **bağlanma2* kullanarak [Ekle AzApplicationGatewayRequestRoutingRule](/powershell/module/az.network/add-azapplicationgatewayrequestroutingrule).

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

Bu örnekte, oluşturduğunuz üç arka uç havuzunu destekleyen üç sanal makine ölçek kümesi oluşturacaksınız. Oluşturduğunuz ölçek kümeleri *myvmss1*, *myvmss2* ve *myvmss3* olarak adlandırılır. Her bir ölçek kümesi IIS yükleyeceğiniz iki sanal makine örneği içerir. IP ayarlarını yapılandırırken ölçek kümesini arka uç havuzuna atayın.

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

Kullanabileceğiniz [Get-AzPublicIPAddress](/powershell/module/az.network/get-azpublicipaddress) uygulama ağ geçidinin genel IP adresini almak için. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın. Gibi `http://52.168.55.24`, `http://52.168.55.24:8080/images/test.htm`, veya `http://52.168.55.24:8080/video/test.htm`.

```azurepowershell-interactive
Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![Temel URL’yi uygulama ağ geçidinde test etme](./media/application-gateway-create-url-route-arm-ps/application-gateway-iistest.png)

URL'yi `http://<ip-address>:8080/video/test.htm`, değiştirme, IP adresi `<ip-address>`, aşağıdaki örneğe benzer bir şey görmeniz gerekir:

![Görüntü URL’sini uygulama ağ geçidinde test etme](./media/application-gateway-create-url-route-arm-ps/application-gateway-iistest-images.png)

URL'yi `http://<ip-address>:8080/video/test.htm` ve aşağıdaki örnekte olduğu gibi bir şey görmeniz gerekir:

![Video URL’sini uygulama ağ geçidinde test etme](./media/application-gateway-create-url-route-arm-ps/application-gateway-iistest-video.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrendiğiniz nasıl yapılır:

> [!div class="checklist"]
> * Ağı ayarlama
> * URL eşlemesi ile bir uygulama ağ geçidi oluşturma
> * Arka uç havuzları ile sanal makine ölçek kümeleri oluşturma

Uygulama ağ geçitleri ve bunların ilişkili kaynakları hakkında daha fazla bilgi edinmek için nasıl yapılır makaleleriyle devam edin.
