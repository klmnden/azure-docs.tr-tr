---
title: "İş ortağı Azure VPN ağ geçitleri için bağlanmak için VPN cihaz yapılandırmalarını | Microsoft Docs"
description: "Bu makale Azure VPN ağ geçitleri için bağlamak için iş ortağı VPN cihaz yapılandırmalarını genel bir bakış sağlar."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: 
ms.assetid: a8bfc955-de49-4172-95ac-5257e262d7ea
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: yushwang
ms.openlocfilehash: b3806d16d3b78347e183ecbd2ab5a463a2142110
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-partner-vpn-device-configurations"></a>İş ortağı VPN cihaz yapılandırmalarını genel bakış
Bu makale Azure VPN ağ geçitleri için bağlamak için şirket içi VPN aygıtları yapılandırma genel bir bakış sağlar. Bir Azure sanal ağı örnek ve VPN ağ geçidi için farklı şirket içi VPN cihaz yapılandırmalarını aynı parametreleri kullanarak bağlanmak nasıl göstermek için kullanılır.

## <a name="device-requirements"></a>Cihaz gereksinimleri
Azure VPN ağ geçitleri kullanmak standart IPSec/IKE protokolü paketleri için siteden siteye (S2S) VPN tüneli. IPSec/IKE parametreleri ve Azure VPN ağ geçitleri için şifreleme algoritmaları listesi için bkz: [VPN cihazları hakkında](vpn-gateway-about-vpn-devices.md). Ayrıca tam algoritmaları ve anahtar gücü belirli bir bağlantı için açıklandığı gibi belirtebilirsiniz [şifreleme gereksinimleri hakkında](vpn-gateway-about-compliance-crypto.md).

## <a name ="singletunnel"></a>Tek VPN tüneli
Örnekteki ilk yapılandırmasını Azure VPN ağ geçidi ve bir şirket içi VPN cihazı arasında tek bir S2S VPN tüneli oluşur. İsteğe bağlı olarak yapılandırabilirsiniz [sınır ağ geçidi Protokolü (BGP) VPN tüneli üzerinden](#bgp).

![Tek S2S VPN tüneli diyagramı](./media/vpn-gateway-3rdparty-device-config-overview/singletunnel.png)

Tek bir VPN tüneli ayarlamak adım adım yönergeler için bkz: [siteden siteye bağlantı yapılandırma](vpn-gateway-howto-site-to-site-resource-manager-portal.md). Aşağıdaki bölümlerde örnek yapılandırması için bağlantı parametrelerini belirtin ve başlamanıza yardımcı olmak için bir PowerShell komut dosyası sağlar.

### <a name="connection-parameters"></a>Bağlantı parametrelerini
Bu bölümde önceki bölümlerde açıklanan örnekler için parametreleri listeler.

| **Parametre**                | **Değer**                    |
| ---                          | ---                          |
| Sanal ağ adres önekleri        | 10.11.0.0/16<br>10.12.0.0/16 |
| Azure VPN ağ geçidi IP         | Azure VPN ağ geçidi IP         |
| Şirket içi adres önekleri | 10.51.0.0/16<br>10.52.0.0/16 |
| Şirket içi VPN cihazının IP    | Şirket içi VPN cihazının IP    |
| * Sanal ağ BGP ASN                | 65010                        |
| * Azure BGP eşdeğer IP           | 10.12.255.30                 |
| * Şirket içi BGP ASN         | 65050                        |
| * Şirket içi BGP eşdeğer IP     | 10.52.255.254                |

\*İsteğe bağlı parametresi BGP yalnızca.

### <a name="sample-powershell-script"></a>Örnek PowerShell komut dosyası
Bu bölüm başlamanıza yardımcı olmak için bir örnek komut dosyası sağlar. Ayrıntılı yönergeler için bkz: [PowerShell kullanarak bir S2S VPN bağlantısı oluşturma](vpn-gateway-create-site-to-site-rm-powershell.md).

```powershell
# Declare your variables

$Sub1          = "Replace_With_Your_Subcription_Name"
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

Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1

# Create virtual network

$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1 $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

# Create VPN gateway

$gwpip1    = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1     = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1

New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet1ASN

# Create local network gateway

New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix51,$LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5

# Create the S2S VPN connection

$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $False
```

### <a name ="policybased"></a>(İsteğe bağlı) UsePolicyBasedTrafficSelectors ile özel IPSec/IKE İlkesi'ni kullanın
VPN cihazlarınızı rota veya VTI tabanlı yapılandırmaları gibi herhangi herhangi trafik Seçici desteklemiyorsa ile özel bir IPSec/IKE ilke oluşturmak [UsePolicyBasedTrafficSelectors](vpn-gateway-connect-multiple-policybased-rm-ps.md) seçeneği.

> [!IMPORTANT]
> Etkinleştirmek için bir IPSec/IKE ilkesi oluşturmalısınız **UsePolicyBasedTrafficSelectors** bağlantı seçeneği.


Örnek komut dosyası için aşağıdaki algoritmaları ve parametrelerine sahip bir IPSec/IKE ilkesi oluşturur:
* Ikev2: AES256, SHA384 DHGroup24
* IPSec: AES256, SHA1, PFS24, SA ömrü 7.200 saniye ve 20,480,000 KB (20 GB)

Komut dosyası IPSec/IKE ilke uygulanır ve sağlayan **UsePolicyBasedTrafficSelectors** bağlantı seçeneği.

```powershell
$ipsecpolicy5 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA1 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 20480000

$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $False -IpsecPolicies $ipsecpolicy5 -UsePolicyBasedTrafficSelectors $True
```

### <a name ="bgp"></a>(İsteğe bağlı) BGP S2S VPN bağlantısı kullan
S2S VPN bağlantısı oluşturduğunuzda, isteğe bağlı olarak kullanabileceğiniz [VPN ağ geçidi için BGP](vpn-gateway-bgp-resource-manager-ps.md). Bu yaklaşımın iki farklar vardır:

* Şirket içi adres öneklerini tek ana bilgisayar adresi olabilir. Şirket içi BGP eş IP adresi gibi belirtilir:

    ```powershell
    New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
    ```

* Bağlantı oluşturduğunuzda, ayarlamalısınız **- EnableBGP** $True seçeneğini:

    ```powershell
    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
    ```

## <a name="next-steps"></a>Sonraki adımlar
Etkin-etkin VPN ağ geçitleri ayarlamak adım adım yönergeler için bkz: [etkin-etkin VPN ağ geçitleri şirketler arası ve VNet-VNet bağlantıları için yapılandırma](vpn-gateway-activeactive-rm-powershell.md).

