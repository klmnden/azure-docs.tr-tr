---
title: Azure Site Recovery ile ikincil bir Azure bölgesine çoğaltılmış Azure VM’lere yük devretme veya VM’leri geri döndürme
description: Azure Site Recovery ile ikincil bir Azure bölgesine çoğaltılmış Azure VM’lere nasıl yük devredeceğinizi veya bu VM’leri nasıl geri döndüreceğinizi öğrenin
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 05/15/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 4a27142f9110fd26daa8ea0ebd151a67769e6568
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34209282"
---
# <a name="fail-over-and-fail-back-azure-vms-between-azure-regions"></a>Azure bölgeleri arasında Azure VM yük devretme ve ilk duruma döndürme

[Azure Site Recovery](site-recovery-overview.md) hizmeti, şirket içi makinelerin ve Azure sanal makinelerinin (VM) çoğaltma, yük devretme ve yeniden çalışma işlemlerini yöneterek ve düzenleyerek olağanüstü durum kurtarma stratejinize katkı sağlar.

Bu öğretici, tek bir Azure VM’den ikincil bir Azure bölgesine nasıl yük devredeceğinizi açıklar. Yük devrettikten sonra, uygun olduğunda birincil bölgeye geri dönersiniz. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure VM’ye yük devretme
> * İkincil Azure VM’yi yeniden koruyarak birincil bölgeye çoğaltmasını sağlayın
> * İkincil VM’yi geri döndürme
> * Birincil VM’yi ikincil bölgeye yeniden koruyun

Azure’dan Azure’a çoğaltma şu anda önizlemededir.

## <a name="prerequisites"></a>Ön koşullar

- Her şeyin beklenildiği gibi çalışıp çalışmadığını denetlemek için bir [olağanüstü durum kurtarma tatbikatını](azure-to-azure-tutorial-dr-drill.md) tamamladığınızdan emin olun.
- Yük devretme testini çalıştırmadan önce VM özelliklerini doğrulayın. VM, [Azure gereksinimlerine](azure-to-azure-support-matrix.md#support-for-replicated-machine-os-versions) uymalıdır.

## <a name="run-a-failover-to-the-secondary-region"></a>İkincil bölgeye yük devretme çalıştırma

1. **Çoğaltılmış öğeler** bölümünde, yük devretmek istediğiniz VM’yi seçin > **Yük devretme**

   ![Yük devretme](./media/azure-to-azure-tutorial-failover-failback/failover.png)

2. **Yük devretme** kısmında, yük devredeceğiniz bir **Kurtarma Noktası** seçin. Şu seçeneklerden birini kullanabilirsiniz:

   * **En son** (varsayılan): Bu seçenek, Site Recovery hizmetindeki tüm verileri işler ve en düşük Kurtarma Noktası Hedefi (RPO) sağlar.
   * **En son işlenen**: Bu seçenek sanal makineyi, Site Recovery hizmeti tarafından işlenmiş en son kurtarma noktasına geri alır.
   * **Özel**: Belirli bir kurtarma noktasına yük devretmek için bu seçeneği kullanın. Bu seçenek, bir yük devretme testi gerçekleştirmek için faydalıdır.

3. Yük devretmeyi tetiklemeden önce Site Recovery’nin kaynak sanal makineleri kapatma girişiminde bulunmasını istiyorsanız, **Yük devretme başlamadan önce makineyi kapat** seçeneğini belirleyin. Kapatma işlemi başarısız olsa bile yük devretme devam eder.

4. Yük devretme ilerleme durumunu **İşler** sayfasından takip edin.

5. Yük devretmeden sonra, sanal makineyi doğrulamak için makinede oturum açın. Sanal makine için başka bir kurtarma noktasına gitmek istiyorsanız, **Kurtarma noktasını değiştir** seçeneğini kullanabilirsiniz.

6. Yük devredilmiş sanal makineden memnun kaldığınızda, yük devretmeyi **Yürütebilirsiniz**.
   Yürütme işlemi, hizmette kullanılabilir olan tüm kurtarma noktalarını siler. **Kurtarma noktasını değiştir** seçeneği artık kullanılabilir değil.

## <a name="reprotect-the-secondary-vm"></a>İkincil VM’yi yeniden koruma

VM’nin yük devretmesinden sonra, birincil bölgeye geri çoğaltması için VM’yi yeniden korumalısınız.

1. VM’nin **Yük devretme yürütüldü** durumunda olduğundan emin olun ve birincil bölgenin kullanılabilir olduğunu ve içinde yeni kaynaklar oluşturup bunlara erişebildiğinizi denetleyin.
2. **Kasa** > **Çoğaltılmış öğeler** bölümünde, yük devredilmiş VM’ye sağ tıklayın ve sonra **Yeniden Koru** seçeneğini belirleyin.

   ![Yeniden korumaya sağ tıklayın](./media/azure-to-azure-tutorial-failover-failback/reprotect.png)

2. Birincil bölgeden sonra gelen koruma yönünün zaten seçili olduğuna dikkat edin.
3. **Kaynak grubu, Ağ, Depolama ve Kullanılabilirlik kümeleri** bilgilerini gözden geçirin. (Yeni) olarak işaretli tüm kaynaklar, yeniden koruma işleminin bir parçası olarak oluşturulmuştur.
4. Yeniden koruma işini tetiklemek için **Tamam**’a tıklayın. Bu iş, hedef siteye en son verileri sağlar. Ardından, deltaları birincil bölgeye çoğaltır. VM artık korunan bir durumdadır.

## <a name="fail-back-to-the-primary-region"></a>Birincil bölgeye geri dönme

VM’ler yeniden korunduktan sonra, gerektiği gibi birincil bölgeye geri dönebilirsiniz. Bunu yapmak için, [yük devretme](#run-a-failover) yönergelerini takip edin.
