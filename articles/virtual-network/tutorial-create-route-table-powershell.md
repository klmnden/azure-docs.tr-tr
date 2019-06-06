---
title: Azure PowerShell ağ trafiğini yönlendirme | Microsoft Docs
description: Bu makalede, PowerShell kullanarak bir yönlendirme tablosu ile ağ trafiğini yönlendirme hakkında bilgi edinin.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
Customer intent: I want to route traffic from one subnet, to a different subnet, through a network virtual appliance.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: cd13b3a7a3bc4d5a80e44d146e08c14e81ffdb60
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66730062"
---
# <a name="route-network-traffic-with-a-route-table-using-powershell"></a>PowerShell kullanarak bir yönlendirme tablosu ile ağ trafiğini yönlendirme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure varsayılan olarak bir sanal ağ içindeki tüm alt ağlar arasında gerçekleşen trafiği otomatik olarak yönlendirir. Azure’ın varsayılan yönlendirmesini geçersiz kılmak için kendi yönlendirmelerinizi oluşturabilirsiniz. Örneğin, bir ağ sanal gereci üzerinden alt ağlar arasındaki trafiği yönlendirmek isteyebilirsiniz. Bu makalede şunları öğreneceksiniz:

* Yönlendirme tablosu oluşturma
* Yönlendirme oluşturma
* Birden fazla alt ağa sahip bir sanal ağ oluşturma
* Yönlendirme tablosunu bir alt ağ ile ilişkilendirme
* Trafiği yönlendiren bir NVA oluşturma
* Sanal makineleri (VM) farklı alt ağlara dağıtma
* NVA aracılığıyla trafiği bir alt ağdan başka birine yönlendirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu makale Azure PowerShell modülü sürüm 1.0.0 gerekir veya üzeri. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-route-table"></a>Yönlendirme tablosu oluşturma

Bir yol tablosu oluşturabilmeniz için önce bir kaynak grubu oluşturun [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Aşağıdaki örnekte adlı bir kaynak grubu oluşturur *myResourceGroup* bu makalede oluşturulan tüm kaynaklar için.

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Yönlendirme tablosu ile oluşturma [yeni AzRouteTable](/powershell/module/az.network/new-azroutetable). Aşağıdaki örnekte adlı bir rota tablosu oluşturur *myRouteTablePublic*.

```azurepowershell-interactive
$routeTablePublic = New-AzRouteTable `
  -Name 'myRouteTablePublic' `
  -ResourceGroupName myResourceGroup `
  -location EastUS
```

## <a name="create-a-route"></a>Yönlendirme oluşturma

Rota tablosu nesnesiyle alarak yönlendirme oluşturma [Get-AzRouteTable](/powershell/module/az.network/get-azroutetable), bir yol oluşturmak [Ekle AzRouteConfig](/powershell/module/az.network/add-azrouteconfig), rota tablosuyla yol yapılandırması yazılamadı [ Set-AzRouteTable](/powershell/module/az.network/set-azroutetable).

```azurepowershell-interactive
Get-AzRouteTable `
  -ResourceGroupName "myResourceGroup" `
  -Name "myRouteTablePublic" `
  | Add-AzRouteConfig `
  -Name "ToPrivateSubnet" `
  -AddressPrefix 10.0.1.0/24 `
  -NextHopType "VirtualAppliance" `
  -NextHopIpAddress 10.0.2.4 `
 | Set-AzRouteTable
```

## <a name="associate-a-route-table-to-a-subnet"></a>Yönlendirme tablosunu bir alt ağ ile ilişkilendirme

Bir yönlendirme tablosunu bir alt ağ ilişkilendirmeden önce bir sanal ağ ve alt ağ oluşturmanız gerekir. İle sanal ağ oluşturma [yeni AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork). Aşağıdaki örnekte adlı bir sanal ağ oluşturur *myVirtualNetwork* adres ön eki ile *10.0.0.0/16*.

```azurepowershell-interactive
$virtualNetwork = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

Üç alt ağ ile üç alt ağ yapılandırmalarını oluşturarak oluşturma [yeni AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig). Aşağıdaki örnek için üç alt ağ yapılandırmalarını oluşturur *genel*, *özel*, ve *DMZ* alt ağlar:

```azurepowershell-interactive
$subnetConfigPublic = Add-AzVirtualNetworkSubnetConfig `
  -Name Public `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigPrivate = Add-AzVirtualNetworkSubnetConfig `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigDmz = Add-AzVirtualNetworkSubnetConfig `
  -Name DMZ `
  -AddressPrefix 10.0.2.0/24 `
  -VirtualNetwork $virtualNetwork
```

Sanal ağ alt ağ yapılandırmalarını yazmak [kümesi AzVirtualNetwork](/powershell/module/az.network/Set-azVirtualNetwork), sanal ağ alt ağları oluşturur:

```azurepowershell-interactive
$virtualNetwork | Set-AzVirtualNetwork
```

İlişkilendirme *myRouteTablePublic* yol tablosuna *genel* alt ağ ile [kümesi AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig) ve ardından için alt ağ yapılandırmasını yazma sanal ağ ile [kümesi AzVirtualNetwork](/powershell/module/az.network/set-azvirtualnetwork).

```azurepowershell-interactive
Set-AzVirtualNetworkSubnetConfig `
  -VirtualNetwork $virtualNetwork `
  -Name 'Public' `
  -AddressPrefix 10.0.0.0/24 `
  -RouteTable $routeTablePublic | `
Set-AzVirtualNetwork
```

## <a name="create-an-nva"></a>NVA oluşturma

NVA; yönlendirme, güvenlik duvarı oluşturma veya WAN iyileştirmesi gibi ağ işlevlerini gerçekleştiren bir VM'dir.

VM oluşturmadan önce bir ağ arabirimi oluşturun.

### <a name="create-a-network-interface"></a>Ağ arabirimini oluşturun

Bir ağ arabirimi oluşturmadan önce sanal almak sahip olduğunuz kimliğine sahip ağ [Get-AzVirtualNetwork](/powershell/module/az.network/get-azvirtualnetwork), alt kimliği ile [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/get-azvirtualnetworksubnetconfig). Bir ağ arabirimi ile oluşturma [yeni AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) içinde *DMZ* IP iletme etkinleştirilmiş olan bir alt ağ:

```azurepowershell-interactive
# Retrieve the virtual network object into a variable.
$virtualNetwork=Get-AzVirtualNetwork `
  -Name myVirtualNetwork `
  -ResourceGroupName myResourceGroup

# Retrieve the subnet configuration into a variable.
$subnetConfigDmz = Get-AzVirtualNetworkSubnetConfig `
  -Name DMZ `
  -VirtualNetwork $virtualNetwork

# Create the network interface.
$nic = New-AzNetworkInterface `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name 'myVmNva' `
  -SubnetId $subnetConfigDmz.Id `
  -EnableIPForwarding
```

### <a name="create-a-vm"></a>VM oluşturma

Bir VM oluşturun ve buna bir mevcut ağ arabirimi eklemek için öncelikle bir VM yapılandırması ile oluşturmanız gerekir [yeni AzVMConfig](/powershell/module/az.compute/new-azvmconfig). Yapılandırma önceki adımda oluşturduğunuz ağ arabirimini içerir. Bir kullanıcı adı ve parola sorulduğunda, kullanıcı adı ve parola, VM ile oturum açmak istediğiniz seçin.

```azurepowershell-interactive
# Create a credential object.
$cred = Get-Credential -Message "Enter a username and password for the VM."

# Create a VM configuration.
$vmConfig = New-AzVMConfig `
  -VMName 'myVmNva' `
  -VMSize Standard_DS2 | `
  Set-AzVMOperatingSystem -Windows `
    -ComputerName 'myVmNva' `
    -Credential $cred | `
  Set-AzVMSourceImage `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest | `
  Add-AzVMNetworkInterface -Id $nic.Id
```

VM yapılandırması ile kullanarak VM oluşturma [New-AzVM](/powershell/module/az.compute/new-azvm). Aşağıdaki örnekte adlı bir VM oluşturur *myVmNva*.

```azurepowershell-interactive
$vmNva = New-AzVM `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -VM $vmConfig `
  -AsJob
```

`-AsJob` Seçeneği, sonraki adıma devam edebilmesi bu VM için arka planda oluşturur.

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Gelen trafiğin doğrulayabilmek sanal ağda iki VM oluşturma *genel* alt yönlendirileceğini *özel* sonraki bir adımda ağ sanal Gereci üzerinden alt ağ.

Bir VM oluşturma *genel* alt ağ ile [New-AzVM](/powershell/module/az.compute/new-azvm). Aşağıdaki örnekte adlı bir VM oluşturur *myVmPublic* içinde *genel* alt *myVirtualNetwork* sanal ağ.

```azurepowershell-interactive
New-AzVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Public" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmPublic" `
  -AsJob
```

Bir VM oluşturma *özel* alt ağ.

```azurepowershell-interactive
New-AzVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Private" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmPrivate"
```

Sanal makinenin oluşturulması birkaç dakika sürer. VM oluşturulur ve Azure PowerShell'e çıktı döndürür kadar sonraki adıma geçmeyin.

## <a name="route-traffic-through-an-nva"></a>Trafiği NVA üzerinden yönlendirme

Kullanım [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) genel IP adresini döndürmek için *myVmPrivate* VM. Aşağıdaki örnek, genel IP adresini döndürür *myVmPrivate* VM:

```azurepowershell-interactive
Get-AzPublicIpAddress `
  -Name myVmPrivate `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Bir Uzak Masaüstü oturumu oluşturmak için aşağıdaki komutu kullanın *myVmPrivate* yerel bilgisayarınızdan VM. `<publicIpAddress>` değerini önceki komutta döndürülen IP adresi ile değiştirin.

```
mstsc /v:<publicIpAddress>
```

İndirilen RDP dosyasını açın. İstendiğinde **Bağlan**’ı seçin.

Sanal makineyi oluştururken belirttiğiniz kullanıcı adını ve parolayı girin (sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan**’ı seçmeniz gerekebilir), ardından **Tamam**’ı seçin. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet**’i seçin.

Bir sonraki adımda `tracert.exe` komutu yönlendirmeyi test etmek için kullanılır. Tracert Windows Güvenlik Duvarı üzerinden reddedilen Internet Denetim İletisi Protokolü (ICMP) kullanır. PowerShell’den *myVmPrivate* sanal makinesinde aşağıdaki komutu girerek Windows güvenlik duvarı üzerinden ICMP’yi etkinleştirin:

```powershell
New-NetFirewallRule -DisplayName "Allow ICMPv4-In" -Protocol ICMPv4
```

Bu makalede yönlendirmeyi test etmek için izleme yönlendirme kullanılsa da, üretim dağıtımları için Windows Güvenlik Duvarı üzerinden ICMP'ye izin verilmesi önerilmez.

Sanal makinenin ağ arabiriminde IP iletimini Etkinleştir için azure'da IP iletimini etkinleştirdiğiniz. VM içindeki işletim sistemi veya VM içinde çalışan bir uygulama da ağ trafiğini iletebilmelidir. İşletim sistemi içinde IP iletimini etkinleştirmeniz *myVmNva*.

Bir komut isteminden *myVmPrivate* VM, Uzak Masaüstü Bağlantısı *myVmNva*:

``` 
mstsc /v:myvmnva
```

İşletim sistemi içinde IP iletmeyi etkinleştirmek için *myVmNva* sanal makinesinden PowerShell’de aşağıdaki komutu girin:

```powershell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
```

*myVmNva* sanal makinesini yeniden başlatarak uzak masaüstü oturumunun bağlantısını da kesin.

*myVmPrivate* VM bağlantısı devam ederken, *myVmNva* VM yeniden başlatıldıktan sonra *myVmPublic* VM ile bir uzak masaüstü oturumu oluşturun:

``` 
mstsc /v:myVmPublic
```

PowerShell’den *myVmPublic* sanal makinesinde aşağıdaki komutu girerek Windows güvenlik duvarı üzerinden ICMP’yi etkinleştirin:

```powershell
New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
```

Ağ trafiğinin *myVmPrivate* sanal makinesinden *myVmPublic* sanal makinesine yönlendirilmesini test etmek için *myVmPublic* sanal makinesinde PowerShell’den aşağıdaki komutu girin:

```
tracert myVmPrivate
```

Yanıt aşağıdaki örneğe benzer:

```
Tracing route to myVmPrivate.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
over a maximum of 30 hops:

1    <1 ms     *        1 ms  10.0.2.4
2     1 ms     1 ms     1 ms  10.0.1.4

Trace complete.
```

İlk atlamanın, NVA özel IP adresi olan 10.0.2.4 olduğunu görebilirsiniz. İkinci atlama ise *myVmPrivate* sanal makinesinin özel IP adresi olan 10.0.1.4’tür. *myRouteTablePublic* yönlendirme tablosuna eklenip *Genel* alt ağ ile ilişkilendirilen yönlendirme, Azure’un trafiği doğrudan *Özel* alt ağına yönlendirmek yerine NVA aracılığıyla yönlendirmesine neden olmuştur.

*myVmPublic* VM ile uzak masaüstü oturumunu kapatın. *myVmPrivate* VM bağlantınız hala açıktır.

Ağ trafiğinin *myVmPublic* sanal makinesinden *myVmPrivate* sanal makinesine yönlendirilmesini test etmek için *myVmPrivate* sanal makinesinde bir komut isteminden aşağıdaki komutu girin:

```
tracert myVmPublic
```

Yanıt aşağıdaki örneğe benzer:

```
Tracing route to myVmPublic.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
over a maximum of 30 hops:

1     1 ms     1 ms     1 ms  10.0.0.4

Trace complete.
```

Trafiğin *myVmPrivate* sanal makinesinden *myVmPublic* sanal makinesine doğrudan yönlendirildiğini görebilirsiniz. Varsayılan olarak Azure, trafiği doğrudan alt ağlar arasında yönlendirir.

*myVmPrivate* VM ile uzak masaüstü oturumunu kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse [Remove-AzResourcegroup](/powershell/module/az.resources/remove-azresourcegroup) kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir yönlendirme tablosu oluşturup bir alt ağ ile ilişkilendirilmiş. Trafiği bir genel alt ağdan özel alt ağa bir basit ağ sanal Gereci oluşturduğunuz. Çeşitli güvenlik duvarı ve WAN iyileştirme dışında gibi ağ işlevleri gerçekleştiren, önceden yapılandırılmış bir ağ sanal gereçlerini dağıtma [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking). Yönlendirme hakkında daha fazla bilgi için bkz. [Yönlendirmeye genel bakış](virtual-networks-udr-overview.md) ve [Yönlendirme tablosunu yönetme](manage-route-table.md).

Bir sanal ağ içinde çok sayıda Azure kaynağına dağıtabilmenize karşın, bazı Azure PaaS hizmetlerinin kaynakları bir sanal ağa dağıtılamaz. Yine de, bazı Azure PaaS hizmetlerinin kaynaklarına erişimi yalnızca bir sanal ağ alt ağından gelecek trafikle kısıtlayabilirsiniz. Bilgi edinmek için bkz. nasıl [PaaS kaynaklarına ağ erişimini kısıtlama](tutorial-restrict-network-access-to-resources-powershell.md).