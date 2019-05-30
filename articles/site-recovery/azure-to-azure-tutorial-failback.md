---
title: Azure Site Recovery hizmeti ile olağanüstü durum kurtarma için ikincil bir Azure bölgesinde çoğaltılmasını geri Azure Vm'leri başarısız.
description: Azure Site Recovery hizmeti ile geri Azure Vm'leri başarısız öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 05/30/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: a3b67e9b0dc41eeb14000400912892fbf29acfe2
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399493"
---
# <a name="fail-back-an-azure-vm-between-azure-regions"></a>Azure bölgeleri arasında Azure VM ilk duruma döndürme

[Azure Site Recovery](site-recovery-overview.md) Hizmet Yönetimi ve çoğaltma, yük devretme ve şirket içi makinelerin ve Azure sanal makineleri (VM) yeniden çalışma işlemlerini olağanüstü durum kurtarma stratejinize katkıda bulunur.

Bu öğreticide, tek bir Azure VM geri dönecek şekilde açıklar. Yük devrettikten sonra uygun olduğunda birincil bölgeye geri başarısız olmalıdır. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> 
> * İkincil bölgedeki VM ilk duruma döndürme.
> * Birincil VM geri ikincil bölgeye yeniden koruyun.
> 
> [!NOTE]
> 
> Bu öğretici, hedef bölge ve kaynak bölge en düşük özelleştirmelerle geri için birkaç sanal makine yük devretmek için yardımcı olur. Daha fazla ayrıntılı yönergeler için gözden [nasıl yapılır kılavuzları Azure Vm'lerinde](https://docs.microsoft.com/azure/virtual-machines/windows/).

## <a name="before-you-start"></a>Başlamadan önce

* Sanal Makinenin durumu olduğundan emin olun **yük devretme yürütüldü**.
* Birincil bölgenin kullanılabilir olduğunu ve oluşturabilmek ve yeni kaynaklarına erişimi denetleyin.
* Bu yeniden koruma etkin olduğundan emin olun.

## <a name="fail-back-to-the-primary-region"></a>Birincil bölgeye geri dönme

VM'ler yeniden korunduktan sonra gerektiği gibi birincil bölgeye geri dönebilirsiniz.

1. Kasada seçin **çoğaltılan öğeler**ve ardından yeniden korumaya alınmış VM seçin.

    ![Birincile yeniden çalışma](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback.png)

3. Seçin **yük devretme testi** yük devretme testi gerçekleştirmek için birincil bölgeye geri.
4. Yük devretme testi için sanal ağ ve kurtarma noktası seçin ve ardından **Tamam**. Birincil bölgede oluşturulan VM test gözden geçirebilirsiniz.
5. Yük devretme testi başarıyla tamamlandıktan sonra Seç **yük devretme testini Temizle** yük devretme testi için kaynak bölgede oluşturulan kaynakları temizlemek için.
6. İçinde **çoğaltılan öğeler**VM'yi seçin ve ardından **yük devretme**.
7. İçinde **yük devretme**, yük devretme için bir kurtarma noktası seçin:
    - **En son (varsayılan)** : Site Recovery hizmetindeki tüm verileri işler ve en düşük kurtarma noktası hedefi (RPO) sağlar.
    - **En son işlenen**: VM'nin Site Recovery tarafından işlenen en son kurtarma noktasına geri döner.
    - **Özel**: Belirli kurtarma noktasına devreder. Bu seçenek, bir yük devretme testi gerçekleştirmek için faydalıdır.

8. Seçin **yük devretmeye başlamadan önce makineyi Kapat** Site Recovery, yük devretmeyi tetiklemeden önce kaynak sanal makineleri kapatmayı denemesini istiyorsanız. Kapatma başarısız olsa bile yük devretme devam eder. Site Recovery temiz kaynak yük devretme sonrasında yukarı değil olduğunu unutmayın.
9. Yük devretme ilerleme durumunu **İşler** sayfasından takip edin.
10. Yük devretme işlemi tamamlandıktan sonra VM için oturum açarak doğrulayın. Kurtarma noktası gerektiği gibi değiştirebilirsiniz.
11. Yük devretme doğruladıktan sonra seçin **yük devretmeyi yürütürsünüz**. İşleme, tüm kullanılabilir kurtarma noktalarını siler. Değişiklik kurtarma noktası seçeneği artık kullanılabilir.
12. VM üzerinde başarısız olarak göstermesi gerekir ve geri başarısız oldu.

    ![Birincil ve ikincil bölgeler VM](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback-vm-view.png)

> [!NOTE]
> Olağanüstü durum kurtarma VM kapatma/serbest bırakılmış durumda kalır. Site Recovery birincil daha sonra ikincil bölgeye yük devretme için yararlı olabilir VM bilgilerini gerektirmediğinden, bu tasarım gereğidir. Böylece bunlar tutulması gereken şekilde serbest VM'ler için ücretlendirilmezsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Daha fazla bilgi edinin](azure-to-azure-how-to-reprotect.md#what-happens-during-reprotection) yeniden koruma akışla ilgili.
