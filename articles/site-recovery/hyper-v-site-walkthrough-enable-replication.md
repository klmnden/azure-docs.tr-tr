---
title: "Azure Site Recovery ile azure'a (System Center VMM olmadan) Hyper-V Vm'lerini çoğaltma için çoğaltma etkinleştirme | Microsoft Docs"
description: "Azure VM'ler için çoğaltma Hyper-V Azure Site Recovery hizmetini kullanarak etkinleştirmek için gereken adımları özetler"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: bd31ef01-69f1-4591-a519-e42510a6e2f4
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: aabe99dbd375b80e4a87ca7a067927008672b4ed
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="step-10-enable-replication-for-hyper-v-vms-replicating-to-azure"></a>10. adım: Hyper-V Vm'lerini Azure'a çoğaltma için çoğaltmayı etkinleştirme


Bu makalede Azure, şirket içi Hyper-V sanal (System Center VMM tarafından yönetilmediğinden) makineler için çoğaltma etkinleştirme kullanarak [Azure Site Recovery](site-recovery-overview.md) Azure portalında hizmet.

POST açıklamaları ve soruları alt bu makalenin veya üzerinde [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).




## <a name="before-you-start"></a>Başlamadan önce

Başlamadan önce Azure kullanıcı hesabınızın gerekli olduğundan emin olun [izinleri](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) yeni bir sanal makineye Azure çoğaltmayı etkinleştirmek için.

## <a name="exclude-disks-from-replication"></a>Diskleri çoğaltmanın dışında tutma

Varsayılan olarak, bir makine üzerindeki tüm diskleri çoğaltılır. Diskleri çoğaltmanın dışında bırakabilirsiniz. Örneğin, geçici veriler veya her zaman bir makine yeniledi veri disklerini çoğaltmak istemeyebilirsiniz veya (örneğin pagefile.sys ya da SQL Server tempdb) uygulamasını yeniden başlatır. [Daha fazla bilgi](site-recovery-exclude-disk.md)


## <a name="replicate-vms"></a>Vm'lerini çoğaltma

VM'ler için çoğaltma gibi etkinleştirin:          

1. Tıklatın **uygulama çoğaltma** > **kaynak**. Çoğaltma ilk kez ayarladıktan sonra tıklayabilirsiniz **+ Çoğalt** ilave makineler için çoğaltmayı etkinleştirmek için.

    ![Çoğaltmayı etkinleştirme](./media/hyper-v-site-walkthrough-enable-replication/enable-replication.png)
2. İçinde **kaynak**, Hyper-V sitesi seçin. Daha sonra, **Tamam**'a tıklayın.
3. İçinde **hedef**, kasa aboneliği seçin ve yük devretme modelini yük devretme sonrasında Azure (Klasik veya resource management) kullanmak istediğiniz.
4. Kullanmak istediğiniz depolama hesabını seçin. Kullanmak istediğiniz bir hesabınız yoksa, şunları yapabilirsiniz [oluşturmak](#set-up-an-azure-storage-account). Daha sonra, **Tamam**'a tıklayın.
5. Azure ağı ve yük devretme oluşturulduğunda Azure Vm'lerinin bağlanacağı alt ağı seçin.

    - Etkinleştirmeniz tüm makineleri çoğaltma için ağ ayarlarını uygulamak için seçin **seçili makineler için Şimdi Yapılandır**.
    - Makineler için Azure ağını ayrı ayrı seçmek için **Daha sonra yapılandır**'ı seçin.
    - Kullanmak istediğiniz ağ yoksa, şunları yapabilirsiniz [oluşturmak](#set-up-an-azure-network). Bir alt ağ (varsa) seçin. Daha sonra, **Tamam**'a tıklayın.

   ![Çoğaltmayı etkinleştirme](./media/hyper-v-site-walkthrough-enable-replication/enable-replication11.png)

6. **Sanal Makineler** > **Sanal makine seçin** seçeneklerine tıklayın ve çoğaltmak istediğiniz makineleri seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. Daha sonra, **Tamam**'a tıklayın.

    ![Çoğaltmayı etkinleştirme](./media/hyper-v-site-walkthrough-enable-replication/enable-replication5-for-exclude-disk.png)

7. **Özellikler** > **Özellikleri yapılandır** seçeneklerinde, seçili VM'ler için işletme sistemini ve OS diskini seçin.
8. Azure VM adının (hedef ad) [Azure sanal makine gereksinimlerine](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) uygun olduğundan emin olun.
9. Varsayılan olarak VM'nin tüm diskleri çoğaltma için seçilir. İçermeyecek şekilde Temizle diskler.
10. Değişiklikleri kaydetmek için **Tamam**'a tıklayın. Daha sonra ek özellikleri ayarlayabilirsiniz.

    ![Çoğaltmayı etkinleştirme](./media/hyper-v-site-walkthrough-enable-replication/enable-replication6-with-exclude-disk.png)

11. **Çoğaltma ayarları** > **Çoğaltma ayarlarını yapılandır** seçeneklerinde, korumalı VM'lere uygulamak istediğiniz çoğaltma ilkesini seçin. Daha sonra, **Tamam**'a tıklayın. Çoğaltma ilkesini değiştirebilirsiniz **çoğaltma ilkeleri** > ilke adı > **ayarlarını Düzenle**. Uyguladığınız değişiklikler, zaten çoğaltılmış olan makinelere ve yeni makinelere uygulanır.


   ![Çoğaltmayı etkinleştirme](./media/hyper-v-site-walkthrough-enable-replication/enable-replication7.png)

**İşler** > **Site Recovery işleri** üzerinden **Korumayı Etkinleştir** işinin ilerleyişini izleyebilirsiniz. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.


## <a name="next-steps"></a>Sonraki adımlar


Git [11. adım: yük devretme testi çalıştırma](hyper-v-site-walkthrough-test-failover.md)
