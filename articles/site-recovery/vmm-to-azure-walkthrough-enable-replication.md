---
title: "VMM bulutlarındaki Hyper-V sanal makineleri Azure Site Recovery ile Azure için çoğaltmayı etkinleştirme | Microsoft Docs"
description: "Azure'a çoğaltma için Azure Site Recovery hizmeti ile VMM bulutlarındaki Hyper-V sanal makineleri için etkinleştirmeyi açıklar"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 89a2c4fc-7e03-4a86-a2c0-52831ccebc1a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 96a817e43a830e836f2faa4603fc88ed9c0b1828
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="step-11-enable-replication-to-azure-for-hyper-v-vms-in-vmm-clouds"></a>11. adım: VMM bulutlarındaki Hyper-V sanal makineleri Azure için çoğaltmayı etkinleştirin

Bir çoğaltma ilkesi ayarladıktan sonra bu makalede System Center Virtual Machine Manager (VMM) bulutlarında yönetilen şirket içi Hyper-V sanal makineleri (VM'ler) için çoğaltmayı etkinleştirmek için kullanın), Azure, kullanarak [Azure Site Recovery](site-recovery-overview.md) Azure portalında hizmeti.

POST açıklamaları ve soruları alt bu makalenin veya üzerinde [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Başlamadan önce

Azure hesabınızda doğru olduğundan emin olun [izinleri](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines)Azure VM'ler oluşturmak için. [Daha fazla bilgi edinin](../active-directory/role-based-access-built-in-roles.md) Azure rol tabanlı erişim denetimi hakkında.

## <a name="exclude-disks-from-replication"></a>Diskleri çoğaltmanın dışında tutma

Varsayılan olarak, bir makine üzerindeki tüm diskleri çoğaltılır. Diskleri çoğaltmanın dışında bırakabilirsiniz. Örneğin, geçici veriler veya her zaman bir makine yeniledi veri disklerini çoğaltmak istemeyebilirsiniz veya (örneğin pagefile.sys ya da SQL Server tempdb) uygulamasını yeniden başlatır. [Daha fazla bilgi](site-recovery-exclude-disk.md)

## <a name="replicate-vms"></a>Vm'lerini çoğaltma

VM'ler için çoğaltma gibi etkinleştirin:  

1. **2. Adım: Uygulama çoğaltma** > **Kaynak** seçeneklerine tıklayın. Çoğaltmayı ilk kez etkinleştirdikten sonra ek makineler için çoğaltma işlemini etkinleştirmek istiyorsanız kasada **+Çoğalt**'a tıklayın.

    ![Çoğaltmayı etkinleştirme](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication1.png)
2. **Kaynak** dikey penceresinde Hyper-V konaklarının bulunduğu VMM sunucusunu ve bulutu seçin. Daha sonra, **Tamam**'a tıklayın.

    ![Çoğaltmayı etkinleştirme](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication-source.png)
3. **Hedef** kısmında aboneliği, yük devretme sonrası dağıtım modelini ve çoğaltılan veriler için kullandığınız depolama hesabını seçin.

    ![Çoğaltmayı etkinleştirme](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication-target.png)
4. Kullanmak istediğiniz depolama hesabını seçin. Sahip olduğunuz hesaplardan farklı bir depolama hesabı kullanmak isterseniz [yeni bir hesap](#set-up-an-azure-storage-account) oluşturabilirsiniz. Çoğaltılan veriler için bir premium depolama hesabı kullanıyorsanız şirket içi verilerde gerçekleşen değişiklikleri yakalayan çoğaltma günlüklerini depolamak üzere ek bir standart depolama hesabı seçmeniz gerekir. Resource Manager kullanarak depolama hesabı oluşturmak için **Yeni oluştur**'a tıklayın. Klasik modeli kullanarak bir depolama hesabı oluşturmak istiyorsanız bu işlemi [Azure portalından](../storage/common/storage-create-storage-account.md) gerçekleştirin. Daha sonra, **Tamam**'a tıklayın.
5. Yük devretme sonrasında oluşturulan Azure VM'lerinin bağlanacağı Azure ağını ve alt ağını seçin. Koruma için seçtiğiniz tüm makinelere ağ ayarını uygulamak için **Seçili makineler için şimdi yapılandır**’ı seçin. Her makine için Azure ağını ayrı ayrı seçmek üzere **Daha sonra yapılandır**'ı seçin. Sahip olduğunuz ağlardan farklı bir ağ kullanmak isterseniz [yeni bir ağ](#set-up-an-azure-network) oluşturabilirsiniz. Resource Manager modelini kullanarak bir ağ oluşturmak için **Yeni oluştur**'a tıklayın. Klasik modeli kullanarak bir ağ oluşturmak isterseniz bu işlemi [Azure portalından](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) gerçekleştirin. Bir alt ağ (varsa) seçin. Daha sonra, **Tamam**'a tıklayın.
6. **Sanal Makineler** > **Sanal makine seçin** seçeneklerine tıklayın ve çoğaltmak istediğiniz makineleri seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. Daha sonra, **Tamam**'a tıklayın.

    ![Çoğaltmayı etkinleştirme](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication5.png)

7. **Özellikler** > **Özellikleri yapılandır** seçeneklerinde, seçili VM'ler için işletme sistemini ve OS diskini seçin.

    - Azure VM adının (hedef ad) [Azure sanal makine gereksinimlerine](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) uygun olduğundan emin olun.   
    - Varsayılan olarak VM'nin tüm diskleri çoğaltma için seçilir ancak istemediklerinizin işaretini kaldırarak onları hariç tutabilirsiniz.

        - Çoğaltma bant genişliğini azaltmak için diskleri hariç tutmak isteyebilirsiniz. Örneğin geçici verilere veya bilgisayar ya da uygulama yeniden başlatıldığında yenilenen verilere (pagefile.sys veya Microsoft SQL Server tempdb gibi) sahip diskleri çoğaltmak istemeyebilirsiniz. Diskin seçimini kaldırarak çoğaltma kapsamı dışında bırakabilirsiniz.
        - Yalnızca temel diskler dışarıda bırakılabilir. İşletim sistemi diskleri dışarıda bırakılamaz.
        - Dinamik diskleri dışarıda tutmamanızı öneririz. Site Recovery, konuk VM içerisindeki bir sanal sabit diskin temel mi yoksa dinamik mi olduğunu anlayamaz. Bağımlı dinamik birim disklerinin tümü hariç bırakılmazsa, korumalı dinamik disk VM yük devrinde başarısız olur ve diskteki verilere erişim sağlanamaz.
        - Çoğaltmayı etkinleştirdikten sonra çoğaltma için disk ekleme veya kaldırma gerçekleştiremezsiniz. Disk eklemek veya dışlamak istiyorsanız, VM’nin korumasını devre dışı bırakmanız ve yeniden etkinleştirmeniz gerekir.
        - Azure'da el ile oluşturduğunuz diskler yeniden çalışır duruma getirilmez. Örneğin, üç disk için yük devretme gerçekleştirir ve ikisini doğrudan Azure sanal makinesinde oluşturursanız, yalnızca yük devretme gerçekleştirilen üç disk Azure'dan Hyper-V'ye çoğaltılarak yeniden çalışır hale getirilir. El ile oluşturulmuş diskleri yeniden çalışma veya Hyper-V'den Azure'a geri çoğaltma işlemine dahil edemezsiniz.
        - Bir uygulamanın çalışması için gerekli olan bir diski hariç tutarsanız, Azure'a yük devretme gerçekleştirildikten sonra çoğaltılan uygulamanın çalışması için Azure'da el ile oluşturmanız gerekir. Alternatif olarak makinenin yük devri sırasında diskin otomatik olarak oluşturulması için Azure otomasyonunu bir kurtarma planıyla tümleştirebilirsiniz.

    Değişiklikleri kaydetmek için **Tamam**'a tıklayın. Daha sonra ek özellikleri ayarlayabilirsiniz.

    ![Çoğaltmayı etkinleştirme](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication6-with-exclude-disk.png)

8. **Çoğaltma ayarları** > **Çoğaltma ayarlarını yapılandır** seçeneklerinde, korumalı VM'lere uygulamak istediğiniz çoğaltma ilkesini seçin. Daha sonra, **Tamam**'a tıklayın. **Çoğaltma ilkeleri** > ilke adı > **Ayarları Düzenle**’de çoğaltma ilkesini değiştirebilirsiniz. Uyguladığınız değişiklikler, zaten çoğaltılmakta olan makinelere ve yeni makinelere uygulanır.

   ![Çoğaltmayı etkinleştirme](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication7.png)

**İşler** > **Site Recovery işleri** üzerinden **Korumayı Etkinleştir** işinin ilerleyişini izleyebilirsiniz. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.



## <a name="next-steps"></a>Sonraki adımlar

Git [adım 12: yük devretme testi çalıştırma](vmm-to-azure-walkthrough-test-failover.md)
