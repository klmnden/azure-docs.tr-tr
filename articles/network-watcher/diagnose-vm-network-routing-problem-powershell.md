---
title: Bir sanal makine ağ yönlendirme sorunu - Azure PowerShell tanılamak | Microsoft Docs
description: Bu makalede, Azure Ağ İzleyicisi sonraki atlama kapasitesini kullanan bir sanal makine ağ yönlendirme bir sorunu tanılamak öğrenin.
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
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="diagnose-a-virtual-machine-network-routing-problem---azure-powershell"></a>Bir sanal makine ağ yönlendirme sorunu - Azure PowerShell tanılama

Bu makalede, bir sanal makine (VM) dağıtma ve bir IP adresi ve URL iletişimler denetleyin. Bir iletişim hatası ve nasıl giderebileceğiniz nedenini belirleyin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-powershell.md)]

Yükleme ve yerel olarak PowerShell kullanma seçerseniz, bu makalede AzureRM PowerShell modülü sürümü 5.4.1 gerektirir veya sonraki bir sürümü. Yüklü sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-vm"></a>VM oluşturma

Bir VM oluşturmadan önce VM içerecek şekilde bir kaynak grubu oluşturmanız gerekir. [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup) komutunu kullanarak bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

VM oluşturma [AzureRmVM yeni](/powershell/module/azurerm.compute/new-azurermvm). Bu adımı çalıştırırken kimlik bilgileri istenir. Girdiğiniz değerler, sanal makinenin kullanıcı adı ve parolası olarak yapılandırılır.

```azurepowershell-interactive
$vM = New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVm" `
    -Location "East US"
```

Sanal makinenin oluşturulması birkaç dakika sürer. Kalan adımlar ile VM oluşturulur ve PowerShell çıkışı döndürür kadar devam yok.

## <a name="test-network-communication"></a>Test ağ iletişimi

Ağ iletişimi Ağ İzleyicisi ile test etmek için önce test etmek istediğiniz VM'nin bulunduğu bölgede bulunan bir Ağ İzleyicisi etkinleştirin ve iletişim test etmek için Ağ İzleyicisi'nin sonraki atlama yetenek kullanmanız gerekir.

## <a name="enable-network-watcher"></a>Ağ İzleyicisi'ni etkinleştir

Doğu ABD bölgesinde etkin bir Ağ İzleyicisi zaten varsa, [Get-AzureRmNetworkWatcher](/powershell/module/azurerm.network/get-azurermnetworkwatcher) Ağ İzleyicisi'ni alınamadı. Aşağıdaki örnek adlı varolan bir Ağ İzleyicisi alır *NetworkWatcher_eastus* alanında *NetworkWatcherRG* kaynak grubu:

```azurepowershell-interactive
$networkWatcher = Get-AzureRmNetworkWatcher `
  -Name NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG
```

Doğu ABD bölgesinde etkin bir Ağ İzleyicisi yoksa kullanmak [yeni AzureRmNetworkWatcher](/powershell/module/azurerm.network/new-azurermnetworkwatcher) Doğu ABD bölgesinde bir Ağ İzleyicisi oluşturmak için:

```azurepowershell-interactive
$networkWatcher = New-AzureRmNetworkWatcher `
  -Name "NetworkWatcher_eastus" `
  -ResourceGroupName "NetworkWatcherRG" `
  -Location "East US"
```

### <a name="use-next-hop"></a>Sonraki atlama kullanın

Azure varsayılan hedeflere yolları otomatik olarak oluşturur. Geçersiz kılma varsayılan yolların özel yollar oluşturabilir. Bazı durumlarda, özel yollar iletişimi başarısız olmasına neden olabilir. Bir sanal makineden yönlendirme sınamak için kullanın [Get-AzureRmNetworkWatcherNextHop](/powershell/module/azurerm.network/get-azurermnetworkwatchernexthop) trafiği için belirli bir adresi hedefleyen sonraki yönlendirme atlama belirlerken komutu.

Test giden iletişim VM'den www.bing.com IP adreslerinden biri:

```azurepowershell-interactive
Get-AzureRmNetworkWatcherNextHop `
  -NetworkWatcher $networkWatcher `
  -TargetVirtualMachineId $VM.Id `
  -SourceIPAddress 192.168.1.4 `
  -DestinationIPAddress 13.107.21.200
```

Birkaç saniye sonra çıktısı size bildirir, **NextHopType** olan **Internet**ve **RouteTableId** olan **sistem yolu**. Bu sonuç, geçerli bir hedef yolu olduğunu bilmenizi sağlar.

Sanal makineden giden iletişim 172.31.0.100 test edin:

```azurepowershell-interactive
Get-AzureRmNetworkWatcherNextHop `
  -NetworkWatcher $networkWatcher `
  -TargetVirtualMachineId $VM.Id `
  -SourceIPAddress 192.168.1.4 `
  -DestinationIPAddress 172.31.0.100
```

Döndürülen çıktının size bildirir, **hiçbiri** olan **NextHopType**ve **RouteTableId** de **sistem yolu**. Bu sonucu, hedef bir geçerli sistem yolu olsa da, olduğundan, hedef trafiği yönlendirmek için bir sonraki atlama bilmenizi sağlar.

## <a name="view-details-of-a-route"></a>Bir rota ayrıntılarını görüntüleme

Daha fazla yönlendirme çözümlemek için sahip ağ arabirimi için etkili rotaları gözden [Get-AzureRmEffectiveRouteTable](/powershell/module/azurerm.network/get-azurermeffectiveroutetable) komutu:

```azurepowershell-interactive
Get-AzureRmEffectiveRouteTable `
  -NetworkInterfaceName myVm `
  -ResourceGroupName myResourceGroup |
  Format-table
```

Aşağıdaki metin bulunur çıktı döndürülür:

```powershell
Name State  Source  AddressPrefix           NextHopType NextHopIpAddress
---- -----  ------  -------------           ----------- ----------------
     Active Default {192.168.0.0/16}        VnetLocal   {}              
     Active Default {0.0.0.0/0}             Internet    {}              
     Active Default {10.0.0.0/8}            None        {}              
     Active Default {100.64.0.0/10}         None        {}              
     Active Default {172.16.0.0/12}         None        {}              
```

Rota ile önceki çıktıda görüldüğü gibi **AaddressPrefix** , **0.0.0.0/0** ,birsonrakiatlamailediğerrotanınadresönekleriniiçindeadreslerihedeflemeyentümtrafiğinyönlendirir**Internet**. Ayrıca çıktıda görüldüğü gibi ancak yoktur 172.31.0.100 içeren 172.16.0.0/12 önek varsayılan rotaya adresi **nextHopType** olan **hiçbiri**. Azure 172.16.0.0/12 için varsayılan bir yol oluşturur, ancak bir nedeni kadar bir sonraki atlama türü belirtmiyor. Örneğin, 172.16.0.0/12 adres aralığını sanal ağın adres alanı eklediyseniz, Azure değiştirir **nextHopType** için **sanal ağ** rota için. Bir onay sonra göstermeniz **sanal ağ** olarak **nextHopType**.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak kaynak grubunu ve içerdiği tüm kaynakları kaldırabilirsiniz:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir VM oluşturulur ve sanal makineden ağ yönlendirme tanı koydu. Azure birkaç varsayılan yol oluşturur ve iki farklı hedefe yönlendirme test öğrendiniz. Daha fazla bilgi edinmek [Azure'da yönlendirme](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) ve nasıl [özel yollar oluşturmayı](../virtual-network/manage-route-table.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#create-a-route).

Giden VM bağlantılarında, gecikme süresini de belirleyebilirsiniz ve izin verilen ve VM Ağ İzleyicisi'nin kullanarak bir uç nokta arasındaki ağ trafiğini reddedildi [bağlantı sorunlarını giderme](network-watcher-connectivity-powershell.md) yeteneği. Ağ İzleyicisi Bağlantı İzleyicisi özelliği kullanarak zamanla VM URL veya bir IP adresi gibi bir uç nokta arasındaki iletişimi izleyebilirsiniz. Bilgi edinmek için bkz [bir ağ bağlantısı izlemek](connection-monitor.md).