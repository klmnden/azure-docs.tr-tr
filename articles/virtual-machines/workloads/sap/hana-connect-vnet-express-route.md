---
title: SAP hana (büyük örnekler) azure'da sanal ağ üzerinden bağlantı kurulumu | Microsoft Docs
description: Bağlantı kurulumu sanal ağdan SAP HANA (büyük örnekler) Azure üzerinde kullanın.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 071db2d9aeda2373c85ae62c47bbef175dcb7678
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58483412"
---
# <a name="connect-a-virtual-network-to-hana-large-instances"></a>HANA büyük örnekleri için bir sanal ağı bağlama

Bir Azure sanal ağı oluşturduktan sonra o ağ üzerinde Azure büyük örneklerde SAP HANA için bağlanabilirsiniz. Bir Azure ExpressRoute ağ geçidi sanal ağ oluşturun. Bu ağ geçidi müşteri kiracısında büyük örneği damgasında bağlandığı ExpressRoute bağlantı hattına sanal ağa bağlamanıza olanak sağlar.

> [!NOTE] 
> Bu adımın tamamlanması 30 dakika sürebilir. Yeni ağ geçidine atanan bir Azure aboneliği oluşturulur ve ardından belirtilen Azure sanal ağa bağlı.

Bir ağ geçidi zaten varsa, bir ExpressRoute ağ geçidi olup olmadığını denetleyin. Aksi durumda, ağ geçidini silin ve bir ExpressRoute ağ geçidi olarak yeniden oluşturun. Bir ExpressRoute ağ geçidiyle zaten varsa, bu makalenin "Bağlantısı sanal ağlar." aşağıdaki bölüme bakın 

- Hangisini [Azure portalında](https://portal.azure.com/) veya bir ExpressRoute VPN ağ geçidi oluşturmak için PowerShell, sanal ağınıza bağlı.
  - Azure portalını kullanıyorsanız, yeni bir ekleme **sanal ağ geçidi**ve ardından **ExpressRoute** ağ geçidi türü.
  - PowerShell kullanıyorsanız, indirmeniz ve en günceli kullan [Azure PowerShell SDK'sı](https://azure.microsoft.com/downloads/). Aşağıdaki komutlar bir ExpressRoute ağ geçidi oluşturur. Metinleri öncesinde bir _$_ özel bilgileri ile güncelleştirilmesi kullanıcı tanımlı değişkenler.

```powershell
# These Values should already exist, update to match your environment
$myAzureRegion = "eastus"
$myGroupName = "SAP-East-Coast"
$myVNetName = "VNet01"

# These values are used to create the gateway, update for how you wish the GW components to be named
$myGWName = "VNet01GW"
$myGWConfig = "VNet01GWConfig"
$myGWPIPName = "VNet01GWPIP"
$myGWSku = "HighPerformance" # Supported values for HANA large instances are: HighPerformance or UltraPerformance

# These Commands create the Public IP and ExpressRoute Gateway
$vnet = Get-AzureRmVirtualNetwork -Name $myVNetName -ResourceGroupName $myGroupName
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
New-AzureRmPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName `
-Location $myAzureRegion -AllocationMethod Dynamic
$gwpip = Get-AzureRmPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name $myGWConfig -SubnetId $subnet.Id `
-PublicIpAddressId $gwpip.Id

New-AzureRmVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName -Location $myAzureRegion `
-IpConfigurations $gwipconfig -GatewayType ExpressRoute `
-GatewaySku $myGWSku -VpnType PolicyBased -EnableBgp $true
```

Bu örnekte, yüksek performanslı ağ geçidi SKU'sunu kullanıldı. Seçenekleriniz (büyük örnekler) Azure üzerinde SAP HANA için desteklenen tek ağ geçidi SKU'ları olarak HighPerformance veya UltraPerformance şunlardır.

> [!IMPORTANT]
> HANA büyük örnekler Type II sınıfının SKU, UltraPerformance ağ geçidi SKU'su kullanmanız gerekir.

## <a name="link-virtual-networks"></a>Bağlantıyı sanal ağlar

Azure sanal ağı bir ExpressRoute ağ geçidi artık sahiptir. SAP HANA (büyük örnekler) Azure ExpressRoute bağlantı hattı ExpressRoute ağ geçidine bağlanmak için Microsoft tarafından sağlanan yetkilendirme bilgileri kullanın. Azure portal veya PowerShell kullanarak bağlantı kurabilir. Portal önerilir, ancak PowerShell kullanmak istiyorsanız, yönergeler aşağıda belirtilmiştir. 

Her bağlantı için farklı bir AuthGUID kullanarak her sanal ağ geçidi için aşağıdaki komutları çalıştırın. Aşağıdaki kodda gösterilen ilk iki girişe Microsoft tarafından sağlanan bilgileri gelir. Ayrıca, her sanal ağ ve ağ geçidini AuthGUID özeldir. Başka bir Azure sanal ağı eklemek istiyorsanız, HANA büyük örnekleri azure'a bağlayan ExpressRoute bağlantı hattı için başka bir AuthID almanız gerekir. 

```powershell
# Populate with information provided by Microsoft Onboarding team
$PeerID = "/subscriptions/9cb43037-9195-4420-a798-f87681a0e380/resourceGroups/Customer-USE-Circuits/providers/Microsoft.Network/expressRouteCircuits/Customer-USE01"
$AuthGUID = "76d40466-c458-4d14-adcf-3d1b56d1cd61"

# Your ExpressRoute Gateway information
$myGroupName = "SAP-East-Coast"
$myGWName = "VNet01GW"
$myGWLocation = "East US"

# Define the name for your connection
$myConnectionName = "VNet01GWConnection"

# Create a new connection between the ER Circuit and your Gateway using the Authorization
$gw = Get-AzureRmVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName
    
New-AzureRmVirtualNetworkGatewayConnection -Name $myConnectionName `
-ResourceGroupName $myGroupName -Location $myGWLocation -VirtualNetworkGateway1 $gw `
-PeerId $PeerID -ConnectionType ExpressRoute -AuthorizationKey $AuthGUID
```

Aboneliğinizle ilişkili birden fazla ExpressRoute bağlantı hattı ağ geçidine bağlanmak için bu adımı birden çok kez çalıştırmanız gerekebilir. Örneğin, aynı sanal ağ geçidi sanal ağı şirket içi ağınıza bağlanır ExpressRoute bağlantı hattına bağlamak için büyük olasılıkla dağıtacağız.

## <a name="next-steps"></a>Sonraki adımlar

- [HLI için ek donanım gereksinimleri](hana-additional-network-requirements.md)