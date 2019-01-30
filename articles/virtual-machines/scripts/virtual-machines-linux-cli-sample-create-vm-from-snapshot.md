---
title: Azure CLI Betik Örneği - Anlık görüntüden VM oluşturma | Microsoft Docs
description: Azure CLI Betik Örneği - Anlık görüntüden VM oluşturma
services: virtual-machines-linux
documentationcenter: virtual-machines
author: ramankumarlive
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: db0aa1781c3e35b68a59082cf7a1760f7e9a34b4
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55239579"
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-cli"></a>CLI ile Anlık görüntüden sanal makine oluşturma

Bu betik bir işletim sistemi diskinin anlık görüntüsünden sanal makine oluşturur.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.sh "Create VM from snapshot")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir yönetilen disk, sanal makine ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az snapshot show](https://docs.microsoft.com/cli/azure/snapshot#az_snapshot_show) | Anlık görüntü adı ve kaynak grubu adını kullanarak anlık görüntüyü alır. Yönetilen bir disk oluşturmak için döndürülen nesnenin kimlik özelliği kullanılır.  |
| [az disk create](https://docs.microsoft.com/cli/azure/disk#az_disk_create) | Anlık görüntü kimliği, disk adı, depolama türü ve boyutu kullanarak anlık görüntüden yönetilen diskler oluşturur  |
| [az vm create](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Yönetilen bir işletim sistemi diskini kullanarak VM oluşturur |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek sanal makine CLI betiği örnekleri, [Azure Linux VM belgeleri](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) içinde bulunabilir.
