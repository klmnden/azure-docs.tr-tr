---
title: "Azure Site Recovery ile azure'a şirket içi makineler için olağanüstü durum kurtarma ayrıntıya çalıştırma | Microsoft Docs"
description: "Olağanüstü durum kurtarma ayrıntıya Azure Site Recovery ile azure'a şirket içi çalıştırma hakkında bilgi alın"
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 12/31/2017
ms.author: raynew
ms.openlocfilehash: f7dc5e2df95a64685a8b70d25e839c371d4fc2de
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="run-a-disaster-recovery-drill-to-azure"></a>Azure’da olağanüstü durum kurtarma tatbikatı çalıştırma

Bu öğretici bir yük devretme testi kullanarak nasıl Azure için bir şirket içi makineyi için olağanüstü durum kurtarma ayrıntıya çalıştırılacağını gösterir. Bir detaylandırma çoğaltma stratejinizi veri kaybı olmadan doğrular. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yük devretme sınaması için yalıtılmış bir ağı ayarlama
> * Yük devretme sonrasında Azure VM'ye bağlanmak hazırlama
> * Bir yük devretme sınaması için tek bir makineye çalıştırın

Bu, bir dizideki dördüncü öğreticidir. Bu öğreticinin önceki eğitimlerine görevleri önceden tamamlamış varsayar.

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
2. [Şirket içi VMware’leri hazırlama](tutorial-prepare-on-premises-vmware.md)
3. [Olağanüstü durum kurtarmayı ayarlama](tutorial-vmware-to-azure.md)

## <a name="verify-vm-properties"></a>VM özelliklerini doğrulayın

Yük devretme testi çalıştırmadan önce VM özelliklerini doğrulayın ve VM ile uyumlu olduğundan emin olun [Azure gereksinimleri](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

1. İçinde **korunan öğeler**, tıklatın **çoğaltılan öğeler** > VM.
2. İçinde **yinelenmiş öğesi** bölmesinde VM bilgileri, sistem durumu özetini yoktur ve en son kullanılabilir kurtarma noktaları. Tıklatın **özellikleri** daha fazla ayrıntı görüntülemek için.
3. İçinde **işlem ve ağ**, değiştirebilirsiniz Azure ad, kaynak grubu, hedef boyutu [kullanılabilirlik kümesi](../virtual-machines/windows/tutorial-availability-sets.md)ve disk ayarlarını yönetilir.
   
      >[!NOTE]
      Şirket içi Hyper-V makineler yeniden çalışma yönetilen disklerle Azure vm'lerden şu anda desteklenmiyor. Bunları geri başarısız olmadan Azure için şirket içi sanal makineleri geçirmeyi planlıyorsanız, yük devretme için yalnızca yönetilen diskleri seçeneği kullanmanız gerekir.
   
4. Görüntüleyin ve hangi Azure VM yük devretme ve kendisine atanmış IP adresi sonra yer alacağı ağını/alt ağını dahil olmak üzere ağ ayarlarını değiştirin.
5. İçinde **diskleri**, VM işletim sistemi ve veri diskleri hakkındaki bilgileri görebilirsiniz.

## <a name="run-a-test-failover-for-a-single-vm"></a>Tek bir VM için yük devretme testi çalıştırma

Yük devretme testi çalıştırdığınızda, şunlar olur:

1. Tüm yük devretme için gereken koşulların karşılandığından emin olmak için çalışır bir ön koşullarını denetleyin.
2. Bir Azure VM oluşturulan yük devretme verileri işler. En son kurtarma noktası, bir kurtarma noktası seçin, verilerden oluşturulur.
3. Bir Azure VM, önceki adımda işlenen veriler kullanılarak oluşturulur.

Yük devretme testi çalıştırmak için şunları yapın:

1. İçinde **ayarları** > **çoğaltılan öğeler**, VM tıklayın > **+ yük devretme testi**.
2. Seçin **son işlenen** Bu öğretici için kurtarma noktası. Bu en son kullanılabilir noktasına VM üzerinden zamanında başarısız olur. Zaman damgası gösterilir. Bu seçenek ile düşük RTO (Kurtarma süresi hedefi) sağlayan verileri işleme hiçbir zaman harcanmaktadır.
3. İçinde **yük devretme testi**, hedef Azure seçin Azure Vm'lerinin bir ağa bağlı olacak yük devretme gerçekleştikten sonra.
4. Yük devretmeyi başlatmak için **Tamam**'a tıklayın. VM özelliklerini açmak için tıklayarak ilerleme durumunu izleyebilirsiniz. Veya tıklatabilirsiniz **yük devretme testi** kasa adı işinde > **ayarları** > **işleri** >
   **Site Recovery işleri**.
5. Yük devretme işlemi tamamlandıktan sonra çoğaltma Azure VM Azure portalında görünür. > **sanal makineleri**. VM sağ ağa bağlı uygun boyutta olduğundan ve çalışıp çalışmadığını denetleyin.
6. Artık Azure çoğaltılmış VM'yi bağlanabiliyor olması gerekir.
7. Yük devretme testi sırasında oluşturulan Azure VM'ler silmek için tıklatın **temizleme yük devretme testi** VM üzerinde. İçinde **notları**, kaydetme ve yük devretme testiyle ilişkili gözlemlerinizi kaydetmek.

Bazı senaryolarda, yük devretme tamamlamak için yaklaşık sekiz ile on dakika sürer ek işleme gerektirir. Artık fark edebilirsiniz test VMware Linux makineler, DHCP hizmetini etkinleştirir yok VMware Vm'lerini ve aşağıdaki önyükleme sürücüleri yok VMware Vm'leri için yük devretme süreleri: storvsc, vmbus, storflt, Intelide, ATAPI.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Şirket içi VMware Vm'leri için bir yük devretme ve yeniden çalışma](tutorial-vmware-to-azure-failover-failback.md).
