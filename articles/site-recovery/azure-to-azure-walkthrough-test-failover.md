---
title: "Azure Site Recovery ile yük devretme sınaması Azure VM çoğaltması için çalıştırma | Microsoft Docs"
description: "Azure Site Recovery hizmetini kullanarak başka bir Azure bölgesine Azure Vm'lerini çoğaltma için bir yük devretme testi çalıştırmak için gereken adımları özetler."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: e15c1b0c-5d75-4fdf-acb0-e61def9e9339
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: 8babb0d016729f318442af93596d206c38d91206
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="step-6-run-a-test-failover-for-azure-vm-replication"></a>6. adım: bir yük devretme sınaması için Azure VM çoğaltma çalıştırma

Azure sanal makinesi (VM) için çoğaltma etkinleştirdikten sonra bir Azure bölgesinden diğerine kullanarak yük devretme testi çalıştırmak için bu makaledeki adımları [Azure Site Recovery](site-recovery-overview.md) Azure portalında hizmet.

- Makaleyi tamamladığınızda, doğrulandı bir test yük devretmesi ile en az bir Azure VM, ikincil bir Azure bölgesine yük devredebildiğini. 
- Bu makalenin sonundaki yorumları gönderin ya da sorular sormak [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)

>[!NOTE]
>
> Azure VM çoğaltma şu anda önizlemede değil.


## <a name="before-you-start"></a>Başlamadan önce

- Yük devretme testi çalıştırmadan önce VM özelliklerini doğrulayın ve istediğiniz değişiklikleri yapın öneririz. VM Özellikleri'nde erişebilirsiniz **öğeleri çoğaltılan**. **Essentials** dikey makineler ayarlarını ve durumu hakkında bilgileri gösterir.
- Yük devretme sınaması ve ağ için ayrı bir Azure VM ağ kullanmanız önerilir (varsayılan veya özelleştirilmiş), ayarlanan üretim yük devretme için.
- İkincil bir Azure bölgesine Azure Vm'leri (ve bunların depolama) yük devretme sınaması başarısız olur. Herhangi bir bağımlı uygulamaları veya kaynakları kopyalamaz. VM'ler başarısız çalışan uygulamaları Active Directory veya DNS gibi başka kaynaklar bağımlı olması durumunda zaten ikincil Bölgenizde kullanılabilir değillerse, bunlar çok, çoğaltma gerekir. [Daha fazla bilgi](site-recovery-test-failover-to-azure.md#prepare-active-directory-and-dns)
- Bir şirket içi siteden yük devretme sonrasında çoğaltılmış VM'ler erişmek isterseniz, gerek [bağlamaya hazırlanmak](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover) bu VM'ler için.

## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

1. İçinde **ayarları** > **çoğaltılan öğeler**, VM tıklatın **+ yük devretme testi** simgesi. 

2. İçinde **yük devretme testi**, yük devretme için kullanılacak bir kurtarma noktası seçin:

    - **En son işlenen**: VM üzerinden Site Recovery hizmeti tarafından işlenen en son kurtarma noktası başarısız olur. Zaman damgası gösterilir. Bu seçenek ile düşük RTO (Kurtarma süresi hedefi) sağlayan verileri işleme hiçbir zaman harcanmaktadır.
    - **Son uygulama tutarlı**: tüm sanal makineleri en son uygulamayla tutarlı kurtarma noktası için bu seçeneği yöneltilir. Zaman damgası gösterilir. 
    - **Özel**: herhangi bir kurtarma noktası seçin.
 
3. Hedef Azure seçin ikincil bölge içindeki Azure Vm'lerinin sanal ağa bağlı olacak, yük devretme gerçekleştikten sonra.
4. Yük devretmeyi başlatmak için tıklatın **Tamam**. İlerleme durumunu izlemek için VM özelliklerini açmak için tıklatın. Ya da tıklayabilirsiniz **yük devretme testi** kasa adını işinde > **ayarları** > **işleri** > **Site Recovery işleri**.
5. Yük devretme işlemi tamamlandıktan sonra çoğaltma Azure VM Azure portalında görünür. > **sanal makineleri**. VM, uygun bir ağa bağlı olduğu uygun boyutta olduğundan ve çalıştığından emin olun.
6. Yük devretme testi sırasında oluşturulan sanal makineleri silmek için tıklatın **temizleme yük devretme testi** çoğaltılmış öğesi ya da kurtarma planı. İçinde **notları**, kaydetme ve yük devretme testiyle ilişkili gözlemlerinizi kaydetmek. 

[Daha fazla bilgi edinin](site-recovery-test-failover-to-azure.md) yük devretme sınaması işlemlerini hakkında.

## <a name="next-steps"></a>Sonraki adımlar

Yük devretme test ettikten sonra bu kılavuzda tamamlanır. Şimdi, yük devretme işlemlerini üretimde çalıştırma hakkında bilgi alın:

- [Daha fazla bilgi edinin](site-recovery-failover.md) farklı türdeki yük devretmeler ve bunları çalıştırma hakkında.
- Birden çok VM başarısız hakkında daha fazla bilgi [bir kurtarma planı kullanarak](site-recovery-create-recovery-plans.md).
- Daha fazla bilgi edinmek [kurtarma planlarına kullanarak](site-recovery-create-recovery-plans.md).
- Daha fazla bilgi edinmek [Azure sanal makineleri yeniden korumayı](site-recovery-how-to-reprotect.md) yük devretme sonrasında.

