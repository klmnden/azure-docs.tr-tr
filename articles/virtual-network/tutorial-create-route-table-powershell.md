---
title: Azure PowerShell ağ trafiğini yönlendirme | Microsoft Docs
description: Bu makalede, PowerShell kullanarak bir yönlendirme tablosu ile ağ trafiğini yönlendirme hakkında bilgi edinin.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
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
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 9b4dc2e48093398077071eb2423a80c86eb62c67
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55894947"
---
# <a name="route-network-traffic-with-a-route-table-using-powershell"></a>PowerShell kullanarak bir yönlendirme tablosu ile ağ trafiğini yönlendirme

Azure varsayılan olarak bir sanal ağ içindeki tüm alt ağlar arasında gerçekleşen trafiği otomatik olarak yönlendirir. Azure’ın varsayılan yönlendirmesini geçersiz kılmak için kendi yönlendirmelerinizi oluşturabilirsiniz. Örneğin, bir ağ sanal gereci üzerinden alt ağlar arasındaki trafiği yönlendirmek isteyebilirsiniz. Bu makalede şunları öğreneceksiniz:

* Yönlendirme tablosu oluşturma
* Yönlendirme oluşturma
* Birden fazla alt ağa sahip bir sanal ağ oluşturma
* Yönlendirme tablosunu bir alt ağ ile ilişkilendirme
* Trafiği yönlendiren bir NVA oluşturma
* Sanal makineleri (VM) farklı alt ağlara dağıtma
* NVA aracılığıyla trafiği bir alt ağdan başka birine yönlendirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure PowerShell modülü 5.4.1 veya sonraki bir sürümünü gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/azurerm/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir. 

## <a name="create-a-route-table"></a>Yönlendirme tablosu oluşturma

Bir yol tablosu oluşturabilmeniz için önce bir kaynak grubu oluşturun [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Aşağıdaki örnekte adlı bir kaynak grubu oluşturur *myResourceGroup* bu makalede oluşturulan tüm kaynaklar için. 

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Yönlendirme tablosu ile oluşturma [yeni AzureRmRouteTable](/powershell/module/azurerm.network/new-azurermroutetable). Aşağıdaki örnekte adlı bir rota tablosu oluşturur *myRouteTablePublic*.

```azurepowershell-interactive
$routeTablePublic = New-AzureRmRouteTable `
  -Name 'myRouteTablePublic' `
  -ResourceGroupName myResourceGroup `
  -location EastUS
```

## <a name="create-a-route"></a>Yönlendirme oluşturma

Rota tablosu nesnesiyle alarak yönlendirme oluşturma [Get-AzureRmRouteTable](/powershell/module/azurerm.network/get-azurermroutetable), bir yol oluşturmak [Ekle AzureRmRouteConfig](/powershell/module/azurerm.network/add-azurermrouteconfig), rota tablosuyla yolyapılandırmasıyazılamadı[Kümesi AzureRmRouteTable](/powershell/module/azurerm.network/set-azurermroutetable). 

```azurepowershell-interactive
Get-AzureRmRouteTable `
  -ResourceGroupName "myResourceGroup" `
  -Name "myRouteTablePublic" `
  | Add-AzureRmRouteConfig `
  -Name "ToPrivateSubnet" `
  -AddressPrefix 10.0.1.0/24 `
  -NextHopType "VirtualAppliance" `
  -NextHopIpAddress 10.0.2.4 `
 | Set-AzureRmRouteTable
```

## <a name="associate-a-route-table-to-a-subnet"></a>Yönlendirme tablosunu bir alt ağ ile ilişkilendirme

Bir yönlendirme tablosunu bir alt ağ ilişkilendirmeden önce bir sanal ağ ve alt ağ oluşturmanız gerekir. [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) ile sanal ağ oluşturun. Aşağıdaki örnekte adlı bir sanal ağ oluşturur *myVirtualNetwork* adres ön eki ile *10.0.0.0/16*.

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

Üç alt ağ ile üç alt ağ yapılandırmalarını oluşturarak oluşturma [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Aşağıdaki örnek için üç alt ağ yapılandırmalarını oluşturur *genel*, *özel*, ve *DMZ* alt ağlar:

```azurepowershell-interactive
$subnetConfigPublic = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Public `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigPrivate = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigDmz = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name DMZ `
  -AddressPrefix 10.0.2.0/24 `
  -VirtualNetwork $virtualNetwork
```

Sanal ağ alt ağ yapılandırmalarını yazmak [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork), sanal ağ alt ağları oluşturur:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

İlişkilendirme *myRouteTablePublic* yol tablosuna *genel* alt ağ ile [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) ve ardından için alt ağ yapılandırmasını yazma sanal ağ ile [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork).

```azurepowershell-interactive
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $virtualNetwork `
  -Name 'Public' `
  -AddressPrefix 10.0.0.0/24 `
  -RouteTable $routeTablePublic | `
Set-AzureRmVirtualNetwork
```

## <a name="create-an-nva"></a>NVA oluşturma

NVA; yönlendirme, güvenlik duvarı oluşturma veya WAN iyileştirmesi gibi ağ işlevlerini gerçekleştiren bir VM'dir.

VM oluşturmadan önce bir ağ arabirimi oluşturun.

### <a name="create-a-network-interface"></a>Ağ arabirimini oluşturun

Bir ağ arabirimi oluşturmadan önce sanal almak sahip olduğunuz kimliğine sahip ağ [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork), alt kimliği ile [Get-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig). Bir ağ arabirimi ile oluşturma [New-Azurermnetworkınterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) içinde *DMZ* IP iletme etkinleştirilmiş olan bir alt ağ:

```azurepowershell-interactive
# Retrieve the virtual network object into a variable.
$virtualNetwork=Get-AzureRmVirtualNetwork `
  -Name myVirtualNetwork `
  -ResourceGroupName myResourceGroup

# Retrieve the subnet configuration into a variable.
$subnetConfigDmz = Get-AzureRmVirtualNetworkSubnetConfig `
  -Name DMZ `
  -VirtualNetwork $virtualNetwork

# Create the network interface.
$nic = New-AzureRmNetworkInterface `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name 'myVmNva' `
  -SubnetId $subnetConfigDmz.Id `
  -EnableIPForwarding
```

### <a name="create-a-vm"></a>VM oluşturma

Bir VM oluşturun ve buna bir mevcut ağ arabirimi eklemek için öncelikle bir VM yapılandırması ile oluşturmanız gerekir [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig). Yapılandırma önceki adımda oluşturduğunuz ağ arabirimini içerir. Bir kullanıcı adı ve parola sorulduğunda, kullanıcı adı ve parola, VM ile oturum açmak istediğiniz seçin. 

```azurepowershell-interactive
# Create a credential object.
$cred = Get-Credential -Message "Enter a username and password for the VM."

# Create a VM configuration.
$vmConfig = New-AzureRmVMConfig `
  -VMName 'myVmNva' `
  -VMSize Standard_DS2 | `
  Set-AzureRmVMOperatingSystem -Windows `
    -ComputerName 'myVmNva' `
    -Credential $cred | `
  Set-AzureRmVMSourceImage `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest | `
  Add-AzureRmVMNetworkInterface -Id $nic.Id
```

VM yapılandırması ile kullanarak VM oluşturma [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Aşağıdaki örnekte adlı bir VM oluşturur *myVmNva*. 

```azurepowershell-interactive
$vmNva = New-AzureRmVM `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -VM $vmConfig `
  -AsJob
```

`-AsJob` Seçeneği, sonraki adıma devam edebilmesi bu VM için arka planda oluşturur.

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Gelen trafiğin doğrulayabilmek sanal ağda iki VM oluşturma *genel* alt yönlendirileceğini *özel* sonraki bir adımda ağ sanal Gereci üzerinden alt ağ. 

Bir VM oluşturma *genel* alt ağ ile [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Aşağıdaki örnekte adlı bir VM oluşturur *myVmPublic* içinde *genel* alt *myVirtualNetwork* sanal ağ. 

```azurepowershell-interactive
New-AzureRmVm `
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
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Private" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmPrivate"
```

Sanal makinenin oluşturulması birkaç dakika sürer. VM oluşturulur ve Azure PowerShell'e çıktı döndürür kadar sonraki adıma geçmeyin.

## <a name="route-traffic-through-an-nva"></a>Trafiği NVA üzerinden yönlendirme

Kullanım [Get-Azurermpublicıpaddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) genel IP adresini döndürmek için *myVmPrivate* VM. Aşağıdaki örnek, genel IP adresini döndürür *myVmPrivate* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
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

Daha sonraki bir adımda yönlendirmeyi test etmek için tracert.exe komutu kullanılır. Tracert Windows Güvenlik Duvarı üzerinden reddedilen Internet Denetim İletisi Protokolü (ICMP) kullanır. PowerShell’den *myVmPrivate* sanal makinesinde aşağıdaki komutu girerek Windows güvenlik duvarı üzerinden ICMP’yi etkinleştirin:

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

Artık gerekli değilse [Remove-AzureRmResourcegroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir yönlendirme tablosu oluşturup bir alt ağ ile ilişkilendirilmiş. Trafiği bir genel alt ağdan özel alt ağa bir basit ağ sanal Gereci oluşturduğunuz. Çeşitli güvenlik duvarı ve WAN iyileştirme dışında gibi ağ işlevleri gerçekleştiren, önceden yapılandırılmış bir ağ sanal gereçlerini dağıtma [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking). Yönlendirme hakkında daha fazla bilgi için bkz. [Yönlendirmeye genel bakış](virtual-networks-udr-overview.md) ve [Yönlendirme tablosunu yönetme](manage-route-table.md).

Bir sanal ağ içinde çok sayıda Azure kaynağına dağıtabilmenize karşın, bazı Azure PaaS hizmetlerinin kaynakları bir sanal ağa dağıtılamaz. Yine de, bazı Azure PaaS hizmetlerinin kaynaklarına erişimi yalnızca bir sanal ağ alt ağından gelecek trafikle kısıtlayabilirsiniz. Bilgi edinmek için bkz. nasıl [PaaS kaynaklarına ağ erişimini kısıtlama](tutorial-restrict-network-access-to-resources-powershell.md).
