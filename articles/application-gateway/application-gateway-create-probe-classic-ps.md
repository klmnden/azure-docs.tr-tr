---
title: Özel araştırma - Azure uygulama ağ geçidi - PowerShell Klasik oluşturun | Microsoft Docs
description: Klasik dağıtım modelinde PowerShell kullanarak uygulama ağ geçidi için özel bir araştırma oluşturmayı öğrenin
services: application-gateway
documentationcenter: na
author: vhorne
manager: jpconnock
editor: ''
tags: azure-service-management
ms.assetid: 338a7be1-835c-48e9-a072-95662dc30f5e
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: victorh
ms.openlocfilehash: 97d1376dc7908b72d8e8ec15145229cf3cf4acae
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a>Özel bir araştırma için Azure uygulama ağ geçidi (Klasik) PowerShell kullanarak oluşturma

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-probe-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-probe-ps.md)
> * [Azure Klasik PowerShell](application-gateway-create-probe-classic-ps.md)

Bu makalede, PowerShell ile var olan bir uygulama ağ geçidi için özel bir araştırma ekleyin. Özel araştırmalara başarılı yanıt varsayılan web uygulaması üzerinde sağlamaz, uygulamaları veya belirli bir sağlık denetimi sayfası uygulamaları için kullanışlıdır.

> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. [Bu adımları Resource Manager modeli kullanarak gerçekleştirmeyi](application-gateway-create-probe-ps.md) öğrenin.

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Bir uygulama ağ geçidi oluşturmak için:

1. Uygulama ağ geçidi kaynağı oluşturun.
2. Bir XML yapılandırma dosyası veya yapılandırma nesnesi oluşturun.
3. Yapılandırmayı, yeni oluşturulmuş uygulama ağ geçidi kaynağına uygulayın.

### <a name="create-an-application-gateway-resource-with-a-custom-probe"></a>Özel bir araştırma ile uygulama ağ geçidi kaynağı oluşturma

Ağ geçidini oluşturmak için, `New-AzureApplicationGateway` cmdlet’ini kullanın ve değerleri kendi değerlerinizle değiştirin. Ağ geçidinin faturalanması bu aşamada başlamaz. Daha sonra ağ geçidi başarıyla başlatıldığında faturalama da başlar. 

Aşağıdaki örnek, "testvnet1" adlı sanal ağı ve "subnet-1" aklı alt ağı kullanarak bir uygulama ağ geçidi oluşturur.

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

Ağ geçidinin oluşturulduğunu doğrulamak için `Get-AzureApplicationGateway` cmdlet’ini kullanabilirsiniz.

```powershell
Get-AzureApplicationGateway AppGwTest
```

> [!NOTE]
> *InstanceCount* için varsayılan değer 2 ile 10 arasıdır. *GatewaySize* için varsayılan değer Medium’dur. Küçük, Orta ve büyük arasında seçim yapabilirsiniz.
> 
> 

Ağ geçidi daha başlatılmadığından dolayı *VirtualIPs* ve *DnsName* boş görünür. Ağ geçidi çalışır durumda olduğunda bu değerleri oluşturulur.

### <a name="configure-an-application-gateway-by-using-xml"></a>XML kullanarak uygulama ağ geçidi yapılandırma

Aşağıdaki örnekte, tüm uygulama ağ geçidi ayarlarını yapılandırmak için bir XML dosyası kullanır ve bu ayarları uygulama ağ geçidi kaynağına uygularsınız.  

Aşağıdaki metni Notepad’a kopyalayın.

```xml
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
<FrontendIPConfigurations>
    <FrontendIPConfiguration>
        <Name>fip1</Name>
        <Type>Private</Type>
    </FrontendIPConfiguration>
</FrontendIPConfigurations>
<FrontendPorts>
    <FrontendPort>
        <Name>port1</Name>
        <Port>80</Port>
    </FrontendPort>
</FrontendPorts>
<Probes>
    <Probe>
        <Name>Probe01</Name>
        <Protocol>Http</Protocol>
        <Host>contoso.com</Host>
        <Path>/path/custompath.htm</Path>
        <Interval>15</Interval>
        <Timeout>15</Timeout>
        <UnhealthyThreshold>5</UnhealthyThreshold>
    </Probe>
    </Probes>
    <BackendAddressPools>
    <BackendAddressPool>
        <Name>pool1</Name>
        <IPAddresses>
            <IPAddress>1.1.1.1</IPAddress>
            <IPAddress>2.2.2.2</IPAddress>
        </IPAddresses>
    </BackendAddressPool>
</BackendAddressPools>
<BackendHttpSettingsList>
    <BackendHttpSettings>
        <Name>setting1</Name>
        <Port>80</Port>
        <Protocol>Http</Protocol>
        <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        <RequestTimeout>120</RequestTimeout>
        <Probe>Probe01</Probe>
    </BackendHttpSettings>
</BackendHttpSettingsList>
<HttpListeners>
    <HttpListener>
        <Name>listener1</Name>
        <FrontendIP>fip1</FrontendIP>
    <FrontendPort>port1</FrontendPort>
        <Protocol>Http</Protocol>
    </HttpListener>
</HttpListeners>
<HttpLoadBalancingRules>
    <HttpLoadBalancingRule>
        <Name>lbrule1</Name>
        <Type>basic</Type>
        <BackendHttpSettings>setting1</BackendHttpSettings>
        <Listener>listener1</Listener>
        <BackendAddressPool>pool1</BackendAddressPool>
    </HttpLoadBalancingRule>
</HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

Parantez içindeki değerleri yapılandırma öğeleri için düzenleyin. Dosyası .xml. uzantısıyla kaydedin.

Aşağıdaki örnek bir yapılandırma dosyası ortak bağlantı noktası 80 üzerinde HTTP trafiği dengelemek ve arka uç bağlantı noktası 80'özel bir araştırma kullanarak iki IP adresi arasındaki ağ trafiğini göndermek için uygulama ağ geçidi ayarlamak için nasıl kullanılacağını gösterir.

> [!IMPORTANT]
> Http veya Https protokol öğesi büyük/küçük harf duyarlıdır.

Yeni bir yapılandırma öğesi \<araştırma\> özel araştırmalara yapılandırmak için eklenir.

Yapılandırma parametrelerini şunlardır:

|Parametre|Açıklama|
|---|---|
|**Ad** |Özel araştırma için başvuru adı. |
* **Protokolü** | Kullanılan protokol (olası değerler HTTP veya HTTPS).|
| **Ana bilgisayar** ve **yolu** | Örneğinin sistem durumunu belirlemek için uygulama ağ geçidi tarafından çağrılan tam URL yolu. Örneğin, bir Web siteniz varsa http://contoso.com/, özel araştırma için yapılandırılabilir sonra "http://contoso.com/path/custompath.htm" başarılı bir HTTP yanıt için araştırma denetimleri için.|
| **Aralık** | Yoklama aralığı denetimleri saniye olarak yapılandırır.|
| **Zaman aşımı** | Bir HTTP yanıt denetimi için yoklama zaman aşımını tanımlar.|
| **UnhealthyThreshold** | Arka uç örneği olarak işaretlemek için gereken başarısız HTTP yanıt sayısı *sağlıksız*.|

Araştırma adı başvuru \<BackendHttpSettings\> hangi arka uç havuzuna atamak için yapılandırma özel araştırma ayarlarını kullanır.

## <a name="add-a-custom-probe-to-an-existing-application-gateway"></a>Özel bir araştırma eklemek için var olan bir uygulama ağ geçidi

Geçerli bir uygulama ağ geçidi yapılandırmasını değiştirme üç adımı gerektirir: geçerli XML yapılandırma dosyası alma, özel bir araştırma olmasını değiştirin ve uygulama ağ geçidi yeni XML ayarlarla yapılandırın.

1. XML dosyasını kullanarak alma `Get-AzureApplicationGatewayConfig`. Bu cmdlet, bir yoklama ayarı eklemek için değiştirilecek XML yapılandırmasını verir.

  ```powershell
  Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path to file>"
  ```

1. XML dosyasını bir metin düzenleyicisinde açın. Ekleme bir `<probe>` sonra bölümünde `<frontendport>`.

  ```xml
<Probes>
    <Probe>
        <Name>Probe01</Name>
        <Protocol>Http</Protocol>
        <Host>contoso.com</Host>
        <Path>/path/custompath.htm</Path>
        <Interval>15</Interval>
        <Timeout>15</Timeout>
        <UnhealthyThreshold>5</UnhealthyThreshold>
    </Probe>
</Probes>
  ```

  XML backendHttpSettings bölümünde araştırma adı aşağıdaki örnekte gösterildiği gibi ekleyin:

  ```xml
    <BackendHttpSettings>
        <Name>setting1</Name>
        <Port>80</Port>
        <Protocol>Http</Protocol>
        <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        <RequestTimeout>120</RequestTimeout>
        <Probe>Probe01</Probe>
    </BackendHttpSettings>
  ```

  XML dosyasını kaydedin.

1. Kullanarak yeni bir XML dosyası ile uygulama ağ geçidi yapılandırması güncelleştirme `Set-AzureApplicationGatewayConfig`. Bu cmdlet yeni yapılandırmayı uygulama ağ geçidiniz güncelleştirir.

```powershell
Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path to file>"
```

## <a name="next-steps"></a>Sonraki adımlar

Güvenli Yuva Katmanı (SSL) yük boşaltmayı yapılandırmak istiyorsanız, bkz: [SSL yük boşaltımı için bir uygulama ağ geçidi](application-gateway-ssl.md).

İç yük dengeleyiciyle kullanacağınız uygulama ağ geçidi yapılandırmak istiyorsanız, bkz. [İç yük dengeleyici (ILB) ile uygulama ağ geçidi oluşturma](application-gateway-ilb.md).

