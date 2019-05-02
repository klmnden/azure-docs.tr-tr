---
title: Sanal ağları birbirine bağlama sanal ağ eşlemesi ile - PowerShell | Microsoft Docs
description: Bu makalede, eşlemesi, Azure PowerShell kullanarak sanal ağ ile sanal ağları bağlama hakkında bilgi edinin.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
Customer intent: I want to connect two virtual networks so that virtual machines in one virtual network can communicate with virtual machines in the other virtual network.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: 0ee39e83ef49db1d6231b5c20eee4dbf984f9f13
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64698968"
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-powershell"></a>PowerShell kullanarak sanal ağ eşlemesi ile sanal ağları birbirine bağlama

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Sanal ağ eşlemesi ile sanal ağları birbirine bağlayabilirsiniz. Sanal ağlar eşlendikten sonra, kaynaklar aynı sanal ağ üzerindeymiş gibi, aynı gecikme süresi ve bant genişliği ile her iki sanal ağdaki kaynaklar birbiriyle iletişim kurabilir. Bu makalede şunları öğreneceksiniz:

* İki sanal ağ oluşturma
* Sanal ağ eşlemesi iki sanal ağı bağlama
* Her sanal ağa sanal makine (VM) dağıtma
* Sanal makineler arasında iletişim

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu makale Azure PowerShell modülü sürüm 1.0.0 gerekir veya üzeri. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-virtual-networks"></a>Sanal ağlar oluşturma

Bir sanal ağ oluşturmadan önce sanal ağ ve bu makalede oluşturulan tüm kaynakları için bir kaynak grubu oluşturmanız gerekir. Bir kaynak grubu oluşturun [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

İle sanal ağ oluşturma [yeni AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork). Aşağıdaki örnekte adlı bir sanal ağ oluşturur *myVirtualNetwork1* adres ön eki ile *10.0.0.0/16*.

```azurepowershell-interactive
$virtualNetwork1 = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork1 `
  -AddressPrefix 10.0.0.0/16
```

Bir alt ağ yapılandırması ile [yeni AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig). Aşağıdaki örnek, 10.0.0.0/24 adres ön eki ile bir alt ağ yapılandırması oluşturur:

```azurepowershell-interactive
$subnetConfig = Add-AzVirtualNetworkSubnetConfig `
  -Name Subnet1 `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork1
```

Sanal ağ için alt ağ yapılandırmasını yazma [kümesi AzVirtualNetwork](/powershell/module/az.network/Set-azVirtualNetwork), alt ağ oluşturur:

```azurepowershell-interactive
$virtualNetwork1 | Set-AzVirtualNetwork
```

10.1.0.0/16 adres ön eki ve bir alt ağ ile sanal ağ oluşturun:

```azurepowershell-interactive
# Create the virtual network.
$virtualNetwork2 = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork2 `
  -AddressPrefix 10.1.0.0/16

# Create the subnet configuration.
$subnetConfig = Add-AzVirtualNetworkSubnetConfig `
  -Name Subnet1 `
  -AddressPrefix 10.1.0.0/24 `
  -VirtualNetwork $virtualNetwork2

# Write the subnet configuration to the virtual network.
$virtualNetwork2 | Set-AzVirtualNetwork
```

## <a name="peer-virtual-networks"></a>Sanal ağları eşleme

Eşleme oluşturmaya [Ekle AzVirtualNetworkPeering](/powershell/module/az.network/add-azvirtualnetworkpeering). Aşağıdaki örnekte eşlerden *myVirtualNetwork1* için *myVirtualNetwork2*.

```azurepowershell-interactive
Add-AzVirtualNetworkPeering `
  -Name myVirtualNetwork1-myVirtualNetwork2 `
  -VirtualNetwork $virtualNetwork1 `
  -RemoteVirtualNetworkId $virtualNetwork2.Id
```

Önceki komut yürütüldükten sonra döndürülen Çıkışta gördüğünüz **PeeringState** olduğu *başlatılan*. Eşleme saklanır *başlatılan* gelen eşleme oluşturana kadar durum *myVirtualNetwork2* için *myVirtualNetwork1*. Gelen bir eşleme oluşturma *myVirtualNetwork2* için *myVirtualNetwork1*.

```azurepowershell-interactive
Add-AzVirtualNetworkPeering `
  -Name myVirtualNetwork2-myVirtualNetwork1 `
  -VirtualNetwork $virtualNetwork2 `
  -RemoteVirtualNetworkId $virtualNetwork1.Id
```

Önceki komut yürütüldükten sonra döndürülen Çıkışta gördüğünüz **PeeringState** olduğu *bağlı*. Azure da eşleme durumu değişti *myVirtualNetwork1 myVirtualNetwork2* için eşleme *bağlı*. Onaylamak için eşleme durumunu *myVirtualNetwork1 myVirtualNetwork2* eşleme değiştirildi *bağlı* ile [Get-AzVirtualNetworkPeering](/powershell/module/az.network/get-azvirtualnetworkpeering).

```azurepowershell-interactive
Get-AzVirtualNetworkPeering `
  -ResourceGroupName myResourceGroup `
  -VirtualNetworkName myVirtualNetwork1 `
  | Select PeeringState
```

Bir sanal ağ kuramıyor.%nÇözüm kaynaklarla kadar diğer sanal ağ içindeki kaynaklarla **PeeringState** içinde her iki sanal ağ eşlemeleri için *bağlı*.

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Sonraki bir adımda aralarında iletişim kurabilmeniz için her sanal ağ üzerinde bir sanal makine oluşturun.

### <a name="create-the-first-vm"></a>Birinci sanal makineyi oluşturma

İle bir VM [yeni-AzVM](/powershell/module/az.compute/new-azvm). Aşağıdaki örnekte adlı bir VM oluşturur *myVm1* içinde *myVirtualNetwork1* sanal ağ. `-AsJob` Seçeneği, sonraki adıma devam edebilmesi bu VM için arka planda oluşturur. İstendiğinde kullanıcı adını ve VM ile oturum açmak için kullanmak istediğiniz parolayı girin.

```azurepowershell-interactive
New-AzVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork1" `
  -SubnetName "Subnet1" `
  -ImageName "Win2016Datacenter" `
  -Name "myVm1" `
  -AsJob
```

### <a name="create-the-second-vm"></a>İkinci sanal makineyi oluşturma

```azurepowershell-interactive
New-AzVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork2" `
  -SubnetName "Subnet1" `
  -ImageName "Win2016Datacenter" `
  -Name "myVm2"
```

Sanal makinenin oluşturulması birkaç dakika sürer. Azure sanal makine oluşturur ve PowerShell için bir çıktı döndürür kadar sonraki adımlara devam etmeyin.

## <a name="communicate-between-vms"></a>Sanal makineler arasında iletişim

İnternet'ten bir sanal makinenin genel IP adresi bağlanabilirsiniz. Kullanım [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) bir VM'nin genel IP adresini döndürmek için. Aşağıdaki örnek,*myVm1* sanal makinesinin genel IP adresini döndürür:

```azurepowershell-interactive
Get-AzPublicIpAddress `
  -Name myVm1 `
  -ResourceGroupName myResourceGroup | Select IpAddress
```

Bir Uzak Masaüstü oturumu oluşturmak için aşağıdaki komutu kullanın *myVm1* yerel bilgisayarınızdan VM. `<publicIpAddress>` değerini önceki komutta döndürülen IP adresi ile değiştirin.

```
mstsc /v:<publicIpAddress>
```

Bir Uzak Masaüstü Protokolü (.rdp) dosyası, bilgisayarınıza indirilir, oluşturulup açılır. Kullanıcı adını ve parolasını girin (seçmeniz gerekebilir **daha fazla seçenek**, ardından **farklı bir hesap kullan**sanal Makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için) ve ardından **Tamam** . Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet** veya **Devam**’a tıklayın.

Üzerinde *myVm1* VM, Internet Denetim İletisi Protokolü (ICMP) aracılığıyla Windows Güvenlik Duvarı bu sanal makineden ping atabilirsiniz şekilde etkinleştir *myVm2* PowerShell kullanarak bir sonraki adımda:

```powershell
New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
```

Bu makalede sanal makineler arasında iletişim kurmak için ping kullanılsa da, üretim dağıtımları için Windows Güvenlik Duvarı üzerinden ICMP'ye izin verilmesi önerilmez.

*myVm2* sanal makinesine bağlanmak için, *myVm1* sanal makinesinde bir komut isteminden aşağıdaki komutu girin:

```
mstsc /v:10.1.0.4
```

Ping makinesinde ping'i *myVm1*, artık, bir komut isteminden IP adresine göre üzerinde ping *myVm2* VM:

```
ping 10.0.0.4
```

Dört yanıt alırsınız. Hem *myVm1* hem de *myVm2* sanal makinesine yönelik RDP oturumlarınızın bağlantısını kesin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse [Remove-AzResourcegroup](/powershell/module/az.resources/remove-azresourcegroup) kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, sanal ağ eşlemesi ile aynı Azure bölgesindeki iki ağı bağlama öğrendiniz. Farklı [desteklenen bölgelerde](virtual-network-manage-peering.md#cross-region) ve [farklı Azure aboneliklerinde](create-peering-different-subscriptions.md#powershell) sanal ağları eşleyebilir ve eşleme ile [hub ve bağlı bileşen ağ tasarımları](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) oluşturabilirsiniz. Sanal ağ eşlemesi hakkında daha fazla bilgi için bkz. [Sanal ağ eşlemesine genel bakış](virtual-network-peering-overview.md) ve [Sanal ağ eşlemelerini yönetme](virtual-network-manage-peering.md).

Yapabilecekleriniz [kendi bilgisayarınızı sanal ağa bağlanmak](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bir VPN aracılığıyla ve bir sanal ağdaki veya eşlenmiş sanal ağlardaki kaynaklarla etkileşim kurun. Sanal ağ makalelerde ele görevlerin çoğunu tamamlamak yeniden kullanılabilir betikler için bkz: [betik örnekleri](powershell-samples.md).