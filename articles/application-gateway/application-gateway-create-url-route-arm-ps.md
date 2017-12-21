---
title: "URL yönlendirme kurallarını kullanarak bir uygulama ağ geçidi oluşturma | Microsoft Docs"
description: "Bu sayfayı oluşturmak ve URL yönlendirme kurallarını kullanarak bir Azure uygulama ağ geçidi yapılandırmak için yönergeler sağlar."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: d141cfbb-320a-4fc9-9125-10001c6fa4cf
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/03/2017
ms.author: davidmu
ms.openlocfilehash: f0b085ebf922cd5b14acd91bf86b9262a6921e9e
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="create-an-application-gateway-by-using-path-based-routing"></a>Yol tabanlı yönlendirme kullanarak bir uygulama ağ geçidi oluşturma

> [!div class="op_single_selector"]
> * [Azure portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

Yol tabanlı yönlendirme bir HTTP isteği URL yola göre yollar ilişkilendirir. Uygulama ağ geçidi sunulan URL için yapılandırılmış bir arka uç havuzu için bir yol yoktur ve ardından ağ trafiğini tanımlı arka uç havuzuna gönderir olup olmadığını denetler. URL tabanlı yönlendirme için bir genel Yük Dengeleme isteklerini farklı arka uç sunucu havuzu için farklı içerik türleri için kullanılır.

Azure uygulama ağ geçidi sahip iki kural türleri: temel Yönlendirme ve yol tabanlı yönlendirme. Basic arka uç havuzları için hepsini hizmet sağlar. Yol tabanlı hepsini dağıtım yanı sıra, yönlendirme, istenen URL yolu desenini arka uç havuzu seçin için de kullanır.

## <a name="scenario"></a>Senaryo

Aşağıdaki örnekte uygulama ağ geçidi trafiği contoso.com için iki arka uç sunucu havuzu ile hizmet eder: bir video sunucu havuzu ve bir görüntü sunucu havuzu.

İstekleri için http://contoso.com/image * görüntü sunucu havuzuna yönlendirilen (**pool1**), ve istekleri için http://contoso.com/video * video sunucu havuzuna yönlendirilen (**pool2**). Yol desenleri hiçbiri eşleşiyorsa, varsayılan sunucu havuzuna (**pool1**) seçilidir.

![URL rota](./media/application-gateway-create-url-route-arm-ps/figure1.png)

## <a name="before-you-begin"></a>Başlamadan önce

1. Web Platformu Yükleyicisi’ni kullanarak Azure PowerShell cmdlet’lerin en son sürümünü yükleyin. **İndirmeler sayfası**’ndaki [Windows PowerShell](https://azure.microsoft.com/downloads/) bölümünden en son sürümü indirip yükleyebilirsiniz.
2. Bir sanal ağ ve bir uygulama ağ geçidi için alt ağ oluşturun. Hiçbir sanal makine veya Bulut dağıtımlarının alt kullandığınızdan emin olun. Uygulama ağ geçidi tek başına bir sanal ağ alt ağında olmalıdır.
3. Uygulama ağ geçidi için arka uç havuzuna eklenen sunucuların mevcut veya atanmış IP/VIP'ye oluşturduğunuz sanal ağda veya bir ortak uç noktaları sahip olduğunuzdan emin olun.

## <a name="requirements-to-create-an-application-gateway"></a>Bir uygulama ağ geçidi oluşturmak için gereksinimleri

* **Arka uç sunucu havuzuna**: arka uç sunucularının IP adresleri listesi. Listede bulunan IP adresleri sanal ağ alt ağına ait veya genel IP/VIP'ye olabilir.
* **Arka uç sunucu havuzu ayarları**: bağlantı noktası, protokol ve tanımlama bilgisi temelli benzeşimi gibi. Bunlar bir havuza bağlıdır ve havuzdaki tüm sunuculara uygulanır.
* **Ön uç bağlantı noktası**: uygulama ağ geçidinde açılan genel bağlantı noktası. Trafiği, bu bağlantı noktasında trafik ve arka uç sunuculardan birine yönlendirir.
* **Dinleyici**: (SSL yük boşaltımı yapılandırılıyorsa) dinleyicisinde bir ön uç bağlantı noktası, bir protokol (Http veya Https büyük küçük harfe duyarlıdır) ve SSL sertifika adı kullanılır.
* **Kural**: kural dinleyiciyi ve arka uç sunucusu havuzunu bağlar ve bir Dinleyicide trafik olduğunda trafiğin hangi havuzuna yönlendirilmesi gerektiğini tanımlar.

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Klasik dağıtım modeli ve Azure Resource Manager kullanma arasındaki fark uygulama ağ geçidi ve yapılandırılması gereken öğeleri oluşturma sırasıdır.

Resource Manager’da uygulama ağ geçidini oluşturan öğeler ayrı ayrı yapılandırılır ve sonra uygulama ağ geçidi kaynağı oluşturmak için bir araya getirilir.

Bir uygulama ağ geçidi oluşturmak için aşağıdaki adımları izleyin:

1. Resource Manager için kaynak grubu oluşturun.
2. Uygulama ağ geçidi için sanal ağ, alt ağ ve genel IP oluşturun.
3. Uygulama ağ geçidi yapılandırma nesnesi oluşturun.
4. Uygulama ağ geçidi kaynağı oluşturun.

## <a name="create-a-resource-group-for-resource-manager"></a>Resource Manager için kaynak grubu oluşturun

Azure PowerShell'in en son sürümünü kullandığınızdan emin olun. Konumundaki daha fazla bilgi bulmak [Resource Manager ile Windows PowerShell kullanarak](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>1. Adım

Azure'da oturum açın.

```powershell
Login-AzureRmAccount
```

Kimlik bilgilerinizle kimliğinizi istenir.<BR>

### <a name="step-2"></a>2. Adım

Hesapla ilişkili abonelikleri kontrol edin.

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a>3. Adım

Hangi Azure aboneliğinizin kullanılacağını seçin. <BR>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a>4. Adım

Bir kaynak grubu oluşturun. (Mevcut bir kaynak grubunu kullanıyorsanız bu adımı atlayın.)

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US"
```

Alternatif olarak, bir uygulama ağ geçidi için bir kaynak grubu için etiketler oluşturabilirsiniz:

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway URL routing"} 
```

Azure Resource Manager kaynak grupları, gruptaki tüm kaynaklar için kullanılan bir varsayılan konum belirtmesini gerektirir. Bir uygulama ağ geçidi oluşturmak için verilecek komutların aynı kaynak grubunda kullandığınızdan emin olun.

Önceki örnekte, biz "appgw-RG" adlı bir kaynak grubu oluşturduk ve kullanılan konumun "Batı ABD."

> [!NOTE]
> Uygulama ağ geçidiniz için özel bir araştırma yapılandırmanız gerekiyorsa, Git [PowerShell kullanarak özel araştırmalara sahip bir uygulama ağ geçidi oluşturma](application-gateway-create-probe-ps.md). Bkz: [ uygulama ağ geçidi durumu izlemeye genel bakış](application-gateway-probe-overview.md) daha fazla bilgi için.
> 
> 

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Uygulama ağ geçidi için bir sanal ağ ve bir alt ağ oluşturun

Aşağıdaki örnek Resource Manager kullanarak nasıl sanal ağ oluşturulacağını gösterir. Bu örnek uygulama ağ geçidi için bir sanal ağ oluşturur. Uygulama ağ geçidi, kendi alt gerektirir. Bu nedenle, uygulama ağ geçidi için oluşturulan alt sanal ağ adres alanı küçüktür. Bu da dahil olmak üzere kaynaklar sağlar ancak web sunucuları için bunlarla sınırlı olmamak aynı sanal ağda yapılandırılması.

### <a name="step-1"></a>1. Adım

10.0.0.0/24 adres aralığını, sanal ağ oluşturmak için kullanılacak bir alt ağ değişkenine atayın.  Bu, sonraki örnekte kullanılan uygulama ağ geçidi için alt ağ yapılandırma nesnesi oluşturur.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

### <a name="step-2"></a>2. Adım

Adlı bir sanal ağ oluşturma **appgwvnet** kaynak grubunda **appgw-rg** 10.0.0.0/24 alt ağıyla önek 10.0.0.0/16 kullanarak Batı ABD bölgesi için. Bu bulunmasını uygulama ağ geçidi için tek bir alt ağ ile sanal ağ yapılandırmasını tamamlar.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

### <a name="step-3"></a>3. Adım

Sonraki adımlarda alt ağ değişkenine atayın. Bu geçirilir `New-AzureRMApplicationGateway` bir sonraki adımda cmdlet'i.

```powershell
$subnet=$vnet.Subnets[0]
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Ön uç yapılandırma için genel bir IP adresi oluşturun

Batı ABD bölgesi için **appgw-rg** kaynak grubunda **publicIP01** genel bir IP kaynağı oluşturun. Uygulama ağ geçidi, bir ortak IP adresi, bir iç IP adresi veya her ikisi de, Yük Dengeleme için istekleri almak için kullanabilirsiniz.  Bu örnek yalnızca genel IP adresi kullanır. Uygulama ağ geçidi genel IP adreslerini özel DNS adlarını desteklemediğinden aşağıdaki örnekte, genel IP adresi oluşturmak için bir DNS adı yapılandırılır.  Genel bir uç nokta için özel bir adı gerekiyorsa, ortak IP adresi için otomatik olarak oluşturulan DNS adını işaret etmek için bir CNAME kaydı oluşturun.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

Hizmet başlatıldığında uygulama ağ geçidine bir IP adresi atanır.

## <a name="create-the-application-gateway-configuration"></a>Uygulama ağ geçidi yapılandırmasını oluşturma

Uygulama ağ geçidi oluşturmadan önce tüm yapılandırma öğeleri ayarlanması gerekir. Aşağıdaki adımlar uygulama ağ geçidi kaynağı için gerekli yapılandırma öğeleri oluşturun.

### <a name="step-1"></a>1. Adım

**gatewayIP01** adlı bir uygulama ağ geçidi IP yapılandırması oluşturun. Uygulama ağ geçidi başladığında, yapılandırılan alt ağdan bir IP adresi seçer ve ağ trafiği arka uç IP havuzundaki IP adreslerine yollar. Her örneğin bir IP adresi aldığını göz önünde bulundurun.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

### <a name="step-2"></a>2. Adım

Adlı arka uç IP adresi havuzu yapılandırmak **pool1** ve **pool2** için IP adresleriyle **pool1** ve **pool2**. Bu uygulama ağ geçidi tarafından korunacak web uygulama ana bilgisayar kaynakları IP adresleridir. Bu arka uç havuzu üyeleri tüm sağlıklı olmasını temel veya özel araştırmalar tarafından doğrulanır. Uygulama ağ geçidine istekler geldiğinde trafik bunlara yönlendirilir. Arka uç havuzları, uygulama ağ geçidi içinde birden çok kurallar tarafından kullanılabilir. Bu, aynı ana bilgisayarda bulunan birden çok web uygulamaları için bir arka uç havuzu kullanılabilir anlamına gelir.

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 134.170.186.47, 134.170.189.222, 134.170.186.51
```

Bu örnekte, iki arka uç havuzları URL yola göre ağ trafiğini yönlendirebilir. Bir havuz alır trafik URL yolu "/ video," ve diğer havuzu trafiği yolundan alır. "/ görüntü." Kendi uygulamanızın IP adresi uç noktalarını eklemek için önceki IP adreslerini değiştirin. 

### <a name="step-3"></a>3. Adım

Uygulama ağ geçidi ayarlarını yapılandırma **poolsetting01** ve **poolsetting02** arka uç havuzundaki yük dengeli ağ trafiği için. Bu örnekte, arka uç havuzları farklı arka uç havuzu ayarlarını yapılandırın. Her bir arka uç havuzu kendi ayarlara sahip olabilir.  Kuralları doğru arka uç havuzu üyelerine trafiğini yönlendirmek için arka uç HTTP ayarları kullanın. Bu arka uç havuzu üyelerine trafiği göndermek için kullanılan bağlantı noktası ve protokolü belirler. Tanımlama bilgisi tabanlı oturum Ayrıca arka uç HTTP ayarları tarafından belirlenir. Etkinleştirilirse, tanımlama bilgisi tabanlı oturum benzeşimi trafiği için aynı arka uç her paket için olarak önceki istekleri gönderir.

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a>4. Adım

Ön uç IP genel IP uç ile yapılandırın. Dinleyici ön uç IP yapılandırması nesnesi dışa dönük IP adresi dinleyicisi ile ilişkilendirmek için kullanır.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a>5. Adım

Bir uygulama ağ geçidi için ön uç bağlantı noktasını yapılandırın. Dinleyici ön uç bağlantı noktası yapılandırma nesnesini uygulama ağ geçidi dinleyicisi trafiğini dinleyen hangi bağlantı noktasını tanımlamak için kullanır.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 80
```

### <a name="step-6"></a>6. Adım

Dinleyici gelen ağ trafiğini almak için kullanılan bağlantı noktası ve genel IP adresi için yapılandırın. Aşağıdaki örnek, önceden yapılandırılmış ön uç IP yapılandırmasını, ön uç bağlantı noktası yapılandırması ve bir protokol (Http veya Https büyük küçük harfe duyarlıdır) alır ve dinleyicisi yapılandırır. Bu örnekte, dinleyici daha önce oluşturulan genel IP adresi üzerinde bağlantı noktası 80 üzerindeki HTTP trafiğini dinler.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Http -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01
```

### <a name="step-7"></a>7. Adım

URL kuralı yolları arka uç havuzları için yapılandırın. Bu adım, uygulama ağ geçidi tarafından kullanılan göreli yolu yapılandırır ve URL yolunu ve gelen trafiği işlemeye atadığı arka uç havuzuna arasındaki eşleme tanımlar.

> [!IMPORTANT]
> Her yol başlamalıdır bir "/ ile" ve bir yıldız işareti yalnızca sonunda izin verilir. Geçerli örnekler /xyz, /xyz*, veya /xyz/*. Yol Eşleştirici sat dize ilk sonra herhangi bir metin içermiyor "?" veya "#" ve bu karakterleri izin verilmez. 

Aşağıdaki örnekte iki kural oluşturur: biri bir "/ Görüntü /" yol yönlendirme trafiği için arka uç **pool1**başka bir "/ video /" yol yönlendirme trafiği için arka uç **pool2**. Bu kurallar URL'leri her kümesi trafiği arka ucuna yönlendirilir emin olun. Http://contoso.com/image/figure1.jpg Örneğin, gider **pool1** ve http://contoso.com/video/example.mp4 gider **pool2**.

```powershell
$imagePathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule1" -Paths "/image/*" -BackendAddressPool $pool1 -BackendHttpSettings $poolSetting01

$videoPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule2" -Paths "/video/*" -BackendAddressPool $pool2 -BackendHttpSettings $poolSetting02
```

Yolun önceden tanımlanmış bir yol kurallardan herhangi birinin eşleşmiyorsa, kural yol haritası yapılandırmasını varsayılan arka uç adres havuzu da yapılandırır. Http://contoso.com/shoppingcart/test.html Örneğin, gider **pool1** eşleşmeyen trafiği için varsayılan havuzu olarak tanımlanmadığından.

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $videoPathRule, $imagePathRule -DefaultBackendAddressPool $pool1 -DefaultBackendHttpSettings $poolSetting02
```

### <a name="step-8"></a>8. Adım

Bir kural ayarı oluşturun. Bu adım, yol tabanlı URL yönlendirmeyi kullanmak için uygulama ağ geçidi yapılandırır. `$urlPathMap` Değişkeni önceki adımda tanımlanan yol tabanlı kuralı oluşturmak için şimdi kullanılır. Bu adımda, biz kuralı ile bir dinleyici ilişkilendirin ve URL yolunu eşleme daha önce oluşturduğunuz.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-9"></a>9. Adım

Uygulama ağ geçidi için örnek sayısını ve boyutu yapılandırın.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Small" -Tier Standard -Capacity 2
```

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Yukarıdaki adımlarda geçen tüm yapılandırma nesnelerle bir uygulama ağ geçidi oluşturun.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku
```

## <a name="get-an-application-gateway-dns-name"></a>Bir uygulama ağ geçidi DNS adını Al

Ağ geçidi oluşturduktan sonra iletişim için ön uç yapılandıracaksınız. Genel IP kullanırken, uygulama ağ geçidi kolay olmayan dinamik olarak atanmış bir DNS adı gerektirir. Müşteriler uygulama ağ geçidi isabet emin olmak için uygulama ağ geçidi için ortak uç noktası için bir CNAME kaydı kullanabilirsiniz. Daha fazla bilgi için bkz: [bir Azure bulut hizmeti için bir özel etki alanı adı yapılandırma](../cloud-services/cloud-services-custom-domain-name-portal.md).

Ön uç IP CNAME kaydını yapılandırmak için uygulama ağ geçidine bağlı Publicıpaddress öğesi kullanarak uygulama ağ geçidi ve ilişkili IP/DNS adı ayrıntılarını alabilirsiniz. Uygulama ağ geçidi DNS adı bir CNAME kaydı oluşturmak için kullanın. A kayıtları kullanımını VIP üzerinde değişebileceğinden uygulama ağ geçidi yeniden öneririz yok.

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

Güvenli Yuva Katmanı (SSL) yük boşaltma hakkında bilgi edinmek istiyorsanız, bkz: [Azure Resource Manager kullanarak SSL yük boşaltımı için bir uygulama ağ geçidi yapılandırma](application-gateway-ssl-arm.md).

