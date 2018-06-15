---
title: Azure portalında özelleştirilmiş bir VHD'den bir Windows VM oluşturma | Microsoft Docs
description: Azure portalında bir VHD'den yeni bir Windows VM oluşturun.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: cynthn
ms.openlocfilehash: 428003747b7c746a2849a042e54647e86361c562
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34716569"
---
# <a name="create-a-vm-from-a-vhd-using-the-azure-portal"></a>Azure Portalı'nı kullanarak bir VHD'den bir VM oluşturma


Azure'da Vm'leri oluşturmanın birkaç yolu vardır. Bir VHD'yi kullanın veya var olan VM ve VHD'den kullanmak için kopyalamak istediğiniz zaten varsa, işletim sistemi diski olarak VHD ekleyerek yeni bir VM oluşturabilirsiniz. Bu işlem *iliştirir* işletim sistemi diski olarak yeni bir VM'yi VHD'ye.

Yeni bir VM silinmiş olan bir VM VHD'den de oluşturabilirsiniz. Örneğin, düzgün çalışmayan bir Azure VM varsa, VM silin ve VHD'yi yeni bir VM oluşturmak için kullanın. Aynı VHD yeniden kullanabilir veya bir anlık görüntü oluşturmak ve ardından yeni bir yönetilen disk anlık görüntüden oluşturarak VHD kopyasını oluşturun. Bu, birkaç adım daha vardır, ancak özgün VHD kullanmaya devam edebilir ve gerekirse geri döner için bir anlık görüntü verir de sağlar.

Azure'da VM oluşturmak için kullanmak istediğiniz bir şirket içi VM var. VHD'nin yüklenmesi ve yeni bir VM'e ekleyin. Bir VHD yüklemek için bir depolama hesabına karşıya yükleyin, ardından VHD'den yönetilen bir disk oluşturmak için PowerShell veya başka bir araç kullanılması gerekir. Daha fazla bilgi için bkz: [özel bir VHD yüklemek](create-vm-specialized.md#option-2-upload-a-specialized-vhd)

Ardından birden çok VM oluşturmak için bir VM veya VHD kullanmak istiyorsanız, bu yöntem kullanmamalısınız. Daha büyük dağıtımlar için yapmanız gerekenler [görüntü oluşturma](capture-image-resource.md) ve ardından [birden çok VM oluşturmak için bu görüntüyü kullanın](create-vm-generalized-managed.md).


## <a name="copy-a-disk"></a>Disket kopyalama

Bir anlık görüntü oluşturun, sonra bir disk anlık görüntüden oluşturun. Bu sonbaharda özgün VHD geri tutmanızı sağlar.

1. Soldaki menüde tıklayın **tüm kaynakları**.
2. İçinde **tüm türleri** açılan listesinde, seçimini **Tümünü Seç** ve ardından aşağı kaydırın ve **diskleri** kullanılabilir disk bulunamadı.
3. Kullanmak istediğiniz diske tıklayın. **Genel bakış** sayfası disk açar.
4. Genel bakış sayfasında menüsünün üstünde tıklatın **+ Oluştur anlık görüntü**. 
5. Anlık görüntü için bir ad yazın.
6. Seçin bir **kaynak grubu** anlık görüntü için. Varolan bir kaynak grubunu kullanın veya yeni bir tane oluşturun.
7. Standart (HDD) ya da Premium (SDD) depolama kullanmak isteyip istemediğinizi seçin.
8. İşiniz bittiğinde tıklatın **oluşturma** anlık görüntü oluşturmak için.
9. Anlık görüntü oluşturulduktan sonra tıklayın **+ kaynak oluşturma** soldaki menüde.
10. Arama çubuğuna **yönetilen disk** seçip **yönetilen diskleri** listeden.
11. Üzerinde **yönetilen diskleri** sayfasında, **oluşturma**.
12. Disk için bir ad yazın.
13. Seçin bir **kaynak grubu** disk. Varolan bir kaynak grubunu kullanın veya yeni bir tane oluşturun. Diskten VM oluşturduğunuz bu kaynak grubu aynı zamanda olacaktır.
14. Standart (HDD) ya da Premium (SDD) depolama kullanmak isteyip istemediğinizi seçin.
15. İçinde **kaynak türünü**, emin olun **anlık görüntü** seçilir.
16. İçinde **kaynak anlık görüntü** açılan listesinde, kullanmak istediğiniz anlık görüntüyü seçin.
17. Gerektiği gibi diğer ayarlamaları yapın ve ardından **oluşturma** disk oluşturmak için.

## <a name="create-a-vm-from-a-disk"></a>Bir diskten VM oluşturma

Yönetilen disk kullanmak istediğiniz VHD'yi oluşturduktan sonra Portalı'nda VM oluşturabilirsiniz.

1. Soldaki menüde tıklayın **tüm kaynakları**.
2. İçinde **tüm türleri** açılan listesinde, seçimini **Tümünü Seç** ve ardından aşağı kaydırın ve **diskleri** kullanılabilir disk bulunamadı.
3. Kullanmak istediğiniz diske tıklayın. **Genel bakış** sayfası disk açar.
Genel bakış sayfasında olduğundan emin olun **DISK durumu** olarak listelenen **Unattached**. Öyle değilse, sanal diski kullanımdan çıkarın ya da disk boşaltmak için VM silmeniz gerekebilir.
4. Bölmenin üstündeki menüde tıklatın **+ Oluştur VM**.
5. Üzerinde **Temelleri** sayfasında yeni VM için bir ad yazın ve ya da mevcut bir kaynak grubu seçin veya yeni bir tane oluşturun.
6. Üzerinde **boyutu** sayfasında, bir VM boyutu sayfa seçin ve ardından **seçin**.
7. Üzerinde **ayarları** sayfasında, tüm yeni kaynaklar oluşturma portal ya da izin ya da var olan seçebilirsiniz **sanal ağ** ve **ağ güvenlik grubu**. Portal her zaman oluştur yeni NIC ve genel IP adresi için yeni VM. 
8. Değişiklik izleme seçeneklerini ve gerektiği gibi tüm uzantıları ekleyin.
9. İşiniz bittiğinde **Tamam**’a tıklayın. 
10. VM yapılandırma doğrulama testlerini geçerse tıklatın **Tamam** dağıtımı başlatmak için.

## <a name="next-steps"></a>Sonraki adımlar

PowerShell için de kullanabilirsiniz [Azure'a VHD yükleyin ve özel bir VM oluşturma](create-vm-specialized.md).


