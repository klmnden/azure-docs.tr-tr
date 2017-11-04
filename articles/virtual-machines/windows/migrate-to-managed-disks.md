---
title: "Yönetilen disklere Azure Vm'leri geçirme | Microsoft Docs"
description: "Yönetilen diskleri kullanmak için yönetilmeyen diskleri depolama hesaplarında kullanılarak oluşturulan Azure sanal makineleri geçirin."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: cynthn
ms.openlocfilehash: b389151b8a1dd0c7a367f83db968bac7b832897a
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="migrate-azure-vms-to-managed-disks-in-azure"></a>Azure yönetilen disklere Azure Vm'leri geçirme

Azure yönetilen diskleri ayrı ayrı depolama hesaplarını yönetmek için gereksinimini ortadan kaldırarak depolama yönetimini basitleştirir.  Ayrıca, mevcut Azure Vm'leriniz yönetilen bir kullanılabilirlik kümesindeki VM'lerin daha iyi güvenilirlik yararlanmasını diskleri geçirebilirsiniz. Bu, farklı sanal makineleri bir kullanılabilirlik kümesindeki disklerinin yeterince yalıtılmış tek hata noktası oluşmasını önlemek için birbirinden olmasını sağlar. Bir kullanılabilirlik tek depolama ölçek birimi hataları nedeniyle donanım neden ve yazılım hataları etkisini sınırlayan kümesinde içinde farklı depolama ölçek birimlerinin (damgaları ') farklı VM'ler diskleri otomatik olarak yerleştirir.
Gereksinimlerinize bağlı olarak, iki tür depolama seçenekleri arasında seçim yapabilirsiniz:

- [Premium yönetilen diskleri](premium-storage.md) olan düz durumu sürücüsü (SSD) dayalı depolama medyası, bir highperformance, g/Ç kullanımı yoğun iş yükleri çalıştıran sanal makineler için düşük gecikmeli disk desteği sunar. Premium yönetilen disklere geçirerek hızını avantajlarından ve bu disklerin performansını alabilir.

- [Standart yönetilen disk](standard-storage.md) geliştirme ve Test ve performans değişkenlik için daha az hassas diğer sık erişim iş yükleri için en uygun ve Sabit Disk sürücüsü (HDD) dayalı depolama medya kullanın.

Aşağıdaki senaryolarda yönetilen disklere geçirebilirsiniz:

| Geçir...                                            | Belge bağlantı                                                                                                                                                                                                                                                                  |
|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Tek başına VM'ler ve sanal makineleri bir kullanılabilirlik kümesinde yönetilen Diske Dönüştür   | [Yönetilen diskleri kullanmak için sanal makineleri Dönüştür](convert-unmanaged-to-managed-disks.md) |
| Bir tek VM'den Klasik yönetilen disklerde Kaynak Yöneticisi     | [Tek bir VM geçirme](migrate-single-classic-to-resource-manager.md)  | 
| Bir sanal ağdan Klasik Kaynak Yöneticisi yönetilen disklerdeki tüm sanal makineleri     | [Iaas kaynaklarını Klasikten Resource Manager geçirme](migration-classic-resource-manager-ps.md) ve ardından [VM yönetilmeyen disklerden yönetilen Diske Dönüştür](convert-unmanaged-to-managed-disks.md) | 






## <a name="plan-for-the-conversion-to-managed-disks"></a>Yönetilen diskleri dönüştürme için planlama

Bu bölümde, VM ve disk türlerinde en iyi kararı yardımcı olur.


## <a name="location"></a>Konum

Azure yönetilen diskleri kullanılabildiği bir konum seçin. Premium yönetilen disklere taşıyorsanız, aynı zamanda Premium depolama burada taşımayı planlıyorsanız bölgede kullanılabilir olduğundan emin olun. Bkz: [bölgeye göre Azure Hizmetleri](https://azure.microsoft.com/regions/#services) kullanılabilir konumları hakkında güncel bilgi.

## <a name="vm-sizes"></a>VM boyutları

Premium yönetilen disklere geçiriyorsanız, Premium depolama yeteneğine sahip bir boyut kullanılabilir VM bulunduğu bölgede VM boyutu güncelleştirmeniz gerekir. Premium depolama özelliğine sahip VM boyutları gözden geçirin. Azure VM boyutu belirtimleri listelenen [sanal makineler için Boyutlar](sizes.md).
Premium Storage ile birlikte çalışmak ve İş yükünüzün uygun en uygun VM boyutunu seçin sanal makineleri performans özelliklerini gözden geçirin. Yeterli bant genişliği kullanılabilir disk trafiği sürücü için de kendi VM'nizi bulunduğundan emin olun.

## <a name="disk-sizes"></a>Disk boyutları

**Premium yönetilen diskleri**

VM ile kullanılan yönetilen premium diskleri yedi tür vardır ve her belirli IOPS ve üretilen iş sahip sınırlar. Bu sınırlar yoğun yükler ve kapasite, performans, ölçeklenebilirlik açısından, uygulamanızın gereksinimlerine, VM için Premium disk türü seçme temel yaparken dikkate.

| Premium diskler türü  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Disk boyutu           | 128 GB| 512 GB| 128 GB| 512 GB            | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| Disk başına IOPS       | 120   | 240   | 500   | 2300              | 5000              | 7500              | 7500              | 
| Disk başına aktarım hızı | Saniye başına 25 MB  | Saniye başına 50 MB  | Saniyede 100 MB | 150 MB / saniye | 200 MB / saniye | Saniye başına 250 MB | Saniye başına 250 MB |

**Standart yönetilen disk**

VM ile kullanılabilecek standart yönetilen disk yedi türü vardır. Bunların her biri farklı kapasiteye sahip ancak aynı IOPS ve üretilen iş sınırı vardır. Uygulamanızı kapasite gereksinimlerine göre standart yönetilen disk türünü seçin.

| Standart Disk Türü  | S4               | S6               | S10              | S20              | S30              | S40              | S50              | 
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------| 
| Disk boyutu           | 30 GB            | 64 GB            | 128 GB           | 512 GB           | 1024 GB (1 TB)   | 2048 GB (2TB)    | 4095 GB (4 TB)   | 
| Disk başına IOPS       | 500              | 500              | 500              | 500              | 500              | 500             | 500              | 
| Disk başına aktarım hızı | Saniye başına 60 MB | Saniye başına 60 MB | Saniye başına 60 MB | Saniye başına 60 MB | Saniye başına 60 MB | Saniye başına 60 MB | Saniye başına 60 MB | 

## <a name="disk-caching-policy"></a>Önbellek İlkesi disk

**Premium yönetilen diskleri**

Varsayılan olarak, ilke önbelleğe alma disktir *salt okunur* tüm Premium veri diskleri için ve *okuma-yazma* için Premium işletim sistemi diski VM'ye eklenmiş. Bu yapılandırma ayarının uygulamanızın IOs için en iyi performans elde etmek için önerilir. (SQL Server günlük dosyaları gibi) ağır yazma ya da salt yazılır veri diskleri için daha iyi uygulama performansı elde etmek için disk önbelleğe almayı devre dışı bırakın.

## <a name="pricing"></a>Fiyatlandırma

Gözden geçirme [yönetilen diskler için fiyatlandırma](https://azure.microsoft.com/en-us/pricing/details/managed-disks/). Premium diskler yönetilen fiyatlandırma Premium yönetilmeyen diskleri olarak aynıdır. Ancak standart yönetilen disk için fiyatlandırma standart yönetilmeyen disklerden farklı.



## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinmek [yönetilen diskleri](managed-disks-overview.md)
