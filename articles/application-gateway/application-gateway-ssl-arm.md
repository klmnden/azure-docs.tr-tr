---
title: Azure Resource Manager kullanarak SSL yük boşaltımı için bir uygulama ağ geçidi oluşturma | Microsoft Docs
description: Bu sayfada Azure Resource Manager kullanarak SSL yük boşaltımıyla uygulama ağ geçidi oluşturmak için yönergeler bulunmaktadır.
documentationcenter: na
services: application-gateway
author: georgewallace
manager: carmonm
editor: tysonn

ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/09/2016
ms.author: gwallace

---
# Azure Resource Manager kullanarak SSL yük boşaltımı için bir uygulama ağ geçidi oluşturma
> [!div class="op_single_selector"]
> -[Azure Portal](application-gateway-ssl-portal.md)
> -[Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> -[Azure Klasik PowerShell](application-gateway-ssl.md)
> 
> 

 Azure Application Gateway, web grubunda maliyetli SSL şifre çözme görevlerinin oluşmasından kaçınmak için Güvenli Yuva Katmanı (SSL) oturumunu sonlandırmak amacıyla yapılandırılabilir. SSL yük boşaltımı ön uç sunucusunun kurulumunu ve web uygulamasının yönetimini de basitleştirir.

## Başlamadan önce
1. Web Platformu Yükleyicisi’ni kullanarak Azure PowerShell cmdlet’lerin en son sürümünü yükleyin. **İndirmeler sayfası**’ndaki [Windows PowerShell](https://azure.microsoft.com/downloads/) bölümünden en son sürümü indirip yükleyebilirsiniz.
2. Uygulama ağ geçidi için bir sanal ağ ve bir alt ağ oluşturacaksınız. Hiçbir sanal makinenin veya bulut dağıtımlarının alt ağı kullanmadığından emin olun. Application Gateway tek başına bir sanal ağ alt ağında olmalıdır.
3. Uygulama ağ geçidi kullanırken yapılandırdığınız sunucular mevcut olmalıdır veya uç noktaları sanal ağda veya atanan genel bir IP/VIP’de oluşturulmuş olmalıdır.

## Bir uygulama ağ geçidi oluşturmak için ne gereklidir?
* **Arka uç sunucusu havuzu:** Arka uç sunucularının IP adreslerinin listesi. Listede bulunan IP adresleri sanal ağ alt ağına veya genel IP/VIP’ye ait olmalıdır.
* **Arka uç sunucu havuzu ayarları**: Her havuzun bağlantı noktası, protokol ve tanımlama bilgisi temelli benzeşim gibi ayarları vardır. Bu ayarlar bir havuza bağlıdır ve havuzdaki tüm sunuculara uygulanır.
* **Ön uç bağlantı noktası:** Bu bağlantı noktası uygulama ağ geçidinde açılan genel bağlantı noktasıdır. Bu bağlantı noktasında trafik olursa arka uç sunuculardan birine yönlendirilir.
* **Dinleyici:** Dinleyicide bir ön uç bağlantı noktası, bir protokol (Http veya Https, bu ayarlar büyük/küçük harfe duyarlıdır) ve SSL sertifika adı (SSL yük boşaltımı yapılandırılıyorsa) vardır.
* **Kural:** Kural, dinleyiciyi ve arka uç sunucusu havuzunu bağlar ve belli bir dinleyicide trafik olduğunda trafiğin hangi arka uç sunucu havuzuna yönlendirileceğini belirler. Şu anda yalnızca *temel* kural desteklenmektedir. *Temel* kural hepsini bir kez deneme yöntemiyle yük dağıtımıdır.

**Ek yapılandırma notları**

SSL sertifikaları yapılandırmada **HttpListener**’daki protokol *Https* (küçük/büyük harf duyarlı) ile değiştirilmelidir. **SslCertificate** öğesi SSL sertifikası için yapılandırılmış değişken değerle **HttpListener**’a eklenir. Ön uç bağlantı noktası 443’e yükseltilmelidir.

**Tanımlama bilgisi temelli benzeşimi etkinleştirme:** Bir uygulama ağ geçidi, bir istemci oturumundan gelen isteğin web grubunda hep aynı VM’e yönlendirildiğinden emin olmak için yapılandırılabilir. Bu senaryo, ağ geçidinin trafiği uygun bir şekilde yönlendirmesini sağlayacak oturum tanımlama bilgisinin eklenmesiyle gerçekleştirilir. Tanımlama bilgisi temelli benzeşimi etkinleştirmek için, **CookieBasedAffinity**’yi *BackendHttpSetting* öğesindeki **Enabled**’a ayarlayın.

## Uygulama ağ geçidi oluşturma
Azure Klasik dağıtım modeli ve Azure Resource Manager arasındaki fark, uygulama ağ geçidi oluştururken takip ettiğiniz sıra ve yapılandırılması gereken öğelerdir.

Resource Manager'da uygulama ağ geçidini oluşturan tüm öğeler ayrı ayrı yapılandırılır ve ardından bir uygulama ağ geçidi kaynağı oluşturmak üzere bir araya getirilir.

Bir uygulama ağ geçidi oluşturmak için takip etmeniz gereken adımlar şunlardır:

1. Resource Manager için kaynak grubu oluşturun
2. Uygulama ağ geçidi için sanal ağ, alt ağ ve genel IP oluşturun
3. Uygulama ağ geçidi yapılandırma nesnesi oluşturun
4. Bir uygulama ağ geçidi kaynağı oluşturma

## Resource Manager için kaynak grubu oluşturun
Azure Resource Manager cmdlet’lerini kullanmak için PowerShell modunu açtığınızdan emin olun. Daha fazla bilgi için bkz.[Resource Manager ile Windows PowerShell Kullanma](../powershell-azure-resource-manager.md)

### 1. Adım
    Login-AzureRmAccount

### 2. Adım
Hesapla ilişkili abonelikleri kontrol edin.

    Get-AzureRmSubscription

Kimlik bilgilerinizle kimliğinizi doğrulamanız istenir.<BR>

### 3. Adım
Hangi Azure aboneliğinizin kullanılacağını seçin. <BR>

    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### 4. Adım
Bir kaynak grubu oluşturun (mevcut bir kaynak grubu kullanıyorsanız bu adımı atlayın).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Resource Manager, tüm kaynak gruplarının bir konum belirtmesini gerektirir. Bu ayar, kaynak grubundaki kaynaklar için varsayılan konum olarak kullanılır. Uygulama ağ geçidi oluşturmak için verilen komutların aynı kaynak grubunu kullandığından emin olun.

Yukarıdaki örnekte, "appgw-RG" adlı "Batı ABD" konumlu bir kaynak grubu oluşturduk.

## Uygulama ağ geçidi için bir sanal ağ ve bir alt ağ oluştur
Aşağıdaki örnek Resource Manager kullanarak nasıl sanal ağ oluşturulacağını gösterir:

### 1. Adım
    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

Bu örnek, 10.0.0.0/24 adres aralığını, sanal ağ oluşturmak için kullanılacak bir alt ağ değişkenine atar.

### 2. Adım
    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

Bu örnek, Batı ABD bölgesi için 10.0.0.0/24 alt ağıyla 10.0.0.0/16 ön ekini kullanarak "appgw-rg" kaynak grubunda "appgwvnet" adlı bir sanal ağ oluşturur.

### 3. Adım
    $subnet = $vnet.Subnets[0]

Bu örnek, sonraki adımlar için alt ağ nesnesini bir $subnet değişkenine atar.

## Ön uç yapılandırma için genel bir IP adresi oluşturun
    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic

Bu örnek, Batı ABD bölgesi için "appgw-rg" kaynak grubunda "publicIP01" adlı bir genel IP kaynağı oluşturur.

## Uygulama ağ geçidi yapılandırma nesnesi oluşturun
### 1. Adım
    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

Bu örnek, "gatewayIP01" adlı bir uygulama ağ geçidi IP yapılandırması oluşturur. Application Gateway başladığında, yapılandırılan alt ağdan bir IP adresi alır ve ağ trafiğini arka uç IP havuzundaki IP adreslerine yönlendirir. Her örneğin bir IP adresi aldığını göz önünde bulundurun.

### 2. Adım
    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

Bu örnek, "pool01" adlı arka uç IP adresi havuzunu "134.170.185.46, 134.170.188.221,134.170.185.50" IP adresleriyle yapılandırır. Bu değerler, ön uç IP uç noktasından gelen ağ trafiğinin yönlendirildiği IP adresleridir. Önceki örnekte yazılan IP adreslerini, kendi web uygulaması uç noktalarınızla değiştirin.

### 3. Adım
    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled

Bu örnek, arka uç havuzundaki yük dengeli ağ trafiği için "poolsetting01" uygulama ağ geçidi ayarını yapılandırır.

### 4. Adım
    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443

Bu örnek, genel IP uç noktası için "frontendport01" adlı ön uç IP bağlantı noktasını yapılandırır.

### 5. Adım
    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password ‘<password>’

Bu örnek, SSL bağlantısı için kullanılan sertifikayı yapılandırır. Sertifikanın .pfx formatında olması gerekir ve parola 4 ile 12 karakter arasında olmalıdır.

### 6. Adım
    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

Bu örnek, "fipconfig01" adlı ön uç IP yapılandırmasını oluşturur ve genel IP adresiyle ön uç IP yapılandırmasını ilişkilendirir.

### 7. Adım
    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert


Bu örnek, "listener01" adlı dinleyiciyi oluşturur ve ön uç bağlantı noktasıyla ön uç IP yapılandırmasını ve sertifikasını ilişkilendirir.

### 8. Adım
    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

Bu örnek, yük dengeleyici davranışını yapılandıran "rule01" adlı yük dengeleyiciyi yönlendirme kuralını oluşturur.

### 9. Adım
    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

Bu örnek, uygulama ağ geçidinin örnek boyutunu yapılandırır.

> [!NOTE]
> *InstanceCount* için varsayılan değer 2 ile 10 arasıdır. *GatewaySize* için varsayılan değer Medium’dur. Aynı zamanda Standard_Small, Standard_Medium ve Standard_Large seçenekleri de bulunmaktadır.
> 
> 

## New-AzureApplicationGateway kullanarak bir uygulama ağ geçidi oluşturma
    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert

Bu örnek, önceki adımlarda geçen tüm yapılandırma öğeleri ile bir uygulama ağ geçidi oluşturur. Örnekte uygulama ağ geçidi "appgwtest" olarak adlandırılmıştır.

## Sonraki adımlar
Bir uygulama ağ geçidini bir iç yük dengeleyici (ILB) ile kullanmak için yapılandırmak istiyorsanız, bkz. [İç yük dengeleyiciyle bir uygulama ağ geçidi oluşturma](application-gateway-ilb.md).

Yük dengeleme seçenekleri hakkında daha fazla genel bilgi edinmek istiyorsanız, bkz.

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

<!--HONumber=Sep16_HO3-->


