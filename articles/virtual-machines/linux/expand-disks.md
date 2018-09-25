---
title: Azure'da bir Linux sanal makinesinde sanal sabit diskleri genişletin | Microsoft Docs
description: Azure CLI ile Linux sanal makinesinde sanal sabit diskleri genişletme hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: roygara
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/13/2017
ms.author: rogarana
ms.openlocfilehash: 0c2d4d1413b6cfd0b5e457e720b59c6c7b575092
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46974553"
---
# <a name="how-to-expand-virtual-hard-disks-on-a-linux-vm-with-the-azure-cli"></a>Azure CLI ile Linux sanal makinesinde sanal sabit diskleri genişletme nasıl

Varsayılan sanal sabit disk boyutu işletim sistemi (OS) için azure'da bir Linux sanal makinesi (VM) genellikle 30 GB açıktır. Yapabilecekleriniz [veri diskleri ekleme](add-disk.md) ek depolama alanı, ancak için sağlamak ayrıca istediğiniz varolan bir veri diski genişletin. Bu makalede, Azure CLI ile Linux VM için yönetilen diskleri genişletme işlemi açıklanmaktadır. 

> [!WARNING]
> Her zaman, disk gerçekleştirmeden önce geri verilerinizi işlemleri yeniden boyutlandırma emin olun. Daha fazla bilgi için [azure'da Linux VM'ler yedekleme](tutorial-backup-vms.md).

## <a name="expand-azure-managed-disk"></a>Azure yönetilen diski genişletin
En son sahip olduğunuzdan emin olun [Azure CLI](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az login](/cli/azure/reference-index#az_login).

Bu makale, mevcut bir VM'yi azure'da bağlı ve hazırlanmış en az bir veri diski gerekir. Kullanabileceğiniz bir VM zaten yoksa bkz [oluşturun ve veri diskleri ile sanal makine hazırlama](tutorial-manage-disks.md#create-and-attach-disks).

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında *myResourceGroup* ve *myVM*.

1. Çalışan VM ile sanal sabit diskler üzerinde işlem gerçekleştirilemiyor. VM'nizi serbest [az vm deallocate](/cli/azure/vm#az_vm_deallocate). Aşağıdaki örnekte adlı VM serbest bırakılır *myVM* adlı kaynak grubunda *myResourceGroup*:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > Sanal sabit diski genişletmek için VM'nin serbest bırakılması gerekir. `az vm stop` işlem kaynaklarını serbest bırakmaz. İşlem kaynaklarını serbest bırakın, kullanın `az vm deallocate`.

1. Bir kaynak grubu ile yönetilen disklerin listesini görüntülemek [az disk listesi](/cli/azure/disk#az_disk_list). Aşağıdaki örnekte adlı kaynak grubunda bulunan yönetilen disklerin listesini görüntüler *myResourceGroup*:

    ```azurecli
    az disk list \
        --resource-group myResourceGroup \
        --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
        --output table
    ```

    Gerekli disk genişletin [az disk update](/cli/azure/disk#az_disk_update). Aşağıdaki örnekte adlı Yönetilen disk genişletir *myDataDisk* olmasını *200*Gb cinsinden boyutu:

    ```azurecli
    az disk update \
        --resource-group myResourceGroup \
        --name myDataDisk \
        --size-gb 200
    ```

    > [!NOTE]
    > Yönetilen disk genişlettiğinizde, güncelleştirilmiş boyutu için en yakın yönetilen disk boyutu eşlenir. Kullanılabilir yönetilen disk boyutları ve katmanları tablo için bkz: [Azure yönetilen diskler fiyatlandırma ve faturalama genel bakış -](../windows/managed-disks-overview.md#pricing-and-billing).

1. VM'nizi Başlat [az vm start](/cli/azure/vm#az_vm_start). Aşağıdaki örnekte adlı sanal makine başlatılmadan *myVM* adlı kaynak grubunda *myResourceGroup*:

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```


## <a name="expand-disk-partition-and-filesystem"></a>Disk bölümü ve dosya sistemi genişletin
Genişletilmiş disk kullanmak için temel alınan bölüm ve dosya sistemi genişletin gerekir.

1. Uygun kimlik bilgileri ile sanal makinenize yönelik SSH. İle sanal makinenizin genel IP adresini alabilirsiniz [az vm show](/cli/azure/vm#az_vm_show):

    ```azurecli
    az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
    ```

1. Genişletilmiş disk kullanmak için temel alınan bölüm ve dosya sistemi genişletin gerekir.

    a. Önceden oluşturulmuş, diski çıkarın:

    ```bash
    sudo umount /dev/sdc1
    ```

    b. Kullanım `parted` disk bilgilerini görüntülemek ve bölüm yeniden boyutlandırmak için:

    ```bash
    sudo parted /dev/sdc
    ```

    Var olan bir bölüm düzeni ile ilgili bilgileri görüntüleyebilir `print`. Temel disk 215 Gb boyutundaki olduğunu gösteren aşağıdaki örneğe benzer bir çıktı.

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

    c. Bölümü Genişlet `resizepart`. Bölüm numarası girin *1*ve yeni bölümü için bir boyut:

    ```bash
    (parted) resizepart
    Partition number? 1
    End?  [107GB]? 215GB
    ```

    d. Çıkmak için girin `quit`

1. Bölüm tutarlılık ile yeniden boyutlandırılmış bölümüyle doğrulayın `e2fsck`:

    ```bash
    sudo e2fsck -f /dev/sdc1
    ```

1. Artık dosya sistemi ile yeniden boyutlandırma `resize2fs`:

    ```bash
    sudo resize2fs /dev/sdc1
    ```

1. İstenen konuma, bölüme gibi bağlama `/datadrive`:

    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```

1. İşletim sistemi diskini yeniden doğrulamak için `df -h`. Aşağıdaki örnek çıktı gösterilmektedir veri sürücüsü */dev/sdc1*, 200 GB sunulmuştur:

    ```bash
    Filesystem      Size   Used  Avail Use% Mounted on
    /dev/sdc1        197G   60M   187G   1% /datadrive
    ```

## <a name="next-steps"></a>Sonraki adımlar
Ek depolama alanı gerekiyorsa, ayrıca [bir Linux VM'ye veri diski ekleme](add-disk.md). Disk şifrelemesi hakkında daha fazla bilgi için bkz: [Azure CLI kullanarak bir Linux sanal diskleri şifreleme](encrypt-disks.md).
