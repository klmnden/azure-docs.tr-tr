---
title: "Hyper-V çoğaltma Azure Site Recovery ile ikincil siteye etkinleştirme | Microsoft Docs"
description: "Azure Site Recovery ile ikincil System Center VMM siteye Hyper-V Vm'lerini çoğaltma için çoğaltmayı etkinleştirmek açıklar."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d8782d14-9fef-4396-8912-ff945efc851b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 6673d192dbc18bfc955d9e7e3c55893512511ffb
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="step-9-enable-replication-to-a-secondary-site-for-hyper-v-vms"></a>9. adım: Hyper-V Vm'lerini ikincil bir site için çoğaltmayı etkinleştirin


Bir çoğaltma ilkesi ayarlama kullandıktan sonra bu makalede bir ikincil şirket içi Hyper-V sanal makinelerini (VM) System Center Virtual Machine Manager (VMM) sitesine çoğaltmayı etkinleştirmek için kullanarak [Azure Site Recovery](site-recovery-overview.md).

Bu makaleyi okuduktan sonra yapmak istediğiniz tüm yorumları makalenin alt kısmında veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda paylaşabilirsiniz.



## <a name="select-vms-to-replicate"></a>Çoğaltma sanal makineleri seçin

1. **2. Adım: Uygulama çoğaltma** > **Kaynak** seçeneklerine tıklayın. 

    ![Çoğaltmayı etkinleştirme](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication1.png)

2. İçinde **kaynak**, VMM sunucusu ve çoğaltmak istediğiniz Hyper-V konakları olduğu bulunduğu Bulutu seçin. Daha sonra, **Tamam**'a tıklayın.

    ![Çoğaltmayı etkinleştirme](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication2.png)
3. İçinde **hedef**, ikincil VMM sunucusu ve bulut doğrulayın.
4. İçinde **sanal makineleri**, listeden korumak istediğiniz sanal makineleri seçin.

    ![Sanal makine korumasını etkinleştirme](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication5.png)

İlerleme durumunu izleyebilirsiniz **korumayı etkinleştir** eylemde **işleri** > **Site Recovery işleri**. Sonra **korumayı Sonlandır** işi tamamlandığında, ilk çoğaltma tamamlandıktan ve sanal makine yük devretme için hazırdır.

Şunlara dikkat edin:

- VMM konsolunda sanal makineler için korumayı etkinleştirebilirsiniz. Tıklatın **korumayı etkinleştir** sanal makine özellikleri araç çubuğunda > **Azure Site Recovery** sekmesi.
- Çoğaltma etkinleştirdikten sonra VM için özelliklerini görüntüleyebilirsiniz **çoğaltılan öğeler**. Üzerinde **Essentials** Pano, VM ve durumunu çoğaltma ilkesi hakkında bilgi görebilirsiniz. Tıklatın **özellikleri** daha fazla ayrıntı için.

## <a name="onboard-existing-vms"></a>Yerleşik VM'ler

Hyper-V çoğaltma ile çoğaltma mevcut sanal makineler VMM varsa, gerçekleştirebilir şekilde Azure Site Recovery çoğaltma için:

1. Var olan VM barındıran Hyper-V server birincil buluta bulunur ve çoğaltma sanal makine barındıran Hyper-V sunucusu ikincil bulutta bulunduğundan emin olun.
2. Bir çoğaltma ilkesi birincil VMM bulutu için yapılandırılmış olduğundan emin olun.
3. Birincil sanal makine çoğaltmayı etkinleştirin. Azure Site Recovery ile VMM aynı çoğaltma konak ve sanal makine algılandığında, ve Azure Site Recovery yeniden kullanmak ve belirtilen ayarları kullanarak çoğaltmayı yeniden oluşturmak emin olun.


## <a name="next-steps"></a>Sonraki adımlar

Git [adım 10: bir yük devretme testi](vmm-to-vmm-walkthrough-test-failover.md).
