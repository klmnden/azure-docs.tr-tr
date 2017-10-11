---
title: "Birden çok sitesi barındırmak için bir uygulama ağ geçidi oluşturma | Microsoft Docs"
description: "Bu sayfa oluşturma, aynı ağ geçidinde birden çok web uygulamalarını barındırmak için bir Azure uygulama ağ geçidi yapılandırmak için yönergeler sağlar."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: b107d647-c9be-499f-8b55-809c4310c783
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/12/2016
ms.author: amsriva
ms.openlocfilehash: d42efa7d359f5c87c14afbfd138328b37c8ae6c2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-application-gateway-for-hosting-multiple-web-applications"></a>Birden çok web uygulamalarını barındırmak için bir uygulama ağ geçidi oluşturma

> [!div class="op_single_selector"]
> * [Azure portal](application-gateway-create-multisite-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-multisite-azureresourcemanager-powershell.md)

Birden çok siteyi barındıran aynı uygulama ağ geçidi birden çok web uygulamasına dağıtmanıza olanak tanır. Hangi dinleyicisi trafiği alacağını belirlemek için gelen HTTP isteği, ana bilgisayar üst bilgisi kullanır. Dinleyici sonra ağ geçidi kuralları tanımı içinde yapılandırıldığı gibi uygun arka uç havuzu trafiğini yönlendirir. SSL etkin web uygulamaları, uygulama ağ geçidi web trafiği için doğru dinleyici seçmek için sunucu adı göstergesi (SNI) uzantısı kullanır. Birden çok sitesi barındırmak için bir genel Yük Dengeleme isteklerini farklı web etki alanları için farklı bir arka uç sunucu havuzları için kullanılır. Benzer şekilde aynı kök etki alanının birden çok alt de aynı uygulama ağ geçidinde barındırılan.

## <a name="scenario"></a>Senaryo

Aşağıdaki örnekte, iki arka uç sunucu havuzu ile trafiği contoso.com ve fabrikam.com için uygulama ağ geçidi hizmet: contoso sunucu havuzu ve fabrikam sunucu havuzu. Benzer Kurulum app.contoso.com ve blog.contoso.com gibi ana bilgisayar alt etki alanları için kullanılabilir.

![imageURLroute](./media/application-gateway-create-multisite-azureresourcemanager-powershell/multisite.png)

## <a name="before-you-begin"></a>Başlamadan önce

1. Web Platformu Yükleyicisi’ni kullanarak Azure PowerShell cmdlet’lerin en son sürümünü yükleyin. **İndirmeler sayfası**’ndaki [Windows PowerShell](https://azure.microsoft.com/downloads/) bölümünden en son sürümü indirip yükleyebilirsiniz.
2. Uygulama ağ geçidi kullanmak için arka uç havuzuna eklediğiniz sunucular mevcut olmalıdır veya uç noktaları ya da sanal ağda ayrı bir alt ağ veya atanan genel IP/VIP'ye oluşturdunuz.

## <a name="requirements"></a>Gereksinimler

* **Arka uç sunucusu havuzu:** Arka uç sunucularının IP adreslerinin listesi. Listede bulunan IP adresleri sanal ağ alt ağına veya genel IP/VIP’ye ait olmalıdır. FQDN de kullanılabilir.
* **Arka uç sunucu havuzu ayarları**: Her havuzun bağlantı noktası, protokol ve tanımlama bilgisi temelli benzeşim gibi ayarları vardır. Bu ayarlar bir havuza bağlıdır ve havuzdaki tüm sunuculara uygulanır.
* **Ön uç bağlantı noktası:** Bu bağlantı noktası uygulama ağ geçidinde açılan genel bağlantı noktasıdır. Bu bağlantı noktasında trafik olursa arka uç sunuculardan birine yönlendirilir.
* **Dinleyici:** Dinleyicide bir ön uç bağlantı noktası, bir protokol (Http veya Https, bu değerler büyük/küçük harfe duyarlıdır) ve SSL sertifika adı (SSL yük boşaltımı yapılandırılıyorsa) vardır. Çok siteli kullanan bir uygulama ağ geçitleri için ana bilgisayar adı ve SNI göstergeleri de eklenir.
* **Kural:** kural dinleyiciyi arka uç sunucusu havuzunu bağlar ve trafiği için belli bir Dinleyicide trafik olduğunda hangi arka uç sunucu havuzuna yönlendirileceğini belirler. Kuralları listelendikleri sırada işlenir ve trafik belirginliğe bağımsız olarak eşleşen ilk kural üzerinden yönlendirilir. Temel bir dinleyici ve çok siteli dinleyicisi hem aynı bağlantı noktasında kullanan bir kural kullanarak bir kuralı varsa, örneğin, çok siteli dinleyicisi kuralla beklendiği şekilde çalışması için temel dinleyicisi çok siteli kuralı için sırada olan kural önce listelenmiş olması gerekir.

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Bir uygulama ağ geçidi oluşturmak için gereken adımlar şunlardır:

1. Resource Manager için kaynak grubu oluşturun.
2. Bir sanal ağ, alt ağlar ve uygulama ağ geçidi için genel IP oluşturun.
3. Uygulama ağ geçidi yapılandırma nesnesi oluşturun.
4. Uygulama ağ geçidi kaynağı oluşturun.

## <a name="create-a-resource-group-for-resource-manager"></a>Resource Manager için kaynak grubu oluşturun

Azure PowerShell’in en yeni sürümünü kullandığınızdan emin olun. Daha fazla bilgi için bkz. [Resource Manager ile Windows PowerShell kullanarak](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>1. Adım

Azure'da oturum açma

```powershell
Login-AzureRmAccount
```
Kimlik bilgilerinizle kimliğinizi doğrulamanız istenir.

### <a name="step-2"></a>2. Adım

Hesapla ilişkili abonelikleri kontrol edin.

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a>3. Adım

Hangi Azure aboneliğinizin kullanılacağını seçin.

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a>4. Adım

Bir kaynak grubu oluşturun (mevcut bir kaynak grubu kullanıyorsanız bu adımı atlayın).

```powershell
New-AzureRmResourceGroup -Name appgw-RG -location "West US"
```

Alternatif olarak, uygulama ağ geçidi için bir kaynak grubu için etiketler de oluşturabilirsiniz:

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway multiple site"}
```

Azure Resource Manager, tüm kaynak gruplarının bir konum belirtmesini gerektirir. Bu konum, kaynak grubundaki kaynaklar için varsayılan konum olarak kullanılır. Bir uygulama ağ geçidi oluşturmak için verilecek komutların aynı kaynak grubunda kullandığınızdan emin olun.

Yukarıdaki örnekte adlı bir kaynak grubu oluşturduk **appgw-RG** konumunu ile **Batı ABD**.

> [!NOTE]
> Uygulama ağ geçidiniz için özel bir araştırma yapılandırmanız gerekiyorsa, bkz. [PowerShell kullanarak özel araştırmalara sahip bir uygulama ağ geçidi oluşturma](application-gateway-create-probe-ps.md). Ziyaret [özel araştırmalar ve sistem durumu izleme](application-gateway-probe-overview.md) daha fazla bilgi için.

## <a name="create-a-virtual-network-and-subnets"></a>Bir sanal ağ ve alt ağlar oluşturun

Aşağıdaki örnek Resource Manager kullanarak nasıl sanal ağ oluşturulacağını gösterir. Bu adımda iki alt ağ oluşturulur. İlk alt uygulama ağ geçidi için kendisini değil. Uygulama ağ geçidi örneklerini tutmak için kendi alt gerektirir. Bu alt ağda yalnızca başka uygulama ağ geçitleri dağıtılabilir. İkinci alt ağ, uygulama arka uç sunucuları tutmak için kullanılır.

### <a name="step-1"></a>1. Adım

10.0.0.0/24 adres aralığını uygulama ağ geçidi tutmak için kullanılan alt ağ değişkenine atayın.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -AddressPrefix 10.0.0.0/24
```
### <a name="step-2"></a>2. Adım

Adres aralığı 10.0.1.0/24 arka uç havuzları için kullanılacak Altağ2 değişkenine atayın.

```powershell
$subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -AddressPrefix 10.0.1.0/24
```

### <a name="step-3"></a>3. Adım

Adlı bir sanal ağ oluşturma **appgwvnet** kaynak grubunda **appgw-rg** 10.0.0.0/24 alt ağıyla önek 10.0.0.0/16 kullanarak Batı ABD bölgesi için ve 10.0.1.0/24.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet,$subnet2
```

### <a name="step-4"></a>4. Adım

Bir uygulama ağ geçidi oluşturur sonraki adımlar için bir alt ağ değişkeni atayın.

```powershell
$appgatewaysubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -VirtualNetwork $vnet
$backendsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -VirtualNetwork $vnet
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Ön uç yapılandırma için genel bir IP adresi oluşturun

Batı ABD bölgesi için **appgw-rg** kaynak grubunda **publicIP01** genel bir IP kaynağı oluşturun.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

Hizmet başlatıldığında uygulama ağ geçidine bir IP adresi atanır.

## <a name="create-application-gateway-configuration"></a>Uygulama ağ geçidi yapılandırmasını oluşturma

Uygulama ağ geçidini oluşturmadan önce tüm yapılandırma öğelerini ayarlamanız gerekir. Aşağıdaki adımlar uygulama ağ geçidi kaynağı için gerekli yapılandırma öğelerini oluşturur.

### <a name="step-1"></a>1. Adım

**gatewayIP01** adlı bir uygulama ağ geçidi IP yapılandırması oluşturun. Uygulama ağ geçidi başladığında, yapılandırılan alt ağdan bir IP adresi seçer ve arka uç IP havuzundaki IP adresleri için ağ trafiğini yönlendirebilir. Her örneğin bir IP adresi aldığını göz önünde bulundurun.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $appgatewaysubnet
```

### <a name="step-2"></a>2. Adım

Adlı arka uç IP adresi havuzu yapılandırmak **pool01** ve **pool2** IP adresleriyle **134.170.185.46**, **134.170.188.221**, **134.170.185.50** için **pool1** ve **134.170.186.46**, **134.170.189.221**, **134.170.186.50** için **pool2**.

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.0.1.100, 10.0.1.101, 10.0.1.102
$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 10.0.1.103, 10.0.1.104, 10.0.1.105
```

Bu örnekte, istenen sitesinde göre ağ trafiği yönlendirmek için iki arka uç havuzu yok. Bir havuz "contoso.com" sitesinden trafiği alır ve diğer havuzu "fabrikam.com" sitesinden trafiği alır. Kendi uygulama IP adresi uç noktalarını eklemek için yukarıdaki IP adreslerini değiştirmek zorunda. İç IP adresleri yerine, arka uç örnekleri için genel IP adresleri, FQDN veya bir sanal makinenin NIC de kullanabilirsiniz. PowerShell kullanmak yerine FQDN'ler yerine IP'leri belirtmek için "-BackendFQDNs" parametresi.

### <a name="step-3"></a>3. Adım

Uygulama ağ geçidi ayarlarını yapılandırmanız **poolsetting01** ve **poolsetting02** arka uç havuzundaki yük dengeli ağ trafiği için. Bu örnekte, arka uç havuzları farklı arka uç havuzu ayarlarını yapılandırın. Her bir arka uç havuzu kendi arka uç havuzu ayarlarına sahip olabilir.

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120
$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a>4. Adım

Ön uç IP’sini genel IP uç noktası ile yapılandırın.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a>5. Adım

Bir uygulama ağ geçidi için ön uç bağlantı noktasını yapılandırın.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 443
```

### <a name="step-6"></a>6. Adım

Biz bu örnekte destekleyeceğinizi iki SSL sertifikaları iki Web siteleri için yapılandırın. Bir sertifika contoso.com trafiği için ve diğer fabrikam.com trafiği için. Bu sertifikalar, Web siteleri için sertifikalar bir sertifika yetkilisi olması gerekir. Otomatik olarak imzalanan sertifikalar desteklenir, ancak üretim trafiği için önerilmez.

```powershell
$cert01 = New-AzureRmApplicationGatewaySslCertificate -Name contosocert -CertificateFile <file path> -Password <password>
$cert02 = New-AzureRmApplicationGatewaySslCertificate -Name fabrikamcert -CertificateFile <file path> -Password <password>
```

### <a name="step-7"></a>7. Adım

Bu örnekte, iki dinleyicileri iki web sitesi için yapılandırın. Bu adım dinleyicileri ortak IP adresi, bağlantı noktası ve gelen trafiği almak için kullanılan ana bilgisayarı için yapılandırır. Ana bilgisayar adı parametresi, çoklu site desteği için gereklidir ve trafiği alınan uygun Web sitesine ayarlamanız gerekir. Requireservernameındication parametresi ayarlanmalıdır SSL için birden çok ana senaryo desteği gerektiren Web siteleri için true. SSL desteği gerekiyorsa, ayrıca, web uygulaması için trafiğinin güvenliğini sağlamak için kullanılan SSL sertifikası belirtmeniz gerekir. Frontendıpconfiguration, FrontendPort ve ana bilgisayar adı bileşimi için bir dinleyici benzersiz olması gerekir. Her dinleyicisi bir sertifikayı destekleyebilir.

```powershell
$listener01 = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "contoso11.com" -RequireServerNameIndication true  -SslCertificate $cert01
$listener02 = New-AzureRmApplicationGatewayHttpListener -Name "listener02" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "fabrikam11.com" -RequireServerNameIndication true -SslCertificate $cert02
```

### <a name="step-8"></a>8. Adım

Bu örnekte, iki web uygulamaları için iki kural ayarı oluşturun. Bir kural dinleyicileri, arka uç havuzları ve http ayarları birbirine bağlar. Bu adım, uygulama ağ geçidi her Web sitesi için bir tane temel yönlendirme kuralı kullanmak üzere yapılandırır. Her Web sitesi trafiğini kendi yapılandırılmış dinleyicisi tarafından alınır ve BackendHttpSettings belirtilen özellikleri kullanarak kendi yapılandırılmış arka uç havuzu yönlendirilir.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule01" -RuleType Basic -HttpListener $listener01 -BackendHttpSettings $poolSetting01 -BackendAddressPool $pool1
$rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule02" -RuleType Basic -HttpListener $listener02 -BackendHttpSettings $poolSetting02 -BackendAddressPool $pool2
```

### <a name="step-9"></a>9. Adım

Uygulama ağ geçidi için örnek sayısını ve boyutu yapılandırın.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Medium" -Tier Standard -Capacity 2
```

## <a name="create-application-gateway"></a>Uygulama ağ geçidi oluşturma

Yukarıdaki adımlarda geçen tüm yapılandırma nesnelerle bir uygulama ağ geçidi oluşturun.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener01, $listener02 -RequestRoutingRules $rule01, $rule02 -Sku $sku -SslCertificates $cert01, $cert02
```

> [!IMPORTANT]
> Uygulama ağ geçidi sağlama, uzun süreli bir işlemdir ve tamamlanması biraz zaman alabilir.
> 
> 

## <a name="get-application-gateway-dns-name"></a>Uygulama ağ geçidi DNS adını alma

Ağ geçidi oluşturulduktan sonraki adım, iletişim için ön uç yapılandırması yapmaktır. Genel IP kullanırken uygulama ağ geçidi için dinamik olarak atanan DNS adı gerekir ve bu durum çok kullanışlı değildir. Son kullanıcıların uygulama ağ geçidine ulaşmasını sağlamak için uygulama ağ geçidinin genel uç noktasını işaret edecek bir CNAME kaydı kullanılabilir. [Azure’da özel etki alanı adı yapılandırma](../cloud-services/cloud-services-custom-domain-name-portal.md). Bunu yapmak için uygulama ağ geçidinin ayrıntılarını ve onunla ilişkilendirilmiş olan IP/DNS adını uygulama ağ geçidine eklenmiş PublicIPAddress öğesini kullanarak alın. Uygulama ağ geçidinin DNS adı, iki web uygulamasını bu DNS adına götüren bir CNAME kaydı oluşturmak için kullanılmalıdır. Uygulama ağ geçidi yeniden başlatıldığında VIP değişebileceğinden A kaydı kullanımı önerilmez.

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

İle Web siteleri korumayı öğrenin [uygulama ağ geçidi - Web uygulaması güvenlik duvarı](application-gateway-webapplicationfirewall-overview.md)

