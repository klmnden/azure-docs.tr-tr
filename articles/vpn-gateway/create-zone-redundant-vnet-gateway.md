---
title: Azure kullanılabilirlik alanları bölgesel olarak yedekli sanal ağ geçidi oluşturma | Microsoft Docs
description: VPN Gateway ve ExpressRoute ağ geçitleri kullanılabilirlik alanlarında dağıtma
services: vpn-gateway
author: cherylmc
Customer intent: As someone with a basic network background, I want to understand how to create zone-redundant gateways.
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: cherylmc
ms.openlocfilehash: f2b459ccfd7e3f513b9b6526864321ce247ae7aa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60405877"
---
# <a name="create-a-zone-redundant-virtual-network-gateway-in-azure-availability-zones"></a>Azure kullanılabilirlik alanları bölgesel olarak yedekli sanal ağ geçidi oluşturma

Azure kullanılabilirlik alanları, VPN ve ExpressRoute ağ geçitleri dağıtabilirsiniz. Bu seçenek, dayanıklılık, ölçeklenebilirlik ve yüksek kullanılabilirlik için sanal ağ geçitleri getirir. Ağ geçitleri Azure kullanılabilirlik alanları, fiziksel ve mantıksal olarak dağıtma, ağ geçitleri bir bölge içinde bölge düzeyinde hatalardan Azure'a, şirket içi ağ bağlantısını korurken ayırır. Bilgi için [bölgesel olarak yedekli sanal ağ geçitleri hakkında](about-zone-redundant-vnet-gateways.md) ve [Azure kullanılabilirlik alanları hakkında](../availability-zones/az-overview.md).

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Yerel olarak yüklü bilgisayarınızda veya Azure Cloud Shell'i PowerShell kullanabilirsiniz. PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu özelliği PowerShell modülünün en son sürümünü gerektirir.

[!INCLUDE [Cloud shell](../../includes/vpn-gateway-cloud-shell-powershell.md)]

### <a name="to-use-powershell-locally"></a>PowerShell'i yerel olarak kullanma

Yerine PowerShell, bilgisayarınızda yerel olarak kullanıyorsanız, Cloud Shell'i kullanmak, PowerShell modülü 1.0.0 yüklemeniz gerekir ya da daha yüksek. Yüklü PowerShell sürümü denetlemek için aşağıdaki komutu kullanın:

```azurepowershell
Get-Module Az -ListAvailable | Select-Object -Property Name,Version,Path
```

Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps).

[!INCLUDE [PowerShell login](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="variables"></a>1. Değişkenlerinizi bildirme

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

## <a name="configure"></a>2. Sanal ağ oluşturma

Bir kaynak grubu oluşturun.

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName $RG1 -Location $Location1
```

Sanal ağ oluşturun.

```azurepowershell-interactive
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name $FESubnet1 -AddressPrefix $FEPrefix1
$besub1 = New-AzVirtualNetworkSubnetConfig -Name $BESubnet1 -AddressPrefix $BEPrefix1
$vnet = New-AzVirtualNetwork -Name $VNet1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNet1Prefix -Subnet $fesub1,$besub1
```

## <a name="gwsub"></a>3. Ağ geçidi alt ağı ekleme

Ağ geçidi alt ağı sanal ağ geçidi hizmetlerinin kullandığı ayrılmış IP adreslerini içerir. Ekleme ve bir ağ geçidi alt ağı ayarlamak için aşağıdaki örnekleri kullanın:

Ağ geçidi alt ağını ekleyin.

```azurepowershell-interactive
$getvnet = Get-AzVirtualNetwork -ResourceGroupName $RG1 -Name VNet1
Add-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.1.255.0/27 -VirtualNetwork $getvnet
```

Sanal ağ geçidi alt ağ yapılandırmasını ayarlayın.

```azurepowershell-interactive
$getvnet | Set-AzVirtualNetwork
```
## <a name="publicip"></a>4. Genel bir IP adresi talep etme
 
Bu adımda, oluşturmak istediğiniz ağ geçidi için geçerli olan yönergeler seçin. Ağ geçidi dağıtmak için bölge seçimini bölgeler için genel IP adresi belirtilen bağlıdır.

### <a name="ipzoneredundant"></a>Bölgesel olarak yedekli ağ geçitleri için

Genel bir IP adresi ile istek bir **standart** Publicıpaddress SKU ve herhangi bir bölge belirtmeyin. Bu durumda, oluşturulan standart genel IP adresini, bölgesel olarak yedekli genel IP olacaktır.   

```azurepowershell-interactive
$pip1 = New-AzPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard
```

### <a name="ipzonalgw"></a>İçin bölgesel ağ geçitleri

Genel bir IP adresi ile istek bir **standart** Publicıpaddress SKU. Bölgeyi (1, 2 veya 3) belirtin. Tüm ağ geçidi örnekleri, bu bölgeye dağıtılır.

```azurepowershell-interactive
$pip1 = New-AzPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard -Zone 1
```

### <a name="ipregionalgw"></a>İçin bölgesel ağ geçitleri

Genel bir IP adresi ile istek bir **temel** Publicıpaddress SKU. Bu durumda, ağ geçidi bölgesel bir ağ geçidi olarak dağıtılır ve ağ geçidinde yerleşik bölge artıklığı sahip değil. Ağ geçidi örnekleri tüm bölgelerde sırasıyla oluşturulur.

```azurepowershell-interactive
$pip1 = New-AzPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Dynamic -Sku Basic
```
## <a name="gwipconfig"></a>5. IP yapılandırması oluştur

```azurepowershell-interactive
$getvnet = Get-AzVirtualNetwork -ResourceGroupName $RG1 -Name $VNet1
$subnet = Get-AzVirtualNetworkSubnetConfig -Name $GwSubnet1 -VirtualNetwork $getvnet
$gwipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GwIPConf1 -Subnet $subnet -PublicIpAddress $pip1
```

## <a name="gwconfig"></a>6. Ağ geçidi oluşturma

Sanal ağ geçidi oluşturun.

### <a name="for-expressroute"></a>ExpressRoute için

```azurepowershell-interactive
New-AzVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType ExpressRoute
```

### <a name="for-vpn-gateway"></a>VPN ağ geçidi

```azurepowershell-interactive
New-AzVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType Vpn -VpnType RouteBased
```

## <a name="faq"></a>SSS

### <a name="what-will-change-when-i-deploy-these-new-skus"></a>Bu yeni SKU'lara dağıtabilirim olduğunda ne değişecek mi?

Açısından bakıldığında, ağ geçitlerinizi bölge yedekliliği ile dağıtabilirsiniz. Bu, ağ geçitleri tüm örneklerini Azure kullanılabilirlik alanları genelinde dağıtılacak ve her kullanılabilirlik alanı farklı hata ve güncelleme etki alanı olduğu anlamına gelir. Bu ağ geçitlerinizi daha güvenilir, kullanılabilir ve dayanıklı bölge hatalarını sağlar.

### <a name="can-i-use-the-azure-portal"></a>Azure portalını kullanabilir miyim?

Evet, yeni SKU'lara dağıtmak için Azure portalını kullanabilirsiniz. Ancak, bu yeni SKU'lara Azure kullanılabilirlik alanları olan bu Azure bölgelerindeki görürsünüz.

### <a name="what-regions-are-available-for-me-to-use-the-new-skus"></a>Hangi bölgeler benim için yeni SKU'lara kullanmak kullanılabilir mi?

Bkz: [kullanılabilirlik](../availability-zones/az-overview.md#services-support-by-region) en son kullanılabilir bölgelerin listesi için.

### <a name="can-i-changemigrateupgrade-my-existing-virtual-network-gateways-to-zone-redundant-or-zonal-gateways"></a>Kullanabilir miyim Değiştir/geçiş/yükseltmesi mevcut sanal ağ geçitlerim bölgesel olarak yedekli ya da bölgesel ağ geçitleri?

Bölgesel olarak yedekli ya da bölgesel ağ geçitleri için var olan sanal ağ geçitlerinizi geçişi şu anda desteklenmiyor. Bununla birlikte, mevcut ağ geçidinizi silin ve bölgesel olarak yedekli ya da bölgesel bir ağ geçidi yeniden oluşturun.

### <a name="can-i-deploy-both-vpn-and-express-route-gateways-in-same-virtual-network"></a>Ben, aynı sanal ağdaki VPN hem Expressroute ağ geçitleri dağıtabilir miyim?

Aynı sanal ağda hem VPN hem de Express Route ağ geçitlerinin aynı anda var olmalarına desteklenir. Bununla birlikte, / 27 ayırmalısınız. ağ geçidi alt ağı için IP adresi aralığı.
