---
title: "Azure Linux VM boyutları - GPU | Microsoft Docs"
description: "Listeleri farklı GPU Azure Linux sanal makineler için kullanılabilir boyutları en iyi duruma getirilmiş. Vcpu, veri diskleri ve NIC yanı sıra bu serideki boyutları için depolama üretilen iş ve ağ bant sayısı hakkında bilgi listeler."
services: virtual-machines-linux
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/14/2017
ms.author: jonbeck
ms.openlocfilehash: 9e8b682d8d7e409f97144d344ec58e710327df7a
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2017
---
# <a name="gpu-optimized-virtual-machine-sizes"></a>Sanal makine boyutlarını GPU en iyi duruma getirilmiş

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

Sürücü yükleme ve doğrulama adımları için bkz [N-serisi sürücü kurulumu Linux için](n-series-driver-setup.md).

[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

* X yüklemek döndürmemelidir sunucu veya kullanan diğer sistemler `Nouveau` Ubuntu NC vm'lerde sürücü. NVIDIA GPU sürücüleri yüklemeden önce devre dışı bırakmanız gerekir `Nouveau` sürücü.  

## <a name="other-sizes"></a>Diğer boyutlara
- [Genel amaçlı](sizes-general.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [Yüksek performanslı işlem](sizes-hpc.md)

## <a name="next-steps"></a>Sonraki adımlar
Hakkında daha fazla bilgi [Azure işlem birimleri (ACU)](acu.md) Azure SKU'ları üzerinde işlem performans karşılaştırmanıza yardımcı olur.