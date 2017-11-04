---
title: "Yük devri ve başarısız geri Azure Azure Site Recovery (Önizleme) ikincil bir Azure bölgesiyle çoğaltılan VM'ler"
description: "Yük devri ve Azure Site Recovery ile ikincil bir Azure bölgesine geri Azure VM'ler çoğaltması başarısız hakkında bilgi edinin"
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/01/2017
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 02b709bb8dbab5d10ce9f4cf6155ff26ce229298
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="fail-over-and-fail-back-azure-vms-between-azure-regions-preview"></a>Yük devri ve Azure bölgeler (Önizleme) arasında geri Azure VM'ler başarısız

[Azure Site Recovery](site-recovery-overview.md) hizmeti yönetme ve çoğaltma, yük devretme ve yeniden çalışma şirket içi makineler ve Azure sanal makineleri (VM'ler) yönetme olağanüstü durum kurtarma stratejiniz katkıda bulunur.

Bu öğretici, ikincil bir Azure bölgesine tek bir Azure VM'ye yük devri açıklar. Devredilir sonra kullanılabilir olduğunda birincil bölgesine başarısız. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure VM başarısız
> * Böylece birincil bölge çoğaltır ikincil Azure VM koruyun
> * Geri ikincil VM başarısız
> * Birincil VM ikincil bölge geri koruyun

## <a name="prerequisites"></a>Ön koşullar

- Artık tamamladığınıza yapma bir [olağanüstü durum kurtarma ayrıntıya](azure-to-azure-tutorial-dr-drill.md) her şeyi çalıştığını beklendiği gibi denetlemek için.
- Yük devretme testini çalıştırmadan önce VM özelliklerini doğrulayın. VM uymalıdır [Azure gereksinimleri](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

## <a name="run-a-failover-to-the-secondary-region"></a>İkincil bölge'ye bir yük devretmeyi çalıştırma

1. İçinde **öğeleri çoğaltılan**, devretmek istediğiniz VM seçin > **yük devretme**

   ![Yük devretme](./media/azure-to-azure-tutorial-failover-failback/failover.png)

2. İçinde **yük devretme**seçin bir **kurtarma noktası** devretmesini. Aşağıdaki seçeneklerden birini kullanabilirsiniz:

   * **En son** (varsayılan): Bu seçenek Site kurtarma hizmetindeki tüm verileri işler ve en düşük kurtarma noktası hedefi (RPO) sağlar.
   * **En son işlenen**: Bu seçenek Site Recovery hizmeti tarafından işlenen en son kurtarma noktası sanal makineye geri döner.
   * **Özel**: belirli kurtarma noktasına devretmek için bu seçeneği kullanın. Bu seçenek, bir yük devretme testi gerçekleştirmek için yararlıdır.

3. Seçin **yük devretme işlemine başlamadan önce makineyi kapatın** Site Kurtarma'nın bir kapanma kaynak sanal makinelerin yük devretme tetiklemeden yapmaya istiyorsanız. Kapatma başarısız olsa bile yük devretme devam eder.

4. Yük devretme işleminin ilerleyişini izleyin **işleri** sayfası.

5. Yük devretme işleminden sonra sanal makine için oturum açarak doğrulayın. Sanal makine için başka bir kurtarma noktası gitmek istiyorsanız sonra kullanabileceğiniz **değiştirmek kurtarma noktası** seçeneği.

6. Başarısız oldu memnun sonra sanal makine üzerinde yapabilecekleriniz **yürütme** yük devretme.
   Yürütme hizmeti ile kullanılabilen tüm kurtarma noktalarını siler. **Değiştirmek kurtarma noktası** seçenek, artık kullanılabilir.

## <a name="reprotect-the-secondary-vm"></a>İkincil VM koruyun

VM yük devretme sonrasında birincil bölgesine çoğaltır, böylece yeniden korumanız gerekir.

1. VM içinde olduğundan emin olun **kaydedilen yük devretme** durum ve birincil bölge kullanılabilir ve oluşturabilir ve bunu yeni kaynaklara erişim denetleyin.
2. İçinde **kasa** > **öğeleri çoğaltılan**üzerinden başarısız VM'ye sağ tıklayın ve ardından **yeniden koruma**.

   ![Yeniden korumak için sağ tıklatın](./media/azure-to-azure-tutorial-failover-failback/reprotect.png)

2. Koruma, birincil bölge için ikincil yönünü zaten seçili dikkat edin.
3. Gözden geçirme **kaynak grubu, ağ, depolama ve kullanılabilirlik kümeleri** bilgi. (Yeni) olarak işaretlenmiş kaynakları yeniden koruma işleminin bir parçası olarak oluşturulur.
4. Tıklatın **Tamam** yeniden koruma işini tetikleyecek. Bu işin en son verileri hedef siteyle çekirdeğini oluşturur. Ardından, birincil bölge farkları çoğaltır. VM, artık korunan bir durumda değil.

## <a name="fail-back-to-the-primary-region"></a>Birincil bölgesine başarısız

Sanal makineleri korunmuş sonra gerektiği gibi birincil bölgesine başarısız olabilir. Bunu yapmak için izleyin [yük devretme](#run-a-failover) yönergeler.
