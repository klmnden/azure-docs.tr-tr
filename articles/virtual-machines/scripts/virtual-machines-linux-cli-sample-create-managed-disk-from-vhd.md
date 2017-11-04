---
title: "Azure CLI betik örnek - yönetilen bir disk bir depolama hesabı aynı abonelikte VHD dosyasında oluşturun | Microsoft Docs"
description: "Azure CLI betik örnek - yönetilen bir disk bir depolama hesabı aynı abonelikte VHD dosyasında oluşturun"
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
ms.openlocfilehash: ede9457f5843d0a8a04503779970a553c5ed4f96
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-the-same-subscription-with-cli"></a>CLI ile aynı abonelikte depolama hesabındaki bir VHD dosyasından yönetilen bir disk oluşturma

Bu komut dosyası yönetilen bir disk bir depolama hesabı aynı abonelikte VHD dosyasında oluşturur. Özelleştirilmiş içeri aktarmak için bu betiği kullanın (değil genelleştirilmiş/Sysprep kullanılarak hazırlanmış) bir sanal makine oluşturmak için yönetilen işletim sistemi diski VHD'ye. Ya da yönetilen veri diski VHD veri aktarmak için kullanın. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-managed-data-disks-from-vhd/create-managed-data-disks-from-vhd.sh "Create managed disk from VHD")]


## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını bir VHD'den yönetilen bir disk oluşturmak için komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [az disketi oluşturma](https://docs.microsoft.com/cli/azure/disk#az_disk_create) | Bir depolama hesabı aynı abonelikte VHD URI kullanılarak yönetilen bir disk oluşturur |

## <a name="next-steps"></a>Sonraki adımlar

[Yönetilen bir disk işletim sistemi diski olarak ekleyerek bir sanal makine oluşturun](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek bir sanal makine ve yönetilen diskleri CLI kod örnekleri bulunabilir [Azure Linux VM'de belgelerine](../../app-service/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
