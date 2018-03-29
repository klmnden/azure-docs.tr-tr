---
title: Azure uygulama ağ geçidi ile uçtan uca SSL yapılandırma
description: Bu makalede PowerShell kullanarak Azure uygulama ağ geçidi ile uçtan uca SSL yapılandırma
services: application-gateway
documentationcenter: na
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 3/27/2018
ms.author: victorh
ms.openlocfilehash: 2de7086d7c26d5a655ad5998678f392126ea7e1d
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-end-to-end-ssl-by-using-application-gateway-with-powershell"></a>PowerShell ile uygulama ağ geçidi kullanarak uçtan uca SSL yapılandırma

## <a name="overview"></a>Genel Bakış

Azure uygulama ağ geçidi uçtan uca şifreleme trafiği destekler. Uygulama ağ geçidi uygulama ağ geçidi SSL bağlantısını sonlandırır. Ağ geçidi ardından yönlendirme kuralları trafiği için geçerlidir, paketin yeniden şifreler ve tanımlanan yönlendirme kurallarına göre uygun arka uç sunucusuna iletir. Web sunucusundan alınan herhangi bir yanıt, son kullanıcıya dönerken aynı süreci izler.

Uygulama ağ geçidi özel SSL seçenekleri tanımlama destekler. Ayrıca aşağıdaki protokol sürümleri devre dışı bırakma destekler: **TLSv1.0**, **TLSv1.1**, ve **TLSv1.2**, yanı kullanmak için hangi şifre paketleri ve tercih sırasına göre tanımlama . Yapılandırılabilir SSL seçenekleri hakkında daha fazla bilgi için bkz: [SSL ilkesine genel bakış](application-gateway-SSL-policy-overview.md).

> [!NOTE]
> SSL 2.0 ve SSL 3.0 varsayılan olarak devre dışıdır ve etkinleştirilemez. Bunlar Güvensiz olarak kabul edilir ve uygulama ağ geçidi ile kullanılamaz.

![Senaryo görüntüsü][scenario]

## <a name="scenario"></a>Senaryo

Bu senaryoda, bir uygulama ağ geçidi PowerShell ile uçtan uca SSL kullanarak oluşturmayı öğrenin.

Bu senaryo aşağıdakileri yapar:

* Adlı bir kaynak grubu oluşturmak **appgw-rg**.
* Adlı bir sanal ağ oluşturma **appgwvnet** bir adres alanı ile **10.0.0.0/16**.
* Adlı iki alt ağ oluşturmak **appgwsubnet** ve **appsubnet**.
* Bir küçük uygulama ağ geçidi destek uçtan uca SSL şifrelemesi bu sınırları SSL protokol sürümleri ve şifre paketleri oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

Bir uygulama ağ geçidi ile uçtan uca SSL'yi yapılandırmak için bir sertifika ağ geçidi için gereklidir ve sertifikaları arka uç sunucuları için gereklidir. Ağ geçidi sertifikası şifrelemek ve şifresini çözmek için SSL üzerinden gönderilen trafiği için kullanılır. Ağ geçidi sertifikası kişisel bilgi değişimi (PFX) biçiminde olması gerekir. Bu dosya biçimi, uygulama ağ geçidi tarafından şifreleme ve şifre çözme trafik gerçekleştirmek için gereken özel anahtarı dışarı olanak sağlar.

Uçtan uca SSL şifrelemesi için arka uç uygulama ağ geçidi ile güvenilir listesinde olması gerekir. Uygulama ağ geçidi arka uç sunucularının ortak sertifika karşıya gerekir. Sertifika ekleniyor uygulama ağ geçidi ile bilinen arka uç örnekleri yalnızca iletişim kurar sağlar. Bu ek uçtan uca iletişimin güvenliğini sağlar.

Yapılandırma işlemi aşağıdaki bölümlerde açıklanmıştır.

## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

Bu bölümde uygulama ağ geçidi içeren bir kaynak grubu oluşturmada size yol gösterir.


   1. Azure hesabınızda oturum açın.

   ```powershell
   Login-AzureRmAccount
   ```


   2. Bu senaryo için kullanılacak aboneliği seçin.

   ```powershell
   Select-AzureRmsubscription -SubscriptionName "<Subscription name>"
   ```


   3. Bir kaynak grubu oluşturun. (Mevcut bir kaynak grubunu kullanıyorsanız bu adımı atlayın.)

   ```powershell
   New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
   ```

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Uygulama ağ geçidi için bir sanal ağ ve bir alt ağ oluştur

Aşağıdaki örnek, bir sanal ağ ve iki alt ağı oluşturur. Bir alt ağ, uygulama ağ geçidi tutmak için kullanılır. Diğer alt web uygulamasını barındırmak arka uçları için kullanılır.


   1. Uygulama ağ geçidi için kullanılan alt ağ için bir adres aralığı atayın.

   ```powershell
   $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24
   ```

   > [!NOTE]
   > Bir uygulama ağ geçidi için yapılandırılmış alt ağlar doğru şekilde boyutlandırılmalıdır. Bir uygulama ağ geçidi için en fazla 10 örneğini yapılandırılabilir. Her örneğin bir IP adresi alt ağdan alır. Çok küçük bir alt ağın, bir uygulama ağ geçidi ölçeklendirme olumsuz yönde etkileyebilir.
   > 
   > 

   2. Arka uç adres havuzu için kullanılacak bir adres aralığı atayın.

   ```powershell
   $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24
   ```

   3. Önceki adımlarda tanımlanan alt ağları ile bir sanal ağ oluşturun.

   ```powershell
   $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
   ```

   4. Aşağıdaki adımlarda kullanılacak alt ağ kaynaklarının ve sanal ağ kaynağı alır.

   ```powershell
   $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
   $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
   $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet
   ```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Ön uç yapılandırma için genel bir IP adresi oluşturun

Uygulama ağ geçidi için kullanılacak genel bir IP kaynağı oluşturun. Bu genel IP adresi adımları birinde kullanılır.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic
```

> [!IMPORTANT]
> Uygulama ağ geçidi tanımlanan etki alanı etiketi ile oluşturulan bir ortak IP adresi kullanımını desteklemiyor. Yalnızca bir ortak IP adresi dinamik olarak oluşturulan etki alanı etiketi ile desteklenir. Uygulama ağ geçidi için kolay bir DNS adı gerekiyorsa, bir diğer ad olarak bir CNAME kaydı kullanmanızı öneririz.

## <a name="create-an-application-gateway-configuration-object"></a>Uygulama ağ geçidi yapılandırma nesnesi oluşturun

Tüm yapılandırma öğeleri, uygulama ağ geçidi oluşturmadan önce ayarlanır. Aşağıdaki adımlar uygulama ağ geçidi kaynağı için gerekli yapılandırma öğelerini oluşturur.

   1. Uygulama ağ geçidi IP yapılandırması oluşturun. Bu ayar, uygulama ağ geçidini kullanan alt yapılandırır. Uygulama ağ geçidi başladığında, yapılandırılan alt ağdan bir IP adresi seçer ve ağ trafiği arka uç IP havuzundaki IP adreslerine yollar. Her örneğin bir IP adresi aldığını göz önünde bulundurun.

   ```powershell
   $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet
   ```


   2. Bir ön uç IP yapılandırmasını oluşturun. Bu ayar, uygulama ağ geçidi ön ucu için bir özel veya genel IP adresi eşler. Aşağıdaki adım önceki adımda genel IP adresi ön uç IP yapılandırmasını ilişkilendirir.

   ```powershell
   $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip
   ```

   3. Arka uç IP adresi havuzu ile arka uç web sunucularının IP adreslerini yapılandırın. Bu IP adresleri ön uç IP uç noktasından gelen ağ trafiğinin yönlendirildiği IP adresleridir. Örnek IP adreslerini kendi uygulama IP adresi uç ile değiştirin.

   ```powershell
   $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3
   ```

   > [!NOTE]
   > Bir tam etki alanı adı (FQDN) de arka uç sunucuları için bir IP adresi yerine kullanılacak geçerli bir değer değil. Kullanarak etkinleştirin **- BackendFqdns** geçin. 


   4. Genel IP uç noktası için ön uç IP bağlantı noktası yapılandırın. Bu bağlantı noktası, son kullanıcılara bağlanan bağlantı noktasıdır.

   ```powershell
   $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443
   ```

   5. Uygulama ağ geçidi için sertifika yapılandırın. Bu sertifika, uygulama ağ geçidi trafiğinde reencrypt ve şifresini çözmek için kullanılır.

   ```powershell
   $password = ConvertTo-SecureString  <password for certificate file> -AsPlainText -Force 
   $cert = New-AzureRmApplicationGatewaySSLCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password $password 
   ```

   > [!NOTE]
   > Bu örnek, SSL bağlantısı için kullanılan sertifikayı yapılandırır. Sertifikanın .pfx formatında olması gerekir ve parola 4 ile 12 karakter olmalıdır.

   6. HTTP dinleyicisi için uygulama ağ geçidi oluşturun. Ön uç IP yapılandırmasını, bağlantı noktası ve kullanmak için SSL sertifikası atayın.

   ```powershell
   $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SSLCertificate $cert
   ```

   7. SSL etkin arka uç havuzu kaynaklardaki kullanılan sertifikayı karşıya yükleyin.

   > [!NOTE]
   > Ortak anahtarı ile varsayılan araştırmasını alır *varsayılan* SSL bağlaması arka uç bilgisayarın IP adresi ve aldığı ortak anahtar değeri, ortak anahtar değeri sağlamak burada karşılaştırır. 
   
   > Arka uçta ana bilgisayar üstbilgilerinin ve sunucu adı göstergesi (SNI) kullanıyorsanız, alınan ortak anahtar hangi trafik akışları hedeflenen siteye olmayabilir. Şüpheli değilseniz, ziyaret https://127.0.0.1/ için kullanılan hangi sertifikanın onaylamak için arka uç sunucularda *varsayılan* SSL bağlaması. Bu bölümde bu istek ortak anahtarı kullanın. Ana bilgisayar üstbilgilerinin ve SNI HTTPS bağlantılarına kullanıyorsanız ve bir yanıt ve sertifika için bir el ile tarayıcı istekten aldığınız değil https://127.0.0.1/ arka uç sunucularda, bunları varsayılan SSL bağlamada ayarlamanız gerekir. Bunu yaparsanız, araştırmalar başarısız ve arka uç izin verilenler listesinde değil.

   ```powershell
   $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile C:\users\gwallace\Desktop\cert.cer
   ```

   > [!NOTE]
   > Bu adımda sağlanan sertifika arka uçta mevcut .pfx sertifika ortak anahtarı olması gerekir. Talep, kanıt ve akıl (CER) biçiminde arka uç sunucuda yüklü sertifikayı (kök sertifikanın değil) verin ve bu adımda kullanın. Bu adım whitelists arka uç uygulama ağ geçidi ile.

   8. Uygulama ağ geçidi arka uç HTTP ayarları yapılandırın. HTTP ayarları için önceki adımda yüklediğiniz sertifikayı atayın.

   ```powershell
   $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert
   ```
   9. Yük Dengeleyici davranışını yapılandıran bir Yük Dengeleyiciyi yönlendirme kuralını oluşturun. Bu örnekte, temel bir hepsini kural oluşturulur.

   ```powershell
   $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
   ```

   10. Uygulama ağ geçidinin örnek boyutunu yapılandırın. Kullanılabilir boyutları: **standart\_küçük**, **standart\_orta**, ve **standart\_büyük**.  Kapasite için kullanılabilen değerlerdir **1** aracılığıyla **10**.

   ```powershell
   $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
   ```

   > [!NOTE]
   > Test amacıyla örnek sayısını 1 seçilebilir. Tüm örnek sayısı iki örneği altında SLA kapsamında değildir ve bu nedenle önerilmez bilmeniz önemlidir. Küçük ağ geçitleri geliştirme test ve üretim amaçları için kullanılacak olan.

   11. Uygulama ağ geçidinde kullanılacak SSL ilkesini yapılandırın. Uygulama ağ geçidi SSL protokol sürümleri için en düşük sürüm ayarlama özelliği destekler.

   Aşağıdaki değerleri tanımlanabilir protokol sürümleri listesi verilmiştir:

    - **TLSV1_0**
    - **TLSV1_1**
    - **TLSV1_2**
    
   Aşağıdaki örnek, en düşük protokol sürümü ayarlar **TLSv1_2** ve sağlar **TLS\_ECDHE\_ECDSA\_ile\_AES\_128\_GCM\_SHA256**, **TLS\_ECDHE\_ECDSA\_ile\_AES\_256\_GCM\_SHA384**, ve **TLS\_RSA\_ile\_AES\_128\_GCM\_SHA256** yalnızca.

   ```powershell
   $SSLPolicy = New-AzureRmApplicationGatewaySSLPolicy -MinProtocolVersion TLSv1_2 -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256"
   ```

## <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

Yukarıdaki adımları kullanarak uygulama ağ geçidi oluşturun. Ağ geçidi oluşturmayı çalıştırmak için uzun süren bir işlemdir.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgateway -SSLCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SSLPolicy $SSLPolicy -AuthenticationCertificates $authcert -Verbose
```

## <a name="limit-ssl-protocol-versions-on-an-existing-application-gateway"></a>Mevcut bir uygulama ağ geçidi SSL protokolü sürümlerinde sınırla

Yukarıdaki adımları uçtan uca SSL ile uygulama oluşturma ve belirli SSL protokol sürümleri devre dışı bırakma sürdü. Aşağıdaki örnekte belirli SSL ilkeleri var olan bir uygulama ağ geçidi üzerinde devre dışı bırakır.

   1. Uygulama ağ geçidi güncelleştirilecek alın.

   ```powershell
   $gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG
   ```

   2. SSL ilke tanımlayın. Aşağıdaki örnekte, **TLSv1.0** ve **TLSv1.1** devre dışı bırakıldı ve şifre paketleri **TLS\_ECDHE\_ECDSA\_ile\_ AES\_128\_GCM\_SHA256**, **TLS\_ECDHE\_ECDSA\_ile\_AES\_256\_GCM\_SHA384**, ve **TLS\_RSA\_ile\_AES\_128\_GCM\_SHA256** yalnızca olan olanları izin verilir.

   ```powershell
   Set-AzureRmApplicationGatewaySSLPolicy -MinProtocolVersion -PolicyType Custom -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256" -ApplicationGateway $gw

   ```

   3. Son olarak, ağ geçidini güncelleştirin. Bu son adım uzun süre çalışan bir görev olduğuna dikkat edin. Tamamlandığında, uçtan uca SSL uygulama ağ geçidinde yapılandırılır.

   ```powershell
   $gw | Set-AzureRmApplicationGateway
   ```

## <a name="get-an-application-gateway-dns-name"></a>Bir uygulama ağ geçidi DNS adını Al

Ağ geçidi oluşturulduktan sonra sonraki adıma iletişimi için ön uç yapılandırmaktır. Uygulama ağ geçidi, bir ortak IP kolay olmayan kullanırken dinamik olarak atanmış bir DNS adı gerektirir. Son kullanıcıların uygulama ağ geçidi isabet emin olmak için uygulama ağ geçidi için ortak uç noktası için bir CNAME kaydı kullanabilirsiniz. Daha fazla bilgi için bkz: [Azure'da bir özel etki alanı adı yapılandırma](../cloud-services/cloud-services-custom-domain-name-portal.md). 

Bir diğer ad yapılandırmak için kullanarak uygulama ağ geçidi ve ilişkili IP/DNS adı ayrıntılarını almak **Publicıpaddress** öğesi uygulama ağ geçidine bağlı. İki web uygulamaları bu DNS adına işaret eden bir CNAME kaydı oluşturmak için uygulama ağ geçidi DNS adını kullanın. Biz A kayıtlarını kullanılmasını öneririz, VIP değiştirebilirsiniz olmadığından, uygulama ağ geçidi yeniden yok.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
```

```
Name                     : publicIP01
ResourceGroupName        : appgw-RG
Location                 : westus
Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
ResourceGuid             : 00000000-0000-0000-0000-000000000000
ProvisioningState        : Succeeded
Tags                     : 
PublicIpAllocationMethod : Dynamic
IpAddress                : xx.xx.xxx.xx
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                                "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                            Configurations/frontend1"
                            }
DnsSettings              : {
                                "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                            }
```

## <a name="next-steps"></a>Sonraki adımlar

Uygulama ağ geçidi üzerinden Web uygulaması güvenlik duvarı ile web uygulamalarınızın güvenliğini artırma hakkında daha fazla bilgi için bkz: [Web uygulaması güvenlik duvarı genel bakış](application-gateway-webapplicationfirewall-overview.md).

[scenario]: ./media/application-gateway-end-to-end-SSL-powershell/scenario.png
