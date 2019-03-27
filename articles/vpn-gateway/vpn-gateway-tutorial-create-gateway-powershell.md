---
title: PowerShell kullanarak Azure VPN ağ geçidi oluşturma ve yönetme | Microsoft Docs
description: Öğretici - Azure PowerShell modülü ile VPN ağ geçidi oluşturma ve yönetme
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: tutorial
ms.date: 02/11/2019
ms.author: yushwang
ms.custom: mvc
ms.openlocfilehash: 790a8b74f437fe8fd7b8660c2ac9d208328b487f
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58445214"
---
# <a name="tutorial-create-and-manage-a-vpn-gateway-using-powershell"></a>Öğretici: Oluşturma ve PowerShell kullanarak VPN ağ geçidi yönetme

Azure VPN ağ geçitleri, müşterinin iş yeri ile Azure arasında konumlar arası bağlantı sağlar. Bu öğretici, bir VPN ağ geçidi oluşturma ve yönetme gibi temel Azure VPN ağ geçidi dağıtım öğelerini kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * VPN ağ geçidi oluşturma
> * Genel IP adresini görüntüleyin
> * Bir VPN ağ geçidini yeniden boyutlandırma
> * Bir VPN ağ geçidini sıfırlama

Aşağıdaki diyagramda, bu öğreticinin bir parçası olarak oluşturulan sanal ağ ve VPN ağ geçidi gösterilmiştir.

![Sanal Ağ ve VPN ağ geçidi](./media/vpn-gateway-tutorial-create-gateway-powershell/vnet1-gateway.png)

### <a name="azure-cloud-shell-and-azure-powershell"></a>Azure Cloud Shell ve Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [working with cloud shell](../../includes/vpn-gateway-cloud-shell-powershell.md)]

## <a name="common-network-parameter-values"></a>Ortak ağ parametre değerleri

Temel ortam ve ağ kurulumu daha sonra kopyalama ve yapıştırma Bu öğretici için değişkenleri ayarlamak için aşağıdaki değerleri değiştirin. Cloud Shell oturumunuzu zaman aşımına uğraması veya farklı bir PowerShell penceresi kullanmanız gerekir, kopyalama ve yeni oturumunuz değişkenlere yapıştırın ve Öğreticisi ile devam edin.

```azurepowershell-interactive
$RG1         = "TestRG1"
$VNet1       = "VNet1"
$Location1   = "East US"
$FESubnet1   = "FrontEnd"
$BESubnet1   = "Backend"
$GwSubnet1   = "GatewaySubnet"
$VNet1Prefix = "10.1.0.0/16"
$FEPrefix1   = "10.1.0.0/24"
$BEPrefix1   = "10.1.1.0/24"
$GwPrefix1   = "10.1.255.0/27"
$VNet1ASN    = 65010
$DNS1        = "8.8.8.8"
$Gw1         = "VNet1GW"
$GwIP1       = "VNet1GWIP"
$GwIPConf1   = "gwipconf1"
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bir kaynak grubu oluşturun [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutu. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Önce bir kaynak grubu oluşturulmalıdır. Aşağıdaki örnekte, *Doğu ABD* bölgesinde *TestRG1* adlı bir kaynak grubu oluşturulur:

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName $RG1 -Location $Location1
```

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Azure VPN ağ geçidi, sanal ağınız için konumlar arası bağlantı ve P2S VPN sunucusu işlevselliği sağlar. VPN ağ geçidini mevcut bir sanal ağa ekleyin veya yeni bir sanal ağ ile ağ geçidi oluşturun. Bu örnek, üç alt ağa sahip yeni bir sanal ağ oluşturur: Ön uç ve arka uç GatewaySubnet kullanarak [yeni AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) ve [yeni AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork):

```azurepowershell-interactive
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name $FESubnet1 -AddressPrefix $FEPrefix1
$besub1 = New-AzVirtualNetworkSubnetConfig -Name $BESubnet1 -AddressPrefix $BEPrefix1
$gwsub1 = New-AzVirtualNetworkSubnetConfig -Name $GWSubnet1 -AddressPrefix $GwPrefix1
$vnet   = New-AzVirtualNetwork `
            -Name $VNet1 `
            -ResourceGroupName $RG1 `
            -Location $Location1 `
            -AddressPrefix $VNet1Prefix `
            -Subnet $fesub1,$besub1,$gwsub1
```

## <a name="request-a-public-ip-address-for-the-vpn-gateway"></a>VPN ağ geçidi için genel bir IP adresi isteme

Azure VPN ağ geçitleri, İnternet üzerinden şirket içi VPN cihazlarınızla iletişim kurarak IKE (İnternet Anahtar Değişimi) anlaşması gerçekleştirir ve IPSec tünelleri oluşturur. Oluşturma ve ile aşağıdaki örnekte gösterildiği gibi VPN ağ geçidinize genel bir IP adresi atama [yeni AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) ve [yeni AzVirtualNetworkGatewayIpConfig](/powershell/module/az.network/new-azvirtualnetworkgatewayipconfig):

> [!IMPORTANT]
> Şu anda ağ geçidi için yalnızca Dinamik bir genel IP adresi kullanabilirsiniz. Azure VPN ağ geçitlerinde Statik IP adresi desteklenmez.

```azurepowershell-interactive
$gwpip    = New-AzPublicIpAddress -Name $GwIP1 -ResourceGroupName $RG1 `
              -Location $Location1 -AllocationMethod Dynamic
$subnet   = Get-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' `
              -VirtualNetwork $vnet
$gwipconf = New-AzVirtualNetworkGatewayIpConfig -Name $GwIPConf1 `
              -Subnet $subnet -PublicIpAddress $gwpip
```

## <a name="create-a-vpn-gateway"></a>VPN ağ geçidi oluşturma

VPN ağ geçidinin oluşturulması 45 dakika veya daha uzun sürebilir. Ağ geçidi oluşturma işlemi tamamlandığında sanal ağınız ile başka bir sanal ağ arasında bağlantı oluşturabilirsiniz. Dilerseniz sanal ağınız ile şirket içindeki bir konum arasında da bir bağlantı oluşturabilirsiniz. Kullanarak bir VPN ağ geçidi oluşturma [yeni AzVirtualNetworkGateway](/powershell/module/az.network/New-azVirtualNetworkGateway) cmdlet'i.

```azurepowershell-interactive
New-AzVirtualNetworkGateway -Name $Gw1 -ResourceGroupName $RG1 `
  -Location $Location1 -IpConfigurations $gwipconf -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
```

Anahtar parametre değerleri:
* Ağ geçidi türü: Kullanım **Vpn** siteden siteye ve VNet-VNet bağlantıları için
* VPN türü: Kullanım **RouteBased** daha geniş bir aralık, VPN cihazları ve daha fazla yönlendirme özellikleri ile etkileşim kurmak için
* GatewaySku: **VpnGw1** daha yüksek aktarım hızı veya daha fazla bağlantı gerekiyorsa VpnGw2 veya VpnGw3 değiştirmek; varsayılandır. Daha fazla bilgi için bkz. [Ağ geçidi SKU'ları](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

TryIt kullanıyorsanız, oturumunuz zaman aşımına uğrayabilir. Tamam. Ağ geçidi hala oluşturacaksınız.

Ağ geçidi oluşturma işlemi tamamlandığında sanal ağınız ile başka bir sanal ağ arasında ya da sanal ağınız ile şirket içi bir konum arasında bağlantı oluşturabilirsiniz. Ayrıca, bir istemci bilgisayardan sanal ağınıza yönelik bir P2S bağlantısı yapılandırabilirsiniz.

## <a name="view-the-gateway-public-ip-address"></a>Ağ geçidi genel IP adresini görüntüleyin

Genel IP adresini adını biliyorsanız kullanın [Get-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress) ağ geçidine atanan genel IP adresini göstermek için.

Oturumunuz zaman aşımına uğradı, bu öğreticinin yeni oturumunuza baştan ortak ağ parametrelerini kopyalayın ve devam etmek ve sonra devam.

```azurepowershell-interactive
$myGwIp = Get-AzPublicIpAddress -Name $GwIP1 -ResourceGroup $RG1
$myGwIp.IpAddress
```

## <a name="resize-a-gateway"></a>Bir ağ geçidini yeniden boyutlandırın

Ağ geçidi oluşturulduktan sonra VPN ağ geçidi SKU’sunu değiştirebilirsiniz. Farklı ağ geçidi SKU’ları aktarım hızı, bağlantı sayısı gibi konularda farklı belirtimleri destekler. Aşağıdaki örnekte [boyutlandırma AzVirtualNetworkGateway](/powershell/module/az.network/Resize-azVirtualNetworkGateway) VpnGw1 geçidinizden VpnGw2 için yeniden boyutlandırabilirsiniz. Daha fazla bilgi için bkz. [Ağ geçidi SKU'ları](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

```azurepowershell-interactive
$gateway = Get-AzVirtualNetworkGateway -Name $Gw1 -ResourceGroup $RG1
Resize-AzVirtualNetworkGateway -GatewaySku VpnGw2 -VirtualNetworkGateway $gateway
```

Bir VPN ağ geçidinin yeniden boyutlandırılması da yaklaşık 30-45 dakika sürer, ancak bu işlem mevcut bağlantıları ve yapılandırmaları kesintiye **uğratmaz** veya kaldırmaz.

## <a name="reset-a-gateway"></a>Bir ağ geçidini sıfırlama

Sorun giderme adımlarının bir parçası olarak VPN ağ geçidinizi IPsec/IKE tünel yapılandırmalarını yeniden başlatmaya zorlamak için VPN ağ geçidini sıfırlayabilirsiniz. Kullanım [sıfırlama AzVirtualNetworkGateway](/powershell/module/az.network/Reset-azVirtualNetworkGateway) , ağ geçidini sıfırlama.

```azurepowershell-interactive
$gateway = Get-AzVirtualNetworkGateway -Name $Gw1 -ResourceGroup $RG1
Reset-AzVirtualNetworkGateway -VirtualNetworkGateway $gateway
```

Daha fazla bilgi için bkz. [Bir VPN ağ geçidini sıfırlama](vpn-gateway-resetgw-classic.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Varsa, için ilerletme [sonraki öğreticiye](vpn-gateway-tutorial-vpnconnection-powershell.md), önkoşul oldukları için bu kaynakları saklamak isteyeceksiniz.

Ancak, ağ geçidini bir prototip, test veya kavram kanıtı dağıtımı parçasıysa, kullanabileceğiniz [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) VPN ağ geçidi, kaynak grubunu kaldırmak için komutu ve tüm ilgili kaynakları.

```azurepowershell-interactive
Remove-AzResourceGroup -Name $RG1
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdakiler gibi temel VPN ağ geçidi oluşturma ve yönetim görevlerini öğrendiniz:

> [!div class="checklist"]
> * VPN ağ geçidi oluşturma
> * Genel IP adresini görüntüleyin
> * Bir VPN ağ geçidini yeniden boyutlandırma
> * Bir VPN ağ geçidini sıfırlama

S2S, Sanal Ağdan Sanal Ağa ve P2S bağlantıları hakkında bilgi edinmek için sonraki öğreticilere ilerleyin.

> [!div class="nextstepaction"]
> * [S2S bağlantıları oluşturma](vpn-gateway-tutorial-vpnconnection-powershell.md)
> * [Sanal Ağdan Sanal Ağa bağlantılar oluşturma](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [P2S bağlantıları oluşturma](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
