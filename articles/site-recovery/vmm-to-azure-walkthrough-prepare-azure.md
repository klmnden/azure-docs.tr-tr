---
title: "Azure Site Kurtarma'yı kullanarak Azure (System Center VMM ile) Hyper-V sanal makineleri çoğaltmak için Azure kaynaklarını hazırlama | Microsoft Docs"
description: "Azure Site RECOVERY'yi kullanarak Azure'a, Hyper-V Vm'lerini (VMM ile) çoğaltma başlamadan önce Azure yerinde gerekenler açıklanmaktadır"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 1568bdc3-e767-477b-b040-f13699ab5644
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 63b005f37ab5e15e8a1b4645446d65f1529f1bbd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="step-5-prepare-azure-resources-for-hyper-v-replication-with-vmm-to-azure"></a>5. adım: (VMM ile) Hyper-V çoğaltma Azure için Azure kaynaklarını hazırlama

Doğruladıktan sonra [ağ gereksinimleri](vmm-to-azure-walkthrough-network.md), böylece System Center Virtual Machine Manager (VMM) bulutlarında şirket içi Hyper-V Vm'lerini Azure'a çoğaltabilirsiniz Azure kaynaklarını hazırlamak için bu makaledeki yönergeleri kullanın kullanarak [Azure Site Recovery](site-recovery-overview.md) hizmet.

Bu makaleyi okuduktan sonra altındaki bir yorum gönderin ya da teknik sorular [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="set-up-an-azure-account"></a>Bir Azure hesabı ayarlama

- Alma bir [Microsoft Azure hesabı](http://azure.microsoft.com/).
- [Ücretsiz deneme sürümüyle](https://azure.microsoft.com/pricing/free-trial/) başlayabilirsiniz.
- Site Recovery, altında coğrafi kullanılabilirlik kısmına desteklenen bölgeleri kontrol [Azure Site Recovery fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/).
- Hakkında bilgi edinin [Site Recovery fiyatlandırma](site-recovery-faq.md#pricing)ve alma [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/).
- Azure hesabınızda doğru olduğundan emin olun [izinleri](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines)Azure VM'ler oluşturmak için. [Daha fazla bilgi edinin](../active-directory/role-based-access-built-in-roles.md) Azure rol tabanlı erişim denetimi hakkında.


## <a name="set-up-an-azure-network"></a>Azure ağı ayarlama

- Ayarlanmış bir [Azure ağ](../virtual-network/virtual-network-get-started-vnet-subnet.md). Yük devretme sonrasında oluşturulduğunda azure sanal makineleri bu ağda yerleştirilir.
- Ağın kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir
- Azure portalında Site Recovery ayarlanan ağlar kullanabilir [Resource Manager](../resource-manager-deployment-model.md), ya da Klasik modda.
- Başlamadan önce bir ağ ayarlamanızı öneririz. Aksi takdirde, Site Recovery dağıtımı sırasında yapmanız gerekir.
- Hakkında bilgi edinin [sanal ağ fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-network/).


## <a name="set-up-an-azure-storage-account"></a>Azure depolama hesabı ayarlama

- Site Recovery, şirket içi makineler Azure depolama alanına çoğaltır. Yük devretme gerçekleştikten sonra azure VM'ler depolama biriminden oluşturulur.
- Standart/Premium'u ayarlama ayarlamak [Azure depolama hesabı](../storage/common/storage-create-storage-account.md#create-a-storage-account) Azure'a çoğaltılan verileri tutmak için.
- [Premium depolama](../storage/common/storage-premium-storage.md) genellikle bir tutarlı bir şekilde yüksek g/ç performans ve düşük gecikme süresi konak g/ç yoğun iş yükleri için gereken sanal makineleri için kullanılır.
- Çoğaltılan veriler için bir premium depolama hesabı kullanmak istiyorsanız, şirket içi verilerde gerçekleşen değişiklikleri yakalayan çoğaltma günlüklerini depolamak üzere ek bir standart depolama hesabı da ayarlamanız gerekir.
- Kullanmak istediğiniz kaynak modeline bağlı olarak Azure vm'lerinde hesabı ayarlamanız [Resource Manager moduna](../storage/common/storage-create-storage-account.md), veya [Klasik modda](../storage/common/storage-create-storage-account.md).
- Başlamadan önce bir depolama hesabı ayarlamanız önerilir. Bunu yapmazsanız Site Recovery dağıtımı sırasında yapmak için gerekir. Hesapları kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.
- Site Recovery tarafından kullanılan aynı abonelik içindeki kaynak grupları arasında veya farklı Aboneliklerdeki depolama hesaplarına taşınamıyor.


## <a name="next-steps"></a>Sonraki adımlar

Git [6. adım: VMM hazırlama](vmm-to-azure-walkthrough-vmm-hyper-v.md)
