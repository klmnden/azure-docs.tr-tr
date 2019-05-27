---
title: 'Zorlamalı tünel Azure siteden siteye bağlantıları için yapılandırın: Resource Manager | Microsoft Docs'
description: Nasıl yeniden yönlendirme veya 'tüm İnternet'e bağlı trafiği şirket içi konumunuza geri force'.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: cbe58db8-b598-4c9f-ac88-62c865eb8721
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2018
ms.author: cherylmc
ms.openlocfilehash: b4d9a469e46d964055d9459901ebdb9c6d04cf24
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66157477"
---
# <a name="configure-forced-tunneling-using-the-azure-resource-manager-deployment-model"></a>Azure Resource Manager dağıtım modelini kullanarak zorlamalı tünel yapılandırma

Zorlamalı tünel yeniden yönlendirme veya tüm İnternet'e bağlı trafiği, inceleme ve denetim için bir siteden siteye VPN tüneli aracılığıyla şirket içi konumunuza geri "force" sağlar. Bu, çoğu kurumsal BT için kritik güvenlik gereksinimdir ilkeleri. Olmadan tünel oluşturma, İnternet'e bağlı trafiği azure'daki sanal makinelerinize her zaman Azure ağ altyapısından doğrudan ilişkilerinden geçen inceleme veya trafiği denetlemek için izin verme seçeneği olmadan Internet'e zorlandı. Yetkisiz Internet erişimi, bilgilerin açıklanması veya diğer tür güvenlik ihlallerini olası neden olabilir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Bu makalede, zorlamalı tünel Resource Manager dağıtım modeli kullanılarak oluşturulmuş sanal ağlar için nasıl yapılandıracağınız anlatılmaktadır. Zorlamalı tünel, portal üzerinden değil, PowerShell kullanarak yapılandırılabilir. Klasik makale, Klasik dağıtım modeli için zorlamalı tünel yapılandırmak istiyorsanız, aşağıdaki açılır listeden seçin:

> [!div class="op_single_selector"]
> * [PowerShell - Klasik](vpn-gateway-about-forced-tunneling.md)
> * [PowerShell - Resource Manager](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="about-forced-tunneling"></a>Zorlamalı tünel hakkında

Aşağıdaki diyagram, nasıl zorlamalı tünel çalıştığını gösterir. 

![Zorlamalı Tünel Oluşturma](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

Yukarıdaki örnekte, ön uç alt ağı değil zorlamalı tünel uygulanmaz. Frontend alt iş yüklerinin kabul etmek ve doğrudan Internet'ten müşteri isteklerine yanıt vermek devam edebilirsiniz. Orta katman ve arka uç alt ağları zorlamalı tünel. Bu iki alt ağa giden tüm bağlantılarından İnternet'e zorlamalı veya bir şirket içi sitede bir S2S VPN tünelleri aracılığıyla yeniden.

Bu kısıtlama ve sanal makinelerinizdeki Internet erişimi denetleme veya çok katmanlı bir hizmet Mimarinizi gerekli etkinleştirmek devam ederken bulut Hizmetleri, azure'da sağlar. Sanal ağlarınızda hiçbir Internet'e yönelik iş yükü varsa, tüm sanal ağları için zorlamalı tünel de uygulayabilirsiniz.

## <a name="requirements-and-considerations"></a>Gereksinimleri ve konular

Zorlamalı tünel Azure'da sanal ağ kullanıcı tanımlı yollar yapılandırılır. Bir şirket içi siteye trafik yönlendirme Azure VPN ağ geçidi için varsayılan bir yol olarak ifade edilir. Kullanıcı tanımlı Yönlendirme ve sanal ağlar hakkında daha fazla bilgi için bkz: [kullanıcı tanımlı yollar ve IP iletme](../virtual-network/virtual-networks-udr-overview.md).

* Her sanal ağ alt ağı, yerleşik bir sistem yönlendirme tablosu vardır. Sistem yönlendirme tablosu yolların aşağıdaki üç grup vardır:
  
  * **Yerel sanal ağ yolları:** Doğrudan hedefe Vm'leri aynı sanal ağda.
  * **Şirket içi yolları:** Azure VPN ağ geçidi için.
  * **Varsayılan yol:** Doğrudan Internet'e. Önceki iki yol tarafından kapsandığından değil özel IP adreslerini hedefleyen paketler bırakılır.
* Bu yordam bu alt ağlarda zorlamalı tüneli etkinleştirmek için sanal ağ alt ağı, başarılı için varsayılan bir yol ekleyin ve sonra yönlendirme tablosunu ilişkilendirme için bir yönlendirme tablosu oluşturmak için kullanıcı tanımlı yollar (UDR) kullanır.
* Zorlamalı tünel rota tabanlı VPN ağ geçidi olan bir sanal ağ ile ilişkilendirilmiş olması gerekir. "Sanal ağa bağlı bir varsayılan site" şirket içi yerel siteleri arasında ayarlamanız gerekir. Ayrıca, şirket içi VPN cihazının 0.0.0.0/0 trafik seçicileri kullanılarak yapılandırılması gerekir. 
* Zorlamalı tünel ExpressRoute Bu mekanizma yapılandırılmamış, ancak bunun yerine, bir varsayılan rota üzerinden ExpressRoute BGP eşliği oturumlarını reklam tarafından etkinleştirilir. Daha fazla bilgi için [ExpressRoute belgeleri](https://azure.microsoft.com/documentation/services/expressroute/).

## <a name="configuration-overview"></a>Yapılandırmasına genel bakış

Aşağıdaki yordam bir kaynak grubunu ve sanal ağ oluşturmanıza yardımcı olur. Ardından, bir VPN gateway oluşturma ve zorlamalı tünel yapılandırma. Bu yordamda, sanal ağ 'MultiTier-VNet' üç alt sahiptir: 'Ön uç', 'Midtier' ve 'Backend' dört ile şirketler arası bağlantılar: 'DefaultSiteHQ' ve üç dalları.

Yordamdaki adımları 'zorlamalı tünel DefaultSiteHQ' varsayılan site bağlantı olarak ayarlayın ve 'Midtier' yapılandırmak ve zorlamalı tünel kullanmak için 'Backend' alt ağlar.

## <a name="before"></a>Başlamadan önce

Azure Resource Manager PowerShell cmdlet'lerinin en son sürümünü yükleyin. PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

> [!IMPORTANT]
> PowerShell cmdlet'lerinin en son sürümü yükleme gereklidir. Aksi takdirde, bazı cmdlet'ler çalışırken, doğrulama hataları alabilirsiniz.
>
>

### <a name="to-log-in"></a>Oturum açmak için

[!INCLUDE [To log in](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="configure-forced-tunneling"></a>Zorlamalı tünel yapılandırma

> [!NOTE]
> "Bu cmdlet'in çıktısı nesne türü gelecekteki bir sürümde değiştirilecek" belirten bir uyarı görebilirsiniz. Bu beklenen bir davranıştır ve bu uyarılar güvenle yok sayabilirsiniz.
>
>


1. Bir kaynak grubu oluşturun.

   ```powershell
   New-AzResourceGroup -Name 'ForcedTunneling' -Location 'North Europe'
   ```
2. Sanal ağ oluşturma ve alt ağları belirtin.

   ```powershell 
   $s1 = New-AzVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
   $s2 = New-AzVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
   $s3 = New-AzVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
   $s4 = New-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
   $vnet = New-AzVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4
   ```
3. Yerel ağ geçidi oluşturun.

   ```powershell
   $lng1 = New-AzLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
   $lng2 = New-AzLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
   $lng3 = New-AzLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
   $lng4 = New-AzLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
   ```
4. Yol tablosu ve yol kuralı oluşturun.

   ```powershell
   New-AzRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
   $rt = Get-AzRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
   Add-AzRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
   Set-AzRouteTable -RouteTable $rt
   ```
5. Midtier ve arka uç alt ağların yol tablosuna ilişkilendirin.

   ```powershell
   $vnet = Get-AzVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
   Set-AzVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
   Set-AzVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
   Set-AzVirtualNetwork -VirtualNetwork $vnet
   ```
6. Sanal ağ geçidi oluşturun. Bu adım, oluşturma ve ağ geçidini yapılandırma olduğundan, bazen 45 dakika veya daha fazla tamamlanması biraz zaman alabilir. GatewaySKU değeri ilgili ValidateSet hatalar görürseniz, yüklediğinizden emin olun [PowerShell cmdlet'lerinin en son sürümünü](#before). PowerShell cmdlet'lerinin en son sürümünü en son ağ geçidi SKU'ları için yeni doğrulanmış değerlerini içerir.

   ```powershell
   $pip = New-AzPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
   $gwsubnet = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
   $ipconfig = New-AzVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
   New-AzVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -EnableBgp $false
   ```
7. Varsayılan site için sanal ağ geçidi atayabilirsiniz. **- GatewayDefaultSite** çalışır, bu nedenle bu ayarı düzgün şekilde yapılandırmak için ilgileniriz zorla yönlendirme yapılandırması sağlar cmdlet'i parametredir. 

   ```powershell
   $LocalGateway = Get-AzLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling"
   $VirtualGateway = Get-AzVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
   Set-AzVirtualNetworkGatewayDefaultSite -GatewayDefaultSite $LocalGateway -VirtualNetworkGateway $VirtualGateway
   ```
8. Siteden siteye VPN bağlantıları kurun.

   ```powershell
   $gateway = Get-AzVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
   $lng1 = Get-AzLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" 
   $lng2 = Get-AzLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" 
   $lng3 = Get-AzLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" 
   $lng4 = Get-AzLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" 
    
   New-AzVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng1 -ConnectionType IPsec -SharedKey "preSharedKey"
   New-AzVirtualNetworkGatewayConnection -Name "Connection2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng2 -ConnectionType IPsec -SharedKey "preSharedKey"
   New-AzVirtualNetworkGatewayConnection -Name "Connection3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng3 -ConnectionType IPsec -SharedKey "preSharedKey"
   New-AzVirtualNetworkGatewayConnection -Name "Connection4" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng4 -ConnectionType IPsec -SharedKey "preSharedKey"
    
   Get-AzVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling"
   ```
