---
title: "VHD bir anlık görüntü oluşturma | Microsoft Docs"
description: "Yukarı veya sorunlarını gidermek için bir geri olarak Azure'da bir kopyasını bir VHD oluşturmayı öğrenin."
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 10/09/2017
ms.author: cynthn
ms.openlocfilehash: 152c5a1103d32af27f689086cfcc9cc1a7acc5d3
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-snapshot"></a>Bir anlık görüntü oluşturma 

Yedekleme veya VM sorun giderme için VHD sorunları bir işletim sistemi veya veri diski bir anlık görüntüsünü. Bir VHD tam, salt okunur bir kopyasını bir anlık görüntüdür. 

## <a name="use-azure-cli-20-to-take-a-snapshot"></a>Bir anlık görüntü almak için Azure CLI 2.0 kullanın

Aşağıdaki örnek, Azure CLI yüklenmiş ve Azure hesabınızda oturum 2.0 gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

Aşağıdaki adımları kullanarak bir anlık görüntü almak nasıl Göster `az snapshot create` komutunu `--source-disk` parametresi. Aşağıdaki örnek olarak adlandırılan bir VM olduğunu varsayar `myVM` bir yönetilen işletim sistemi diski ile oluşturulan `myResourceGroup` kaynak grubu.

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
2. Sol üst köşede başlayarak, tıklatın **kaynak oluşturma** arayın ve **anlık görüntü**.
3. Anlık görüntü dikey penceresinde tıklayın **oluşturma**.
4. Girin bir **adı** anlık görüntü için.
5. Varolan [Kaynak grubunu](../../azure-resource-manager/resource-group-overview.md#resource-groups) seçin veya yenisi için adı yazın. 
6. Azure veri merkezi konum seçin.  
7. İçin **kaynak disk**, anlık görüntü için yönetilen diski seçin.
8. Seçin **hesap türü** anlık görüntü deposu için kullanılacak. Öneririz **Standard_LRS** yüksek performanslı bir diskte depolanan gerekmedikçe.
9. **Oluştur**’a tıklayın.

Yönetilen bir Disk oluşturmak ve yüksek performanslı olması gereken bir VM eklemek için anlık görüntü kullanmayı planlıyorsanız, parametresini kullanın `--sku Premium_LRS` ile `az snapshot create` komutu. Bu anlık görüntü oluşturur, böylece yönetilen bir Premium Disk depolanır. Katı hal sürücüleri (SSD) olan ancak birden çok standart diskler (HDD'ler) maliyet premium yönetilen diskleri daha iyi gerçekleştirilemiyor.


## <a name="next-steps"></a>Sonraki adımlar

 Bir sanal makine bir anlık görüntüden yönetilen bir disk anlık görüntüden oluşturarak ve ardından yeni yönetilen işletim sistemi diski olarak diskindeki oluşturun. Daha fazla bilgi için bkz: [bir anlık görüntüden bir VM oluşturma](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) komut dosyası.

