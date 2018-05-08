---
title: Azure Application Gateway ile web uygulamalarını koruma - PowerShell | Microsoft Docs
description: Bu makalede, mevcut veya yeni bir application gateway üzerinde web uygulamalarını arka uç ana bilgisayarları olarak yapılandırmaya ilişkin yönergeler verilmektedir.
documentationcenter: na
services: application-gateway
author: vhorne
manager: jpconnock
editor: ''
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: victorh
ms.openlocfilehash: abe48c484a232eff6f7ec1cd68e7010353d488d5
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="configure-app-service-web-apps-with-application-gateway"></a>Application Gateway ile App Service Web Apps’i yapılandırma 

Application gateway, bir Azure Web Uygulaması veya diğer çok kiracılı hizmeti arka uç havuzu üyesi olarak kullanmanıza olanak tanır. Bu makalede bir Azure web uygulamasını Application Gateway ile yapılandırma hakkında bilgi alacaksınız. İlk örnekte, arka uç havuzu üyesi olarak web uygulamasını kullanmak için mevcut bir application gateway’i nasıl yapılandıracağınız gösterilmektedir. İkinci örnekte, arka uç havuzu üyesi olarak web uygulaması ile yeni bir application gateway’i nasıl oluşturacağınız gösterilmektedir.

## <a name="configure-a-web-app-behind-an-existing-application-gateway"></a>Mevcut bir application gateway’in arkasında web uygulaması yapılandırma

Aşağıdaki örnek var olan bir application gateway’e arka uç havuzu üyesi olarak web uygulaması ekler. Web uygulamalarının çalışması için hem Araştırma yapılandırması üzerindeki düğme `-PickHostNamefromBackendHttpSettings`hem de arka uç http ayarları üzerindeki `-PickHostNameFromBackendAddress` belirtilmelidir.

```powershell
# FQDN of the web app
$webappFQDN = "<enter your webapp FQDN i.e mywebsite.azurewebsites.net>"

# Retrieve an existing application gateway
$gw = Get-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName $rg.ResourceGroupName

# Define the status codes to match for the probe
$match=New-AzureRmApplicationGatewayProbeHealthResponseMatch -StatusCode 200-399

# Add a new probe to the application gateway
Add-AzureRmApplicationGatewayProbeConfig -name webappprobe2 -ApplicationGateway $gw -Protocol Http -Path / -Interval 30 -Timeout 120 -UnhealthyThreshold 3 -PickHostNameFromBackendHttpSettings -Match $match

# Retrieve the newly added probe
$probe = Get-AzureRmApplicationGatewayProbeConfig -name webappprobe2 -ApplicationGateway $gw

# Configure an existing backend http settings 
Set-AzureRmApplicationGatewayBackendHttpSettings -Name appGatewayBackendHttpSettings -ApplicationGateway $gw -PickHostNameFromBackendAddress -Port 80 -Protocol http -CookieBasedAffinity Disabled -RequestTimeout 30 -Probe $probe

# Add the web app to the backend pool
Set-AzureRmApplicationGatewayBackendAddressPool -Name appGatewayBackendPool -ApplicationGateway $gw -BackendFqdns $webappFQDN

# Update the application gateway
Set-AzureRmApplicationGateway -ApplicationGateway $gw
```

## <a name="configure-a-web-application-behind-a-new-application-gateway"></a>Yeni bir application gateway’in arkasında web uygulaması yapılandırma

Bu senaryo, asp.net başlangıç web sitesi ve bir application gateway ile bir web uygulamasını dağıtır.

```powershell
# Defines a variable for a dotnet get started web app repository location
$gitrepo="https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git"

# Unique web app name
$webappname="mywebapp$(Get-Random)"

# Creates a resource group
$rg = New-AzureRmResourceGroup -Name ContosoRG -Location Eastus

# Create an App Service plan in Free tier.
New-AzureRmAppServicePlan -Name $webappname -Location EastUs -ResourceGroupName $rg.ResourceGroupName -Tier Free

# Creates a web app
$webapp = New-AzureRmWebApp -ResourceGroupName $rg.ResourceGroupName -Name $webappname -Location EastUs -AppServicePlan $webappname

# Configure GitHub deployment from your GitHub repo and deploy once to web app.
$PropertiesObject = @{
    repoUrl = "$gitrepo";
    branch = "master";
    isManualIntegration = "true";
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName $rg.ResourceGroupName -ResourceType Microsoft.Web/sites/sourcecontrols -ResourceName $webappname/web -ApiVersion 2015-08-01 -Force

# Creates a subnet for the application gateway
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

# Creates a vnet for the application gateway
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName $rg.ResourceGroupName -Location EastUs -AddressPrefix 10.0.0.0/16 -Subnet $subnet

# Retrieve the subnet object for use later
$subnet=$vnet.Subnets[0]

# Create a public IP address
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName $rg.ResourceGroupName -name publicIP01 -location EastUs -AllocationMethod Dynamic

# Create a new IP configuration
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

# Create a backend pool with the hostname of the web app
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name appGatewayBackendPool -BackendFqdns $webapp.HostNames

# Define the status codes to match for the probe
$match = New-AzureRmApplicationGatewayProbeHealthResponseMatch -StatusCode 200-399

# Create a probe with the PickHostNameFromBackendHttpSettings switch for web apps
$probeconfig = New-AzureRmApplicationGatewayProbeConfig -name webappprobe -Protocol Http -Path / -Interval 30 -Timeout 120 -UnhealthyThreshold 3 -PickHostNameFromBackendHttpSettings -Match $match

# Define the backend http settings
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name appGatewayBackendHttpSettings -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120 -PickHostNameFromBackendAddress -Probe $probeconfig

# Create a new front-end port
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

# Create a new front end IP configuration
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

# Create a new listener using the front-end ip configuration and port created earlier
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

# Create a new rule
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool 

# Define the application gateway SKU to use
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

# Create the application gateway
$appgw = New-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName $rg.ResourceGroupName -Location EastUs -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -Probes $probeconfig -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

## <a name="get-application-gateway-dns-name"></a>Uygulama ağ geçidi DNS adını alma

Ağ geçidi oluşturulduktan sonraki adım, iletişim için ön uç yapılandırması yapmaktır. Genel IP kullanırken uygulama ağ geçidi için dinamik olarak atanan DNS adı gerekir ve bu durum çok kullanışlı değildir. Son kullanıcıların uygulama ağ geçidine ulaşmasını sağlamak için uygulama ağ geçidinin genel uç noktasını işaret edecek bir CNAME kaydı kullanılabilir. Diğer adı oluşturmak için uygulama ağ geçidinin ayrıntılarını ve onunla ilişkilendirilmiş olan IP/DNS adını uygulama ağ geçidine eklenmiş PublicIPAddress öğesini kullanarak alın. Bu işlem Azure DNS veya diğer DNS sağlayıcıları ile [genel IP adresini](../dns/dns-custom-domain.md#public-ip-address) işaret eden bir CNAME kaydı oluşturularak yapılabilir. Uygulama ağ geçidi yeniden başlatıldığında VIP değişebileceğinden A kaydı kullanımı önerilmez.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName ContosoRG -Name publicIP01
```

```
Name                     : publicIP01
ResourceGroupName        : ContosoRG
Location                 : eastus
Id                       : /subscriptions/<subscription_id>/resourceGroups/ContosoRG/providers/Microsoft.Network/publicIPAddresses/publicIP01
Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
ResourceGuid             : 00000000-0000-0000-0000-000000000000
ProvisioningState        : Succeeded
Tags                     : 
PublicIpAllocationMethod : Dynamic
IpAddress                : xx.xx.xxx.xx
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                                "Id": "/subscriptions/<subscription_id>/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/ContosoAppGateway/frontendIP
                            Configurations/frontend1"
                            }
DnsSettings              : {
                                "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                            }
```

## <a name="next-steps"></a>Sonraki adımlar

Yeniden yönlendirmeyi yapılandırma hakkında bilgi almak için bkz. [PowerShell ile Application Gateway için yeniden yönlendirmeyi yapılandırma](application-gateway-configure-redirect-powershell.md).
