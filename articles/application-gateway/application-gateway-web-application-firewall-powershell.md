---
title: "Bir web uygulaması güvenlik duvarı yapılandırma: Azure uygulama ağ geçidi | Microsoft Docs"
description: "Bu makalede bir mevcut veya yeni uygulama ağ geçidi üzerinde bir web uygulaması Güvenlik Duvarı'nı kullanmaya başlamak hakkında yönergeler açıklanmaktadır."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 670b9732-874b-43e6-843b-d2585c160982
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: davidmu
ms.openlocfilehash: e8106805d21b325e33fb3ab376db75cd783b9042
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-web-application-firewall-on-a-new-or-existing-application-gateway"></a>Yeni veya var olan uygulama ağ geçidi üzerinde bir web uygulaması güvenlik duvarı yapılandırma

> [!div class="op_single_selector"]
> * [Azure portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure CLI](application-gateway-web-application-firewall-cli.md)

Bir web uygulaması Güvenlik Duvarı (WAF) oluşturmayı öğrenin-uygulama ağ geçidi etkin. Ayrıca bir WAF var olan bir uygulama ağ geçidi eklemeyi öğrenin.

Azure uygulama ağ geçidi olarak WAF web uygulamaları, SQL ekleme gibi ortak web tabanlı saldırıları, siteler arası komut dosyası saldırıları ve oturumu ele geçirilmesini korur.

 Uygulama ağ geçidi bir katman 7 yük dengeleyicidir. Bulut veya şirket içi olsanız yük devretme, HTTP isteklerini, farklı sunucular arasında performans amaçlı yönlendirme sağlar. Uygulama ağ geçidi birçok uygulama teslim denetleyicisi (ADC) özellikleri sağlar:

 * HTTP Yük Dengeleme
 * Tanımlama bilgisi tabanlı oturum benzeşimi
 * Güvenli Yuva Katmanı (SSL) yük boşaltma
 * Özel sistem durumu araştırmalarının
 * Çoklu tesis işlevi için destek
 
 Desteklenen özelliklerin tam listesi için bkz [genel bakış, uygulama ağ geçidi](application-gateway-introduction.md).

Bu makalede gösterilmektedir nasıl [var olan bir uygulama ağ geçidi için bir WAF ekleme](#add-web-application-firewall-to-an-existing-application-gateway). Ayrıca gösterir nasıl [bir WAF kullanan bir uygulama ağ geçidi oluşturma](#create-an-application-gateway-with-web-application-firewall).

![Senaryo görüntüsü][scenario]

## <a name="waf-configuration-differences"></a>WAF yapılandırma farklılıkları

Okuduğunuz varsa [PowerShell ile bir uygulama ağ geçidi oluşturma](application-gateway-create-gateway-arm.md), bir uygulama ağ geçidi oluşturduğunuzda yapılandırmak için SKU ayarları anlama. WAF bir SKU üzerinde bir uygulama ağ geçidi yapılandırdığınızda tanımlamak için ek ayarlar sağlar. Uygulama ağ geçidinde kendisini oluşturan ek değişiklik yoktur.

| **Ayar** | **Ayrıntılar**
|---|---|
|**SKU** |Normal uygulama ağ geçidi bir WAF olmadan destekleyen **standart\_küçük**, **standart\_orta**, ve **standart\_büyük**boyutları. Bir WAF başlanmasıyla, iki ek SKU vardır **WAF\_orta** ve **WAF\_büyük**. Bir WAF küçük uygulama ağ geçitleri üzerinde desteklenmiyor.|
|**Katmanı** | Kullanılabilir değerler **standart** veya **WAF**. Bir WAF kullandığınızda, seçmelisiniz **WAF**.|
|**Modu** | Bu ayar WAF modudur. izin verilen değerler **algılama** ve **önleme**. WAF ayarlandığında **algılama** modu, tüm tehditler bir günlük dosyasında depolanır. İçinde **önleme** modu, olayları yine kaydedilir ancak saldırgan yetkisiz 403 alır uygulama ağ geçidi yanıt.|

## <a name="add-a-web-application-firewall-to-an-existing-application-gateway"></a>Var olan bir uygulama ağ geçidi için bir web uygulaması güvenlik duvarı ekleme

Azure PowerShell'in en son sürümünü kullandığınızdan emin olun. Daha fazla bilgi için bkz: [Resource Manager ile Windows PowerShell'i kullanın](../powershell-azure-resource-manager.md).

1. Azure hesabınızda oturum açın.

    ```powershell
    Login-AzureRmAccount
    ```

2. Bu senaryo için kullanılacak aboneliği seçin.

    ```powershell
    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"
    ```

3. Bir WAF eklemek istediğiniz ağ geçidi alın.

    ```powershell
    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"
    ```

4. WAF SKU yapılandırın. Kullanılabilir boyutları: **WAF\_büyük** ve **WAF\_orta**. Bir WAF kullandığınızda, katmanı olmalıdır **WAF**. SKU ayarladığınızda kapasite onaylayın.

    ```powershell
    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2
    ```

5. WAF ayarlarını, aşağıdaki örnekte tanımlandığı şekilde yapılandırın. İçin **FirewallMode**, kullanılabilir değerler **önleme** ve **algılama**.

    ```powershell
    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention
    ```

6. Uygulama ağ geçidi önceki adımda tanımlanan ayarlarıyla güncelleştirin.

    ```powershell
    Set-AzureRmApplicationGateway -ApplicationGateway $gw
    ```

Bu komut, uygulama ağ geçidi WAF ile güncelleştirir. Uygulama ağ geçidiniz için günlükleri görüntülemek nasıl anlamak için bkz: [uygulama ağ geçidi tanılama](application-gateway-diagnostics.md). Bir WAF güvenlik yapısı nedeniyle, güvenlik yaklaşımı, web uygulamalarınızı, düzenli olarak anlamak için günlükleri gözden geçirin.

## <a name="create-an-application-gateway-with-a-web-application-firewall"></a>Web uygulaması güvenlik duvarı ile bir uygulama ağ geçidi oluşturma

Aşağıdaki adımları WAF ile bir uygulama ağ geçidi oluşturma tüm işlemi uygulayın.

Azure PowerShell'in en son sürümünü kullandığınızdan emin olun. Daha fazla bilgi için bkz: [Resource Manager ile Windows PowerShell'i kullanın](../powershell-azure-resource-manager.md).

1. Azure'da çalıştırarak oturum aç `Login-AzureRmAccount`. Kimlik bilgilerinizle kimliğinizi istenir.

2. Hesap için abonelikler denetleyin `Get-AzureRmSubscription`.

3. Kullanmak için hangi Azure aboneliğini seçin.

    ```powershell
    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"
    ```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Uygulama ağ geçidi için bir kaynak grubu oluşturun.

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

Azure Resource Manager, tüm kaynak gruplarının bir konum belirtmesini gerektirir. Bu konum, kaynak grubundaki kaynaklar için varsayılan konum olarak kullanılır. Bir uygulama ağ geçidi oluşturmak için verilecek komutların aynı kaynak grubunda kullandığınızdan emin olun.

Önceki örnekte, "appgw-RG" Konum "Batı ABD." adlı bir kaynak grubu oluşturduk

> [!NOTE]
> Uygulama ağ geçidiniz için özel bir araştırma yapılandırmanız gerekiyorsa, bkz. [PowerShell kullanarak özel araştırmalara sahip bir uygulama ağ geçidi oluşturma](application-gateway-create-probe-ps.md). Daha fazla bilgi için bkz: [özel araştırmalar ve sistem durumu izleme](application-gateway-probe-overview.md).

### <a name="configure-a-virtual-network"></a>Sanal ağ yapılandırma

Bir uygulama ağ geçidi alt ağı için kendi gerektirir. Bu adımda, bir sanal ağ adres alanı 10.0.0.0/16 ve iki alt ağ, uygulama ağ geçidi için bir tane ve arka uç havuzu üyeleri için bir tane oluşturun.

```powershell
# Create a subnet configuration object for the application gateway subnet. A subnet for an application should have a minimum of 28 mask bits. This value leaves 10 available addresses in the subnet for application gateway instances. With a smaller subnet, you might not be able to add more instances of your application gateway in the future.
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

# Create a subnet configuration object for the back-end pool members subnet.
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

# Create the virtual network with the previously created subnets.
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
```

### <a name="configure-the-public-ip-address"></a>Genel IP adresi yapılandırın

Dış istekleri işlemek üzere uygulama ağ geçidi genel bir IP adresi gerektirir. Bu genel IP adresi değil olmalıdır bir `DomainNameLabel` uygulama ağ geçidi tarafından kullanılması için tanımlanmış.

```powershell
# Create a public IP address for use with the application gateway. Defining the `DomainNameLabel` during creation is not supported for use with the application gateway.
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic
```

### <a name="configure-the-application-gateway"></a>Uygulama ağ geçidini yapılandırma

```powershell
# Create an IP configuration to configure which subnet the application gateway uses. When the application gateway starts, it picks up an IP address from the configured subnet and routes network traffic to the IP addresses in the back-end IP pool.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

# Create a back-end pool to hold the addresses or NIC handles for the application that the application gateway is protecting.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

# Upload the authentication certificate to be used to communicate with the back-end servers.
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path to .cer file>

# Configure the back-end HTTP settings to be used to define how traffic is routed to the back-end pool. The authentication certificate used in the previous step is added to the back-end HTTP settings.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

# Create a front-end port to be used by the listener.
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

# Create a front-end IP configuration to associate the public IP address with the application gateway.
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

# Configure the certificate for the application gateway. This certificate is used to decrypt and re-encrypt the traffic on the application gateway.
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

# Create the HTTP listener for the application gateway. Assign the front-end IP configuration, port, and SSL certificate to use.
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

# Create a load-balancer routing rule that configures the load balancer behavior. In this example, a basic round-robin rule is created.
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Configure the SKU of the application gateway.
$sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

# Define the SSL policy to use.
$policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S

# Configure the WAF configuration settings.
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"

# Create the application gateway by using all the previously created configuration objects.
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert
```

> [!NOTE]
> Temel WAF yapılandırma ile oluşturulmuş uygulama ağ geçitleri için koruma CRS 3.0 ile yapılandırılır.

## <a name="get-an-application-gateway-dns-name"></a>Bir uygulama ağ geçidi DNS adını Al

Ağ geçidi oluşturulduktan sonra sonraki adıma iletişimi için ön uç yapılandırmaktır. Genel IP kullandığınızda, uygulama ağ geçidi kolay olmayan dinamik olarak atanmış bir DNS adı gerektirir. Kullanıcıların uygulama ağ geçidi isabet emin olmak için uygulama ağ geçidi için ortak uç noktası için bir CNAME kaydı kullanın. Daha fazla bilgi için bkz: [bir Azure bulut hizmeti için bir özel etki alanı adı yapılandırma](../cloud-services/cloud-services-custom-domain-name-portal.md). 

Bir diğer ad yapılandırmak için uygulama ağ geçidine bağlı Publicıpaddress öğesi kullanarak uygulama ağ geçidi ve ilişkili IP/DNS adı ayrıntılarını alabilirsiniz. İki web uygulamaları bu DNS adına işaret eden bir CNAME kaydı oluşturmak için uygulama ağ geçidi DNS adını kullanın. Uygulama ağ geçidi yeniden başlatıldığında VIP değişebileceğinden A kayıtları kullanarak önermiyoruz.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
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

WAF ile algılandığında veya engelledi olaylarını günlüğe yazacak şekilde tanılama günlük kaydını yapılandırmak öğrenmek için bkz: [uygulama ağ geçidi tanılama](application-gateway-diagnostics.md).

[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png
