---
title: "Azure CLI betik örnek - bir anlık görüntüden bir VM oluşturma | Microsoft Docs"
description: "Azure CLI betik örnek - bir anlık görüntüden bir VM oluşturma"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: ramankum
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: be282f79445c505ece7c6115df7a29c20a6a5f02
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-cli"></a>Anlık görüntü CLI ile bir sanal makine oluşturun

Bu komut dosyasını bir sanal makine işletim sistemi diski bir anlık görüntüden oluşturur.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.sh "Create VM from snapshot")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, yönetilen bir disk, sanal makine ve tüm ilgili kaynaklar oluşturmak için aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [az anlık görüntü Göster](https://docs.microsoft.com/cli/azure/snapshot#az_snapshot_show) | Anlık görüntü anlık görüntü adı ve kaynak grubu adı kullanılarak alır. Döndürülen nesne kimliği özelliği, yönetilen bir disk oluşturmak için kullanılır.  |
| [az disketi oluşturma](https://docs.microsoft.com/cli/azure/disk#az_disk_create) | Bir anlık görüntüden yönetilen diskleri oluşturur kullanarak snapshot kimliği, disk adı, depolama türünü ve boyutu  |
| [az vm oluşturma](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Yönetilen bir işletim sistemi diski kullanarak VM oluşturur |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek sanal makine CLI kod örnekleri bulunabilir [Azure Linux VM'de belgelerine](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
