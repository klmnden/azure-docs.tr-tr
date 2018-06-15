---
title: CLI kullanarak bir Azure VM için takas işletim sistemi diski | Microsoft Docs
description: Bir Azure CLI kullanarak sanal makine tarafından kullanılan işletim sistemi diski olarak değiştirin.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 04/24/2018
ms.author: cynthn
ms.openlocfilehash: 1732b60ee135b765cdeea43f981bcef9575ff32c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32196213"
---
# <a name="change-the-os-disk-used-by-an-azure-vm-using-the-cli"></a>CLI kullanarak bir Azure VM tarafından kullanılan işletim sistemi diski değiştirme


Mevcut bir VM'yi var ancak bir yedek diski veya başka bir işletim sistemi diski için disk takas etmek istediğiniz işletim sistemi diski değiştirmek için Azure CLI kullanabilirsiniz. VM silip gerekmez. Zaten kullanımda olmadığı sürece, yönetilen bir diski başka bir kaynak grubunda bile kullanabilirsiniz.

VM stopped\deallocated olmasına gerek yoktur ve farklı bir yönetilen disk kaynak kimliği ile kaynak kimliği yönetilen diskin değiştirilebilir. 

VM boyutu ve depolama türü eklemek için kullanmak istediğiniz disk ile uyumlu olduğundan emin olun. Premium depolama alanına kullanmak istediğiniz disk ise, örneğin, daha sonra VM (DS serisi boyutu gibi) Premium depolama özelliğine sahip olması gerekir.

Bu makale Azure CLI Sürüm 2.0.25 gerektirir veya daha büyük. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 


Kullanım [az disk listesi](/cli/azure/disk#list) kaynak grubunuzdaki disklerin listesini almak için.

```azurecli-interactive
az disk list \
   -g myResourceGroupDisk \
   --query '[*].{diskId:id}' \
   --output table
```


Kullanım [az vm Dur](/cli/azure/vm#stop) stop\deallocate diskleri değiştirmeden önce VM için.

```azurecli-interactive
az vm stop \
   -n myVM \
   -g myResourceGroup
```


Kullanım [az vm güncelleştirmesi](/cli/azure/vm#az-vm-update) yeni disk için tam kaynak kimliği `--osdisk` parametresi 

```azurecli-interactive 
az vm update \
   -g myResourceGroup \
   -n myVM \
   --os-disk /subscriptions/<subscription ID>/resourceGroups/swap/providers/Microsoft.Compute/disks/myDisk 
   ```
   
VM kullanarak yeniden [az vm başlangıç](/cli/azure/vm#start).

```azurecli-interactive
az vm start \
   -n myVM \
   -g myResourceGroup
```

   
**Sonraki adımlar**

Bir disk bir kopyasını oluşturmak için bkz: [bir disk anlık görüntü](snapshot-copy-managed-disk.md).