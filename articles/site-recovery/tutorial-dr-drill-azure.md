---
title: "Azure Site Recovery ile azure'a şirket içi makineler için olağanüstü durum kurtarma ayrıntıya çalıştırma | Microsoft Docs"
description: "Olağanüstü durum kurtarma ayrıntıya Azure Site Recovery ile azure'a şirket içi çalıştırma hakkında bilgi alın"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ddd17921-68f4-41c7-ba4c-b767d36f1733
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/18/2017
ms.author: raynew
ms.openlocfilehash: 15e4487217ec21bb33380422640cb19dfcbcee39
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="run-a-disaster-recovery-drill-to-azure"></a>Azure’da olağanüstü durum kurtarma tatbikatı çalıştırma

Bu öğretici bir yük devretme testi kullanarak Azure için şirket içi makineler için olağanüstü durum kurtarma ayrıntıya çalıştırılacağını gösterir. Bir detaylandırma çoğaltma stratejinizi veri kaybı olmadan doğrular. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yük devretme sınaması için yalıtılmış bir ağı ayarlama
> * Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma
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
4. Görüntüleyin ve hangi Azure VM yük devretme ve kendisine atanmış IP adresi sonra yer alacağı ağını/alt ağını dahil olmak üzere ağ ayarlarını değiştirin.
5. İçinde **diskleri**, VM işletim sistemi ve veri diskleri hakkındaki bilgileri görebilirsiniz.

## <a name="run-a-test-failover-for-a-single-vm"></a>Tek bir VM için yük devretme testi çalıştırma

Yük devretme testi çalıştırdığınızda, şunlar olur:

1. Tüm yük devretme için gereken koşulların karşılandığından emin olmak için çalışır bir ön koşullarını denetleyin.
2. Bir Azure VM oluşturulan yük devretme verileri işler. En son kurtarma noktası, bir kurtarma noktası seçin, verilerden oluşturulur.
3. Bir Azure VM, önceki adımda işlenen veriler kullanılarak oluşturulur.

Yük devretme testi çalıştırmak için şunları yapın:

1. İçinde **ayarları** > **çoğaltılan öğeler**, VM tıklayın > **+ yük devretme testi**.

2. Yük devretme için kullanılacak bir kurtarma noktası seçin:
    - **En son işlenen** : VM üzerinden Site Recovery tarafından işlenen en son kurtarma noktası başarısız olur. Zaman damgası gösterilir. Bu seçenek ile düşük RTO (Kurtarma süresi hedefi) sağlayan verileri işleme hiçbir zaman harcanmaktadır.
    - **Son uygulama tutarlı**: tüm sanal makineleri en son uygulamayla tutarlı kurtarma noktası için bu seçeneği yöneltilir. Zaman damgası gösterilir.
    - **Özel**: herhangi bir kurtarma noktası seçin.
3. İçinde **yük devretme testi**, hedef Azure seçin Azure Vm'lerinin bir ağa bağlı olacak yük devretme gerçekleştikten sonra.
4. Yük devretmeyi başlatmak için **Tamam**'a tıklayın. VM özelliklerini açmak için tıklayarak ilerleme durumunu izleyebilirsiniz. Veya tıklatabilirsiniz **yük devretme testi** kasa adı işinde > **ayarları** > **işleri** >
   **Site Recovery işleri**.
5. Yük devretme işlemi tamamlandıktan sonra çoğaltma Azure VM Azure portalında görünür. > **sanal makineleri**. VM sağ ağa bağlı uygun boyutta olduğundan ve çalışıp çalışmadığını denetleyin.
6. Artık Azure çoğaltılmış VM'yi bağlanabiliyor olması gerekir.
7. Yük devretme testi sırasında oluşturulan Azure VM'ler silmek için tıklatın **temizleme yük devretme testi** kurtarma planı üzerinde. İçinde **notları**, kaydetme ve yük devretme testiyle ilişkili gözlemlerinizi kaydetmek.

Bazı senaryolarda, yük devretme tamamlamak için yaklaşık sekiz ile on dakika sürer ek işleme gerektirir. Artık fark edebilirsiniz test VMware Linux makineler, DHCP hizmetini etkinleştirir yok VMware Vm'lerini ve aşağıdaki önyükleme sürücüleri yok VMware Vm'leri için yük devretme süreleri: storvsc, vmbus, storflt, Intelide, ATAPI.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Şirket içi VMware Vm'leri için bir yük devretme ve yeniden çalışma](tutorial-vmware-to-azure-failover-failback.md).
