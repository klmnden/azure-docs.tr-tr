---
title: Bir Azure IaaS VM’si için ikincil Azure bölgesine olağanüstü durum kurtarma ayarlama
description: Bu hızlı başlangıçta Azure Site Recovery hizmetini kullanarak farklı Azure bölgeleri arasında Azure IaaS VM olağanüstü durum kurtarma gerçekleştirmek için gerekli adımlar gösterilmektedir.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: quickstart
ms.date: 10/28/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: e5f6cbec75a9f652cf05c9608c412e00489288b9
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212939"
---
# <a name="set-up-disaster-recovery-to-a-secondary-azure-region-for-an-azure-vm"></a>Bir Azure VM’si için ikincil Azure bölgesine olağanüstü durum kurtarma ayarlama 

[Azure Site Recovery](site-recovery-overview.md) hizmeti, planlı ve plansız kesintiler sırasında iş uygulamalarınızı çalışır durumda tutarak, iş sürekliliğinize ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur. Site Recovery, şirket içi makinelerin ve Azure sanal makinelerinin çoğaltma, yük devretme ve kurtarma gibi olağanüstü durum kurtarma işlemlerini yönetir ve düzenler.

Bu hızlı başlangıç, bir Azure VM’nin farklı bir Azure bölgesine nasıl çoğaltılacağını açıklar.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!NOTE]
> Bu makale, yeni kullanıcıları Azure Site Recovery deneyimleri boyunca varsayılan seçenekler ve minimum özelleştirme ile yönlendirmeyi amaçlamaktadır. Özelleştirilebilen çeşitli ayarlar hakkında daha fazla bilgi almak istiyorsanız [Azure VM’leri için çoğaltmayı etkinleştirme öğreticisi](azure-to-azure-tutorial-enable-replication.md)’ne danışın

## <a name="log-in-to-azure"></a>Azure'da oturum açma

http://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="enable-replication-for-the-azure-vm"></a>Azure VM için çoğaltmayı etkinleştirme

1. Azure portalında, **Sanal makineler** seçeneğine tıklayın ve çoğaltmak istediğiniz VM’yi seçin.
2. **İşlemler** menüsünde **Olağanüstü durum kurtarma** seçeneğine tıklayın.
3. **Olağanüstü durumdan kurtarma yapılandırma** > **Hedef bölge** bölümünde, çoğaltma yapacağınız hedef bölgeyi seçin.
4. Bu Hızlı Başlangıç için, diğer varsayılan ayarları kabul edin.
5. **Çoğaltmayı etkinleştir**’e tıklayın. Bu, sanal makineye yönelik çoğaltmayı etkinleştirmek için bir iş başlatır.

    ![çoğaltmayı etkinleştir](media/azure-to-azure-quickstart/enable-replication1.png)

## <a name="verify-settings"></a>Ayarları doğrulama

Çoğaltma işlemi bittikten sonra, çoğaltma durumunu denetleyebilir, çoğaltma ayarlarını değiştirebilir ve dağıtımı test edebilirsiniz.

1. VM menüsünde, **Olağanüstü durum kurtarma** seçeneğine tıklayın.
2. Çoğaltma durumunu, oluşturulan kurtarma noktalarını ve haritadaki kaynak ve hedef bölgelerini doğrulayabilirsiniz.

   ![Çoğaltma durumu](media/azure-to-azure-quickstart/replication-status.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Çoğaltma işlemini devre dışı bıraktığınızda, birincil bölgedeki VM çoğaltmayı durdurur:

- Kaynak çoğaltma ayarları otomatik olarak temizlenir.
- VM’nin Site Recovery faturalaması da ayrıca durur.

Şu adımlara göre çoğaltmayı durdurun:

1. VM’yi seçin.
2. **Olağanüstü durum kurtarma** bölümünde **Çoğaltmayı devre dışı bırak**'a tıklayın.

   ![Çoğaltmayı devre dışı bırakma](media/azure-to-azure-quickstart/disable2-replication.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, tek bir VM’yi ikincil bir bölgeye çoğalttınız. Şimdi daha fazla seçeneği keşfedebilir ve bir keşif planı kullanarak bir dizi Azure VM’sini çoğaltmayı deneyebilirsiniz.

> [!div class="nextstepaction"]
> [Azure VM’leri için olağanüstü durum kurtarmayı yapılandır](azure-to-azure-tutorial-enable-replication.md)
