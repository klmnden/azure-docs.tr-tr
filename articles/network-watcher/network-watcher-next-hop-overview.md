---
title: Azure Ağ İzleyicisi sonraki atlama giriş | Microsoft Docs
description: Bu makalede, sonraki atlama özelliği Ağ İzleyicisi'ne genel bakış sağlar.
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.assetid: febf7bca-e0b7-41d5-838f-a5a40ebc5aac
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: kumud
ms.openlocfilehash: eb613455ccd6b6630d0d75b5a8458f812bb36846
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64730216"
---
# <a name="use-next-hop-to-diagnose-virtual-machine-routing-problems"></a>Sanal makine yönlendirme sorunları tanılamak için sonraki atlama kullanın

Trafiği bir sanal makineden (VM) bir ağ arabirimi (NIC) ile ilişkili geçerli rotalar dayalı bir hedefe gönderilir. Sonraki atlama IP adresi için bir paket ve sonraki atlama türü belirli bir sanal makine ve NIC alır Sonraki atlama bilerek istenen hedefe yönlendirilen trafik veya trafiği hiçbir yerde gönderilen olup olmadığını belirlemenize yardımcı olur. Burada bir şirket içi konum veya bir sanal gereç trafik yönlendirilir, yolların hatalı bir yapılandırma bağlantı sorunlarına yol açabilir. Sonraki atlama, ayrıca sonraki atlama ile ilişkili yol tablosuna döndürür. Bu yol, yol, kullanıcı tanımlı bir yol tanımlanırsa, döndürülür. Aksi halde, sonraki atlama döndürür **sistem yolu**.

![sonraki atlama genel bakış](./media/network-watcher-next-hop-overview/figure1.png)

Sonraki atlama özelliği tarafından döndürülen sonraki atlamalarla aşağıdaki gibidir:

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VirtualNetwork
* VirtualNetworkPeering
* VirtualNetworkServiceEndpoint 
* MicrosoftEdge
* None

Her bir sonraki atlama türü hakkında daha fazla bilgi için bkz: [yönlendirmeye genel bakış](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

## <a name="next-steps"></a>Sonraki adımlar

VM ağ yönlendirme sorunlarını tanılamak için sonraki atlama kullanmayı öğrenmek için ağ yönlendirme sorunları kullanarak VM tanılama Bkz [Azure portalında](diagnose-vm-network-routing-problem.md), [PowerShell](diagnose-vm-network-routing-problem-powershell.md), veya [Azure CLI](diagnose-vm-network-routing-problem-cli.md).
