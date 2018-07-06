---
title: Yük devretme ve ilk durumuna geri döndürme Hyper-V Site Recovery ile azure'a çoğaltılan VM'ler | Microsoft Docs
description: Hyper-V Vm'lerini azure'a yük devretme ve Azure Site Recovery ile şirket içi siteye geri başarısız hakkında bilgi edinin
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 06/20/2018
ms.author: raynew
ms.openlocfilehash: b87ec5144154714dfc8f9f2d5a06c158bc956a2d
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37858546"
---
# <a name="failover-and-failback-hyper-v-vms-replicated-to-azure"></a>Yük devretme ve yeniden çalışma Hyper-V Vm'lerini Azure'a çoğaltılır

Bu öğreticide açıklanır azure'a Hyper-V VM yük devretme. Kullanılabilir olduğunda, yeniden çalışma, üzerinden şirket içi sitenize devrettikten sonra. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Hyper-V VM özelliklerini denetlemek için Azure gereksinimleriyle uyumlu doğrulayın
> * Azure'a yük devretme işlemini çalıştırma
> * Azure'dan şirket içine
> * Ters çoğaltma VM tekrar Azure'a çoğaltmaya başlamak için şirket içinde

Bu öğreticide, serideki beşinci öğreticidir. Bu önceki öğreticilerdeki görevleri zaten tamamladığınız varsayılır.    

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
2. [Şirket içi Hyper-V’leri hazırlama](tutorial-prepare-on-premises-hyper-v.md)
3. İçin olağanüstü durum kurtarma ayarlama [Hyper-V Vm'lerini](tutorial-hyper-v-to-azure.md), veya [System Center VMM bulutlarında yönetilen Hyper-V Vm'leri](tutorial-hyper-v-vmm-to-azure.md)
4. [Olağanüstü durum kurtarma tatbikatı çalıştırma](tutorial-dr-drill-azure.md)

## <a name="prepare-for-failover-and-failback"></a>Yük devretme ve yeniden çalışma için hazırlama

VM'de anlık görüntü yok vardır ve şirket içi VM yeniden çalışma sırasında kapalı olduğundan emin olun. Bu, çoğaltma sırasında veri tutarlılığını sağlamak yardımcı olur. Yeniden çalışma sırasında şirket içi VM'yi açmayın. 

Yük devretme ve yeniden çalışma üç aşama vardır:

1. **Azure'a yük devretme**: şirket içi siteden azure'a yük devretme Hyper-V Vm'leri.
2. **Şirket içine yeniden çalışma**: şirket içi siteniz kullanılabilir olduğunda, şirket içi sitenize yük devretme Azure Vm'leri. Veri azure'dan şirket içine ve tamamlandığında eşitleme başlar, bunu şirket içi Vm'lerde getirir.  
3. **Ters çoğaltma şirket içi Vm'leri**: yeniden şirket içine başarısız sonra bunları Azure'a çoğaltmaya başlamak için şirket içi Vm'leri ters çoğaltma.

## <a name="verify-vm-properties"></a>VM özelliklerini doğrulama

Yük devretmeden önce VM özelliklerini doğrulayın ve VM ile karşıladığından emin olun [Azure gereksinimleri](hyper-v-azure-support-matrix.md#replicated-vms).

**Korunan Öğeler**’de **Çoğaltılan Öğeler** > VM seçeneğine tıklayın.

2. **Çoğaltılan öğe** bölmesinde VM bilgileri ile sistem durumunun bir özeti ve kullanılabilen son kurtarma noktaları yer alır. Daha fazla ayrıntı görüntülemek için **Özellikler**’e tıklayın.

3. **İşlem ve Ağ** bölümünde Azure adını, kaynak grubunu, hedef boyutunu, [kullanılabilirlik kümesini](../virtual-machines/windows/tutorial-availability-sets.md) ve yönetilen disk ayarlarını değiştirebilirsiniz.

4. Ağ ayalarını, yük devretmeden sonra Azure VM’nin yerleştirileceği ağ/alt ağ ve VM’ye atanacak IP adresi dahil olmak üzere görüntüleyebilir ve değiştirebilirsiniz.

5. **Diskler** bölümünde VM’deki işletim sistemi ve veri diskleriyle ilgili bilgileri görüntüleyebilirsiniz.

## <a name="failover-to-azure"></a>Azure'a yük devretme

1. İçinde **ayarları** > **çoğaltılan öğeler**, VM'ye tıklayın > **yük devretme**.
2. İçinde **yük devretme**seçin **son** kurtarma noktası. 
3. **Yük devretmeyi başlatmadan önce makineyi kapatın** seçeneğini belirleyin. Site Recovery, yük devretmeyi tetiklemeden önce kaynak sanal makineleri kapatmayı dener. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.
4. Yük devretme doğruladıktan sonra tıklayın **işleme**. Tüm kullanılabilir kurtarma noktalarını siler.

> [!WARNING]
> **Devam eden bir yük devretme işlemini iptal etmeyin**:, iptal ediyor, yük devretme durdurulur, ancak VM yeniden çoğaltılmaz.

## <a name="failback-azure-vm-to-on-premises-and-reverse-replicate-the-on-premises-vm"></a>Yeniden çalışma Azure VM'yi şirket içinde hem de ters için şirket içi VM çoğaltma

Azure'dan yük devretme ters çoğaltma Vm'leri şirket içi siteden Azure'a çoğaltılması yeniden başlatır ve şirket içi siteye yeniden çalışma işlemi temel var.

1. İçinde **ayarları** > **çoğaltılan öğeler**, VM'ye tıklayın > **planlı yük devretme**.
2. İçinde **planlı yük devretmeyi Onayla**, yük devretme yönü (azure'dan) doğrulayın ve kaynak ve hedef konumları seçin.
3. Seçin **yük devretmeden önce veri eşitleme (yalnızca delta değişiklikler eşitleme)**. Bu seçenek VM'yi kapatmadan eşitlediğinden VM kapalı kalma süresini en aza indirir.
4. Yük devretme başlatın. Yük devretme işleminin ilerleyişini izleyin **işleri** sekmesi.
5. Sonra ilk veri eşitlemesi gerçekleştirilir ve Azure Vm'leri öğesini kapatmaya hazır **işleri** > Planlı yük devretme-işlem-adı > **yük devretmeyi tamamlamak**. Azure VM'yi kapatır, en son değişiklikleri şirket içi aktarır ve şirket içi VM'yi başlatır.
6. Şirket içi VM beklendiği gibi kullanılabilir kontrol etmek için oturum açın.
7. Şirket içi VM artık bulunduğu bir **işleme bekleyen** durumu. Tıklayın **işleme**. Azure Vm'leri ve disklerine siler ve şirket içi VM için çoğaltmayı tersine çevirme hazırlar.
Şirket içi sanal Makineyi Azure'a çoğaltmaya başlamak için Etkinleştir **ters çoğaltma**. Azure VM değiştirildi devre dışı olduğundan, ortaya çıkan delta değişikliklerinin tetikler.  
