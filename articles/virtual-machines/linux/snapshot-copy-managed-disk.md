---
title: "Yönetilen bir Azure diski geri için yukarı kopyalama | Microsoft Docs"
description: "Yedekleme için kullanılacak bir Azure yönetilen Disk veya disk sorunlarını giderme bir kopyasını oluşturmayı öğrenin."
documentationcenter: 
author: squillace
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 2/6/2017
ms.author: rasquill
ms.openlocfilehash: c91367ef11c9d531bebac7c069d2df586607ec29
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-copy-of-a-vhd-stored-as-an-azure-managed-disk-by-using-managed-snapshots"></a>Yönetilen bir Azure diski yönetilen anlık görüntülerini kullanarak tarafından depolanan VHD bir kopyasını oluşturun
Yönetilen disk yedekleme için bir anlık görüntüsünü veya yönetilen bir Disk anlık görüntüden oluşturabilir ve sorun giderme için test sanal makineye Ekle. Yönetilen bir anlık görüntü yönetilen bir VM Disk noktası zaman tam kopyasıdır. Bu, VHD salt okunur bir kopyasını oluşturur ve isteğe bağlı olarak varsayılan olarak, standart yönetilen Disk olarak depolar. 

Fiyatlandırma hakkında daha fazla bilgi için bkz: [Azure Storage fiyatlandırması](https://azure.microsoft.com/pricing/details/managed-disks/). <!--Add link to topic or blog post that explains managed disks. -->

Azure portalında veya Azure CLI 2.0 yönetilen diskin anlık görüntüsünü almak için kullanın.

## <a name="use-azure-cli-20-to-take-a-snapshot"></a>Bir anlık görüntü almak için Azure CLI 2.0 kullanın

> [!NOTE] 
> Aşağıdaki örnek, Azure CLI yüklenmiş ve Azure hesabınızda oturum 2.0 gerektirir.

Aşağıdaki adımlar nasıl elde edilir ve yönetilen bir işletim sistemi diski kullanarak, bir anlık görüntüsünü gösterir `az snapshot create` komutunu `--source-disk` parametresi. Aşağıdaki örnek olarak adlandırılan bir VM olduğunu varsayar `myVM` bir yönetilen işletim sistemi diski ile oluşturulan `myResourceGroup` kaynak grubu.

```azure-cli
# take the disk id with which to create a snapshot
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create -g myResourceGroup --source "$osDiskId" --name osDisk-backup
```

Çıktı aşağıdakine benzer görünmelidir:

```json
{
  "accountType": "Standard_LRS",
  "creationData": {
    "createOption": "Copy",
    "imageReference": null,
    "sourceResourceId": null,
    "sourceUri": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/disks/osdisk_6NexYgkFQU",
    "storageAccountId": null
  },
  "diskSizeGb": 30,
  "encryptionSettings": null,
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/snapshots/osDisk-backup",
  "location": "westus",
  "name": "osDisk-backup",
  "osType": "Linux",
  "ownerId": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "timeCreated": "2017-02-06T21:27:10.172980+00:00",
  "type": "Microsoft.Compute/snapshots"
}
```

## <a name="use-azure-portal-to-take-a-snapshot"></a>Bir anlık görüntüsünü için Azure portalını kullanma 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol üst köşede başlayarak, tıklatın **yeni** arayın ve **anlık görüntü**.
3. Anlık görüntü dikey penceresinde tıklayın **oluşturma**.
4. Girin bir **adı** anlık görüntü için.
5. Varolan [Kaynak grubunu](../../azure-resource-manager/resource-group-overview.md#resource-groups) seçin veya yenisi için adı yazın. 
6. Azure veri merkezi konum seçin.  
7. İçin **kaynak disk**, anlık görüntü için yönetilen diski seçin.
8. Seçin **hesap türü** anlık görüntü deposu için kullanılacak. Öneririz **Standard_LRS** yüksek performanslı bir diskte depolanan gerekmedikçe.
9. **Oluştur**'a tıklayın.

Yönetilen bir Disk oluşturmak ve yüksek performanslı olması gereken bir VM eklemek için anlık görüntü kullanmayı planlıyorsanız, parametresini kullanın `--sku Premium_LRS` ile `az snapshot create` komutu. Bu anlık görüntü oluşturur, böylece yönetilen bir Premium Disk depolanır. Katı hal sürücüleri (SSD) olan ancak birden çok standart diskler (HDD'ler) maliyet premium yönetilen diskleri daha iyi gerçekleştirilemiyor.


