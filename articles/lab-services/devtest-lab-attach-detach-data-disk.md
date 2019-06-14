---
title: Ekleyebilir veya Azure DevTest Labs'de sanal makinesine veri diski çıkarma | Microsoft Docs
description: Ekleyebilir veya Azure DevTest Labs'de sanal makinesine veri diski çıkarma konusunda bilgi edinin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 9616bf38-7db8-4915-a32a-e4f40a7a56ad
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: spelluru
ms.openlocfilehash: 2e168867ed342fb0b0545b5fdc330ba790f78de0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60304522"
---
# <a name="attach-or-detach-a-data-disk-to-a-virtual-machine-in-azure-devtest-labs"></a>Ekleyebilir veya Azure DevTest Labs'de sanal makinesine veri diski çıkarma
[Azure yönetilen diskler](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview) sanal makine diskleri ile ilişkilendirilmiş depolama hesaplarını yönetir. Yeni bir veri diski bir VM için bir kullanıcı ekler, gerekli olan disk boyutunu ve türünü belirtir ve Azure oluşturur ve disk otomatik olarak yönetir. Veri diski sanal makineden ardından ayrılabilir ve daha sonra aynı VM'e ya da aynı kullanıcıya ait farklı bir VM ekli ya da eklenemeyeceği.

Bu işlev, depolama veya her bir sanal makinenin dışında yazılım yönetmek için kullanışlı bir yöntemdir. Depolama ya da yazılım içindeki bir veri diski zaten varsa kolayca bağlı, ayrılmış ve bu veri diskine sahip bir kullanıcı tarafından sahip olunan herhangi bir VM için eklenemez.

## <a name="attach-a-data-disk"></a>Veri diski ekleme
Bir VM'ye veri diski ekleme önce bu ipuçlarını gözden geçirin:

- VM boyutunu, iliştirebilirsiniz kaç veri diskinin denetler. Ayrıntılar için bkz [sanal makine boyutları](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).
- Yalnızca, çalışan bir VM'ye veri diski ekleyebilirsiniz. Veri diski denemeden önce VM'nin çalıştığından emin olun.

### <a name="attach-a-new-disk"></a>Yeni bir disk ekleme
Azure DevTest labs'deki bir VM için yeni yönetilen veri diski ekleme ve oluşturmak için aşağıdaki adımları izleyin.

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
1. İstenen Laboratuvar labs listesinden seçin. 
1. Listesinden **sanal makinelerim**, çalışan bir VM seçin.
1. Sol taraftaki menüden **diskleri**.

    ![Bir sanal makine için veri disklerini seçin](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-data-disk.png)
1. Seçin **iliştirme yeni** yeni bir veri diski oluşturulup VM'e ekleyin.

    ![Bir sanal makine için yeni veri diski ekleme](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-new.png)
1. Tamamlamak **yeni disk Attach** bölmesinde bir veri disk adı, türü ve boyutu girerek.

    !["Yeni diski" formu doldurun](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-new-form.png)
1. **Tamam**’ı seçin.

Birkaç dakika sonra yeni bir veri diski oluşturulur ve VM'ye listesinde görünür **veri DİSKLERİ** bu VM için.

### <a name="attach-an-existing-disk"></a>Var olan bir diski ekleme
Çalışan bir VM için mevcut bir kullanılabilir veri diskini yeniden ekleme için bu adımları izleyin. 

1. Bir veri diskini yeniden ekleme istediğiniz çalışan bir VM seçin.
1. Sol taraftaki menüden **diskleri**.
1. Seçin **ekleme mevcut** kullanılabilir veri diski sanal makineye bağlayın.

    ![Bir sanal makineye mevcut veri diski ekleme](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-existing2.png)

1. Gelen **mevcut diski** bölmesinde Tamam'ı seçin.

    ![Bir sanal makineye mevcut veri diski ekleme](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-existing.png)

Birkaç dakika sonra veri diski VM'ye bağlı ve listesinde görünür **veri DİSKLERİ** bu VM için.

## <a name="detach-a-data-disk"></a>Veri diski çıkarma
Bir VM'ye ekli bir veri diskine ihtiyacınız olmadığında bunu kolayca ayırabilirsiniz. Ayırma diski sanal makineden kaldırır, ancak daha sonra kullanmak için depolama alanında tutar.

Mevcut verileri disk üzerinde yeniden kullanmak isterseniz, aynı sanal makineye veya başka bir iliştirebilirsiniz.

### <a name="detach-from-the-vms-management-pane"></a>Sanal makinenin yönetim bölmesinden Ayır
1. Ekli bir veri diskine sahip bir VM, sanal makineler listesinden seçin.
1. Sol taraftaki menüden **diskleri**.

    ![Bir sanal makine için veri disklerini seçin](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-data-disk.png) 
1. Listesinden **veri DİSKLERİ**, ayırmak istediğiniz veri diski seçin.
1. Seçin **ayırma** diskin Ayrıntılar bölmesinin üst.

    ![Veri diski çıkarma](./media/devtest-lab-attach-detach-data-disk/devtest-lab-detach-data-disk2.png)
1. Seçin **Evet** veri diskini ayırmak istediğinizi onaylayın.

Disk ayrıldı ve başka bir sanal makineye eklemek kullanılabilir. 
### <a name="detach-from-the-labs-main-pane"></a>Laboratuvar ana bölmeden Ayır
1. Laboratuvarınızı kişinin ana bölmeden **veri disklerim**.

    ![Veri diskleri Laboratuvar erişim](./media/devtest-lab-attach-detach-data-disk/devtest-lab-my-data-disks.png)
1. Veri diskini ayırma – veya üç nokta işaretine (...) – seçip istediğiniz sağ **ayırma**.

    ![Veri diski çıkarma](./media/devtest-lab-attach-detach-data-disk/devtest-lab-detach-data-disk.png)
1. Seçin **Evet** diski ayırmaya istediğinizi onaylayın.

   > [!NOTE]
   > Veri diski kullanımdan zaten çıkarılmış, seçerek mevcut veri disklerinin listenizden kaldırmak seçebilirsiniz **Sil**.
   >
   >

## <a name="upgrade-an-unmanaged-data-disk"></a>Yönetilmeyen veri diski yükseltme
Yönetilmeyen veri disklerini kullanan mevcut bir VM varsa, yönetilen diskleri kullanmak üzere VM kolayca dönüştürebilirsiniz. Bu işlem, hem işletim sistemi diski hem de bağlı veri diskleri dönüştürür.

Yönetilmeyen veri diski yükseltmek için bu makalede açıklanan adımları izleyin. [veri diski çıkarma](#detach-a-data-disk) yönetilmeyen bir VM'den. Ardından, [diskini yeniden ekleme](#attach-an-existing-disk) verileri otomatik olarak yükseltmek için yönetilen bir sanal makine için diskten yönetilene.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
Veri diskleri için yönetmeyi öğrenin [talep edilebilir sanal makineler](devtest-lab-add-claimable-vm.md#unclaim-a-vm).

