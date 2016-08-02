<properties
   pageTitle="Azure Resource Manager kullanarak uygulama ağ geçidi oluşturma, başlatma veya silme | Microsoft Azure"
   description="Bu sayfa, Azure Resource Manager’ı kullanarak Azure uygulama ağ geçidi oluşturma, yapılandırma, başlatma ve silme yönergelerini sağlar"
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


# Azure Resource Manager kullanarak bir uygulama ağ geçidi oluşturma, başlatma veya silme

Azure Application Gateway, bir katman 7 yük dengeleyicidir. Bulutta veya şirket içinde olmalarından bağımsız olarak, farklı sunucular arasında yük devretme ile HTTP istekleri için performans amaçlı yönlendirme sağlar. Application Gateway şu uygulama teslim özelliklerine sahiptir: HTTP yük dengeleme, tanımlama bilgisi tabanlı oturum benzeşimi ve Güvenli Yuva Katmanı (SSL) yük boşaltma.


> [AZURE.SELECTOR]
- [Azure Klasik PowerShell adımları](application-gateway-create-gateway.md)
- [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
- [Azure Resource Manager şablonu ](application-gateway-create-gateway-arm-template.md)


<BR>


Bu makale, uygulama ağ geçidi oluşturma, yapılandırma, başlatma ve silme adımlarında size eşlik eder.


>[AZURE.IMPORTANT] Azure kaynaklarıyla çalışmadan önce Azure’da şu anda iki dağıtım modeli olduğunu anlamak önemlidir: Resource Manager ve klasik. Azure kaynaklarıyla çalışmadan önce [dağıtım modellerini ve araçlarını](../azure-classic-rm.md) iyice anladığınızdan emin olun. Bu makalenin en üstündeki sekmelere tıklayarak farklı araçlarla ilgili belgeleri görüntüleyebilirsiniz. Bu belge, Azure Resource Manager’ı kullanarak uygulama ağ geçidi oluşturmayı kapsar. Klasik sürümü kullanmak için [PowerShell’i kullanarak uygulama ağ geçidi klasik dağıtımı oluşturma](application-gateway-create-gateway.md) bağlantısına gidin.



## Başlamadan önce

1. Web Platformu Yükleyicisi’ni kullanarak Azure PowerShell cmdlet’lerin en son sürümünü yükleyin. **İndirmeler sayfası**’ndaki [Windows PowerShell](https://azure.microsoft.com/downloads/) bölümünden en son sürümü indirip yükleyebilirsiniz.
2. Application Gateway için bir sanal ağ ve alt ağ oluşturabileceksiniz. Hiçbir sanal makinenin veya bulut dağıtımlarının alt ağı kullanmadığından emin olun. Uygulama ağ geçidi tek başına bir sanal ağ alt ağında olmalıdır.
3. Uygulama ağ geçidi kullanırken yapılandıracağınız sunucular mevcut olmalıdır veya uç noktaları sanal ağda veya atanan genel bir IP/VIP’de oluşturulmuş olmalıdır.

## Bir uygulama ağ geçidi oluşturmak için ne gereklidir?


- **Arka uç sunucusu havuzu:** Arka uç sunucularının IP adreslerinin listesi. Listede bulunan IP adresleri sanal ağ alt ağına veya genel IP/VIP’ye ait olmalıdır.
- **Arka uç sunucu havuzu ayarları**: Her havuzun bağlantı noktası, protokol ve tanımlama bilgisi temelli benzeşim gibi ayarları vardır. Bu ayarlar bir havuza bağlıdır ve havuzdaki tüm sunuculara uygulanır.
- **Ön uç bağlantı noktası:** Bu bağlantı noktası uygulama ağ geçidinde açılan genel bağlantı noktasıdır. Bu bağlantı noktasında trafik olursa arka uç sunuculardan birine yönlendirilir.
- **Dinleyici:** Dinleyicide bir ön uç bağlantı noktası, bir protokol (büyük/küçük harfe duyarlı Http veya Https) ve SSL sertifika adı (SSL yük boşaltımı yapılandırılıyorsa) vardır.
- **Kural:** Kural dinleyiciyi arka uç sunucusu havuzunu bağlar ve belli bir dinleyicide trafik olduğunda trafiğin hangi arka uç sunucu havuzuna yönlendirileceğini belirler. 



## Yeni bir uygulama ağ geçidi oluşturun

Azure Klasik ve Azure Resource Manager’ın kullanımı arasındaki fark uygulama ağ geçidi oluştururken takip ettiğiniz sıra ve yapılandırılması gereken öğelerdir.

Resource Manager’da uygulama ağ geçidini oluşturan öğeler ayrı ayrı yapılandırılır ve sonra uygulama ağ geçidi kaynağı oluşturmak için bir araya getirilir.


Uygulama ağ geçidi oluşturmak için takip etmeniz gereken adımlar şunlardır:

1. Resource Manager için kaynak grubu oluşturun.
2. Uygulama ağ geçidi için sanal ağ, alt ağ ve genel IP oluşturun.
3. Uygulama ağ geçidi yapılandırma nesnesi oluşturun.
4. Uygulama ağ geçidi kaynağı oluşturun.


## Resource Manager için kaynak grubu oluşturun

Azure PowerShell’in en yeni sürümünü kullandığınızdan emin olun. Daha fazla bilgi için bkz.[Resource Manager ile Windows PowerShell Kullanma](../powershell-azure-resource-manager.md)

### 1. Adım
Azure’da oturum açın Login-AzureRmAccount

Kimlik bilgilerinizle kimliğinizi doğrulamanız istenir.<BR>
### 2. Adım
Hesapla ilişkili abonelikleri kontrol edin.

        Get-AzureRmSubscription

### 3. Adım
Hangi Azure aboneliğinizin kullanılacağını seçin. <BR>

        Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### 4. Adım
Yeni bir kaynak grubu oluşturun (mevcut bir kaynak grubu kullanıyorsanız bu adımı atlayın).

    New-AzureRmResourceGroup -Name appgw-rg -location "West US"

Azure Resource Manager, tüm kaynak gruplarının bir konum belirtmesini gerektirir. Bu, kaynak grubundaki kaynaklar için varsayılan konum olarak kullanılır. Uygulama ağ geçidi oluşturmak için verilecek komutların aynı kaynak grubunu kullandığından emin olun.

Yukarıdaki örnekte, "appgw-RG" adlı "Batı ABD" konumlu bir kaynak grubu oluşturduk.

>[AZURE.NOTE] Uygulama ağ geçidiniz için özel bir araştırma yapılandırmanız gerekiyorsa, bkz. [PowerShell kullanarak özel araştırmalara sahip bir uygulama ağ geçidi oluşturma](application-gateway-create-probe-ps.md). Daha fazla bilgi için [özel araştırmalar ve sistem durumu izleme](application-gateway-probe-overview.md) konusunu inceleyin.



## Uygulama ağ geçidi için bir sanal ağ ve bir alt ağ oluşturun

Aşağıdaki örnek Resource Manager kullanarak nasıl sanal ağ oluşturulacağını gösterir.

### 1. Adım

10.0.0.0/24 adres aralığını, sanal ağ oluşturmak için kullanılacak bir alt ağ değişkenine atayın.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24


### 2. Adım

Batı ABD bölgesi için 10.0.0.0/24 alt ağıyla 10.0.0.0/16 ön ekini kullanarak "appgw-rg" kaynak grubunda "appgwvnet" adlı bir sanal ağ oluşturun.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet


### 3. Adım

Uygulama ağ geçidi oluşturan sonraki adımlar için bir alt ağ değişkeni atayın.

    $subnet=$vnet.Subnets[0]

## Ön uç yapılandırma için genel bir IP adresi oluşturun

Batı ABD bölgesi için "appgw-rg" kaynak grubunda "publicIP01" genel bir IP kaynağı oluşturun.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic


## Uygulama ağ geçidi yapılandırma nesnesi oluşturun

Uygulama ağ geçidini oluşturmadan önce tüm yapılandırma öğelerini ayarlamanız gerekir. Aşağıdaki adımlar uygulama ağ geçidi kaynağı için gerekli yapılandırma öğelerini oluşturur.

### 1. Adım

"gatewayIP01" adlı uygulama ağ geçidi IP yapılandırması oluşturun. Application Gateway başladığında, yapılandırılan alt ağdan bir IP adresi alır ve ağ trafiğini arka uç IP havuzundaki IP adreslerine yönlendirir. Her örneğin bir IP adresi alacağını göz önünde bulundurun.


    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet


### 2. Adım

"pool01" adlı arka uç IP adresi havuzunu "134.170.185.46, 134.170.188.221,134.170.185.50." IP adresleriyle yapılandırın. Bu adresler ön uç IP uç noktasından gelen ağ trafiğinin yönlendirildiği IP adresleridir. Kendi uygulamanızın IP adresi uç noktalarını eklemek için Yukarıdaki IP adreslerini değiştireceksiniz.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50



### 3. Adım

"poolsetting01" uygulama ağ geçidi ayarlarını arka uç havuzundaki yük dengeli ağ trafiği için yapılandırın.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled


### 4. Adım

Genel IP uç noktası için "frontendport01" adlı ön uç IP bağlantı noktasını yapılandırın.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

### 5. Adım

"fipconfig01" adlı ön uç IP yapılandırmasını oluşturun ve genel IP adresiyle ön uç IP yapılandırmasını ilişkilendirin.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip


### 6. Adım

"listener01" adlı dinleyiciyi oluşturun ve ön uç bağlantı noktasıyla ön uç IP yapılandırmasını ilişkilendirin.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### 7. Adım

Yük dengeleyici davranışını yapılandıran "rule01" adlı yük dengeleyiciyi yönlendirme kuralını oluşturun.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### 8. Adım

Uygulama ağ geçidinin örnek boyutunu yapılandırın.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

>[AZURE.NOTE]  *InstanceCount* için varsayılan değer 2 ile 10 arasıdır. *GatewaySize* için varsayılan değer Medium’dur. Aynı zamanda Standard_Small, Standard_Medium ve Standard_Large seçenekleri de bulunmaktadır.

## New-AzureRmApplicationGateway kullanarak bir uygulama ağ geçidi oluşturma

Yukarıdaki adımlarda geçen tüm yapılandırma öğeleri ile bir uygulama ağ geçidi oluşturun. Bu örnekte uygulama ağ geçidi "appgwtest" olarak adlandırılmıştır.

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku


## Uygulama ağ geçidini silme

Uygulama ağ geçidini silmek için aşağıdaki adımları izleyin:

1. Ağ geçidini durdurmak için **Stop-AzureRmApplicationGateway** cmdlet’ini kullanın.
2. Ağ geçidini kaldırmak için **Remove-AzureRmApplicationGateway** cmdlet’ini kullanın.
3. **Get-AzureRmApplicationGateway** cmdlet’ini kullanarak kaldırılan ağ geçidini doğrulayın.

### 1. Adım

Uygulama ağ geçidi nesnesini alın ve "$getgw" değişkenine ilişkilendirin.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### 2. Adım

Uygulama ağ geçidini durdurmak için **Stop-AzureRmApplicationGateway**’i kullanın.

    Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  


Uygulama ağ geçidi durdurulmuş durumda olduğunda hizmeti kaldırmak için **Remove-AzureRmApplicationGateway** cmdlet’ini kullanın.


    Remove-AzureRmApplicationGateway -Name $appgwtest -ResourceGroupName appgw-rg -Force



>[AZURE.NOTE]  **-force** anahtarı, kaldırma onayı iletisini gizlemek için kullanılabilir.


Kaldırılan hizmeti doğrulamak için **Get-AzureRmApplicationGateway** cmdlet’ini kullanabilirsiniz. Bu adım gerekli değildir.


    Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg


## Sonraki adımlar

SSL yük boşaltmayı yapılandırmak istiyorsanız, bkz. [SSL yük boşaltımı için uygulama ağ geçidi yapılandırma](application-gateway-ssl.md).

İç yük dengeleyiciyle kullanacağınız uygulama ağ geçidi yapılandırmak istiyorsanız, bkz. [İç yük dengeleyici (ILB) ile uygulama ağ geçidi oluşturma](application-gateway-ilb.md).

Yük dengeleme seçenekleri hakkında daha fazla genel bilgi edinmek istiyorsanız, bkz.

- [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)



<!--HONumber=Jun16_HO2-->


