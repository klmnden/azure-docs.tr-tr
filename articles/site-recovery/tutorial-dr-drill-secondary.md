---
title: "Azure Site Recovery ile ikincil şirket içi sitenizi bir olağanüstü durum kurtarma ayrıntıya çalıştırmayı | Microsoft Docs"
description: "Azure Site Recovery ile ikincil şirket içi sitenize bir olağanüstü durum kurtarma ayrıntıya çalıştırma hakkında bilgi edinin"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 445878e2-6682-49ba-914d-4c6824ab08a6
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/18/2017
ms.author: raynew
ms.openlocfilehash: 8737c2a22bd729cfc15d5448e1ec0becef643fd5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="run-a-disaster-recovery-drill-for-hyper-v-vms-to-your-secondary-on-premises-site"></a>Bir olağanüstü durum kurtarma ayrıntıya Hyper-V VM'ler için ikincil şirket içi sitenize çalıştırın.

[Azure Site Recovery](site-recovery-overview.md) hizmeti yönetme ve çoğaltma, yük devretme ve yeniden çalışma şirket içi makineler ve Azure sanal makineleri (VM'ler) yönetme olağanüstü durum kurtarma stratejiniz katkıda bulunur.

Bu öğretici bir olağanüstü durum kurtarma ayrıntıya ikincil şirket içi sitenize için Hyper-V sanal makineleri çalıştırmak nasıl gösterir. Hyper-V sanal makineleri bir System Center Virtual Machine Manager (VMM) özel bulutta yönetiliyor olması gerekir. Bir detaylandırma çoğaltma stratejinizi veri kaybı veya kapalı kalma süresi olmadan doğrular ve üretim ortamınızı etkilemeden etkilemez. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yük devretme sınaması için Önkoşullar hazırlama
> * Bir yük devretme sınaması için tek bir makineye çalıştırın


## <a name="prerequisites"></a>Ön koşullar

- İkincil sitede ağ seçeneklerini birkaç yük devretme testi çalıştırabilirsiniz. Mevcut bir ağ ile bir ağı olmadan yük devretme çalıştırmak veya Site Recovery otomatik olarak bir test ağı oluşturmak istiyorum. 
**Yük devretme sınaması için var olan bir üretim ağı kullanmak isterseniz:**:
    - Test yük devretme yapılırken birincil VM kapatılmalıdır. Aksi takdirde, aynı kimliğe sahip iki VM aynı ağ, aynı anda çalıştırırsınız. 
    - VM'ler test etmek için değişiklik yaparsanız, yük devretmeyi temiz değişiklikler kaybolur. Birincil VM değişikliklerini çoğaltılmadığından.
    - Bir üretim ağı test üretim iş yükleri için kapalı kalma süresi neden olur. Olağanüstü durum kurtarma ayrıntıya devam ederken ilgili uygulamalar kullanmamayı, kullanıcılarınızın isteyin. 
- Çoğaltma test ağı yük devretme sınaması için kullanılan VMM mantıksal ağ türü eşleşmesi gerekmez. Ancak, bazı birleşimleri çalışmıyor. Örneğin DHCP ve VLAN tabanlı yalıtım çoğaltma kullanıyorsa, test yük devretme ağı için IP adresi havuzları gerektiğinden Windows ağ sanallaştırma kullanamazsınız. 
- Yük devretme sınaması için Ağ eşlemesi için seçtiğiniz ağ kullanmamanızı öneririz.


## <a name="run-a-test-failover-for-a-vm"></a>Bir VM için yük devretme testi çalıştırma

1. İçinde **çoğaltılan öğeler**, VM tıklayın > **yük devretme testi**.
2. İçinde **sınama yük devretmesi**, test yük devretme sonrasında nasıl test sanal makineleri ağa bağlanacak belirtin. Bu öğreticinin amaçları doğrultusunda, Site Recovery otomatik olarak bir test ağı oluşturma izin vermenizi öneririz. [Daha fazla bilgi edinin](site-recovery-test-failover-vmm-to-vmm.md#network-options-in-site-recovery).
3. Yük devretmeyi başlatmak için **Tamam**'a tıklayın. İlerleme durumunu izlemek **işleri** sekmesi.
4. Yük devretme işlemi tamamlandıktan sonra test sanal makineleri başarıyla başlatıldığını doğrulayın.
5. İşiniz bittiğinde tıklatın **temizleme yük devretme testi**. Bu test sanal makineleri ve yük devretme testi sırasında oluşturulan herhangi bir ağa siler.
6. İçinde **notları**, kaydetme ve yük devretme testiyle ilişkili gözlemlerinizi kaydetmek. 


## <a name="next-steps"></a>Sonraki adımlar

[Bir üretim yük devretmeyi çalıştırma](tutorial-vmm-to-vmm-failover-failback.md)






