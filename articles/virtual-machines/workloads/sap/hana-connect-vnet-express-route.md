---
title: Bağlantı (büyük örnekler) Azure üzerinde SAP HANA için sanal ağdan ayarlama | Microsoft Docs
description: Bağlantı sanal ağdan SAP HANA (büyük örnekler) Azure'da kullanacak şekilde ayarlanmış.
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
ms.date: 05/25/2019
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9cd290f9e5e7819f3dffa2dd1efea9cd0028fc2c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66239461"
---
# <a name="connect-a-virtual-network-to-hana-large-instances"></a>HANA büyük örnekleri için bir sanal ağı bağlama

Bir Azure sanal ağı oluşturduktan sonra o ağ üzerinde Azure büyük örneklerde SAP HANA için bağlanabilirsiniz. Bir Azure ExpressRoute ağ geçidi sanal ağ oluşturun. Bu ağ geçidi müşteri kiracısında HANA büyük örneği damgasında bağlandığı ExpressRoute bağlantı hattına sanal ağa bağlamanıza olanak sağlar.

> [!NOTE] 
> Bu adımın tamamlanması 30 dakika sürebilir. Yeni ağ geçidine atanan bir Azure aboneliği oluşturulur ve ardından belirtilen Azure sanal ağa bağlı.

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

Bir ağ geçidi zaten varsa, bir ExpressRoute ağ geçidi olup olmadığını denetleyin. Bir ExpressRoute ağ geçidi değilse, ağ geçidini silin ve bir ExpressRoute ağ geçidi olarak yeniden oluşturun. Bir ExpressRoute ağ geçidiyle zaten varsa, bu makalenin "Bağlantısı sanal ağlar." aşağıdaki bölüme bakın 

- Hangisini [Azure portalında](https://portal.azure.com/) veya bir ExpressRoute VPN ağ geçidi oluşturmak için PowerShell, sanal ağınıza bağlı.
  - Azure portalını kullanıyorsanız, yeni bir ekleme **sanal ağ geçidi**ve ardından **ExpressRoute** ağ geçidi türü.
  - PowerShell kullanıyorsanız, indirmeniz ve en günceli kullan [Azure PowerShell SDK'sı](https://azure.microsoft.com/downloads/). 
 
Aşağıdaki komutlar bir ExpressRoute ağ geçidi oluşturur. Metinleri öncesinde bir _$_ kullanıcı tanımlı değişkenler, özel bilgileri ile güncelleştirilmelidir.

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
$vnet = Get-AzVirtualNetwork -Name $myVNetName -ResourceGroupName $myGroupName
$subnet = Get-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
New-AzPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName `
-Location $myAzureRegion -AllocationMethod Dynamic
$gwpip = Get-AzPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName
$gwipconfig = New-AzVirtualNetworkGatewayIpConfig -Name $myGWConfig -SubnetId $subnet.Id `
-PublicIpAddressId $gwpip.Id

New-AzVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName -Location $myAzureRegion `
-IpConfigurations $gwipconfig -GatewayType ExpressRoute `
-GatewaySku $myGWSku -VpnType PolicyBased -EnableBgp $true
```

Bu örnekte, yüksek performanslı ağ geçidi SKU'sunu kullanıldı. Seçenekleriniz (büyük örnekler) Azure üzerinde SAP HANA için desteklenen tek ağ geçidi SKU'ları olarak HighPerformance veya UltraPerformance şunlardır.

> [!IMPORTANT]
> HANA büyük örnekler Type II sınıfının SKU, UltraPerformance ağ geçidi SKU'su kullanmanız gerekir.

## <a name="link-virtual-networks"></a>Bağlantıyı sanal ağlar

Azure sanal ağı bir ExpressRoute ağ geçidi artık sahiptir. SAP HANA büyük örnekleri ExpressRoute bağlantı hattı ExpressRoute ağ geçidine bağlanmak için Microsoft tarafından sağlanan yetkilendirme bilgileri kullanın. Azure portal veya PowerShell kullanarak bağlantı kurabilir. PowerShell yönergeleri aşağıdaki gibidir. 

Her bağlantı için farklı bir AuthGUID kullanarak her bir ExpressRoute ağ geçidi için aşağıdaki komutları çalıştırın. Aşağıdaki kodda gösterilen ilk iki girişe Microsoft tarafından sağlanan bilgileri gelir. Ayrıca, her sanal ağ ve ağ geçidini AuthGUID özeldir. Başka bir Azure sanal ağı eklemek istiyorsanız, Microsoft'un sunduğu HANA büyük örnekleri azure'a bağlayan ExpressRoute bağlantı hattı için başka bir AuthID almanız gerekir. 

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
$gw = Get-AzVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName
    
New-AzVirtualNetworkGatewayConnection -Name $myConnectionName `
-ResourceGroupName $myGroupName -Location $myGWLocation -VirtualNetworkGateway1 $gw `
-PeerId $PeerID -ConnectionType ExpressRoute -AuthorizationKey $AuthGUID -ExpressRouteGatewayBypass
```

> [!NOTE]
> New-AzVirtualNetworkGatewayConnection komut son parametresinde **ExpressRouteGatewayBypass** ExpressRoute hızlı yol sağlayan yeni bir parametredir. Azure Vm'leri ve HANA büyük örneği birimleri arasındaki ağ gecikmesini azaltır işlevselliği. İşlevsellik Mayıs 2019'içinde Daha fazla ayrıntı için makale onay [SAP HANA (büyük örnekler) ağ mimarisi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture). Komutları çalıştırmadan önce PowerShell cmdlet'lerinin en son sürümünü çalıştırdığınızdan emin olun.

Aboneliğinizle ilişkili birden fazla ExpressRoute bağlantı hattı ağ geçidine bağlanmak için bu adımı birden çok kez çalıştırmanız gerekebilir. Örneğin, aynı sanal ağ geçidi sanal ağı şirket içi ağınıza bağlanır ExpressRoute bağlantı hattına bağlamak için büyük olasılıkla dağıtacağız.

## <a name="applying-expressroute-fast-path-to-existing-hana-large-instance-expressroute-circuits"></a>ExpressRoute hızlı yolu mevcut HANA büyük örneği ExpressRoute bağlantı hatları için uygulama
Şu ana kadar belgeleri bir HANA büyük örnek dağıtım için bir Azure ExpressRoute ağ geçidi bir Azure sanal ağlarınıza ile oluşturulan yeni bir ExpressRoute bağlantı hattına bağlama açıklanmıştır. Ancak birçok müşteri, ExpressRoute devreleri Kurulumu zaten zaten sahip ve sanal ağlarını HANA büyük örnekler için zaten bağlı. Yeni ExpressRoute hızlı yolu, ağ gecikme süresini azaltma gibi bu işlevselliği kullanmak için değişiklik uygulamanız önerilir. Yeni bir ExpreesRoute devre bağlanmak ve mevcut bir ExpressRoute bağlantı hattı değiştirmek için komutları aynıdır. Sonuç olarak bu dizi kullanmak için mevcut bir devreyi değiştirmek için PowerShell komutlarını çalıştırmak ihtiyacınız 

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
$gw = Get-AzVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName
    
New-AzVirtualNetworkGatewayConnection -Name $myConnectionName `
-ResourceGroupName $myGroupName -Location $myGWLocation -VirtualNetworkGateway1 $gw `
-PeerId $PeerID -ConnectionType ExpressRoute -AuthorizationKey $AuthGUID -ExpressRouteGatewayBypass
```

ExpressRoute hızlı yolu işlevselliğini etkinleştirmek için yukarıda görüntülenen son parametre eklemek önemlidir


## <a name="expressroute-global-reach"></a>ExpressRoute küresel erişim
Global erişim birini veya ikisini de iki senaryo için etkinleştirmek istediğiniz gibi kullanın:

 - Herhangi bir ek proxy'leri veya güvenlik duvarlarını olmadan HANA sistem çoğaltması
 - Sistem kopyaları veya sistem yenileme gerçekleştirmek için iki farklı bölgede HANA büyük örneği birimleri arasında kopyalama yedeklemeleri

dikkate almanız:

- Bir/29 bir adres alanı aralığı sağlamanız gereken adres alanı. Adres aralığı kadar HANA büyük örnekleri Azure'a bağlanan kullanılan ve değil çakışabilir başka adres alanı aralıklarıyla herhangi biriyle IP adresi aralıklarınızı, hiçbiriyle değil, Azure'da veya şirket içinde başka bir yerde kullanılır.
- HANA büyük örnekleri, şirket içi yolları tanıtmak için kullanılan Asn'ler (Otonom sistem numarası) bir sınırlama yoktur. Şirket içi özel Asn'ler 65000 – 65020 aralığında olan herhangi bir yol veya 65515 bildirilmeyecek gerekir. 
- Şirket içi doğrudan erişim HANA büyük örneklerine bağlanma senaryosu için Azure'a bağlanan bağlantı hattı için bir ücret hesaplamak gerekir. Fiyatlar için fiyatı denetleyin [genel ulaşmak eklenti](https://azure.microsoft.com/pricing/details/expressroute/).

Dağıtımınız için uygulanan senaryoları biri veya almak için bir destek iletisini Azure ile açıklandığı açın [HANA büyük örnekleri için bir destek isteği açın](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-li-portal#open-a-support-request-for-hana-large-instances)

Gerekli veri ve Microsoft Yönlendirme ve isteğiniz üzerinde yürütmek için kullanmanız gereken anahtar sözcükler gibi görünür:

- Hizmet: SAP HANA Büyük Örneği
- Sorun türü: Yapılandırma ve Kurulum
- Sorun alt tür: Sorunum yukarıdaki listede değil
- 'Global erişim Ekle - ağım Değiştir' konu
- Ayrıntılar: ' Global erişim HANA büyük örneği için HANA büyük örneği kiracıya ekleyin veya ' Ekle genel şirket içi HANA büyük örneği kiracıya ulaşın.
- HANA büyük örneği Kiracı çalışması için HANA büyük örneği için ek ayrıntılar: Tanımlamanız gereken **iki Azure bölgesi** bağlanmak için iki Kiracı yerleştirildiği **ve** göndermeniz gerekir   **/29 IP adresi aralığı**
- Şirket içi HANA büyük örneği Kiracı çalışması için ek ayrıntılar: Tanımlamanız gereken **Azure bölgesi** doğrudan bağlanmak istediğiniz burada HANA büyük örneği Kiracı dağıtılır. Ayrıca sağlamanız gereken **Auth GUID** ve **bağlantı hattını Eş Kimliği** ExpressRoute devreniz şirket içi ile Azure arasında kurulan zaman aldı. Ek olarak, ad gerekir, **ASN**. Son teslim edilebilir olduğu bir **/29 IP adresi aralığı** ExpressRoute Global erişim için.

> [!NOTE]
> İşlenen her iki durumda olmasını istiyorsanız, farklı iki girilmesi gerekiyor/çakışmadığından 29 IP adresi aralıklarını içeren herhangi bir IP adres aralığı kadar kullanılır. 




## <a name="next-steps"></a>Sonraki adımlar

- [HLI için ek donanım gereksinimleri](hana-additional-network-requirements.md)
