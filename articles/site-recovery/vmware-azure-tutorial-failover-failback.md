---
title: Olağanüstü durum kurtarma sırasında VMware VM’lerde ve fiziksel sunucularda Azure Site Recovery’ye yük devretme ve yeniden çalışma | Microsoft Docs
description: VMware Vm'leri ve fiziksel sunucuları azure'a yük devretme işlemini ve şirket içi sitede yeniden, Site Recovery kullanarak azure'a olağanüstü durum kurtarma sırasında başarısız öğrenin.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.topic: tutorial
ms.date: 04/08/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 7a089b3e4d7b8a38f2bf88c8ccf6e269331589be
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65966286"
---
# <a name="fail-over-and-fail-back-vmware-vms"></a>Yük devretme ve ilk durumuna geri döndürme VMware Vm'leri

Bu makalede bir şirket içi VMware sanal makinesi (VM) üzerinde başarısız nasıl [Azure Site Recovery](site-recovery-overview.md).

Bu, şirket içi makineler için Azure'da olağanüstü durum kurtarma ayarlama gösteren serideki beşinci öğreticidir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * VMware VM özelliklerini Azure gereksinimleriyle uyumlu olduğunu doğrulayın.
> * Azure'a yük devretme çalıştırın.

> [!NOTE]
> Öğreticiler bir senaryo için en basit dağıtım yolu gösterir. Bunlar, mümkün olduğunda varsayılan seçenekleri kullanın ve tüm olası ayarları ve yol gösterme. Ayrıntılı yük devretme hakkında bilgi edinmek istiyorsanız bkz [Vm'leri ve fiziksel sunucuları başarısız](site-recovery-failover.md).

## <a name="before-you-start"></a>Başlamadan önce

Önceki öğreticilerde tamamlayın:

1. Seçtiğiniz emin [Azure yedekleme](tutorial-prepare-azure.md) azure'a olağanüstü durum kurtarmaya şirket içi VMware Vm'leri, Hyper-V Vm'leri ve fiziksel makineler için.
2. Şirket içi hazırlama [VMware](vmware-azure-tutorial-prepare-on-premises.md) veya [Hyper-V](hyper-v-prepare-on-premises-tutorial.md) ortamınızı olağanüstü durum kurtarma. Fiziksel sunucular için olağanüstü durum kurtarma ayarlama yoksa gözden [destek matrisi](vmware-physical-secondary-support-matrix.md).
3. İçin olağanüstü durum kurtarma ayarlama [VMware Vm'lerini](vmware-azure-tutorial.md), [Hyper-V Vm'lerini](hyper-v-azure-tutorial.md), veya [fiziksel makineler](physical-azure-disaster-recovery.md).
4. Çalıştıran bir [olağanüstü durum kurtarma tatbikatı](tutorial-dr-drill-azure.md) her şeyin beklendiği gibi çalıştığından emin olmak için.

## <a name="failover-and-failback"></a>Yük devretme ve yeniden çalışma

Yük devretme ve yeniden çalışma dört aşamalıdır:

1. **Azure'a yük:** Şirket içi birincil sitenizi çıktığında, makineleri Azure'a yük. Yük devretme sonrasında çoğaltılmış verileri Azure Vm'leri oluşturulur.
2. **Azure Vm'lerini yeniden koruma:** Böylece geri şirket içi VMware Vm'lerine çoğaltılmaya başlangıç, Azure'da Azure Vm'lerini yeniden koruyun. Şirket içi VM, veri tutarlılığı sağlamak için yeniden koruma sırasında kapatılır.
3. **Şirket içine yük devretme:** Şirket içi siteniz yukarı olduğunda ve çalışan, Azure'dan yeniden çalışma için yük devretme çalıştırın.
4. **Şirket içi Vm'leri yeniden koruma:** Veri çalıştıktan sonra böylece bunlar Azure'a çoğaltmaya başlamak için geri başarısız şirket içi Vm'leri yeniden koruyun.

## <a name="verify-vm-properties"></a>VM özelliklerini doğrulama

Bir yük devretmeyi çalıştırmadan önce Vm'leri karşıladığından emin olmak için VM özelliklerini denetleyin [Azure gereksinimleri](vmware-physical-azure-support-matrix.md#replicated-machines).

Özellikleri aşağıda gösterildiği gibi doğrulayın:

1. İçinde **korunan öğeler**seçin **çoğaltılan öğeler**ve ardından doğrulamak istediğiniz VM'yi seçin.

2. **Çoğaltılan öğe** bölmesinde VM bilgileri ile sistem durumunun bir özeti ve kullanılabilen son kurtarma noktaları yer alır. Seçin **özellikleri** daha fazla ayrıntı görüntülemek için.

3. İçinde **işlem ve ağ**, bu özellikleri gerektiği gibi değiştirebilirsiniz:
    * Azure ad
    * Kaynak grubu
    * Hedef boyutu
    * [Kullanılabilirlik kümesi](../virtual-machines/windows/tutorial-availability-sets.md)
    * Yönetilen disk ayarları

4. Görüntüleyebilir ve dahil olmak üzere, ağ ayarlarını değiştirme:

    * Ağ ve alt ağ, Azure VM yük devretme işleminden sonra yerleştirilir.
    * Kendisine atanmış bir IP adresi.

5. **Diskler** bölümünde VM’deki işletim sistemi ve veri diskleriyle ilgili bilgileri görüntüleyebilirsiniz.

## <a name="run-a-failover-to-azure"></a>Azure'a yük devretme işlemini çalıştırma

1. İçinde **ayarları** > **çoğaltılan öğeler**, yük devretme ve ardından istediğiniz VM'yi seçin **yük devretme**.
2. **Yük devretme** kısmında, yük devredeceğiniz bir **Kurtarma Noktası** seçin. Şu seçeneklerden birini kullanabilirsiniz:
   * **En son**: Bu seçenek öncelikle Site Recovery'ye gönderilen tüm verileri işler. Yük devretme sonrasında oluşturulan Azure VM yük devretme tetiklendiğinde Site Recovery'ye çoğaltılan tüm verilere sahip olduğundan en düşük kurtarma noktası hedefi (RPO) sağlar.
   * **En son işlenen**: Bu seçenek VM'nin Site Recovery tarafından işlenen en son kurtarma noktasına devreder. Bu seçenek, işlenmemiş verileri işlemeye zaman harcanmadığından düşük RTO (Kurtarma süresi hedefi) sağlar.
   * **Uygulamayla tutarlı olan sonuncu**: Bu seçeneği VM'nin Site Recovery tarafından işlenen en son uygulamayla tutarlı kurtarma noktasına devreder.
   * **Özel**: Bu seçenek bir kurtarma noktası belirtmenize olanak sağlar.

3. Seçin **yük devretmeye başlamadan önce makineyi Kapat** için yük devretmeyi tetiklemeden önce kaynak sanal makineleri kapatmayı deneyin. Kapatma başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.

Bazı senaryolarda yük devretme tamamlanması yaklaşık 8-10 dakika sürer, ek işleme gerektirir. Uzun yük devretme testi süreleriyle:

* VMware Vm'lerini çalıştıran 9.8 eski bir Mobility hizmeti sürümü.
* Fiziksel sunucuları.
* VMware Linux VM'ler.
* Hyper-V Vm'leri korumalı fiziksel sunucuları.
* Etkin DHCP hizmeti sahip olmayan VMware VM'ler.
* Aşağıdaki önyükleme sürücülerine sahip olmayan VMware Vm'lerini: storvsc, vmbus, storflt, intelide, atapi.

> [!WARNING]
> Devam eden bir yük devretme işlemini iptal etmeyin. Yük devretme başlatılmadan önce VM çoğaltması durdurulur. Devam eden bir yük devretme işlemini iptal ederseniz yük devretme durdurulur, ancak VM yeniden çoğaltılmaz.

## <a name="connect-to-failed-over-vm"></a>Devredilen VM'ye bağlanma

1. Uzak Masaüstü Protokolü (RDP) ve güvenli Kabuk (SSH) kullanarak yük devretme sonrasında Azure Vm'lerine bağlanmak isterseniz [gereksinimleri karşıladığınızdan emin olun](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
2. Yük devretme sonrasında sanal Makineye gidin ve tarafından doğrulama [bağlanma](../virtual-machines/windows/connect-logon.md) ona.
3. Kullanma **kurtarma noktasını Değiştir** yük devretmeden sonra farklı bir kurtarma noktası kullanmak istiyorsanız. Sonraki adımda bir yük devretme yürütüldükten sonra bu seçeneği artık kullanılabilir.
4. Doğrulama sonrasında seçin **işleme** yük devretmeden sonra sanal makinenin kurtarma noktasını sonlandırmak için.
5. Yürüttükten sonra diğer tüm kullanılabilir kurtarma noktaları silinir. Bu adım, yük devretme işlemini tamamlar.

>[!TIP]
> Yük devretme sonrasında herhangi bir bağlantı sorunla karşılaşırsanız izleyin [sorun giderme kılavuzu](site-recovery-failover-to-azure-troubleshoot.md).

## <a name="next-steps"></a>Sonraki adımlar

Yük devretme sonrasında Azure Vm'lerini şirket içine yeniden koruyun. Vm'leri yeniden korunan ve şirket içi siteye çoğaltan olduktan sonra hazır olduğunuzda sonra Azure'dan yeniden çalışma.

> [!div class="nextstepaction"]
> [Azure Vm'lerini yeniden koruma](vmware-azure-reprotect.md)
> [Azure'dan yeniden çalışma](vmware-azure-failback.md)
