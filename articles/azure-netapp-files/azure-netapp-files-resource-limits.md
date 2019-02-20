---
title: Azure NetApp dosyaları için kaynak sınırları | Microsoft Docs
description: NetApp hesapları, kapasitesi havuzu, birimler, anlık görüntüler ve temsilci alt ağ için sınırları dahil olmak üzere, NetApp dosyaları Azure kaynakları için limitler açıklanmıştır.
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
ms.topic: concepts
ms.date: 02/14/2019
ms.author: b-juche
ms.openlocfilehash: 196d85917e0a9900e141d58bff171beeb8540409
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430022"
---
# <a name="resource-limits-for-azure-netapp-files"></a>Azure NetApp Files için kaynak sınırları

Azure için NetApp dosyaları kaynak sınırları anlama birimlerinizi yönetmenize yardımcı olur.

- Her Azure aboneliği, en fazla 10 NetApp hesabı olabilir.
- NetApp her hesap en çok 25 kapasitesi havuzu olabilir.
- Her kapasitesi havuzu, yalnızca bir NetApp hesabına ait olabilir.  
- Tek kapasitesi havuzu için en düşük Boyut 4 TiB, ve en büyük boyutu 500 TiB. 
- En fazla 500 birimlerin her kapasitesi havuzu olabilir.
- En küçük boyut tek bir birim için 100 GiB, ve en büyük boyutu 92 TiB.
- Her birim, en fazla 255 anlık görüntü olabilir.
- Her Azure sanal ağı (Vnet), Azure için NetApp dosyaları temsilci yalnızca bir alt ağa sahip olabilir.

**Sonraki adımlar**

[NetApp dosyaları Azure depolama hiyerarşisini anlama](azure-netapp-files-understand-storage-hierarchy.md)
