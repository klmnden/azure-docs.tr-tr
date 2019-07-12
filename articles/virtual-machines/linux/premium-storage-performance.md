---
title: "Azure Premium Depolama: Windows vm'lerinde performans için tasarlama | Microsoft Docs"
description: Azure Premium depolama kullanan yüksek performanslı uygulamalar tasarlayın. Premium depolama, Azure sanal makinelerinde çalışan g/Ç açısından yoğun iş yükleri için yüksek performanslı, düşük gecikme süreli disk desteği sunar.
services: virtual-machines-linux,storage
author: roygara
ms.service: virtual-machines-linux
ms.tgt_pltfrm: linux
ms.topic: article
ms.date: 06/27/2017
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: c6de3144a87a5bfad38e1b33148f292b26c0f181
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67658239"
---
[!INCLUDE [virtual-machines-common-premium-storage-introduction](../../../includes/virtual-machines-common-premium-storage-introduction.md)]

> [!NOTE]
> Bazen, bir disk performans sorunu görünüyor ne aslında bir ağ sorunu olabilir. Bu gibi durumlarda, iyileştirmek, [ağ performansı](../../virtual-network/virtual-network-optimize-network-bandwidth.md).
>
> Makalemizi diskinize Kıyaslama istiyorsanız bakın [bir disk değerlendirmesi](disks-benchmarks.md).
>
> Hızlandırılmış ağ, sanal Makinenizin destekliyorsa, etkin olduğundan emin olun. Etkinleştirilmezse, hem de zaten dağıtılmış vm'lerde etkinleştirebilirsiniz [Windows](../../virtual-network/create-vm-accelerated-networking-powershell.md#enable-accelerated-networking-on-existing-vms) ve [Linux](../../virtual-network/create-vm-accelerated-networking-cli.md#enable-accelerated-networking-on-existing-vms).

Premium depolamaya bilginiz yoksa, başlamadan önce okumanız [Iaas sanal makineleri için Azure disk türü seçin](disks-types.md) ve [depolama hesapları için Azure depolama ölçeklenebilirlik ve performans hedefleri](../../storage/common/storage-scalability-targets.md).


[!INCLUDE [virtual-machines-common-premium-storage-performance.md](../../../includes/virtual-machines-common-premium-storage-performance.md)]
