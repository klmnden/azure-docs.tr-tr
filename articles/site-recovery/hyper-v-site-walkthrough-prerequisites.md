---
title: "Azure Site Kurtarma'yı kullanarak Azure çoğaltma (System Center VMM olmadan) Hyper-V için önkoşulları gözden | Microsoft Docs"
description: "Çoğaltma, yük devretme ve şirket içi Hyper-V sanal makineleri kurtarma Azure Site Recovery ile azure'a ayarlamak için Önkoşullar açıklanmaktadır"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 7ef3fb46-52f5-4c8a-b1a1-658c2305762a
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: cbb5d3598ef91512991d7d1e9f854eb12980752b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="step-2-review-the-prerequisites-for-hyper-v-without-vmm-to-azure-replication"></a>2. adım: Azure çoğaltma için Hyper-V (VMM olmadan) önkoşulları gözden geçirin

Önkoşullar tabloda özetlenmiştir.


**Önkoşul** | **Ayrıntılar** 
--- | --- 
**Azure** | [Azure gereksinimleri](site-recovery-prereq.md#azure-requirements) hakkında bilgi edinin.
**Şirket içi sunucular** | [Daha fazla bilgi edinin](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm) şirket içi Hyper-V konakları için gereksinimleri hakkında.
**Şirket içi Hyper-V VM'leri** | Çoğaltmak istediğiniz VM'lerin [desteklenen işletim sistemi](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) çalıştırıyor olması ve [Azure önkoşullarına](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) uygun olması gerekir.
**Azure URL'leri** | Hyper-V konakları bu URL'lere erişim gerekir:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> IP adresi tabanlı güvenlik duvarı kurallarına sahipseniz bu kuralların Azure ile iletişim kurmaya izin verdiğinden emin olun.<br/></br> [Azure Veri Merkezi IP Aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653)'na ve HTTPS (443) bağlantı noktasına izin verin.<br/></br> Aboneliğinizin Azure bölgesi ve Batı ABD (Access Control ve Identity Management için kullanılır) için IP adresi aralıklarına izin verin.



## <a name="next-steps"></a>Sonraki adımlar

- Tam dağıtımını yaptığınız, Git [3. adım: kapasite planlama](hyper-v-site-walkthrough-capacity.md)
- Basit test dağıtımını yaptığınız, Git [4. adım: ağ planlama](hyper-v-site-walkthrough-network.md).
