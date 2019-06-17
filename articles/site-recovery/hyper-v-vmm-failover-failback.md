---
title: Yük devretme ve ilk durumuna geri döndürme Hyper-V Vm'lerini Azure Site Recovery ile olağanüstü durum kurtarma sırasında bir ikincil veri merkezine çoğaltılmasını | Microsoft Docs
description: İçinde Hyper-V Vm'lerini ikincil şirket içi sitenize başarısız ve Azure Site Recovery ile olağanüstü durum kurtarma sırasında yeniden birincil siteye başarısız öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: raynew
ms.openlocfilehash: 39b2e4f37abe77439410fa4a83e06a0ca7941787
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66397987"
---
# <a name="fail-over-and-fail-back-hyper-v-vms-replicated-to-your-secondary-on-premises-site"></a>Yük devretme ve ilk durumuna geri döndürme, ikincil şirket içi siteye çoğaltılan Hyper-V Vm'leri

[Azure Site Recovery](site-recovery-overview.md) hizmeti yönetir ve çoğaltma, yük devretme ve şirket içi makinelerin ve Azure sanal makineleri (VM) yeniden çalışma işlemlerini düzenler.

Bu makalede, ikincil bir VMM sitesine bir System Center Virtual Machine Manager (VMM) bulutta yönetilen bir Hyper-V VM yük devretme açıklar. Yük devrettikten sonra şirket içi siteniz kullanılabilir olduğunda yeniden çalıştırabilirsiniz. Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Birincil VMM bulutuna ikincil bir VMM bulutu için bir Hyper-V VM yük devretme
> * İkincil siteden birincil siteye yeniden koruyun ve ilk duruma döndürme
> * İsteğe bağlı olarak birincil ikincil siteden yeniden çoğaltmaya başlayın

## <a name="failover-and-failback"></a>Yük devretme ve yeniden çalışma

Yük devretme ve yeniden çalışma, üç aşama vardır:

1. **İkincil siteye yük devretme**: Makineleri birincil siteden ikincil veritabanına yük devretme.
2. **İkincil siteden başarısız**: İkincil siteden Vm'lerini çoğaltma ve yeniden çalışma için planlanmış bir yük devretme çalıştırın.
3. Planlı yük devretme sonrasında isteğe bağlı olarak, birincil siteden ikincil yeniden çoğaltmaya başlayın.


## <a name="prerequisites"></a>Önkoşullar

- Tamamladığınızdan emin olun bir [olağanüstü durum kurtarma tatbikatı](hyper-v-vmm-test-failover.md) her şeyin beklendiği gibi çalıştığını denetlemek için.
- Yeniden çalışmayı tamamla için birincil ve ikincil VMM sunucuları için Site Recovery bağlı olduğunuzdan emin olun.



## <a name="run-a-failover-from-primary-to-secondary"></a>Yük devretme birincil ikincil siteden çalıştırma

Hyper-V Vm'leri için normal veya planlanan bir yük devretme çalıştırabilirsiniz.

- Normal bir yük devretme için beklenmeyen kesintiler kullanın. Bu yük devretme çalıştırdığınızda, Site Recovery ikincil sitede bir sanal makine oluşturur ve yukarı güçlendirir. Eşitlenmiş edilmemiş bekleyen veri bağlı olarak veri kaybı oluşabilir.
- Planlı yük devretme, bakım için veya beklenen bir kesinti sırasında kullanılabilir. Bu seçenek, sıfır veri kaybı sağlar. Planlı yük devretme tetiklendiğinde kaynak VM'ler kapatılır. Eşitlenmemiş veriler eşitlenir ve yük devretme tetiklenir. 
- 
  Bu yordamda, normal bir yük devretme çalıştırma açıklanmaktadır.


1. **Ayarlar** > **Çoğaltılan öğeler** bölümünde VM > **Yük devretme**’ye tıklayın.
1. Seçin **yük devretmeye başlamadan önce makineyi Kapat** Site Recovery, yük devretmeyi tetiklemeden önce kaynak sanal makineleri kapatmayı denemek istiyorsanız. Site Recovery, yük devretmeyi tetiklemeden önce ikincil siteye henüz gönderilmedi şirket içi verileri eşitlemek ayrıca çalışacaktır. Kapatma başarısız olsa bile, yük devretme devam edin. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.
2. Artık VM ikincil VMM bulutunda görmeye olmalıdır.
3. VM doğruladıktan sonra **işleme** yük devretme. Bu, tüm kullanılabilir kurtarma noktalarını siler.

> [!WARNING]
> **Devam eden bir yük devretme işlemini iptal etmeyin**: Yük devretme başlatılmadan önce VM çoğaltması durdurulur. Devam eden bir yük devretme işlemini iptal ederseniz yük devretme durdurulur, ancak VM yeniden çoğaltılmaz.  


## <a name="reverse-replicate-and-failover"></a>Ters çoğaltma ve yük devretme

İkincil siteden birincil siteye çoğaltmaya başlar ve birincil sitede yeniden çalıştırmak. Vm'leri birincil sitede yeniden çalıştırıldıktan sonra ikincil bir siteye çoğaltabilirsiniz.  

 
1. VM'ye tıklayın > tıklayarak **ters çoğaltma**.
2. İş tamamlandıktan sonra VM'ye tıklayın > içinde **yük devretme**, yük devretme yönü (buluttan ikincil VMM) doğrulayın ve kaynak ve hedef konumları seçin. 
4. Yük devretmeyi başlatın. Yük devretme işleminin ilerleme durumunu **İşler** sekmesinden takip edebilirsiniz.
5. Birincil VMM bulutundaki VM kullanılabilir olup olmadığını denetleyin.
6. İkincil sitenin birincil VM çoğaltma, yeniden başlatmak istiyorsanız, tıklayarak **ters çoğaltma**.

## <a name="next-steps"></a>Sonraki adımlar
[Adım gözden geçirme](hyper-v-vmm-disaster-recovery.md) Hyper-V Vm'lerini ikincil bir siteye çoğaltmak için.
