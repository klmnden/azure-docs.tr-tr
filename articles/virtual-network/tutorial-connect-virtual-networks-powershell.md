---
title: Sanal Ağ eşlemesi ile - sanal ağlara bağlanabilir PowerShell | Microsoft Docs
description: Bu makalede, eşliği, Azure PowerShell kullanarak sanal ağ ile sanal ağlara bağlanabilir öğrenin.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
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
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 2490b96716519ef749dd1e3a1fbe6846c6b5d999
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-powershell"></a>PowerShell kullanarak sanal ağ eşlemesi ile sanal ağlara bağlanabilir

Sanal ağlar birbirlerine sanal ağ eşlemesi ile bağlayabilirsiniz. Sanal ağlar eşlendikten sonra iki sanal ağlarda bulunan kaynaklar kaynaklar aynı sanal ağda değilmiş gibi aynı gecikme süresi ve bant genişliği ile birbirleri ile iletişim kuramıyor. Bu makalede, bilgi nasıl yapılır:

* İki sanal ağ oluşturma
* Sanal Ağ eşlemesi iki sanal ağlara bağlanabilir
* Her sanal ağ içinde bir sanal makine (VM) dağıtma
* VM'ler arasında iletişim

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Yüklemek ve PowerShell yerel olarak kullanmak seçerseniz, bu makale Azure PowerShell modülü sürümü 5.4.1 gerektirir veya sonraki bir sürümü. Yüklü sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir. 

## <a name="create-virtual-networks"></a>Sanal ağlar oluşturma

Bir sanal ağ oluşturmadan önce sanal ağ ve bu makalede oluşturulan tüm kaynaklar için bir kaynak grubu oluşturmanız gerekir. Bir kaynak grubu ile oluşturmak [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

[New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) ile sanal ağ oluşturun. Aşağıdaki örnek adlı bir sanal ağ oluşturur *myVirtualNetwork1* adres ön ekine sahip *10.0.0.0/16*.

```azurepowershell-interactive
$virtualNetwork1 = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork1 `
  -AddressPrefix 10.0.0.0/16
```

Bir alt ağ yapılandırması ile oluşturma [yeni AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Aşağıdaki örnek, bir alt ağ yapılandırması 10.0.0.0/24 adres ön eki ile oluşturur:

```azurepowershell-interactive
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Subnet1 `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork1
```

Sanal ağ ile alt ağ yapılandırması yazma [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork), alt ağı oluşturur:

```azurepowershell-interactive
$virtualNetwork1 | Set-AzureRmVirtualNetwork
```

10.1.0.0/16 adres öneki ve bir alt ağı ile bir sanal ağ oluşturun:

```azurepowershell-interactive
# Create the virtual network.
$virtualNetwork2 = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork2 `
  -AddressPrefix 10.1.0.0/16

# Create the subnet configuration.
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Subnet1 `
  -AddressPrefix 10.1.0.0/24 `
  -VirtualNetwork $virtualNetwork2

# Write the subnet configuration to the virtual network.
$virtualNetwork2 | Set-AzureRmVirtualNetwork
```

## <a name="peer-virtual-networks"></a>Eş sanal ağlar

İle eşlemesi oluşturmak [Ekle AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering). Aşağıdaki örnek eş *myVirtualNetwork1* için *myVirtualNetwork2*.

```azurepowershell-interactive
Add-AzureRmVirtualNetworkPeering `
  -Name myVirtualNetwork1-myVirtualNetwork2 `
  -VirtualNetwork $virtualNetwork1 `
  -RemoteVirtualNetworkId $virtualNetwork2.Id
```

Önceki komutu yürütüldükten sonra döndürülen çıktısında, gördüğünüz **PeeringState** olan *başlatılan*. Eşleme kalırken *başlatılan* gelen eşlemesi oluşturmak kadar durum *myVirtualNetwork2* için *myVirtualNetwork1*. Gelen eşlemesi oluşturmak *myVirtualNetwork2* için *myVirtualNetwork1*. 

```azurepowershell-interactive
Add-AzureRmVirtualNetworkPeering `
  -Name myVirtualNetwork2-myVirtualNetwork1 `
  -VirtualNetwork $virtualNetwork2 `
  -RemoteVirtualNetworkId $virtualNetwork1.Id
```

Önceki komutu yürütüldükten sonra döndürülen çıktısında, gördüğünüz **PeeringState** olan *bağlı*. Azure eşleme durumunu da değişir *myVirtualNetwork1 myVirtualNetwork2* için eşleme *bağlı*. Onaylamak için eşleme durumu *myVirtualNetwork1 myVirtualNetwork2* eşliği değiştirildi *bağlı* ile [Get-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/get-azurermvirtualnetworkpeering).

```azurepowershell-interactive
Get-AzureRmVirtualNetworkPeering `
  -ResourceGroupName myResourceGroup `
  -VirtualNetworkName myVirtualNetwork1 `
  | Select PeeringState
```

Kadar diğer sanal ağ kaynaklarında bir sanal ağ olamaz iletişim kaynaklarla **PeeringState** hem de sanal ağlarda eşlemeleri için *bağlı*. 

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Sonraki adımda aralarında iletişim kurabilmesi için bir VM her sanal ağ oluşturun.

### <a name="create-the-first-vm"></a>İlk VM oluşturma

Bir VM ile oluşturma [AzureRmVM yeni](/powershell/module/azurerm.compute/new-azurermvm). Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVm1* içinde *myVirtualNetwork1* sanal ağ. `-AsJob` Seçeneği bir sonraki adıma devam etmek için bu VM arka planda oluşturur. İstendiğinde, kullanıcı adını ve VM ile oturum açmak için istediğiniz parolayı girin.

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork1" `
  -SubnetName "Subnet1" `
  -ImageName "Win2016Datacenter" `
  -Name "myVm1" `
  -AsJob
```

### <a name="create-the-second-vm"></a>İkinci VM oluşturma

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork2" `
  -SubnetName "Subnet1" `
  -ImageName "Win2016Datacenter" `
  -Name "myVm2"
```

VM oluşturmak için birkaç dakika sürer. Azure VM oluşturur ve PowerShell çıkışı döndürür kadar sonraki adımlara devam etmeyin.

## <a name="communicate-between-vms"></a>VM'ler arasında iletişim

İnternet'ten VM'in genel IP adresi bağlanabilir. Kullanım [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) VM genel IP adresi dönün. Aşağıdaki örnek genel IP adresi döndürür *myVm1* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVm1 `
  -ResourceGroupName myResourceGroup | Select IpAddress
```

İle Uzak Masaüstü oturumu oluşturmak için aşağıdaki komutu kullanın *myVm1* yerel bilgisayarınızdan VM. Değiştir `<publicIpAddress>` IP adresi ile döndürülen önceki komutu.

```
mstsc /v:<publicIpAddress>
```

Bir Uzak Masaüstü Protokolü (.rdp) dosyası oluşturulur, bilgisayarınıza indirilmeden ve açılır. Kullanıcı adı ve parola girin (seçmek için gerek duyabileceğiniz **daha fazla seçenek**, ardından **farklı bir hesap kullan**, VM oluşturduğunuz sırada girdiğiniz kimlik bilgileri belirtmek için) ve ardından **Tamam** . Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet** veya **Devam**’a tıklayın.

Üzerinde *myVm1* VM, bu sanal makineden ping işlemi yapmak için Internet Denetim İletisi Protokolü (ICMP) üzerinden Windows Güvenlik Duvarı etkinleştir *myVm2* PowerShell kullanarak bir sonraki adımda:

```powershell
New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
```

Bu makalede yer alan VM'ler arasında iletişim kurmak için ping kullanılsa da ICMP üretim dağıtımları için Windows Güvenlik Duvarı aracılığıyla izin vererek önerilmez.

Bağlanmak için *myVm2* VM, bir komut isteminden aşağıdaki komutu girin *myVm1* VM:

```
mstsc /v:10.1.0.4
```

Ping etkinleştirilmiş olduğundan *myVm1*, şimdi onu bir komut isteminden IP adresiyle üzerinde ping *myVm2* VM:

```
ping 10.0.0.4
```

Dört yanıt alırsınız. Hem sizin RDP oturumların bağlantısını kesmek *myVm1* ve *myVm2*.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerektiğinde kullanmak [Remove-AzureRmResourcegroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) kaynak grubu ve içerdiği kaynakların tümünü kaldırmak için.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, sanal ağ eşlemesi ile iki ağ aynı Azure bölgesinde bağlanma öğrendiniz. Ayrıca farklı sanal ağlar eş [bölgeler desteklenen](virtual-network-manage-peering.md#cross-region) ve [farklı Azure abonelikleri](create-peering-different-subscriptions.md#powershell), yanı sıra oluşturma [hub ve bağlı bileşen ağ tasarımları](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) ile eşleme. Sanal Ağ eşlemesi hakkında daha fazla bilgi için bkz: [sanal ağ eşleme genel bakış](virtual-network-peering-overview.md) ve [sanal ağ eşlemeleri yönetme](virtual-network-manage-peering.md).

Yapabilecekleriniz [kendi bilgisayarınızda bir sanal ağa bağlanmak](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bir VPN üzerinden ve sanal ağ veya eşlenmiş sanal ağlar kaynakları ile etkileşim. Birçok sanal ağ makalelerinde ele görevi tamamlamak yeniden kullanılabilir komut dosyaları için bkz: [komut örnekleri](powershell-samples.md).
