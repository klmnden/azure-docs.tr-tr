---
title: SSL boşaltma - Azure uygulama ağ geçidi - PowerShell Klasik yapılandırma | Microsoft Docs
description: Bu makale Azure Klasik dağıtım modelini kullanarak SSL ile bir uygulama ağ geçidi oluşturma yönergelerini boşaltma sağlar
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
ms.openlocfilehash: e620730b86d648c1ac9db7a9e6faa7a2d206b46e
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-classic-deployment-model"></a>Klasik dağıtım modeli kullanarak SSL yük boşaltımı için bir uygulama ağ geçidi yapılandırma

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Azure Klasik PowerShell](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Azure Application Gateway, web grubunda maliyetli SSL şifre çözme görevlerinin oluşmasından kaçınmak için Güvenli Yuva Katmanı (SSL) oturumunu sonlandırmak amacıyla yapılandırılabilir. SSL yük boşaltımı ön uç sunucusunun kurulumunu ve web uygulamasının yönetimini de basitleştirir.

## <a name="before-you-begin"></a>Başlamadan önce

1. Web Platformu Yükleyicisi’ni kullanarak Azure PowerShell cmdlet’lerin en son sürümünü yükleyin. **İndirmeler sayfası**’ndaki [Windows PowerShell](https://azure.microsoft.com/downloads/) bölümünden en son sürümü indirip yükleyebilirsiniz.
2. Geçerli bir alt ağla çalışan bir sanal ağa sahip olduğunuzu doğrulayın. Hiçbir sanal makinenin veya bulut dağıtımlarının alt ağı kullanmadığından emin olun. Uygulama ağ geçidi tek başına bir sanal ağ alt ağında olmalıdır.
3. Uygulama ağ geçidi kullanmak için yapılandırmanız sunucuların mevcut veya sanal ağ veya ortak IP adresi veya atanan sanal IP adresi (VIP) ile oluşturulan uç noktaları sahip olmanız gerekir.

Bir uygulama ağ geçidi üzerinde SSL yük boşaltmayı yapılandırmak için listelenen sırayla aşağıdaki adımları tamamlayın:

1. [Bir uygulama ağ geçidi oluşturma](#create-an-application-gateway)
2. [SSL sertifikalarını karşıya yükleme](#upload-ssl-certificates)
3. [Ağ geçidini yapılandırma](#configure-the-gateway)
4. [Ağ geçidi yapılandırmasını ayarlayın](#set-the-gateway-configuration)
5. [Ağ geçidini başlatma](#start-the-gateway)
6. [Ağ geçidi durumunu doğrulama](#verify-the-gateway-status)

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Ağ geçidi oluşturmak için şunu girin `New-AzureApplicationGateway` cmdlet'ini değerleri kendi değerlerinizle değiştirerek. Ağ geçidinin faturalanması bu aşamada başlamaz. Daha sonra ağ geçidi başarıyla başlatıldığında faturalama da başlar. 

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

Ağ geçidinin oluşturulduğunu, girebilir doğrulamak için `Get-AzureApplicationGateway` cmdlet'i.

Örnekte, **açıklama**, **Instancecount**, ve **GatewaySize** isteğe bağlı parametrelerdir. İçin varsayılan değer **Instancecount** olan **2**, en büyük değerini **10**. İçin varsayılan değer **GatewaySize** olan **orta**. Küçük ve büyük diğer değerleri kullanılabilir. **VirtualIPs** ve **DnsName** ağ geçidi henüz başlatılmamış olduğundan boş olarak gösterilir. Ağ geçidi çalışır duruma geldikten sonra bu değerleri oluşturulur.

```powershell
Get-AzureApplicationGateway AppGwTest
```

## <a name="upload-ssl-certificates"></a>SSL sertifikalarını karşıya yükleme

Girin `Add-AzureApplicationGatewaySslCertificate` uygulama ağ geçidi için PFX biçimi sunucu sertifikasını karşıya yüklemek için. Sertifika adı, bir kullanıcı tarafından seçilen adıdır ve uygulama ağ geçidi içinde benzersiz olmalıdır. Bu sertifika, sertifika yönetimi uygulama ağ geçidi alt işlemlerde'nda bu adla yapılan verilir.

Aşağıdaki örnek cmdlet gösterir. Örnek değerleri kendinizinkilerle değiştirin.

```powershell
Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path to pfx file>
```

Ardından, sertifika yükleme doğrulayın. Girin `Get-AzureApplicationGatewayCertificate` cmdlet'i.

Aşağıdaki örnek çıktı tarafından takip edilen ilk satırdaki cmdlet gösterir:

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
> Sertifika parolası harf veya sayı oluşan 4 ile 12 karakter arasında olmalıdır. Özel karakterleri kabul edilmedi.

## <a name="configure-the-gateway"></a>Ağ geçidini yapılandırma

Bir uygulama ağ geçidi yapılandırması birden çok değerden oluşur. Değerleri bağlı yapılandırma birlikte oluşturulamadı.

Değerler şunlardır:

* **Arka uç sunucu havuzuna**: arka uç sunucularının IP adresleri listesi. Listede bulunan IP adresleri sanal ağ alt ağına ait olması gerekir veya genel bir VIP ve IP adresi olmalıdır.
* **Arka uç sunucu havuzu ayarları**: her bir havuz bağlantı noktası, protokol ve tanımlama bilgisi temelli benzeşim gibi ayarları vardır. Bu ayarlar bir havuza bağlıdır ve havuzdaki tüm sunuculara uygulanır.
* **Ön uç bağlantı noktası**: Bu bağlantı noktası uygulama ağ geçidinde açılan genel bağlantı noktasıdır. Bu bağlantı noktasında trafik olursa arka uç sunuculardan birine yönlendirilir.
* **Dinleyici**: dinleyicisinde bir ön uç bağlantı noktası, bir iletişim kuralı kullanılır (Http veya Https; bu değerleri büyük/küçük harfe duyarlıdır) ve (SSL yük boşaltımı yapılandırıyorsanız) SSL sertifika adı.
* **Kural**: kural dinleyiciyi ve arka uç sunucusu havuzunu bağlar ve ne zaman belli bir Dinleyicide trafik trafiği yönlendirmek için hangi arka uç sunucu havuzuna tanımlar. Şu anda yalnızca *temel* kural desteklenmektedir. *Temel* kural hepsini bir kez deneme yöntemiyle yük dağıtımıdır.

**Ek yapılandırma notları**

SSL sertifikaları yapılandırmada **HttpListener**’daki protokol **Https** (küçük/büyük harf duyarlı) ile değiştirilmelidir. Ekleme **SslCert** öğesine **HttpListener** kullanılan aynı ad için ayarlanan değer ile [karşıya SSL sertifikalarını](#upload-ssl-certificates) bölümü. Ön uç bağlantı noktası için güncelleştirilmesi **443**.

**Tanımlama bilgisi temelli benzeşimi etkinleştirmek için**: bir istemci oturumundan gelen isteğin web grubunda aynı VM için her zaman yönlendirilir emin olmak için bir uygulama ağ geçidini yapılandırabilirsiniz. Bunu başarmak için ağ geçidinin trafiği uygun şekilde doğrudan bir oturum tanımlama bilgisi ekleyin. Tanımlama bilgisi temelli benzeşimi etkinleştirmek için, **CookieBasedAffinity**’yi **BackendHttpSetting** öğesindeki **Enabled**’a ayarlayın.

Bir yapılandırma nesnesi oluşturma veya bir XML yapılandırma dosyası kullanarak yapılandırmanızı oluşturabilirsiniz.
Bir XML yapılandırma dosyası kullanarak yapılandırmanızı oluşturmak için aşağıdaki örnek girin:


```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
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

Sonra, uygulama ağ geçidini kurun. Girdiğiniz `Set-AzureApplicationGatewayConfig` cmdlet'iyle bir yapılandırma nesnesi veya bir XML yapılandırma dosyası.

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml
```

## <a name="start-the-gateway"></a>Ağ geçidini başlatma

Ağ geçidi yapılandırıldıktan sonra girin `Start-AzureApplicationGateway` başlatmak için cmdlet. Uygulama ağ geçidinin faturalanması ağ geçidi başarıyla başlatıldıktan sonra başlar.

> [!NOTE]
> `Start-AzureApplicationGateway` Cmdlet tamamlanması 15-20 dakika sürebilir.
>
>

```powershell
Start-AzureApplicationGateway AppGwTest
```

## <a name="verify-the-gateway-status"></a>Ağ geçidi durumunu doğrulama

Girin `Get-AzureApplicationGateway` cmdlet'ini ağ geçidi durumunu kontrol edin. Varsa `Start-AzureApplicationGateway` önceki adımda başarılı **durumu** olmalıdır **çalıştıran**ve **VirtualIPs** ve **DnsName** gerekir Geçerli girdilere sahip.

Bu örnek, çalışır ve trafiği almaya hazır bir uygulama ağ geçidi gösterir:

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

Yük Dengeleme hakkında daha fazla bilgi için genel olarak, bkz:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
