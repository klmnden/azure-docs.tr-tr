---
title: "System Center VMM Hyper-V çoğaltma Azure için hazırlama | Microsoft Docs"
description: "System Center VMM sunucusu Hyper-V çoğaltma Azure Site Kurtarma'yı kullanarak Azure için hazırlamayı açıklar"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: afcd81ae-d192-4013-a0af-3dac45b3c7e9
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: ec118ed837dbf140083b3ae1e4ecd41c81562018
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2017
---
# <a name="step-6-prepare-vmm-servers-and-hyper-v-hosts-for-hyper-v-replication-to-azure"></a>6. adım: Hyper-V çoğaltma Azure için VMM sunucuları ve Hyper-V ana bilgisayarları hazırlama

Ayarladıktan sonra [Azure bileşenleri](vmm-to-azure-walkthrough-prepare-azure.md) şirket içi VMM sunucuları ve Azure Site Recovery ile etkileşim kurmak için Hyper-V konakları hazırlamak için bu makaledeki dağıtımı için yönergeleri kullanın.

Bu makaleyi okuduktan sonra altındaki bir yorum gönderin ya da teknik sorular [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prepare-vmm-servers"></a>VMM sunucuları hazırlama

- Site Recovery çoğaltma (site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers) için destek gereksinimlerini karşılayan en az bir VMM sunucusu gerekir.
- VMM sunucusu için hazır olduğundan emin olun [ağ eşlemesi](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).
- VMM sunucusunun bu URL'leri erişebildiğinden emin olun

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- IP adresi tabanlı güvenlik duvarı kurallarına sahipseniz bu kuralların Azure ile iletişim kurmaya izin verdiğinden emin olun.
- [Azure Veri Merkezi IP Aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653)'na ve HTTPS (443) bağlantı noktasına izin verin.
- Aboneliğinizin Azure bölgesi ve Batı ABD (Access Control ve Identity Management için kullanılır) için IP adresi aralıklarına izin verin.

Site Recovery dağıtımı sırasında Site kurtarma Sağlayıcısı'nı indirin ve her VMM sunucusuna yükleyin. VMM sunucusu kurtarma Hizmetleri kasasına kayıtlı.




## <a name="next-steps"></a>Sonraki adımlar

Git [adım 7: bir kasa oluşturun](vmm-to-azure-walkthrough-create-vault.md)

