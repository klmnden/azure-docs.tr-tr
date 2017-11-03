---
title: "Azure uygulama ağ geçidi - PowerShell SSL boşaltma - yapılandırma | Microsoft Docs"
description: "Bu makalede Azure Resource Manager kullanarak SSL ile bir uygulama ağ geçidi oluşturma yönergelerini boşaltma sağlar"
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 3c3681e0-f928-4682-9d97-567f8e278e13
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: davidmu
ms.openlocfilehash: ee48ca45ae0d337b5b919dbbb28341caf8af0d45
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a>Azure Resource Manager kullanarak SSL yük boşaltımı için bir uygulama ağ geçidi oluşturma

> [!div class="op_single_selector"]
> * [Azure portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Azure Klasik PowerShell](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Azure Application Gateway, web grubunda maliyetli SSL şifre çözme görevlerinin oluşmasından kaçınmak için Güvenli Yuva Katmanı (SSL) oturumunu sonlandırmak amacıyla yapılandırılabilir. SSL yük boşaltımı ön uç sunucusunun kurulumunu ve web uygulamasının yönetimini de basitleştirir.

## <a name="before-you-begin"></a>Başlamadan önce

1. Web Platformu Yükleyicisi’ni kullanarak Azure PowerShell cmdlet’lerin en son sürümünü yükleyin. **İndirmeler sayfası**’ndaki [Windows PowerShell](https://azure.microsoft.com/downloads/) bölümünden en son sürümü indirip yükleyebilirsiniz.
2. Bir sanal ağ ve uygulama ağ geçidi için bir alt ağ oluşturun. Hiçbir sanal makinenin veya bulut dağıtımlarının alt ağı kullanmadığından emin olun. Uygulama ağ geçidi tek başına bir sanal ağ alt ağında olmalıdır.
3. Uygulama ağ geçidi kullanmak için yapılandırmanız sunucular mevcut olmalıdır veya uç noktaları sanal ağda veya bir ortak IP adresi veya atanan sanal IP adresi (VIP) ile oluşturdunuz.

## <a name="what-is-required-to-create-an-application-gateway"></a>Bir uygulama ağ geçidi oluşturmak için ne gereklidir?

* **Arka uç sunucu havuzuna**: arka uç sunucularının IP adresleri listesi. Listede bulunan IP adresleri sanal ağ alt ağına ait olması gerekir veya genel IP/VIP'ye olmalıdır.
* **Arka uç sunucu havuzu ayarları**: her bir havuz bağlantı noktası, protokol ve tanımlama bilgisi temelli benzeşim gibi ayarları vardır. Bu ayarlar bir havuza bağlıdır ve havuzdaki tüm sunuculara uygulanır.
* **Ön uç bağlantı noktası**: Bu bağlantı noktası uygulama ağ geçidinde açılan genel bağlantı noktasıdır. Bu bağlantı noktasında trafik olursa arka uç sunuculardan birine yönlendirilir.
* **Dinleyici**: dinleyicisinde bir ön uç bağlantı noktası, bir iletişim kuralı kullanılır (Http veya Https; bu ayarları büyük/küçük harfe duyarlıdır) ve (SSL yük boşaltımı yapılandırılıyorsa) SSL sertifika adı.
* **Kural**: kural dinleyiciyi ve arka uç sunucusu havuzunu bağlar ve ne zaman belli bir Dinleyicide trafik trafiği yönlendirmek için hangi arka uç sunucu havuzuna tanımlar. Şu anda yalnızca *temel* kural desteklenmektedir. *Temel* kural hepsini bir kez deneme yöntemiyle yük dağıtımıdır.

**Ek yapılandırma notları**

SSL sertifikaları yapılandırmada **HttpListener**’daki protokol **Https** (küçük/büyük harf duyarlı) ile değiştirilmelidir. Ekleme **SslCertificate** öğesine **HttpListener** SSL sertifikası için yapılandırılmış değişken değeri. Ön uç bağlantı noktası için güncelleştirilmesi **443**.

**Tanımlama bilgisi temelli benzeşimi etkinleştirmek için**: bir istemci oturumundan gelen isteğin web grubunda aynı VM için her zaman yönlendirilir emin olmak için bir uygulama ağ geçidini yapılandırabilirsiniz. Bunu başarmak için ağ geçidinin trafiği uygun şekilde doğrudan bir oturum tanımlama bilgisi ekleyin. Tanımlama bilgisi temelli benzeşimi etkinleştirmek için, **CookieBasedAffinity**’yi **BackendHttpSetting** öğesindeki **Enabled**’a ayarlayın.

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Azure Klasik dağıtım modeli kullanılarak ve Azure Resource Manager kullanma arasındaki fark uygulama ağ geçidi ve yapılandırılması gereken öğeleri oluşturma sırasıdır.

Resource Manager ile uygulama ağ geçidi'nın tüm bileşenleri ayrı ayrı yapılandırılır ve ardından put birlikte bir uygulama ağ geçidi kaynağı oluşturmak için.

Bir uygulama ağ geçidi oluşturmak için takip etmeniz gereken adımlar şunlardır:

1. [Resource Manager için bir kaynak grubu oluşturun](#create-a-resource-group-for-resource-manager)
2. [Sanal ağ, alt ağ ve uygulama ağ geçidi için genel IP oluşturun](#create-virtual-network-subnet-and-public-IP-for-the-application-gateway)
3. [Bir uygulama ağ geçidi yapılandırma nesnesi oluşturun](#create-an-application-gateway-configuration-object)
4. [Bir uygulama ağ geçidi kaynağı oluşturun](#create-an-application-gateway-resource)

## <a name="create-a-resource-group-for-resource-manager"></a>Resource Manager için kaynak grubu oluşturun

Azure Resource Manager cmdlet’lerini kullanmak için PowerShell modunu açtığınızdan emin olun. Daha fazla bilgi için [Windows PowerShell’i Resource Manager ile kullanma](../powershell-azure-resource-manager.md) konusuna bakın.

   1. Aşağıdaki komutu girin:

   ```powershell
   Login-AzureRmAccount
   ```

   2. Hesap için abonelikler denetlemek için aşağıdaki komutları girin:

   ```powershell
   Get-AzureRmSubscription
   ```

   Kimlik bilgilerinizle kimliğinizi doğrulamanız istenir.

   3. Kullanmak için Azure abonelikleri seçmek için aşağıdaki komutu girin:

   ```powershell
   Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
   ```

   4. Bir kaynak grubu oluşturmak için aşağıdaki komutu girin. (Mevcut bir kaynak grubunu kullanıyorsanız bu adımı atlayın.)

   ```powershell
   New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
   ```

Azure Resource Manager, tüm kaynak gruplarının bir konum belirtmesini gerektirir. Bu ayar, kaynak grubundaki kaynaklar için varsayılan konum olarak kullanılır. Bir uygulama ağ geçidi oluşturmak için verilecek komutların aynı kaynak grubunda kullandığınızdan emin olun.

Önceki örnekte adlı bir kaynak grubu oluşturduk **appgw-RG** ve konum **Batı ABD**.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Uygulama ağ geçidi için bir sanal ağ ve bir alt ağ oluşturun

Aşağıdaki örnek Resource Manager kullanarak nasıl sanal ağ oluşturulacağını gösterir:

   1. Aşağıdaki komutu girin:

   ```powershell
   $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
   ```

   Bu örnek adres aralığı atar **10.0.0.0/24** bir sanal ağ oluşturmak için kullanılacak bir alt ağ değişkenine.

   2. Aşağıdaki komutu girin:

   ```powershell
   $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
   ```

   Bu örnek adlı bir sanal ağ oluşturur **appgwvnet** kaynak grubunda **appgw-rg** için **Batı ABD** önek kullanarak bölge **10.0.0.0/16** alt ağ ile **10.0.0.0/24**.

   3. Aşağıdaki komutu girin:

   ```powershell
   $subnet = $vnet.Subnets[0]
   ```

   Bu örnek alt ağ nesnesini değişkenine atar **$subnet** sonraki adımlar için.

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Ön uç yapılandırma için genel bir IP adresi oluşturun

Ön uç yapılandırma için genel bir IP adresi oluşturmak için aşağıdaki komutu girin:

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

Bu örnek bir genel IP kaynağı oluşturur **Publicıp01** kaynak grubunda **appgw-rg** için **Batı ABD** bölge.

## <a name="create-an-application-gateway-configuration-object"></a>Uygulama ağ geçidi yapılandırma nesnesi oluşturun

   1. Uygulama ağ geçidi yapılandırma nesnesi oluşturmak için aşağıdaki komutu girin:

   ```powershell
   $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
   ```

   Bu örnek adlı uygulama ağ geçidi IP yapılandırması oluşturur **Gatewayıp01**. Uygulama ağ geçidi başladığında, yapılandırılan alt ağdan bir IP adresi seçer ve arka uç IP havuzundaki IP adresleri için ağ trafiğini yönlendirir. Her örneğin bir IP adresi aldığını göz önünde bulundurun.

   2. Aşağıdaki komutu girin:

   ```powershell
   $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50
   ```

   Bu örnek adlı arka uç IP adresi havuzunu yapılandırır **pool01** IP adresleriyle **134.170.185.46**, **134.170.188.221**, ve **134.170.185.50** . Bu değerler, ön uç IP uç noktasından gelen ağ trafiğinin yönlendirildiği IP adresleridir. Önceki örnekte yazılan IP adreslerini, kendi web uygulaması uç noktalarınızla değiştirin.

   3. Aşağıdaki komutu girin:

   ```powershell
   $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled
   ```

   Bu örnek uygulama ağ geçidi ayarı yapılandırır **poolsetting01** arka uç havuzundaki Yük Dengeleme ağ trafiği.

   4. Aşağıdaki komutu girin:

   ```powershell
   $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443
   ```

   Bu örnek adlı ön uç IP bağlantı noktasını yapılandırır **frontendport01** genel IP uç noktası için.

   5. Aşağıdaki komutu girin:

   ```powershell
   $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password "<password>"
   ```

   Bu örnek, SSL bağlantısı için kullanılan sertifikayı yapılandırır. Sertifikayı PFX biçiminde olması gerekir ve parola 4 ile 12 karakter arasında olmalıdır.

   6. Aşağıdaki komutu girin:

   ```powershell
   $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip
   ```

   Bu örnek adlı ön uç IP yapılandırmasını oluşturur **fipconfig01** ve genel IP adresi ön uç IP yapılandırmasını ilişkilendirir.

   7. Aşağıdaki komutu girin:

   ```powershell
   $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert
   ```

   Bu örnek adlı dinleyiciyi oluşturur **listener01** ve ön uç bağlantı noktasıyla ön uç IP yapılandırmasını ve sertifikasını ilişkilendirir.

   8. Aşağıdaki komutu girin:

   ```powershell
   $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
   ```

   Bu örnek adlı Yük Dengeleyiciyi yönlendirme kuralını oluşturur **rule01** , yük dengeleyici davranışını yapılandırır.

   9. Aşağıdaki komutu girin:

   ```powershell
   $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
   ```

   Bu örnek, uygulama ağ geçidinin örnek boyutunu yapılandırır.

   > [!NOTE]
   > İçin varsayılan değer **Instancecount** olan **2**, 10 en fazla bir değere sahip. İçin varsayılan değer **GatewaySize** olan **orta**. Aynı zamanda Standard_Small, Standard_Medium ve Standard_Large seçenekleri de bulunmaktadır.

   10. Aşağıdaki komutu girin:

   ```powershell
   $policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S
   ```

   Bu adım, uygulama ağ geçidinde kullanmak için SSL İlkesi tanımlar. Daha fazla bilgi için bkz: [SSL yapılandırma İlkesi sürümlerini ve şifre paketleri uygulama ağ geçidi üzerinde](application-gateway-configure-ssl-policy-powershell.md).

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>New-AzureApplicationGateway kullanarak bir uygulama ağ geçidi oluşturma

Aşağıdaki komutu girin:

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert -SslPolicy $policy
```

Bu örnek, yukarıdaki adımları tüm yapılandırma öğelerinden bir uygulama ağ geçidi oluşturur. Örnekte uygulama ağ geçidi olarak adlandırılır **appgwtest**.

## <a name="get-the-application-gateway-dns-name"></a>Uygulama ağ geçidi DNS adını Al

Ağ geçidi oluşturulduktan sonra sonraki adıma iletişimi için ön uç yapılandırmaktır. Uygulama ağ geçidi, bir ortak IP kolay olmayan kullanırken dinamik olarak atanmış bir DNS adı gerektirir. Son kullanıcıların uygulama ağ geçidi isabet emin olmak için uygulama ağ geçidi için ortak uç noktası için bir CNAME kaydı kullanabilirsiniz. Daha fazla bilgi için bkz: [Azure'da bir özel etki alanı adı yapılandırma](../cloud-services/cloud-services-custom-domain-name-portal.md). 

Uygulama ağ geçidi DNS adını almak için kullanarak uygulama ağ geçidi ve ilişkili IP/DNS adı ayrıntılarını almak **Publicıpaddress** öğesi uygulama ağ geçidine bağlı. İki web uygulamaları bu DNS adına işaret eden bir CNAME kaydı oluşturmak için uygulama ağ geçidi DNS adını kullanın. Biz A kayıtlarını kullanılmasını öneririz, VIP değiştirebilirsiniz olmadığından, uygulama ağ geçidi yeniden yok.


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

Bir iç yük dengeleyici ile kullanmak için bkz: bir uygulama ağ geçidi yapılandırmak istiyorsanız, [bir iç yük dengeleyiciyle bir uygulama ağ geçidi oluşturma](application-gateway-ilb.md).

Yük Dengeleme hakkında daha fazla bilgi için genel olarak, bkz:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
