---
title: "Azure Ağ İzleyicisi sonraki atlama giriş | Microsoft Docs"
description: "Bu sayfa Ağ İzleyicisi'ne genel bakış sonraki atlama yeteneği sağlar."
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: febf7bca-e0b7-41d5-838f-a5a40ebc5aac
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: bb2ca0486b3b3d27a77b70927cb3cbfbeac12c7c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-next-hop-in-azure-network-watcher"></a>Azure Ağ İzleyicisi sonraki atlama giriş

Bir VM'ye gelen trafiği bir NIC ile ilişkili etkili rotalarını dayalı bir hedefe gönderilir Sonraki atlama IP adresini bir paket ve sonraki atlama türü bir belirli bir sanal makine ve NIC alır Bu paketin hedef yönlendirilmiş veya siyah olan trafik holed belirlemeye yardımcı olur. Burada trafik bir şirket içi konumu veya bir sanal gereç yönlendirilir, bir kullanıcı tarafından yolların hatalı bir yapılandırma bağlantısı sorunlarına yol açabilir. Sonraki atlama ayrıca sonraki atlama ile ilişkili yol tablosu döndürür. Varsa rota kullanıcı tanımlı bir yol olarak tanımlanan bir sonraki atlama sorgularken bu rota döndürülür. Aksi takdirde sonraki atlama "Sistem yolu" döndürür.

![sonraki atlama genel bakış][1]

Sonraki atlama sorgulanırken döndürülen sonraki atlama türlerini listesi verilmiştir.

* Internet
* Değerinin VirtualAppliance
* VirtualNetworkGateway
* VnetLocal
* HyperNetGateway
* VnetPeering
* None

### <a name="next-steps"></a>Sonraki adımlar

Sonraki atlama adresini ziyaret ederek ile ağ bağlantısı sorunları bulmak için nasıl kullanılacağını öğrenin [bir VM'de sonraki atlama denetleyin](network-watcher-check-next-hop-portal.md)

<!--Image references-->
[1]: ./media/network-watcher-next-hop-overview/figure1.png













