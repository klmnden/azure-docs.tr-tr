---
title: "Azure Site Recovery ile ikincil siteye Hyper-V çoğaltma için bir kasa oluşturun | Microsoft Docs"
description: "Azure Site Recovery ile ikincil System Center VMM siteye Hyper-V sanal makineleri çoğaltırken bir kasa oluşturmayı açıklar."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ff65dbfb-cb26-410e-ab48-76971625db08
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 28cfcf12b2e369f96664c163c0b6f2aa8a6ddcb9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="step-5-create-a-vault-for-hyper-v-replication-to-a-secondary-site"></a>5. adım: Hyper-V çoğaltma için bir kasa ikincil bir siteye oluşturma

Şirket içi hazırlandıktan sonra [System Center Virtual Machine Manager (VMM) sunucuları ve Hyper-V konakları/kümeleri](vmm-to-vmm-walkthrough-vmm-hyper-v.md) kullanarak bir ikincil site için Hyper-V çoğaltma için [Azure Site Recovery](site-recovery-overview.md), oluşturabileceğiniz bir Kurtarma Hizmetleri kasası ve çoğaltma senaryosuna seçin.

Bu makaleyi okuduktan sonra yapmak istediğiniz tüm yorumları makalenin alt kısmında veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda paylaşabilirsiniz.


## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="choose-a-protection-goal"></a>Koruma hedefi seçin

Neleri çoğaltmak istediğinizi ve bunları nereye çoğaltacağınızı seçin.

1. Tıklatın **Site Recovery** > **1. adım: altyapıyı hazırlama** > **koruma hedefi**.
2. Seçin **kurtarma sitesine**seçip **Evet, Hyper-V ile**.
3. Seçin **Evet** Hyper-V ana bilgisayarları yönetmek için VMM kullandığınızı belirtmek için.
4. Seçin **Evet** bir ikincil VMM sunucunuz varsa. Tek bir VMM sunucusundaki Bulutlar arasında çoğaltma dağıtıyorsanız tıklatın **Hayır**. Daha sonra, **Tamam**'a tıklayın.

    ![Hedefleri seçme](./media/vmm-to-vmm-walkthrough-create-vault/choose-goals.png)



## <a name="next-steps"></a>Sonraki adımlar

Git [6. adım: çoğaltma kaynağı ve hedef ayarlama](vmm-to-vmm-walkthrough-source-target.md).