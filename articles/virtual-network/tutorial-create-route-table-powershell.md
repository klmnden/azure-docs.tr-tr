---
title: "Ağ trafiği - PowerShell | Microsoft Docs"
description: "PowerShell kullanarak bir yol tablosu ile ağ trafiğini yönlendirmek öğrenin."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/05/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: f91b143c75a82aa6760796770b3ae4d0e4ec53dd
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="route-network-traffic-with-a-route-table-using-powershell"></a>PowerShell kullanarak bir yol tablosu ile ağ trafiği yönlendirme

Azure otomatik olarak yollar varsayılan olarak bir sanal ağ içindeki tüm alt ağlar arasında trafiği. Azure'nın geçersiz kılmak için kendi Rota oluşturabilmeniz için varsayılan yönlendirme. Örneğin, bir güvenlik duvarı üzerinden alt ağlar arasında trafiği yönlendirmek istiyorsanız, özel yollar oluşturma olanağı yararlıdır. Bu makalede, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Rota tablosu oluşturma
> * Bir yol oluşturma
> * Bir sanal ağ alt ağı için bir yol tablosu ilişkilendirme
> * Test yönlendirme
> * Yönlendirme sorunlarını giderme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Yüklemek ve PowerShell yerel olarak kullanmak seçerseniz, bu makale Azure PowerShell modülü sürümü 5.4.1 gerektirir veya sonraki bir sürümü. Çalıştırma `Get-Module -ListAvailable AzureRM` yüklü olan sürümü bulunamıyor. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir. 

## <a name="create-a-route-table"></a>Rota tablosu oluşturma

Varsayılan olarak bir sanal ağdaki tüm alt ağlar arasında trafiği Azure yollar. Azure'nın varsayılan yollar hakkında daha fazla bilgi için bkz: [sistem yolları](virtual-networks-udr-overview.md). Azure'nın varsayılan yönlendirme geçersiz kılmak için rotaları içeren bir yol tablosu oluşturmanız ve bir sanal ağ alt ağı için yol tablosu ilişkilendirebilirsiniz.

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

Bir rota tablosu sıfır veya daha fazla yol içerir. Rota tablosu nesnesiyle alarak bir yol oluşturma [Get-AzureRmRouteTable](/powershell/module/azurerm.network/get-azurermroutetable), bir rota oluşturmak [Ekle AzureRmRouteConfig](/powershell/module/azurerm.network/add-azurermrouteconfig), ileyoltablosunayolyapılandırmasıyazma[Kümesi AzureRmRouteTable](/powershell/module/azurerm.network/set-azurermroutetable). 

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

Rota 10.0.2.4 IP adresiyle bir ağ sanal gereç aracılığıyla 10.0.1.0/24 adres ön eki giden tüm trafiği yönlendirir. Ağ sanal gereç ve alt ağ belirtilen adres ön eki ile daha sonraki adımlarda oluşturulur. Rota yönlendirme, doğrudan alt ağlar arasında trafiği yönlendirir Azure'nın varsayılan değerini geçersiz kılar. Her yol, bir sonraki atlama türü belirtir. Sonraki atlama türü trafiği yönlendirmek nasıl Azure bildirir. Bu örnekte, sonraki atlama türü olan *değerinin VirtualAppliance*. Kullanılabilir tüm sonraki atlama türlerini Azure ve bunların ne zaman kullanılacağı hakkında daha fazla bilgi için bkz: [türleri'sonraki atlama](virtual-networks-udr-overview.md#custom-routes).

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

Adres öneklerini, sanal ağ için tanımlanan adres ön eki içinde olmalıdır. Alt ağ adres öneklerini birbirleri ile örtüşemez.

Sanal ağ ile alt ağ yapılandırmalarını yazmak [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork), sanal ağ alt ağlar oluşturur:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

Bir yol tablosu sıfır veya daha fazla alt ağlara ilişkilendirebilirsiniz. Bir alt ağ için ilişkili sıfır veya bir yol tablosu olabilir. Bir alt ağdan giden trafik, Azure'nın varsayılan yollar ve bir yol tablosu bir alt ağa ilişkilendirmek için eklediğiniz tüm özel yollar göre yönlendirilir. İlişkilendirme *myRouteTablePublic* yol tablosuna *ortak* alt ağ ile [kümesi AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) ve alt ağ yapılandırması yazma sanal ağ ile [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork).

```azurepowershell-interactive
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $virtualNetwork `
  -Name 'Public' `
  -AddressPrefix 10.0.0.0/24 `
  -RouteTable $routeTablePublic | `
Set-AzureRmVirtualNetwork
```

Yönlendirme tabloları üretim kullanımı için dağıtmadan önce baştan sona ile öğrenmeniz olduğunu önerilir [Azure'da yönlendirme](virtual-networks-udr-overview.md) ve [Azure sınırları](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="test-routing"></a>Test yönlendirme

Yönlendirme sınamak için önceki adımda oluşturduğunuz rota üzerinden yönlendiren ağ sanal gereç olarak hizmet veren bir sanal makine oluşturacaksınız. Ağ sanal gereç oluşturduktan sonra bir sanal makineye dağıtacaksınız *ortak* ve *özel* alt ağlar. Ardından gelen trafiği yönlendirmek *ortak* alt ağına *özel* alt ağ sanal gereç aracılığıyla.

### <a name="create-a-network-virtual-appliance"></a>Ağ sanal uygulaması oluşturma

Bir ağ uygulaması çalıştıran bir sanal makine, genellikle bir ağ sanal gereç da adlandırılır. Ağ sanal Gereçleri genellikle ağ trafiğini almak, bazı eylemleri, ardından İleri gerçekleştirmek veya ağ uygulamasında yapılandırılmış mantığı göre ağ trafiği bırakılamıyor. 

#### <a name="create-a-network-interface"></a>Bir ağ arabirimi oluştur

Önceki adımda, bir ağ sanal gereç sonraki atlama türü belirtilmiş bir yol oluşturuldu. Bir ağ uygulaması çalıştıran bir sanal makine, genellikle bir ağ sanal gereç da adlandırılır. Üretim ortamlarında, dağıttığınız ağ sanal gereç önceden yapılandırılmış bir sanal makine görülür. Birçok ağ sanal Gereçleri edinilebilir [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?search=network%20virtual%20appliance&page=1). Bu makalede, temel bir sanal makine oluşturulur.

Bir sanal makine ağ ile iletişim kurmak sanal makine sağlayan ekli bir veya daha fazla ağ arabirimine sahiptir. Kendi IP adresi için hedeflendiği değil, kendisine gönderilen ağ trafiği iletmek bir ağ arabirimi için IP iletme ağ arabirimi için etkinleştirilmesi gerekir. Bir ağ arabirimi oluşturmadan önce sanal almak zorunda kimliğine sahip ağ [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork), alt kimliği ile [Get-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig). Bir ağ arabirimi oluştur [yeni AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) içinde *DMZ* IP iletmenin etkin olan bir alt ağ:

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

#### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Bir sanal makine oluşturun ve mevcut bir ağ arabirimini eklemektir için önce bir sanal makine yapılandırmasıyla oluşturmanız gerekir [yeni AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig). Yapılandırma, önceki adımda oluşturulan ağ arabirimi içerir. Bir kullanıcı adı ve parola istendiğinde, kullanıcı adı ve parola ile sanal makine içine günlüğe kaydetmek istediğiniz seçin. 

```azurepowershell-interactive
# Create a credential object.
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a virtual machine configuration.
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

Sanal makine yapılandırmasıyla kullanarak sanal makine oluşturun [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Aşağıdaki örnek, bir sanal makine adlı oluşturur *myVmNva*. 

```azurepowershell-interactive
$vmNva = New-AzureRmVM `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -VM $vmConfig `
  -AsJob
```

`-AsJob` Seçeneği bir sonraki adıma devam etmek için bu sanal makine arka planda oluşturur. İstendiğinde, kullanıcı adı ve sanal makine ile oturum açmak için istediğiniz parolayı girin. Üretim ortamlarında, dağıttığınız ağ sanal gereç önceden yapılandırılmış bir sanal makine görülür. Birçok ağ sanal Gereçleri edinilebilir [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?search=network%20virtual%20appliance&page=1).

Azure 10.0.2.4 ilk kullanılabilir IP adresi olduğundan bu 10.0.2.4 sanal makinenin özel IP adresi atanan *DMZ* alt *myVirtualNetwork*.

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Bu trafiğinden doğrulamak için iki sanal makine sanal ağ oluşturma *ortak* alt ağ için yönlendirilir *özel* sonraki adımda ağ sanal gereç aracılığıyla alt ağ. 

Bir sanal makine oluşturmak *ortak* alt ağ ile [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Aşağıdaki örnek, bir sanal makine adlı oluşturur *myVmWeb* içinde *ortak* alt *myVirtualNetwork* sanal ağ. 

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Public" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmWeb" `
  -AsJob
```

Azure 10.0.1.4 ilk kullanılabilir IP adresi olduğundan bu 10.0.0.4 sanal makinenin özel IP adresi atanan *ortak* alt *myVirtualNetwork*.

Bir sanal makine oluşturmak *özel* alt ağ.

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Private" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmMgmt"
```

Sanal makine oluşturmak için birkaç dakika sürer. Azure 10.0.1.4 ilk kullanılabilir IP adresi olduğundan bu 10.0.1.4 sanal makinenin özel IP adresi atanan *özel* alt *myVirtualNetwork*. 

Sonraki adıma sanal makineniz oluşturulur ve Azure PowerShell çıkış döndürür kadar devam yok.

### <a name="route-traffic-through-a-network-virtual-appliance"></a>Bir ağ sanal gereç yoluyla trafiği yönlendirme

Kullanım [kümesi AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) ICMP etkinleştirmek için gelen *myVmWeb* ve *myVmMgmt* test etmek için tracert kullanmak için Windows Güvenlik Duvarı aracılığıyla sanal makineler sonraki adımda sanal makineler arasındaki iletişim:

```powershell-interactive
Set-AzureRmVMExtension `
  -ResourceGroupName myResourceGroup `
  -VMName myVmWeb `
  -ExtensionName AllowICMP `
  -Publisher Microsoft.Compute `
  -ExtensionType CustomScriptExtension `
  -TypeHandlerVersion 1.8 `
  -SettingString '{"commandToExecute": "netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow"}' `
  -Location EastUS

Set-AzureRmVMExtension `
  -ResourceGroupName myResourceGroup `
  -VMName myVmMgmt `
  -ExtensionName AllowICMP `
  -Publisher Microsoft.Compute `
  -ExtensionType CustomScriptExtension `
  -TypeHandlerVersion 1.8 `
  -SettingString '{"commandToExecute": "netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow"}' `
  -Location EastUS
```

Yukarıdaki komutları tamamlanması birkaç dakika sürebilir. Sonraki adıma komutları tamamlamak ve çıktı için PowerShell döndürülür kadar devam etmeyin. Bu makalede yönlendirme test etmek için tracert kullanılsa da ICMP üretim dağıtımları için Windows Güvenlik Duvarı aracılığıyla izin vererek önerilmez.

Internet'ten bir sanal makinenin ortak IP adresine bağlanın. Kullanım [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) ortak IP adresi, bir sanal makinenin dönün. Aşağıdaki örnek genel IP adresi döndürür *myVmMgmt* sanal makine:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmMgmt `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

İle Uzak Masaüstü oturumu oluşturmak için aşağıdaki komutu kullanın *myVmMgmt* yerel bilgisayarınızdan sanal makine. Değiştir `<publicIpAddress>` IP adresi ile döndürülen önceki komutu.

```
mstsc /v:<publicIpAddress>
```

Bir Uzak Masaüstü Protokolü (.rdp) dosyası oluşturulur, bilgisayarınıza indirilmeden ve açılır. Kullanıcı adı ve sanal makine oluştururken belirttiğiniz parolayı girin (seçmek için gerek duyabileceğiniz **daha fazla seçenek**, ardından **farklı bir hesap kullan**, ne zaman girdiğiniz kimlik bilgileri belirtmek için sanal makine oluşturulan) ve ardından **Tamam**. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet** veya **Devam**’a tıklayın. 

Sanal makinenin ağ arabirimi için IP iletimini Azure içindeki etkin [etkinleştirmek IP fowarding](#enable-ip-forwarding). Sanal makinede işletim sistemi ya da sanal makinede çalışan bir uygulama aynı zamanda ağ trafiğini iletebilir olması gerekir. Bir üretim ortamında Ağ sanal gereç dağıttığınızda, Gereci genellikle filtreleri, günlükleri veya trafiği iletmeden önce başka bir işlevi gerçekleştirir. Bu makalede ancak, işletim sisteminin yalnızca aldığı tüm trafiği iletir. İşletim sistemi içinde IP iletimini etkinleştirmeniz *myVmNva*:

Bir komut isteminden *myVmMgmt* sanal makine, Uzak Masaüstü ' *myVmNva* sanal makine:

``` 
mstsc /v:myVmNva
```
    
İşletim sistemi içinde IP iletimini etkinleştirmek için *myVmNva* sanal makine PowerShell'de aşağıdaki komutu girin *myVmNva* sanal makine:

```powershell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
```
    
Yeniden *myVmNva* de açtığınız için Uzak Masaüstü oturumu içinde bırakarak Uzak Masaüstü oturumu bağlantısını keser sanal makine *myVmMgmt* sanal makine.

Sonra *myVmNva* sanal makine yeniden başlatıldığında, ağ trafiği için yönlendirme test etmek için aşağıdaki komutu kullanın *myVmWeb* sanal makineden *myVmMgmt* sanal Makine.

```
tracert myvmweb
```

Yanıt aşağıdaki örneğe benzer:

```
Tracing route to myvmweb.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
over a maximum of 30 hops:
    
1     1 ms     1 ms     1 ms  10.0.0.4
  
Trace complete.
```

Trafik, doğrudan yönlendirilir görebilirsiniz *myVmMgmt* sanal makineye *myVmWeb* sanal makine. Azure'nın varsayılan yollar, doğrudan alt ağlar arasında trafiği yönlendirme.

Uzak Masaüstü için aşağıdaki komutu kullanın *myVmWeb* sanal makineden *myVmMgmt* sanal makine:

``` 
mstsc /v:myVmWeb
```

Ağ trafiği için yönlendirme test etmek için *myVmMgmt* sanal makineden *myVmWeb* sanal makine, bir komut isteminden aşağıdaki komutu girin:

```
tracert myvmmgmt
```

Yanıt aşağıdaki örneğe benzer:

```
Tracing route to myvmmgmt.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
over a maximum of 30 hops:
        
1    <1 ms     *        1 ms  10.0.2.4
2     1 ms     1 ms     1 ms  10.0.1.4
        
Trace complete.
```

İlk atlama ağ sanal gereç ait özel IP adresi 10.0.2.4 olduğunu görebilirsiniz. İkinci atlama 10.0.1.4, özel IP adresi olan *myVmMgmt* sanal makine. Eklenen rota *myRouteTablePublic* rota tablosu ve ilişkili *ortak* alt neden NVA aracılığıyla yerine doğrudan trafiği yönlendirmek Azure *özel* alt ağ.

Her ikisine de uzak masaüstü oturumları kapatmak *myVmWeb* ve *myVmMgmt* sanal makineler.

## <a name="troubleshoot-routing"></a>Yönlendirme sorunlarını giderme

Önceki adımlarda öğrenilen Azure, isteğe bağlı olarak, kendi yollar geçersiz kılabilir varsayılan yolların geçerlidir. Bazı durumlarda, olmasını beklediğiniz gibi trafik yönlendirilemeyebilir. Kullanım [yeni AzureRmNetworkWatcher](/powershell/module/azurerm.network/new-azurermnetworkwatcher) bu bölgede bir Ağ İzleyicisi yoksa bir Ağ İzleyicisi EastUS bölgede etkinleştirmek için:

```azurepowershell-interactive
# Enable network watcher for east region, if you don't already have a network watcher enabled for the region:
$nw = New-AzureRmNetworkWatcher `
 -Location eastus `
 -Name myNetworkWatcher_eastus `
 -ResourceGroupName myResourceGroup
```

Kullanım [Get-AzureRmNetworkWatcherNextHop](/powershell/module/azurerm.network/get-azurermnetworkwatchernexthop) iki sanal makineler arasındaki trafik nasıl yönlendirildiğini belirlemek için. Örneğin, aşağıdaki komutu gelen trafik yönlendirme testleri *myVmWeb* (10.0.0.4) sanal makineye *myVmMgmt* (10.0.1.4) sanal makine:

```azurepowershell-interactive
$vmWeb = Get-AzureRmVM `
  -Name myVmWeb `
  -ResourceGroupName myResourceGroup

Get-AzureRmNetworkWatcherNextHop `
  -DestinationIPAddress 10.0.1.4 `
  -NetworkWatcherName myNetworkWatcher_eastus `
  -ResourceGroupName myResourceGroup `
  -SourceIPAddress 10.0.0.4 `
  -TargetVirtualMachineId $vmWeb.Id
```
Aşağıdaki çıkış sonra kısa bekleme verilir:

```azurepowershell
NextHopIpAddress    NextHopType       RouteTableId
------------------  ---------------- ---------------------------------------------------------------------------------------------------------------------------
10.0.2.4            VirtualAppliance  /subscriptions/<Subscription-Id>/resourceGroups/myResourceGroup/providers/Microsoft.Network/routeTables/myRouteTablePublic
```

Çıktı sonraki atlama IP adresi gelen trafiği için size bildirir *myVmWeb* için *myVmMgmt* 10.0.2.4 olduğu ( *myVmNva* sanal makine), sonraki türü atlama olduğu *Değerinin VirtualAppliance*, ve yönlendirme neden rota tablosu olduğunu *myRouteTablePublic*.

Her ağ arabirimi için etkili rotaları, Azure'nın varsayılan yollar ve tanımladığınız yollar birleşimidir. Bir sanal makineyle bir ağ arabirimi için geçerli olan tüm yolları görmek için [Get-AzureRmEffectiveRouteTable](/powershell/module/azurerm.network/get-azurermeffectiveroutetable). Örneğin, etkin yollar için göstermek için *myVmWeb* ağ arabiriminde *myVmWeb* sanal makine, aşağıdaki komutu girin:

```azurepowershell-interactive
Get-AzureRmEffectiveRouteTable `
  -NetworkInterfaceName myVmWeb `
  -ResourceGroupName myResourceGroup `
  | Format-Table
```

Tüm varsayılan yollar ve bir önceki adımda eklediğiniz rota döndürülür.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerektiğinde kullanmak [Remove-AzureRmResourcegroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) kaynak grubu ve içerdiği kaynakların tümünü kaldırmak için.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir yol tablosu oluşturulur ve bir alt ağa ilişkilendirilmiş. Ortak bir alt ağ trafiği için özel bir alt ağa yönlendirilmiş bir ağ sanal gereç oluşturuldu. Bir sanal ağ içinde birçok Azure kaynakları dağıtabilirsiniz, ancak bazı Azure PaaS hizmetler için kaynaklar sanal bir ağa dağıtılamıyor. Hala erişimi bazı Azure PaaS Hizmetleri'nden trafik için yalnızca bir sanal ağ alt kaynaklara yine de kısıtlayabilirsiniz. Ağ erişimi Azure PaaS kaynaklarına erişimi kısıtlayabilir öğrenmek için sonraki makalede ilerleyin.

> [!div class="nextstepaction"]
> [Ağ erişimi PaaS kaynaklarına erişimi kısıtlayabilir](virtual-network-service-endpoints-configure.md#azure-powershell)