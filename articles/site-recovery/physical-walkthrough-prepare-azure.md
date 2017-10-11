---
title: "Azure Site Recovery Azure'u şirket içi fiziksel sunucuları çoğaltmak için Azure kaynaklarını hazırlama | Microsoft Docs"
description: "Şirket içi sunucular Azure Site Recovery hizmetini kullanarak Azure'a, çoğaltma başlamadan önce Azure yerinde gerekenler açıklanmaktadır"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 4e320d9b-8bb8-46bb-ba21-77c5d16748ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/25/2017
ms.author: raynew
ms.openlocfilehash: b7411fa6aba04ffd34f3f4bd03e706ca75afc9c8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="step-5-prepare-azure-resources-for-physical-server-replication-to-azure"></a>5. adım: Fiziksel sunucu çoğaltma Azure Azure kaynaklarını hazırlama


Böylece Azure kullanarak şirket içi sunucular çoğaltabilirsiniz Azure kaynaklarını hazırlamak için bu makaledeki yönergeleri kullanın [Azure Site Recovery](site-recovery-overview.md) hizmet.

POST açıklamaları ve soruları alt bu makalenin veya üzerinde [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Başlamadan önce

Okuma olduğundan emin olun [Önkoşullar](physical-walkthrough-prerequisites.md).

## <a name="set-up-an-azure-account"></a>Bir Azure hesabı ayarlama

- Alma bir [Microsoft Azure hesabı](http://azure.microsoft.com/).
- [Ücretsiz deneme sürümüyle](https://azure.microsoft.com/pricing/free-trial/) başlayabilirsiniz.
- Site Recovery için desteklenen bölgeler altında denetleyin **coğrafi kullanılabilirlik** içinde [Azure Site Recovery fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/).
- Hakkında bilgi edinin [Site Recovery fiyatlandırma](site-recovery-faq.md#pricing)ve alma [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/).



## <a name="set-up-an-azure-network"></a>Azure ağı ayarlama

- Azure ağı ayarlama ayarlayın. Yük devretme sonrasında oluşturulduğunda azure sanal makineleri bu ağda yerleştirilir.
- Azure portalında Site Recovery ayarlanan ağlar kullanabilir [Resource Manager](../resource-manager-deployment-model.md), ya da Klasik modda.
- Ağın, Kurtarma Hizmetleri kasasıyla aynı konumda olması gerekir.
- Hakkında bilgi edinin [sanal ağ fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-network/).
- Daha fazla bilgi edinmek [Azure VM bağlantı](physical-walkthrough-network.md) yük devretme sonrasında.


## <a name="set-up-an-azure-storage-account"></a>Azure depolama hesabı ayarlama

- Site Recovery, şirket içi sunucuları Azure depolama alanına çoğaltır. Yük devretme gerçekleştikten sonra azure VM'ler depolama biriminden oluşturulur.
- Ayarlanmış bir [Azure depolama hesabı](../storage/common/storage-create-storage-account.md#create-a-storage-account) çoğaltılan veriler için.
- Azure portalında Site Recovery Kaynak Yöneticisi'nde veya Klasik modda ayarlanmış depolama hesaplarını kullanabilirsiniz.
- Depolama hesabı standart olabilir veya [premium](../storage/common/storage-premium-storage.md).
- Premium hesabınızı ayarlarsanız, ek bir standart hesap için günlük verileri gerekir.


## <a name="next-steps"></a>Sonraki adımlar

Git [adım 6: bir kasasını oluşturup](physical-walkthrough-create-vault.md)
