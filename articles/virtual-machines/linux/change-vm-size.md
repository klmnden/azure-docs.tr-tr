---
title: Azure CLI ile Linux VM yeniden boyutlandırma | Microsoft Docs
description: Bir Linux sanal makineyi, VM boyutunu değiştirerek ölçeğini artırabilir veya nasıl.
services: virtual-machines-linux
documentationcenter: na
author: mikewasson
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: e163f878-b919-45c5-9f5a-75a64f3b14a0
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2017
ms.author: mwasson
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8bd8136c61a277e730c07c8789afd1bf3214bd7c
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62127081"
---
# <a name="resize-a-linux-virtual-machine-using-azure-cli"></a>Azure CLI kullanarak bir Linux sanal makinesi yeniden boyutlandırma 

Bir sanal makine (VM) sağladıktan sonra VM ölçeğini artırıp değiştirerek ölçeklendirebilirsiniz [VM boyutu][vm-sizes]. Bazı durumlarda, ilk VM ayırması gerekir. İstenen boyut VM'yi barındıran donanım kümesinde kullanılabilir değilse, VM'yi serbest bırakın gerekir. Bu makalede, Azure CLI ile Linux VM'yi yeniden boyutlandırma işlemi açıklanmaktadır. 

## <a name="resize-a-vm"></a>VM’yi yeniden boyutlandırma
VM'yi yeniden boyutlandırma için en son gerekir [Azure CLI](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az login](/cli/azure/reference-index).

1. İle sanal Makinenin barındırıldığı donanım kümesinde kullanılabilen VM boyutlarının listesini görmek [az vm list-vm-resize-options](/cli/azure/vm). Aşağıdaki örnekte adlı VM'nin VM boyutları listeler `myVM` kaynak grubundaki `myResourceGroup` bölgesi:
   
    ```azurecli
    az vm list-vm-resize-options --resource-group myResourceGroup --name myVM --output table
    ```

2. İstenen VM boyutu listeleniyorsa, VM yeniden boyutlandırma [az vm yeniden boyutlandırma](/cli/azure/vm). Aşağıdaki örnekte adlı VM yeniden boyutlandırır `myVM` için `Standard_DS3_v2` boyutu:
   
    ```azurecli
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    ```
   
    Bu işlem sırasında yeniden başlatır. Yeniden başlatma sonrasında, mevcut işletim sistemi ve veri diskleri eşleştirilir. Geçici disk üzerinde herhangi bir şey kaybolur.

3. İstenen VM boyutu listede yoksa, önce sanal makine ile serbest gerek [az vm deallocate](/cli/azure/vm). Bu işlem, bölge destekler ve sonradan başlatılmasından sonra herhangi bir boyuta kullanılabilir boyutlandırılmaya VM sağlar. Aşağıdaki adımları serbest bırakın, yeniden boyutlandırma ve adlı sanal makine başlatın `myVM` adlı kaynak grubunda `myResourceGroup`:
   
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    az vm start --resource-group myResourceGroup --name myVM
    ```
   
   > [!WARNING]
   > VM serbest bırakılıyor da sanal Makineye atanan dinamik IP adreslerini serbest bırakır. İşletim sistemi ve veri diskleri etkilenmez.

## <a name="next-steps"></a>Sonraki adımlar
Ek ölçeklenebilirlik için birden fazla VM örneği çalıştırın ve ölçeği genişletme. Daha fazla bilgi için [Linux makineleri bir sanal makine ölçek kümesi'nde otomatik ölçeklendirme][scale-set]. 

<!-- links -->
[boot-diagnostics]: https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]:sizes.md
