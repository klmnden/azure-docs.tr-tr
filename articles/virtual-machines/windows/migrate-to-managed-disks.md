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
ms.date: 01/03/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 12cd1caa4cb96dbd5862776589d4a34aeb294ca1
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64689728"
---
# <a name="migrate-azure-vms-to-managed-disks-in-azure"></a>Azure Vm'leri azure'da yönetilen disklere geçirme

Azure yönetilen diskler, ayrı ayrı depolama hesaplarını yönetme ihtiyacını ortadan kaldırarak depolama yönetimini basitleştirir.  Ayrıca mevcut Azure Vm'lerinizi bir kullanılabilirlik kümesindeki VM'ler, daha fazla güvenilirlik yararlanmasını yönetilen diskler geçirebilirsiniz. Bu, bir kullanılabilirlik kümesindeki farklı VM disklerinin yeterince ayrılmasını tek hata noktasını önlemek için olmasını sağlar. Bir kullanılabilirlik sınırlayan tek depolama ölçek birimi hatalarına neden nedeniyle donanım ve yazılım hataları etkisini kümesi'nde, farklı depolama ölçek birimi (damgaları ') farklı bir VM disklerinin otomatik olarak geçirir.
Gereksinimlerinize bağlı olarak, dört tür depolama seçeneği seçebilirsiniz. Kullanılabilir disk türleri hakkında bilgi edinmek için bkz: makalemizi [bir disk türü seçin](disks-types.md)

## <a name="migrate-scenarios"></a>Geçiş senaryoları

Aşağıdaki senaryolarda yönetilen disklere geçirebilirsiniz:

| **Geçiş...**                                            | **Belgeleri bağlantısı**                                                                                                                                                                                                                                                                  |
|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Tek başına VM'leri ve bir kullanılabilirlik kümesindeki VM'leri yönetilen disklere dönüştürme   | [Vm'leri yönetilen diskleri kullanacak şekilde dönüştürme](convert-unmanaged-to-managed-disks.md) |
| Tek bir VM'yi klasikten yönetilen diskler üzerinde Resource Manager'a dönüştürme     | [Klasik bir VHD'den VM oluşturma](create-vm-specialized-portal.md)  | 
| Bir sanal ağ içindeki tüm VM'leri klasikten yönetilen diskler üzerinde Resource Manager'a dönüştürme     | [Iaas kaynaklarının Klasikten Resource Manager'a geçiş](migration-classic-resource-manager-ps.md) ardından [VM'yi yönetilmeyen disklerden yönetilen disklere dönüştürme](convert-unmanaged-to-managed-disks.md) | 


## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [yönetilen diskler](managed-disks-overview.md)
- Gözden geçirme [yönetilen diskler için fiyatlandırma](https://azure.microsoft.com/pricing/details/managed-disks/).
