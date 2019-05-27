---
title: Özel bir araştırma - Azure Application Gateway - PowerShell oluşturma | Microsoft Docs
description: Resource Manager'da PowerShell kullanarak Application Gateway için özel bir araştırma oluşturmayı öğrenin
services: application-gateway
documentationcenter: na
author: vhorne
manager: jpconnock
editor: ''
tags: azure-resource-manager
ms.assetid: 68feb660-7fa4-4f69-a7e4-bdd7bdc474db
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: victorh
ms.openlocfilehash: acd70bacd23755cd764bc782a297d80db3622424
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66135238"
---
# <a name="create-a-custom-probe-for-azure-application-gateway-by-using-powershell-for-azure-resource-manager"></a>Azure Resource Manager için PowerShell kullanarak Azure Application Gateway için özel bir araştırma oluşturma

> [!div class="op_single_selector"]
> * [Azure portal](application-gateway-create-probe-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-probe-ps.md)
> * [Azure Klasik PowerShell](application-gateway-create-probe-classic-ps.md)

Bu makalede PowerShell ile mevcut bir application gateway için özel bir araştırma ekleyin. Özel araştırmalar, belirli bir sistem durumu denetimi sayfası olan uygulamalar için veya başarılı bir yanıt varsayılan web uygulaması üzerinde sağlamayan uygulamalar için yararlıdır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway-with-a-custom-probe"></a>Application gateway ile özel bir araştırma oluşturma

### <a name="sign-in-and-create-resource-group"></a>Oturum açma ve kaynak grubu oluşturma

1. Kullanım `Connect-AzAccount` kimliğini doğrulamak için.

   ```powershell
   Connect-AzAccount
   ```

1. Hesap aboneliklerini edinin.

   ```powershell
   Get-AzSubscription
   ```

1. Hangi Azure aboneliğinizin kullanılacağını seçin.

   ```powershell
   Select-AzSubscription -Subscriptionid '{subscriptionGuid}'
   ```

1. Bir kaynak grubu oluşturun. Mevcut bir kaynak grubu varsa, bu adımı atlayabilirsiniz.

   ```powershell
   New-AzResourceGroup -Name appgw-rg -Location 'West US'
   ```

Azure Resource Manager, tüm kaynak gruplarının bir konum belirtmesini gerektirir. Bu konum, kaynak grubundaki kaynaklar için varsayılan konum olarak kullanılır. Tüm uygulama ağ geçidi oluşturma komutlarının aynı kaynak grubunu kullandığından emin olun.

Önceki örnekte adlı bir kaynak grubu oluşturduk **appgw-RG** konumda **Batı ABD**.

### <a name="create-a-virtual-network-and-a-subnet"></a>Sanal ağ ve alt ağ oluşturma

Aşağıdaki örnek, bir sanal ağ ve uygulama ağ geçidi için bir alt ağ oluşturur. Uygulama ağ geçidini kullanmak için kendi alt ağına gerektirir. Bu nedenle, uygulama ağ geçidi için oluşturulan alt ağı oluşturulması ve kullanılan diğer alt ağlar için izin vermek için sanal ağın adres alanı değerinden küçük olmalıdır.

```powershell
# Assign the address range 10.0.0.0/24 to a subnet variable to be used to create a virtual network.
$subnet = New-AzVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

# Create a virtual network named appgwvnet in resource group appgw-rg for the West US region using the prefix 10.0.0.0/16 with subnet 10.0.0.0/24.
$vnet = New-AzVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet

# Assign a subnet variable for the next steps, which create an application gateway.
$subnet = $vnet.Subnets[0]
```

### <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Ön uç yapılandırma için genel bir IP adresi oluşturun

Batı ABD bölgesi için **appgw-rg** kaynak grubunda **publicIP01** genel bir IP kaynağı oluşturun. Bu örnekte uygulama ağ geçidi ön uç IP adresi için bir genel IP adresi kullanır.  Uygulama ağ geçidi gerektirir, bu nedenle dinamik olarak oluşturulmuş bir DNS adı sağlamak için genel IP adresi `-DomainNameLabel` genel IP adresi oluşturma sırasında belirtilemez.

```powershell
$publicip = New-AzPublicIpAddress -ResourceGroupName appgw-rg -Name publicIP01 -Location 'West US' -AllocationMethod Dynamic
```

### <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Tüm yapılandırma öğelerini ayarlamanız, uygulama ağ geçidi oluşturmadan önce ayarlayın. Aşağıdaki örnek bir uygulama ağ geçidi kaynağı için gerekli olan yapılandırma öğelerini oluşturur.

| **Bileşen** | **Açıklama** |
|---|---|
| **Ağ geçidi IP yapılandırması** | Bir uygulama ağ geçidi için bir IP yapılandırması.|
| **Arka uç havuzu** | IP adresleri, FQDN'ın veya web uygulamasını barındıran uygulama sunucuları için NIC havuzu|
| **Durum araştırması** | Arka uç havuzu üyelerine durumunu izlemek için kullanılan özel bir araştırma|
| **HTTP ayarları** | Ayarları dahil olmak üzere, bağlantı noktası, protokol, tanımlama bilgisi temelli benzeşimi, araştırma ve zaman aşımı koleksiyonu.  Bu ayarlar, trafik için arka uç havuzu üyelerine nasıl yönlendirileceğini belirler.|
| **Ön uç bağlantı noktası** | Uygulama ağ geçidi üzerinde trafiği dinlediği bağlantı noktası|
| **Dinleyici** | Bir protokol, ön uç IP yapılandırması ve ön uç bağlantı noktası birleşimi. Gelen istekler için dinleyen ne budur.
|**Kural**| Yollar, trafik uygun arka uç HTTP ayarları temel alarak.|

```powershell
# Creates an application gateway Frontend IP configuration named gatewayIP01
$gipconfig = New-AzApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

#Creates a back-end IP address pool named pool01 with IP addresses 134.170.185.46, 134.170.188.221, 134.170.185.50.
$pool = New-AzApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

# Creates a probe that will check health at http://contoso.com/path/path.htm
$probe = New-AzApplicationGatewayProbeConfig -Name probe01 -Protocol Http -HostName 'contoso.com' -Path '/path/path.htm' -Interval 30 -Timeout 120 -UnhealthyThreshold 8

# Creates the backend http settings to be used. This component references the $probe created in the previous command.
$poolSetting = New-AzApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 80

# Creates a frontend port for the application gateway to listen on port 80 that will be used by the listener.
$fp = New-AzApplicationGatewayFrontendPort -Name frontendport01 -Port 80

# Creates a frontend IP configuration. This associates the $publicip variable defined previously with the front-end IP that will be used by the listener.
$fipconfig = New-AzApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

# Creates the listener. The listener is a combination of protocol and the frontend IP configuration $fipconfig and frontend port $fp created in previous steps.
$listener = New-AzApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

# Creates the rule that routes traffic to the backend pools.  In this example we create a basic rule that uses the previous defined http settings and backend address pool.  It also associates the listener to the rule
$rule = New-AzApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Sets the SKU of the application gateway, in this example we create a small standard application gateway with 2 instances.
$sku = New-AzApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

# The final step creates the application gateway with all the previously defined components.
$appgw = New-AzApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location 'West US' -BackendAddressPools $pool -Probes $probe -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

## <a name="add-a-probe-to-an-existing-application-gateway"></a>Bir araştırma eklemek için var olan bir uygulama ağ geçidi

Aşağıdaki kod parçacığı var olan bir uygulama ağ geçidi için bir araştırma ekler.

```powershell
# Load the application gateway resource into a PowerShell variable by using Get-AzApplicationGateway.
$getgw =  Get-AzApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

# Create the probe object that will check health at http://contoso.com/path/path.htm
$getgw = Add-AzApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name probe01 -Protocol Http -HostName 'contoso.com' -Path '/path/custompath.htm' -Interval 30 -Timeout 120 -UnhealthyThreshold 8

# Set the backend HTTP settings to use the new probe
$getgw = Set-AzApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 120

# Save the application gateway with the configuration changes
Set-AzApplicationGateway -ApplicationGateway $getgw
```

## <a name="remove-a-probe-from-an-existing-application-gateway"></a>Var olan bir uygulama ağ geçidi'nden bir araştırma Kaldır

Aşağıdaki kod parçacığı bir araştırma var olan bir uygulama ağ geçidi'nden kaldırır.

```powershell
# Load the application gateway resource into a PowerShell variable by using Get-AzApplicationGateway.
$getgw =  Get-AzApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

# Remove the probe from the application gateway configuration object
$getgw = Remove-AzApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name $getgw.Probes.name

# Set the backend HTTP settings to remove the reference to the probe. The backend http settings now use the default probe
$getgw = Set-AzApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol http -CookieBasedAffinity Disabled

# Save the application gateway with the configuration changes
Set-AzApplicationGateway -ApplicationGateway $getgw
```

## <a name="get-application-gateway-dns-name"></a>Uygulama ağ geçidi DNS adını alma

Ağ geçidi oluşturulduktan sonraki adım, iletişim için ön uç yapılandırması yapmaktır. Genel IP kullanırken uygulama ağ geçidi için dinamik olarak atanan DNS adı gerekir ve bu durum çok kullanışlı değildir. Son kullanıcıların uygulama ağ geçidine ulaşmasını sağlamak için uygulama ağ geçidinin genel uç noktasını işaret edecek bir CNAME kaydı kullanılabilir. [Azure’da özel etki alanı adı yapılandırma](../cloud-services/cloud-services-custom-domain-name-portal.md). Bunu yapmak için uygulama ağ geçidinin ayrıntılarını ve onunla ilişkilendirilmiş olan IP/DNS adını uygulama ağ geçidine eklenmiş PublicIPAddress öğesini kullanarak alın. Uygulama ağ geçidinin DNS adı, iki web uygulamasını bu DNS adına götüren bir CNAME kaydı oluşturmak için kullanılmalıdır. Uygulama ağ geçidi yeniden başlatıldığında VIP değişebileceğinden A kaydı kullanımı önerilmez.

```powershell
Get-AzPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
```

```
Name                     : publicIP01
ResourceGroupName        : appgw-RG
Location                 : westus
Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
ResourceGuid             : 00000000-0000-0000-0000-000000000000
ProvisioningState        : Succeeded
Tags                     : 
PublicIpAllocationMethod : Dynamic
IpAddress                : xx.xx.xxx.xx
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                                "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                            Configurations/frontend1"
                            }
DnsSettings              : {
                                "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                            }
```

## <a name="next-steps"></a>Sonraki adımlar

SSL yük boşaltma ederek yapılandırmayı öğrenin: [SSL yük boşaltmasını yapılandırma](application-gateway-ssl-arm.md)

