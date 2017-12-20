---
title: "Zorlamalı tünel Azure siteden siteye bağlantıları için yapılandırın: Resource Manager | Microsoft Docs"
description: "Yeniden yönlendirme veya 'force' tüm Internet'e bağlı trafik, şirket içi konumuna geri nasıl."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: cbe58db8-b598-4c9f-ac88-62c865eb8721
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/31/2017
ms.author: cherylmc
ms.openlocfilehash: cc8a3e7f2a907b1eea4ecf39df2b291b0fb8b207
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="configure-forced-tunneling-using-the-azure-resource-manager-deployment-model"></a>Azure Resource Manager dağıtım modelini kullanarak zorlamalı tünel yapılandırma

Zorlamalı tünel yeniden yönlendirme veya "tüm Internet'e bağlı trafik, denetleme ve denetim için bir siteden siteye VPN tüneli aracılığıyla şirket içi konumunuza geri zorla" olanak sağlar. Bu, çoğu kurumsal BT için kritik güvenlik gereksinimdir ilkeleri. Olmadan tünel oluşturma, Internet'e bağlı trafik, vm'lerden azure'da her zaman Internet inceleme veya trafiği denetlemek için izin verme seçeneği olmadan Azure ağ altyapısından doğrudan geçer zorlandı. Yetkisiz Internet erişimine potansiyel olarak bilgilerin açığa çıkmasına veya diğer tür güvenlik ihlallerini yol açabilir.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Bu makalede Resource Manager dağıtım modeli kullanılarak oluşturulan sanal ağlar için tünel zorlanmış'ı yapılandıracağınız anlatılmaktadır. Zorlamalı tünel, Portalı aracılığıyla değil, PowerShell kullanarak yapılandırılabilir. Klasik dağıtım modeli için zorlamalı tünel yapılandırmak istiyorsanız, Klasik makale aşağıdaki açılan listeden seçin:

> [!div class="op_single_selector"]
> * [PowerShell - Klasik](vpn-gateway-about-forced-tunneling.md)
> * [PowerShell - Resource Manager](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="about-forced-tunneling"></a>Zorlanan tünel hakkında

Aşağıdaki diyagramda, nasıl zorlamalı tünel works gösterilmektedir. 

![Zorlamalı Tünel Oluşturma](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

Yukarıdaki örnekte, ön uç alt ağı değil zorlamalı tünel. Ön uç alt iş yükleri kabul etmek ve doğrudan Internet'ten müşteri isteklerine yanıt vermek devam edebilirsiniz. Orta katman ve arka uç alt zorlamalı tünel. Bu iki alt internet tüm giden bağlantılar zorlanmış veya siteye bir şirket içi S2S VPN tünelleri biri aracılığıyla yeniden yönlendirildi.

Bu kısıtlama ve Internet erişimi, sanal makinelerden inceleme veya Bulut Hizmetleri, azure'da çok katmanlı bir hizmet Mimarinizi gerekli etkinleştirmek devam ederken olanak sağlar. Sanal ağlarınıza hiçbir Internet'e iş yükleri varsa, tüm sanal ağların zorlamalı tünel de uygulayabilirsiniz.

## <a name="requirements-and-considerations"></a>Gereksinimleri ve konular

Zorlamalı tünel Azure'da sanal ağ kullanıcı tanımlı yollar yapılandırılır. Bir şirket içi siteye trafiği yönlendirerek Azure VPN ağ geçidi için varsayılan bir yol olarak ifade edilir. Kullanıcı tanımlı Yönlendirme ve sanal ağlar hakkında daha fazla bilgi için bkz: [kullanıcı tanımlı yollar ve IP iletimini](../virtual-network/virtual-networks-udr-overview.md).

* Her sanal ağ alt yerleşik sistem yönlendirme tablosu vardır. Sistem yönlendirme tablosu yolların aşağıdaki üç grup vardır:
  
  * **Yerel VNet yollar:** doğrudan hedefe Vm'leri aynı sanal ağda.
  * **Şirket içi yollar:** için Azure VPN ağ geçidi.
  * **Varsayılan yol:** doğrudan Internet'e. Önceki iki yoldan tarafından kapsanmayan özel IP adreslerine hedeflenen paketlerin bırakılır.
* Bu yordamı bu alt ağlardaki zorlamalı tüneli etkinleştirmek için sanal ağ alt ağı için varsayılan bir yol ekleyin ve ardından yönlendirme tablosu ilişkilendirmek için bir yönlendirme tablosu oluşturmak için kullanıcı tanımlı yolları (UDR) kullanır.
* Zorlamalı tünel rota tabanlı VPN ağ geçidi sahip bir sanal ağ ile ilişkilendirilmiş olması gerekir. "Sanal ağa bağlı bir varsayılan site" şirket içi yerel siteleri arasında ayarlamanız gerekir. Ayrıca, şirket içi VPN cihazı trafiğini Seçici 0.0.0.0/0 kullanılarak yapılandırılmalıdır. 
* Zorlanan tünel ExpressRoute bu düzenek yapılandırılmamış, ancak bunun yerine, varsayılan rota ExpressRoute BGP eşliği oturumlarını üzerinden reklam tarafından etkinleştirilir. Daha fazla bilgi için bkz: [ExpressRoute belgeleri](https://azure.microsoft.com/documentation/services/expressroute/).

## <a name="configuration-overview"></a>Yapılandırmasına genel bakış

Aşağıdaki yordamı, bir kaynak grubu ve bir sanal ağ oluşturmanıza yardımcı olur. Sonra bir VPN ağ geçidi oluşturabilir ve zorlamalı tüneli yapılandırma. Bu yordamda, sanal ağ 'MultiTier-VNet' üç alt ağa sahip değil: 'Ön uç', 'Midtier' ve 'Backend' dört ile şirket içi bağlantılar: 'DefaultSiteHQ' ve üç dalları.

Yordamdaki adımları 'zorlamalı tünel DefaultSiteHQ' varsayılan site bağlantı olarak ayarlayın ve 'Midtier' yapılandırmak ve kullanmak için 'Backend' alt ağlar zorlanan tünel.

## <a name="before"></a>Başlamadan önce

Azure Resource Manager PowerShell cmdlet'lerinin en son sürümünü yükleyin. PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

> [!IMPORTANT]
> PowerShell cmdlet'lerinin en yeni sürümünü yükleme gereklidir. Aksi takdirde, bazı cmdlet'ler çalışırken doğrulama hataları alabilirsiniz.
>
>

### <a name="to-log-in"></a>Oturum açmak için

[!INCLUDE [To log in](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="configure-forced-tunneling"></a>Zorlamalı tünel yapılandırma

> [!NOTE]
> "Bu cmdlet'in çıktısı nesne türü gelecekteki bir sürümde değiştirilecek" bildiren uyarıları görebilirsiniz. Bu beklenen davranıştır ve bu uyarılar güvenle yok sayabilirsiniz.
>
>


1. Bir kaynak grubu oluşturun.

  ```powershell
  New-AzureRmResourceGroup -Name 'ForcedTunneling' -Location 'North Europe'
  ```
2. Bir sanal ağ oluşturun ve alt ağları belirtin.

  ```powershell 
  $s1 = New-AzureRmVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
  $s2 = New-AzureRmVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
  $s3 = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
  $s4 = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
  $vnet = New-AzureRmVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4
  ```
3. Yerel ağ geçitleri oluşturun.

  ```powershell
  $lng1 = New-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
  $lng2 = New-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
  $lng3 = New-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
  $lng4 = New-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
  ```
4. Yol tablosu ve yol kuralı oluşturun.

  ```powershell
  New-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
  $rt = Get-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
  Add-AzureRmRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
  Set-AzureRmRouteTable -RouteTable $rt
  ```
5. Yol tablosu Midtier ve arka uç alt ağlara ilişkilendirebilirsiniz.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
  Set-AzureRmVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
  Set-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
6. Ağ geçidi ile bir varsayılan sitesi oluşturun. Bu adımı oluşturma ve ağ geçidi yapılandırma olduğundan, bazen 45 dakika veya daha fazla tamamlanması biraz zaman alır.<br> **- GatewayDefaultSite** çalışmak için bu nedenle bu düzgün ayarını yapılandırmak için dikkatli zorlanmış yönlendirme yapılandırması sağlar cmdlet parametresi. GatewaySKU değeri ilgili ValidateSet hatalar görürseniz, yüklü olduğunu doğrulayın [PowerShell cmdlet'lerinin en yeni sürümünü](#before). PowerShell cmdlet'lerinin en yeni sürümünü en son ağ geçidi SKU'ları için yeni doğrulanmış değerlerini içerir.

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
  $gwsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $ipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
  New-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -GatewayDefaultSite $lng1 -EnableBgp $false
  ```
7. Siteden siteye VPN bağlantıları kurun.

  ```powershell
  $gateway = Get-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
  $lng1 = Get-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" 
  $lng2 = Get-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" 
  $lng3 = Get-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" 
  $lng4 = Get-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" 
    
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng1 -ConnectionType IPsec -SharedKey "preSharedKey"
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng2 -ConnectionType IPsec -SharedKey "preSharedKey"
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng3 -ConnectionType IPsec -SharedKey "preSharedKey"
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection4" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng4 -ConnectionType IPsec -SharedKey "preSharedKey"
    
  Get-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling"
  ```
