---
title: "Yük devri ve başarısız geri Hyper-V sanal makinelerini Azure Site Recovery ile çoğaltılan | Microsoft Docs"
description: "Hyper-V Vm'lerini azure'a yük devri ve Azure Site Recovery ile şirket içi siteye geri başarısız hakkında bilgi edinin"
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 03/8/2018
ms.author: raynew
ms.openlocfilehash: 7863feb29fbb04f643aa3b7e1984209f44cdbe9a
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="fail-over-and-fail-back-hyper-v-vms-replicated-to-azure"></a>Yük devri ve başarısız geri Hyper-V Azure'a kopyalanan VM

Bu öğretici, Azure'da bir Hyper-V VM yük devri açıklar. Yük devrettikten sonra şirket içi siteniz kullanılabilir olduğunda yeniden çalıştırabilirsiniz. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Hyper-V VM özelliklerini denetlemek için Azure gereksinimlerine uygun doğrulayın
> * Azure'a yük devretme işlemini çalıştırma
> * Azure VM’lerini şirket içi sitede yeniden koruma
> * Azure'dan şirket içi başarısız
> * Tekrar Azure’a çoğaltmaya başlamak için şirket içi VM’leri yeniden koruma

Bu, serideki beşinci öğreticidir. Bu öğreticide, önceki öğreticilerdeki adımları zaten tamamladığınız varsayılır.

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
2. [Şirket içi VMware’leri hazırlama](tutorial-prepare-on-premises-hyper-v.md)
3. Olağanüstü durum kurtarma için ayarlama [Hyper-V sanal makineleri](tutorial-hyper-v-to-azure.md), veya [System Center VMM bulutlarında yönetilen Hyper-V Vm'lerini](tutorial-hyper-v-vmm-to-azure.md)
4. [Olağanüstü durum kurtarma tatbikatı çalıştırma](tutorial-dr-drill-azure.md)

## <a name="prepare-for-failover-and-failback"></a>Yük devretme ve yeniden çalışma için hazırlama

VM anlık görüntü yok vardır ve şirket içi VM sırasında yükü kapalı olduğundan emin olun. Bu işlem, çoğaltma sırasında veri tutarlığını sağlar. Yeniden koruma bittikten sonra VM’yi açmayın. 

Yük devretme ve yeniden çalışma dört aşamalıdır:

1. **Azure’a yük devretme**: Makineleri şirket içi siteden Azure’a devredin.
2. **Azure Vm'leri koruyun**: geri şirket içi Hyper-V Vm'lerini çoğaltma başlatmaları böylece Azure Vm'leri koruyun.
3. **Şirket içi yük devri**: kullanılabilir olduğunda bir yük devretme Azure'dan şirket içi sitenize çalıştırın.
4. **Yeniden koruma şirket içi sanal makineleri**: veri geri başarısız oldu sonra şirket içi sanal makineleri Azure'a çoğaltma işlemi başlatma için yeniden koruyun.

## <a name="verify-vm-properties"></a>VM özelliklerini doğrulama

VM özelliklerini doğrulayın ve VM’nin [Azure gereksinimleriyle](hyper-v-azure-support-matrix.md#replicated-vms) uyumlu olduğundan emin olun.

1. İçinde **korunan öğeler**, tıklatın **çoğaltılan öğeler** >< VM-adı >.

2. İçinde **yinelenmiş öğesi** bölmesinde, VM bilgileri, sistem durumu ve en son kullanılabilir kurtarma noktalarını gözden geçirin. Daha fazla ayrıntı görüntülemek için **Özellikler**’e tıklayın.
     - İçinde **işlem ve ağ**, VM ayarlarını değiştirin ve ağ ayarlarını, ağ/alt ağ içinde dahil olmak üzere Azure VM. Yönetilen diskleri azure'dan Hyper-v için desteklenmez.
   Yük devretme ve kendisine atanmış IP adresi sonra bulunur.
    - **Diskler** bölümünde VM’deki işletim sistemi ve veri diskleriyle ilgili bilgileri görüntüleyebilirsiniz.

## <a name="fail-over-to-azure"></a>Azure'a yük

1. **Ayarlar** > **Çoğaltılan öğeler** bölümünde VM > **Yük devretme**’ye tıklayın.
2. İçinde **yük devretme** seçin **son** kurtarma noktası. 
3. **Yük devretmeyi başlatmadan önce makineyi kapatın** seçeneğini belirleyin. Yük devretme tetiklemeden önce kaynak VM'lerin bir kapatma yapmak Site Recovery çalışır. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.
4. Yük devretme tıklatın doğruladıktan sonra **yürütme**. Bu, tüm kullanılabilir kurtarma noktalarını siler.

> [!WARNING]
> **Devam eden yük devretme işlemini iptal etmeyin**: Yük devretme başlatılmadan önce VM çoğaltma durdurulur. Sürüyor, yük devretme durduruyor, iptal ancak VM yeniden çoğaltma olmaz

## <a name="reprotect-azure-vms"></a>Azure VM’lerini yeniden koruma

1. İçinde **AzureVMVault** > **öğeleri çoğaltılan**üzerinde başarısız oldu VM'ye sağ tıklayın ve seçin **yeniden koruma**.
2. Koruma yön değerine ayarlandığını doğrulayın **şirket içi Azure**.
3. Şirket içi VM veri tutarlılığını sağlamak için yükü sırasında devre dışı bırakılır. Yükü tamamladıktan sonra açmak yok.
4. Yükü işleminden sonra VM Azure'dan şirket içi siteye çoğaltmaya başlar.



## <a name="fail-over-from-azure-and-reprotect-the-on-premises-vm"></a>Azure'dan yük devri ve şirket içi VM koruyun

Azure'dan şirket içi siteye yük devri ve şirket içi siteden Vm'lerini Azure'a çoğaltma başlatın.

1. İçinde **ayarları** > **öğeleri çoğaltılan**, VM tıklayın > **planlı yük devretme**.
2. İçinde **planlı yük devretme onaylayın**, yük devretme yönünden (Azure) doğrulayın ve kaynak ve hedef konumları seçin.
3. Seçin **yük devretme önce verileri eşitle (yalnızca delta değişiklikler eşitleme)**. Bu seçenek VM kapatmadan eşitlediğinden VM kapalı kalma süresi en aza indirir.
4. Yük devretme başlatın. Yük devretme işleminin ilerleyişini izleyin **işleri** sekmesi.
5. Sonra ilk veri eşitlemesi gerçekleştirilir ve Azure VM'ler tıklatın kapatmaya hazır **işleri** > Planlı yük devretme-proje-adı > **yük devretmeyi tamamlamak**. Bu Azure VM kapatan, en son değişiklikleri şirket içi aktarır ve şirket içi VM başlatır.
6. Beklendiği gibi kullanılabilir denetlemek için şirket içi VM oturum açın.
7. Şirket içi VM olduğunu bir **Commit bekleyen** durumu. Tıklatın **yürütme**. Bu, Azure Vm'leri ve kendi diskleri siler ve şirket içi VM çoğaltmayı tersine çevirme için hazırlar.
Şirket içi VM Azure'a çoğaltma başlatmak için etkinleştirme **ters çoğaltma**. Bu, Azure VM değiştirildi devre dışı olduğundan oluşan delta değişikliklerini tetikler.  
