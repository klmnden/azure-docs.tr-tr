---
title: Bir sanal makine ağ yönlendirme sorunu - Azure PowerShell tanılama | Microsoft Docs
description: Bu makalede, Azure Ağ İzleyicisi sonraki atlama özelliğini kullanarak bir sanal makine ağ yönlendirme bir problemi tanılamaya öğrenin.
services: network-watcher
documentationcenter: network-watcher
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I need to diagnose virtual machine (VM) network routing problem that prevents communication to different destinations.
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: network-watcher
ms.workload: infrastructure
ms.date: 04/20/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: f793a201b3fbf57ac2f420c4f4e57a230bc11468
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38299033"
---
# <a name="diagnose-a-virtual-machine-network-routing-problem---azure-powershell"></a>Bir sanal makine ağ yönlendirme sorunu - Azure PowerShell tanılama

Bu makalede, bir sanal makine (VM) dağıtın ve ardından bir IP adresi ve URL iletişimler olup olmadığını denetleyin. Bir iletişim hatasının nedenini ve bu hatayı nasıl çözeceğinizi belirlersiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu makale AzureRM PowerShell modülünün 5.4.1 gerekir veya üzeri. Yüklü sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-vm"></a>VM oluşturma

Sanal makine oluşturabilmeniz için sanal makineyi içerecek bir kaynak grubu oluşturmanız gerekir. [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup) komutunu kullanarak bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

[New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) ile sanal makineyi oluşturun. Bu adımı çalıştırırken kimlik bilgileri istenir. Girdiğiniz değerler, sanal makinenin kullanıcı adı ve parolası olarak yapılandırılır.

```azurepowershell-interactive
$vM = New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVm" `
    -Location "East US"
```

Sanal makinenin oluşturulması birkaç dakika sürer. Sanal makine oluşturulup PowerShell çıktı döndürünceye kadar kalan adımlara devam etmeyin.

## <a name="test-network-communication"></a>Ağ iletişimini test etme

Ağ İzleyicisi ile ağ iletişimi test etmek için önce test etmek istediğiniz VM'nin bulunduğu bölgede bir Ağ İzleyicisi'ni etkinleştirin ve ardından iletişimi test etme için Ağ İzleyicisi'nin sonraki atlama özelliği kullanın.

## <a name="enable-network-watcher"></a>Ağ izleyicisini etkinleştirme

Doğu ABD bölgesinde zaten etkinleştirilmiş bir ağ izleyiciniz varsa, bu ağ izleyicisini almak için [Get-AzureRmNetworkWatcher](/powershell/module/azurerm.network/get-azurermnetworkwatcher) komutunu kullanın. Aşağıdaki örnekte, *NetworkWatcherRG* kaynak grubunda bulunan *NetworkWatcher_eastus* adlı mevcut ağ izleyicisi alınır:

```azurepowershell-interactive
$networkWatcher = Get-AzureRmNetworkWatcher `
  -Name NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG
```

Doğu ABD bölgesinde henüz etkinleştirilmiş bir ağ izleyiciniz yoksa, Doğu ABD bölgesinde ağ izleyicisi oluşturmak için [New-AzureRmNetworkWatcher](/powershell/module/azurerm.network/new-azurermnetworkwatcher) komutunu kullanın:

```azurepowershell-interactive
$networkWatcher = New-AzureRmNetworkWatcher `
  -Name "NetworkWatcher_eastus" `
  -ResourceGroupName "NetworkWatcherRG" `
  -Location "East US"
```

### <a name="use-next-hop"></a>Sonraki atlamayı kullanma

Azure, varsayılan hedeflerin yollarını otomatik olarak oluşturur. Varsayılan yolları geçersiz kılmak için özel yollar oluşturabilirsiniz. Bazı durumlarda, özel yollar iletişimin başarısız olmasına neden olabilir. Bir sanal makineden yönlendirmeyi test etmek için [Get-AzureRmNetworkWatcherNextHop](/powershell/module/azurerm.network/get-azurermnetworkwatchernexthop) trafiği belirli bir adresi için hedeflenen yönlendirme sonraki atlama belirlerken komutu.

Sanal makineden, www.bing.com adresinin IP adreslerinden birine giden iletişimi test etme:

```azurepowershell-interactive
Get-AzureRmNetworkWatcherNextHop `
  -NetworkWatcher $networkWatcher `
  -TargetVirtualMachineId $VM.Id `
  -SourceIPAddress 192.168.1.4 `
  -DestinationIPAddress 13.107.21.200
```

Birkaç saniye sonra çıktı size bildirir, **NextHopType** olduğu **Internet**ve **RouteTableId** olduğu **sistem yolu**. Bu sonucu, geçerli bir hedef yolu olduğunu bilmenizi sağlar.

Sanal makineden 172.31.0.100 adresine giden iletişimi test etme:

```azurepowershell-interactive
Get-AzureRmNetworkWatcherNextHop `
  -NetworkWatcher $networkWatcher `
  -TargetVirtualMachineId $VM.Id `
  -SourceIPAddress 192.168.1.4 `
  -DestinationIPAddress 172.31.0.100
```

Döndürülen çıktının size bildirir, **hiçbiri** olduğu **NextHopType**ve **RouteTableId** de **sistem yolu**. Bu sonuç, hedefin geçerli bir sistem yolu olmasına rağmen trafiği hedefe yönlendiren bir sonraki atlama olmadığını size bildirir.

## <a name="view-details-of-a-route"></a>Bir yolun ayrıntılarını görüntüleme

Daha fazla yönlendirme analiz etmek için ağ arabirimi için geçerli rotalar gözden [Get-AzureRmEffectiveRouteTable](/powershell/module/azurerm.network/get-azurermeffectiveroutetable) komutu:

```azurepowershell-interactive
Get-AzureRmEffectiveRouteTable `
  -NetworkInterfaceName myVm `
  -ResourceGroupName myResourceGroup |
  Format-table
```

Aşağıdaki metni içeren bir çıktı döndürülür:

```powershell
Name State  Source  AddressPrefix           NextHopType NextHopIpAddress
---- -----  ------  -------------           ----------- ----------------
     Active Default {192.168.0.0/16}        VnetLocal   {}              
     Active Default {0.0.0.0/0}             Internet    {}              
     Active Default {10.0.0.0/8}            None        {}              
     Active Default {100.64.0.0/10}         None        {}              
     Active Default {172.16.0.0/12}         None        {}              
```

Rota ile önceki çıktıda görüldüğü **AaddressPrefix** , **0.0.0.0/0** başka bir yolun adres ön ekleri 'ınbirsonrakiatlamaadreslerihedefleyendeğiltümtrafiğiyönlendiren**Internet**. Çıktıda de görebileceğiniz gibi ancak yoktur 172.31.0.100 içerir 172.16.0.0/12 önekini varsayılan bir yolu adresi **nextHopType** olduğu **hiçbiri**. Azure, 172.16.0.0/12 için varsayılan bir yol oluşturur ancak bir neden olmadıkça sonraki atlama türünü belirtmez. Örneğin, 172.16.0.0/12 adres aralığını sanal ağın adres alanına eklediyseniz, Azure değişiklikleri **nextHopType** için **sanal ağ** rota. Bir onay ardından gösterebilir **sanal ağ** olarak **nextHopType**.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak kaynak grubunu ve içerdiği tüm kaynakları kaldırabilirsiniz:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir VM oluşturulur ve sanal makineden ağ yönlendirme tanı koydu. Azure’un birkaç varsayılan yol oluşturduğunu öğrendiniz ve iki farklı hedefin yolunu test ettiniz. [Azure'da yönlendirme](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) ve [özel yollar oluşturma](../virtual-network/manage-route-table.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#create-a-route) hakkında daha fazla bilgi edinin.

Giden sanal makine bağlantılarında, gecikme süresini de belirleyebilirsiniz ve izin verilen ve VM Ağ İzleyicisi'nin kullanarak bir uç nokta arasındaki ağ trafiğini reddedildi [bağlantı sorunlarını giderme](network-watcher-connectivity-powershell.md) yeteneği. Ağ İzleyicisi Bağlantı İzleyicisi özelliğini kullanarak zaman içinde bir VM ile URL veya bir IP adresi gibi bir uç noktası arasındaki iletişimi izleyebilirsiniz. Bilgi edinmek için bkz [Ağ Bağlantı İzleyicisi](connection-monitor.md).