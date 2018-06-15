---
title: 'Hızlı Başlangıç: Azure Application Gateway ile web trafiğini yönlendirme - Azure PowerShell | Microsoft Docs'
description: Web trafiğini arka uç havuzundaki sanal makinelere yönlendiren bir Azure Application Gateway oluşturmak üzere Azure PowerShell’i kullanmayı öğrenin.
services: application-gateway
author: vhorne
manager: jpconnock
editor: ''
tags: azure-resource-manager
ms.service: application-gateway
ms.devlang: azurepowershell
ms.topic: quickstart
ms.workload: infrastructure-services
ms.date: 01/25/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 95b806eaabaf0ba93b1e8b6823340e82842b0f1c
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
ms.locfileid: "33202057"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-powershell"></a>Hızlı Başlangıç: Azure Application Gateway ile web trafiğini yönlendirme - Azure PowerShell

Azure Application Gateway ile bağlantı noktalarına dinleyiciler atayarak ve arka uç havuzuna kaynaklar ekleyerek uygulama web trafiğinizi belirli kaynaklara yönlendirebilirsiniz.

Bu hızlı başlangıç, arka uç havuzunda iki sanal makine bulunan bir uygulama ağ geçidini hızla oluşturmak üzere Azure portalını kullanmayı göstermektedir. Ardından doğru bir şekilde çalışıp çalışmadığından emin olmak için test edersiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Daima kaynak grubunda kaynaklar oluşturmanız gerekir. [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) komutunu kullanarak yeni bir Azure kaynak grubu oluşturun. 

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroupAG -Location eastus
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma 

Diğer kaynaklarla iletişim kurmak için uygulama ağ geçidine yönelik bir sanal makine oluşturmanız gerekir. Bu örnekte iki alt ağ oluşturulmuştur: biri uygulama ağ geçidi ve diğeri de arka uç sunucuları içindir. 

[New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) komutunu kullanarak alt ağ yapılandırmasını oluşturun. Alt ağ yapılandırmaları ile [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) komutunu kullanarak sanal ağı oluşturun. Son olarak da [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) komutunu kullanarak genel IP adresini oluşturun.

```azurepowershell-interactive
$backendSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.1.0/24
$agSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.2.0/24
New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $backendSubnetConfig, $agSubnetConfig
New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```
## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu örnekte, uygulama ağ geçidi için arka uç sunucular olarak kullanılacak iki sanal makine oluşturacaksınız. 

Uygulama ağ geçidinin başarıyla oluşturulduğunu doğrulamak için sanal makinelere IIS de yükleyin.

### <a name="create-two-virtual-machines"></a>İki sanal makine oluşturma

[New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) komutuyla bir ağ arabirimi oluşturun. [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) ile bir sanal makine yapılandırması oluşturun. Aşağıdaki komutları çalıştırdığınızda sizden kimlik bilgileri istenir. Kullanıcı adı için *azureuser* ve parola için *Azure123456!* girin. [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) ile sanal makineyi oluşturun.

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$cred = Get-Credential
for ($i=1; $i -le 2; $i++)
{
  $nic = New-AzureRmNetworkInterface `
    -Name myNic$i `
    -ResourceGroupName myResourceGroupAG `
    -Location EastUS `
    -SubnetId $vnet.Subnets[1].Id
  $vm = New-AzureRmVMConfig `
    -VMName myVM$i `
    -VMSize Standard_DS2
  $vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM$i `
    -Credential $cred
  $vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
  $vm = Add-AzureRmVMNetworkInterface `
    -VM $vm `
    -Id $nic.Id
  $vm = Set-AzureRmVMBootDiagnostics `
    -VM $vm `
    -Disable
  New-AzureRmVM -ResourceGroupName myResourceGroupAG -Location EastUS -VM $vm
  Set-AzureRmVMExtension `
    -ResourceGroupName myResourceGroupAG `
    -ExtensionName IIS `
    -VMName myVM$i `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
    -Location EastUS
}
```

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

### <a name="create-the-ip-configurations-and-frontend-port"></a>IP yapılandırmaları ve ön uç bağlantı noktası oluşturma

Daha önce oluşturduğunuz alt ağı uygulama ağ geçidi ile ilişkilendiren yapılandırmayı oluşturmak için [New-AzureRmApplicationGatewayIPConfiguration](/powershell/module/azurerm.network/new-azurermapplicationgatewayipconfiguration) komutunu kullanın. Yine daha önce oluşturduğunuz genel IP adresini uygulama ağ geçidine atayan yapılandırmayı oluşturmak için [New-AzureRmApplicationGatewayFrontendIPConfig](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendipconfig) komutunu kullanın. Uygulama ağ geçidine erişmek için kullanılacak 80 numaralı bağlantı noktasını atamak için [New-AzureRmApplicationGatewayFrontendPort](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendport) komutunu kullanın.

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$pip = Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress 
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

### <a name="create-the-backend-pool"></a>Arka uç havuzunu oluşturma

Uygulama ağ geçidinin arka uç havuzunu oluşturmak için [New-AzureRmApplicationGatewayBackendAddressPool](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendaddresspool) komutunu kullanın. [New-AzureRmApplicationGatewayBackendHttpSettings](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendhttpsettings) komutunu kullanarak arka uç havuzunun ayarlarını yapılandırın.

```azurepowershell-interactive
$address1 = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroupAG -Name myNic1
$address2 = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroupAG -Name myNic2
$backendPool = New-AzureRmApplicationGatewayBackendAddressPool `
  -Name myAGBackendPool `
  -BackendIPAddresses $address1.ipconfigurations[0].privateipaddress, $address2.ipconfigurations[0].privateipaddress
$poolSettings = New-AzureRmApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-listener-and-add-a-rule"></a>Dinleyiciyi oluşturma ve kural ekleme

Uygulama ağ geçidinin trafiği arka uç havuzuna uygun şekilde yönlendirmesini sağlamak için bir dinleyici gereklidir. Daha önce oluşturduğunuz ön uç yapılandırması ve ön uç bağlantı noktası ile birlikte [New-AzureRmApplicationGatewayHttpListener](/powershell/module/azurerm.network/new-azurermapplicationgatewayhttplistener) komutunu kullanarak bir dinleyici oluşturun. Dinleyicinin gelen trafik için kullanacağı arka uç havuzunu bilmesi için bir kural gerekir. *rule1* adlı bir kural oluşturmak için [New-AzureRmApplicationGatewayRequestRoutingRule](/powershell/module/azurerm.network/new-azurermapplicationgatewayrequestroutingrule) komutunu kullanın.

```azurepowershell-interactive
$defaultlistener = New-AzureRmApplicationGatewayHttpListener `
  -Name myAGListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport
$frontendRule = New-AzureRmApplicationGatewayRequestRoutingRule `
  -Name rule1 `
  -RuleType Basic `
  -HttpListener $defaultlistener `
  -BackendAddressPool $backendPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

Gerekli destekleyici kaynakları oluşturduktan sonra, uygulama ağ geçidinin parametrelerini belirtmek için [New-AzureRmApplicationGatewaySku](/powershell/module/azurerm.network/new-azurermapplicationgatewaysku) ve sonra uygulama ağ geçidini oluşturmak için [New-AzureRmApplicationGateway](/powershell/module/azurerm.network/new-azurermapplicationgateway) komutunu kullanın.

```azurepowershell-interactive
$sku = New-AzureRmApplicationGatewaySku `
  -Name Standard_Medium `
  -Tier Standard `
  -Capacity 2
New-AzureRmApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -BackendAddressPools $backendPool `
  -BackendHttpSettingsCollection $poolSettings `
  -FrontendIpConfigurations $fipconfig `
  -GatewayIpConfigurations $gipconfig `
  -FrontendPorts $frontendport `
  -HttpListeners $defaultlistener `
  -RequestRoutingRules $frontendRule `
  -Sku $sku
```

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

Uygulama ağ geçidini oluşturmak için NGINX’in yüklenmesi gerekli değildir ancak IIS’i bu hızlı başlangıçta uygulama ağ geçidinin başarılı bir şekilde oluşturulup oluşturulmadığını doğrulamak için yüklediniz. Uygulama ağ geçidinin genel IP adresini almak için [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) komutunu kullanın. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın.

```azurepowershell-interactive
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![Uygulama ağ geçidini test etme](./media/quick-create-powershell/application-gateway-iistest.png)

Tarayıcıyı yenilediğinizde diğer VM’nin adının göründüğünü görürsünüz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

İlk olarak uygulama ağ geçidiyle oluşturulan kaynakları keşfedin ve ardından artık gerekmediğinde kaynak grubu, uygulama ağ geçidi ve tüm ilgili kaynakları kaldırmak için [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanabilirsiniz.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroupAG
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure PowerShell kullanarak bir uygulama ağ geçidi ile web trafiğini yönetme](./tutorial-manage-web-traffic-powershell.md)

