---
title: Bölgesel olarak yedekli sanal ağ geçidi oluşturma Azure kullanılabilirlik alanları - Önizleme | Microsoft Docs
description: VPN Gateway ve ExpressRoute ağ geçitleri kullanılabilirlik alanlarında - Preview'ı dağıtın.
services: vpn-gateway
documentationcenter: na
author: cherylmc
Customer intent: As someone with a basic network background, I want to understand how to create zone-redundant gateways.
ms.service: vpn-gateway
ms.topic: article
ms.date: 07/09/2018
ms.author: cherylmc
ms.openlocfilehash: fa349555a5effd41ca519cbd5a29005203d79543
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952564"
---
# <a name="create-a-zone-redundant-virtual-network-gateway-in-azure-availability-zones---preview"></a>Azure kullanılabilirlik alanları - Önizleme bölgesel olarak yedekli sanal ağ geçidi oluşturma

VPN ve ExpressRoute ağ geçitleri dağıtabilir [Azure kullanılabilirlik alanları](../availability-zones/az-overview.md). Bu seçenek, dayanıklılık, ölçeklenebilirlik ve yüksek kullanılabilirlik için sanal ağ geçitleri getirir. Ağ geçitleri Azure kullanılabilirlik alanları, fiziksel ve mantıksal olarak dağıtma, ağ geçitleri bir bölge içinde bölge düzeyinde hatalardan Azure'a, şirket içi ağ bağlantısını korurken ayırır.

Bölgesel ve bölgesel olarak yedekli ağ geçitleri normal sanal ağ geçitleri temel performans geliştirmeleri vardır. Ayrıca, bölgesel olarak yedekli ya da bölgesel sanal ağ geçidi oluşturma, diğer ağ geçitleri oluşturmaktan daha hızlıdır. Sınav zamanı yaklaşık 15 dakika boyunca bir ExpressRoute ağ geçidi ve VPN ağ geçidi için 19 dakika zaman 45 dakika sürüyor yerine oluşturun.

### <a name="zrgw"></a>Bölgesel olarak yedekli ağ geçitleri

Sanal ağ geçitlerinizi kullanılabilirlik alanları genelinde otomatik olarak dağıtmak için bölgesel olarak yedekli sanal ağ geçitlerini de kullanabilirsiniz. Bölgesel olarak yedekli ağ geçitleri ile % 99,99 çalışma süresi SLA'sı, azure'da görev açısından kritik, ölçeklenebilir hizmetleriniz için erişim GA çekirdeğinden yararlanarak.

<br>
<br>

![Bölge redunant ağ geçitleri grafiği](./media/create-zone-redundant-vnet-gateway/zonered.png)

### <a name="zgw"></a>Bölgesel ağ geçitleri

Belirli bir bölgedeki ağ geçidi dağıtmak için bölgesel ağ geçidi kullanın. Bölgesel bir ağ geçidi dağıttığınızda, her iki ağ geçidi örneklerini aynı kullanılabilirlik alanında dağıtılır.

<br>
<br>

![Bölgesel ağ geçitleri grafiği](./media/create-zone-redundant-vnet-gateway/zonal.png)

## <a name="gwskus"></a>Ağ Geçidi SKU'ları

Bölgesel olarak yedekli ve bölgesel ağ geçitleri için yeni ağ geçidi SKU'ları kullanmanız gerekir. Sonra [kendi kendinize kaydetmeniz önizlemesinde](#enroll), yeni sanal ağ geçidi SKU'ları Azure AZ bölgeleri tümünde görürsünüz. Bölgesel olarak yedekli ve bölgesel ağ geçitleri için belirli şeylerdir bu SKU'ları ilgili SKU'ları için ExpressRoute ve VPN Gateway de benzerdir.

Yeni ağ geçidi SKU'ları şunlardır:

### <a name="vpn-gateway"></a>VPN Gateway

* VpnGw1AZ
* VpnGw2AZ
* VpnGw3AZ

### <a name="expressroute"></a>ExpressRoute

* ErGw1AZ
* ErGw2AZ
* ErGw3AZ

## <a name="pipskus"></a>Genel IP SKU'ları

Bölgesel olarak yedekli ağ geçitleri ve bölgesel ağ geçitlerini kullanan Azure genel IP kaynağı üzerinde *standart* SKU. Azure genel IP kaynağı yapılandırmasını dağıttığınız ağ geçidi bölge yedekli olup olmadığını belirler veya bölgesel. Bir genel IP kaynağı oluşturursanız, bir *temel* SKU, ağ geçidi herhangi bir bölge artıklığı olmaz ve ağ geçidi kaynakları bölgesel olacaktır.

### <a name="pipzrg"></a>Bölgesel olarak yedekli ağ geçitleri

Genel bir IP adresi kullanarak oluşturduğunuzda **standart** bölge belirtmeden genel IP SKU'su, davranışı farklı ağ geçidi bir VPN ağ geçidi veya ExpressRoute ağ geçidi olduğuna bağlı olarak. 

* Bir VPN ağ geçidi için iki ağ geçidi örnekleri herhangi 2 bölgesi yedeklilik sağlamak için bu üç bölgeler dışında dağıtılır. 
* İkiden fazla örnekleri olabileceği bir ExpressRoute ağ geçidi için ağ geçidi üç tüm bölgeler arasında yayılabilir.

### <a name="pipzg"></a>Bölgesel ağ geçitleri

Genel bir IP adresi kullanarak oluşturduğunuzda **standart** genel IP SKU'su ve bölgeyi (1, 2 veya 3) belirtin, tüm ağ geçidi örnekleri aynı bölgede dağıtılır.

### <a name="piprg"></a>Bölgesel ağ geçitleri

Genel bir IP adresi kullanarak oluşturduğunuzda **temel** genel IP SKU'su, ağ geçidi bölgesel bir ağ geçidi olarak dağıtılır ve ağ geçidinde yerleşik bölge artıklığı sahip değil.

## <a name="before-you-begin"></a>Başlamadan önce

Yerel olarak yüklü bilgisayarınızda veya Azure Cloud Shell'i PowerShell kullanabilirsiniz. PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu özelliği PowerShell modülünün en son sürümünü gerektirir.

[!INCLUDE [Cloud shell](../../includes/vpn-gateway-cloud-shell-powershell.md)]

### <a name="to-use-powershell-locally"></a>PowerShell'i yerel olarak kullanma

Yerine PowerShell, bilgisayarınızda yerel olarak kullanıyorsanız, Cloud Shell'i kullanmak, PowerShell modülü 6.1.1 yüklemeniz gerekir ya da daha yüksek. Yüklü PowerShell sürümü denetlemek için aşağıdaki komutu kullanın:

```azurepowershell
Get-Module AzureRM -ListAvailable | Select-Object -Property Name,Version,Path
```

Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps).

[!INCLUDE [PowerShell login](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="enroll"></a>1. Önizlemede kaydetme

Bölgesel olarak yedekli ya da bölgesel bir ağ geçidi yapılandırmadan önce ilk önizleme aboneliğinizde Self kaydetmelisiniz. Aboneliğinizi sağlandıktan sonra yeni ağ geçidi SKU'ları Azure AZ bölgeleri tümünde görmeye başlarsınız. 

Azure hesabınızda oturum açmış ve bu Önizleme için beyaz listeye almak istediğiniz aboneliği kullanarak emin olun. Kaydetmek için aşağıdaki örneği kullanın:

```azurepowershell-interactive
Register-AzureRmProviderFeature -FeatureName AllowVMSSVirtualNetworkGateway -ProviderNamespace Microsoft.Network
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

'AllowVMSSVirtualNetworkGateway' özelliği, aboneliğiniz ile kaydedildiğini doğrulamak için aşağıdaki komutu kullanın:

```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace Microsoft.Network
```

Sonuçları şu örneğe benzer görünecektir:

![sağlanan](./media/create-zone-redundant-vnet-gateway/verifypreview.png)

## <a name="variables"></a>2. Değişkenlerinizi bildirme

Örnek adımları için kullanılan değerleri aşağıda listelenmiştir. Ayrıca, bazı örneklerde adımları içinde bildirilen değişkenleri kullanır. Kendi ortamınızda adımları kullanıyorsanız, bu değerleri kendi değerlerinizle değiştirdiğinizden emin olun. Konumu belirtirken, belirttiğiniz bölgelerin desteklendiğini doğrulayın. Daha fazla bilgi için [SSS](#faq).

```azurepowershell-interactive
$RG1         = "TestRG1"
$VNet1       = "VNet1"
$Location1   = "CentralUS"
$FESubnet1   = "FrontEnd"
$BESubnet1   = "Backend"
$GwSubnet1   = "GatewaySubnet"
$VNet1Prefix = "10.1.0.0/16"
$FEPrefix1   = "10.1.0.0/24"
$BEPrefix1   = "10.1.1.0/24"
$GwPrefix1   = "10.1.255.0/27"
$Gw1         = "VNet1GW"
$GwIP1       = "VNet1GWIP"
$GwIPConf1   = "gwipconf1"
```

## <a name="configure"></a>3. Sanal ağ oluşturma

Bir kaynak grubu oluşturun.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName $RG1 -Location $Location1
```

Sanal ağ oluşturun.

```azurepowershell-interactive
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubnet1 -AddressPrefix $FEPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubnet1 -AddressPrefix $BEPrefix1
$vnet = New-AzureRmVirtualNetwork -Name $VNet1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNet1Prefix -Subnet $fesub1,$besub1
```

## <a name="gwsub"></a>4. Ağ geçidi alt ağı ekleme

Ağ geçidi alt ağı sanal ağ geçidi hizmetlerinin kullandığı ayrılmış IP adreslerini içerir. Ekleme ve bir ağ geçidi alt ağı ayarlamak için aşağıdaki örnekleri kullanın:

Ağ geçidi alt ağını ekleyin.

```azurepowershell-interactive
$getvnet = Get-AzureRmVirtualNetwork -ResourceGroupName $RG1 -Name VNet1
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.1.255.0/27 -VirtualNetwork $getvnet
```

Sanal ağ geçidi alt ağ yapılandırmasını ayarlayın.

```azurepowershell-interactive
$getvnet | Set-AzureRmVirtualNetwork
```
## <a name="publicip"></a>5. Genel bir IP adresi talep etme
 
Bu adımda, oluşturmak istediğiniz ağ geçidi için geçerli olan yönergeler seçin. Ağ geçidi dağıtmak için bölge seçimini bölgeler için genel IP adresi belirtilen bağlıdır.

### <a name="ipzoneredundant"></a>Bölgesel olarak yedekli ağ geçitleri için

Genel bir IP adresi ile istek bir **standart** Publicıpaddress SKU ve herhangi bir bölge belirtmeyin. Bu durumda, oluşturulan standart genel IP adresini, bölgesel olarak yedekli genel IP olacaktır.   

```azurepowershell-interactive
$pip1 = New-AzureRmPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard
```

### <a name="ipzonalgw"></a>İçin bölgesel ağ geçitleri

Genel bir IP adresi ile istek bir **standart** Publicıpaddress SKU. Bölgeyi (1, 2 veya 3) belirtin. Tüm ağ geçidi örnekleri, bu bölgeye dağıtılır.

```azurepowershell-interactive
$pip1 = New-AzureRmPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard -Zone 1
```

### <a name="ipregionalgw"></a>İçin bölgesel ağ geçitleri

Genel bir IP adresi ile istek bir **temel** Publicıpaddress SKU. Bu durumda, ağ geçidi bölgesel bir ağ geçidi olarak dağıtılır ve ağ geçidinde yerleşik bölge artıklığı sahip değil. Ağ geçidi örnekleri tüm bölgelerde sırasıyla oluşturulur.

```azurepowershell-interactive
$pip1 = New-AzureRmPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Dynamic -Sku Basic
```
## <a name="gwipconfig"></a>6. IP yapılandırması oluştur

```azurepowershell-interactive
$getvnet = Get-AzureRmVirtualNetwork -ResourceGroupName $RG1 -Name $VNet1
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $GwSubnet1 -VirtualNetwork $getvnet
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GwIPConf1 -Subnet $subnet -PublicIpAddress $pip1
```

## <a name="gwconfig"></a>7. Ağ geçidi oluşturma

Sanal ağ geçidi oluşturun.

### <a name="for-expressroute"></a>ExpressRoute için

```azurepowershell-interactive
New-AzureRmVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType ExpressRoute
```

### <a name="for-vpn-gateway"></a>VPN ağ geçidi

```azurepowershell-interactive
New-AzureRmVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType Vpn -VpnType RouteBased
```

## <a name="feedback"></a>Geri bildirim sağlama

Görüşleriniz bizim için önemlidir. Bir e-posta Gönder aznetworkgateways@microsoft.com herhangi bir sorun bildirin veya bölgesel olarak yedekli ve bölgesel VPN ve Expressroute ağ geçitleri için (pozitif veya negatif) geri bildirim sağlamak için. "[]" Şirket adınızı konu satırında içerir. Bir sorun bildiriyorsanız de abonelik Kimliğinizi içerir.

## <a name="faq"></a>SSS

### <a name="how-do-i-sign-up-for-the-preview"></a>Önizleme için nasıl kaydolabilirim?

Yapabilecekleriniz [kendi kendinize kaydetmeniz](#enroll) bu makalede PowerShell komutlarını kullanarak.

### <a name="what-will-change-when-i-enroll"></a>I kaydettiğinizde ne değişecek mi?

Açısından bakıldığında, Önizleme sırasında bölge yedekliliği ile ağ geçitlerinizi dağıtabilirsiniz. Bu, ağ geçitleri tüm örneklerini Azure kullanılabilirlik alanları genelinde dağıtılacak ve her kullanılabilirlik alanı farklı hata ve güncelleme etki alanı olduğu anlamına gelir. Bu ağ geçitlerinizi daha güvenilir, kullanılabilir ve dayanıklı bölge hatalarını sağlar.

### <a name="can-i-use-the-azure-portal"></a>Azure portalını kullanabilir miyim?

Evet, Önizleme için Azure portalını kullanabilirsiniz. Ancak, yine de PowerShell kullanarak kendi kendilerine kaydolmalarına gerekiyorsa veya portal Önizleme sırasında kullanmanız mümkün olmayacaktır.

### <a name="what-regions-are-available-for-the-preview"></a>Önizleme için hangi bölgeler mevcuttur?

Bölgesel olarak yedekli ve bölgesel ağ geçitleri üretim/Azure genel bölgelerde kullanılabilir.

### <a name="will-i-be-billed-for-participating-in-this-preview"></a>Bu önizleme sürümünde katıldığınız için faturalandırılırım?

Ağ geçitlerinizi için Önizleme sırasında faturalandırılırsınız değil. Ancak, dağıtımınıza bağlı SLA yoktur. Geri bildirimlerinizi almaktan içinde çok ilgi duyuyoruz.

> [!NOTE]
> ExpressRoute ağ geçitleri için ağ geçidi olarak faturalandırılır ve dolu değil. Ancak, bağlantı hattı kendisi (ağ geçidini değil) faturalandırılırsınız.

### <a name="what-regions-are-available-for-me-to-try-this-in"></a>Bana bu denemek için hangi bölgeler mevcuttur?

Orta ABD ve orta Fransa'da bölgelerde (kullanılabilirlik alanları genel kullanıma açık olan Azure bölgeleri) genel önizlemede kullanılabilir. Bundan sonra bölgesel olarak yedekli ağ geçitleri kullanılabilir, diğer Azure ortak bölgelerde bulunacağız.

### <a name="can-i-change-my-existing-virtual-network-gateways-to-zone-redundant-or-zonal-gateways"></a>Bölgesel olarak yedekli ya da bölgesel ağ geçitleri için var olan sanal ağ geçitlerim değiştirebilirim?

Bölgesel olarak yedekli ya da bölgesel ağ geçitleri için var olan sanal ağ geçitlerinizi geçişi şu anda desteklenmiyor. Bununla birlikte, mevcut ağ geçidinizi silin ve bölgesel olarak yedekli ya da bölgesel bir ağ geçidi yeniden oluşturun.

### <a name="can-i-deploy-both-vpn-and-express-route-gateways-in-same-virtual-network"></a>Ben, aynı sanal ağdaki VPN hem Expressroute ağ geçitleri dağıtabilir miyim?

Aynı anda var olmalarına aynı sanal ağda hem VPN hem de Express Route ağ geçidi, genel Önizleme sırasında desteklenir. Ancak, aşağıdaki gereksinimler ve sınırlamalar dikkat edin:

* Ayrılan en az/27 bir ağ geçidi alt ağı için IP adresi aralığı.
* Bölge-yedekli/bölgesel Express Route ağ geçidi ile bölgeye-yedekli/bölgesel VPN ağ geçitleri yalnızca birlikte bulunabilir.
* Bölge-yedekli/bölgesel VPN ağ geçidi dağıtmadan önce bölge-yedekli/bölgesel Express Route ağ geçidi dağıtın.
* Bölge-yedekli/bölgesel bir Express Route ağ geçidi en fazla 4 bağlantı hatları için bağlantılı olabilir.

## <a name="next-steps"></a>Sonraki adımlar

Görüşleriniz bizim için önemlidir. Bir e-posta Gönder aznetworkgateways@microsoft.com herhangi bir sorun bildirin veya bölgesel olarak yedekli ve bölgesel VPN ve Expressroute ağ geçitleri için (pozitif veya negatif) geri bildirim sağlamak için. "[]" Şirket adınızı konu satırında içerir. Bir sorun bildiriyorsanız de abonelik Kimliğinizi içerir.
