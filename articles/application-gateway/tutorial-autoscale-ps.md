---
title: Ayrılmış bir IP adresi ile otomatik ölçeklendirme yapan ve bölgesel olarak yedekli bir uygulama ağ geçidi oluşturma - Azure PowerShell
description: Azure Powershell kullanarak ayrılmış bir IP adresi ile otomatik ölçeklendirme yapan ve bölgesel olarak yedekli bir uygulama ağ geçidi oluşturmayı öğrenin.
services: application-gateway
author: amitsriva
ms.service: application-gateway
ms.topic: tutorial
ms.date: 9/26/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: d86ce2e1bac2fb58df8df748381a00eac21e65cb
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48016943"
---
# <a name="tutorial-create-an-autoscaling-zone-redundant-application-gateway-with-a-reserved-virtual-ip-address-using-azure-powershell"></a>Öğretici: Azure PowerShell kullanarak ayrılmış bir IP adresi ile otomatik ölçeklendirme yapan ve bölgesel olarak yedekli bir uygulama ağ geçidi oluşturma

Bu öğretici, Azure PowerShell cmdlet'lerini ve Azure Resource Manager dağıtım modelini kullanarak nasıl bir Azure Application Gateway oluşturulacağını açıklar. Bu öğretici, yeni Otomatik Ölçeklendirme SKU’su ile mevcut Standart SKU arasındaki farkları ele alır. Özellikle otomatik ölçeklendirmeyi, bölgesel yedekliliği ve ayrılmış VIP’leri (statik IP) destekleme özelliklerine odaklanır.

Uygulama ağ geçidi otomatik ölçeklendirmesi ve bölgesel yedekliliği hakkında daha fazla bilgi için bkz. [Otomatik Ölçeklendirme Yapan ve Bölgesel Olarak Yedekli Application Gateway (Genel Önizleme)](application-gateway-autoscaling-zone-redundant.md).

> [!IMPORTANT]
> Otomatik ölçeklendirme yapan ve bölgesel olarak yedekli uygulama ağ geçidi SKU'su şu anda genel önizleme aşamasındadır. Bu önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Otomatik ölçeklendirme yapılandırma parametresini ayarlama
> * Bölge parametresini kullanma
> * Statik VIP'yi kullanma
> * Uygulama ağ geçidi oluşturma


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu öğretici için Azure PowerShell’i yerel olarak çalıştırmanız gerekir. Azure PowerShell modülünün 6.9.0 veya daha sonraki bir sürümünün yüklü olması gerekir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). PowerShell sürümünü doğruladıktan sonra, Azure ile bağlantı oluşturmak için `Login-AzureRmAccount` komutunu çalıştırın.

## <a name="sign-in-to-your-azure-account"></a>Azure hesabınızda oturum açma

```azurepowershell
Connect-AzureRmAccount
Select-AzureRmSubscription -Subscription "<sub name>"
```
## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Kullanılabilir konumlardan birinde bir kaynak grubu oluşturun.

```azurepowershell
$location = "East US 2"
$rg = "<rg name>"

#Create a new Resource Group
New-AzureRmResourceGroup -Name $rg -Location $location
```

## <a name="create-a-vnet"></a>Sanal ağ oluşturma
Otomatik ölçeklendirme yapan bir uygulama ağ geçidi için ayrılmış bir alt ağı olan sanal ağ oluşturun. Şu anda her ayrılmış alt ağda yalnızca bir otomatik ölçeklendirme yapan uygulama ağ geçidi dağıtılabilir.

```azurepowershell
#Create VNet with two subnets
$sub1 = New-AzureRmVirtualNetworkSubnetConfig -Name "AppGwSubnet" -AddressPrefix "10.0.0.0/24"
$sub2 = New-AzureRmVirtualNetworkSubnetConfig -Name "BackendSubnet" -AddressPrefix "10.0.1.0/24"
$vnet = New-AzureRmvirtualNetwork -Name "AutoscaleVNet" -ResourceGroupName $rg `
       -Location $location -AddressPrefix "10.0.0.0/16" -Subnet $sub1, $sub2
```

## <a name="create-a-reserved-public-ip"></a>Ayrılmış genel IP adresi oluşturma

PublicIPAddress ayırma yöntemini **Statik** olarak belirleme. Otomatik ölçeklendirme yapan uygulama ağ geçidi VIP’si yalnızca statik olabilir. Dinamik IP’ler desteklenmez. Yalnızca standart PublicIpAddress SKU’su desteklenir.

```azurepowershell
#Create static public IP
$pip = New-AzureRmPublicIpAddress -ResourceGroupName $rg -name "AppGwVIP" `
       -location $location -AllocationMethod Static -Sku Standard
```

## <a name="retrieve-details"></a>Ayrıntıları alma

Uygulama ağ geçidi IP yapılandırması ayrıntılarını oluşturmak için yerel bir nesnede kaynak grubu, alt ağ ve IP ayrıntılarını alın.

```azurepowershell
$resourceGroup = Get-AzureRmResourceGroup -Name $rg
$publicip = Get-AzureRmPublicIpAddress -ResourceGroupName $rg -name "AppGwVIP"
$vnet = Get-AzureRmvirtualNetwork -Name "AutoscaleVNet" -ResourceGroupName $rg
$gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "AppGwSubnet" -VirtualNetwork $vnet
```
## <a name="configure-application-gateway-infrastructure"></a>Uygulama ağ geçidi altyapısı yapılandırma
IP yapılandırması, ön uç IP yapılandırması, arka uç havuzu, http ayarları, sertifika, bağlantı noktası, dinleyici ve kuralı mevcut Standart Application Gateway’e aynı biçimde yapılandırın. Yeni SKU, standart SKU ile aynı nesne modelini izler.

```azurepowershell
$ipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name "IPConfig" -Subnet $gwSubnet
$fip = New-AzureRmApplicationGatewayFrontendIPConfig -Name "FrontendIPCOnfig" -PublicIPAddress $publicip
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name "Pool1" `
       -BackendIPAddresses testbackend1.westus.cloudapp.azure.com, testbackend2.westus.cloudapp.azure.com
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "SSLPort" -Port 443
$fp02 = New-AzureRmApplicationGatewayFrontendPort -Name "HTTPPort" -Port 80

$securepfxpwd = ConvertTo-SecureString -String "scrap" -AsPlainText -Force
$sslCert01 = New-AzureRmApplicationGatewaySslCertificate -Name "SSLCert" -Password $securepfxpwd `
            -CertificateFile "D:\Networking\ApplicationGateway\scrap.pfx"
$listener01 = New-AzureRmApplicationGatewayHttpListener -Name "SSLListener" `
             -Protocol Https -FrontendIPConfiguration $fip -FrontendPort $fp01 -SslCertificate $sslCert01
$listener02 = New-AzureRmApplicationGatewayHttpListener -Name "HTTPListener" `
             -Protocol Http -FrontendIPConfiguration $fip -FrontendPort $fp02

$setting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "BackendHttpSetting1" `
          -Port 80 -Protocol Http -CookieBasedAffinity Disabled
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "Rule1" -RuleType basic `
         -BackendHttpSettings $setting -HttpListener $listener01 -BackendAddressPool $pool
$rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "Rule2" -RuleType basic `
         -BackendHttpSettings $setting -HttpListener $listener02 -BackendAddressPool $pool
```

## <a name="specify-autoscale"></a>Otomatik ölçeklendirmeyi belirtme

Artık uygulama ağ geçidi için otomatik ölçeklendirme yapılandırmasını belirtebilirsiniz. İki otomatik ölçeklendirme yapılandırması türü desteklenir:

- **Sabit kapasite modu**. Bu modda, uygulama ağ geçidi otomatik ölçeklendirme yapmaz ve sabit bir Ölçek Birimi kapasitesinde çalışır.

   ```azurepowershell
   $sku = New-AzureRmApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2 -Capacity 2
   ```
- **Otomatik ölçeklendirme modu**. Bu modda, uygulama ağ geçidi uygulamanın trafik desenine bağlı olarak otomatik ölçeklendirme yapar.

   ```azurepowershell
   $autoscaleConfig = New-AzureRmApplicationGatewayAutoscaleConfiguration -MinCapacity 2
   $sku = New-AzureRmApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2
   ```

## <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

Application Gateway oluşturun ve yedeklilik bölgelerini dahil edin. 

Bölge yapılandırması yalnızca Azure Bölgeleri’nin kullanılabilir olduğu bölgelerde desteklenir. Azure Bölgeleri’nin kullanılabilir olmadığı bölgelerde alan parametresi kullanılmamalıdır. Bir uygulama ağ geçidi, tek bir bölgede, iki bölgede veya üç bölgenin tümünde de dağıtılabilir. Tek bir bölge uygulama ağ geçidi için PublicIPAddress aynı bölgeye bağlı olmalıdır. İki veya üç bölgesel olarak yedekli uygulama ağ geçidi için PublicIPAddress de bölgesel olarak yedekli olmalıdır, bu nedenle belirtilen bölge yoktur.

```azurepowershell
$appgw = New-AzureRmApplicationGateway -Name "AutoscalingAppGw" -Zone 1,2,3 `
  -ResourceGroupName $rg -Location $location -BackendAddressPools $pool `
  -BackendHttpSettingsCollection $setting -GatewayIpConfigurations $ipconfig `
  -FrontendIpConfigurations $fip -FrontendPorts $fp01, $fp02 `
  -HttpListeners $listener01, $listener02 -RequestRoutingRules $rule01, $rule02 `
  -Sku $sku -sslCertificates $sslCert01 -AutoscaleConfiguration $autoscaleConfig
```

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

Uygulama ağ geçidinin genel IP adresini almak için [Get-AzureRmPublicIPAddress](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermpublicipaddress) komutunu kullanın. Genel IP adresini veya DNS adını kopyalayıp tarayıcınızın adres çubuğuna yapıştırın.

`Get-AzureRmPublicIPAddress -ResourceGroupName $rg -Name AppGwVIP`

## <a name="clean-up-resources"></a>Kaynakları temizleme
İlk olarak uygulama ağ geçidiyle oluşturulan kaynakları keşfedin ve ardından artık gerekmediğinde kaynak grubu, uygulama ağ geçidi ve tüm ilgili kaynakları kaldırmak için `Remove-AzureRmResourceGroup` komutunu kullanabilirsiniz.

`Remove-AzureRmResourceGroup -Name $rg`

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Statik VIP'yi kullanma
> * Otomatik ölçeklendirme yapılandırma parametresini ayarlama
> * Bölge parametresini kullanma
> * Uygulama ağ geçidi oluşturma

> [!div class="nextstepaction"]
> [URL yolu tabanlı yönlendirme kuralları ile bir uygulama ağ geçidi oluşturma](./tutorial-url-route-powershell.md)
