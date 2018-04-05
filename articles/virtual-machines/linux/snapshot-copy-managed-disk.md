---
title: VHD bir anlık görüntü oluşturma | Microsoft Docs
description: Yukarı veya sorunlarını gidermek için bir geri olarak Azure'da bir kopyasını bir VHD oluşturmayı öğrenin.
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 03/20/2018
ms.author: cynthn
ms.openlocfilehash: e5882b2ddc708544a7715da13c1f0d18384ce4e3
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="create-a-snapshot"></a>Bir anlık görüntü oluşturma 

Yedekleme veya VM sorunlarını gidermek için bir işletim sistemi veya veri diski bir anlık görüntüsünü. Bir VHD tam, salt okunur bir kopyasını bir anlık görüntüdür. 

## <a name="use-azure-cli"></a>Azure CLI kullanma 

Aşağıdaki örnek, Azure CLI yüklenmiş ve Azure hesabınızda oturum 2.0 gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

Aşağıdaki adımları kullanarak bir anlık görüntü almak nasıl Göster `az snapshot create` komutunu `--source-disk` parametresi. Aşağıdaki örnek olarak adlandırılan bir VM olduğunu varsayar `myVM` içinde `myResourceGroup` kaynak grubu.

Disk kimliği alma
```azure-cli
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
```

Adlı bir anlık görüntüsünü *osDisk yedekleme*.

```azurecli-interactive
az snapshot create \
    -g myResourceGroup \
    --source "$osDiskId" \
    --name osDisk-backup
```

> [!NOTE]
> Bölge esnek depolama alanında, anlık görüntü deposu istiyorsanız destekleyen bir bölgede oluşturmanıza gerek [kullanılabilirlik bölgeleri](../../availability-zones/az-overview.md) ve dahil `--sku Standard_ZRS` parametresi.

## <a name="use-azure-portal"></a>Azure portalını kullanma 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol üst köşede başlayarak, tıklatın **kaynak oluşturma** arayın ve **anlık görüntü**.
3. Anlık görüntü dikey penceresinde tıklayın **oluşturma**.
4. Girin bir **adı** anlık görüntü için.
5. Varolan [Kaynak grubunu](../../azure-resource-manager/resource-group-overview.md#resource-groups) seçin veya yenisi için adı yazın. 
6. Azure veri merkezi konum seçin.  
7. İçin **kaynak disk**, anlık görüntü için yönetilen diski seçin.
8. Seçin **hesap türü** anlık görüntü deposu için kullanılacak. Öneririz **Standard_LRS** yüksek performanslı bir diskte depolanan gerekmedikçe.
9. **Oluştur**’a tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

 Bir sanal makine bir anlık görüntüden yönetilen bir disk anlık görüntüden oluşturarak ve ardından yeni yönetilen işletim sistemi diski olarak diskindeki oluşturun. Daha fazla bilgi için bkz: [bir anlık görüntüden bir VM oluşturma](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) komut dosyası.

