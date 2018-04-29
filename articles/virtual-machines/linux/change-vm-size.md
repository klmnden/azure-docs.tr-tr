---
title: Bir Linux VM Azure CLI 2.0 ile yeniden boyutlandırmak nasıl | Microsoft Docs
description: Yukarı veya Linux sanal makine, VM boyutunu değiştirerek ölçeklendirmek nasıl.
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
ms.openlocfilehash: b5c396ad047567799b7541c1a125de0c1c51c39e
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="resize-a-linux-virtual-machine-using-cli-20"></a>CLI 2.0 kullanarak bir Linux sanal makine yeniden boyutlandırma

Bir sanal makine (VM) sağlama sonra VM yukarı veya aşağı değiştirerek ölçeklendirmek [VM boyutu][vm-sizes]. Bazı durumlarda, ilk VM ayırması gerekir. İstenen boyut VM barındırma donanım kümede kullanılabilir değilse, VM ayırması gerekir. Bu makalede Azure CLI 2.0 ile bir Linux VM yeniden boyutlandırmak nasıl ayrıntılarını verir. Bu adımları [Azure CLI 1.0](change-vm-size-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ile de gerçekleştirebilirsiniz.

## <a name="resize-a-vm"></a>VM’yi yeniden boyutlandırma
Bir VM yeniden boyutlandırmak için en son gereksinim [Azure CLI 2.0](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az oturum açma](/cli/azure/reference-index#az_login).

1. İle VM barındırıldığı donanım kümede kullanılabilir VM boyutları listesini görüntülemek [az vm listesi-vm-yeniden boyutlandırma-seçenekleri](/cli/azure/vm#az_vm_list_vm_resize_options). Aşağıdaki örnek adlı VM için VM boyutları listeler `myVM` kaynak grubunda `myResourceGroup` bölge:
   
    ```azurecli
    az vm list-vm-resize-options --resource-group myResourceGroup --name myVM --output table
    ```

2. İstenen VM boyutu listeleniyorsa, VM ile yeniden boyutlandırın [az VM'yi yeniden boyutlandırın](/cli/azure/vm#az_vm_resize). Aşağıdaki örnek adlı VM yeniden boyutlandırır `myVM` için `Standard_DS3_v2` boyutu:
   
    ```azurecli
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    ```
   
    Bu işlem sırasında VM'yi yeniden başlatır. Yeniden başladıktan sonra var olan işletim sistemi ve veri diskleri eşleştirilir. Geçici diskteki herhangi bir şey kaybolur.

3. İstenen VM boyutu listede yoksa, ilk VM ile serbest bırakma gerek [az vm serbest bırakma](/cli/azure/vm#az_vm_deallocate). Bu işlem, bölge destekler ve ardından başlatılan sonra herhangi bir boyuta kullanılabilir boyutlandırılmasını VM sağlar. Aşağıdaki adımları serbest bırakma, yeniden boyutlandırma ve adlı VM Başlat `myVM` kaynak grubunda adlı `myResourceGroup`:
   
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    az vm start --resource-group myResourceGroup --name myVM
    ```
   
   > [!WARNING]
   > VM serbest bırakma ayrıca VM'ye atanan dinamik IP adreslerini serbest bırakır. İşletim sistemi ve veri diskleri etkilenmez.

## <a name="next-steps"></a>Sonraki adımlar
Ek ölçeklenebilirlik için birden çok VM örnekleri çalıştırın ve ölçeğini. Daha fazla bilgi için bkz: [otomatik olarak bir sanal makine ölçek kümesindeki Linux makineler ölçekleme][scale-set]. 

<!-- links -->
[boot-diagnostics]: https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]:sizes.md
