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
ms.openlocfilehash: d41480def95137e1dc938c845f8cec1d59e26036
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59521793"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-powershell"></a>Hızlı Başlangıç: Azure Application Gateway - Azure PowerShell ile doğrudan web trafiği

Bu hızlı başlangıçta hızlı bir şekilde uygulama ağ geçidi oluşturmak için Azure PowerShell kullanmayı gösterir.  Uygulama ağ geçidi oluşturduktan sonra ardından düzgün çalıştığından emin olmak için test edin. Azure Application Gateway ile bağlantı noktalarına dinleyicileri atama, kuralları oluşturma ve arka uç havuzu için kaynak ekleme, uygulama web trafiği belirli kaynaklara doğrudan. Basitleştirmek amacıyla, bu makalede bir genel ön uç IP ile basit bir Kurulum, konağa tek bir sitede bu uygulama ağ geçidinde temel dinleyiciyi arka uç havuzunu ve temel istek yönlendirme kuralı için kullanılan iki sanal makine kullanılmaktadır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

## <a name="prerequisites"></a>Önkoşullar

### <a name="azure-powershell-module"></a>Azure PowerShell modülü

Azure PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz Bu öğretici Azure PowerShell modülü sürüm 1.0.0 gerektirir veya üzeri.

1. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). 
2. Azure ile bağlantı oluşturmak için çalıştırın `Login-AzAccount`.

### <a name="resource-group"></a>Kaynak grubu

Azure'da, bir kaynak grubu için ilgili kaynakları ayırın. Mevcut bir kaynak grubunu kullanın veya yeni bir tane oluşturun. Bu örnekte, yeni bir kaynak grubu kullanarak oluşturacağız [yeni AzResourceGroup](/powershell/module/Az.resources/new-Azresourcegroup) cmdlet'i aşağıdaki gibi: 

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroupAG -Location eastus
```

### <a name="required-network-resources"></a>Gerekli ağ kaynakları

Oluşturduğunuz kaynaklar arasında iletişim kurmak Azure için sanal ağ gerekir.  Uygulama ağ geçidi alt ağı, yalnızca uygulama ağ geçitleri içerebilir. Başka kaynaklar izin verilir.  Application Gateway için yeni bir alt ağ oluşturun veya var olanı kullanın. Bu örnekte, bu örnekte iki alt ağ oluşturun: bir uygulama ağ geçidi ve diğeri arka uç sunucuları için. Kullanım Örneğinize ilişkin genel veya özel olacak şekilde uygulama ağ geçidi ön uç IP'si yapılandırabilirsiniz. Bu örnekte, genel bir ön uç IP seçeceğiz.

1. Alt ağ yapılandırmalarını çağırarak denetlediği oluşturma [yeni AzVirtualNetworkSubnetConfig](/powershell/module/Az.network/new-Azvirtualnetworksubnetconfig).
2. Çağırarak alt ağ yapılandırmaları ile sanal ağ oluşturma [yeni AzVirtualNetwork](/powershell/module/Az.network/new-Azvirtualnetwork). 
3. Genel IP adresini çağırarak denetlediği oluşturma [yeni AzPublicIpAddress](/powershell/module/Az.network/new-Azpublicipaddress). 

```azurepowershell-interactive
$agSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.1.0/24
$backendSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.2.0/24
New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $agSubnetConfig, $backendSubnetConfig
New-AzPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```
### <a name="backend-servers"></a>Arka uç sunucuları

Arka uç ağ, sanal makine ölçek kümeleri, genel IP'ler birleştirilebilir, iç IP'ler, tam etki alanı adlarını (FQDN) ve çok kiracılı arka-Azure App Service gibi biter. Bu örnekte, Azure application gateway için arka uç sunucular olarak kullanılacak iki sanal makine oluşturun. Ayrıca Azure uygulama ağ geçidi başarıyla oluşturuldu doğrulamak için sanal makinelere IIS yüklersiniz.

#### <a name="create-two-virtual-machines"></a>İki sanal makine oluşturma

1. Bir ağ arabirimi ile oluşturma [yeni AzNetworkInterface](/powershell/module/Az.network/new-Aznetworkinterface). 
2. Bir sanal makine yapılandırmasıyla oluşturma [yeni AzVMConfig](/powershell/module/Az.compute/new-Azvmconfig).
3. İle sanal makine oluşturma [New-AzVM](/powershell/module/Az.compute/new-Azvm).

Azure sanal makineler oluşturmak için aşağıdaki kod örneği çalıştırdığınızda, kimlik bilgilerini ister. Kullanıcı adı için *azureuser* ve parola için *Azure123456!* parolası:
    
```azurepowershell-interactive
$vnet   = Get-AzVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name myBackendSubnet
$cred = Get-Credential
for ($i=1; $i -le 2; $i++)
{
  $nic = New-AzNetworkInterface `
    -Name myNic$i `
    -ResourceGroupName myResourceGroupAG `
    -Location EastUS `
    -SubnetId $subnet.Id
  $vm = New-AzVMConfig `
    -VMName myVM$i `
    -VMSize Standard_DS2_v2
  Set-AzVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM$i `
    -Credential $cred
  Set-AzVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
  Add-AzVMNetworkInterface `
    -VM $vm `
    -Id $nic.Id
  Set-AzVMBootDiagnostics `
    -VM $vm `
    -Disable
  New-AzVM -ResourceGroupName myResourceGroupAG -Location EastUS -VM $vm
  Set-AzVMExtension `
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

1. Kullanım [yeni AzApplicationGatewayIPConfiguration](/powershell/module/Az.network/new-Azapplicationgatewayipconfiguration) alt ilişkilendiren bir yapılandırma oluşturmak üzere application gateway ile oluşturulmuş. 
2. Kullanım [yeni AzApplicationGatewayFrontendIPConfig](/powershell/module/Az.network/new-Azapplicationgatewayfrontendipconfig) uygulama ağ geçidine daha önce oluşturduğunuz genel IP adresini atar yapılandırmasını oluşturmak için. 
3. Kullanım [yeni AzApplicationGatewayFrontendPort](/powershell/module/Az.network/new-Azapplicationgatewayfrontendport) application gateway'e erişmek için 80 numaralı bağlantı noktasını atamak için.

```azurepowershell-interactive
$vnet   = Get-AzVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name myAGSubnet
$pip    = Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress 
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

### <a name="create-the-backend-pool"></a>Arka uç havuzunu oluşturma

1. Kullanım [yeni AzApplicationGatewayBackendAddressPool](/powershell/module/Az.network/new-Azapplicationgatewaybackendaddresspool) application gateway için arka uç havuzu oluşturun. 
2. Arka uç havuzu için ayarları yapılandırın [yeni AzApplicationGatewayBackendHttpSettings](/powershell/module/Az.network/new-Azapplicationgatewaybackendhttpsettings).

```azurepowershell-interactive
$address1 = Get-AzNetworkInterface -ResourceGroupName myResourceGroupAG -Name myNic1
$address2 = Get-AzNetworkInterface -ResourceGroupName myResourceGroupAG -Name myNic2
$backendPool = New-AzApplicationGatewayBackendAddressPool `
  -Name myAGBackendPool `
  -BackendIPAddresses $address1.ipconfigurations[0].privateipaddress, $address2.ipconfigurations[0].privateipaddress
$poolSettings = New-AzApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-listener-and-add-a-rule"></a>Dinleyiciyi oluşturma ve kural ekleme

Azure application gateway trafiği yönlendirme için arka uç havuzu için uygun şekilde etkinleştirmek bir dinleyici gerektirir. Azure ayrıca dinleyici için gelen trafiği kullanmak için hangi arka uç havuzu bilmesi için bir kuralı gerektirir. 

1. Kullanarak bir dinleyici oluşturun [yeni AzApplicationGatewayHttpListener](/powershell/module/Az.network/new-Azapplicationgatewayhttplistener) daha önce oluşturduğunuz ön uç bağlantı noktasını ve ön uç yapılandırması. 
2. Kullanım [yeni AzApplicationGatewayRequestRoutingRule](/powershell/module/Az.network/new-Azapplicationgatewayrequestroutingrule) adlı bir kural oluşturmak üzere *bağlanma1*. 

```azurepowershell-interactive
$defaultlistener = New-AzApplicationGatewayHttpListener `
  -Name myAGListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport
$frontendRule = New-AzApplicationGatewayRequestRoutingRule `
  -Name rule1 `
  -RuleType Basic `
  -HttpListener $defaultlistener `
  -BackendAddressPool $backendPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

Gerekli destekleyici kaynakları oluşturduğunuza göre uygulama ağ geçidi oluşturun:

1. Kullanım [yeni AzApplicationGatewaySku](/powershell/module/Az.network/new-Azapplicationgatewaysku) application gateway için parametreleri belirtmek için.
2. Kullanım [yeni AzApplicationGateway](/powershell/module/Az.network/new-Azapplicationgateway) uygulama ağ geçidi oluşturma.

```azurepowershell-interactive
$sku = New-AzApplicationGatewaySku `
  -Name Standard_Medium `
  -Tier Standard `
  -Capacity 2
New-AzApplicationGateway `
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

1. Çalıştırma [Get-AzPublicIPAddress](/powershell/module/Az.network/get-Azpublicipaddress) uygulama ağ geçidinin genel IP adresini almak için. 
2. Kopyalama ve genel IP adresi, tarayıcınızın adres çubuğuna yapıştırın. Tarayıcıyı yenileyin, sanal makinenin adını görmeniz gerekir. Uygulama ağ geçidi başarıyla oluşturuldu ve arka uç ile başarıyla bağlanabilmek için geçerli bir yanıt doğrular.

```azurepowershell-interactive
Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![Uygulama ağ geçidini test etme](./media/quick-create-powershell/application-gateway-iistest.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Application gateway ile oluşturduğunuz kaynaklara artık ihtiyacınız olduğunda, kaynak grubunu kaldırın. Kaynak grubu kaldırarak, ayrıca uygulama ağ geçidi ve tüm ilgili kaynakları kaldırın. 

Kaynak grubunu kaldırmak için çağrı [Remove-AzResourceGroup](/powershell/module/Az.resources/remove-Azresourcegroup) cmdlet'i aşağıdaki gibi:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroupAG
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure PowerShell kullanarak bir uygulama ağ geçidi ile web trafiğini yönetme](./tutorial-manage-web-traffic-powershell.md)

