---
title: "Azure Ağ İzleyicisi sonraki atlama - PowerShell ile sonraki atlama Bul | Microsoft Docs"
description: "Bu makalede, sonraki atlama türü nedir nasıl bulabilir ve PowerShell kullanarak sonraki atlama kullanarak IP adresi anlatmaktadır."
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 6a656c55-17bd-40f1-905d-90659087639c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: ef559fbbd3e8448d64167552cacee04790418343
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="find-out-what-the-next-hop-type-is-using-the-next-hop-capability-in-azure-network-watcher-using-powershell"></a>Hangi bir sonraki atlama türü sonraki atlama yetenek PowerShell kullanarak Azure Ağ İzleyicisi içinde kullandığını bulmak

> [!div class="op_single_selector"]
> - [Azure portal](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [CLI 1.0](network-watcher-check-next-hop-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-next-hop-cli.md)
> - [Azure REST API'si](network-watcher-check-next-hop-rest.md)

Sonraki atlama özelliğini get sonraki atlama türü ve belirtilen bir sanal makineye dayalı IP adresi sağlayan Ağ İzleyicisi özelliğidir. Bu özellik, bir sanal makine trafiğe bir ağ geçidi, internet veya sanal ağlar hedefine almak için geçiyorsa belirlemede yararlıdır.

## <a name="before-you-begin"></a>Başlamadan önce

Bu senaryoda sonraki atlama türü ve IP adresini bulmak için Azure portalını kullanacaksınız.

Bu senaryo zaten izlediğiniz adımlarda varsayar [bir Ağ İzleyicisi oluşturma](network-watcher-create.md) bir Ağ İzleyicisi oluşturmak için. Senaryo da geçerli bir sanal makine ile bir kaynak grubu kullanılacak var olduğunu varsayar.

## <a name="scenario"></a>Senaryo

Bu makalede ele alınan senaryosu, sonraki atlama, sonraki atlama türü ve IP adresi için bir kaynak bulur Ağ İzleyicisi özelliğini kullanır. Sonraki atlama hakkında daha fazla bilgi için [sonraki atlama genel bakış](network-watcher-next-hop-overview.md).

## <a name="retrieve-network-watcher"></a>Ağ İzleyicisi alma

Ağ İzleyicisi örneği almak için ilk adımdır bakın. `$networkWatcher` Değişkeni sonraki atlama geçirilen cmdlet doğrulayın.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-virtual-machine"></a>Bir sanal makine Al

Sonraki atlama, bir sanal makineden sonraki atlama ve sonraki atlama IP adresini döndürür. Bir sanal makine kimliği cmdlet'i için gereklidir. Kullanmak için sanal makine Kimliğini biliyorsanız, bu adımı atlayabilirsiniz.

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

> [!NOTE]
> Sonraki atlama VM kaynak çalıştırmak için ayrılır gerektirir.

## <a name="get-the-network-interfaces"></a>Ağ arabirimlerini Al

Bu örnekte, bir sanal makinede NIC alıyoruz sanal makinede bir NIC IP adresi gereklidir. Sanal makinede test etmek istediğiniz IP adresi zaten biliyorsanız, bu adımı atlayabilirsiniz.

```powershell
$Nics = Get-AzureRmNetworkInterface | Where {$_.Id -eq $vm.NetworkProfile.NetworkInterfaces.Id.ForEach({$_})}
```

## <a name="get-next-hop"></a>Sonraki atlama Al

Diyoruz artık `Get-AzureRmNetworkWatcherNextHop` cmdlet'i. Cmdlet Ağ İzleyicisi, sanal makine kimliği, kaynak IP adresi ve hedef IP adresi geçirin. Bu örnekte, başka bir sanal ağ içinde bir VM hedef IP adresi değil. İki sanal ağ arasında bir sanal ağ geçidi yok.

```powershell
Get-AzureRmNetworkWatcherNextHop -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id -SourceIPAddress $nics[0].IpConfigurations[0].PrivateIpAddress  -DestinationIPAddress 10.0.2.4 
```

## <a name="review-results"></a>Sonuçları gözden geçirin

Tamamlandığında, sonuçları sağlanır. Sonraki atlama IP adresi bu kaynak türü yanı sıra döndürülür. Bu senaryoda, sanal ağ geçidinin genel IP adresi değil.

```
NextHopIpAddress NextHopType           RouteTableId 
---------------- -----------           ------------ 
13.78.238.92     VirtualNetworkGateway Gateway Route
```

Aşağıdaki liste, şu anda kullanılabilir NextHopType değerleri gösterir:

**Sonraki atlama türü**

* Internet
* Değerinin VirtualAppliance
* VirtualNetworkGateway
* VnetLocal
* HyperNetGateway
* VnetPeering
* None

## <a name="next-steps"></a>Sonraki adımlar

Ağ güvenlik grubu ayarlarınızı program aracılığıyla ziyaret ederek gözden öğrenin [NSG Denetim Ağ İzleyicisi](network-watcher-nsg-auditing-powershell.md)

















