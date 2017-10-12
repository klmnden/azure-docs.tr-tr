---
title: "Azure Site Recovery kullanarak Hyper-V'den Azure'a çoğaltma işleminin (System Center VMM ile) gereksinimlerini inceleme | Microsoft Docs"
description: "VMM bulutlarındaki şirket içi Hyper-V sanal makinelerini Azure'da çoğaltma, yük devretme ve kurtarma için Azure Site Recovery aracılığıyla ayarlamaya yönelik önkoşulları açıklar."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a1c30fd5-c979-473c-af44-4f725ad3e3ba
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/24/2017
ms.author: raynew
ms.openlocfilehash: 47c178c66ec98fe5d333edd725b64465026e73ed
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="step-2-review-the-prerequisites-for-hyper-v-with-vmm-to-azure-replication"></a>2. adım: Hyper-V’den (VMM ile) Azure’a çoğaltma önkoşullarını gözden geçirin.

[Senaryo mimarisini](vmm-to-azure-walkthrough-architecture.md) inceledikten sonra, dağıtım önkoşulları anladığınızdan emin olmak için bu makaleyi okuyun. 

## <a name="prerequisites-and-limitations"></a>Önkoşullar ve sınırlamalar

**Gereksinim** | **Ayrıntılar**
--- | ---
**Azure hesabı** | Bir [Microsoft Azure hesabınızın](http://azure.microsoft.com/) olması gerekir.
**Azure depolama alanı** | Çoğaltılan verileri depolamak için bir Azure depolama hesabınızın olması gerekir.<br/><br/> Depolama hesabının, Azure Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.<br/><br/>[Coğrafi olarak yedekli depolama](../storage/common/storage-redundancy.md#geo-redundant-storage) veya yerel olarak yedekli depolama kullanabilirsiniz. Coğrafi olarak yedekli depolama kullanmanızı öneririz. Coğrafi olarak yedekli depolama ile, bölgesel bir kesintinin meydana gelmesi veya birincil bölgenin kurtarılamaması durumunda veriler korunur.<br/><br/> Standart Azure depolama hesabı kullanabilir veya Azure [premium depolama](../storage/common/storage-premium-storage.md) kullanabilirsiniz. Premium depolama, G/Ç yoğun iş yüklerini destekler ve genellikle tutarlı bir şekilde yüksek G/Ç performansı ve düşük gecikme süresi gerektiren VM'ler için kullanılır. Çoğaltılan veriler için premium depolama kullanırsanız, ayrı bir standart depolama hesabı gerekir. Standart depolama hesabı şirket içi verilerde gerçekleşen değişiklikleri yakalamak çoğaltma günlüklerinde depolar.
**Azure ağı** | Yük devretme işleminden sonra Azure VM'lerinin bağlanacağı bir [Azure ağınızın](../virtual-network/virtual-network-get-started-vnet-subnet.md) olması gerekir. Azure ağının, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.
**Şirket içi VMM sunucuları** | System Center 2012 R2 veya sonraki sürümlerini çalıştıran bir veya daha fazla VMM sunucunuzun olması gerekir.<br/><br/> Her VMM sunucusunun bir veya daha fazla özel bulutu olmalıdır. Her bulut bir veya birden fazla konak grubuna sahip olmalıdır.<br/><br/> VMM sunucusunun İnternet erişimi olmalıdır.
**Şirket içi Hyper-V** | Hyper-V konağı sunucularının, en az Microsoft Hyper-V Server 2012 R2 veya Hyper-V rolü etkin Windows Server 2012 R2 çalıştırması gerekir. En son güncelleştirmeler yüklü olmalıdır.<br/><br/> Hyper-V konağının bir VMM konak grubunda (VMM bulutundadır) bulunması gerekir.<br/><br/> Bir konakta, çoğaltılmasını istediğiniz bir veya daha fazla sanal makine olması gerekir.<br/><br/> Hyper-V konaklarının Azure’a çoğaltılması için doğrudan veya proxy ile İnternet’e bağlı olması gerekir. Hyper-V sunucularında [2961977](https://support.microsoft.com/kb/2961977) numaralı makalede belirtilen düzeltmeler olmalıdır.
**Şirket içi Hyper-V VM'leri** | Çoğaltmak istediğiniz VM'lerin [desteklenen işletim sistemi](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) çalıştırıyor olması ve [Azure önkoşullarına](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) uygun olması gerekir. Çoğaltma etkinleştirildikten sonra VM adı değiştirilebilir. 




## <a name="next-steps"></a>Sonraki adımlar

[3. Adım: Kapasiteyi planlama](vmm-to-azure-walkthrough-capacity.md)’ya gidin.
