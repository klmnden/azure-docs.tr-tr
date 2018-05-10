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
ms.openlocfilehash: f4207b8def3a5cd240b7a3ecdffde34a27f2a833
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
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

- Normal bir yük devretme için beklenmedik kesintileri kullanın. Bu yük devretme çalıştırdığınızda, Site kurtarma ikincil sitede bir VM oluşturur ve yukarı çalıştırır. Belirli bir kurtarma noktası karşı yük devretme çalıştırın. Kullandığınız kurtarma noktası bağlı olarak veri kaybı oluşabilir.
- Planlanmış bir yük devretme sırasında beklenen kesinti veya bakım için kullanılabilir. Bu seçenek, sıfır veri kaybı sağlar. Planlanmış bir yük devretme tetiklendiğinde, kaynak sanal makineleri kapatılır. Eşitlenmemiş veriler eşitlenir ve yük devretme tetiklenir. 
- 
Bu yordamda, normal bir yük devretmeyi çalıştırma açıklanmaktadır.


1. **Ayarlar** > **Çoğaltılan öğeler** bölümünde VM > **Yük devretme**’ye tıklayın.
2. **Yük devretme**’de yük devretmenin yapılacağı bir **Kurtarma Noktası** seçin. Aşağıdaki seçeneklerden birini kullanabilirsiniz:
    - **En son** (varsayılan): Bu seçenekte öncelikle Site Recovery’ye gönderilen tüm veriler işlenir. Yük devretme yük devretme tetiklendiğinde Site Recovery çoğaltılan tüm verilerini sahip olduktan sonra çoğaltma VM'de oluşturduğundan düşük RPO (kurtarma noktası hedefi) sağlar.
    - **En son işlenen**: Bu seçenekte VM yükü, Site Recovery tarafından işlenen en son kurtarma noktasına devredilir. İşlenmemiş verileri işlemek için zaman harcanmadığından bu seçenekte düşük bir RTO (Kurtarma Süresi Hedefi) sağlanır.
    - **En son uygulamayla tutarlı**: Bu seçenekte VM yükü, Site Recovery tarafından işlenen, uygulamayla tutarlı en son kurtarma noktasına devredilir. 
3. Şifreleme anahtarı bu senaryoyla ilgili değildir.
4. Seçin **yük devretme işlemine başlamadan önce makineyi kapatın** yük devretme tetiklemeden önce kaynak VM'lerin bir kapatma yapma girişiminde Site Recovery istiyorsanız. Site Recovery, aynı zamanda yük devretme tetiklemeden önce ikincil siteye henüz gönderilmedi şirket içi verileri eşitlemek deneyecek. Kapatma başarısız olsa bile, yük devretme devam unutmayın. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.
5. Şimdi ikincil VMM bulutta VM görüyor olmalısınız.
6. VM doğruladıktan sonra **yürütme** yük devretme. Bu, tüm kullanılabilir kurtarma noktalarını siler.

> [!WARNING]
> **Devam eden yük devretme işlemini iptal etmeyin**: Yük devretme başlatılmadan önce VM çoğaltma durdurulur. Devam eden bir yük devretme işlemini iptal ederseniz yük devretme durdurulur, ancak VM yeniden çoğaltılmaz.  


## <a name="reprotect-and-fail-back"></a>Koruyun ve geri başarısız

Birincil ikincil sitedeki çoğaltma işlemi başlatma ve birincil sitenin başarısız. Sanal makineleri yeniden birincil sitede çalıştırmayı sonra bunları ikincil siteye yeniden çoğaltabilirsiniz.  

1. İçinde **ayarları** > **öğeleri çoğaltılan** VM tıklayın ve etkinleştirme **ters çoğaltma**. VM, birincil sitenin çoğaltmaya başlar.
2. VM tıklayın > **planlanan yük devretme**.
3. İçinde **planlı yük devretme onaylayın**, yük devretme yönünden (ikincil VMM Bulutu) doğrulayın ve kaynak ve hedef konumları seçin. 
4. İçinde **veri eşitleme**, nasıl eşitlemek istediğinizi belirtin:
    - **Yük devretme önce verileri eşitle (yalnızca delta değişiklikler eşitleme)**— VM kapatmadan eşitlediğinden bu seçeneği VM kapalı kalma süresini en aza indirir. İşte ne yapar:
        - VM çoğaltmasının anlık görüntüsünü alır ve birincil Hyper-V konağına kopyalar. Çoğaltma VM çalıştıran tutar.
        - Yeni değişiklik yok gerçekleşmesi VM, çoğaltma kapatır. Delta değişikliklerin son kümesi, birincil siteye aktarılır ve birincil sitedeki VM başlatılır.
    - **Verileri (tam yükleme) yalnızca yük devretme sırasında eşitleme**— ikincil sitede uzun süredir çalışan, bu seçeneği kullanın. Biz birden çok disk değişiklikler beklediğiniz ve sağlama toplamı hesaplamaları süre beklemesini yoktur çünkü bu seçenek daha hızlıdır. Bu seçenek, bir disk yükleme gerçekleştirir. Birincil VM silindiğinde de yararlıdır.
5. Şifreleme anahtarı bu senaryoyla ilgili değildir.
6. Yük devretme başlatın. Yük devretme işleminin ilerleyişini izleyin **işleri** sekmesi.
7. Yük devretme önce verileri eşitlemek için seçtiyseniz, ilk veri eşitlemeyi yapılır ve VM ikincil sitedeki çoğaltma kapatmak hazır sonra **işleri** > Planlı yük devretme iş adı >  **Yük devretme tamamlayın**. Bu ikincil VM kapatan, en son değişiklikleri birincil siteye aktarır ve birincil VM başlatır.
8. Birincil VMM bulutta VM kullanılabilir olup olmadığını denetleyin.
9. Birincil VM yürütme bekleme durumunda sunulmuştur. Tıklatın **tamamlama**, yük devretme kaydedilemedi.
10. Birincil VM ikincil sitenin yeniden çoğaltma işlemi başlatma istiyorsanız etkinleştirmek **ters çoğaltma**.


> [!NOTE]
> Çoğaltmayı tersine çevirme yalnızca VM kapalı ve yalnızca delta değişiklikler gönderilen çoğaltma bu yana oluşan değişiklikleri çoğaltır.

## <a name="next-steps"></a>Sonraki adımlar
[Adım gözden geçirme](hyper-v-vmm-disaster-recovery.md) Hyper-V Vm'lerini ikincil bir siteye çoğaltmak için.
