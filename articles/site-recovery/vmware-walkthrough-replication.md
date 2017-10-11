---
title: "Azure Site Recovery ile azure'a VMware VM çoğaltması için bir çoğaltma ilkesi ayarlama | Microsoft Docs"
description: "VMware Vm'leri Azure depolama alanına çoğaltırken bir çoğaltma ilkesi ayarlamak için gereken adımları özetler"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d7874bd8-6626-4668-9ec9-dbd2d26f8f81
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 3c4b7ad16e6a03fb605447def18f7475d502fdd1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="step-9-set-up-a-replication-policy-for-vmware-vm-replication-to-azure"></a>9. adım: VMware VM Azure'a çoğaltma için bir çoğaltma ilkesi ayarlama


Bu makalede Azure kullanarak VMware sanal makinelerini çoğaltma yapıyorsanız, bir çoğaltma ilkesi ayarlamak nasıl [Azure Site Recovery](site-recovery-overview.md) Azure portalında hizmet.


POST açıklamaları ve soruları alt bu makalenin veya üzerinde [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="configure-a-policy"></a>Bir ilke yapılandırın

Başlamadan önce hızlı bir video genel bakış alın:
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video2-vCenter-Server-Discovery-and-Replication-Policy/player]

1. Yeni bir çoğaltma ilkesi oluşturmak için tıklatın **Site Recovery altyapısı** > **çoğaltma ilkeleri** > **+ Çoğaltma İlkesi**.
2. İçinde **çoğaltma ilkesi oluşturun**, bir ilke adı belirtin.
3. **RPO eşiği** bölümünde RPO limitini belirtin. Bu değer, ne sıklıkta belirtir veri kurtarma noktaları oluşturulur. Sürekli çoğaltma bu sınırı aşarsa bir uyarı üretilir.
4. İçinde **kurtarma noktası bekletme**, (saat olarak) ne kadar süreyle bekletme penceresi için her kurtarma noktası belirtin. Çoğaltılan VM'ler penceresinde herhangi bir noktaya kurtarılabilir. 24 saat bekletme için premium depolama ve standart depolama için 72 saat çoğaltılan makineler için desteklenir.
5. İçinde **uygulamayla tutarlı anlık görüntü sıklığı**, belirtin ne sıklıkta (dakika cinsinden) uygulamayla tutarlı anlık görüntüleri içeren kurtarma noktaları oluşturulur. Tıklatın **Tamam** bir ilke oluşturmak için.

    ![Çoğaltma ilkesi](./media/vmware-walkthrough-replication/gs-replication2.png)
8. Yeni bir ilke oluşturduğunuzda, otomatik olarak yapılandırma sunucusu ile ilişkili. Varsayılan olarak, eşleşen bir ilkesi yeniden çalışma için otomatik olarak oluşturulur. Örneğin, çoğaltma ilkesi ise **rep İlkesi** geri dönme ilkesini sonra **rep İlkesi yeniden**. Bir azure'dan başlatana kadar bu ilkeyi kullanılmaz.

## <a name="next-steps"></a>Sonraki adımlar

Git [adım 10: Mobility hizmetini yükleme](vmware-walkthrough-install-mobility.md)
