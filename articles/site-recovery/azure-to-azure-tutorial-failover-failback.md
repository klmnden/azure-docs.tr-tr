---
title: Yük devretme ve Azure Site Recovery hizmeti ile olağanüstü durum kurtarma için ikincil Azure bölgesine çoğaltılmış Azure Vm'lerini yeniden koruyun.
description: Yük devretme ve Azure Site Recovery hizmeti ile olağanüstü durum kurtarma için ikincil Azure bölgesine çoğaltılmış Azure Vm'lerini yeniden koruma hakkında bilgi edinin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 05/30/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: be9edd0497cca894e4daa87f97b037065379127f
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66398274"
---
# <a name="fail-over-and-reprotect-azure-vms-between-regions"></a>Yük devretme ve bölgeleri arasında Azure Vm'lerini yeniden koruma

Bu öğreticide bir Azure sanal makine (VM) üzerinden yük devredebilir ile ikincil bir Azure bölgesine açıklar [Azure Site Recovery](site-recovery-overview.md) hizmeti. Yük devrettikten sonra VM'yi yeniden koruyun. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure VM’ye yük devretme
> * Birincil bölgeye çoğaltır, böylece ikincil Azure VM'yi yeniden koruyun.

> [!NOTE]
> Bu öğreticide, varsayılan ayarlar ve en az özelleştirme en basit yolu içerir. Daha karmaşık senaryolarda, Azure Vm'leri için 'Nasıl yapılır' altında makaleleri kullanın.


## <a name="prerequisites"></a>Önkoşullar

- Başlamadan önce gözden [sık sorulan sorular](site-recovery-faq.md#failover) yük devretme hakkında.
- Her şeyin beklenildiği gibi çalışıp çalışmadığını denetlemek için bir [olağanüstü durum kurtarma tatbikatını](azure-to-azure-tutorial-dr-drill.md) tamamladığınızdan emin olun.
- Yük devretme testini çalıştırmadan önce VM özelliklerini doğrulayın. VM, [Azure gereksinimlerine](azure-to-azure-support-matrix.md#replicated-machine-operating-systems) uymalıdır.

## <a name="run-a-failover-to-the-secondary-region"></a>İkincil bölgeye yük devretme çalıştırma

1. **Çoğaltılmış öğeler** bölümünde, yük devretmek istediğiniz VM’yi seçin > **Yük devretme**

   ![Yük devretme](./media/azure-to-azure-tutorial-failover-failback/failover.png)

2. **Yük devretme** kısmında, yük devredeceğiniz bir **Kurtarma Noktası** seçin. Şu seçeneklerden birini kullanabilirsiniz:

   * **En son** (varsayılan): Site Recovery hizmetindeki tüm verileri işler ve en düşük kurtarma noktası hedefi (RPO) sağlar.
   * **En son işlenen**: Site Recovery hizmeti tarafından işlenen en son kurtarma noktasını sanal makineye geri döner.
   * **Özel**: Belirli kurtarma noktasına devreder. Bu seçenek, bir yük devretme testi gerçekleştirmek için faydalıdır.

3. Seçin **yük devretmeye başlamadan önce makineyi Kapat** Site Recovery, yük devretmeyi tetiklemeden önce kaynak sanal makineleri kapatmayı denemek istiyorsanız. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Site Recovery temiz kaynak yük devretme sonrasında yukarı değil.

4. Yük devretme ilerleme durumunu **İşler** sayfasından takip edin.

5. Yük devretmeden sonra, sanal makineyi doğrulamak için makinede oturum açın. Sanal makine için başka bir kurtarma noktasına gitmek istiyorsanız, **Kurtarma noktasını değiştir** seçeneğini kullanabilirsiniz.

6. Yük devredilmiş sanal makineden memnun kaldığınızda, yük devretmeyi **Yürütebilirsiniz**.
   Yürütme işlemi, hizmette kullanılabilir olan tüm kurtarma noktalarını siler. Artık kurtarma noktasını değiştirmesi mümkün olmayacaktır.

> [!NOTE]
> Sanal makine için çoğaltmayı etkinleştirdikten sonra bir disk eklemeniz VM yük devretme, çoğaltma noktaları kurtarma için kullanılabilir olan diskler gösterir. Örneğin, bir sanal makine tek bir diske sahiptir ve yeni bir tane ekleyin, disk eklemeden önce oluşturulan çoğaltma noktaları "2 disk 1" çoğaltma noktası içerdiğini gösterir.

![Eklenen bir disk ile yük devretme](./media/azure-to-azure-tutorial-failover-failback/failover-added.png)

## <a name="reprotect-the-secondary-vm"></a>İkincil VM’yi yeniden koruma

VM’nin yük devretmesinden sonra, birincil bölgeye geri çoğaltması için VM’yi yeniden korumalısınız.

1. VM’nin **Yük devretme yürütüldü** durumunda olduğundan emin olun ve birincil bölgenin kullanılabilir olduğunu ve içinde yeni kaynaklar oluşturup bunlara erişebildiğinizi denetleyin.
2. **Kasa** > **Çoğaltılmış öğeler** bölümünde, yük devredilmiş VM’ye sağ tıklayın ve sonra **Yeniden Koru** seçeneğini belirleyin.

   ![Yeniden korumaya sağ tıklayın](./media/azure-to-azure-tutorial-failover-failback/reprotect.png)

2. Koruma, birincil bölgeden yönünü zaten seçili olduğunu doğrulayın.
3. **Kaynak grubu, Ağ, Depolama ve Kullanılabilirlik kümeleri** bilgilerini gözden geçirin. Yeni olarak işaretli tüm kaynaklar yeniden koruma işleminin bir parçası olarak oluşturulur.
4. Yeniden koruma işini tetiklemek için **Tamam**’a tıklayın. Bu iş, hedef siteye en son verileri sağlar. Ardından, deltaları birincil bölgeye çoğaltır. VM artık korunan bir durumdadır.

## <a name="next-steps"></a>Sonraki adımlar
- Yeniden korunuyor sonra [öğrenin nasıl](azure-to-azure-tutorial-failback.md) uygun olduğunda birincil bölgeye geri başarısız.
- [Daha fazla bilgi edinin](azure-to-azure-how-to-reprotect.md#what-happens-during-reprotection) yeniden koruma akışla ilgili.
