---
title: Azure Windows VM boyutları - GPU | Microsoft Docs
description: Azure'da Windows sanal makineler için kullanılabilir boyutları listeler farklı GPU İyileştirildi. Vcpu, veri diskleri ve NIC yanı sıra bu serideki boyutları için depolama aktarım hızı ve ağ bant sayısı hakkında bilgiler listelenir.
services: virtual-machines-windows
documentationcenter: ''
author: jonbeck7
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/11/2019
ms.author: jonbeck
ms.openlocfilehash: 86725fccba810284677aeb3324839a52c91c3021
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67710195"
---
# <a name="gpu-optimized-virtual-machine-sizes"></a>GPU için iyileştirilmiş sanal makine boyutları

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>Desteklenen işletim sistemleri ve sürücüler

Windows çalıştıran Azure N serisi sanal makineler, GPU özelliklerine yararlanmak için NVIDIA GPU sürücülerine yüklenmesi gerekir. [NVIDIA GPU sürücüsünün uzantısı](../extensions/hpccompute-gpu-windows.md) bir N serisi sanal makinesinde uygun NVIDIA CUDA veya kılavuz sürücüleri yükler. Yükleme veya uzantısını Azure portalı veya Azure PowerShell veya Azure Resource Manager şablonları gibi araçları kullanarak yönetin. Bkz: [NVIDIA GPU sürücüsünün uzantı belgesini](../extensions/hpccompute-gpu-windows.md) desteklenen işletim sistemleri ve dağıtım adımları için. VM uzantıları hakkında genel bilgi için bkz: [Azure sanal makine uzantıları ve özellikleri](../extensions/overview.md).

NVIDIA GPU sürücüleri el ile yüklemek isterseniz, bkz. [Windows için N serisi GPU sürücü kurulumu](n-series-driver-setup.md) yükleme ve doğrulama adımlarını desteklenen işletim sistemleri ve sürücüler için.

[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

## <a name="other-sizes"></a>Diğer boyutları
- [Genel amaçlı](sizes-general.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [Önceki nesil](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar
Hakkında daha fazla bilgi [Azure işlem birimleri (ACU)](acu.md) Azure SKU'ları arasında işlem performansını karşılaştırmanıza yardımcı olabilir.

