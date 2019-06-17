---
title: İş ortağı, Azure VPN ağ geçitlerine bağlanmak için VPN cihazı yapılandırmaları | Microsoft Docs
description: Bu makalede Azure VPN ağ geçitlerine bağlanmak için VPN cihazı yapılandırmaları iş ortağı genel bakış sağlar.
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: ''
tags: ''
ms.assetid: a8bfc955-de49-4172-95ac-5257e262d7ea
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: yushwang
ms.openlocfilehash: 7d3a32b5f2b2742a36716bac9747f20c47c98858
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66150162"
---
# <a name="overview-of-partner-vpn-device-configurations"></a>VPN cihaz yapılandırmalarını iş ortağı genel bakış
Bu makalede Azure VPN ağ geçitlerine bağlanmak için şirket içi VPN cihazları yapılandırmaya genel bakış sağlar. Bir Azure sanal ağı örnek ve VPN ağ geçidi, aynı parametreleri kullanarak için farklı şirket içi VPN cihazı yapılandırmaları bağlanma göstermek için kullanılır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="device-requirements"></a>Cihaz gereksinimleri
Azure VPN ağ geçitleri kullanan standart IPSec/IKE protokolü paketleri için siteden siteye (S2S) VPN tünelleri. IPSec/IKE parametreleri ve Azure VPN ağ geçitleri için şifreleme algoritmaları listesi için bkz: [VPN cihazları hakkında](vpn-gateway-about-vpn-devices.md). Tam algoritmalar ve anahtar güçleriyle belirli bir bağlantı için açıklandığı gibi belirtebilirsiniz [şifreleme gereksinimleri hakkında](vpn-gateway-about-compliance-crypto.md).

## <a name ="singletunnel"></a>Tek bir VPN tüneli
İlk örnek yapılandırmada bir Azure VPN gateway ile bir şirket içi VPN cihazınız arasındaki tek S2S VPN tüneli oluşur. İsteğe bağlı olarak yapılandırabileceğiniz [Border Gateway Protocol (BGP) VPN tüneli üzerinden](#bgp).

![Tek bir S2S VPN tüneli diyagramı](./media/vpn-gateway-3rdparty-device-config-overview/singletunnel.png)

Tek bir VPN tüneli ayarlamak adım adım yönergeler için bkz: [siteden siteye bağlantı yapılandırma](vpn-gateway-howto-site-to-site-resource-manager-portal.md). Aşağıdaki bölümlerde örnek yapılandırması için bağlantı parametrelerini belirtin ve başlamanıza yardımcı olmak için bir PowerShell Betiği belirtin.

### <a name="connection-parameters"></a>Bağlantı parametreleri
Bu bölümde, önceki bölümlerde açıklanan örnekler için parametreleri listeler.

| **Parametre**                | **Değer**                    |
| ---                          | ---                          |
| Sanal ağ adres ön ekleri        | 10.11.0.0/16<br>10.12.0.0/16 |
| Azure VPN ağ geçidi IP         | Azure VPN ağ geçidi IP         |
| Şirket içi adres ön ekleri | 10.51.0.0/16<br>10.52.0.0/16 |
| Şirket içi VPN cihazının IP    | Şirket içi VPN cihazının IP    |
| * BGP ASN'sini sanal ağ                | 65010                        |
| * Azure BGP eşdeğer IP           | 10.12.255.30                 |
| * Şirket içi BGP ASN'sini         | 65050                        |
| * Şirket içi BGP eşdeğer IP     | 10.52.255.254                |

\* Yalnızca isteğe bağlı parametre için BGP.

### <a name="sample-powershell-script"></a>Örnek PowerShell Betiği
Bu bölümde, başlamanıza yardımcı olmak için örnek betik sağlanır. Ayrıntılı yönergeler için bkz. [PowerShell kullanarak bir S2S VPN bağlantısı oluşturma](vpn-gateway-create-site-to-site-rm-powershell.md).

```powershell
# Declare your variables

$Sub1          = "Replace_With_Your_Subscription_Name"
$RG1           = "TestRG1"
$Location1     = "East US 2"
$VNetName1     = "TestVNet1"
$FESubName1    = "FrontEnd"
$BESubName1    = "Backend"
$GWSubName1    = "GatewaySubnet"
$VNetPrefix11  = "10.11.0.0/16"
$VNetPrefix12  = "10.12.0.0/16"
$FESubPrefix1  = "10.11.0.0/24"
$BESubPrefix1  = "10.12.0.0/24"
$GWSubPrefix1  = "10.12.255.0/27"
$VNet1ASN      = 65010
$DNS1          = "8.8.8.8"
$GWName1       = "VNet1GW"
$GWIPName1     = "VNet1GWIP"
$GWIPconfName1 = "gwipconf1"
$Connection15  = "VNet1toSite5"
$LNGName5      = "Site5"
$LNGPrefix50   = "10.52.255.254/32"
$LNGPrefix51   = "10.51.0.0/16"
$LNGPrefix52   = "10.52.0.0/16"
$LNGIP5        = "Your_VPN_Device_IP"
$LNGASN5       = 65050
$BGPPeerIP5    = "10.52.255.254"

# Connect to your subscription and create a new resource group

Connect-AzAccount
Select-AzSubscription -SubscriptionName $Sub1
New-AzResourceGroup -Name $RG1 -Location $Location1

# Create virtual network

$fesub1 = New-AzVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1 $besub1 = New-AzVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

# Create VPN gateway

$gwpip1    = New-AzPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1     = Get-AzVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1   = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gwipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1

New-AzVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet1ASN

# Create local network gateway

New-AzLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix51,$LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5

# Create the S2S VPN connection

$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1

New-AzVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $False
```

### <a name ="policybased"></a>(İsteğe bağlı) UsePolicyBasedTrafficSelectors ile özel IPSec/IKE İlkesi'ni kullanın
VPN cihazlarınız VTI tabanlı ya da rota tabanlı yapılandırmaları gibi herhangi bir ağdan herhangi bir trafik seçicileri desteklemiyorsa ile özel bir IPSec/IKE ilke oluşturma [UsePolicyBasedTrafficSelectors](vpn-gateway-connect-multiple-policybased-rm-ps.md) seçeneği.

> [!IMPORTANT]
> Etkinleştirmek için bir IPSec/IKE ilke oluşturmalıdır **UsePolicyBasedTrafficSelectors** bağlantı seçeneği.


Örnek betik, aşağıdaki algoritmaları ve parametrelerle bir IPSec/IKE ilkesi oluşturur:
* Ikev2: AES256, SHA384 DHGroup24
* IPSec: AES256, SHA1, PFS24, SA yaşam süresi 7.200 saniye ve 20,480,000 KB (20 GB)

Betik IPSec/IKE ilke uygulanır ve sağlayan **UsePolicyBasedTrafficSelectors** bağlantı seçeneği.

```powershell
$ipsecpolicy5 = New-AzIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA1 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 20480000

$vnet1gw = Get-AzVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1

New-AzVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $False -IpsecPolicies $ipsecpolicy5 -UsePolicyBasedTrafficSelectors $True
```

### <a name ="bgp"></a>(İsteğe bağlı) BGP S2S VPN bağlantısı kullanın.
S2S VPN bağlantısı oluşturduğunuzda, isteğe bağlı olarak kullanabileceğiniz [VPN ağ geçidi için BGP](vpn-gateway-bgp-resource-manager-ps.md). Bu yaklaşımın iki fark vardır:

* Şirket içi adres ön ekleri tek ana bilgisayar adresi olabilir. Şirket içi BGP eş IP adresi şu şekilde belirlenir:

    ```powershell
    New-AzLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
    ```

* Bağlantı oluşturduğunuzda, ayarlamalısınız **- Enablebgp'yi** $True seçeneğini:

    ```powershell
    New-AzVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
    ```

## <a name="next-steps"></a>Sonraki adımlar
Etkin-etkin VPN ağ geçitleri ayarlarsınız adım adım yönergeler için bkz: [etkin-etkin VPN ağ geçitlerini şirketler arası ve VNet-VNet bağlantıları için yapılandırma](vpn-gateway-activeactive-rm-powershell.md).

