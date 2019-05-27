---
title: Azure uygulama ağ geçidi kullanarak iç yük dengeleyici - PowerShell | Microsoft Docs
description: Bu sayfa, Azure Resource Manager için iç yük dengeleyiciye (ILB) sahip bir Azure uygulama ağ geçidi oluşturma, yapılandırma, başlatma ve silme yönergelerini sağlar
documentationcenter: na
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.assetid: 75cfd5a2-e378-4365-99ee-a2b2abda2e0d
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/23/2018
ms.author: victorh
ms.openlocfilehash: 70b350e228785e47a41cb83ce0d80b93c8a601c1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66135237"
---
# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb"></a>İç yük dengeleyici (ILB) ile bir uygulama ağ geçidi oluşturma

Azure Application Gateway İnternet’e yönelik bir VIP veya İnternet’e sunulmamış iç yük dengeleyici uç noktası olarak da bilinen iç uç nokta ile yapılandırılabilir. Ağ geçidini bir ILB ile yapılandırma İnternet’e sunulmamış iç iş kolu uygulamaları için kullanışlıdır. Güvenlik sınırı içinde bulunan, İnternet’e sunulmamış ancak hala hepsini bir kez deneme yük dağıtımı, oturum sürekliliği veya Güvenli Yuva Katmanı (SLL) sonlandırması gerektiren çok katmanlı uygulamalar içindeki hizmetler ve katmanlar için de kullanışlıdır.

Bu makale, ILB ile uygulama ağ geçidi yapılandırma adımlarında size yol gösterir.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Aşağıdaki Azure PowerShell modülünün en son sürümünü yüklemek [yükleme yönergeleri](/powershell/azure/install-az-ps).
2. Application Gateway için bir sanal ağ ve bir alt ağ oluşturacaksınız. Hiçbir sanal makinenin veya bulut dağıtımlarının alt ağı kullanmadığından emin olun. Application Gateway tek başına bir sanal ağ alt ağında olmalıdır.
3. Uygulama ağ geçidi kullanırken yapılandırdığınız sunucular mevcut olmalıdır veya uç noktaları sanal ağda veya atanan genel bir IP/VIP’de oluşturulmuş olmalıdır.

## <a name="what-is-required-to-create-an-application-gateway"></a>Bir uygulama ağ geçidi oluşturmak için ne gereklidir?

* **Arka uç sunucu havuzu:** Arka uç sunucularının IP adresleri listesi. Listede bulunan IP adresleri, uygulama ağ geçidi için farklı alt ağa sahip sanal ağ alt ağına veya genel IP/VIP’ye ait olmalıdır.
* **Arka uç sunucu havuzu ayarları:** Her havuzun bağlantı noktası, protokol ve tanımlama bilgisi temelli benzeşim gibi ayarları vardır. Bu ayarlar bir havuza bağlıdır ve havuzdaki tüm sunuculara uygulanır.
* **Ön uç bağlantı noktası:** Bu bağlantı noktası uygulama ağ geçidinde açılan genel bağlantı noktasıdır. Bu bağlantı noktasında trafik olursa arka uç sunuculardan birine yönlendirilir.
* **Dinleyici:** Dinleyicinin sahip bir ön uç bağlantı noktası, bir protokol (Http veya Https, bu duyarlıdır) ve SSL sertifika adı (SSL yük boşaltımı yapılandırılıyorsa).
* **Kural:** Kural, dinleyiciyi ve arka uç sunucusu havuzunu bağlar ve için bir Dinleyicide trafik olduğunda trafiğin yönlendirileceği hangi arka uç sunucu havuzuna yönlendirileceğini belirler. Şu anda yalnızca *temel* kural desteklenmektedir. *Temel* kural hepsini bir kez deneme yöntemiyle yük dağıtımıdır.

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Azure Klasik ve Azure Resource Manager’ın kullanımı arasındaki fark, uygulama ağ geçidi oluştururken takip ettiğiniz sıra ve yapılandırılması gereken öğelerdir.
Resource Manager’da uygulama ağ geçidini oluşturan öğeler ayrı ayrı yapılandırılır ve sonra uygulama ağ geçidi kaynağı oluşturmak için bir araya getirilir.

Uygulama ağ geçidi oluşturmak için takip etmeniz gereken adımlar şunlardır:

1. Resource Manager için kaynak grubu oluşturun
2. Uygulama ağ geçidi için bir sanal ağ ve bir alt ağ oluşturun
3. Uygulama ağ geçidi yapılandırma nesnesi oluşturun
4. Bir uygulama ağ geçidi kaynağı oluşturma

## <a name="create-a-resource-group-for-resource-manager"></a>Resource Manager için kaynak grubu oluşturun

Azure Resource Manager cmdlet’lerini kullanmak için PowerShell modunu açtığınızdan emin olun. Daha fazla bilgi için bkz.[Resource Manager ile Windows PowerShell Kullanma](../powershell-azure-resource-manager.md)

### <a name="step-1"></a>Adım 1

```powershell
Connect-AzAccount
```

### <a name="step-2"></a>Adım 2

Hesapla ilişkili abonelikleri kontrol edin.

```powershell
Get-AzSubscription
```

Kimlik bilgilerinizle kimliğinizi doğrulamanız istenir.

### <a name="step-3"></a>Adım 3

Hangi Azure aboneliğinizin kullanılacağını seçin.

```powershell
Select-AzSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a>4. Adım

Yeni bir kaynak grubu oluşturun (mevcut bir kaynak grubu kullanıyorsanız bu adımı atlayın).

```powershell
New-AzResourceGroup -Name appgw-rg -location "West US"
```

Azure Resource Manager, tüm kaynak gruplarının bir konum belirtmesini gerektirir. Bu, kaynak grubundaki kaynaklar için varsayılan konum olarak kullanılır. Uygulama ağ geçidi oluşturmak için verilen komutların aynı kaynak grubunu kullandığından emin olun.

Önceki örnekte, "Appgw-rg" "Batı ABD" konumlu adlı bir kaynak grubu oluşturduk.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Uygulama ağ geçidi için bir sanal ağ ve bir alt ağ oluştur

Aşağıdaki örnek Resource Manager kullanarak nasıl sanal ağ oluşturulacağını gösterir:

### <a name="step-1"></a>Adım 1

```powershell
$subnetconfig = New-AzVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

Bu adım, 10.0.0.0/24 adres aralığını bir sanal ağ oluşturmak için kullanılacak bir alt ağ değişkenine atar.

### <a name="step-2"></a>Adım 2

```powershell
$vnet = New-AzVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig
```

Bu adım, 10.0.0.0/24 alt ağıyla önek 10.0.0.0/16 kullanarak Batı ABD bölgesi için "appgw-rg" kaynak grubunda "appgwvnet" adlı bir sanal ağ oluşturur.

### <a name="step-3"></a>Adım 3

```powershell
$subnet = $vnet.subnets[0]
```

Bu adım, alt ağ nesnesini sonraki adımlar için $subnet değişkenine atar.

## <a name="create-an-application-gateway-configuration-object"></a>Uygulama ağ geçidi yapılandırma nesnesi oluşturun

### <a name="step-1"></a>Adım 1

```powershell
$gipconfig = New-AzApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

Bu adım, "Gatewayıp01" adlı uygulama ağ geçidi IP yapılandırması oluşturur. Application Gateway başladığında, yapılandırılan alt ağdan bir IP adresi alır ve ağ trafiğini arka uç IP havuzundaki IP adreslerine yönlendirir. Her örneğin bir IP adresi aldığını göz önünde bulundurun.

### <a name="step-2"></a>Adım 2

```powershell
$pool = New-AzApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.1.1.8,10.1.1.9,10.1.1.10
```

Bu adım adlı arka uç IP adresi havuzunu yapılandırır. "pool01" IP adresleri "10.1.1.8, 10.1.1.9, 10.1.1.10". Bu adresler, ön uç IP uç noktasından gelen ağ trafiğinin yönlendirildiği IP adresleridir. Kendi uygulamanızın IP adresi uç noktalarını eklemek için önceki IP adreslerini değiştirin.

### <a name="step-3"></a>Adım 3

```powershell
$poolSetting = New-AzApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

Bu adım, uygulama "dengeli poolsetting01" ağ geçidi yük ağ trafiğini arka uç havuzunda yapılandırır.

### <a name="step-4"></a>4. Adım

```powershell
$fp = New-AzApplicationGatewayFrontendPort -Name frontendport01  -Port 80
```

Bu adım, ILB için "frontendport01" adlı ön uç IP bağlantı noktasını yapılandırır.

### <a name="step-5"></a>5. Adım

```powershell
$fipconfig = New-AzApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet
```

Bu adım, "fipconfig01" adlı ön uç IP yapılandırmasını oluşturur ve geçerli sanal ağ alt ağından özel IP ile ilişkilendirir.

### <a name="step-6"></a>6. Adım

```powershell
$listener = New-AzApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp
```

Bu adım, "listener01" adlı dinleyiciyi oluşturur ve ön uç bağlantı noktasıyla ön uç IP yapılandırmasını ilişkilendirir.

### <a name="step-7"></a>7. Adım

```powershell
$rule = New-AzApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

Bu adım yük dengeleyici davranışını yapılandıran "rule01" adlı Yük Dengeleyiciyi yönlendirme kuralını oluşturur.

### <a name="step-8"></a>8. Adım

```powershell
$sku = New-AzApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

Bu adım, uygulama ağ geçidinin örnek boyutunu yapılandırır.

> [!NOTE]
> Kapasite için varsayılan değer 2'dir. SKU adı için aynı zamanda Standard_Small, Standard_Medium ve Standard_Large arasında seçebilirsiniz.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>New-AzureApplicationGateway kullanarak bir uygulama ağ geçidi oluşturma

Önceki adımlarda geçen tüm yapılandırma öğeleri ile bir uygulama ağ geçidi oluşturur. Bu örnekte uygulama ağ geçidi "appgwtest" olarak adlandırılmıştır.

```powershell
$appgw = New-AzApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

Bu adım, önceki adımlarda geçen tüm yapılandırma öğeleri ile bir uygulama ağ geçidi oluşturur. Örnekte uygulama ağ geçidi "appgwtest" olarak adlandırılmıştır.

## <a name="delete-an-application-gateway"></a>Uygulama ağ geçidini silme

Bir uygulama ağ geçidini silmek için aşağıdaki adımları sırasıyla uygulamanız gerekir:

1. Ağ geçidini durdurmak için `Stop-AzApplicationGateway` cmdlet’ini kullanın.
2. Ağ geçidini kaldırmak için `Remove-AzApplicationGateway` cmdlet’ini kullanın.
3. Ağ geçidinin kaldırıldığını doğrulamak için `Get-AzureApplicationGateway` cmdlet’ini kullanın.

### <a name="step-1"></a>Adım 1

Uygulama ağ geçidi nesnesini alın ve "$getgw" değişkenine ilişkilendirin.

```powershell
$getgw =  Get-AzApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

### <a name="step-2"></a>Adım 2

Uygulama ağ geçidini sonlandırmak için `Stop-AzApplicationGateway` hizmetini kullanın. Bu örnek, gösterir `Stop-AzApplicationGateway` cmdlet'i ilk satırdaki devamında girdinin.

```powershell
Stop-AzApplicationGateway -ApplicationGateway $getgw  
```

```
VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8
```

Uygulama ağ geçidi durdurulmuş konumda olduğunda, hizmeti kaldırmak için `Remove-AzApplicationGateway` cmdlet’ini kullanın.

```powershell
Remove-AzApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Force
```

```
VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301
```

> [!NOTE]
>  **-force** anahtarı, kaldırma onayı iletisini gizlemek için kullanılabilir.

Hizmetin kaldırıldığını doğrulamak için `Get-AzApplicationGateway` cmdlet’ini kullanabilirsiniz. Bu adım gerekli değildir.

```powershell
Get-AzApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

```
VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
```

## <a name="next-steps"></a>Sonraki adımlar

SSL yük boşaltmayı yapılandırmak istiyorsanız, bkz. [SSL yük boşaltımı için uygulama ağ geçidi yapılandırma](application-gateway-ssl.md).

Bir uygulama ağ geçidini bir ILB ile kullanmak için yapılandırmak istiyorsanız, bkz. [İç yük dengeleyiciyle bir uygulama ağ geçidi oluşturma](application-gateway-ilb.md).

Yük dengeleme seçenekleri hakkında daha fazla genel bilgi edinmek istiyorsanız, bkz.

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

