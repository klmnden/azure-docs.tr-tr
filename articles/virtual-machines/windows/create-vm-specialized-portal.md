---
title: Azure Portalı'ndaki özelleştirilmiş bir VHD'den bir Windows VM oluşturma | Microsoft Docs
description: Azure portalında bir VHD'den yeni bir Windows VM oluşturun.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/18/2019
ms.author: cynthn
ms.openlocfilehash: cadd4b16ab111f46e49429c6d99e0e692325b3b1
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67718943"
---
# <a name="create-a-vm-from-a-vhd-by-using-the-azure-portal"></a>Azure portalını kullanarak bir VHD'den VM oluşturma

Azure'da bir sanal makine (VM) oluşturmanın birkaç yolu vardır: 

- Zaten sahip olduğunuz bir sanal sabit disk (VHD) kullanmak için veya VHD'yi kullanmak için mevcut bir VM'den kopyalamak isterseniz yeni bir VM tarafından oluşturabilirsiniz *ekleme* VHD bir işletim sistemi diski olarak yeni VM. 

- Silinmiş olan bir VM VHD'den yeni bir VM oluşturabilirsiniz. Örneğin, düzgün çalışmayan bir Azure VM varsa, VM'yi silin ve yeni bir VM oluşturmak için VHD kullanın. Aynı VHD'ye yeniden kullanabilir veya bir anlık görüntü oluşturma ve ardından yeni bir yönetilen disk anlık görüntüden oluşturarak VHD'nin kopyasını oluşturma. Anlık görüntü oluşturma birkaç adım daha sürmekle birlikte, özgün VHD korur ve geri dönüş konusunda sağlar.

- Klasik VM'yi alın ve Resource Manager dağıtım modeli ve yönetilen diskler kullanan yeni bir VM oluşturmak için VHD'yi kullanın. En iyi sonuçlar için **Durdur** anlık görüntüsünü oluşturmadan önce Azure portalında Klasik VM.
 
- Bir Azure VM, şirket içi VHD'yi karşıya yüklemek ve yeni bir sanal makineye ekleyerek şirket içi bir VHD'den oluşturabilirsiniz. Bir depolama hesabına VHD yüklemek için PowerShell veya başka bir araç kullanın ve ardından bir VHD'den yönetilen disk oluşturun. Daha fazla bilgi için [özelleştirilmiş bir VHD'yi karşıya yükleme](create-vm-specialized.md#option-2-upload-a-specialized-vhd). 

Birden çok VM oluşturmak istiyorsanız özel bir disk kullanmayın. Bunun yerine, daha büyük dağıtımlar için [görüntü oluşturma](capture-image-resource.md) ardından [birden çok VM oluşturmak için bu görüntüyü kullanan](create-vm-generalized-managed.md).


## <a name="copy-a-disk"></a>Bir diski kopyalayın

Anlık görüntü oluşturma ve bir disk anlık görüntüden oluşturun. Bu strateji, bir geri dönüş olarak özgün VHD tutmanızı sağlar:

1. Gelen [Azure portalında](https://portal.azure.com), sol menüsünde **tüm hizmetleri**.
2. İçinde **tüm hizmetleri** girin, arama kutusuna **diskleri** seçip **diskleri** kullanılabilir disklerin listesini görüntülemek için.
3. Kullanmak istediğiniz diski seçin. **Disk** görünür bu diskin sayfası.
4. Üstteki menüden **oluşturma anlık görüntüsü**. 
5. Girin bir **adı** anlık görüntü.
6. Seçin bir **kaynak grubu** anlık görüntü. Bir ya da bir mevcut kaynak grubunu kullanın veya yeni bir tane oluşturun.
7. İçin **hesap türü**, seçin ya da **standart (HDD)** veya **Premium (SSD)** depolama.
8. İşiniz bittiğinde **Oluştur** anlık görüntüsünü oluşturmak için.
9. Anlık görüntü oluşturulduktan sonra seçin **kaynak Oluştur** soldaki menüde.
10. Arama kutusuna **yönetilen disk** seçip **yönetilen diskler** listeden.
11. Üzerinde **yönetilen diskler** sayfasında **Oluştur**.
12. Girin bir **adı** disk için.
13. Seçin bir **kaynak grubu** disk için. Bir ya da bir mevcut kaynak grubunu kullanın veya yeni bir tane oluşturun. Bu seçim, burada diskten VM oluşturma kaynak grubu olarak da kullanılır.
14. İçin **hesap türü**, seçin ya da **standart (HDD)** veya **Premium (SSD)** depolama.
15. İçinde **kaynak türünü**, olun **anlık görüntü** seçilir.
16. İçinde **kaynak anlık görüntüsü** açılan listesinde, kullanmak istediğiniz bir anlık görüntü seçin.
17. Gerektiği gibi diğer ayarlamaları yapın ve ardından **Oluştur** disk oluşturmak için.

## <a name="create-a-vm-from-a-disk"></a>Bir diskten VM oluşturma

Yönetilen disk kullanmak istediğiniz VHD'yi oluşturduktan sonra portalda VM oluşturabilirsiniz:

1. Gelen [Azure portalında](https://portal.azure.com), sol menüsünde **tüm hizmetleri**.
2. İçinde **tüm hizmetleri** girin, arama kutusuna **diskleri** seçip **diskleri** kullanılabilir disklerin listesini görüntülemek için.
3. Kullanmak istediğiniz diski seçin. **Disk** sayfası bu diski açılır.
4. İçinde **genel bakış** sayfasında **DISK durumu** olarak listelenen **eklenmemiş**. Aksi takdirde, diski sanal makineden çıkarın veya diskte boşaltmak için VM'yi silin gerekebilir.
4. Sayfanın üst kısmındaki menüde **VM Oluştur**.
5. Üzerinde **Temelleri** sayfasında yeni VM için girin bir **sanal makine adı** seçip ya da mevcut bir **kaynak grubu** veya yeni bir tane oluşturun.
6. İçin **boyutu**seçin **değiştirme boyutu** erişimi **boyutu** sayfası.
7. Bir VM boyutu satırı seçin ve ardından **seçin**.
8. Üzerinde **ağ** sayfasında, tüm yeni kaynaklar oluşturan portal ya da izin veya mevcut bir seçebilirsiniz **sanal ağ** ve **ağ güvenlik grubu**. Portal her zaman yeni bir VM için yeni ağ arabirimi ve genel IP adresi oluşturur. 
9. Üzerinde **Yönetim** sayfasında, herhangi bir değişiklik izleme seçenekleri.
10. Üzerinde **Konuk config** sayfasında, tüm uzantıları gerektiği gibi ekleyin.
11. İşiniz bittiğinde **gözden geçir + Oluştur**. 
12. VM yapılandırması doğrulama testlerini geçerse, seçin **Oluştur** dağıtımı başlatmak için.

## <a name="next-steps"></a>Sonraki adımlar

PowerShell de kullanabilirsiniz [özel bir VM oluşturma ve Azure'a VHD yükleme](create-vm-specialized.md).


