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
ms.date: 09/19/2017
ms.author: sasolank
ms.openlocfilehash: c7d4351a9691c9787c42107306220e075f8648a0
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37435132"
---
# <a name="integrate-api-management-in-an-internal-vnet-with-application-gateway"></a>API yönetimi bir iç sanal ağ'ı Application Gateway ile tümleştirme

##<a name="overview"> </a> Genel bakış

API Management hizmeti, bir sanal ağ sanal ağ içindeki yalnızca erişilebilir olmasını sağlayan iç modunda yapılandırılabilir. Azure Application Gateway, bir katman 7 yük dengeleyici sağlayan bir PAAS hizmetidir. Ters proxy hizmeti olarak çalışır ve onun bir Web uygulaması Güvenlik Duvarı (WAF) sunarak arasında sağlar.

API Management'ın bir iç sanal ağ ile uygulama ağ geçidi ön uç sağlanan birleştirme, aşağıdaki senaryolara olanak tanır:

* Hem iç tüketicilere hem de dış tüketiciler tarafından kullanılmaya aynı API Management kaynağının kullanın.
* API'leri dış müşterileri için kullanılabilir API Yönetimi'nde tanımlanan bir alt kümesine sahip ve tek bir API Management kaynak'ı kullanın.
* Anahtar teslim erişim açıp API Management'a genel Internet'ten geçiş olanağını sağlar.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede açıklanan adımları gerçekleştirmek için aşağıdakiler gerekir:

+ Etkin bir Azure aboneliği.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ APIM örneği. Daha fazla bilgi için [Azure API Management örneği oluşturma](get-started-create-service-instance.md).

##<a name="scenario"> </a> Senaryo
Bu makalede, iç ve dış müşteriler için tek bir API Management hizmet ve hem şirket içi için tek bir ön uç işlevi görür ve bulut API'leri nasıl kullanılacağı anlatılmaktadır. Ayrıca, yalnızca bir alt kümesini (yeşil renkte vurgulanmış örnekte) API'leri dış Application Gateway'i kullanılabilir PathBasedRouting işlevini kullanarak tüketimi için nasıl sunacağınızı öğrenin görürsünüz.

İlk kurulum örnekte tüm API'leri yalnızca sanal ağınızdaki yönetilir. İç ve dış tüm Apı'lerinizi iç tüketiciler (vurgulanmış turuncu) erişebilir. Trafik hiçbir zaman bir yüksek performanslı teslim Internet'e Expressroute bağlantı hatları gider.

![URL yolu](./media/api-management-howto-integrate-internal-vnet-appgateway/api-management-howto-integrate-internal-vnet-appgateway.png)

## <a name="before-you-begin"> </a> Başlamadan önce

1. Web Platformu Yükleyicisi’ni kullanarak Azure PowerShell cmdlet’lerin en son sürümünü yükleyin. **İndirmeler sayfası**’ndaki [Windows PowerShell](https://azure.microsoft.com/downloads/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) bölümünden en son sürümü indirip yükleyebilirsiniz.
2. Sanal ağ oluşturma ve API yönetimi ve uygulama ağ geçidi için ayrı alt ağlar oluşturun.
3. Özel bir DNS sunucusu için sanal ağ oluşturmak istiyorsanız dağıtımı başlatmadan önce bunu yapın. Sanal ağda yeni bir alt ağ içinde oluşturulmuş bir sanal makinenin sağlayarak çalıştığını kontrol edin, çözümlemek ve tüm Azure hizmet uç noktalarına erişmesi.

## <a name="what-is-required-to-create-an-integration-between-api-management-and-application-gateway"></a>API yönetimi ve uygulama ağ geçidi arasında bir tümleştirme oluşturmak için gereken nedir?

* **Arka uç sunucu havuzu:** iç sanal IP adresi API Management hizmetinin budur.
* **Arka uç sunucu havuzu ayarları**: Her havuzun bağlantı noktası, protokol ve tanımlama bilgisi temelli benzeşim gibi ayarları vardır. Bu ayarlar, havuzdaki tüm sunuculara uygulanır.
* **Ön uç bağlantı noktası:** bu uygulama ağ geçidinde açılan genel bağlantı noktasıdır. Bu trafiğe arka uç sunuculardan birine yönlendirilir.
* **Dinleyici:** Dinleyicide bir ön uç bağlantı noktası, bir protokol (Http veya Https, bu değerler büyük/küçük harfe duyarlıdır) ve SSL sertifika adı (SSL yük boşaltımı yapılandırılıyorsa) vardır.
* **Kural:** kural, bir arka uç sunucu havuzuna bir dinleyici bağlar.
* **Özel durum araştırması:** Application Gateway, varsayılan olarak, IP adresi tabanlı araştırmaları BackendAddressPool hangi sunucular etkin olmadığını öğrenmek için kullanır. API Management hizmeti yalnızca doğru ana bilgisayar üstbilgisi olan isteklerine yanıt verip, bu nedenle varsayılan araştırmaları başarısız. Özel durum araştırması, uygulama ağ geçidi canlı bir hizmettir ve istekleri iletmek belirlemek amacıyla tanımlanması gerekir.
* **Özel etki alanı sertifikası:** API Management, ana bilgisayar adı ön uç DNS adını uygulama ağ geçidi için bir CNAME eşlemesi oluşturmanız gereken internet'ten erişmek için. Bu API Yönetimi'ne iletilen bir Application Gateway için gönderilen bir sertifika ve ana bilgisayar üst bilgisi bir APIM geçerli olarak tanıyabilmesi sağlar.

## <a name="overview-steps"> </a> API yönetimi ve uygulama ağ geçidi tümleştirmek için gereken adımlar

1. Resource Manager için kaynak grubu oluşturun.
2. Application Gateway için bir sanal ağ, alt ağ ve genel IP oluşturun. API yönetimi için başka bir alt ağ oluşturun.
3. Yukarıda oluşturulan sanal ağ alt ağ içinde bir API Management hizmeti oluşturma ve iç modu kullandığınızdan emin olun.
4. API Management Service'te özel etki alanı adını ayarlayın.
5. Bir uygulama ağ geçidi yapılandırma nesnesi oluşturun.
6. Bir uygulama ağ geçidi kaynağı oluşturun.
7. Genel DNS adını uygulama ağ geçidinin API Management proxy konak adı için bir CNAME oluşturun.

## <a name="create-a-resource-group-for-resource-manager"></a>Resource Manager için kaynak grubu oluşturun

Azure PowerShell’in en yeni sürümünü kullandığınızdan emin olun. Daha fazla bilgi için bkz.[Resource Manager ile Windows PowerShell Kullanma](https://docs.microsoft.com/azure/azure-resource-manager/powershell-azure-resource-manager)

### <a name="step-1"></a>1. Adım

Azure'da oturum açma

```powershell
Connect-AzureRmAccount
```

Kimlik bilgilerinizle kimliğinizi.<BR>

### <a name="step-2"></a>2. Adım

Hesabın aboneliklerini denetleyin ve bu seçeneği belirleyin.

```powershell
Get-AzureRmSubscription -Subscriptionid "GUID of subscription" | Select-AzureRmSubscription
```

### <a name="step-3"></a>3. Adım

Bir kaynak grubu oluşturun (mevcut bir kaynak grubu kullanıyorsanız bu adımı atlayın).

```powershell
New-AzureRmResourceGroup -Name "apim-appGw-RG" -Location "West US"
```
Azure Resource Manager, tüm kaynak gruplarının bir konum belirtmesini gerektirir. Bu, kaynak grubunda kaynaklar için varsayılan konum olarak kullanılır. Tüm uygulama ağ geçidi oluşturma komutlarının aynı kaynak grubunu kullandığından emin olun.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Bir sanal ağ ve uygulama ağ geçidi için bir alt ağ oluşturma

Aşağıdaki örnek, kaynak kullanarak sanal bir ağ yöneticisi oluşturma işlemi gösterilmektedir.

### <a name="step-1"></a>1. Adım

10.0.0.0/24 adres aralığını, sanal ağ oluşturulurken Application Gateway için kullanılacak alt ağ değişkenine atayın.

```powershell
$appgatewaysubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim01" -AddressPrefix "10.0.0.0/24"
```

### <a name="step-2"></a>2. Adım

Adres aralığı 10.0.1.0/24 bir sanal ağ oluşturulurken API yönetimi için kullanılacak alt ağ değişkenine atayın.

```powershell
$apimsubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim02" -AddressPrefix "10.0.1.0/24"
```

### <a name="step-3"></a>3. Adım

Adlı bir sanal ağ oluşturma **appgwvnet** kaynak grubundaki **apim-appGw-RG** önek 10.0.0.0/16 kullanarak Batı ABD bölgesi için 10.0.0.0/24 alt ağlar ve 10.0.1.0/24.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name "appgwvnet" -ResourceGroupName "apim-appGw-RG" -Location "West US" -AddressPrefix "10.0.0.0/16" -Subnet $appgatewaysubnet,$apimsubnet
```

### <a name="step-4"></a>4. Adım

Sonraki adımlar için alt ağ değişkenine atayın

```powershell
$appgatewaysubnetdata=$vnet.Subnets[0]
$apimsubnetdata=$vnet.Subnets[1]
```
## <a name="create-an-api-management-service-inside-a-vnet-configured-in-internal-mode"></a>İç modunda yapılandırılmış bir sanal ağ içindeki bir API Management hizmeti oluşturma

Aşağıdaki örnek, bir API Management hizmeti yalnızca iç erişimi için yapılandırılmış bir sanal ağ oluşturma işlemi gösterilmektedir.

### <a name="step-1"></a>1. Adım
Yukarıda oluşturulan $apimsubnetdata alt ağı kullanarak bir API Yönetim sanal ağ nesnesi oluşturun.

```powershell
$apimVirtualNetwork = New-AzureRmApiManagementVirtualNetwork -Location "West US" -SubnetResourceId $apimsubnetdata.Id
```
### <a name="step-2"></a>2. Adım
Sanal ağ içinde bir API Management hizmeti oluşturun.

```powershell
$apimService = New-AzureRmApiManagement -ResourceGroupName "apim-appGw-RG" -Location "West US" -Name "ContosoApi" -Organization "Contoso" -AdminEmail "admin@contoso.com" -VirtualNetwork $apimVirtualNetwork -VpnType "Internal" -Sku "Developer"
```
Yukarıdaki komut başarılı olduktan sonra başvurmak [DNS yapılandırması gerekli iç sanal ağ API Management hizmeti erişmeye](api-management-using-with-internal-vnet.md#apim-dns-configuration) erişmek için.

## <a name="set-up-a-custom-domain-name-in-api-management"></a>API Management özel etki alanı Kurulumu

### <a name="step-1"></a>1. Adım
Etki alanı için özel anahtarla sertifikayı yükleyin. Bu örnekte olur `*.contoso.net`.

```powershell
$certUploadResult = Import-AzureRmApiManagementHostnameCertificate -ResourceGroupName "apim-appGw-RG" -Name "ContosoApi" -HostnameType "Proxy" -PfxPath <full path to .pfx file> -PfxPassword <password for certificate file> -PassThru
```

### <a name="step-2"></a>2. Adım
Sertifika karşıya yüklendikten sonra bir ana bilgisayar adını proxy'si için bir ana bilgisayar adı yapılandırma nesnesini oluşturun `api.contoso.net`, örnek sertifika yetkilisi için sağladığından `*.contoso.net` etki alanı.

```powershell
$proxyHostnameConfig = New-AzureRmApiManagementHostnameConfiguration -CertificateThumbprint $certUploadResult.Thumbprint -Hostname "api.contoso.net"
$result = Set-AzureRmApiManagementHostnames -Name "ContosoApi" -ResourceGroupName "apim-appGw-RG" -ProxyHostnameConfiguration $proxyHostnameConfig
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Ön uç yapılandırma için genel bir IP adresi oluşturun

Genel IP kaynağı oluşturun **Publicıp01** kaynak grubundaki **apim-appGw-RG** Batı ABD bölgesi için.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -name "publicIP01" -location "West US" -AllocationMethod Dynamic
```

Hizmet başlatıldığında uygulama ağ geçidine bir IP adresi atanır.

## <a name="create-application-gateway-configuration"></a>Uygulama ağ geçidi yapılandırmasını oluşturma

Tüm yapılandırma öğeleri, uygulama ağ geçidi oluşturulmadan önce ayarlanmalıdır. Aşağıdaki adımlar uygulama ağ geçidi kaynağı için gerekli yapılandırma öğelerini oluşturur.

### <a name="step-1"></a>1. Adım

**gatewayIP01** adlı bir uygulama ağ geçidi IP yapılandırması oluşturun. Application Gateway başladığında, yapılandırılan alt ağdan bir IP adresi alır ve ağ trafiğini arka uç IP havuzundaki IP adreslerine yönlendirir. Her örneğin bir IP adresi aldığını göz önünde bulundurun.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name "gatewayIP01" -Subnet $appgatewaysubnetdata
```

### <a name="step-2"></a>2. Adım

Genel IP uç noktası için ön uç IP bağlantı noktasını yapılandırın. Bu bağlantı noktası, son kullanıcılarının bağlantı noktasıdır.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "port01"  -Port 443
```
### <a name="step-3"></a>3. Adım

Ön uç IP’sini genel IP uç noktası ile yapılandırın.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-4"></a>4. Adım

Üzerinden geçen trafiğin yeniden şifrelemek ve şifresini çözmek için kullanılan uygulama ağ geçidi için sertifika yapılandırın.

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name "cert01" -CertificateFile <full path to .pfx file> -Password <password for certificate file>
```

### <a name="step-5"></a>5. Adım

HTTP dinleyicisi için uygulama ağ geçidi oluşturun. Ön uç IP yapılandırması, bağlantı noktası ve ssl sertifika atayabilirsiniz.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol "Https" -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $cert
```

### <a name="step-6"></a>6. Adım

API Management hizmeti için özel bir araştırma oluşturma `ContosoApi` proxy etki alanı uç noktası. Yolun `/status-0123456789abcdef` olan API Management services üzerinde barındırılan bir varsayılan sistem durumu uç nokta. Ayarlama `api.contoso.net` SSL sertifikası ile güvenli hale getirmek için bir özel araştırma konak adı olarak.

> [!NOTE]
> Ana bilgisayar adı `contosoapi.azure-api.net` adlı bir hizmeti yapılandırılmış varsayılan proxy konak adı olduğundan `contosoapi` genel Azure'da oluşturulur.
>

```powershell
$apimprobe = New-AzureRmApplicationGatewayProbeConfig -Name "apimproxyprobe" -Protocol "Https" -HostName "api.contoso.net" -Path "/status-0123456789abcdef" -Interval 30 -Timeout 120 -UnhealthyThreshold 8
```

### <a name="step-7"></a>7. Adım

SSL etkin arka uç havuzu kaynaklar üzerinde kullanılacak sertifikayı yükleyin. Bu, adım 4'te sağlanan aynı sertifikadır.

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name "whitelistcert1" -CertificateFile <full path to .cer file>
```

### <a name="step-8"></a>8. Adım

Application Gateway için HTTP arka uç ayarlarını yapılandırın. Bu, daha sonra iptal arka uç isteği için bir zaman aşımı sınırını ayarlama içerir. Bu değer, yoklama zaman aşımı farklıdır.

```powershell
$apimPoolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "apimPoolSetting" -Port 443 -Protocol "Https" -CookieBasedAffinity "Disabled" -Probe $apimprobe -AuthenticationCertificates $authcert -RequestTimeout 180
```

### <a name="step-9"></a>9. Adım

Adlı bir arka uç IP adresi havuzu yapılandırmak **apimbackend** iç sanal IP adresi API Management hizmetinin, yukarıda oluşturulan.

```powershell
$apimProxyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "apimbackend" -BackendIPAddresses $apimService.StaticIPs[0]
```

### <a name="step-10"></a>10. adım

Ayarları bir kukla (var olmayan) arka uç oluşturun. API Management uygulama ağ geçidi aracılığıyla kullanıma sunmak istiyoruz değil API yolları isteklerini, bu arka uç isabet ve 404 döndüren.

İşlevsiz arka uç HTTP ayarları yapılandırın.

```powershell
$dummyBackendSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "dummySetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

İşlevsiz bir arka uç yapılandırma **dummyBackendPool**, işaret ettiği için bir FQDN adresi **dummybackend.com**. Bu FQDN adresi sanal ağ içinde mevcut değil.

```powershell
$dummyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "dummyBackendPool" -BackendFqdns "dummybackend.com"
```

Uygulama ağ geçidi mevcut olmayan arka ucuna işaret eden varsayılan olarak kullanacağı bir kural ayarı oluşturma **dummybackend.com** sanal ağ.

```powershell
$dummyPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "nonexistentapis" -Paths "/*" -BackendAddressPool $dummyBackendPool -BackendHttpSettings $dummyBackendSetting
```

### <a name="step-11"></a>11. adım

Arka uç havuzu için URL kural yolları yapılandırın. Bu, yalnızca bazı API'leri genel kullanıma için API Yönetimi'nden seçerek sağlar. Örneğin, varsa `Echo API` (/ echo /) `Calculator API` yalnızca (/calc/) vb. yapma `Echo API` Internet'ten erişilebilir).

Aşağıdaki örnek, "/ echo /" yolu yönlendirme trafiği arka uç "apimProxyBackendPool" için basit bir kuralı oluşturur.

```powershell
$echoapiRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "externalapis" -Paths "/echo/*" -BackendAddressPool $apimProxyBackendPool -BackendHttpSettings $apimPoolSetting
```

Yolun istiyoruz API Yönetimi'nden etkinleştirmek için yol ekleme kuralları eşleşmiyorsa, kural yol haritası Yapılandırması adlı varsayılan bir arka uç adres havuzu da yapılandırır. **dummyBackendPool**. Örneğin, http://api.contoso.net/calc/sum gider **dummyBackendPool** beklemediğiniz eşleşen trafik için varsayılan havuzu olarak tanımlanan.

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $echoapiRule, $dummyPathRule -DefaultBackendAddressPool $dummyBackendPool -DefaultBackendHttpSettings $dummyBackendSetting
```

Yukarıdaki adım yolu yalnızca istekleri sağlar. "/ echo" uygulama ağ geçidi üzerinden izin verilir. API Yönetimi'nde yapılandırılan diğer API'ler isteklerine 404 hataları uygulama Internet'ten erişilen ağ geçidi'ndeki durum oluşturur.

### <a name="step-12"></a>12. adım

URL yolu tabanlı yönlendirmeyi kullanmak uygulama ağ geçidi için bir kural ayarı oluşturun.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-13"></a>13. adım

Application Gateway için örnek sayısını ve boyutu yapılandırın. Burada kullandığımız [WAF SKU](../application-gateway/application-gateway-webapplicationfirewall-overview.md) API Management kaynağının güvenliği artırmak için.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "WAF_Medium" -Tier "WAF" -Capacity 2
```

### <a name="step-14"></a>14. adım

WAF "Önleme" modunda olacak şekilde yapılandırın.
```powershell
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

## <a name="create-application-gateway"></a>Uygulama ağ geçidi oluşturma

Önceki adımlarda geçen tüm yapılandırma nesnelerini içeren bir uygulama ağ geçidi oluşturun.

```powershell
$appgw = New-AzureRmApplicationGateway -Name $applicationGatewayName -ResourceGroupName $resourceGroupName  -Location $location -BackendAddressPools $apimProxyBackendPool, $dummyBackendPool -BackendHttpSettingsCollection $apimPoolSetting, $dummyBackendSetting  -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert -Probes $apimprobe
```

## <a name="cname-the-api-management-proxy-hostname-to-the-public-dns-name-of-the-application-gateway-resource"></a>API Management proxy ana bilgisayar adı uygulama ağ geçidi kaynağının genel DNS adı için CNAME

Ağ geçidi oluşturulduktan sonraki adım, iletişim için ön uç yapılandırması yapmaktır. Bir genel IP kullanırken uygulama ağ geçidi kullanımı kolay olmayabilir bir dinamik olarak atanan DNS adı gerektirir.

Uygulama ağ geçidinin DNS adı, APIM Ara sunucu konak adını işaret eden bir CNAME kaydı oluşturmak için kullanılmalıdır (örneğin `api.contoso.net` Yukarıdaki örneklerde) bu DNS adı. Ön uç IP CNAME kaydını yapılandırmak için uygulama ağ geçidinin ayrıntılarını ve onunla ilişkilendirilmiş IP/DNS adını Publicıpaddress öğesini kullanarak alın. Ağ geçidi başlatmada VIP değişebilir olduğundan A kaydı kullanımı önerilmez.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -Name "publicIP01"
```

##<a name="summary"> </a> Özeti
Azure API yönetimi bir sanal ağda yapılandırılmış tek bir ağ geçidi arabirimi için yapılandırılan tüm API'leri, barındırılan şirket içinde olmalarından veya bulutta sağlar. Application Gateway API Management ile tümleştirme, bir ön uç API Management Örneğinize olarak bir Web uygulaması güvenlik duvarı sağlayan yanı sıra belirli API'lerini kullanarak İnternet'ten erişilmesine seçmeli olarak etkinleştirme esnekliği sağlar.

##<a name="next-steps"> </a> Sonraki adımlar
* Azure Application Gateway hakkında daha fazla bilgi edinin
  * [Application Gateway'e genel bakış](../application-gateway/application-gateway-introduction.md)
  * [Application Gateway Web uygulaması güvenlik duvarı](../application-gateway/application-gateway-webapplicationfirewall-overview.md)
  * [Yol tabanlı yönlendirme kullanarak uygulama ağ geçidi](../application-gateway/application-gateway-create-url-route-arm-ps.md)
* API Management ve sanal ağlar hakkında daha fazla bilgi edinin
  * [Sanal ağ içinde yalnızca kullanılabilir API Management'ı kullanma](api-management-using-with-internal-vnet.md)
  * [VNET içinde API Yönetimi'ni kullanma](api-management-using-with-vnet.md)
