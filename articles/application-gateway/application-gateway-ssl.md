---
title: Yapılandırma SSL yük boşaltma - Azure Application Gateway - PowerShell Klasik | Microsoft Docs
description: Bu makalede Azure Klasik dağıtım modelini kullanarak SSL ile bir uygulama ağ geçidi oluşturma yönergelerini boşaltma sağlar.
documentationcenter: na
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.assetid: 63f28d96-9c47-410e-97dd-f5ca1ad1b8a4
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: victorh
ms.openlocfilehash: 89a88d79b6b93a233dbd4f335d0eb449e49d5289
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62122209"
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-classic-deployment-model"></a>Klasik dağıtım modelini kullanarak SSL yük boşaltımı için uygulama ağ geçidi yapılandırma

> [!div class="op_single_selector"]
> * [Azure portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Azure Klasik PowerShell](application-gateway-ssl.md)
> * [Azure CLI](application-gateway-ssl-cli.md)

Azure Application Gateway, web grubunda maliyetli SSL şifre çözme görevlerinin oluşmasından kaçınmak için Güvenli Yuva Katmanı (SSL) oturumunu sonlandırmak amacıyla yapılandırılabilir. SSL yük boşaltımı ön uç sunucusunun kurulumunu ve web uygulamasının yönetimini de basitleştirir.

## <a name="before-you-begin"></a>Başlamadan önce

1. Web Platformu Yükleyicisi’ni kullanarak Azure PowerShell cmdlet’lerin en son sürümünü yükleyin. **İndirmeler sayfası**’ndaki [Windows PowerShell](https://azure.microsoft.com/downloads/) bölümünden en son sürümü indirip yükleyebilirsiniz.
2. Geçerli bir alt ağla çalışan bir sanal ağa sahip olduğunuzu doğrulayın. Hiçbir sanal makinenin veya bulut dağıtımlarının alt ağı kullanmadığından emin olun. Uygulama ağ geçidi tek başına bir sanal ağ alt ağında olmalıdır.
3. Uygulama ağ geçidi kullanırken yapılandırdığınız sunucular mevcut veya uç noktaları sanal ağda veya bir genel IP adresi veya atanan sanal IP adresi (VIP) ile oluşturulmuş olması gerekir.

Bir application gateway üzerinde SSL yük boşaltmayı yapılandırmak için aşağıdaki adımları listelendikleri sırada tamamlayın:

1. [Bir uygulama ağ geçidi oluşturma](#create-an-application-gateway)
2. [SSL sertifikalarını karşıya yükleme](#upload-ssl-certificates)
3. [Ağ geçidini yapılandırma](#configure-the-gateway)
4. [Ağ geçidi yapılandırmasını ayarlayın](#set-the-gateway-configuration)
5. [Ağ geçidini başlatma](#start-the-gateway)
6. [Ağ geçidi durumunu doğrulama](#verify-the-gateway-status)

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Ağ geçidi oluşturmak için girin `New-AzureApplicationGateway` cmdlet'ini değerleri kendi değerlerinizle değiştirin. Ağ geçidinin faturalanması bu aşamada başlamaz. Daha sonra ağ geçidi başarıyla başlatıldığında faturalama da başlar. 

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

Ağ geçidinin oluşturulduğunu, girdiğiniz doğrulamak için `Get-AzureApplicationGateway` cmdlet'i.

Örnekte, **açıklama**, **Instancecount**, ve **GatewaySize** isteğe bağlı parametrelerdir. İçin varsayılan değer **Instancecount** olduğu **2**, maksimum değerini **10**. İçin varsayılan değer **GatewaySize** olduğu **orta**. Küçük ve büyük diğer değerleri kullanılabilir. **Virtualıps** ve **DnsName** ağ geçidi henüz başlatılmamış olduğundan boş olarak gösterilir. Bu değerler, ağ geçidinin çalışır durumda olduktan sonra oluşturulur.

```powershell
Get-AzureApplicationGateway AppGwTest
```

## <a name="upload-ssl-certificates"></a>SSL sertifikalarını karşıya yükleme

Girin `Add-AzureApplicationGatewaySslCertificate` uygulama ağ geçidi sunucusu sertifikayı PFX biçiminde yüklenecek. Sertifika adı kullanıcı tarafından seçilen adıdır ve uygulama ağ geçidi içinde benzersiz olmalıdır. Bu sertifikayı tüm sertifika yönetimi işlemleri uygulama ağ geçidi üzerinde bu ada göre adlandırılır.

Aşağıdaki örnek cmdlet gösterir. Örnek değerleri kendi değerlerinizle değiştirin.

```powershell
Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path to pfx file>
```

Ardından, sertifika karşıya yüklemeyi doğrulayın. Girin `Get-AzureApplicationGatewayCertificate` cmdlet'i.

Aşağıdaki örnek cmdlet çıktı tarafından takip ilk satırdaki gösterir:

```powershell
Get-AzureApplicationGatewaySslCertificate AppGwTest
```

```
VERBOSE: 5:07:54 PM - Begin Operation: Get-AzureApplicationGatewaySslCertificate
VERBOSE: 5:07:55 PM - Completed Operation: Get-AzureApplicationGatewaySslCertificate
Name           : SslCert
SubjectName    : CN=gwcert.app.test.contoso.com
Thumbprint     : AF5ADD77E160A01A6......EE48D1A
ThumbprintAlgo : sha1RSA
State..........: Provisioned
```

> [!NOTE]
> Sertifika parolası, harf veya sayı oluşan 4 ila 12 karakter arasında olmalıdır. Özel karakterler kabul edilmez.

## <a name="configure-the-gateway"></a>Ağ geçidini yapılandırma

Bir uygulama ağ geçidi yapılandırması birden çok değerden oluşur. Değerleri bağlı birlikte yapılandırmasını oluşturmak için.

Değerler şunlardır:

* **Arka uç sunucu havuzu**: Arka uç sunucularının IP adresleri listesi. Listede bulunan IP adresleri, sanal ağ alt ağına ait olmalıdır veya genel bir VIP ve IP adresi olmalıdır.
* **Arka uç sunucu havuzu ayarları**: Her havuzun bağlantı noktası, protokol ve tanımlama bilgisi temelli benzeşim gibi ayarları vardır. Bu ayarlar bir havuza bağlıdır ve havuzdaki tüm sunuculara uygulanır.
* **Ön uç bağlantı noktası**: Bu bağlantı noktası uygulama ağ geçidinde açılan genel bağlantı noktasıdır. Bu bağlantı noktasında trafik olursa arka uç sunuculardan birine yönlendirilir.
* **Dinleyici**: Dinleyicinin sahip bir ön uç bağlantı noktası, bir protokol (Http veya Https; bu değerler büyük/küçük harfe duyarlıdır) ve SSL sertifika adı (yapılandırma bir SSL yük boşaltımı yapılandırılıyorsa).
* **Kural**: Kural, dinleyiciyi ve arka uç sunucusu havuzunu bağlar ve ne zaman bir Dinleyicide denk gelir trafiği yönlendirmek için hangi arka uç sunucu havuzuna yönlendirileceğini belirler. Şu anda yalnızca *temel* kural desteklenmektedir. *Temel* kural hepsini bir kez deneme yöntemiyle yük dağıtımıdır.

**Ek yapılandırma notları**

SSL sertifikaları yapılandırmada **HttpListener**’daki protokol **Https** (küçük/büyük harf duyarlı) ile değiştirilmelidir. Ekleme **SslCert** öğesine **HttpListener** kullanılan ile aynı ad ayarlanan değer ile [karşıya SSL sertifikaları](#upload-ssl-certificates) bölümü. Ön uç bağlantı noktasıyla güncelleştirilmelidir **443**.

**Tanımlama bilgisi temelli benzeşimi etkinleştirmek için**: Bir istemci oturumundan gelen bir isteğin web grubunda aynı VM'e her zaman yönlendirildiğinden emin olmak için bir uygulama ağ geçidi yapılandırabilirsiniz. Bunu gerçekleştirmek için ağ geçidinin trafiği uygun bir şekilde yönlendirmesini sağlayacak oturum tanımlama bilgisinin ekleyin. Tanımlama bilgisi temelli benzeşimi etkinleştirmek için, **CookieBasedAffinity**’yi **BackendHttpSetting** öğesindeki **Enabled**’a ayarlayın.

Yapılandırma, bir yapılandırma nesnesi oluşturma veya yapılandırma XML dosyasını kullanarak oluşturabilirsiniz.
Bir yapılandırma XML dosyasını kullanarak yapılandırmanızı oluşturmak için aşağıdaki örnek girin:


```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations />
    <FrontendPorts>
        <FrontendPort>
            <Name>FrontendPort1</Name>
            <Port>443</Port>
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
            <Protocol>Https</Protocol>
            <SslCert>GWCert</SslCert>
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

Sonra, uygulama ağ geçidini kurun. Girdiğiniz `Set-AzureApplicationGatewayConfig` cmdlet'i bir yapılandırma nesnesi veya bir yapılandırma XML dosyası.

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml
```

## <a name="start-the-gateway"></a>Ağ geçidini başlatma

Ağ geçidi yapılandırıldıktan sonra girin `Start-AzureApplicationGateway` başlatmak için cmdlet. Uygulama ağ geçidinin faturalanması ağ geçidi başarıyla başlatıldıktan sonra başlar.

> [!NOTE]
> `Start-AzureApplicationGateway` Cmdlet'in bitmesi 15-20 dakika sürebilir.
>
>

```powershell
Start-AzureApplicationGateway AppGwTest
```

## <a name="verify-the-gateway-status"></a>Ağ geçidi durumunu doğrulama

Girin `Get-AzureApplicationGateway` ağ geçidinin durumunu denetlemek için cmdlet'i. Varsa `Start-AzureApplicationGateway` önceki adımda başarılı **durumu** olmalıdır **çalıştıran**ve **Virtualıps** ve **DnsName** gerekir Geçerli girdilere sahip.

Bu örnek, çalışan ve trafiği almaya hazır olan bir uygulama ağ geçidi gösterir:

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
Name          : AppGwTest2
Description   :
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Running
VirtualIPs    : {23.96.22.241}
DnsName       : appgw-4c960426-d1e6-4aae-8670-81fd7a519a43.cloudapp.net
```

## <a name="next-steps"></a>Sonraki adımlar

Yük Dengeleme hakkında daha fazla bilgi için bkz: genel olarak, seçenekleri:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
