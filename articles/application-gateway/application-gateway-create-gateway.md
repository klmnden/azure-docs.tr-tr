---
title: "Bir uygulama ağ geçidi oluşturma, başlatma veya silme | Microsoft Belgeleri"
description: "Bu sayfa bir Azure uygulama ağ geçidi oluşturma, yapılandırma, başlatma ve silme yönergelerini sağlar"
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 577054ca-8368-4fbf-8d53-a813f29dc3bc
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: davidmu
ms.translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: 79e373a69f3b899dea1f10ac447a0284931648f4
ms.contentlocale: tr-tr
ms.lasthandoff: 05/02/2017

---
# <a name="create-start-or-delete-an-application-gateway-with-powershell"></a>PowerShell ile bir uygulama ağ geçidi oluşturma, başlatma veya silme 

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Azure Klasik PowerShell](application-gateway-create-gateway.md)
> * [Azure Resource Manager şablonu](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI](application-gateway-create-gateway-cli.md)

Azure Application Gateway, bir katman 7 yük dengeleyicidir. Bulutta veya şirket içinde olmalarından bağımsız olarak, farklı sunucular arasında yük devretme ile HTTP istekleri için performans amaçlı yönlendirme sağlar. Application Gateway; HTTP yük dengeleme, tanımlama bilgisi tabanlı oturum benzeşimi, Güvenli Yuva Katmanı (SSL) boşaltma, özel sistem durumu araştırmaları, çoklu site desteği gibi birçok Application Delivery Controller (ADC) özelliği sunar. Desteklenen özelliklerin tam listesi için bkz. [Application Gateway’e Genel Bakış](application-gateway-introduction.md)

Bu makale, uygulama ağ geçidi oluşturma, yapılandırma, başlatma ve silme adımlarında size eşlik eder.

## <a name="before-you-begin"></a>Başlamadan önce

1. Web Platformu Yükleyicisi’ni kullanarak Azure PowerShell cmdlet’lerin en son sürümünü yükleyin. **İndirmeler sayfası**’ndaki [Windows PowerShell](https://azure.microsoft.com/downloads/) bölümünden en son sürümü indirip yükleyebilirsiniz.
2. Mevcut bir sanal ağınız varsa var olan boş bir alt ağı seçin ya da var olan sanal ağınızda yalnızca uygulama ağ geçidinin kullanımına yönelik yeni bir alt ağ oluşturun. Sanal ağ eşleme kullanılmıyorsa uygulama ağ geçidini, uygulama ağ geçidinin arkasına dağıtmak istediğiniz kaynaklardan farklı bir sanal ağa dağıtamazsınız. Daha fazla bilgi için bkz. [Sanal Ağ Eşleme](../virtual-network/virtual-network-peering-overview.md)
3. Geçerli bir alt ağla çalışan bir sanal ağa sahip olduğunuzu doğrulayın. Hiçbir sanal makinenin veya bulut dağıtımlarının alt ağı kullanmadığından emin olun. Uygulama ağ geçidi tek başına bir sanal ağ alt ağında olmalıdır.
4. Uygulama ağ geçidi kullanırken yapılandırdığınız sunucular mevcut olmalıdır veya uç noktaları sanal ağda veya atanan genel bir IP/VIP’de oluşturulmuş olmalıdır.

## <a name="what-is-required-to-create-an-application-gateway"></a>Bir uygulama ağ geçidi oluşturmak için ne gereklidir?

Uygulama ağ geçidini oluşturmak için `New-AzureApplicationGateway` komutunu kullanırsanız, bu noktada yapılandırma ayarlanmaz ve yeni oluşturulmuş kaynak, XML veya bir yapılandırma nesnesi kullanılarak yapılandırılır.

Değerler şunlardır:

* **Arka uç sunucusu havuzu:** Arka uç sunucularının IP adreslerinin listesi. Listede bulunan IP adresleri sanal ağ alt ağına veya genel IP/VIP’ye ait olmalıdır.
* **Arka uç sunucu havuzu ayarları**: Her havuzun bağlantı noktası, protokol ve tanımlama bilgisi temelli benzeşim gibi ayarları vardır. Bu ayarlar bir havuza bağlıdır ve havuzdaki tüm sunuculara uygulanır.
* **Ön uç bağlantı noktası:** Bu bağlantı noktası uygulama ağ geçidinde açılan genel bağlantı noktasıdır. Bu bağlantı noktasında trafik olursa arka uç sunuculardan birine yönlendirilir.
* **Dinleyici:** Dinleyicide bir ön uç bağlantı noktası, bir protokol (Http veya Https, bu değerler büyük/küçük harfe duyarlıdır) ve SSL sertifika adı (SSL yük boşaltımı yapılandırılıyorsa) vardır.
* **Kural:** Kural, dinleyiciyi ve arka uç sunucusu havuzunu bağlar ve belli bir dinleyicide trafik olduğunda trafiğin hangi arka uç sunucu havuzuna yönlendirileceğini belirler.

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Bir uygulama ağ geçidi oluşturmak için:

1. Uygulama ağ geçidi kaynağı oluşturun.
2. Bir XML yapılandırma dosyası veya yapılandırma nesnesi oluşturun.
3. Yapılandırmayı, yeni oluşturulmuş uygulama ağ geçidi kaynağına uygulayın.

> [!NOTE]
> Uygulama ağ geçidiniz için özel bir araştırma yapılandırmanız gerekiyorsa, bkz. [PowerShell kullanarak özel araştırmalara sahip bir uygulama ağ geçidi oluşturma](application-gateway-create-probe-classic-ps.md). Daha fazla bilgi için [özel araştırmalar ve sistem durumu izleme](application-gateway-probe-overview.md) konusunu inceleyin.

![Senaryo örneği][scenario]

### <a name="create-an-application-gateway-resource"></a>Bir uygulama ağ geçidi kaynağı oluşturma

Ağ geçidini oluşturmak için, `New-AzureApplicationGateway` cmdlet’ini kullanın ve değerleri kendi değerlerinizle değiştirin. Ağ geçidinin faturalanması bu aşamada başlamaz. Daha sonra ağ geçidi başarıyla başlatıldığında faturalama da başlar. 

Aşağıdaki örnek, "testvnet1" adlı sanal ağı ve "subnet-1" aklı alt ağı kullanarak bir uygulama ağ geçidi oluşturur:

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

*Description*, *InstanceCount* ve *GatewaySize* tercihe bağlı parametrelerdir.

Ağ geçidinin oluşturulduğunu doğrulamak için `Get-AzureApplicationGateway` cmdlet’ini kullanabilirsiniz.

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
Name          : AppGwTest
Description   :
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Stopped
VirtualIPs    : {}
DnsName       :
```

> [!NOTE]
> *InstanceCount* için varsayılan değer 2 ile 10 arasıdır. *GatewaySize* için varsayılan değer Medium’dur. Small, Medium ve Large seçenekleri bulunur.

Ağ geçidi daha başlatılmadığından dolayı *VirtualIPs* ve *DnsName* boş görünür. Bunlar ağ geçidi çalışma durumuna geçtiğinde oluşturulur.

## <a name="configure-the-application-gateway"></a>Uygulama ağ geçidini yapılandırma

XML veya bir yapılandırma nesnesi kullanarak uygulama ağ geçidini yapılandırabilirsiniz.

### <a name="configure-the-application-gateway-by-using-xml"></a>XML kullanarak uygulama ağ geçidi yapılandırma

Aşağıdaki örnekte, tüm uygulama ağ geçidi ayarlarını yapılandırmak için bir XML dosyası kullanır ve bu ayarları uygulama ağ geçidi kaynağına uygularsınız.  

#### <a name="step-1"></a>1. Adım

Aşağıdaki metni Notepad’a kopyalayın.

```xml
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
```

Parantez içindeki değerleri yapılandırma öğeleri için düzenleyin. Dosyası .xml. uzantısıyla kaydedin.

> [!IMPORTANT]
> Http veya Https protokol öğesi büyük/küçük harf duyarlıdır.

Aşağıdaki örnekte uygulama ağ geçidi ayarlamak için yapılandırma dosyası kullanma işlemi gösterilmektedir. Örnek, genel bağlantı noktası 80 üzerinde HTTP trafiğinin yük dengelemesini yapar ve iki IP adresi arasındaki arka uç bağlantı noktası 80’e ağ trafiği gönderir.

```xml
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
```

#### <a name="step-2"></a>2. Adım

Sonra, uygulama ağ geçidini kurun. `Set-AzureApplicationGatewayConfig` cmdlet’ini, yapılandırma XML dosyasıyla kullanın.

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"
```

### <a name="configure-the-application-gateway-by-using-a-configuration-object"></a>Bir yapılandırma nesnesi kullanarak uygulama ağ geçidi yapılandırma

Aşağıdaki örnek yapılandırma nesnesi kullanarak nasıl uygulama ağ geçidi yapılandırılacağını gösterir. Tüm yapılandırma öğeleri ayrı ayrı yapılandırılıp bir uygulama ağ geçidi yapılandırma nesnesine eklenmelidir. Yapılandırma nesnesini oluşturduktan sonra, yapılandırmayı daha önce oluşturulmuş bir uygulama ağ geçidi kaynağına uygulamak için `Set-AzureApplicationGateway` komutunu kullanın.

> [!NOTE]
> Her yapılandırma nesnesine değer atamadan önce, PowerShell’in depolama için ne tür bir nesneyi kullanacağını belirtmeniz gerekir. Bağımsız öğeleri oluşturan birinci satır, kullanılan `Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model(object name)` öğelerini tanımlar.

#### <a name="step-1"></a>1. Adım

Tüm bireysel yapılandırma öğelerini oluşturun.

Ön uç IP’sini aşağıdaki örnekte gösterildiği gibi oluşturun.

```powershell
$fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
$fip.Name = "fip1"
$fip.Type = "Private"
$fip.StaticIPAddress = "10.0.0.5"
```

Ön uç IP bağlantı noktasını aşağıdaki örnekte gösterildiği gibi oluşturun.

```powershell
$fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
$fep.Name = "fep1"
$fep.Port = 80
```

Arka uç sunucu havuzunu oluşturun.

Arka uç sunucu havuzuna eklenecek IP adreslerini sonraki örnekte gösterildiği gibi tanımlayın.

```powershell
$servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
$servers.Add("10.0.0.1")
$servers.Add("10.0.0.2")
```

Arka uç havuzu nesnesine değer eklemek için $sunucu nesnesini kullanın ($havuzu).

```powershell
$pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
$pool.BackendServers = $servers
$pool.Name = "pool1"
```

Arka uç sunucu havuzu ayarlarını oluşturun.

```powershell
$setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
$setting.Name = "setting1"
$setting.CookieBasedAffinity = "enabled"
$setting.Port = 80
$setting.Protocol = "http"
```

Dinleyiciyi oluşturun.

```powershell
$listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
$listener.Name = "listener1"
$listener.FrontendPort = "fep1"
$listener.FrontendIP = "fip1"
$listener.Protocol = "http"
$listener.SslCert = ""
```

Kuralı oluşturun.

```powershell
$rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
$rule.Name = "rule1"
$rule.Type = "basic"
$rule.BackendHttpSettings = "setting1"
$rule.Listener = "listener1"
$rule.BackendAddressPool = "pool1"
```

#### <a name="step-2"></a>2. Adım

Tüm bireysel yapılandırma öğelerini bir uygulama ağ geçidi yapılandırma nesnesine atayın ($appgwconfig).

Yapılandırmaya ön uç IP’sini ekleyin.

```powershell
$appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
$appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
$appgwconfig.FrontendIPConfigurations.Add($fip)
```

Yapılandırmaya ön uç bağlantı noktasını ekleyin.

```powershell
$appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
$appgwconfig.FrontendPorts.Add($fep)
```
Yapılandırmaya arka uç sunucu havuzunu ekleyin.

```powershell
$appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
$appgwconfig.BackendAddressPools.Add($pool)
```

Yapılandırmaya arka uç havuzu ayarlarını ekleyin.

```powershell
$appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
$appgwconfig.BackendHttpSettingsList.Add($setting)
```

Yapılandırmaya dinleyiciyi ekleyin.

```powershell
$appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
$appgwconfig.HttpListeners.Add($listener)
```

Yapılandırmaya kuralı ekleyin.

```powershell
$appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
$appgwconfig.HttpLoadBalancingRules.Add($rule)
```

### <a name="step-3"></a>3. Adım
`Set-AzureApplicationGatewayConfig` kullanarak, yapılandırma nesnesini uygulama ağ geçidi kaynağına uygulayın.

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig
```

## <a name="start-the-gateway"></a>Ağ geçidini başlatma

Ağ geçidi yapılandırıldıktan sonra, ağ geçidini başlatmak için `Start-AzureApplicationGateway` cmdlet’ini kullanın. Uygulama ağ geçidinin faturalanması ağ geçidi başarıyla başlatıldıktan sonra başlar.

> [!NOTE]
> `Start-AzureApplicationGateway` cmdlet’inin tamamlanması 15-20 dakika sürebilir.

```powershell
Start-AzureApplicationGateway AppGwTest
```

## <a name="verify-the-gateway-status"></a>Ağ geçidi durumunu doğrulama

Ağ geçidinin durumunu denetlemek için `Get-AzureApplicationGateway` cmdlet’ini kullanın. Bir önceki adımda `Start-AzureApplicationGateway` başarılı olduysa, *State* Running durumunda olmalı, *Vip* ve *DnsName* ise geçerli girdilere sahip olmalıdır.

Aşağıdaki örnek, çalışır durumda ve `http://<generated-dns-name>.cloudapp.net` için atanmış trafiği almaya hazır bir uygulama ağ geçidini gösterir.

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
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
```

## <a name="delete-the-application-gateway"></a>Uygulama ağ geçidini silme

Uygulama ağ geçidini silmek için:

1. Ağ geçidini durdurmak için `Stop-AzureApplicationGateway` cmdlet’ini kullanın.
2. Ağ geçidini kaldırmak için `Remove-AzureApplicationGateway` cmdlet’ini kullanın.
3. Ağ geçidinin kaldırıldığını doğrulamak için `Get-AzureApplicationGateway` cmdlet’ini kullanın.

Aşağıdaki örnek, ilk satırda, devamında çıktının bulunduğu `Stop-AzureApplicationGateway` cmdlet’ini gösterir.

```powershell
Stop-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8
```

Uygulama ağ geçidi durdurulmuş konumda olduğunda, hizmeti kaldırmak için `Remove-AzureApplicationGateway` cmdlet’ini kullanın.

```powershell
Remove-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301
```

Hizmetin kaldırıldığını doğrulamak için `Get-AzureApplicationGateway` cmdlet’ini kullanabilirsiniz. Bu adım gerekli değildir.

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
.....
```

## <a name="next-steps"></a>Sonraki adımlar

SSL yük boşaltmayı yapılandırmak istiyorsanız, bkz. [SSL yük boşaltımı için uygulama ağ geçidi yapılandırma](application-gateway-ssl.md).

İç yük dengeleyiciyle kullanacağınız uygulama ağ geçidi yapılandırmak istiyorsanız, bkz. [İç yük dengeleyici (ILB) ile uygulama ağ geçidi oluşturma](application-gateway-ilb.md).

Yük dengeleme seçenekleri hakkında daha fazla genel bilgi edinmek istiyorsanız, bkz.

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

[scenario]: ./media/application-gateway-create-gateway/scenario.png

