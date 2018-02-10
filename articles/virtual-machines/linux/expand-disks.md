---
title: "Azure'da bir Linux VM sanal sabit disklerde genişletin | Microsoft Docs"
description: "Bir Linux VM Azure CLI 2.0 ile sanal sabit disklerde genişletin öğrenin"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/13/2017
ms.author: iainfou
ms.openlocfilehash: ded90be3da52770a88dd1746fae2bd3584ba9280
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="how-to-expand-virtual-hard-disks-on-a-linux-vm-with-the-azure-cli"></a>Bir Linux VM Azure CLI ile sanal sabit disklerde genişletmek nasıl
Varsayılan sanal sabit disk boyutu işletim sistemi (OS) için azure'da bir Linux sanal makine (VM) genellikle 30 GB açıktır. Yapabilecekleriniz [veri diski Ekle](add-disk.md) ek depolama alanı, ancak için sağlamak üzere ayrıca istediğiniz varolan bir veri diski genişletin. Bu makale için bir Linux VM Azure CLI 2.0 ile yönetilen diskleri genişletmek nasıl ayrıntılarını verir. Yönetilmeyen işletim sistemi diski ile genişletebilirsiniz [Azure CLI 1.0](expand-disks-nodejs.md).

> [!WARNING]
> Her zaman, disk gerçekleştirmeden önce verileri yedekleyin operations yeniden boyutlandırma emin olun. Daha fazla bilgi için bkz: [Linux VM'ler için Azure'da yedekleme](tutorial-backup-vms.md).

## <a name="expand-azure-managed-disk"></a>Azure yönetilen Disk genişletme
En son sahip olduğunuzdan emin olun [Azure CLI 2.0](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az oturum açma](/cli/azure/#az_login).

Bu makalede Azure mevcut bir VM'yi sahip bağlı ve hazırlanan en az bir veri diski gerektirir. Kullanabileceğiniz VM zaten yoksa bkz [oluşturma ve veri diskleri içeren bir VM hazırlama](tutorial-manage-disks.md#create-and-attach-disks).

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında *myResourceGroup* ve *myVM*.

1. Çalışan VM ile sanal sabit diskler üzerinde işlem gerçekleştirilemiyor. VM ile serbest bırakma [az vm serbest bırakma](/cli/azure/vm#az_vm_deallocate). Aşağıdaki örnek adlı VM kaldırır *myVM* kaynak grubunda adlı *myResourceGroup*:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > Sanal sabit diski genişletmek için VM serbest gerekir. `az vm stop`işlem kaynaklarını serbest değil. İşlem kaynakları serbest bırakmak için kullanmak `az vm deallocate`.

2. Bir kaynak grubu ile yönetilen disklerin listesini görüntülemek [az disk listesi](/cli/azure/disk#az_disk_list). Aşağıdaki örnek adlı kaynak grubunda yönetilen disklerin listesini görüntüler *myResourceGroup*:

    ```azurecli
    az disk list \
        --resource-group myResourceGroup \
        --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
        --output table
    ```

    Gerekli disk ile genişletin [az disk güncelleştirme](/cli/azure/disk#az_disk_update). Aşağıdaki örnek adlı Yönetilen disk genişletir *myDataDisk* olmasını *200*Gb boyutunda:

    ```azurecli
    az disk update \
        --resource-group myResourceGroup \
        --name myDataDisk \
        --size-gb 200
    ```

    > [!NOTE]
    > Yönetilen bir disk genişlettiğinizde, güncelleştirilmiş boyutuna yakın yönetilen disk boyutuna eşlenir. Kullanılabilir yönetilen disk boyutları ve katmanları tablosu için bkz: [Azure yönetilen diskleri fiyatlandırma ve faturalama genel bakış -](../windows/managed-disks-overview.md#pricing-and-billing).

3. VM ile başlangıç [az vm başlangıç](/cli/azure/vm#az_vm_start). Aşağıdaki örnek adlı VM başlatır *myVM* kaynak grubunda adlı *myResourceGroup*:

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```


## <a name="expand-disk-partition-and-filesystem"></a>Disk bölümü ve dosya sistemi genişletin
Genişletilmiş disk kullanmak için temel alınan bölümü ve dosya sistemi genişletmek gerekir.

1. SSH, VM uygun kimlik bilgilerine sahip. VM ile genel IP adresi elde edebilirsiniz [az vm Göster](/cli/azure/vm#az_vm_show):

    ```azurecli
    az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
    ```

2. Genişletilmiş disk kullanmak için temel alınan bölümü ve dosya sistemi genişletmek gerekir.

    a. Takılı, disk çıkarın:

    ```bash
    sudo umount /dev/sdc1
    ```

    b. Kullanım `parted` disk bilgilerini görüntülemek ve bölüm yeniden boyutlandırmak için:

    ```bash
    sudo parted /dev/sdc
    ```

    Var olan bölüm düzeni ile hakkındaki bilgileri görüntüleyin `print`. Temel disk 215 Gb boyutunda gösterilir aşağıdaki örneğe benzer bir çıkış:

    ```bash
    GNU Parted 3.2
    Using /dev/sdc1
    Welcome to GNU Parted! Type 'help' to view a list of commands.
    (parted) print
    Model: Unknown Msft Virtual Disk (scsi)
    Disk /dev/sdc1: 215GB
    Sector size (logical/physical): 512B/4096B
    Partition Table: loop
    Disk Flags:
    
    Number  Start  End    Size   File system  Flags
        1      0.00B  107GB  107GB  ext4
    ```

    c. Bölümü genişletin `resizepart`. Bölüm numarası girin *1*ve yeni bölüm için bir boyut:

    ```bash
    (parted) resizepart
    Partition number? 1
    End?  [107GB]? 215GB
    ```

    d. Çıkmak için girin`quit`

3. Yeniden boyutlandırılabilir bölümüyle ile bölüm tutarlılığını doğrula `e2fsck`:

    ```bash
    sudo e2fsck -f /dev/sdc1
    ```

4. Artık dosya sistemi ile yeniden boyutlandırın `resize2fs`:

    ```bash
    sudo resize2fs /dev/sdc1
    ```

5. İstenen konuma bölüm gibi takma `/datadrive`:

    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```

6. İşletim sistemi diski yeniden doğrulamak için kullanılan `df -h`. Aşağıdaki örnek çıkış veri sürücüsü gösterir */dev/sdc1*, 200 GB sunulmuştur:

    ```bash
    Filesystem      Size   Used  Avail Use% Mounted on
    /dev/sdc1        197G   60M   187G   1% /datadrive
    ```

## <a name="next-steps"></a>Sonraki adımlar
Ek depolama alanı, gerekirse, ayrıca [bir Linux VM için veri diski Ekle](add-disk.md). Disk şifrelemesi hakkında daha fazla bilgi için bkz: [Azure CLI kullanarak bir Linux VM disklerde şifrelemek](encrypt-disks.md).
