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
ms.openlocfilehash: 5efdda485e4e1f5013948c6636b267f0d388f4d5
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44164765"
---
# <a name="connecting-a-vnet-to-hana-large-instance-expressroute"></a>HANA büyük örnek ExpressRoute için sanal ağ bağlama

Tüm IP adresi aralıklarını tanımlanan ve artık veri Microsoft'tan alındı, HANA büyük örnekler için daha önce oluşturduğunuz sanal ağa bağlanma başlatabilirsiniz. Azure sanal ağı oluşturulduktan sonra sanal ağ üzerinde büyük örneği damgasında müşteri kiracısında bağlandığı ExpressRoute bağlantı hattına bağlamak için VNet üzerinde bir ExpressRoute ağ geçidi oluşturulmalıdır.

> [!NOTE] 
> Bu adım, yeni ağ geçidine atanan bir Azure aboneliği oluşturulur ve ardından belirtilen Azure Vnet'e bağlı olarak tamamlanması 30 dakika sürebilir.

Bir ağ geçidi zaten varsa, bir ExpressRoute ağ geçidi olup olmadığını denetleyin. Aksi halde ağ geçidi silinmesi ve yeniden oluşturulması bir ExpressRoute ağ geçidi olarak. Azure sanal ağı ExpressRoute bağlantı hattı şirket içi bağlantı için zaten bağlı olduğundan bir ExpressRoute ağ geçidiyle zaten varsa, sanal ağları bağlama bölümüne geçin.

- Kullanın ya da (yeni) [Azure portalında](https://portal.azure.com/), veya bir ExpressRoute VPN ağ geçidi oluşturmak için PowerShell, sanal ağınıza bağlı.
  - Azure portalını kullanıyorsanız, yeni bir ekleme **sanal ağ geçidi** seçip **ExpressRoute** ağ geçidi türü.
  - Bunun yerine PowerShell seçerseniz, indirmeniz ve en günceli kullan [Azure PowerShell SDK'sı](https://azure.microsoft.com/downloads/) bir en iyi deneyimi sağlamak için. Aşağıdaki komutlar bir ExpressRoute ağ geçidi oluşturur. Metinleri öncesinde bir _$_ özel bilgileri ile güncelleştirilmesi gereken kullanıcı tanımlı değişkenler.

```PowerShell
# These Values should already exist, update to match your environment
$myAzureRegion = "eastus"
$myGroupName = "SAP-East-Coast"
$myVNetName = "VNet01"

# These values are used to create the gateway, update for how you wish the GW components to be named
$myGWName = "VNet01GW"
$myGWConfig = "VNet01GWConfig"
$myGWPIPName = "VNet01GWPIP"
$myGWSku = "HighPerformance" # Supported values for HANA Large Instances are: HighPerformance or UltraPerformance

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

Bu örnekte, yüksek performanslı ağ geçidi SKU'sunu kullanıldı. Seçenekleriniz olarak yalnızca ağ geçidi SKU'ları (büyük örnekler) Azure üzerinde SAP HANA için desteklenen yüksek performanslı veya UltraPerformance şunlardır.

> [!IMPORTANT]
> HANA büyük örnekleri için SKU türü II classs, UltraPerformance ağ geçidi SKU'ların kullanımı zorunludur.

**Sanal ağları bağlama**

Azure sanal ağı bir ExpressRoute ağ geçidi var, bu bağlantı için oluşturulan Azure (büyük örnekler) ExpressRoute bağlantı hattı üzerinde SAP HANA ExpressRoute ağ geçidine bağlanmak için Microsoft tarafından sağlanan yetkilendirme bilgileri kullanın. Bu adım, Azure portal veya PowerShell kullanarak gerçekleştirilebilir. Portal önerilir ancak PowerShell yönergeleri aşağıda belirtilmiştir. 

- Her bağlantı için farklı bir AuthGUID kullanarak her VNet ağ geçidi için aşağıdaki komutları yürütün. Betik aşağıdaki ilk iki girdileri Microsoft tarafından sağlanan bilgileri gelir. Ayrıca, her sanal ağ ve ağ geçidini AuthGUID özeldir. Anlamına gelir, başka bir Azure sanal ağı eklemek istiyorsanız, HANA büyük örnekleri Azure'a bağlanan ExpressRoute bağlantı hattı için başka bir AuthID edinmeniz gerekir. 

```PowerShell
# Populate with information provided by Microsoft Onboarding team
$PeerID = "/subscriptions/9cb43037-9195-4420-a798-f87681a0e380/resourceGroups/Customer-USE-Circuits/providers/Microsoft.Network/expressRouteCircuits/Customer-USE01"
$AuthGUID = "76d40466-c458-4d14-adcf-3d1b56d1cd61"

# Your ExpressRoute Gateway Information
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

Aboneliğinizle ilişkili olan birden çok ExpressRoute bağlantı hatları ağ geçidine bağlanmak istiyorsanız, bu adımı birden çok kez çalıştırmak gerekebilir. Örneğin, sanal ağın şirket içi ağınıza bağlanır ExpressRoute bağlantı hattı aynı VNet ağ geçidine bağlanmak için büyük olasılıkla seçeceksiniz.

**Sonraki adımlar**

- Başvuru [HLI için ek donanım gereksinimleri](hana-additional-network-requirements.md).