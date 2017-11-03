---
title: "Azure CLI betik örnek - yönetilen bir disk anlık görüntüden oluşturun. | Microsoft Docs"
description: "Azure CLI betik örnek - bir anlık görüntüden yönetilen bir disk oluşturma"
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: ffdad7faa34fec09623a415664b5a260868e9dbc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-managed-disk-from-a-snapshot-with-cli"></a>Anlık görüntü CLI ile yönetilen bir disk oluşturun

Bu komut dosyasını bir anlık görüntüden yönetilen bir disk oluşturur. Bir sanal makine işletim sistemi ve veri diskleri anlık görüntülerden geri yüklemek için kullanın. İşletim sistemi oluşturun ve veri diskleri ilgili anlık görüntülerden yönetilen ve ardından yönetilen diskleri ekleyerek yeni bir sanal makine oluşturun. Anlık görüntülerden oluşturulan veri diskleri ekleyerek, mevcut bir VM'yi veri diskleri geri yükleyebilirsiniz.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-managed-disks-from-snapshot/create-managed-disks-from-snapshot.sh "Create managed disk from snapshot")]


## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını bir anlık görüntüden yönetilen bir disk oluşturmak için komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [az anlık görüntü Göster](https://docs.microsoft.com/cli/azure/snapshot#az_snapshot_show) | Tüm ad kullanarak bir anlık görüntü özelliklerini ve kaynak grubu özellikleri anlık görüntü alır. ID özelliği, yönetilen disk oluşturmak için kullanılır.  |
| [az disketi oluşturma](https://docs.microsoft.com/cli/azure/disk#az_disk_create) | Yönetilen oluşturur diski kullanarak, yönetilen bir anlık görüntü kimliğini anlık görüntüsünü alın |

## <a name="next-steps"></a>Sonraki adımlar

[Yönetilen bir disk işletim sistemi diski olarak ekleyerek bir sanal makine oluşturun](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek bir sanal makine ve yönetilen diskleri CLI kod örnekleri bulunabilir [Azure Linux VM'de belgelerine](../../app-service/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
