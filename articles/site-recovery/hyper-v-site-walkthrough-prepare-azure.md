---
title: "Azure Site Kurtarma'yı kullanarak Azure (System Center VMM olmadan) Hyper-V sanal makineleri çoğaltmak için Azure kaynaklarını hazırlama | Microsoft Docs"
description: "Hyper-V Vm'lerini (VMM olmadan) Azure Site RECOVERY'yi kullanarak Azure'a çoğaltma başlamadan önce Azure yerinde gerekenler açıklanmaktadır"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 28fa722c-675e-4637-98eb-7ccbf3806d69
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: 1a30cadaab7e053184f0be133f1da5bfddc1fd91
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="step-5-prepare-azure-resources-for-hyper-v-replication-to-azure"></a>5. adım: Hyper-V çoğaltma Azure Azure kaynaklarını hazırlama

Azure kullanarak şirket içi Hyper-V sanal makinelerini (System Center VMM olmadan) çoğaltabilirsiniz böylece Azure kaynaklarını hazırlamak için bu makaledeki yönergeleri kullanın [Azure Site Recovery](site-recovery-overview.md) hizmet.

Bu makaleyi okuduktan sonra altındaki bir yorum gönderin ya da teknik sorular [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Başlamadan önce

Okuma olduğundan emin olun [önkoşulları](hyper-v-site-walkthrough-prerequisites.md)

## <a name="set-up-an-azure-account"></a>Bir Azure hesabı ayarlama

- Alma bir [Microsoft Azure hesabı](http://azure.microsoft.com/).
- [Ücretsiz deneme sürümüyle](https://azure.microsoft.com/pricing/free-trial/) başlayabilirsiniz.
- Site Recovery, altında coğrafi kullanılabilirlik kısmına desteklenen bölgeleri kontrol [Azure Site Recovery fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/).
- Hakkında bilgi edinin [Site Recovery fiyatlandırma](site-recovery-faq.md#pricing)ve alma [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/).


## <a name="set-up-an-azure-network"></a>Azure ağı ayarlama

- Azure ağı ayarlama ayarlayın. Yük devretme sonrasında oluşturulduğunda azure sanal makineleri bu ağda yerleştirilir.
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

Git [adım 6: hazırlama Hyper-V kaynakları](hyper-v-site-walkthrough-prepare-hyper-v.md)
