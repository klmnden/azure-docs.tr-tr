---
title: PowerShell kullanarak Azure VPN ağ geçidi oluşturma ve yönetme | Microsoft Docs
description: Öğretici - Azure PowerShell modülü ile VPN ağ geçidi oluşturma ve yönetme
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
ms.date: 05/14/2018
ms.author: yushwang
ms.custom: mvc
ms.openlocfilehash: 0f10384e7e21d65b3a16869a10f8294b9643c74c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34210207"
---
# <a name="create-and-manage-vpn-gateway-with-the-azure-powershell-module"></a>Azure PowerShell modülü ile VPN ağ geçidi oluşturma ve yönetme

Azure VPN ağ geçitleri, müşterinin iş yeri ile Azure arasında konumlar arası bağlantı sağlar. Bu öğretici, bir VPN ağ geçidi oluşturma ve yönetme gibi temel Azure VPN ağ geçidi dağıtım öğelerini kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * VPN ağ geçidi oluşturma
> * Bir VPN ağ geçidini yeniden boyutlandırma
> * Bir VPN ağ geçidini sıfırlama

Aşağıdaki diyagramda, bu öğreticinin bir parçası olarak oluşturulan sanal ağ ve VPN ağ geçidi gösterilmiştir.

![Sanal Ağ ve VPN ağ geçidi](./media/vpn-gateway-tutorial-create-gateway-powershell/vnet1-gateway.png)

### <a name="azure-cloud-shell-and-azure-powershell"></a>Azure Cloud Shell ve Azure PowerShell

[!INCLUDE [working with cloudshell](../../includes/vpn-gateway-cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, Azure PowerShell modülü 5.3 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir. 

## <a name="common-network-parameter-values"></a>Ortak ağ parametre değerleri

Aşağıdaki değerleri ortamınıza ve ağ kurulumunuza göre değiştirin.

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

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) komutu ile yeni bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Önce bir kaynak grubu oluşturulmalıdır. Aşağıdaki örnekte, *Doğu ABD* bölgesinde *TestRG1* adlı bir kaynak grubu oluşturulur:

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName $RG1 -Location $Location1
```

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Azure VPN ağ geçidi, sanal ağınız için konumlar arası bağlantı ve P2S VPN sunucusu işlevselliği sağlar. VPN ağ geçidini mevcut bir sanal ağa ekleyin veya yeni bir sanal ağ ile ağ geçidi oluşturun. Bu örnekte [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) ve [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) komutları kullanılarak üç alt ağı olan yeni bir sanal ağ oluşturulur: Frontend, Backend ve GatewaySubnet:

```azurepowershell-interactive
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubnet1 -AddressPrefix $FEPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubnet1 -AddressPrefix $BEPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubnet1 -AddressPrefix $GwPrefix1
$vnet   = New-AzureRmVirtualNetwork `
            -Name $VNet1 `
            -ResourceGroupName $RG1 `
            -Location $Location1 `
            -AddressPrefix $VNet1Prefix `
            -Subnet $fesub1,$besub1,$gwsub1
```

## <a name="request-a-public-ip-address-for-the-vpn-gateway"></a>VPN ağ geçidi için genel bir IP adresi isteme

Azure VPN ağ geçitleri, İnternet üzerinden şirket içi VPN cihazlarınızla iletişim kurarak IKE (İnternet Anahtar Değişimi) anlaşması gerçekleştirir ve IPSec tünelleri oluşturur. Aşağıdaki örnekte gösterildiği gibi [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) ve [New-AzureRmVirtualNetworkGatewayIpConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworkgatewayipconfig) komutlarıyla genel bir IP adresi oluşturup VPN ağ geçidinize atayın:

> [!IMPORTANT]
> Şu anda ağ geçidi için yalnızca Dinamik bir genel IP adresi kullanabilirsiniz. Azure VPN ağ geçitlerinde Statik IP adresi desteklenmez.

```azurepowershell-interactive
$gwpip    = New-AzureRmPublicIpAddress -Name $GwIP1 -ResourceGroupName $RG1 `
              -Location $Location1 -AllocationMethod Dynamic
$subnet   = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' `
              -VirtualNetwork $vnet
$gwipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GwIPConf1 `
              -Subnet $subnet -PublicIpAddress $gwpip
```

## <a name="create-vpn-gateway"></a>VPN ağ geçidi oluşturma

VPN ağ geçidinin oluşturulması 45 dakika veya daha uzun sürebilir. Ağ geçidi oluşturma işlemi tamamlandığında sanal ağınız ile başka bir sanal ağ arasında bağlantı oluşturabilirsiniz. Dilerseniz sanal ağınız ile şirket içindeki bir konum arasında da bir bağlantı oluşturabilirsiniz. [New-AzureRmVirtualNetworkGateway](/powershell/module/azurerm.network/New-AzureRmVirtualNetworkGateway) cmdlet’ini kullanarak bir VPN ağ geçidi oluşturun.

```azurepowershell-interactive
New-AzureRmVirtualNetworkGateway -Name $Gw1 -ResourceGroupName $RG1 `
  -Location $Location1 -IpConfigurations $gwipconf -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
```

Anahtar parametre değerleri:
* GatewayType: Siteden siteye ve sanal ağdan sanal ağa bağlantılar için **Vpn** değerini kullanın
* VpnType: Daha geniş bir VPN cihazı yelpazesiyle etkileşim kurmak ve daha fazla yönlendirme özelliğinden yararlanmak için **RouteBased** seçeneğini kullanın
* GatewaySku: Varsayılan değer **VpnGw1**’dir; daha yüksek aktarım hızına veya daha fazla bağlantıya gereksinim duyuyorsanız VpnGw2 veya VpnGw3 olarak değiştirin. Daha fazla bilgi için bkz. [Ağ geçidi SKU'ları](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

Ağ geçidi oluşturma işlemi tamamlandığında sanal ağınız ile başka bir sanal ağ arasında ya da sanal ağınız ile şirket içi bir konum arasında bağlantı oluşturabilirsiniz. Ayrıca, bir istemci bilgisayardan sanal ağınıza yönelik bir P2S bağlantısı yapılandırabilirsiniz.

## <a name="resize-vpn-gateway"></a>VPN ağ geçidini yeniden boyutlandırma

Ağ geçidi oluşturulduktan sonra VPN ağ geçidi SKU’sunu değiştirebilirsiniz. Farklı ağ geçidi SKU’ları aktarım hızı, bağlantı sayısı gibi konularda farklı belirtimleri destekler. Aşağıdaki örnekte ağ geçidinizin VpnGw1 olan boyutunun VpnGw2 olarak ayarlanması için [Resize-AzureRmVirtualNetworkGateway](/powershell/module/azurerm.network/Resize-AzureRmVirtualNetworkGateway) komutu kullanılır. Daha fazla bilgi için bkz. [Ağ geçidi SKU'ları](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

```azurepowershell-interactive
$gw = Get-AzureRmVirtualNetworkGateway -Name $Gw1 -ResourceGroup $RG1
Resize-AzureRmVirtualNetworkGateway -GatewaySku VpnGw2 -VirtualNetworkGateway $gateway
```

Bir VPN ağ geçidinin yeniden boyutlandırılması da yaklaşık 30-45 dakika sürer, ancak bu işlem mevcut bağlantıları ve yapılandırmaları kesintiye **uğratmaz** veya kaldırmaz.

## <a name="reset-vpn-gateway"></a>VPN ağ geçidini sıfırlama

Sorun giderme adımlarının bir parçası olarak VPN ağ geçidinizi IPsec/IKE tünel yapılandırmalarını yeniden başlatmaya zorlamak için VPN ağ geçidini sıfırlayabilirsiniz. Ağ geçidinizi sıfırlamak için [Reset-AzureRmVirtualNetworkGateway](/powershell/module/azurerm.network/Reset-AzureRmVirtualNetworkGateway) komutunu kullanın.

```azurepowershell-interactive
$gw = Get-AzureRmVirtualNetworkGateway -Name $Gw1 -ResourceGroup $RG1
Reset-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gateway
```

Daha fazla bilgi için bkz. [Bir VPN ağ geçidini sıfırlama](vpn-gateway-resetgw-classic.md).

## <a name="get-the-gateway-public-ip-address"></a>Ağ geçidi genel IP adresini alma

Genel IP adresinin adını biliyorsanız, ağ geçidine atanan genel IP adresini göstermek için [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/Reset-AzureRmPublicIpAddress) komutunu kullanın.

```azurepowershell-interactive
$myGwIp = Get-AzureRmPublicIpAddress -Name $GwIP1 -ResourceGroup $RG1
$myGwIp.IpAddress
```

## <a name="delete-vpn-gateway"></a>VPN ağ geçidini silme

Konumlar arası ve sanal ağdan sanal ağa bağlantının tam olarak yapılandırılması için VPN ağ geçidine ek olarak birden çok kaynak türü gerekir. Ağ geçidinin kendisini silmeden önce VPN ağ geçidiyle ilişkili bağlantıları silin. Ağ geçidi silindikten sonra ağ geçidinin genel IP adreslerini silebilirsiniz. Adımların ayrıntılı bir açıklaması için bkz. [Bir VPN ağ geçidini silme](vpn-gateway-delete-vnet-gateway-powershell.md).

Ağ geçidi bir prototip ya da kavram kanıtı dağıtımının bir parçasıysa [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak kaynak grubunu, VPN ağ geçidini ve tüm ilgili kaynakları kaldırabilirsiniz.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name $RG1
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdakiler gibi temel VPN ağ geçidi oluşturma ve yönetim görevlerini öğrendiniz:

> [!div class="checklist"]
> * VPN ağ geçidi oluşturma
> * Bir VPN ağ geçidini yeniden boyutlandırma
> * Bir VPN ağ geçidini sıfırlama

S2S, Sanal Ağdan Sanal Ağa ve P2S bağlantıları hakkında bilgi edinmek için sonraki öğreticilere ilerleyin.

> [!div class="nextstepaction"]
> * [S2S bağlantıları oluşturma](vpn-gateway-tutorial-vpnconnection-powershell.md)
> * [Sanal Ağdan Sanal Ağa bağlantılar oluşturma](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [P2S bağlantıları oluşturma](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
