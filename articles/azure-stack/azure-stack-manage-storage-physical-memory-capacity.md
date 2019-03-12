---
title: Azure Stack için fiziksel bellek kapasitesi yönetme | Microsoft Docs
description: İzleme ve Azure Stack için kullanılabilir depolama alanı yönetin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 84518E90-75E1-4037-8D4E-497EAC72AAA1
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/14/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: 875e823aa2958ee38b3510e93ffac7918be661cb
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57774022"
---
# <a name="manage-physical-memory-capacity-for-azure-stack"></a>Azure Stack için fiziksel bellek kapasitesi yönetme

*Uygulama hedefi: Azure Stack tümleşik sistemleri*

Azure Stack için toplam kullanılabilir bellek kapasitesini artırmak için ek bellek ekleyebilirsiniz. Azure Stack'te, fiziksel sunucu olarak da adlandırılan bir *ölçek birimi düğüm*. Tek bir ölçek biriminin üyesi olan tüm ölçek birimi düğümleri aynı miktarda bellek olmalıdır.

> [!note]  
> Devam etmeden önce görmek için donanım üreticisinin sağladığı belgelere başvurun bir üreticiniz bir fiziksel bellek yükseltmeyi destekler. OEM donanım satıcısı Destek sözleşmeniz, satıcı fiziksel sunucu raf yerleşimi ve cihaz üretici yazılımı güncelleştirmesi gerçekleştirmek gerekebilir.

Her ölçek birimi düğüme bellek eklemek için genel süreç aşağıdaki akış diyagramı gösterir.

![Bellek her ölçek birimi düğümüne ekleyin.](media/azure-stack-manage-storage-physical-capacity/process-to-add-memory-to-scale-unit.png)

## <a name="add-memory-to-an-existing-node"></a>Bellek için var olan bir düğüm Ekle
Aşağıdaki adımlar, bellek ekleme işlemi üst düzey bir genel bakış sağlar. 

> [!Warning]  
OEM tarafından sağlanan belgelerinize başvuruda bulunmadan bu adımları izlemeyin.

> [!Warning]  
Sıralı yükseltme bellek desteklenmediğinden ölçek biriminin tamamı kapatılmalıdır.

1. Azure Stack konusunda belgelenen adımları kullanarak Durdur [başlatma ve durdurma Azure Stack](azure-stack-start-and-stop.md) makalesi.
2. Bellek her fiziksel bilgisayardaki donanım üreticisinin sağladığı belgelere kullanarak yükseltin.
3. İçindeki adımları kullanarak Azure Stack Başlat [başlatma ve durdurma Azure Stack](azure-stack-start-and-stop.md) makalesi.

## <a name="next-steps"></a>Sonraki adımlar

 - Bulmak, kurtarma ve iş ihtiyaçlarına göre depolama kapasiteyi geri kazanmak için Azure stack'teki depolama hesapları yönetme konusunda bilgi almak için bkz: [Azure stack'teki depolama hesapları yönetme](azure-stack-manage-storage-accounts.md).
 - Azure Stack bulut operatörü izler ve bunların Azure Stack dağıtım depolama kapasitesi yönetir öğrenmek için bkz: [Azure Stack için depolama kapasitesi yönetme](azure-stack-manage-storage-shares.md). 
