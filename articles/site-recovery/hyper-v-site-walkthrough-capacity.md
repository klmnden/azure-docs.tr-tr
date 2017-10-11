---
title: "Hyper-V VM (VMM olmadan) Azure'a çoğaltma için Azure Site Recovery ile ölçekleme ve kapasite planlaması | Microsoft Docs"
description: "Hyper-V sanal makineleri Azure Site Recovery ile azure'a çoğaltırken planı kapasite ve ölçek bu makaleyi kullanın"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 8687af60-5b50-481c-98ee-0750cbbc2c57
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: c7891c188c2cecbbf056fa79672a13bb16fa7fcf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="step-3-plan-capacity-and-scaling-for-hyper-v-to-azure-replication"></a>3. adım: kapasite ve Hyper-V için Azure çoğaltma ölçeklendirme planlama

Bu makale için kapasite planlaması ve ölçeklendirme, şirket içi Hyper-V Vm'lerini (System Center VMM olmadan) çoğaltırken ekleneceğini Azure ile kullanmak [Azure Site Recovery](site-recovery-overview.md).

Bu makaleyi okuduktan sonra altındaki bir yorum gönderin ya da teknik sorular [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="how-do-i-start-capacity-planning"></a>Kapasite planlama nasıl başlamanız gerekir?


Çoğaltma ortamınız hakkında bilgi toplamak ve bu makalede vurgulanmış konuları birlikte bu bilgileri kullanarak kapasite planlama.


## <a name="gather-information"></a>Bilgileri toplayın

1. VM'ler, VM başına disk ve disk başına depolama dahil olmak üzere çoğaltma ortamınız hakkında bilgi toplayın.
2. Çoğaltılan veriler için günlük değişim (dalgalanma) oranı tanımlayın. Karşıdan [Hyper-V kapasite planlama aracı](https://www.microsoft.com/download/details.aspx?id=39057) değişim oranı alınamıyor. Bu aracı ortalamalar yakalamak için bir hafta içinde çalıştırmak öneririz.
 

## <a name="figure-out-capacity"></a>Kapasitesi Şekil

Toplama seçtiğiniz bilgilere dayanarak, çalıştırmanız [Site kurtarma kapasite planlayıcısı aracı](http://aka.ms/asr-capacity-planner-excel) kaynak ortamı ve iş yükleri çözümlemek için bant genişliği gereksinimlerini ve sunucu kaynaklarını kaynak konum ve kaynakları (tahmin etme sanal makineler ve depolama vb.) hedef konumda gereken. Aracı modları birkaç içinde çalıştırabilirsiniz:

- Hızlı planlama: aracı ağ ve sunucu tahminleri almak için bu modda ortalama bir VM'ler, diskleri, depolama ve değişim oranı sayısına göre çalıştır.
- Ayrıntılı planlama: Bu modda aracını çalıştırın ve her iş yükü VM düzeyinde ayrıntılarını sağlayın. VM uyumluluk çözümlemek ve ağ ve sunucu tahminleri alın.

Şimdi aracını çalıştırın:

1. Karşıdan [aracı](http://aka.ms/asr-capacity-planner-excel)
2. Hızlı Planlayıcısı çalıştırmak için izleyin [bu yönergeleri](site-recovery-capacity-planner.md#run-the-quick-planner), senaryo seçip **Hyper-V Azure**.
3. Ayrıntılı Planlayıcısı çalıştırmak için izleyin [bu yönergeleri](site-recovery-capacity-planner.md#run-the-detailed-planner), senaryo seçip **Hyper-V Azure**.

## <a name="control-network-bandwidth"></a>Ağ bant genişliğini denetlemek

Gereksinim duyduğunuz bant genişliği hesaplanan sonra çoğaltma için kullanılan bant genişliği miktarını denetleme seçenekleri birkaç vardır:

* **Bant genişliğini kısıtlama**: Azure'a çoğaltılan Hyper-V trafiği belirli bir Hyper-V ana bilgisayar üzerinden gider. Ana bilgisayar sunucusunda bant genişliğini kısıtlayabilirsiniz.
* **Bant genişliği üzerinde etki**: birkaç kayıt defteri anahtarlarını kullanarak çoğaltma için kullanılan bant genişliği etkileyebilir.

### <a name="throttle-bandwidth"></a>Bant genişliğini kısıtlama
1. Hyper-V ana bilgisayar sunucusunda Microsoft Azure Backup MMC ek bileşenini açın. Varsayılan olarak Microsoft Azure Backup için masaüstünde veya C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin yolunda bir kısayol bulunur.
2. Ek bileşende **Özellikleri Değiştir**'e tıklayın.
3. **Azaltma** sekmesinde **Yedekleme işlemleri için internet bant genişliği kullanımını azaltmayı etkinleştir** seçeneğini belirleyin ve çalışma saatleri ve çalışma dışı saatler için sınırları ayarlayın. Geçerli aralıklar saniye başına 512 Kbps ila 102 Mbps arasındadır.

    ![Bant genişliğini kısıtlama](./media/hyper-v-site-walkthrough-capacity/throttle2.png)

Ayrıca, azaltma ayarı için [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet'ini de kullanabilirsiniz. Bu ayara ilişkin örneği aşağıda bulabilirsiniz:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting - NoThrottle** hiçbir azaltmanın gerekli olmadığını gösterir.

### <a name="influence-network-bandwidth"></a>Ağ bant genişliği üzerinde etki oluşturma
1. Kayıt defterinde, **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication** hedefine gidin.
   * Bir çoğaltma diskte bant genişliği trafiği etkilemek için değerini değiştirin **UploadThreadsPerVM**, veya yoksa anahtar oluşturun.
   * Azure'dan yeniden çalışma trafiği için bant genişliği etkilemek için değerini değiştirin **DownloadThreadsPerVM**.
2. Varsayılan değer 4'tür. "Fazla sağlanan" bir ağda, bu kayıt defteri anahtarlarının varsayılan değerlerinin değiştirilmesi gerekir. Maksimum değer 32'dir. Değeri iyileştirmek için trafiği izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Git [4. adım: ağ planlama](hyper-v-site-walkthrough-network.md).
