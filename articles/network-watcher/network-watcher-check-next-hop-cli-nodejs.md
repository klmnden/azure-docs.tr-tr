---
title: "Azure Ağ İzleyicisi sonraki atlama - Azure CLI 1.0 ile sonraki atlama Bul | Microsoft Docs"
description: "Bu makalede, sonraki atlama türü nedir nasıl bulabilir ve Azure CLI kullanarak sonraki atlama kullanarak IP adresi anlatmaktadır."
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 0700c274-3e0d-4dca-acfa-3ceac8990613
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: e849f7952962d177f40ce99307ef1c305e089827
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="find-out-what-the-next-hop-type-is-using-the-next-hop-capability-in-azure-network-watcher-using-azure-cli-10"></a>Hangi bir sonraki atlama türü sonraki atlama yetenek Azure CLI 1.0 kullanarak Azure Ağ İzleyicisi içinde kullandığını bulmak

> [!div class="op_single_selector"]
> - [Azure portal](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [CLI 1.0](network-watcher-check-next-hop-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-next-hop-cli.md)
> - [Azure REST API'si](network-watcher-check-next-hop-rest.md)

Sonraki atlama özelliğini get sonraki atlama türü ve belirtilen bir sanal makineye dayalı IP adresi sağlayan Ağ İzleyicisi özelliğidir. Bu özellik, bir sanal makine trafiğe bir ağ geçidi, internet veya sanal ağlar hedefine almak için geçiyorsa belirlemede yararlıdır.

Bu makalede, platformlar arası Azure CLI 1.0, Windows, Mac ve Linux için kullanılabilir olduğu kullanır.

## <a name="before-you-begin"></a>Başlamadan önce

Bu senaryoda sonraki atlama türü ve IP adresini bulmak için Azure CLI kullanır.

Bu senaryo zaten izlediğiniz adımlarda varsayar [bir Ağ İzleyicisi oluşturma](network-watcher-create.md) bir Ağ İzleyicisi oluşturmak için. Senaryo da geçerli bir sanal makine ile bir kaynak grubu kullanılacak var olduğunu varsayar.

## <a name="scenario"></a>Senaryo

Bu makalede ele alınan senaryosu, sonraki atlama, sonraki atlama türü ve IP adresi için bir kaynak bulur Ağ İzleyicisi özelliğini kullanır. Sonraki atlama hakkında daha fazla bilgi için [sonraki atlama genel bakış](network-watcher-next-hop-overview.md).


## <a name="get-next-hop"></a>Sonraki atlama Al

Diyoruz sonraki atlama almak için `azure netowrk watcher next-hop` cmdlet'i. Cmdlet Ağ İzleyicisi kaynak grubu, NetworkWatcher, sanal makine kimliği, kaynak IP adresi ve hedef IP adresi geçirin. Bu örnekte, başka bir sanal ağ içinde bir VM hedef IP adresi değil. İki sanal ağ arasında bir sanal ağ geçidi yok. 

```azurecli
azure network watcher next-hop -g resourceGroupName -n networkWatcherName -t targetResourceId -a <source-ip> -d <destination-ip>
```

> [!NOTE]
Birden çok NIC VM varsa ve IP iletimini herhangi NIC'ler, daha sonra NIC parametre etkinse (-ı NIC-ID) belirtilmelidir. Aksi takdirde isteğe bağlıdır.

## <a name="review-results"></a>Sonuçları gözden geçirin

Tamamlandığında, sonuçları sağlanır. Sonraki atlama IP adresi bu kaynak türü yanı sıra döndürülür.

```
data:    Next Hop Ip Address             : 10.0.1.2
info:    network watcher next-hop command OK
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
