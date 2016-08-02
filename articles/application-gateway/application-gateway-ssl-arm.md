<properties
   pageTitle="Azure Resource Manager kullanarak SSL yük boşaltımı için bir uygulama ağ geçidi oluşturma | Microsoft Azure"
   description="Bu sayfada Azure Resource Manager kullanarak SSL yük boşaltımıyla uygulama ağ geçidi oluşturmak için yönergeler bulunmaktadır."
   documentationCenter="na"
   services="application-gateway"
   authors="joaoma"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/03/2016"
   ms.author="joaoma"/>

# Azure Resource Manager kullanarak SSL yük boşaltımı için bir uygulama ağ geçidi oluşturma

> [AZURE.SELECTOR]
-[Azure Klasik PowerShell](application-gateway-ssl.md)
-[Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)

 Azure Application Gateway, web grubunda maliyetli SSL şifre çözme görevlerinin oluşmasından kaçınmak için Güvenli Yuva Katmanı (SSL) oturumunu sonlandırmak amacıyla yapılandırılabilir. SSL yük boşaltımı ön uç sunucusunun kurulumunu ve web uygulamasının yönetimini de basitleştirir.


## Başlamadan önce

1. Web Platformu Yükleyicisi’ni kullanarak Azure PowerShell cmdlets’in en son sürümünü yükleyin. **İndirmeler sayfası**’ndaki [Windows PowerShell](https://azure.microsoft.com/downloads/) bölümünden en son sürümü indirip yükleyebilirsiniz.
2. Sanal ağ ve uygulama ağ geçidi için bir alt ağ oluşturabileceksiniz. Ağ geçidi hiçbir sanal makinenin veya bulut dağıtımının kullanmadığından emin olun. Application Gateway tek başına bir sanal ağ alt ağında olmalıdır.
3. Uygulama ağ geçidi kullanırken yapılandıracağınız sunucular mevcut olmalıdır veya uç noktaları sanal ağda veya atanan genel bir IP/VIP’de oluşturulmuş olmalıdır.

## Bir uygulama ağ geçidi oluşturmak için ne gereklidir?


- **Arka uç sunucusu havuzu:** Arka uç sunucularının IP adreslerinin listesi. Listede bulunan IP adresleri sanal ağ alt ağına veya genel IP/VIP’ye ait olmalıdır.
- **Arka uç sunucu havuzu ayarları**: Her havuzun bağlantı noktası, protokol ve tanımlama bilgisi temelli benzeşim gibi ayarları vardır. Bu ayarlar bir havuza bağlıdır ve havuzdaki tüm sunuculara uygulanır.
- **Ön uç bağlantı noktası:** Bu bağlantı noktası uygulama ağ geçidinde açılan genel bağlantı noktasıdır. Bu bağlantı noktasında trafik olursa arka uç sunuculardan birine yönlendirilir.
- **Dinleyici:** Dinleyicide bir ön uç bağlantı noktası, bir protokol (büyük/küçük harfe duyarlı Http veya Https) ve SSL sertifika adı (SSL yük boşaltımı yapılandırılıyorsa) vardır.
- **Kural:** Kural dinleyiciyi ve arka uç sunucusu havuzunu bağlar ve belli bir dinleyicide trafik olduğunda trafiğin hangi arka uç sunucu havuzuna yönlendirileceğini belirler. Şu anda yalnızca *temel* kural desteklenmektedir. *Temel* kural hepsini bir kez deneme yöntemiyle yük dağıtımıdır.

**Ek yapılandırma notları**

SSL sertifikaları yapılandırmada **HttpListener**’daki protokol *Https* (küçük/büyük harf duyarlı) ile değiştirilmelidir. **SslCertificate** öğesi SSL sertifikası için yapılandırılmış değişken değerle **HttpListener**’a eklenmelidir. Ön uç bağlantı noktası 443’e yükseltilmelidir.

**Tanımlama bilgisi temelli benzeşimi etkinleştirme:** Bir uygulama ağ geçidi, bir istemci oturumundan gelen isteğin web grubunda hep aynı VM’e yönlendirildiğinden emin olmak için yapılandırılabilir. Bu, ağ geçidinin trafiği uygun bir şekilde yönlendirmesini sağlayacak oturum tanımlama bilgisinin eklenmesiyle gerçekleştirilir. Tanımlama bilgisi temelli benzeşimi etkinleştirmek için, **CookieBasedAffinity**’yi *BackendHttpSetting* öğesindeki **Enabled**’a ayarlayın.


## Yeni bir uygulama ağ geçidi oluşturun

Azure Klasik dağıtım modeli ve Azure Resource Manager arasındaki fark uygulama ağ geçidi oluştururken takip ettiğiniz sıra ve yapılandırılması gereken öğelerdir.

Resource Manager’da bir uygulama ağ geçidini oluşturan öğeler teker teker yapılandırılır ve sonra uygulama ağ geçidi kaynağı oluşturmak için bir araya getirilir.


Bir uygulama ağ geçidi oluşturmak için takip etmeniz gereken adımlar şunlardır:

1. Resource Manager için kaynak grubu oluşturun
2. Uygulama ağ geçidi için sanal ağ, alt ağ ve genel IP oluşturun
3. Bir uygulama ağ geçidi yapılandırma nesnesi oluşturun
4. Bir uygulama ağ geçidi kaynağı oluşturun


## Resource Manager için kaynak grubu oluşturun

Azure Resource Manager cmdlets’i kullanmak için PowerShell modunu açtığınızdan emin olun. Daha fazla bilgi için bkz.[Resource Manager ile Windows Powershell Kullanma](../powershell-azure-resource-manager.md)

### 1. Adım

        PS C:\> Login-AzureRmAccount



### 2. Adım

Hesapla ilişkili abonelikleri kontrol edin.

        PS C:\> get-AzureRmSubscription

Kimlik bilgilerinizle kimliğinizi doğrulamanız istenir.<BR>

### 3. Adım

Hangi Azure aboneliğinizin kullanılacağını seçin. <BR>


        PS C:\> Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### 4. Adım

Yeni bir kaynak grubu oluştur (mevcut bir kaynak grubu kullanıyorsanız bu adımı atlayın).

    New-AzureRmResourceGroup -Name appgw-rg -location "West US"

Azure Resource Manager, tüm kaynak gruplarının bir konum belirtmesini gerektirir. Bu, kaynak grubundaki kaynaklar için varsayılan konum olarak kullanılır. Bir uygulama ağ geçidi oluşturmak için verilecek komutların aynı kaynak grubunu kullandığından emin olun.

Yukarıdaki örnekte, "appgw-RG" adlı "Batı ABD" konumlu bir kaynak grubu oluşturduk.

## Uygulama ağ geçidi için bir sanal ağ ve bir alt ağ oluştur

Aşağıdaki örnek Resource Manager kullanarak nasıl sanal ağ oluşturulacağını gösterir:

### 1. Adım

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

Bu, 10.0.0.0/24 adres aralığını, bir sanal ağ oluşturmak için kullanılacak bir alt ağ değişkenine atar.

### 2. Adım
    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

Bu, Batı ABD bölgesi için 10.0.0.0/24 alt ağıyla 10.0.0.0/16 ön ekini kullanarak "appgw-rg" kaynak grubunda "appgwvnet" adlı bir sanal ağ oluşturur.

### 3. Adım

    $subnet=$vnet.Subnets[0]

Bu, sonraki adımlarda alt ağ nesnesini bir değişken alt ağına atar.

## Ön uç yapılandırma için genel bir IP adresi oluşturun

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic

Bu, Batı ABD bölgesi için "appgw-rg" kaynak grubunda "publicIP01" genel IP kaynağı oluşturur.


## Bir uygulama ağ geçidi yapılandırma nesnesi oluşturun

### 1. Adım

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

Bu, "gatewayIP01" adlı uygulama ağ geçidi IP yapılandırması oluşturur. Application Gateway başladığında, yapılandırılan alt ağdan bir IP adresi alır ve ağ trafiğini arka uç IP havuzundaki IP adreslerine yönlendirir. Her örneğin bir IP adresi alacağını göz önünde bulundurun.

### 2. Adım

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

Bu, "pool01" adlı arka uç IP adresi havuzunu "134.170.185.46, 134.170.188.221,134.170.185.50." IP adresleriyle yapılandırır. Bu adresler ön uç IP uç noktasından gelen ağ trafiğinin yönlendirildiği IP adresleridir. Yukarıdaki örnekte yazılan IP adreslerini kendi web uygulama uç noktalarıyla değiştirin.

### 3. Adım

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled

Bu, "poolsetting01" uygulama ağ geçidi ayarlarını arka uç havuzundaki yük dengeli ağ trafiğine yapılandırır.

### 4. Adım

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443

Bu, "frontendport01" adlı ön uç IP bağlantı noktasını genel IP uç noktasına yapılandırır.

### 5. Adım

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password ‘<password>’

Bu, SSL bağlantısı için kullanılan sertifikayı yapılandırır. Sertifikanın .pfx formatında olması gerekir ve parola 4 ile 12 karakter arasında olmalıdır.

### 6. Adım

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

Bu, "fipconfig01" adlı ön uç IP yapılandırmasını oluşturur ve genel IP adresiyle ön uç IP yapılandırmasını ilişkilendirir.

### 7. Adım

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert


Bu, "listener01" adlı dinleyiciyi oluşturur ve ön uç bağlantı noktasıyla ön uç IP yapılandırmasını ve sertifikasını ilişkilendirir.

### 8. Adım

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

Bu, yük dengeleyici davranışını yapılandıran "rule01" adlı yük dengeleyiciyi yönlendirme kuralını oluşturur.

### 9. Adım

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

Bu, uygulama ağ geçidinin örnek boyutunu yapılandırır.

>[AZURE.NOTE]  *InstanceCount* için varsayılan değer 2 ile 10 arasıdır. *GatewaySize* için varsayılan değer Ortadır. Aynı zamanda Standard_Small, Standard_Medium ve Standard_Large seçenekleri de bulunmaktadır.

## New-AzureApplicationGateway kullanarak bir uygulama ağ geçidi oluşturma

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert

Bu, yukarıdaki adımlarda geçen tüm yapılandırma öğelerinden bir uygulama ağ geçidi oluşturur. Örnekte uygulama ağ geçidi "appgwtest" olarak adlandırılmıştır.

## Sonraki adımlar

Bir uygulama ağ geçidini bir iç yük dengeleyici (ILB) ile kullanmak için yapılandırmak istiyorsanız, bkz. [İç yük dengeleyiciyle bir uygulama ağ geçidi oluşturma](application-gateway-ilb.md).

Yük dengeleme seçenekleri hakkında daha fazla genel bilgi edinmek istiyorsanız, bkz.

- [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)



<!---HONumber=Jun16_HO2-->


