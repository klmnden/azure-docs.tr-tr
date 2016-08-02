<properties
   pageTitle="Bir uygulama ağ geçidi oluşturma, başlatma veya silme | Microsoft Azure"
   description="Bu sayfa bir Azure uygulama ağ geçidi oluşturma, yapılandırma, başlatma ve silme yönergelerini sağlar"
   documentationCenter="na"
   services="application-gateway"
   authors="joaoma"
   manager="jdial"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/05/2016"
   ms.author="joaoma"/>

# Bir uygulama ağ geçidi oluşturun, başlayın veya silin

Azure Application Gateway, bir katman 7 yük dengeleyicidir. Bulutta veya şirket içinde olmalarından bağımsız olarak, farklı sunucular arasında yük devretme ile HTTP istekleri için performans amaçlı yönlendirme sağlar. Application Gateway şu uygulama teslim özelliklerine sahiptir: HTTP yük dengeleme, tanımlama bilgisi tabanlı oturum benzeşimi ve Güvenli Yuva Katmanı (SSL) yük boşaltma.

> [AZURE.SELECTOR]
- [Azure Klasik PowerShell](application-gateway-create-gateway.md)
- [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
- [Azure Resource Manager şablonu](application-gateway-create-gateway-arm-template.md)


<BR>

Bu makale, uygulama ağ geçidi oluşturma, yapılandırma, başlatma ve silme adımlarında size eşlik eder.


## Başlamadan önce

1. Web Platformu Yükleyicisi’ni kullanarak Azure PowerShell cmdlet’lerin en son sürümünü yükleyin. **İndirmeler sayfası**’ndaki [Windows PowerShell](https://azure.microsoft.com/downloads/) bölümünden en son sürümü indirip yükleyebilirsiniz.
2. Geçerli bir alt ağla çalışan bir sanal ağa sahip olduğunuzu doğrulayın. Hiçbir sanal makinenin veya bulut dağıtımlarının alt ağı kullanmadığından emin olun. Uygulama ağ geçidi tek başına bir sanal ağ alt ağında olmalıdır.
3. Uygulama ağ geçidi kullanırken yapılandıracağınız sunucular mevcut olmalıdır veya uç noktaları sanal ağda veya atanan genel bir IP/VIP’de oluşturulmuş olmalıdır.

## Bir uygulama ağ geçidi oluşturmak için ne gereklidir?


Uygulama ağ geçidi oluşturmak için **New-AzureApplicationGateway** komutunu kullanırken hiçbir yapılandırma bulunmaz ve yeni oluşturulmuş kaynağın XML veya bir yapılandırma nesnesi kullanılarak yapılandırılması gerektir.


Değerler şunlardır:

- **Arka uç sunucusu havuzu:** Arka uç sunucularının IP adreslerinin listesi. Listede bulunan IP adresleri sanal ağ alt ağına veya genel IP/VIP’ye ait olmalıdır.
- **Arka uç sunucu havuzu ayarları**: Her havuzun bağlantı noktası, protokol ve tanımlama bilgisi temelli benzeşim gibi ayarları vardır. Bu ayarlar bir havuza bağlıdır ve havuzdaki tüm sunuculara uygulanır.
- **Ön uç bağlantı noktası:** Bu bağlantı noktası uygulama ağ geçidinde açılan genel bağlantı noktasıdır. Bu bağlantı noktasında trafik olursa arka uç sunuculardan birine yönlendirilir.
- **Dinleyici:** Dinleyicide bir ön uç bağlantı noktası, bir protokol (büyük/küçük harfe duyarlı Http veya Https) ve SSL sertifika adı (SSL yük boşaltımı yapılandırılıyorsa) vardır.
- **Kural:** Kural, dinleyiciyi ve arka uç sunucusu havuzunu bağlar ve belli bir dinleyicide trafik olduğunda trafiğin hangi arka uç sunucu havuzuna yönlendirileceğini belirler.


## Yeni bir uygulama ağ geçidi oluşturun

Bir uygulama ağ geçidi oluşturmak için:

1. Uygulama ağ geçidi kaynağı oluşturun.
2. Bir XML yapılandırma dosyası veya yapılandırma nesnesi oluşturun.
3. Yapılandırmayı, yeni oluşturulmuş uygulama ağ geçidi kaynağına uygulayın.

>[AZURE.NOTE] Uygulama ağ geçidiniz için özel bir araştırma yapılandırmanız gerekiyorsa, bkz. [PowerShell kullanarak özel araştırmalara sahip bir uygulama ağ geçidi oluşturma](application-gateway-create-probe-classic-ps.md). Daha fazla bilgi için [özel araştırmalar ve sistem durumu izleme](application-gateway-probe-overview.md) konusunu inceleyin.


### Bir uygulama ağ geçidi kaynağı oluşturma

Ağ geçidini oluşturmak için, **New-AzureApplicationGateway** cmdlet’ini kullanın ve değerleri kendinizinkilerle değiştirin. Ağ geçidinin faturalanmasının henüz bu aşamada başlamadığını hatırlatmak isteriz.  Daha sonra ağ geçidi başarıyla başlatıldığında faturalama da başlar. 

Aşağıdaki örnek, "testvnet1" adlı sanal ağı ve "subnet-1" aklı alt ağı kullanarak yeni bir uygulama ağ geçidi oluşturur.


    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399


 *Description*, *InstanceCount* ve *GatewaySize* tercihe bağlı parametrelerdir.


Ağ geçidinin oluşturulduğunu doğrulamak için **Get-AzureApplicationGateway** cmdlet’ini kullanabilirsiniz.




    Get-AzureApplicationGateway AppGwTest
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Stopped
    VirtualIPs    : {}
    DnsName       :

>[AZURE.NOTE]  *InstanceCount* için varsayılan değer 2 ile 10 arasıdır. *GatewaySize* için varsayılan değer Medium’dur. Small, Medium ve Large seçenekleri bulunur.


 Ağ geçidi daha başlatılmadığından dolayı *VirtualIPs* ve *DnsName* boş görünür. Ağ geçidi çalışma durumuna geçtiğinde oluşturulurlar.

## Uygulama ağ geçidini yapılandırma

XML veya bir yapılandırma nesnesi kullanarak uygulama ağ geçidini yapılandırabilirsiniz.

## XML kullanarak uygulama ağ geçidi yapılandırma

Aşağıdaki örnekte, tüm uygulama ağ geçidi ayarlarını yapılandırmak için bir XML dosyası kullanacaksınız ve bu ayarları uygulama ağ geçidi kaynağına uygulayacaksınız.  

### 1. Adım  

Aşağıdaki metni Notepad’a kopyalayın.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>(name-of-your-frontend-port)</Name>
                <Port>(port number)</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>(name-of-your-backend-pool)</Name>
                <IPAddresses>
                    <IPAddress>(your-IP-address-for-backend-pool)</IPAddress>
                    <IPAddress>(your-second-IP-address-for-backend-pool)</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>(backend-setting-name-to-configure-rule)</Name>
                <Port>80</Port>
                <Protocol>[Http|Https]</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>(name-of-the-listener)</Name>
                <FrontendPort>(name-of-your-frontend-port)</FrontendPort>
                <Protocol>[Http|Https]</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>(name-of-load-balancing-rule)</Name>
                <Type>basic</Type>
                <BackendHttpSettings>(backend-setting-name-to-configure-rule)</BackendHttpSettings>
                <Listener>(name-of-the-listener)</Listener>
                <BackendAddressPool>(name-of-your-backend-pool)</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>

Parantez içindeki değerleri yapılandırma öğeleri için düzenleyin. Dosyası .xml. uzantısıyla kaydedin.

>[AZURE.IMPORTANT] Http veya Https protokol öğesi büyük/küçük harf duyarlıdır.

Aşağıdaki örnek, bir yapılandırma dosyası kullanarak, 80 numaralı genel bağlantı noktasındaki HTTP trafiğinin yükünü dengelemek ve ağ trafiğini iki IP adresi arasından 80 numarası bağlantı noktası arka ucuna yönlendirmek amacıyla uygulama ağ geçidinin nasıl oluşturulacağını gösterir.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>80</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>BackendPool1</Name>
                <IPAddresses>
                    <IPAddress>10.0.0.1</IPAddress>
                    <IPAddress>10.0.0.2</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>BackendSetting1</Name>
                <Port>80</Port>
                <Protocol>Http</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>HTTPListener1</Name>
                <FrontendPort>FrontendPort1</FrontendPort>
                <Protocol>Http</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>HttpLBRule1</Name>
                <Type>basic</Type>
                <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
                <Listener>HTTPListener1</Listener>
                <BackendAddressPool>BackendPool1</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


### 2. Adım

Sonra, uygulama ağ geçidini kurun. **Set-AzureApplicationGatewayConfig** cmdlet’ini bir XML yapılandırma dosyasıyla kullanın.


    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## Bir yapılandırma nesnesi kullanarak uygulama ağ geçidi yapılandırma

Aşağıdaki örnek yapılandırma nesnesi kullanarak nasıl uygulama ağ geçidi yapılandırılacağını gösterir. Tüm yapılandırma öğeleri ayrı ayrı yapılandırılıp bir uygulama ağ geçidi yapılandırma nesnesine eklenmelidir. Yapılandırma nesnesini oluşturduktan sonra, yapılandırmayı, daha önce oluşturulmuş bir uygulama ağ geçidi kaynağına uygulamak için **Set-AzureApplicationGateway** komutunu kullanın.

>[AZURE.NOTE] Her yapılandırma nesnesine değer atamadan önce, PowerShell’in depolama için ne tür bir nesneyi kullanacağını belirtmeniz gerekir. Bireysel öğeleri oluşturan ilk satır hangi Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model(nesne adı)’in kullanılacağını tanımlar.

### 1. Adım

Tüm bireysel yapılandırma öğelerini oluşturun.

Ön uç IP’sini aşağıdaki örnekte gösterildiği gibi oluşturun.

    PS C:\> $fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
    PS C:\> $fip.Name = "fip1"
    PS C:\> $fip.Type = "Private"
    PS C:\> $fip.StaticIPAddress = "10.0.0.5"

Ön uç IP bağlantı noktasını aşağıdaki örnekte gösterildiği gibi oluşturun.

    PS C:\> $fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
    PS C:\> $fep.Name = "fep1"
    PS C:\> $fep.Port = 80

Arka uç sunucu havuzunu oluşturun.

 Arka uç sunucu havuzuna eklenecek IP adreslerini sonraki örnekte gösterildiği gibi tanımlayın.


    PS C:\> $servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
    PS C:\> $servers.Add("10.0.0.1")
    PS C:\> $servers.Add("10.0.0.2")

 Arka uç havuzu nesnesine değer eklemek için $sunucu nesnesini kullanın ($havuzu).

    PS C:\> $pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
    PS C:\> $pool.BackendServers = $servers
    PS C:\> $pool.Name = "pool1"

Arka uç sunucu havuzu ayarlarını oluşturun.

    PS C:\> $setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
    PS C:\> $setting.Name = "setting1"
    PS C:\> $setting.CookieBasedAffinity = "enabled"
    PS C:\> $setting.Port = 80
    PS C:\> $setting.Protocol = "http"

Dinleyiciyi oluşturun.

    PS C:\> $listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
    PS C:\> $listener.Name = "listener1"
    PS C:\> $listener.FrontendPort = "fep1"
    PS C:\> $listener.FrontendIP = "fip1"
    PS C:\> $listener.Protocol = "http"
    PS C:\> $listener.SslCert = ""

Kuralı oluşturun.

    PS C:\> $rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
    PS C:\> $rule.Name = "rule1"
    PS C:\> $rule.Type = "basic"
    PS C:\> $rule.BackendHttpSettings = "setting1"
    PS C:\> $rule.Listener = "listener1"
    PS C:\> $rule.BackendAddressPool = "pool1"

### 2. Adım

Tüm bireysel yapılandırma öğelerini bir uygulama ağ geçidi yapılandırma nesnesine atayın ($appgwconfig).

Yapılandırmaya ön uç IP’sini ekleyin.

    PS C:\> $appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
    PS C:\> $appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
    PS C:\> $appgwconfig.FrontendIPConfigurations.Add($fip)

Yapılandırmaya ön uç bağlantı noktasını ekleyin.

    PS C:\> $appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
    PS C:\> $appgwconfig.FrontendPorts.Add($fep)

Yapılandırmaya arka uç sunucu havuzunu ekleyin.

    PS C:\> $appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
    PS C:\> $appgwconfig.BackendAddressPools.Add($pool)  

Yapılandırmaya arka uç havuzu ayarlarını ekleyin.

    PS C:\> $appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
    PS C:\> $appgwconfig.BackendHttpSettingsList.Add($setting)

Yapılandırmaya dinleyiciyi ekleyin.

    PS C:\> $appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
    PS C:\> $appgwconfig.HttpListeners.Add($listener)

Yapılandırmaya kuralı ekleyin.

    PS C:\> $appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
    PS C:\> $appgwconfig.HttpLoadBalancingRules.Add($rule)

### 3. Adım

**Set-AzureApplicationGatewayConfig** kullanarak yapılandırma nesnesini uygulama ağ geçidi kaynağına uygulayın.

    Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig

## Ağ geçidini başlatma

Ağ geçidi yapılandırıldıktan sonra başlatmak için **Start-AzureApplicationGateway** cmdlet’ini kullanın. Uygulama ağ geçidinin faturalanması ağ geçidi başarıyla başlatıldıktan sonra başlar.


> [AZURE.NOTE] **Start-AzureApplicationGateway** cmdlet’in bitmesi 15-20 dakika sürebilir.



    Start-AzureApplicationGateway AppGwTest

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## Ağ geçidi durumunu doğrulama

**Get-AzureApplicationGateway** cmdlet’ini kullanarak ağ geçidinin durumunu inceleyin. Bir önceki adımda **Start-AzureApplicationGateway** başarılı olduysa, *State* Running durumunda olmalı, *Vip* ve *DnsName* ise geçerli girdilere sahip olmalıdır.

Aşağıdaki örnek, çalışır durumda ve `http://<generated-dns-name>.cloudapp.net` için atanmış trafiği almaya hazır bir uygulama ağ geçidini gösterir.

    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    Vip           : 138.91.170.26
    DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net


## Uygulama ağ geçidini silme

Uygulama ağ geçidi silmek için:

1. Ağ geçidini durdurmak için **Stop-AzureApplicationGateway** cmdlet’ini kullanın.
2. Ağ geçidini kaldırmak için **Remove-AzureApplicationGateway** cmdlet’ini kullanın.
3. Ağ geçidinin **Get-AzureApplicationGateway** cmdlet’i kullanılarak kaldırıldığını doğrulayın.

Aşağıdaki örnek ilk satırdaki devamında girdinin bulunduğu **Stop-AzureApplicationGateway** cmdlet’ini gösterir.

    Stop-AzureApplicationGateway AppGwTest

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Uygulama ağ geçidi durdurulmuş durumda olduğunda hizmeti kaldırmak için **Remove-AzureApplicationGateway** cmdlet’ini kullanın.


    Remove-AzureApplicationGateway AppGwTest

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

Hizmetin kaldırıldığını doğrulamak için **Get-AzureApplicationGateway** cmdlet’ini kullanabilirsiniz. Bu adım gerekli değildir.


    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## Sonraki adımlar

SSL yük boşaltmayı yapılandırmak istiyorsanız, bkz. [SSL yük boşaltımı için uygulama ağ geçidi yapılandırma](application-gateway-ssl.md).

İç yük dengeleyiciyle kullanacağınız uygulama ağ geçidi yapılandırmak istiyorsanız, bkz. [İç yük dengeleyici (ILB) ile uygulama ağ geçidi oluşturma](application-gateway-ilb.md).

Yük dengeleme seçenekleri hakkında daha fazla genel bilgi edinmek istiyorsanız, bkz.

- [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)



<!--HONumber=Jun16_HO2-->


