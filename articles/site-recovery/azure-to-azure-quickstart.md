---
title: "Başka bir Azure bölgesi (Önizleme) bir Azure VM Çoğalt"
description: "Bu hızlı başlangıç farklı bir bölgeye bir Azure bölgesindeki bir Azure VM çoğaltmak için gereken adımları sağlar."
services: site-recovery
author: rajani-janaki-ram
manager: carmonm
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: rajanaki
ms.custom: mvc
ms.openlocfilehash: 369ffed823bc76ee4273d7866935c0ddc7ffa515
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="replicate-an-azure-vm-to-another-azure-region-preview"></a>Başka bir Azure bölgesi (Önizleme) bir Azure VM Çoğalt

[Azure Site Recovery](site-recovery-overview.md) hizmeti tarafından iş uygulamalarınızı çalışır halde tutmaktan planlanan ve planlanmayan kesintiler sırasında kullanılabilir iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkı. Site Recovery yönetir ve şirket içi makineler ve Azure sanal makineleri (VM'ler), çoğaltma, yük devretme ve kurtarma gibi olağanüstü durum kurtarma düzenler.

Bu Hızlı Başlangıç, farklı bir Azure bölgesine bir Azure VM çoğaltmak açıklar.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

http://portal.azure.com sayfasından Azure portalda oturum açın.

## <a name="enable-replication-for-the-azure-vm"></a>Azure sanal makine için çoğaltmayı etkinleştirme

1. Azure portalında tıklatın **sanal makineleri**ve çoğaltmak istediğiniz VM seçin.

2. İçinde **ayarları**, tıklatın **olağanüstü durum kurtarma (Önizleme)**.
3. İçinde **olağanüstü durum kurtarma yapılandırma** > **hedef bölgesi** , çoğaltmak hedef bölgeyi seçin.
4. Bu Hızlı Başlangıç için diğer varsayılan ayarları kabul edin.
5. Tıklatın **çoğaltmasını etkinleştir**. Bu sanal makine için çoğaltmayı etkinleştirmek için bir iş başlatır.

    ![Çoğaltmayı etkinleştirme](media/azure-to-azure-quickstart/enable-replication1.png)



## <a name="verify-settings"></a>Ayarları doğrulayın

Çoğaltma işi tamamlandıktan sonra çoğaltma durumunu denetlemek, çoğaltma ayarlarını değiştirin ve dağıtımı test etme.

1. VM menüye tıklayın **olağanüstü durum kurtarma (Önizleme)**.
2. Çoğaltma durumunu, oluşturulmuş kurtarma noktaları ve kaynak doğrulayabilir ve hedef bölgeler harita üzerinde.

   ![Çoğaltma durumu](media/azure-to-azure-quickstart/replication-status.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Çoğaltmayı devre dışı bıraktığınızda çoğaltma VM birincil bölge içinde durdurur:

- Kaynak çoğaltma ayarları otomatik olarak temizlenir.
- Ayrıca VM için Site Recovery Faturalaması durdurulur.

Çoğaltma gibi durdurun:

1. VM seçin.
2. İçinde **olağanüstü durum kurtarma (Önizleme)**, tıklatın **daha fazla**.
3. Tıklatın **çoğaltma devre dışı bırakma**.

   ![Çoğaltma devre dışı bırak](media/azure-to-azure-quickstart/disable2-replication.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç bir ikincil bölge'ye tek bir VM'ye kopyalanan.

> [!div class="nextstepaction"]
> [Azure VM'ler için olağanüstü durum kurtarma yapılandırma](azure-to-azure-tutorial-enable-replication.md)
