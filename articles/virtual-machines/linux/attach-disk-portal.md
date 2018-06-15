---
title: Bir Linux VM için bir veri diski ekleme | Microsoft Docs
description: Bir Linux VM için yeni veya var olan veri diski eklemek için Portalı'nı kullanın.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 5e1c6212-976c-4962-a297-177942f90907
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: cynthn
ms.openlocfilehash: 4acfe53d68db3192c1f6c3c9e5f91b55bd5df7b8
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
ms.locfileid: "30832509"
---
# <a name="use-the-portal-to-attach-a-data-disk-to-a-linux-vm"></a>Bir Linux VM için bir veri diski eklemek üzere portalı kullanın 
Bu makalede Azure portalı üzerinden Linux sanal makine için yeni ve mevcut diskleri ekleme gösterilmiştir. Ayrıca [bir Windows VM Azure portalında bir veri diski ekleme](../windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

VM'nize diskleri eklemeden önce bu ipuçlarını gözden geçirin:

* Sanal makinenin boyutunu, iliştirebilirsiniz kaç tane veri diskleri denetler. Ayrıntılar için bkz [sanal makineler için Boyutlar](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Premium depolama kullanmak için DS serisi veya GS serisi bir sanal makine gerekir. Bu sanal makinelerle Premium ve standart diskler kullanabilirsiniz. Premium depolama belirli bölgelerde kullanılabilir. Ayrıntılar için bkz [Premium Storage: Azure sanal makine iş yükleri için yüksek performanslı depolama](../windows/premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Sanal makinelere bağlı diskler Azure'da depolanan gerçekte .vhd dosyalarıdır. Ayrıntılar için bkz [diskler ve sanal makineler için VHD'ler hakkında](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="find-the-virtual-machine"></a>Sanal makine bulunamadı
1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Sol menüsünde **sanal makineleri**.
3. Listeden sanal makineyi seçin.
4. Sanal makineleri sayfa içinde **Essentials**, tıklatın **diskleri**.
   
    ![Disk Ayarları'nı açın](./media/attach-disk-portal/find-disk-settings.png)


## <a name="attach-a-new-disk"></a>Yeni bir diski kullanıma açın

1. Üzerinde **diskleri** bölmesinde tıklatın **+ Ekle veri diski**.
2. Aşağı açılan menüsüne tıklayın **adı** seçip **oluşturma disk**:

    ![Azure oluşturmak yönetilen disk](./media/attach-disk-portal/create-new-md.png)

3. Yönetilen disk için bir ad girin. Varsayılan ayarları gözden geçirin, gerektiği gibi güncelleştirin ve ardından **oluşturma**.
   
   ![Disk ayarlarını gözden geçirin](./media/attach-disk-portal/create-new-md-settings.png)

4. Tıklatın **kaydetmek** yönetilen disk oluşturmak ve VM yapılandırmasını güncelleştirmek için:

   ![Yeni Azure diski yönetilen Kaydet](./media/attach-disk-portal/confirm-create-new-md.png)

5. Azure disk oluşturur ve sanal makineye iliştirir sonra yeni disk sanal makinenin disk ayarları altında listelenen **veri diskleri**. Yönetilen diskleri en üst düzey bir kaynak olduğundan, diskin kaynak grubu kökünde görünür:

   ![Kaynak grubundaki yönetilen Azure Disk](./media/attach-disk-portal/view-md-resource-group.png)

## <a name="attach-an-existing-disk"></a>Var olan bir diski ekleme
1. Üzerinde **diskleri** bölmesinde tıklatın **+ Ekle veri diski**.
2. Aşağı açılan menüsüne tıklayın **adı** Azure aboneliğinize erişilebilir olan yönetilen disklerin listesini görüntülemek için. Yönetilen bir disk eklemek için seçin:

   ![Yönetilen mevcut Azure diski ekleme](./media/attach-disk-portal/select-existing-md.png)

3. Tıklatın **kaydetmek** mevcut yönetilen diski ekleyin ve VM yapılandırmasını güncelleştirmek için:
   
   ![Azure yönetilen diski güncelleştirmelerini kaydedin](./media/attach-disk-portal/confirm-attach-existing-md.png)

4. Azure disk sanal makineye iliştirir sonra sanal makinenin disk ayarları altında listelenen **veri diskleri**.



## <a name="next-steps"></a>Sonraki adımlar
Ayrıca [bir veri diskini](add-disk.md) Azure CLI kullanarak.
