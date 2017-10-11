---
title: "Azure bölgeler arasında Azure Vm'lerini çoğaltma | Microsoft Docs"
description: "Azure portalında Azure Site Recovery hizmeti ile Azure bölgeler arasında Azure Vm'lerini çoğaltma için gereken adımları özetler"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: 6dd36239-4363-4538-bf80-a18e71b8ec67
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: 9258613161a61e36b1d0c5796d5763c916d66859
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="replicate-azure-vms-between-regions-with-azure-site-recovery"></a>Azure Site Recovery ile bölgeler arasında Azure Vm'lerini çoğaltma

>Bu makalede Azure vm'lerine farklı bir bölgede bir Azure bölgesinde Azure sanal makine (VM) çoğaltmak için gerekli olan adımları genel bir bakış sağlar. 

>[!NOTE]
>
> Azure VM çoğaltma şu anda önizlemede değil.

Bu makalenin veya üzerinde altındaki açıklamaları ve soruları sonrası [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="step-1-review-architecture"></a>1. adım: Gözden geçirme mimarisi

Dağıtıma başlamadan önce senaryo mimarisinin ve dağıtmak için ihtiyaç duydukları bileşenleri gözden geçirin.

Git [1. adım: mimarisi gözden geçirin](azure-to-azure-walkthrough-architecture.md)


## <a name="step-2-review-prerequisites"></a>2. adım: Gözden geçirme önkoşulları

Abonelik, sanal ağlar, depolama hesapları ve VM gereksinimleri çeşitli yerlerde Azure önkoşulları olup olmadığını denetleyin.

Git [2. adım: Önkoşullar ve sınırlamalar doğrulayın](azure-to-azure-walkthrough-prerequisites.md)


## <a name="step-3-plan-networking"></a>3. adım: ağ planlama

Giden bağlantı çoğaltmak istediğiniz Azure Vm'lerinde ayarlama ve şirket içi bağlantılarından ayarlanır denetleyin.

Git [4. adım: ağ planı](azure-to-azure-walkthrough-network.md)



## <a name="step-4-create-a-vault"></a>4. adım: bir kasa oluşturma 

Düzenlemek ve çoğaltmayı yönetmek için bir kurtarma Hizmetleri kasasını oluşturup ve kaynak bölge belirtmeniz gerekir.

Git [4. adım: bir kasa oluşturun](azure-to-azure-walkthrough-vault.md)


## <a name="step-5-enable-replication"></a>5. adım: Çoğaltma etkinleştirme


Çoğaltmayı etkinleştirmek için hedef konum ayarları yapılandırmak, bir çoğaltma ilkesini ayarlayın ve çoğaltmak istediğiniz Azure sanal makineleri seçin. Etkinleştirdikten sonra VM başlangıç çoğaltması gerçekleşir.

Git [5. adım: çoğaltmasını etkinleştir](azure-to-azure-walkthrough-enable-replication.md)


## <a name="step-6-run-a-test-failover"></a>6. adım: yük devretme testi çalıştırma

İlk çoğaltma sonlandırıldıktan ve değişim çoğaltması çalıştıran sonra her şeyin beklendiği gibi çalıştığından emin olmak için yük devretme testi çalıştırabilirsiniz.

Git [6. adım: yük devretme testi çalıştırma](azure-to-azure-walkthrough-test-failover.md)


