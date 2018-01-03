---
title: "Ekleyebilir veya Azure DevTest Labs içindeki bir sanal makineye bir veri diskini | Microsoft Docs"
description: "Ekleme veya Azure DevTest Labs içindeki bir sanal makineye bir veri diskini öğrenin"
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 9616bf38-7db8-4915-a32a-e4f40a7a56ad
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: v-craic
ms.openlocfilehash: c1f83097fe4e5da3a46f693d37b001dbb6831527
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="attach-or-detach-a-data-disk-to-a-virtual-machine-in-azure-devtest-labs"></a>Ekleme veya Azure DevTest Labs içindeki bir sanal makineye bir veri diskini ayırma
[Azure yönetilen diskleri](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview) sanal makine veri diski ile ilişkilendirilmiş depolama hesaplarını yönetir. Yeni bir veri diski bir VM için bir kullanıcı ekler, gerekli olan disk boyutunu ve türünü belirtir ve Azure oluşturur ve disk otomatik olarak yönetir. Veri diski sanal makineden sonra ayrılabilir ve ya da aynı VM sonraki ya da aynı kullanıcıya ait farklı bir VM ekli yeniden.

Bu işlevsellik, depolama veya her bir sanal makine dışında yazılım yönetmek için kullanışlıdır. İçinde bir veri diski depolama veya yazılım zaten varsa, kolayca bağlı, ayrılmış ve bu veri diski sahip kullanıcıya ait VM yeniden.

## <a name="attach-a-data-disk"></a>Veri diski ekleme
Bir VM için bir veri diski eklemeden önce bu ipuçlarını gözden geçirin:

- VM boyutu, iliştirebilirsiniz kaç tane veri diskleri denetler. Ayrıntılar için bkz [sanal makineler için Boyutlar](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).
- Bu gibi durumlarda, bir veri diski yalnızca çalışmakta olan bir VM ekleyebilirsiniz. Bir veri diskini denemeden önce VM'nin çalıştığından emin olun.

### <a name="attach-a-new-disk"></a>Yeni bir diski kullanıma açın
Oluşturun ve VM Azure DevTest Labs ile yeni bir yönetilen veri diski eklemek için aşağıdaki adımları izleyin.

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Seçin **daha Hizmetleri**ve ardından **DevTest Labs** listeden.
1. İstenen Laboratuvar labs listesinden seçin. 
1. Listesinden **My sanal makineleri**, çalışan bir VM seçin.
1. Sol taraftaki menüden seçin **diskleri**.

    ![Bir sanal makine için veri diskleri seçin](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-data-disk.png)
1. Seçin **Attach yeni** yeni bir veri diski oluşturma ve VM'e ekleyin.

    ![Yeni veri diski bir sanal makineye İliştir](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-new.png)
1. Tamamlamak **Attach yeni disk** veri disk adı, türü ve boyutunu girerek bölmesi.

    !["Yeni disk ekleme" formu doldurun](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-new-form.png)
1. **Tamam**’ı seçin.

Birkaç dakika sonra yeni veri diski oluşturulur ve VM'ye bağlı ve listesinde görünür **veri DİSKLERİ** bu VM.

### <a name="attach-an-existing-disk"></a>Var olan bir diski ekleme
Çalışan bir VM için mevcut bir kullanılabilir veri diski yeniden eklemek için aşağıdaki adımları izleyin. 

1. Bir veri diski yeniden bağlayın istediğiniz çalışan bir VM seçin.
1. Sol taraftaki menüden seçin **diskleri**.
1. Seçin **Attach varolan** kullanılabilir veri diski VM'e.

    ![Varolan bir veri diski bir sanal makineye İliştir](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-existing2.png)

1. Gelen **varolan bir diski İlişti** bölmesinde, select Tamam.

    ![Varolan bir veri diski bir sanal makineye İliştir](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-existing.png)

Birkaç dakika sonra veri diski VM'ye ekli ve listesinde görünür **veri DİSKLERİ** bu VM.

## <a name="detach-a-data-disk"></a>Veri diski çıkarma
Bir VM'ye ekli bir veri diski artık ihtiyacınız olduğunda, kolayca ayırabilirsiniz. Ayırma diski sanal makineden kaldırır, ancak daha sonra kullanmak için depolama birimindeki tutar.

Disk üzerinde var olan verileri yeniden kullanmak istiyorsanız, aynı sanal makine ya da başka bir iliştirebilirsiniz.

### <a name="detach-from-the-vms-management-pane"></a>VM'ın yönetim bölmesinden ayırma
1. Sanal makineler listesinden, bağlı bir veri diski VM seçin.
1. Sol taraftaki menüden seçin **diskleri**.

    ![Bir sanal makine için veri diskleri seçin](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-data-disk.png) 
1. Listesinden **veri DİSKLERİ**, ayırmak istediğiniz veri diski seçin.
1. Seçin **ayırma** diskin Ayrıntılar bölmesinin en üstünden.

    ![Veri diski çıkarma](./media/devtest-lab-attach-detach-data-disk/devtest-lab-detach-data-disk2.png)
1. Seçin **Evet** veri diskini istediğinizi onaylamak için.

Disk ayrılır ve başka bir VM'e kullanılabilir. 
### <a name="detach-from-the-labs-main-pane"></a>Laboratuvar ait ana bölmeden ayırma
1. Laboratuvarınızı ait ana bölmede, seçin **veri disklerim**.

    ![Laboratuvar ait veri diskleri erişim](./media/devtest-lab-attach-detach-data-disk/devtest-lab-my-data-disks.png)
1. Detach – veya üç nokta (...) – seçip istediğiniz veri diski sağ **ayırma**.

    ![Veri diski çıkarma](./media/devtest-lab-attach-detach-data-disk/devtest-lab-detach-data-disk.png)
1. Seçin **Evet** onu ayırmanız istediğinizi onaylamak için.

   > [!NOTE]
   > Bir veri diski zaten ayrıldı, seçerek kullanılabilir veri diskleri listenizden kaldırmayı seçebilirsiniz **silmek**.
   >
   >

## <a name="upgrade-an-unmanaged-data-disk"></a>Bir yönetilmeyen veri diski yükseltme
Yönetilmeyen veri diskleri kullanan mevcut bir VM'yi varsa, yönetilen diskleri kullanmak üzere VM kolayca dönüştürebilirsiniz. Bu işlem, işletim sistemi diski ve her eklenen veri disklerini dönüştürür.

Bir yönetilmeyen veri diski yükseltmek için bu makalede açıklanan adımları izleyin [veri diskini](#detach-a-data-disk) yönetilmeyen bir VM'den. Ardından, [diski yeniden bağlayın](#attach-an-existing-disk) verileri otomatik olarak yükseltmek için yönetilen bir sanal diskten yönetilene.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
Veri diskleri için yönetmeyi öğrenin [claimable sanal makineleri](devtest-lab-add-claimable-vm.md#unclaim-a-vm).

