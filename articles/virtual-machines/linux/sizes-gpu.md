---
title: Azure Linux VM boyutları - GPU | Microsoft Docs
description: Azure'da Linux sanal makineler için kullanılabilir boyutları listeler farklı GPU İyileştirildi. Vcpu, veri diskleri ve NIC yanı sıra bu serideki boyutları için depolama aktarım hızı ve ağ bant sayısı hakkında bilgiler listelenir.
services: virtual-machines-linux
documentationcenter: ''
author: jonbeck7
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: jonbeck
ms.openlocfilehash: 8f50f090fe38382b8bc3cb7f669ab4025d36ff76
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60799433"
---
# <a name="gpu-optimized-virtual-machine-sizes"></a>GPU için iyileştirilmiş sanal makine boyutları

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-distributions-and-drivers"></a>Desteklenen dağıtımlar ve sürücüler

Linux çalıştıran Azure N serisi sanal makineler, GPU özelliklerine yararlanmak için NVIDIA GPU sürücülerine yüklenmesi gerekir. [NVIDIA GPU sürücüsünün uzantısı](../extensions/hpccompute-gpu-linux.md) bir N serisi sanal makinesinde uygun NVIDIA CUDA veya kılavuz sürücüleri yükler. Yükleme veya uzantısını Azure portalı veya Azure CLI veya Azure Resource Manager şablonları gibi araçları kullanarak yönetin. Bkz: [NVIDIA GPU sürücüsünün uzantı belgesini](../extensions/hpccompute-gpu-linux.md) desteklenen dağıtımları ve dağıtım adımları için. VM uzantıları hakkında genel bilgi için bkz: [Azure sanal makine uzantıları ve özellikleri](../extensions/overview.md).

NVIDIA GPU sürücüleri el ile yüklemek isterseniz, bkz. [Linux için N serisi GPU sürücü kurulumu](n-series-driver-setup.md) desteklenen dağıtımları, sürücüler ve yükleme ve doğrulama adımlarını.


[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

* X yüklememeniz sunucusu ya da kullanan diğer sistemlerle `Nouveau` Ubuntu NC VM'lerin sürücüsü. NVIDIA GPU sürücüleri yüklemeden önce devre dışı bırakmanız `Nouveau` sürücü.  

## <a name="other-sizes"></a>Diğer boyutları
- [Genel amaçlı](sizes-general.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesil](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar
Hakkında daha fazla bilgi [Azure işlem birimleri (ACU)](acu.md) Azure SKU'ları arasında işlem performansını karşılaştırmanıza yardımcı olabilir.