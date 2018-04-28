---
title: Dış yönlendirmesi - Azure PowerShell ile bir uygulama ağ geçidi oluşturma | Microsoft Docs
description: Web trafiği Azure Powershell kullanarak bir dış sitesine yönlendiren bir uygulama ağ geçidi oluşturmayı öğrenin.
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/24/2018
ms.author: davidmu
ms.openlocfilehash: 6ae672ba20ed6314f7baf49fee3310cecf7837ab
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="create-an-application-gateway-with-external-redirection-using-azure-powershell"></a>Azure PowerShell kullanarak dış yeniden yönlendirme ile bir uygulama ağ geçidi oluşturma

Azure Powershell yapılandırmak için kullanabileceğiniz [web trafiği yeniden yönlendirme](multiple-site-overview.md) oluştururken bir [uygulama ağ geçidi](overview.md). Bu öğreticide, bir dinleyici ve uygulama ağ geçidi harici bir siteye ulaştığında web trafiğini yönlendiren kuralı yapılandırın.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Ağ kurma
> * Dinleyici ve yeniden yönlendirme kuralı oluşturma
> * Uygulama ağ geçidi oluşturma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürüm bulmak için Çalıştır ` Get-Module -ListAvailable AzureRM` . Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Kullanarak bir Azure kaynak grubu oluşturma [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).  

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroupAG -Location eastus
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma

Alt ağ yapılandırması oluşturma *myAGSubnet* kullanarak [yeni AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Adlı sanal ağ oluşturma *myVNet* kullanarak [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) alt ağ yapılandırmasına sahip. Ve son olarak, genel IP adresini kullanarak oluşturmak [yeni AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress). Bu kaynaklar, uygulama ağ geçidi ve onun ilişkili kaynakları için ağ bağlantısı sağlamak için kullanılır.

```azurepowershell-interactive
$agSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.1.0/24
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $agSubnetConfig
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

### <a name="create-the-ip-configurations-and-frontend-port"></a>IP yapılandırmaları ve ön uç bağlantı noktası oluşturma

İlişkilendirme *myAGSubnet* daha önce oluşturduğunuz uygulama ağ geçidi kullanarak [yeni AzureRmApplicationGatewayIPConfiguration](/powershell/module/azurerm.network/new-azurermapplicationgatewayipconfiguration). Uygulama ağ geçidi kullanarak genel IP adresi atamak [yeni AzureRmApplicationGatewayFrontendIPConfig](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendipconfig). Ve ardından HTTP bağlantı noktası kullanarak oluşturabileceğiniz [yeni AzureRmApplicationGatewayFrontendPort](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendport).

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet
$subnet=$vnet.Subnets[0]
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration `
  -Name myAGIPConfig `
  -Subnet $subnet
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig `
  -Name myAGFrontendIPConfig `
  -PublicIPAddress $pip
$frontendport = New-AzureRmApplicationGatewayFrontendPort `
  -Name myFrontendPort `
  -Port 80
```

### <a name="create-the-backend-pool-and-settings"></a>Arka uç havuzu ve ayarları oluşturma

Adlı arka uç havuzu oluşturma *defaultPool* kullanarak uygulama ağ geçidi için [yeni AzureRmApplicationGatewayBackendAddressPool](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendaddresspool). Kullanarak havuzu ayarlarını yapılandırmak [yeni AzureRmApplicationGatewayBackendHttpSettings](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendhttpsettings).

```azurepowershell-interactive
$defaultPool = New-AzureRmApplicationGatewayBackendAddressPool `
  -Name defaultPool 
$poolSettings = New-AzureRmApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-listener-and-rule"></a>Dinleyici ve kural oluşturma

Dinleyici uygun şekilde trafiği yönlendirmek uygulama ağ geçidi etkinleştirmek için gereklidir. Kullanarak bir dinleyici oluşturun [yeni AzureRmApplicationGatewayHttpListener](/powershell/module/azurerm.network/new-azurermapplicationgatewayhttplistener) ön uç yapılandırma ve daha önce oluşturduğunuz ön uç bağlantı noktası. Bir kural gelen trafiği göndermek nereye bilmesi için dinleyici gereklidir. Adlı temel bir kural oluşturmak *redirectRule* kullanarak [yeni AzureRmApplicationGatewayRequestRoutingRule](/powershell/module/azurerm.network/new-azurermapplicationgatewayrequestroutingrule).

```azurepowershell-interactive
$defaultListener = New-AzureRmApplicationGatewayHttpListener `
  -Name defaultListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport
$redirectConfig = New-AzureRmApplicationGatewayRedirectConfiguration `
  -Name myredirect `
  -RedirectType Temporary `
  -TargetUrl "http://bing.com"
$redirectRule = New-AzureRmApplicationGatewayRequestRoutingRule `
  -Name redirectRule `
  -RuleType Basic `
  -HttpListener $defaultListener `
  -RedirectConfiguration $redirectConfig
```

### <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

Gerekli destekleyici kaynakları oluşturduğunuza göre adlı uygulama ağ geçidi için parametre belirtin *myAppGateway* kullanarak [yeni AzureRmApplicationGatewaySku](/powershell/module/azurerm.network/new-azurermapplicationgatewaysku)ve kullanarakoluşturma[ Yeni-AzureRmApplicationGateway](/powershell/module/azurerm.network/new-azurermapplicationgateway).

```azurepowershell-interactive
$sku = New-AzureRmApplicationGatewaySku `
  -Name Standard_Medium `
  -Tier Standard `
  -Capacity 2
$appgw = New-AzureRmApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -BackendAddressPools $defaultPool `
  -BackendHttpSettingsCollection $poolSettings `
  -FrontendIpConfigurations $fipconfig `
  -GatewayIpConfigurations $gipconfig `
  -FrontendPorts $frontendport `
  -HttpListeners $defaultListener `
  -RequestRoutingRules $redirectRule `
  -RedirectConfigurations $redirectConfig `
  -Sku $sku
```

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidi sınama

Kullanabileceğiniz [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) uygulama ağ geçidi genel IP adresi alınamıyor. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın.

```azurepowershell-interactive
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

Görmeniz gerekir *aratıp* tarayıcınızda görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrenilen nasıl yapılır:

> [!div class="checklist"]
> * Ağ kurma
> * Dinleyici ve yeniden yönlendirme kuralı oluşturma
> * Uygulama ağ geçidi oluşturma