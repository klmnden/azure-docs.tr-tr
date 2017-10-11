---
title: "Azure Site Recovery ile ikincil site için bir yük devretme sınaması için Hyper-V VM çoğaltma çalıştırın | Microsoft Docs"
description: "Azure Site Recovery ile ikincil System Center VMM siteye bir yük devretme sınaması için Hyper-V VM çoğaltma çalıştırılacağını açıklar."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a3fd11ce-65a1-4308-8435-45cf954353ef
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 23d235d326273e7ec59feee6588a39f685401e52
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="step-10-run-a-test-failover-for-hyper-v-replication-to-a-secondary-site"></a>10. adım: yük devretme sınaması Hyper-V çoğaltma için ikincil bir siteye çalıştırma


Hyper-V sanal makineleri (VM'ler) için çoğaltma ile etkinleştirdikten sonra [Azure Site Recovery](site-recovery-overview.md), yük devretme testi çalıştırmak için bu makaleyi kullanın. Yük devretme testi çoğaltmanın üretim ortamınızı etkilemeden düzgün çalıştığını doğrular. 


Bu makaleyi okuduktan sonra yapmak istediğiniz tüm yorumları makalenin alt kısmında veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda paylaşabilirsiniz.


## <a name="before-you-start"></a>Başlamadan önce

- Yük devretme testi tetiklendiğinde test çoğaltma sanal makineleri bağlanacağı ağ belirtebilirsiniz. [Daha fazla bilgi edinin](site-recovery-test-failover-vmm-to-vmm.md#network-options-in-site-recovery) ağ seçenekleri hakkında.
- Ağ eşleme sırasında seçtiğiniz bir ağ seçmeyin öneririz.
- Bu makaledeki yönergeleri, tek bir VM vermesine açıklar. Hakkında bilgi edinin [kurtarma planları oluşturma](site-recovery-create-recovery-plans.md) birden çok VM birlikte başarısız istiyorsanız.
- Hakkında bilgi edinin [yük devretme sınaması işlemlerini sınırlamalar](site-recovery-test-failover-vmm-to-vmm.md#things-to-note).
- Bir VM'ye yük devretme testi sırasında verilen IP adresi, VM alacağı aynı IP adresidir (IP adresi test yük devretme ağda kullanılabilir olduğunu pek fazla) planlanmış veya planlanmamış bir yük devretme için. IP adresi test yük devretme ağda kullanılabilir değilse, VM test yük devretme ağı testinde kullanılabilirse başka bir IP adresi alır.
- Yük devretme sınaması için uçtan uca ağ bağlantısı makinenin tam validatation, üretim ağınıza yapmak istiyorsanız, dikkat edin:
    - Test yük devretme yapılırken birincil VM kapatılmalıdır. Aksi takdirde, aynı kimliğe sahip iki VM aynı ağda birlikte aynı anda çalıştırırsınız. 
    - Yük devretmeyi temiz olduğunda VM'ler test etmek için değişiklikler yapmak isterseniz, bu değişiklikler kaybolur. Bu değişiklikler birincil VM çoğaltılmadığından.
    - Testleri bir üretim ağı üretim iş yükleri için kapalı kalma süresi yol açar. Olağanüstü durum kurtarma ayrıntıya devam ederken uygulama kullanmamayı kullanıcılar isteyin.  


## <a name="run-a-test-failover-for-a-vm"></a>Bir VM için yük devretme testi çalıştırma

1. Tek bir VM'ye yük devretme için **çoğaltılan öğeler**, VM tıklayın > **yük devretme testi**.
2. İçinde **sınama yük devretmesi**, test yük devretme sonrasında nasıl test sanal makineleri ağa bağlanacak belirtin. 
3. Yük devretmeyi başlatmak için **Tamam**'a tıklayın. İlerleme durumunu izlemek **işleri** sekmesi.
5. Yük devretme işlemi tamamlandıktan sonra test sanal makineleri başarıyla başlatıldığını doğrulayın.
6. İşiniz bittiğinde tıklatın **temizleme yük devretme testi** kurtarma planı üzerinde.
7. İçinde **notları**, kaydetme ve yük devretme testiyle ilişkili gözlemlerinizi kaydetmek. Bu adım, yük devretme testi sırasında oluşturulan ağlar ve sanal makinelere siler.


## <a name="next-steps"></a>Sonraki adımlar

Dağıtımı test ettikten sonra diğer türleri hakkında daha fazla bilgi [yük devretme](site-recovery-failover.md).
