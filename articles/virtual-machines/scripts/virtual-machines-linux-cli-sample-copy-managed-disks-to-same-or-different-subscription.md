---
title: Azure CLI Betik Örneği - Yönetilen diskleri aynı veya farklı aboneliğe kopyalama (taşıma) | Microsoft Docs
description: Azure CLI Betik Örneği - Yönetilen diskleri aynı veya farklı aboneliğe kopyalama (taşıma)
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: dfdbc0563810447a1a214356b5153afe38d9cf2f
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
ms.locfileid: "29846800"
---
# <a name="copy-managed-disks-to-same-or-different-subscription-with-cli"></a>CLI ile yönetilen diskleri aynı veya farklı aboneliğe kopyalama

Bu betik bir yönetilen diski aynı bölgedeki aynı veya farklı bir aboneliğe kopyalar. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.sh "Copy managed disk")]


## <a name="script-explanation"></a>Betik açıklaması

Bu betik, kaynak yönetilen diskin kimliğini kullanarak hedef abonelikte yeni bir yönetilen disk oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az disk show](https://docs.microsoft.com/cli/azure/disk#az_disk_show) | Yönetilen diskin kaynak ve grup özelliklerini kullanarak tüm özelliklerini alır. Yönetilen diski farklı aboneliğe kopyalamak için kimlik özelliği kullanılır.  |
| [az disk create](https://docs.microsoft.com/cli/azure/disk#az_disk_create) | Üst yönetilen diskin kimliği ve adı ile farklı abonelikte yeni bir yönetilen disk oluşturarak yönetilen diski kopyalar.  |

## <a name="next-steps"></a>Sonraki adımlar

[Yönetilen diskten sanal makine oluşturma](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek sanal makine ve yönetilen disk CLI betiği örnekleri, [Azure Linux VM belgeleri](../../app-service/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) içinde bulunabilir.
