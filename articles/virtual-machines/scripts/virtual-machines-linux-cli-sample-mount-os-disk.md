---
title: Azure CLI Betik Örneği - İşletim sistemi diskini bağlama | Microsoft Docs
description: Azure CLI Betik Örneği - İşletim sistemi diskini bağlama
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 996fd1be24abea240d2c2a1a53769b663fef6bde
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34656050"
---
# <a name="troubleshoot-a-vms-operating-system-disk"></a>VM işletim sistemi diski ile ilgili sorun giderme

Bu betik, hatalı veya sorunlu bir sanal makinenin işletim sistemi diskini ikinci bir sanal makineye veri diski olarak bağlar. Disk sorunlarını giderirken veya verileri kurtarırken bu yöntem kullanışlı olabilir. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/mount-os-disk/mount-os-disk.sh "Quick Create VM")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir kaynak grubu, sanal makine ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az vm show](https://docs.microsoft.com/cli/azure/vm#az_vm_show) | Sanal makinelerin listesini döndürür. Bu örnekte sanal makine işletim sistemi diskini döndürmek için sorgu seçeneği kullanılır. Bu değer daha sonra 'uri' değişken adına eklenir. |
| [az vm delete](https://docs.microsoft.com/cli/azure/vm#az_vm_delete) | Bir sanal makineyi siler. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Bir sanal makine oluşturur.  |
| [az vm disk attach](https://docs.microsoft.com/cli/azure/vm/disk#az_vm_disk_attach) | Bir diski sanal makineye ekler. |
| [az vm list-ip-addresses](https://docs.microsoft.com/cli/azure/vm#az_vm_list_ip_addresses) | Bir sanal makinenin IP adreslerini döndürür. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek sanal makine CLI betiği örnekleri, [Azure Linux VM belgeleri](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) içinde bulunabilir.
