---
title: Azure CLI kullanarak bir VM için takas işletim sistemi diski | Microsoft Docs
description: CLI kullanarak Azure sanal makinesi tarafından kullanılan işletim sistemi diski olarak değiştirin.
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
ms.openlocfilehash: b17647a09c88491e2486046b1ca99ee277f0cc28
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61473901"
---
# <a name="change-the-os-disk-used-by-an-azure-vm-using-the-cli"></a>Azure CLI kullanarak bir VM tarafından kullanılan işletim sistemi diskini değiştirme


Mevcut bir VM'ye sahip, ancak Yedekleme diski veya başka bir işletim sistemi diski için disk takas etmek istediğiniz işletim sistemi diskleri takas etmek için Azure CLI'yı kullanabilirsiniz. VM'yi silip yeniden yükleme gerekmez. Zaten kullanımda olmadığı sürece, başka bir kaynak grubunda bile yönetilen disk kullanabilirsiniz.

VM, stopped\deallocated olmasına gerek yoktur ve sonra yönetilen diskin kaynak kimliği farklı bir yönetilen diskin kaynak kimliği ile değiştirilebilir. 

VM boyutu ile depolama türü eklemek için kullanmak istediğiniz disk ile uyumlu olduğundan emin olun. Kullanmak istediğiniz disk bir Premium depolama ise, örneğin, sonra VM (DS serisi boyutu gibi) Premium depolama özelliğine sahip olması gerekir.

Bu makalede Azure CLI 2.0.25 sürüm gerektirir veya büyük. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli). 


Kullanım [az disk listesi](/cli/azure/disk) kaynak grubunuzda disklerin listesini almak için.

```azurecli-interactive
az disk list \
   -g myResourceGroupDisk \
   --query '[*].{diskId:id}' \
   --output table
```


Kullanım [az vm stop](/cli/azure/vm) stop\deallocate diskleri geçirmeden önce VM için.

```azurecli-interactive
az vm stop \
   -n myVM \
   -g myResourceGroup
```


Kullanım [az vm update](/cli/azure/vm#az-vm-update) yeni disk için tam kaynak kimliği `--osdisk` parametresi 

```azurecli-interactive 
az vm update \
   -g myResourceGroup \
   -n myVM \
   --os-disk /subscriptions/<subscription ID>/resourceGroups/swap/providers/Microsoft.Compute/disks/myDisk 
   ```
   
Kullanarak VM'yi yeniden [az vm start](/cli/azure/vm).

```azurecli-interactive
az vm start \
   -n myVM \
   -g myResourceGroup
```

   
**Sonraki adımlar**

Bir diski bir kopyasını oluşturmak için bkz: [bir diskin anlık görüntüsünü alma](snapshot-copy-managed-disk.md).
