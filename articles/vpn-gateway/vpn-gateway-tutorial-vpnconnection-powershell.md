---
title: PowerShell kullanarak Azure S2S VPN bağlantıları oluşturma ve yönetme | Microsoft Docs
description: Öğretici - Azure PowerShell modülü ile S2S VPN bağlantıları Oluşturma ve Yönetme
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 05/08/2018
ms.author: yushwang
ms.custom: mvc
ms.openlocfilehash: da077f013c558448be63dce9b215ded99362d22e
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38452473"
---
# <a name="create-and-manage-s2s-vpn-connections-with-the-azure-powershell-module"></a>Azure PowerShell modülü ile S2S VPN bağlantıları Oluşturma ve Yönetme

Azure S2S VPN bağlantıları müşterinin iş yeri ile Azure arasında güvenli, konumlar arası bağlantı sağlar. Bu öğretici, IPsec S2S VPN bağlantısı yaşam döngüleri için bir S2S VPN bağlantısı oluşturup yönetme gibi konularda rehberlik sağlar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * S2S VPN bağlantısı oluşturma
> * Bağlantı özelliğini güncelleştirme: önceden paylaşılan anahtar, BGP, IPsec/IKE ilkesi
> * Daha fazla VPN bağlantısı ekleme
> * Bir VPN bağlantısını silme

Aşağıdaki diyagramda bu öğreticinin topolojisi gösterilmektedir:

![Konumdan Konuma VPN bağlantısı diyagramı](./media/vpn-gateway-tutorial-vpnconnection-powershell/site-to-site-diagram.png)

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, Azure PowerShell modülü 5.3 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="requirements"></a>Gereksinimler

Aşağıdaki kaynakları oluşturmak için "[Azure PowerShell ile VPN ağ geçidi oluşturma](vpn-gateway-tutorial-create-gateway-powershell.md)" başlıklı ilk öğreticiyi tamamlayın:

1. Kaynak grubu (TestRG1), sanal ağ (VNet1) ve GatewaySubnet
2. VPN ağ geçidi (VNet1GW)

Sanal ağ parametre değerleri aşağıda listelenmiştir. Şirket içi ağınızı temsil eden yerel ağ geçidine ait ek değerleri not alın. Değerleri ortamınıza ve ağ kurulumunuza göre değiştirin.

```azurepowershell-interactive
# Virtual network
$RG1         = "TestRG1"
$VNet1       = "VNet1"
$Location1   = "East US"
$VNet1Prefix = "10.1.0.0/16"
$VNet1ASN    = 65010
$Gw1         = "VNet1GW"

# On-premises network
$LNG1        = "VPNsite1"
$LNGprefix1  = "10.101.0.0/24"
$LNGprefix2  = "10.101.1.0/24"
$LNGIP1      = "YourDevicePublicIP"

# Optional - on-premises BGP properties
$LNGASN1     = 65011
$BGPPeerIP1  = "10.101.1.254"

# Connection
$Connection1 = "VNet1ToSite1"
```

S2S VPN bağlantısı oluşturmak için uygulanması gereken iş akışı basittir:

1. Şirket içi ağınızı temsil eden yerel bir ağ geçidi oluşturun
2. Azure VPN ağ geçidiniz ile yerel ağ geçidiniz arasında bağlantı oluşturma

## <a name="create-a-local-network-gateway"></a>Yerel ağ geçidi oluşturma

Şirket içi ağınızı yerel bir ağ geçidi temsil eder. Yerel ağ geçidinde aşağıdakiler dahil olmak üzere şirket içi ağınızın özelliklerini belirtebilirsiniz:

* VPN cihazınızın genel IP adresi
* Şirket içi adres alanı
* (İsteğe bağlı) BGP öznitelikleri (BGP eş IP adresi ve AS numarası)

[New-AzureRmLocalNetworkGateway](/powershell/module/azurerm.resources/new-azurermlocalnetworkgateway) komutu ile bir yerel ağ geçidi oluşturun.

```azurepowershell-interactive
New-AzureRmLocalNetworkGateway -Name $LNG1 -ResourceGroupName $RG1 `
  -Location 'East US' -GatewayIpAddress $LNGIP1 -AddressPrefix $LNGprefix1,$LNGprefix2
```

## <a name="create-a-s2s-vpn-connection"></a>S2S VPN bağlantısı oluşturma

Şimdi de [New-AzureRmVirtualNetworkGatewayConnection](/powershell/module/azurerm.resources/new-azurermvirtualnetworkgatewayconnection) komutunu kullanarak sanal ağ geçidiniz ile VPN cihazınız arasında bir Siteden Siteye VPN bağlantısı oluşturacaksınız. Siteden Siteye VPN bağlantısı için ‘-ConnectionType’ değerinin *IPsec* olduğunu unutmayın.

```azurepowershell-interactive
$vng1 = Get-AzureRmVirtualNetworkGateway -Name $GW1  -ResourceGroupName $RG1
$lng1 = Get-AzureRmLocalNetworkGateway   -Name $LNG1 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection1 -ResourceGroupName $RG1 `
  -Location $Location1 -VirtualNetworkGateway1 $vng1 -LocalNetworkGateway2 $lng1 `
  -ConnectionType IPsec -SharedKey "Azure@!b2C3"
```

BGP kullanıyorsanız isteğe bağlı "**-EnableBGP $True**" özelliğini ekleyerek bağlantı için BGP'yi etkinleştirebilirsiniz. Varsayılan olarak devre dışıdır.

## <a name="update-the-vpn-connection-pre-shared-key-bgp-and-ipsecike-policy"></a>VPN bağlantısı önceden paylaşılan anahtarını, BGP’yi ve IPsec/IKE ilkesini güncelleştirme

### <a name="view-and-update-your-pre-shared-key"></a>Önceden paylaşılan anahtarınızı görüntüleme ve güncelleştirme

Azure S2S VPN bağlantısı, şirket içi VPN cihazınız ile Azure VPN ağ geçidi arasında kimlik doğrulaması gerçekleştirmek için önceden paylaşılan bir anahtar (gizli dizi) kullanır. Bir bağlantının önceden paylaşılan anahtarını [Get-AzureRmVirtualNetworkGatewayConnectionSharedKey](/powershell/module/azurerm.resources/get-azurermvirtualnetworkgatewayconnectionsharedkey) ve [Set-AzureRmVirtualNetworkGatewayConnectionSharedKey](/powershell/module/azurerm.resources/set-azurermvirtualnetworkgatewayconnectionsharedkey) komutlarını kullanarak görüntüleyebilir ve güncelleştirebilirsiniz.

> [!IMPORTANT]
> Önceden paylaşılan anahtar, en fazla 128 **yazdırılabilir ASCII karakterden** oluşan bir dizedir.

Bu komut, bağlantının önceden paylaşılan anahtarını gösterir:

```azurepowershell-interactive
Get-AzureRmVirtualNetworkGatewayConnectionSharedKey `
  -Name $Connection1 -ResourceGroupName $RG1
```

Yukarıdaki örnek temel alındığında çıktı "**Azure@!b2C3**" olur. Aşağıdaki komutu kullanarak önceden paylaşılan anahtar değerini "**Azure@!_b2=C3**" olarak değiştirin:

```azurepowershell-interactive
Set-AzureRmVirtualNetworkGatewayConnectionSharedKey `
  -Name $Connection1 -ResourceGroupName $RG1 `
  -Value "Azure@!_b2=C3"
```

### <a name="enable-bgp-on-vpn-connection"></a>VPN bağlantısında BGP'yi etkinleştirme

Azure VPN ağ geçidi, BGP dinamik yönlendirme protokolünü destekler. Şirket içi ağlarınızda ve cihazlarınızda BGP’yi kullanıp kullanmadığınıza bağlı olarak her bağlantıda BGP’yi etkinleştirebilirsiniz. Bağlantıda BGP’yi etkinleştirmeden önce aşağıdaki BGP özelliklerini belirtin:

* Azure VPN ASN (Otonom Sistem Numarası)
* Şirket içi yerel ağ geçidi ASN’si
* Şirket içi yerel ağ geçidi BGP eş IP adresi

BGP özelliklerini yapılandırmadıysanız şu komutları kullanarak VPN ağ geçidinize ve yerel ağ geçidinize bu özellikleri ekleyin: [Set-AzureRmVirtualNetworkGateway](/powershell/module/azurerm.resources/set-azurermvirtualnetworkgateway) ve [Set-AzureRmLocalNetworkGateway](/powershell/module/azurerm.resources/set-azurermlocalnetworkgateway).

```azurepowershell-interactive
$vng1 = Get-AzureRmVirtualNetworkGateway -Name $GW1  -ResourceGroupName $RG1
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $vng1 -Asn $VNet1ASN

$lng1 = Get-AzureRmLocalNetworkGateway   -Name $LNG1 -ResourceGroupName $RG1
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $lng1 `
  -Asn $LNGASN1 -BgpPeeringAddress $BGPPeerIP1
```

[Set-AzureRmVirtualNetworkGatewayConnection](/powershell/module/azurerm.resources/set-azurermvirtualnetworkgatewayconnection) komutu ile BGP’yi etkinleştirin.

```azurepowershell-interactive
$connection = Get-AzureRmVirtualNetworkGatewayConnection `
  -Name $Connection1 -ResourceGroupName $RG1

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection `
  -EnableBGP $True
```

"-EnableBGP" özellik değerini **$False** olacak şekilde değiştirerek BGP’yi devre dışı bırakabilirsiniz. Azure VPN ağ geçitlerinde BGP’ye ilişkin daha ayrıntılı açıklamalar için [Azure VPN ağ geçitlerinde BGP](vpn-gateway-bgp-resource-manager-ps.md) konusuna başvurun.

### <a name="apply-a-custom-ipsecike-policy-on-the-connection"></a>Bağlantıda özel bir IPSec/IKE ilkesi uygulama

İsteğe bağlı bir IPSec/IKE ilkesi belirterek bağlantıda [varsayılan öneriler](vpn-gateway-about-vpn-devices.md#ipsec) yerine tam olarak hangi IPSec/IKE şifreleme algoritması ve anahtar gücü kombinasyonunun kullanılacağını belirtebilirsiniz. Aşağıdaki örnek betik, şu algoritmalar ve parametreler ile farklı bir IPsec/IKE ilkesi oluşturur:

* IKEv2: AES256, SHA256, DHGroup14
* IPsec: AES128, SHA1, PFS14, SA Yaşam Süresi 14.400 saniye ve 102.400.000 KB

```azurepowershell-interactive
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection1 `
                -ResourceGroupName $RG1
$newpolicy  = New-AzureRmIpsecPolicy `
                -IkeEncryption AES256 -IkeIntegrity SHA256 -DhGroup DHGroup14 `
                -IpsecEncryption AES128 -IpsecIntegrity SHA1 -PfsGroup PFS2048 `
                -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection `
  -IpsecPolicies $newpolicy
```

Algoritmaların ve yönergelerin tam listesini görmek için bkz. [S2S veya Sanal Ağdan Sanal Ağa bağlantıları için IPSec/IKE İlkesi](vpn-gateway-ipsecikepolicy-rm-powershell.md).

## <a name="add-another-s2s-vpn-connection"></a>Başka bir S2S VPN bağlantısı ekleme

Aynı VPN ağ geçidine ek bir S2S VPN bağlantısı eklemek için başka bir yerel ağ geçidi oluşturun ve yeni yerel ağ geçidi ile VPN ağ geçidi arasında yeni bir bağlantı oluşturun. Bu makaledeki örneği izleyebilirsiniz.

```azurepowershell-interactive
# On-premises network
$LNG2        = "VPNsite2"
$Location2   = "West US"
$LNGprefix21 = "10.102.0.0/24"
$LNGprefix22 = "10.102.1.0/24"
$LNGIP2      = "YourDevicePublicIP"
$Connection2 = "VNet1ToSite2"

New-AzureRmLocalNetworkGateway -Name $LNG2 -ResourceGroupName $RG1 `
  -Location $Location2 -GatewayIpAddress $LNGIP2 -AddressPrefix $LNGprefix21,$LNGprefix22

$vng1 = Get-AzureRmVirtualNetworkGateway -Name $GW1  -ResourceGroupName $RG1
$lng2 = Get-AzureRmLocalNetworkGateway   -Name $LNG2 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection2 -ResourceGroupName $RG1 `
  -Location $Location1 -VirtualNetworkGateway1 $vng1 -LocalNetworkGateway2 $lng2 `
  -ConnectionType IPsec -SharedKey "AzureA1%b2_C3+"
```

Azure VPN ağ geçidiniz ile iki S2S VPN bağlantısı kurmuş oldunuz.

![Çok siteli VPN bağlantıları](./media/vpn-gateway-tutorial-vpnconnection-powershell/multisite-connections.png)

## <a name="delete-a-s2s-vpn-connection"></a>Bir S2S VPN bağlantısını silme

Bir S2S VPN bağlantısını [Remove-AzureRmVirtualNetworkGatewayConnection](/powershell/module/azurerm.resources/remove-azurermvirtualnetworkgatewayconnection) komutu ile silebilirsiniz.

```azurepowershell-interactive
Remove-AzureRmVirtualNetworkGatewayConnection -Name $Connection2 -ResourceGroupName $RG1
```

Yerel ağ geçidi artık gerekli değilse ağ geçidini silin. Kendisiyle ilişkili başka bağlantılar olan bir yerel ağ geçidini silemezsiniz.

```azurepowershell-interactive
Remove-AzureRmVirtualNetworkGatewayConnection -Name $LNG2 -ResourceGroupName $RG1
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, S2S VPN bağlantıları oluşturup yönetme konusunda aşağıdaki gibi işlemler yapmayı öğrendiniz:

> [!div class="checklist"]
> * S2S VPN bağlantısı oluşturma
> * Bağlantı özelliğini güncelleştirme: önceden paylaşılan anahtar, BGP, IPsec/IKE ilkesi
> * Daha fazla VPN bağlantısı ekleme
> * Bir VPN bağlantısını silme

S2S, Sanal Ağdan Sanal Ağa ve P2S bağlantıları hakkında bilgi edinmek için sonraki öğreticilere ilerleyin.

> [!div class="nextstepaction"]
> * [Sanal Ağdan Sanal Ağa bağlantılar oluşturma](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [P2S bağlantıları oluşturma](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
