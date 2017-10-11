---
title: "Azure Site Recovery kullanarak Hyper-V çoğaltma (System Center VMM olmadan) için bir kasa ayarlama | Microsoft Docs"
description: "Azure Site Recovery kullanarak Hyper-V çoğaltma için bir kasa ayarlamak için gereken adımları özetler"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a936b3e2-0dbe-47ac-a98e-5285d96dc786
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: 8212ff011633c3a89d3310e828b6d5f1cda6ce3f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="step-7-set-up-a-vault-for-hyper-v-replication"></a>7. adım: Hyper-V çoğaltma için bir kasa ayarlayın

Bu makalede bir kasasını oluşturup ve Azure kullanarak, şirket içi konumdan çoğaltmak istediğiniz belirtmek nasıl [Azure Site Recovery](site-recovery-overview.md) Azure portalında hizmet.


POST açıklamaları ve soruları alt bu makalenin veya üzerinde [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]



## <a name="select-a-protection-goal"></a>Koruma hedefi seçin

Neleri çoğaltmak istediğinizi ve bunları nereye çoğaltacağınızı seçin.

1. Tıklatın **kurtarma Hizmetleri kasaları** > kasası.
2. Kaynak menüye tıklayın **Site Recovery** > **altyapıyı hazırlama** > **koruma hedefi**.
3. İçinde **koruma hedefi**seçin **için Azure** > **Evet, Hyper-V ile**. Seçin **Hayır** değil VMM kullandığınızı doğrulamak için. 

    ![Hedefleri seçme](./media/hyper-v-site-walkthrough-create-vault/choose-goals2.png)



## <a name="next-steps"></a>Sonraki adımlar

Git [adım 8: kaynak ve hedef ayarlama](hyper-v-site-walkthrough-source-target.md)
