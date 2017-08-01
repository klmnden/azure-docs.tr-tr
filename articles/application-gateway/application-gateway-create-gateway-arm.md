---
title: "Azure Application Gateway oluşturma ve yönetme - PowerShell | Microsoft Docs"
description: "Bu sayfa, Azure Resource Manager’ı kullanarak Azure uygulama ağ geçidi oluşturma, yapılandırma, başlatma ve silme yönergelerini sağlar"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 866e9b5f-0222-4b6a-a95f-77bc3d31d17b
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/04/2017
ms.author: gwallace
ms.translationtype: HT
ms.sourcegitcommit: 141270c353d3fe7341dfad890162ed74495d48ac
ms.openlocfilehash: 6d38dd6802a25b147fd014b4d26ca432ca87a07d
ms.contentlocale: tr-tr
ms.lasthandoff: 07/25/2017

---
# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a>Azure Resource Manager kullanarak bir uygulama ağ geçidi oluşturma, başlatma veya silme

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Azure Klasik PowerShell](application-gateway-create-gateway.md)
> * [Azure Resource Manager şablonu](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI](application-gateway-create-gateway-cli.md)

Azure Application Gateway, bir katman 7 yük dengeleyicidir. Bulutta veya şirket içinde olmalarından bağımsız olarak, farklı sunucular arasında yük devretme ile HTTP istekleri için performans amaçlı yönlendirme sağlar.
Application Gateway; HTTP yük dengeleme, tanımlama bilgisi tabanlı oturum benzeşimi, Güvenli Yuva Katmanı (SSL) boşaltma, özel sistem durumu araştırmaları, çoklu site desteği gibi birçok Application Delivery Controller (ADC) özelliği sunar.

Desteklenen özelliklerin tam listesi için bkz. [Application Gateway’e Genel Bakış](application-gateway-introduction.md)

Bu makale, uygulama ağ geçidi oluşturma, yapılandırma, başlatma ve silme adımlarında size eşlik eder.

> [!IMPORTANT]
> Azure kaynaklarıyla çalışmadan önce Azure’da şu anda iki dağıtım modeli olduğunu anlamak önemlidir: Resource Manager ve klasik. Azure kaynaklarıyla çalışmadan önce [dağıtım modellerini ve araçlarını](../azure-classic-rm.md) iyice anladığınızdan emin olun. Bu makalenin en üstündeki sekmelere tıklayarak farklı araçlarla ilgili belgeleri görüntüleyebilirsiniz. Bu belge, Azure Resource Manager’ı kullanarak uygulama ağ geçidi oluşturmayı kapsar. Klasik sürümü kullanmak için [PowerShell’i kullanarak uygulama ağ geçidi klasik dağıtımı oluşturma](application-gateway-create-gateway.md) bağlantısına gidin.

## <a name="before-you-begin"></a>Başlamadan önce

1. Web Platformu Yükleyicisi’ni kullanarak Azure PowerShell cmdlet’lerin en son sürümünü yükleyin. **İndirmeler sayfası**’ndaki [Windows PowerShell](https://azure.microsoft.com/downloads/) bölümünden en son sürümü indirip yükleyebilirsiniz.
1. Mevcut bir sanal ağınız varsa, var olan boş bir alt ağı seçin ya da var olan sanal ağınızda yalnızca uygulama ağ geçidinin kullanımına yönelik bir alt ağ oluşturun. Uygulama ağ geçidini, uygulama ağ geçidinin arkasına dağıtmak istediğiniz kaynaklardan farklı bir sanal ağa dağıtamazsınız.
1. Uygulama ağ geçidi kullanırken yapılandırdığınız sunucular mevcut olmalıdır veya uç noktaları sanal ağda veya atanan genel bir IP/VIP’de oluşturulmuş olmalıdır.

## <a name="what-is-required-to-create-an-application-gateway"></a>Bir uygulama ağ geçidi oluşturmak için ne gereklidir?

* **Arka uç sunucu havuzu:** Arka uç sunucularının IP adreslerini, FQDN’lerini veya ağ arabirimlerini içeren liste. IP adresleri kullanılıyorsa, sanal ağ alt ağına veya genel IP/VIP’ye ait olmalıdır.
* **Arka uç sunucu havuzu ayarları**: Her havuzun bağlantı noktası, protokol ve tanımlama bilgisi temelli benzeşim gibi ayarları vardır. Bu ayarlar bir havuza bağlıdır ve havuzdaki tüm sunuculara uygulanır.
* **Ön uç bağlantı noktası:** Bu bağlantı noktası uygulama ağ geçidinde açılan genel bağlantı noktasıdır. Bu bağlantı noktasında trafik olursa arka uç sunuculardan birine yönlendirilir.
* **Dinleyici:** Dinleyicide bir ön uç bağlantı noktası, bir protokol (Http veya Https, bu değerler büyük/küçük harfe duyarlıdır) ve SSL sertifika adı (SSL yük boşaltımı yapılandırılıyorsa) vardır.
* **Kural:** Kural dinleyiciyi arka uç sunucusu havuzunu bağlar ve belli bir dinleyicide trafik olduğunda trafiğin hangi arka uç sunucu havuzuna yönlendirileceğini belirler.

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Azure Klasik ve Azure Resource Manager’ın kullanımı arasındaki fark, uygulama ağ geçidi oluştururken takip ettiğiniz sıra ve yapılandırılması gereken öğelerdir.

Resource Manager’da uygulama ağ geçidini oluşturan öğeler ayrı ayrı yapılandırılır ve sonra uygulama ağ geçidi kaynağı oluşturmak için bir araya getirilir.

Uygulama ağ geçidi oluşturmak için gereken adımlar aşağıda verilmiştir.

## <a name="create-a-resource-group-for-resource-manager"></a>Resource Manager için kaynak grubu oluşturun

Azure PowerShell’in en yeni sürümünü kullandığınızdan emin olun. Daha fazla bilgi için bkz.[Resource Manager ile Windows PowerShell Kullanma](../powershell-azure-resource-manager.md)

### <a name="step-1"></a>1. Adım

Azure'da oturum açma

```powershell
Login-AzureRmAccount
```

Kimlik bilgilerinizle kimliğinizi doğrulamanız istenir.

### <a name="step-2"></a>2. Adım

Hesapla ilişkili abonelikleri kontrol edin.

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a>3. Adım

Hangi Azure aboneliğinizin kullanılacağını seçin.

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a>4. Adım

Bir kaynak grubu oluşturun (mevcut bir kaynak grubu kullanıyorsanız bu adımı atlayın).

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

Azure Resource Manager, tüm kaynak gruplarının bir konum belirtmesini gerektirir. Bu konum, kaynak grubundaki kaynaklar için varsayılan konum olarak kullanılır. Uygulama ağ geçidi oluşturmak için verilen komutların aynı kaynak grubunu kullandığından emin olun.

Yukarıdaki örnekte, **appgw-RG** adlı, **Batı ABD** konumlu bir kaynak grubu oluşturduk.

> [!NOTE]
> Uygulama ağ geçidiniz için özel bir araştırma yapılandırmanız gerekiyorsa, [PowerShell kullanarak özel araştırmalara sahip bir uygulama ağ geçidi oluşturma](application-gateway-create-probe-ps.md) sayfasını ziyaret edin. Daha fazla bilgi için [özel araştırmalar ve sistem durumu izleme](application-gateway-probe-overview.md) konusunu inceleyin.

## <a name="create-a-virtual-network-and-a-subnet"></a>Sanal ağ ve alt ağ oluşturma

Aşağıdaki örnek Resource Manager kullanarak nasıl sanal ağ oluşturulacağını gösterir. Bu örnek, Application Gateway için bir sanal ağ oluşturur. Application Gateway kendi alt ağını gerektirdiğinden, Application Gateway için oluşturulan alt ağ, sanal ağ adres alanından küçüktür. Daha küçük bir alt ağ kullanılması, web sunucularını da kapsayan ancak bunlarla sınırlı olmayan diğer kaynakların aynı sanal ağda yapılandırılmasına olanak tanır.

### <a name="step-1"></a>1. Adım

10.0.0.0/24 adres aralığını, sanal ağ oluşturmak için kullanılacak bir alt ağ değişkenine atayın. Bu adım, Application Gateway için sonraki örnekte kullanılan alt ağ yapılandırma nesnesini oluşturur.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

### <a name="step-2"></a>2. Adım

Batı ABD bölgesi için 10.0.0.0/24 alt ağıyla 10.0.0.0/16 ön ekini kullanarak **appgw-rg** kaynak grubunda **appgwvnet** adlı bir sanal ağ oluşturun. Bu adım, Application Gateway’in bulunacağı tek bir alt ağ ile sanal ağın yapılandırmasını tamamlar.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

### <a name="step-3"></a>3. Adım

Sonraki adımlar için alt ağ değişkenini atayın; bu değişken sonraki bir adımda `New-AzureRMApplicationGateway` cmdlet’ine geçirilir.

```powershell
$subnet=$vnet.Subnets[0]
```

## <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

Batı ABD bölgesi için **appgw-rg** kaynak grubunda **publicIP01** genel bir IP kaynağı oluşturun. Application Gateway, yük dengeleme isteklerini almak için genel IP adresi, iç IP adresi veya her ikisini birden kullanabilir.  Bu örnekte yalnızca genel IP adresi kullanılmaktadır. Aşağıdaki örnekte, genel IP adresi oluşturmak için bir DNS adı yapılandırılmaz.  Application Gateway, genel IP adreslerinde özel DNS adlarını desteklemez.  Genel bir uç nokta için özel bir ad gerekirse, genel IP adresi için otomatik oluşturulan DNS adını işaret eden bir CNAME kaydı oluşturulmalıdır.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

> [!NOTE]
> Hizmet başlatıldığında uygulama ağ geçidine bir IP adresi atanır.

## <a name="create-the-application-gateway-configuration-objects"></a>Uygulama ağ geçidi yapılandırma nesnelerini oluşturun

Tüm yapılandırma öğeleri, uygulama ağ geçidi oluşturulmadan önce ayarlanmalıdır. Aşağıdaki adımlar uygulama ağ geçidi kaynağı için gerekli yapılandırma öğelerini oluşturur.

### <a name="step-1"></a>1. Adım

**gatewayIP01** adlı bir uygulama ağ geçidi IP yapılandırması oluşturun. Application Gateway başladığında, yapılandırılan alt ağdan bir IP adresi alır ve ağ trafiğini arka uç IP havuzundaki IP adreslerine yönlendirir. Her örneğin bir IP adresi aldığını göz önünde bulundurun.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

### <a name="step-2"></a>2. Adım

**pool01** adlı arka uç IP adresi havuzunu **pool1** için IP adresleri ile yapılandırın. Bu IP adresleri, uygulama ağ geçidi tarafından korunacak web uygulamasını barındıran kaynakların IP adresleridir. Bu arka uç havuzunun tüm üyelerinin sistem durumu, temel araştırma veya özel araştırma olmasına bakılmaksızın araştırmalarla doğrulanmıştır.  Uygulama ağ geçidine istekler geldiğinde trafik bunlara yönlendirilir. Arka uç havuzları, uygulama ağ geçidinde birden fazla kural tarafından kullanılabilir; diğer bir deyişle, bir arka uç havuzu aynı ana bilgisayarda bulunan birden fazla web uygulaması için kullanılabilir.

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50
```

Bu örnekte, URL yolu temel alınarak ağ trafiğini yönlendirmek üzere iki arka uç havuzu bulunur. Bir havuz "/video" URL yolundan trafiği alırken, diğer havuz "/image" yolundan trafiği alır. Kendi uygulamanızın IP adresi uç noktalarını eklemek için önceki IP adreslerini değiştirin.

### <a name="step-3"></a>3. Adım

Arka uç havuzundaki yük dengeli ağ trafiği için **poolsetting** uygulama ağ geçidi ayarını yapılandırın. Her bir arka uç havuzu kendi arka uç havuzu ayarlarına sahip olabilir.  Arka uç HTTP ayarları, trafiği doğru arka uç havuzu üyelerine yönlendirmek üzere kurallar tarafından kullanılır. Arka uç HTTP ayarları, arka uç havuz üyelerine trafik gönderirken kullanılan protokolü ve bağlantı noktasını belirler. Tanımlama bilgisine dayalı oturumlar da arka uç HTTP ayarları tarafından belirlenir.  Etkinleştirilirse, tanımlama bilgisine dayalı oturum benzeşimi, her paket için önceki isteklerle aynı arka uca trafik gönderir.

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120
```

### <a name="step-4"></a>4. Adım

Bir uygulama ağ geçidi için ön uç bağlantı noktasını yapılandırın. Ön uç bağlantı noktası yapılandırma nesnesi, Application Gateway’in dinleyici üzerindeki trafik için hangi bağlantı noktasını dinlediğini tanımlamak üzere dinleyici tarafından kullanılır.

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80
```

### <a name="step-5"></a>5. Adım

Ön uç IP’sini genel IP uç noktası ile yapılandırın. Ön uç IP yapılandırma nesnesi, dışa doğru IP adresini dinleyici ile ilişkilendirmek üzere dinleyici tarafından kullanılır.

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip
```

### <a name="step-6"></a>6. Adım

Dinleyiciyi yapılandırın. Bu adım, gelen ağ trafiğini almak için kullanılan genel IP adresi ve bağlantı noktası için dinleyiciyi yapılandırır. Aşağıdaki örnek daha önce yapılandırılmış ön uç IP yapılandırmasını, ön uç bağlantı noktası yapılandırmasını ve bir protokolü (http veya https) alıp dinleyiciyi yapılandırır. Bu örnekte, dinleyici daha önce oluşturulan genel IP adresi üzerinde bağlantı noktası 80 üzerindeki HTTP trafiğini dinler.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp
```

### <a name="step-7"></a>7. Adım

Yük dengeleyici davranışını yapılandıran **rule01** adlı yük dengeleyiciyi yönlendirme kuralını oluşturun. Arka uç havuzu ayarları, dinleyici ve önceki adımlarda oluşturulan arka uç havuzu kuralı oluşturur. Tanımlanan ölçütler temel alınarak, trafik uygun arka uca yönlendirilir.

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

### <a name="step-8"></a>8. Adım

Uygulama ağ geçidi için örnek sayısını ve boyutu yapılandırın.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

> [!NOTE]
> **InstanceCount** için varsayılan değer 2 ile 10 arasıdır. **GatewaySize** için varsayılan değer Medium’dur. Aynı zamanda **Standard_Small**, **Standard_Medium**, ve **Standard_Large** seçenekleri de bulunmaktadır.

## <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

Önceki adımlarda geçen tüm yapılandırma öğeleri ile bir uygulama ağ geçidi oluşturun. Bu örnekte uygulama ağ geçidi **appgwtest** olarak adlandırılmıştır.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

Uygulama ağ geçidine eklenen ortak IP kaynağından uygulama ağ geçidinin DNS ve VIP bilgilerini alın.

```powershell
Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName appgw-rg  
```

## <a name="delete-the-application-gateway"></a>Uygulama ağ geçidini silme

Uygulama ağ geçidini silmek için aşağıdaki adımları izleyin:

### <a name="step-1"></a>1. Adım

Uygulama ağ geçidi nesnesini alın ve `$getgw` değişkenine ilişkilendirin.

```powershell
$getgw = Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

### <a name="step-2"></a>2. Adım

Uygulama ağ geçidini sonlandırmak için `Stop-AzureRmApplicationGateway` hizmetini kullanın.

```powershell
Stop-AzureRmApplicationGateway -ApplicationGateway $getgw
```

Uygulama ağ geçidi durdurulmuş konumda olduğunda, hizmeti kaldırmak için `Remove-AzureRmApplicationGateway` cmdlet’ini kullanın.

```powershell
Remove-AzureRmApplicationGateway -Name $appgwtest -ResourceGroupName appgw-rg -Force
```

> [!NOTE]
> **-force** anahtarı, kaldırma onayı iletisini gizlemek için kullanılabilir.

Hizmetin kaldırıldığını doğrulamak için `Get-AzureRmApplicationGateway` cmdlet’ini kullanabilirsiniz. Bu adım gerekli değildir.

```powershell
Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

## <a name="get-application-gateway-dns-name"></a>Uygulama ağ geçidi DNS adını alma

Ağ geçidi oluşturulduktan sonraki adım, iletişim için ön uç yapılandırması yapmaktır. Genel IP kullanırken uygulama ağ geçidi için dinamik olarak atanan DNS adı gerekir ve bu durum çok kullanışlı değildir. Son kullanıcıların uygulama ağ geçidine ulaşmasını sağlamak için uygulama ağ geçidinin genel uç noktasını işaret edecek bir CNAME kaydı kullanılabilir. [Azure’da özel etki alanı adı yapılandırma](../cloud-services/cloud-services-custom-domain-name-portal.md). Dinamik olarak oluşturulan DNS adını bulmak için, uygulama ağ geçidinin ayrıntılarını ve onunla ilişkilendirilmiş olan IP/DNS adını uygulama ağ geçidine eklenmiş PublicIPAddress öğesini kullanarak alın. Uygulama ağ geçidinin DNS adı, iki web uygulamasını bu DNS adına götüren bir CNAME kaydı oluşturmak için kullanılmalıdır. Uygulama ağ geçidi yeniden başlatıldığında VIP değişebileceğinden A kaydı kullanımı önerilmez.

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

## <a name="delete-all-resources"></a>Tüm kaynakları silme

Bu makalede oluşturulan tüm kaynakları silmek için, aşağıdaki adımları tamamlayın:

```powershell
Remove-AzureRmResourceGroup -Name appgw-RG
```

## <a name="next-steps"></a>Sonraki adımlar

SSL yükü boşaltmayı yapılandırmak istiyorsanız [SSL yükü boşaltma için uygulama ağ geçidi yapılandırma](application-gateway-ssl.md) sayfasını ziyaret edin.

İç yük dengeleyiciyle kullanacağınız uygulama ağ geçidi yapılandırmak istiyorsanız [İç yük dengeleyici (ILB) ile uygulama ağ geçidi oluşturma](application-gateway-ilb.md) sayfasını ziyaret edin.

Yük dengeleme seçenekleri hakkında daha fazla genel bilgi edinmek istiyorsanız, bkz.:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)


