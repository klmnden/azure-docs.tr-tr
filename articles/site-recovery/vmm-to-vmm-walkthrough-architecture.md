---
title: "Azure Site Recovery ile ikincil siteye Hyper-V çoğaltma işleminin mimarisini inceleme | Microsoft Docs"
description: "Bu makalede, Azure Site Recovery ile ikincil System Center VMM sitesine şirket içi Hyper-V VM’lerini çoğaltma mimarisine genel bir bakış sunulmaktadır."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 07099161-4cc7-4f32-8eb9-2a71bbf0750b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/30/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: b78cd0d5a5395873afaddc8856004775f447e8ea
ms.contentlocale: tr-tr
ms.lasthandoff: 08/01/2017

---
# <a name="step-1-review-the-architecture-for-hyper-v-replication-to-a-secondary-site"></a>1. Adım: İkincil siteye Hyper-V çoğaltma işleminin mimarisini inceleme

Bu makalede, Azure portalından [Azure Site Recovery](site-recovery-overview.md) hizmeti kullanılarak, System Center Virtual Machine Manager (VMM) bulutlarındaki Hyper-V sanal makinelerini (VM) ikincil bir VMM sitesine çoğaltırken kullanılan bileşenler ve işlemler açıklanmaktadır.

Tüm yorumlarınızı bu makalenin alt kısmında veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda paylaşabilirsiniz.



## <a name="architectural-components"></a>Mimari bileşenler

Hyper-V VM’lerini ikincil bir VMM sitesine çoğaltmak için aşağıdakiler gerekir.

**Bileşen** | **Konum** | **Ayrıntılar**
--- | --- | ---
**Azure** | Azure aboneliği. | VMM konumları arasında çoğaltmayı düzenleyip yönetmek için Azure aboneliğinde bir Kurtarma Hizmetleri kasası oluşturun.
**VMM sunucusu** | Birincil ve ikincil VMM konumu gerekir. | Bir tane birincil sitede, bir tane de ikincil sitede VMM sunucusu olması önerilir 
**Hyper-V sunucusu** |  Birincil ve ikincil VMM bulutlarında bir veya daha fazla Hyper-V konak sunucusu. | Verilerin, Kerberos veya sertifika kimlik doğrulaması kullanılarak, LAN ya da VPN üzerinden birincil ve ikincil Hyper-V ana bilgisayar sunucuları arasında çoğaltılması gerekir.  
**Hyper-V VM’leri** | Hyper-V ana bilgisayar sunucusunda. | Kaynak ana bilgisayar sunucusunda çoğaltmak istediğiniz en az bir VM olması gerekir.

## <a name="replication-process"></a>Çoğaltma işlemi

1. Azure hesabını ayarlayın, bir Kurtarma Hizmetleri kasası oluşturun ve çoğaltmak istediğiniz öğeleri belirtin.
2. VMM sunucularına Azure Site Recovery Sağlayıcısını ve her bir Hyper-V ana bilgisayarına Microsoft Azure Kurtarma Hizmetleri aracısını yüklemeyi de içeren kaynak ve hedef çoğaltma ayarlarını yapılandırın.
3. Kaynak VMM bulutu için bir çoğaltma ilkesi oluşturun. İlke, buluttaki tüm konaklarda yer alan tüm VM’lere uygulanır.
4. Her VMM için çoğaltmayı etkinleştirdiğinizde, ilk VM çoğaltma işlemi seçtiğiniz ayarlara uygun olarak gerçekleşir.
5. İlk çoğaltma sonrasında, delta değişikliklerin çoğaltılması başlar. Bir öğe için izlenen değişiklikler bir .hrl dosyasında saklanır.


![Şirket içinden şirket içine](./media/vmm-to-vmm-walkthrough-architecture/arch-onprem-onprem.png)

## <a name="failover-and-failback-process"></a>Yük devretme ve yeniden çalışma işlemi

1. Şirket içi siteler arasında planlanmış veya planlanmamış bir [yük devretme](site-recovery-failover.md) gerçekleştirebilirsiniz. Planlı bir yük devretme çalıştırırsanız, veri kaybı olmaması için kaynak VM’ler kapatılır.
2. Tek bir makine üzerinden yük devredebilir veya [kurtarma planları](site-recovery-create-recovery-plans.md) oluşturarak birden çok makinenin devredilmesini düzenleyebilirsiniz.
4. İkincil bir siteye yönelik planlanmamış bir yük devretme gerçekleştirirseniz, işlem tamamlandıktan sonra ikincil konumdaki makineler koruma veya çoğaltma için etkinleştirilmez. Planlı bir yük devretme gerçekleştirdiyseniz, işlemden sonra ikincil konumdaki makineler korunur.
5. Daha sonra, kopya VM’deki iş yüküne erişmeye başlamak için yük devretmeyi yürütürsünüz.
6. Birincil sitenizi yeniden kullanılabilir duruma geldiğinde ters çoğaltma başlatarak ikincil siteden birincil siteye çoğaltma gerçekleştirirsiniz. Ters çoğaltma sanal makineleri korumalı bir duruma getirir, ancak ikincil veri merkezi hala etkin konumdur.
7. Birincil siteyi yeniden etkin konum durumuna getirmek için ikincil siteden birincil siteye planlı yük devretme başlatır ve arkasından başka bir ters çoğaltma gerçekleştirirsiniz.



## <a name="next-steps"></a>Sonraki adımlar

[2. Adım: Önkoşulları ve sınırlamaları gözden geçirme](vmm-to-vmm-walkthrough-prerequisites.md) bölümüne gidin.

