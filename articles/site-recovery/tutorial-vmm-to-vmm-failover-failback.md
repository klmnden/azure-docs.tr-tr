---
title: "Yük devri ve başarısız geri Hyper V sanal makinelerini Site Recovery ile ikincil veri merkezine çoğaltılmasını | Microsoft Docs"
description: "Hyper-V sanal makineleri üzerinde ikincil şirket içi sitenize başarısız ve Azure Site Recovery ile birincil siteye geri başarısız öğrenin"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 44a662fa-2e7a-4996-86df-fdd6d6f5dedf
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/16/2017
ms.author: raynew
ms.openlocfilehash: 8f139070de99c4249207d048d445e86dd41e9060
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="fail-over-and-fail-back-hyper-v-vms-replicated-to-your-secondary-on-premises-site"></a>Yük devri ve başarısız ikincil şirket içi siteye çoğaltılan Hyper-V Vm'lerini yedekleme

[Azure Site Recovery](site-recovery-overview.md) hizmet yöneten ve çoğaltma, yük devretme ve yeniden çalışma şirket içi makineler ve Azure sanal makineleri (VM'ler) yönetir.

Bu öğretici, bir ikincil VMM sitesi için bir System Center Virtual Machine Manager (VMM) bulutta yönetilen bir Hyper-V VM yük devri açıklar. Devredilir sonra kullanılabilir olduğunda, şirket içi sitenize başarısız. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Birincil VMM Bulutu bir Hyper-V VM ikincil VMM Bulutu yük devri
> * Birincil ikincil siteden koruyun ve geri başarısız
> * İsteğe bağlı olarak birincil ikincil yeniden çoğaltma işlemi başlatma

## <a name="overview"></a>Genel Bakış

Yük devretme ve yeniden çalışma üç aşama vardır:

1. **İkincil siteye yük devri**: başarısız makineler üzerinden birincil siteden ikincil.
2. **İkincil site veritabanından başarısız**: birincil ikincil çoğaltma VM'lerin ve Çalıştır planlanmış bir yük devretme geri dönecek.
3. Planlı yük devretme sonrasında isteğe bağlı olarak birincil siteden ikincil yeniden çoğaltma işlemi başlatma.


## <a name="fail-over-to-a-secondary-site"></a>İkincil bir siteye yük devri

### <a name="failover-prerequisites"></a>Yük devretme önkoşulları

Tamamlandı emin olun bir [olağanüstü durum kurtarma ayrıntıya](tutorial-dr-drill-secondary.md) her şeyin beklendiği gibi çalıştığını denetlemek için.


### <a name="run-a-failover-from-primary-to-secondary"></a>Yük devretme birincil ikincil çalıştırma

Hyper-V VM'ler için normal veya planlı bir yük devretme çalıştırabilirsiniz.

- Normal bir yük devretme için beklenmedik kesintileri kullanın. Bu yük devretme çalıştırdığınızda, Site kurtarma ikincil sitede bir VM oluşturur ve yukarı çalıştırır. Belirli bir kurtarma noktası karşı yük devretme çalıştırın. Kullandığınız kurtarma noktası bağlı olarak veri kaybı oluşabilir.
- Planlanmış bir yük devretme sırasında beklenen kesinti veya bakım için kullanılabilir. Bu seçenek, sıfır veri kaybı sağlar. Planlanmış bir yük devretme tetiklendiğinde, kaynak sanal makineleri kapatılır. Eşitlenmemiş veriler eşitlenir ve yük devretme tetiklenir. 
- 
Bu yordamda, normal bir yük devretmeyi çalıştırma açıklanmaktadır.


1. İçinde **ayarları** > **öğeleri çoğaltılan** VM tıklayın > **yük devretme**.
2. İçinde **yük devretme** seçin bir **kurtarma noktası** devretmesini. Aşağıdaki seçeneklerden birini kullanabilirsiniz:
    - **En son** (varsayılan): Bu seçenek ilk Site Recovery hizmetine gönderilen tüm veriler işler. Yük devretme yük devretme tetiklendiğinde Site Recovery çoğaltılan tüm verilerini sahip olduktan sonra çoğaltma VM'de oluşturduğundan düşük RPO (kurtarma noktası hedefi) sağlar.
    - **En son işlenen**: Bu seçenek, Site Recovery tarafından işlenen en son kurtarma noktası VM'ye yöneltilir. Bu seçenek, hiçbir zaman işlenmemiş verileri işlerken harcanan çünkü düşük RTO (Kurtarma süresi hedefi) sağlar.
    - **Son uygulama tutarlı**: Bu seçenek, Site Recovery tarafından işlenen son uygulamayla tutarlı kurtarma noktası VM'ye yöneltilir. 
3. Şifreleme anahtarı bu senaryoyla ilgili değildir.
4. Seçin **yük devretme işlemine başlamadan önce makineyi kapatın** yük devretme tetiklemeden önce kaynak VM'lerin bir kapatma yapma girişiminde Site Recovery istiyorsanız. Site Recovery, aynı zamanda yük devretme tetiklemeden önce ikincil siteye henüz gönderilmedi şirket içi verileri eşitlemek deneyecek. Kapatma başarısız olsa bile, yük devretme devam unutmayın. Yük devretme işleminin ilerleyişini izleyin **işleri** sayfası.
5. Şimdi ikincil VMM bulutta VM görüyor olmalısınız.
6. VM doğruladıktan sonra **yürütme** yük devretme. Bu, tüm kullanılabilir kurtarma noktalarını siler.

> [!WARNING]
> **Bir yük devretme devam ediyor iptal etme**: yük devretme başlatılmadan önce VM çoğaltma durduruldu. Bir yük devretme devam ediyor, yük devretme durduruyor, iptal ancak VM yeniden çoğaltma olmaz  


## <a name="reprotect-and-fail-back-from-secondary-to-primary"></a>Koruyun ve geri gelen birincil ikincil başarısız

### <a name="prerequisites-for-failback"></a>Yeniden çalışma için Önkoşullar

Yeniden çalışma tamamlamak için birincil ve ikincil VMM sunucuları için Site Recovery bağlı olduğunuzdan emin olun.


### <a name="reprotect-and-fail-back"></a>Koruyun ve geri başarısız

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

