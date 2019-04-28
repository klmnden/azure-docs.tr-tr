---
title: Azure'da bir Linux sanal makinesi yönetilmeyen disklerden yönetilen disklere - Azure yönetilen diskleri dönüştürme | Microsoft Docs
description: Nasıl bir Linux VM Resource Manager dağıtım modelinde Azure CLI kullanarak yönetilmeyen disklerden yönetilen disklere dönüştürme
services: virtual-machines-linux
documentationcenter: ''
author: roygara
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 12/15/2017
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: d1783890ba1b304ab658e827bfd24091e208fda4
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63765708"
---
# <a name="convert-a-linux-virtual-machine-from-unmanaged-disks-to-managed-disks"></a>Linux sanal makinesi yönetilmeyen disklerden yönetilen disklere dönüştürme

Yönetilmeyen diskler kullanan bir var olan Linux sanal makineleri (VM'ler) varsa, VM'lerin kullanmasını dönüştürebilirsiniz [Azure yönetilen diskler](../linux/managed-disks-overview.md). Bu işlem, hem işletim sistemi diski hem de bağlı veri diskleri dönüştürür.

Bu makalede, Azure CLI kullanarak Vm'leri dönüştürme işlemini göstermektedir. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme](/cli/azure/install-azure-cli). 

## <a name="before-you-begin"></a>Başlamadan önce
* Gözden geçirme [yönetilen Diskler'e geçiş hakkında SSS](faq-for-disks.md#migrate-to-managed-disks).

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]


## <a name="convert-single-instance-vms"></a>Tek Örnekli VM'ler Dönüştür
Bu bölümde, tek örnek Azure Vm'leri yönetilmeyen disklerden yönetilen disklere dönüştürme ele alınmaktadır. (Bir kullanılabilirlik kümesindeki sanal makineleriniz varsa sonraki bölüme bakın.) Bu işlem, sanal makinelerin yönetilmeyen premium (SSD) yönetilmeyen diskler için premium yönetilen diskler veya standart (HDD) diskleri için standart yönetilen diskler dönüştürmek için kullanabilirsiniz.

1. Kullanarak VM'yi serbest bırakın [az vm deallocate](/cli/azure/vm). Aşağıdaki örnekte adlı VM serbest bırakılır `myVM` adlı kaynak grubunda `myResourceGroup`:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

2. Kullanarak VM'yi yönetilen disklere dönüştürme [az vm dönüştürme](/cli/azure/vm). Aşağıdaki işlem adlı VM dönüştürür `myVM`işletim sistemi diski ve veri diskleri dahil olmak üzere:

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

3. Kullanarak yönetilen disklere dönüştürme sonrasında VM'i başlatın [az vm start](/cli/azure/vm). Aşağıdaki örnekte adlı sanal makine başlatılmadan `myVM` adlı kaynak grubunda `myResourceGroup`.

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="convert-vms-in-an-availability-set"></a>Vm'leri bir kullanılabilirlik kümesine dönüştürme

Dönüştürmek istediğiniz Vm'leri yönetilen diskleri olan bir kullanılabilirlik kümesindeki, ilk kullanılabilirlik kümesini bir yönetilen kullanılabilirlik kümesine dönüştürmeniz gerekir.

Kullanılabilirlik kümesi dönüştürmeden önce kullanılabilirlik kümesindeki tüm sanal makineler serbest bırakılması gerekir. Bir yönetilen kullanılabilirlik kümesine öğesi kendisine ayarlanmış kullanılabilirlik dönüştürüldükten sonra tüm Vm'leri yönetilen disklere dönüştürme planlayın. Ardından, tüm sanal makineleri başlatın ve normal şekilde çalışmaya devam.

1. Bir kullanılabilirlik kümesinde kullanarak tüm Vm'leri listelemek [az vm kullanılabilirlik kümesi listesini](/cli/azure/vm/availability-set). Aşağıdaki örnekte adlı kullanılabilirlik kümesi içinde tüm sanal makineleri listeler `myAvailabilitySet` adlı kaynak grubunda `myResourceGroup`:

    ```azurecli
    az vm availability-set show \
        --resource-group myResourceGroup \
        --name myAvailabilitySet \
        --query [virtualMachines[*].id] \
        --output table
    ```

2. Tüm sanal makineleri kullanarak serbest [az vm deallocate](/cli/azure/vm). Aşağıdaki örnekte adlı VM serbest bırakılır `myVM` adlı kaynak grubunda `myResourceGroup`:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

3. Kullanılabilirlik kümesini kullanarak dönüştürme [az vm kullanılabilirlik kümesi dönüştürme](/cli/azure/vm/availability-set). Aşağıdaki örnekte adlı kullanılabilirlik kümesi dönüştürür `myAvailabilitySet` adlı kaynak grubunda `myResourceGroup`:

    ```azurecli
    az vm availability-set convert \
        --resource-group myResourceGroup \
        --name myAvailabilitySet
    ```

4. Kullanarak tüm sanal makineleri yönetilen disklere dönüştürme [az vm dönüştürme](/cli/azure/vm). Aşağıdaki işlem adlı VM dönüştürür `myVM`işletim sistemi diski ve veri diskleri dahil olmak üzere:

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

5. Kullanarak tüm sanal makineleri yönetilen disklere dönüştürme başlangıç [az vm start](/cli/azure/vm). Aşağıdaki örnekte adlı sanal makine başlatılmadan `myVM` adlı kaynak grubunda `myResourceGroup`:

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="next-steps"></a>Sonraki adımlar
Depolama seçenekleri hakkında daha fazla bilgi için bkz. [Azure yönetilen disklere genel bakış](../windows/managed-disks-overview.md).
