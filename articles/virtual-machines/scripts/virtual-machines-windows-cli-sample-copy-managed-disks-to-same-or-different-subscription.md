---
title: Azure CLI Betik Örneği - Yönetilen diskleri aynı veya farklı aboneliğe kopyalama (taşıma) | Microsoft Docs
description: Azure CLI Betik Örneği - Yönetilen diskleri aynı veya farklı aboneliğe kopyalama (taşıma)
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: abb051e9646d547907384ed06413439845a29d5e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60318147"
---
# <a name="copy-managed-disks-to-same-or-different-subscription-with-cli"></a>CLI ile yönetilen diskleri aynı veya farklı aboneliğe kopyalama

Bu betik bir yönetilen diski aynı bölgedeki aynı veya farklı bir aboneliğe kopyalar. Kopya, yalnızca abonelik aynı AAD kiracısındaki bir parçası olduğunda çalışır.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.sh "Copy managed disk")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, kaynak yönetilen diskin kimliğini kullanarak hedef abonelikte yeni bir yönetilen disk oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az disk show](https://docs.microsoft.com/cli/azure/disk) | Yönetilen diskin kaynak ve grup özelliklerini kullanarak tüm özelliklerini alır. Yönetilen diski farklı aboneliğe kopyalamak için kimlik özelliği kullanılır.  |
| [az disk create](https://docs.microsoft.com/cli/azure/disk) | Üst yönetilen diskin kimliği ve adı ile farklı abonelikte yeni bir yönetilen disk oluşturarak yönetilen diski kopyalar.  |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek sanal makine ve yönetilen disk CLI betiği örnekleri bulunabilir [Azure Windows VM belgeleri](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
