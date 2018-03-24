---
title: Ağ trafiği - Azure PowerShell | Microsoft Docs
description: PowerShell kullanarak bir yol tablosu ile ağ trafiğini yönlendirmek öğrenin.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: f7be6aa58c6779150d3e79893e6e179d08611567
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="route-network-traffic-with-a-route-table-using-powershell"></a>PowerShell kullanarak bir yol tablosu ile ağ trafiği yönlendirme

Azure otomatik olarak yollar varsayılan olarak bir sanal ağ içindeki tüm alt ağlar arasında trafiği. Azure'nın geçersiz kılmak için kendi Rota oluşturabilmeniz için varsayılan yönlendirme. Örneğin, bir ağ sanal gereç (NVA) aracılığıyla alt ağlar arasında trafiği yönlendirmek istiyorsanız, özel yollar oluşturma olanağı yararlıdır. Bu makalede, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Rota tablosu oluşturma
> * Bir yol oluşturma
> * Birden çok alt ağı ile bir sanal ağ oluşturma
> * Bir alt ağ için bir yol tablosu ilişkilendirme
> * Trafiğini yönlendiren bir NVA oluşturma
> * Sanal makineler (VM) farklı alt dağıtma
> * Bir NVA aracılığıyla başka bir yolu trafiğini bir alt ağdan

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Yüklemek ve PowerShell yerel olarak kullanmak seçerseniz, bu makale Azure PowerShell modülü sürümü 5.4.1 gerektirir veya sonraki bir sürümü. Yüklü sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir. 

## <a name="create-a-route-table"></a>Rota tablosu oluşturma

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

## <a name="create-a-route"></a>Bir yol oluşturma

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

## <a name="associate-a-route-table-to-a-subnet"></a>Bir alt ağ için bir yol tablosu ilişkilendirme

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

## <a name="create-an-nva"></a>Bir NVA oluşturma

Bir NVA yönlendirme, saldırısından veya WAN iyileştirmesi gibi bir ağ işlevi gerçekleştiren bir VM'dir.

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

VM oluşturmak için birkaç dakika sürer. Sonraki adıma VM oluşturulur ve Azure PowerShell çıkış döndürür kadar devam yok.

## <a name="route-traffic-through-an-nva"></a>Bir NVA arasında trafiği yönlendirme

Kullanım [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) genel IP adresi döndürülecek *myVmPrivate* VM. Aşağıdaki örnek genel IP adresi döndürür *myVmPrivate* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmPrivate `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

İle Uzak Masaüstü oturumu oluşturmak için aşağıdaki komutu kullanın *myVmPrivate* yerel bilgisayarınızdan VM. Değiştir `<publicIpAddress>` IP adresi ile döndürülen önceki komutu.

```
mstsc /v:<publicIpAddress>
```

İndirilen RDP dosyasını açın. İstenirse, seçin **Bağlan**.

Kullanıcı adını ve VM oluştururken belirttiğiniz parolayı girin (seçmek için gerek duyabileceğiniz **daha fazla seçenek**, ardından **farklı bir hesap kullan**, VM oluşturduğunuz sırada girdiğiniz kimlik bilgileri belirtmek için), ardından **Tamam**. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Seçin **Evet** bağlantı ile devam etmek için. 

Bir sonraki adımda tracert.exe komut yönlendirme test etmek için kullanılır. Tracert Windows Güvenlik Duvarı üzerinden reddedildi Internet Denetim İletisi Protokolü (ICMP) kullanır. ICMP Powershell'den aşağıdaki komutu girerek Windows Güvenlik Duvarı aracılığıyla etkinleştirin:

```powershell
New-NetFirewallRule ???DisplayName ???Allow ICMPv4-In??? ???Protocol ICMPv4
```

Bu makalede yönlendirme test etmek için tracert kullanılsa da ICMP üretim dağıtımları için Windows Güvenlik Duvarı aracılığıyla izin vererek önerilmez.

İşletim sistemi içinde IP iletimini etkinleştirmeniz *myVmNva* yer alan aşağıdaki adımları tamamlayarak *myVmPrivate* VM:

Uzak Masaüstü ' *myVmNva* VM aşağıdaki PowerShell komutu ile:

``` 
mstsc /v:myvmnva
```
    
İşletim sistemi içinde iletme IP etkinleştirmek için PowerShell içinde aşağıdaki komutu girin:

```powershell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
```
    
Ayrıca Uzak Masaüstü oturumu bağlantısını keser VM'yi yeniden başlatın.

Halen bağlı sırasında *myVmPrivate* VM, sonra *myVmNva* VM yeniden başlatma, Uzak Masaüstü oturumu oluşturmak *myVmPublic* VM şu komutla:

``` 
mstsc /v:myVmPublic
```
    
ICMP Powershell'den aşağıdaki komutu girerek Windows Güvenlik Duvarı aracılığıyla etkinleştirin:

```powershell
New-NetFirewallRule ???DisplayName ???Allow ICMPv4-In??? ???Protocol ICMPv4
```

Ağ trafiği yönlendirmesini test etmek için *myVmPrivate* VM'den *myVmPublic* VM Powershell'den aşağıdaki komutu girin:

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
      
İlk atlama ağ sanal gereç ait özel IP adresi 10.0.2.4 olduğunu görebilirsiniz. İkinci atlama 10.0.1.4, özel IP adresi olan *myVmPrivate* VM. Eklenen rota *myRouteTablePublic* rota tablosu ve ilişkili *ortak* alt neden NVA aracılığıyla yerine doğrudan trafiği yönlendirmek Azure *özel* alt ağ.

Uzak Masaüstü oturumu kapatmak *myVmPublic* , hala bağlı bırakır VM *myVmPrivate* VM.
Ağ trafiği yönlendirmesini test etmek için *myVmPublic* VM'den *myVmPrivate* VM, bir komut isteminden aşağıdaki komutu girin:

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

Trafik, doğrudan yönlendirilir görebilirsiniz *myVmPrivate* VM *myVmPublic* VM. Varsayılan olarak, Azure yollar doğrudan alt ağlar arasında trafiği.

Uzak Masaüstü oturumu kapatmak *myVmPrivate* VM.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerektiğinde kullanmak [Remove-AzureRmResourcegroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) kaynak grubu ve içerdiği kaynakların tümünü kaldırmak için.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir yol tablosu oluşturulur ve bir alt ağa ilişkilendirilmiş. Ortak bir alt ağ trafiği için özel bir alt ağa yönlendirilmiş bir basit ağ sanal gereç oluşturuldu. Güvenlik Duvarı ve WAN iyileştirme dışında gibi ağ işlevleri gerçekleştirmek önceden yapılandırılmış ağ sanal Gereçleri çeşitli dağıtmak [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking). Yönlendirme tabloları üretim kullanımı için dağıtmadan önce baştan sona ile öğrenmeniz olduğunu önerilir [Azure'da yönlendirme](virtual-networks-udr-overview.md), [Yönet yol tablolarını](manage-route-table.md), ve [Azuresınırlar](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

Bir sanal ağ içinde birçok Azure kaynakları dağıtabilirsiniz, ancak bazı Azure PaaS hizmetler için kaynaklar sanal bir ağa dağıtılamıyor. Hala erişimi bazı Azure PaaS Hizmetleri'nden trafik için yalnızca bir sanal ağ alt kaynaklara yine de kısıtlayabilirsiniz. Ağ erişimi Azure PaaS kaynaklarına erişimi kısıtlayabilir öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Ağ erişimi PaaS kaynaklarına erişimi kısıtlayabilir](tutorial-restrict-network-access-to-resources-powershell.md)