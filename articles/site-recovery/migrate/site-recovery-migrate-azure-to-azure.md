---
title: "Azure bölgeler arasında Azure Iaas Vm'leri geçirme | Microsoft Docs"
description: "Azure Iaas sanal makineleri bir Azure bölgesinden diğerine geçirmek için Azure Site Recovery kullanın."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8a29e0d9-0010-4739-972f-02b8bdf360f6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: raynew
ms.openlocfilehash: f645150aa7b93ebbd98899cc8fdd495dfeaf3076
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>Azure Site Recovery ile Azure bölgeler arasında Azure Iaas sanal makineleri geçirme

Azure Site Recovery'ye hoş geldiniz! Azure VM'ler Azure bölgeler arasında geçirmek istiyorsanız bu makaleyi kullanın.
>[!NOTE]
>
> Olağanüstü durum kurtarma ve geçiş ihtiyaçlarınız için başka bir bölge için Azure sanal makineleri çoğaltmak için başvurmak [bu belgeyi](../site-recovery-azure-to-azure.md). Azure sanal makineler için Site Recovery çoğaltma şu anda önizlemede değil.


## <a name="prerequisites"></a>Ön koşullar
İşte bu dağıtım için gerekenler:

* **Iaas sanal makineleri**: geçiş yapmak istediğiniz sanal makineleri. Bu sanal makineleri fiziksel makineleri düşünerek geçirin.

## <a name="deployment-steps"></a>Dağıtım adımları
Bu bölümde, yeni Azure portalında dağıtım adımları açıklanmaktadır.

1. [Bir kasa oluşturun](../site-recovery-azure-to-azure.md#create-a-recovery-services-vault).
2. [Çoğaltmayı etkinleştirin]... /(Site-Recovery-Azure-to-Azure.MD) geçirmek ve Azure kaynağı olarak seçmek istediğiniz VM'ler için.
  >[!NOTE]
  >
  > Şu anda yönetilen diskleri kullanarak Azure VM'ler yerel çoğaltmasını desteklenmez. "Azure fiziksel için" seçeneğini kullanabilirsiniz [bu belgeyi](../site-recovery-vmware-to-azure.md) yönetilen diskler ile sanal makineleri geçirmek için.
3. [Yük devretme gerçekleştirme](../site-recovery-failover.md). İlk çoğaltma tamamlandıktan sonra bir yük devretme bir Azure bölgesinden diğerine çalıştırabilirsiniz. İsteğe bağlı olarak, bir kurtarma planı oluşturmak ve birden çok sanal makine bölgeler arasında geçirmek için bir yük devretme çalıştırın. [Daha fazla bilgi edinin](../site-recovery-create-recovery-plans.md) kurtarma planları hakkında.

## <a name="next-steps"></a>Sonraki adımlar
Diğer çoğaltma senaryolarda hakkında daha fazla bilgi [Azure Site Recovery nedir?](../site-recovery-overview.md)
