---
title: "Yük devri ve başarısız geri Hyper-V sanal makinelerini Azure Site Recovery ile çoğaltılan | Microsoft Docs"
description: "Hyper-V Vm'lerini azure'a yük devri ve Azure Site Recovery ile şirket içi siteye geri başarısız hakkında bilgi edinin"
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 12/31/2017
ms.author: raynew
ms.openlocfilehash: 390fe98bc718da4fe07f580bbf1dcbffbf8fc1ba
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="fail-over-and-fail-back-hyper-v-vms-replicated-to-azure"></a>Yük devri ve başarısız geri Hyper-V Azure'a kopyalanan VM

Bu öğretici, Azure'da bir Hyper-V VM yük devri açıklar. Devredilir sonra kullanılabilir olduğunda, şirket içi sitenize başarısız. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Hyper-V VM özelliklerini denetlemek için Azure gereksinimlerine uygun doğrulayın
> * Azure'a yük devretme gerçekleştirme
> * Şirket içi siteye Azure Vm'leri koruyun
> * Azure'dan şirket içi başarısız
> * Azure'a çoğaltma yeniden başlatmak için şirket içi VM'ler koruyun

Beşinci öğretici serisinde budur. Bu öğreticinin önceki eğitimlerine görevleri önceden tamamlamış varsayar.

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
2. [Şirket içi VMware’leri hazırlama](tutorial-prepare-on-premises-hyper-v.md)
3. Olağanüstü durum kurtarma için ayarlama [Hyper-V sanal makineleri](tutorial-hyper-v-to-azure.md), veya [System Center VMM bulutlarında yönetilen Hyper-V Vm'lerini](tutorial-hyper-v-vmm-to-azure.md)
4. [Olağanüstü durum kurtarma tatbikatı çalıştırma](tutorial-dr-drill-azure.md)

## <a name="prepare-for-failover-and-failback"></a>Yük devretme ve yeniden çalışma için hazırlama

VM anlık görüntü yok vardır ve şirket içi VM sırasında yükü kapalı olduğundan emin olun. Bu, çoğaltma sırasında veri tutarlılığını sağlamaya yardımcı olur. Yükü bittikten sonra VM kapatmayın. 

Yük devretme ve yeniden çalışma dört aşama bulunur:

1. **Azure'a yük**: başarısız makineler üzerinden şirket içi sitesinden Azure'a.
2. **Azure Vm'leri koruyun**: geri şirket içi Hyper-V Vm'lerini çoğaltma başlatmaları böylece Azure Vm'leri koruyun.
3. **Şirket içi yük devri**: kullanılabilir olduğunda bir yük devretme Azure'dan şirket içi sitenize çalıştırın.
4. **Yeniden koruma şirket içi sanal makineleri**: veri geri başarısız oldu sonra şirket içi sanal makineleri Azure'a çoğaltma işlemi başlatma için yeniden koruyun.

## <a name="verify-vm-properties"></a>VM özelliklerini doğrulayın

VM özelliklerini doğrulayın ve VM ile uyumlu olduğundan emin olun [Azure gereksinimleri](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

1. İçinde **korunan öğeler**, tıklatın **çoğaltılan öğeler** >< VM-adı >.

2. İçinde **yinelenmiş öğesi** bölmesinde, VM bilgileri, sistem durumu ve en son kullanılabilir kurtarma noktalarını gözden geçirin. Tıklatın **özellikleri** daha fazla ayrıntı görüntülemek için.
     - İçinde **işlem ve ağ**, VM ayarlarını değiştirin ve ağ ayarlarını, ağ/alt ağ içinde dahil olmak üzere Azure VM. Yönetilen diskleri azure'dan Hyper-v için desteklenmez.
   Yük devretme ve kendisine atanmış IP adresi sonra bulunur.
    - İçinde **diskleri**, VM işletim sistemi ve veri diskleri hakkındaki bilgileri görebilirsiniz.

## <a name="fail-over-to-azure"></a>Azure'a yük

1. İçinde **ayarları** > **öğeleri çoğaltılan** VM tıklayın > **yük devretme**.
2. İçinde **yük devretme** seçin **son** kurtarma noktası. 
3. Seçin **yük devretme işlemine başlamadan önce makineyi kapatın**. Yük devretme tetiklemeden önce kaynak VM'lerin bir kapatma yapmak Site Recovery çalışır. Kapatma başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleyişini izleyin **işleri** sayfası.
4. Yük devretme tıklatın doğruladıktan sonra **yürütme**. Bu, tüm kullanılabilir kurtarma noktalarını siler.

> [!WARNING]
> **Bir yük devretme devam ediyor iptal etme**: yük devretme başlatılmadan önce VM çoğaltma durduruldu. Sürüyor, yük devretme durduruyor, iptal ancak VM yeniden çoğaltma olmaz

## <a name="reprotect-azure-vms"></a>Azure VM'ler koruyun

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
