---
title: Azure Site Recovery tarafından çoğaltılmış Azure VM'ye eklenen bir disk için çoğaltmayı etkinleştirme | Microsoft Docs
description: Bu makalede Azure Site Recovery ile olağanüstü durum kurtarma için etkin bir Azure VM'ye eklenen bir disk için çoğaltmayı etkinleştirme
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/29/2018
ms.author: asgang
ms.openlocfilehash: 69122ffe9cefa3e1b9c6c8fbadfa80492ebebbde
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64928070"
---
# <a name="enable-replication-for-a-disk-added-to-an-azure-vm"></a>Bir Azure VM'ye eklenen bir disk için çoğaltmayı etkinleştirme


Bu makalede zaten başka bir Azure bölgesine olağanüstü durum kurtarma için etkin bir Azure VM'ye eklenen veri diskleri için çoğaltma etkinleştirme kullanarak [Azure Site Recovery](site-recovery-overview.md).

Bir sanal makineye eklediğiniz bir disk için çoğaltma etkinleştirme Azure Vm'leri için yönetilen disklerle desteklenir.

Azure VM'deki başka bir Azure bölgesine çoğaltmak için yeni bir disk eklediğinizde, aşağıdakiler gerçekleşir:

-   VM için çoğaltma durumu bir uyarı gösterir ve Portalı'nda Not tek bildirir veya daha fazla disk koruması için kullanılabilir.
-   Eklenen diskleri için korumayı etkinleştirin, uyarı diskin ilk çoğaltmadan sonra kaybolur.
-   Diske ait çoğaltma etkinleştirmemeyi seçerseniz, uyarıyı Kapat seçeneğini belirleyebilirsiniz.

![Yeni disk eklendi](./media/azure-to-azure-enable-replication-added-disk/newdisk.png)



## <a name="before-you-start"></a>Başlamadan önce

Bu makalede, disk eklediğiniz VM için olağanüstü durum kurtarma işlemini önceden ayarladığınız varsayılmaktadır. Siz yapmadıysanız izleyin [Azure'dan Azure'a olağanüstü durum kurtarma öğretici](azure-to-azure-tutorial-enable-replication.md). 

## <a name="enable-replication-for-an-added-disk"></a>Eklenen bir disk için çoğaltmayı etkinleştirme 

Eklenen bir disk için çoğaltmayı etkinleştirmek için aşağıdakileri yapın:

1. Kasadaki > **çoğaltılan öğeler**, disk eklediğiniz VM'ye tıklayın.
2. Tıklayın **diskleri**ve ardından çoğaltmayı etkinleştirmek istediğiniz veri diski seçin (bu disklere sahip bir **korumalı** durumu).
3.  İçinde **Disk ayrıntıları**, tıklayın **çoğaltmayı etkinleştir**.

    ![Eklenen diski için çoğaltmayı etkinleştirme](./media/azure-to-azure-enable-replication-added-disk/enabled-added.png)

Çoğaltma sistem durumu uyarısı disk sorunu için çoğaltma etkinleştirme işini çalıştırır ve ilk çoğaltma sonlandırıldıktan sonra kaldırılır.



# <a name="next-steps"></a>Sonraki adımlar

[Daha fazla bilgi edinin](site-recovery-test-failover-to-azure.md) yük devretme testi çalıştırma hakkında.
