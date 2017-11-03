---
title: "Azure Ağ İzleyicisi topolojisinde giriş | Microsoft Docs"
description: "Bu sayfa Ağ İzleyicisi topoloji özelliklerine genel bakış sağlar."
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: e753a435-38e0-482b-846b-121cb547555c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: d5cb5ba431eeae1956a9dbf1d48561c66faef9a6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-topology-in-azure-network-watcher"></a>Azure Ağ İzleyicisi topolojisinde giriş

Topoloji, bir sanal ağ ağ kaynaklarının bir grafik döndürür. Grafikte uçtan uca ağ bağlantısını temsil etmek için kaynakları arasındaki bağlantısı gösterilmektedir.

![Topoloji genel bakış][1]

Portalda, topoloji kaynak nesneleri döndürür bir sanal ağ temelinde. Kaynak grubu görüntülenmeyecek olsa bile Ağ İzleyicisi bölgesi dışında kaynakları kaynakları arasındaki çizgilerle ilişkileri gösterilen. Portal görünümünde döndürülen kaynakları grafiği çizilecek ağ bileşenleri bir alt kümesidir. Ağ kaynakları kullanabileceğiniz tam listesini görmek için [PowerShell](network-watcher-topology-powershell.md) veya [REST](network-watcher-topology-rest.md)

> [!NOTE]
> Ağ İzleyicisi örneği topoloji çalıştırmak istediğiniz her bir bölge gereklidir.

Kaynakları arasında bağlantı döndürülen gibi altında iki ilişki Modellenen.

- **Kapsama** -örnek: sanal ağ, bir NIC içeren bir alt ağ içerir
- **İlişkili** -örnek: bir NIC VM ile ilişkili

### <a name="next-steps"></a>Sonraki adımlar

Ziyaret ederek topoloji görünümü almak için PowerShell kullanmayı öğrenin [PowerShell ile Ağ İzleyicisi topolojisi](network-watcher-topology-powershell.md)

<!--Image references-->

[1]: ./media/network-watcher-topology-overview/topology.png
