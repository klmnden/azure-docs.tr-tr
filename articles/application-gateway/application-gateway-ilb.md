---
title: "Azure uygulama ağ geçidi iç yük dengeleyici ile kullanma | Microsoft Docs"
description: "Bu sayfa, Azure uygulama ağ geçidi ile iç yük dengeli uç nokta yapılandırmaya yönelik yönergeler sağlar"
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 7403d28e-909f-46a2-b282-43a8e942f53c
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: davidmu
ms.openlocfilehash: 7ca9307e8a78f6dade4b231fa3a0d83a68af3f21
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb"></a>İç Load Balancer (ILB) aracılığıyla Application Gateway oluşturun

> [!div class="op_single_selector"]
> * [Azure Klasik PowerShell](application-gateway-ilb.md)
> * [Azure Resource Manager PowerShell](application-gateway-ilb-arm.md)

Uygulama ağ geçidi olarak da bilinen iç yük dengeleyici (ILB) uç nokta Internet'e açık olmayan bir iç uç nokta veya internet'e yönelik sanal IP ile yapılandırılabilir. Ağ geçidini bir ILB ile yapılandırma internet'e açık değil iç iş kolu satır uygulamaları için kullanışlıdır. İnternet'e açık olmayan bir güvenlik sınırı içinde bulunur, ancak hala hepsini bir kez deneme yük dağıtımı, oturum sürekliliği veya SSL sonlandırması gerektiren çok katmanlı uygulama içinde Hizmetleri/katmanları için de yararlıdır. Bu makale, ILB ile uygulama ağ geçidi yapılandırma adımlarında size yol gösterir.

## <a name="before-you-begin"></a>Başlamadan önce

1. Web Platformu Yükleyicisi'ni kullanarak Azure PowerShell cmdlet'lerinin en yeni sürümünü yükleyin. En son sürümü yükleyip **Windows PowerShell** bölümünü [indirme sayfası](https://azure.microsoft.com/downloads/).
2. Geçerli bir alt ağ ile çalışan bir sanal ağa sahip olduğunuzu doğrulayın.
3. Sanal ağda veya atanan genel IP/VIP'ye ile arka uç sunucularına sahip olduğunuzu doğrulayın.

Bir uygulama ağ geçidi oluşturmak için listelenen sırayla aşağıdaki adımları gerçekleştirin. 

1. [Bir uygulama ağ geçidi oluşturma](#create-a-new-application-gateway)
2. [Ağ geçidini yapılandırma](#configure-the-gateway)
3. [Ağ geçidi yapılandırmasını ayarlayın](#set-the-gateway-configuration)
4. [Ağ geçidini başlatma](#start-the-gateway)
5. [Ağ geçidi doğrulayın](#verify-the-gateway-status)

## <a name="create-an-application-gateway"></a>Bir uygulama ağ geçidi oluşturun:

**Ağ geçidi oluşturmak için**, kullanın `New-AzureApplicationGateway` cmdlet'ini değerleri kendi değerlerinizle değiştirerek. Ağ geçidinin faturalanmasının henüz bu aşamada başlamadığını hatırlatmak isteriz.  Daha sonra ağ geçidi başarıyla başlatıldığında faturalama da başlar. 

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

```
VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway 
VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error 
----       ----------------     ------------                             ----
Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399
```

**Doğrulanacak** ağ geçidinin oluşturulduğunu, kullanabileceğiniz `Get-AzureApplicationGateway` cmdlet'i. 

Örnekte, *açıklama*, *Instancecount*, ve *GatewaySize* isteğe bağlı parametrelerdir. *InstanceCount* için varsayılan değer 2 ile 10 arasıdır. *GatewaySize* için varsayılan değer Medium’dur. Küçük ve büyük diğer değerleri kullanılabilir. *VIP* ve *DnsName* ağ geçidi henüz başlatılmamış olduğundan boş olarak gösterilir. Bunlar ağ geçidi çalışma durumuna geçtiğinde oluşturulur. 

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 4:39:39 PM - Begin Operation:
Get-AzureApplicationGateway VERBOSE: 4:39:40 PM - Completed 
Operation: Get-AzureApplicationGateway
Name: AppGwTest    
Description: 
VnetName: testvnet1 
Subnets: {Subnet-1} 
InstanceCount: 2 
GatewaySize: Medium 
State: Stopped 
VirtualIPs: 
DnsName:
```

## <a name="configure-the-gateway"></a>Ağ geçidini yapılandırma
Bir uygulama ağ geçidi yapılandırması birden çok değerden oluşur. Değerleri bağlı yapılandırma birlikte oluşturulamadı.

Değerler şunlardır:

* **Arka uç sunucusu havuzu:** arka uç sunucularının IP adreslerinin listesi. Listede bulunan IP adresleri ya da sanal ağ alt ağına ait olması gerekir veya genel IP/VIP'ye olmalıdır. 
* **Arka uç sunucu havuzu ayarları**: Her havuzun bağlantı noktası, protokol ve tanımlama bilgisi temelli benzeşim gibi ayarları vardır. Bu ayarlar bir havuza bağlıdır ve havuzdaki tüm sunuculara uygulanır.
* **Ön uç bağlantı noktası:** Bu bağlantı noktası uygulama ağ geçidinde açılan genel bağlantı noktasıdır. Bu bağlantı noktasında trafik olursa arka uç sunuculardan birine yönlendirilir.
* **Dinleyici:** dinleyicisinde bir ön uç bağlantı noktası, bir iletişim kuralı kullanılır (Http veya Https, bunlar büyük küçük harfe duyarlı) ve (SSL yük boşaltımı yapılandırılıyorsa) SSL sertifika adı. 
* **Kural:** kural dinleyiciyi ve arka uç sunucusu havuzunu bağlar ve trafiği için belli bir Dinleyicide trafik olduğunda hangi arka uç sunucu havuzuna yönlendirileceğini belirler. Şu anda yalnızca *temel* kural desteklenmektedir. *Temel* kural hepsini bir kez deneme yöntemiyle yük dağıtımıdır.

Bir yapılandırma nesnesi oluşturarak veya bir XML yapılandırma dosyası kullanarak yapılandırmanızı oluşturabilirsiniz. Bir XML yapılandırma dosyası kullanarak yapılandırmanızı oluşturmak için aşağıdaki örneği kullanın.

Şunlara dikkat edin:

* *Frontendıpconfigurations'a* öğesi bir ILB ile uygulama ağ geçidi yapılandırılmasıyla ilgili ILB ayrıntıları açıklar. 
* Ön uç IP *türü* 'Özel' olarak ayarlanmalıdır.
* *StaticIPAddress* ağ geçidi üzerinde trafiği alır istenen iç IP ayarlamanız gerekir. Unutmayın *StaticIPAddress* öğesidir isteğe bağlıdır. Aksi durumda kümesi, kullanılabilir bir iç IP dağıtılan alt ağdan seçilir. 
* Değeri *adı* belirtilen öğesi *Frontendıpconfiguration* HTTPListener içinde 's kullanılmalıdır *FrontendIP* Frontendıpconfiguration başvurmak için öğesi.
  
  **Yapılandırma XML örneği**
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations>
        <FrontendIPConfiguration>
            <Name>fip1</Name> 
            <Type>Private</Type> 
            <StaticIPAddress>10.0.0.10</StaticIPAddress> 
        </FrontendIPConfiguration>
    </FrontendIPConfigurations>
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
            <FrontendIP>fip1</FrontendIP>
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


## <a name="set-the-gateway-configuration"></a>Ağ geçidi yapılandırmasını ayarlayın
Ardından, uygulama ağ geçidi ayarlarsınız. Kullanabileceğiniz `Set-AzureApplicationGatewayConfig` cmdlet'i bir yapılandırma nesnesi veya bir XML yapılandırma dosyası. 

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml
```

```
VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig 
VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
Name       HTTP Status Code     Operation ID                             Error 
----       ----------------     ------------                             ----
Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d
```

## <a name="start-the-gateway"></a>Ağ geçidini başlatma

Ağ geçidi yapılandırıldıktan sonra, ağ geçidini başlatmak için `Start-AzureApplicationGateway` cmdlet’ini kullanın. Uygulama ağ geçidinin faturalanması ağ geçidi başarıyla başlatıldıktan sonra başlar. 

> [!NOTE]
> `Start-AzureApplicationGateway` Cmdlet 15-20 tamamlamak için dakika kadar sürebilir. 
> 
> 

```powershell
Start-AzureApplicationGateway AppGwTest 
```

```
VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway 
VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error 
----       ----------------     ------------                             ----
Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b
```

## <a name="verify-the-gateway-status"></a>Ağ geçidi durumunu doğrulama

Kullanım `Get-AzureApplicationGateway` cmdlet'ini ağ geçidi durumunu kontrol edin. Varsa `Start-AzureApplicationGateway` önceki adımda başarılı oldu, durum olmalıdır *çalıştıran*, ve VIP ve DnsName geçerli girdilere sahip olmalıdır. Bu örnek cmdlet ilk satırında gösterir çıktı tarafından takip. Bu örnek, ağ geçidi çalışıyor ve trafiği almaya hazır. 

> [!NOTE]
> Uygulama ağ geçidi 10.0.0.10 Bu örnekte, yapılandırılmış ILB uç noktada trafiğini kabul edecek şekilde yapılandırılır.

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
VirtualIPs    : {10.0.0.10}
DnsName       : appgw-b2a11563-2b3a-4172-a4aa-226ee4c23eed.cloudapp.net
```

## <a name="next-steps"></a>Sonraki adımlar
Yük dengeleme seçenekleri hakkında daha fazla genel bilgi edinmek istiyorsanız, bkz.

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

