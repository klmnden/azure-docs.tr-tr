---
title: Azure CLI 1.0 ile Linux VM üzerindeki işletim sistemi diski genişletin | Microsoft Docs
description: Azure CLI 1.0 ve Resource Manager dağıtım modeli kullanarak bir Linux VM üzerindeki işletim sistemi (OS) sanal diski Genişlet öğrenin
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: f81054727bb1f0e8ffa752783e866a72d573589d
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30905342"
---
# <a name="expand-os-disk-on-a-linux-vm-using-the-azure-cli-with-the-azure-cli-10"></a>Azure CLI 1.0 ile Azure CLI kullanarak bir Linux VM üzerindeki işletim sistemi diski genişletin
Varsayılan sanal sabit disk boyutu işletim sistemi (OS) için azure'da bir Linux sanal makine (VM) genellikle 30 GB açıktır. Yapabilecekleriniz [veri diski Ekle](add-disk.md) ek depolama alanı, ancak için sağlamak üzere ayrıca istediğiniz işletim sistemi diski genişletin. Bu makalede Azure CLI 1.0 ile yönetilmeyen diskleri kullanarak bir Linux VM için işletim sistemi diski genişletmek nasıl ayrıntılarını verir.

## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri
Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

- [Azure CLI 1.0](#prerequisites) – bizim CLI Klasik ve kaynak yönetimi dağıtım modeline (Bu makalede)
- [Azure CLI 2.0](expand-disks.md): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız

## <a name="prerequisites"></a>Önkoşullar
Gereksinim duyduğunuz [en son Azure CLI 1.0](../../cli-install-nodejs.md) yüklü ve oturum açılan bir [Azure hesabı](https://azure.microsoft.com/pricing/free-trial/) Resource Manager modunu aşağıdaki gibi kullanarak:

```azurecli
azure config mode arm
```

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında *myResourceGroup* ve *myVM*.

## <a name="expand-os-disk"></a>İşletim sistemi diskini genişletme

1. Çalışan VM ile sanal sabit diskler üzerinde işlem gerçekleştirilemiyor. Aşağıdaki örnek durdurur ve adlı VM kaldırır *myVM* kaynak grubunda adlı *myResourceGroup*:

    ```azurecli
    azure vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > `azure vm stop` işlem kaynaklarını serbest değil. İşlem kaynakları serbest bırakmak için kullanmak `azure vm deallocate`. Sanal sabit diski genişletmek için VM serbest gerekir.

2. Yönetilmeyen işletim sistemi diski kullanarak boyutunu güncelleştirme `azure vm set` komutu. Aşağıdaki örnek adlı VM güncelleştirmeleri *myVM* kaynak grubunda adlı *myResourceGroup* olmasını *50* GB:

    ```azurecli
    azure vm set \
        --resource-group myResourceGroup \
        --name myVM \
        --new-os-disk-size 50
    ```

3. VM şu şekilde başlatın:

    ```azurecli
    azure vm start --resource-group myResourceGroup --name myVM
    ```

4. SSH, VM uygun kimlik bilgilerine sahip. İşletim sistemi diski yeniden doğrulamak için kullanılan `df -h`. Aşağıdaki örnek çıkış birincil bölüm gösterir (*/dev/sda1*) 50 GB sunulmuştur:

    ```bash
    Filesystem      Size  Used Avail Use% Mounted on
    udev            1.7G     0  1.7G   0% /dev
    tmpfs           344M  5.0M  340M   2% /run
    /dev/sda1        49G  1.3G   48G   3% /
    ```

## <a name="next-steps"></a>Sonraki adımlar
Ek depolama alanı, gerekirse, ayrıca [bir Linux VM için veri diski Ekle](add-disk.md). Disk şifrelemesi hakkında daha fazla bilgi için bkz: [Azure CLI kullanarak bir Linux VM disklerde şifrelemek](encrypt-disks.md).
