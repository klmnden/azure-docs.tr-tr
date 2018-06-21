---
title: Yük devri ve başarısız geri Hyper-V sanal makinelerini Azure Site Recovery ile çoğaltılan | Microsoft Docs
description: Hyper-V Vm'lerini azure'a yük devri ve Azure Site Recovery ile şirket içi siteye geri başarısız hakkında bilgi edinin
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 06/20/2018
ms.author: raynew
ms.openlocfilehash: 6a34187a87c6ecda461357a1c2fc8747ddf4b056
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36294301"
---
# <a name="failover-and-failback-hyper-v-vms-replicated-to-azure"></a>Yük devretme ve yeniden çalışma Hyper-V Azure'a kopyalanan VM

Bu öğretici açıklayan bir Hyper-V VM azure'a yük devretme nasıl. Kullanılabilir olduğunda, yeniden çalışma üzerinden, şirket içi sitenize başarısız sonra. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Hyper-V VM özelliklerini denetlemek için Azure gereksinimlerine uygun doğrulayın
> * Azure'a yük devretme işlemini çalıştırma
> * Azure'dan şirket içi
> * Ters çoğaltmak Azure'a çoğaltma yeniden başlatmak için sanal makineleri, şirket içi

Bu öğretici serisinde beşinci öğreticidir. Bunu zaten önceki eğitimlerine görevleri tamamladığınızdan emin varsayar.    

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
2. [Şirket içi Hyper-V’leri hazırlama](tutorial-prepare-on-premises-hyper-v.md)
3. Olağanüstü durum kurtarma için ayarlama [Hyper-V sanal makineleri](tutorial-hyper-v-to-azure.md), veya [System Center VMM bulutlarında yönetilen Hyper-V Vm'lerini](tutorial-hyper-v-vmm-to-azure.md)
4. [Olağanüstü durum kurtarma tatbikatı çalıştırma](tutorial-dr-drill-azure.md)

## <a name="prepare-for-failover-and-failback"></a>Yük devretme ve yeniden çalışma için hazırlama

VM anlık görüntü yok vardır ve şirket içi VM yeniden çalışma sırasında kapalı olduğundan emin olun. Bu, çoğaltma sırasında veri tutarlılığını sağlamaya yardımcı olur. Şirket içi sanal makine yeniden çalışma sırasında kapatmayın. 

Yük devretme ve yeniden çalışma üç aşama vardır:

1. **Azure'a yük devretme**: şirket içi siteden yük devretme Hyper-V Vm'lerini azure'a.
2. **Şirket içi yeniden çalışma**: yük devretme Azure VM'ler kullanılabilir olduğunda, şirket içi sitenize. Bu sanal makineleri şirket içi Hyper-V Vm'lerini geri çoğaltmaya başlar. İlk veri eşitlemeyi sonra Yük devretme Azure VM'ler şirket içi siteye.  
3. **Ters çoğaltmak şirket içi sanal makineleri**: veri geri başarısız oldu sonra ters çoğaltma Azure'a çoğaltma başlatmak için şirket içi sanal makineleri.

## <a name="verify-vm-properties"></a>VM özelliklerini doğrulama

Yük devretme önce VM özelliklerini doğrulayın ve VM karşılamasına ile dikkat edin [Azure gereksinimleri](hyper-v-azure-support-matrix.md#replicated-vms).

1. İçinde **korunan öğeler**, tıklatın **çoğaltılan öğeler** >< VM-adı >.

2. İçinde **yinelenmiş öğesi** bölmesinde, VM bilgileri, sistem durumu ve en son kullanılabilir kurtarma noktalarını gözden geçirin. Daha fazla ayrıntı görüntülemek için **Özellikler**’e tıklayın.
     - İçinde **işlem ve ağ**VM ayarlarını değiştirebilir ve ağ ayarlarını, ağ/Azure VM yük devretme sonrasında bulunur ve IP adresi alt ağı içeren kendisine atanacaktır. Yönetilen diskleri azure'dan Hyper-v için desteklenmez.
      - **Diskler** bölümünde VM’deki işletim sistemi ve veri diskleriyle ilgili bilgileri görüntüleyebilirsiniz.

## <a name="failover-to-azure"></a>Azure'a yük devretme

1. **Ayarlar** > **Çoğaltılan öğeler** bölümünde VM > **Yük devretme**’ye tıklayın.
2. İçinde **yük devretme** seçin **son** kurtarma noktası. 
3. **Yük devretmeyi başlatmadan önce makineyi kapatın** seçeneğini belirleyin. Yük devretme tetiklemeden önce kaynak VM'lerin bir kapatma yapmak Site Recovery çalışır. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.
4. Yük devretme tıklatın doğruladıktan sonra **yürütme**. Tüm kullanılabilir kurtarma noktalarını siler.

> [!WARNING]
> **Bir yük devretme devam ediyor iptal etme**: sürüyor, yük devretme durduruyor, iptal ancak VM yeniden çoğaltma olmaz.

## <a name="failback-azure-vm-to-on-premises-and-reverse-replicate-the-on-premises-vm"></a>Yeniden çalışma Azure VM şirket içi ve geriye doğru çoğaltma şirket içi VM

Yeniden çalışma temel bir yük devretme azure'dan şirket içi siteye ve şirket içi siteden Vm'lerini Azure'a çoğaltma yeniden başlar ters çoğaltmak işlemdir.

1. İçinde **ayarları** > **öğeleri çoğaltılan**, VM tıklayın > **planlı yük devretme**.
2. İçinde **planlı yük devretme onaylayın**, yük devretme yönünden (Azure) doğrulayın ve kaynak ve hedef konumları seçin.
3. Seçin **yük devretme önce verileri eşitle (yalnızca delta değişiklikler eşitleme)**. Bu seçenek VM kapatmadan eşitlediğinden VM kapalı kalma süresi en aza indirir.
4. Yük devretme başlatın. Yük devretme işleminin ilerleyişini izleyin **işleri** sekmesi.
5. Sonra ilk veri eşitlemesi gerçekleştirilir ve Azure VM'ler tıklatın kapatmaya hazır **işleri** > Planlı yük devretme-proje-adı > **yük devretmeyi tamamlamak**. Azure VM kapatan, en son değişiklikleri şirket içi aktarır ve şirket içi VM başlatır.
6. Beklendiği gibi kullanılabilir denetlemek için şirket içi VM oturum açın.
7. Şirket içi VM olduğunu bir **Commit bekleyen** durumu. Tıklatın **yürütme**. Azure sanal makinelerini ve onun diskleri siler ve şirket içi VM çoğaltmayı tersine çevirme için hazırlar.
Şirket içi VM Azure'a çoğaltma başlatmak için etkinleştirme **ters çoğaltma**. Azure VM değiştirildi devre dışı olduğundan oluşan delta değişiklikleri çoğaltmasını tetikler.  
