---
title: Application gateway ile HTTP HTTPS'ye yönlendirmeyi - Azure PowerShell oluşturma | Microsoft Docs
description: HTTP yeniden yönlendirilen trafiğin Azure PowerShell kullanarak HTTPS ile bir uygulama ağ geçidi oluşturmayı öğrenin.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 7/13/2018
ms.author: victorh
ms.openlocfilehash: a9bff2ab8569a6e070ce5ea4c02b022a172450a3
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62098022"
---
# <a name="create-an-application-gateway-with-http-to-https-redirection-using-azure-powershell"></a>Application gateway, Azure PowerShell kullanarak HTTPS yeniden yönlendirmesi için HTTP ile oluşturma

Azure PowerShell oluşturmak için kullanabileceğiniz bir [uygulama ağ geçidi](overview.md) ile SSL sonlandırma için bir sertifika. Yönlendirme kuralı, application gateway'iniz HTTPS bağlantı noktasına HTTP trafiğini yönlendirmek için kullanılır. Ayrıca, bu örnekte, oluşturduğunuz bir [sanal makine ölçek kümesi](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) iki sanal makine örnekleri içeren application Gateway arka uç havuzu için. 

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Otomatik olarak imzalanan sertifika oluşturma
> * Ağ ayarlama
> * Sertifikalı bir uygulama ağ geçidi oluşturma
> * Dinleyici ve yeniden yönlendirme kuralı Ekle
> * Varsayılan arka uç havuzuyla bir sanal makine ölçek kümesi oluşturma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu öğretici Azure PowerShell modülü sürüm 1.0.0 gerektirir veya üzeri. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). Bu öğreticideki komutları çalıştırmak için de çalışmasına ihtiyacınız `Login-AzAccount` Azure ile bir bağlantı oluşturmak için.

## <a name="create-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika oluşturma

Üretim kullanımı için güvenilen bir sağlayıcı tarafından imzalanan geçerli bir sertifikayı içeri aktarmalısınız. Bu öğretici için [New-SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/new-selfsignedcertificate) komutunu kullanarak otomatik olarak imzalanan bir sertifika oluşturursunuz. Sertifikadan pfx dosyası dışarı aktarmak için döndürülen Parmak izi ile [Export-PfxCertificate](https://docs.microsoft.com/powershell/module/pkiclient/export-pfxcertificate) komutunu kullanabilirsiniz.

```powershell
New-SelfSignedCertificate `
  -certstorelocation cert:\localmachine\my `
  -dnsname www.contoso.com
```

Bu sonuca benzer bir şey görmeniz gerekir:

```
PSParentPath: Microsoft.PowerShell.Security\Certificate::LocalMachine\my

Thumbprint                                Subject
----------                                -------
E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630  CN=www.contoso.com
```

pfx dosyasını oluşturmak için parmak izini kullanın:

```powershell
$pwd = ConvertTo-SecureString -String "Azure123456!" -Force -AsPlainText
Export-PfxCertificate `
  -cert cert:\localMachine\my\E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630 `
  -FilePath c:\appgwcert.pfx `
  -Password $pwd
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Adlı bir Azure kaynak grubu oluşturma *myResourceGroupAG* kullanarak [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). 

```powershell
New-AzResourceGroup -Name myResourceGroupAG -Location eastus
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma

Alt ağ yapılandırmalarını oluşturma *myBackendSubnet* ve *myAGSubnet* kullanarak [yeni AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig). Adlı sanal ağ oluşturma *myVNet* kullanarak [yeni AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) alt ağ yapılandırmalarını ile. Ve son olarak, adlı ortak IP adresi oluşturma *myAGPublicIPAddress* kullanarak [yeni AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress). Bu kaynaklar, uygulama ağ geçidi ve ilişkili kaynakları ile ağ bağlantısı sağlamak için kullanılır.

```powershell
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

İlişkilendirme *myAGSubnet* uygulama ağ geçidi kullanarak daha önce oluşturduğunuz [yeni AzApplicationGatewayIPConfiguration](/powershell/module/az.network/new-azapplicationgatewayipconfiguration). Ata *myAGPublicIPAddress* kullanarak uygulama ağ geçidi [yeni AzApplicationGatewayFrontendIPConfig](/powershell/module/az.network/new-azapplicationgatewayfrontendipconfig). Ve ardından HTTPS bağlantı noktası kullanarak oluşturabilirsiniz [yeni AzApplicationGatewayFrontendPort](/powershell/module/az.network/new-azapplicationgatewayfrontendport).

```powershell
$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet
$subnet=$vnet.Subnets[0]
$gipconfig = New-AzApplicationGatewayIPConfiguration `
  -Name myAGIPConfig `
  -Subnet $subnet
$fipconfig = New-AzApplicationGatewayFrontendIPConfig `
  -Name myAGFrontendIPConfig `
  -PublicIPAddress $pip
$frontendPort = New-AzApplicationGatewayFrontendPort `
  -Name myFrontendPort `
  -Port 443
```

### <a name="create-the-backend-pool-and-settings"></a>Arka uç havuzu ve ayarları oluşturma

Adlı arka uç havuzu oluşturun *appGatewayBackendPool* kullanarak uygulama ağ geçidi için [yeni AzApplicationGatewayBackendAddressPool](/powershell/module/az.network/new-azapplicationgatewaybackendaddresspool). Kullanarak arka uç havuzu için ayarları yapılandırma [yeni AzApplicationGatewayBackendHttpSettings](/powershell/module/az.network/new-azapplicationgatewaybackendhttpsettings).

```powershell
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

Uygulama ağ geçidinin trafiği arka uç havuzuna uygun şekilde yönlendirmesini sağlamak içn bir dinleyici gereklidir. Bu örnekte, kök URL’deki HTTPS trafiğini dinleyen temel bir dinleyici oluşturacaksınız. 

Nesnesi kullanarak bir sertifika oluşturma [yeni AzApplicationGatewaySslCertificate](/powershell/module/az.network/new-azapplicationgatewaysslcertificate) ve adlı bir dinleyici oluşturup *appGatewayHttpListener* kullanarak [ Yeni AzApplicationGatewayHttpListener](/powershell/module/az.network/new-azapplicationgatewayhttplistener) ön uç yapılandırması, ön uç bağlantı noktası ve önceden oluşturduğunuz sertifika. Dinleyicinin gelen trafik için kullanacağı arka uç havuzunu bilmesi için bir kural gerekir. Adlı bir temel kuralı oluşturun *bağlanma1* kullanarak [yeni AzApplicationGatewayRequestRoutingRule](/powershell/module/az.network/new-azapplicationgatewayrequestroutingrule).

```powershell
$pwd = ConvertTo-SecureString `
  -String "Azure123456!" `
  -Force `
  -AsPlainText
$cert = New-AzApplicationGatewaySslCertificate `
  -Name "appgwcert" `
  -CertificateFile "c:\appgwcert.pfx" `
  -Password $pwd
$defaultListener = New-AzApplicationGatewayHttpListener `
  -Name appGatewayHttpListener `
  -Protocol Https `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendPort `
  -SslCertificate $cert
$frontendRule = New-AzApplicationGatewayRequestRoutingRule `
  -Name rule1 `
  -RuleType Basic `
  -HttpListener $defaultListener `
  -BackendAddressPool $defaultPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

Gerekli destekleyici kaynakları oluşturduğunuz, adlı uygulama ağ geçidi için parametreleri belirtin *myAppGateway* kullanarak [yeni AzApplicationGatewaySku](/powershell/module/az.network/new-azapplicationgatewaysku)ve ardından kullanarakoluşturun[Yeni AzApplicationGateway](/powershell/module/az.network/new-azapplicationgateway) sertifika ile.

```powershell
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
  -FrontendPorts $frontendPort `
  -HttpListeners $defaultListener `
  -RequestRoutingRules $frontendRule `
  -Sku $sku `
  -SslCertificates $cert
```

## <a name="add-a-listener-and-redirection-rule"></a>Dinleyici ve yeniden yönlendirme kuralı Ekle

### <a name="add-the-http-port"></a>HTTP bağlantı noktası Ekle

HTTP bağlantı noktası kullanarak uygulama ağ geçidi ekleyin [Ekle AzApplicationGatewayFrontendPort](/powershell/module/az.network/add-azapplicationgatewayfrontendport).

```powershell
$appgw = Get-AzApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG
Add-AzApplicationGatewayFrontendPort `
  -Name httpPort  `
  -Port 80 `
  -ApplicationGateway $appgw
```

### <a name="add-the-http-listener"></a>HTTP dinleyicisi Ekle

Adlı bir HTTP dinleyicisi Ekle *myListener* kullanarak uygulama ağ geçidi [Ekle AzApplicationGatewayHttpListener](/powershell/module/az.network/add-azapplicationgatewayhttplistener).

```powershell
$fipconfig = Get-AzApplicationGatewayFrontendIPConfig `
  -Name myAGFrontendIPConfig `
  -ApplicationGateway $appgw
$fp = Get-AzApplicationGatewayFrontendPort `
  -Name httpPort `
  -ApplicationGateway $appgw
Add-AzApplicationGatewayHttpListener `
  -Name myListener `
  -Protocol Http `
  -FrontendPort $fp `
  -FrontendIPConfiguration $fipconfig `
  -ApplicationGateway $appgw
```

### <a name="add-the-redirection-configuration"></a>Yeniden yönlendirme yapılandırması Ekle

HTTP, HTTPS yeniden yönlendirmesi yapılandırma kullanarak uygulama ağ geçidi eklemek [Ekle AzApplicationGatewayRedirectConfiguration](/powershell/module/az.network/add-azapplicationgatewayredirectconfiguration).

```powershell
$defaultListener = Get-AzApplicationGatewayHttpListener `
  -Name appGatewayHttpListener `
  -ApplicationGateway $appgw
Add-AzApplicationGatewayRedirectConfiguration -Name httpToHttps `
  -RedirectType Permanent `
  -TargetListener $defaultListener `
  -IncludePath $true `
  -IncludeQueryString $true `
  -ApplicationGateway $appgw
```

### <a name="add-the-routing-rule"></a>Yönlendirme kuralı Ekle

Uygulama ağ geçidi kullanarak yeniden yönlendirme yapılandırması ile yönlendirme kuralı Ekle [Ekle AzApplicationGatewayRequestRoutingRule](/powershell/module/az.network/add-azapplicationgatewayrequestroutingrule).

```powershell
$myListener = Get-AzApplicationGatewayHttpListener `
  -Name myListener `
  -ApplicationGateway $appgw
$redirectConfig = Get-AzApplicationGatewayRedirectConfiguration `
  -Name httpToHttps `
  -ApplicationGateway $appgw
Add-AzApplicationGatewayRequestRoutingRule `
  -Name rule2 `
  -RuleType Basic `
  -HttpListener $myListener `
  -RedirectConfiguration $redirectConfig `
  -ApplicationGateway $appgw
Set-AzApplicationGateway -ApplicationGateway $appgw
```

## <a name="create-a-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi oluşturma

Bu örnekte uygulama ağ geçidinde arka uç havuzu için sunucu sağlayan bir sanal makine ölçek kümesi oluşturacaksınız. IP ayarlarını yapılandırırken ölçek kümesini arka uç havuzuna atayın.

```powershell
$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet
$appgw = Get-AzApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway
$backendPool = Get-AzApplicationGatewayBackendAddressPool `
  -Name appGatewayBackendPool `
  -ApplicationGateway $appgw
$ipConfig = New-AzVmssIpConfig `
  -Name myVmssIPConfig `
  -SubnetId $vnet.Subnets[1].Id `
  -ApplicationGatewayBackendAddressPoolsId $backendPool.Id
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
  -ComputerNamePrefix myvmss
Add-AzVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name myVmssNetConfig `
  -Primary $true `
  -IPConfiguration $ipConfig
New-AzVmss `
  -ResourceGroupName myResourceGroupAG `
  -Name myvmss `
  -VirtualMachineScaleSet $vmssConfig
```

### <a name="install-iis"></a>IIS yükleme

```powershell
$publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/appgatewayurl.ps1"); 
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }
$vmss = Get-AzVmss -ResourceGroupName myResourceGroupAG -VMScaleSetName myvmss
Add-AzVmssExtension -VirtualMachineScaleSet $vmss `
  -Name "customScript" `
  -Publisher "Microsoft.Compute" `
  -Type "CustomScriptExtension" `
  -TypeHandlerVersion 1.8 `
  -Setting $publicSettings
Update-AzVmss `
  -ResourceGroupName myResourceGroupAG `
  -Name myvmss `
  -VirtualMachineScaleSet $vmss
```

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

Kullanabileceğiniz [Get-AzPublicIPAddress](/powershell/module/az.network/get-azpublicipaddress) uygulama ağ geçidinin genel IP adresini almak için. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın. Örneğin, http://52.170.203.149

```powershell
Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![Güvenli uyarı](./media/redirect-http-to-https-powershell/application-gateway-secure.png)

Otomatik olarak imzalanan sertifika kullandıysanız güvenlik uyarısını kabul etmek için, **Ayrıntılar**’ı seçin ve sonra **Web sayfasına gidin**: Güvenli IIS siteniz, sonra aşağıdaki örnekte olduğu gibi görüntülenir:

![Temel URL’yi uygulama ağ geçidinde test etme](./media/redirect-http-to-https-powershell/application-gateway-iistest.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Otomatik olarak imzalanan sertifika oluşturma
> * Ağ ayarlama
> * Sertifikalı bir uygulama ağ geçidi oluşturma
> * Dinleyici ve yeniden yönlendirme kuralı Ekle
> * Varsayılan arka uç havuzuyla bir sanal makine ölçek kümesi oluşturma
