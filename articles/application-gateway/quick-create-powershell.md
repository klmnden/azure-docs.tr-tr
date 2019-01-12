---
title: 'Hızlı Başlangıç: Azure Application Gateway ile web trafiğini yönlendirme - Azure PowerShell | Microsoft Docs'
description: Web trafiğini arka uç havuzundaki sanal makinelere yönlendiren bir Azure Application Gateway oluşturmak üzere Azure PowerShell’i kullanmayı öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 1/11/2019
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 9edfa85105bbc20cf7f149d4c31b60d9e570a7ad
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2019
ms.locfileid: "54243738"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-powershell"></a>Hızlı Başlangıç: Azure Application Gateway - Azure PowerShell ile doğrudan web trafiği

Bu hızlı başlangıçta hızlı bir şekilde, arka uç havuzunda iki sanal makine ile bir uygulama ağ geçidi oluşturmak için Azure portalını kullanmayı gösterir. Ardından doğru bir şekilde çalışıp çalışmadığından emin olmak için test edersiniz. Azure Application Gateway ile belirli kaynaklar tarafından uygulama web trafiği doğrudan: dinleyici bağlantı noktalarına atama, kuralları oluşturma ve kaynakları bir arka uç havuzuna ekleme.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

## <a name="run-azure-powershell-locally"></a>Azure PowerShell'i yerel olarak çalıştırın

Azure PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz Bu öğretici Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir.

1. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). 
2. Azure ile bağlantı oluşturmak için çalıştırın `Login-AzureRmAccount`.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure'da, bir kaynak grubu için ilgili kaynakları ayırın. Kullanarak bir kaynak grubu oluşturma [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet'i aşağıdaki gibi: 

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroupAG -Location eastus
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma

Uygulama ağ geçidi diğer kaynaklarla iletişim kurabilmesi için bir sanal ağ oluşturun. Bu örnekte, iki alt ağ oluşturulur: bir uygulama ağ geçidi ve diğeri arka uç sunucuları için. Uygulama ağ geçidi alt ağı, yalnızca uygulama ağ geçitleri içerebilir. Başka kaynaklar izin verilir.

1. Alt ağ yapılandırmalarını çağırarak denetlediği oluşturma [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig).
2. Çağırarak alt ağ yapılandırmaları ile sanal ağ oluşturma [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).
3. Genel IP adresini çağırarak denetlediği oluşturma [New-Azurermpublicıpaddress](/powershell/module/azurerm.network/new-azurermpublicipaddress).

```azurepowershell-interactive
$agSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.1.0/24
$backendSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.2.0/24
New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $agSubnetConfig, $backendSubnetConfig
New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```
## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu örnekte, Azure application gateway için arka uç sunucular olarak kullanılacak iki sanal makine oluşturun. Ayrıca Azure uygulama ağ geçidi başarıyla oluşturuldu doğrulamak için sanal makinelere IIS yüklersiniz.

### <a name="create-two-virtual-machines"></a>İki sanal makine oluşturma

1. [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) komutuyla bir ağ arabirimi oluşturun. 
2. [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) ile bir sanal makine yapılandırması oluşturun.
3. [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) ile sanal makineyi oluşturun.

Azure sanal makineler oluşturmak için aşağıdaki kod örneği çalıştırdığınızda, kimlik bilgilerini ister. Kullanıcı adı için *azureuser* ve parola için *Azure123456!* parolası:
    
```azurepowershell-interactive
$vnet   = Get-AzureRmVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name myBackendSubnet
$cred = Get-Credential
for ($i=1; $i -le 2; $i++)
{
  $nic = New-AzureRmNetworkInterface `
    -Name myNic$i `
    -ResourceGroupName myResourceGroupAG `
    -Location EastUS `
    -SubnetId $subnet.Id
  $vm = New-AzureRmVMConfig `
    -VMName myVM$i `
    -VMSize Standard_DS2_v2
  Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM$i `
    -Credential $cred
  Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
  Add-AzureRmVMNetworkInterface `
    -VM $vm `
    -Id $nic.Id
  Set-AzureRmVMBootDiagnostics `
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

1. Kullanım [yeni AzureRmApplicationGatewayIPConfiguration](/powershell/module/azurerm.network/new-azurermapplicationgatewayipconfiguration) alt ilişkilendiren bir yapılandırma oluşturmak üzere application gateway ile oluşturulmuş. 
2. Kullanım [yeni AzureRmApplicationGatewayFrontendIPConfig](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendipconfig) uygulama ağ geçidine daha önce oluşturduğunuz genel IP adresini atar yapılandırmasını oluşturmak için. 
3. Kullanım [yeni AzureRmApplicationGatewayFrontendPort](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendport) application gateway'e erişmek için 80 numaralı bağlantı noktasını atamak için.

```azurepowershell-interactive
$vnet   = Get-AzureRmVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name myAGSubnet
$pip    = Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress 
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

1. Uygulama ağ geçidinin arka uç havuzunu oluşturmak için [New-AzureRmApplicationGatewayBackendAddressPool](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendaddresspool) komutunu kullanın. 
2. Arka uç havuzu için ayarları yapılandırın [yeni AzureRmApplicationGatewayBackendHttpSettings](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendhttpsettings).

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

Azure application gateway trafiği yönlendirme için arka uç havuzu için uygun şekilde etkinleştirmek bir dinleyici gerektirir. Azure ayrıca dinleyici için gelen trafiği kullanmak için hangi arka uç havuzu bilmesi için bir kuralı gerektirir. 

1. Kullanarak bir dinleyici oluşturun [yeni AzureRmApplicationGatewayHttpListener](/powershell/module/azurerm.network/new-azurermapplicationgatewayhttplistener) daha önce oluşturduğunuz ön uç bağlantı noktasını ve ön uç yapılandırması. 
2. *rule1* adlı bir kural oluşturmak için [New-AzureRmApplicationGatewayRequestRoutingRule](/powershell/module/azurerm.network/new-azurermapplicationgatewayrequestroutingrule) komutunu kullanın. 

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

Gerekli destekleyici kaynakları oluşturduğunuza göre uygulama ağ geçidi oluşturun:

1. Kullanım [yeni AzureRmApplicationGatewaySku](/powershell/module/azurerm.network/new-azurermapplicationgatewaysku) application gateway için parametreleri belirtmek için.
2. Kullanım [New-AzureRmApplicationGateway](/powershell/module/azurerm.network/new-azurermapplicationgateway) uygulama ağ geçidi oluşturma.

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

IIS uygulama ağ geçidi oluşturmak için gerekli değildir, ancak bu hızlı başlangıçta Azure uygulama ağ geçidi başarıyla oluşturulmuş olup olmadığını doğrulamak için yüklü. IIS, uygulama ağ geçidi test etmek için kullanın:

1. Çalıştırma [Get-Azurermpublicıpaddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) uygulama ağ geçidinin genel IP adresini almak için. 
2. Kopyalama ve genel IP adresi, tarayıcınızın adres çubuğuna yapıştırın. Tarayıcıyı yenileyin, sanal makinenin adını görmeniz gerekir.

```azurepowershell-interactive
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![Uygulama ağ geçidini test etme](./media/quick-create-powershell/application-gateway-iistest.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Application gateway ile oluşturduğunuz kaynaklara artık ihtiyacınız olduğunda, kaynak grubunu kaldırın. Kaynak grubu kaldırarak, ayrıca uygulama ağ geçidi ve tüm ilgili kaynakları kaldırın. 

Kaynak grubunu kaldırmak için çağrı [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) cmdlet'i aşağıdaki gibi:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroupAG
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure PowerShell kullanarak bir uygulama ağ geçidi ile web trafiğini yönetme](./tutorial-manage-web-traffic-powershell.md)

