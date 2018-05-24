---
title: Azure Site Recovery ile şirket içi makineler için olağanüstü durum kurtarma tatbikatı gerçekleştirme | Microsoft Docs
description: Azure Site Recovery ile şirket içinden Azure’a olağanüstü durum kurtarma tatbikatı gerçekleştirme hakkında bilgi alın
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 05/16/2018
ms.author: raynew
ms.openlocfilehash: 724144e8f2f2f76c4ad98b4c5cad84e69dadadbb
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34209734"
---
# <a name="run-a-disaster-recovery-drill-to-azure"></a>Azure’da olağanüstü durum kurtarma tatbikatı çalıştırma

Bu öğretici, Azure’da bir yük devretme testi kullanarak şirket içi makine için olağanüstü durum tatbikatı gerçekleştirme işlemini gösterir. Tatbikat, veri kaybı olmadan çoğaltma stratejinizi doğrular. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yük devretme testi için yalıtılmış bir ağ ayarlama
> * Yük devretme sonrasında Azure VM'ye bağlanmak için hazırlık yapma
> * Tek bir makine için yük devretme testi çalıştırma

Bu, serideki dördüncü öğreticidir. Bu öğreticide, önceki öğreticilerdeki adımları zaten tamamladığınız varsayılır.

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
2. [Şirket içi VMware’leri hazırlama](tutorial-prepare-on-premises-vmware.md)
3. [Olağanüstü durum kurtarmayı ayarlama](tutorial-vmware-to-azure.md)

## <a name="verify-vm-properties"></a>VM özelliklerini doğrulama

Bir yük devretme testi gerçekleştirmeden önce VM özelliklerini doğrulayın ve Hyper-V VM[hyper-v-azure-support-matrix.md#replicated-vms], [VMware VM ya da fiziksel sunucunun](vmware-physical-azure-support-matrix.md#replicated-machines) Azure gereksinimlerine uygun olduğundan emin olun.

1. **Korunan Öğeler**’de **Çoğaltılan Öğeler** > VM seçeneğine tıklayın.
2. **Çoğaltılan öğe** bölmesinde VM bilgileri ile sistem durumunun bir özeti ve kullanılabilen son kurtarma noktaları yer alır. Daha fazla ayrıntı görüntülemek için **Özellikler**’e tıklayın.
3. **İşlem ve Ağ** bölümünde Azure adını, kaynak grubunu, hedef boyutunu, [kullanılabilirlik kümesini](../virtual-machines/windows/tutorial-availability-sets.md) ve yönetilen disk ayarlarını değiştirebilirsiniz.
   
      >[!NOTE]
      Yönetilen disklerle Azure VM’lerinden şirket içi Hyper-V makinelerine yeniden çalışma şu anda desteklenmemektedir. Yönetilen diskler seçeneğini yalnızca şirket içi VM’leri yeniden çalıştırmadan Azure’a geçirmeyi planlıyorsanız yük devretme için kullanmalısınız.
   
4. Ağ ayalarını, yük devretmeden sonra Azure VM’nin yerleştirileceği ağ/alt ağ ve VM’ye atanacak IP adresi dahil olmak üzere görüntüleyebilir ve değiştirebilirsiniz.
5. **Diskler** bölümünde VM’deki işletim sistemi ve veri diskleriyle ilgili bilgileri görüntüleyebilirsiniz.

## <a name="run-a-test-failover-for-a-single-vm"></a>Tek bir VM için yük devretme testi çalıştırma

Yük devretme testi çalıştırdığınızda şunlar olur:

1. Yük devretme için gerekli tüm koşulların karşılandığından emin olmak için bir önkoşul denetimi çalıştırılır.
2. Yük devretme, bir Azure sanal makinesi oluşturulabilmesi için verileri işler. En son kurtarma noktasını seçerseniz verilerden bir kurtarma noktası oluşturulur.
3. Önceki adımda işlenen veriler kullanılarak bir Azure sanal makinesi oluşturulur.

Yük devretme testini aşağıdaki gibi çalıştırın:

1. **Ayarlar** > **Çoğaltılmış Öğeler** bölümünde, CM > **+Yük Devretme Testi**’ne tıklayın.
2. Bu öğretici için **Son işlenen** kurtarma noktasını seçin. Bu işlem VM yükünü zamandaki en son kullanılabilir noktaya devreder. Zaman damgası gösterilir. Bu seçenekte, verileri işlemek için zaman harcanmadığından düşük bir RTO (kurtarma süresi hedefi) sağlanır.
3. **Yük Devretme Testi** bölümünde, yük devretme gerçekleştikten sonra Azure VM’lerinin bağlanacağı hedef Azure ağını seçin.
4. Yük devretmeyi başlatmak için **Tamam**'a tıklayın. Sanal makine özelliklerini açmak için sanal makineye tıklayarak ilerleme durumunu izleyebilirsiniz. Ya da kasa adı > **Ayarlar** > **İşler** >
   **Site Recovery işleri** bölümünde **Yük Devretme Testi** işine tıklayabilirsiniz.
5. Yük devretme bittikten sonra, çoğaltma Azure VM, Azure portalı > **Sanal Makineler** bölümünde görünür. Sanal makinenin uygun boyutta olduğundan, doğru ağa bağlandığından ve çalıştığından emin olun.
6. Şimdi Azure’da çoğaltılan sanal makineye bağlanabiliyor olmanız gerekir.
7. Yük devretme testi sırasında oluşturulan Azure sanal makinelerini silmek için, VM’de **Yük devretme testini temizle**’ye tıklayın. Yük devretme testiyle ilişkili gözlemlerinizi **Notlar**’da kaydedin veya saklayın.

Bazı senaryolarda yük devretme için sekiz ila on dakikada tamamlanan ek işlem gerekir. VMware Linux makinelerinde, DHCP hizmeti etkinleştirilmemiş VMware VM’lerinde ve storvsc, vmbus, storflt, intelide, atapi önyükleme sürücülerine sahip olmayan VMware VM’lerinde uzun yük devretme testi süreleriyle karşılaşabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Şirket içi VMware VM’ler arasında yük devretme ve yeniden çalışma işlemi gerçekleştirme](vmware-azure-tutorial-failover-failback.md).
