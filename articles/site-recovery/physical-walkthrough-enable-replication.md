---
title: "Fiziksel sunucuyu Azure Site Recovery ile azure'a çoğaltmak için çoğaltmayı etkinleştirme | Microsoft Docs"
description: "Azure Site Recovery hizmetini kullanarak Azure'a çoğaltma için fiziksel sunucuları için etkinleştirmeniz gerekiyor adımları özetler"
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
ms.openlocfilehash: 42f35c53eec06a346281fd90c97aecfd2269307d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="step-10-enable-replication-for-physical-servers-to-azure"></a>10. adım: Azure fiziksel sunucuları için çoğaltmayı etkinleştirme


Bu makalede Azure, şirket içi Windows/Linux fiziksel sunucuları için çoğaltma etkinleştirme kullanarak [Azure Site Recovery](site-recovery-overview.md) Azure portalında hizmet.

POST açıklamaları ve soruları alt bu makalenin veya üzerinde [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Başlamadan önce

- Sunucuları olmalıdır [Mobility hizmeti bileşeninin yüklü](physical-walkthrough-install-mobility.md).
- Bir VM gönderme yüklemesi için hazırlanmış, çoğaltma etkinleştirdiğinizde işlem sunucusu Mobility hizmeti otomatik olarak yükler.
- Bir makine için çoğaltma etkinleştirdiğinizde, Azure kullanıcı hesabınızın belirli gereken [izinleri](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) Azure depolama kullanan ve Azure VM'ler oluşturmak mümkün emin olmak için.
- Ekleyin veya sunucularının IP adreslerini değiştirdiğinizde, 15 dakika veya daha uzun değişikliklerin etkili olması ve sunumların portalda görünebilmesi kadar sürebilir.


## <a name="exclude-disks-from-replication"></a>Diskleri çoğaltmanın dışında tutma

Varsayılan olarak, bir makine üzerindeki tüm diskleri çoğaltılır. Diskleri çoğaltmanın dışında bırakabilirsiniz. Örneğin, geçici veriler veya her zaman bir makine yeniledi veri disklerini çoğaltmak istemeyebilirsiniz veya (örneğin pagefile.sys ya da SQL Server tempdb) uygulamasını yeniden başlatır. [Daha fazla bilgi](site-recovery-exclude-disk.md)

## <a name="replicate-servers"></a>Çoğaltma sunucuları

1. **2. Adım: Uygulama çoğaltma** > **Kaynak** seçeneklerine tıklayın.
2. İçinde **kaynak**, yapılandırma sunucusu seçin.
3. İçinde **makine türü**seçin **fiziksel makineleri**.
4. İşlem sunucusu seçin. Herhangi bir ek işlem sunucusu oluşturmadıysanız bu yapılandırma sunucusu olacaktır. Daha sonra, **Tamam**'a tıklayın.
5. İçinde **hedef**, abonelik ve başarısız VM'ler üzerinde oluşturmak istediğiniz kaynak grubunu seçin. Devredilen sanal makineleri için Azure (Yönetim), Klasik veya resource kullanmak istediğiniz dağıtım modelini seçin.
6. Veri çoğaltmak için kullanmak istediğiniz Azure depolama hesabı seçin. Ayarlamış bir hesap kullanmak istemiyorsanız, yeni bir tane oluşturabilirsiniz.
7. Seçin **Azure ağ** ve **alt** , yük devretme sonrasında Azure Vm'lerinin bağlanmak için. Koruma için seçtiğiniz tüm makinelere ağ ayarını uygulamak için **Seçili makineler için şimdi yapılandır**’ı seçin. Makineler için Azure ağını ayrı ayrı seçmek için **Daha sonra yapılandır**'ı seçin. Varolan bir ağı kullanmak istemiyorsanız, bir tane oluşturabilirsiniz.

    ![Çoğaltmayı etkinleştirme](./media/physical-walkthrough-enable-replication/targetsettings.png)

8. İçinde **fiziksel makineleri**, tıklatın **+ fiziksel makine** ve girin **adı** ve **IP adresi**. Çoğaltmak istediğiniz makinenin işletim sistemini seçin. Makineler bulunan ve listede gösterilen kadar birkaç dakika sürer.
9. İçinde **özellikleri** > **özelliklerini yapılandırma**, işlem sunucusu tarafından otomatik olarak makinede Mobility hizmeti yüklemek için kullanılacak hesabı seçin.
10. Varsayılan olarak tüm diskler çoğaltılır. Tıklatın **tüm diskleri** ve çoğaltmak için istemediğiniz tüm diskleri temizleyin. Daha sonra, **Tamam**'a tıklayın. Ek VM özellikleri daha sonra ayarlayabilirsiniz.

    ![Çoğaltmayı etkinleştirme](./media/physical-walkthrough-enable-replication/enable-replication6.png)
11. İçinde **çoğaltma ayarları** > **çoğaltma ayarlarını yapılandırın**, doğru Çoğaltma İlkesi'nin seçili olduğunu doğrulayın. Bir ilkeyi değiştirirseniz, değişiklikler makinenin çoğaltıldığını ve yeni makinelere uygulanır.
12. Etkinleştirme **çoklu VM tutarlılığını** makineler çoğaltma grubuna toplayın ve grup için bir ad belirtmek istiyorsanız. Daha sonra, **Tamam**'a tıklayın. Şunlara dikkat edin:

    * Çoğaltma gruplarındaki makineler birlikte çoğaltılır ve kilitlenme tutarlı ve uygulamayla tutarlı kurtarma noktaları üzerinden başarısız olduğunda paylaşılan.
    * Böylece, iş yüklerini yansıtma fiziksel sunucuları araya toplamak öneririz. Çoklu VM tutarlılığını etkinleştirmek, iş yükü performansını etkileyebilir ve yalnızca makineler aynı iş yükünü çalıştırıyorsa ve tutarlılık ihtiyacınız varsa kullanılmalıdır.

13. Tıklatın **çoğaltmasını etkinleştir**. İlerleme durumunu izleyebilirsiniz **korumayı etkinleştir** iş **ayarları** > **işleri** > **Site Recovery işleri**. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.

## <a name="next-steps"></a>Sonraki adımlar

Git [11. adım: yük devretme testi çalıştırma](physical-walkthrough-test-failover.md)
