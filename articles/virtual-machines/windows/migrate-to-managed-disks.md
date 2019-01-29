---
title: Azure Vm'leri yönetilen disklere geçirme | Microsoft Docs
description: Yönetilen diskleri kullanacak şekilde depolama hesaplarındaki yönetilmeyen diskler kullanılarak oluşturulan Azure sanal makineleri geçirin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.author: cynthn
ms.component: disks
ms.openlocfilehash: b87e27ae914a01f03ce78eafe5792433d18e417f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55193718"
---
# <a name="migrate-azure-vms-to-managed-disks-in-azure"></a>Azure Vm'leri azure'da yönetilen disklere geçirme

Azure yönetilen diskler, ayrı ayrı depolama hesaplarını yönetme ihtiyacını ortadan kaldırarak depolama yönetimini basitleştirir.  Ayrıca mevcut Azure Vm'lerinizi bir kullanılabilirlik kümesindeki VM'ler, daha fazla güvenilirlik yararlanmasını yönetilen diskler geçirebilirsiniz. Bu, bir kullanılabilirlik kümesindeki farklı VM disklerinin yeterince ayrılmasını tek hata noktasını önlemek için olmasını sağlar. Bir kullanılabilirlik sınırlayan tek depolama ölçek birimi hatalarına neden nedeniyle donanım ve yazılım hataları etkisini kümesi'nde, farklı depolama ölçek birimi (damgaları ') farklı bir VM disklerinin otomatik olarak geçirir.
Gereksinimlerinize bağlı olarak, iki depolama seçeneği türlerinden birini seçebilirsiniz:

- [Premium yönetilen diskler](premium-storage.md) olan katı hal sürücüsü (SSD) tabanlı depolama medyası, bir yüksek performanslı, g/Ç açısından yoğun iş yüklerini çalıştıran sanal makineler için düşük gecikme süreli disk desteği sunar. Premium yönetilen disklere geçirerek hızını avantajlarından ve bu disklerin performans alabilir.

- [Standart yönetilen diskler](standard-storage.md) Sabit Disk sürücüsü (HDD) tabanlı depolama medyası kullanır ve geliştirme/Test ve performans değişkenliğine karşı daha az duyarlı olan diğer sık erişilmeyen iş yükleri için uygundur.

Aşağıdaki senaryolarda yönetilen disklere geçirebilirsiniz:

| Geçiş...                                            | Belgeleri bağlantısı                                                                                                                                                                                                                                                                  |
|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Tek başına Vm'leri ve Vm'leri bir kullanılabilirlik kümesindeki yönetilen disklere dönüştürme   | [Vm'leri yönetilen diskleri kullanacak şekilde dönüştürme](convert-unmanaged-to-managed-disks.md) |
| Tek bir VM Klasik'ten Resource Manager'için yönetilen diskler üzerindeki     | [Klasik bir VHD'den VM oluşturma](create-vm-specialized-portal.md)  | 
| Bir sanal ağ içindeki Klasik'ten Resource Manager'a yönetilen diskler üzerindeki tüm sanal makineler     | [Iaas kaynaklarının Klasikten Resource Manager'a geçiş](migration-classic-resource-manager-ps.md) ardından [VM'yi yönetilmeyen disklerden yönetilen disklere dönüştürme](convert-unmanaged-to-managed-disks.md) | 






## <a name="plan-for-the-conversion-to-managed-disks"></a>Yönetilen disklere dönüştürme için planlama

Bu bölümde sanal makine disk türlerinde en iyi kararın yapmanıza yardımcı olur.


## <a name="location"></a>Konum

Azure yönetilen diskler olduğu bir konum seçin. Premium yönetilen disklere taşıyorsanız, ayrıca Premium depolama burada taşımayı planlıyorsanız bölgede kullanılabilir olduğundan emin olun. Bkz: [bölgeye göre Azure Hizmetleri](https://azure.microsoft.com/regions/#services) kullanılabilir konumların hakkında güncel bilgi için.

## <a name="vm-sizes"></a>VM boyutları

Premium yönetilen disklere geçiriyorsanız, VM boyutu Premium depolama özellikli boyutu kullanılabilir VM bulunduğu bölgede güncelleştirmeniz gerekir. Premium depolama özelliğine sahip VM boyutları gözden geçirin. Azure VM boyutu belirtimleri listelenen [sanal makine boyutları](sizes.md).
Premium depolama ile çalışır ve iş yükünüz gereksinimlerinize en uygun bir VM boyutu seçme sanal makineleri performans özelliklerini gözden geçirin. Kullanılabilir yeterli bant genişliği, VM diski trafiği yönlendirmek emin olun.

## <a name="disk-sizes"></a>Disk boyutları

**Premium yönetilen diskler**

VM'nizi kullanılabilir premium yönetilen diskler yedi türü vardır ve her belirli IOPS ve aktarım hızı sahip sınırları. Bu limitler kapasite, performans, ölçeklenebilirlik açısından uygulamanızın ihtiyaçlarını temel VM'niz için Premium disk türünü seçme ve en yüksek yükler yaparken dikkate.

| Premium disk türü  | P4    | P6    | P10   | P15   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|-------|
| Disk boyutu           | 32 GB| 64 GB| 128 GB| 256 GB|512 GB | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| Disk başına IOPS       | 120   | 240   | 500   | 1100  |2300              | 5000              | 7500              | 7500              | 
| Disk başına aktarım hızı | Saniye başına 25 MB  | Saniye başına 50 MB  | Saniye başına 100 MB | Saniye başına 125 MB |150 MB / saniye | Saniye başına 200 MB | Saniye başına 250 MB | Saniye başına 250 MB |

**Standart yönetilen diskler**

VM'nizi ile kullanılabilecek standart yönetilen diskler yedi türü vardır. Bunların her biri farklı bir kapasiteye sahip ancak aynı IOPS ve aktarım hızı sınırlarına sahip. Standart yönetilen diskler, uygulamanızın kapasite ihtiyaçlarını temel alarak türünü seçin.

| Standart Disk Türü  | S4               | S6               | S10              | S15              | S20              | S30              | S40              | S50              | 
|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------| 
| Disk boyutu           | 30 GB            | 64 GB            | 128 GB           | 256 GB           |512 GB           | 1024 GB (1 TB)   | 2048 GB (2TB)    | 4095 GB (4 TB)   | 
| Disk başına IOPS       | 500              | 500              | 500              | 500              |500              | 500              | 500             | 500              | 
| Disk başına aktarım hızı | Saniyede 60 MB | Saniyede 60 MB | Saniyede 60 MB | Saniyede 60 MB |Saniyede 60 MB | Saniyede 60 MB | Saniyede 60 MB | Saniyede 60 MB | 

## <a name="disk-caching-policy"></a>Diski önbelleğe alma İlkesi

**Premium yönetilen diskler**

Varsayılan olarak, önbelleğe alma İlkesi disktir *salt okunur* tüm Premium veri disklerinde, ve *okuma-yazma* Premium işletim sistemi diski, VM'ye bağlı. Bu yapılandırma ayarının, uygulamanızın IOs için en iyi performans elde etmek için önerilir. (Örneğin, SQL Server günlük dosyası) yazma yoğunluklu veya salt yazılır veri diskleri için daha iyi uygulama performansı elde etmek için disk önbelleğe almayı devre dışı bırakın.

## <a name="pricing"></a>Fiyatlandırma

Gözden geçirme [yönetilen diskler için fiyatlandırma](https://azure.microsoft.com/pricing/details/managed-disks/). Premium yönetilen diskler fiyatlandırma, Premium yönetilmeyen diskler aynıdır. Ancak, standart yönetilen diskler için fiyatlandırmaya göz standart yönetilmeyen disklerden farklıdır.



## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [yönetilen diskler](managed-disks-overview.md)
