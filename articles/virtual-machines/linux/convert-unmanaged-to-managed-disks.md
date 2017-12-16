---
title: "Azure'da bir Linux sanal makine yönetilmeyen disklerden yönetilen disklere - Azure yönetilen diskleri dönüştürün. | Microsoft Docs"
description: "Resource Manager dağıtım modelinde Azure CLI 2.0 kullanarak bir Linux VM yönetilmeyen disklerden yönetilen diskleri dönüştürme"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 12/15/2017
ms.author: iainfou
ms.openlocfilehash: 533d4ddfc645843ed8feb8652021f47d93ed2ac1
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2017
---
# <a name="convert-a-linux-virtual-machine-from-unmanaged-disks-to-managed-disks"></a>Linux sanal makine yönetilmeyen disklerden yönetilen Diske Dönüştür

Yönetilmeyen diskleri kullanan bir varolan Linux sanal makineleri (VM'ler) varsa, kullanmak için sanal makineleri dönüştürebilirsiniz [Azure yönetilen diskleri](../linux/managed-disks-overview.md). Bu işlem, işletim sistemi diski ve her eklenen veri disklerini dönüştürür.

Bu makalede Azure CLI kullanarak sanal makineleri dönüştürmek nasıl gösterir. Gerekirse yüklemek veya yükseltmek için bkz: [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). 

## <a name="before-you-begin"></a>Başlamadan önce
* Gözden geçirme [yönetilen disklere geçişi hakkında SSS](faq-for-disks.md#migrate-to-managed-disks).

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]


## <a name="convert-single-instance-vms"></a>Tek Örnekli VM'ler Dönüştür
Bu bölümde tek örnekli Azure VM'ler yönetilmeyen disklerden yönetilen disklere dönüştürmek nasıl ele alınmaktadır. (Bir kullanılabilirlik kümesine Vm'leriniz varsa sonraki bölüme bakın.) Standart yönetilen disk yönetilmeyen premium (SSD) yönetilen premium diskleri veya standardı (HDD) diskleri yönetilmeyen disklerden VM'ler dönüştürmek için bu işlemi kullanın.

1. Kullanarak VM serbest bırakma [az vm serbest bırakma](/cli/azure/vm#deallocate). Aşağıdaki örnek adlı VM kaldırır `myVM` kaynak grubunda adlı `myResourceGroup`:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

2. VM kullanarak yönetilen Diske Dönüştür [az vm dönüştürme](/cli/azure/vm#convert). Aşağıdaki işlem adlı VM dönüştürür `myVM`işletim sistemi diski ve veri diskleri dahil olmak üzere:

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

3. Kullanarak yönetilen disklere dönüştürmeden sonra VM'yi başlatın [az vm başlangıç](/cli/azure/vm#start). Aşağıdaki örnek adlı VM başlatır `myVM` kaynak grubunda adlı `myResourceGroup`.

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="convert-vms-in-an-availability-set"></a>Sanal makineleri bir kullanılabilirlik kümesine Dönüştür

Dönüştürmek istediğiniz sanal makineleri yönetiliyorsa diskleri bir kullanılabilirlik kümesine, önce bir yönetilen kullanılabilirlik kümesi kullanılabilirlik dönüştürmeniz gerekir.

Kullanılabilirlik kümesi dönüştürmeden önce kullanılabilirlik kümesindeki tüm VM'ler serbest gerekir. Kullanılabilirlik kendisini kümesini yönetilen kullanılabilirlik kümesine dönüştürüldükten sonra tüm VM'ler için yönetilen diskleri dönüştürme planlayın. Ardından, tüm sanal makineleri başlatın ve normal olarak çalışmaya devam.

1. Kullanılabilirlik kümesi kullanarak tüm sanal makineleri listesinde [az vm kullanılabilirlik kümesi listesi](/cli/azure/vm/availability-set#list). Aşağıdaki örnek, kullanılabilirlik adlandırılmış kümesi tüm sanal makineleri listeler `myAvailabilitySet` kaynak grubunda adlı `myResourceGroup`:

    ```azurecli
    az vm availability-set show \
        --resource-group myResourceGroup \
        --name myAvailabilitySet \
        --query [virtualMachines[*].id] \
        --output table
    ```

2. Tüm sanal makineleri kullanarak ayırması [az vm serbest bırakma](/cli/azure/vm#deallocate). Aşağıdaki örnek adlı VM kaldırır `myVM` kaynak grubunda adlı `myResourceGroup`:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

3. Kullanılabilirlik kümesini kullanarak Dönüştür [az vm kullanılabilirlik kümesi dönüştürme](/cli/azure/vm/availability-set#convert). Aşağıdaki örnek kullanılabilirlik adlandırılmış kümesini dönüştürür `myAvailabilitySet` kaynak grubunda adlı `myResourceGroup`:

    ```azurecli
    az vm availability-set convert \
        --resource-group myResourceGroup \
        --name myAvailabilitySet
    ```

4. Tüm sanal makineleri kullanarak yönetilen Diske Dönüştür [az vm dönüştürme](/cli/azure/vm#convert). Aşağıdaki işlem adlı VM dönüştürür `myVM`işletim sistemi diski ve veri diskleri dahil olmak üzere:

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

5. Kullanarak tüm sanal makineleri yönetilen diskleri dönüştürme başlangıç [az vm başlangıç](/cli/azure/vm#start). Aşağıdaki örnek adlı VM başlatır `myVM` kaynak grubunda adlı `myResourceGroup`:

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="next-steps"></a>Sonraki adımlar
Depolama seçenekleri hakkında daha fazla bilgi için bkz: [Azure yönetilen diskleri genel bakış](../windows/managed-disks-overview.md).
