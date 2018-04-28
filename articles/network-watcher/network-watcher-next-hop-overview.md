---
title: Azure Ağ İzleyicisi sonraki atlama giriş | Microsoft Docs
description: Bu makalenin sonraki atlama özelliği Ağ İzleyicisi'ne genel bakış sağlar.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: febf7bca-e0b7-41d5-838f-a5a40ebc5aac
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: bbb782e700781dcfedbbd340c7d10db53767b035
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="use-next-hop-to-diagnose-virtual-machine-routing-problems"></a>Sanal makine yönlendirme sorunlarını tanılamak için sonraki atlama kullanın

Bir sanal makineden (VM) trafiği bir ağ arabirimi (NIC) ile ilişkili etkili rotalarını dayalı bir hedefe gönderilir. Sonraki atlama IP adresini bir paket ve sonraki atlama türü belirli bir VM ve NIC alır Sonraki atlama bilerek hedeflenen hedefe yönelik trafiği veya trafiği hiçbir yerde gönderilen olup olmadığını belirlemenize yardımcı olur. Burada trafiği bir şirket içi konumu veya bir sanal gereç yönlendirilir, yolların hatalı bir yapılandırma bağlantısı sorunlarına yol açabilir. Sonraki atlama ayrıca sonraki atlama ile ilişkili yol tablosu döndürür. Rota kullanıcı tanımlı bir yol olarak tanımlanmışsa, bu rota döndürülür. Aksi halde, sonraki atlama döndürür **sistem yolu**.

![sonraki atlama genel bakış](./media/network-watcher-next-hop-overview/figure1.png)

Sonraki atlama yeteneği tarafından döndürülen sonraki atlama aşağıdaki gibidir:

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VnetLocal
* VnetPeering
* None

Her sonraki atlama türü hakkında daha fazla bilgi için bkz: [yönlendirmeye genel bakış](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

## <a name="next-steps"></a>Sonraki adımlar

VM ağ yönlendirme sorunları tanılamak için sonraki atlama kullanmayı öğrenmek için bkz: [tanılama VM ağ yönlendirme sorunlarını](diagnose-vm-network-routing-problem.md).
