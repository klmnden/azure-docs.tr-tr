---
title: "Azure yönetilen disklere AWS ve diğer platformlar geçirme | Microsoft Docs"
description: "AWS veya diğer sanallaştırma platformları gibi diğer bulut gelen karşıya VHD'lerin azure'da VM'ler oluşturun ve Azure yönetilen diskleri yararlanabilir."
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
ms.date: 10/07/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3b427556c589c7cc5205bfda16edc8d891814326
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="migrate-from-amazon-web-services-aws-and-other-platforms-to-managed-disks-in-azure"></a>Azure yönetilen disklere Amazon Web Hizmetleri (AWS) ve diğer platformlar geçirme

Yönetilen diskleri yararlanmak VM'ler oluşturmak için Azure AWS veya şirket içi Sanallaştırma Çözümleri VHD dosyalarının karşıya yükleyebilirsiniz. Azure yönetilen diskleri, depolama hesaplarını Azure Iaas VM'ler için yönetme ihtiyacını ortadan kaldırır. Yalnızca boyutunu ve türünü (Premium veya standart) belirtmek zorunda duyduğunuz disk ve Azure oluşturur ve disk tarafından yönetilir. 

Genelleştirilmiş ve özelleştirilmiş VHD'leri karşıya yükleyebilirsiniz. 
- **VHD genelleştirilmiş** -, sahip tüm kişisel hesap bilgilerinizi Sysprep kullanarak kaldırıldı. 
- **VHD özelleştirilmiş** -kullanıcı hesapları, uygulamaları ve diğer özgün VM Durum verilerini korur. 

> [!IMPORTANT]
> Herhangi bir VHD'yi Azure'a karşıya yüklemeden önce uygulamanız gereken [Windows VHD veya VHDX Azure'a karşıya yüklemek için hazırlama](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
>
>


| Senaryo                                                                                                                         | Belgeler                                                                                                                       |
|----------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| Azure yönetilen diskleri kullanarak sanal makineleri geçirmek istediğiniz varolan AWS EC2 örneğe sahip                              | [Bir VM'yi Amazon Web Hizmetleri'ndeki (AWS) Azure'a taşıyın.](aws-to-azure.md)                           |
| Bir sanal makineden bir görüntü olarak birden fazla Azure VM'yi oluşturmak için kullanmak istediğiniz başka bir sanallaştırma platformunda var. | [Genelleştirilmiş bir VHD yüklemek ve Azure'da yeni bir VM oluşturmak için kullanın](upload-generalized-managed.md) |
| Azure'da yeniden oluşturmak istediğiniz benzersiz şekilde özelleştirilmiş bir VM var.                                                      | [Özel bir VHD Azure'a yükleyin ve yeni bir VM oluşturun](create-vm-specialized.md)         |


## <a name="overview-of-managed-disks"></a>Yönetilen diskleri genel bakış

Azure yönetilen diskleri depolama hesaplarını yönetmek için gereksinimini ortadan kaldırarak VM yönetimini basitleştirir. Diskleri bir kullanılabilirlik kümesindeki VM'lerin daha iyi güvenilirlik avantajı da yönetilen. Bu, farklı sanal makineleri bir kullanılabilirlik kümesindeki disklerinin yeterince yalıtılmış bir tek hata noktası önlemek için birbirinden olmasını sağlar. Bir kullanılabilirlik tek depolama ölçek birimi hataları nedeniyle donanım neden ve yazılım hataları etkisini sınırlayan kümesinde içinde farklı depolama ölçek birimlerinin (damgaları ') farklı VM'ler diskleri otomatik olarak yerleştirir. Gereksinimlerinize bağlı olarak, iki tür depolama seçenekleri arasında seçim yapabilirsiniz: 
 
- [Premium yönetilen diskleri](premium-storage.md) olan düz durumu sürücüsü (SSD) dayalı depolama ortamı, yüksek performans, düşük gecikme süreli disk g/Ç kullanımı yoğun iş yükleri çalıştıran sanal makineler için destek sunar. Premium yönetilen disklere geçirerek hızını avantajlarından ve bu disklerin performansını alabilir.  

- [Standart yönetilen disk](standard-storage.md) geliştirme ve Test ve performans değişkenlik için daha az hassas diğer sık erişim iş yükleri için en uygun ve Sabit Disk sürücüsü (HDD) dayalı depolama medya kullanın.  

## <a name="plan-for-the-migration-to-managed-disks"></a>Yönetilen disklere geçişi için planlama

Bu bölümde, VM ve disk türlerinde en iyi kararı yardımcı olur.


### <a name="location"></a>Konum

Azure yönetilen diskleri kullanılabildiği bir konum seçin. Premium yönetilen disklere geçiriyorsanız, ayrıca Premium depolama burada geçiş yapmayı planlıyorsanız bölgede kullanılabilir olduğundan emin olun. Bkz: [bölgeye göre Azure Hizmetleri](https://azure.microsoft.com/regions/#services) kullanılabilir konumları hakkında güncel bilgi.

### <a name="vm-sizes"></a>VM boyutları

Premium yönetilen disklere geçiriyorsanız, Premium depolama yeteneğine sahip bir boyut kullanılabilir VM bulunduğu bölgede VM boyutu güncelleştirmeniz gerekir. Premium depolama özelliğine sahip VM boyutları gözden geçirin. Azure VM boyutu belirtimleri listelenen [sanal makineler için Boyutlar](sizes.md).
Premium Storage ile birlikte çalışmak ve İş yükünüzün uygun en uygun VM boyutunu seçin sanal makineleri performans özelliklerini gözden geçirin. Yeterli bant genişliği kullanılabilir disk trafiği sürücü için de kendi VM'nizi bulunduğundan emin olun.

### <a name="disk-sizes"></a>Disk boyutları

**Premium yönetilen diskleri**

VM ile kullanılan Premium yönetilen diskleri üç tür vardır ve her belirli IOPS ve üretilen iş sahip sınırlar. Kapasite, performans, ölçeklenebilirlik açısından, uygulamanızın gereksinimlerine göre VM için Premium disk türü seçerken bu sınırları göz önünde bulundurun ve en yüksek yükler.

| Premium diskler türü  | P10               | P20               | P30               |
|---------------------|-------------------|-------------------|-------------------|
| Disk boyutu           | 128 GB            | 512 GB            | 1024 GB (1 TB)    |
| Disk başına IOPS       | 500               | 2300              | 5000              |
| Disk başına aktarım hızı | Saniyede 100 MB | 150 MB / saniye | 200 MB / saniye |

**Standart yönetilen disk**

VM ile kullanılabilecek standart yönetilen disk beş türü vardır. Bunların her biri farklı kapasiteye sahip ancak aynı IOPS ve üretilen iş sınırı vardır. Uygulamanızı kapasite gereksinimlerine göre standart yönetilen disk türünü seçin.

| Standart Disk Türü  | S4               | S6               | S10              | S20              | S30              |
|---------------------|------------------|------------------|------------------|------------------|------------------|
| Disk boyutu           | 30 GB            | 64 GB            | 128 GB           | 512 GB           | 1024 GB (1 TB)   |
| Disk başına IOPS       | 500              | 500              | 500              | 500              | 500              |
| Disk başına aktarım hızı | Saniye başına 60 MB | Saniye başına 60 MB | Saniye başına 60 MB | Saniye başına 60 MB | Saniye başına 60 MB |

### <a name="disk-caching-policy"></a>Önbellek İlkesi disk 

**Premium yönetilen diskleri**

Varsayılan olarak, ilke önbelleğe alma disktir *salt okunur* tüm Premium veri diskleri için ve *okuma-yazma* için Premium işletim sistemi diski VM'ye eklenmiş. Bu yapılandırma ayarının uygulamanızın IOs için en iyi performans elde etmek için önerilir. (SQL Server günlük dosyaları gibi) ağır yazma ya da salt yazılır veri diskleri için daha iyi uygulama performansı elde etmek için disk önbelleğe almayı devre dışı bırakın.

### <a name="pricing"></a>Fiyatlandırma

Gözden geçirme [yönetilen diskler için fiyatlandırma](https://azure.microsoft.com/en-us/pricing/details/managed-disks/). Premium diskler yönetilen fiyatlandırma Premium yönetilmeyen diskleri olarak aynıdır. Ancak standart yönetilen disk için fiyatlandırma standart yönetilmeyen disklerden farklı.


## <a name="next-steps"></a>Sonraki Adımlar

- Herhangi bir VHD'yi Azure'a karşıya yüklemeden önce uygulamanız gereken [Windows VHD veya VHDX Azure'a karşıya yüklemek için hazırlama](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
