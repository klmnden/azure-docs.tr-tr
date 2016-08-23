<properties
   pageTitle="Azure Resource Manager kullanarak iç yük dengeleyiciye (ILB) sahip bir uygulama ağ geçidi oluşturma ve yapılandırma | Microsoft Azure"
   description="Bu sayfa, Azure Resource Manager için iç yük dengeleyiciye (ILB) sahip bir Azure uygulama ağ geçidi oluşturma, yapılandırma, başlatma ve silme yönergelerini sağlar"
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
   ms.date="04/05/2016"
   ms.author="joaoma"/>


# Azure Resource Manager kullanarak iç yük dengeleyiciye (ILB) sahip bir uygulama ağ geçidi oluşturma

> [AZURE.SELECTOR]
- [Azure klasik adımları](application-gateway-ilb.md)
- [Resource Manager PowerShell adımları](application-gateway-ilb-arm.md)

Azure Application Gateway İnternet’e yönelik bir VIP veya İnternet’e sunulmamış iç yük dengeleyici uç noktası olarak da bilinen iç uç nokta ile yapılandırılabilir. Ağ geçidini bir ILB ile yapılandırma İnternet’e sunulmamış iç iş kolu uygulamaları için kullanışlıdır. Güvenlik sınırı içinde bulunan, İnternet’e sunulmamış ancak hala hepsini bir kez deneme yük dağıtımı, oturum sürekliliği veya Güvenli Yuva Katmanı (SLL) sonlandırması gerektiren çok katmanlı uygulamalar içindeki hizmetler ve katmanlar için de kullanışlıdır.

Bu makale, ILB ile uygulama ağ geçidi yapılandırma adımlarında size yol gösterir.

## Başlamadan önce

1. Web Platformu Yükleyicisi’ni kullanarak Azure PowerShell cmdlet’lerin en son sürümünü yükleyin. **İndirmeler sayfası**’ndaki [Windows PowerShell](https://azure.microsoft.com/downloads/) bölümünden en son sürümü indirip yükleyebilirsiniz.
2. Application Gateway için bir sanal ağ ve bir alt ağ oluşturabileceksiniz. Hiçbir sanal makinenin veya bulut dağıtımlarının alt ağı kullanmadığından emin olun. Application Gateway tek başına bir sanal ağ alt ağında olmalıdır.
3. Uygulama ağ geçidi kullanırken yapılandıracağınız sunucular mevcut olmalıdır veya uç noktaları sanal ağda veya atanan genel bir IP/VIP’de oluşturulmuş olmalıdır.

## Bir uygulama ağ geçidi oluşturmak için ne gereklidir?


- **Arka uç sunucusu havuzu:** Arka uç sunucularının IP adreslerinin listesi. Listede bulunan IP adresleri, uygulama ağ geçidi için farklı alt ağa sahip sanal ağ alt ağına veya genel IP/VIP’ye ait olmalıdır.
- **Arka uç sunucu havuzu ayarları**: Her havuzun bağlantı noktası, protokol ve tanımlama bilgisi temelli benzeşim gibi ayarları vardır. Bu ayarlar bir havuza bağlıdır ve havuzdaki tüm sunuculara uygulanır.
- **Ön uç bağlantı noktası:** Bu bağlantı noktası uygulama ağ geçidinde açılan genel bağlantı noktasıdır. Bu bağlantı noktasında trafik olursa arka uç sunuculardan birine yönlendirilir.
- **Dinleyici:** Dinleyicide bir ön uç bağlantı noktası, bir protokol (büyük/küçük harfe duyarlı Http veya Https) ve SSL sertifika adı (SSL yük boşaltımı yapılandırılıyorsa) vardır.
- **Kural:** Kural, dinleyiciyi ve arka uç sunucusu havuzunu bağlar ve belli bir dinleyicide trafik olduğunda trafiğin hangi arka uç sunucu havuzuna yönlendirileceğini belirler. Şu anda yalnızca *temel* kural desteklenmektedir. *Temel* kural hepsini bir kez deneme yöntemiyle yük dağıtımıdır.



## Yeni bir uygulama ağ geçidi oluşturun

Azure Klasik ve Azure Resource Manager’ın kullanımı arasındaki fark uygulama ağ geçidi oluştururken takip ettiğiniz sıra ve yapılandırılması gereken öğelerdir.
Resource Manager’da uygulama ağ geçidini oluşturan öğeler ayrı ayrı yapılandırılır ve sonra uygulama ağ geçidi kaynağı oluşturmak için bir araya getirilir.


Uygulama ağ geçidi oluşturmak için takip etmeniz gereken adımlar şunlardır:

1. Resource Manager için kaynak grubu oluşturun
2. Uygulama ağ geçidi için bir sanal ağ ve bir alt ağ oluşturun
3. Uygulama ağ geçidi yapılandırma nesnesi oluşturun
4. Bir uygulama ağ geçidi kaynağı oluşturma


## Resource Manager için kaynak grubu oluşturun

Azure Resource Manager cmdlet’lerini kullanmak için PowerShell modunu açtığınızdan emin olun. Daha fazla bilgi için bkz.[Resource Manager ile Windows PowerShell Kullanma](../powershell-azure-resource-manager.md)

### 1. Adım

        Login-AzureRmAccount

### 2. Adım

Hesapla ilişkili abonelikleri kontrol edin.

        get-AzureRmSubscription

Kimlik bilgilerinizle kimliğinizi doğrulamanız istenir.<BR>

### 3. Adım

Hangi Azure aboneliğinizin kullanılacağını seçin. <BR>


        Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### 4. Adım

Yeni bir kaynak grubu oluşturun (mevcut bir kaynak grubu kullanıyorsanız bu adımı atlayın).

    New-AzureRmResourceGroup -Name appgw-rg -location "West US"

Azure Resource Manager, tüm kaynak gruplarının bir konum belirtmesini gerektirir. Bu, kaynak grubundaki kaynaklar için varsayılan konum olarak kullanılır. Uygulama ağ geçidi oluşturmak için verilecek komutların aynı kaynak grubunu kullandığından emin olun.

Yukarıdaki örnekte, "appgw-RG" adlı "Batı ABD" konumlu bir kaynak grubu oluşturduk.

## Uygulama ağ geçidi için bir sanal ağ ve bir alt ağ oluşturun

Aşağıdaki örnek Resource Manager kullanarak nasıl sanal ağ oluşturulacağını gösterir:

### 1. Adım

    $subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

Bu, 10.0.0.0/24 adres aralığını, bir sanal ağ oluşturmak için kullanılacak bir alt ağ değişkenine atar.

### 2. Adım

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig

Bu, Batı ABD bölgesi için 10.0.0.0/24 alt ağıyla 10.0.0.0/16 ön ekini kullanarak "appgw-rg" kaynak grubunda "appgwvnet" adlı bir sanal ağ oluşturur.

### 3. Adım

    $subnet=$vnet.subnets[0]

Bu, sonraki adımlarda alt ağ nesnesini bir $subnet değişkenine atar.

## Uygulama ağ geçidi yapılandırma nesnesi oluşturun

### 1. Adım

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

Bu, "gatewayIP01" adlı uygulama ağ geçidi IP yapılandırması oluşturur. Application Gateway başladığında, yapılandırılan alt ağdan bir IP adresi alır ve ağ trafiğini arka uç IP havuzundaki IP adreslerine yönlendirir. Her örneğin bir IP adresi alacağını göz önünde bulundurun.


### 2. Adım

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

Bu, "pool01" adlı arka uç IP adresi havuzunu "134.170.185.46, 134.170.188.221,134.170.185.50." IP adresleriyle yapılandırır. Bu adresler ön uç IP uç noktasından gelen ağ trafiğinin yönlendirildiği IP adresleridir. Kendi uygulamanızın IP adresi uç noktalarını eklemek için Yukarıdaki IP adreslerini değiştireceksiniz.

### 3. Adım

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

Bu, "poolsetting01" uygulama ağ geçidi ayarını arka uç havuzundaki yük dengeli ağ trafiği için yapılandırır.

### 4. Adım

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

Bu, "frontendport01" adlı ön uç IP bağlantı noktasını ILB için yapılandırır.

### 5. Adım

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet

Bu, "fipconfig01" adlı ön uç IP yapılandırmasını oluşturur ve geçerli sanal ağ alt ağından özel IP ile ilişkilendirir.

### 6. Adım

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

Bu, "listener01" adlı dinleyiciyi oluşturur ve ön uç bağlantı noktasıyla ön uç IP yapılandırmasını ilişkilendirir.

### 7. Adım

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

Bu, yük dengeleyici davranışını yapılandıran "rule01" adlı yük dengeleyiciyi yönlendirme kuralını oluşturur.

### 8. Adım

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

Bu, uygulama ağ geçidinin örnek boyutunu yapılandırır.

>[AZURE.NOTE]  *InstanceCount* için varsayılan değer 2 ile 10 arasıdır. *GatewaySize* için varsayılan değer Medium’dur. Aynı zamanda Standard_Small, Standard_Medium ve Standard_Large seçenekleri de bulunmaktadır.

## New-AzureApplicationGateway kullanarak bir uygulama ağ geçidi oluşturma

Yukarıdaki adımlarda geçen tüm yapılandırma öğelerinden bir uygulama ağ geçidi oluşturur. Bu örnekte uygulama ağ geçidi "appgwtest" olarak adlandırılmıştır.


    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

Bu, yukarıdaki adımlarda geçen tüm yapılandırma öğelerinden bir uygulama ağ geçidi oluşturur. Örnekte uygulama ağ geçidi "appgwtest" olarak adlandırılmıştır.


## Uygulama ağ geçidini silme

Bir uygulama ağ geçidini silmek için sırayla aşağıdakileri yapmanız gerekir:

1. Ağ geçidini durdurmak için **Stop-AzureRmApplicationGateway** cmdlet’ini kullanın.
2. Ağ geçidini kaldırmak için **Remove-AzureRmApplicationGateway** cmdlet’ini kullanın.
3. Ağ geçidinin **Get-AzureApplicationGateway** cmdlet’i kullanılarak kaldırıldığını doğrulayın.


### 1. Adım

Uygulama ağ geçidi nesnesini alın ve "$getgw" değişkenine ilişkilendirin.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### 2. Adım

Uygulama ağ geçidini durdurmak için **Stop-AzureRmApplicationGateway**’i kullanın. Bu örnek, çıktı tarafından takip edilen ilk satırdaki **Stop-AzureRmApplicationGateway** cmdlet’ini gösterir.

    PS C:\> Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Uygulama ağ geçidi durdurulmuş durumda olduğunda hizmeti kaldırmak için **Remove-AzureRmApplicationGateway** cmdlet’ini kullanın.


    PS C:\> Remove-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Force

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

>[AZURE.NOTE]  **-force** anahtarı, kaldırma onayı iletisini gizlemek için kullanılabilir.


Kaldırılan hizmeti doğrulamak için **Get-AzureRmApplicationGateway** cmdlet’ini kullanabilirsiniz. Bu adım gerekli değildir.


    PS C:\>Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## Sonraki adımlar

SSL yük boşaltmayı yapılandırmak istiyorsanız, bkz. [SSL yük boşaltımı için uygulama ağ geçidi yapılandırma](application-gateway-ssl.md).

Bir uygulama ağ geçidini bir ILB ile kullanmak için yapılandırmak istiyorsanız, bkz. [İç yük dengeleyiciyle bir uygulama ağ geçidi oluşturma](application-gateway-ilb.md).

Yük dengeleme seçenekleri hakkında daha fazla genel bilgi edinmek istiyorsanız, bkz.

- [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)



<!--HONumber=Aug16_HO1-->


