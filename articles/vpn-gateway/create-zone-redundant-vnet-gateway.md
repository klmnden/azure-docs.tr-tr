---
title: Azure kullanılabilirlik bölgeleri - Önizleme bölge olarak yedekli sanal ağ geçidi oluşturma | Microsoft Docs
description: VPN Gateway ve ExpressRoute ağ geçidi kullanılabilirliği bölgelerde - Önizleme dağıtın.
services: vpn-gateway
documentationcenter: na
author: cherylmc
Customer intent: As someone with a basic network background, I want to understand how to create zone-redundant gateways.
ms.service: vpn-gateway
ms.topic: article
ms.date: 06/28/2018
ms.author: cherylmc
ms.openlocfilehash: c484358bf98f0121cfc3ce270b162b01c75b5b09
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37096242"
---
# <a name="create-a-zone-redundant-virtual-network-gateway-in-azure-availability-zones---preview"></a>Bölge olarak yedekli sanal ağ geçidi Azure kullanılabilirlik bölgeleri - Önizleme oluşturma

VPN ve ExpressRoute ağ geçitleri dağıtabilir [Azure kullanılabilirlik bölgeleri](../availability-zones/az-overview.md). Bu seçenek, dayanıklılık, ölçeklenebilirlik ve yüksek kullanılabilirlik için sanal ağ geçitlerini getirir. Fiziksel ve mantıksal ağ geçitleri Azure kullanılabilirlik bölgelerde dağıtma, ağ geçitleri bir bölge içinde şirket içi ağ bağlantınızı Azure'a bölge düzeyinde hatalarından korurken ayırır.

Zonal ve bölge olarak yedekli ağ geçitlerinde normal sanal ağ geçitleri temel performans geliştirmeleri vardır. Ayrıca, bir bölge olarak yedekli veya zonal sanal ağ geçidi oluşturma, diğer ağ geçitlerine oluşturmaktan daha hızlıdır. Take yaklaşık 15 dakika boyunca bir ExpressRoute ağ geçidi ve bir VPN ağ geçidi için 19 dakika zaman 45 dakika alma yerine oluşturun.

### <a name="zrgw"></a>Bölge olarak yedekli ağ geçitleri

Sanal ağ geçitlerini kullanılabilirlik dilimlerinde otomatik olarak dağıtmak için bölge olarak yedekli sanal ağ geçitlerini kullanabilirsiniz. Bölge olarak yedekli ağ geçitleri ile Azure ile ilgili kritik, ölçeklenebilir hizmetleriniz için erişim GA konumundaki % 99,99 çalışma süresi SLA kadar yararlanmak istiyorsanız.

<br>
<br>

![Bölge redunant ağ geçitleri grafiği](./media/create-zone-redundant-vnet-gateway/zonered.png)

### <a name="zgw"></a>Zonal ağ geçitleri

Belirli bir bölgedeki ağ geçitleri dağıtmak için zonal ağ geçitleri kullanın. Zonal bir ağ geçidi dağıttığınızda, her iki ağ geçidi örneği aynı kullanılabilirlik bölgede dağıtılır.

<br>
<br>

![zonal ağ geçitleri grafiği](./media/create-zone-redundant-vnet-gateway/zonal.png)

## <a name="gwskus"></a>Ağ Geçidi SKU'ları

Bölge olarak yedekli ve zonal ağ geçitleri, yeni ağ geçidi SKU'ları kullanmanız gerekir. Bir kez, [kendi kendinize kaydetmeniz önizlemede](#enroll), yeni sanal ağ geçidi SKU'ları Azure AZ bölgeler tümünde görürsünüz. Bölge olarak yedekli ve zonal ağ geçitleri belirli dışında bu SKU'ları ExpressRoute ve VPN ağ geçidi için karşılık gelen SKU'ları benzerdir.

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

Bölge olarak yedekli ağ geçitleri ve zonal ağ geçitlerini kullanan Azure genel IP kaynağı *standart* SKU. Azure genel IP kaynağı yapılandırması, dağıttığınız ağ geçidi bölge yedekli olup olmadığını belirler veya zonal. Genel IP kaynağı ile oluşturursanız, bir *temel* SKU, ağ geçidi sahip olmaz hiçbir bölge artıklık ve ağ geçidi kaynakları bölgesel olacaktır.

### <a name="pipzrg"></a>Bölge olarak yedekli ağ geçitleri

Ortak IP adresi kullanarak bir oluşturduğunuzda **standart** davranışı bir bölge belirtmeden ortak IP SKU, farklı ağ geçidi bir VPN ağ geçidi veya ExpressRoute ağ geçidi olmasına bağlı olarak. 

* Bir VPN ağ geçidi için iki ağ geçidi örnekleri bölge artıklık sağlamak için bu üç bölgeler dışında tüm 2'deki dağıtılır. 
* Olabilir beri ikiden fazla örnekleri, bir ExpressRoute ağ geçidi için ağ geçidi üç dilimlerinde yayılabilir.

### <a name="pipzg"></a>Zonal ağ geçitleri

Ortak IP adresi kullanarak bir oluşturduğunuzda **standart** ortak IP SKU ve bölge (1, 2 veya 3) belirtin, tüm ağ geçidi örnekleri aynı bölgede dağıtılır.

### <a name="piprg"></a>Bölgesel ağ geçitleri

Ortak IP adresi kullanarak bir oluşturduğunuzda **temel** ortak IP SKU, ağ geçidi bölgesel bir ağ geçidi olarak dağıtılır ve ağ geçidine yerleşik bölge artıklık yok.

## <a name="before-you-begin"></a>Başlamadan önce

Yerel bilgisayarınızda veya Azure bulut Kabuk üzerinde yüklü ya da PowerShell kullanabilirsiniz. Bu özellik yüklemek ve PowerShell yerel olarak kullanmak seçerseniz, PowerShell modülü en son sürümünü gerektirir.

[!INCLUDE [Cloud shell](../../includes/vpn-gateway-cloud-shell-powershell.md)]

### <a name="to-use-powershell-locally"></a>PowerShell yerel olarak kullanmak için

Yerine PowerShell, bilgisayarınızda yerel olarak kullanıyorsanız, bulut Kabuğu'nu kullanarak, PowerShell modülü 6.1.1 yüklemelisiniz ya da daha yüksek. Yüklediğiniz PowerShell sürümünü denetlemek için aşağıdaki komutu kullanın:

```azurepowershell
Get-Module AzureRM -ListAvailable | Select-Object -Property Name,Version,Path
```

Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps).

[!INCLUDE [PowerShell login](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="enroll"></a>1. Önizlemede kaydetme

Bölge olarak yedekli veya zonal bir ağ geçidi yapılandırmadan önce ilk önizleme aboneliğinizde Self kaydetmelisiniz. Aboneliğinizi sağlandıktan sonra yeni ağ geçidi SKU'ları Azure AZ bölgeler tümünde görmek başlar. 

Azure hesabınızda oturum ve bu Önizleme için beyaz liste ile istediğiniz abonelik kullandığınızdan emin olun. Kaydetmek için aşağıdaki örneği kullanın:

```azurepowershell-interactive
Register-AzureRmProviderFeature -FeatureName AllowVMSSVirtualNetworkGateway -ProviderNamespace Microsoft.Network
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

'AllowVMSSVirtualNetworkGateway' özelliği, aboneliğiniz ile kaydedildiğini doğrulamak için aşağıdaki komutu kullanın:

```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace Microsoft.Network
```

Sonuç bu örneğe benzer görünür:

![sağlanan](./media/create-zone-redundant-vnet-gateway/verifypreview.png)

## <a name="variables"></a>2. Değişkenlerinizi bildirme

Örnek adımlar için kullanılan değerleri aşağıda listelenmiştir. Ayrıca, bazı örnekler bildirilen değişkenler adımları kullanın. Bu adımları, kendi ortamınızda kullanıyorsanız, bu değerleri kendinizinkilerle değiştirildiğinden emin olun. Konumu belirtirken, belirttiğiniz bölge desteklendiğinden emin olun. Daha fazla bilgi için bkz: [SSS](#faq).

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

Ağ geçidi alt ağı sanal ağ geçidi hizmetlerini kullanmak ayrılmış IP adresleri içerir. Aşağıdaki örnekler eklemek ve bir ağ geçidi alt ağı belirlemek için kullanın:

Ağ geçidi alt ağını ekleyin.

```azurepowershell-interactive
$getvnet = Get-AzureRmVirtualNetwork -ResourceGroupName $RG1 -Name VNet1
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.1.255.0/27 -VirtualNetwork $getvnet
```

Sanal ağ için ağ geçidi alt ağ yapılandırması ayarlayın.

```azurepowershell-interactive
$getvnet | Set-AzureRmVirtualNetwork
```
## <a name="publicip"></a>5. Genel bir IP adresi talep etme
 
Bu adımda, oluşturmak istediğiniz ağ geçidi için geçerli olan yönergeleri seçin. Ağ geçitleri dağıtmak için bölgeler seçimi için genel IP adresi belirtilen bölgeleri bağlıdır.

### <a name="ipzoneredundant"></a>Bölge olarak yedekli ağ geçitleri için

Bir ortak IP adresiyle isteği bir **standart** Publicıpaddress SKU ve herhangi bir bölgeye belirtmeyin. Bu durumda, oluşturulan standart ortak IP adresi bölge olarak yedekli bir genel IP olacaktır.   

```azurepowershell-interactive
$pip1 = New-AzureRmPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard
```

### <a name="ipzonalgw"></a>Zonal ağ geçitleri için

Bir ortak IP adresiyle isteği bir **standart** Publicıpaddress SKU. (1, 2 veya 3) dilimini belirtin. Tüm ağ geçidi örnekleri bu bölgeye dağıtılır.

```azurepowershell-interactive
$pip1 = New-AzureRmPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard -Zone 1
```

### <a name="ipregionalgw"></a>İçin bölgesel ağ geçitleri

Bir ortak IP adresiyle isteği bir **temel** Publicıpaddress SKU. Bu durumda, ağ geçidi bölgesel bir ağ geçidi olarak dağıtılır ve ağ geçidine yerleşik bölge artıklık yok. Ağ geçidi örnekleri tüm bölgelerde sırasıyla oluşturulur.

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

>[!NOTE]
>Bu aşamada, ağ geçidi SKU'su belirtemezsiniz. SKU otomatik olarak ErGw1AZ için ExpressRoute ve VpnGw1AZ VPN ağ geçidi için varsayılan olarak alır.
>

### <a name="for-expressroute"></a>ExpressRoute için

```azurepowershell-interactive
New-AzureRmVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType ExpressRoute
```

### <a name="for-vpn-gateway"></a>VPN ağ geçidi için

```azurepowershell-interactive
New-AzureRmVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType Vpn -VpnType RouteBased
```

## <a name="feedback"></a>Geribildirim sağlama

Görüşleriniz bizim için önemlidir. E-posta Gönder aznetworkgateways@microsoft.com sorunları veya bölge olarak yedekli ve zonal VPN ve Expressroute ağ geçitleri için (olumlu veya olumsuz) görüş bildirin. "[]" Şirket adınızı konu satırında içerir. Bir sorun bildiriyorsanız, abonelik kimliği de içerir.

## <a name="faq"></a>SSS

### <a name="how-do-i-sign-up-for-the-preview"></a>Nasıl Önizleme için kaydolabilirim?

Yapabilecekleriniz [kendi kendinize kaydetmeniz](#enroll) bu makalede PowerShell komutlarını kullanarak.

### <a name="what-will-change-when-i-enroll"></a>Cihazımı kaydettiğimde ne değişir mi?

Açısından bakıldığında, Önizleme süresince, ağ geçitleri bölge artıklık ile dağıtabilirsiniz. Bu, ağ geçitleri tüm örneklerini Azure kullanılabilirlik dilimlerinde dağıtılır ve farklı bir arıza ve güncelleştirme etki her kullanılabilirlik bölgedir anlamına gelir. Bu, ağ geçitleri daha güvenilir, kullanılabilir ve esnek bölge hatalarını hale getirir.

### <a name="what-regions-are-available-for-the-preview"></a>Önizleme için hangi bölgelerde kullanılabilir?

Bölge olarak yedekli ve zonal ağ geçitleri üretim/Azure ortak bölgelerde kullanılabilir.

### <a name="will-i-be-billed-for-participating-in-this-preview"></a>I bu önizlemede katılmak için Fatura edilecek?

Size, ağ geçitleri için Önizleme sırasında Fatura edilecek değil. Ancak, dağıtımınıza bağlı hiçbir SLA yoktur. Geri bildiriminiz işitme çok ilgi duyuyoruz.

> [!NOTE]
> ExpressRoute ağ geçidi için ağ geçidi faturalandırılır ve Ücret değil. Ancak, bağlantı hattı kendisini (ağ geçidi değil) Fatura edilecek.

### <a name="what-regions-are-available-for-me-to-try-this-in"></a>Benim bu denemek için hangi bölgelerde kullanılabilir?

Genel Önizleme Orta ABD ve Fransa merkezi bölgelerde (genel olarak kullanılabilir kullanılabilirlik bölgeleri Azure bölgeleri) kullanılabilir. İleride, bölge olarak yedekli ağ geçitleri kullanılabilir, diğer Azure ortak bölgelerde yapacağız.

### <a name="can-i-change-my-existing-virtual-network-gateways-to-zone-redundant-or-zonal-gateways"></a>Bölge olarak yedekli veya zonal ağ geçitleri için varolan my sanal ağ geçitlerini değiştirebilir miyim?

Bölge olarak yedekli veya zonal ağ geçitleri, var olan sanal ağ geçitlerine geçirme şu anda desteklenmiyor. Ancak, mevcut ağ geçidini silin ve bölge olarak yedekli veya zonal bir ağ geçidi yeniden oluşturun.

### <a name="can-i-deploy-both-vpn-and-express-route-gateways-in-same-virtual-network"></a>Aynı sanal ağda hem VPN hem de hızlı rota ağ geçitleri dağıtabilir miyim?

VPN ve Expressroute ağ geçitlerinin aynı sanal ağda birlikte bulunma genel Önizleme sırasında desteklenir. Ancak, aşağıdaki gereksinimleri ve sınırlamaları dikkat edin:

* Ayırma /27 bir ağ geçidi alt ağı için IP adresi aralığı.
* Bölge-yedek/zonal hızlı rota ağ geçitleri yalnızca bölge-yedek/zonal VPN ağ geçitleri ile birlikte var olabilir.
* Bölge-yedek/zonal VPN ağ geçidi dağıtmadan önce bölge-yedek/zonal Expressroute ağ geçidi dağıtın.
* Bir bölge-yedek/zonal Expressroute ağ geçidi için en fazla 4 devreler bağlanabilir.

## <a name="next-steps"></a>Sonraki adımlar

Görüşleriniz bizim için önemlidir. E-posta Gönder aznetworkgateways@microsoft.com sorunları veya bölge olarak yedekli ve zonal VPN ve Expressroute ağ geçitleri için (olumlu veya olumsuz) görüş bildirin. "[]" Şirket adınızı konu satırında içerir. Bir sorun bildiriyorsanız, abonelik kimliği de içerir.
