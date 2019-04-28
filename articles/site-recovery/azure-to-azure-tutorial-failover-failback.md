---
title: Yük devretme ve Azure Site Recovery hizmeti ile olağanüstü durum kurtarma için ikincil Azure bölgesine çoğaltılmış Azure Vm'lerini yeniden koruyun.
description: Yük devretme ve Azure Site Recovery hizmeti ile olağanüstü durum kurtarma için ikincil Azure bölgesine çoğaltılmış Azure Vm'lerini yeniden koruma hakkında bilgi edinin.
services: site-recovery
author: rockboyfor
manager: digimobile
ms.service: site-recovery
ms.topic: tutorial
origin.date: 04/08/2019
ms.date: 04/22/2019
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 96e3c0b761a9ed4c5f84d8ece1ba504bd5aacf6f
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62115967"
---
# <a name="fail-over-and-reprotect-azure-vms-between-regions"></a>Yük devretme ve bölgeleri arasında Azure Vm'lerini yeniden koruma

[Azure Site Recovery](site-recovery-overview.md) hizmeti, şirket içi makinelerin ve Azure sanal makinelerinin (VM) çoğaltma, yük devretme ve yeniden çalışma işlemlerini yöneterek ve düzenleyerek olağanüstü durum kurtarma stratejinize katkı sağlar.

Bu öğreticide bir Azure sanal makinesi için ikincil Azure bölgesine yük devretme işlemini açıklamaktadır. Yük devrettikten sonra VM'yi yeniden koruyun. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure VM’ye yük devretme
> * Birincil bölgeye çoğaltır, böylece ikincil Azure VM'yi yeniden koruyun.

> [!NOTE]
> Bu öğreticide, varsayılan ayarlar ve en az özelleştirme en basit yolu içerir. Daha karmaşık senaryolarda, Azure Vm'leri için 'Nasıl yapılır' altında makaleleri kullanın.

## <a name="prerequisites"></a>Önkoşullar

- Her şeyin beklenildiği gibi çalışıp çalışmadığını denetlemek için bir [olağanüstü durum kurtarma tatbikatını](azure-to-azure-tutorial-dr-drill.md) tamamladığınızdan emin olun.
- Yük devretme testini çalıştırmadan önce VM özelliklerini doğrulayın. VM, [Azure gereksinimlerine](azure-to-azure-support-matrix.md#replicated-machine-operating-systems) uymalıdır.

<a name="run-a-failover"></a>
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

## <a name="reprotect-the-secondary-vm"></a>İkincil VM’yi yeniden koruma

VM’nin yük devretmesinden sonra, birincil bölgeye geri çoğaltması için VM’yi yeniden korumalısınız.

1. VM’nin **Yük devretme yürütüldü** durumunda olduğundan emin olun ve birincil bölgenin kullanılabilir olduğunu ve içinde yeni kaynaklar oluşturup bunlara erişebildiğinizi denetleyin.
2. **Kasa** > **Çoğaltılmış öğeler** bölümünde, yük devredilmiş VM’ye sağ tıklayın ve sonra **Yeniden Koru** seçeneğini belirleyin.

    ![Yeniden korumaya sağ tıklayın](./media/azure-to-azure-tutorial-failover-failback/reprotect.png)

2. Koruma, birincil bölgeden yönünü zaten seçili olduğunu doğrulayın.
3. **Kaynak grubu, Ağ, Depolama ve Kullanılabilirlik kümeleri** bilgilerini gözden geçirin. Yeni olarak işaretli tüm kaynaklar yeniden koruma işleminin bir parçası olarak oluşturulur.
4. Yeniden koruma işini tetiklemek için **Tamam**’a tıklayın. Bu iş, hedef siteye en son verileri sağlar. Ardından, deltaları birincil bölgeye çoğaltır. VM artık korunan bir durumdadır.

## <a name="next-steps"></a>Sonraki adımlar
- Bulunmayı sonra [öğrenin nasıl](azure-to-azure-tutorial-failback.md) uygun olduğunda birincil bölgeye geri başarısız.
- [Daha fazla bilgi edinin](azure-to-azure-how-to-reprotect.md#what-happens-during-reprotection) yeniden koruma akışla ilgili.

<!-- Update_Description: update meta properties, wording update -->