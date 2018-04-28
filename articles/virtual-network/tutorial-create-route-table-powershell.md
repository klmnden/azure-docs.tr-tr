---
title: Ağ trafiği Azure PowerShell | Microsoft Docs
description: Bu makalede, PowerShell kullanarak bir yol tablosu ile ağ trafiğini yönlendirmek öğrenin.
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
ms.openlocfilehash: 2aca1de567dbd4d37daf7f9dd7c407b669396a47
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="route-network-traffic-with-a-route-table-using-powershell"></a>PowerShell kullanarak bir yol tablosu ile ağ trafiği yönlendirme

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

Yüklemek ve PowerShell yerel olarak kullanmak seçerseniz, bu makale Azure PowerShell modülü sürümü 5.4.1 gerektirir veya sonraki bir sürümü. Yüklü sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir. 

## <a name="create-a-route-table"></a>Yönlendirme tablosu oluşturma

Bir yol tablosu oluşturmadan önce bir kaynak grubuyla oluşturmanız [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroup* bu makalede oluşturulan tüm kaynaklar için. 

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Bir yol tablosu ile oluşturma [yeni AzureRmRouteTable](/powershell/module/azurerm.network/new-azurermroutetable). Aşağıdaki örnek, adında bir yol tablosu oluşturur *myRouteTablePublic*.

```azurepowershell-interactive
$routeTablePublic = New-AzureRmRouteTable `
  -Name 'myRouteTablePublic' `
  -ResourceGroupName myResourceGroup `
  -location EastUS
```

## <a name="create-a-route"></a>Yönlendirme oluşturma

Rota tablosu nesnesiyle alarak bir yol oluşturma [Get-AzureRmRouteTable](/powershell/module/azurerm.network/get-azurermroutetable), bir rota oluşturmak [Ekle AzureRmRouteConfig](/powershell/module/azurerm.network/add-azurermrouteconfig), ileyoltablosunayolyapılandırmasıyazma[Kümesi AzureRmRouteTable](/powershell/module/azurerm.network/set-azurermroutetable). 

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

Bir alt ağ için bir yol tablosu ilişkilendirmeden önce bir sanal ağ ve alt ağ oluşturmanız gerekir. [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) ile sanal ağ oluşturun. Aşağıdaki örnek adlı bir sanal ağ oluşturur *myVirtualNetwork* adres ön ekine sahip *10.0.0.0/16*.

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

Üç alt ağ yapılandırmaları oluşturarak üç alt ağ oluşturmak [yeni AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Aşağıdaki örnek için üç alt ağ yapılandırmalarını oluşturur *ortak*, *özel*, ve *DMZ* alt ağlar:

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

Sanal ağ ile alt ağ yapılandırmalarını yazmak [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork), sanal ağ alt ağlar oluşturur:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

İlişkilendirme *myRouteTablePublic* yol tablosuna *ortak* alt ağ ile [kümesi AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) ve alt ağ yapılandırması yazma sanal ağ ile [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork).

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

Bir VM oluşturmadan önce bir ağ arabirimi oluşturur.

### <a name="create-a-network-interface"></a>Bir ağ arabirimi oluştur

Bir ağ arabirimi oluşturmadan önce sanal almak zorunda kimliğine sahip ağ [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork), alt kimliği ile [Get-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig). Bir ağ arabirimi oluştur [yeni AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) içinde *DMZ* IP iletmenin etkin olan bir alt ağ:

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

Bir VM oluşturun ve mevcut bir ağ arabirimi için eklemek için önce bir VM yapılandırması ile oluşturmalısınız [yeni AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig). Yapılandırma, önceki adımda oluşturulan ağ arabirimi içerir. Bir kullanıcı adı ve parola istendiğinde, kullanıcı adı ve parola ile VM içine günlüğe kaydetmek istediğiniz seçin. 

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

VM yapılandırmayla kullanarak VM oluşturma [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVmNva*. 

```azurepowershell-interactive
$vmNva = New-AzureRmVM `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -VM $vmConfig `
  -AsJob
```

`-AsJob` Seçeneği bir sonraki adıma devam etmek için bu VM arka planda oluşturur.

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Bu trafiğinden doğrulamak için iki VM sanal ağ oluşturma *ortak* alt ağ için yönlendirilir *özel* sonraki adımda ağ sanal gereç aracılığıyla alt ağ. 

Bir VM oluşturma *ortak* alt ağ ile [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVmPublic* içinde *ortak* alt *myVirtualNetwork* sanal ağ. 

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

Sanal makinenin oluşturulması birkaç dakika sürer. Sonraki adıma VM oluşturulur ve Azure PowerShell çıkış döndürür kadar devam yok.

## <a name="route-traffic-through-an-nva"></a>Trafiği NVA üzerinden yönlendirme

Kullanım [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) genel IP adresi döndürülecek *myVmPrivate* VM. Aşağıdaki örnek genel IP adresi döndürür *myVmPrivate* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmPrivate `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

İle Uzak Masaüstü oturumu oluşturmak için aşağıdaki komutu kullanın *myVmPrivate* yerel bilgisayarınızdan VM. `<publicIpAddress>` değerini önceki komutta döndürülen IP adresi ile değiştirin.

```
mstsc /v:<publicIpAddress>
```

İndirilen RDP dosyasını açın. İstendiğinde **Bağlan**’ı seçin.

Sanal makineyi oluştururken belirttiğiniz kullanıcı adını ve parolayı girin (sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan**’ı seçmeniz gerekebilir), ardından **Tamam**’ı seçin. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet**’i seçin. 

Bir sonraki adımda tracert.exe komut yönlendirme test etmek için kullanılır. Tracert Windows Güvenlik Duvarı üzerinden reddedildi Internet Denetim İletisi Protokolü (ICMP) kullanır. PowerShell’den *myVmPrivate* sanal makinesinde aşağıdaki komutu girerek Windows güvenlik duvarı üzerinden ICMP’yi etkinleştirin:

```powershell
New-NetFirewallRule -DisplayName "Allow ICMPv4-In" -Protocol ICMPv4
```

İzleme yolu, bu makaledeki yönlendirme test etmek için kullanılsa da ICMP üretim dağıtımları için Windows Güvenlik Duvarı aracılığıyla izin vererek önerilmez.

[IP iletmeyi etkinleştirme](#enable-ip-forwarding) bölümünde, VM'nin ağ arabirimi için Azure’da IP iletimini etkinleştirdiniz. VM içindeki işletim sistemi veya VM içinde çalışan bir uygulama da ağ trafiğini iletebilmelidir. İşletim sistemi içinde IP iletimini etkinleştirmeniz *myVmNva*.

Bir komut isteminden *myVmPrivate* VM, Uzak Masaüstü ' *myVmNva*:

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

Artık gerektiğinde kullanmak [Remove-AzureRmResourcegroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) kaynak grubu ve içerdiği kaynakların tümünü kaldırmak için.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir yol tablosu oluşturulur ve bir alt ağa ilişkilendirilmiş. Ortak bir alt ağ trafiği için özel bir alt ağa yönlendirilmiş bir basit ağ sanal gereç oluşturuldu. Güvenlik Duvarı ve WAN iyileştirme dışında gibi ağ işlevleri gerçekleştirmek önceden yapılandırılmış ağ sanal Gereçleri çeşitli dağıtmak [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking). Yönlendirme hakkında daha fazla bilgi için bkz. [Yönlendirmeye genel bakış](virtual-networks-udr-overview.md) ve [Yönlendirme tablosunu yönetme](manage-route-table.md).

Bir sanal ağ içinde çok sayıda Azure kaynağına dağıtabilmenize karşın, bazı Azure PaaS hizmetlerinin kaynakları bir sanal ağa dağıtılamaz. Yine de, bazı Azure PaaS hizmetlerinin kaynaklarına erişimi yalnızca bir sanal ağ alt ağından gelecek trafikle kısıtlayabilirsiniz. Bilgi edinmek için bkz [PaaS kaynaklarına ağ erişimini kısıtlayabilir](tutorial-restrict-network-access-to-resources-powershell.md).
