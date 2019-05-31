---
title: Azure Vm'leri yönetilen disklere geçirme | Microsoft Docs
description: Yönetilen diskleri kullanacak şekilde depolama hesaplarındaki yönetilmeyen diskler kullanılarak oluşturulan Azure sanal makineleri geçirin.
services: virtual-machines-windows
documentationcenter: ''
author: roygara
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 7a390c5231f715ce778c0cc267211b8bfb5934d2
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66418408"
---
# <a name="migrate-azure-vms-to-managed-disks-in-azure"></a>Azure Vm'leri azure'da yönetilen disklere geçirme

Azure yönetilen diskler, ayrı ayrı depolama hesaplarını yönetme ihtiyacını ortadan kaldırarak depolama yönetimini basitleştirir.  Ayrıca mevcut Azure Vm'lerinizi bir kullanılabilirlik kümesindeki VM'ler, daha fazla güvenilirlik yararlanmasını yönetilen diskler geçirebilirsiniz. Bir kullanılabilirlik kümesindeki farklı VM disklerinin yeterince tek hata noktasını önlemek için birbirinden yalıtılmasını sağlar. Bir kullanılabilirlik sınırlayan tek depolama ölçek birimi hatalarına neden nedeniyle donanım ve yazılım hataları etkisini kümesi'nde, farklı depolama ölçek birimi (damgaları ') farklı bir VM disklerinin otomatik olarak geçirir.
Gereksinimlerinize bağlı olarak, dört tür depolama seçeneği seçebilirsiniz. Kullanılabilir disk türleri hakkında bilgi edinmek için bkz: makalemizi [bir disk türü seçin](disks-types.md)

## <a name="migration-scenarios"></a>Geçiş senaryoları

Aşağıdaki senaryolarda yönetilen disklere geçirebilirsiniz:

|Senaryo  |Makale  |
|---------|---------|
|Tek başına VM'leri ve bir kullanılabilirlik kümesindeki VM'leri yönetilen disklere dönüştürme     |[Vm'leri yönetilen diskleri kullanacak şekilde dönüştürme](convert-unmanaged-to-managed-disks.md)         |
|Tek bir VM'yi Klasikten Resource Manager'a yönetilen diskler üzerindeki Dönüştür     |[Klasik bir VHD'den VM oluşturma](create-vm-specialized-portal.md)         |
|Bir sanal ağ içindeki tüm Vm'leri Klasik moddan Resource Manager için yönetilen diskler üzerindeki Dönüştür     |[Iaas kaynaklarının Klasikten Resource Manager'a geçiş](migration-classic-resource-manager-ps.md) ardından [VM'yi yönetilmeyen disklerden yönetilen disklere dönüştürme](convert-unmanaged-to-managed-disks.md)         |
|VM'ler, vm'lere standart yönetilmeyen diskler ile premium yönetilen diskler ile yükseltme     | İlk olarak, [bir Windows sanal makine yönetilmeyen disklerden yönetilen disklere dönüştürme](convert-unmanaged-to-managed-disks.md). Ardından [yönetilen disk depolama türü güncelleştirme](convert-disk-storage.md).         |

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [yönetilen diskler](managed-disks-overview.md)
- Gözden geçirme [yönetilen diskler için fiyatlandırma](https://azure.microsoft.com/pricing/details/managed-disks/).
