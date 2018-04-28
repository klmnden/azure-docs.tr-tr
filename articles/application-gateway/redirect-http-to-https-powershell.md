---
title: Bir uygulama ağ geçidi ile HTTP HTTPS yeniden yönlendirmesi - Azure PowerShell oluşturun. | Microsoft Docs
description: Azure PowerShell kullanarak HTTPS HTTP yeniden yönlendirilen trafiği ile bir uygulama ağ geçidi oluşturmayı öğrenin.
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/23/2018
ms.author: davidmu
ms.openlocfilehash: 01e22ba6e15b526602e9c07800bdcb422984bbff
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="create-an-application-gateway-with-http-to-https-redirection-using-azure-powershell"></a>Bir uygulama ağ geçidi ile HTTP Azure PowerShell kullanarak HTTPS yeniden yönlendirmesi için oluşturma

Azure PowerShell oluşturmak için kullanabileceğiniz bir [uygulama ağ geçidi](overview.md) SSL sonlandırma için bir sertifika ile. Yönlendirme kuralı HTTPS bağlantı noktası uygulama ağ geçidiniz için HTTP trafiği yönlendirmek için kullanılır. Bu örnekte, ayrıca oluşturduğunuz bir [sanal makine ölçek kümesi](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) iki sanal makine örneklerini içeren uygulama ağ geçidi arka uç havuzu için. 

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Otomatik olarak imzalanan sertifika oluşturma
> * Ağ kurma
> * Sertifika ile bir uygulama ağ geçidi oluşturma
> * Dinleyici ve yeniden yönlendirme kuralı Ekle
> * Bir sanal makineyi ölçeği varsayılan arka uç havuzuyla Ayarla oluşturma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu öğretici, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). Bu öğreticide komutları çalıştırmak için de çalıştırmak ihtiyacınız `Login-AzureRmAccount` Azure ile bir bağlantı oluşturmak için.

## <a name="create-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika oluşturma

Üretim kullanımı için güvenilen bir sağlayıcı tarafından imzalanmış geçerli bir sertifika almanız gerekir. Bu öğretici için kullanarak otomatik olarak imzalanan sertifika oluşturma [yeni SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/new-selfsignedcertificate). Kullanabileceğiniz [verme PfxCertificate](https://docs.microsoft.com/powershell/module/pkiclient/export-pfxcertificate) bir pfx dosyası sertifikadan dışarı aktarmak için döndürülen parmak izine sahip.

```powershell
New-SelfSignedCertificate `
  -certstorelocation cert:\localmachine\my `
  -dnsname www.contoso.com
```

Bu sonuç gibi bir şey görmeniz gerekir:

```
PSParentPath: Microsoft.PowerShell.Security\Certificate::LocalMachine\my

Thumbprint                                Subject
----------                                -------
E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630  CN=www.contoso.com
```

Parmak izi pfx dosyasını oluşturmak için kullanın:

```powershell
$pwd = ConvertTo-SecureString -String "Azure123456!" -Force -AsPlainText
Export-PfxCertificate `
  -cert cert:\localMachine\my\E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630 `
  -FilePath c:\appgwcert.pfx `
  -Password $pwd
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Adlı bir Azure kaynak grubu oluşturma *myResourceGroupAG* kullanarak [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). 

```powershell
New-AzureRmResourceGroup -Name myResourceGroupAG -Location eastus
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma

Alt ağ yapılandırmalarını oluşturma *myBackendSubnet* ve *myAGSubnet* kullanarak [yeni AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Adlı sanal ağ oluşturma *myVNet* kullanarak [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) alt ağ yapılandırmaları ile. Ve son olarak, adlı ortak IP adresi oluşturun *myAGPublicIPAddress* kullanarak [yeni AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress). Bu kaynaklar, uygulama ağ geçidi ve onun ilişkili kaynakları için ağ bağlantısı sağlamak için kullanılır.

```powershell
$backendSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.1.0/24
$agSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.2.0/24
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $backendSubnetConfig, $agSubnetConfig
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

### <a name="create-the-ip-configurations-and-frontend-port"></a>IP yapılandırmaları ve ön uç bağlantı noktası oluşturma

İlişkilendirme *myAGSubnet* daha önce oluşturduğunuz uygulama ağ geçidi kullanarak [yeni AzureRmApplicationGatewayIPConfiguration](/powershell/module/azurerm.network/new-azurermapplicationgatewayipconfiguration). Ata *myAGPublicIPAddress* uygulama ağ geçidi kullanarak [yeni AzureRmApplicationGatewayFrontendIPConfig](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendipconfig). Ve ardından HTTPS bağlantı noktası kullanarak oluşturabilirsiniz [yeni AzureRmApplicationGatewayFrontendPort](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendport).

```powershell
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
$frontendPort = New-AzureRmApplicationGatewayFrontendPort `
  -Name myFrontendPort `
  -Port 443
```

### <a name="create-the-backend-pool-and-settings"></a>Arka uç havuzu ve ayarları oluşturma

Adlı arka uç havuzu oluşturma *appGatewayBackendPool* kullanarak uygulama ağ geçidi için [yeni AzureRmApplicationGatewayBackendAddressPool](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendaddresspool). Kullanarak arka uç havuzu ayarlarını yapılandırmak [yeni AzureRmApplicationGatewayBackendHttpSettings](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendhttpsettings).

```powershell
$defaultPool = New-AzureRmApplicationGatewayBackendAddressPool `
  -Name appGatewayBackendPool 
$poolSettings = New-AzureRmApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-default-listener-and-rule"></a>Varsayılan dinleyici ve kural oluşturma

Dinleyici için arka uç havuzu uygun şekilde trafiği yönlendirmek uygulama ağ geçidi etkinleştirmek için gereklidir. Bu örnekte, kök URL'sindeki HTTPS trafiğini dinleyen temel bir dinleyici oluşturun. 

Kullanarak bir sertifika nesnesi oluşturmak [yeni AzureRmApplicationGatewaySslCertificate](/powershell/module/azurerm.network/new-azurermapplicationgatewaysslcertificate) ve adlı bir dinleyici oluşturun *appGatewayHttpListener* kullanarak [ AzureRmApplicationGatewayHttpListener yeni](/powershell/module/azurerm.network/new-azurermapplicationgatewayhttplistener) ön uç yapılandırma, ön uç bağlantı noktası ve önceden oluşturduğunuz sertifika ile. Bir kural hangi arka uç havuzuna gelen trafiği için kullanılacak bilmeniz dinleyici için gereklidir. Adlı temel bir kural oluşturmak *kuralı 1* kullanarak [yeni AzureRmApplicationGatewayRequestRoutingRule](/powershell/module/azurerm.network/new-azurermapplicationgatewayrequestroutingrule).

```powershell
$pwd = ConvertTo-SecureString `
  -String "Azure123456!" `
  -Force `
  -AsPlainText
$cert = New-AzureRmApplicationGatewaySslCertificate `
  -Name "appgwcert" `
  -CertificateFile "c:\appgwcert.pfx" `
  -Password $pwd
$defaultListener = New-AzureRmApplicationGatewayHttpListener `
  -Name appGatewayHttpListener `
  -Protocol Https `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendPort `
  -SslCertificate $cert
$frontendRule = New-AzureRmApplicationGatewayRequestRoutingRule `
  -Name rule1 `
  -RuleType Basic `
  -HttpListener $defaultListener `
  -BackendAddressPool $defaultPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

Gerekli destekleyici kaynakları oluşturduğunuza göre adlı uygulama ağ geçidi için parametre belirtin *myAppGateway* kullanarak [yeni AzureRmApplicationGatewaySku](/powershell/module/azurerm.network/new-azurermapplicationgatewaysku)ve kullanarakoluşturma[ Yeni-AzureRmApplicationGateway](/powershell/module/azurerm.network/new-azurermapplicationgateway) sertifikasıyla.

```powershell
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
  -FrontendPorts $frontendPort `
  -HttpListeners $defaultListener `
  -RequestRoutingRules $frontendRule `
  -Sku $sku `
  -SslCertificates $cert
```

## <a name="add-a-listener-and-redirection-rule"></a>Dinleyici ve yeniden yönlendirme kuralı Ekle

### <a name="add-the-http-port"></a>HTTP bağlantı noktası ekleme

HTTP bağlantı noktası kullanarak uygulama ağ geçidi eklemek [Ekle AzureRmApplicationGatewayFrontendPort](/powershell/module/azurerm.network/add-azurermapplicationgatewayfrontendport).

```powershell
$appgw = Get-AzureRmApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG
Add-AzureRmApplicationGatewayFrontendPort `
  -Name httpPort  `
  -Port 80 `
  -ApplicationGateway $appgw
```

### <a name="add-the-http-listener"></a>HTTP dinleyicisi ekleme

Adlı HTTP dinleyicisi eklemek *myListener* uygulama ağ geçidi kullanarak [Ekle AzureRmApplicationGatewayHttpListener](/powershell/module/azurerm.network/add-azurermapplicationgatewayhttplistener).

```powershell
$fipconfig = Get-AzureRmApplicationGatewayFrontendIPConfig `
  -Name myAGFrontendIPConfig `
  -ApplicationGateway $appgw
$fp = Get-AzureRmApplicationGatewayFrontendPort `
  -Name httpPort `
  -ApplicationGateway $appgw
Add-AzureRmApplicationGatewayHttpListener `
  -Name myListener `
  -Protocol Http `
  -FrontendPort $fp `
  -FrontendIPConfiguration $fipconfig `
  -ApplicationGateway $appgw
```

### <a name="add-the-redirection-configuration"></a>Yeniden yönlendirme yapılandırması Ekle

Uygulama ağ geçidi kullanarak HTTPS yeniden yönlendirme yapılandırması HTTP eklemek [Ekle AzureRmApplicationGatewayRedirectConfiguration](/powershell/module/azurerm.network/add-azurermapplicationgatewayredirectconfiguration).

```powershell
$defaultListener = Get-AzureRmApplicationGatewayHttpListener `
  -Name appGatewayHttpListener `
  -ApplicationGateway $appgw
Add-AzureRmApplicationGatewayRedirectConfiguration -Name httpToHttps `
  -RedirectType Permanent `
  -TargetListener $defaultListener `
  -IncludePath $true `
  -IncludeQueryString $true `
  -ApplicationGateway $appgw
```

### <a name="add-the-routing-rule"></a>Yönlendirme kuralı Ekle

Uygulama ağ geçidi kullanarak yönlendirme yapılandırması ile yönlendirme kuralı Ekle [Ekle AzureRmApplicationGatewayRequestRoutingRule](/powershell/module/azurerm.network/add-azurermapplicationgatewayrequestroutingrule).

```powershell
$myListener = Get-AzureRmApplicationGatewayHttpListener `
  -Name myListener `
  -ApplicationGateway $appgw
$redirectConfig = Get-AzureRmApplicationGatewayRedirectConfiguration `
  -Name httpToHttps `
  -ApplicationGateway $appgw
Add-AzureRmApplicationGatewayRequestRoutingRule `
  -Name rule2 `
  -RuleType Basic `
  -HttpListener $myListener `
  -RedirectConfiguration $redirectConfig `
  -ApplicationGateway $appgw
Set-AzureRmApplicationGateway -ApplicationGateway $appgw
```

## <a name="create-a-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi oluşturma

Bu örnekte uygulama ağ geçidi arka uç havuzu için sunucuları sağlamak için ayarlanmış bir sanal makine ölçek oluşturun. Ölçeği IP ayarlarını yapılandırdığınızda arka uç havuzuna Ayarla atayın.

```powershell
$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet
$appgw = Get-AzureRmApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway
$backendPool = Get-AzureRmApplicationGatewayBackendAddressPool `
  -Name appGatewayBackendPool `
  -ApplicationGateway $appgw
$ipConfig = New-AzureRmVmssIpConfig `
  -Name myVmssIPConfig `
  -SubnetId $vnet.Subnets[1].Id `
  -ApplicationGatewayBackendAddressPoolsId $backendPool.Id
$vmssConfig = New-AzureRmVmssConfig `
  -Location eastus `
  -SkuCapacity 2 `
  -SkuName Standard_DS2 `
  -UpgradePolicyMode Automatic
Set-AzureRmVmssStorageProfile $vmssConfig `
  -ImageReferencePublisher MicrosoftWindowsServer `
  -ImageReferenceOffer WindowsServer `
  -ImageReferenceSku 2016-Datacenter `
  -ImageReferenceVersion latest
Set-AzureRmVmssOsProfile $vmssConfig `
  -AdminUsername azureuser `
  -AdminPassword "Azure123456!" `
  -ComputerNamePrefix myvmss
Add-AzureRmVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name myVmssNetConfig `
  -Primary $true `
  -IPConfiguration $ipConfig
New-AzureRmVmss `
  -ResourceGroupName myResourceGroupAG `
  -Name myvmss `
  -VirtualMachineScaleSet $vmssConfig
```

### <a name="install-iis"></a>IIS yükleme

```powershell
$publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/davidmu1/samplescripts/master/appgatewayurl.ps1"); 
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }
$vmss = Get-AzureRmVmss -ResourceGroupName myResourceGroupAG -VMScaleSetName myvmss
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss `
  -Name "customScript" `
  -Publisher "Microsoft.Compute" `
  -Type "CustomScriptExtension" `
  -TypeHandlerVersion 1.8 `
  -Setting $publicSettings
Update-AzureRmVmss `
  -ResourceGroupName myResourceGroupAG `
  -Name myvmss `
  -VirtualMachineScaleSet $vmss
```

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidi sınama

Kullanabileceğiniz [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) uygulama ağ geçidi genel IP adresi alınamıyor. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın. Örneğin, http://52.170.203.149

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![Güvenli uyarı](./media/redirect-http-to-https-powershell/application-gateway-secure.png)

Kendinden imzalı bir sertifika kullanıyorsa uyarı güvenlik kabul etmeyi seçin **ayrıntıları** ve ardından **Web sayfasına gidin**. Güvenli IIS siteniz, sonra aşağıdaki örnekte olduğu gibi görüntülenir:

![Temel uygulama ağ geçidi URL'de test](./media/redirect-http-to-https-powershell/application-gateway-iistest.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Otomatik olarak imzalanan sertifika oluşturma
> * Ağ kurma
> * Sertifika ile bir uygulama ağ geçidi oluşturma
> * Dinleyici ve yeniden yönlendirme kuralı Ekle
> * Bir sanal makineyi ölçeği varsayılan arka uç havuzuyla Ayarla oluşturma
