---
title: Azure API Management Application Gateway ile sanal ağ kullanma | Microsoft Docs
description: Kurulum ve iç sanal ağ ile Application Gateway (WAF) ön uç olarak Azure API Management yapılandırma hakkında bilgi edinin
services: api-management
documentationcenter: ''
author: solankisamir
manager: kjoshi
editor: vlvinogr
ms.assetid: a8c982b2-bca5-4312-9367-4a0bbc1082b1
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: sasolank
ms.openlocfilehash: 4ee970f14a6da3d65849a79ff4afae68601f106f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66141660"
---
# <a name="integrate-api-management-in-an-internal-vnet-with-application-gateway"></a>API yönetimi bir iç sanal ağ'ı Application Gateway ile tümleştirme

## <a name="overview"> </a> Genel bakış

API Management hizmeti, bir sanal ağ sanal ağ içindeki yalnızca erişilebilir olmasını sağlayan iç modunda yapılandırılabilir. Azure Application Gateway, bir katman 7 yük dengeleyici sağlar bir PAAS hizmeti, ' dir. Ters proxy hizmeti olarak çalışır ve onun bir Web uygulaması Güvenlik Duvarı (WAF) sunarak arasında sağlar.

API Management'ın bir iç sanal ağ ile uygulama ağ geçidi ön uç sağlanan birleştirme, aşağıdaki senaryolara olanak tanır:

* Hem iç tüketicilere hem de dış tüketiciler tarafından kullanılmaya aynı API Management kaynağının kullanın.
* API'leri dış müşterileri için kullanılabilir API Yönetimi'nde tanımlanan bir alt kümesine sahip ve tek bir API Management kaynak'ı kullanın.
* Anahtar teslim erişim açıp API Management'a genel Internet'ten geçiş olanağını sağlar.

[!INCLUDE [premium-dev.md](../../includes/api-management-availability-premium-dev.md)]

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu makalede açıklanan adımları takip etmek için şunlara sahip olmalısınız:

* Etkin bir Azure aboneliği.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

* Sertifikalar - pfx ve API ana bilgisayar adı için cer ve pfx Geliştirici portal'ın ana bilgisayar adı için.

## <a name="scenario"> </a> Senaryo

Bu makalede, iç ve dış müşteriler için tek bir API Management hizmet ve şirket içi hem de tek bir ön uç davranır ve bulut API'leri nasıl kullanılacağı anlatılmaktadır. Ayrıca, yalnızca bir alt kümesini (yeşil renkte vurgulanmış örnekte) API'leri dış Application Gateway'i kullanılabilir yönlendirme işlevini kullanarak tüketimi için nasıl sunacağınızı öğrenin görürsünüz.

İlk kurulum örnekte tüm API'leri yalnızca sanal ağınızdaki yönetilir. İç ve dış tüm Apı'lerinizi iç tüketiciler (vurgulanmış turuncu) erişebilir. Trafik hiçbir zaman internet'e gider. Yüksek performanslı bağlantı, Expressroute bağlantı hatları teslim edilir.

![URL yolu](./media/api-management-howto-integrate-internal-vnet-appgateway/api-management-howto-integrate-internal-vnet-appgateway.png)

## <a name="before-you-begin"> </a> Başlamadan önce

* Azure PowerShell’in en yeni sürümünü kullandığınızdan emin olun. Yükleme yönergelerine bakın [Azure PowerShell yükleme](/powershell/azure/install-az-ps). 

## <a name="what-is-required-to-create-an-integration-between-api-management-and-application-gateway"></a>API yönetimi ve uygulama ağ geçidi arasında bir tümleştirme oluşturmak için gereken nedir?

* **Arka uç sunucu havuzu:** İç sanal IP adresi API Management hizmetinin budur.
* **Arka uç sunucu havuzu ayarları:** Her havuzun bağlantı noktası, protokol ve tanımlama bilgisi temelli benzeşim gibi ayarları vardır. Bu ayarlar, havuzdaki tüm sunuculara uygulanır.
* **Ön uç bağlantı noktası:** Bu uygulama ağ geçidinde açılan genel bağlantı noktasıdır. Bu trafiğe arka uç sunuculardan birine yönlendirilir.
* **Dinleyici:** Dinleyicinin sahip bir ön uç bağlantı noktası, bir protokol (Http veya Https, bu değerler büyük küçük harfe duyarlı) ve SSL sertifika adı (SSL yük boşaltımı yapılandırılıyorsa).
* **Kural:** Kural, bir arka uç sunucu havuzuna bir dinleyici bağlar.
* **Özel durum araştırması:** Uygulama ağ geçidi, varsayılan olarak, hangi sunucuları BackendAddressPool etkin olmadığını öğrenmek için IP adresi tabanlı araştırmaları kullanır. API Management hizmeti yalnızca doğru ana bilgisayar üstbilgisi isteklerini yanıtlar, bu nedenle varsayılan araştırmaları başarısız. Özel durum araştırması, uygulama ağ geçidi canlı bir hizmettir ve istekleri iletmek belirlemek amacıyla tanımlanması gerekir.
* **Özel etki alanı sertifikaları:** API Management'ı internet'ten erişmek için kendi ana bilgisayar adı ön uç DNS adını uygulama ağ geçidi için bir CNAME eşlemesi oluşturmanız gerekir. Bu API Yönetimi'ne iletilen bir Application Gateway için gönderilen bir sertifika ve ana bilgisayar üst bilgisi bir APIM geçerli olarak tanıyabilmesi sağlar. Bu örnekte, iki sertifika - Geliştirici portalını ve arka uç için kullanacağız.  

## <a name="overview-steps"> </a> API yönetimi ve uygulama ağ geçidi tümleştirmek için gereken adımlar

1. Resource Manager için kaynak grubu oluşturun.
2. Application Gateway için bir sanal ağ, alt ağ ve genel IP oluşturun. API yönetimi için başka bir alt ağ oluşturun.
3. Yukarıda oluşturulan sanal ağ alt ağ içinde bir API Management hizmeti oluşturma ve iç modu kullandığınızdan emin olun.
4. API Management hizmeti bir özel etki alanı adı ayarlayın.
5. Bir uygulama ağ geçidi yapılandırma nesnesi oluşturun.
6. Bir uygulama ağ geçidi kaynağı oluşturun.
7. Genel DNS adını uygulama ağ geçidinin API Management proxy konak adı için bir CNAME oluşturun.

## <a name="exposing-the-developer-portal-externally-through-application-gateway"></a>Geliştirici Portalı Uygulama ağ geçidi üzerinden harici olarak gösterme

Bu kılavuzda biz de açığa çıkarır **Geliştirici Portalı** uygulama ağ geçidi üzerinden dış kitlelere. Geliştirici portalının dinleyicisi, araştırma, ayarları ve kuralları oluşturmak için ek adımlar gerektirir. Tüm ayrıntıları, ilgili adımları sağlanır.

> [!WARNING]
> Azure AD veya kimlik doğrulaması'üçüncü taraf kullanıyorsanız, lütfen etkinleştirme [tanımlama bilgilerine dayalı oturum benzeşimi](https://docs.microsoft.com/azure/application-gateway/overview#session-affinity) özelliği'Application Gateway'de.

## <a name="create-a-resource-group-for-resource-manager"></a>Resource Manager için kaynak grubu oluşturun

### <a name="step-1"></a>1\. Adım

Azure'da oturum açma

```powershell
Connect-AzAccount
```

Kimlik bilgilerinizle kimliğinizi.

### <a name="step-2"></a>2\. Adım

İstediğiniz aboneliği seçin.

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000" # GUID of your Azure subscription
Get-AzSubscription -Subscriptionid $subscriptionId | Select-AzSubscription
```

### <a name="step-3"></a>3\. Adım

Bir kaynak grubu oluşturun (mevcut bir kaynak grubu kullanıyorsanız bu adımı atlayın).

```powershell
$resGroupName = "apim-appGw-RG" # resource group name
$location = "West US"           # Azure region
New-AzResourceGroup -Name $resGroupName -Location $location
```

Azure Resource Manager, tüm kaynak gruplarının bir konum belirtmesini gerektirir. Bu, kaynak grubunda kaynaklar için varsayılan konum olarak kullanılır. Tüm uygulama ağ geçidi oluşturma komutlarının aynı kaynak grubunu kullandığından emin olun.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Bir sanal ağ ve uygulama ağ geçidi için bir alt ağ oluşturma

Aşağıdaki örnek, kaynak kullanarak sanal bir ağ yöneticisi oluşturma işlemi gösterilmektedir.

### <a name="step-1"></a>1\. Adım

10\.0.0.0/24 adres aralığını, sanal ağ oluşturulurken Application Gateway için kullanılacak alt ağ değişkenine atayın.

```powershell
$appgatewaysubnet = New-AzVirtualNetworkSubnetConfig -Name "apim01" -AddressPrefix "10.0.0.0/24"
```

### <a name="step-2"></a>2\. Adım

Adres aralığı 10.0.1.0/24 bir sanal ağ oluşturulurken API yönetimi için kullanılacak alt ağ değişkenine atayın.

```powershell
$apimsubnet = New-AzVirtualNetworkSubnetConfig -Name "apim02" -AddressPrefix "10.0.1.0/24"
```

### <a name="step-3"></a>3\. Adım

Adlı bir sanal ağ oluşturma **appgwvnet** kaynak grubundaki **apim-appGw-RG** Batı ABD bölgesi için. Önek 10.0.0.0/16 kullanın 10.0.0.0/24 alt ağlar ve 10.0.1.0/24.

```powershell
$vnet = New-AzVirtualNetwork -Name "appgwvnet" -ResourceGroupName $resGroupName -Location $location -AddressPrefix "10.0.0.0/16" -Subnet $appgatewaysubnet,$apimsubnet
```

### <a name="step-4"></a>4\. Adım

Sonraki adımlar için alt ağ değişkenine atayın

```powershell
$appgatewaysubnetdata = $vnet.Subnets[0]
$apimsubnetdata = $vnet.Subnets[1]
```

## <a name="create-an-api-management-service-inside-a-vnet-configured-in-internal-mode"></a>İç modunda yapılandırılmış bir sanal ağ içindeki bir API Management hizmeti oluşturma

Aşağıdaki örnek, bir API Management hizmeti yalnızca iç erişimi için yapılandırılmış bir sanal ağ oluşturma işlemi gösterilmektedir.

### <a name="step-1"></a>1\. Adım

Yukarıda oluşturulan $apimsubnetdata alt ağı kullanarak bir API Yönetim sanal ağ nesnesi oluşturun.

```powershell
$apimVirtualNetwork = New-AzApiManagementVirtualNetwork -SubnetResourceId $apimsubnetdata.Id
```

### <a name="step-2"></a>2\. Adım

Sanal ağ içinde bir API Management hizmeti oluşturun.

```powershell
$apimServiceName = "ContosoApi"       # API Management service instance name
$apimOrganization = "Contoso"         # organization name
$apimAdminEmail = "admin@contoso.com" # administrator's email address
$apimService = New-AzApiManagement -ResourceGroupName $resGroupName -Location $location -Name $apimServiceName -Organization $apimOrganization -AdminEmail $apimAdminEmail -VirtualNetwork $apimVirtualNetwork -VpnType "Internal" -Sku "Developer"
```

Yukarıdaki komut başarılı olduktan sonra başvurmak [DNS yapılandırması gerekli iç sanal ağ API Management hizmeti erişmeye](api-management-using-with-internal-vnet.md#apim-dns-configuration) erişmek için. Bu adım, birden fazla yarım saat sürebilir.

## <a name="set-up-a-custom-domain-name-in-api-management"></a>API Management özel etki alanı Kurulumu

### <a name="step-1"></a>1\. Adım

Etki alanları için özel anahtarları olan sertifikaları, ayrıntılarla aşağıdaki değişkenleri başlatır. Bu örnekte, kullanacağız `api.contoso.net` ve `portal.contoso.net`.  

```powershell
$gatewayHostname = "api.contoso.net"                 # API gateway host
$portalHostname = "portal.contoso.net"               # API developer portal host
$gatewayCertCerPath = "C:\Users\Contoso\gateway.cer" # full path to api.contoso.net .cer file
$gatewayCertPfxPath = "C:\Users\Contoso\gateway.pfx" # full path to api.contoso.net .pfx file
$portalCertPfxPath = "C:\Users\Contoso\portal.pfx"   # full path to portal.contoso.net .pfx file
$gatewayCertPfxPassword = "certificatePassword123"   # password for api.contoso.net pfx certificate
$portalCertPfxPassword = "certificatePassword123"    # password for portal.contoso.net pfx certificate

$certPwd = ConvertTo-SecureString -String $gatewayCertPfxPassword -AsPlainText -Force
$certPortalPwd = ConvertTo-SecureString -String $portalCertPfxPassword -AsPlainText -Force
```

### <a name="step-2"></a>2\. Adım

Oluşturun ve ana bilgisayar yapılandırma nesnelerini proxy için portalı için ayarlayın.  

```powershell
$proxyHostnameConfig = New-AzApiManagementCustomHostnameConfiguration -Hostname $gatewayHostname -HostnameType Proxy -PfxPath $gatewayCertPfxPath -PfxPassword $certPwd
$portalHostnameConfig = New-AzApiManagementCustomHostnameConfiguration -Hostname $portalHostname -HostnameType Portal -PfxPath $portalCertPfxPath -PfxPassword $certPortalPwd

$apimService.ProxyCustomHostnameConfiguration = $proxyHostnameConfig
$apimService.PortalCustomHostnameConfiguration = $portalHostnameConfig
Set-AzApiManagement -InputObject $apimService
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Ön uç yapılandırma için genel bir IP adresi oluşturun

Genel IP kaynağı oluşturun **Publicıp01** kaynak grubunda.

```powershell
$publicip = New-AzPublicIpAddress -ResourceGroupName $resGroupName -name "publicIP01" -location $location -AllocationMethod Dynamic
```

Hizmet başlatıldığında uygulama ağ geçidine bir IP adresi atanır.

## <a name="create-application-gateway-configuration"></a>Uygulama ağ geçidi yapılandırmasını oluşturma

Tüm yapılandırma öğeleri, uygulama ağ geçidi oluşturulmadan önce ayarlanmalıdır. Aşağıdaki adımlar uygulama ağ geçidi kaynağı için gerekli yapılandırma öğelerini oluşturur.

### <a name="step-1"></a>1\. Adım

**gatewayIP01** adlı bir uygulama ağ geçidi IP yapılandırması oluşturun. Application Gateway başladığında, yapılandırılan alt ağdan bir IP adresi alır ve ağ trafiğini arka uç IP havuzundaki IP adreslerine yönlendirir. Her örneğin bir IP adresi aldığını göz önünde bulundurun.

```powershell
$gipconfig = New-AzApplicationGatewayIPConfiguration -Name "gatewayIP01" -Subnet $appgatewaysubnetdata
```

### <a name="step-2"></a>2\. Adım

Genel IP uç noktası için ön uç IP bağlantı noktasını yapılandırın. Bu bağlantı noktası, son kullanıcılarının bağlantı noktasıdır.

```powershell
$fp01 = New-AzApplicationGatewayFrontendPort -Name "port01"  -Port 443
```

### <a name="step-3"></a>3\. Adım

Ön uç IP’sini genel IP uç noktası ile yapılandırın.

```powershell
$fipconfig01 = New-AzApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-4"></a>4\. Adım

Üzerinden geçen trafiğin yeniden şifrelemek ve şifresini çözmek için kullanılan Application Gateway için sertifikaları yapılandırın.

```powershell
$cert = New-AzApplicationGatewaySslCertificate -Name "cert01" -CertificateFile $gatewayCertPfxPath -Password $certPwd
$certPortal = New-AzApplicationGatewaySslCertificate -Name "cert02" -CertificateFile $portalCertPfxPath -Password $certPortalPwd
```

### <a name="step-5"></a>5\. Adım

HTTP dinleyicileri için uygulama ağ geçidi oluşturun. Ön uç IP yapılandırması, bağlantı noktası ve ssl sertifikaları atayabilirsiniz.

```powershell
$listener = New-AzApplicationGatewayHttpListener -Name "listener01" -Protocol "Https" -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $cert -HostName $gatewayHostname -RequireServerNameIndication true
$portalListener = New-AzApplicationGatewayHttpListener -Name "listener02" -Protocol "Https" -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $certPortal -HostName $portalHostname -RequireServerNameIndication true
```

### <a name="step-6"></a>6\. Adım

API Management hizmeti için özel araştırmalara oluşturma `ContosoApi` proxy etki alanı uç noktası. Yolun `/status-0123456789abcdef` olan API Management services üzerinde barındırılan bir varsayılan sistem durumu uç nokta. Ayarlama `api.contoso.net` SSL sertifikası ile güvenli hale getirmek için bir özel araştırma konak adı olarak.

> [!NOTE]
> Ana bilgisayar adı `contosoapi.azure-api.net` adlı bir hizmeti yapılandırılmış varsayılan proxy konak adı olduğundan `contosoapi` genel Azure'da oluşturulur.
>

```powershell
$apimprobe = New-AzApplicationGatewayProbeConfig -Name "apimproxyprobe" -Protocol "Https" -HostName $gatewayHostname -Path "/status-0123456789abcdef" -Interval 30 -Timeout 120 -UnhealthyThreshold 8
$apimPortalProbe = New-AzApplicationGatewayProbeConfig -Name "apimportalprobe" -Protocol "Https" -HostName $portalHostname -Path "/signin" -Interval 60 -Timeout 300 -UnhealthyThreshold 8
```

### <a name="step-7"></a>7\. Adım

SSL etkin arka uç havuzu kaynaklar üzerinde kullanılacak sertifikayı yükleyin. Bu, adım 4'te sağlanan aynı sertifikadır.

```powershell
$authcert = New-AzApplicationGatewayAuthenticationCertificate -Name "whitelistcert1" -CertificateFile $gatewayCertCerPath
```

### <a name="step-8"></a>8\. Adım

Application Gateway için HTTP arka uç ayarlarını yapılandırın. Bu bir sonra bunlar istek iptal edildi arka uç, zaman aşımı sınırını ayarlama içerir. Bu değer, yoklama zaman aşımı farklıdır.

```powershell
$apimPoolSetting = New-AzApplicationGatewayBackendHttpSettings -Name "apimPoolSetting" -Port 443 -Protocol "Https" -CookieBasedAffinity "Disabled" -Probe $apimprobe -AuthenticationCertificates $authcert -RequestTimeout 180
$apimPoolPortalSetting = New-AzApplicationGatewayBackendHttpSettings -Name "apimPoolPortalSetting" -Port 443 -Protocol "Https" -CookieBasedAffinity "Disabled" -Probe $apimPortalProbe -AuthenticationCertificates $authcert -RequestTimeout 180
```

### <a name="step-9"></a>9\. Adım

Adlı bir arka uç IP adresi havuzu yapılandırmak **apimbackend** iç sanal IP adresi API Management hizmetinin, yukarıda oluşturulan.

```powershell
$apimProxyBackendPool = New-AzApplicationGatewayBackendAddressPool -Name "apimbackend" -BackendIPAddresses $apimService.PrivateIPAddresses[0]
```

### <a name="step-10"></a>10\. adım

Application Gateway, temel yönlendirmeyi kullanmak kurallar oluşturun.

```powershell
$rule01 = New-AzApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType Basic -HttpListener $listener -BackendAddressPool $apimProxyBackendPool -BackendHttpSettings $apimPoolSetting
$rule02 = New-AzApplicationGatewayRequestRoutingRule -Name "rule2" -RuleType Basic -HttpListener $portalListener -BackendAddressPool $apimProxyBackendPool -BackendHttpSettings $apimPoolPortalSetting
```

> [!TIP]
> -Kural türü değiştirebilir ve geliştirici portalının belirli sayfalara erişimi kısıtlamak için yönlendirme.

### <a name="step-11"></a>11\. adım

Application Gateway için örnek sayısını ve boyutu yapılandırın. Bu örnekte, kullanıyoruz [WAF SKU](../application-gateway/application-gateway-webapplicationfirewall-overview.md) API Management kaynağının güvenliği artırmak için.

```powershell
$sku = New-AzApplicationGatewaySku -Name "WAF_Medium" -Tier "WAF" -Capacity 2
```

### <a name="step-12"></a>12\. adım

WAF "Önleme" modunda olacak şekilde yapılandırın.

```powershell
$config = New-AzApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

## <a name="create-application-gateway"></a>Uygulama ağ geçidi oluşturma

Önceki adımlarda geçen tüm yapılandırma nesnelerini içeren bir uygulama ağ geçidi oluşturun.

```powershell
$appgwName = "apim-app-gw"
$appgw = New-AzApplicationGateway -Name $appgwName -ResourceGroupName $resGroupName -Location $location -BackendAddressPools $apimProxyBackendPool -BackendHttpSettingsCollection $apimPoolSetting, $apimPoolPortalSetting  -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener, $portalListener -RequestRoutingRules $rule01, $rule02 -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert, $certPortal -AuthenticationCertificates $authcert -Probes $apimprobe, $apimPortalProbe
```

## <a name="cname-the-api-management-proxy-hostname-to-the-public-dns-name-of-the-application-gateway-resource"></a>API Management proxy ana bilgisayar adı uygulama ağ geçidi kaynağının genel DNS adı için CNAME

Ağ geçidi oluşturulduktan sonraki adım, iletişim için ön uç yapılandırması yapmaktır. Bir genel IP kullanırken uygulama ağ geçidi kullanımı kolay olmayabilir bir dinamik olarak atanan DNS adı gerektirir.

Uygulama ağ geçidinin DNS adı, APIM Ara sunucu konak adını işaret eden bir CNAME kaydı oluşturmak için kullanılmalıdır (örneğin `api.contoso.net` Yukarıdaki örneklerde) bu DNS adı. Ön uç IP CNAME kaydını yapılandırmak için uygulama ağ geçidinin ayrıntılarını ve onunla ilişkilendirilmiş IP/DNS adını Publicıpaddress öğesini kullanarak alın. Ağ geçidi başlatmada VIP değişebilir olduğundan A kaydı kullanımı önerilmez.

```powershell
Get-AzPublicIpAddress -ResourceGroupName $resGroupName -Name "publicIP01"
```

## <a name="summary"> </a> Özeti
Şirket içinde veya bulutta barındırılan olup olmadığını bir sanal ağda yapılandırılmış azure API yönetimi için yapılandırılan tüm API'leri, tek bir ağ geçidi arabirimi sağlar. Application Gateway API Management ile tümleştirme, bir ön uç API Management Örneğinize olarak bir Web uygulaması güvenlik duvarı sağlayan yanı sıra belirli API'lerini kullanarak İnternet'ten erişilmesine seçmeli olarak etkinleştirme esnekliği sağlar.

## <a name="next-steps"> </a> Sonraki adımlar
* Azure Application Gateway hakkında daha fazla bilgi edinin
  * [Application Gateway'e genel bakış](../application-gateway/application-gateway-introduction.md)
  * [Application Gateway Web uygulaması güvenlik duvarı](../application-gateway/application-gateway-webapplicationfirewall-overview.md)
  * [Yol tabanlı yönlendirme kullanarak uygulama ağ geçidi](../application-gateway/application-gateway-create-url-route-arm-ps.md)
* API Management ve sanal ağlar hakkında daha fazla bilgi edinin
  * [Sanal ağ içinde yalnızca kullanılabilir API Management'ı kullanma](api-management-using-with-internal-vnet.md)
  * [VNET içinde API Yönetimi'ni kullanma](api-management-using-with-vnet.md)
