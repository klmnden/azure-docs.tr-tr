---
title: Azure'da bir VHD anlık görüntüsünü oluşturma | Microsoft Docs
description: Bir yedekleme ayarlamak veya ilgili sorunları gidermeye yönelik olarak Azure'da bir kopyasını bir VHD oluşturmayı öğrenin.
documentationcenter: ''
author: roygara
manager: twooley
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 07/11/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 9f2f3ac3668f0e48716fc30fb69cd1782dbd4e56
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63765673"
---
# <a name="create-a-snapshot"></a>Anlık görüntü oluşturma 

Yedekleme için veya VM sorunlarını gidermek için bir işletim sistemi veya veri diski anlık görüntüsünü alın. Anlık görüntü, bir VHD, tam ve salt okunur bir kopyasıdır. 

## <a name="use-azure-cli"></a>Azure CLI kullanma 

Aşağıdaki örnek kullanmanızı gerektirir. [Cloud Shell](https://shell.azure.com/bash) veya Azure CLI'ın yüklü olması gerekmektedir.

Aşağıdaki adımlarda bir anlık görüntü kullanarak nasıl **az anlık görüntü oluşturma** komutunu **--kaynak disk** parametresi. Aşağıdaki örnekte adlı bir VM olduğu varsayılmaktadır. *myVM* içinde *myResourceGroup* kaynak grubu.

Disk Kimliğini kullanarak alma [az vm show](/cli/azure/vm#az-vm-show).

```azurecli-interactive
osDiskId=$(az vm show \
   -g myResourceGroup \
   -n myVM \
   --query "storageProfile.osDisk.managedDisk.id" \
   -o tsv)
```

Adlı bir anlık görüntüsünü alma *osDisk yedekleme* kullanarak [az anlık görüntü oluşturma](/cli/azure/snapshot#az-snapshot-create).

```azurecli-interactive
az snapshot create \
    -g myResourceGroup \
    --source "$osDiskId" \
    --name osDisk-backup
```

> [!NOTE]
> Bölge dayanıklı depolama, anlık görüntü depolamak istediğiniz desteklediği bir bölgede oluşturmanız gerekirse [kullanılabilirlik](../../availability-zones/az-overview.md) ve **--sku Standard_ZRS** parametresi.

Kullanarak anlık görüntülerin bir listesini görebilirsiniz [az anlık görüntü listesi](/cli/azure/snapshot#az-snapshot-list).

```azurecli-interactive
az snapshot list \
   -g myResourceGroup \
   - table
```

## <a name="use-azure-portal"></a>Azure portalı kullanma 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol üstten başlayarak, tıklayın **kaynak Oluştur** araması **anlık görüntü**. Seçin **anlık görüntü** Arama sonuçlarından.
3. İçinde **anlık görüntü** dikey penceresinde tıklayın **Oluştur**.
4. Girin bir **adı** anlık görüntü.
5. Mevcut bir kaynak grubunu seçin veya yeni bir ad yazın. 
7. İçin **kaynak disk**, yönetilen diskin anlık görüntüsünü seçin.
8. Seçin **hesap türü** anlık görüntü deposu için kullanılacak. Kullanım **standart HDD** yüksek performanslı SSD üzerinde depolanan gerekmedikçe.
9. **Oluştur**’a tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

 Bir sanal makine anlık görüntüden yönetilen disk oluşturma ve ardından yeni bir yönetilen diski işletim sistemi diski olarak ekleyerek bir anlık görüntüden oluşturun. Daha fazla bilgi için [anlık görüntüden VM oluşturma](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) betiği.

