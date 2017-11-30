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
ms.date: 08/31/2017
ms.author: raynew
ms.openlocfilehash: 805582d89ee906e21fbe588e895ce0fe3a926a66
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2017
---
# <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>Azure Site Recovery ile Azure bölgeler arasında Azure Iaas sanal makineleri geçirme

Bu makalede Azure sanal makineleri (VM'ler) geçirmek istediğiniz kullanarak Azure bölgeleri arasında kullanırsanız [Site Recovery](../site-recovery-overview.md) hizmet.

## <a name="prerequisites"></a>Ön koşullar

Iaas Vm'lerine geçirmek istediğiniz gerekir.

## <a name="deployment-steps"></a>Dağıtım adımları

1. [Bir kasa oluşturun](azure-to-azure-tutorial-enable-replication.md#create-a-vault).
2. [Çoğaltmayı etkinleştirme](azure-to-azure-tutorial-enable-replication.md#enable-replication) geçirmek ve Azure kaynağı olarak seçmek istediğiniz VM'ler için. Azure VM'lerin yönetilen diskleri kullanarak yerel çoğaltma şu anda desteklenmiyor.
3. [Yük devretme gerçekleştirme](../site-recovery-failover.md). İlk çoğaltma tamamlandıktan sonra bir yük devretme bir Azure bölgesinden diğerine çalıştırabilirsiniz. İsteğe bağlı olarak, bir kurtarma planı oluşturmak ve birden çok sanal makine bölgeler arasında geçirmek için bir yük devretme çalıştırın. [Daha fazla bilgi edinin](../site-recovery-create-recovery-plans.md) kurtarma planları hakkında.

## <a name="next-steps"></a>Sonraki adımlar
Diğer çoğaltma senaryolarda hakkında bilgi edinin [Azure Site Recovery nedir?](../site-recovery-overview.md)
