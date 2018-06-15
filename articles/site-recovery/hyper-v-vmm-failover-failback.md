---
title: Yük devri ve başarısız geri Hyper V sanal makinelerini Site Recovery ile ikincil veri merkezine çoğaltılmasını | Microsoft Docs
description: Hyper-V sanal makineleri üzerinde ikincil şirket içi sitenize başarısız ve Azure Site Recovery ile birincil siteye geri başarısız öğrenin
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 05/02/2018
ms.author: raynew
ms.openlocfilehash: ecb0b9395ce7071442ddf0dd976e1ca57b8be906
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34161150"
---
# <a name="fail-over-and-fail-back-hyper-v-vms-replicated-to-your-secondary-on-premises-site"></a>Yük devri ve başarısız ikincil şirket içi siteye çoğaltılan Hyper-V Vm'lerini yedekleme

[Azure Site Recovery](site-recovery-overview.md) hizmet yöneten ve çoğaltma, yük devretme ve yeniden çalışma şirket içi makineler ve Azure sanal makineleri (VM'ler) yönetir.

Bu makalede, bir ikincil VMM sitesi için bir System Center Virtual Machine Manager (VMM) bulutta yönetilen bir Hyper-V VM yük devri açıklar. Yük devrettikten sonra şirket içi siteniz kullanılabilir olduğunda yeniden çalıştırabilirsiniz. Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Birincil VMM Bulutu bir Hyper-V VM ikincil VMM Bulutu yük devri
> * Birincil ikincil siteden koruyun ve geri başarısız
> * İsteğe bağlı olarak birincil ikincil yeniden çoğaltma işlemi başlatma

## <a name="failover-and-failback"></a>Yük devretme ve yeniden çalışma

Yük devretme ve yeniden çalışma üç aşama vardır:

1. **İkincil siteye yük devri**: başarısız makineler üzerinden birincil siteden ikincil.
2. **İkincil site veritabanından başarısız**: birincil ikincil çoğaltma VM'lerin ve Çalıştır planlanmış bir yük devretme geri dönecek.
3. Planlı yük devretme sonrasında isteğe bağlı olarak birincil siteden ikincil yeniden çoğaltma işlemi başlatma.


## <a name="prerequisites"></a>Önkoşullar

- Tamamlandı emin olun bir [olağanüstü durum kurtarma ayrıntıya](hyper-v-vmm-test-failover.md) her şeyin beklendiği gibi çalıştığını denetlemek için.
- Yeniden çalışma tamamlamak için birincil ve ikincil VMM sunucuları için Site Recovery bağlı olduğunuzdan emin olun.



## <a name="run-a-failover-from-primary-to-secondary"></a>Yük devretme birincil ikincil çalıştırma

Hyper-V VM'ler için normal veya planlı bir yük devretme çalıştırabilirsiniz.

- Normal bir yük devretme için beklenmedik kesintileri kullanın. Bu yük devretme çalıştırdığınızda, Site kurtarma ikincil sitede bir VM oluşturur ve yukarı çalıştırır. Bekleyen eşitlenmiş kurmadı veri bağlı olarak veri kaybı oluşabilir.
- Planlanmış bir yük devretme sırasında beklenen kesinti veya bakım için kullanılabilir. Bu seçenek, sıfır veri kaybı sağlar. Planlanmış bir yük devretme tetiklendiğinde, kaynak sanal makineleri kapatılır. Eşitlenmemiş veriler eşitlenir ve yük devretme tetiklenir. 
- 
Bu yordamda, normal bir yük devretmeyi çalıştırma açıklanmaktadır.


1. **Ayarlar** > **Çoğaltılan öğeler** bölümünde VM > **Yük devretme**’ye tıklayın.
1. Seçin **yük devretme işlemine başlamadan önce makineyi kapatın** yük devretme tetiklemeden önce kaynak VM'lerin bir kapatma yapma girişiminde Site Recovery istiyorsanız. Site Recovery, aynı zamanda yük devretme tetiklemeden önce ikincil siteye henüz gönderilmedi şirket içi verileri eşitlemek deneyecek. Kapatma başarısız olsa bile, yük devretme devam unutmayın. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.
2. Şimdi ikincil VMM bulutta VM görüyor olmalısınız.
3. VM doğruladıktan sonra **yürütme** yük devretme. Bu, tüm kullanılabilir kurtarma noktalarını siler.

> [!WARNING]
> **Devam eden yük devretme işlemini iptal etmeyin**: Yük devretme başlatılmadan önce VM çoğaltma durdurulur. Devam eden bir yük devretme işlemini iptal ederseniz yük devretme durdurulur, ancak VM yeniden çoğaltılmaz.  


## <a name="reverse-replicate-and-failover"></a>Ters çoğaltmak ve yük devretme

Birincil ikincil sitedeki çoğaltma işlemi başlatma ve birincil sitenin başarısız. Sanal makineleri yeniden birincil sitede çalıştırmayı sonra ikincil siteye çoğaltabilirsiniz.  

 
1. VM tıklayın > tıklayın **ters çoğaltma**.
2. İş tamamlandıktan sonra VM tıklayın > içinde **yük devretme**, yük devretme yönünden (ikincil VMM Bulutu) doğrulayın ve kaynak ve hedef konumları seçin. 
4. Yük devretme başlatın. Yük devretme işleminin ilerleyişini izleyin **işleri** sekmesi.
5. Birincil VMM bulutta VM kullanılabilir olup olmadığını denetleyin.
6. Birincil VM çoğaltma ikincil sitenin yeniden başlatmak istiyorsanız, tıklayın **ters çoğaltma**.

## <a name="next-steps"></a>Sonraki adımlar
[Adım gözden geçirme](hyper-v-vmm-disaster-recovery.md) Hyper-V Vm'lerini ikincil bir siteye çoğaltmak için.
