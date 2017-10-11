---
title: "Azure Site RECOVERY'yi kullanarak azure'da VMware çoğaltma için bir kasa ayarlama | Microsoft Docs"
description: "Azure Site RECOVERY'yi kullanarak azure'da VMware çoğaltma için bir kasa ayarlamak için gereken adımları özetler"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 8bce940e-f19f-4418-8360-aee7b073519a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: dca95ad46b8de587140c3573ba6ed5702a122032
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="step-7-set-up-a-vault-for-vmware-replication-to-azure"></a>7. adım: Azure VMware çoğaltma için bir kasa ayarlayın


Bu makalede bir kasasını oluşturup ve Azure kullanarak, şirket içi konumdan çoğaltmak istediğiniz belirtmek nasıl [Azure Site Recovery](site-recovery-overview.md) Azure portalında hizmet.


POST açıklamaları ve soruları alt bu makalenin veya üzerinde [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).




## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>Koruma hedefi seçin

Neleri çoğaltmak istediğinizi ve bunları nereye çoğaltacağınızı seçin.

1. Tıklatın **kurtarma Hizmetleri kasaları** > kasası.
2. Kaynak menüye tıklayın **Site Recovery** > **altyapıyı hazırlama** > **koruma hedefi**.
3. İçinde **koruma hedefi**seçin **için Azure** > **Evet, VMware vSphere hiper yönetici ile**.



## <a name="next-steps"></a>Sonraki adımlar

Git [adım 8: kaynak ve hedef ayarlama](vmware-walkthrough-source-target.md)
