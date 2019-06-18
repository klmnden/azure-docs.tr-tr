---
title: Özel araştırma - Azure Application Gateway - PowerShell Klasik oluştur | Microsoft Docs
description: Klasik dağıtım modelinde PowerShell kullanarak Application Gateway için özel bir araştırma oluşturmayı öğrenin
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
ms.openlocfilehash: 01c1768f60da98206f0dfd041745428256f545fc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "58861888"
---
# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a>Özel bir araştırma için Azure uygulama ağ geçidi (Klasik) PowerShell kullanarak oluşturma

> [!div class="op_single_selector"]
> * [Azure portal](application-gateway-create-probe-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-probe-ps.md)
> * [Azure Klasik PowerShell](application-gateway-create-probe-classic-ps.md)

Bu makalede PowerShell ile mevcut bir application gateway için özel bir araştırma ekleyin. Özel araştırmalar, belirli bir sistem durumu denetimi sayfası olan uygulamalar için veya başarılı bir yanıt varsayılan web uygulaması üzerinde sağlamayan uygulamalar için yararlıdır.

> [!IMPORTANT]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. [Bu adımları Resource Manager modeli kullanarak gerçekleştirmeyi](application-gateway-create-probe-ps.md) öğrenin.

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
> *InstanceCount* için varsayılan değer 2 ile 10 arasıdır. *GatewaySize* için varsayılan değer Medium’dur. Küçük, Orta ve büyük arasında seçebilirsiniz.
> 
> 

Ağ geçidi daha başlatılmadığından dolayı *VirtualIPs* ve *DnsName* boş görünür. Bu değerler, ağ geçidi çalışma durumuna geçtiğinde oluşturulur.

### <a name="configure-an-application-gateway-by-using-xml"></a>XML kullanarak uygulama ağ geçidi yapılandırma

Aşağıdaki örnekte, tüm uygulama ağ geçidi ayarlarını yapılandırmak için bir XML dosyası kullanır ve bu ayarları uygulama ağ geçidi kaynağına uygularsınız.  

Aşağıdaki metni Notepad’a kopyalayın.

```xml
<ApplicationGatewayConfiguration xmlns:i="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
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

Aşağıdaki örnek genel bağlantı noktası 80 üzerinde HTTP trafiği Yük Dengelemesi ve özel bir araştırma kullanarak iki IP adresi arasındaki arka uç bağlantı noktası 80 ağ trafiği göndermek için uygulama ağ geçidini ayarlamak için bir yapılandırma dosyası kullanmayı gösterir.

> [!IMPORTANT]
> Http veya Https protokol öğesi büyük/küçük harf duyarlıdır.

Yeni bir yapılandırma öğesi \<araştırma\> özel araştırmalarını yapılandırma eklenir.

Yapılandırma parametreleri şunlardır:

|Parametre|Açıklama|
|---|---|
|**Ad** |Özel araştırma için başvuru adı. |
| **Protokolü** | Kullanılan protokol (olası değerler: HTTP veya HTTPS).|
| **Konak** ve **yolu** | Örneğinin durumunu belirlemek için uygulama ağ geçidi tarafından çağrılan tam URL yolu. Örneğin, bir Web sitesi http varsa:\//için contoso.com/ sonra özel araştırma yapılandırılabilir "http:\//contoso.com/path/custompath.htm" başarılı HTTP yanıt için araştırma denetimleri için.|
| **Aralık** | Saniye cinsinden yoklama aralığı denetimleri yapılandırır.|
| **zaman aşımı** | Bir HTTP yanıt denetimi için yoklama zaman aşımı tanımlar.|
| **UnhealthyThreshold** | Başarısız HTTP yanıtlarını arka uç örnek olarak işaretlemek için gereken sayıda *sağlıksız*.|

Araştırma adı başvurulduğundan \<BackendHttpSettings\> hangi arka uç havuzuna atamak için yapılandırma özel araştırma ayarlarını kullanır.

## <a name="add-a-custom-probe-to-an-existing-application-gateway"></a>Var olan bir uygulama ağ geçidi için özel bir araştırma Ekle

Geçerli bir uygulama ağ geçidi yapılandırmasını değiştirme üç adımı gerektirir: Uygulama ağ geçidinin yeni XML ayarlarla yapılandırın ve özel bir araştırma için değiştirebilir veya geçerli XML yapılandırma dosyasını alın.

1. XML dosyasını almak `Get-AzureApplicationGatewayConfig`. Bu cmdlet, bir yoklama ayarı eklemek için değiştirilecek XML yapılandırmasını dışarı aktarır.

   ```powershell
   Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path to file>"
   ```

1. XML dosyasını bir metin düzenleyicisinde açın. Ekleme bir `<probe>` sonra bölüm `<frontendport>`.

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

1. Kullanarak uygulama ağ geçidi yapılandırması ile yeni XML dosyasını güncelleştirin `Set-AzureApplicationGatewayConfig`. Bu cmdlet, application gateway'iniz yeni yapılandırmayla güncelleştirir.

```powershell
Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path to file>"
```

## <a name="next-steps"></a>Sonraki adımlar

Güvenli Yuva Katmanı (SSL) boşaltma yapılandırmak istiyorsanız, bkz. [SSL yük boşaltımı için bir uygulama ağ geçidi](application-gateway-ssl.md).

İç yük dengeleyiciyle kullanacağınız uygulama ağ geçidi yapılandırmak istiyorsanız, bkz. [İç yük dengeleyici (ILB) ile uygulama ağ geçidi oluşturma](application-gateway-ilb.md).

