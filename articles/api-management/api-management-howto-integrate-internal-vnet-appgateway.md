---
title: "Uygulama ağ geçidi ile sanal ağındaki Azure API Management kullanma | Microsoft Docs"
description: "Kurulum ve iç sanal ağ ile uygulama ağ geçidi (WAF) ön uç olarak Azure API Management yapılandırmak hakkında bilgi edinin"
services: api-management
documentationcenter: 
author: solankisamir
manager: kjoshi
editor: antonba
ms.assetid: a8c982b2-bca5-4312-9367-4a0bbc1082b1
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: sasolank
ms.openlocfilehash: e138241139329b8bb956157ab55b7d22dc2a9b67
ms.sourcegitcommit: 4ea06f52af0a8799561125497f2c2d28db7818e7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2017
---
# <a name="integrate-api-management-in-an-internal-vnet-with-application-gateway"></a>Bir iç sanal ağ API Management'te uygulama ağ geçidi ile tümleştirme 

##<a name="overview"></a> Genel bakış
 
API Management hizmeti, bir sanal ağdaki sanal ağda yalnızca erişilebilir kılan iç modunda yapılandırılabilir. Azure uygulama ağ geçidi bir katman 7 yük dengeleyici sağlayan bir PAAS hizmetidir. Ters proxy hizmeti davranır ve onun bir Web uygulaması Güvenlik Duvarı (WAF) sunan arasında sağlar.

API uygulama ağ geçidi ön uç ile dahili bir VNET içinde sağlanan yönetim birleştirme, aşağıdaki senaryolarda sağlar:

* Hem iç tüketiciler hem de dış tüketiciler tarafından tüketimi için aynı API Management kaynağı kullanın.
* Bir alt kümesini API'leri dış Tüketiciler için kullanılabilir API Management tanımladığınız ve tek bir API Management kaynağı kullanın.
* API Management genel Internet'ten açma ve kapatma anahtar erişimi için bir anahtar teslim yol sağlar. 

## <a name="prerequisites"></a>Ön koşullar

Bu makalede açıklanan adımları gerçekleştirmek için şunlara sahip olmalısınız:

+ Etkin bir Azure aboneliği.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ APIM örneği. Daha fazla bilgi için bkz: [bir Azure API Management örneği oluşturma](get-started-create-service-instance.md).

##<a name="scenario"></a> Senaryosu
Bu makalede, iç ve dış tüketicileri için tek bir API Management hizmeti kullanmak ve her iki şirket içi için tek bir ön uç görevi görür ve bulut API'leri hale alınmaktadır. Ayrıca dış uygulama ağ geçidi mevcut PathBasedRouting işlevselliğini kullanarak tüketimi için yalnızca bir alt kümesini Apı'lerinizi (yeşil renkte vurgulanır örnekte) kullanıma sunmak nasıl görürsünüz.

İlk kurulum örnekte tüm API'leri yalnızca sanal ağınızın içinde yönetilir. İç tüketicileri (vurgulanmış turuncu) tüm iç ve dış API'leri erişebilir. Trafik hiçbir zaman yüksek performanslı teslim Internet'e Expressroute bağlantı hatları gider.

![URL rota](./media/api-management-howto-integrate-internal-vnet-appgateway/api-management-howto-integrate-internal-vnet-appgateway.png)

## <a name="before-you-begin"></a> Başlamadan önce

1. Web Platformu Yükleyicisi’ni kullanarak Azure PowerShell cmdlet’lerin en son sürümünü yükleyin. **İndirmeler sayfası**’ndaki [Windows PowerShell](https://azure.microsoft.com/downloads/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) bölümünden en son sürümü indirip yükleyebilirsiniz.
2. Bir sanal ağ oluşturma ve API Management ve uygulama ağ geçidi için ayrı alt ağlar oluşturun. 
3. Sanal ağ için özel bir DNS sunucusu oluşturmak istiyorsanız, dağıtıma başlamadan önce bunu. Sanal ağda yeni bir alt ağ içinde oluşturulmuş bir sanal makinenin sağlayarak çalıştığını kontrol edin, çözümlemek ve tüm Azure hizmet uç noktalarına erişebilirsiniz.

## <a name="what-is-required-to-create-an-integration-between-api-management-and-application-gateway"></a>API Management ile uygulama ağ geçidi arasında bir tümleştirme oluşturmak için gerekli nedir?

* **Arka uç sunucusu havuzu:** API Management hizmeti iç sanal IP adresine budur.
* **Arka uç sunucu havuzu ayarları**: Her havuzun bağlantı noktası, protokol ve tanımlama bilgisi temelli benzeşim gibi ayarları vardır. Bu ayarlar, havuzdaki tüm sunuculara uygulanır.
* **Ön uç bağlantı noktası:** bu uygulama ağ geçidinde açılan genel bağlantı noktasıdır. Bunu basarsa trafiği arka uç sunuculardan birine yönlendirilir.
* **Dinleyici:** Dinleyicide bir ön uç bağlantı noktası, bir protokol (Http veya Https, bu değerler büyük/küçük harfe duyarlıdır) ve SSL sertifika adı (SSL yük boşaltımı yapılandırılıyorsa) vardır.
* **Kural:** kural dinleyici bir arka uç sunucu havuzuna bağlar.
* **Özel durum araştırması:** uygulama ağ geçidi, varsayılan olarak, kullanan IP adreslerini göre araştırmalar BackendAddressPool hangi sunucuları etkin olduğunu anlamak için. API Management hizmeti yalnızca doğru ana bilgisayar üstbilgisi olan isteklerine yanıt verir, bu nedenle varsayılan araştırmalar başarısız. Bir özel durum araştırması uygulama ağ geçidi hizmeti kullanımda ve isteklerini iletmek belirlemek amacıyla tanımlanması gerekiyor.
* **Özel etki alanı sertifikası:** API Management kendi ana bilgisayar adı uygulama ağ geçidi ön uç DNS adına CNAME eşlemesi oluşturmanız internet'ten erişmek için. Bu uygulama için API Management ileten ağ geçidi için gönderilen sertifikayı ve ana bilgisayar üstbilgisi bir APIM geçerli olarak tanıyabilmesi için sağlanır.

## <a name="overview-steps"></a> API Management ve uygulama ağ geçidi tümleştirmek için gerekli adımları 

1. Resource Manager için kaynak grubu oluşturun.
2. Uygulama ağ geçidi için bir sanal ağ alt ağı ve genel IP oluşturun. API yönetimi için başka bir alt ağ oluşturun.
3. Yukarıda oluşturduğunuz sanal alt ağ içinde bir API Management hizmeti oluşturma ve iç modu kullandığınızdan emin olun.
4. API Management hizmetinde özel etki alanı adı ayarlayın.
5. Bir uygulama ağ geçidi yapılandırma nesnesi oluşturun.
6. Bir uygulama ağ geçidi kaynağı oluşturun.
7. Uygulama ağ geçidi API Management proxy ana bilgisayar adı için Genel DNS adından bir CNAME oluşturun.

## <a name="create-a-resource-group-for-resource-manager"></a>Resource Manager için kaynak grubu oluşturun

Azure PowerShell’in en yeni sürümünü kullandığınızdan emin olun. Daha fazla bilgi için bkz.[Resource Manager ile Windows PowerShell Kullanma](https://docs.microsoft.com/en-us/azure/azure-resource-manager/powershell-azure-resource-manager)

### <a name="step-1"></a>1. Adım

Azure'da oturum açma

```powershell
Login-AzureRmAccount
```

Kimlik bilgilerinizle kimlik doğrulaması.<BR>

### <a name="step-2"></a>2. Adım

Hesap için abonelikleri kontrol edin ve seçin.

```powershell
Get-AzureRmSubscription -Subscriptionid "GUID of subscription" | Select-AzureRmSubscription
```

### <a name="step-3"></a>3. Adım

Bir kaynak grubu oluşturun (mevcut bir kaynak grubu kullanıyorsanız bu adımı atlayın).

```powershell
New-AzureRmResourceGroup -Name "apim-appGw-RG" -Location "West US"
```
Azure Resource Manager, tüm kaynak gruplarının bir konum belirtmesini gerektirir. Bu, kaynak grubunda kaynaklar için varsayılan konum olarak kullanılır. Bir uygulama ağ geçidi oluşturmak için verilecek komutların aynı kaynak grubunda kullandığınızdan emin olun.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Bir sanal ağ ve uygulama ağ geçidi için bir alt ağ oluşturma

Aşağıdaki örnek Yöneticisi kaynağı kullanan bir sanal ağ oluşturulacağını gösterir.

### <a name="step-1"></a>1. Adım

10.0.0.0/24 adres aralığını, sanal ağ oluşturulurken uygulama ağ geçidi için kullanılacak bir alt ağ değişkenine atayın.

```powershell
$appgatewaysubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim01" -AddressPrefix "10.0.0.0/24"
```

### <a name="step-2"></a>2. Adım

Adres aralığı 10.0.1.0/24 bir sanal ağ oluşturulurken API yönetimi için kullanılacak bir alt ağ değişkenine atayın.

```powershell
$apimsubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim02" -AddressPrefix "10.0.1.0/24"
```

### <a name="step-3"></a>3. Adım

Adlı bir sanal ağ oluşturma **appgwvnet** kaynak grubunda **apim-appGw-RG** önek 10.0.0.0/16 kullanarak Batı ABD bölgesi için 10.0.0.0/24 alt ağları ve 10.0.1.0/24.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name "appgwvnet" -ResourceGroupName "apim-appGw-RG" -Location "West US" -AddressPrefix "10.0.0.0/16" -Subnet $appgatewaysubnet,$apimsubnet
```

### <a name="step-4"></a>4. Adım

Sonraki adımlar için bir alt ağ değişkeni atayın

```powershell
$appgatewaysubnetdata=$vnet.Subnets[0]
$apimsubnetdata=$vnet.Subnets[1]
```
## <a name="create-an-api-management-service-inside-a-vnet-configured-in-internal-mode"></a>İç modunda yapılandırılmış bir sanal ağ içinde bir API Management hizmeti oluşturma

Aşağıdaki örnekte nasıl bir API Management hizmeti yalnızca iç erişimi için yapılandırılmış bir sanal ağ oluşturulacağını gösterir.

### <a name="step-1"></a>1. Adım
Yukarıda oluşturduğunuz $apimsubnetdata alt ağı kullanarak bir API Yönetim sanal ağ nesnesi oluşturun.

```powershell
$apimVirtualNetwork = New-AzureRmApiManagementVirtualNetwork -Location "West US" -SubnetResourceId $apimsubnetdata.Id
```
### <a name="step-2"></a>2. Adım
Sanal ağ içindeki bir API Management hizmeti oluşturun.

```powershell
$apimService = New-AzureRmApiManagement -ResourceGroupName "apim-appGw-RG" -Location "West US" -Name "ContosoApi" -Organization "Contoso" -AdminEmail "admin@contoso.com" -VirtualNetwork $apimVirtualNetwork -VpnType "Internal" -Sku "Developer"
```
Yukarıdaki komut başarılı olduktan sonra başvurmak [DNS yapılandırma gerekli iç VNET API Management hizmetine erişmek için](api-management-using-with-internal-vnet.md#apim-dns-configuration) erişmek için.

## <a name="set-up-a-custom-domain-name-in-api-management"></a>Kurulum API Management'te özel etki alanı adı

### <a name="step-1"></a>1. Adım
Etki alanı için özel anahtara sahip sertifika yükleyin. Bu örnek için olacak `*.contoso.net`. 

```powershell
$certUploadResult = Import-AzureRmApiManagementHostnameCertificate -ResourceGroupName "apim-appGw-RG" -Name "ContosoApi" -HostnameType "Proxy" -PfxPath <full path to .pfx file> -PfxPassword <password for certificate file> -PassThru
```

### <a name="step-2"></a>2. Adım
Sertifika yüklendikten sonra ana bilgisayar adını proxy'si için ana bilgisayar yapılandırma nesnesi oluşturun `api.contoso.net`, örnek sertifika yetkilisi için sağladığından `*.contoso.net` etki alanı. 

```powershell
$proxyHostnameConfig = New-AzureRmApiManagementHostnameConfiguration -CertificateThumbprint $certUploadResult.Thumbprint -Hostname "api.contoso.net"
$result = Set-AzureRmApiManagementHostnames -Name "ContosoApi" -ResourceGroupName "apim-appGw-RG" -ProxyHostnameConfiguration $proxyHostnameConfig
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Ön uç yapılandırma için genel bir IP adresi oluşturun

Genel IP kaynağı oluşturun **Publicıp01** kaynak grubunda **apim-appGw-RG** Batı ABD bölgesi için.

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

Genel IP uç noktası için ön uç IP bağlantı noktası yapılandırın. Bu bağlantı noktası, son kullanıcılara bağlanan bağlantı noktasıdır.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "port01"  -Port 443
```
### <a name="step-3"></a>3. Adım

Ön uç IP’sini genel IP uç noktası ile yapılandırın.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-4"></a>4. Adım

Komut zincirinden geçen trafik yeniden şifrelemek ve şifresini çözmek için kullanılan uygulama ağ geçidi için sertifika yapılandırın.

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name "cert01" -CertificateFile <full path to .pfx file> -Password <password for certificate file>
```

### <a name="step-5"></a>5. Adım

HTTP dinleyicisi için uygulama ağ geçidi oluşturun. Ön uç IP yapılandırması, bağlantı noktası ve ssl sertifika atayabilirsiniz.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol "Https" -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $cert
```

### <a name="step-6"></a>6. Adım

API Management hizmeti için özel bir araştırma oluşturmak `ContosoApi` proxy etki alanı uç noktası. Yolun `/status-0123456789abcdef` olan varsayılan, tüm API Management services üzerinde barındırılan bir sistem durumu uç. Ayarlama `api.contoso.net` SSL sertifikası ile güvenli hale getirmek için bir özel araştırma ana bilgisayar adı olarak.

> [!NOTE]
> Ana bilgisayar adı `contosoapi.azure-api.net` olan adlı bir hizmette, yapılandırılan varsayılan proxy ana bilgisayar adı `contosoapi` ortak Azure içinde oluşturulur. 
> 

```powershell
$apimprobe = New-AzureRmApplicationGatewayProbeConfig -Name "apimproxyprobe" -Protocol "Https" -HostName "api.contoso.net" -Path "/status-0123456789abcdef" -Interval 30 -Timeout 120 -UnhealthyThreshold 8
```

### <a name="step-7"></a>7. Adım

SSL etkin arka uç havuzu kaynaklardaki kullanılan sertifikayı karşıya yükleyin. Bu, adım 4'te sağlanan aynı sertifikadır.

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name "whitelistcert1" -CertificateFile <full path to .cer file>
```

### <a name="step-8"></a>8. Adım

Uygulama ağ geçidi için HTTP arka uç ayarlarını yapılandırın. Bu, daha sonra iptal arka uç istek için zaman aşımı sınırı ayarı içerir. Bu değer, yoklama zaman aşımı farklıdır.

```powershell
$apimPoolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "apimPoolSetting" -Port 443 -Protocol "Https" -CookieBasedAffinity "Disabled" -Probe $apimprobe -AuthenticationCertificates $authcert -RequestTimeout 180
```

### <a name="step-9"></a>9. Adım

Adlı bir arka uç IP adresi havuzu yapılandırmak **apimbackend** yukarıda iç sanal IP adresi API Management hizmeti, oluşturduğunuz.

```powershell
$apimProxyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "apimbackend" -BackendIPAddresses $apimService.StaticIPs[0]
```

### <a name="step-10"></a>10. adım

Bir kukla (mevcut olmayan) arka uç ayarlarını oluşturun. API Management uygulama ağ geçidi aracılığıyla kullanıma sunmak için istiyoruz değil API yolları isteklerine bu arka uç isabet ve 404 döndürür.

Sahte arka uç HTTP ayarları yapılandırın.

```powershell
$dummyBackendSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "dummySetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

Sahte bir arka uç yapılandırma **dummyBackendPool**, bir FQDN adresine işaret eden **dummybackend.com**. Bu FQDN adresi sanal ağında yok.

```powershell
$dummyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "dummyBackendPool" -BackendFqdns "dummybackend.com"
```

Uygulama ağ geçidi için mevcut olmayan arka uç noktaları varsayılan olarak kullanacağı bir kural ayarı oluşturma **dummybackend.com** sanal ağda.

```powershell
$dummyPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "nonexistentapis" -Paths "/*" -BackendAddressPool $dummyBackendPool -BackendHttpSettings $dummyBackendSetting
```

### <a name="step-11"></a>11. adım

URL kuralı yolları arka uç havuzları için yapılandırın. Bu API'ları yalnızca bir kısmını herkese açık için API Yönetimi'nden seçerek sağlar. Örneğin, varsa `Echo API` (/ Yankı /) `Calculator API` (/calc/) vb. yapma yalnızca `Echo API` Internet'ten erişilebilir). 

Aşağıdaki örnek, "/ Yankı /" yol yönlendirme trafiği için arka uç "apimProxyBackendPool" basit bir kural oluşturur.

```powershell
$echoapiRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "externalapis" -Paths "/echo/*" -BackendAddressPool $apimProxyBackendPool -BackendHttpSettings $apimPoolSetting
```

Yolun istiyoruz API Yönetimi'nden etkinleştirmek için yol kuralları eşleşmiyorsa, kural yol haritası yapılandırmasını da adlı bir varsayılan arka uç adres havuzu yapılandırır **dummyBackendPool**. Örneğin, http://api.contoso.net/calc/ * gider **dummyBackendPool** beklemediğiniz eşleşen trafik için varsayılan havuzu olarak tanımlanan.

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $echoapiRule, $dummyPathRule -DefaultBackendAddressPool $dummyBackendPool -DefaultBackendHttpSettings $dummyBackendSetting
```

Yukarıdaki adımı yolu yalnızca istekleri sağlar "/ echo" uygulama ağ geçidi üzerinden izin verilir. API Yönetimi'nde yapılandırılmış diğer API'leri isteklerine Internet'ten erişilen uygulama geçidinden 404 hataları atar. 

### <a name="step-12"></a>12. adımı

Yol tabanlı URL yönlendirmeyi kullanmak uygulama ağ geçidi için bir kuralı ayarı oluşturun.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-13"></a>13. adım

Uygulama ağ geçidi için örnekleri ve boyutu sayısını yapılandırın. Burada kullanıyoruz [WAF SKU](../application-gateway/application-gateway-webapplicationfirewall-overview.md) API Management kaynağının güvenliği artırmak için.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "WAF_Medium" -Tier "WAF" -Capacity 2
```

### <a name="step-14"></a>14. adım

WAF "Önleme" modunda olacak şekilde yapılandırın.
```powershell
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

## <a name="create-application-gateway"></a>Uygulama ağ geçidi oluşturma

Yukarıdaki adımlarda geçen tüm yapılandırma nesne içeren bir uygulama ağ geçidi oluşturun.

```powershell
$appgw = New-AzureRmApplicationGateway -Name $applicationGatewayName -ResourceGroupName $resourceGroupName  -Location $location -BackendAddressPools $apimProxyBackendPool, $dummyBackendPool -BackendHttpSettingsCollection $apimPoolSetting, $dummyBackendSetting  -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert -Probes $apimprobe
```

## <a name="cname-the-api-management-proxy-hostname-to-the-public-dns-name-of-the-application-gateway-resource"></a>CNAME uygulama ağ geçidi kaynak ortak DNS adına API Management proxy konak adı

Ağ geçidi oluşturulduktan sonraki adım, iletişim için ön uç yapılandırması yapmaktır. Genel IP kullanırken, uygulama ağ geçidi kullanımı kolay olmayabilir dinamik olarak atanmış bir DNS adı gerektirir. 

Uygulama ağ geçidi DNS adı APIM proxy ana bilgisayar adını gösteren bir CNAME kaydı oluşturmak için kullanılması gereken (örneğin `api.contoso.net` Yukarıdaki örneklerde) bu DNS adı. Ön uç IP CNAME kaydını yapılandırmak için uygulama ağ geçidi ve Publicıpaddress öğesini kullanarak ilişkili IP/DNS adı ayrıntılarını alamadı. Ağ geçidi başlatmada VIP değişebileceği A kayıtlarını kullanılması önerilmez.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -Name "publicIP01"
```

##<a name="summary"></a> Özeti
Azure API Management sanal ağ içinde yapılandırılmış bir tek ağ geçidi arabirimi barındırılan şirket içi olmalarından bağımsız veya bulutta tüm yapılandırılmış API'ler sağlar. Uygulama ağ geçidi API Management ile tümleştirme, API Management örneği için bir ön olarak bir Web uygulaması güvenlik duvarı sağlama yanı sıra seçmeli olarak Internet üzerinden erişilebilir olması için belirli API'ler etkinleştirme esnekliğini sağlar.

##<a name="next-steps"></a> Sonraki adımlar
* Azure uygulama ağ geçidi hakkında daha fazla bilgi edinin
  * [Uygulama ağ geçidi'ne genel bakış](../application-gateway/application-gateway-introduction.md)
  * [Uygulama ağ geçidi Web uygulaması güvenlik duvarı](../application-gateway/application-gateway-webapplicationfirewall-overview.md)
  * [Yol tabanlı yönlendirme kullanarak uygulama ağ geçidi](../application-gateway/application-gateway-create-url-route-arm-ps.md)
* API Management ve sanal ağlar hakkında daha fazla bilgi edinin
  * [Yalnızca sanal ağ içinde kullanılabilir API Management kullanma](api-management-using-with-internal-vnet.md)
  * [VNET içinde API Management kullanma](api-management-using-with-vnet.md)
