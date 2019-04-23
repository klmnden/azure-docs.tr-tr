---
title: Olağanüstü durum kurtarma sırasında VMware VM’lerde ve fiziksel sunucularda Azure Site Recovery’ye yük devretme ve yeniden çalışma | Microsoft Docs
description: Olağanüstü durum kurtarma sırasında Azure Site Recovery ile VMware VM’lerde ve fiziksel sunucularda Azure’a yük devretme ve şirket içi sitede yeniden çalışma hakkında bilgi edinin
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.topic: tutorial
ms.date: 04/08/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 9206e751fadab7a09c696fbe262aecdde002ae74
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59797674"
---
# <a name="fail-over-and-fail-back-vmware-vms"></a>Yük devretme ve ilk durumuna geri döndürme VMware Vm'leri

Bu makalede, bir şirket içi VMware azure'a VM yük devretmeyi açıklanır [Azure Site Recovery](site-recovery-overview.md) hizmeti. 

Bu, şirket içi makineler için Azure'da olağanüstü durum kurtarma ayarlama gösteren serideki beşinci öğreticidir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure gereksinimleriyle uyumlu olup olmadığını denetlemek için VMware VM özelliklerini doğrulama
> * Azure'a yük devretme işlemini çalıştırma


> [!NOTE]
> Öğreticiler bir senaryo için en basit dağıtım yolu gösterir. Mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı, yük devretme hakkında bilgi edinmek istiyorsanız [bu makaleyi gözden geçirin](site-recovery-failover.md).

## <a name="before-you-start"></a>Başlamadan önce
Önceki öğreticilerde tamamlayın:

1. Seçtiğiniz emin [Azure yedekleme](tutorial-prepare-azure.md) azure'a olağanüstü durum kurtarmaya şirket içi VMware Vm'leri, Hyper-V Vm'leri ve fiziksel makineler için.
2. Şirket içi hazırlama [VMware](vmware-azure-tutorial-prepare-on-premises.md) veya [Hyper-V](hyper-v-prepare-on-premises-tutorial.md) ortamınızı olağanüstü durum kurtarma. Fiziksel sunucular için olağanüstü durum kurtarma ayarlama yoksa gözden [destek matrisi](vmware-physical-secondary-support-matrix.md).
3. İçin olağanüstü durum kurtarma ayarlama [VMware Vm'lerini](vmware-azure-tutorial.md), [Hyper-V Vm'lerini](hyper-v-azure-tutorial.md), veya [fiziksel makineler](physical-azure-disaster-recovery.md).
4. Çalıştıran bir [olağanüstü durum kurtarma tatbikatı](tutorial-dr-drill-azure.md) her şeyin beklendiği gibi çalıştığından emin olmak için.


## <a name="failover-and-failback"></a>Yük devretme ve yeniden çalışma

Yük devretme ve yeniden çalışma dört aşamalıdır:

1. **Azure'a yük devretme**: Şirket içi birincil sitenizi çıktığında, makineleri Azure'a yük. Yük devretme sonrasında çoğaltılmış verileri Azure Vm'leri oluşturulur.
2. **Azure Vm'lerini yeniden koruma**: Böylece geri şirket içi VMware Vm'lerine çoğaltılmaya başlangıç, Azure'da Azure Vm'lerini yeniden koruyun. Şirket içi VM, veri tutarlılığı sağlamak için yeniden koruma sırasında kapatılır.
3. **Şirket içine yük devretme**: Şirket içi siteniz yukarı olduğunda ve çalışan, Azure'dan yeniden çalışma için yük devretme çalıştırın.
4. **Yeniden koruma şirket içi Vm'leri**: Veri çalıştıktan sonra böylece bunlar Azure'a çoğaltmaya başlamak için geri başarısız şirket içi Vm'leri yeniden koruyun.

## <a name="verify-vm-properties"></a>VM özelliklerini doğrulama

Bir yük devretme çalıştırmadan önce VM özelliklerini doğrulayın ve sanal makineleri ile uyumlu olduğundan emin [Azure gereksinimleri](vmware-physical-azure-support-matrix.md#replicated-machines).

Özellikleri aşağıda gösterildiği gibi doğrulayın:

1. **Korunan Öğeler**’de **Çoğaltılan Öğeler** > VM seçeneğine tıklayın.

2. **Çoğaltılan öğe** bölmesinde VM bilgileri ile sistem durumunun bir özeti ve kullanılabilen son kurtarma noktaları yer alır. Daha fazla ayrıntı görüntülemek için **Özellikler**’e tıklayın.

3. İçinde **işlem ve ağ**, değiştirebileceğiniz Azure ad, kaynak grubunu, hedef boyutunu, [kullanılabilirlik kümesi](../virtual-machines/windows/tutorial-availability-sets.md)ve yönetilen disk ayarlarını

4. Ağ ayalarını, yük devretmeden sonra Azure VM’nin yerleştirileceği ağ/alt ağ ve VM’ye atanacak IP adresi dahil olmak üzere görüntüleyebilir ve değiştirebilirsiniz.

5. **Diskler** bölümünde VM’deki işletim sistemi ve veri diskleriyle ilgili bilgileri görüntüleyebilirsiniz.

## <a name="run-a-failover-to-azure"></a>Azure'a yük devretme işlemini çalıştırma

1. **Ayarlar** > **Çoğaltılan öğeler** bölümünde VM > **Yük devretme**’ye tıklayın.
2. **Yük devretme** kısmında, yük devredeceğiniz bir **Kurtarma Noktası** seçin. Şu seçeneklerden birini kullanabilirsiniz:
   - **En son**: Bu seçenek öncelikle Site Recovery'ye gönderilen tüm verileri işler. Yük devretmeden sonra oluşturulan Azure VM, yük devretme tetiklendiğinde Site Recovery’ye çoğaltılan tüm verilere sahip olduğundan en düşük RPO (Kurtarma Noktası Hedefi) sağlanır.
   - **En son işlenen**: Bu seçenek VM'nin Site Recovery tarafından işlenen en son kurtarma noktasına devreder. İşlenmemiş verileri işlemek için zaman harcanmadığından bu seçenekte düşük bir RTO (Kurtarma Süresi Hedefi) sağlanır.
   - **Uygulamayla tutarlı olan sonuncu**: Bu seçenekte VM yükü Site Recovery tarafından işlenen en son uygulamayla tutarlı kurtarma noktası devredilir.
   - **Özel**: Bir kurtarma noktası belirtin.

3. Seçin **yük devretmeye başlamadan önce makineyi Kapat** yük devretmeyi tetiklemeden önce kaynak sanal makineleri kapatmayı denemek için. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.

Bazı senaryolarda yük devretme için sekiz ila on dakikada tamamlanan ek işlem gerekir. Uzun yük devretme testi süreleriyle:
- 9.8 eski bir Mobility hizmeti sürümü çalıştıran VMware sanal makinelerini
- Fiziksel sunucular
- VMware sanal makineleri
- Fiziksel sunucuları olarak korunan Hyper-V Vm'leri
- Etkin DHCP hizmeti sahip olmayan VMware Vm'leri
- Aşağıdaki önyükleme sürücülerine sahip olmayan VMware Vm'lerini: storvsc, vmbus, storflt, intelide, atapi.

> [!WARNING]
> **Devam eden bir yük devretme işlemini iptal etmeyin**: Yük devretme başlatılmadan önce VM çoğaltması durdurulur. Devam eden bir yük devretme işlemini iptal ederseniz yük devretme durdurulur, ancak VM yeniden çoğaltılmaz.

## <a name="connect-to-failed-over-vm"></a>VM üzerinde başarısız bağlanma

1. Yük devretmeden sonra RDP/SSH'yi kullanarak Azure vm'lerine bağlanmak isterseniz [bu gereksinimlerini doğrulama](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
2. Yük devretme sonrasında sanal Makineye gidin ve tarafından doğrulama [bağlanma](../virtual-machines/windows/connect-logon.md) ona.
3. Kullanma **kurtarma noktasını Değiştir** yük devretmeden sonra farklı bir kurtarma noktası kullanmak istiyorsanız. Sonraki adımda bir yük devretme yürütüldükten sonra bu seçeneği artık kullanılabilir.
4. Doğrulama sonrasında tıklayarak **işleme** yük devretmeden sonra sanal makinenin kurtarma noktasını sonlandırmak için.
5. Yürütmeden sonra diğer tüm kullanılabilir kurtarma noktaları silinir. Bu, yük devretme tamamlar.

>[!TIP]
> Yük devretme sonrasında herhangi bir bağlantı sorunla karşılaşırsanız izleyin [sorun giderme kılavuzu](site-recovery-failover-to-azure-troubleshoot.md).

## <a name="next-steps"></a>Sonraki adımlar

Yük devretme sonrasında Azure Vm'lerini şirket içine yeniden koruyun. Vm'leri yeniden korunan ve şirket içi siteye çoğaltan olduktan sonra hazır olduğunuzda sonra Azure'dan yeniden çalışma.

> [!div class="nextstepaction"]
> [Azure Vm'lerini yeniden koruma](vmware-azure-reprotect.md)
> [Azure'dan yeniden çalışma](vmware-azure-failback.md) 
