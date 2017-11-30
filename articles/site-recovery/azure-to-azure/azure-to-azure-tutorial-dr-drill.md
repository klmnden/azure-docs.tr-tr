---
title: "Bir olağanüstü durum kurtarma ayrıntısı seçeneğini, Azure Site Recovery (Önizleme) ile ikincil bir Azure bölgesine Azure VM'ler için çalıştırın."
description: "Azure Site Recovery hizmetini kullanarak bir ikincil bir Azure bölgesine Azure VM'ler için olağanüstü durum kurtarma ayrıntıya çalıştırmayı öğrenin."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: d763865bb0b86b1bdf15258a3caf36561dd3385c
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2017
---
# <a name="run-a-disaster-recovery-drill-for-azure-vms-to-a-secondary-azure-region-preview"></a>Bir olağanüstü durum kurtarma ayrıntıya Azure VM'ler için ikincil bir Azure bölgesine (Önizleme) çalıştırın.

[Azure Site Recovery](../site-recovery-overview.md) hizmeti tarafından iş uygulamalarınızı çalışır halde tutmaktan planlanan ve planlanmayan kesintiler sırasında kullanılabilir iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkı. Site Recovery yönetir ve şirket içi makineler ve Azure sanal makineleri (VM'ler), çoğaltma, yük devretme ve kurtarma gibi olağanüstü durum kurtarma düzenler.

Bu öğretici bir Azure bölgesinden diğerine yük devretme testi ile bir Azure VM için bir olağanüstü durum kurtarma ayrıntıya çalıştırılacağını gösterir. Bir detaylandırma çoğaltma stratejinizi veri kaybı veya kapalı kalma süresi olmadan doğrular ve üretim ortamınıza etkilemez. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Önkoşul denetimi
> * Tek bir VM için yük devretme testi çalıştırma

## <a name="prerequisites"></a>Ön koşullar

- Yük devretme testi çalıştırmadan önce her şeyin beklendiği gibi olduğundan emin olmak için VM özelliklerini doğrulamanız önerilir.  VM Özellikleri'nde erişim **öğeleri çoğaltılan**. **Essentials** dikey makineler ayarlarını ve durumu hakkında bilgileri gösterir.
- Yük devretme sınaması ve çoğaltma etkin olduğunda, ayarlanmış varsayılan ağ için ayrı bir Azure VM ağ kullanmanızı öneririz.


## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

1. İçinde **ayarları** > **çoğaltılan öğeler**, VM tıklatın **+ yük devretme testi** simgesi.

2. İçinde **yük devretme testi**, yük devretme için kullanılacak bir kurtarma noktası seçin:

   - **En son işlenen**: VM üzerinden Site Recovery hizmeti tarafından işlenen en son kurtarma noktası başarısız olur. Zaman damgası gösterilir. Bu seçenek, hiçbir zaman düşük RTO (Kurtarma süresi hedefi) sağlayan verileri işleme harcanan
   - **Son uygulama tutarlı**: tüm sanal makineleri en son uygulamayla tutarlı kurtarma noktası için bu seçeneği yöneltilir. Zaman damgası gösterilir.
   - **Özel**: herhangi bir kurtarma noktası seçin.

3. Hedef Azure seçin ikincil bölge içindeki Azure Vm'lerinin sanal ağa bağlı olacak, yük devretme gerçekleştikten sonra.

4. Yük devretmeyi başlatmak için tıklatın **Tamam**. İlerleme durumunu izlemek için VM özelliklerini açmak için tıklatın. Ya da tıklayabilirsiniz **yük devretme testi** kasa adını işinde > **ayarları** > **işleri** > **Site Recovery işleri**.
5. Yük devretme işlemi tamamlandıktan sonra çoğaltma Azure VM Azure portalında görünür. > **sanal makineleri**. VM, uygun şekilde boyutlandırılmış ve uygun bir ağa bağlı çalıştığından emin olun.
6. Yük devretme testi sırasında oluşturulan sanal makineleri silmek için tıklatın **temizleme yük devretme testi** çoğaltılmış öğesi ya da kurtarma planı. İçinde **notları**, kaydetme ve yük devretme testiyle ilişkili gözlemlerinizi kaydetmek.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir üretim yük devretmeyi çalıştırma](azure-to-azure-tutorial-failover-failback.md)
