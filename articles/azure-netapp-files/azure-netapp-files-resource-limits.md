---
title: Azure NetApp dosyaları için kaynak sınırları | Microsoft Docs
description: Sınırları kapasitesi havuzu, birimleri ve temsilci alt ağ sınırları dahil olmak üzere, NetApp dosyaları Azure kaynakları için açıklar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 01/03/2019
ms.author: b-juche
ms.openlocfilehash: f34afb1df2ae38353f29a80bfb6798c16856dbeb
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54057162"
---
# <a name="resource-limits-for-azure-netapp-files"></a>Azure NetApp dosyaları için kaynak sınırları
Azure için NetApp dosyaları kaynak sınırları anlama birimlerinizi yönetmenize yardımcı olur.

## <a name="capacity_pools"></a>Kapasite havuzları

- Tek kapasitesi havuzu için en düşük Boyut 4 TiB, ve en büyük boyutu 500 TiB. 
- Her kapasitesi havuzu, yalnızca bir NetApp hesabına ait olabilir. Ancak, bir NetApp hesabında birden fazla kapasitesi havuzu olabilir.  

## <a name="volumes"></a>Birimler

- En küçük boyut tek bir birim için 100 GiB, ve en büyük boyutu 92 TiB.
- Bölge başına en fazla Azure aboneliği başına 100 birim olabilir.  

## <a name="delegated_subnet"></a>Temsilci alt ağ 

Her Azure sanal ağı (Vnet), yalnızca bir alt ağ, Azure için NetApp dosyaları atanabilir.

## <a name="next-steps"></a>Sonraki adımlar

[NetApp dosyaları Azure depolama hiyerarşisini anlama](azure-netapp-files-understand-storage-hierarchy.md)
