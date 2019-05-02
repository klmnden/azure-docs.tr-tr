---
title: 'Öğretici: Ayrılmış bir IP adresi ile otomatik ölçeklendirme yapan ve bölgesel olarak yedekli bir uygulama ağ geçidi oluşturma - Azure PowerShell'
description: Bu öğreticide, Azure PowerShell kullanarak bir ayrılmış IP adresi içeren bir otomatik ölçeklendirme, bölgesel olarak yedekli bir uygulama ağ geçidi oluşturma konusunda bilgi edinin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: tutorial
ms.date: 2/14/2019
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 8ee43a54df019b862d1f8698363d8b0a022bdcb4
ms.sourcegitcommit: ed66a704d8e2990df8aa160921b9b69d65c1d887
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64947139"
---
# <a name="tutorial-create-an-application-gateway-that-improves-web-application-access"></a>Öğretici: Web uygulaması erişimi geliştiren bir uygulama ağ geçidi oluşturma

Web uygulama erişimi geliştirme ile ilgili bir BT yöneticisi iseniz, uygulama ağ geçidinizin bağlı olarak müşteri ölçeklendirme için en iyi duruma getirebilirsiniz talep ve birden fazla kullanılabilirlik yayabilirsiniz. Bu öğreticide bunu Azure Application Gateway özelliklerini yapılandırmanıza yardımcı olur: otomatik ölçeklendirme, yedeklilik bölge ve ayrılmış VIP (statik IP). Sorunu çözmek için Azure PowerShell cmdlet'leri ve Azure Resource Manager dağıtım modeli kullanacaksınız.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Otomatik olarak imzalanan sertifika oluşturma
> * Otomatik ölçeklendirme sanal ağ oluşturma
> * Ayrılmış genel IP adresi oluşturma
> * Uygulama ağ geçidi altyapısını kurma
> * Otomatik ölçeklendirmeyi belirtme
> * Uygulama ağ geçidi oluşturma
> * Uygulama ağ geçidini test etme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu öğretici için Azure PowerShell’i yerel olarak çalıştırmanız gerekir. Sonraki bir sürümünün yüklü veya Azure PowerShell modülü sürüm 1.0.0 olmalıdır. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps). PowerShell sürümünü doğruladıktan sonra, Azure ile bağlantı oluşturmak için `Connect-AzAccount` komutunu çalıştırın.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

```azurepowershell
Connect-AzAccount
Select-AzSubscription -Subscription "<sub name>"
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Kullanılabilir konumlardan birinde bir kaynak grubu oluşturun.

```azurepowershell
$location = "East US 2"
$rg = "AppGW-rg"

#Create a new Resource Group
New-AzResourceGroup -Name $rg -Location $location
```

## <a name="create-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika oluşturma

Üretim sırasında kullanım için, güvenilen bir sağlayıcı tarafından imzalanan geçerli bir sertifikayı içeri aktarmalısınız. Bu öğretici için [New-SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/new-selfsignedcertificate) komutunu kullanarak otomatik olarak imzalanan bir sertifika oluşturursunuz. Sertifikadan pfx dosyası dışarı aktarmak için döndürülen Parmak izi ile [Export-PfxCertificate](https://docs.microsoft.com/powershell/module/pkiclient/export-pfxcertificate) komutunu kullanabilirsiniz.

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

IP yapılandırması, ön uç IP yapılandırması, arka uç havuzu, HTTP ayarları, sertifika, bağlantı noktası, dinleyici ve kuralı mevcut standart uygulama ağ geçidi için aynı biçimde yapılandırın. Yeni SKU, standart SKU ile aynı nesne modelini izler.

```azurepowershell
$ipconfig = New-AzApplicationGatewayIPConfiguration -Name "IPConfig" -Subnet $gwSubnet
$fip = New-AzApplicationGatewayFrontendIPConfig -Name "FrontendIPCOnfig" -PublicIPAddress $publicip
$pool = New-AzApplicationGatewayBackendAddressPool -Name "Pool1" `
       -BackendIPAddresses testbackend1.westus.cloudapp.azure.com, testbackend2.westus.cloudapp.azure.com
$fp01 = New-AzApplicationGatewayFrontendPort -Name "SSLPort" -Port 443
$fp02 = New-AzApplicationGatewayFrontendPort -Name "HTTPPort" -Port 80

$securepfxpwd = ConvertTo-SecureString -String "Azure123456!" -AsPlainText -Force
$sslCert01 = New-AzApplicationGatewaySslCertificate -Name "SSLCert" -Password $securepfxpwd `
            -CertificateFile "c:\appgwcert.pfx"
$listener01 = New-AzApplicationGatewayHttpListener -Name "SSLListener" `
             -Protocol Https -FrontendIPConfiguration $fip -FrontendPort $fp01 -SslCertificate $sslCert01
$listener02 = New-AzApplicationGatewayHttpListener -Name "HTTPListener" `
             -Protocol Http -FrontendIPConfiguration $fip -FrontendPort $fp02

$setting = New-AzApplicationGatewayBackendHttpSettings -Name "BackendHttpSetting1" `
          -Port 80 -Protocol Http -CookieBasedAffinity Disabled
$rule01 = New-AzApplicationGatewayRequestRoutingRule -Name "Rule1" -RuleType basic `
         -BackendHttpSettings $setting -HttpListener $listener01 -BackendAddressPool $pool
$rule02 = New-AzApplicationGatewayRequestRoutingRule -Name "Rule2" -RuleType basic `
         -BackendHttpSettings $setting -HttpListener $listener02 -BackendAddressPool $pool
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
$appgw = New-AzApplicationGateway -Name "AutoscalingAppGw" -Zone 1,2,3 `
  -ResourceGroupName $rg -Location $location -BackendAddressPools $pool `
  -BackendHttpSettingsCollection $setting -GatewayIpConfigurations $ipconfig `
  -FrontendIpConfigurations $fip -FrontendPorts $fp01, $fp02 `
  -HttpListeners $listener01, $listener02 -RequestRoutingRules $rule01, $rule02 `
  -Sku $sku -sslCertificates $sslCert01 -AutoscaleConfiguration $autoscaleConfig
```

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

Uygulama ağ geçidinin genel IP adresini almak için get-AzPublicIPAddress kullanın. Genel IP adresini veya DNS adını kopyalayıp tarayıcınızın adres çubuğuna yapıştırın.

`Get-AzPublicIPAddress -ResourceGroupName $rg -Name AppGwVIP`

## <a name="clean-up-resources"></a>Kaynakları temizleme

İlk uygulama ağ geçidi ile oluşturulan kaynakları keşfedin. Ardından, artık ihtiyaç duyulan, kullanabileceğiniz `Remove-AzResourceGroup` komutunu kullanarak kaynak grubunu, uygulama ağ geçidini kaldırmak için ve tüm ilgili kaynakları.

`Remove-AzResourceGroup -Name $rg`

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [URL yolu tabanlı yönlendirme kuralları ile bir uygulama ağ geçidi oluşturma](./tutorial-url-route-powershell.md)
