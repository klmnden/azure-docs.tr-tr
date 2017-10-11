---
title: "Azure Site Recovery ile bölgeler arasında Azure VM repliction için bir kasa ayarlama | Microsoft Docs"
description: "Azure Site Kurtarma'yı kullanarak Azure bölgeler arasında Azure çoğaltma için bir kasa ayarlamak için gereken adımları özetler"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: 40472189-3d80-4963-b175-8bddcbc2f61f
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: e03d17992ee0b12049636e40188950bcc4a6f31e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="step-4-set-up-a-vault-for-azure-to-azure-replication"></a>4. adım: Azure için Azure'a çoğaltma için bir kasa ayarlayın

Sonra [ağlarını planlama](azure-to-azure-walkthrough-network.md), Azure sanal başka bir Azure bölgesine çoğaltma, kullanarak makineleri (VM'ler) için bir kasa ayarlamak için bu makaleyi kullanın [Azure Site Recovery](site-recovery-overview.md) Azure portalında hizmet.

- Makaleyi bitirdikten sonra ayarlanmış bir kurtarma Hizmetleri kasası olmalıdır.
- Tüm yorumlarınızı bu makalenin alt kısmında paylaşabilir veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda soru sorabilirsiniz.



>[!NOTE]
>
> Azure VM çoğaltma şu anda önizlemede değil.




## <a name="create-a-vault"></a>Bir kasa oluşturun

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

>[!NOTE]
>
> Kurtarma Hizmetleri kasası, sanal makineleri çoğaltmak istediğiniz konumda oluşturmanızı öneririz. Örneğin, hedef konumunuz Orta ise ABD, kasaya oluşturma **Orta ABD**.


## <a name="next-steps"></a>Sonraki adımlar

Git [5. adım: çoğaltmasını etkinleştir](azure-to-azure-walkthrough-enable-replication.md)
