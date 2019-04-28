---
title: Azure Site Recovery hizmeti ile olağanüstü durum kurtarma için ikincil bir Azure bölgesinde çoğaltılmasını geri Azure Vm'leri başarısız.
description: Azure Site Recovery hizmeti ile geri Azure Vm'leri başarısız öğrenin.
services: site-recovery
author: rockboyfor
manager: digimobile
ms.service: site-recovery
ms.topic: tutorial
origin.date: 03/18/2019
ms.date: 04/22/2019
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: c8ce05e644ad556542314b17151b808586734824
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62118188"
---
# <a name="fail-back-azure-vms-between-azure-regions"></a>Azure bölgeleri arasında geri Azure Vm'leri başarısız

[Azure Site Recovery](site-recovery-overview.md) hizmeti yönetme ve çoğaltma, yük devretme ve yeniden şirket içi makinelerin ve Azure sanal makineleri (VM'ler), başarısız işlemlerini olağanüstü durum kurtarma stratejinize katkıda bulunur.

Bu öğreticide, tek bir Azure VM geri dönecek şekilde açıklar. Yük devrettikten sonra, uygun olduğunda birincil bölgeye geri dönersiniz. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> 
> * İkincil bölgedeki Azure VM geri dönecek.
> * Birincil Azure VM geri ikincil bölgeye yeniden koruyun.
> 
> [!NOTE]
> 
> Bu öğretici, hedef bölge ve kaynak bölge en düşük özelleştirmelerle geri için birkaç sanal makine yük devretmek için yardımcı olur. Daha fazla ayrıntılı yönergeler için Azure Vm'leri için 'Nasıl yapılır' altında makaleleri inceleyin.

## <a name="before-you-start"></a>Başlamadan önce

> * Sanal makine olduğundan emin olun **yük devretme yürütüldü** durumu.
> * Birincil bölgenin kullanılabilir olduğunu ve oluşturabilmek ve yeni kaynaklarına erişimi denetleyin.
> * Yeniden koruma etkin olduğundan emin olun.

## <a name="fail-back-to-the-primary-region"></a>Birincil bölgeye geri dönme

Vm'leri yeniden koruma altına sonra gerektiği gibi birincil bölgeye geri dönebilirsiniz.

1. Kasaya tıklayarak **çoğaltılan öğeler** ve yeniden korumalı VM'yi seçin.

    ![Birincile yeniden çalışma](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback.png)

3. Tıklayın **sınama Yük Devretmesini** yük devretme testi gerçekleştirmek için birincil bölgeye geri.
4. Yük devretme testi için sanal ağ ve kurtarma noktası seçin ve tıklayın **Tamam**. Birincil bölgede oluşturulan VM test gözden geçirebilirsiniz.
5. Yük devretme testi başarıyla tamamladıktan sonra **yük devretme testini Temizle** yük devretme testi için kaynak bölgede oluşturulan kaynakları temizlemek için.
6. İçinde **çoğaltılan öğeler**, sanal Makineyi seçin ve tıklayın **yük devretme**.
7. İçinde **yük devretme**, yük devretme için bir kurtarma noktası seçin.
    - **En son (varsayılan)**: Site Recovery hizmetindeki tüm verileri işler ve en düşük kurtarma noktası hedefi (RPO) sağlar.
    - **En son işlenen**: VM'nin Site Recovery tarafından işlenen en son kurtarma noktasına geri döner.
    - **Özel**: Belirli kurtarma noktasına devreder. Bu seçenek, bir yük devretme testi gerçekleştirmek için faydalıdır.

8. Seçin **yük devretmeye başlamadan önce makineyi Kapat** Site Recovery, yük devretmeyi tetiklemeden önce kaynak sanal makineleri kapatmayı denemek istiyorsanız. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Site Recovery temiz kaynak yük devretme sonrasında yukarı değil olduğunu unutmayın.
9. Yük devretme ilerleme durumunu **İşler** sayfasından takip edin.
10. Yük devretme işleminden sonra VM için oturum açarak doğrulayın. Kurtarma noktası gerektiği gibi değiştirebilirsiniz.
11. Yük devretme doğruladıktan sonra tıklayın **yük devretmeyi yürütürsünüz**. İşleme, tüm kullanılabilir kurtarma noktalarını siler. Değişiklik kurtarma noktası seçeneği artık kullanılabilir.
12. VM üzerinde başarısız olarak göstermesi gerekir ve geri başarısız oldu.

    ![Birincil ve ikincil bölgeler VM](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback-vm-view.png)

> [!NOTE]
> Olağanüstü durum kurtarma VM kapatma durumu serbest kalır. Site Recovery birincil daha sonra ikincil bölgeye yük devretme için yararlı olabilir VM bilgilerini gerektirmediğinden, bu tasarım gereğidir. Böylece bunlar tutulması gereken şekilde serbest VM'ler için ücretlendirilmezsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Daha fazla bilgi edinin](azure-to-azure-how-to-reprotect.md#what-happens-during-reprotection) yeniden koruma akışla ilgili.

<!--Update_Description: new articles on azure to azure tutorial failback -->
<!--ms.date: 04/22/2019-->