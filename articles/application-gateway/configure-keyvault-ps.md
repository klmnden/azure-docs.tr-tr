---
title: Azure PowerShell kullanarak SSL sonlandırma Key Vault ile sertifikaları yapılandırma
description: Azure Application Gateway ile Key Vault için HTTPS etkin dinleyiciler için bağlı sunucu sertifikaları nasıl tümleştirilebileceğini öğrenmek.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 4/22/2019
ms.author: victorh
ms.openlocfilehash: e011caa8c7a0c7383d16c81f4bff29d3c1c99f99
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65827621"
---
# <a name="configure-ssl-termination-with-key-vault-certificates-by-using-azure-powershell"></a>Azure PowerShell kullanarak SSL sonlandırma Key Vault ile sertifikaları yapılandırma

[Azure Key Vault](../key-vault/key-vault-whatis.md) platform yönetilen gizli dizi depoladığı gizli dizileri, anahtarlar ve SSL sertifikalarınızı korumak için kullanabilirsiniz. Azure Application Gateway, HTTPS etkin dinleyiciler için bağlı sunucu sertifikaları için (genel önizlemede) anahtar kasası ile tümleştirmeyi destekler. Bu destek, Application Gateway için v2 SKU sınırlıdır.

Daha fazla bilgi için [sertifikaları Key Vault ile SSL sonlandırma](key-vault-certs.md).

Bu makalede, application gateway SSL sonlandırma sertifikaları için anahtar kasanıza tümleştirmek için Azure PowerShell Betiği kullanmayı gösterir.

Bu makale Azure PowerShell modülü sürüm 1.0.0 gerekir veya üzeri. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). Bu makalede komutları çalıştırmak için ayrıca çalıştırarak Azure ile bir bağlantı oluşturmak için ihtiyacınız `Connect-AzAccount`.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce ManagedServiceIdentity Modülü yüklü olması gerekir:

```azurepowershell
Install-Module -Name Az.ManagedServiceIdentity
Connect-AzAccount
Select-AzSubscription -Subscription <your subscription>
```

## <a name="example-script"></a>Örnek betik

### <a name="set-up-variables"></a>Değişkenleri ayarlayın

```azurepowershell
$rgname = "KeyVaultTest"
$location = "East US"
$kv = "TestKeyVaultAppGw"
$appgwName = "AppGwKVIntegration"
```

### <a name="create-a-resource-group-and-a-user-managed-identity"></a>Bir kaynak grubu ve kullanıcı tarafından yönetilen bir kimlik oluşturma

```azurepowershell
$resourceGroup = New-AzResourceGroup -Name $rgname -Location $location
$identity = New-AzUserAssignedIdentity -Name "appgwKeyVaultIdentity" `
  -Location $location -ResourceGroupName $rgname
```

### <a name="create-a-key-vault-policy-and-certificate-to-be-used-by-the-application-gateway"></a>Bir anahtar kasası, ilke ve uygulama ağ geçidi tarafından kullanılacak bir sertifika oluşturun

```azurepowershell
$keyVault = New-AzKeyVault -Name $kv -ResourceGroupName $rgname -Location $location -EnableSoftDelete 
Set-AzKeyVaultAccessPolicy -VaultName $kv -PermissionsToSecrets get -ObjectId $identity.PrincipalId

$policy = New-AzKeyVaultCertificatePolicy -ValidityInMonths 12 `
  -SubjectName "CN=www.contoso11.com" -IssuerName self `
  -RenewAtNumberOfDaysBeforeExpiry 30
$certificate = Add-AzKeyVaultCertificate -VaultName $kv -Name "cert1" -CertificatePolicy $policy
$certificate = Get-AzKeyVaultCertificate -VaultName $kv -Name "cert1"
$secretId = $certificate.SecretId.Replace($certificate.Version, "")
```

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

```azurepowershell
$sub1 = New-AzVirtualNetworkSubnetConfig -Name "appgwSubnet" -AddressPrefix "10.0.0.0/24"
$sub2 = New-AzVirtualNetworkSubnetConfig -Name "backendSubnet" -AddressPrefix "10.0.1.0/24"
$vnet = New-AzvirtualNetwork -Name "Vnet1" -ResourceGroupName $rgname -Location $location `
  -AddressPrefix "10.0.0.0/16" -Subnet @($sub1, $sub2)
```

### <a name="create-a-static-public-virtual-ip-vip-address"></a>Statik bir genel sanal IP (VIP) adresi oluşturma

```azurepowershell
$publicip = New-AzPublicIpAddress -ResourceGroupName $rgname -name "AppGwIP" `
  -location $location -AllocationMethod Static -Sku Standard
```

### <a name="create-pool-and-front-end-ports"></a>Havuzu ve ön uç bağlantı noktası oluşturma

```azurepowershell
$gwSubnet = Get-AzVirtualNetworkSubnetConfig -Name "appgwSubnet" -VirtualNetwork $vnet

$gipconfig = New-AzApplicationGatewayIPConfiguration -Name "AppGwIpConfig" -Subnet $gwSubnet
$fipconfig01 = New-AzApplicationGatewayFrontendIPConfig -Name "fipconfig" -PublicIPAddress $publicip
$pool = New-AzApplicationGatewayBackendAddressPool -Name "pool1" `
  -BackendIPAddresses testbackend1.westus.cloudapp.azure.com, testbackend2.westus.cloudapp.azure.com
$fp01 = New-AzApplicationGatewayFrontendPort -Name "port1" -Port 443
$fp02 = New-AzApplicationGatewayFrontendPort -Name "port2" -Port 80
```

### <a name="point-the-ssl-certificate-to-your-key-vault"></a>Noktası anahtar kasanıza SSL sertifikası

```azurepowershell
$sslCert01 = New-AzApplicationGatewaySslCertificate -Name "SSLCert1" -KeyVaultSecretId $secretId
```

### <a name="create-listeners-rules-and-autoscale"></a>Dinleyiciler, kuralları ve otomatik ölçeklendirme oluşturun

```azurepowershell
$listener01 = New-AzApplicationGatewayHttpListener -Name "listener1" -Protocol Https `
  -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $sslCert01
$listener02 = New-AzApplicationGatewayHttpListener -Name "listener2" -Protocol Http `
  -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp02
$poolSetting01 = New-AzApplicationGatewayBackendHttpSettings -Name "setting1" -Port 80 `
  -Protocol Http -CookieBasedAffinity Disabled
$rule01 = New-AzApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType basic `
  -BackendHttpSettings $poolSetting01 -HttpListener $listener01 -BackendAddressPool $pool
$rule02 = New-AzApplicationGatewayRequestRoutingRule -Name "rule2" -RuleType basic `
  -BackendHttpSettings $poolSetting01 -HttpListener $listener02 -BackendAddressPool $pool
$autoscaleConfig = New-AzApplicationGatewayAutoscaleConfiguration -MinCapacity 3
$sku = New-AzApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2
```

### <a name="assign-the-user-managed-identity-to-the-application-gateway"></a>Uygulama ağ geçidine kullanıcı tarafından yönetilen kimlik atama

```azurepowershell
$appgwIdentity = New-AzApplicationGatewayIdentity -UserAssignedIdentityId $identity.Id
```

### <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

```azurepowershell
$appgw = New-AzApplicationGateway -Name $appgwName -Identity $appgwIdentity -ResourceGroupName $rgname `
  -Location $location -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting01 `
  -GatewayIpConfigurations $gipconfig -FrontendIpConfigurations $fipconfig01 `
  -FrontendPorts @($fp01, $fp02) -HttpListeners @($listener01, $listener02) `
  -RequestRoutingRules @($rule01, $rule02) -Sku $sku `
  -SslCertificates $sslCert01 -AutoscaleConfiguration $autoscaleConfig
```

## <a name="next-steps"></a>Sonraki adımlar

[SSL sonlandırma hakkında daha fazla bilgi edinin](ssl-overview.md)