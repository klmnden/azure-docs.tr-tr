---
title: "Hyper-V çoğaltma Azure Site Recovery ile ikincil VMM siteye önkoşullarını gözden geçirme | Microsoft Docs"
description: "Azure Site Recovery ile ikincil VMM sitesi için Hyper-V Vm'lerini çoğaltma önkoşulları açıklanmaktadır."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 21ff0545-8be5-4495-9804-78ab6e24add6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 7897a30bf1774609ca8e6037dabcd5fbf4151271
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="step-2-review-the-prerequisites-and-limitations-for-hyper-v-vm-replication-to-a-secondary-vmm-site"></a>2. adım: Önkoşullar ve sınırlamalar Hyper-V VM çoğaltma ikincil VMM sitesi için gözden geçirin


Gözden geçirdikten sonra [senaryo mimarisinin](vmm-to-vmm-walkthrough-architecture.md), şirket içi Hyper-V sanal makineleri (VM'ler) çoğaltma kullanarak bir ikincil site için System Center Virtual Machine Manager (VMM) bulutlarında yönetilen dağıtım önkoşulları anladığınızdan emin olmak için bu makaleyi okuyun [Azure Site Recovery](site-recovery-overview.md) Azure portalında.

Bu makaleyi okuduktan sonra yapmak istediğiniz tüm yorumları makalenin alt kısmında veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda paylaşabilirsiniz.


## <a name="prerequisites-and-limitations"></a>Önkoşullar ve sınırlamalar

**Gereksinim** | **Ayrıntılar**
--- | ---
**Azure** | A [Microsoft Azure](http://azure.microsoft.com/) abonelik.<br/><br/> [Ücretsiz deneme sürümüyle](https://azure.microsoft.com/pricing/free-trial/) başlayabilirsiniz.<br/><br/> Site Recovery fiyatlandırması hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/site-recovery/).<br/><br/> Site Recovery, altında coğrafi kullanılabilirlik kısmına desteklenen bölgeleri kontrol [Azure Site Recovery fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/).
**VMM sunucuları** | İki VMM sunucusu, bir birincil site ve bir ikincil sahip öneririz.<br/><br/> Tek bir Bulutlar arasında çoğaltma VMM sunucusunda desteklenir.<br/><br/> VMM sunucuları çalışıyor. en son güncelleştirmeleri içeren System Center 2012 SP1.<br/><br/> VMM sunucuları Internet erişimine gerek vardır.
**VMM Bulutları** | Her bir VMM sunucusunun bir veya daha fazla bulut olmalıdır ve tüm Bulutlar Hyper-V Kapasite profili kümesine sahip olması gerekir. <br/><br/>Bulut bir veya daha fazla VMM ana bilgisayar grubu içermesi gerekir.<br/><br/> Yalnızca bir VMM sunucunuz varsa, birincil ve ikincil davranmak üzere en az iki bulut gerekir.
**Hyper-V** | Hyper-V sunucuları çalışıyor olmalıdır en az Windows Server 2012 Hyper-V rolüne sahip ve en son güncelleştirmelerin yüklü olması.<br/><br/> Bir Hyper-V sunucusunun bir veya daha fazla VM içermesi gerekir.<br/><br/>  Hyper-V ana bilgisayar sunucularının birincil ve ikincil VMM bulutlarında konak gruplarındaki bulunmalıdır.<br/><br/> Windows Server 2012 R2'de bir kümede Hyper-V çalıştırırsanız, yükleme [2961977 güncelleştir](https://support.microsoft.com/kb/2961977)<br/><br/> Windows Server 2012'de bir kümede Hyper-V çalıştırırsanız, bir statik IP adresi tabanlı kümeniz varsa küme aracısının otomatik olarak oluşturulmaz. Küme aracısını el ile yapılandırın. [Daha fazla bilgi](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).<br/><br/> Hyper-V sunucuları Internet erişimine gerek vardır.




## <a name="next-steps"></a>Sonraki adımlar

Git [3. adım: ağ planlama](vmm-to-vmm-walkthrough-network.md).
