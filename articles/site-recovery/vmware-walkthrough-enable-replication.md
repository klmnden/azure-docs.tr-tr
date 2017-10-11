---
title: "Azure Site Recovery ile azure'a VMware Vm'lerini çoğaltma için çoğaltma etkinleştirme | Microsoft Docs"
description: "Azure VM'ler için çoğaltma VMware Azure Site Recovery hizmetini kullanarak etkinleştirmek için gereken adımları özetler"
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 519c5559-7032-4954-b8b5-f24f5242a954
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 470b9ddd8df4a4e74ec7174f79020c252323e502
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="step-11-enable-replication-for-vmware-virtual-machines-to-azure"></a>11. adım: Azure VMware sanal makineler için çoğaltmayı etkinleştirme


Bu makalede Azure, şirket içi VMware sanal makineler için çoğaltma etkinleştirme kullanarak [Azure Site Recovery](site-recovery-overview.md) Azure portalında hizmet.

POST açıklamaları ve soruları alt bu makalenin veya üzerinde [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Başlamadan önce

- VMware sanal makinelerini olmalıdır [Mobility hizmeti bileşeninin yüklü](vmware-walkthrough-install-mobility.md). -Bir VM gönderme yüklemesi için hazırlanmış, çoğaltma etkinleştirdiğinizde işlem sunucusu otomatik olarak Mobility hizmetini yükler.
- Azure kullanıcı hesabınızın belirli gereken [izinleri](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) Azure bir VM'nin çoğaltmayı etkinleştirmek için
- Ekleyin veya VM'ler değiştirdiğinizde, 15 dakika veya daha uzun değişikliklerin etkili olması ve sunumların portalda görünebilmesi kadar sürebilir.
- Vm'lerde bulunan en son ne zaman denetleyebilir **yapılandırma sunucularına** > **en son kişi**.
- VM'ler için zamanlanmış bulma beklemeden eklemek için yapılandırma sunucusu vurgulayın (tıklatın yok), tıklatıp **yenileme**.



## <a name="exclude-disks-from-replication"></a>Diskleri çoğaltmanın dışında tutma

Varsayılan olarak, bir makine üzerindeki tüm diskleri çoğaltılır. Diskleri çoğaltmanın dışında bırakabilirsiniz. Örneğin, geçici veriler veya her zaman bir makine yeniledi veri disklerini çoğaltmak istemeyebilirsiniz veya (örneğin pagefile.sys ya da SQL Server tempdb) uygulamasını yeniden başlatır. [Daha fazla bilgi](site-recovery-exclude-disk.md)

## <a name="replicate-vms"></a>Vm'lerini çoğaltma

Başlamadan önce hızlı bir video genel bakış izleyin

>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video3-Protect-VMware-Virtual-Machines/player]

1. **2. Adım: Uygulama çoğaltma** > **Kaynak** seçeneklerine tıklayın.
2. İçinde **kaynak**, yapılandırma sunucusu seçin.
3. İçinde **makine türü**seçin **sanal makineleri**.
4. İçinde **vCenter/vSphere hiper yönetici**vSphere ana yöneten vCenter sunucusu seçin veya konağı seçin.
5. İşlem sunucusu seçin. Herhangi bir ek işlem sunucusu oluşturmadıysanız bu yapılandırma sunucusu olacaktır. Daha sonra, **Tamam**'a tıklayın.

    ![Çoğaltmayı etkinleştirme](./media/vmware-walkthrough-enable-replication/enable-replication2.png)

6. İçinde **hedef**, abonelik ve başarısız VM'ler üzerinde oluşturmak istediğiniz kaynak grubunu seçin. Devredilen sanal makineleri için Azure (Yönetim), Klasik veya resource kullanmak istediğiniz dağıtım modelini seçin.


7. Veri çoğaltmak için kullanmak istediğiniz Azure depolama hesabı seçin. Ayarlamış bir hesap kullanmak istemiyorsanız, yeni bir tane oluşturabilirsiniz.

8. Yük devretme sonrasında oluşturulan Azure VM'lerinin bağlanacağı Azure ağını ve alt ağını seçin. Koruma için seçtiğiniz tüm makinelere ağ ayarını uygulamak için **Seçili makineler için şimdi yapılandır**’ı seçin. Makineler için Azure ağını ayrı ayrı seçmek için **Daha sonra yapılandır**'ı seçin. Varolan bir ağı kullanmak istemiyorsanız, bir tane oluşturabilirsiniz.

    ![Çoğaltmayı etkinleştirme](./media/vmware-walkthrough-enable-replication/enable-rep3.png)
9. **Sanal Makineler** > **Sanal makine seçin** seçeneklerine tıklayın ve çoğaltmak istediğiniz makineleri seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. Daha sonra, **Tamam**'a tıklayın.

    ![Çoğaltmayı etkinleştirme](./media/vmware-walkthrough-enable-replication/enable-replication5.png)
10. İçinde **özellikleri** > **özelliklerini yapılandırma**, işlem sunucusu tarafından otomatik olarak makinede Mobility hizmeti yüklemek için kullanılacak hesabı seçin.
11. Varsayılan olarak tüm diskler çoğaltılır. Tıklatın **tüm diskleri** ve çoğaltmak için istemediğiniz tüm diskleri temizleyin. Daha sonra, **Tamam**'a tıklayın. Ek VM özellikleri daha sonra ayarlayabilirsiniz.

    ![Çoğaltmayı etkinleştirme](./media/vmware-walkthrough-enable-replication/enable-replication6.png)
11. İçinde **çoğaltma ayarları** > **çoğaltma ayarlarını yapılandırın**, doğru Çoğaltma İlkesi'nin seçili olduğunu doğrulayın. Bir ilkeyi değiştirirseniz, değişiklikler makinenin çoğaltıldığını ve yeni makinelere uygulanır.
12. Etkinleştirme **çoklu VM tutarlılığını** makineler çoğaltma grubuna toplayın ve grup için bir ad belirtmek istiyorsanız. Daha sonra, **Tamam**'a tıklayın. Şunlara dikkat edin:

    * Çoğaltma gruplarındaki makineler birlikte çoğaltılır ve kilitlenme tutarlı ve uygulamayla tutarlı kurtarma noktaları üzerinden başarısız olduğunda paylaşılan.
    * Böylece, iş yüklerini yansıtma sanal makineleri ve fiziksel sunucuları araya toplamak öneririz. Çoklu VM tutarlılığını etkinleştirmek, iş yükü performansını etkileyebilir ve yalnızca makineler aynı iş yükünü çalıştırıyorsa ve tutarlılık ihtiyacınız varsa kullanılmalıdır.

    ![Çoğaltmayı etkinleştirme](./media/vmware-walkthrough-enable-replication/enable-replication7.png)
13. Tıklatın **çoğaltmasını etkinleştir**. İlerleme durumunu izleyebilirsiniz **korumayı etkinleştir** iş **ayarları** > **işleri** > **Site Recovery işleri**. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.

## <a name="next-steps"></a>Sonraki adımlar

Git [adım 12: yük devretme testi çalıştırma](vmware-walkthrough-test-failover.md)
