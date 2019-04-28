---
title: AWS ve diğer platformlardan azure'da yönetilen disklere geçirme | Microsoft Docs
description: AWS veya diğer sanallaştırma platformları gibi diğer bulut kaynaklardan yüklenen VHD kullanarak azure'da VM oluşturma ve Azure yönetilen diskler yararlanın.
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
ms.date: 10/07/2017
ms.author: rogarana
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 42ad7bc10cb7b93bd4db9260f950ae4ca12aba44
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61126923"
---
# <a name="migrate-from-amazon-web-services-aws-and-other-platforms-to-managed-disks-in-azure"></a>Amazon Web Services (AWS) ve diğer platformlardan azure'da yönetilen disklere geçirme

Azure yönetilen diskler yararlanan sanal makineler oluşturmak için AWS veya şirket içi Sanallaştırma Çözümleri öncesinden VHD dosyalarının karşıya yükleyebilirsiniz. Azure yönetilen diskler, Azure Iaas Vm'leri için depolama hesapları yönetme ihtiyacını ortadan kaldırır. Yalnızca türü (Premium veya standart) ve boyutunu belirtmek zorunda duyduğunuz disk ve Azure oluşturur ve diski oluşturup yönetebilmesi. 

Genelleştirilmiş ve özelleştirilmiş VHD yükleyebilirsiniz. 
- **Genelleştirilmiş VHD** -vardı tüm kişisel hesap bilgilerinizi Sysprep kullanarak kaldırıldı. 
- **Özelleştirilmiş VHD** -kullanıcı hesaplarını, uygulamaları ve orijinal VM'yi diğer Durum verilerini tutar. 

> [!IMPORTANT]
> Azure'a herhangi bir VHD'yi karşıya yüklemeden önce uygulamanız gereken [Windows VHD veya VHDX yüklemek için hazırlama](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
>
>


| Senaryo                                                                                                                         | Belgeler                                                                                                                       |
|----------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| Yönetilen diskleri kullanarak Azure Vm'lerine geçirmek istediğiniz mevcut AWS EC2 örneklerini sahip                              | [Bir VM, Amazon Web Services'den (AWS) Azure'a taşıyın.](aws-to-azure.md)                           |
| Birden fazla Azure sanal makineler oluşturmak için bir görüntü olarak kullanmak istediğiniz başka bir sanallaştırma platformundan VM var. | [Genelleştirilmiş VHD yükleme ve Azure'da yeni bir VM oluşturmak için bunu kullanın](upload-generalized-managed.md) |
| Azure'da yeniden oluşturmak için istediğiniz benzersiz olarak özelleştirilmiş bir VM var.                                                      | [Azure'a özelleştirilmiş bir VHD'yi karşıya yükleme ve yeni VM oluşturma](create-vm-specialized.md)         |


## <a name="overview-of-managed-disks"></a>Yönetilen disklere genel bakış

Azure yönetilen diskler, depolama hesaplarını yönetme ihtiyacını ortadan kaldırarak VM Yönetimi basitleştirir. Ayrıca avantajı öğesinden daha fazla güvenilirlik bir kullanılabilirlik kümesindeki sanal makinelerin yönetilen diskler. Bir kullanılabilirlik kümesindeki farklı VM disklerinin yeterince bir tek hata noktasını önlemek için birbirinden yalıtılmasını sağlar. Bir kullanılabilirlik sınırlayan tek depolama ölçek birimi hatalarına neden nedeniyle donanım ve yazılım hataları etkisini kümesi'nde, farklı depolama ölçek birimi (damgaları ') farklı bir VM disklerinin otomatik olarak geçirir.
Gereksinimlerinize bağlı olarak, dört tür depolama seçeneği seçebilirsiniz. Kullanılabilir disk türleri hakkında bilgi edinmek için bkz: makalemizi [bir disk türü seçin](disks-types.md).

## <a name="plan-for-the-migration-to-managed-disks"></a>Yönetilen Diskler'e geçiş planlama

Bu bölümde sanal makine disk türlerinde en iyi kararın yapmanıza yardımcı olur.

Yönetilmeyen disklerden yönetilen disklere geçirmeyi planlıyorsanız bilmeniz gereken söz konusu kullanıcılarla [sanal makine Katılımcısı](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) rol (ön dönüştürme verebilir gibi) VM boyutunu değiştirmek mümkün olmayacaktır. Yönetilen disklere sahip VM'ler işletim sistemi disklerinde Microsoft.Compute/disks/write izninin gerektirmek olmasıdır.

### <a name="location"></a>Location

Azure yönetilen diskler olduğu bir konum seçin. Premium yönetilen disklere geçiriyorsanız, ayrıca Premium depolama burada geçirmeyi planlıyorsanız bölgede kullanılabilir olduğundan emin olun. Bkz: [bölgeye göre Azure Hizmetleri](https://azure.microsoft.com/regions/#services) kullanılabilir konumların hakkında güncel bilgi için.

### <a name="vm-sizes"></a>VM boyutları

Premium yönetilen disklere geçiriyorsanız, VM boyutu Premium depolama özellikli boyutu kullanılabilir VM bulunduğu bölgede güncelleştirmeniz gerekir. Premium depolama özelliğine sahip VM boyutları gözden geçirin. Azure VM boyutu belirtimleri listelenen [sanal makine boyutları](sizes.md).
Premium depolama ile çalışır ve iş yükünüz gereksinimlerinize en uygun bir VM boyutu seçme sanal makineleri performans özelliklerini gözden geçirin. Kullanılabilir yeterli bant genişliği, VM diski trafiği yönlendirmek emin olun.

### <a name="disk-sizes"></a>Disk boyutları

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

### <a name="disk-caching-policy"></a>Diski önbelleğe alma İlkesi 

**Premium yönetilen diskler**

Varsayılan olarak, önbelleğe alma İlkesi disktir *salt okunur* tüm Premium veri disklerinde, ve *okuma-yazma* Premium işletim sistemi diski, VM'ye bağlı. Bu yapılandırma ayarının, uygulamanızın IOs için en iyi performans elde etmek için önerilir. (Örneğin, SQL Server günlük dosyası) yazma yoğunluklu veya salt yazılır veri diskleri için daha iyi uygulama performansı elde etmek için disk önbelleğe almayı devre dışı bırakın.

### <a name="pricing"></a>Fiyatlandırma

Gözden geçirme [yönetilen diskler için fiyatlandırma](https://azure.microsoft.com/pricing/details/managed-disks/). Premium yönetilen diskler fiyatlandırma, Premium yönetilmeyen diskler aynıdır. Ancak, standart yönetilen diskler için fiyatlandırmaya göz standart yönetilmeyen disklerden farklıdır.


## <a name="next-steps"></a>Sonraki Adımlar

- Azure'a herhangi bir VHD'yi karşıya yüklemeden önce uygulamanız gereken [Windows VHD veya VHDX yüklemek için hazırlama](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
