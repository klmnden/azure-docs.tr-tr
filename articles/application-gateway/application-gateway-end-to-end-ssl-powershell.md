---
title: Azure Application Gateway ile uçtan uca SSL'yi yapılandırma
description: Bu makalede PowerShell kullanarak Azure Application Gateway ile uçtan uca SSL'yi yapılandırma
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 4/8/2019
ms.author: victorh
ms.openlocfilehash: 8c715cb84dff6e2e739de59aba33041ec1b8db52
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65786277"
---
# <a name="configure-end-to-end-ssl-by-using-application-gateway-with-powershell"></a>Application Gateway ve PowerShell kullanarak uçtan uca SSL'yi yapılandırma

## <a name="overview"></a>Genel Bakış

Azure Application Gateway, trafiğin uçtan uca şifrelenmesini destekler. Uygulama ağ geçidi, application Gateway SSL bağlantısını sonlandırır. Ağ geçidini daha sonra yönlendirme kurallarını trafiğe uygular, paketi yeniden şifreler ve tanımlanan yönlendirme kurallarına göre uygun arka uç sunucusuna iletir. Web sunucusundan alınan herhangi bir yanıt, son kullanıcıya dönerken aynı süreci izler.

Application Gateway, özel SSL seçeneklerini tanımlama destekler. Ayrıca, aşağıdaki protokol sürümleri devre dışı bırakmayı destekler: **TLSv1.0**, **TLSv1.1**, ve **TLSv1.2**, ayrıca kullanmak için hangi şifre paketleri ve tercih sırasına göre tanımlama. Yapılandırılabilir SSL seçenekleri hakkında daha fazla bilgi edinmek için [SSL ilkesine genel bakış](application-gateway-SSL-policy-overview.md).

> [!NOTE]
> SSL 2.0 ve SSL 3.0 varsayılan olarak devre dışıdır ve etkinleştirilemez. Bunlar, güvenli olarak kabul edilir ve Application Gateway ile kullanılamaz.

![Senaryo görüntüsü][scenario]

## <a name="scenario"></a>Senaryo

Bu senaryoda, uçtan uca SSL ve PowerShell kullanarak application gateway oluşturmayı öğrenin.

Bu senaryo olur:

* Adlı bir kaynak grubu oluşturma **appgw-rg**.
* Adlı bir sanal ağ oluşturma **appgwvnet** bir adres alanı ile **10.0.0.0/16**.
* Adlı iki alt ağa oluşturma **appgwsubnet** ve **appsubnet**.
* Bir kısa uygulama ağ geçidi destekleyici uçtan uca SSL şifrelemesini bu sınırları SSL protokolü sürümlerini ve şifre paketleri oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bir uygulama ağ geçidi ile uçtan uca SSL'yi yapılandırmak için bir sertifika ağ geçidi için gereklidir ve arka uç sunucuları için sertifikaları gereklidir. Ağ geçidi sertifikası, bir simetrik anahtar SSL protokolü belirtimi uyarınca türetmek için kullanılır. Ardından kullanılan simetrik anahtarı şifrelemek ve şifresini çözmek için ağ geçidi gönderilen trafik. Ağ geçidi sertifikası kişisel bilgi değişimi (PFX) biçiminde olması gerekir. Bu dosya biçimi, uygulama ağ geçidi tarafından şifreleme ve şifre çözme trafik gerçekleştirmek için gerekli olan özel anahtarı dışarı olanak tanır.

Uçtan uca SSL şifrelemesi için arka uç uygulama ağ geçidiyle Güvenilenler listesinde olmalıdır. Ortak sertifikayı arka uç sunucuları uygulama ağ geçidine yükleyin. Sertifika ekleme, uygulama ağ geçidi yalnızca bilinen arka uç örnekleriyle iletişim sağlar. Bu daha fazla uçtan uca iletişimin güvenliğini sağlar.

Yapılandırma işlemini aşağıdaki bölümlerde açıklanmıştır.

## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

Bu bölüm, uygulama ağ geçidi içeren bir kaynak grubu oluşturma işleminde size yol gösterir.

1. Azure hesabınızda oturum açın.

   ```powershell
   Connect-AzAccount
   ```

2. Bu senaryo için kullanılacak aboneliği seçin.

   ```powershell
   Select-Azsubscription -SubscriptionName "<Subscription name>"
   ```

3. Bir kaynak grubu oluşturun. (Mevcut bir kaynak grubunu kullanıyorsanız bu adımı atlayın.)

   ```powershell
   New-AzResourceGroup -Name appgw-rg -Location "West US"
   ```

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Uygulama ağ geçidi için bir sanal ağ ve bir alt ağ oluştur

Aşağıdaki örnek, bir sanal ağ ve iki alt ağ oluşturur. Bir alt ağ, uygulama ağ geçidi tutmak için kullanılır. Diğer alt ağı, web uygulamasını barındıran arka uçları için kullanılır.

1. Application gateway için kullanılacak alt ağ adres aralığı atayın.

   ```powershell
   $gwSubnet = New-AzVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24
   ```

   > [!NOTE]
   > Bir uygulama ağ geçidi için yapılandırılmış alt ağlar düzgün şekilde boyutlandırılmalıdır. Bir uygulama ağ geçidi en fazla 10 örnekleri için yapılandırılabilir. Her örnek, alt ağdan bir IP adresi alır. Çok küçük bir alt ağın, bir uygulama ağ geçidi ölçeklendirme olumsuz yönde etkileyebilir.
   >

2. Arka uç adres havuzu için kullanılacak bir adres aralığı atayın.

   ```powershell
   $nicSubnet = New-AzVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24
   ```

3. Önceki adımlarda tanımlanan alt ağları ile sanal ağ oluşturun.

   ```powershell
   $vnet = New-AzvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
   ```

4. Sonraki adımlarda kullanılacak alt ağ kaynaklarının ve sanal ağ kaynağı alın.

   ```powershell
   $vnet = Get-AzvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
   $gwSubnet = Get-AzVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
   $nicSubnet = Get-AzVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet
   ```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Ön uç yapılandırma için genel bir IP adresi oluşturun

Application gateway için kullanılacak bir genel IP kaynağı oluşturun. Bu genel IP adresi, aşağıdaki adımları biri kullanılır.

```powershell
$publicip = New-AzPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic
```

> [!IMPORTANT]
> Uygulama ağ geçidi bir tanımlanmış etki alanı etiketi ile oluşturulmuş bir genel IP adresi kullanımını desteklemiyor. Yalnızca genel IP adresi dinamik olarak oluşturulan etki alanı etiketi ile desteklenir. Uygulama ağ geçidi için kolay bir DNS adı gerekiyorsa, diğer ad olarak bir CNAME kaydı kullanmanızı öneririz.

## <a name="create-an-application-gateway-configuration-object"></a>Uygulama ağ geçidi yapılandırma nesnesi oluşturun

Tüm yapılandırma öğeleri, uygulama ağ geçidi oluşturmadan önce ayarlanır. Aşağıdaki adımlar uygulama ağ geçidi kaynağı için gerekli yapılandırma öğelerini oluşturur.

1. Bir uygulama ağ geçidi IP yapılandırması oluşturun. Bu ayar, uygulama ağ geçidinin kullandığı alt yapılandırır. Application gateway başladığında, yapılandırılan alt ağdan bir IP adresi seçer ve ağ trafiğini arka uç IP havuzundaki IP adreslerine yollar. Her örneğin bir IP adresi aldığını göz önünde bulundurun.

   ```powershell
   $gipconfig = New-AzApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet
   ```

2. Bir ön uç IP yapılandırmasını oluşturun. Bu ayar, uygulama ağ geçidi ön uç için özel veya genel bir IP adresi eşler. Aşağıdaki adımı genel IP adresi önceki adımda, ön uç IP yapılandırmasını ilişkilendirir.

   ```powershell
   $fipconfig = New-AzApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip
   ```

3. Arka uç IP adresi havuzu ile arka uç web sunucularının IP adreslerini yapılandırın. Bu IP adresleri ön uç IP uç noktasından gelen ağ trafiğinin yönlendirildiği IP adresleridir. Örnek IP adreslerini, kendi uygulamanızın IP adresi uç noktalarını ile değiştirin.

   ```powershell
   $pool = New-AzApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3
   ```

   > [!NOTE]
   > Bir tam etki alanı adı (FQDN) da arka uç sunucuları için IP adresi yerine kullanmak için geçerli bir değer var. Kullanarak etkinleştirin **- BackendFqdns** geçin. 

4. Genel IP uç noktası için ön uç IP bağlantı noktasını yapılandırın. Bu bağlantı noktası, son kullanıcılarının bağlantı noktasıdır.

   ```powershell
   $fp = New-AzApplicationGatewayFrontendPort -Name 'port01'  -Port 443
   ```

5. Uygulama ağ geçidi sertifikası yapılandırın. Bu sertifika, uygulama ağ geçidinde trafiği giden ve şifresini çözmek için kullanılır.

   ```powershell
   $passwd = ConvertTo-SecureString  <certificate file password> -AsPlainText -Force 
   $cert = New-AzApplicationGatewaySSLCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password $passwd 
   ```

   > [!NOTE]
   > Bu örnek SSL bağlantısı için kullanılan sertifikayı yapılandırır. Sertifikanın .pfx formatında olması gerekir ve parola 4 ile 12 karakter olmalıdır.

6. HTTP dinleyicisi için uygulama ağ geçidi oluşturun. Ön uç IP yapılandırması, bağlantı noktası ve kullanmak için SSL sertifikası atayın.

   ```powershell
   $listener = New-AzApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SSLCertificate $cert
   ```

7. SSL etkin arka uç havuzu kaynaklar üzerinde kullanılacak sertifikayı yükleyin.

   > [!NOTE]
   > Varsayılan araştırmasını alır ortak anahtardan *varsayılan* SSL bağlaması arka ucun IP adresi ve aldığı ortak anahtar değeri için ortak anahtar değerini sağlayın burada karşılaştırır. 
   > 
   > Alınan ortak anahtarı, arka uçta barındırma üstbilgileri ve sunucu adı belirtme (SNI) kullanıyorsanız, hangi trafik akışı için hedef siteye olmayabilir. Şüpheli olduğunuz, ziyaret https://127.0.0.1/ hangi sertifikanın için kullanılan onaylamak için arka uç sunucularda *varsayılan* SSL bağlaması. Bu bölümde, isteğinden ortak anahtarı kullanın. HTTPS bağlantılarına barındırma üstbilgileri ve SNI kullanıyorsanız ve bir yanıt ve sertifika için bir el ile tarayıcı istekten aldığınız değil https://127.0.0.1/ arka uç sunucularda varsayılan SSL bağlaması bunlara ayarlamanız gerekir. Bunu yaparsanız araştırmaları başarısız ve arka uç izin verilenler listesinde değil.

   ```powershell
   $authcert = New-AzApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile C:\cert.cer
   ```

   > [!NOTE]
   > Önceki adımda sağlanan sertifikanın ortak anahtarını .pfx sertifikasını arka uçta mevcut olmalıdır. Talep ve kanıt akıl (CER) biçiminde arka uç sunucuda yüklü sertifika (kök sertifika değil) dışarı aktarma ve bu adımı kullanın. Bu adım beyaz arka uç uygulama ağ geçidiyle.

   Ardından Application Gateway v2 SKU kullanıyorsanız, güvenilen kök sertifika yerine bir kimlik doğrulama sertifikası oluşturun. Daha fazla bilgi için [ile Application Gateway uçtan uca SSL'ne genel bakış](ssl-overview.md#end-to-end-ssl-with-the-v2-sku):

   ```powershell
   $trustedRootCert01 = New-AzApplicationGatewayTrustedRootCertificate -Name "test1" -CertificateFile  <path to root cert file>
   ```

8. Uygulama ağ geçidi arka uç HTTP ayarları yapılandırın. HTTP ayarları için önceki adımda yüklenen sertifikanın atayın.

   ```powershell
   $poolSetting = New-AzApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert
   ```

   Application Gateway v2 SKU için aşağıdaki komutu kullanın:

   ```powershell
   $poolSetting01 = New-AzApplicationGatewayBackendHttpSettings -Name “setting01” -Port 443 -Protocol Https -CookieBasedAffinity Disabled -TrustedRootCertificate $trustedRootCert01 -HostName "test1"
   ```

9. Yük Dengeleyici davranışını yapılandıran bir Yük Dengeleyiciyi yönlendirme kuralını oluşturun. Bu örnekte, bir temel hepsini bir kez deneme kuralı oluşturulur.

   ```powershell
   $rule = New-AzApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
   ```

10. Uygulama ağ geçidinin örnek boyutunu yapılandırın. Kullanılabilir boyutlar **standart\_küçük**, **standart\_orta**, ve **standart\_büyük**.  Kapasite için kullanılabilir değerlerdir **1** aracılığıyla **10**.

    ```powershell
    $sku = New-AzApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
    ```

    > [!NOTE]
    > Test amacıyla örnek sayısı 1 olarak seçilebilir. İki örnek altında'herhangi bir örnek sayısı SLA tarafından kapsanmaz ve bu nedenle önerilmez bilmek önemlidir. Küçük geliştirme ve test ve üretim amacıyla kullanılmak üzere ağ geçitleridir.

11. Uygulama ağ geçidinde kullanılacak SSL ilkesini yapılandırın. Application Gateway SSL protokolü sürümleri için en düşük sürüm ayarlama özelliğini destekler.

    Aşağıdaki değerleri tanımlanabilir protokol sürümleri listesi verilmiştir:

    - **TLSV1_0**
    - **TLSV1_1**
    - **TLSV1_2**
    
    Aşağıdaki örnek, en düşük protokol sürümü ayarlar **TLSv1_2** ve sağlayan **TLS\_ECDHE\_ECDSA\_ile\_AES\_128\_GCM\_SHA256**, **TLS\_ECDHE\_ECDSA\_ile\_AES\_256\_GCM\_SHA384**, ve **TLS\_RSA\_ile\_AES\_128\_GCM\_SHA256** yalnızca.

    ```powershell
    $SSLPolicy = New-AzApplicationGatewaySSLPolicy -MinProtocolVersion TLSv1_2 -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256" -PolicyType Custom
    ```

## <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

Yukarıdaki adımları kullanarak, uygulama ağ geçidi oluşturun. Ağ geçidi oluşturulmasını çalıştırmak uzun zaman alan bir işlemdir.

```powershell
$appgw = New-AzApplicationGateway -Name appgateway -SSLCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SSLPolicy $SSLPolicy -AuthenticationCertificates $authcert -Verbose
```

## <a name="apply-a-new-certificate-if-the-back-end-certificate-is-expired"></a>Arka uç sertifikanın süresi doldu, yeni bir sertifika Uygula

Arka uç sertifikanın süresi doldu, yeni bir sertifika uygulamak için bu yordamı kullanın.

1. Güncelleştirmek için uygulama ağ geçidini alın.

   ```powershell
   $gw = Get-AzApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG
   ```
   
2. Sertifikanın ortak anahtarı içeren .cer dosyasını, yeni sertifika kaynağı ekleyin ve application Gateway SSL sonlandırma için dinleyicisi eklenen aynı sertifikayı da olabilir.

   ```powershell
   Add-AzApplicationGatewayAuthenticationCertificate -ApplicationGateway $gw -Name 'NewCert' -CertificateFile "appgw_NewCert.cer" 
   ```
    
3. Yeni kimlik doğrulama sertifikası nesnesini bir değişkene Al (TypeName: Microsoft.Azure.Commands.Network.Models.PSApplicationGatewayAuthenticationCertificate).

   ```powershell
   $AuthCert = Get-AzApplicationGatewayAuthenticationCertificate -ApplicationGateway $gw -Name NewCert
   ```
 
 4. Yeni sertifikayı ata **BackendHttp** ayarı ve $AuthCert değişkenle bakın. (Değiştirmek istediğiniz HTTP ayarı adı belirtin.)
 
   ```powershell
   $out= Set-AzApplicationGatewayBackendHttpSetting -ApplicationGateway $gw -Name "HTTP1" -Port 443 -Protocol "Https" -CookieBasedAffinity Disabled -AuthenticationCertificates $Authcert
   ```
    
 5. Uygulama ağ geçidine değişikliği kaydetmek ve $out değişkene bulunan yeni yapılandırma geçişi.
 
   ```powershell
   Set-AzApplicationGateway -ApplicationGateway $gw  
   ```

## <a name="remove-an-unused-expired-certificate-from-http-settings"></a>Kullanılmayan süresi dolmuş bir sertifika HTTP kaldırın

Kullanılmayan süresi dolmuş bir sertifika HTTP ayarlarından kaldırmak için bu yordamı kullanın.

1. Güncelleştirmek için uygulama ağ geçidini alın.

   ```powershell
   $gw = Get-AzApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG
   ```
   
2. Kaldırmak istediğiniz kimlik doğrulama sertifikası adını listeler.

   ```powershell
   Get-AzApplicationGatewayAuthenticationCertificate -ApplicationGateway $gw | select name
   ```
    
3. Bir uygulama ağ geçidinden kimlik doğrulaması sertifikayı kaldırın.

   ```powershell
   $gw=Remove-AzApplicationGatewayAuthenticationCertificate -ApplicationGateway $gw -Name ExpiredCert
   ```
 
 4. Değişikliği işleyin.
 
   ```powershell
   Set-AzApplicationGateway -ApplicationGateway $gw
   ```

   
## <a name="limit-ssl-protocol-versions-on-an-existing-application-gateway"></a>Var olan bir uygulama ağ geçidinde SSL protokolü sürümlerini sınırlama

Önceki adımlarda uçtan uca SSL ile bir uygulama oluşturma ve belirli SSL protokolü sürümlerini devre dışı bırakma sürdü. Aşağıdaki örnekte mevcut bir uygulama ağ geçidi üzerinde belirli SSL ilkeler devre dışı bırakır.

1. Güncelleştirmek için uygulama ağ geçidini alın.

   ```powershell
   $gw = Get-AzApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG
   ```

2. SSL ilkesi tanımlayın. Aşağıdaki örnekte, **TLSv1.0** ve **TLSv1.1** devre dışı bırakıldı ve şifre paketleri **TLS\_ECDHE\_ECDSA\_ile\_ AES\_128\_GCM\_SHA256**, **TLS\_ECDHE\_ECDSA\_ile\_AES\_256\_GCM\_SHA384**, ve **TLS\_RSA\_ile\_AES\_128\_GCM\_SHA256** yalnızca olanları izin verilir.

   ```powershell
   Set-AzApplicationGatewaySSLPolicy -MinProtocolVersion TLSv1_2 -PolicyType Custom -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256" -ApplicationGateway $gw

   ```

3. Son olarak, ağ geçidi güncelleştirin. Uzun süre çalışan bir görev bu son adımdır. İşlem tamamlandığında, uygulama ağ geçidinde uçtan uca SSL yapılandırılır.

   ```powershell
   $gw | Set-AzApplicationGateway
   ```

## <a name="get-an-application-gateway-dns-name"></a>Bir uygulama ağ geçidi DNS adını alma

Ağ geçidi oluşturulduktan sonra sonraki adıma iletişim için ön uç yapılandırmaktır. Uygulama ağ geçidi, kolay değil bir genel IP kullanırken bir dinamik olarak atanan DNS adı gerektirir. Son kullanıcıların uygulama ağ geçidine ulaşmasını sağlamak için uygulama ağ geçidinin genel uç noktaya işaret edecek bir CNAME kaydı kullanabilirsiniz. Daha fazla bilgi için [Azure'da için bir özel etki alanı adı yapılandırma](../cloud-services/cloud-services-custom-domain-name-portal.md). 

Bir diğer ad yapılandırmak için uygulama ağ geçidinin ayrıntılarını ve onunla ilişkilendirilmiş olan IP/DNS adını kullanarak almak **Publicıpaddress** öğe uygulama ağ geçidine eklenmiş. Uygulama ağ geçidinin DNS adı, iki web uygulamasını bu DNS adını işaret eden bir CNAME kaydı oluşturmak için kullanın. Biz yoksa A kaydı kullanılması önerilir, VIP değiştirebilirsiniz çünkü uygulama ağ geçidi yeniden başlatın.

```powershell
Get-AzPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
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

Web uygulaması güvenlik duvarı uygulama ağ geçidi üzerinden, web uygulamalarınızın güvenliğini artırma hakkında daha fazla bilgi için bkz. [Web uygulaması güvenlik duvarına genel bakış](application-gateway-webapplicationfirewall-overview.md).

[scenario]: ./media/application-gateway-end-to-end-SSL-powershell/scenario.png
